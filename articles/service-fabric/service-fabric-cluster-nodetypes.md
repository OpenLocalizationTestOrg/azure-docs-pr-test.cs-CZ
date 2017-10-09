---
title: "aaaService Fabric typů uzlů a škálovatelné sady virtuálních počítačů | Microsoft Docs"
description: "Popisuje, jak Service Fabric typy uzlů vztahují tooVM sady škálování a připojení tooremote tooa instance Škálovací sady virtuálního počítače nebo uzel clusteru."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: chackdan
ms.openlocfilehash: 830ea2816f5864de146a77483c85de26f91c2425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Hello vztah mezi typy uzlů Service Fabric a sady škálování virtuálního počítače
Sady škálování virtuálního počítače jsou prostředek Azure Compute můžete použít toodeploy a spravovat kolekci virtuálních počítačů jako sada. Každý typ uzlu, který je definován v clusteru Service Fabric je nastavený jako samostatné sady škálování virtuálního počítače. Každý typ uzlu je možné škálovat pak nebo dolů nezávisle, mají různé sady otevřené porty a může mít různé kapacity metriky.

Hello následující snímek obrazovky ukazuje cluster, který má dva typy uzlů: front-endové a back-end.  Každý typ uzlu má pět uzlů.

![Snímek obrazovky, který má dva typy uzlů clusteru][NodeTypes]

## <a name="mapping-vm-scale-set-instances-toonodes"></a>Mapování toonodes instance Škálovací sady virtuálního počítače
Jak je uvedeno výše, spusťte z instance 0 hello instance Škálovací sady virtuálního počítače a pak se zvyšuje. číslování Hello se projeví v názvech hello. Například BackEnd_0 je instance 0 hello sady škálování virtuálních počítačů v back-end. Tato konkrétní Škálovací sadě virtuálních počítačů má pět instancí s názvem BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 a BackEnd_4.

Při škálování Škálovací sadě virtuálních počítačů je vytvořena nová instance. Hello nové sady škálování virtuálního počítače název instance bude obvykle název sady škálování virtuálního počítače hello + hello další instance číslo. V našem příkladu je BackEnd_5.

## <a name="mapping-vm-scale-set-load-balancers-tooeach-node-typevm-scale-set"></a>Mapování virtuálních počítačů sady škálování zatížení nástroje pro vyrovnávání tooeach uzel typu nebo virtuálních počítačů sady škálování
Pokud jste nasadili cluster z portálu hello nebo použili šablony Resource Manageru ukázka hello, který jsme zadali, pak když získáte seznam všech prostředků ve skupině prostředků, uvidíte hello služby Vyrovnávání zatížení pro každý typ Škálovací sady virtuálního počítače nebo uzel.

Hello název by podobný: **LB -&lt;NodeType název&gt;**. Například LB-sfcluster4doc-0, jak je vidět na tomto snímku obrazovky:

![Zdroje][Resources]

## <a name="remote-connect-tooa-vm-scale-set-instance-or-a-cluster-node"></a>Vzdálené připojení tooa instance Škálovací sady virtuálního počítače nebo uzlu clusteru
Každý typ uzlu, který je definován v clusteru s podporou je nastavený jako samostatné sady škálování virtuálního počítače.  Aby znamená hello typy uzlů je možné rozšířit až nebo nezávisle a může být z jiné SKU virtuálních počítačů. Na rozdíl od jedné instance virtuálních počítačů instance Škálovací sady virtuálního počítače hello Nezískávat virtuální IP adresy s vlastními. Aby mohli být chvíli náročné, když hledáte IP adresu a port, který můžete použít tooremote připojit tooa konkrétní instanci.

Tady jsou kroky hello toodiscover můžete postupovat podle nich.

### <a name="step-1-find-out-hello-virtual-ip-address-for-hello-node-type-and-then-inbound-nat-rules-for-rdp"></a>Krok 1: Zjistěte hello virtuální IP adresy pro typ uzlu hello a potom pravidla příchozí NAT pro protokol RDP
V pořadí tooget, je nutné, aby tooget hello příchozích pravidel NAT pravidla hodnoty, které byly definované jako součást hello definice prostředků pro **Microsoft.Network/loadBalancers**.

Hello portálu, přejděte okno nástroje pro vyrovnávání zatížení toohello a potom **nastavení**.

![LBBlade][LBBlade]

V **nastavení**, klikněte na **pravidla příchozí NAT**. Tato teď poskytuje hello IP adresu a port, které můžete použít tooremote připojit toohello první instance Škálovací sady virtuálního počítače. Na snímku obrazovky hello níže, je **104.42.106.156** a **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-hello-port-that-you-can-use-tooremote-connect-toohello-specific-vm-scale-set-instancenode"></a>Krok 2: Připojení zjistěte hello portu, které můžete použít tooremote toohello konkrétní sady škálování virtuálního počítače instance/uzlu
Dříve v tomto dokumentu I věnovala tomu, jak toohello uzlů mapování hello instance Škálovací sady virtuálního počítače. Budeme používat tento toofigure hello přesný portu.

Hello porty jsou přiděleny ve vzestupném pořadí hello instance Škálovací sady virtuálního počítače. v mé příklad hello typ uzlu front-endu, tak hello porty pro každou hello pět instancí jsou následující hello. je teď třeba toodo hello stejné mapování pro instance Škálovací sady virtuálního počítače.

| **Instance sady škálování virtuálního počítače** | **Port** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

### <a name="step-3-remote-connect-toohello-specific-vm-scale-set-instance"></a>Krok 3: Vzdáleného připojení toohello konkrétní instance Škálovací sady virtuálního počítače
V následující snímek obrazovky hello použít připojení ke vzdálené ploše tooconnect toohello FrontEnd_1:

![PROTOKOLU RDP][RDP]

## <a name="how-toochange-hello-rdp-port-range-values"></a>Jak toochange hello portu RDP rozsahu hodnot
### <a name="before-cluster-deployment"></a>Před nasazením clusteru
Když nastavujete hello clusteru pomocí šablony Resource Manager, můžete zadat rozsah hello v hello **inboundNatPools**.

Přejděte toohello definice prostředků **Microsoft.Network/loadBalancers**. V části, který najdete popis hello **inboundNatPools**.  Nahraďte hello *frontendPortRangeStart* a *frontendPortRangeEnd* hodnoty.

![InboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a>Po nasazení clusteru
Toto je složitější a může vést k virtuálním počítačům hello recyklaci. Nyní máte tooset nových hodnot, pomocí prostředí Azure PowerShell. Ujistěte se, že je v počítači nainstalován prostředí Azure PowerShell 1.0 nebo novější. Pokud jste to před neučinili, I důrazně doporučujeme postupujte podle kroků hello uvedených v [jak tooinstall a konfigurace prostředí Azure PowerShell.](/powershell/azure/overview)

Přihlaste se tooyour účet Azure. Pokud z nějakého důvodu selže tento příkaz prostředí PowerShell, byste měli zkontrolovat, jestli máte Azure PowerShell správně nainstalován.

```
Login-AzureRmAccount
```

Spustit hello následujících podrobnostech tooget na nástroj pro vyrovnávání zatížení a zobrazí hello hodnoty pro popis hello **inboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Nastaví *frontendPortRangeEnd* a *frontendPortRangeStart* toohello hodnoty, které mají.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use hello API version that get returned> -Force
```


## <a name="next-steps"></a>Další kroky
* [Přehled funkce "Kdekoli nasadit" hello a porovnání s Azure spravované clustery](service-fabric-deploy-anywhere.md)
* [Zabezpečení clusteru](service-fabric-cluster-security.md)
* [Service Fabric SDK a Začínáme](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
