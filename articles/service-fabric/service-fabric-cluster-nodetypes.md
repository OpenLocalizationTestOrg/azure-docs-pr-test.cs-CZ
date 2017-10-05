---
title: "Typy uzlů Service Fabric a škálovatelné sady virtuálních počítačů | Microsoft Docs"
description: "Popisuje, jak Service Fabric typy uzlů se týkají škálovatelné sady virtuálních počítačů a jak pro vzdálené připojení k do instance Škálovací sady virtuálního počítače nebo uzel clusteru."
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
ms.openlocfilehash: 3b1a22bb3653abb68fc73645ad2cb623fabc7736
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Vztah mezi typy uzlů Service Fabric a sady škálování virtuálního počítače
Sady škálování virtuálního počítače jsou prostředek Azure Compute, které můžete použít k nasazení a správě kolekci jako sada virtuálních počítačů. Každý typ uzlu, který je definován v clusteru Service Fabric je nastavený jako samostatné sady škálování virtuálního počítače. Každý typ uzlu je možné škálovat pak nebo dolů nezávisle, mají různé sady otevřené porty a může mít různé kapacity metriky.

Následující snímek obrazovky ukazuje cluster, který má dva typy uzlů: front-endové a back-end.  Každý typ uzlu má pět uzlů.

![Snímek obrazovky, který má dva typy uzlů clusteru][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>Mapování instance Škálovací sady virtuálního počítače na uzly
Jak je uvedeno výše, spustit instance Škálovací sady virtuálního počítače z instance, 0 a poté přejde nahoru. Číslování se projeví v názvech. Například BackEnd_0 je instance 0 sady škálování virtuálního počítače back-end. Tato konkrétní Škálovací sadě virtuálních počítačů má pět instancí s názvem BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 a BackEnd_4.

Při škálování Škálovací sadě virtuálních počítačů je vytvořena nová instance. Nový název instance Škálovací sady virtuálního počítače bude obvykle název sady škálování virtuálního počítače + číslo další instance. V našem příkladu je BackEnd_5.

## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Mapování škálování virtuálních počítačů pro každý uzel typu nebo virtuální počítač Škálovací sadu nastavení nástroje pro vyrovnávání zatížení
Pokud jste nasadili cluster z portálu nebo jste použili šablony Resource Manageru ukázkový, který jsme zadali, pak když získáte seznam všech prostředků ve skupině prostředků, se zobrazí u každé sady škálování virtuálního počítače nebo uzel typu služby Vyrovnávání zatížení.

Název by podobný: **LB -&lt;NodeType název&gt;**. Například LB-sfcluster4doc-0, jak je vidět na tomto snímku obrazovky:

![Zdroje][Resources]

## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Vzdálené připojení k do instance Škálovací sady virtuálního počítače nebo uzlu clusteru
Každý typ uzlu, který je definován v clusteru s podporou je nastavený jako samostatné sady škálování virtuálního počítače.  To znamená, že je možné rozšířit typy uzlů až nebo nezávisle a může být z jiné SKU virtuálních počítačů. Na rozdíl od jedné instance virtuálních počítačů Nezískávat instance Škálovací sady virtuálního počítače virtuální IP adresy s vlastními. Proto může být trochu náročné když hledáte IP adresu a port, který můžete použít pro vzdálené připojení ke konkrétní instanci.

Tady jsou kroky, pomocí kterých je zjišťovat.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Krok 1: Zjistěte virtuální IP adresu pro typ uzlu a potom pravidla příchozí NAT pro protokol RDP
Chcete-li získat, který, potřebujete získat hodnoty příchozích pravidel NAT, které byly definované jako součást definice prostředků pro **Microsoft.Network/loadBalancers**.

Na portálu, přejděte do okna nástroje pro vyrovnávání zatížení a potom **nastavení**.

![LBBlade][LBBlade]

V **nastavení**, klikněte na **pravidla příchozí NAT**. Tuto chybu získáte IP adresu a port, který můžete použít pro vzdálené připojení k první instance Škálovací sady virtuálního počítače. Na tomto snímku obrazovky je **104.42.106.156** a **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Krok 2: Najít se port, který můžete použít vzdálené připojení k instanci/uzlu konkrétní Škálovací sady virtuálního počítače na více systémů
Dříve v tomto dokumentu I věnovala mapování instance Škálovací sady virtuálního počítače do uzlu. Použijeme, a pokuste se zjistit přesnou port.

Porty jsou přiděleny ve vzestupném pořadí instance Škálovací sady virtuálního počítače. proto v tomto příkladu pro typ uzlu front-endu jsou tyto porty pro jednotlivé pět instancí. Teď je potřeba provést stejné mapování pro instance Škálovací sady virtuálního počítače.

| **Instance sady škálování virtuálního počítače** | **Port** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>Krok 3: Vzdálené připojení k konkrétní instance Škálovací sady virtuálního počítače
Na tomto snímku obrazovky I pro připojení k FrontEnd_1 použijte připojení ke vzdálené ploše:

![PROTOKOLU RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>Jak změnit hodnoty rozsahu portu protokolu RDP
### <a name="before-cluster-deployment"></a>Před nasazením clusteru
Když nastavujete clusteru pomocí šablony Resource Manager, můžete zadat rozsah v **inboundNatPools**.

Přejděte do definice prostředků pro **Microsoft.Network/loadBalancers**. V části, který najdete popis **inboundNatPools**.  Nahraďte *frontendPortRangeStart* a *frontendPortRangeEnd* hodnoty.

![InboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a>Po nasazení clusteru
Toto je složitější a může vést k recyklaci virtuálních počítačů. Nyní máte nastavit nové hodnoty pomocí Azure PowerShell. Ujistěte se, že je v počítači nainstalován prostředí Azure PowerShell 1.0 nebo novější. Pokud jste to před neučinili, I důrazně doporučujeme postupujte podle kroků uvedených v [postup instalace a konfigurace prostředí Azure PowerShell.](/powershell/azure/overview)

Přihlaste se k účtu Azure. Pokud z nějakého důvodu selže tento příkaz prostředí PowerShell, byste měli zkontrolovat, jestli máte Azure PowerShell správně nainstalován.

```
Login-AzureRmAccount
```

Spusťte následující příkaz a zobrazí podrobnosti o nástroj pro vyrovnávání zatížení a zobrazí hodnoty pro popis **inboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Nastaví *frontendPortRangeEnd* a *frontendPortRangeStart* na požadované hodnoty.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Další kroky
* [Přehled funkce "Nasazení odkudkoli" a porovnání s Azure spravované clustery](service-fabric-deploy-anywhere.md)
* [Zabezpečení clusteru](service-fabric-cluster-security.md)
* [Service Fabric SDK a Začínáme](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
