---
title: "aaaCreate Azure Service Fabric clusterů v systému Windows Server a Linux | Microsoft Docs"
description: "Spusťte na serveru Windows a Linux, což znamená, že byste clusterů Service Fabric budete mít možnost toodeploy a aplikací Service Fabric, kdekoli můžete spustit systém Windows Server nebo Linux."
services: service-fabric
documentationcenter: .net
author: Chackdan
manager: timlt
editor: 
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/08/2017
ms.author: chackdan
ms.openlocfilehash: 46d5f3d019339c57a0024f5a9d47d9018cca01a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Vytvoření clusterů Service Fabric na Windows Server nebo Linux
Cluster služby Azure Service Fabric je sada virtuální nebo fyzické počítače, do kterých jsou nasazené a spravovat vaše mikroslužeb připojené k síti. Počítač nebo virtuální počítač, který je součástí clusteru, se nazývá uzlem clusteru. Clustery můžete škálovat toothousands uzlů. Pokud chcete přidat nový cluster toohello uzly, Service Fabric znovu vytvoří rovnováhu hello repliky oddílu služby a instance napříč hello zvýšit počet uzlů. Celkové zlepšuje výkon aplikace a snižuje kolize pro toomemory přístup. Pokud hello uzly v clusteru hello nejsou používány efektivně, může snížit hello počet uzlů v clusteru hello. Service Fabric znovu vytvoří znovu rovnováhu replik hello oddílů a lepší využití instance napříč hello zmenšit počet uzlů toomake hello hardwaru na každém uzlu.

Service Fabric umožňuje hello vytváření clusterů Service Fabric na všechny virtuální počítače nebo počítače se systémem Windows Server nebo Linux. To znamená, jsou možné toodeploy a spouštět aplikace Service Fabric v jakémkoli prostředí, kde máte sadu Windows Server nebo Linux počítače, které jsou vzájemně provázány, je jej na místních počítačích Microsoft Azure nebo kteréhokoli poskytovatele cloudových služeb.

## <a name="create-service-fabric-clusters-on-azure"></a>Vytvoření clusterů Service Fabric v Azure
Vytvoření clusteru v Azure se provádí prostřednictvím šablony modelu prostředků nebo hello portálu Azure. Čtení [vytvořit cluster Service Fabric pomocí šablony Resource Manageru](service-fabric-cluster-creation-via-arm.md) nebo [z hello portálu Azure vytvořit cluster Service Fabric](service-fabric-cluster-creation-via-portal.md) Další informace.

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Podporované operační systémy pro clustery v Azure
Budete mít toocreate clustery na virtuálních počítačích s těmito operačními systémy:

* Windows Server 2012 R2
* Windows Server 2016 
* 16.04 Ubuntu Linux (ve verzi public preview) 

## <a name="create-service-fabric-standalone-clusters-on-premises-or-with-any-cloud-provider"></a>Vytvořit samostatný Service Fabric clustery v místě nebo kteréhokoli poskytovatele cloudových služeb
Service Fabric nabízí balíček instalace pro jste toocreate samostatné Service Fabric clustery místně nebo na všechny poskytovatele cloudových služeb

Další informace o nastavení infrastruktury služby samostatné clustery v systému Windows Server, přečtěte si [vytváření clusteru Service Fabric pro systém Windows Server](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Všechna nasazení cloudu a místních nasazení
Hello proces vytváření místní cluster Service Fabric je podobný toohello proces vytváření clusteru s podporou na všechny cloudové zvoleného sadu virtuálních počítačů. Hello počáteční kroky tooprovision hello virtuální počítače se řídí hello poskytovatele cloudové nebo místní prostředí, který používáte. Až budete mít sadu virtuálních počítačů s připojení k síti povolena mezi nimi, pak hello kroky tooset hello balíček Service Fabric, upravit nastavení clusteru hello a spustit hello vytváření clusteru a správy skriptů jsou identické. To zajišťuje, že znalosti a zkušenosti provoz a správu clusterů Service Fabric převoditelné když zvolíte tootarget nové hostitelských prostředích.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Výhody vytvoření samostatných clusterů Service Fabric
* Jsou volné toochoose žádné cloudu toohost zprostředkovatele clusteru.
* Aplikace Service Fabric, jednou zapsána, můžete spustit v prostředí s více hostitelských s minimálním toono změny.
* Informace o vytváření aplikací Service Fabric se přenesou z jednoho tooanother hostování prostředí.
* Provozní prostředí, jak spouštět a spravovat Service Fabric clusterů má u sebe přes z jednoho prostředí tooanother.
* Široká zákazníka reach je bez vazby hostování prostředí omezení.
* Další úroveň spolehlivosti a ochranu proti výpadkům rozšířeným existuje, protože můžete přesunout hello služby přes prostředí nasazení tooanother Pokud datovém centru nebo poskytovatele cloudových služeb má přerušení spojení.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Podporované operační systémy pro samostatné clustery
Budete mít toocreate clustery na virtuální počítače nebo počítačů s těmito operačními systémy:

* Windows Server 2012 R2
* Windows Server 2016 
* Linux (už brzy)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Vytvořit místní výhod clusterů Service Fabric v Azure přes samostatnou bitovou clusterů Service Fabric
Clusterů Service Fabric systémem Azure poskytuje výhod oproti možnost místní hello, takže pokud nemáte konkrétní potřeby kterém spouštíte clusterů, pak doporučujeme vám spustit v Azure. V Azure poskytujeme integrace s dalšími funkce Azure a službami, které umožňuje operace a Správa clusteru hello jednodušší a spolehlivější.

* **Portál Azure:** portál Azure umožňuje snadno toocreate a Správa clusterů.
* **Azure Resource Manager:** pomocí nástroje Azure Resource Manager umožňuje snadnou správu všechny prostředky používané hello clusteru jako jednotku a zjednodušuje sledování nákladů a fakturace.
* **Cluster Service Fabric jako prostředek Azure** cluster A Service Fabric je prostředek ARM, abyste mohli ho stejně jako ostatní prostředky ARM v Azure.
* **Integrace s infrastruktury Azure** Service Fabric koordinuje s hello základní infrastrukturu Azure operačního systému, sítě a další upgrady tooimprove dostupnosti a spolehlivosti aplikací.  
* **Diagnostika:** v Azure, poskytujeme integraci s Azure diagnostiku a analýzy protokolů.
* **Automatické škálování:** pro clustery v Azure, poskytujeme integrovanou funkci automatického škálování z důvodu tooVirtual škálovací sady počítačů. V jiných prostředích cloudu a místní máte toobuild vlastní automatické škálování funkce nebo škálování ručně pomocí rozhraní API, která zveřejňuje Service Fabric hello škálování clusterů.

## <a name="next-steps"></a>Další kroky

* Vytvoření clusteru s podporou na virtuální počítače nebo počítače se systémem Windows Server: [vytváření clusteru Service Fabric pro systém Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Vytvoření clusteru s podporou na virtuální počítače nebo počítače se systémem Linux: [Service Fabric v systému Linux](service-fabric-linux-overview.md)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

