---
title: "aaaCreate vaše první kontejner instancí kontejnerů Azure | Dokumentace Azure"
description: "Nasazení služby Azure Container Instances a zahájení práce"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b57c52714933bd3b28c44d33f9af7cd1f23fb951
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a><span data-ttu-id="39cc9-103">Vytvoření prvního kontejneru ve službě Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="39cc9-103">Create your first container in Azure Container Instances</span></span>

<span data-ttu-id="39cc9-104">Azure instancí kontejnerů umožňuje snadno toocreate a Správa kontejnerů v Azure.</span><span class="sxs-lookup"><span data-stu-id="39cc9-104">Azure Container Instances makes it easy toocreate and manage containers in Azure.</span></span> <span data-ttu-id="39cc9-105">V této úvodní vytvořit kontejner ve službě Azure a zveřejnění toohello internet s veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="39cc9-105">In this quick start, you will create a container in Azure and expose it toohello internet with a public IP address.</span></span> <span data-ttu-id="39cc9-106">K dokončení této operace stačí jediný příkaz.</span><span class="sxs-lookup"><span data-stu-id="39cc9-106">This operation is completed in a single command.</span></span> <span data-ttu-id="39cc9-107">Během několika sekund uvidíte ve svém prohlížeči toto:</span><span class="sxs-lookup"><span data-stu-id="39cc9-107">Within just a few seconds, you will see this in your browser:</span></span>

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

<span data-ttu-id="39cc9-109">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="39cc9-109">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="39cc9-110">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento rychlý start vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello 2.0.12 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="39cc9-110">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.12 or later.</span></span> <span data-ttu-id="39cc9-111">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="39cc9-111">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="39cc9-112">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="39cc9-112">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="39cc9-113">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="39cc9-113">Create a resource group</span></span>

<span data-ttu-id="39cc9-114">Služba Azure Container Instances je prostředek Azure a musí být umístěná ve skupině prostředků Azure, což je logická kolekce, ve které se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="39cc9-114">Azure Container Instances are Azure resources and must be placed in an Azure resource group, a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="39cc9-115">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="39cc9-115">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="39cc9-116">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="39cc9-116">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a><span data-ttu-id="39cc9-117">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="39cc9-117">Create a container</span></span>

<span data-ttu-id="39cc9-118">Kontejner můžete vytvořit zadáním názvu, image Dockeru a skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="39cc9-118">You can create a container by providing a name, a Docker image, and an Azure resource group.</span></span> <span data-ttu-id="39cc9-119">Volitelně můžete vystavit hello kontejneru toohello internet s veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="39cc9-119">You can optionally expose hello container toohello internet with a public IP address.</span></span> <span data-ttu-id="39cc9-120">V tomto případě použijeme kontejner, který je hostitelem velmi jednoduché webové aplikace napsané v [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="39cc9-120">In this case, we'll use a container that hosts a very simple web app written in [Node.js](http://nodejs.org).</span></span>

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

<span data-ttu-id="39cc9-121">Během pár sekund měli byste obdržet žádost o tooyour odpovědi.</span><span class="sxs-lookup"><span data-stu-id="39cc9-121">Within a few seconds, you should get a response tooyour request.</span></span> <span data-ttu-id="39cc9-122">Na začátku hello kontejner bude v **vytváření** stavu, ale by se měl spustit během několika sekund.</span><span class="sxs-lookup"><span data-stu-id="39cc9-122">Initially, hello container will be in a **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="39cc9-123">Můžete zkontrolovat stav hello pomocí hello `show` příkaz:</span><span class="sxs-lookup"><span data-stu-id="39cc9-123">You can check hello status using hello `show` command:</span></span>

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="39cc9-124">V dolní části hello hello výstupu zobrazí se stav zřizování hello kontejner a jeho IP adresu:</span><span class="sxs-lookup"><span data-stu-id="39cc9-124">At hello bottom of hello output, you will see hello container's provisioning state and its IP address:</span></span>

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

<span data-ttu-id="39cc9-125">Jakmile hello kontejneru přesune toohello **úspěšné** stavu, můžete dosáhnout v prohlížeči hello pomocí hello IP adresa zadaná.</span><span class="sxs-lookup"><span data-stu-id="39cc9-125">Once hello container moves toohello **Succeeded** state, you can reach it in hello browser using hello IP address provided.</span></span> 

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

## <a name="pull-hello-container-logs"></a><span data-ttu-id="39cc9-127">Hello kontejneru protokoly pro vyžádání obsahu</span><span class="sxs-lookup"><span data-stu-id="39cc9-127">Pull hello container logs</span></span>

<span data-ttu-id="39cc9-128">Může vyžádat hello protokoly pro kontejner hello jste vytvořili pomocí hello `logs` příkaz:</span><span class="sxs-lookup"><span data-stu-id="39cc9-128">You can pull hello logs for hello container you created using hello `logs` command:</span></span>

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="39cc9-129">Výstup:</span><span class="sxs-lookup"><span data-stu-id="39cc9-129">Output:</span></span>

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-hello-container"></a><span data-ttu-id="39cc9-130">Odstranit kontejner hello</span><span class="sxs-lookup"><span data-stu-id="39cc9-130">Delete hello container</span></span>

<span data-ttu-id="39cc9-131">Až skončíte s hello kontejneru, můžete odebrat pomocí hello `delete` příkaz:</span><span class="sxs-lookup"><span data-stu-id="39cc9-131">When you are done with hello container, you can remove it using hello `delete` command:</span></span>

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="39cc9-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39cc9-132">Next steps</span></span>

<span data-ttu-id="39cc9-133">Všechny hello kódu pro kontejner hello použitý v této úvodní je k dispozici [na Githubu][app-github-repo], společně s jeho soubor Docker.</span><span class="sxs-lookup"><span data-stu-id="39cc9-133">All of hello code for hello container used in this quick start is available [on GitHub][app-github-repo], along with its Dockerfile.</span></span> <span data-ttu-id="39cc9-134">Pokud chcete tootry vytváření sami a jeho nasazení tooAzure kontejner instancí pomocí hello registru kontejner Azure, pokračujte v kurzu toohello instancí kontejnerů Azure.</span><span class="sxs-lookup"><span data-stu-id="39cc9-134">If you'd like tootry building it yourself and deploying it tooAzure Container Instances using hello Azure Container Registry, continue toohello Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="39cc9-135">Kurzy služby Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="39cc9-135">Azure Container Instances tutorials</span></span>](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png