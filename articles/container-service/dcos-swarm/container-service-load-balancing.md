---
title: "kontejnery vyrovnávání aaaLoad v clusteru Azure DC/OS | Microsoft Docs"
description: "Vyrovnávání zatížení v rámci několika kontejnerů na clusteru Azure Container Service DC/OS."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kontejnery, mikroslužby, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 2249cb06880cdb7e9a3aa94c0750c6a27316d349
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a>Kontejnery Vyrovnávání zatížení v clusteru služby Azure Container Service DC/OS
V tomto článku jsme prozkoumejte spravováni Azure Container Service pomocí Marathon-LB toocreate interní zátěže v DC/OS. Tato konfigurace umožňuje vám tooscale vodorovně vaší aplikace. Můžete taky využít tootake hello veřejné a privátní agenta clusterů tím, že nástroje pro vyrovnávání zatížení v clusteru veřejné hello a vaše kontejnery aplikací v clusteru privátní hello. V tomto kurzu jste:

> [!div class="checklist"]
> * Konfigurace služby Vyrovnávání zatížení Marathon
> * Nasazení aplikace pomocí nástroje pro vyrovnávání zatížení hello
> * Konfigurace a nástroj pro vyrovnávání zatížení Azure

Budete potřebovat DC/OS ACS clusteru toocomplete hello kroky v tomto kurzu. V případě potřeby [tento ukázkový skript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) můžete vytvořit za vás.

Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a>Přehled Vyrovnávání zatížení

Existují dvě vrstvy Vyrovnávání zatížení v clusteru služby Azure Container Service DC/OS: 

**Azure nástroj pro vyrovnávání zatížení** poskytuje veřejné vstupní body (ty, které této koncoví uživatelé přístup hello). Azure LB automaticky poskytuje Azure Container Service a je ve výchozím nastavení nakonfigurované tooexpose port 80 a 443 8080.

**Hello Marathon nástroj pro vyrovnávání zatížení (marathon-lb)** trasy příchozí požadavky toocontainer instancí, které tyto žádosti o služby. Škály hello kontejnerů, které poskytují naše webové služby, hello marathon-lb dynamicky přizpůsobení. Tento nástroj pro vyrovnávání zatížení není dostupné ve výchozím nastavení v kontejneru služby, ale je snadno tooinstall.

## <a name="configure-marathon-load-balancer"></a>Konfigurace pro vyrovnávání zatížení Marathon

Nástroj pro vyrovnávání zatížení Marathon dynamicky změní konfiguraci založena na hello kontejnery, které jste nasadili. Je také odolný toohello ztrátě kontejneru nebo agenta – Pokud k tomu dojde, restartuje hello kontejner na jiném místě Apache Mesos a marathon-lb přizpůsobení.

Spusťte následující příkaz tooinstall hello marathon nástroj pro vyrovnávání zatížení v clusteru hello veřejného agenta hello.

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a>Nasazení aplikace skupinu s vyrovnáváním zatížení

Teď, když máme balíček marathon-lb hello, můžeme nasadit kontejner aplikace, že nám chcete vyrovnávat tooload. 

Nejdřív získáte hello plně kvalifikovaný název domény hello veřejně vystaven agentů.

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

Dále vytvořte soubor s názvem *hello web.json* a kopírování v hello následující obsah. Hello `HAPROXY_0_VHOST` popisku musí toobe aktualizováno hello plně kvalifikovaný název domény hello agentů DC/OS. 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

Hello rozhraní příkazového řádku DC/OS toorun hello aplikaci použijte. Ve výchozím nastavení nasadí Marathon hello hello aplikace toohello privátní clusteru. To znamená, že hello výše nasazení je pouze přístupné přes nástroj pro vyrovnávání zatížení, která je obvykle hello požadovaného chování.

```azurecli-interactive
dcos marathon app add hello-web.json
```

Jakmile aplikace hello nasazen, procházejte toohello plně kvalifikovaný název domény aplikace hello agenta clusteru tooview skupinu s vyrovnáváním zatížení.

![Bitové kopie aplikace skupinu s vyrovnáváním zatížení](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a>Konfigurace pro vyrovnávání zatížení Azure

Služba Azure Load Balancer ve výchozím nastavení zpřístupňuje porty 80, 8080 a 443. Pokud používáte některý z těchto tří portů (jako My v hello výše příkladu), pak není co že budete potřebovat toodo. Musí být schopný toohit plně kvalifikovaný název domény služby Vyrovnávání zatížení agenta a pokaždé, když aktualizujete, můžete budete použít jeden ze tří webových serverů v kruhového dotazování. 

Pokud používáte jiný port, musíte pravidlo tooadd kruhového dotazování a kontroly na Vyrovnávání zatížení hello hello portu, který jste použili pro. Můžete to provést z hello [rozhraní příkazového řádku Azure](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), pomocí příkazů hello `azure network lb rule create` a `azure network lb probe create`.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli o vyrovnávání zatížení v ACS s hello Marathon i zatížení Azure včetně služby Vyrovnávání hello následující akce:

> [!div class="checklist"]
> * Konfigurace služby Vyrovnávání zatížení Marathon
> * Nasazení aplikace pomocí nástroje pro vyrovnávání zatížení hello
> * Konfigurace a nástroj pro vyrovnávání zatížení Azure

Posunutí další kurz toolearn toohello o integraci Azure storage s DC/OS v Azure.

> [!div class="nextstepaction"]
> [Azure připojit sdílenou složku v clusteru DC/OS](container-service-dcos-fileshare.md)