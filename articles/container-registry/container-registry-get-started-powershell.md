---
title: "Kontejner Azure registru úložiště | Microsoft Docs"
description: "Jak používat Azure registru kontejner úložiště pro imagí Dockeru"
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
ms.openlocfilehash: 1e5d5ea5b1ec121fe008abc48178b1d58f540ce1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-private-docker-container-registry-using-the-azure-powershell"></a><span data-ttu-id="1e8e0-103">Vytvořit privátní registru kontejner Docker pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1e8e0-103">Create a private Docker container registry using the Azure PowerShell</span></span>
<span data-ttu-id="1e8e0-104">Pomocí příkazů v [prostředí Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) vytvoření kontejneru registru a jeho nastavení z počítače se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-104">Use commands in [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) to create a container registry and manage its settings from your Windows computer.</span></span> <span data-ttu-id="1e8e0-105">Můžete také vytvořit a spravovat pomocí registrech kontejneru [portál Azure](container-registry-get-started-portal.md), [rozhraní příkazového řádku Azure](container-registry-get-started-azure-cli.md), nebo prostřednictvím kódu programu s registrem kontejneru [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span><span class="sxs-lookup"><span data-stu-id="1e8e0-105">You can also create and manage container registries using the [Azure portal](container-registry-get-started-portal.md), the [Azure CLI](container-registry-get-started-azure-cli.md), or programmatically with the Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>


* <span data-ttu-id="1e8e0-106">Související informace a koncepty najdete v tématu [s přehledem](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="1e8e0-106">For background and concepts, see [the overview](container-registry-intro.md)</span></span>
* <span data-ttu-id="1e8e0-107">Úplný seznam podporovaných rutin najdete v tématu [rutiny správy registru kontejner Azure](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).</span><span class="sxs-lookup"><span data-stu-id="1e8e0-107">For a full list of supported cmdlets, see [Azure Container Registry Management Cmdlets](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="1e8e0-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1e8e0-108">Prerequisites</span></span>
* <span data-ttu-id="1e8e0-109">**Prostředí Azure PowerShell**: instalace a Začínáme s Azure PowerShell najdete v tématu [pokyny k instalaci](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="1e8e0-109">**Azure PowerShell**: To install and get started with Azure PowerShell, see the [installation instructions](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span> <span data-ttu-id="1e8e0-110">Přihlaste se ke svému předplatnému Azure spuštěním příkazu `Login-AzureRMAccount`.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-110">Log in to your Azure subscription by running `Login-AzureRMAccount`.</span></span> <span data-ttu-id="1e8e0-111">Další informace najdete v tématu [Začínáme s Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).</span><span class="sxs-lookup"><span data-stu-id="1e8e0-111">For more information, see [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).</span></span>
* <span data-ttu-id="1e8e0-112">**Skupina prostředků:** Než vytvoříte registr kontejnerů, vytvořte [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo použijte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-112">**Resource group**: Create a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) before creating a container registry, or use an existing resource group.</span></span> <span data-ttu-id="1e8e0-113">Ujistěte se, že je skupina prostředků v umístění, kde je služba Container Registry [dostupná](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="1e8e0-113">Make sure the resource group is in a location where the Container Registry service is [available](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="1e8e0-114">Vytvoření skupiny prostředků pomocí Azure Powershellu naleznete v tématu [referenční informace prostředí PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).</span><span class="sxs-lookup"><span data-stu-id="1e8e0-114">To create a resource group using Azure PowerShell, see [the PowerShell reference](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).</span></span>
* <span data-ttu-id="1e8e0-115">**Účet úložiště** (volitelné): Pro účely zálohování registru kontejnerů vytvořte standardní [účet úložiště](../storage/common/storage-introduction.md) Azure ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-115">**Storage account** (optional): Create a standard Azure [storage account](../storage/common/storage-introduction.md) to back the container registry in the same location.</span></span> <span data-ttu-id="1e8e0-116">Pokud při vytváření registru pomocí příkazu `New-AzureRMContainerRegistry` nezadáte účet úložiště, příkaz ho vytvoří za vás.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-116">If you don't specify a storage account when creating a registry with `New-AzureRMContainerRegistry`, the command creates one for you.</span></span> <span data-ttu-id="1e8e0-117">Pokud chcete vytvořit účet úložiště pomocí prostředí PowerShell, najdete v části [referenční informace prostředí PowerShell](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount).</span><span class="sxs-lookup"><span data-stu-id="1e8e0-117">To create a storage account using PowerShell, see [the PowerShell reference](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount).</span></span> <span data-ttu-id="1e8e0-118">Storage úrovně Premium se v tuto chvíli nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-118">Currently Premium Storage is not supported.</span></span>
* <span data-ttu-id="1e8e0-119">**Instanční objekt** (volitelné): Když vytvoříte soubor registru pomocí prostředí PowerShell, ve výchozím nastavení není nastaven pro přístup.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-119">**Service principal** (optional): When you create a registry with PowerShell, by default it is not set up for access.</span></span> <span data-ttu-id="1e8e0-120">Podle potřeby můžete přiřadit existující objekt služby Azure Active Directory do registru nebo vytvořit a přiřadit nový.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-120">Depending on your needs, you can assign an existing Azure Active Directory service principal to a registry or create and assign a new one.</span></span> <span data-ttu-id="1e8e0-121">Alternativně můžete povolit v registru Správce uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-121">Alternatively, you can enable the registry's admin user account.</span></span> <span data-ttu-id="1e8e0-122">Pokyny najdete v dalších částech tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-122">See the sections later in this article.</span></span> <span data-ttu-id="1e8e0-123">Další informace o přístupu k registru najdete v tématu [Ověřování pomocí registru kontejnerů](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="1e8e0-123">For more information about registry access, see [Authenticate with the container registry](container-registry-authentication.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="1e8e0-124">Vytvoření registru kontejnerů</span><span class="sxs-lookup"><span data-stu-id="1e8e0-124">Create a container registry</span></span>
<span data-ttu-id="1e8e0-125">Spuštěním příkazu `New-AzureRMContainerRegistry` vytvořte registr kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-125">Run the `New-AzureRMContainerRegistry` command to create a container registry.</span></span>

> [!TIP]
> <span data-ttu-id="1e8e0-126">Při vytváření registru zadejte globálně jedinečný název domény nejvyšší úrovně obsahující pouze písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-126">When you create a registry, specify a globally unique top-level domain name, containing only letters and numbers.</span></span> <span data-ttu-id="1e8e0-127">V tomto příkladu je název registru `MyRegistry`, ale nahraďte jej vlastním jedinečným názvem.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-127">The registry name in the examples is `MyRegistry`, but substitute a unique name of your own.</span></span>
>
>

<span data-ttu-id="1e8e0-128">Následující příkaz pomocí minimálního počtu parametrů vytvoří registr kontejnerů `MyRegistry` ve skupině prostředků `MyResourceGroup` v umístění Střed USA – jih:</span><span class="sxs-lookup"><span data-stu-id="1e8e0-128">The following command uses the minimal parameters to create container registry `MyRegistry` in the resource group `MyResourceGroup` in the South Central US location:</span></span>

```PowerShell
$Registry = New-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

* <span data-ttu-id="1e8e0-129">Parametr `-StorageAccountName` je volitelný.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-129">`-StorageAccountName` is optional.</span></span> <span data-ttu-id="1e8e0-130">Pokud není zadán, vytvoří se v zadané skupině prostředků účet úložiště s názvem, který se skládá z názvu registru a časového razítka.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-130">If not specified, a storage account is created with a name consisting of the registry name and a timestamp in the specified resource group.</span></span>

## <a name="assign-a-service-principal"></a><span data-ttu-id="1e8e0-131">Přiřazení instančního objektu</span><span class="sxs-lookup"><span data-stu-id="1e8e0-131">Assign a service principal</span></span>
<span data-ttu-id="1e8e0-132">Použít příkazy prostředí PowerShell přiřadit Azure Active Directory [instanční objekt](../azure-resource-manager/resource-group-authenticate-service-principal.md) do registru.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-132">Use PowerShell commands to assign an Azure Active Directory [service principal](../azure-resource-manager/resource-group-authenticate-service-principal.md) to a registry.</span></span> <span data-ttu-id="1e8e0-133">Instančnímu objektu v těchto příkladech je přiřazena role vlastníka, ale pokud chcete, můžete mu přiřadit [jiné role](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="1e8e0-133">The service principal in these examples is assigned the Owner role, but you can assign [other roles](../active-directory/role-based-access-control-configure.md) if you want.</span></span>

### <a name="create-a-service-principal"></a><span data-ttu-id="1e8e0-134">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="1e8e0-134">Create a service principal</span></span>
<span data-ttu-id="1e8e0-135">V následující příkaz je vytvořen nový instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-135">In the following command, a new service principal is created.</span></span> <span data-ttu-id="1e8e0-136">V parametru `-Password` zadejte silné heslo.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-136">Specify a strong password with the `-Password` parameter.</span></span>

```PowerShell
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName ApplicationDisplayName -Password "MyPassword"
```

### <a name="assign-a-new-or-existing-service-principal"></a><span data-ttu-id="1e8e0-137">Přiřadit nový nebo existující instanční objekt</span><span class="sxs-lookup"><span data-stu-id="1e8e0-137">Assign a new or existing service principal</span></span>
<span data-ttu-id="1e8e0-138">Nový nebo existující službu objektu zabezpečení můžete přiřadit k registru.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-138">You can assign a new or an existing service principal to a registry.</span></span> <span data-ttu-id="1e8e0-139">Pokud chcete přiřadit vlastníka role přístup k registru, spusťte příkaz podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1e8e0-139">To assign it Owner role access to the registry, run a command similar to the following example:</span></span>

```PowerShell
New-AzureRMRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Registry.Id
```

##<a name="sign-in-to-the-registry-with-the-service-principal"></a><span data-ttu-id="1e8e0-140">Přihlaste se k registru s hlavní službou</span><span class="sxs-lookup"><span data-stu-id="1e8e0-140">Sign in to the registry with the service principal</span></span>
<span data-ttu-id="1e8e0-141">Po přiřazení objektu služby v registru, můžete přihlásit pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="1e8e0-141">After assigning the service principal to the registry, you can sign in using the following command:</span></span>

```PowerShell
docker login -u $ServicePrincipal.ApplicationId -p myPassword
```

## <a name="manage-admin-credentials"></a><span data-ttu-id="1e8e0-142">Správa přihlašovacích údajů správce</span><span class="sxs-lookup"><span data-stu-id="1e8e0-142">Manage admin credentials</span></span>
<span data-ttu-id="1e8e0-143">Účet správce se automaticky vytvoří pro každý registr kontejnerů a ve výchozím nastavení je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-143">An admin account is automatically created for each container registry and is disabled by default.</span></span> <span data-ttu-id="1e8e0-144">Následující příklady ukazují příkazy prostředí PowerShell ke správě přihlašovacích údajů správce pro vašeho kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="1e8e0-144">The following examples show PowerShell commands to manage the admin credentials for your container registry.</span></span>

### <a name="obtain-admin-user-credentials"></a><span data-ttu-id="1e8e0-145">Získání přihlašovacích údajů uživatele s právy pro správu</span><span class="sxs-lookup"><span data-stu-id="1e8e0-145">Obtain admin user credentials</span></span>
```PowerShell
Get-AzureRMContainerRegistryCredential -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

### <a name="enable-admin-user-for-an-existing-registry"></a><span data-ttu-id="1e8e0-146">Povolení uživatele s právy pro správu pro existující registr</span><span class="sxs-lookup"><span data-stu-id="1e8e0-146">Enable admin user for an existing registry</span></span>
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -EnableAdminUser
```

### <a name="disable-admin-user-for-an-existing-registry"></a><span data-ttu-id="1e8e0-147">Zakázání uživatele s právy pro správu pro existující registr</span><span class="sxs-lookup"><span data-stu-id="1e8e0-147">Disable admin user for an existing registry</span></span>
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -DisableAdminUser
```

## <a name="next-steps"></a><span data-ttu-id="1e8e0-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e8e0-148">Next steps</span></span>
* [<span data-ttu-id="1e8e0-149">Nahrání první image pomocí rozhraní příkazového řádku Dockeru</span><span class="sxs-lookup"><span data-stu-id="1e8e0-149">Push your first image using the Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
