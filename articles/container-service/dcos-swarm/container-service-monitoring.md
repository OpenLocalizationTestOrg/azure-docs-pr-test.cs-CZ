---
title: cluster Azure DC/OS aaaMonitor - Datadog | Microsoft Docs
description: "Monitorování clusteru Azure Container Service s Datadog. Použijte hello DC/OS webového uživatelského rozhraní toodeploy hello Datadog agenti tooyour clusteru."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Kontejnery Azure DC/OS, Docker Swarm
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 10268c04b5c2ef393429e706ed4a467fff80f718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>Monitorování clusteru Azure Container Service DC/OS s Datadog
V tomto článku jsme nasadí Datadog agenti tooall hello agenta uzlů v clusteru Azure Container Service. Pro tuto konfiguraci budete potřebovat účet s Datadog. 

## <a name="prerequisites"></a>Požadavky
[Nasaďte](container-service-deployment.md) a [připojte](../container-service-connect.md) cluster nakonfigurovaný službou Azure Container Service. Prozkoumejte hello [uživatelského rozhraní Marathon](container-service-mesos-marathon-ui.md). Přejděte příliš[http://datadoghq.com](http://datadoghq.com) tooset Datadog účet. 

## <a name="datadog"></a>Datadog
Datadog je monitorování služba, která shromažďuje data monitorování z kontejnerů v rámci clusteru Azure Container Service. Datadog má řídicí panel integrace Dockeru kde můžete zobrazit konkrétní metriky v rámci kontejnerů. Metriky shromážděná z kontejnerů jsou uspořádané podle využití procesoru, paměti, síťové a vstupně-výstupní operace. Datadog rozdělí metriky na kontejnery a obrázků. Příkladem jaké hello uživatelského rozhraní vypadá takto: pro procesor využití je menší než.

![Datadog uživatelského rozhraní](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Konfigurace nasazení Datadog pomocí Marathonu
Tyto kroky se dozvíte, jak tooconfigure a Datadog aplikace tooyour cluster pomocí Marathonu nasadit. 

Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/). Jednou v hello uživatelského rozhraní DC/OS přejděte toohello "Universe", který je na hello dolů, doleva a poté vyhledejte "Datadog" a klikněte na možnost "Nainstalovat."

![Balíček Datadog v rámci hello Universe DC/OS](./media/container-service-monitoring/datadog1.png)

Nyní toocomplete hello konfigurace budete potřebovat účet Datadog nebo Bezplatný zkušební účet. Jakmile jste se přihlásili toohello Datadog webu vypadají toohello doleva a přejděte tooIntegrations -> pak [rozhraní API](https://app.datadoghq.com/account/settings#api). 

![Klíč rozhraní API Datadog](./media/container-service-monitoring/datadog2.png)

Potom zadejte klíč rozhraní API do hello Datadog konfigurace v rámci hello Universe DC/OS. 

![Konfigurace Datadog v hello Universe DC/OS](./media/container-service-monitoring/datadog3.png) 

V hello výše konfigurace instance jsou nastaveny too10000000, tak pokaždé, když je přidán nový uzel clusteru toohello Datadog automaticky nasadí do agenta toothat uzlu. Toto je předběžná řešení. Po instalaci balíčku hello můžete přejít zpět toohello Datadog webu a najít "[řídicí panely](https://app.datadoghq.com/dash/list)." Zde se zobrazí vlastní a integrace řídicí panely. Hello [řídicí panel Docker](https://app.datadoghq.com/screen/integration/docker) budou mít všechny metriky hello kontejner pro monitorování vašeho clusteru potřebujete. 

