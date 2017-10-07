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
# <a name="create-and-mount-a-file-share-tooa-dcos-cluster"></a><span data-ttu-id="ba44a-104">Vytvořit a připojit cluster DC/OS tooa sdílené složky souborů</span><span class="sxs-lookup"><span data-stu-id="ba44a-104">Create and mount a file share tooa DC/OS cluster</span></span>
<span data-ttu-id="ba44a-105">V tomto kurzu podrobné informace o tom, jak soubor sdílet v Azure a připojte ho na každého agenta a k hlavnímu serveru toocreate hello clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="ba44a-105">This tutorial details how toocreate a file share in Azure and mount it on each agent and master of hello DC/OS cluster.</span></span> <span data-ttu-id="ba44a-106">Nastavení sdílené složky je snazší tooshare soubory mezi vašeho clusteru, například konfigurace, přístup, protokoly a další.</span><span class="sxs-lookup"><span data-stu-id="ba44a-106">Setting up a file share makes it easier tooshare files across your cluster such as configuration, access, logs, and more.</span></span> <span data-ttu-id="ba44a-107">v tomto kurzu se dokončil Hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="ba44a-107">hello following tasks are completed in this tutorial:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ba44a-108">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="ba44a-108">Create an Azure storage account</span></span>
> * <span data-ttu-id="ba44a-109">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="ba44a-109">Create a file share</span></span>
> * <span data-ttu-id="ba44a-110">Připojit hello sdílenou složku v clusteru DC/OS hello</span><span class="sxs-lookup"><span data-stu-id="ba44a-110">Mount hello share in hello DC/OS cluster</span></span>

<span data-ttu-id="ba44a-111">Budete potřebovat DC/OS ACS clusteru toocomplete hello kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ba44a-111">You need an ACS DC/OS cluster toocomplete hello steps in this tutorial.</span></span> <span data-ttu-id="ba44a-112">V případě potřeby [tento ukázkový skript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="ba44a-112">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="ba44a-113">Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ba44a-113">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="ba44a-114">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="ba44a-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ba44a-115">Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ba44a-115">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-file-share-on-microsoft-azure"></a><span data-ttu-id="ba44a-116">Vytvoření sdílené složky v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="ba44a-116">Create a file share on Microsoft Azure</span></span>

<span data-ttu-id="ba44a-117">Než sdílenou složku Azure pomocí clusteru služby ACS DC/OS, musí být vytvořeny hello úložiště účet a sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="ba44a-117">Before using an Azure file share with an ACS DC/OS cluster, hello storage account and file share must be created.</span></span> <span data-ttu-id="ba44a-118">Spusťte následující skript toocreate hello úložiště a sdílení souborů hello.</span><span class="sxs-lookup"><span data-stu-id="ba44a-118">Run hello following script toocreate hello storage and file share.</span></span> <span data-ttu-id="ba44a-119">Aktualizujte hello parametry s thoes ze svého prostředí.</span><span class="sxs-lookup"><span data-stu-id="ba44a-119">Update hello parameters with thoes from your environment.</span></span>

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

## <a name="mount-hello-share-in-your-cluster"></a><span data-ttu-id="ba44a-120">Připojit hello sdílenou složku v clusteru</span><span class="sxs-lookup"><span data-stu-id="ba44a-120">Mount hello share in your cluster</span></span>

<span data-ttu-id="ba44a-121">V dalším kroku hello sdílená vyžaduje toobe připojit na každý virtuální počítač v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="ba44a-121">Next, hello file share needs toobe mounted on every virtual machine inside your cluster.</span></span> <span data-ttu-id="ba44a-122">Tato úloha je dokončit pomocí hello cifs nástroj nebo protokolu.</span><span class="sxs-lookup"><span data-stu-id="ba44a-122">This task is completed using hello cifs tool/protocol.</span></span> <span data-ttu-id="ba44a-123">operace připojení Hello můžete dokončit ručně na každém uzlu clusteru hello nebo spuštěním skriptu na každém uzlu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ba44a-123">hello mount operation can be completed manually on each node of hello cluster, or by running a script against each node in hello cluster.</span></span>

<span data-ttu-id="ba44a-124">V tomto příkladu jsou dva skripty spustit, jeden toomount hello Azure souboru sdílené složky a druhý toorun tento skript na každém uzlu clusteru DC/OS hello.</span><span class="sxs-lookup"><span data-stu-id="ba44a-124">In this example, two scripts are run, one toomount hello Azure file share, and a second toorun this script on each node of hello DC/OS cluster.</span></span>

<span data-ttu-id="ba44a-125">Nejdřív je potřeba hello název účtu úložiště Azure a přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="ba44a-125">First, hello Azure storage account name, and access key are needed.</span></span> <span data-ttu-id="ba44a-126">Spusťte následující příkazy tooget hello tyto informace.</span><span class="sxs-lookup"><span data-stu-id="ba44a-126">Run hello following commands tooget this information.</span></span> <span data-ttu-id="ba44a-127">Všimněte si, se používají tyto hodnoty později.</span><span class="sxs-lookup"><span data-stu-id="ba44a-127">Take note of each, these values are used in a later step.</span></span>

<span data-ttu-id="ba44a-128">Název účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="ba44a-128">Storage account name:</span></span>

```azurecli-interactive
STORAGE_ACCT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCT
```

<span data-ttu-id="ba44a-129">Přístupový klíč účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="ba44a-129">Storage account access key:</span></span>

```azurecli-interactive
az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCT --query "[0].value" -o tsv
```

<span data-ttu-id="ba44a-130">V dalším kroku získáte hello plně kvalifikovaný název domény hello DC/OS hlavní a uložte ho do proměnné.</span><span class="sxs-lookup"><span data-stu-id="ba44a-130">Next, get hello FQDN of hello DC/OS master and store it in a variable.</span></span>

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

<span data-ttu-id="ba44a-131">Zkopírujte vaší privátní klíče toohello hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="ba44a-131">Copy your private key toohello master node.</span></span> <span data-ttu-id="ba44a-132">Tento klíč je potřebné toocreate ssh připojení se všechny uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ba44a-132">This key is needed toocreate an ssh connection with all nodes in hello cluster.</span></span> <span data-ttu-id="ba44a-133">Aktualizujte hello uživatelské jméno, pokud byla použita jiná než výchozí hodnotu, při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ba44a-133">Update hello user name if a non-default value was used when creating hello cluster.</span></span> 

```azurecli-interactive
scp ~/.ssh/id_rsa azureuser@$FQDN:~/.ssh
```

<span data-ttu-id="ba44a-134">Vytvoření připojení SSH s hello hlavního serveru (nebo hello první) clusteru založenými na DC/OS.</span><span class="sxs-lookup"><span data-stu-id="ba44a-134">Create an SSH connection with hello master (or hello first master) of your DC/OS-based cluster.</span></span> <span data-ttu-id="ba44a-135">Aktualizujte hello uživatelské jméno, pokud byla použita jiná než výchozí hodnotu, při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ba44a-135">Update hello user name if a non-default value was used when creating hello cluster.</span></span>

```azurecli-interactive
ssh azureuser@$FQDN
```

<span data-ttu-id="ba44a-136">Vytvořte soubor s názvem **cifsMount.sh**, a kopírování hello do něj následující obsah.</span><span class="sxs-lookup"><span data-stu-id="ba44a-136">Create a file named **cifsMount.sh**, and copy hello following contents into it.</span></span> 

<span data-ttu-id="ba44a-137">Tento skript je použité toomount hello sdílenou složku Azure.</span><span class="sxs-lookup"><span data-stu-id="ba44a-137">This script is used toomount hello Azure file share.</span></span> <span data-ttu-id="ba44a-138">Aktualizace hello `STORAGE_ACCT_NAME` a `ACCESS_KEY` proměnné s hello informace shromážděné dříve.</span><span class="sxs-lookup"><span data-stu-id="ba44a-138">Update hello `STORAGE_ACCT_NAME` and `ACCESS_KEY` variables with hello information collected earlier.</span></span>

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
<span data-ttu-id="ba44a-139">Vytvořte druhý soubor s názvem **getNodesRunScript.sh** a kopírování hello následující obsah do souboru hello.</span><span class="sxs-lookup"><span data-stu-id="ba44a-139">Create a second file named **getNodesRunScript.sh** and copy hello following contents into hello file.</span></span> 

<span data-ttu-id="ba44a-140">Tento skript zjistí všechny uzly clusteru a poté spustí hello **cifsMount.sh** skriptu toomount hello sdílené složce na každém.</span><span class="sxs-lookup"><span data-stu-id="ba44a-140">This script discovers all cluster nodes, and then runs hello **cifsMount.sh** script toomount hello file share on each.</span></span>

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

<span data-ttu-id="ba44a-141">Spouštět hello skriptu toomount hello sdílenou složku Azure na všech uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ba44a-141">Run hello script toomount hello Azure file share on all nodes of hello cluster.</span></span>

```azurecli-interactive
sh ./getNodesRunScript.sh
```  

<span data-ttu-id="ba44a-142">Hello sdílení souborů je nyní přístupná na `/mnt/share/dcosshare` na každém uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ba44a-142">hello file share is now accessible at `/mnt/share/dcosshare` on each node of hello cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba44a-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba44a-143">Next steps</span></span>

<span data-ttu-id="ba44a-144">V tomto kurzu Azure sdílenou složku byla učiněna k dispozici tooa clusteru DC/OS pomocí hello kroky:</span><span class="sxs-lookup"><span data-stu-id="ba44a-144">In this tutorial an Azure file share was made available tooa DC/OS cluster using hello steps:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ba44a-145">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="ba44a-145">Create an Azure storage account</span></span>
> * <span data-ttu-id="ba44a-146">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="ba44a-146">Create a file share</span></span>
> * <span data-ttu-id="ba44a-147">Připojit hello sdílenou složku v clusteru DC/OS hello</span><span class="sxs-lookup"><span data-stu-id="ba44a-147">Mount hello share in hello DC/OS cluster</span></span>

<span data-ttu-id="ba44a-148">Posunutí další kurz toolearn toohello o integraci služby Azure kontejneru registru s DC/OS v Azure.</span><span class="sxs-lookup"><span data-stu-id="ba44a-148">Advance toohello next tutorial toolearn about integrating an Azure Container Registry with DC/OS in Azure.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="ba44a-149">Vyrovnávání zatížení aplikací</span><span class="sxs-lookup"><span data-stu-id="ba44a-149">Load balance applications</span></span>](container-service-dcos-acr.md)