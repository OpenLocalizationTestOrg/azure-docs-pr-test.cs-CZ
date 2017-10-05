---
title: "Nasazení do Azure kontejner instancí z registru kontejner Azure | Dokumentace Azure"
description: "Nasazení do Azure kontejner instancí z registru kontejner Azure"
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
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: aa1c4ea379c10dff246e2f924a345f9fa444aa64
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a><span data-ttu-id="7137c-103">Nasazení do Azure kontejner instancí z registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="7137c-103">Deploy to Azure Container Instances from the Azure Container Registry</span></span>

<span data-ttu-id="7137c-104">Registr kontejner Azure je založené na Azure, privátní registru, pro kontejner Docker bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="7137c-104">The Azure Container Registry is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="7137c-105">Tento článek vysvětluje postup nasazení bitových kopií kontejneru uložené v registru kontejner Azure do Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="7137c-105">This article covers how to deploy container images stored in the Azure Container Registry to Azure Container Instances.</span></span>

## <a name="using-the-azure-cli"></a><span data-ttu-id="7137c-106">Použití Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7137c-106">Using the Azure CLI</span></span>

<span data-ttu-id="7137c-107">Rozhraní příkazového řádku Azure obsahuje příkazy pro vytváření a Správa kontejnerů v Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="7137c-107">The Azure CLI includes commands for creating and managing containers in Azure Container Instances.</span></span> <span data-ttu-id="7137c-108">Pokud zadáte privátní bitové kopie v `create` příkazu, můžete také zadat heslo registru image vyžadovaný k ověření s registrem kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7137c-108">If you specify a private image in the `create` command, you can also specify the image registry password required to authenticate with the container registry.</span></span>

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

<span data-ttu-id="7137c-109">`create` Příkaz také podporuje určení `registry-login-server` a `registry-username`.</span><span class="sxs-lookup"><span data-stu-id="7137c-109">The `create` command also supports specifying the `registry-login-server` and `registry-username`.</span></span> <span data-ttu-id="7137c-110">Však server přihlášení pro registru kontejner Azure, je vždycky *registryname*. azurecr.io a výchozí uživatelské jméno je *registryname*, takže pokud není, jsou tyto hodnoty odvodit z název bitové kopie explicitně zadat.</span><span class="sxs-lookup"><span data-stu-id="7137c-110">However, the login server for the Azure Container Registry is always *registryname*.azurecr.io and the default username is *registryname*, so these values are inferred from the image name if not explicitly provided.</span></span>

## <a name="using-an-azure-resource-manager-template"></a><span data-ttu-id="7137c-111">Pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7137c-111">Using an Azure Resource Manager template</span></span>

<span data-ttu-id="7137c-112">Můžete zadat vlastnosti registr kontejner Azure v šablonu Azure Resource Manager včetně `imageRegistryCredentials` vlastnost v definici skupiny kontejneru:</span><span class="sxs-lookup"><span data-stu-id="7137c-112">You can specify the properties of your Azure Container Registry in an Azure Resource Manager template by including the `imageRegistryCredentials` property in the container group definition:</span></span>

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

<span data-ttu-id="7137c-113">Abyste se vyhnuli ukládání heslo registru kontejneru přímo v šabloně, doporučujeme uložit jako tajný klíč v [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) a odkazovat pomocí šablony [nativní integrace mezi službou Azure Resource Manager a trezoru klíčů](../azure-resource-manager/resource-manager-keyvault-parameter.md).</span><span class="sxs-lookup"><span data-stu-id="7137c-113">To avoid storing your container registry password directly in the template, we recommend that you store it as a secret in [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) and reference it in the template using the [native integration between the Azure Resource Manager and Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md).</span></span>

## <a name="using-the-azure-portal"></a><span data-ttu-id="7137c-114">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7137c-114">Using the Azure portal</span></span>

<span data-ttu-id="7137c-115">Pokud chcete zachovat kontejneru bitové kopie v registru kontejner Azure, můžete snadno vytvořit kontejner v instancí kontejneru Azure pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7137c-115">If you maintain container images in the Azure Container Registry, you can easily create a container in Azure Container Instances using the Azure portal.</span></span>

1. <span data-ttu-id="7137c-116">Na portálu Azure přejděte do kontejneru registr.</span><span class="sxs-lookup"><span data-stu-id="7137c-116">In the Azure portal, navigate to your container registry.</span></span>

2. <span data-ttu-id="7137c-117">Vyberte úložiště.</span><span class="sxs-lookup"><span data-stu-id="7137c-117">Choose Repositories.</span></span>

    ![V nabídce registru kontejner Azure na portálu Azure][acr-menu]

3. <span data-ttu-id="7137c-119">Vyberte, kterou chcete nasadit z úložiště.</span><span class="sxs-lookup"><span data-stu-id="7137c-119">Choose the repository that you want to deploy from.</span></span>

4. <span data-ttu-id="7137c-120">Klikněte pravým tlačítkem na značce pro kontejner bitovou kopii, kterou chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="7137c-120">Right-click the tag for the container image you want to deploy.</span></span>

    ![Kontextové nabídky pro spuštění kontejner s instancemi Azure kontejneru][acr-runinstance-contextmenu]

5. <span data-ttu-id="7137c-122">Zadejte název kontejneru a název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7137c-122">Enter a name for the container and a name for the resource group.</span></span> <span data-ttu-id="7137c-123">Pokud chcete můžete taky změnit výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7137c-123">You can also change the default values if you wish.</span></span>

    ![Vytvořit nabídku pro instance kontejner Azure][acr-create-deeplink]

6. <span data-ttu-id="7137c-125">Po dokončení nasazení můžete přejít do kontejneru skupiny z podokna oznámení najít IP adresu a další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7137c-125">Once the deployment completes, you can navigate to the container group from the notifications pane to find its IP address and other properties.</span></span>

    ![Zobrazení podrobností pro skupinu kontejner instancí kontejnerů Azure][aci-detailsview]

## <a name="next-steps"></a><span data-ttu-id="7137c-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7137c-127">Next steps</span></span>

<span data-ttu-id="7137c-128">Naučte se vytvářet kontejnery, vložit je privátní kontejneru registru a jejich nasazení do instance kontejner Azure podle [dokončení tohoto kurzu](container-instances-tutorial-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="7137c-128">Learn how to build containers, push them to a private container registry, and deploy them to Azure Container Instances by [completing the tutorial](container-instances-tutorial-prepare-app.md).</span></span>

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
