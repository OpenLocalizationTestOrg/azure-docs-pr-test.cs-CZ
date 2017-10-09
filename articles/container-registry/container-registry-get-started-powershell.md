---
title: "aaaAzure kontejneru registru úložiště | Microsoft Docs"
description: "Jak toouse registru Azure kontejner úložiště pro Docker bitové kopie"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: cristyg
ms.openlocfilehash: 448fb812f537c9502041ce5fb372b0681a9dac4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-powershell"></a><span data-ttu-id="002c4-103">Vytvořit privátní registru kontejner Docker pomocí hello prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="002c4-103">Create a private Docker container registry using hello Azure PowerShell</span></span>
<span data-ttu-id="002c4-104">Pomocí příkazů v [prostředí Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) toocreate registru kontejneru a spravovat nastavení z počítače se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="002c4-104">Use commands in [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) toocreate a container registry and manage its settings from your Windows computer.</span></span> <span data-ttu-id="002c4-105">Můžete také vytvořit a spravovat pomocí hello registrech kontejneru [portál Azure](container-registry-get-started-portal.md), hello [rozhraní příkazového řádku Azure](container-registry-get-started-azure-cli.md), nebo prostřednictvím kódu programu hello kontejneru registru [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span><span class="sxs-lookup"><span data-stu-id="002c4-105">You can also create and manage container registries using hello [Azure portal](container-registry-get-started-portal.md), hello [Azure CLI](container-registry-get-started-azure-cli.md), or programmatically with hello Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>


* <span data-ttu-id="002c4-106">Pozadí a koncepty najdete v tématu [hello – přehled](container-registry-intro.md)</span><span class="sxs-lookup"><span data-stu-id="002c4-106">For background and concepts, see [hello overview](container-registry-intro.md)</span></span>
* <span data-ttu-id="002c4-107">Úplný seznam podporovaných rutin najdete v tématu [rutiny správy registru kontejner Azure](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).</span><span class="sxs-lookup"><span data-stu-id="002c4-107">For a full list of supported cmdlets, see [Azure Container Registry Management Cmdlets](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="002c4-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="002c4-108">Prerequisites</span></span>
* <span data-ttu-id="002c4-109">**Prostředí Azure PowerShell**: tooinstall a začít pracovat s Azure PowerShell najdete v tématu hello [pokyny k instalaci](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="002c4-109">**Azure PowerShell**: tooinstall and get started with Azure PowerShell, see hello [installation instructions](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span> <span data-ttu-id="002c4-110">Přihlaste se spuštěním tooyour předplatného Azure `Login-AzureRMAccount`.</span><span class="sxs-lookup"><span data-stu-id="002c4-110">Log in tooyour Azure subscription by running `Login-AzureRMAccount`.</span></span> <span data-ttu-id="002c4-111">Další informace najdete v tématu [Začínáme s Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).</span><span class="sxs-lookup"><span data-stu-id="002c4-111">For more information, see [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).</span></span>
* <span data-ttu-id="002c4-112">**Skupina prostředků:** Než vytvoříte registr kontejnerů, vytvořte [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo použijte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="002c4-112">**Resource group**: Create a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) before creating a container registry, or use an existing resource group.</span></span> <span data-ttu-id="002c4-113">Zkontrolujte, zda je skupina prostředků hello v umístění, kde je hello registru kontejneru služby [k dispozici](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="002c4-113">Make sure hello resource group is in a location where hello Container Registry service is [available](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="002c4-114">toocreate skupiny prostředků pomocí Azure PowerShell, najdete v části [hello referenční informace prostředí PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).</span><span class="sxs-lookup"><span data-stu-id="002c4-114">toocreate a resource group using Azure PowerShell, see [hello PowerShell reference](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).</span></span>
* <span data-ttu-id="002c4-115">**Účet úložiště** (volitelné): vytvořte standardní Azure [účet úložiště](../storage/common/storage-introduction.md) tooback hello kontejneru registru hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="002c4-115">**Storage account** (optional): Create a standard Azure [storage account](../storage/common/storage-introduction.md) tooback hello container registry in hello same location.</span></span> <span data-ttu-id="002c4-116">Pokud nezadáte účet úložiště při vytváření registru s `New-AzureRMContainerRegistry`, hello příkaz vytvoří za vás.</span><span class="sxs-lookup"><span data-stu-id="002c4-116">If you don't specify a storage account when creating a registry with `New-AzureRMContainerRegistry`, hello command creates one for you.</span></span> <span data-ttu-id="002c4-117">toocreate úložiště účtu pomocí prostředí PowerShell najdete v tématu [hello referenční informace prostředí PowerShell](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount).</span><span class="sxs-lookup"><span data-stu-id="002c4-117">toocreate a storage account using PowerShell, see [hello PowerShell reference](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount).</span></span> <span data-ttu-id="002c4-118">Storage úrovně Premium se v tuto chvíli nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="002c4-118">Currently Premium Storage is not supported.</span></span>
* <span data-ttu-id="002c4-119">**Instanční objekt** (volitelné): Když vytvoříte soubor registru pomocí prostředí PowerShell, ve výchozím nastavení není nastaven pro přístup.</span><span class="sxs-lookup"><span data-stu-id="002c4-119">**Service principal** (optional): When you create a registry with PowerShell, by default it is not set up for access.</span></span> <span data-ttu-id="002c4-120">Podle potřeby můžete přiřadit existujícího registru hlavní tooa služby Azure Active Directory nebo vytvořte a přiřaďte nové.</span><span class="sxs-lookup"><span data-stu-id="002c4-120">Depending on your needs, you can assign an existing Azure Active Directory service principal tooa registry or create and assign a new one.</span></span> <span data-ttu-id="002c4-121">Alternativně můžete povolit hello registru Správce uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="002c4-121">Alternatively, you can enable hello registry's admin user account.</span></span> <span data-ttu-id="002c4-122">Najdete v části hello později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="002c4-122">See hello sections later in this article.</span></span> <span data-ttu-id="002c4-123">Další informace o přístup k registru najdete v tématu [ověřit s hello kontejneru registru](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="002c4-123">For more information about registry access, see [Authenticate with hello container registry](container-registry-authentication.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="002c4-124">Vytvoření registru kontejnerů</span><span class="sxs-lookup"><span data-stu-id="002c4-124">Create a container registry</span></span>
<span data-ttu-id="002c4-125">Spustit hello `New-AzureRMContainerRegistry` příkaz toocreate registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="002c4-125">Run hello `New-AzureRMContainerRegistry` command toocreate a container registry.</span></span>

> [!TIP]
> <span data-ttu-id="002c4-126">Při vytváření registru zadejte globálně jedinečný název domény nejvyšší úrovně obsahující pouze písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="002c4-126">When you create a registry, specify a globally unique top-level domain name, containing only letters and numbers.</span></span> <span data-ttu-id="002c4-127">Název registru Hello v příkladech hello je `MyRegistry`, ale nahraďte vlastní jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="002c4-127">hello registry name in hello examples is `MyRegistry`, but substitute a unique name of your own.</span></span>
>
>

<span data-ttu-id="002c4-128">Dobrý den, následující příkaz používá hello minimální parametry toocreate kontejneru registru `MyRegistry` ve skupině prostředků hello `MyResourceGroup` v hello jihu USA umístění:</span><span class="sxs-lookup"><span data-stu-id="002c4-128">hello following command uses hello minimal parameters toocreate container registry `MyRegistry` in hello resource group `MyResourceGroup` in hello South Central US location:</span></span>

```PowerShell
$Registry = New-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

* <span data-ttu-id="002c4-129">Parametr `-StorageAccountName` je volitelný.</span><span class="sxs-lookup"><span data-stu-id="002c4-129">`-StorageAccountName` is optional.</span></span> <span data-ttu-id="002c4-130">Pokud není zadán, s názvem, který se skládá z název registru hello je vytvoření účtu úložiště a časovým razítkem v hello zadat skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="002c4-130">If not specified, a storage account is created with a name consisting of hello registry name and a timestamp in hello specified resource group.</span></span>

## <a name="assign-a-service-principal"></a><span data-ttu-id="002c4-131">Přiřazení instančního objektu</span><span class="sxs-lookup"><span data-stu-id="002c4-131">Assign a service principal</span></span>
<span data-ttu-id="002c4-132">Použít tooassign příkazy prostředí PowerShell Azure Active Directory [instanční objekt](../azure-resource-manager/resource-group-authenticate-service-principal.md) tooa registru.</span><span class="sxs-lookup"><span data-stu-id="002c4-132">Use PowerShell commands tooassign an Azure Active Directory [service principal](../azure-resource-manager/resource-group-authenticate-service-principal.md) tooa registry.</span></span> <span data-ttu-id="002c4-133">Hello instančního objektu v těchto příkladech je přiřazena hello vlastníka role, ale můžete přiřadit [jiné role](../active-directory/role-based-access-control-configure.md) podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="002c4-133">hello service principal in these examples is assigned hello Owner role, but you can assign [other roles](../active-directory/role-based-access-control-configure.md) if you want.</span></span>

### <a name="create-a-service-principal"></a><span data-ttu-id="002c4-134">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="002c4-134">Create a service principal</span></span>
<span data-ttu-id="002c4-135">V hello následující příkaz je vytvořen nový instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="002c4-135">In hello following command, a new service principal is created.</span></span> <span data-ttu-id="002c4-136">Zadejte silné heslo s hello `-Password` parametr.</span><span class="sxs-lookup"><span data-stu-id="002c4-136">Specify a strong password with hello `-Password` parameter.</span></span>

```PowerShell
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName ApplicationDisplayName -Password "MyPassword"
```

### <a name="assign-a-new-or-existing-service-principal"></a><span data-ttu-id="002c4-137">Přiřadit nový nebo existující instanční objekt</span><span class="sxs-lookup"><span data-stu-id="002c4-137">Assign a new or existing service principal</span></span>
<span data-ttu-id="002c4-138">Můžete přiřadit nového nebo existujícího objektu tooa registru služby.</span><span class="sxs-lookup"><span data-stu-id="002c4-138">You can assign a new or an existing service principal tooa registry.</span></span> <span data-ttu-id="002c4-139">tooassign jeho vlastníka role přístup toohello registru, spusťte příkaz podobný toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="002c4-139">tooassign it Owner role access toohello registry, run a command similar toohello following example:</span></span>

```PowerShell
New-AzureRMRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Registry.Id
```

##<a name="sign-in-toohello-registry-with-hello-service-principal"></a><span data-ttu-id="002c4-140">Přihlaste se toohello registr s využitím hello instančního objektu</span><span class="sxs-lookup"><span data-stu-id="002c4-140">Sign in toohello registry with hello service principal</span></span>
<span data-ttu-id="002c4-141">Po přiřazení hello služby hlavní toohello registru, můžete přihlásit pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="002c4-141">After assigning hello service principal toohello registry, you can sign in using hello following command:</span></span>

```PowerShell
docker login -u $ServicePrincipal.ApplicationId -p myPassword
```

## <a name="manage-admin-credentials"></a><span data-ttu-id="002c4-142">Správa přihlašovacích údajů správce</span><span class="sxs-lookup"><span data-stu-id="002c4-142">Manage admin credentials</span></span>
<span data-ttu-id="002c4-143">Účet správce se automaticky vytvoří pro každý registr kontejnerů a ve výchozím nastavení je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="002c4-143">An admin account is automatically created for each container registry and is disabled by default.</span></span> <span data-ttu-id="002c4-144">Hello následující příklady ukazují příkazy prostředí PowerShell přihlašovací údaje správce hello toomanage pro vaše registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="002c4-144">hello following examples show PowerShell commands toomanage hello admin credentials for your container registry.</span></span>

### <a name="obtain-admin-user-credentials"></a><span data-ttu-id="002c4-145">Získání přihlašovacích údajů uživatele s právy pro správu</span><span class="sxs-lookup"><span data-stu-id="002c4-145">Obtain admin user credentials</span></span>
```PowerShell
Get-AzureRMContainerRegistryCredential -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

### <a name="enable-admin-user-for-an-existing-registry"></a><span data-ttu-id="002c4-146">Povolení uživatele s právy pro správu pro existující registr</span><span class="sxs-lookup"><span data-stu-id="002c4-146">Enable admin user for an existing registry</span></span>
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -EnableAdminUser
```

### <a name="disable-admin-user-for-an-existing-registry"></a><span data-ttu-id="002c4-147">Zakázání uživatele s právy pro správu pro existující registr</span><span class="sxs-lookup"><span data-stu-id="002c4-147">Disable admin user for an existing registry</span></span>
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -DisableAdminUser
```

## <a name="next-steps"></a><span data-ttu-id="002c4-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="002c4-148">Next steps</span></span>
* [<span data-ttu-id="002c4-149">Push vaší první image pomocí příkazového řádku Dockeru hello</span><span class="sxs-lookup"><span data-stu-id="002c4-149">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
