---
title: "Monitorování clusteru služby Azure Container Service pomocí služby Sysdig | Dokumentace Microsoftu"
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
ms.openlocfilehash: e61001161e632a5d2e513107e30f1eaf06103989
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Monitorování clusteru služby Azure Container Service pomocí služby Sysdig
V tomto článku nasadíme na všechny agentské uzly v clusteru služby Azure Container Service agenty služby Sysdig. Pro tuto konfiguraci potřebujete účet se službou Sysdig. 

## <a name="prerequisites"></a>Požadavky
[Nasaďte](container-service-deployment.md) a [připojte](../container-service-connect.md) cluster nakonfigurovaný službou Azure Container Service. Prozkoumejte [uživatelské rozhraní Marathon](container-service-mesos-marathon-ui.md). Přejděte na [http://app.sysdigcloud.com](http://app.sysdigcloud.com) a vytvořte si cloudový účet služby Sysdig. 

## <a name="sysdig"></a>Sysdig
Sysdig je monitorovací služba, která vám umožňuje monitorovat kontejnery v rámci vašeho clusteru. Služba Sysdig pomáhá s odstraňováním potíží, ale obsahuje taky základní monitorovací metriky procesoru, sítí, paměti a vstupně-výstupních procesů. Služba Sysdig nabízí přehled o tom, které kontejnery jsou nejvytíženější nebo využívají nejvíc paměti a výkonu procesoru. To zjistíte v části „Přehled“, který je v současné době dostupný v beta verzi. 

![Uživatelské rozhraní služby Sysdig](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Konfigurace nasazení služby Sysdig s uživatelským rozhraním Marathon
Tento postup vám ukáže, jak nakonfigurovat aplikace služby Sysdig a nasadit je do clusteru pomocí Marathonu. 

Otevřete uživatelské rozhraní DC/OS prostřednictvím adresy [http://localhost:80/](http://localhost:80/). V uživatelském rozhraní DC/OS potom vlevo dole přejděte na položku „Universe“ vyhledejte „Sysdig“.

![Sysdig v rozhraní DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig1.png)

K dokončení konfigurace budete potřebovat cloudový účet služby Sysdig nebo bezplatný zkušební účet. Po přihlášení na web cloudu Sysdig klikněte na uživatelské jméno. Zobrazí se stránka, na které byste měli najít svůj „přístupový klíč“. 

![Klíč rozhraní API služby Sysdig](./media/container-service-monitoring-sysdig/sysdig2.png) 

Svůj přístupový klíč zadejte do konfigurace služby Sysdig v rozhraní DC/OS Universe. 

![Konfigurace služby Sysdig v rozhraní DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig3.png)

Teď nastavte instance na hodnotu 10000000, aby služba Sysdig při každém přidání nového uzlu do clusteru automaticky do tohoto nového uzlu nasadila agenta. Je to dočasné řešení, kterým zajistíte, aby se služba Sysdig nasadila do všech nových agentů v clusteru. 

![Konfigurace služby Sysdig v rozhraní DC/OS Universe – instance](./media/container-service-monitoring-sysdig/sysdig4.png)

Po instalaci balíčku přejděte zpátky do uživatelského rozhraní služby Sysdig. Teď už budete moc prozkoumávat různé metriky využití kontejnerů v clusteru. 

Můžete také nainstalovat řídicí panely specifické pro Mesos a Marathon prostřednictvím [průvodce novým řídicím panelem](https://app.sysdigcloud.com/#/dashboards/new).
