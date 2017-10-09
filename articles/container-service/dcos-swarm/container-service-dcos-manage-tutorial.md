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
# <a name="azure-container-service-tutorial---manage-dcos"></a><span data-ttu-id="1fb88-104">Kurz pro Azure Container Service – spravovat DC/OS</span><span class="sxs-lookup"><span data-stu-id="1fb88-104">Azure Container Service tutorial - Manage DC/OS</span></span>

<span data-ttu-id="1fb88-105">DC/OS poskytuje distribuované platformu pro spuštění moderní a kontejnerizované aplikace.</span><span class="sxs-lookup"><span data-stu-id="1fb88-105">DC/OS provides a distributed platform for running modern and containerized applications.</span></span> <span data-ttu-id="1fb88-106">S Azure Container Service zřizování provozní připravený cluster DC/OS je jednoduchý a rychlé.</span><span class="sxs-lookup"><span data-stu-id="1fb88-106">With Azure Container Service, provisioning of a production ready DC/OS cluster is simple and quick.</span></span> <span data-ttu-id="1fb88-107">Tento úvodní podrobnosti základní kroky potřebné toodeploy clusteru DC/OS a spuštění základní úlohy.</span><span class="sxs-lookup"><span data-stu-id="1fb88-107">This quick start details basic steps needed toodeploy a DC/OS cluster and run basic workload.</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1fb88-108">Vytvoření clusteru služby ACS DC/OS</span><span class="sxs-lookup"><span data-stu-id="1fb88-108">Create an ACS DC/OS cluster</span></span>
> * <span data-ttu-id="1fb88-109">Připojte toohello cluster</span><span class="sxs-lookup"><span data-stu-id="1fb88-109">Connect toohello cluster</span></span>
> * <span data-ttu-id="1fb88-110">Nainstalujte hello rozhraní příkazového řádku DC/OS</span><span class="sxs-lookup"><span data-stu-id="1fb88-110">Install hello DC/OS CLI</span></span>
> * <span data-ttu-id="1fb88-111">Nasazení clusteru toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="1fb88-111">Deploy an application toohello cluster</span></span>
> * <span data-ttu-id="1fb88-112">Škálování aplikace na clusteru hello</span><span class="sxs-lookup"><span data-stu-id="1fb88-112">Scale an application on hello cluster</span></span>
> * <span data-ttu-id="1fb88-113">Škálování uzly clusteru DC/OS hello</span><span class="sxs-lookup"><span data-stu-id="1fb88-113">Scale hello DC/OS cluster nodes</span></span>
> * <span data-ttu-id="1fb88-114">Základní správu DC/OS</span><span class="sxs-lookup"><span data-stu-id="1fb88-114">Basic DC/OS management</span></span>
> * <span data-ttu-id="1fb88-115">Odstranění clusteru DC/OS hello</span><span class="sxs-lookup"><span data-stu-id="1fb88-115">Delete hello DC/OS cluster</span></span>

<span data-ttu-id="1fb88-116">Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1fb88-116">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="1fb88-117">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="1fb88-117">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="1fb88-118">Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1fb88-118">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-dcos-cluster"></a><span data-ttu-id="1fb88-119">Vytvoření clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="1fb88-119">Create DC/OS cluster</span></span>

<span data-ttu-id="1fb88-120">Nejprve vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1fb88-120">First, create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="1fb88-121">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="1fb88-121">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="1fb88-122">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westeurope* umístění.</span><span class="sxs-lookup"><span data-stu-id="1fb88-122">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="1fb88-123">Dále vytvořte cluster DC/OS s hello [vytvořit acs az](/cli/azure/acs#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1fb88-123">Next, create a DC/OS cluster with hello [az acs create](/cli/azure/acs#create) command.</span></span>

<span data-ttu-id="1fb88-124">Hello následující příklad vytvoří cluster DC/OS s názvem *myDCOSCluster* a vytvoří klíče SSH, pokud dosud neexistují.</span><span class="sxs-lookup"><span data-stu-id="1fb88-124">hello following example creates a DC/OS cluster named *myDCOSCluster* and creates SSH keys if they do not already exist.</span></span> <span data-ttu-id="1fb88-125">toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.</span><span class="sxs-lookup"><span data-stu-id="1fb88-125">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

<span data-ttu-id="1fb88-126">Po několika minutách hello příkaz dokončí a vrátí informace o nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-126">After several minutes, hello command completes, and returns information about hello deployment.</span></span>

## <a name="connect-toodcos-cluster"></a><span data-ttu-id="1fb88-127">Připojení clusteru tooDC/OS</span><span class="sxs-lookup"><span data-stu-id="1fb88-127">Connect tooDC/OS cluster</span></span>

<span data-ttu-id="1fb88-128">Po vytvoření clusteru DC/OS, může být přistupuje prostřednictvím tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="1fb88-128">Once a DC/OS cluster has been created, it can be accesses through an SSH tunnel.</span></span> <span data-ttu-id="1fb88-129">Spusťte následující příkaz tooreturn hello veřejnou IP adresu hlavního serveru DC/OS hello hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-129">Run hello following command tooreturn hello public IP address of hello DC/OS master.</span></span> <span data-ttu-id="1fb88-130">Tato IP adresa je uložené v proměnné a používat v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-130">This IP address is stored in a variable and used in hello next step.</span></span>

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

<span data-ttu-id="1fb88-131">toocreate hello tunelového propojení SSH, spusťte následující příkaz hello a podle pokynů hello na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="1fb88-131">toocreate hello SSH tunnel, run hello following command and follow hello on-screen instructions.</span></span> <span data-ttu-id="1fb88-132">Pokud port 80 je již používán, hello příkaz se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="1fb88-132">If port 80 is already in use, hello command fails.</span></span> <span data-ttu-id="1fb88-133">Aktualizace hello tunelovým propojením tooone port není používán, jako například `85:localhost:80`.</span><span class="sxs-lookup"><span data-stu-id="1fb88-133">Update hello tunneled port tooone not in use, such as `85:localhost:80`.</span></span> 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a><span data-ttu-id="1fb88-134">Instalace rozhraní příkazového řádku DC/OS</span><span class="sxs-lookup"><span data-stu-id="1fb88-134">Install DC/OS CLI</span></span>

<span data-ttu-id="1fb88-135">Instalace rozhraní příkazového řádku DC/OS hello pomocí hello [az acs orchestrátoru DC/OS instalace rozhraní příkazového řádku](/azure/acs/dcos#install-cli) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1fb88-135">Install hello DC/OS cli using hello [az acs dcos install-cli](/azure/acs/dcos#install-cli) command.</span></span> <span data-ttu-id="1fb88-136">Pokud používáte Azure CloudShell, hello rozhraní příkazového řádku DC/OS je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="1fb88-136">If you are using Azure CloudShell, hello DC/OS CLI is already installed.</span></span> <span data-ttu-id="1fb88-137">Pokud používáte hello rozhraní příkazového řádku Azure v systému macOS nebo Linux, bude pravděpodobně nutné příkaz hello toorun s sudo.</span><span class="sxs-lookup"><span data-stu-id="1fb88-137">If you are running hello Azure CLI on macOS or Linux, you might need toorun hello command with sudo.</span></span>

```azurecli
az acs dcos install-cli
```

<span data-ttu-id="1fb88-138">Před hello rozhraní příkazového řádku lze použít s hello clusteru musí být nakonfigurované toouse tunelové propojení SSH hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-138">Before hello CLI can be used with hello cluster, it must be configured toouse hello SSH tunnel.</span></span> <span data-ttu-id="1fb88-139">toodo tak, spusťte následující příkaz, úpravě hello portu v případě potřeby hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-139">toodo so, run hello following command, adjusting hello port if needed.</span></span>

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a><span data-ttu-id="1fb88-140">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="1fb88-140">Run an application</span></span>

<span data-ttu-id="1fb88-141">Výchozí hodnota Hello plánování mechanismus pro cluster služby ACS DC/OS je Marathon.</span><span class="sxs-lookup"><span data-stu-id="1fb88-141">hello default scheduling mechanism for an ACS DC/OS cluster is Marathon.</span></span> <span data-ttu-id="1fb88-142">Marathonu je použité toostart aplikace a spravovat stav hello hello aplikace v clusteru DC/OS hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-142">Marathon is used toostart an application and manage hello state of hello application on hello DC/OS cluster.</span></span> <span data-ttu-id="1fb88-143">tooschedule aplikaci přes Marathon, vytvořte soubor s názvem **marathon app.json**, a kopírování hello do něj následující obsah.</span><span class="sxs-lookup"><span data-stu-id="1fb88-143">tooschedule an application through Marathon, create a file named **marathon-app.json**, and copy hello following contents into it.</span></span> 

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

<span data-ttu-id="1fb88-144">Spusťte následující příkaz tooschedule hello aplikace toorun na clusteru DC/OS hello hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-144">Run hello following command tooschedule hello application toorun on hello DC/OS cluster.</span></span>

```azurecli
dcos marathon app add marathon-app.json
```

<span data-ttu-id="1fb88-145">toosee hello stav nasazení pro aplikaci hello, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-145">toosee hello deployment status for hello app, run hello following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="1fb88-146">Když hello **úlohy** hodnota sloupce přepíná z *0 nebo 1* příliš*1 nebo 1*, nasazení aplikace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="1fb88-146">When hello **TASKS** column value switches from *0/1* too*1/1*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a><span data-ttu-id="1fb88-147">Škálování aplikací Marathon</span><span class="sxs-lookup"><span data-stu-id="1fb88-147">Scale Marathon application</span></span>

<span data-ttu-id="1fb88-148">V předchozím příkladu hello byla vytvořena jedna instance aplikace.</span><span class="sxs-lookup"><span data-stu-id="1fb88-148">In hello previous example, a single instance application was created.</span></span> <span data-ttu-id="1fb88-149">Toto nasazení tak, aby byly k dispozici tři instance aplikace hello otevře hello tooupdate **marathon app.json** souboru a aktualizovat too3 vlastnost instance hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-149">tooupdate this deployment so that three instances of hello application are available, open up hello **marathon-app.json** file, and update hello instance property too3.</span></span>

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

<span data-ttu-id="1fb88-150">Aktualizovat aplikaci hello pomocí hello `dcos marathon app update` příkaz.</span><span class="sxs-lookup"><span data-stu-id="1fb88-150">Update hello application using hello `dcos marathon app update` command.</span></span>

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

<span data-ttu-id="1fb88-151">toosee hello stav nasazení pro aplikaci hello, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-151">toosee hello deployment status for hello app, run hello following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="1fb88-152">Když hello **úlohy** hodnota sloupce přepíná z *1/3* příliš*3/1*, nasazení aplikace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="1fb88-152">When hello **TASKS** column value switches from *1/3* too*3/1*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a><span data-ttu-id="1fb88-153">Spuštění aplikace dostupné pro internet</span><span class="sxs-lookup"><span data-stu-id="1fb88-153">Run internet accessible app</span></span>

<span data-ttu-id="1fb88-154">Hello clusteru ACS DC/OS se skládá ze dvou sad uzlu, hello jeden veřejný, která je přístupná na Internetu, a jeden privátní, která není přístupná na hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="1fb88-154">hello ACS DC/OS cluster consists of two node sets, one public which is accessible on hello internet, and one private which is not accessible on hello internet.</span></span> <span data-ttu-id="1fb88-155">Hello výchozí sada je hello privátní uzlů, která byla použita v posledním příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-155">hello default set is hello private nodes, which was used in hello last example.</span></span>

<span data-ttu-id="1fb88-156">toomake aplikaci přístupné na hello internet, je nasadit sady toohello veřejné uzlu.</span><span class="sxs-lookup"><span data-stu-id="1fb88-156">toomake an application accessible on hello internet, deploy them toohello public node set.</span></span> <span data-ttu-id="1fb88-157">toodo tedy poskytnout hello `acceptedResourceRoles` objektu hodnotu `slave_public`.</span><span class="sxs-lookup"><span data-stu-id="1fb88-157">toodo so, give hello `acceptedResourceRoles` object a value of `slave_public`.</span></span>

<span data-ttu-id="1fb88-158">Vytvořte soubor s názvem **nginx public.json** a kopírování hello do něj následující obsah.</span><span class="sxs-lookup"><span data-stu-id="1fb88-158">Create a file named **nginx-public.json** and copy hello following contents into it.</span></span>

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

<span data-ttu-id="1fb88-159">Spusťte následující příkaz tooschedule hello aplikace toorun na clusteru DC/OS hello hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-159">Run hello following command tooschedule hello application toorun on hello DC/OS cluster.</span></span>

```azurecli 
dcos marathon app add nginx-public.json
```

<span data-ttu-id="1fb88-160">Získáte hello veřejnou IP adresu hello agentů veřejné clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="1fb88-160">Get hello public IP address of hello DC/OS public cluster agents.</span></span>

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

<span data-ttu-id="1fb88-161">Procházení toothis adresu vrátí hello výchozí NGINX lokality.</span><span class="sxs-lookup"><span data-stu-id="1fb88-161">Browsing toothis address returns hello default NGINX site.</span></span>

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a><span data-ttu-id="1fb88-163">Škálování clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="1fb88-163">Scale DC/OS cluster</span></span>

<span data-ttu-id="1fb88-164">V předchozích příkladech hello aplikace se dokončilo toomultiple instance.</span><span class="sxs-lookup"><span data-stu-id="1fb88-164">In hello previous examples, an application was scaled toomultiple instance.</span></span> <span data-ttu-id="1fb88-165">Hello DC/OS infrastruktury může být také škálovat tooprovide více nebo méně výpočetní kapacitu.</span><span class="sxs-lookup"><span data-stu-id="1fb88-165">hello DC/OS infrastructure can also be scaled tooprovide more or less compute capacity.</span></span> <span data-ttu-id="1fb88-166">To lze provést pomocí hello [škálování služby acs az]() příkaz.</span><span class="sxs-lookup"><span data-stu-id="1fb88-166">This is done with hello [az acs scale]() command.</span></span> 

<span data-ttu-id="1fb88-167">toosee hello aktuální počet agentů DC/OS použijte hello [zobrazit acs az](/cli/azure/acs#show) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1fb88-167">toosee hello current count of DC/OS agents, use hello [az acs show](/cli/azure/acs#show) command.</span></span>

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

<span data-ttu-id="1fb88-168">tooincrease hello počet too5, použijte hello [škálování služby acs az](/cli/azure/acs#scale) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1fb88-168">tooincrease hello count too5, use hello [az acs scale](/cli/azure/acs#scale) command.</span></span> 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a><span data-ttu-id="1fb88-169">Odstranění clusteru DC/OS</span><span class="sxs-lookup"><span data-stu-id="1fb88-169">Delete DC/OS cluster</span></span>

<span data-ttu-id="1fb88-170">Pokud již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove clusteru DC/OS a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="1fb88-170">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, DC/OS cluster, and all related resources.</span></span>

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a><span data-ttu-id="1fb88-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1fb88-171">Next steps</span></span>

<span data-ttu-id="1fb88-172">V tomto kurzu jste se naučili o základní úlohy správy DC/OS, včetně následujících hello.</span><span class="sxs-lookup"><span data-stu-id="1fb88-172">In this tutorial, you have learned about basic DC/OS management task including hello following.</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="1fb88-173">Vytvoření clusteru služby ACS DC/OS</span><span class="sxs-lookup"><span data-stu-id="1fb88-173">Create an ACS DC/OS cluster</span></span>
> * <span data-ttu-id="1fb88-174">Připojte toohello cluster</span><span class="sxs-lookup"><span data-stu-id="1fb88-174">Connect toohello cluster</span></span>
> * <span data-ttu-id="1fb88-175">Nainstalujte hello rozhraní příkazového řádku DC/OS</span><span class="sxs-lookup"><span data-stu-id="1fb88-175">Install hello DC/OS CLI</span></span>
> * <span data-ttu-id="1fb88-176">Nasazení clusteru toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="1fb88-176">Deploy an application toohello cluster</span></span>
> * <span data-ttu-id="1fb88-177">Škálování aplikace na clusteru hello</span><span class="sxs-lookup"><span data-stu-id="1fb88-177">Scale an application on hello cluster</span></span>
> * <span data-ttu-id="1fb88-178">Škálování uzly clusteru DC/OS hello</span><span class="sxs-lookup"><span data-stu-id="1fb88-178">Scale hello DC/OS cluster nodes</span></span>
> * <span data-ttu-id="1fb88-179">Odstranění clusteru DC/OS hello</span><span class="sxs-lookup"><span data-stu-id="1fb88-179">Delete hello DC/OS cluster</span></span>

<span data-ttu-id="1fb88-180">ADVANCE toohello další kurz toolearn o načtení vyrovnávání aplikace v DC/OS v Azure.</span><span class="sxs-lookup"><span data-stu-id="1fb88-180">Advance toohello next tutorial toolearn about load balancing application in DC/OS on Azure.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="1fb88-181">Vyrovnávání zatížení aplikací</span><span class="sxs-lookup"><span data-stu-id="1fb88-181">Load balance applications</span></span>](container-service-load-balancing.md)