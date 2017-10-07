---
title: "aaaDelete Azure cluster a jeho prostředky | Microsoft Docs"
description: "Zjistěte, jak odstranit toocompletely Service Fabric clusteru buď odstraňování skupiny prostředků hello obsahující hello clusteru nebo selektivně odstraněním prostředky."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: de422950-2d22-4ddb-ac47-dd663a946a7e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/24/2017
ms.author: chackdan
ms.openlocfilehash: 5c15a4184644da715cd69397f2150de86ab433ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-service-fabric-cluster-on-azure-and-hello-resources-it-uses"></a>Odstranění clusteru Service Fabric na Azure a hello prostředky, které používá
Cluster Service Fabric se skládá z mnoha dalším prostředkům služby Azure kromě toohello prostředek clusteru, sám sebe. Takže toocompletely odstranit cluster Service Fabric je také nutné toodelete všechny prostředky, které se provádí z hello.
Máte dvě možnosti: Skupina prostředků buď odstranit hello hello clusteru je v (která odstraňuje hello prostředku clusteru a všechny další prostředky ve skupině prostředků hello) nebo konkrétně odstranit prostředek clusteru hello a je přiřazen prostředky (ale ne jiné prostředky ve skupině prostředků hello).

> [!NOTE]
> Odstranění prostředku clusteru hello **nemá** odstranit všechny další prostředky, které váš cluster Service Fabric tvoří hello.
> 
> 

## <a name="delete-hello-entire-resource-group-rg-that-hello-service-fabric-cluster-is-in"></a>Odstraňte skupinu prostředků celý hello (RG), který hello Cluster Service Fabric
Toto je hello nejjednodušší způsob, jak tooensure odstranit všechny prostředky hello přidruženého k vašemu clusteru, včetně hello skupinu prostředků. Odstraněním skupiny prostředků hello pomocí prostředí PowerShell nebo prostřednictvím hello portálu Azure. Pokud vaše skupina prostředků má prostředky, které nejsou clusteru související tooService infrastruktury, můžete odstranit konkrétní prostředky.

### <a name="delete-hello-resource-group-using-azure-powershell"></a>Odstraňte skupinu prostředků hello pomocí Azure PowerShell
Můžete také odstranit skupinu prostředků hello spuštěním následující rutiny prostředí Azure PowerShell hello. Ujistěte se, že Azure PowerShell 1.0 nebo vyšší je v počítači nainstalována. Pokud jste to před neudělali, postupujte podle kroků hello uvedených v [jak tooinstall a konfigurace prostředí Azure PowerShell.](/powershell/azure/overview)

Otevřete okno prostředí PowerShell a spusťte následující rutiny PS hello:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Zobrazí se výzva tooconfirm odstranění hello, pokud jste nepoužili hello *-Force* možnost. Na potvrzovací hello RG a jsou odstraněny všechny hello prostředky, které obsahuje.

### <a name="delete-a-resource-group-in-hello-azure-portal"></a>Odstranění skupiny prostředků v hello portálu Azure
1. Přihlášení toohello [portál Azure](https://portal.azure.com).
2. Přejděte cluster Service Fabric toohello chcete toodelete.
3. Klikněte na hello název skupiny prostředků na stránce essentials hello clusteru.
4. Otevře hello **Essentials skupiny prostředků** stránky.
5. Klikněte na **Odstranit**.
6. Postupujte podle pokynů hello na této stránce toocomplete hello odstranění skupiny prostředků hello.

![Odstranění skupiny prostředků][ResourceGroupDelete]

## <a name="delete-hello-cluster-resource-and-hello-resources-it-uses-but-not-other-resources-in-hello-resource-group"></a>Odstranit prostředek clusteru hello a hello prostředky, které používá, ale ne další prostředky ve skupině prostředků hello
Pokud skupina prostředků má jenom prostředky, které se cluster Service Fabric související toohello chcete toodelete, pak je snazší toodelete hello celé skupiny prostředků. Pokud chcete tooselectively odstranění hello prostředky po jednom ve vaší skupině prostředků, postupujte podle těchto kroků.

Pokud jste nasadili cluster pomocí hello portálu nebo pomocí jednoho ze šablony služby Správce prostředků infrastruktury hello z Galerie šablon hello, pak všechny hello prostředky, které hello používá clusteru jsou označené hello následující dvě značky. Můžete je použít toodecide prostředky, ke kterým chcete toodelete.

***Značka č. 1:*** klíč = clusterName, hodnota = "název clusteru hello.

***Značka č. 2:*** klíč = resourceName, hodnota = ServiceFabric

### <a name="delete-specific-resources-in-hello-azure-portal"></a>Odstranit konkrétní prostředky v hello portálu Azure
1. Přihlášení toohello [portál Azure](https://portal.azure.com).
2. Přejděte cluster Service Fabric toohello chcete toodelete.
3. Přejděte příliš**všechna nastavení** v okně essentials hello.
4. Klikněte na **značky** pod **Správa prostředků** v okně Nastavení hello.
5. Klikněte na jednu z hello **značky** v hello značky okno tooget seznam všech prostředků hello s touto značkou.
   
    ![Značky prostředku][ResourceTags]
6. Jakmile máte seznam hello s příznakem prostředků, klikněte na každou hello prostředků a odstraňte je.
   
    ![Prostředky s příznakem][TaggedResources]

### <a name="delete-hello-resources-using-azure-powershell"></a>Odstranit hello prostředků pomocí Azure PowerShell
Spuštěním následující rutiny prostředí Azure PowerShell hello můžete odstranit hello prostředky po jednom. Ujistěte se, že Azure PowerShell 1.0 nebo vyšší je v počítači nainstalována. Pokud jste to před neudělali, postupujte podle kroků hello uvedených v [jak tooinstall a konfigurace prostředí Azure PowerShell.](/powershell/azure/overview)

Otevřete okno prostředí PowerShell a spusťte následující rutiny PS hello:

```powershell
Login-AzureRmAccount
```
Pro každou hello prostředků má toodelete, spusťte následující hello:

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of hello resource group>" -Force
```

toodelete hello prostředek clusteru, spusťte následující hello:

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of hello resource group>" -Force
```

## <a name="next-steps"></a>Další kroky
Čtení hello následující tooalso Další informace o upgradu clusteru a rozdělení do oddílů služby:

* [Další informace o upgrade clusteru](service-fabric-cluster-upgrade.md)
* [Další informace o vytváření oddílů stavové služby pro maximální měřítko](service-fabric-concepts-partitioning.md)

<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
