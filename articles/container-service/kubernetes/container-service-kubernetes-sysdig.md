---
title: cluster Azure Kubernetes aaaMonitor - Sysdig | Microsoft Docs
description: "Monitorování Kubernetes clusteru v Azure Container Service pomocí Sysdig"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: a27813d01ee4624b9e993f6185169ad73aeec3a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a>Monitorování clusteru Azure Container Service Kubernetes pomocí Sysdig

## <a name="prerequisites"></a>Požadavky
Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).

Taky se předpokládá, že máte hello azure cli a kubectl nástroje nainstalované.

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

## <a name="sysdig"></a>Sysdig
Sysdig je, že externí monitorování jako služba společnosti, které můžete sledovat kontejnery v clusteru Kubernetes běžící v Azure. Použití Sysdig vyžaduje aktivní Sysdig účet.
Můžete si zaregistrovat účet jejich [lokality](https://app.sysdigcloud.com).

Jakmile jste přihlášeni toohello Sysdig cloudu web, klikněte na své uživatelské jméno a na stránce hello byste měli vidět vaší "přístupový klíč." 

![Klíč rozhraní API služby Sysdig](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-hello-sysdig-agents-tookubernetes"></a>Instalace tooKubernetes agenti Sysdig hello
toomonitor kontejnerů, Sysdig spustí proces na každý počítač, použití Kubernetes `DaemonSet`.
DaemonSets jsou objekty Kubernetes rozhraní API, které spustit jednu instanci kontejner na počítač.
Jsou ideální pro instalaci nástroje jako hello agenta monitorování na Sysdig.

tooinstall hello Sysdig daemonset, byste si měli nejdřív Stáhnout [hello šablony](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) z sysdig. Uložte tento soubor jako `sysdig-daemonset.yaml`.

Na Linuxu a OS X můžete spustit:

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

V prostředí PowerShell:

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

Tento soubor tooinsert upravte vedle přístupový klíč, který jste získali z vašeho účtu Sysdig.

Nakonec vytvořte hello DaemonSet:

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a>Zobrazení monitorování
Jakmile nainstalovaná a spuštěná, by měl hello agenty čerpadla data back tooSysdig.  Přejděte zpět toothe [řídicí panel sysdig](https://app.sysdigcloud.com) a měli byste vidět informace o kontejnerů.

Můžete taky nainstalovat specifické Kubernetes řídicí panely prostřednictvím [Průvodce novým řídicím panelu](https://app.sysdigcloud.com/#/dashboards/new).
