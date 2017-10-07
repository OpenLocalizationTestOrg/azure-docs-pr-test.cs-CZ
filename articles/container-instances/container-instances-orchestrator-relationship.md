---
title: "aaaAzure instancí kontejnerů a kontejner Orchestration"
description: "Pochopit, jak Azure kontejner instancí interakci s orchestrators kontejneru"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 69a39edc6f14d885c1ac300990ed1399002ccfee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Azure instancí kontejnerů a kontejner orchestrators

Kvůli jejich malá velikost a orientaci aplikace je výhodné pro agilní doručení prostředí a na základě mikroslužbu architektury kontejnery. Hello úlohy automatizace a správa velký počet kontejnerů a jejich vzájemné interakce se označuje jako *orchestration*. Oblíbených kontejneru orchestrators zahrnují Kubernetes, DC/OS a Docker Swarm, které jsou k dispozici v hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).

Azure instancí kontejneru obsahuje některé z hello základní plánovací orchestration platforem, ale nezahrnuje hello vyšší hodnota služby, aby tyto platformy poskytují a může být ve skutečnosti doplňkové s nimi. Tento článek popisuje hello oboru co zpracovává instancí kontejnerů Azure a jak úplné orchestrators kontejner může pracovat s ním.

## <a name="traditional-orchestration"></a>Tradiční orchestration

standardní definice Hello Orchestrace zahrnuje hello následující úlohy:

- **Plánování**: daného kontejneru image a žádost o prostředek, najít vhodný počítač, na který toorun hello kontejner.
- **Spřažení/proti-affinity**: určení, že by měl sadu kontejnery spuštěny nedaleko druhou (pro výkon) nebo dostatečně daleko od sebe (pro dostupnost).
- **Sledování stavu**: Podívejte se na chyby kontejneru a automaticky znovu je naplánujte.
- **Převzetí služeb při selhání**: sledovat co běží na každém počítači a změnit plán naplánovaných kontejnery z uzlů toohealthy počítače se nezdařilo.
- **Škálování**: Přidání nebo odebrání kontejneru instancí toomatch vyžádání, ručně nebo automaticky.
- **Sítě**: zadejte překryvné sítě pro spolupráci kontejnery toocommunicate napříč více hostitelských počítačích.
- **Zjišťování služby**: Povolit kontejnery toolocate navzájem automaticky to i v případě jejich přechodu mezi hostitele počítače a změnit IP adresy.
- **Koordinované upgradů aplikací**: Spravovat aplikace tooavoid upgrady kontejnerů výpadek a povolte vrácení zpět, pokud dojde k chybě.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Orchestration s instancemi Azure kontejneru: vrstveného přístupu

Azure instancí kontejnerů umožňuje tooorchestration vrstveného přístupu poskytuje všechny hello plánování a možnosti správy vyžaduje toorun jediný kontejner, zatímco orchestrator platformy toomanage více kontejneru úlohy nad jeho.

Protože všechny hello základní infrastruktury pro kontejner instancí je spravovaná službou Azure, nemusí platformě orchestrator s hledání příslušný hostitelský počítač, na které toorun jediný kontejner tooconcern sám sebe. pružnost Hello hello cloudu zajistí, že než je vždy k dispozici. Místo toho hello orchestrator můžete soustředit na hello úlohy, které zjednodušují vývoj hello architektury více kontejneru, včetně změny a koordinované upgrady.



## <a name="potential-scenarios"></a>Možných scénářů

Sice stále nascent orchestrator integrace s instancemi Azure kontejneru, Očekáváme, že můžou vznikat v několika různých prostředích:

### <a name="orchestration-of-container-instances-exclusively"></a>Orchestration instancí kontejneru výhradně

Protože rychle začít a účtovat podle hello druhé, prostředí založené výhradně na kontejner instancí Azure nabízí hello nejrychlejší způsob, jak tooget spuštění a toodeal s vysoce proměnné úlohy.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Kombinace instancí kontejnerů a kontejnery v virtuální počítače

Pro dlouhodobé, budou za stabilní úlohy, Orchestrace kontejnery v clusteru s podporou vyhrazených virtuálních počítačů obvykle levnější než systémem hello stejné kontejnery s instancí kontejnerů. Kontejner instancí však nabízí vynikající řešení pro rychlé rozšiřování a smluvní vaší toodeal celkovou kapacitu s neočekávanou nebo krátkodobou špičky využití. Místo škálování hello počet virtuálních počítačů v clusteru, pak nasazení další kontejnery na tyto počítače, hello orchestrator můžete jednoduše naplánovat hello další kontejnery pomocí kontejner instancí a odstranit, když jsou žádné nebudete potřebovat.

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a>Ukázka implementace: Azure kontejner instancí konektor pro Kubernetes

jsme spustili toodemonstrate jak integrovat platformy orchestration kontejner s instancemi Azure kontejneru, sestavování [ukázka konektor pro Kubernetes][aci-connector-k8s]. 

Hello konektor pro Kubernetes napodobuje hello [kubelet] [ kubelet-doc] registrace jako uzel neomezená kapacitu a odeslání hello vytvoření [pracovními stanicemi soustředěnými kolem] [ pod-doc] kontejneru a skupiny v Azure kontejner instancí. 

<!-- ![ACI Connector for Kubernetes][aci-connector-k8s-gif] -->

Konektory pro ostatní orchestrators může být postavená, podobně jako integrovaná se službami platformy primitiv toocombine hello napájení nástroje orchestrator hello rozhraní API pomocí hello rychlost a zjednodušení správy kontejnerů v Azure kontejner instancí.

> [!WARNING]
> Hello ACI konektor pro Kubernetes je *experimentální* a neměl by se používat v produkčním prostředí.

## <a name="next-steps"></a>Další kroky

Vytvoření vaší první kontejneru Azure kontejner s instancemi s využitím hello [úvodní příručka](container-instances-quickstart.md).

<!-- IMAGES -->
[aci-connector-k8s-gif]: ./media/container-instances-orchestrator-relationship/aci-connector-k8s.gif

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/azure/aci-connector-k8s
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/