---
title: "Kurz pro Azure Container Service – spravovat DC/OS | Microsoft Docs"
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
ms.openlocfilehash: e93f782c26c32f97749e817ec59ee3c2ecb7e119
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-container-service-tutorial---manage-dcos"></a><span data-ttu-id="4434d-104">Kurz pro Azure Container Service – spravovat DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-104">Azure Container Service tutorial - Manage DC/OS</span></span>

<span data-ttu-id="4434d-105">DC/OS poskytuje distribuované platformu pro spuštění moderní a kontejnerizované aplikace.</span><span class="sxs-lookup"><span data-stu-id="4434d-105">DC/OS provides a distributed platform for running modern and containerized applications.</span></span> <span data-ttu-id="4434d-106">S Azure Container Service zřizování provozní připravený cluster DC/OS je jednoduchý a rychlé.</span><span class="sxs-lookup"><span data-stu-id="4434d-106">With Azure Container Service, provisioning of a production ready DC/OS cluster is simple and quick.</span></span> <span data-ttu-id="4434d-107">Tento úvodní informace, které základní kroky potřebné k nasazení clusteru DC/OS a spuštění základní úlohy.</span><span class="sxs-lookup"><span data-stu-id="4434d-107">This quick start details basic steps needed to deploy a DC/OS cluster and run basic workload.</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4434d-108">Vytvoření clusteru služby ACS DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-108">Create an ACS DC/OS cluster</span></span>
> * <span data-ttu-id="4434d-109">Připojení ke clusteru</span><span class="sxs-lookup"><span data-stu-id="4434d-109">Connect to the cluster</span></span>
> * <span data-ttu-id="4434d-110">Instalace rozhraní příkazového řádku DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-110">Install the DC/OS CLI</span></span>
> * <span data-ttu-id="4434d-111">Nasazení aplikace do clusteru</span><span class="sxs-lookup"><span data-stu-id="4434d-111">Deploy an application to the cluster</span></span>
> * <span data-ttu-id="4434d-112">Škálování aplikace v clusteru</span><span class="sxs-lookup"><span data-stu-id="4434d-112">Scale an application on the cluster</span></span>
> * <span data-ttu-id="4434d-113">Škálování uzly clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-113">Scale the DC/OS cluster nodes</span></span>
> * <span data-ttu-id="4434d-114">Základní správu DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-114">Basic DC/OS management</span></span>
> * <span data-ttu-id="4434d-115">Odstranění clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-115">Delete the DC/OS cluster</span></span>

<span data-ttu-id="4434d-116">Tento kurz vyžaduje Azure CLI verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="4434d-116">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="4434d-117">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="4434d-117">Run `az --version` to find the version.</span></span> <span data-ttu-id="4434d-118">Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4434d-118">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-dcos-cluster"></a><span data-ttu-id="4434d-119">Vytvoření clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-119">Create DC/OS cluster</span></span>

<span data-ttu-id="4434d-120">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="4434d-120">First, create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="4434d-121">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="4434d-121">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="4434d-122">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *westeurope*.</span><span class="sxs-lookup"><span data-stu-id="4434d-122">The following example creates a resource group named *myResourceGroup* in the *westeurope* location.</span></span>

```azurecli
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="4434d-123">Dále vytvořte cluster DC/OS s [vytvořit acs az](/cli/azure/acs#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="4434d-123">Next, create a DC/OS cluster with the [az acs create](/cli/azure/acs#create) command.</span></span>

<span data-ttu-id="4434d-124">Následující příklad vytvoří cluster DC/OS s názvem *myDCOSCluster* a vytvoří klíče SSH, pokud dosud neexistují.</span><span class="sxs-lookup"><span data-stu-id="4434d-124">The following example creates a DC/OS cluster named *myDCOSCluster* and creates SSH keys if they do not already exist.</span></span> <span data-ttu-id="4434d-125">Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="4434d-125">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

<span data-ttu-id="4434d-126">Po několika minutách se příkaz dokončí a vrátí informace o nasazení.</span><span class="sxs-lookup"><span data-stu-id="4434d-126">After several minutes, the command completes, and returns information about the deployment.</span></span>

## <a name="connect-to-dcos-cluster"></a><span data-ttu-id="4434d-127">Připojení ke clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-127">Connect to DC/OS cluster</span></span>

<span data-ttu-id="4434d-128">Po vytvoření clusteru DC/OS, může být přistupuje prostřednictvím tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="4434d-128">Once a DC/OS cluster has been created, it can be accesses through an SSH tunnel.</span></span> <span data-ttu-id="4434d-129">Spusťte následující příkaz, který vrátí veřejnou IP adresu hlavního serveru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="4434d-129">Run the following command to return the public IP address of the DC/OS master.</span></span> <span data-ttu-id="4434d-130">Tato IP adresa je uložené v proměnné a používat v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="4434d-130">This IP address is stored in a variable and used in the next step.</span></span>

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

<span data-ttu-id="4434d-131">Vytvoření tunelu SSH, spusťte následující příkaz a postupujte podle na obrazovce pokyny.</span><span class="sxs-lookup"><span data-stu-id="4434d-131">To create the SSH tunnel, run the following command and follow the on-screen instructions.</span></span> <span data-ttu-id="4434d-132">Pokud port 80 je již používán, příkaz se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4434d-132">If port 80 is already in use, the command fails.</span></span> <span data-ttu-id="4434d-133">Aktualizace tunelových port na jedno není ve použití, například `85:localhost:80`.</span><span class="sxs-lookup"><span data-stu-id="4434d-133">Update the tunneled port to one not in use, such as `85:localhost:80`.</span></span> 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a><span data-ttu-id="4434d-134">Instalace rozhraní příkazového řádku DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-134">Install DC/OS CLI</span></span>

<span data-ttu-id="4434d-135">Instalace pomocí rozhraní příkazového řádku DC/OS [az acs orchestrátoru DC/OS instalace rozhraní příkazového řádku](/azure/acs/dcos#install-cli) příkaz.</span><span class="sxs-lookup"><span data-stu-id="4434d-135">Install the DC/OS cli using the [az acs dcos install-cli](/azure/acs/dcos#install-cli) command.</span></span> <span data-ttu-id="4434d-136">Pokud používáte Azure CloudShell, rozhraní příkazového řádku DC/OS je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="4434d-136">If you are using Azure CloudShell, the DC/OS CLI is already installed.</span></span> <span data-ttu-id="4434d-137">Pokud používáte Azure CLI v systému macOS nebo Linux, může být potřeba spustit příkaz s sudo.</span><span class="sxs-lookup"><span data-stu-id="4434d-137">If you are running the Azure CLI on macOS or Linux, you might need to run the command with sudo.</span></span>

```azurecli
az acs dcos install-cli
```

<span data-ttu-id="4434d-138">Před použitím rozhraní příkazového řádku s clusterem, je nutné nakonfigurovat na použití tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="4434d-138">Before the CLI can be used with the cluster, it must be configured to use the SSH tunnel.</span></span> <span data-ttu-id="4434d-139">Uděláte to tak, spusťte následující příkaz, nastavení portu, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="4434d-139">To do so, run the following command, adjusting the port if needed.</span></span>

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a><span data-ttu-id="4434d-140">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="4434d-140">Run an application</span></span>

<span data-ttu-id="4434d-141">Výchozí hodnota plánování mechanismus pro cluster služby ACS DC/OS je Marathon.</span><span class="sxs-lookup"><span data-stu-id="4434d-141">The default scheduling mechanism for an ACS DC/OS cluster is Marathon.</span></span> <span data-ttu-id="4434d-142">Marathon slouží ke spuštění aplikace a správu stavu aplikace na clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="4434d-142">Marathon is used to start an application and manage the state of the application on the DC/OS cluster.</span></span> <span data-ttu-id="4434d-143">Při plánování aplikace přes Marathon, vytvořte soubor s názvem **marathon app.json**a zkopírujte do něj následující obsah.</span><span class="sxs-lookup"><span data-stu-id="4434d-143">To schedule an application through Marathon, create a file named **marathon-app.json**, and copy the following contents into it.</span></span> 

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

<span data-ttu-id="4434d-144">Spusťte následující příkaz, který naplánovat spuštění aplikace v clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="4434d-144">Run the following command to schedule the application to run on the DC/OS cluster.</span></span>

```azurecli
dcos marathon app add marathon-app.json
```

<span data-ttu-id="4434d-145">Pokud chcete zobrazit stav nasazení pro aplikaci, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="4434d-145">To see the deployment status for the app, run the following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="4434d-146">Když **úlohy** hodnota sloupce přepíná z *0 nebo 1* k *1 nebo 1*, nasazení aplikace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="4434d-146">When the **TASKS** column value switches from *0/1* to *1/1*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a><span data-ttu-id="4434d-147">Škálování aplikací Marathon</span><span class="sxs-lookup"><span data-stu-id="4434d-147">Scale Marathon application</span></span>

<span data-ttu-id="4434d-148">V předchozím příkladu byla vytvořena jedna instance aplikace.</span><span class="sxs-lookup"><span data-stu-id="4434d-148">In the previous example, a single instance application was created.</span></span> <span data-ttu-id="4434d-149">K aktualizaci tohoto nasazení tak, aby byly k dispozici tři instance aplikace, otevře **marathon app.json** souboru a aktualizujte vlastnost instance na 3.</span><span class="sxs-lookup"><span data-stu-id="4434d-149">To update this deployment so that three instances of the application are available, open up the **marathon-app.json** file, and update the instance property to 3.</span></span>

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

<span data-ttu-id="4434d-150">Aktualizace aplikace pomocí `dcos marathon app update` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4434d-150">Update the application using the `dcos marathon app update` command.</span></span>

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

<span data-ttu-id="4434d-151">Pokud chcete zobrazit stav nasazení pro aplikaci, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="4434d-151">To see the deployment status for the app, run the following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="4434d-152">Když **úlohy** hodnota sloupce přepíná z *1/3* k *3/1*, nasazení aplikace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="4434d-152">When the **TASKS** column value switches from *1/3* to *3/1*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a><span data-ttu-id="4434d-153">Spuštění aplikace dostupné pro internet</span><span class="sxs-lookup"><span data-stu-id="4434d-153">Run internet accessible app</span></span>

<span data-ttu-id="4434d-154">Clusteru ACS DC/OS se skládá z dvě sady uzlu, jeden veřejný, která je přístupná v síti internet a jeden privátní, která není přístupná v síti internet.</span><span class="sxs-lookup"><span data-stu-id="4434d-154">The ACS DC/OS cluster consists of two node sets, one public which is accessible on the internet, and one private which is not accessible on the internet.</span></span> <span data-ttu-id="4434d-155">Výchozí sada je privátní uzly, který byl použit v posledním příkladu.</span><span class="sxs-lookup"><span data-stu-id="4434d-155">The default set is the private nodes, which was used in the last example.</span></span>

<span data-ttu-id="4434d-156">Chcete-li aplikaci dostupný na Internetu, nasaďte do sady veřejné uzlu.</span><span class="sxs-lookup"><span data-stu-id="4434d-156">To make an application accessible on the internet, deploy them to the public node set.</span></span> <span data-ttu-id="4434d-157">K tomu, dát `acceptedResourceRoles` objektu hodnotu `slave_public`.</span><span class="sxs-lookup"><span data-stu-id="4434d-157">To do so, give the `acceptedResourceRoles` object a value of `slave_public`.</span></span>

<span data-ttu-id="4434d-158">Vytvořte soubor s názvem **nginx public.json** a zkopírujte do něj následující obsah.</span><span class="sxs-lookup"><span data-stu-id="4434d-158">Create a file named **nginx-public.json** and copy the following contents into it.</span></span>

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

<span data-ttu-id="4434d-159">Spusťte následující příkaz, který naplánovat spuštění aplikace v clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="4434d-159">Run the following command to schedule the application to run on the DC/OS cluster.</span></span>

```azurecli 
dcos marathon app add nginx-public.json
```

<span data-ttu-id="4434d-160">Získáte veřejnou IP adresu clusteru veřejných agentů DC/OS.</span><span class="sxs-lookup"><span data-stu-id="4434d-160">Get the public IP address of the DC/OS public cluster agents.</span></span>

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

<span data-ttu-id="4434d-161">Procházení na tuto adresu vrátí výchozí web NGINX.</span><span class="sxs-lookup"><span data-stu-id="4434d-161">Browsing to this address returns the default NGINX site.</span></span>

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a><span data-ttu-id="4434d-163">Škálování clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-163">Scale DC/OS cluster</span></span>

<span data-ttu-id="4434d-164">V předchozích příkladech byla aplikace škálovat na více instancí.</span><span class="sxs-lookup"><span data-stu-id="4434d-164">In the previous examples, an application was scaled to multiple instance.</span></span> <span data-ttu-id="4434d-165">DC/OS infrastruktury můžete škálovat také k zajištění vyšší nebo nižší výpočetní kapacity.</span><span class="sxs-lookup"><span data-stu-id="4434d-165">The DC/OS infrastructure can also be scaled to provide more or less compute capacity.</span></span> <span data-ttu-id="4434d-166">To lze provést pomocí [škálování služby acs az]() příkaz.</span><span class="sxs-lookup"><span data-stu-id="4434d-166">This is done with the [az acs scale]() command.</span></span> 

<span data-ttu-id="4434d-167">Pokud chcete zobrazit aktuální počet agentů DC/OS, použijte [zobrazit acs az](/cli/azure/acs#show) příkaz.</span><span class="sxs-lookup"><span data-stu-id="4434d-167">To see the current count of DC/OS agents, use the [az acs show](/cli/azure/acs#show) command.</span></span>

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

<span data-ttu-id="4434d-168">Chcete-li zvýšit počet 5, použijte [škálování služby acs az](/cli/azure/acs#scale) příkaz.</span><span class="sxs-lookup"><span data-stu-id="4434d-168">To increase the count to 5, use the [az acs scale](/cli/azure/acs#scale) command.</span></span> 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a><span data-ttu-id="4434d-169">Odstranění clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-169">Delete DC/OS cluster</span></span>

<span data-ttu-id="4434d-170">Pokud již nepotřebujete, můžete použít [odstranění skupiny az](/cli/azure/group#delete) příkaz, který má-li odebrat skupinu prostředků, clusteru DC/OS a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="4434d-170">When no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, DC/OS cluster, and all related resources.</span></span>

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a><span data-ttu-id="4434d-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4434d-171">Next steps</span></span>

<span data-ttu-id="4434d-172">V tomto kurzu jste se naučili o základní úlohy správy DC/OS, včetně následujících.</span><span class="sxs-lookup"><span data-stu-id="4434d-172">In this tutorial, you have learned about basic DC/OS management task including the following.</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="4434d-173">Vytvoření clusteru služby ACS DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-173">Create an ACS DC/OS cluster</span></span>
> * <span data-ttu-id="4434d-174">Připojení ke clusteru</span><span class="sxs-lookup"><span data-stu-id="4434d-174">Connect to the cluster</span></span>
> * <span data-ttu-id="4434d-175">Instalace rozhraní příkazového řádku DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-175">Install the DC/OS CLI</span></span>
> * <span data-ttu-id="4434d-176">Nasazení aplikace do clusteru</span><span class="sxs-lookup"><span data-stu-id="4434d-176">Deploy an application to the cluster</span></span>
> * <span data-ttu-id="4434d-177">Škálování aplikace v clusteru</span><span class="sxs-lookup"><span data-stu-id="4434d-177">Scale an application on the cluster</span></span>
> * <span data-ttu-id="4434d-178">Škálování uzly clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-178">Scale the DC/OS cluster nodes</span></span>
> * <span data-ttu-id="4434d-179">Odstranění clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="4434d-179">Delete the DC/OS cluster</span></span>

<span data-ttu-id="4434d-180">Přechodu na v dalším kurzu se dozvíte o aplikaci v DC/OS v Azure Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="4434d-180">Advance to the next tutorial to learn about load balancing application in DC/OS on Azure.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="4434d-181">Vyrovnávání zatížení aplikací</span><span class="sxs-lookup"><span data-stu-id="4434d-181">Load balance applications</span></span>](container-service-load-balancing.md)