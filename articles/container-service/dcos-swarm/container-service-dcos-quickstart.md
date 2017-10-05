---
title: "Kontejner Azure rychlý start služba – nasazení clusteru DC/OS | Microsoft Docs"
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
ms.openlocfilehash: a31170369de9bc1ddcddb97171281b0014af95f9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-dcos-cluster"></a><span data-ttu-id="d1355-104">Nasadit cluster DC/OS</span><span class="sxs-lookup"><span data-stu-id="d1355-104">Deploy a DC/OS cluster</span></span>

<span data-ttu-id="d1355-105">DC/OS poskytuje distribuované platformu pro spuštění moderní a kontejnerizované aplikace.</span><span class="sxs-lookup"><span data-stu-id="d1355-105">DC/OS provides a distributed platform for running modern and containerized applications.</span></span> <span data-ttu-id="d1355-106">S Azure Container Service zřizování provozní připravený cluster DC/OS je jednoduchý a rychlé.</span><span class="sxs-lookup"><span data-stu-id="d1355-106">With Azure Container Service, provisioning of a production ready DC/OS cluster is simple and quick.</span></span> <span data-ttu-id="d1355-107">Podrobnosti o této úvodní základní kroky potřebné k nasazení DC/OS clusteru a spuštění základní úlohy.</span><span class="sxs-lookup"><span data-stu-id="d1355-107">This quick start details the basic steps needed to deploy a DC/OS cluster and run basic workload.</span></span>

<span data-ttu-id="d1355-108">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="d1355-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="d1355-109">Tento kurz vyžaduje Azure CLI verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d1355-109">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="d1355-110">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="d1355-110">Run `az --version` to find the version.</span></span> <span data-ttu-id="d1355-111">Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d1355-111">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="log-in-to-azure"></a><span data-ttu-id="d1355-112">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="d1355-112">Log in to Azure</span></span> 

<span data-ttu-id="d1355-113">Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="d1355-113">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span>

```azurecli
az login
```

## <a name="create-a-resource-group"></a><span data-ttu-id="d1355-114">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="d1355-114">Create a resource group</span></span>

<span data-ttu-id="d1355-115">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="d1355-115">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="d1355-116">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="d1355-116">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="d1355-117">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.</span><span class="sxs-lookup"><span data-stu-id="d1355-117">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a><span data-ttu-id="d1355-118">Vytvoření clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="d1355-118">Create DC/OS cluster</span></span>

<span data-ttu-id="d1355-119">Vytvořit cluster DC/OS s [vytvořit acs az](/cli/azure/acs#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="d1355-119">Create a DC/OS cluster with the [az acs create](/cli/azure/acs#create) command.</span></span>

<span data-ttu-id="d1355-120">Následující příklad vytvoří cluster DC/OS s názvem *myDCOSCluster* a vytvoří klíče SSH, pokud dosud neexistují.</span><span class="sxs-lookup"><span data-stu-id="d1355-120">The following example creates a DC/OS cluster named *myDCOSCluster* and creates SSH keys if they do not already exist.</span></span> <span data-ttu-id="d1355-121">Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="d1355-121">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

<span data-ttu-id="d1355-122">Po několika minutách se příkaz dokončí a vrátí informace o nasazení.</span><span class="sxs-lookup"><span data-stu-id="d1355-122">After several minutes, the command completes, and returns information about the deployment.</span></span>

## <a name="connect-to-dcos-cluster"></a><span data-ttu-id="d1355-123">Připojení ke clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="d1355-123">Connect to DC/OS cluster</span></span>

<span data-ttu-id="d1355-124">Po vytvoření clusteru DC/OS, může být přistupuje prostřednictvím tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="d1355-124">Once a DC/OS cluster has been created, it can be accesses through an SSH tunnel.</span></span> <span data-ttu-id="d1355-125">Spusťte následující příkaz, který vrátí veřejnou IP adresu hlavního serveru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="d1355-125">Run the following command to return the public IP address of the DC/OS master.</span></span> <span data-ttu-id="d1355-126">Tato IP adresa je uložené v proměnné a používat v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="d1355-126">This IP address is stored in a variable and used in the next step.</span></span>

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

<span data-ttu-id="d1355-127">Vytvoření tunelu SSH, spusťte následující příkaz a postupujte podle na obrazovce pokyny.</span><span class="sxs-lookup"><span data-stu-id="d1355-127">To create the SSH tunnel, run the following command and follow the on-screen instructions.</span></span> <span data-ttu-id="d1355-128">Pokud port 80 je již používán, příkaz se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="d1355-128">If port 80 is already in use, the command fails.</span></span> <span data-ttu-id="d1355-129">Aktualizace tunelových port na jedno není ve použití, například `85:localhost:80`.</span><span class="sxs-lookup"><span data-stu-id="d1355-129">Update the tunneled port to one not in use, such as `85:localhost:80`.</span></span> 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

<span data-ttu-id="d1355-130">Tunelové propojení SSH může být testována procházením `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="d1355-130">The SSH tunnel can be tested by browsing to `http://localhost`.</span></span> <span data-ttu-id="d1355-131">Pokud port dalších že byl použit 80, nastavit umístění tak, aby odpovídaly.</span><span class="sxs-lookup"><span data-stu-id="d1355-131">If a port other that 80 has been used, adjust the location to match.</span></span> 

<span data-ttu-id="d1355-132">Pokud byl úspěšně vytvořen tunel SSH, je vrácena portálu DC/OS.</span><span class="sxs-lookup"><span data-stu-id="d1355-132">If the SSH tunnel was successfully created, the DC/OS portal is returned.</span></span>

![ORCHESTRÁTORU DC/OS UŽIVATELSKÉHO ROZHRANÍ](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a><span data-ttu-id="d1355-134">Instalace rozhraní příkazového řádku DC/OS</span><span class="sxs-lookup"><span data-stu-id="d1355-134">Install DC/OS CLI</span></span>

<span data-ttu-id="d1355-135">Rozhraní příkazového řádku DC/OS se používá ke správě clusteru DC/OS z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d1355-135">The DC/OS command line interface is used to manage a DC/OS cluster from the command-line.</span></span> <span data-ttu-id="d1355-136">Instalace pomocí rozhraní příkazového řádku DC/OS [az acs orchestrátoru DC/OS instalace rozhraní příkazového řádku](/azure/acs/dcos#install-cli) příkaz.</span><span class="sxs-lookup"><span data-stu-id="d1355-136">Install the DC/OS cli using the [az acs dcos install-cli](/azure/acs/dcos#install-cli) command.</span></span> <span data-ttu-id="d1355-137">Pokud používáte Azure CloudShell, rozhraní příkazového řádku DC/OS je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="d1355-137">If you are using Azure CloudShell, the DC/OS CLI is already installed.</span></span> 

<span data-ttu-id="d1355-138">Pokud používáte Azure CLI v systému macOS nebo Linux, může být potřeba spustit příkaz s sudo.</span><span class="sxs-lookup"><span data-stu-id="d1355-138">If you are running the Azure CLI on macOS or Linux, you might need to run the command with sudo.</span></span>

```azurecli
az acs dcos install-cli
```

<span data-ttu-id="d1355-139">Před použitím rozhraní příkazového řádku s clusterem, je nutné nakonfigurovat na použití tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="d1355-139">Before the CLI can be used with the cluster, it must be configured to use the SSH tunnel.</span></span> <span data-ttu-id="d1355-140">Uděláte to tak, spusťte následující příkaz, nastavení portu, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="d1355-140">To do so, run the following command, adjusting the port if needed.</span></span>

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a><span data-ttu-id="d1355-141">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="d1355-141">Run an application</span></span>

<span data-ttu-id="d1355-142">Výchozí hodnota plánování mechanismus pro cluster služby ACS DC/OS je Marathon.</span><span class="sxs-lookup"><span data-stu-id="d1355-142">The default scheduling mechanism for an ACS DC/OS cluster is Marathon.</span></span> <span data-ttu-id="d1355-143">Marathon slouží ke spuštění aplikace a správu stavu aplikace na clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="d1355-143">Marathon is used to start an application and manage the state of the application on the DC/OS cluster.</span></span> <span data-ttu-id="d1355-144">Při plánování aplikace přes Marathon, vytvořte soubor s názvem *marathon app.json*a zkopírujte do něj následující obsah.</span><span class="sxs-lookup"><span data-stu-id="d1355-144">To schedule an application through Marathon, create a file named *marathon-app.json*, and copy the following contents into it.</span></span> 

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

<span data-ttu-id="d1355-145">Spusťte následující příkaz, který naplánovat spuštění aplikace v clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="d1355-145">Run the following command to schedule the application to run on the DC/OS cluster.</span></span>

```azurecli
dcos marathon app add marathon-app.json
```

<span data-ttu-id="d1355-146">Pokud chcete zobrazit stav nasazení pro aplikaci, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="d1355-146">To see the deployment status for the app, run the following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="d1355-147">Když **čekání na** hodnota sloupce přepíná z *True* k *False*, nasazení aplikace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="d1355-147">When the **WAITING** column value switches from *True* to *False*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

<span data-ttu-id="d1355-148">Získáte veřejnou IP adresu clusteru agentů DC/OS.</span><span class="sxs-lookup"><span data-stu-id="d1355-148">Get the public IP address of the DC/OS cluster agents.</span></span>

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

<span data-ttu-id="d1355-149">Procházení na tuto adresu vrátí výchozí web NGINX.</span><span class="sxs-lookup"><span data-stu-id="d1355-149">Browsing to this address returns the default NGINX site.</span></span>

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a><span data-ttu-id="d1355-151">Odstranění clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="d1355-151">Delete DC/OS cluster</span></span>

<span data-ttu-id="d1355-152">Pokud již nepotřebujete, můžete použít [odstranění skupiny az](/cli/azure/group#delete) příkaz, který má-li odebrat skupinu prostředků, clusteru DC/OS a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="d1355-152">When no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, DC/OS cluster, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a><span data-ttu-id="d1355-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d1355-153">Next steps</span></span>

<span data-ttu-id="d1355-154">V této úvodní jste nasadili cluster DC/OS a spustili jednoduché kontejner Docker v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d1355-154">In this quick start, you’ve deployed a DC/OS cluster and have run a simple Docker container on the cluster.</span></span> <span data-ttu-id="d1355-155">Další informace o Azure Container Service, i nadále kurzy služby ACS.</span><span class="sxs-lookup"><span data-stu-id="d1355-155">To learn more about Azure Container Service, continue to the ACS tutorials.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d1355-156">Správa clusteru služby ACS DC/OS</span><span class="sxs-lookup"><span data-stu-id="d1355-156">Manage an ACS DC/OS Cluster</span></span>](container-service-dcos-manage-tutorial.md)