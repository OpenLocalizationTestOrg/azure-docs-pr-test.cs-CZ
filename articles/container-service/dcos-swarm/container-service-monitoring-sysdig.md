---
title: aaaMonitor Azure Container Service cluster s Sysdig | Microsoft Docs
description: "Cluster služby Azure Container Service můžete monitorovat pomocí služby Sysdig."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Kontejnery, DC/OS, Azure
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 72f2d3d6f6885f9876fa158b88aae58b84a4610f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Monitorování clusteru služby Azure Container Service pomocí služby Sysdig
V tomto článku jsme nasadí Sysdig agenti tooall hello agenta uzlů v clusteru Azure Container Service. Pro tuto konfiguraci potřebujete účet se službou Sysdig. 

## <a name="prerequisites"></a>Požadavky
[Nasaďte](container-service-deployment.md) a [připojte](../container-service-connect.md) cluster nakonfigurovaný službou Azure Container Service. Prozkoumejte hello [uživatelského rozhraní Marathon](container-service-mesos-marathon-ui.md). Přejděte příliš[http://app.sysdigcloud.com](http://app.sysdigcloud.com) tooset si účet Sysdig cloudu. 

## <a name="sysdig"></a>Sysdig
Sysdig je monitorování služba, která vám umožní toomonitor kontejnerů v rámci clusteru. Sysdig se označuje toohelp při řešení problémů, ale také obsahuje vaše základní monitorování metriky pro procesor, sítě, paměť a vstupně-výstupních operací. Sysdig umožňuje snadno toosee kontejnery, které pracují hello používá hardest nebo v podstatě hello většina paměti a procesoru. Toto zobrazení je v části "Přehled", který je aktuálně ve verzi beta hello. 

![Uživatelské rozhraní služby Sysdig](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Konfigurace nasazení služby Sysdig s uživatelským rozhraním Marathon
Tyto kroky se dozvíte, jak tooconfigure a Sysdig aplikace tooyour cluster pomocí Marathonu nasadit. 

Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/) jednou v hello uživatelského rozhraní DC/OS přejděte toohello "Universe", který je na hello dolů, doleva a poté vyhledejte "Sysdig."

![Sysdig v rozhraní DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig1.png)

Nyní toocomplete hello konfigurace nezbytné Sysdig cloudu účet nebo Bezplatný zkušební účet. Jakmile jste přihlášeni toohello Sysdig cloudu web, klikněte na své uživatelské jméno a na stránce hello byste měli vidět vaší "přístupový klíč." 

![Klíč rozhraní API služby Sysdig](./media/container-service-monitoring-sysdig/sysdig2.png) 

Potom zadejte přístupový klíč do hello Sysdig konfigurace v rámci hello Universe DC/OS. 

![Konfigurace Sysdig v hello Universe DC/OS](./media/container-service-monitoring-sysdig/sysdig3.png)

Teď nastavte hello instancí too10000000 tak vždy, když je přidán nový uzel clusteru toohello Sysdig automaticky nasadit agenta toothat nový uzel. Toto je toomake dočasné řešení, která je opravdu že sysdig nasadí tooall nové agenty v rámci clusteru hello. 

![Konfigurace Sysdig v hello DC/OS Universe-instance](./media/container-service-monitoring-sysdig/sysdig4.png)

Jakmile jste nainstalovali balíček hello přejděte zpět toohello Sysdig uživatelského rozhraní a budete moct tooexplore hello různých využití metriky pro hello kontejnery v rámci clusteru. 

Můžete také nainstalovat řídicí panely specifické pro Mesos a Marathon prostřednictvím [průvodce novým řídicím panelem](https://app.sysdigcloud.com/#/dashboards/new).
