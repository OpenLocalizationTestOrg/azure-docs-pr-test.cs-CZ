---
title: "aaaTag Azure prostředky pro logické organizace | Microsoft Docs"
description: "Ukazuje, jak tooapply značky tooorganize Azure prostředky pro fakturace a správy."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 003a78e5-2ff8-4685-93b4-e94d6fb8ed5b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: AzurePortal
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: tomfitz
ms.openlocfilehash: e07470463d160f8cefe5c80bc91e66a96af6ca45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-tags-tooorganize-your-azure-resources"></a>Pomocí značky tooorganize vašich prostředků Azure
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

> [!NOTE]
> Můžete použít pouze tooresources značky, které podporují operace Azure Resource Manager. Pokud jste vytvořili virtuální počítač, virtuální sítě nebo účet úložiště pomocí modelu nasazení classic hello (hello například jako prostřednictvím portálu Azure classic), nemůžete použít prostředek toothat značky. toosupport označování, znovu nasadit tyto prostředky prostřednictvím Resource Manager. Všechny ostatní prostředky podporují označování.
> 
> 

## <a name="policies-for-tag-consistency"></a>Zásady pro značka konzistence

Prostředek zásady toocreate standardní pravidla můžete použít pro vaši organizaci. Můžete vytvořit zásady, které zajišťují, že prostředky jsou označené hello příslušné hodnoty. Další informace najdete v tématu [zásad prostředků pro značky](resource-manager-policy-tags.md).

## <a name="powershell"></a>PowerShell
[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure CLI

toosee hello stávající značky pro *skupiny prostředků*, použijte:

```azurecli
az group show -n examplegroup --query tags
```

Tento skript vrátí hello následující formát:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

toosee hello stávající značky pro *prostředek, který má zadaný prostředek ID*, použijte:

```azurecli
az resource show --id {resource-id} --query tags
```

Nebo toosee hello stávající značky pro *prostředek, který má zadaný název, typ a prostředků skupinu*, použijte:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

použití skupin prostředků tooget, které mají s konkrétní značkou tag `az group list`:

```azurecli
az group list --tag Dept=IT
```

tooget všechny hello prostředky, které mají konkrétní značku a hodnota, používají `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Pokaždé, když použijete značky tooa prostředek nebo skupina zdrojů, můžete přepsat hello existující značek pro daný prostředek nebo skupina prostředků. Proto je nutné použít jiný přístup na základě toho, jestli hello prostředek nebo skupina prostředků má stávající značky. 

tooadd značky tooa *skupiny prostředků bez existující značky*, použijte:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

tooadd značky tooa *prostředku bez existující značky*, použijte:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
``` 

tooadd značky tooa prostředek, který již má značky, načtení hello existující značky, přeformátujte tuto hodnotu a znovu zásadu použijte hello stávající a nové značky: 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

tooapply všechny značky ze zdroje tooits skupiny prostředků, a *není zachována stávající značky na prostředcích hello*, použijte hello následující skript:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    az resource tag --tags $t --id $resid
  done 
done
```

tooapply všechny značky ze zdroje tooits skupiny prostředků, a *zachovat stávající značky na prostředcích*, použijte hello následující skript:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done 
done
```


## <a name="templates"></a>Šablony

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>Portál
[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]


## <a name="rest-api"></a>REST API
Hello portál Azure a prostředí PowerShell použít hello [REST API Resource Manageru](https://docs.microsoft.com/rest/api/resources/) pozadí hello. Pokud potřebujete toointegrate označování do jiného prostředí, můžete získat značky pomocí **získat** hello prostředků ID a aktualizace hello sadu pomocí značek **oprava** volání.

## <a name="tags-and-billing"></a>Značky a fakturace
Můžete vytvořit značky toogroup fakturační údaje. Například pokud používáte víc virtuálních počítačů pro jiné organizace, použijte hello značky toogroup využití podle nákladové středisko. Můžete taky náklady toocategorize značky prostředí runtime, jako je například hello fakturace využití pro virtuální počítače spuštěné v provozním prostředí hello.


Můžete načíst informace o značkách prostřednictvím hello [využití prostředků Azure a rozhraní API RateCard](../billing/billing-usage-rate-card-overview.md) nebo soubor hodnot oddělených čárkami (CSV) využití hello. Stáhnout soubor využití hello z hello [účet Azure portal](https://account.windowsazure.com/) nebo [EA portál](https://ea.azure.com). Další informace o programový přístup toobilling informace najdete v tématu [proniknout do vaší spotřeby prostředků Microsoft Azure](../billing/billing-usage-rate-card-overview.md). Operace REST API, najdete v části [referenční dokumentace rozhraní API Azure fakturace REST](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).


Při stahování hello použití sdíleného svazku clusteru pro služby, které podporují značky s fakturace hello značky se zobrazují v hello **značky** sloupce. Další informace najdete v tématu [porozumět vaší faktuře pro Microsoft Azure](../billing/billing-understand-your-bill.md).

![Najdete v části značky k fakturaci](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Další kroky
* Pomocí vlastních zásad, můžete použít omezení a pravidla týkající se vašeho předplatného. Zásady, které definujete může vyžadovat, že všechny prostředky obsahovat hodnotu pro konkrétní značku. Další informace najdete v tématu [používají zásady toomanage prostředky a řízení přístupu](resource-manager-policy.md).
* Úvod toousing prostředí Azure PowerShell Pokud nasazujete prostředky, najdete v části [použití Azure Powershellu s Azure Resource Manager](powershell-azure-resource-manager.md).
* Úvod toousing hello rozhraní příkazového řádku Azure Pokud nasazujete prostředky, najdete v části [hello pomocí Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).
* Úvod toousing hello portálu, najdete v části [pomocí hello Azure portálu toomanage vašich prostředků Azure](resource-group-portal.md).  
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

