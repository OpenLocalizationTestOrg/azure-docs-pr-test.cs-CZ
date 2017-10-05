---
title: "ACR pomocí clusteru služby Azure DC/OS | Microsoft Docs"
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
ms.openlocfilehash: 618d32ca919e50d41b85c800225fe72c3b94e537
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-acr-with-a-dcos-cluster-to-deploy-your-application"></a><span data-ttu-id="f7423-104">Použití ACR s cluster DC/OS pro nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="f7423-104">Use ACR with a DC/OS cluster to deploy your application</span></span>

<span data-ttu-id="f7423-105">V tomto článku jsme prozkoumejte použití registru kontejner Azure s cluster DC/OS.</span><span class="sxs-lookup"><span data-stu-id="f7423-105">In this article, we explore how to use Azure Container Registry with a DC/OS cluster.</span></span> <span data-ttu-id="f7423-106">Pomocí ACR umožňuje soukromě ukládání a správě bitových kopií kontejneru.</span><span class="sxs-lookup"><span data-stu-id="f7423-106">Using ACR allows you to privately store and manage container images.</span></span> <span data-ttu-id="f7423-107">Tento kurz obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="f7423-107">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f7423-108">Nasazení registru kontejner Azure (v případě potřeby)</span><span class="sxs-lookup"><span data-stu-id="f7423-108">Deploy Azure Container Registry (if needed)</span></span>
> * <span data-ttu-id="f7423-109">Konfigurace ověřování ACR na cluster DC/OS</span><span class="sxs-lookup"><span data-stu-id="f7423-109">Configure ACR authentication on a DC/OS cluster</span></span>
> * <span data-ttu-id="f7423-110">Odeslat bitovou kopii do registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="f7423-110">Uploaded an image to the Azure Container Registry</span></span>
> * <span data-ttu-id="f7423-111">Spusťte kontejner image z registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="f7423-111">Run a container image from the Azure Container Registry</span></span>

<span data-ttu-id="f7423-112">Je třeba cluster služby ACS DC/OS, pokud chcete provést kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f7423-112">You need an ACS DC/OS cluster to complete the steps in this tutorial.</span></span> <span data-ttu-id="f7423-113">V případě potřeby [tento ukázkový skript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="f7423-113">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="f7423-114">Tento kurz vyžaduje Azure CLI verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f7423-114">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="f7423-115">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="f7423-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="f7423-116">Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f7423-116">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="f7423-117">Nasadit kontejner Azure registru</span><span class="sxs-lookup"><span data-stu-id="f7423-117">Deploy Azure Container Registry</span></span>

<span data-ttu-id="f7423-118">V případě potřeby vytvořte registru kontejneru Azure s [vytvořit az acr](/cli/azure/acr#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f7423-118">If needed, create an Azure Container registry with the [az acr create](/cli/azure/acr#create) command.</span></span> 

<span data-ttu-id="f7423-119">Následující příklad vytvoří registr s využitím náhodnému generování název.</span><span class="sxs-lookup"><span data-stu-id="f7423-119">The following example creates a registry with a randomly generate name.</span></span> <span data-ttu-id="f7423-120">Registr je také nakonfigurovaný se k pomocí účtu správce `--admin-enabled` argument.</span><span class="sxs-lookup"><span data-stu-id="f7423-120">The registry is also configured with an admin account using the `--admin-enabled` argument.</span></span>

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic --admin-enabled true
```

<span data-ttu-id="f7423-121">Po vytvoření registru rozhraní příkazového řádku Azure výstupy data podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="f7423-121">Once the registry has been created, the Azure CLI outputs data similar to the following.</span></span> <span data-ttu-id="f7423-122">Poznamenejte si `name` a `loginServer`, ty se používají v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="f7423-122">Take note of the `name` and `loginServer`, these are used in later steps.</span></span>

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

<span data-ttu-id="f7423-123">Získání kontejneru přihlašovacích údajů registru pomocí [az acr pověření zobrazit](/cli/azure/acr/credential) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f7423-123">Get the container registry credentials using the [az acr credential show](/cli/azure/acr/credential) command.</span></span> <span data-ttu-id="f7423-124">Nahraďte `--name` se si poznamenali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="f7423-124">Substitute the `--name` with the one noted in the last step.</span></span> <span data-ttu-id="f7423-125">Všimněte si jeden hesla, je potřeba v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="f7423-125">Take note of one password, it is needed in a later step.</span></span>

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

<span data-ttu-id="f7423-126">Další informace o registru kontejner Azure najdete v tématu [Úvod do privátní registrech kontejner Docker](../../container-registry/container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="f7423-126">For more information on Azure Container Registry, see [Introduction to private Docker container registries](../../container-registry/container-registry-intro.md).</span></span> 

## <a name="manage-acr-authentication"></a><span data-ttu-id="f7423-127">Spravovat ACR ověřování</span><span class="sxs-lookup"><span data-stu-id="f7423-127">Manage ACR authentication</span></span>

<span data-ttu-id="f7423-128">Konvenční způsob, jak nabízení a vyžadování image privátní registr, je nejprve ověřit pomocí registru.</span><span class="sxs-lookup"><span data-stu-id="f7423-128">The conventional way to push and pull image from a private registry is to first authenticate with the registry.</span></span> <span data-ttu-id="f7423-129">Chcete-li to provést, použijte `docker login` na libovolného klienta, který potřebuje přístup k privátním registru.</span><span class="sxs-lookup"><span data-stu-id="f7423-129">To do so, you would use the `docker login` command on any client that needs to access the private registry.</span></span> <span data-ttu-id="f7423-130">Vzhledem k tomu, že cluster DC/OS může obsahovat mnoho uzlů, které potřebujete se ACR ověřit, je užitečné pro automatizaci tohoto procesu mezi všechny uzly.</span><span class="sxs-lookup"><span data-stu-id="f7423-130">Because a DC/OS cluster can contain many nodes, all of which need to be authenticated with the ACR, it is helpful to automate this process across each node.</span></span> 

### <a name="create-shared-storage"></a><span data-ttu-id="f7423-131">Vytvoření sdílené úložiště</span><span class="sxs-lookup"><span data-stu-id="f7423-131">Create shared storage</span></span>

<span data-ttu-id="f7423-132">Tento proces používá sdílenou složku Azure, namontování na každém uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f7423-132">This process uses an Azure file share that has been mounted on each node in the cluster.</span></span> <span data-ttu-id="f7423-133">Pokud již jste nenastavili sdílené úložiště, najdete v části [nastavit sdílenou složku v clusteru DC/OS](container-service-dcos-fileshare.md).</span><span class="sxs-lookup"><span data-stu-id="f7423-133">If you have not already set up shared storage, see [Setup a file share inside a DC/OS cluster](container-service-dcos-fileshare.md).</span></span>

### <a name="configure-acr-authentication"></a><span data-ttu-id="f7423-134">Konfigurace ověřování ACR</span><span class="sxs-lookup"><span data-stu-id="f7423-134">Configure ACR authentication</span></span>

<span data-ttu-id="f7423-135">Nejdřív získat plně kvalifikovaný název domény hlavního serveru DC/OS a uložte ho do proměnné.</span><span class="sxs-lookup"><span data-stu-id="f7423-135">First, get the FQDN of the DC/OS master and store it in a variable.</span></span>

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

<span data-ttu-id="f7423-136">Vytvoření připojení SSH s hlavním serverem (nebo prvního hlavního serveru) ve vašem clusteru založenými na DC/OS.</span><span class="sxs-lookup"><span data-stu-id="f7423-136">Create an SSH connection with the master (or the first master) of your DC/OS-based cluster.</span></span> <span data-ttu-id="f7423-137">Aktualizujte uživatelské jméno, pokud při vytvoření clusteru se použila jiná než výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f7423-137">Update the user name if a non-default value was used when creating the cluster.</span></span>

```azurecli-interactive
ssh azureuser@$FQDN
```

<span data-ttu-id="f7423-138">Spusťte následující příkaz k přihlášení do registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="f7423-138">Run the following command to login to the Azure Container Registry.</span></span> <span data-ttu-id="f7423-139">Nahraďte `--username` s názvem kontejneru registru a `--password` s jedním z zadaná hesla.</span><span class="sxs-lookup"><span data-stu-id="f7423-139">Replace the `--username` with the name of the container registry, and the `--password` with one of the provided passwords.</span></span> <span data-ttu-id="f7423-140">Nahraďte poslední argument *mycontainerregistry.azurecr.io* v příkladu s názvem loginServer registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="f7423-140">Replace the last argument *mycontainerregistry.azurecr.io* in the example with the loginServer name of the container registry.</span></span> 

<span data-ttu-id="f7423-141">Tento příkaz uloží hodnoty ověřování místně v části `~/.docker` cesta.</span><span class="sxs-lookup"><span data-stu-id="f7423-141">This command stores the authentication values locally under the `~/.docker` path.</span></span>

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

<span data-ttu-id="f7423-142">Vytvořte komprimovaný soubor, který obsahuje hodnoty registru ověřování kontejneru.</span><span class="sxs-lookup"><span data-stu-id="f7423-142">Create a compressed file that contains the container registry authentication values.</span></span>

```azurecli-interactive
tar czf docker.tar.gz .docker
```

<span data-ttu-id="f7423-143">Zkopírujte tento soubor do clusteru sdíleného úložiště.</span><span class="sxs-lookup"><span data-stu-id="f7423-143">Copy this file to the cluster shared storage.</span></span> <span data-ttu-id="f7423-144">Tento krok umožňuje soubor k dispozici na všech uzlech clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="f7423-144">This step makes the file available on all nodes of the DC/OS cluster.</span></span>

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-to-acr"></a><span data-ttu-id="f7423-145">Odeslat bitovou kopii na ACR</span><span class="sxs-lookup"><span data-stu-id="f7423-145">Upload image to ACR</span></span>

<span data-ttu-id="f7423-146">Nyní z vývojovém počítači, nebo všechny ostatní systémy s Docker nainstalovaná, vytvoření bitové kopie a nahrajte ho do registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="f7423-146">Now from a development machine, or any other system with Docker installed, create an image and upload it to the Azure Container Registry.</span></span>

<span data-ttu-id="f7423-147">Vytvořte kontejner z Ubuntu image.</span><span class="sxs-lookup"><span data-stu-id="f7423-147">Create a container from the Ubuntu image.</span></span>

```azurecli-interactive
docker run ubunut --name base-image
```

<span data-ttu-id="f7423-148">Nyní zachytit kontejneru do novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="f7423-148">Now capture the container into a new image.</span></span> <span data-ttu-id="f7423-149">Musí obsahovat název bitové kopie `loginServer` název kontejneru registrywith a formát `loginServer/imageName`.</span><span class="sxs-lookup"><span data-stu-id="f7423-149">The image name needs to include the `loginServer` name of the container registrywith a format of `loginServer/imageName`.</span></span>

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

<span data-ttu-id="f7423-150">Přihlaste se do registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="f7423-150">Login into the Azure Container Registry.</span></span> <span data-ttu-id="f7423-151">Nahraďte název názvu loginServer – uživatelské jméno s názvem kontejneru registru a--heslo s jedním z zadaná hesla.</span><span class="sxs-lookup"><span data-stu-id="f7423-151">Replace the name with the loginServer name, the --username with the name of the container registry, and the --password with one of the provided passwords.</span></span>

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

<span data-ttu-id="f7423-152">Nakonec nahrajte image na registru ACR.</span><span class="sxs-lookup"><span data-stu-id="f7423-152">Finally, upload the image to the ACR registry.</span></span> <span data-ttu-id="f7423-153">Tento příklad odešle obrázek s názvem demo orchestrátoru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="f7423-153">This example uploads an image named dcos-demo.</span></span>

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a><span data-ttu-id="f7423-154">Spusťte bitovou kopii z ACR</span><span class="sxs-lookup"><span data-stu-id="f7423-154">Run an image from ACR</span></span>

<span data-ttu-id="f7423-155">Chcete-li použít bitovou kopii z registru ACR, vytvořte soubor názvy *acrDemo.json* a zkopírujte následující text do ní.</span><span class="sxs-lookup"><span data-stu-id="f7423-155">To use an image from the ACR registry, create a file names *acrDemo.json* and copy the following text into it.</span></span> <span data-ttu-id="f7423-156">Nahraďte název bitové kopie s názvem loginServer registru kontejneru a název bitové kopie, například `loginServer/imageName`.</span><span class="sxs-lookup"><span data-stu-id="f7423-156">Replace the image name with the container registry loginServer name and image name, for example `loginServer/imageName`.</span></span> <span data-ttu-id="f7423-157">Poznamenejte si `uris` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f7423-157">Take note of the `uris` property.</span></span> <span data-ttu-id="f7423-158">Tato vlastnost obsahuje umístění kontejneru ověřování souboru registru, který v tomto případě je Azure sdílené složky, která je připojena v každém uzlu v clusteru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="f7423-158">This property holds the location of the container registry authentication file, which in this case is the Azure file share that is mounted on each node in the DC/OS cluster.</span></span>

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

<span data-ttu-id="f7423-159">Nasazení aplikace pomocí rozhraní příkazového řádku DC / ° c.</span><span class="sxs-lookup"><span data-stu-id="f7423-159">Deploy the application with the DC/OC CLI.</span></span>

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a><span data-ttu-id="f7423-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7423-160">Next steps</span></span>

<span data-ttu-id="f7423-161">V tomto kurzu máte nakonfigurovat DC/OS použijte registru kontejner Azure, včetně těchto úloh:</span><span class="sxs-lookup"><span data-stu-id="f7423-161">In this tutorial you have configure DC/OS to use Azure Container Registry including the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f7423-162">Nasazení registru kontejner Azure (v případě potřeby)</span><span class="sxs-lookup"><span data-stu-id="f7423-162">Deploy Azure Container Registry (if needed)</span></span>
> * <span data-ttu-id="f7423-163">Konfigurace ověřování ACR na cluster DC/OS</span><span class="sxs-lookup"><span data-stu-id="f7423-163">Configure ACR authentication on a DC/OS cluster</span></span>
> * <span data-ttu-id="f7423-164">Odeslat bitovou kopii do registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="f7423-164">Uploaded an image to the Azure Container Registry</span></span>
> * <span data-ttu-id="f7423-165">Spusťte kontejner image z registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="f7423-165">Run a container image from the Azure Container Registry</span></span>
