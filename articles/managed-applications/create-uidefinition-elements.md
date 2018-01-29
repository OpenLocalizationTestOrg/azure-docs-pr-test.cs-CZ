---
title: "Spravované aplikace Azure vytvořit definici funkcí uživatelského rozhraní | Microsoft Docs"
description: "Popisuje funkce pro použití při vytváření definice uživatelského rozhraní pro spravované aplikace Azure"
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 23e407bf93bc51116ca45339bffcb801d69290f0
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="createuidefinition-elements"></a>CreateUiDefinition elementy
Tento článek popisuje vlastnosti schématu a pro všechny podporované elementy CreateUiDefinition. Tyto prvky můžete použít při [vytváření spravovaných aplikací Azure](publish-service-catalog-app.md). Schéma pro většinu prvků vypadá takto:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Keep calm and visit the [Azure Portal](portal.azure.com).",
  "constraints": {},
  "options": {},
  "visible": true
}
```

| Vlastnost | Požaduje se | Popis |
| -------- | -------- | ----------- |
| jméno | Ano | Interní identifikátor tak, aby odkazovaly konkrétní instanci elementu. Nejběžnější využití název elementu je v `outputs`, kde jsou hodnoty výstup zadaných elementů namapované na parametry šablony. Můžete ji použít i k vytvoření vazby výstupní hodnotu elementu na `defaultValue` jiného elementu. |
| type | Ano | Kontrolní mechanismus uživatelského rozhraní pro vykreslení elementu. Seznam podporovaných typů najdete v tématu [elementy](#elements). |
| Popisek | Ano | Zobrazovaný text elementu. Některé typy element obsahovat více štítků, takže hodnotou může být objekt obsahující více řetězců. |
| Výchozí hodnota | Ne | Výchozí hodnota elementu. Některé typy element podporují komplexní výchozí hodnoty, takže hodnotou může být objekt. |
| Popisek | Ne | Text, který zobrazit jako popis elementu. Podobně jako `label`, některé prvky podporovat více nástroj tip řetězců. Vložené odkazy lze jej vkládat pomocí syntaxe Markdownu.
| Omezení | Ne | Jednu nebo více vlastností, které slouží k přizpůsobení chování ověřování elementu. Podporované vlastnosti pro omezení se liší podle typu elementu. Některé typy element nepodporuje přizpůsobení chování ověření a proto mít vlastnost žádné omezení. |
| Možnosti | Ne | Další vlastnosti, které přizpůsobují chování elementu. Podobně jako `constraints`, podporovaných vlastností se liší podle typu elementu. |
| Viditelné | Ne | Označuje, zda se zobrazí elementu. Pokud `true`, se zobrazí elementu a příslušné podřízené elementy. Výchozí hodnota je `true`. Použití [logické funkce](create-uidefinition-functions.md#logical-functions) dynamicky řídit hodnota této vlastnosti.

## <a name="elements"></a>Elementy

V dokumentaci pro každý prvek obsahuje ukázku uživatelského rozhraní, schéma, remarks na chování elementu (obvykle týkají se ověření a podporované přizpůsobení) a ukázkový výstup.

- [Microsoft.Common.DropDown](microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](microsoft-common-fileupload.md)
- [Microsoft.Common.OptionsGroup](microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](microsoft-common-section.md)
- [Microsoft.Common.TextBox](microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](microsoft-compute-usernametextbox.md)
- [Microsoft.Network.PublicIpAddressCombo](microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Další kroky
* Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](overview.md).
* Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](create-uidefinition-overview.md).
