---
title: "aaaAzure skupiny kontejner instancí kontejnerů"
description: "Pochopit, jak fungují skupiny kontejnerů v Azure kontejner instancí"
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
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2b0e784609979045c8f77d7b6d0987ec3fee714c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="container-groups-in-azure-container-instances"></a>Skupiny kontejnerů v Azure kontejner instancí

Hello nejvyšší úrovně prostředků v Azure kontejner instancí je skupina kontejneru. Tento článek popisuje, jaké jsou skupiny kontejnerů a jaké druhy scénářů umožňují.

## <a name="how-a-container-group-works"></a>Jak funguje skupina kontejneru

Skupina kontejneru je kolekce kontejnery, které získat naplánovat na hello stejné hostitele počítače a sdílené složky životního cyklu, místní sítí a svazky úložiště. Je podobný koncept toohello *pod* v [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) a [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).

Hello následující diagram ukazuje příklad skupinou kontejneru, která obsahuje více kontejnerů.

![Příklad skupiny kontejneru][container-groups-example]

Poznámky:

- skupiny Hello je naplánováno na jeden hostitelský počítač.
- skupiny Hello zpřístupní jednu veřejnou IP adresu, s zveřejněné jeden port.
- skupiny Hello je tvořen dvěma kontejnery. Jednoho kontejneru typu naslouchá na portu 80, zatímco jiné hello naslouchá na portu 5000.
- Hello skupina obsahuje dvě sdílené složky Azure jako svazek připojení zařízení a každý kontejner připojí jeden hello místně sdílených složek.

### <a name="networking"></a>Sítě

Skupiny kontejnerů sdílet IP adresy a portu obor názvů na tuto IP adresu. Externí klienti tooreach tooenable kontejner v rámci skupiny hello, musí vystavit hello port hello IP adresy a z kontejneru hello. Vzhledem k tomu, že kontejnery v rámci skupiny hello sdílet port obor názvů, mapování portů se nepodporuje. Kontejnery v rámci skupiny dosáhnout vzájemně prostřednictvím místního hostitele na hello porty, které se mají vystavený, i když tyto porty se nezobrazí externě na IP adrese hello skupiny.

### <a name="storage"></a>Úložiště

Můžete zadat toomount externí svazky v rámci skupiny kontejneru. Můžete namapovat tyto svazky do konkrétních cest v rámci jednotlivých kontejnerů hello ve skupině.

## <a name="common-scenarios"></a>Obvyklé scénáře

Více kontejner skupiny jsou užitečné v případech, místo, kam chcete toodivide až jeden úkol funkční do malý počet kontejneru Image, které mohou být dodány pomocí různými týmy a obsahují požadavky samostatné prostředků. Příklad použití můžou zahrnovat:

- Kontejner aplikace a kontejner protokolování. kontejner protokolování Hello shromažďuje protokoly hello a metriky výstup hello hlavní aplikace a zapíše toolong termín úložiště.
- Aplikace a kontejner monitorování. Hello monitorování kontejneru pravidelně díky tooensure aplikace požadavek toohello, že je spuštěná a správně reagovat a vyvolá výstrahu, pokud není.
- Kontejner obsluhující webové aplikace a kontejner stahování hello nejnovější obsah od správy zdrojového kódu.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak příliš[nasazení s více kontejner skupiny](container-instances-multi-container-group.md) pomocí šablony Azure Resource Manager.

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png