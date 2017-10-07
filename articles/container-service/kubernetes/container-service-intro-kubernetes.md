---
title: aaaIntroduction tooAzure Container Service pro Kubernetes | Microsoft Docs
description: "Azure Container Service pro Kubernetes umožňuje jednoduché toodeploy a spravovat aplikace založené na kontejneru v Azure."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Kubernetes, Docker, Containers, Microservices, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: bfc85a180bdf4a405c9047eb882d3eed01640dd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-container-service-for-kubernetes"></a>Úvod tooAzure Container Service pro Kubernetes
Azure Container Service pro Kubernetes umožňuje jednoduché toocreate, konfigurovat a spravovat cluster virtuálních počítačů, které jsou předkonfigurované toorun kontejnerové aplikace. To umožňuje vám toouse existujících dovedností, nebo kreslení na text velké a stále se rozrůstající komunita odborných znalostí, toodeploy a spravovat aplikace založené na kontejneru v Microsoft Azure.

Pomocí Azure Container Service můžete využít výhod hello podnikové úrovni funkce Azure, a současně se zachovává přenositelnost aplikace prostřednictvím Kubernetes a hello Docker formát obrázku.

## <a name="using-azure-container-service-for-kubernetes"></a>Použití služby Azure Container Service pro Kubernetes
Naším cílem s Azure Container Service je tooprovide kontejneru hostitelské prostředí pomocí nástroje open source a technologie, které jsou dnes důležitá u našich zákazníků. toothis end zveřejňujeme hello standardní koncové body Kubernetes rozhraní API. Pomocí těchto standardních koncových bodů, můžete využít veškerý software, který je schopen rozhovoru tooa Kubernetes clusteru. Můžete například zvolit [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) nebo [draft](https://github.com/Azure/draft).

## <a name="creating-a-kubernetes-cluster-using-azure-container-service"></a>Vytvoření clusteru Kubernetes pomocí služby Azure Container Service
toobegin pomocí Azure Container Service, nasazení clusteru Azure Container Service s hello [Azure CLI 2.0](container-service-kubernetes-walkthrough.md) nebo přes portál hello (hledání hello Marketplace pro **Azure Container Service**). Pokud jste pro pokročilé uživatele, kteří musí větší kontrolu nad hello šablony Azure Resource Manager, můžete použít s otevřeným zdrojem hello [acs modul](https://github.com/Azure/acs-engine) toobuild projektu vlastní vlastní Kubernetes clusteru a nasadit prostřednictvím hello `az` rozhraní příkazového řádku.

### <a name="using-kubernetes"></a>S použitím Kubernetes
Kubernetes automatizuje nasazení, škálování a správu kontejnerizovaných aplikací. Má bohatou výbavu funkcí, například:
* Automatické balení do kontejnerů
* Samoopravení
* Horizontální škálování
* Zjišťování služeb a vyrovnávání zatížení
* Automatická uvádění a vracení zpět
* Správa tajných kódů a konfigurací
* Orchestrace úložiště
* Spouštění dávek

Diagram architektury systému Kubernetes nasazeného prostřednictvím služby Azure Container Service:

![Azure Container Service nakonfigurovat toouse Kubernetes.](media/acs-intro/kubernetes.png)

## <a name="videos"></a>Videa

Podpora pro Kubernetes ve službě Azure Container Service (Azure Friday, leden 2017):

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Kubernetes-Support-in-Azure-Container-Services/player]
>
>

Nástroje pro vývoj a nasazování aplikací v Kubernetes (Azure OpenDev, červen 2017):

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

## <a name="next-steps"></a>Další kroky

Prozkoumejte hello [rychlý start Kubernetes](container-service-kubernetes-walkthrough.md) toobegin dnes zkoumat Azure Container Service.
