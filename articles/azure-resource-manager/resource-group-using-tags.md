---
title: "Označit prostředky Azure pro logické organizaci | Microsoft Docs"
description: "Ukazuje způsob použití značek k uspořádání prostředků Azure pro fakturace a správy."
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
ms.date: 01/19/2018
ms.author: tomfitz
ms.openlocfilehash: 31477cbf478d2d836c2d7c3472e3a53f13831480
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="use-tags-to-organize-your-azure-resources"></a>Používání značek k uspořádání prostředků Azure

[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

## <a name="powershell"></a>PowerShell

[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure CLI

Pokud chcete zobrazit existující značky pro *skupinu prostředků*, použijte:

```azurecli
az group show -n examplegroup --query tags
```

Výstup tohoto skriptu bude v následujícím formátu:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

Nebo, pokud chcete zobrazit stávající značky pro *prostředek, který má zadaný název, typ a prostředků skupinu*, použijte:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Když ve smyčce přes kolekce prostředků, můžete chtít zobrazit ID prostředku podle zdroje. Úplný příklad se zobrazí později v tomto článku. Pokud chcete zobrazit existující značky pro *prostředek s konkrétním ID prostředku*, použijte:

```azurecli
az resource show --id <resource-id> --query tags
```

Skupiny prostředků, které mají s konkrétní značkou tag, použijte `az group list`:

```azurecli
az group list --tag Dept=IT
```

Všechny prostředky, které mají určitý značky a hodnoty, použijte `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Pokaždé, když použijete značky na prostředek nebo skupinu prostředků, přepíšete pro daný prostředek nebo skupinu prostředků existující značky. Proto je nutné použít jiný přístup na základě toho, jestli prostředek nebo skupina prostředků má existující značky.

Pokud chcete přidat značky ke *skupině prostředků bez existujících značek*, použijte:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

Pokud chcete přidat značky k *prostředku bez existujících značek*, použijte:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Přidání značek k prostředku, který již má značky, načíst existující značky, přeformátujte tuto hodnotu a znovu použít stávající a nové značky: 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Pokud chcete použít všechny značky ze skupiny prostředků na prostředky a *nezachovat existující značky u prostředků*, použijte tento skript:

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

Použít všechny značky ze skupiny zdrojů na jeho prostředky, a *zachovat stávající značky na prostředcích*, použijte tento skript:

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

Portál Azure a prostředí PowerShell, jak používat [REST API Resource Manageru](https://docs.microsoft.com/rest/api/resources/) na pozadí. Pokud potřebujete integrovat označování do jiného prostředí, můžete získat značky pomocí **získat** na ID prostředku a aktualizace sadu značky pomocí **oprava** volání.

## <a name="tags-and-billing"></a>Značky a fakturace

Značky můžete použít k seskupení fakturační údaje. Například pokud používáte víc virtuálních počítačů pro jiné organizace, použití značek k použití skupiny podle nákladové středisko. Značky lze použít také ke kategorizaci náklady prostředí runtime, jako je fakturace využití pro virtuální počítače spuštěné v provozním prostředí.

Můžete načíst informace o značkách prostřednictvím [využití prostředků Azure a rozhraní API RateCard](../billing/billing-usage-rate-card-overview.md) nebo využití soubor hodnot oddělených čárkami (CSV). Stáhnout soubor využití z [účet Azure portal](https://account.windowsazure.com/) nebo [EA portál](https://ea.azure.com). Další informace o programový přístup k fakturační informace najdete v tématu [proniknout do vaší spotřeby prostředků Microsoft Azure](../billing/billing-usage-rate-card-overview.md). Operace REST API, najdete v části [referenční dokumentace rozhraní API Azure fakturace REST](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Když si stáhnete použití sdíleného svazku clusteru pro služby, které podporují značky s fakturace, značky se zobrazí v **značky** sloupce. Další informace najdete v tématu [porozumět vaší faktuře pro Microsoft Azure](../billing/billing-understand-your-bill.md).

![Najdete v části značky k fakturaci](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Další postup

* Pomocí vlastních zásad, můžete použít omezení a pravidla týkající se vašeho předplatného. Zásady, které definujete může vyžadovat, že všechny prostředky obsahovat hodnotu pro konkrétní značku. Další informace najdete v tématu [Co je Azure Policy?](../azure-policy/azure-policy-introduction.md).
* Úvod do pomocí Azure PowerShell, pokud nasazujete prostředky, najdete v části [použití Azure Powershellu s Azure Resource Manager](powershell-azure-resource-manager.md).
* Úvod do používání rozhraní příkazového řádku Azure, pokud nasazujete prostředky, najdete v části [pomocí rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).
* Úvod do portálu, najdete v části [použití portálu Azure ke správě prostředků Azure](resource-group-portal.md).  
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).
