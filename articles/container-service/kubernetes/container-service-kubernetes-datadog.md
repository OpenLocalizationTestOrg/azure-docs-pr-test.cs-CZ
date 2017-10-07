---
title: aaaMonitor Azure Kubernetes cluster s Datadog | Microsoft Docs
description: "Monitorování Kubernetes clusteru v Azure Container Service pomocí Datadog"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: bccd8b59a048e0f791172fcfc4eeba6370dafcc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Monitorování clusteru Azure Container Service s DataDog

## <a name="prerequisites"></a>Požadavky
Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).

Také předpokládá, že máte hello `az` rozhraní příkazového řádku Azure a `kubectl` nástroje nainstalované.

Pokud máte hello můžete otestovat `az` nainstalovaná, spuštěním nástroje:

```console
$ az --version
```

Pokud nemáte hello `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).

Pokud máte hello můžete otestovat `kubectl` nainstalovaná, spuštěním nástroje:

```console
$ kubectl version
```

Pokud nemáte `kubectl` nainstalován, můžete spustit:

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a>DataDog
Datadog je monitorování služba, která shromažďuje data monitorování z kontejnerů v rámci clusteru Azure Container Service. Datadog má řídicí panel integrace Dockeru kde můžete zobrazit konkrétní metriky v rámci kontejnerů. Metriky shromážděná z kontejnerů jsou uspořádané podle využití procesoru, paměti, síťové a vstupně-výstupní operace. Datadog rozdělí metriky na kontejnery a obrázků.

Je nutné nejprve příliš[vytvoření účtu](https://www.datadoghq.com/lpg/)

## <a name="installing-hello-datadog-agent-with-a-daemonset"></a>Instalace s DaemonSet hello Datadog agenta
DaemonSets používají Kubernetes toorun jednu instanci kontejner na jednotlivých hostitelích v clusteru hello.
Jsou ideální pro spuštění monitorovací agenty.

Po přihlášení do Datadog můžete postupovat podle hello [Datadog pokyny](https://app.datadoghq.com/account/settings#agent/kubernetes) tooinstall Datadog agentů v clusteru pomocí DaemonSet.

## <a name="conclusion"></a>Závěr
A to je vše! Jakmile jsou agenti hello a systémem jste měli vidět data v konzole hello za pár minut. Můžete taky navštívit hello integrované [řídicí panel kubernetes](https://app.datadoghq.com/screen/integration/kubernetes) toosee souhrn clusteru.
