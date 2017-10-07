---
title: aaaQuickstart - clusteru Azure Docker CE pro Linux | Microsoft Docs
description: "Naučte se rychle toocreate clusteru Docker CE Linux kontejnerů v Azure Container Service s hello rozhraní příkazového řádku Azure."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, Docker, Swarm
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nepeters
ms.custom: 
ms.openlocfilehash: 6c26c12ed085ec379c3486095a5fa51379afc5a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-docker-ce-cluster"></a>Nasazení clusteru Docker CE

V této úvodní clusteru Docker CE je nasazená pomocí hello rozhraní příkazového řádku Azure. Aplikace více kontejneru, který se skládá z webového front-endu a instanci Redis nasazení a poté běží na clusteru hello. Po dokončení aplikace hello je přístupné prostřednictvím Internetu hello.

Docker CE ve službě Azure Container Service je ve verzi Preview a **neměl by se používat pro produkční úlohy**.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logická skupina, ve které se nasazují a spravují prostředky Azure.

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *ukwest* umístění.

```azurecli-interactive
az group create --name myResourceGroup --location ukwest
```

Výstup:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westcentralus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-docker-swarm-cluster"></a>Vytvoření clusteru Docker Swarm

Vytvoření clusteru Docker CE v Azure Container Service s hello [vytvořit acs az](/cli/azure/acs#create) příkaz. 

Hello následující příklad vytvoří cluster s názvem *mySwarmCluster* s Linuxem jeden hlavní uzel a tři uzly Linux agent.

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type dockerce --resource-group myResourceGroup --generate-ssh-keys
```

Po několika minutách hello příkaz dokončí a vrátí formátu json informace o clusteru hello.

## <a name="connect-toohello-cluster"></a>Připojte toohello cluster

V rámci této úvodní musíte hello plně kvalifikovaný název domény hello Docker Swarm hlavní a hello Docker agenta fondu. Spusťte následující příkaz tooreturn hello obě hello hlavní a agent plně kvalifikované názvy domén.


```bash
az acs list --resource-group myResourceGroup --query '[*].{Master:masterProfile.fqdn,Agent:agentPoolProfiles[0].fqdn}' -o table
```

Výstup:

```bash
Master                                                               Agent
-------------------------------------------------------------------  --------------------------------------------------------------------
myswarmcluster-myresourcegroup-d5b9d4mgmt.ukwest.cloudapp.azure.com  myswarmcluster-myresourcegroup-d5b9d4agent.ukwest.cloudapp.azure.com
```

Vytvořte hlavní Swarm toohello tunelového propojení SSH. Nahraďte `MasterFQDN` s adresou hello plně kvalifikovaný název domény hlavního serveru Swarm hello.

```bash
ssh -p 2200 -fNL localhost:2374:/var/run/docker.sock azureuser@MasterFQDN
```

Sada hello `DOCKER_HOST` proměnné prostředí. To vám umožní toorun docker příkazy proti hello Docker Swarm bez nutnosti toospecify hello název hostitele hello.

```bash
export DOCKER_HOST=localhost:2374
```

Nyní je připraven toorun Docker služeb v hello Docker Swarm.


## <a name="run-hello-application"></a>Spuštění aplikace hello

Vytvořte soubor s názvem `azure-vote.yaml` a kopírování hello do něj následující obsah.


```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

Spustit hello [docker zásobníku nasazení](https://docs.docker.com/engine/reference/commandline/stack_deploy/) příkaz toocreate hello Azure hlas služby.

```bash
docker stack deploy azure-vote --compose-file azure-vote.yaml
```

Výstup:

```bash
Creating network azure-vote_default
Creating service azure-vote_azure-vote-back
Creating service azure-vote_azure-vote-front
```

Použití hello [docker zásobníku ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) příkaz tooreturn hello stav nasazení aplikace hello.

```bash
docker stack ps azure-vote
```

Jednou hello `CURRENT STATE` každé služby je `Running`, hello aplikace je připravena.

```bash
ID                  NAME                            IMAGE                                 NODE                               DESIRED STATE       CURRENT STATE                ERROR               PORTS
tnklkv3ogu3i        azure-vote_azure-vote-front.1   microsoft/azure-vote-front:redis-v1   swarmm-agentpool0-66066781000004   Running             Running 5 seconds ago                            
lg99i4hy68r9        azure-vote_azure-vote-back.1    redis:latest                          swarmm-agentpool0-66066781000002   Running             Running about a minute ago
```

## <a name="test-hello-application"></a>Testování aplikace hello

Procházejte toohello plně kvalifikovaný název domény hello Swarm agenta fondu tootest out hello hlas Azure aplikace.

![Obrázek procházení tooAzure hlas](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Odstranění clusteru
Pokud hello cluster je již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, container service a všechny související prostředky.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-hello-code"></a>Získat kód hello

V této úvodní předem vytvořené kontejneru bitové kopie byly použité toocreate Docker služby. Hello související s kódu aplikace, soubor Docker, a soubor vytvářené jsou dostupné na Githubu.

[https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a>Další kroky

V této úvodní nasazení clusteru Docker Swarm a nasadit aplikace s více kontejnerů tooit.

toolearn o Docker záložním integraci s Visual Studio Team Services, pokračovat toohello CI/CD s Docker Swarm a služby VSTS.

> [!div class="nextstepaction"]
> [CI/CD s Docker Swarm a VSTS](./container-service-docker-swarm-setup-ci-cd.md)