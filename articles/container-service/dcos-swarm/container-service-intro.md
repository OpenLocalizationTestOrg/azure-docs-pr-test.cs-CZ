---
title: "aaaDocker kontejneru hostování v cloudu Azure | Microsoft Docs"
description: "Azure Container Service poskytuje způsob toosimplify hello vytvoření, konfiguraci a správu clusteru s podporou virtuálních počítačů, které jsou předkonfigurované toorun kontejnerizované aplikace."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, mikroslužby, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: rogardle
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 46a0071a7497a3ff44d75413b49f1d06f844c446
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toodocker-container-hosting-solutions-with-azure-container-service"></a>Kontejner tooDocker Úvod hostování řešení s Azure Container Service 
Azure Container Service umožňuje jednodušší pro vás toocreate, konfigurovat a spravovat cluster virtuálních počítačů, které jsou předkonfigurované toorun kontejnerizované aplikace. Používá optimalizovanou konfiguraci oblíbených nástrojů open source pro plánování a orchestraci. To umožňuje vám toouse existujících dovedností, nebo kreslení na text velké a stále se rozrůstající komunita odborných znalostí, toodeploy a spravovat aplikace založené na kontejneru v Microsoft Azure.

![Azure Container Service poskytuje prostředky toomanage kontejnerizované aplikace na několika hostitelích v Azure.](./media/acs-intro/acs-cluster-new.png)

Azure Container Service využívá hello Docker kontejneru formátu tooensure, že jsou vaše kontejnery aplikace plně přenositelné. Také podporuje vaši volbu Marathon a DC/OS, Docker Swarm nebo Kubernetes tak, aby tyto aplikace toothousands kontejnerů nebo i desetitisíce můžete škálovat.

Pomocí Azure Container Service můžete využít výhod podnikové úrovni funkce Azure, a současně se zachovává aplikace přenositelnost – včetně přenositelnost u vrstvy orchestration hello.

## <a name="using-azure-container-service"></a>Používání služby Azure Container Service
Naším cílem s Azure Container Service je tooprovide kontejneru hostitelské prostředí pomocí nástroje open source a technologie, které jsou dnes důležitá u našich zákazníků. toothis end zveřejňujeme hello standardních koncových bodů rozhraní API pro vaši zvolenou orchestrator (DC/OS, Docker Swarm nebo Kubernetes). Pomocí těchto koncových bodů, můžete využít veškerý software, který je schopen rozhovoru toothose koncové body. Například v případě hello hello Docker Swarm koncového bodu, můžete zvolit rozhraní příkazového řádku Dockeru hello toouse (CLI). Pro DC/OS můžete zvolit hello orchestrátoru DC/OS rozhraní příkazového řádku. V případě systému Kubernetes můžete zvolit použití `kubectl`.

## <a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Vytvoření clusteru Dockeru s použitím služby Azure Container Service
toobegin pomocí Azure Container Service, nasazení clusteru Azure Container Service přes portál hello (hledání hello Marketplace pro **Azure Container Service**), pomocí šablony Azure Resource Manager ([Docker Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm), [DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos), nebo [Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)), nebo s hello [Azure CLI 2.0](container-service-create-acs-cluster-cli.md). Hello, pokud šablony rychlý start může být upravený tooinclude další nebo pokročilá konfigurace Azure. Další informace najdete v části [Nasazení clusteru Azure Container Service](container-service-deployment.md).

## <a name="deploying-an-application"></a>Nasazení aplikace
Služba Azure Container Service poskytuje možnost orchestrace prostřednictvím Docker Swarmu, systému DC/OS nebo Kubernetes. Způsob nasazení aplikace závisí na zvoleném orchestrátoru.

### <a name="using-dcos"></a>S použitím systému DC/OS
DC/OS je distribuované operačního systému podle hello Apache Mesos distribuovaných systémů jádra. Apache Mesos je umístěna v hello Apache Software Foundation a jsou uvedeny některé hello [největších názvy v IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/) uživatelů a přispěvatele.

![Služba Azure Container Service nakonfigurovaná pro systém DC/OS se zobrazením agentů a nadřízených serverů.](media/acs-intro/dcos.png)

Systém DC/OS a Apache Mesos zahrnují rozsáhlou nabídku funkcí:

* Ověřená škálovatelnost
* Replikované nadřízené a podřízené servery odolné proti chybám s použitím Apache ZooKeeper
* Podpora kontejnerů ve formátu Dockeru
* Nativní izolace mezi úkoly s kontejnery Linuxu
* Plánování s více prostředky (paměť, procesor, disk a porty)
* Rozhraní API jazyků Java, Python a C++ pro vývoj nových paralelních aplikací
* Webové uživatelské rozhraní pro zobrazení stavu clusteru

Ve výchozím nastavení zahrnuje DC/OS spuštěné v Azure Container Service hello Marathon orchestration platforma pro plánování úloh. Součástí hello DC/OS nasazení služby ACS je však hello Mesosphere Universe služeb, které lze přidat tooyour služby. Mezi služby v hello Universe patří, Spark, systému Hadoop, Cassandra a mnoho dalšího.

![Universe pro systém DC/OS ve službě Azure Container Service](media/dcos/universe.png)

#### <a name="using-marathon"></a>S použitím systému Marathon
Marathonu je platné pro celý cluster init a systém správy pro služby v cgroups--nebo v případě hello Azure Container Service kontejnery formátované Dockerem. Systém Marathon poskytuje webové uživatelské rozhraní, ze kterého můžete nasazovat své aplikace. K dispozici je přístup prostřednictvím adresy URL, která vypadá přibližně takto: `http://DNS_PREFIX.REGION.cloudapp.azure.com` Proměnné DNS\_PREFIX a REGION se definují v době nasazení. Samozřejmě můžete také zadat vlastní název DNS. Další informace o spouštění kontejner pomocí webového uživatelského rozhraní Marathon hello najdete v tématu [DC/OS Správa kontejnerů prostřednictvím webového uživatelského rozhraní Marathon hello](container-service-mesos-marathon-ui.md).

![Seznam aplikací systému Marathon](media/dcos/marathon-applications-list.png)

Můžete taky hello rozhraní REST API pro komunikaci pomocí Marathonu. Existuje mnoho klientských knihoven, které jsou k dispozici pro jednotlivé nástroje. Pokrývaly různých jazycích – a samozřejmě můžete použít protokol HTTP hello v libovolném jazyce. Podporu pro systém Marathon navíc poskytuje mnoho oblíbených nástrojů DevOps. Díky tomu má váš provozní tým při práci s clusterem Azure Container Service k dispozici maximální flexibilitu. Další informace o spouštění kontejner pomocí hello Marathon REST API, najdete v části [DC/OS Správa kontejnerů přes rozhraní REST API Marathonu hello](container-service-mesos-marathon-rest.md).

### <a name="using-docker-swarm"></a>S použitím Docker Swarmu
Docker Swarm poskytuje nativní clustering pro Docker. Protože Docker Swarm poskytuje hello standardní Docker rozhraní API, jakýkoli nástroj, který již komunikuje s démon Docker můžete použít hostitele toomultiple škálování tootransparently Swarm v Azure Container Service.

![Azure Container Service nakonfigurovat toouse Swarm.](media/acs-intro/acs-swarm2.png)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Podporované nástroje pro správu kontejnerů v clusteru Swarm zahrnovat, ale nejsou omezeny na hello následující:

* Dokku
* Rozhraní příkazového řádku Dockeru a Docker Compose
* Krane
* Jenkins

### <a name="using-kubernetes"></a>S použitím Kubernetes
Kubernetes je často používaný opensourcový nástroj pro orchestraci kontejnerů na úrovni produkce. Kubernetes automatizuje nasazení, škálování a správu kontejnerizovaných aplikací. Protože je řešení open source a vycházejí z komunity hello open source, bezproblémově běží v Azure Container Service a může být použité toodeploy kontejnery škálované na Azure Container Service.

![Azure Container Service nakonfigurovat toouse Kubernetes.](media/acs-intro/kubernetes.png)

Má bohatou výbavu funkcí, například:
* Horizontální škálování
* Zjišťování služeb a vyrovnávání zatížení
* Správa tajných klíčů a konfigurace
* Automatická uvedení a vrácení zpět založená na rozhraní API
* Samoopravení

## <a name="videos"></a>Videa
Začínáme se službou Azure Container Service (101):  

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Container-Service-101/player]
>
>

Vytváření aplikací pomocí hello Azure Container Service (Build 2016)

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/B822/player]
>
>

## <a name="next-steps"></a>Další kroky

Nasazení clusteru služby kontejneru pomocí hello [portál](container-service-deployment.md) nebo [Azure CLI 2.0](container-service-create-acs-cluster-cli.md).
