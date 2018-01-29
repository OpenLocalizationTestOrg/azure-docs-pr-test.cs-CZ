---
title: "Aktualizovat prostředky ve službě Azure spravované aplikace | Microsoft Docs"
description: "Popisuje, jak pracovat s prostředky v spravovanou skupinu prostředků Azure spravované aplikace."
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/26/2017
ms.author: tomfitz
ms.openlocfilehash: d3c955d7be0e7e6d45751c0e685bad498e524d94
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="work-with-resources-in-the-managed-resource-group-for-azure-managed-application"></a>Pracovat s prostředky v spravovanou skupinu prostředků pro Azure spravované aplikace

Tento článek popisuje postup aktualizace prostředky, které jsou nasazeny jako součást spravované aplikace. Jako vydavatel spravované aplikace máte přístup k prostředkům ve skupině spravovaných prostředků. K aktualizaci těchto prostředků, potřebujete najít skupinu spravovaných prostředků, které jsou přidružené k spravované aplikace a přístup k prostředku v příslušné skupině prostředků.

Tento článek předpokládá, že jste nasadili spravované aplikace [spravované webové aplikace (IaaS) pomocí služby Azure pro](https://github.com/Azure/azure-managedapp-samples/tree/master/samples/201-managed-web-app) ukázkový projekt. Zahrnuje spravované aplikace **Standard_D1_v2** virtuálního počítače. Pokud jste nenasadili spravované aplikace, stále můžete tento článek se seznámit s kroky pro aktualizaci skupinu spravovaných prostředků.

Následující obrázek znázorňuje nasazené spravované aplikace.

![Nasadit spravované aplikace](./media/update-managed-resources/deployed.png)

V tomto článku použijte rozhraní příkazového řádku Azure pro:

* Identifikovat spravované aplikace
* Identifikovat skupinu spravovaných prostředků
* Identifikovat prostředky virtuálního počítače ve skupině spravovaných prostředků
* Změnit velikost virtuálního počítače (buď menší velikost, pokud nejsou použity nebo větší pro podporu více zatížení)
* Přiřadit zásady pro skupinu spravovaných prostředků, která určuje povolených umístění

## <a name="get-managed-application-and-managed-resource-group"></a>Spravované aplikace a skupinu spravovaných prostředků

Spravované aplikace ve skupině prostředků, použijte:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')]"
```

ID skupiny spravovaných prostředků, použijte:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')].{ managedResourceGroup:managedResourceGroupId }"
```

## <a name="resize-vms-in-managed-resource-group"></a>Změna velikosti virtuálních počítačů ve skupině spravovaných prostředků

Informace o virtuálních počítačů ve skupině spravovaných prostředků, zadejte název skupiny spravovaných prostředků.

```azurecli-interactive
az vm list -g DemoApp6zkevchqk7sfq --query "[].{VMName:name,OSType:storageProfile.osDisk.osType,VMSize:hardwareProfile.vmSize}"
```

Chcete-li aktualizovat velikost virtuálního počítače, použijte:

```azurecli-interactive
az vm resize --size Standard_D2_v2 --ids $(az vm list -g DemoApp6zkevchqk7sfq --query "[].id" -o tsv)
```

Po dokončení operace, ověřte, zda že je aplikace spuštěna na standardní D2 v2.

![Spravované aplikace pomocí standardní D2 v2](./media/update-managed-resources/upgraded.png)

## <a name="apply-policy-to-managed-resource-group"></a>Použít zásady do skupiny spravovaných prostředků

Získejte skupinu spravovaných prostředků a přiřazení zásad v tomto oboru. Zásady **e56962a6-4747-49cd-b67b-bf8b01975c4c** předdefinované zásady pro zadání povolených umístění.

```azurecli-interactive
managedGroup=$(az managedapp show --name <app-name> --resource-group DemoApp --query managedResourceGroupId --output tsv)

az policy assignment create --name locationAssignment --policy e56962a6-4747-49cd-b67b-bf8b01975c4c --scope $managedGroup --params '{
                            "listofallowedLocations": {
                                "value": [
                                    "northeurope",
                                    "westeurope"
                                ]
                            }
                        }'
```

Pokud chcete zobrazit povolených umístění, použijte:

```azurecli-interactive
az policy assignment show --name locationAssignment --scope $managedGroup --query parameters.listofallowedLocations.value
```

Přiřazení zásady se zobrazí na portálu.

![Přiřazení zásady zobrazení](./media/update-managed-resources/assignment.png)

## <a name="next-steps"></a>Další kroky

* Úvod ke spravovaným aplikacím najdete v [přehledu spravovaných aplikací](overview.md).
* Ukázkové projekty, najdete v části [ukázkové projekty Azure spravované aplikace](sample-projects.md).
