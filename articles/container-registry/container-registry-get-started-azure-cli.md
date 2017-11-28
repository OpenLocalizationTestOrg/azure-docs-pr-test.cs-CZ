---
title: "aaaCreate privátní registru kontejner Docker - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Začínáme vytváření a správě privátního registrech kontejner Docker s hello 2.0 rozhraní příkazového řádku Azure"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0d876a70b71a5e1bd564fbc9198f693dfe8a347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-cli-20"></a><span data-ttu-id="87a0b-103">Vytvořit privátní registru kontejner Docker pomocí hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="87a0b-103">Create a private Docker container registry using hello Azure CLI 2.0</span></span>
<span data-ttu-id="87a0b-104">Používání příkazů v hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) toocreate kontejneru registru a jeho nastavení z počítače systému Linux, Mac nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="87a0b-104">Use commands in hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) toocreate a container registry and manage its settings from your Linux, Mac, or Windows computer.</span></span> <span data-ttu-id="87a0b-105">Můžete také vytvořit a spravovat pomocí hello registrech kontejneru [portál Azure](container-registry-get-started-portal.md) nebo prostřednictvím kódu programu hello kontejneru registru [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span><span class="sxs-lookup"><span data-stu-id="87a0b-105">You can also create and manage container registries using hello [Azure portal](container-registry-get-started-portal.md) or programmatically with hello Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>


* <span data-ttu-id="87a0b-106">Pozadí a koncepty najdete v tématu [hello – přehled](container-registry-intro.md)</span><span class="sxs-lookup"><span data-stu-id="87a0b-106">For background and concepts, see [hello overview](container-registry-intro.md)</span></span>
* <span data-ttu-id="87a0b-107">Nápovědu k registru kontejneru rozhraní příkazového řádku (`az acr` příkazy), předejte hello `-h` parametr tooany příkaz.</span><span class="sxs-lookup"><span data-stu-id="87a0b-107">For help on Container Registry CLI commands (`az acr` commands), pass hello `-h` parameter tooany command.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="87a0b-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="87a0b-108">Prerequisites</span></span>
* <span data-ttu-id="87a0b-109">**Azure CLI 2.0**: tooinstall a začít pracovat s hello 2.0 rozhraní příkazového řádku najdete v tématu hello [pokyny k instalaci](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="87a0b-109">**Azure CLI 2.0**: tooinstall and get started with hello CLI 2.0, see hello [installation instructions](/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="87a0b-110">Přihlaste se spuštěním tooyour předplatného Azure `az login`.</span><span class="sxs-lookup"><span data-stu-id="87a0b-110">Log in tooyour Azure subscription by running `az login`.</span></span> <span data-ttu-id="87a0b-111">Další informace najdete v tématu [začít pracovat s hello 2.0 rozhraní příkazového řádku](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="87a0b-111">For more information, see [Get started with hello CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>
* <span data-ttu-id="87a0b-112">**Skupina prostředků:** Než vytvoříte registr kontejnerů, vytvořte [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo použijte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="87a0b-112">**Resource group**: Create a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) before creating a container registry, or use an existing resource group.</span></span> <span data-ttu-id="87a0b-113">Zkontrolujte, zda je skupina prostředků hello v umístění, kde je hello registru kontejneru služby [k dispozici](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="87a0b-113">Make sure hello resource group is in a location where hello Container Registry service is [available](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="87a0b-114">hello toocreate skupinu prostředků pomocí rozhraní příkazového řádku 2.0, najdete v části [hello referenční informace o rozhraní příkazového řádku 2.0](/cli/azure/group).</span><span class="sxs-lookup"><span data-stu-id="87a0b-114">toocreate a resource group using hello CLI 2.0, see [hello CLI 2.0 reference](/cli/azure/group).</span></span>
* <span data-ttu-id="87a0b-115">**Účet úložiště** (volitelné): vytvořte standardní Azure [účet úložiště](../storage/common/storage-introduction.md) tooback hello kontejneru registru hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="87a0b-115">**Storage account** (optional): Create a standard Azure [storage account](../storage/common/storage-introduction.md) tooback hello container registry in hello same location.</span></span> <span data-ttu-id="87a0b-116">Pokud nezadáte účet úložiště při vytváření registru s `az acr create`, hello příkaz vytvoří za vás.</span><span class="sxs-lookup"><span data-stu-id="87a0b-116">If you don't specify a storage account when creating a registry with `az acr create`, hello command creates one for you.</span></span> <span data-ttu-id="87a0b-117">účet úložiště pomocí toocreate hello 2.0 rozhraní příkazového řádku najdete v tématu [hello referenční informace o rozhraní příkazového řádku 2.0](/cli/azure/storage/account).</span><span class="sxs-lookup"><span data-stu-id="87a0b-117">toocreate a storage account using hello CLI 2.0, see [hello CLI 2.0 reference](/cli/azure/storage/account).</span></span> <span data-ttu-id="87a0b-118">Storage úrovně Premium se v tuto chvíli nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="87a0b-118">Currently Premium Storage is not supported.</span></span>
* <span data-ttu-id="87a0b-119">**Instanční objekt** (volitelné): Když vytvoříte soubor registru s hello rozhraní příkazového řádku, ve výchozím nastavení není nastaven pro přístup.</span><span class="sxs-lookup"><span data-stu-id="87a0b-119">**Service principal** (optional): When you create a registry with hello CLI, by default it is not set up for access.</span></span> <span data-ttu-id="87a0b-120">Podle potřeby můžete přiřadit existujícího registru hlavní tooa služby Azure Active Directory (nebo vytvořit a přiřadit nový), nebo povolte hello registru Správce uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="87a0b-120">Depending on your needs, you can assign an existing Azure Active Directory service principal tooa registry (or create and assign a new one), or enable hello registry's admin user account.</span></span> <span data-ttu-id="87a0b-121">Najdete v části hello později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="87a0b-121">See hello sections later in this article.</span></span> <span data-ttu-id="87a0b-122">Další informace o přístup k registru najdete v tématu [ověřit s hello kontejneru registru](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="87a0b-122">For more information about registry access, see [Authenticate with hello container registry](container-registry-authentication.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="87a0b-123">Vytvoření registru kontejnerů</span><span class="sxs-lookup"><span data-stu-id="87a0b-123">Create a container registry</span></span>
<span data-ttu-id="87a0b-124">Spustit hello `az acr create` příkaz toocreate registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="87a0b-124">Run hello `az acr create` command toocreate a container registry.</span></span>

> [!TIP]
> <span data-ttu-id="87a0b-125">Při vytváření registru zadejte globálně jedinečný název domény nejvyšší úrovně obsahující pouze písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="87a0b-125">When you create a registry, specify a globally unique top-level domain name, containing only letters and numbers.</span></span> <span data-ttu-id="87a0b-126">Název registru Hello v příkladech hello je `myRegistry1`, ale nahraďte vlastní jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="87a0b-126">hello registry name in hello examples is `myRegistry1`, but substitute a unique name of your own.</span></span>
>
>

<span data-ttu-id="87a0b-127">Dobrý den, následující příkaz používá hello minimální parametry toocreate kontejneru registru `myRegistry1` ve skupině prostředků hello `myResourceGroup`a pomocí hello *základní* sku:</span><span class="sxs-lookup"><span data-stu-id="87a0b-127">hello following command uses hello minimal parameters toocreate container registry `myRegistry1` in hello resource group `myResourceGroup`, and using hello *Basic* sku:</span></span>

```azurecli
az acr create --name myRegistry1 --resource-group myResourceGroup --sku Basic
```

* <span data-ttu-id="87a0b-128">Parametr `--storage-account-name` je volitelný.</span><span class="sxs-lookup"><span data-stu-id="87a0b-128">`--storage-account-name` is optional.</span></span> <span data-ttu-id="87a0b-129">Pokud není zadán, s názvem, který se skládá z název registru hello je vytvoření účtu úložiště a časovým razítkem v hello zadat skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="87a0b-129">If not specified, a storage account is created with a name consisting of hello registry name and a timestamp in hello specified resource group.</span></span>

<span data-ttu-id="87a0b-130">Při vytváření hello registru výstup hello je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="87a0b-130">When hello registry is created, hello output is similar toohello following:</span></span>

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T18:36:29.124842+00:00",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry
/registries/myRegistry1",
  "location": "southcentralus",
  "loginServer": "myregistry1.azurecr.io",
  "name": "myRegistry1",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myregistry123456789"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}

```


<span data-ttu-id="87a0b-131">Všimněte si zejména těchto hodnot:</span><span class="sxs-lookup"><span data-stu-id="87a0b-131">Take special note:</span></span>

* <span data-ttu-id="87a0b-132">`id`-Identifikátor hello registru v rámci vašeho předplatného, které budete potřebovat, pokud chcete, aby tooassign hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="87a0b-132">`id` - Identifier for hello registry in your subscription, which you need if you want tooassign a service principal.</span></span>
* <span data-ttu-id="87a0b-133">`loginServer`-hello plně kvalifikovaný název zadáte příliš[protokolu v registru toohello](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="87a0b-133">`loginServer` - hello fully qualified name you specify too[log in toohello registry](container-registry-authentication.md).</span></span> <span data-ttu-id="87a0b-134">V tomto příkladu je název hello `myregistry1.exp.azurecr.io` (všechny malá písmena).</span><span class="sxs-lookup"><span data-stu-id="87a0b-134">In this example, hello name is `myregistry1.exp.azurecr.io` (all lowercase).</span></span>

## <a name="assign-a-service-principal"></a><span data-ttu-id="87a0b-135">Přiřazení instančního objektu</span><span class="sxs-lookup"><span data-stu-id="87a0b-135">Assign a service principal</span></span>
<span data-ttu-id="87a0b-136">Pomocí rozhraní příkazového řádku 2.0 příkazy tooassign registru hlavní tooa služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="87a0b-136">Use CLI 2.0 commands tooassign an Azure Active Directory service principal tooa registry.</span></span> <span data-ttu-id="87a0b-137">Hello instančního objektu v těchto příkladech je přiřazena hello vlastníka role, ale můžete přiřadit [jiné role](../active-directory/role-based-access-control-configure.md) podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="87a0b-137">hello service principal in these examples is assigned hello Owner role, but you can assign [other roles](../active-directory/role-based-access-control-configure.md) if you want.</span></span>

### <a name="create-a-service-principal-and-assign-access-toohello-registry"></a><span data-ttu-id="87a0b-138">Vytvořit objekt služby a přiřadit přístup toohello registru</span><span class="sxs-lookup"><span data-stu-id="87a0b-138">Create a service principal and assign access toohello registry</span></span>
<span data-ttu-id="87a0b-139">V hello následující příkaz, je přiřazen nový instanční objekt vlastníka role přístup toohello registru identifikátor byla dokončena s hello `--scopes` parametr.</span><span class="sxs-lookup"><span data-stu-id="87a0b-139">In hello following command, a new service principal is assigned Owner role access toohello registry identifier passed with hello `--scopes` parameter.</span></span> <span data-ttu-id="87a0b-140">Zadejte silné heslo s hello `--password` parametr.</span><span class="sxs-lookup"><span data-stu-id="87a0b-140">Specify a strong password with hello `--password` parameter.</span></span>

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a><span data-ttu-id="87a0b-141">Přiřazení existujícího instančního objektu</span><span class="sxs-lookup"><span data-stu-id="87a0b-141">Assign an existing service principal</span></span>
<span data-ttu-id="87a0b-142">Pokud již máte hlavní název služby a chcete tooassign jeho vlastníka role přístup toohello registru, spusťte příkaz podobný toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="87a0b-142">If you already have a service principal and want tooassign it Owner role access toohello registry, run a command similar toohello following example.</span></span> <span data-ttu-id="87a0b-143">Předat ID hlavní aplikace hello služby pomocí hello `--assignee` parametr:</span><span class="sxs-lookup"><span data-stu-id="87a0b-143">You pass hello service principal app ID using hello `--assignee` parameter:</span></span>

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a><span data-ttu-id="87a0b-144">Správa přihlašovacích údajů správce</span><span class="sxs-lookup"><span data-stu-id="87a0b-144">Manage admin credentials</span></span>
<span data-ttu-id="87a0b-145">Účet správce se automaticky vytvoří pro každý registr kontejnerů a ve výchozím nastavení je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="87a0b-145">An admin account is automatically created for each container registry and is disabled by default.</span></span> <span data-ttu-id="87a0b-146">Následující příklady zobrazují Hello `az acr` toomanage hello pověření správce pro vaše kontejneru registru příkazy rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="87a0b-146">hello following examples show `az acr` CLI commands toomanage hello admin credentials for your container registry.</span></span>

### <a name="obtain-admin-user-credentials"></a><span data-ttu-id="87a0b-147">Získání přihlašovacích údajů uživatele s právy pro správu</span><span class="sxs-lookup"><span data-stu-id="87a0b-147">Obtain admin user credentials</span></span>
```azurecli
az acr credential show -n myRegistry1
```

### <a name="enable-admin-user-for-an-existing-registry"></a><span data-ttu-id="87a0b-148">Povolení uživatele s právy pro správu pro existující registr</span><span class="sxs-lookup"><span data-stu-id="87a0b-148">Enable admin user for an existing registry</span></span>
```azurecli
az acr update -n myRegistry1 --admin-enabled true
```

### <a name="disable-admin-user-for-an-existing-registry"></a><span data-ttu-id="87a0b-149">Zakázání uživatele s právy pro správu pro existující registr</span><span class="sxs-lookup"><span data-stu-id="87a0b-149">Disable admin user for an existing registry</span></span>
```azurecli
az acr update -n myRegistry1 --admin-enabled false
```

## <a name="list-images-and-tags"></a><span data-ttu-id="87a0b-150">Výpis obrázků a značek</span><span class="sxs-lookup"><span data-stu-id="87a0b-150">List images and tags</span></span>
<span data-ttu-id="87a0b-151">Použití hello `az acr` příkazy rozhraní příkazového řádku tooquery hello Image a značky v úložišti.</span><span class="sxs-lookup"><span data-stu-id="87a0b-151">Use hello `az acr` CLI commands tooquery hello images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="87a0b-152">Kontejner registru v současné době nepodporuje hello `docker search` tooquery příkaz pro bitové kopie a značky.</span><span class="sxs-lookup"><span data-stu-id="87a0b-152">Currently, Container Registry does not support hello `docker search` command tooquery for images and tags.</span></span>


### <a name="list-repositories"></a><span data-ttu-id="87a0b-153">Výpis úložišť</span><span class="sxs-lookup"><span data-stu-id="87a0b-153">List repositories</span></span>
<span data-ttu-id="87a0b-154">Hello následující příklad uvádí hello úložiště v registru, ve formátu JSON (JavaScript Object Notation):</span><span class="sxs-lookup"><span data-stu-id="87a0b-154">hello following example lists hello repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="87a0b-155">Výpis značek</span><span class="sxs-lookup"><span data-stu-id="87a0b-155">List tags</span></span>
<span data-ttu-id="87a0b-156">Hello následující příklad vypíše značky hello na hello **ukázky/nginx** úložiště, ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="87a0b-156">hello following example lists hello tags on hello **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="87a0b-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87a0b-157">Next steps</span></span>
* [<span data-ttu-id="87a0b-158">Push vaší první image pomocí příkazového řádku Dockeru hello</span><span class="sxs-lookup"><span data-stu-id="87a0b-158">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
