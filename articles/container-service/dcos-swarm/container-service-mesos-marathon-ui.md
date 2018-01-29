---
title: "Správa clusteru Azure DC/OS pomocí uživatelského rozhraní Marathon"
description: "Využijte webového uživatelského rozhraní Marathon k nasazení kontejnerů do clusteru Azure Container Service."
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: b1c5fc223105b1dae0ce07f242a6b42b34fd2ab3
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a>Správa clusteru Azure Container Service DC/OS přes webové uživatelské rozhraní Marathon

DC/OS poskytuje prostředí pro nasazování a škálování clusterových úloh a zároveň poskytuje abstrakci používaného hardwaru. Nad DC/OS je rozhraní, které spravuje plánování a provádění výpočetních úloh.

Jsou k dispozici pro mnoho populárních úloh rozhraní, tento dokument popisuje, jak začít nasazení kontejnerů pomocí Marathonu. 


## <a name="prerequisites"></a>Požadavky
Než si projdete tyto příklady, budete potřebovat cluster DC/OS nakonfigurovaný v Azure Container Service. Kromě toho je nutné mít možnost se k tomuto clusteru připojit vzdáleně. Další informace k těmto záležitostem najdete v těchto článcích:

* [Nasazení clusteru Azure Container Service](container-service-deployment.md)
* [Připojení ke clusteru Azure Container Service](../container-service-connect.md)

> [!NOTE]
> Tento článek předpokládá, že máte k dispozici tunel na clusteru DC/OS prostřednictvím místního portu 80.
>

## <a name="explore-the-dcos-ui"></a>Zkoumáme uživatelské rozhraní DC/OS
Po [vytvoření](../container-service-connect.md) tunelu Secure Shell (SSH) přejděte na http://localhost/. Načte se webové uživatelské rozhraní DC/OS a zobrazí se informace o clusteru, například využité prostředky, aktivní agenti a spuštěné služby.

![Uživatelské rozhraní DC/OS](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Zkoumáme uživatelské rozhraní Marathon
Pokud chcete zobrazit uživatelské rozhraní Marathon, přejděte na http://localhost/marathon. Prostřednictvím této obrazovky můžete spustit nový kontejner nebo jinou aplikaci z clusteru Azure Container Service na bázi DC/OS. Taky uvidíte informace o spuštěných kontejnerech a aplikacích.  

![Uživatelské rozhraní Marathon](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Nasazení kontejneru formátovaného Dockerem
Pokud chcete pomocí Marathonu nasadit nový kontejner, klikněte na **Create Application** (Vytvořit aplikaci) a zadejte na kartách formuláře následující informace:

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

Pokud chcete staticky namapovat port kontejneru na port agenta, budete muset použít režim JSON. Provedete to tak, že pomocí přepínače přepnete průvodce novou aplikací na **JSON Mode** (Režim JSON). Potom v části `portMappings` definice aplikace zadejte následující nastavení. Tento příklad namapuje port číslo 80 kontejneru na port číslo 80 agenta DC/OS. Po provedení této změny můžete režim JSON v průvodci opět vypnout.

```none
"hostPort": 80,
```

![Nová aplikace uživatelského rozhraní – příklad u portu číslo 80](./media/container-service-mesos-marathon-ui/dcos13.png)

Pokud chcete povolit kontroly stavu, nastavte cestu na kartě **Health Checks** (Kontroly stavu).

![Nová aplikace uživatelského rozhraní – kontroly stavu](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

Cluster DC/OS se nasazuje se sadou privátních a veřejných agentů. Pokud chcete, aby měl cluster přístup k aplikacím z internetu, musíte aplikace nasadit s použitím veřejného agenta. Uděláte to následovně: V průvodci novou aplikací vyberete kartu **Optional** (Volitelné) a do **Accepted Resource Roles** (Povolené role prostředků) zadáte **slave_public**.

Pak klikněte na **Create Application** (Vytvořit aplikaci).

![Nová aplikace uživatelského rozhraní – nastavení veřejného agenta](./media/container-service-mesos-marathon-ui/dcos14.png)

Když se vrátíte na hlavní stránku Marathonu, uvidíte stav nasazení daného kontejneru. Zpočátku uvidíte stav **Deploying** (Nasazování). Po úspěšném nasazení se stav změní na **Running** (spuštěno).

![Hlavní stránka uživatelského rozhraní Marathon – stav nasazení kontejneru](./media/container-service-mesos-marathon-ui/dcos7.png)

Když přepnete zpět do webového uživatelského rozhraní DC/OS (http://localhost/), uvidíte, že úloha (v tomto případě kontejner formátovaný Dockerem) je spuštěná na clusteru DC/OS.

![Webové uživatelské rozhraní DC/OS – úloha spuštěná na clusteru](./media/container-service-mesos-marathon-ui/dcos8.png)

Pokud chcete zobrazit uzel clusteru, na kterém je úloha spuštěná, klikněte na kartu **Nodes** (Uzly).

![Webové uživatelské rozhraní DC/OS – uzel clusteru úlohy](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-the-container"></a>Dosažení kontejneru

V tomto příkladu aplikace běží na uzlu veřejného agenta. Nedostanete aplikaci z Internetu procházením agenta plně kvalifikovaný název domény clusteru: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, kde:

* **DNSPREFIX** je předpona DNS zadaná ve chvíli, kdy jste nasadili cluster.
* **REGION** je oblast, ve které je umístěna skupina prostředků.

    ![Nginx z Internetu](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a>Další kroky
* [Práce s DC/OS a rozhraním API Marathonu](container-service-mesos-marathon-rest.md)

* Podrobná prohlídka služby Azure Container Service s Mesos

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
