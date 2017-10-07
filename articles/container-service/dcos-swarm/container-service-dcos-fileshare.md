---
title: "aaaFile sdílené složky pro cluster Azure DC/OS | Microsoft Docs"
description: "Vytvořit a připojit cluster DC/OS tooa sdílené složky souboru Azure Container Service"
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, Micro-services, Mesos, Azure, sdílení souborů, cifs"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: d18090d414a3e00202ccde442ac9b865d74f1e34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-mount-a-file-share-tooa-dcos-cluster"></a>Vytvořit a připojit cluster DC/OS tooa sdílené složky souborů
V tomto kurzu podrobné informace o tom, jak soubor sdílet v Azure a připojte ho na každého agenta a k hlavnímu serveru toocreate hello clusteru DC/OS. Nastavení sdílené složky je snazší tooshare soubory mezi vašeho clusteru, například konfigurace, přístup, protokoly a další. v tomto kurzu se dokončil Hello následující úlohy:

> [!div class="checklist"]
> * Vytvoření účtu úložiště Azure
> * Vytvoření sdílené složky
> * Připojit hello sdílenou složku v clusteru DC/OS hello

Budete potřebovat DC/OS ACS clusteru toocomplete hello kroky v tomto kurzu. V případě potřeby [tento ukázkový skript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) můžete vytvořit za vás.

Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-file-share-on-microsoft-azure"></a>Vytvoření sdílené složky v Microsoft Azure

Než sdílenou složku Azure pomocí clusteru služby ACS DC/OS, musí být vytvořeny hello úložiště účet a sdílení souborů. Spusťte následující skript toocreate hello úložiště a sdílení souborů hello. Aktualizujte hello parametry s thoes ze svého prostředí.

```azurecli-interactive
# Change these four parameters
DCOS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
DCOS_PERS_RESOURCE_GROUP=myResourceGroup
DCOS_PERS_LOCATION=eastus
DCOS_PERS_SHARE_NAME=dcosshare

# Create hello storage account with hello parameters
az storage account create -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -l $DCOS_PERS_LOCATION --sku Standard_LRS

# Export hello connection string as an environment variable, this is used when creating hello Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -o tsv`

# Create hello share
az storage share create -n $DCOS_PERS_SHARE_NAME
```

## <a name="mount-hello-share-in-your-cluster"></a>Připojit hello sdílenou složku v clusteru

V dalším kroku hello sdílená vyžaduje toobe připojit na každý virtuální počítač v rámci clusteru. Tato úloha je dokončit pomocí hello cifs nástroj nebo protokolu. operace připojení Hello můžete dokončit ručně na každém uzlu clusteru hello nebo spuštěním skriptu na každém uzlu v clusteru hello.

V tomto příkladu jsou dva skripty spustit, jeden toomount hello Azure souboru sdílené složky a druhý toorun tento skript na každém uzlu clusteru DC/OS hello.

Nejdřív je potřeba hello název účtu úložiště Azure a přístupový klíč. Spusťte následující příkazy tooget hello tyto informace. Všimněte si, se používají tyto hodnoty později.

Název účtu úložiště:

```azurecli-interactive
STORAGE_ACCT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCT
```

Přístupový klíč účtu úložiště:

```azurecli-interactive
az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCT --query "[0].value" -o tsv
```

V dalším kroku získáte hello plně kvalifikovaný název domény hello DC/OS hlavní a uložte ho do proměnné.

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

Zkopírujte vaší privátní klíče toohello hlavní uzel. Tento klíč je potřebné toocreate ssh připojení se všechny uzly v clusteru hello. Aktualizujte hello uživatelské jméno, pokud byla použita jiná než výchozí hodnotu, při vytváření clusteru hello. 

```azurecli-interactive
scp ~/.ssh/id_rsa azureuser@$FQDN:~/.ssh
```

Vytvoření připojení SSH s hello hlavního serveru (nebo hello první) clusteru založenými na DC/OS. Aktualizujte hello uživatelské jméno, pokud byla použita jiná než výchozí hodnotu, při vytváření clusteru hello.

```azurecli-interactive
ssh azureuser@$FQDN
```

Vytvořte soubor s názvem **cifsMount.sh**, a kopírování hello do něj následující obsah. 

Tento skript je použité toomount hello sdílenou složku Azure. Aktualizace hello `STORAGE_ACCT_NAME` a `ACCESS_KEY` proměnné s hello informace shromážděné dříve.

```azurecli-interactive
#!/bin/bash

# Azure storage account name and access key
STORAGE_ACCT_NAME=mystorageaccount
ACCESS_KEY=mystorageaccountKey

# Install hello cifs utils, should be already installed
sudo apt-get update && sudo apt-get -y install cifs-utils

# Create hello local folder that will contain our share
if [ ! -d "/mnt/share/dcosshare" ]; then sudo mkdir -p "/mnt/share/dcosshare" ; fi

# Mount hello share under hello previous local folder created
sudo mount -t cifs //$STORAGE_ACCT_NAME.file.core.windows.net/dcosshare /mnt/share/dcosshare -o vers=3.0,username=$STORAGE_ACCT_NAME,password=$ACCESS_KEY,dir_mode=0777,file_mode=0777
```
Vytvořte druhý soubor s názvem **getNodesRunScript.sh** a kopírování hello následující obsah do souboru hello. 

Tento skript zjistí všechny uzly clusteru a poté spustí hello **cifsMount.sh** skriptu toomount hello sdílené složce na každém.

```azurecli-interactive
#!/bin/bash

# Install jq used for hello next command
sudo apt-get install jq -y

# Get hello IP address of each node using hello mesos API and store it inside a file called nodes
curl http://leader.mesos:1050/system/health/v1/nodes | jq '.nodes[].host_ip' | sed 's/\"//g' | sed '/172/d' > nodes

# From hello previous file created, run our script toomount our share on each node
cat nodes | while read line
do
  ssh `whoami`@$line -o StrictHostKeyChecking=no < ./cifsMount.sh
  done
```

Spouštět hello skriptu toomount hello sdílenou složku Azure na všech uzlech clusteru hello.

```azurecli-interactive
sh ./getNodesRunScript.sh
```  

Hello sdílení souborů je nyní přístupná na `/mnt/share/dcosshare` na každém uzlu clusteru hello.

## <a name="next-steps"></a>Další kroky

V tomto kurzu Azure sdílenou složku byla učiněna k dispozici tooa clusteru DC/OS pomocí hello kroky:

> [!div class="checklist"]
> * Vytvoření účtu úložiště Azure
> * Vytvoření sdílené složky
> * Připojit hello sdílenou složku v clusteru DC/OS hello

Posunutí další kurz toolearn toohello o integraci služby Azure kontejneru registru s DC/OS v Azure.  

> [!div class="nextstepaction"]
> [Vyrovnávání zatížení aplikací](container-service-dcos-acr.md)