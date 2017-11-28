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
# <a name="deploy-a-dcos-cluster"></a><span data-ttu-id="92b56-104">Nasadit cluster DC/OS</span><span class="sxs-lookup"><span data-stu-id="92b56-104">Deploy a DC/OS cluster</span></span>

<span data-ttu-id="92b56-105">DC/OS poskytuje distribuované platformu pro spuštění moderní a kontejnerizované aplikace.</span><span class="sxs-lookup"><span data-stu-id="92b56-105">DC/OS provides a distributed platform for running modern and containerized applications.</span></span> <span data-ttu-id="92b56-106">S Azure Container Service zřizování provozní připravený cluster DC/OS je jednoduchý a rychlé.</span><span class="sxs-lookup"><span data-stu-id="92b56-106">With Azure Container Service, provisioning of a production ready DC/OS cluster is simple and quick.</span></span> <span data-ttu-id="92b56-107">Tento základní postup úvodní podrobnosti hello potřeba toodeploy clusteru DC/OS a spuštění základní úlohy.</span><span class="sxs-lookup"><span data-stu-id="92b56-107">This quick start details hello basic steps needed toodeploy a DC/OS cluster and run basic workload.</span></span>

<span data-ttu-id="92b56-108">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="92b56-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="92b56-109">Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="92b56-109">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="92b56-110">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="92b56-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="92b56-111">Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="92b56-111">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="log-in-tooazure"></a><span data-ttu-id="92b56-112">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="92b56-112">Log in tooAzure</span></span> 

<span data-ttu-id="92b56-113">Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="92b56-113">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli
az login
```

## <a name="create-a-resource-group"></a><span data-ttu-id="92b56-114">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="92b56-114">Create a resource group</span></span>

<span data-ttu-id="92b56-115">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="92b56-115">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="92b56-116">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="92b56-116">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="92b56-117">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="92b56-117">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a><span data-ttu-id="92b56-118">Vytvoření clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="92b56-118">Create DC/OS cluster</span></span>

<span data-ttu-id="92b56-119">Vytvořte cluster DC/OS s hello [vytvořit acs az](/cli/azure/acs#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="92b56-119">Create a DC/OS cluster with hello [az acs create](/cli/azure/acs#create) command.</span></span>

<span data-ttu-id="92b56-120">Hello následující příklad vytvoří cluster DC/OS s názvem *myDCOSCluster* a vytvoří klíče SSH, pokud dosud neexistují.</span><span class="sxs-lookup"><span data-stu-id="92b56-120">hello following example creates a DC/OS cluster named *myDCOSCluster* and creates SSH keys if they do not already exist.</span></span> <span data-ttu-id="92b56-121">toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.</span><span class="sxs-lookup"><span data-stu-id="92b56-121">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

<span data-ttu-id="92b56-122">Po několika minutách hello příkaz dokončí a vrátí informace o nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="92b56-122">After several minutes, hello command completes, and returns information about hello deployment.</span></span>

## <a name="connect-toodcos-cluster"></a><span data-ttu-id="92b56-123">Připojení clusteru tooDC/OS</span><span class="sxs-lookup"><span data-stu-id="92b56-123">Connect tooDC/OS cluster</span></span>

<span data-ttu-id="92b56-124">Po vytvoření clusteru DC/OS, může být přistupuje prostřednictvím tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="92b56-124">Once a DC/OS cluster has been created, it can be accesses through an SSH tunnel.</span></span> <span data-ttu-id="92b56-125">Spusťte následující příkaz tooreturn hello veřejnou IP adresu hlavního serveru DC/OS hello hello.</span><span class="sxs-lookup"><span data-stu-id="92b56-125">Run hello following command tooreturn hello public IP address of hello DC/OS master.</span></span> <span data-ttu-id="92b56-126">Tato IP adresa je uložené v proměnné a používat v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="92b56-126">This IP address is stored in a variable and used in hello next step.</span></span>

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

<span data-ttu-id="92b56-127">toocreate hello tunelového propojení SSH, spusťte následující příkaz hello a podle pokynů hello na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="92b56-127">toocreate hello SSH tunnel, run hello following command and follow hello on-screen instructions.</span></span> <span data-ttu-id="92b56-128">Pokud port 80 je již používán, hello příkaz se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="92b56-128">If port 80 is already in use, hello command fails.</span></span> <span data-ttu-id="92b56-129">Aktualizace hello tunelovým propojením tooone port není používán, jako například `85:localhost:80`.</span><span class="sxs-lookup"><span data-stu-id="92b56-129">Update hello tunneled port tooone not in use, such as `85:localhost:80`.</span></span> 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

<span data-ttu-id="92b56-130">tunelové propojení SSH Hello může být testována procházením příliš`http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="92b56-130">hello SSH tunnel can be tested by browsing too`http://localhost`.</span></span> <span data-ttu-id="92b56-131">Pokud port dalších že byl použit 80, upravte toomatch umístění hello.</span><span class="sxs-lookup"><span data-stu-id="92b56-131">If a port other that 80 has been used, adjust hello location toomatch.</span></span> 

<span data-ttu-id="92b56-132">Pokud tunelu SSH hello byla úspěšně vytvořena, bude vrácena hello DC/OS portálu.</span><span class="sxs-lookup"><span data-stu-id="92b56-132">If hello SSH tunnel was successfully created, hello DC/OS portal is returned.</span></span>

![ORCHESTRÁTORU DC/OS UŽIVATELSKÉHO ROZHRANÍ](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a><span data-ttu-id="92b56-134">Instalace rozhraní příkazového řádku DC/OS</span><span class="sxs-lookup"><span data-stu-id="92b56-134">Install DC/OS CLI</span></span>

<span data-ttu-id="92b56-135">rozhraní příkazového řádku Hello DC/OS je použité toomanage cluster DC/OS z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="92b56-135">hello DC/OS command line interface is used toomanage a DC/OS cluster from hello command-line.</span></span> <span data-ttu-id="92b56-136">Instalace rozhraní příkazového řádku DC/OS hello pomocí hello [az acs orchestrátoru DC/OS instalace rozhraní příkazového řádku](/azure/acs/dcos#install-cli) příkaz.</span><span class="sxs-lookup"><span data-stu-id="92b56-136">Install hello DC/OS cli using hello [az acs dcos install-cli](/azure/acs/dcos#install-cli) command.</span></span> <span data-ttu-id="92b56-137">Pokud používáte Azure CloudShell, hello rozhraní příkazového řádku DC/OS je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="92b56-137">If you are using Azure CloudShell, hello DC/OS CLI is already installed.</span></span> 

<span data-ttu-id="92b56-138">Pokud používáte hello rozhraní příkazového řádku Azure v systému macOS nebo Linux, bude pravděpodobně nutné příkaz hello toorun s sudo.</span><span class="sxs-lookup"><span data-stu-id="92b56-138">If you are running hello Azure CLI on macOS or Linux, you might need toorun hello command with sudo.</span></span>

```azurecli
az acs dcos install-cli
```

<span data-ttu-id="92b56-139">Před hello rozhraní příkazového řádku lze použít s hello clusteru musí být nakonfigurované toouse tunelové propojení SSH hello.</span><span class="sxs-lookup"><span data-stu-id="92b56-139">Before hello CLI can be used with hello cluster, it must be configured toouse hello SSH tunnel.</span></span> <span data-ttu-id="92b56-140">toodo tak, spusťte následující příkaz, úpravě hello portu v případě potřeby hello.</span><span class="sxs-lookup"><span data-stu-id="92b56-140">toodo so, run hello following command, adjusting hello port if needed.</span></span>

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a><span data-ttu-id="92b56-141">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="92b56-141">Run an application</span></span>

<span data-ttu-id="92b56-142">Výchozí hodnota Hello plánování mechanismus pro cluster služby ACS DC/OS je Marathon.</span><span class="sxs-lookup"><span data-stu-id="92b56-142">hello default scheduling mechanism for an ACS DC/OS cluster is Marathon.</span></span> <span data-ttu-id="92b56-143">Marathonu je použité toostart aplikace a spravovat stav hello hello aplikace v clusteru DC/OS hello.</span><span class="sxs-lookup"><span data-stu-id="92b56-143">Marathon is used toostart an application and manage hello state of hello application on hello DC/OS cluster.</span></span> <span data-ttu-id="92b56-144">tooschedule aplikaci přes Marathon, vytvořte soubor s názvem *marathon app.json*, a kopírování hello do něj následující obsah.</span><span class="sxs-lookup"><span data-stu-id="92b56-144">tooschedule an application through Marathon, create a file named *marathon-app.json*, and copy hello following contents into it.</span></span> 

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

<span data-ttu-id="92b56-145">Spusťte následující příkaz tooschedule hello aplikace toorun na clusteru DC/OS hello hello.</span><span class="sxs-lookup"><span data-stu-id="92b56-145">Run hello following command tooschedule hello application toorun on hello DC/OS cluster.</span></span>

```azurecli
dcos marathon app add marathon-app.json
```

<span data-ttu-id="92b56-146">toosee hello stav nasazení pro aplikaci hello, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="92b56-146">toosee hello deployment status for hello app, run hello following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="92b56-147">Když hello **čekání** hodnota sloupce přepíná z *True* příliš*False*, nasazení aplikace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="92b56-147">When hello **WAITING** column value switches from *True* too*False*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

<span data-ttu-id="92b56-148">Získáte hello veřejnou IP adresu hello agentů clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="92b56-148">Get hello public IP address of hello DC/OS cluster agents.</span></span>

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

<span data-ttu-id="92b56-149">Procházení toothis adresu vrátí hello výchozí NGINX lokality.</span><span class="sxs-lookup"><span data-stu-id="92b56-149">Browsing toothis address returns hello default NGINX site.</span></span>

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a><span data-ttu-id="92b56-151">Odstranění clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="92b56-151">Delete DC/OS cluster</span></span>

<span data-ttu-id="92b56-152">Pokud již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove clusteru DC/OS a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="92b56-152">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, DC/OS cluster, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a><span data-ttu-id="92b56-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="92b56-153">Next steps</span></span>

<span data-ttu-id="92b56-154">V této úvodní jste nasadili cluster DC/OS a spustili jednoduché kontejner Docker v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="92b56-154">In this quick start, you’ve deployed a DC/OS cluster and have run a simple Docker container on hello cluster.</span></span> <span data-ttu-id="92b56-155">toolearn Další informace o Azure Container Service pokračovat kurzy toohello služby ACS.</span><span class="sxs-lookup"><span data-stu-id="92b56-155">toolearn more about Azure Container Service, continue toohello ACS tutorials.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="92b56-156">Správa clusteru služby ACS DC/OS</span><span class="sxs-lookup"><span data-stu-id="92b56-156">Manage an ACS DC/OS Cluster</span></span>](container-service-dcos-manage-tutorial.md)