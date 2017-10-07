---
title: "aaaManage Azure DC/OS clusteru pomocí uživatelského rozhraní Marathon | Microsoft Docs"
description: "Nasazení služby clusteru Azure Container Service tooan kontejnery pomocí webového uživatelského rozhraní Marathon hello."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, mikroslužby, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: a90180e1b4763e6d2ddfa699ed4b7269f209f728
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-hello-marathon-web-ui"></a>Spravovat clusteru Azure Container Service DC/OS prostřednictvím webového uživatelského rozhraní Marathon hello
DC/OS poskytuje prostředí pro nasazování a škálování clusterových úloh a zároveň poskytuje abstrakci používaného hardwaru hello. Nad DC/OS je rozhraní, které spravuje plánování a provádění výpočetních úloh.

Jsou k dispozici pro mnoho populárních úloh rozhraní, tento dokument popisuje, jak tooget začít s nasazením kontejnery pomocí Marathonu. 


## <a name="prerequisites"></a>Požadavky
Než si projdete tyto příklady, budete potřebovat cluster DC/OS nakonfigurovaný v Azure Container Service. Budete také potřebovat clusteru toothis toohave vzdáleného připojení. Další informace o těchto položek najdete v tématu hello následující články:

* [Nasazení clusteru Azure Container Service](container-service-deployment.md)
* [Připojení clusteru Azure Container Service tooan](../container-service-connect.md)

> [!NOTE]
> Tento článek předpokládá, že máte k dispozici tunel clusteru DC/OS toohello prostřednictvím místního portu 80.
>

## <a name="explore-hello-dcos-ui"></a>Prozkoumejte hello uživatelského rozhraní DC/OS
S tunel Secure Shell (SSH) [navázat](../container-service-connect.md), procházet toohttp://localhost/. To načte hello DC/OS webového uživatelského rozhraní a informace o hello clusteru, například využité prostředky, aktivní agenti a spuštěné služby.

![Uživatelské rozhraní DC/OS](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-hello-marathon-ui"></a>Prozkoumejte hello uživatelského rozhraní Marathon
toosee hello uživatelského rozhraní Marathon, přejděte toohttp://localhost/marathon. Prostřednictvím této obrazovky můžete spustit nový kontejner nebo jinou aplikaci v clusteru Azure Container Service DC/OS hello. Taky uvidíte informace o spuštěných kontejnerech a aplikacích.  

![Uživatelské rozhraní Marathon](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Nasazení kontejneru formátovaného Dockerem
Klikněte na tlačítko toodeploy nový kontejner pomocí Marathonu, **vytvořit aplikaci**a zadejte následující informace do formuláře karet hello hello:

| Pole | Hodnota |
| --- | --- |
| ID |nginx |
| Memory (Paměť) | 32 |
| Image |nginx |
| Network (Síť) |Bridged (Zapojeno do mostu) |
| Host Port (Port hostitele) |80 |
| Protocol (Protokol) |TCP |

![Nová aplikace uživatelského rozhraní – obecné](./media/container-service-mesos-marathon-ui/dcos4.png)

![Nová aplikace uživatelského rozhraní – kontejner Docker](./media/container-service-mesos-marathon-ui/dcos5.png)

![Nová aplikace uživatelského rozhraní – porty a zjišťování služby](./media/container-service-mesos-marathon-ui/dcos6.png)

Pokud chcete, aby toostatically mapy hello port kontejneru tooa port hello agenta, je třeba toouse režim JSON. toodo příliš tedy přepínač Průvodce novou aplikací hello**režim JSON** pomocí přepnutí hello. Potom zadejte následující nastavení v části hello hello `portMappings` části definice aplikace hello. Tento příklad vytvoří vazbu na port 80 hello kontejneru tooport 80 agenta DC/OS hello. Po provedení této změny můžete režim JSON v průvodci opět vypnout.

```none
"hostPort": 80,
```

![Nová aplikace uživatelského rozhraní – příklad u portu číslo 80](./media/container-service-mesos-marathon-ui/dcos13.png)

Pokud chcete, aby tooenable kontroly stavu, nastavte cestu na hello **kontroluje stav** kartě.

![Nová aplikace uživatelského rozhraní – kontroly stavu](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

Hello clusteru DC/OS se nasazuje se sadou privátních a veřejných agentů. Hello clusteru toobe možné tooaccess aplikací z hello Internet musíte toodeploy hello aplikace tooa veřejného agenta. toodo tedy vyberte hello **volitelné** karta hello Průvodce novou aplikaci a zadejte **ACCEPTED** pro hello **role prostředků**.

Pak klikněte na **Create Application** (Vytvořit aplikaci).

![Nová aplikace uživatelského rozhraní – nastavení veřejného agenta](./media/container-service-mesos-marathon-ui/dcos14.png)

Zpět na hlavní stránku Marathonu hello uvidíte stav nasazení hello hello kontejneru. Zpočátku uvidíte stav **Deploying** (Nasazování). Po úspěšné nasazení se hello změny stavu příliš**systémem**.

![Hlavní stránka uživatelského rozhraní Marathon – stav nasazení kontejneru](./media/container-service-mesos-marathon-ui/dcos7.png)

Pokud přepnete zpět toohello DC/OS webového uživatelského rozhraní (http://localhost/), uvidíte, že úloha (v tomto případě kontejner formátovaný) běží na clusteru DC/OS hello.

![DC/OS webové uživatelské rozhraní – úloha spuštěná na clusteru hello](./media/container-service-mesos-marathon-ui/dcos8.png)

toosee hello uzlu, který hello úloha běží na, klikněte na tlačítko hello **uzly** kartě.

![Webové uživatelské rozhraní DC/OS – uzel clusteru úlohy](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-hello-container"></a>Dosažení hello kontejneru

V tomto příkladu hello aplikace běží na uzlu veřejného agenta. Nedostanete hello aplikace hello internet procházením toohello agenta plně kvalifikovaný název domény clusteru hello: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, kde:

* **DNSPREFIX** hello předpona DNS, který jste zadali při nasazení clusteru hello.
* **OBLAST** hello oblast, ve kterém se nachází vaší skupiny prostředků.

    ![Nginx z Internetu](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a>Další kroky
* [Práce s DC/OS a hello Marathon API](container-service-mesos-marathon-rest.md)

* Podrobné informace o hello Azure Container Service s Mesos

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
