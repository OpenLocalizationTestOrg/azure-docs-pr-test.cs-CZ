---
title: "Vytvoření soukromého registru Dockeru – Azure Portal | Dokumentace Microsoftu"
description: "Začínáme vytvářet a spravovat soukromé registry kontejnerů Dockeru pomocí webu Azure Portal"
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
ms.openlocfilehash: 560aee42b0c5a61c37c594d7937f833ab7183d49
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="create-a-managed-container-registry-using-the-azure-portal"></a><span data-ttu-id="452af-103">Vytvoření spravovaného registru kontejnerů pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="452af-103">Create a managed container registry using the Azure portal</span></span>

<span data-ttu-id="452af-104">Azure Container Registry je spravovaná služba registru kontejnerů Dockeru sloužící k ukládání privátních imagí kontejnerů Dockeru.</span><span class="sxs-lookup"><span data-stu-id="452af-104">Azure Container Registry is a managed Docker container registry service used for storing private Docker container images.</span></span> <span data-ttu-id="452af-105">Tato příručka podrobně popisuje vytvoření spravované instance služby Azure Container Registry pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="452af-105">This guide details creating a managed Azure Container Registry instance using the Azure portal.</span></span>

<span data-ttu-id="452af-106">Spravované registry kontejnerů Azure jsou ve verzi Preview a nejsou dostupné ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="452af-106">Managed Azure container registries are in preview and not available in all regions.</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="452af-107">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="452af-107">Log in to Azure</span></span>

<span data-ttu-id="452af-108">Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="452af-108">Log in to the Azure portal at http://portal.azure.com.</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="452af-109">Vytvoření registru kontejnerů</span><span class="sxs-lookup"><span data-stu-id="452af-109">Create a container registry</span></span>

1. <span data-ttu-id="452af-110">Klikněte na tlačítko **Nový** v levém horním rohu portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="452af-110">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

2. <span data-ttu-id="452af-111">Na webu Marketplace vyhledejte a vyberte **Azure Container Registry**.</span><span class="sxs-lookup"><span data-stu-id="452af-111">Search the marketplace for **Azure container registry** and select it.</span></span>

3. <span data-ttu-id="452af-112">Klikněte na **Vytvořit**. Otevře se okno pro vytvoření služby ACR.</span><span class="sxs-lookup"><span data-stu-id="452af-112">Click **Create** which will open the ACR creation blade.</span></span>

    ![Nastavení registru kontejnerů](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. <span data-ttu-id="452af-114">V okně **Azure Container Registry** zadejte následující informace.</span><span class="sxs-lookup"><span data-stu-id="452af-114">In the **Azure Container Registry** blade, enter the following information.</span></span> <span data-ttu-id="452af-115">Až budete hotovi, klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="452af-115">Click **Create** when you are done.</span></span>

    <span data-ttu-id="452af-116">a.</span><span class="sxs-lookup"><span data-stu-id="452af-116">a.</span></span> <span data-ttu-id="452af-117">**Název registru:** Globálně jedinečný název domény nejvyšší úrovně konkrétního registru.</span><span class="sxs-lookup"><span data-stu-id="452af-117">**Registry name**: A globally unique top-level domain name for your specific registry.</span></span> <span data-ttu-id="452af-118">V tomto příkladu je název registru *myAzureContainerRegistry1*, ale nahraďte jej vlastním jedinečným názvem.</span><span class="sxs-lookup"><span data-stu-id="452af-118">In this example, the registry name is *myAzureContainerRegistry1*, but substitute a unique name of your own.</span></span> <span data-ttu-id="452af-119">Název může obsahovat pouze písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="452af-119">The name can contain only letters and numbers.</span></span>

    <span data-ttu-id="452af-120">b.</span><span class="sxs-lookup"><span data-stu-id="452af-120">b.</span></span> <span data-ttu-id="452af-121">**Skupina prostředků:** Vyberte existující [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo zadejte název nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="452af-121">**Resource group**: Select an existing [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) or type the name for a new one.</span></span>

    <span data-ttu-id="452af-122">c.</span><span class="sxs-lookup"><span data-stu-id="452af-122">c.</span></span> <span data-ttu-id="452af-123">**Umístění:** Vyberte umístění datového centra Azure, ve kterém je tato služba [dostupná](https://azure.microsoft.com/regions/services/), například **Střed USA – jih**.</span><span class="sxs-lookup"><span data-stu-id="452af-123">**Location**: Select an Azure datacenter location where the service is [available](https://azure.microsoft.com/regions/services/), such as **South Central US**.</span></span>

    <span data-ttu-id="452af-124">d.</span><span class="sxs-lookup"><span data-stu-id="452af-124">d.</span></span> <span data-ttu-id="452af-125">**Uživatel s právy pro správu:** Pokud chcete, povolte uživateli s právy pro správu přístup k registru.</span><span class="sxs-lookup"><span data-stu-id="452af-125">**Admin user**: If you want, enable an admin user to access the registry.</span></span> <span data-ttu-id="452af-126">Toto nastavení můžete po vytvoření registru změnit.</span><span class="sxs-lookup"><span data-stu-id="452af-126">You can change this setting after creating the registry.</span></span>

    <span data-ttu-id="452af-127">e.</span><span class="sxs-lookup"><span data-stu-id="452af-127">e.</span></span> <span data-ttu-id="452af-128">**Použít spravovaný registr:** Vyberte Ano, pokud chcete, aby služba ACR automaticky spravovala úložiště registru a používala webhooky a ověřování AAD.</span><span class="sxs-lookup"><span data-stu-id="452af-128">**Use managed registry**: Select yes to have ACR automatically manage the registry storage, use webhooks, and use AAD authentication.</span></span>

    <span data-ttu-id="452af-129">f.</span><span class="sxs-lookup"><span data-stu-id="452af-129">f.</span></span> <span data-ttu-id="452af-130">**Cenová úroveň:** Vyberte cenovou úroveň; další informace najdete tady v cenách služby ACR.</span><span class="sxs-lookup"><span data-stu-id="452af-130">**Pricing Tier**: Select a pricing tier, see here ACR pricing for more information.</span></span>

## <a name="log-in-to-acr-instance"></a><span data-ttu-id="452af-131">Přihlášení k instanci služby ACR</span><span class="sxs-lookup"><span data-stu-id="452af-131">Log in to ACR instance</span></span>

<span data-ttu-id="452af-132">Před odesíláním a vyžadováním imagí kontejnerů se musíte přihlásit k instanci služby ACR.</span><span class="sxs-lookup"><span data-stu-id="452af-132">Before pushing and pulling container images, you must log in to the ACR instance.</span></span> 

<span data-ttu-id="452af-133">Použijte k tomu Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="452af-133">To do so, use the Azure CLI 2.0.</span></span> <span data-ttu-id="452af-134">Nejprve se pomocí příkazu [az login](/cli/azure/#login) přihlaste do Azure, pokud je to potřeba.</span><span class="sxs-lookup"><span data-stu-id="452af-134">First, if needed, log into Azure using the [az login](/cli/azure/#login) command.</span></span> 

```azurecli
az login
```

<span data-ttu-id="452af-135">Potom se pomocí příkazu [az acr login](/cli/azure/acr#login) přihlaste ke službě Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="452af-135">Next, use the [az acr login](/cli/azure/acr#login) command to log in to the Azure Container Registry.</span></span>

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

## <a name="use-azure-container-registry"></a><span data-ttu-id="452af-136">Použití služby Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="452af-136">Use Azure Container Registry</span></span>

### <a name="list-container-images"></a><span data-ttu-id="452af-137">Výpis imagí kontejnerů</span><span class="sxs-lookup"><span data-stu-id="452af-137">List container images</span></span>

<span data-ttu-id="452af-138">Pomocí příkazů rozhraní příkazového řádku `az acr` se můžete dotazovat na obrázky a značky v úložišti.</span><span class="sxs-lookup"><span data-stu-id="452af-138">Use the `az acr` CLI commands to query the images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="452af-139">Container Registry v současné době nepodporuje použití příkazu `docker search` k dotazování obrázků a značek.</span><span class="sxs-lookup"><span data-stu-id="452af-139">Currently, Container Registry does not support the `docker search` command to query for images and tags.</span></span>

### <a name="list-repositories"></a><span data-ttu-id="452af-140">Výpis úložišť</span><span class="sxs-lookup"><span data-stu-id="452af-140">List repositories</span></span>

<span data-ttu-id="452af-141">Následující příklad zobrazí seznam úložišť v registru ve formátu JSON (JavaScript Object Notation):</span><span class="sxs-lookup"><span data-stu-id="452af-141">The following example lists the repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="452af-142">Výpis značek</span><span class="sxs-lookup"><span data-stu-id="452af-142">List tags</span></span>

<span data-ttu-id="452af-143">Následující příklad zobrazí seznam značek na úložišti **samples/nginx** ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="452af-143">The following example lists the tags on the **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="452af-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="452af-144">Next steps</span></span>

<span data-ttu-id="452af-145">V tomto rychlém startu jste vytvořili spravovanou instanci služby Azure Container Registry pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="452af-145">In this quick start, you've created a managed Azure Container Registry instance using the Azure portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="452af-146">Nahrání první image pomocí rozhraní příkazového řádku Dockeru</span><span class="sxs-lookup"><span data-stu-id="452af-146">Push your first image using the Docker CLI</span></span>](container-registry-get-started-docker-cli.md)