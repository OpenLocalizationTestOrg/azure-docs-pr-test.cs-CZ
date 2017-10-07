---
title: cluster Azure DC/OS aaaMonitor - Dynatrace | Microsoft Docs
description: "Monitorování clusteru Azure Container Service DC/OS s Dynatrace. Nasaďte hello Dynatrace OneAgent pomocí řídicího panelu hello DC/OS."
services: container-service
documentationcenter: 
author: MartinGoodwell
manager: 
editor: 
tags: acs, azure-container-service
keywords: Kontejnery, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 9e2e2d1c8b454422d1db30dac7c385d31d336853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a>Monitorování clusteru Azure Container Service DC/OS s Dynatrace SaaS/spravované
V tomto článku jsme ukazují, jak toodeploy hello [Dynatrace](https://www.dynatrace.com/) OneAgent toomonitor všechny hello agenta uzlů v clusteru Azure Container Service. Potřebujete účet s Dynatrace SaaS/spravované pro tuto konfiguraci. 

## <a name="dynatrace-saasmanaged"></a>Dynatrace SaaS/spravované
Dynatrace je řešení monitorování cloudu nativní pro vysoce dynamické kontejneru a prostředí clusteru. Umožňuje vám toobetter optimalizovat nasazení kontejnerů a přidělení paměti s použitím data o využití v reálném čase. Je schopný automaticky přesným rozpoznáním aplikace a infrastrukturu problémů tím, že poskytuje automatizované monitorování standardních hodnot, korelace problém a zjištění hlavní příčiny.

Hello následující obrázek ukazuje hello Dynatrace uživatelského rozhraní:

![Dynatrace uživatelského rozhraní](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a>Požadavky 
[Nasazení](container-service-deployment.md) a [připojit](./../container-service-connect.md) tooa cluster nakonfigurovaný v Azure Container Service. Prozkoumejte hello [uživatelského rozhraní Marathon](container-service-mesos-marathon-ui.md). Přejděte příliš[https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) tooset Dynatrace SaaS účet.  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a>Konfigurace nasazení Dynatrace pomocí Marathonu
Tyto kroky vám ukážou, jak tooconfigure a Dynatrace aplikace tooyour cluster pomocí Marathonu nasadit.

1. Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/). Jednou v hello uživatelského rozhraní DC/OS, přejděte toohello **Universe** kartě a poté vyhledejte **Dynatrace**.

    ![Dynatrace v Universe DC/OS](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. Konfigurace hello toocomplete, potřebujete účet Dynatrace SaaS nebo Bezplatný zkušební účet. Po přihlášení do řídicího panelu Dynatrace hello vybrat **nasazení Dynatrace**.

    ![Nastavení Dynatrace PaaS integrace](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. Na stránce hello vyberte **nastavení integrace PaaS**. 

    ![Rozhraní API Dynatrace tokenu](./media/container-service-monitoring-dynatrace/api-token.png) 

4. Zadejte token vaše rozhraní API do hello Dynatrace OneAgent konfigurace v rámci hello Universe DC/OS. 

    ![Konfigurace Dynatrace OneAgent v hello Universe DC/OS](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. Nastavit hello instancí toohello počet uzlů, který chcete toorun. Nastaví větší číslo také funguje, ale DC/OS se zkusit nové instance toofind toto číslo je ve skutečnosti ukončen. Pokud dáváte přednost, můžete také nastavit tuto hodnotu tooa jako 1000000. V takovém případě vždy, když toohello clusteru je přidán nový uzel, Dynatrace automaticky nasadí do agenta toothat nového uzlu, za cenu hello neustále pokusu o další instance toodeploy DC/OS.

    ![Konfigurace Dynatrace v hello DC/OS Universe-instance](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a>Další kroky

Jakmile jste nainstalovali balíček hello, přejděte zpět toohello Dynatrace řídicího panelu. V rámci clusteru můžete prozkoumat hello různých využití metriky pro kontejnery hello. 
