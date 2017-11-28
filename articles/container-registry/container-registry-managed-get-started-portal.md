---
title: "aaaCreate privátní registru Docker - portálu Azure | Microsoft Docs"
description: "Začínáme vytváření a správě privátního registrech kontejner Docker s hello portálu Azure"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.openlocfilehash: cf3ce0dcf3036d0e9cd1eaf01721deccb00248d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-container-registry-using-hello-azure-portal"></a><span data-ttu-id="b3b37-103">Vytvoření spravované kontejneru registru pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b3b37-103">Create a managed container registry using hello Azure portal</span></span>

<span data-ttu-id="b3b37-104">Azure Container Registry je spravovaná služba registru kontejnerů Dockeru sloužící k ukládání privátních imagí kontejnerů Dockeru.</span><span class="sxs-lookup"><span data-stu-id="b3b37-104">Azure Container Registry is a managed Docker container registry service used for storing private Docker container images.</span></span> <span data-ttu-id="b3b37-105">Tato příručka podrobně popisuje vytváření spravované pomocí instance registru kontejner Azure hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b3b37-105">This guide details creating a managed Azure Container Registry instance using hello Azure portal.</span></span>

<span data-ttu-id="b3b37-106">Spravované registry kontejnerů Azure jsou ve verzi Preview a nejsou dostupné ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="b3b37-106">Managed Azure container registries are in preview and not available in all regions.</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="b3b37-107">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="b3b37-107">Log in tooAzure</span></span>

<span data-ttu-id="b3b37-108">Přihlaste se toohello portál Azure na http://portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="b3b37-108">Log in toohello Azure portal at http://portal.azure.com.</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="b3b37-109">Vytvoření registru kontejnerů</span><span class="sxs-lookup"><span data-stu-id="b3b37-109">Create a container registry</span></span>

1. <span data-ttu-id="b3b37-110">Klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b3b37-110">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

2. <span data-ttu-id="b3b37-111">Hledání hello marketplace pro **kontejner Azure registru** a vyberte ho.</span><span class="sxs-lookup"><span data-stu-id="b3b37-111">Search hello marketplace for **Azure container registry** and select it.</span></span>

3. <span data-ttu-id="b3b37-112">Klikněte na tlačítko **vytvořit** které se otevře okno pro vytvoření ACR hello.</span><span class="sxs-lookup"><span data-stu-id="b3b37-112">Click **Create** which will open hello ACR creation blade.</span></span>

    ![Nastavení registru kontejnerů](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. <span data-ttu-id="b3b37-114">V hello **registru kontejner Azure** okno, zadejte následující informace hello.</span><span class="sxs-lookup"><span data-stu-id="b3b37-114">In hello **Azure Container Registry** blade, enter hello following information.</span></span> <span data-ttu-id="b3b37-115">Až budete hotovi, klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b3b37-115">Click **Create** when you are done.</span></span>

    <span data-ttu-id="b3b37-116">a.</span><span class="sxs-lookup"><span data-stu-id="b3b37-116">a.</span></span> <span data-ttu-id="b3b37-117">**Název registru:** Globálně jedinečný název domény nejvyšší úrovně konkrétního registru.</span><span class="sxs-lookup"><span data-stu-id="b3b37-117">**Registry name**: A globally unique top-level domain name for your specific registry.</span></span> <span data-ttu-id="b3b37-118">V tomto příkladu je název registru hello *myAzureContainerRegistry1*, ale nahraďte vlastní jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="b3b37-118">In this example, hello registry name is *myAzureContainerRegistry1*, but substitute a unique name of your own.</span></span> <span data-ttu-id="b3b37-119">Hello název může obsahovat jenom písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="b3b37-119">hello name can contain only letters and numbers.</span></span>

    <span data-ttu-id="b3b37-120">b.</span><span class="sxs-lookup"><span data-stu-id="b3b37-120">b.</span></span> <span data-ttu-id="b3b37-121">**Skupina prostředků**: Vyberte existující [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo název typu hello nové.</span><span class="sxs-lookup"><span data-stu-id="b3b37-121">**Resource group**: Select an existing [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span>

    <span data-ttu-id="b3b37-122">c.</span><span class="sxs-lookup"><span data-stu-id="b3b37-122">c.</span></span> <span data-ttu-id="b3b37-123">**Umístění**: Vyberte datové centrum Azure umístění, kde je služba hello [k dispozici](https://azure.microsoft.com/regions/services/), jako například **jihu USA**.</span><span class="sxs-lookup"><span data-stu-id="b3b37-123">**Location**: Select an Azure datacenter location where hello service is [available](https://azure.microsoft.com/regions/services/), such as **South Central US**.</span></span>

    <span data-ttu-id="b3b37-124">d.</span><span class="sxs-lookup"><span data-stu-id="b3b37-124">d.</span></span> <span data-ttu-id="b3b37-125">**Uživatel s oprávněními správce**: Pokud chcete, povolení registru hello tooaccess uživatele správce.</span><span class="sxs-lookup"><span data-stu-id="b3b37-125">**Admin user**: If you want, enable an admin user tooaccess hello registry.</span></span> <span data-ttu-id="b3b37-126">Toto nastavení po vytvoření hello registru můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="b3b37-126">You can change this setting after creating hello registry.</span></span>

    <span data-ttu-id="b3b37-127">e.</span><span class="sxs-lookup"><span data-stu-id="b3b37-127">e.</span></span> <span data-ttu-id="b3b37-128">**Použití spravovaných registru**: klepněte na tlačítko Ano toohave ACR automaticky spravovat úložiště hello registru, pomocí webhooků a použít ověřování AAD.</span><span class="sxs-lookup"><span data-stu-id="b3b37-128">**Use managed registry**: Select yes toohave ACR automatically manage hello registry storage, use webhooks, and use AAD authentication.</span></span>

    <span data-ttu-id="b3b37-129">f.</span><span class="sxs-lookup"><span data-stu-id="b3b37-129">f.</span></span> <span data-ttu-id="b3b37-130">**Cenová úroveň:** Vyberte cenovou úroveň; další informace najdete tady v cenách služby ACR.</span><span class="sxs-lookup"><span data-stu-id="b3b37-130">**Pricing Tier**: Select a pricing tier, see here ACR pricing for more information.</span></span>

## <a name="log-in-tooacr-instance"></a><span data-ttu-id="b3b37-131">Přihlaste se tooACR instance</span><span class="sxs-lookup"><span data-stu-id="b3b37-131">Log in tooACR instance</span></span>

<span data-ttu-id="b3b37-132">Před vkládání a stahování kontejneru bitové kopie, musíte být přihlášení toohello ACR instance.</span><span class="sxs-lookup"><span data-stu-id="b3b37-132">Before pushing and pulling container images, you must log in toohello ACR instance.</span></span> 

<span data-ttu-id="b3b37-133">toodo Ano, použít hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b3b37-133">toodo so, use hello Azure CLI 2.0.</span></span> <span data-ttu-id="b3b37-134">Nejprve v případě potřeby, přihlaste se k Azure pomocí hello [az přihlášení](/cli/azure/#login) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b3b37-134">First, if needed, log into Azure using hello [az login](/cli/azure/#login) command.</span></span> 

```azurecli
az login
```

<span data-ttu-id="b3b37-135">Pak pomocí hello [az acr přihlášení](/cli/azure/acr#login) příkaz toolog v toohello registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="b3b37-135">Next, use hello [az acr login](/cli/azure/acr#login) command toolog in toohello Azure Container Registry.</span></span>

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

## <a name="use-azure-container-registry"></a><span data-ttu-id="b3b37-136">Použití služby Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="b3b37-136">Use Azure Container Registry</span></span>

### <a name="list-container-images"></a><span data-ttu-id="b3b37-137">Výpis imagí kontejnerů</span><span class="sxs-lookup"><span data-stu-id="b3b37-137">List container images</span></span>

<span data-ttu-id="b3b37-138">Použití hello `az acr` příkazy rozhraní příkazového řádku tooquery hello Image a značky v úložišti.</span><span class="sxs-lookup"><span data-stu-id="b3b37-138">Use hello `az acr` CLI commands tooquery hello images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="b3b37-139">Kontejner registru v současné době nepodporuje hello `docker search` tooquery příkaz pro bitové kopie a značky.</span><span class="sxs-lookup"><span data-stu-id="b3b37-139">Currently, Container Registry does not support hello `docker search` command tooquery for images and tags.</span></span>

### <a name="list-repositories"></a><span data-ttu-id="b3b37-140">Výpis úložišť</span><span class="sxs-lookup"><span data-stu-id="b3b37-140">List repositories</span></span>

<span data-ttu-id="b3b37-141">Hello následující příklad uvádí hello úložiště v registru, ve formátu JSON (JavaScript Object Notation):</span><span class="sxs-lookup"><span data-stu-id="b3b37-141">hello following example lists hello repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="b3b37-142">Výpis značek</span><span class="sxs-lookup"><span data-stu-id="b3b37-142">List tags</span></span>

<span data-ttu-id="b3b37-143">Hello následující příklad vypíše značky hello na hello **ukázky/nginx** úložiště, ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="b3b37-143">hello following example lists hello tags on hello **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="b3b37-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3b37-144">Next steps</span></span>

<span data-ttu-id="b3b37-145">V této úvodní jste vytvořili spravované instance registru kontejner Azure pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b3b37-145">In this quick start, you've created a managed Azure Container Registry instance using hello Azure portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b3b37-146">Push vaší první image pomocí příkazového řádku Dockeru hello</span><span class="sxs-lookup"><span data-stu-id="b3b37-146">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
