---
title: "aaaAzure zprostředkovatelé prostředků a typy prostředků | Microsoft Docs"
description: "Popisuje zprostředkovatele hello prostředků, které podporují Resource Manager, jejich schémat a k dispozici verze rozhraní API a hello oblasti, které může být hostitelem hello prostředky."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 23db1d3808a20166f3b44ec801e1bcc46fbb9bd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resource-providers-and-types"></a>Poskytovatelé prostředků a typy

Pokud nasazujete prostředky, musíte často tooretrieve informace o poskytovatelích prostředků hello a typy. V tomto článku se dozvíte na:

* Zobrazení všech poskytovatelů prostředků v Azure
* Zkontrolovat stav registrace poskytovatele prostředků
* Registrace poskytovatele prostředků
* Zobrazit typy prostředků pro poskytovatele prostředků
* Zobrazení platná umístění pro typ prostředku
* Zobrazit platná verze rozhraní API pro typ prostředku

Abyste mohli provést tyto kroky prostřednictvím portálu hello, prostředí PowerShell nebo rozhraní příkazového řádku Azure.

## <a name="powershell"></a>PowerShell

toosee všech poskytovatelů prostředků v Azure a hello stav registrace pro vaše předplatné, použijte:

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Které vrátí výsledky podobné:

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Registruje se poskytovatel prostředků nakonfiguruje vaše předplatné toowork s poskytovatelem prostředků hello. Hello oboru pro registraci je vždy hello předplatného. Ve výchozím nastavení se automaticky registruje mnoho poskytovatelů prostředků. Ale musíte toomanually zaregistrovat někteří poskytovatelé prostředků. tooregister poskytovatele prostředků, musíte mít oprávnění tooperform hello `/register/action` operace pro poskytovatele prostředků hello. Tato operace je součástí hello Přispěvatel a vlastníka role.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Které vrátí výsledky podobné:

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Pokud máte pořád typy prostředků z tohoto zprostředkovatele prostředků v rámci vašeho předplatného, nelze zrušit registraci poskytovatele prostředků.

informace o toosee pro určitý prostředek zprostředkovatele, použijte:

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Které vrátí výsledky podobné:

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

toosee hello typy prostředků pro poskytovatele prostředků, použijte:

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Která vrací:

```powershell
batchAccounts
operations
locations
locations/quotas
```

verze rozhraní API Hello odpovídá verzi tooa operací REST API, které jsou vydané poskytovatelem prostředků hello. Zprostředkovatel prostředků umožňuje nové funkce, uvolní novou verzi hello REST API. 

tooget hello k dispozici rozhraní API verze pro typ prostředku, použijte:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Která vrací:

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Správce prostředků se podporuje ve všech oblastech, ale nemusí být hello prostředky, které nasazujete podporován ve všech oblastech. Kromě toho může být omezení na vaše předplatné, které zabránit vám v použití některé oblasti, které podporují hello prostředků. 

umístění tooget hello podporované pro typ prostředku použít.

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Která vrací:

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure CLI
toosee všech poskytovatelů prostředků v Azure a hello stav registrace pro vaše předplatné, použijte:

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Které vrátí výsledky podobné:

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Registruje se poskytovatel prostředků nakonfiguruje vaše předplatné toowork s poskytovatelem prostředků hello. Hello oboru pro registraci je vždy hello předplatného. Ve výchozím nastavení se automaticky registruje mnoho poskytovatelů prostředků. Ale musíte toomanually zaregistrovat někteří poskytovatelé prostředků. tooregister poskytovatele prostředků, musíte mít oprávnění tooperform hello `/register/action` operace pro poskytovatele prostředků hello. Tato operace je součástí hello Přispěvatel a vlastníka role.

```azurecli
az provider register --namespace Microsoft.Batch
```

Které vrátí zprávu, že registrace se průběžné.

Pokud máte pořád typy prostředků z tohoto zprostředkovatele prostředků v rámci vašeho předplatného, nelze zrušit registraci poskytovatele prostředků.

informace o toosee pro určitý prostředek zprostředkovatele, použijte:

```azurecli
az provider show --namespace Microsoft.Batch
```

Které vrátí výsledky podobné:

```azurecli
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

toosee hello typy prostředků pro poskytovatele prostředků, použijte:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Která vrací:

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

verze rozhraní API Hello odpovídá verzi tooa operací REST API, které jsou vydané poskytovatelem prostředků hello. Zprostředkovatel prostředků umožňuje nové funkce, uvolní novou verzi hello REST API. 

tooget hello k dispozici rozhraní API verze pro typ prostředku, použijte:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Která vrací:

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Správce prostředků se podporuje ve všech oblastech, ale nemusí být hello prostředky, které nasazujete podporován ve všech oblastech. Kromě toho může být omezení na vaše předplatné, které zabránit vám v použití některé oblasti, které podporují hello prostředků. 

umístění tooget hello podporované pro typ prostředku použít.

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Která vrací:

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a>Portál

Vyberte všechny poskytovatele prostředků v Azure a hello stav registrace pro vaše předplatné toosee **odběry**.

![Vyberte předplatná](./media/resource-manager-supported-services/select-subscriptions.png)

Zvolte předplatné tooview hello.

![Zadejte předplatné](./media/resource-manager-supported-services/subscription.png)

Vyberte **zprostředkovatelé prostředků** a hello zobrazit seznam dostupných poskytovatelů prostředků.

![Zobrazit zprostředkovatelé prostředků](./media/resource-manager-supported-services/show-resource-providers.png)

Registruje se poskytovatel prostředků nakonfiguruje vaše předplatné toowork s poskytovatelem prostředků hello. Hello oboru pro registraci je vždy hello předplatného. Ve výchozím nastavení se automaticky registruje mnoho poskytovatelů prostředků. Ale musíte toomanually zaregistrovat někteří poskytovatelé prostředků. tooregister poskytovatele prostředků, musíte mít oprávnění tooperform hello `/register/action` operace pro poskytovatele prostředků hello. Tato operace je součástí hello Přispěvatel a vlastníka role. Vyberte tooregister zprostředkovatel prostředků, **zaregistrovat**.

![registrace poskytovatele prostředků](./media/resource-manager-supported-services/register-provider.png)

Pokud máte pořád typy prostředků z tohoto zprostředkovatele prostředků v rámci vašeho předplatného, nelze zrušit registraci poskytovatele prostředků.

informace o toosee pro určitý prostředek poskytovatele, vyberte **další služby**.

![Vyberte další služby](./media/resource-manager-supported-services/more-services.png)

Vyhledejte **Průzkumníka prostředků** a vyberte z dostupných možností hello.

![Vyberte Průzkumník prostředků](./media/resource-manager-supported-services/select-resource-explorer.png)

Vyberte **zprostředkovatelé**.

![Vyberte zprostředkovatele](./media/resource-manager-supported-services/select-providers.png)

Vyberte hello poskytovatele prostředků a prostředků zadejte, že chcete tooview.

![Vyberte typ prostředku](./media/resource-manager-supported-services/select-resource-type.png)

Správce prostředků se podporuje ve všech oblastech, ale nemusí být hello prostředky, které nasazujete podporován ve všech oblastech. Kromě toho může být omezení na vaše předplatné, které zabránit vám v použití některé oblasti, které podporují hello prostředků. Průzkumník prostředků Hello zobrazí platná umístění pro typ prostředku hello.

![Zobrazit umístění](./media/resource-manager-supported-services/show-locations.png)

verze rozhraní API Hello odpovídá verzi tooa operací REST API, které jsou vydané poskytovatelem prostředků hello. Zprostředkovatel prostředků umožňuje nové funkce, uvolní novou verzi hello REST API. Průzkumník prostředků Hello zobrazí platná verze rozhraní API pro typ prostředku hello.

![Zobrazit verze rozhraní API](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a>Další kroky
* toolearn o vytváření šablon Resource Manageru, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).
* toolearn o nasazení prostředků, najdete v části [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).
* operace hello tooview pro poskytovatele prostředků, najdete v části [rozhraní REST API Azure](/rest/api/).

