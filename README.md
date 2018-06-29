# Hermitage_Bitrix
## Технология эрмитаж <b>без компонента</b>


<img src="img.png" />

### Как добавить кнопки «Изменить элемент», «Добавить элемент», «Удалить элемент» в 1С-Битрикс

Итак, разбираем AddEditAction — первый параметр ID элемента, второй ссылка на Action, третий Text кнопки. Обратите внимание, что для удаление используется AddDeleteAction, где мы передаем четвертым параметром подтверждение через alert.
Так как мы делаем эрмитаж не в компоненте, нам нужно создать новый объект <b>$comp = new CBitrixComponent;</b>

Дальше всё просто:

```php
// Получаем ссылки для редактирования и удаления элемента
  $arButtons = CIBlock::GetPanelButtons(
      $arItem["IBLOCK_ID"],
      $arItem["ID"],
      0,
      array("SECTION_BUTTONS"=>false, "SESSID"=>false)
  );
  $arItem["EDIT_LINK"] = $arButtons["edit"]["edit_element"]["ACTION_URL"];
  $arItem["DELETE_LINK"] = $arButtons["edit"]["delete_element"]["ACTION_URL"];

  // Вызываем действия для элемента методами AddEditAction, AddDeleteAction
  $comp = new CBitrixComponent;
  $comp->AddEditAction($sheetRow[ID], $sheetRow['EDIT_LINK'], CIBlock::GetArrayByID($arItem["IBLOCK_ID"], "ELEMENT_EDIT"));
  $comp->AddDeleteAction($sheetRow[ID], $sheetRow['DELETE_LINK'], CIBlock::GetArrayByID($arItem["IBLOCK_ID"], "ELEMENT_DELETE"), array("CONFIRM" => GetMessage('CT_BNL_ELEMENT_DELETE_CONFIRM')));
?>
```
Чтобы это заработало, блоку нужно передать id:

```html
  <div id="<?=$comp->GetEditAreaId($arItem[ID])?>">Элемент, который выделится эрмитажем</div>
```
