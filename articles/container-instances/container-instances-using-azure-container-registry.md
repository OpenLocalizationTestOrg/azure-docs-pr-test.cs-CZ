---
title: "aaaDeploy tooAzure kontejner instancí z hello registru kontejner Azure | Dokumentace Azure"
description: "Nasazování tooAzure kontejner instancí z hello registru kontejner Azure"
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
ms.openlocfilehash: 2667f91db8ed92a9ccc9ba722a2b1f5c5ea93886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-container-instances-from-hello-azure-container-registry"></a><span data-ttu-id="8df07-103">Nasazování tooAzure kontejner instancí z hello registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="8df07-103">Deploy tooAzure Container Instances from hello Azure Container Registry</span></span>

<span data-ttu-id="8df07-104">Hello registru kontejner Azure je založené na Azure, privátní registru, pro kontejner Docker bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="8df07-104">hello Azure Container Registry is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="8df07-105">Tento článek popisuje, jak Image kontejneru toodeploy uložené v registru kontejner Azure hello tooAzure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="8df07-105">This article covers how toodeploy container images stored in hello Azure Container Registry tooAzure Container Instances.</span></span>

## <a name="using-hello-azure-cli"></a><span data-ttu-id="8df07-106">Pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="8df07-106">Using hello Azure CLI</span></span>

<span data-ttu-id="8df07-107">Hello rozhraní příkazového řádku Azure obsahuje příkazy pro vytváření a Správa kontejnerů v Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="8df07-107">hello Azure CLI includes commands for creating and managing containers in Azure Container Instances.</span></span> <span data-ttu-id="8df07-108">Pokud zadáte privátní bitové kopie v hello `create` příkazů, můžete také určit hello image registru požadováno heslo tooauthenticate s hello kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="8df07-108">If you specify a private image in hello `create` command, you can also specify hello image registry password required tooauthenticate with hello container registry.</span></span>

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

<span data-ttu-id="8df07-109">Hello `create` příkaz také podporuje určení hello `registry-login-server` a `registry-username`.</span><span class="sxs-lookup"><span data-stu-id="8df07-109">hello `create` command also supports specifying hello `registry-login-server` and `registry-username`.</span></span> <span data-ttu-id="8df07-110">Ale hello přihlášení na server pro hello registru kontejner Azure je vždy *registryname*. azurecr.io a hello výchozí uživatelské jméno *registryname*, takže tyto hodnoty jsou odvodit z hello název bitové kopie, pokud k dispozici není explicitně.</span><span class="sxs-lookup"><span data-stu-id="8df07-110">However, hello login server for hello Azure Container Registry is always *registryname*.azurecr.io and hello default username is *registryname*, so these values are inferred from hello image name if not explicitly provided.</span></span>

## <a name="using-an-azure-resource-manager-template"></a><span data-ttu-id="8df07-111">Pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="8df07-111">Using an Azure Resource Manager template</span></span>

<span data-ttu-id="8df07-112">Můžete zadat vlastnosti hello registru kontejneru systému Azure v šablonu Azure Resource Manager včetně hello `imageRegistryCredentials` vlastnost v definici skupiny hello kontejneru:</span><span class="sxs-lookup"><span data-stu-id="8df07-112">You can specify hello properties of your Azure Container Registry in an Azure Resource Manager template by including hello `imageRegistryCredentials` property in hello container group definition:</span></span>

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

<span data-ttu-id="8df07-113">tooavoid ukládání heslo registru kontejneru přímo v šabloně hello, doporučujeme uložit jako tajný klíč v [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) a odkazovat v šabloně hello pomocí hello [nativní integrace mezi službou Hello Azure Resource Manageru a Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md).</span><span class="sxs-lookup"><span data-stu-id="8df07-113">tooavoid storing your container registry password directly in hello template, we recommend that you store it as a secret in [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) and reference it in hello template using hello [native integration between hello Azure Resource Manager and Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md).</span></span>

## <a name="using-hello-azure-portal"></a><span data-ttu-id="8df07-114">Pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8df07-114">Using hello Azure portal</span></span>

<span data-ttu-id="8df07-115">Pokud chcete zachovat kontejneru obrázků v hello registru kontejner Azure, můžete snadno vytvořit kontejner v Azure kontejner instancí pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8df07-115">If you maintain container images in hello Azure Container Registry, you can easily create a container in Azure Container Instances using hello Azure portal.</span></span>

1. <span data-ttu-id="8df07-116">V hello portálu Azure přejděte tooyour kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="8df07-116">In hello Azure portal, navigate tooyour container registry.</span></span>

2. <span data-ttu-id="8df07-117">Vyberte úložiště.</span><span class="sxs-lookup"><span data-stu-id="8df07-117">Choose Repositories.</span></span>

    ![nabídky Azure kontejneru registru Hello v hello portálu Azure][acr-menu]

3. <span data-ttu-id="8df07-119">Zvolte hello úložiště, který chcete toodeploy z.</span><span class="sxs-lookup"><span data-stu-id="8df07-119">Choose hello repository that you want toodeploy from.</span></span>

4. <span data-ttu-id="8df07-120">Klikněte pravým tlačítkem na hello značky pro bitovou kopii kontejneru hello chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="8df07-120">Right-click hello tag for hello container image you want toodeploy.</span></span>

    ![Kontextové nabídky pro spuštění kontejner s instancemi Azure kontejneru][acr-runinstance-contextmenu]

5. <span data-ttu-id="8df07-122">Zadejte název pro kontejner hello a název skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="8df07-122">Enter a name for hello container and a name for hello resource group.</span></span> <span data-ttu-id="8df07-123">Pokud chcete můžete také změnit hello výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8df07-123">You can also change hello default values if you wish.</span></span>

    ![Vytvořit nabídku pro instance kontejner Azure][acr-create-deeplink]

6. <span data-ttu-id="8df07-125">Po dokončení nasazení hello, můžete přejít skupina kontejneru toohello z hello oznámení podokně toofind jeho IP adresu a další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8df07-125">Once hello deployment completes, you can navigate toohello container group from hello notifications pane toofind its IP address and other properties.</span></span>

    ![Zobrazení podrobností pro skupinu kontejner instancí kontejnerů Azure][aci-detailsview]

## <a name="next-steps"></a><span data-ttu-id="8df07-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8df07-127">Next steps</span></span>

<span data-ttu-id="8df07-128">Zjistěte, jak toobuild kontejnery, vložit je tooa privátní kontejneru registru a jejich nasazení instance kontejneru tooAzure podle [dokončení kurzu hello](container-instances-tutorial-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="8df07-128">Learn how toobuild containers, push them tooa private container registry, and deploy them tooAzure Container Instances by [completing hello tutorial](container-instances-tutorial-prepare-app.md).</span></span>

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
