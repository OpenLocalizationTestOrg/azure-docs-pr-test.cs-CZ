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
# <a name="use-acr-with-a-dcos-cluster-toodeploy-your-application"></a><span data-ttu-id="1775c-104">Pomocí ACR toodeploy clusteru DC/OS vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="1775c-104">Use ACR with a DC/OS cluster toodeploy your application</span></span>

<span data-ttu-id="1775c-105">V tomto článku jsme prozkoumat jak toouse registru kontejner Azure s cluster DC/OS.</span><span class="sxs-lookup"><span data-stu-id="1775c-105">In this article, we explore how toouse Azure Container Registry with a DC/OS cluster.</span></span> <span data-ttu-id="1775c-106">Pomocí ACR vám umožní tooprivately úložiště a správě bitových kopií kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1775c-106">Using ACR allows you tooprivately store and manage container images.</span></span> <span data-ttu-id="1775c-107">Tento kurz se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="1775c-107">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1775c-108">Nasazení registru kontejner Azure (v případě potřeby)</span><span class="sxs-lookup"><span data-stu-id="1775c-108">Deploy Azure Container Registry (if needed)</span></span>
> * <span data-ttu-id="1775c-109">Konfigurace ověřování ACR na cluster DC/OS</span><span class="sxs-lookup"><span data-stu-id="1775c-109">Configure ACR authentication on a DC/OS cluster</span></span>
> * <span data-ttu-id="1775c-110">Nahrát bitovou kopii toohello registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="1775c-110">Uploaded an image toohello Azure Container Registry</span></span>
> * <span data-ttu-id="1775c-111">Spusťte kontejner image z hello registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="1775c-111">Run a container image from hello Azure Container Registry</span></span>

<span data-ttu-id="1775c-112">Budete potřebovat DC/OS ACS clusteru toocomplete hello kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1775c-112">You need an ACS DC/OS cluster toocomplete hello steps in this tutorial.</span></span> <span data-ttu-id="1775c-113">V případě potřeby [tento ukázkový skript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="1775c-113">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="1775c-114">Tento kurz vyžaduje hello Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1775c-114">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="1775c-115">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="1775c-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="1775c-116">Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1775c-116">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="1775c-117">Nasadit kontejner Azure registru</span><span class="sxs-lookup"><span data-stu-id="1775c-117">Deploy Azure Container Registry</span></span>

<span data-ttu-id="1775c-118">V případě potřeby vytvořte registru kontejneru Azure s hello [vytvořit az acr](/cli/azure/acr#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1775c-118">If needed, create an Azure Container registry with hello [az acr create](/cli/azure/acr#create) command.</span></span> 

<span data-ttu-id="1775c-119">Hello následující příklad vytvoří registr s využitím náhodnému generování název.</span><span class="sxs-lookup"><span data-stu-id="1775c-119">hello following example creates a registry with a randomly generate name.</span></span> <span data-ttu-id="1775c-120">Hello registru je také nakonfigurovaný se na správce účtu pomocí hello `--admin-enabled` argument.</span><span class="sxs-lookup"><span data-stu-id="1775c-120">hello registry is also configured with an admin account using hello `--admin-enabled` argument.</span></span>

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic --admin-enabled true
```

<span data-ttu-id="1775c-121">Po vytvoření hello registru hello rozhraní příkazového řádku Azure výstupy podobné toohello následující data.</span><span class="sxs-lookup"><span data-stu-id="1775c-121">Once hello registry has been created, hello Azure CLI outputs data similar toohello following.</span></span> <span data-ttu-id="1775c-122">Poznamenejte si hello `name` a `loginServer`, ty se používají v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="1775c-122">Take note of hello `name` and `loginServer`, these are used in later steps.</span></span>

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

<span data-ttu-id="1775c-123">Získat hello kontejneru registru přihlašovacích údajů pomocí hello [az acr pověření zobrazit](/cli/azure/acr/credential) příkaz.</span><span class="sxs-lookup"><span data-stu-id="1775c-123">Get hello container registry credentials using hello [az acr credential show](/cli/azure/acr/credential) command.</span></span> <span data-ttu-id="1775c-124">SUBSTITUTE hello `--name` s hello jeden si poznamenali v posledním kroku hello.</span><span class="sxs-lookup"><span data-stu-id="1775c-124">Substitute hello `--name` with hello one noted in hello last step.</span></span> <span data-ttu-id="1775c-125">Všimněte si jeden hesla, je potřeba v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="1775c-125">Take note of one password, it is needed in a later step.</span></span>

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

<span data-ttu-id="1775c-126">Další informace o registru kontejner Azure najdete v tématu [registrech kontejner Docker tooprivate ÚVOD](../../container-registry/container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="1775c-126">For more information on Azure Container Registry, see [Introduction tooprivate Docker container registries](../../container-registry/container-registry-intro.md).</span></span> 

## <a name="manage-acr-authentication"></a><span data-ttu-id="1775c-127">Spravovat ACR ověřování</span><span class="sxs-lookup"><span data-stu-id="1775c-127">Manage ACR authentication</span></span>

<span data-ttu-id="1775c-128">Hello konvenční způsob, jakým toopush a vyžadování image z privátní registru je toofirst ověřit pomocí registru hello.</span><span class="sxs-lookup"><span data-stu-id="1775c-128">hello conventional way toopush and pull image from a private registry is toofirst authenticate with hello registry.</span></span> <span data-ttu-id="1775c-129">toodo, takže byste použili hello `docker login` na libovolného klienta, který potřebuje tooaccess hello privátní registru.</span><span class="sxs-lookup"><span data-stu-id="1775c-129">toodo so, you would use hello `docker login` command on any client that needs tooaccess hello private registry.</span></span> <span data-ttu-id="1775c-130">Vzhledem k tomu cluster DC/OS může obsahovat mnoho uzlů, které potřebovat toobe ověření pomocí hello ACR, je užitečné tooautomate tento proces v každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="1775c-130">Because a DC/OS cluster can contain many nodes, all of which need toobe authenticated with hello ACR, it is helpful tooautomate this process across each node.</span></span> 

### <a name="create-shared-storage"></a><span data-ttu-id="1775c-131">Vytvoření sdílené úložiště</span><span class="sxs-lookup"><span data-stu-id="1775c-131">Create shared storage</span></span>

<span data-ttu-id="1775c-132">Tento proces používá sdílenou složku Azure, namontování na každém uzlu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1775c-132">This process uses an Azure file share that has been mounted on each node in hello cluster.</span></span> <span data-ttu-id="1775c-133">Pokud již jste nenastavili sdílené úložiště, najdete v části [nastavit sdílenou složku v clusteru DC/OS](container-service-dcos-fileshare.md).</span><span class="sxs-lookup"><span data-stu-id="1775c-133">If you have not already set up shared storage, see [Setup a file share inside a DC/OS cluster](container-service-dcos-fileshare.md).</span></span>

### <a name="configure-acr-authentication"></a><span data-ttu-id="1775c-134">Konfigurace ověřování ACR</span><span class="sxs-lookup"><span data-stu-id="1775c-134">Configure ACR authentication</span></span>

<span data-ttu-id="1775c-135">Nejdřív získáte hello plně kvalifikovaný název domény hello DC/OS hlavní a uložte ho do proměnné.</span><span class="sxs-lookup"><span data-stu-id="1775c-135">First, get hello FQDN of hello DC/OS master and store it in a variable.</span></span>

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

<span data-ttu-id="1775c-136">Vytvoření připojení SSH s hello hlavního serveru (nebo hello první) clusteru založenými na DC/OS.</span><span class="sxs-lookup"><span data-stu-id="1775c-136">Create an SSH connection with hello master (or hello first master) of your DC/OS-based cluster.</span></span> <span data-ttu-id="1775c-137">Aktualizujte hello uživatelské jméno, pokud byla použita jiná než výchozí hodnotu, při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1775c-137">Update hello user name if a non-default value was used when creating hello cluster.</span></span>

```azurecli-interactive
ssh azureuser@$FQDN
```

<span data-ttu-id="1775c-138">Spusťte následující příkaz toologin toohello registru kontejner Azure hello.</span><span class="sxs-lookup"><span data-stu-id="1775c-138">Run hello following command toologin toohello Azure Container Registry.</span></span> <span data-ttu-id="1775c-139">Nahraďte hello `--username` s názvem hello hello kontejneru registru a hello `--password` s jedním z hello zadaná hesla.</span><span class="sxs-lookup"><span data-stu-id="1775c-139">Replace hello `--username` with hello name of hello container registry, and hello `--password` with one of hello provided passwords.</span></span> <span data-ttu-id="1775c-140">Nahraďte hello poslední argument *mycontainerregistry.azurecr.io* v příkladu hello s názvem loginServer hello hello kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="1775c-140">Replace hello last argument *mycontainerregistry.azurecr.io* in hello example with hello loginServer name of hello container registry.</span></span> 

<span data-ttu-id="1775c-141">Tento příkaz uloží hodnoty ověřování hello místně v rámci hello `~/.docker` cesta.</span><span class="sxs-lookup"><span data-stu-id="1775c-141">This command stores hello authentication values locally under hello `~/.docker` path.</span></span>

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

<span data-ttu-id="1775c-142">Vytvořte komprimovaný soubor, který obsahuje hodnoty ověřování registru hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="1775c-142">Create a compressed file that contains hello container registry authentication values.</span></span>

```azurecli-interactive
tar czf docker.tar.gz .docker
```

<span data-ttu-id="1775c-143">Zkopírujte tento soubor toohello sdílené úložiště clusteru.</span><span class="sxs-lookup"><span data-stu-id="1775c-143">Copy this file toohello cluster shared storage.</span></span> <span data-ttu-id="1775c-144">Tento krok zpřístupní hello soubor na všech uzlech clusteru DC/OS hello.</span><span class="sxs-lookup"><span data-stu-id="1775c-144">This step makes hello file available on all nodes of hello DC/OS cluster.</span></span>

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-tooacr"></a><span data-ttu-id="1775c-145">Nahrát tooACR bitové kopie</span><span class="sxs-lookup"><span data-stu-id="1775c-145">Upload image tooACR</span></span>

<span data-ttu-id="1775c-146">Nyní z vývojovém počítači, nebo všechny ostatní systémy s Docker nainstalovaná, vytvoření bitové kopie a nahrajte ho toohello registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="1775c-146">Now from a development machine, or any other system with Docker installed, create an image and upload it toohello Azure Container Registry.</span></span>

<span data-ttu-id="1775c-147">Vytvořte kontejner z hello Ubuntu image.</span><span class="sxs-lookup"><span data-stu-id="1775c-147">Create a container from hello Ubuntu image.</span></span>

```azurecli-interactive
docker run ubunut --name base-image
```

<span data-ttu-id="1775c-148">Nyní zachytit hello kontejneru do novou bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="1775c-148">Now capture hello container into a new image.</span></span> <span data-ttu-id="1775c-149">název bitové kopie Hello musí tooinclude hello `loginServer` název kontejneru registrywith hello a formát `loginServer/imageName`.</span><span class="sxs-lookup"><span data-stu-id="1775c-149">hello image name needs tooinclude hello `loginServer` name of hello container registrywith a format of `loginServer/imageName`.</span></span>

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

<span data-ttu-id="1775c-150">Přihlaste se do hello registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="1775c-150">Login into hello Azure Container Registry.</span></span> <span data-ttu-id="1775c-151">Nahraďte název hello názvem hello loginServer, hello – uživatelské jméno s názvem hello hello kontejneru registru a hello – heslo s jedním z hello zadaná hesla.</span><span class="sxs-lookup"><span data-stu-id="1775c-151">Replace hello name with hello loginServer name, hello --username with hello name of hello container registry, and hello --password with one of hello provided passwords.</span></span>

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

<span data-ttu-id="1775c-152">Nakonec nahrajte hello image toohello ACR registru.</span><span class="sxs-lookup"><span data-stu-id="1775c-152">Finally, upload hello image toohello ACR registry.</span></span> <span data-ttu-id="1775c-153">Tento příklad odešle obrázek s názvem demo orchestrátoru DC/OS.</span><span class="sxs-lookup"><span data-stu-id="1775c-153">This example uploads an image named dcos-demo.</span></span>

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a><span data-ttu-id="1775c-154">Spusťte bitovou kopii z ACR</span><span class="sxs-lookup"><span data-stu-id="1775c-154">Run an image from ACR</span></span>

<span data-ttu-id="1775c-155">toouse na bitovou kopii z registru ACR hello, vytvořte soubor názvy *acrDemo.json* a kopírování hello následující text do ní.</span><span class="sxs-lookup"><span data-stu-id="1775c-155">toouse an image from hello ACR registry, create a file names *acrDemo.json* and copy hello following text into it.</span></span> <span data-ttu-id="1775c-156">Nahraďte název bitové kopie hello název loginServer registru hello kontejneru a název bitové kopie, například `loginServer/imageName`.</span><span class="sxs-lookup"><span data-stu-id="1775c-156">Replace hello image name with hello container registry loginServer name and image name, for example `loginServer/imageName`.</span></span> <span data-ttu-id="1775c-157">Poznamenejte si hello `uris` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1775c-157">Take note of hello `uris` property.</span></span> <span data-ttu-id="1775c-158">Tato vlastnost obsahuje umístění hello hello kontejneru registru ověřování souboru, který v tomto případě je hello Azure sdílené složky, která je připojena v každém uzlu v clusteru DC/OS hello.</span><span class="sxs-lookup"><span data-stu-id="1775c-158">This property holds hello location of hello container registry authentication file, which in this case is hello Azure file share that is mounted on each node in hello DC/OS cluster.</span></span>

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

<span data-ttu-id="1775c-159">Nasazení aplikace hello s hello rozhraní příkazového řádku DC / ° c.</span><span class="sxs-lookup"><span data-stu-id="1775c-159">Deploy hello application with hello DC/OC CLI.</span></span>

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a><span data-ttu-id="1775c-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1775c-160">Next steps</span></span>

<span data-ttu-id="1775c-161">V tomto kurzu máte nakonfigurovat toouse DC/OS, které registru kontejner Azure včetně hello následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="1775c-161">In this tutorial you have configure DC/OS toouse Azure Container Registry including hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1775c-162">Nasazení registru kontejner Azure (v případě potřeby)</span><span class="sxs-lookup"><span data-stu-id="1775c-162">Deploy Azure Container Registry (if needed)</span></span>
> * <span data-ttu-id="1775c-163">Konfigurace ověřování ACR na cluster DC/OS</span><span class="sxs-lookup"><span data-stu-id="1775c-163">Configure ACR authentication on a DC/OS cluster</span></span>
> * <span data-ttu-id="1775c-164">Nahrát bitovou kopii toohello registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="1775c-164">Uploaded an image toohello Azure Container Registry</span></span>
> * <span data-ttu-id="1775c-165">Spusťte kontejner image z hello registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="1775c-165">Run a container image from hello Azure Container Registry</span></span>
