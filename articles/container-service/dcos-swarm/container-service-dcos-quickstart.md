---
title: "aaaAzure kontejneru služby rychlý start - nasadit Cluster DC/OS | Microsoft Docs"
description: "Kontejner Azure rychlý start služba – nasazení clusteru DC/OS"
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
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b961f15bd73deeafda5a3fc25ab53c839195219b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-dcos-cluster"></a>Nasadit cluster DC/OS

DC/OS poskytuje distribuované platformu pro spuštění moderní a kontejnerizované aplikace. S Azure Container Service zřizování provozní připravený cluster DC/OS je jednoduchý a rychlé. Tento základní postup úvodní podrobnosti hello potřeba toodeploy clusteru DC/OS a spuštění základní úlohy.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure 

Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a>Vytvoření clusteru DC/OS

Vytvořte cluster DC/OS s hello [vytvořit acs az](/cli/azure/acs#create) příkaz.

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

tunelové propojení SSH Hello může být testována procházením příliš`http://localhost`. Pokud port dalších že byl použit 80, upravte toomatch umístění hello. 

Pokud tunelu SSH hello byla úspěšně vytvořena, bude vrácena hello DC/OS portálu.

![ORCHESTRÁTORU DC/OS UŽIVATELSKÉHO ROZHRANÍ](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a>Instalace rozhraní příkazového řádku DC/OS

rozhraní příkazového řádku Hello DC/OS je použité toomanage cluster DC/OS z příkazového řádku hello. Instalace rozhraní příkazového řádku DC/OS hello pomocí hello [az acs orchestrátoru DC/OS instalace rozhraní příkazového řádku](/azure/acs/dcos#install-cli) příkaz. Pokud používáte Azure CloudShell, hello rozhraní příkazového řádku DC/OS je již nainstalován. 

Pokud používáte hello rozhraní příkazového řádku Azure v systému macOS nebo Linux, bude pravděpodobně nutné příkaz hello toorun s sudo.

```azurecli
az acs dcos install-cli
```

Před hello rozhraní příkazového řádku lze použít s hello clusteru musí být nakonfigurované toouse tunelové propojení SSH hello. toodo tak, spusťte následující příkaz, úpravě hello portu v případě potřeby hello.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Spuštění aplikace

Výchozí hodnota Hello plánování mechanismus pro cluster služby ACS DC/OS je Marathon. Marathonu je použité toostart aplikace a spravovat stav hello hello aplikace v clusteru DC/OS hello. tooschedule aplikaci přes Marathon, vytvořte soubor s názvem *marathon app.json*, a kopírování hello do něj následující obsah. 

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
dcos marathon app add marathon-app.json
```

toosee hello stav nasazení pro aplikaci hello, spusťte následující příkaz hello.

```azurecli
dcos marathon app list
```

Když hello **čekání** hodnota sloupce přepíná z *True* příliš*False*, nasazení aplikace byla dokončena.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

Získáte hello veřejnou IP adresu hello agentů clusteru DC/OS.

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Procházení toothis adresu vrátí hello výchozí NGINX lokality.

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a>Odstranění clusteru DC/OS

Pokud již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove clusteru DC/OS a všechny související prostředky.

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Další kroky

V této úvodní jste nasadili cluster DC/OS a spustili jednoduché kontejner Docker v clusteru hello. toolearn Další informace o Azure Container Service pokračovat kurzy toohello služby ACS.

> [!div class="nextstepaction"]
> [Správa clusteru služby ACS DC/OS](container-service-dcos-manage-tutorial.md)