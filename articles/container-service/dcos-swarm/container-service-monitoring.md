---
title: "Cluster Azure DC/OS monitorování - Datadog"
description: "Monitorování clusteru Azure Container Service s Datadog. K nasazení agentů Datadog ke clusteru pomocí webového uživatelského rozhraní DC/OS."
services: container-service
author: sauryadas
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: b895ef906a8c8f3f8cc21267d80f8b59b64837f4
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>Monitorování clusteru Azure Container Service DC/OS s Datadog

V tomto článku jsme nasadí Datadog agentů na všechny uzly agenta v clusteru Azure Container Service. Pro tuto konfiguraci budete potřebovat účet s Datadog. 

## <a name="prerequisites"></a>Požadavky
[Nasaďte](container-service-deployment.md) a [připojte](../container-service-connect.md) cluster nakonfigurovaný službou Azure Container Service. Prozkoumejte [uživatelské rozhraní Marathon](container-service-mesos-marathon-ui.md). Přejděte na [http://datadoghq.com](http://datadoghq.com) nastavit účet Datadog. 

## <a name="datadog"></a>Datadog
Datadog je monitorování služba, která shromažďuje data monitorování z kontejnerů v rámci clusteru Azure Container Service. Datadog má řídicí panel integrace Dockeru kde můžete zobrazit konkrétní metriky v rámci kontejnerů. Metriky shromážděná z kontejnerů jsou uspořádané podle využití procesoru, paměti, síťové a vstupně-výstupní operace. Datadog rozdělí metriky na kontejnery a obrázků. Příklad vzhled uživatelského rozhraní pro využití procesoru je pod.

![Datadog uživatelského rozhraní](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Konfigurace nasazení Datadog pomocí Marathonu
Tyto kroky vám ukáže, jak nakonfigurovat a nasadit aplikace Datadog ke clusteru pomocí Marathonu. 

Přístup přes uživatelské rozhraní DC/OS [http://localhost:80 /](http://localhost:80/). Jednou v uživatelském rozhraní DC/OS přejděte na "základní soubor" což je ve spodní levé a poté vyhledejte "Datadog" a klikněte na možnost "Nainstalovat."

![Balíček Datadog v rámci Universe DC/OS](./media/container-service-monitoring/datadog1.png)

Teď k dokončení konfigurace budete potřebovat účet Datadog nebo Bezplatný zkušební účet. Po přihlášeni vzhledu webu Datadog doleva a přejděte do integrace -> pak [rozhraní API](https://app.datadoghq.com/account/settings#api). 

![Klíč rozhraní API Datadog](./media/container-service-monitoring/datadog2.png)

Potom zadejte klíč rozhraní API do Datadog konfigurace v rámci Universe DC/OS. 

![Konfigurace Datadog v Universe DC/OS](./media/container-service-monitoring/datadog3.png) 

Ve výše uvedené konfigurace instance jsou nastaveny na 10000000 tak vždy, když je přidán nový uzel clusteru Datadog automaticky nasadí agent na tomto uzlu. Toto je předběžná řešení. Jakmile jste nainstalovali balíček, měli byste přejděte zpět na web Datadog a najít "[řídicí panely](https://app.datadoghq.com/dash/list)." Zde se zobrazí vlastní a integrace řídicí panely. [Řídicí panel Docker](https://app.datadoghq.com/screen/integration/docker) budou mít všechny metriky kontejner pro monitorování vašeho clusteru potřebujete. 

