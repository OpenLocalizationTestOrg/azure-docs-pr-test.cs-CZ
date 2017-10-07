---
title: "aaaDeploy cluster kontejner Docker - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Nasazení řešení Kubernetes, DC/OS nebo Docker Swarm ve službě Azure Container Service pomocí Azure CLI 2.0"
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: saudas
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: cdfa4ce69de343dcc7070bc2c58b5132c4062084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-docker-container-hosting-solution-using-hello-azure-cli-20"></a>Nasazení řešení pomocí Azure CLI 2.0 hello hostování kontejner Docker

Použití hello `az acs` příkazů v toocreate hello Azure CLI 2.0 a spravovat clustery v Azure Container Service. Můžete také nasazení clusteru Azure Container Service pomocí hello [portál Azure](container-service-deployment.md) nebo hello rozhraní API Správce Azure Container Service.

Nápovědu k `az acs` příkazy, předat hello `-h` parametr tooany příkaz. Například: `az acs create -h`.



## <a name="prerequisites"></a>Požadavky
toocreate clusteru Azure Container Service pomocí hello 2.0 rozhraní příkazového řádku Azure, musíte:
* účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)),
* instalaci a nastavení hello [2.0 rozhraní příkazového řádku Azure](/cli/azure/install-az-cli2)

## <a name="get-started"></a>Začínáme 
### <a name="log-in-tooyour-account"></a>Přihlaste se tooyour účtu
```azurecli
az login 
```

Postupujte podle pokynů toolog hello v interaktivně. Ostatní metody toolog v, najdete v části [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-az-cli2).

### <a name="set-your-azure-subscription"></a>Nastavení předplatného Azure

Pokud máte více než jedno předplatné, nastavte hello výchozí předplatné. Například:

```
az account set --subscription "f66xxxxx-xxxx-xxxx-xxx-zgxxxx33cha5"
```


### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Doporučujeme vytvořit skupinu prostředků pro každý cluster. Zadejte oblast Azure, ve které je Azure Container Service [k dispozici](https://azure.microsoft.com/en-us/regions/services/). Například:

```azurecli
az group create -n acsrg1 -l "westus"
```
Výstup je podobné toohello následující:

![Vytvoření skupiny prostředků](./media/container-service-create-acs-cluster-cli/rg-create.png)


## <a name="create-an-azure-container-service-cluster"></a>Vytvoření clusteru Azure Container Service

toocreate cluster, použijte `az acs create`.
Název pro hello cluster a hello název skupiny prostředků hello vytvořili v předchozím kroku hello jsou povinné parametry. 

Další vstupní hodnoty jsou nastavené hodnoty toodefault (viz následující obrazovka hello) není-li přepsat pomocí jejich odpovídajících přepínačů. Hello orchestrator je třeba nastavit ve výchozím nastavení tooDC/OS. A pokud nezadáte jeden, předpony názvu DNS je vytvořena na základě názvu clusteru hello.

![použití příkazu az acs create](./media/container-service-create-acs-cluster-cli/create-help.png)


### <a name="quick-acs-create-using-defaults"></a>Rychlý příkaz `acs create` s využitím výchozích hodnot
Pokud máte soubor veřejného klíče SSH RSA `id_rsa.pub` ve výchozím umístění hello (nebo jeden pro vytvoření [OS X a Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) nebo [Windows](../../virtual-machines/linux/ssh-from-windows.md)), použijte příkaz jako hello následující:

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```
Pokud veřejný klíč SSH nemáte, použijte druhý příkaz. Tento příkaz s hello `--generate-ssh-keys` přepínač vytvoří za vás.

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

Po zadání příkazu hello, Počkejte přibližně 10 minut pro toobe hello clusteru vytvořen. výstup příkazu Hello zahrnuje plně kvalifikované názvy domény (FQDN) hello hlavní a uzly agenta a SSH příkaz tooconnect toohello prvního hlavního serveru. Tady je zkrácený výstup:

![Obrázek s příkazem ACS create](./media/container-service-create-acs-cluster-cli/cluster-create.png)

> [!TIP]
> Hello [Kubernetes návod](../kubernetes/container-service-kubernetes-walkthrough.md) ukazuje, jak toouse `az acs create` s toocreate výchozí hodnoty Kubernetes clusteru.
>

## <a name="manage-acs-clusters"></a>Správa clusterů ACS

Použití další `az acs` příkazy toomanage vašeho clusteru. Zde je několik příkladů:

### <a name="list-clusters-under-a-subscription"></a>Výpis clusterů v rámci předplatného

```azurecli
az acs list --output table
```

### <a name="list-clusters-in-a-resource-group"></a>Výpis clusterů ve skupině prostředků

```azurecli
az acs list -g acsrg1 --output table
```

![příkaz acs list](./media/container-service-create-acs-cluster-cli/acs-list.png)


### <a name="display-details-of-a-container-service-cluster"></a>Zobrazení podrobností o clusteru Container Service

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![příkaz acs show](./media/container-service-create-acs-cluster-cli/acs-show.png)


### <a name="scale-hello-cluster"></a>Škálování hello clusteru
Je povolené horizontální navýšení i snížení kapacity agentských uzlů. Hello parametr `new-agent-count` je hello nový počet agentů v clusteru hello služby ACS.

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![příkaz acs scale](./media/container-service-create-acs-cluster-cli/acs-scale.png)

## <a name="delete-a-container-service-cluster"></a>Odstranění clusteru služby Container Service
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
Tento příkaz neodstraní všechny prostředky (sítě a úložiště), které jsou vytvořené během vytváření služby kontejneru hello. toodelete všechny prostředky snadno, je doporučeno, nasadíte každý cluster ve skupině prostředků jedinečné. Potom odstraňte skupinu prostředků hello při hello clusteru se už nevyžaduje.

## <a name="next-steps"></a>Další kroky
Nyní když máte funkční cluster, nahlédněte do těchto dokumentů, kde naleznete podrobnosti týkající se připojení a správy:

* [Připojení clusteru Azure Container Service tooan](../container-service-connect.md)
* [Práce se službou Azure Container Service a DC/OS](container-service-mesos-marathon-rest.md)
* [Práce se službou Azure Container Service a nástrojem Docker Swarm](container-service-docker-swarm.md)
* [Práce s Azure Container Service a Kubernetes](../kubernetes/container-service-kubernetes-walkthrough.md)