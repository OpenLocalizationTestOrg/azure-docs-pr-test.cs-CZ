---
title: "kurz pro službu kontejneru aaaAzure – spravovat DC/OS | Microsoft Docs"
description: "Kurz pro Azure Container Service – spravovat DC/OS"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b91c433bfd7e48ec405cc62be1486d9d4662839d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-service-tutorial---manage-dcos"></a>Kurz pro Azure Container Service – spravovat DC/OS

DC/OS poskytuje distribuované platformu pro spuštění moderní a kontejnerizované aplikace. S Azure Container Service zřizování provozní připravený cluster DC/OS je jednoduchý a rychlé. Tento úvodní podrobnosti základní kroky potřebné toodeploy clusteru DC/OS a spuštění základní úlohy.

> [!div class="checklist"]
> * Vytvoření clusteru služby ACS DC/OS
> * Připojte toohello cluster
> * Nainstalujte hello rozhraní příkazového řádku DC/OS
> * Nasazení clusteru toohello aplikace
> * Škálování aplikace na clusteru hello
> * Škálování uzly clusteru DC/OS hello
> * Základní správu DC/OS
> * Odstranění clusteru DC/OS hello

Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-dcos-cluster"></a>Vytvoření clusteru DC/OS

Nejprve vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westeurope* umístění.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Dále vytvořte cluster DC/OS s hello [vytvořit acs az](/cli/azure/acs#create) příkaz.

Hello následující příklad vytvoří cluster DC/OS s názvem *myDCOSCluster* a vytvoří klíče SSH, pokud dosud neexistují. toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

Po několika minutách hello příkaz dokončí a vrátí informace o nasazení hello.

## <a name="connect-toodcos-cluster"></a>Připojení clusteru tooDC/OS

Po vytvoření clusteru DC/OS, může být přistupuje prostřednictvím tunelového propojení SSH. Spusťte následující příkaz tooreturn hello veřejnou IP adresu hlavního serveru DC/OS hello hello. Tato IP adresa je uložené v proměnné a používat v dalším kroku hello.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

toocreate hello tunelového propojení SSH, spusťte následující příkaz hello a podle pokynů hello na obrazovce. Pokud port 80 je již používán, hello příkaz se nezdaří. Aktualizace hello tunelovým propojením tooone port není používán, jako například `85:localhost:80`. 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a>Instalace rozhraní příkazového řádku DC/OS

Instalace rozhraní příkazového řádku DC/OS hello pomocí hello [az acs orchestrátoru DC/OS instalace rozhraní příkazového řádku](/azure/acs/dcos#install-cli) příkaz. Pokud používáte Azure CloudShell, hello rozhraní příkazového řádku DC/OS je již nainstalován. Pokud používáte hello rozhraní příkazového řádku Azure v systému macOS nebo Linux, bude pravděpodobně nutné příkaz hello toorun s sudo.

```azurecli
az acs dcos install-cli
```

Před hello rozhraní příkazového řádku lze použít s hello clusteru musí být nakonfigurované toouse tunelové propojení SSH hello. toodo tak, spusťte následující příkaz, úpravě hello portu v případě potřeby hello.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Spuštění aplikace

Výchozí hodnota Hello plánování mechanismus pro cluster služby ACS DC/OS je Marathon. Marathonu je použité toostart aplikace a spravovat stav hello hello aplikace v clusteru DC/OS hello. tooschedule aplikaci přes Marathon, vytvořte soubor s názvem **marathon app.json**, a kopírování hello do něj následující obsah. 

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Spusťte následující příkaz tooschedule hello aplikace toorun na clusteru DC/OS hello hello.

```azurecli
dcos marathon app add marathon-app.json
```

toosee hello stav nasazení pro aplikaci hello, spusťte následující příkaz hello.

```azurecli
dcos marathon app list
```

Když hello **úlohy** hodnota sloupce přepíná z *0 nebo 1* příliš*1 nebo 1*, nasazení aplikace byla dokončena.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a>Škálování aplikací Marathon

V předchozím příkladu hello byla vytvořena jedna instance aplikace. Toto nasazení tak, aby byly k dispozici tři instance aplikace hello otevře hello tooupdate **marathon app.json** souboru a aktualizovat too3 vlastnost instance hello.

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 3,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Aktualizovat aplikaci hello pomocí hello `dcos marathon app update` příkaz.

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

toosee hello stav nasazení pro aplikaci hello, spusťte následující příkaz hello.

```azurecli
dcos marathon app list
```

Když hello **úlohy** hodnota sloupce přepíná z *1/3* příliš*3/1*, nasazení aplikace byla dokončena.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a>Spuštění aplikace dostupné pro internet

Hello clusteru ACS DC/OS se skládá ze dvou sad uzlu, hello jeden veřejný, která je přístupná na Internetu, a jeden privátní, která není přístupná na hello Internetu. Hello výchozí sada je hello privátní uzlů, která byla použita v posledním příkladu hello.

toomake aplikaci přístupné na hello internet, je nasadit sady toohello veřejné uzlu. toodo tedy poskytnout hello `acceptedResourceRoles` objektu hodnotu `slave_public`.

Vytvořte soubor s názvem **nginx public.json** a kopírování hello do něj následující obsah.

```json
{
  "id": "demo-app",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  },
  "acceptedResourceRoles": [
    "slave_public"
  ]
}
```

Spusťte následující příkaz tooschedule hello aplikace toorun na clusteru DC/OS hello hello.

```azurecli 
dcos marathon app add nginx-public.json
```

Získáte hello veřejnou IP adresu hello agentů veřejné clusteru DC/OS.

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Procházení toothis adresu vrátí hello výchozí NGINX lokality.

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a>Škálování clusteru DC/OS

V předchozích příkladech hello aplikace se dokončilo toomultiple instance. Hello DC/OS infrastruktury může být také škálovat tooprovide více nebo méně výpočetní kapacitu. To lze provést pomocí hello [škálování služby acs az]() příkaz. 

toosee hello aktuální počet agentů DC/OS použijte hello [zobrazit acs az](/cli/azure/acs#show) příkaz.

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

tooincrease hello počet too5, použijte hello [škálování služby acs az](/cli/azure/acs#scale) příkaz. 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a>Odstranění clusteru DC/OS

Pokud již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove clusteru DC/OS a všechny související prostředky.

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili o základní úlohy správy DC/OS, včetně následujících hello. 

> [!div class="checklist"]
> * Vytvoření clusteru služby ACS DC/OS
> * Připojte toohello cluster
> * Nainstalujte hello rozhraní příkazového řádku DC/OS
> * Nasazení clusteru toohello aplikace
> * Škálování aplikace na clusteru hello
> * Škálování uzly clusteru DC/OS hello
> * Odstranění clusteru DC/OS hello

ADVANCE toohello další kurz toolearn o načtení vyrovnávání aplikace v DC/OS v Azure. 

> [!div class="nextstepaction"]
> [Vyrovnávání zatížení aplikací](container-service-load-balancing.md)