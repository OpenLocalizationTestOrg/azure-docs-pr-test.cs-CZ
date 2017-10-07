---
title: "cluster Service Fabric aaaScale příchozí nebo odchozí | Microsoft Docs"
description: "Cluster Service Fabric příchozí nebo odchozí toomatch vyžádání škálovat podle nastavení automatického škálování pravidel pro každý uzel typu nebo virtuální počítač škálovací sadu. Přidat nebo odebrat cluster Service Fabric tooa uzly"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: 37cfeaf80edc016cf6de017d1c2dc6fbcb8acc2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Škálování clusteru Service Fabric v nebo pomocí pravidel automatického škálování
Sady škálování virtuálního počítače jsou prostředek výpočtů Azure, můžete použít toodeploy a spravovat kolekci virtuálních počítačů jako sada. Každý typ uzlu, který je definován v clusteru Service Fabric je nastavený jako samostatnou sadu škálování virtuálního počítače. Každý typ uzlu můžete škálovat v nebo na nezávisle, mají různé sady otevřené porty a může mít různé kapacity metriky. Další informace o jeho hello [nodetypes Service Fabric](service-fabric-cluster-nodetypes.md) dokumentu. Vzhledem k tomu, že hello Service Fabric typy uzlů v clusteru jsou tvořeny sady škálování virtuálního počítače v back-end hello, musíte pro každý uzel typu nebo virtuální počítač škálovací sadu tooset až pravidel automatického škálování.

> [!NOTE]
> Vaše předplatné musí mít dostatečný počet jader tooadd hello nové virtuální počítače, které tvoří tento cluster. Proto čas selhání nasazení, když se některé z hello kvótami dosáhl neexistuje žádné ověření modelu v současné době.
> 
> 

## <a name="choose-hello-node-typevirtual-machine-scale-set-tooscale"></a>Vyberte nastavení tooscale hello uzel typu nebo virtuální počítače
V současné době nejsou možné toospecify hello automatickému škálování pravidla pro sady škálování virtuálního počítače pomocí portálu hello, takže dejte nám pomocí typy uzlů hello toolist prostředí Azure PowerShell (1.0 +) a poté přidejte toothem pravidla automatické škálování.

tooget hello seznam virtuálního počítače sady škálování, které tvoří cluster, spusťte následující rutiny hello:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <Virtual Machine scale set name>
```

## <a name="set-auto-scale-rules-for-hello-node-typevirtual-machine-scale-set"></a>Nastavení automatického škálování pravidel pro škálovací sadu hello uzel typu nebo virtuálních počítačů
Pokud váš cluster má více typů uzlu, pak opakujte že nastaví tato pro každý uzel typy nebo virtuální počítače, který má tooscale (příchozí nebo odchozí). Trvat do účtu hello počet uzlů, musí mít před nastavením automatické škálování. minimální počet uzlů, které musí být pro typ primárního uzlu hello Hello doprovází hello úroveň spolehlivosti, kterou jste vybrali. Další informace o [úrovní spolehlivosti](service-fabric-cluster-capacity.md).

> [!NOTE]
> Škálování dolů hello primárního uzlu typu tooless, než je minimální počet hello stát nestabilní hello clusteru nebo tím, že ji. To může vést ke ztrátě dat pro vaše aplikace a hello systémových služeb.
> 
> 

Funkce automatického škálování hello není aktuálně doprovází hello zatížením, že vaše aplikace může reporting tooService prostředků infrastruktury. Proto v tento čas hello automatické škálování, které máte čistě doprovází hello čítače výkonu, které jsou vygenerované každou instancí sady škálování virtuálního počítače hello.  

Postupujte podle těchto pokynů [tooset až automatické škálování pro každý škálovací sadu virtuálních počítačů](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> V vertikálně scénář, pokud váš typ uzlu má úroveň odolnosti zlatý nebo Silver musíte toocall hello [rutinu Remove-ServiceFabricNodeState](https://msdn.microsoft.com/library/azure/mt125993.aspx) s názvem příslušný uzel hello.
> 
> 

## <a name="manually-add-vms-tooa-node-typevirtual-machine-scale-set"></a>Ručně přidejte virtuální počítače tooa uzel typu nebo virtuální počítač škálovací sadu
Postupujte podle hello ukázka nebo pokynů v hello [Galerie šablon úvodní](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) toochange hello počet virtuálních počítačů v každém uzlu. 

> [!NOTE]
> Přidání virtuálních počítačů má čas, proto Nečekejte hello dodatky toobe okamžitou. Proto plánování kapacity tooadd i v průběhu času, tooallow pro více než 10 minut, než je k dispozici pro repliky hello hello kapacity virtuálního počítače nebo služby tooget instancí, které jsou umístěny.
> 
> 

## <a name="manually-remove-vms-from-hello-primary-node-typevirtual-machine-scale-set"></a>Ručně odeberte z hello primárního uzlu typu nebo virtuální počítač škálovací sadu virtuálních počítačů
> [!NOTE]
> v typu hello primárního uzlu v clusteru spusťte Hello service fabric systémových služeb. Tak, aby měli nikdy vypnout nebo snižovat hello počet instancí v tomto uzlu typech poskytuje na menší, než jakou úroveň spolehlivosti hello. Odkazovat příliš[hello informace o spolehlivosti vrstev zde](service-fabric-cluster-capacity.md). 
> 
> 

Je třeba tooexecute hello následující kroky jedna instance virtuálních počítačů současně. To umožňuje, aby hello systémových služeb (a vaše stavové služby) toobe řádné ukončení v instanci virtuálního počítače hello odebíráte a nové repliky vytvořené na jiných uzlech.

1. Spustit [zakázat ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) s úmyslem 'RemoveNode' toodisable hello uzlu budete tooremove (nejvyšší instance pro hello v tomto typu uzlu).
2. Spustit [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) toomake, že tento uzel hello skutečně přešla toodisabled. Pokud ne, počkejte, dokud hello uzlů je zakázáno. Tento krok nelze hurry.
3. Postupujte podle hello ukázka nebo pokynů v hello [Galerie šablon úvodní](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) toochange hello počet virtuálních počítačů pomocí jedné v tomto uzlu. instance Hello odebrat je hello nejvyšší instance virtuálního počítače. 
4. Opakujte kroky 1 až 3 podle potřeby, ale nikdy snižovat hello počet instancí v primárním uzlu typy hello menší než zaručuje, jakou úroveň spolehlivosti hello. Odkazovat příliš[hello informace o spolehlivosti vrstev zde](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-hello-non-primary-node-typevirtual-machine-scale-set"></a>Ručně odeberte z hello není primární uzel typu nebo virtuální počítač škálovací sadu virtuálních počítačů
> [!NOTE]
> Pro stavové služby budete potřebovat k určitému počtu uzlů toobe vždy toomaintain dostupnosti a zachovat stav služby. V hello velmi minimální je nutné hello počet počet sada uzlů rovna toohello cíl repliky hello oddílu nebo služby. 
> 
> 

Hello musíte provést následující kroky jedna instance virtuálních počítačů současně hello. To umožňuje, aby hello systémových služeb (a vaše stavové služby) toobe řádné ukončení na hello instance virtuálního počítače, kterou chcete odebrat a vytvořit nové repliky else where.

1. Spustit [zakázat ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) s úmyslem 'RemoveNode' toodisable hello uzlu budete tooremove (nejvyšší instance pro hello v tomto typu uzlu).
2. Spustit [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) toomake, že tento uzel hello skutečně přešla toodisabled. V opačném případě počkejte na hello uzlů je zakázáno. Tento krok nelze hurry.
3. Postupujte podle hello ukázka nebo pokynů v hello [Galerie šablon úvodní](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) toochange hello počet virtuálních počítačů pomocí jedné v tomto uzlu. Tato akce odebere teď hello nejvyšší instance virtuálního počítače. 
4. Opakujte kroky 1 až 3 podle potřeby, ale nikdy snižovat hello počet instancí v primárním uzlu typy hello menší než zaručuje, jakou úroveň spolehlivosti hello. Odkazovat příliš[hello informace o spolehlivosti vrstev zde](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Chování můžete pozorovat v Service Fabric Exploreru
Při škálování hello clusteru Service Fabric Explorer se projeví hello počet uzlů (instancí sady škálování virtuálního počítače), které jsou součástí clusteru hello.  Při změně měřítka však se cluster dolů můžete zobrazí instance uzlu nebo virtuálního počítače hello odebrat nezobrazí v pořádku, pokud zavoláte [odebrat ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx) s názvem příslušný uzel hello.   

Zde je hello vysvětlení tohoto chování.

Hello uvedené v Service Fabric Explorer jsou uzly odraz jaké hello Service Fabric systémových služeb (FM konkrétně) ví o hello počtu uzlů clusteru hello měl má. Při změně měřítka hello sad škálování virtuálního počítače, hello virtuální počítač byl odstraněn, ale služba system FM stále předpokládá, že tento uzel hello, (který byl namapované toohello virtuálního počítače, který byl odstraněn), vrátí se znovu. Proto Service Fabric Explorer pokračuje toodisplay tento uzel (i když hello stav může být chyba nebo neznámá).

V pořadí toomake jistotu, že uzel je odebrán, když dojde k odebrání virtuálního počítače máte dvě možnosti:

1) Zvolte si úroveň odolnosti zlatý nebo Silver (k dispozici brzy) pro hello typy uzlů v clusteru, který vám dává hello integrace infrastruktury. Které pak automaticky odebere hello uzly z našich služeb (FM) stavu systému při změně měřítka.
Odkazovat příliš[hello podrobnosti o sem úrovně odolnosti](service-fabric-cluster-capacity.md)

2) Jakmile zmenšování hello instance virtuálního počítače je nutné toocall hello [rutinu Remove-ServiceFabricNodeState](https://msdn.microsoft.com/library/mt125993.aspx).

> [!NOTE]
> Clustery služby infrastruktury vyžadují počet uzlů toobe až na celou dobu hello v pořadí toomaintain dostupnosti a zachovat stav – odkazované tooas "údržbu kvora." Proto, pokud jste provedli nejprve je obvykle unsafe tooshut dolů všechny hello počítače v clusteru hello [úplného zálohování vaší stavu](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Další kroky
Čtení hello následující tooalso Další informace o plánování kapacity clusteru, upgradu clusteru a rozdělení do oddílů služby:

* [Plánování kapacity vašeho clusteru](service-fabric-cluster-capacity.md)
* [Upgrade clusteru](service-fabric-cluster-upgrade.md)
* [Oddíl stavové služby pro maximální měřítko](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
