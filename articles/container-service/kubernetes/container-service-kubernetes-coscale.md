---
title: aaaMonitor Azure Kubernetes cluster s CoScale | Microsoft Docs
description: "Monitorování Kubernetes cluster Azure Container Service pomocí CoScale"
services: container-service
documentationcenter: 
author: fryckbos
manager: 
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: f835e82d2be3afe1d85070bd0bf69649cc6dd2ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a>Monitorování clusteru Azure Container Service Kubernetes s CoScale

V tomto článku jsme ukazují, jak toodeploy hello [CoScale](https://www.coscale.com/) toomonitor agenta všechny uzly a kontejnery v vaší Kubernetes cluster v Azure Container Service. Účet s CoScale musíte pro tuto konfiguraci. 


## <a name="about-coscale"></a>O CoScale 

CoScale je monitorování platforma, která shromažďuje metriky a události z všechny kontejnery v několik platforem orchestration. CoScale nabízí úplné zásobníku monitorování pro Kubernetes prostředí. Poskytuje vizualizace a analýzy pro všechny vrstvy v zásobníku hello: hello operačního systému, Kubernetes, Docker a aplikacemi spuštěnými ve kontejnerů. CoScale nabízí několik předdefinovaných monitorování řídicí panely a má integrovanou anomálií detekce tooallow operátory a rychlé vývojáři toofind infrastruktury a aplikace potíže.

![CoScale uživatelského rozhraní](./media/container-service-kubernetes-coscale/coscale.png)

Jak je znázorněno v tomto článku, můžete nainstalovat agenty na clusteru toorun Kubernetes CoScale jako řešení SaaS. Pokud chcete tookeep vaše data na místě, CoScale je také k dispozici pro místní instalaci.


## <a name="prerequisites"></a>Požadavky

Je nutné nejprve příliš[vytvořit účet CoScale](https://www.coscale.com/free-trial).

Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).

Také předpokládá, že máte hello `az` rozhraní příkazového řádku Azure a `kubectl` nástroje nainstalované.

Pokud máte hello můžete otestovat `az` nainstalovaná, spuštěním nástroje:

```azurecli
az --version
```

Pokud nemáte hello `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](/cli/azure/install-azure-cli).

Pokud máte hello můžete otestovat `kubectl` nainstalovaná, spuštěním nástroje:

```bash
kubectl version
```

Pokud nemáte `kubectl` nainstalován, můžete spustit:

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-hello-coscale-agent-with-a-daemonset"></a>Instalace agenta CoScale hello s DaemonSet
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) jsou Kubernetes toorun používá jednu instanci kontejner na jednotlivých hostitelích v clusteru hello.
Jsou ideální pro spuštění monitorovací agenty například hello CoScale agenta.

Po přihlášení tooCoScale přejděte toohello [agent stránka](https://app.coscale.com/) tooinstall CoScale agentů v clusteru pomocí DaemonSet. Hello uživatelského rozhraní CoScale poskytuje toocreate kroky s průvodcem konfigurace agenta a spuštění monitorování vaší Kubernetes clusterů.

![Konfigurace agenta coScale](./media/container-service-kubernetes-coscale/installation.png)

agent hello toostart na hello clusteru, spusťte příkaz hello zadat:

![Spuštění agenta CoScale hello](./media/container-service-kubernetes-coscale/agent_script.png)

A to je vše! Jakmile hello agenti jsou spuštěná, měli byste vidět data v konzole hello za pár minut. Navštivte hello [agent stránka](https://app.coscale.com/) toosee souhrn cluster, provést další kroky konfigurace a v tématu řídicí panely, jako je například hello **Kubernetes clusteru přehled**.

![Přehled Kubernetes clusteru](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

Hello CoScale agenta je automaticky nasadit na nové počítače v clusteru hello. aktualizace agenta Hello automaticky, když je vydána nová verze.


## <a name="next-steps"></a>Další kroky

V tématu hello [CoScale dokumentace](http://docs.coscale.com/) a [blog](https://www.coscale.com/blog) Další informace o CoScale sledování řešení. 

