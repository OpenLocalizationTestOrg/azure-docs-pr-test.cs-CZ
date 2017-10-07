---
title: "aaaAzure spravované aplikace vytvořit definici funkcí uživatelského rozhraní | Microsoft Docs"
description: "Popisuje funkce toouse hello při vytváření definice uživatelského rozhraní pro spravované aplikace Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: a34c6202372168cda769c471b1c9fdd539dd0f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="createuidefinition-elements"></a>CreateUiDefinition elementy
Tento článek popisuje hello schématu a vlastnosti pro všechny podporované elementy CreateUiDefinition. Tyto prvky můžete použít při [vytváření spravovaných aplikací Azure](managed-application-publishing.md). Hello schéma pro většinu prvků vypadá takto:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Keep calm and visit hello [Azure Portal](portal.azure.com).",
  "constraints": {},
  "options": {},
  "visible": true
}
```
| Vlastnost | Požaduje se | Popis |
| -------- | -------- | ----------- |
| jméno | Ano | Tooreference interní identifikátor konkrétní instanci elementu. Hello nejběžnější využití hello název elementu, který je v `outputs`, kde zadané hello výstup hodnoty hello prvky jsou namapované toohello parametry šablony hello. Můžete ji použít i hodnotu výstup hello toobind toohello element `defaultValue` jiného elementu. |
| type | Ano | Hello toorender ovládací prvek uživatelského rozhraní pro hello element. Seznam podporovaných typů najdete v tématu [elementy](#elements). |
| Popisek | Ano | Hello zobrazit text elementu hello. Některé typy element obsahovat více štítků, takže hello hodnotou může být objekt obsahující více řetězců. |
| Výchozí hodnota | Ne | Výchozí hodnota Hello hello elementu. Některé typy element podporují komplexní výchozí hodnoty, takže hello hodnotou může být objekt. |
| Popisek | Ne | Hello toodisplay text v popisu tlačítka hello hello elementu. Podobně jako příliš`label`, některé prvky podporovat více nástroj tip řetězců. Vložené odkazy lze jej vkládat pomocí syntaxe Markdownu.
| Omezení | Ne | Jeden nebo více vlastnosti, které jsou používané toocustomize hello chování při ověřování elementu hello. Hello podporované vlastnosti pro omezení se liší podle typu elementu. Některé typy element nepodporuje přizpůsobení chování hello ověření a proto mít vlastnost žádné omezení. |
| Možnosti | Ne | Další vlastnosti, které přizpůsobují chování hello hello elementu. Podobně jako příliš`constraints`, hello podporované vlastnosti se liší podle typu elementu. |
| Viditelné | Ne | Určuje, zda text hello element zobrazena. Pokud `true`, se zobrazují hello elementu a příslušné podřízené elementy. Hello výchozí hodnota je `true`. Použití [logické funkce](managed-application-createuidefinition-functions.md#logical-functions) toodynamically řídí hodnota této vlastnosti.

## <a name="elements"></a>Elementy

Hello dokumentace pro každý prvek obsahuje ukázku uživatelského rozhraní, schéma, remarks na hello chování element hello (obvykle týkají se ověření a podporované přizpůsobení) a ukázkový výstup.

- [Microsoft.Common.DropDown](managed-application-microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](managed-application-microsoft-common-fileupload.md)
- [Microsoft.Common.OptionsGroup](managed-application-microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](managed-application-microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](managed-application-microsoft-common-section.md)
- [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](managed-application-microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](managed-application-microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](managed-application-microsoft-compute-usernametextbox.md)
- [Microsoft.Network.PublicIpAddressCombo](managed-application-microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](managed-application-microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](managed-application-microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](managed-application-microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Další kroky
* Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
