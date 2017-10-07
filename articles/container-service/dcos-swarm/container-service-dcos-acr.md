---
title: "aaaUsing ACR pomocí clusteru služby Azure DC/OS | Microsoft Docs"
description: "Cluster DC/OS v Azure Container Service pomocí registru kontejneru služby Azure"
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service, acr, azure-container-registry
keywords: "Docker, kontejnery, Micro-services, Mesos, Azure, sdílení souborů, cifs"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: 9a2802da788b50147d8b4259436bdcdb0c929db8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-acr-with-a-dcos-cluster-toodeploy-your-application"></a>Pomocí ACR toodeploy clusteru DC/OS vaší aplikace

V tomto článku jsme prozkoumat jak toouse registru kontejner Azure s cluster DC/OS. Pomocí ACR vám umožní tooprivately úložiště a správě bitových kopií kontejneru. Tento kurz se zabývá hello následující úlohy:

> [!div class="checklist"]
> * Nasazení registru kontejner Azure (v případě potřeby)
> * Konfigurace ověřování ACR na cluster DC/OS
> * Nahrát bitovou kopii toohello registru kontejner Azure
> * Spusťte kontejner image z hello registru kontejner Azure

Budete potřebovat DC/OS ACS clusteru toocomplete hello kroky v tomto kurzu. V případě potřeby [tento ukázkový skript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) můžete vytvořit za vás.

Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a>Nasadit kontejner Azure registru

V případě potřeby vytvořte registru kontejneru Azure s hello [vytvořit az acr](/cli/azure/acr#create) příkaz. 

Hello následující příklad vytvoří registr s využitím náhodnému generování název. Hello registru je také nakonfigurovaný se na správce účtu pomocí hello `--admin-enabled` argument.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic --admin-enabled true
```

Po vytvoření hello registru hello rozhraní příkazového řádku Azure výstupy podobné toohello následující data. Poznamenejte si hello `name` a `loginServer`, ty se používají v dalších krocích.

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T03:40:56.511597+00:00",
  "id": "/subscriptions/f2799821-a08a-434e-9128-454ec4348b10/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry23489",
  "location": "eastus",
  "loginServer": "mycontainerregistry23489.azurecr.io",
  "name": "myContainerRegistry23489",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr034017"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Získat hello kontejneru registru přihlašovacích údajů pomocí hello [az acr pověření zobrazit](/cli/azure/acr/credential) příkaz. SUBSTITUTE hello `--name` s hello jeden si poznamenali v posledním kroku hello. Všimněte si jeden hesla, je potřeba v pozdější fázi.

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

Další informace o registru kontejner Azure najdete v tématu [registrech kontejner Docker tooprivate ÚVOD](../../container-registry/container-registry-intro.md). 

## <a name="manage-acr-authentication"></a>Spravovat ACR ověřování

Hello konvenční způsob, jakým toopush a vyžadování image z privátní registru je toofirst ověřit pomocí registru hello. toodo, takže byste použili hello `docker login` na libovolného klienta, který potřebuje tooaccess hello privátní registru. Vzhledem k tomu cluster DC/OS může obsahovat mnoho uzlů, které potřebovat toobe ověření pomocí hello ACR, je užitečné tooautomate tento proces v každém uzlu. 

### <a name="create-shared-storage"></a>Vytvoření sdílené úložiště

Tento proces používá sdílenou složku Azure, namontování na každém uzlu v clusteru hello. Pokud již jste nenastavili sdílené úložiště, najdete v části [nastavit sdílenou složku v clusteru DC/OS](container-service-dcos-fileshare.md).

### <a name="configure-acr-authentication"></a>Konfigurace ověřování ACR

Nejdřív získáte hello plně kvalifikovaný název domény hello DC/OS hlavní a uložte ho do proměnné.

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

Vytvoření připojení SSH s hello hlavního serveru (nebo hello první) clusteru založenými na DC/OS. Aktualizujte hello uživatelské jméno, pokud byla použita jiná než výchozí hodnotu, při vytváření clusteru hello.

```azurecli-interactive
ssh azureuser@$FQDN
```

Spusťte následující příkaz toologin toohello registru kontejner Azure hello. Nahraďte hello `--username` s názvem hello hello kontejneru registru a hello `--password` s jedním z hello zadaná hesla. Nahraďte hello poslední argument *mycontainerregistry.azurecr.io* v příkladu hello s názvem loginServer hello hello kontejneru registru. 

Tento příkaz uloží hodnoty ověřování hello místně v rámci hello `~/.docker` cesta.

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

Vytvořte komprimovaný soubor, který obsahuje hodnoty ověřování registru hello kontejneru.

```azurecli-interactive
tar czf docker.tar.gz .docker
```

Zkopírujte tento soubor toohello sdílené úložiště clusteru. Tento krok zpřístupní hello soubor na všech uzlech clusteru DC/OS hello.

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-tooacr"></a>Nahrát tooACR bitové kopie

Nyní z vývojovém počítači, nebo všechny ostatní systémy s Docker nainstalovaná, vytvoření bitové kopie a nahrajte ho toohello registru kontejner Azure.

Vytvořte kontejner z hello Ubuntu image.

```azurecli-interactive
docker run ubunut --name base-image
```

Nyní zachytit hello kontejneru do novou bitovou kopii. název bitové kopie Hello musí tooinclude hello `loginServer` název kontejneru registrywith hello a formát `loginServer/imageName`.

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

Přihlaste se do hello registru kontejner Azure. Nahraďte název hello názvem hello loginServer, hello – uživatelské jméno s názvem hello hello kontejneru registru a hello – heslo s jedním z hello zadaná hesla.

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

Nakonec nahrajte hello image toohello ACR registru. Tento příklad odešle obrázek s názvem demo orchestrátoru DC/OS.

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a>Spusťte bitovou kopii z ACR

toouse na bitovou kopii z registru ACR hello, vytvořte soubor názvy *acrDemo.json* a kopírování hello následující text do ní. Nahraďte název bitové kopie hello název loginServer registru hello kontejneru a název bitové kopie, například `loginServer/imageName`. Poznamenejte si hello `uris` vlastnost. Tato vlastnost obsahuje umístění hello hello kontejneru registru ověřování souboru, který v tomto případě je hello Azure sdílené složky, která je připojena v každém uzlu v clusteru DC/OS hello.

```json
{
  "id": "myapp",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "mycontainerregistry30678.azurecr.io/dcos-demo",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
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
  "uris":  [
       "file:///mnt/share/dcosshare/docker.tar.gz"
   ]
}
```

Nasazení aplikace hello s hello rozhraní příkazového řádku DC / ° c.

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu máte nakonfigurovat toouse DC/OS, které registru kontejner Azure včetně hello následující úkoly:

> [!div class="checklist"]
> * Nasazení registru kontejner Azure (v případě potřeby)
> * Konfigurace ověřování ACR na cluster DC/OS
> * Nahrát bitovou kopii toohello registru kontejner Azure
> * Spusťte kontejner image z hello registru kontejner Azure
