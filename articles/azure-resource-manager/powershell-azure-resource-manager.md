---
title: "aaaManage Azure řešení pomocí prostředí PowerShell | Microsoft Docs"
description: "Pomocí prostředí Azure PowerShell a správce prostředků toomanage vašich prostředků."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 47a91af8d7eb59585bcfd43571ce76b90a0d7971
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a><span data-ttu-id="fcadf-103">Správa prostředků pomocí Azure PowerShell a správce prostředků</span><span class="sxs-lookup"><span data-stu-id="fcadf-103">Manage resources with Azure PowerShell and Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fcadf-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fcadf-104">Portal</span></span>](resource-group-portal.md)
> * [<span data-ttu-id="fcadf-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fcadf-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="fcadf-106">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fcadf-106">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="fcadf-107">REST API</span><span class="sxs-lookup"><span data-stu-id="fcadf-107">REST API</span></span>](resource-manager-rest-api.md)
>
>

<span data-ttu-id="fcadf-108">V tomto článku se dozvíte, jak toomanage řešení Azure PowerShell a Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fcadf-108">In this article, you learn how toomanage your solutions with Azure PowerShell and Azure Resource Manager.</span></span> <span data-ttu-id="fcadf-109">Pokud nejste obeznámeni s Resource Managerem, přečtěte si téma [Přehled služby Správce prostředků](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fcadf-109">If you are not familiar with Resource Manager, see [Resource Manager Overview](resource-group-overview.md).</span></span> <span data-ttu-id="fcadf-110">Toto téma se zaměřuje na úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="fcadf-110">This topic focuses on management tasks.</span></span> <span data-ttu-id="fcadf-111">Vaším úkolem je:</span><span class="sxs-lookup"><span data-stu-id="fcadf-111">You will:</span></span>

1. <span data-ttu-id="fcadf-112">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="fcadf-112">Create a resource group</span></span>
2. <span data-ttu-id="fcadf-113">Přidat skupinu prostředků toohello prostředků</span><span class="sxs-lookup"><span data-stu-id="fcadf-113">Add a resource toohello resource group</span></span>
3. <span data-ttu-id="fcadf-114">Přidání značka toohello prostředku</span><span class="sxs-lookup"><span data-stu-id="fcadf-114">Add a tag toohello resource</span></span>
4. <span data-ttu-id="fcadf-115">Zadat dotaz na prostředky na základě názvy nebo hodnoty značky</span><span class="sxs-lookup"><span data-stu-id="fcadf-115">Query resources based on names or tag values</span></span>
5. <span data-ttu-id="fcadf-116">Použít a odebere se zámek na prostředku hello</span><span class="sxs-lookup"><span data-stu-id="fcadf-116">Apply and remove a lock on hello resource</span></span>
6. <span data-ttu-id="fcadf-117">Odstranit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="fcadf-117">Delete a resource group</span></span>

<span data-ttu-id="fcadf-118">Tento článek nezobrazuje jak toodeploy předplatné tooyour šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="fcadf-118">This article does not show how toodeploy a Resource Manager template tooyour subscription.</span></span> <span data-ttu-id="fcadf-119">Podrobnosti naleznete v tématu [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="fcadf-119">For that information, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>

## <a name="get-started-with-azure-powershell"></a><span data-ttu-id="fcadf-120">Začínáme s Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fcadf-120">Get started with Azure PowerShell</span></span>

<span data-ttu-id="fcadf-121">Pokud jste prostředí Azure PowerShell nenainstalovali, přečtěte si téma [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fcadf-121">If you have not installed Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="fcadf-122">Pokud jste nainstalovali Azure PowerShell v posledních hello ale nebyly aktualizovány nedávno, zvažte instalaci nejnovější verze hello.</span><span class="sxs-lookup"><span data-stu-id="fcadf-122">If you have installed Azure PowerShell in hello past but have not updated it recently, consider installing hello latest version.</span></span> <span data-ttu-id="fcadf-123">Můžete aktualizovat verzi hello prostřednictvím hello stejnou metodu používá tooinstall ho.</span><span class="sxs-lookup"><span data-stu-id="fcadf-123">You can update hello version through hello same method you used tooinstall it.</span></span> <span data-ttu-id="fcadf-124">Například pokud jste použili hello instalačního programu webové platformy, spusťte znovu a vyhledejte aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="fcadf-124">For example, if you used hello Web Platform Installer, launch it again and look for an update.</span></span>

<span data-ttu-id="fcadf-125">toocheck vaší verzi hello modul prostředky Azure, použijte následující rutinu hello:</span><span class="sxs-lookup"><span data-stu-id="fcadf-125">toocheck your version of hello Azure Resources module, use hello following cmdlet:</span></span>

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

<span data-ttu-id="fcadf-126">Toto téma bylo aktualizováno pro verzi 3.3.0.</span><span class="sxs-lookup"><span data-stu-id="fcadf-126">This topic was updated for version 3.3.0.</span></span> <span data-ttu-id="fcadf-127">Pokud máte starší verzi, nemusí odpovídat prostředí hello kroky uvedené v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="fcadf-127">If you have an earlier version, your experience might not match hello steps shown in this topic.</span></span> <span data-ttu-id="fcadf-128">Dokumentaci o rutinách hello v této verzi najdete v tématu [AzureRM.Resources modulu](/powershell/module/azurerm.resources).</span><span class="sxs-lookup"><span data-stu-id="fcadf-128">For documentation about hello cmdlets in this version, see [AzureRM.Resources Module](/powershell/module/azurerm.resources).</span></span>

## <a name="log-in-tooyour-azure-account"></a><span data-ttu-id="fcadf-129">Přihlaste se tooyour účet Azure</span><span class="sxs-lookup"><span data-stu-id="fcadf-129">Log in tooyour Azure account</span></span>
<span data-ttu-id="fcadf-130">Před zahájením práce na řešení, musíte se přihlásit v tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="fcadf-130">Before working on your solution, you must log in tooyour account.</span></span>

<span data-ttu-id="fcadf-131">toolog v tooyour účet Azure, použijte hello **Login-AzureRmAccount** rutiny.</span><span class="sxs-lookup"><span data-stu-id="fcadf-131">toolog in tooyour Azure account, use hello **Login-AzureRmAccount** cmdlet.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="fcadf-132">Hello rutina vás vyzve k zadání hello přihlašovací údaje k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="fcadf-132">hello cmdlet prompts you for hello login credentials for your Azure account.</span></span> <span data-ttu-id="fcadf-133">Po přihlášení stahování nastavení svého účtu tak, aby byly k dispozici tooAzure prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fcadf-133">After logging in, it downloads your account settings so they are available tooAzure PowerShell.</span></span>

<span data-ttu-id="fcadf-134">Hello rutina vrátí informace o odběru toouse pro úlohy hello váš účet a hello.</span><span class="sxs-lookup"><span data-stu-id="fcadf-134">hello cmdlet returns information about your account and hello subscription toouse for hello tasks.</span></span>

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

<span data-ttu-id="fcadf-135">Pokud máte více než jedno předplatné, můžete přepnout tooa jiné předplatné.</span><span class="sxs-lookup"><span data-stu-id="fcadf-135">If you have more than one subscription, you can switch tooa different subscription.</span></span> <span data-ttu-id="fcadf-136">První Podíváme se, Všechna předplatná hello pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="fcadf-136">First, let's see all hello subscriptions for your account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="fcadf-137">Vrátí povolených a zakázaných odběrů.</span><span class="sxs-lookup"><span data-stu-id="fcadf-137">It returns enabled and disabled subscriptions.</span></span>

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

<span data-ttu-id="fcadf-138">tooswitch tooa jiného předplatného, zadejte název odběru hello s hello **Set-AzureRmContext** rutiny.</span><span class="sxs-lookup"><span data-stu-id="fcadf-138">tooswitch tooa different subscription, provide hello subscription name with hello **Set-AzureRmContext** cmdlet.</span></span>

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a><span data-ttu-id="fcadf-139">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="fcadf-139">Create a resource group</span></span>
<span data-ttu-id="fcadf-140">Před nasazením libovolné předplatné, tooyour prostředky, musíte vytvořit skupinu prostředků, která bude obsahovat prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="fcadf-140">Before deploying any resources tooyour subscription, you must create a resource group that will contain hello resources.</span></span>

<span data-ttu-id="fcadf-141">toocreate skupinu prostředků použijte hello **New-AzureRmResourceGroup** rutiny.</span><span class="sxs-lookup"><span data-stu-id="fcadf-141">toocreate a resource group, use hello **New-AzureRmResourceGroup** cmdlet.</span></span> <span data-ttu-id="fcadf-142">příkaz Hello používá hello **název** toospecify parametr název skupiny prostředků hello a hello **umístění** parametr toospecify jeho umístění.</span><span class="sxs-lookup"><span data-stu-id="fcadf-142">hello command uses hello **Name** parameter toospecify a name for hello resource group and hello **Location** parameter toospecify its location.</span></span>

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

<span data-ttu-id="fcadf-143">výstup Hello je ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="fcadf-143">hello output is in hello following format:</span></span>

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

<span data-ttu-id="fcadf-144">Pokud potřebujete skupinu prostředků tooretrieve hello později, použijte hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="fcadf-144">If you need tooretrieve hello resource group later, use hello following cmdlet:</span></span>

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

<span data-ttu-id="fcadf-145">tooget všechny hello skupiny prostředků v rámci vašeho předplatného, nezadávejte název:</span><span class="sxs-lookup"><span data-stu-id="fcadf-145">tooget all hello resource groups in your subscription, do not specify a name:</span></span>

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-tooa-resource-group"></a><span data-ttu-id="fcadf-146">Přidat skupinu prostředků tooa prostředky</span><span class="sxs-lookup"><span data-stu-id="fcadf-146">Add resources tooa resource group</span></span>
<span data-ttu-id="fcadf-147">tooadd skupinu prostředků toohello prostředků, můžete použít hello **New-AzureRmResource** rutina nebo rutiny, která je toohello konkrétní typ prostředku, kterou vytváříte (jako je **AzureRmStorageAccount nový**).</span><span class="sxs-lookup"><span data-stu-id="fcadf-147">tooadd a resource toohello resource group, you can use hello **New-AzureRmResource** cmdlet or a cmdlet that is specific toohello type of resource you are creating (like **New-AzureRmStorageAccount**).</span></span> <span data-ttu-id="fcadf-148">Může pro vás snadnější toouse rutinu, která je tooa konkrétní typ prostředku, protože obsahuje parametry pro hello vlastnosti, které jsou potřebné pro nový prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="fcadf-148">You might find it easier toouse a cmdlet that is specific tooa resource type because it includes parameters for hello properties that are needed for hello new resource.</span></span> <span data-ttu-id="fcadf-149">toouse **New-AzureRmResource**, musíte znát všechny vlastnosti tooset hello bez výzvy pro ně.</span><span class="sxs-lookup"><span data-stu-id="fcadf-149">toouse **New-AzureRmResource**, you must know all hello properties tooset without being prompted for them.</span></span>

<span data-ttu-id="fcadf-150">Přidání prostředku pomocí rutin však může být matoucí budoucí protože hello nový prostředek neexistuje v šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fcadf-150">However, adding a resource through cmdlets might cause future confusion because hello new resource does not exist in a Resource Manager template.</span></span> <span data-ttu-id="fcadf-151">Společnost Microsoft doporučuje definování hello infrastrukturu pro vaše řešení Azure v šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fcadf-151">Microsoft recommends defining hello infrastructure for your Azure solution in a Resource Manager template.</span></span> <span data-ttu-id="fcadf-152">Šablony umožňují tooreliably a opakovaného nasazování svého řešení.</span><span class="sxs-lookup"><span data-stu-id="fcadf-152">Templates enable you tooreliably and repeatedly deploy your solution.</span></span> <span data-ttu-id="fcadf-153">Pro toto téma vytvoříte účet úložiště s rutiny prostředí PowerShell, ale později generování šablonu z vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="fcadf-153">For this topic, you create a storage account with a PowerShell cmdlet, but later you generate a template from your resource group.</span></span>

<span data-ttu-id="fcadf-154">Hello následující rutina vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="fcadf-154">hello following cmdlet creates a storage account.</span></span> <span data-ttu-id="fcadf-155">Nepoužívejte název hello v příkladu hello zadejte jedinečný název pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="fcadf-155">Instead of using hello name shown in hello example, provide a unique name for hello storage account.</span></span> <span data-ttu-id="fcadf-156">Název Hello musí být v rozmezí 3 až 24 znaků a použít pouze čísla a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="fcadf-156">hello name must be between 3 and 24 characters in length, and use only numbers and lower-case letters.</span></span> <span data-ttu-id="fcadf-157">Pokud použijete název hello hello příkladu, zobrazí chybu, protože tento název je již používán.</span><span class="sxs-lookup"><span data-stu-id="fcadf-157">If you use hello name shown in hello example, you receive an error because that name is already in use.</span></span>

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

<span data-ttu-id="fcadf-158">Pokud potřebujete tooretrieve tohoto prostředku novější, použijte hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="fcadf-158">If you need tooretrieve this resource later, use hello following cmdlet:</span></span>

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a><span data-ttu-id="fcadf-159">Přidání značky</span><span class="sxs-lookup"><span data-stu-id="fcadf-159">Add a tag</span></span>

<span data-ttu-id="fcadf-160">Značky umožňují tooorganize vaše prostředky podle toodifferent vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fcadf-160">Tags enable you tooorganize your resources according toodifferent properties.</span></span> <span data-ttu-id="fcadf-161">Například může mít několik prostředků v různých prostředků skupiny, které patří toohello stejného oddělení.</span><span class="sxs-lookup"><span data-stu-id="fcadf-161">For example, you may have several resources in different resource groups that belong toohello same department.</span></span> <span data-ttu-id="fcadf-162">Můžete použít toomark oddělení značky a hodnota toothose prostředky je jako patřící toohello stejné kategorii.</span><span class="sxs-lookup"><span data-stu-id="fcadf-162">You can apply a department tag and value toothose resources toomark them as belonging toohello same category.</span></span> <span data-ttu-id="fcadf-163">Nebo můžete označit, zda je prostředek použít v produkčním i testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="fcadf-163">Or, you can mark whether a resource is used in a production or test environment.</span></span> <span data-ttu-id="fcadf-164">V tomto tématu použít jeden prostředek tooonly značky, ale ve vašem prostředí nejpravděpodobnější má smysl, že tooapply značky tooall vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="fcadf-164">In this topic, you apply tags tooonly one resource, but in your environment it most likely makes sense tooapply tags tooall your resources.</span></span>

<span data-ttu-id="fcadf-165">následující rutina Hello platí dva účet úložiště tooyour značky:</span><span class="sxs-lookup"><span data-stu-id="fcadf-165">hello following cmdlet applies two tags tooyour storage account:</span></span>

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

<span data-ttu-id="fcadf-166">Značky jsou aktualizovány jako jednoho objektu.</span><span class="sxs-lookup"><span data-stu-id="fcadf-166">Tags are updated as a single object.</span></span> <span data-ttu-id="fcadf-167">tooadd prostředek tooa značky, která již obsahuje značky, nejdřív načtěte stávající značky hello.</span><span class="sxs-lookup"><span data-stu-id="fcadf-167">tooadd a tag tooa resource that already includes tags, first retrieve hello existing tags.</span></span> <span data-ttu-id="fcadf-168">Přidejte hello nové značky toohello objekt, který obsahuje stávající značky hello a znovu použít všechny hello značky toohello prostředků.</span><span class="sxs-lookup"><span data-stu-id="fcadf-168">Add hello new tag toohello object that contains hello existing tags, and reapply all hello tags toohello resource.</span></span>

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a><span data-ttu-id="fcadf-169">Hledat prostředky</span><span class="sxs-lookup"><span data-stu-id="fcadf-169">Search for resources</span></span>

<span data-ttu-id="fcadf-170">Použití hello **najít AzureRmResource** rutiny tooretrieve prostředky pro různé vyhledávací podmínky.</span><span class="sxs-lookup"><span data-stu-id="fcadf-170">Use hello **Find-AzureRmResource** cmdlet tooretrieve resources for different search conditions.</span></span>

* <span data-ttu-id="fcadf-171">tooget prostředek podle názvu, zadejte hello **ResourceNameContains** parametr:</span><span class="sxs-lookup"><span data-stu-id="fcadf-171">tooget a resource by name, provide hello **ResourceNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* <span data-ttu-id="fcadf-172">tooget všechny hello prostředky ve skupině prostředků, poskytovat hello **ResourceGroupNameContains** parametr:</span><span class="sxs-lookup"><span data-stu-id="fcadf-172">tooget all hello resources in a resource group, provide hello **ResourceGroupNameContains** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* <span data-ttu-id="fcadf-173">tooget všechny prostředky hello se název značky a hodnotou, zadejte hello **TagName** a **TagValue** parametry:</span><span class="sxs-lookup"><span data-stu-id="fcadf-173">tooget all hello resources with a tag name and value, provide hello **TagName** and **TagValue** parameters:</span></span>

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* <span data-ttu-id="fcadf-174">tooall hello zdroje s konkrétní typ prostředku, obsahují hello **ResourceType** parametr:</span><span class="sxs-lookup"><span data-stu-id="fcadf-174">tooall hello resources with a particular resource type, provide hello **ResourceType** parameter:</span></span>

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="lock-a-resource"></a><span data-ttu-id="fcadf-175">Zamknout prostředku</span><span class="sxs-lookup"><span data-stu-id="fcadf-175">Lock a resource</span></span>

<span data-ttu-id="fcadf-176">Pokud budete potřebovat toomake opravdu důležité prostředku není omylem odstranit nebo upravit, použít toohello prostředek lock.</span><span class="sxs-lookup"><span data-stu-id="fcadf-176">When you need toomake sure a critical resource is not accidentally deleted or modified, apply a lock toohello resource.</span></span> <span data-ttu-id="fcadf-177">Můžete buď zadat **CanNotDelete** nebo **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="fcadf-177">You can specify either a **CanNotDelete** or **ReadOnly**.</span></span>

<span data-ttu-id="fcadf-178">zámky správy toocreate nebo odstranit, musíte mít přístup příliš`Microsoft.Authorization/*` nebo `Microsoft.Authorization/locks/*` akce.</span><span class="sxs-lookup"><span data-stu-id="fcadf-178">toocreate or delete management locks, you must have access too`Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions.</span></span> <span data-ttu-id="fcadf-179">Hello předdefinovaných rolí pouze vlastník a správce přístupu uživatelů mají tyto akce.</span><span class="sxs-lookup"><span data-stu-id="fcadf-179">Of hello built-in roles, only Owner and User Access Administrator are granted those actions.</span></span>

<span data-ttu-id="fcadf-180">tooapply zámek, použijte následující rutinu hello:</span><span class="sxs-lookup"><span data-stu-id="fcadf-180">tooapply a lock, use hello following cmdlet:</span></span>

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="fcadf-181">Hello uzamčeném prostředků v předchozím příkladu hello nelze odstranit, dokud se neodstraní hello zámku.</span><span class="sxs-lookup"><span data-stu-id="fcadf-181">hello locked resource in hello preceding example cannot be deleted until hello lock is removed.</span></span> <span data-ttu-id="fcadf-182">tooremove zámek, použijte:</span><span class="sxs-lookup"><span data-stu-id="fcadf-182">tooremove a lock, use:</span></span>

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

<span data-ttu-id="fcadf-183">Další informace o nastavení zámky najdete v tématu [zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="fcadf-183">For more information about setting locks, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

## <a name="remove-resources-or-resource-group"></a><span data-ttu-id="fcadf-184">Odebrat prostředky nebo skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="fcadf-184">Remove resources or resource group</span></span>
<span data-ttu-id="fcadf-185">Můžete odebrat prostředek nebo skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="fcadf-185">You can remove a resource or resource group.</span></span> <span data-ttu-id="fcadf-186">Když odeberete skupinu prostředků, je také odebrat všechny hello prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="fcadf-186">When you remove a resource group, you also remove all hello resources within that resource group.</span></span>

* <span data-ttu-id="fcadf-187">toodelete prostředku z hello skupinu prostředků, použijte hello **odebrat AzureRmResource** rutiny.</span><span class="sxs-lookup"><span data-stu-id="fcadf-187">toodelete a resource from hello resource group, use hello **Remove-AzureRmResource** cmdlet.</span></span> <span data-ttu-id="fcadf-188">Tato rutina odstraní hello prostředků, ale nedojde k odstranění skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="fcadf-188">This cmdlet deletes hello resource, but does not delete hello resource group.</span></span>

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* <span data-ttu-id="fcadf-189">toodelete skupinu prostředků a všechny její prostředky používají hello **Remove-AzureRmResourceGroup** rutiny.</span><span class="sxs-lookup"><span data-stu-id="fcadf-189">toodelete a resource group and all its resources, use hello **Remove-AzureRmResourceGroup** cmdlet.</span></span>

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

<span data-ttu-id="fcadf-190">Pro obě rutiny se zobrazí výzva tooconfirm chcete tooremove hello prostředek nebo skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="fcadf-190">For both cmdlets, you are asked tooconfirm that you wish tooremove hello resource or resource group.</span></span> <span data-ttu-id="fcadf-191">Pokud se odstraní hello operace úspěšně hello prostředek nebo skupina prostředků, vrátí **True**.</span><span class="sxs-lookup"><span data-stu-id="fcadf-191">If hello operation successfully deletes hello resource or resource group, it returns **True**.</span></span>

## <a name="run-resource-manager-scripts-with-azure-automation"></a><span data-ttu-id="fcadf-192">Spusťte skripty správce prostředků se Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="fcadf-192">Run Resource Manager scripts with Azure Automation</span></span>

<span data-ttu-id="fcadf-193">Toto téma ukazuje, jak tooperform základní operace na vašich prostředků Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fcadf-193">This topic shows you how tooperform basic operations on your resources with Azure PowerShell.</span></span> <span data-ttu-id="fcadf-194">Pro pokročilejší scénáře správy můžete obvykle mají toocreate skriptu a znovu použít tento skript podle potřeby nebo podle plánu.</span><span class="sxs-lookup"><span data-stu-id="fcadf-194">For more advanced management scenarios, you typically want toocreate a script, and reuse that script as needed or on a schedule.</span></span> <span data-ttu-id="fcadf-195">[Služby Azure Automation](../automation/automation-intro.md) poskytuje způsob pro vás často používané tooautomate skriptů, které spravují řešení Azure.</span><span class="sxs-lookup"><span data-stu-id="fcadf-195">[Azure Automation](../automation/automation-intro.md) provides a way for you tooautomate frequently used scripts that manage your Azure solutions.</span></span>

<span data-ttu-id="fcadf-196">Hello následující témata ukazují, jak toouse automatizace Azure Resource Manager a prostředí PowerShell tooeffectively provádět úlohy správy:</span><span class="sxs-lookup"><span data-stu-id="fcadf-196">hello following topics show you how toouse Azure Automation, Resource Manager, and PowerShell tooeffectively perform management tasks:</span></span>

- <span data-ttu-id="fcadf-197">Informace o vytvoření sady runbook najdete v tématu [Můj první Powershellový runbook](../automation/automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="fcadf-197">For information about creating a runbook, see [My first PowerShell runbook](../automation/automation-first-runbook-textual-powershell.md).</span></span>
- <span data-ttu-id="fcadf-198">Informace o práci s galerií skriptů, najdete v tématu [Galerie Runbooků a modulů pro Azure Automation](../automation/automation-runbook-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="fcadf-198">For information about working with galleries of scripts, see [Runbook and module galleries for Azure Automation](../automation/automation-runbook-gallery.md).</span></span>
- <span data-ttu-id="fcadf-199">Sady runbook, které spuštění a zastavení virtuálních počítačů, najdete v části [scénáře Azure Automation: formátu JSON pomocí značek toocreate plán pro virtuální počítač Azure spuštění a vypnutí](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span><span class="sxs-lookup"><span data-stu-id="fcadf-199">For runbooks that start and stop virtual machines, see [Azure Automation scenario: Using JSON-formatted tags toocreate a schedule for Azure VM startup and shutdown](../automation/automation-scenario-start-stop-vm-wjson-tags.md).</span></span>
- <span data-ttu-id="fcadf-200">Sady runbook, které spuštění a zastavení počítačem nepracujete virtuální počítače, naleznete v části [spuštění a zastavení virtuálních počítačů během řešení počítačem nepracujete v automatizaci](../automation/automation-solution-vm-management.md).</span><span class="sxs-lookup"><span data-stu-id="fcadf-200">For runbooks that start and stop virtual machines off-hours, see [Start/Stop VMs during off-hours solution in Automation](../automation/automation-solution-vm-management.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fcadf-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fcadf-201">Next steps</span></span>
* <span data-ttu-id="fcadf-202">toolearn o vytváření šablon Resource Manageru, najdete v části [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="fcadf-202">toolearn about creating Resource Manager templates, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="fcadf-203">toolearn o nasazení šablony, najdete v části [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="fcadf-203">toolearn about deploying templates, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="fcadf-204">Můžete přesunout existující prostředky tooa novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="fcadf-204">You can move existing resources tooa new resource group.</span></span> <span data-ttu-id="fcadf-205">Příklady najdete v tématu [tooNew přesunutí prostředků skupiny prostředků nebo předplatného](resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="fcadf-205">For examples, see [Move Resources tooNew Resource Group or Subscription](resource-group-move-resources.md).</span></span>
* <span data-ttu-id="fcadf-206">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="fcadf-206">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

