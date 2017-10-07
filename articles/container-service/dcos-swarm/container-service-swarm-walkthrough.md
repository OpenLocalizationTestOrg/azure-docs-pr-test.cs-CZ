---
title: aaaQuickstart - Azure Docker Swarm clusteru pro Linux | Microsoft Docs
description: "Naučte se rychle toocreate clusteru Docker Swarm Linux kontejnerů v Azure Container Service s hello rozhraní příkazového řádku Azure."
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
ms.date: 08/14/2017
ms.author: nepeters
ms.custom: 
ms.openlocfilehash: 3028d2d00585360ec163518bf98f69bb0dd44dec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-docker-swarm-cluster"></a>Nasazení clusteru Docker Swarm

V této úvodní clusteru Docker Swarm je nasazená pomocí hello rozhraní příkazového řádku Azure. Aplikace více kontejneru, který se skládá z webového front-endu a instanci Redis nasazení a poté běží na clusteru hello. Po dokončení aplikace hello je přístupné prostřednictvím Internetu hello.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logická skupina, ve které se nasazují a spravují prostředky Azure.

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westus* umístění.

```azurecli-interactive
az group create --name myResourceGroup --location westus
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

Vytvoření clusteru Docker Swarm v Azure Container Service s hello [vytvořit acs az](/cli/azure/acs#create) příkaz. 

Hello následující příklad vytvoří cluster s názvem *mySwarmCluster* s Linuxem jeden hlavní uzel a tři uzly Linux agent.

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type Swarm --resource-group myResourceGroup --generate-ssh-keys
```

Po několika minutách hello příkaz dokončí a vrátí formátu json informace o clusteru hello.

## <a name="connect-toohello-cluster"></a>Připojte toohello cluster

V rámci této úvodní potřebujete adresu IP hello hello Docker Swarm hlavní a hello Docker agenta fondu. Spusťte následující příkaz tooreturn hello obě IP adresy.


```bash
az network public-ip list --resource-group myResourceGroup --query '[*].{Name:name,IPAddress:ipAddress}' -o table
```

Výstup:

```bash
Name                                                                 IPAddress
-------------------------------------------------------------------  -------------
swarmm-agent-ip-myswarmcluster-myresourcegroup-d5b9d4agent-66066781  52.179.23.131
swarmm-master-ip-myswarmcluster-myresourcegroup-d5b9d4mgmt-66066781  52.141.37.199
```

Vytvořte hlavní Swarm toohello tunelového propojení SSH. Nahraďte `IPAddress` s IP adresou hello hello Swarm hlavního serveru.

```bash
ssh -p 2200 -fNL 2375:localhost:2375 azureuser@IPAddress
```

Sada hello `DOCKER_HOST` proměnné prostředí. To vám umožní toorun docker příkazy proti hello Docker Swarm bez nutnosti toospecify hello název hostitele hello.

```bash
export DOCKER_HOST=:2375
```

Nyní je připraven toorun Docker služeb v hello Docker Swarm.


## <a name="run-hello-application"></a>Spuštění aplikace hello

Vytvořte soubor s názvem `docker-compose.yaml` a kopírování hello do něj následující obsah.

```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    container_name: azure-vote-back
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    container_name: azure-vote-front
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

Spusťte následující příkaz toocreate hello Azure hlas služby hello.

```bash
docker-compose up -d
```

Výstup:

```bash
Creating network "user_default" with hello default driver
Pulling azure-vote-front (microsoft/azure-vote-front:redis-v1)...
swarm-agent-EE873B23000005: Pulling microsoft/azure-vote-front:redis-v1...
swarm-agent-EE873B23000004: Pulling microsoft/azure-vote-front:redis-v1... : downloaded
Pulling azure-vote-back (redis:latest)...
swarm-agent-EE873B23000004: Pulling redis:latest... : downloaded
Creating azure-vote-front ... 
Creating azure-vote-back ... 
Creating azure-vote-front
Creating azure-vote-back ...
```

## <a name="test-hello-application"></a>Testování aplikace hello

Procházejte toohello IP adresu hello Swarm agenta fondu tootest si aplikaci Azure hlas hello.

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