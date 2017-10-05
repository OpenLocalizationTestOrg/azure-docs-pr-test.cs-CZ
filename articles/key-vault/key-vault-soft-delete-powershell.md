---
ms.assetid: 
title: "Azure Key Vault - použití konfigurace soft odstranění pomocí prostředí PowerShell"
description: "Pomocí prostředí PowerShell výstřižků kódu případu příklady konfigurace soft odstranění"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bruceper
ms.openlocfilehash: 8cf0674f7eb139e50da4a3c22a8d8376a86b0dcc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a><span data-ttu-id="3f623-103">Jak používat Key Vault konfigurace soft odstranění pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3f623-103">How to use Key Vault soft-delete with PowerShell</span></span>

<span data-ttu-id="3f623-104">Azure Key Vault obnovitelného odstranění funkce umožňuje obnovení odstraněné trezory a objekty trezoru.</span><span class="sxs-lookup"><span data-stu-id="3f623-104">Azure Key Vault's soft delete feature allows recovery of deleted vaults and vault objects.</span></span> <span data-ttu-id="3f623-105">Konkrétně obnovitelného odstranění adresy následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="3f623-105">Specifically, soft-delete addresses the following scenarios:</span></span>

- <span data-ttu-id="3f623-106">Podpora pro obnovitelné odstranění trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="3f623-106">Support for recoverable deletion of a key vault</span></span>
- <span data-ttu-id="3f623-107">Podpora pro obnovitelné odstranění trezoru klíčů objektů; klíče, tajné údaje a certifikáty</span><span class="sxs-lookup"><span data-stu-id="3f623-107">Support for recoverable deletion of key vault objects; keys, secrets, and, certificates</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f623-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3f623-108">Prerequisites</span></span>

- <span data-ttu-id="3f623-109">Prostředí Azure PowerShell 4.0.0 nebo novější – Pokud nepoužíváte již je tento instalační program, nainstalujte prostředí Azure PowerShell a přidružit ho ke svému předplatnému Azure, najdete [postup instalace a konfigurace prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3f623-109">Azure PowerShell 4.0.0 or later - If you don't have this already setup, install Azure PowerShell and associate it with your Azure subscription, see [How to install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span> 

>[!NOTE]
> <span data-ttu-id="3f623-110">Zastaralou verzi naše klíč trezoru PowerShell výstupní formátování souboru, který je **může** ho možné zavést do vašeho prostředí místo správnou verzi.</span><span class="sxs-lookup"><span data-stu-id="3f623-110">There is an outdated version of our Key Vault PowerShell output formatting file that **may** be loaded into your environment instead of the correct version.</span></span> <span data-ttu-id="3f623-111">Jsme se očekává aktualizovanou verzi prostředí PowerShell tak, aby obsahovala oprava potřebné pro formátování výstupu a aktualizuje v tomto tématu v daném čase.</span><span class="sxs-lookup"><span data-stu-id="3f623-111">We are anticipating an updated version of PowerShell to contain the needed correction for the output formatting and will update this topic at that time.</span></span> <span data-ttu-id="3f623-112">Aktuální řešení, by měl narazíte na potíže formátování je:</span><span class="sxs-lookup"><span data-stu-id="3f623-112">The current workaround, should you encounter this formatting problem, is:</span></span>
> - <span data-ttu-id="3f623-113">Pomocí následujícího dotazu, pokud si všimnete nevidíte konfigurace soft odstranění povolena vlastnost popsaných v tomto tématu: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span><span class="sxs-lookup"><span data-stu-id="3f623-113">Use the following query if you notice you're not seeing the soft-delete enabled property described in this topic: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span></span>


<span data-ttu-id="3f623-114">Informace o konkrétní refernece Key Vault pro prostředí PowerShell najdete v tématu [Azure Key Vault PowerShell odkaz](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span><span class="sxs-lookup"><span data-stu-id="3f623-114">For Key Vault specific refernece information for PowerShell, see [Azure Key Vault PowerShell reference](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span></span>

## <a name="required-permissions"></a><span data-ttu-id="3f623-115">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="3f623-115">Required permissions</span></span>

<span data-ttu-id="3f623-116">Key Vault operace jsou samostatně spravované prostřednictvím oprávnění k přístupu na základě rolí k řízení (RBAC) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3f623-116">Key Vault operations are separately managed via role-based access control (RBAC) permissions as follows:</span></span>

| <span data-ttu-id="3f623-117">Operace</span><span class="sxs-lookup"><span data-stu-id="3f623-117">Operation</span></span> | <span data-ttu-id="3f623-118">Popis</span><span class="sxs-lookup"><span data-stu-id="3f623-118">Description</span></span> | <span data-ttu-id="3f623-119">Oprávnění uživatele</span><span class="sxs-lookup"><span data-stu-id="3f623-119">User permission</span></span> |
|:--|:--|:--|
|<span data-ttu-id="3f623-120">Seznam</span><span class="sxs-lookup"><span data-stu-id="3f623-120">List</span></span>|<span data-ttu-id="3f623-121">Seznamy odstraňovat trezorů klíčů.</span><span class="sxs-lookup"><span data-stu-id="3f623-121">Lists deleted key vaults.</span></span>|<span data-ttu-id="3f623-122">Microsoft.KeyVault/deletedVaults/read</span><span class="sxs-lookup"><span data-stu-id="3f623-122">Microsoft.KeyVault/deletedVaults/read</span></span>|
|<span data-ttu-id="3f623-123">Zotavit</span><span class="sxs-lookup"><span data-stu-id="3f623-123">Recover</span></span>|<span data-ttu-id="3f623-124">Obnoví odstraněné trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3f623-124">Restores a deleted key vault.</span></span>|<span data-ttu-id="3f623-125">Microsoft.KeyVault/vaults/write</span><span class="sxs-lookup"><span data-stu-id="3f623-125">Microsoft.KeyVault/vaults/write</span></span>|
|<span data-ttu-id="3f623-126">Vyprázdnit</span><span class="sxs-lookup"><span data-stu-id="3f623-126">Purge</span></span>|<span data-ttu-id="3f623-127">Trvale odstraní odstraněné trezoru klíčů a veškerý jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="3f623-127">Permanently removes a deleted key vault and all its contents.</span></span>|<span data-ttu-id="3f623-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span><span class="sxs-lookup"><span data-stu-id="3f623-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span></span>|

<span data-ttu-id="3f623-129">Další informace o oprávněních a řízení přístupu najdete v tématu [zabezpečit váš trezor klíčů](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="3f623-129">For more information on permissions and access control, see [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>

## <a name="enabling-soft-delete"></a><span data-ttu-id="3f623-130">Povolení konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="3f623-130">Enabling soft-delete</span></span>

<span data-ttu-id="3f623-131">Abyste mohli obnovit odstraněné trezoru klíčů nebo objekty uložené v trezoru klíčů, musíte nejdřív povolit konfigurace soft odstranění tohoto klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="3f623-131">To be able to recover a deleted key vault or objects stored in a key vault, you must first enable soft-delete for that key vault.</span></span>

### <a name="existing-key-vault"></a><span data-ttu-id="3f623-132">Existující trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="3f623-132">Existing key vault</span></span>

<span data-ttu-id="3f623-133">Pro existující trezor klíčů s názvem ContosoVault následujícím způsobem povolte obnovitelného odstranění.</span><span class="sxs-lookup"><span data-stu-id="3f623-133">For an existing key vault named ContosoVault, enable soft-delete as follows.</span></span> 

>[!NOTE]
><span data-ttu-id="3f623-134">Aktuálně je nutné použít zpracování prostředků Azure Resource Manager zápis přímo *enableSoftDelete* vlastnost prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3f623-134">Currently you need to use Azure Resource Manager resource manipulation to directly write the *enableSoftDelete* property to the Key Vault resource.</span></span>

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a><span data-ttu-id="3f623-135">Nový trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="3f623-135">New key vault</span></span>

<span data-ttu-id="3f623-136">Povolení konfigurace soft odstranění pro nového trezoru klíčů se provádí v okamžiku vytvoření přidáním příznak konfigurace soft odstranění povolit, aby vaše vytvoření příkazu.</span><span class="sxs-lookup"><span data-stu-id="3f623-136">Enabling soft-delete for a new key vault is done at creation time by adding the soft-delete enable flag to your create command.</span></span>

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a><span data-ttu-id="3f623-137">Ověřte povolování konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="3f623-137">Verify soft-delete enablement</span></span>

<span data-ttu-id="3f623-138">Ověřte, že trezoru klíčů má konfigurace soft odstranění povolena, spusťte *získat* příkazů a podívejte se 'logicky odstranit povolen?"</span><span class="sxs-lookup"><span data-stu-id="3f623-138">To verify that a key vault has soft-delete enabled, run the *get* command and look for the 'Soft Delete Enabled?'</span></span> <span data-ttu-id="3f623-139">atribut a jeho nastavení true nebo false.</span><span class="sxs-lookup"><span data-stu-id="3f623-139">attribute and its setting, true or false.</span></span>

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a><span data-ttu-id="3f623-140">Odstranění trezoru klíčů chráněn konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="3f623-140">Deleting a key vault protected by soft-delete</span></span>

<span data-ttu-id="3f623-141">Příkaz k odstranění (nebo odebrání) trezoru klíčů zůstane stejný, ale její chování mění v závislosti na tom, jestli jste povolili obnovitelného odstranění nebo ne.</span><span class="sxs-lookup"><span data-stu-id="3f623-141">The command to delete (or remove) a key vault remains the same, but its behavior changes depending on whether you have enabled soft-delete or not.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
><span data-ttu-id="3f623-142">Pokud spustíte předchozí příkaz pro trezor klíčů, který nemá konfigurace soft odstranění povoleno, bude trvale odstranit tento trezor klíčů a veškerý jeho obsah bez jakékoli možnosti pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="3f623-142">If you run the previous command for a key vault that does not have soft-delete enabled, you will permanently delete this key vault and all its content without any options for recovery.</span></span>

### <a name="how-soft-delete-protects-your-key-vaults"></a><span data-ttu-id="3f623-143">Jak odstranit soft chrání vaše trezorů klíčů</span><span class="sxs-lookup"><span data-stu-id="3f623-143">How soft-delete protects your key vaults</span></span>

<span data-ttu-id="3f623-144">S konfigurace soft odstranění povoleno:</span><span class="sxs-lookup"><span data-stu-id="3f623-144">With soft-delete enabled:</span></span>

- <span data-ttu-id="3f623-145">Při odstranění trezoru klíčů není odebrán z jeho skupin prostředků a je umístěn v vyhrazené oboru názvů, který je pouze přidružená k umístění, kde se vytvořila.</span><span class="sxs-lookup"><span data-stu-id="3f623-145">When a key vault is deleted, it is removed from its resource group and placed in a reserved namespace that is only associated with the location where it was created.</span></span> 
- <span data-ttu-id="3f623-146">Objekty odstraněné klíč trezoru, jako jsou nedostupné klíče, tajných klíčů a certifikátů a tak zůstanou při jejich obsahující trezoru klíčů je ve stavu deleted.</span><span class="sxs-lookup"><span data-stu-id="3f623-146">Objects in a deleted key vault, such as keys, secrets and, certificates, are inaccessible and remain so while their containing key vault is in the deleted state.</span></span> 
- <span data-ttu-id="3f623-147">Název DNS pro trezoru klíčů v odstraněném stavu je stále vyhrazené proto nelze vytvořit nový trezor klíčů se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="3f623-147">The DNS name for a key vault in a deleted state is still reserved so, a new key vault with same name cannot be created.</span></span>  

<span data-ttu-id="3f623-148">Můžete si zobrazit trezorů klíčů stavu deleted, spojené s vaším předplatným, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="3f623-148">You may view deleted state key vaults, associated with your subscription, using the following command:</span></span>

```powershell
PS C:\> Get-AzureRmKeyVault -InRemovedStateVault 

Name           : ContosoVault
Location             : westus
Id                   : /subscriptions/xxx/providers/Microsoft.KeyVault/locations/westus/deletedVaults/ContosoVault
Resource ID          : /subscriptions/xxx/resourceGroups/ContosoVault/providers/Microsoft.KeyVault/vaults/ContosoVault
Deletion Date        : 5/9/2017 12:14:14 AM
Scheduled Purge Date : 8/7/2017 12:14:14 AM
Tags                 :
```

<span data-ttu-id="3f623-149">*ID prostředku* ve výstupu odkazuje na původní ID prostředku trezoru.</span><span class="sxs-lookup"><span data-stu-id="3f623-149">The *Resource ID* in the output refers to the original resource ID of this vault.</span></span> <span data-ttu-id="3f623-150">Vzhledem k tomu, že tento trezor klíčů je teď ve stavu deleted, neexistuje žádný prostředek s ID tohoto zdroje.</span><span class="sxs-lookup"><span data-stu-id="3f623-150">Since this key vault is now in a deleted state, no resource exists with that resource ID.</span></span> <span data-ttu-id="3f623-151">*Id* pole lze použít k identifikaci prostředků při obnovení nebo vyprazdňování.</span><span class="sxs-lookup"><span data-stu-id="3f623-151">The *Id* field can be used to identify the resource when recovering, or purging.</span></span> <span data-ttu-id="3f623-152">*Naplánované datum vyprázdnění* pole určuje, kdy se trvale odstraní trezoru (Vymazat) Pokud nebyla provedena žádná akce pro tento trezor odstraněné.</span><span class="sxs-lookup"><span data-stu-id="3f623-152">The *Scheduled Purge Date* field indicates when the vault will be permanently deleted (purged) if no action is taken for this deleted vault.</span></span> <span data-ttu-id="3f623-153">Výchozí dobu uchování, používá k výpočtu *naplánované Vyprázdnit data*, je 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="3f623-153">The default retention period, used to calculate the *Scheduled Purge Date*, is 90 days.</span></span>

## <a name="recovering-a-key-vault"></a><span data-ttu-id="3f623-154">Obnovení trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="3f623-154">Recovering a key vault</span></span>

<span data-ttu-id="3f623-155">Pokud chcete obnovit trezoru klíčů, je třeba zadat název trezoru klíčů, skupinu prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="3f623-155">To recover a key vault, you need to specify the key vault name, resource group, and location.</span></span> <span data-ttu-id="3f623-156">Všimněte si umístění a skupiny prostředků odstraněné trezoru klíčů, jako je třeba tyto klíče trezoru procesu obnovení.</span><span class="sxs-lookup"><span data-stu-id="3f623-156">Note the location and the resource group of the deleted key vault as you need these for a key vault recovery process.</span></span>

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

<span data-ttu-id="3f623-157">Když je obnovena trezoru klíčů, výsledkem je nový prostředek s ID trezoru klíčů původního zdroje.</span><span class="sxs-lookup"><span data-stu-id="3f623-157">When a key vault is recovered, the result is a new resource with the key vault's original resource ID.</span></span> <span data-ttu-id="3f623-158">Pokud byl odebrán skupině prostředků, které existovalo trezoru klíčů, musí před trezoru klíčů lze obnovit vytvořit novou skupinu prostředků se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="3f623-158">If the resource group where the key vault existed has been removed, a new resource group with same name must be created before the key vault can be recovered.</span></span>

## <a name="key-vault-objects-and-soft-delete"></a><span data-ttu-id="3f623-159">Key Vault objekty a konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="3f623-159">Key Vault objects and soft-delete</span></span>

<span data-ttu-id="3f623-160">Pro klíč 'ContosoFirstKey' v trezoru klíčů s názvem 'ContosoVault' s konfigurace soft odstranění povoleno, zde je způsob odstranili byste klíči.</span><span class="sxs-lookup"><span data-stu-id="3f623-160">For a key, 'ContosoFirstKey', in a key vault named 'ContosoVault' with soft-delete enabled, here's how you would delete that key.</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="3f623-161">S vaší povolené pro obnovitelného odstranění trezoru klíčů zobrazí odstraněný klíč i stejně, jako je odstraněn s výjimkou, že při explicitně seznamu nebo načtení odstraněné klíčů.</span><span class="sxs-lookup"><span data-stu-id="3f623-161">With your key vault enabled for soft-delete, a deleted key still appears like it's deleted except, when you explicitly list or retrieve deleted keys.</span></span> <span data-ttu-id="3f623-162">Většinu operací pro klíč ve stavu deleted se nezdaří s výjimkou výpis odstraněný klíč, obnovení nebo vyprazdňování ho.</span><span class="sxs-lookup"><span data-stu-id="3f623-162">Most operations on a key in the deleted state will fail except for listing a deleted key, recovering it or purging it.</span></span> 

<span data-ttu-id="3f623-163">Například požadavek na seznamu odstranit klíčů v trezoru klíčů, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3f623-163">For example, to request to list deleted keys in a key vault, use the following command:</span></span>

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a><span data-ttu-id="3f623-164">Přechodový stav.</span><span class="sxs-lookup"><span data-stu-id="3f623-164">Transition state</span></span> 

<span data-ttu-id="3f623-165">Pokud odstraníte klíč v trezoru klíčů s konfigurace soft odstranění povolené, může trvat několik sekund pro přechod k dokončení.</span><span class="sxs-lookup"><span data-stu-id="3f623-165">When you delete a key in a key vault with soft-delete enabled, it may take a few seconds for the transition to complete.</span></span> <span data-ttu-id="3f623-166">Při tomto přechod stavu může zdát, že klíč není v aktivním stavu nebo odstraněném stavu.</span><span class="sxs-lookup"><span data-stu-id="3f623-166">During this transition state, it may appear that the key is not in the active state or the deleted state.</span></span> <span data-ttu-id="3f623-167">Tento příkaz zobrazí seznam všech odstraněných klíčů v trezoru klíčů s názvem 'ContosoVault'.</span><span class="sxs-lookup"><span data-stu-id="3f623-167">This command will list all deleted keys in your key vault named 'ContosoVault'.</span></span>

```powershell
  Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
  Vault Name           : ContosoVault
  Name                 : ContosoFirstKey
  Id                   : https://ContosoVault.vault.azure.net:443/keys/ContosoFirstKey
  Deleted Date         : 2/14/2017 8:20:52 PM
  Scheduled Purge Date : 5/15/2017 8:20:52 PM
  Enabled              : True
  Expires              :
  Not Before           :
  Created              : 2/14/2017 8:16:07 PM
  Updated              : 2/14/2017 8:16:07 PM
  Tags                 :
```

### <a name="using-soft-delete-with-key-vault-objects"></a><span data-ttu-id="3f623-168">Pomocí konfigurace soft odstranění trezoru klíčů objekty</span><span class="sxs-lookup"><span data-stu-id="3f623-168">Using soft-delete with key vault objects</span></span>

<span data-ttu-id="3f623-169">Jenom jako trezorů klíčů, odstraněný klíč tajný klíč nebo certifikát zůstane ve stavu deleted po dobu 90 dnů Pokud jej obnovit, nebo ji vymazat.</span><span class="sxs-lookup"><span data-stu-id="3f623-169">Just like key vaults, a deleted key, secret or, certificate will remain in deleted state for up to 90 days unless you recover it or purge it.</span></span> 

#### <a name="keys"></a><span data-ttu-id="3f623-170">Klíče</span><span class="sxs-lookup"><span data-stu-id="3f623-170">Keys</span></span>

<span data-ttu-id="3f623-171">K obnovení odstraněné klíče:</span><span class="sxs-lookup"><span data-stu-id="3f623-171">To recover a deleted key:</span></span>

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="3f623-172">Trvale odstranit klíč:</span><span class="sxs-lookup"><span data-stu-id="3f623-172">To permanently delete a key:</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
><span data-ttu-id="3f623-173">Vymazání klíč se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="3f623-173">Purging a key will permanently delete it, meaning it will not be recoverable.</span></span>

<span data-ttu-id="3f623-174">**Obnovit** a **mazání** akce mají své vlastní oprávnění v zásadách přístupu k trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3f623-174">The **recover** and **purge** actions have their own permissions associated in a key vault access policy.</span></span> <span data-ttu-id="3f623-175">Pro uživatele nebo instanční objekt by mohl spustit **obnovit** nebo **mazání** akce musí mít odpovídající oprávnění pro tento objekt (klíč nebo tajný klíč) v zásadách přístupu trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3f623-175">For a user or service principal to be able to execute a **recover** or **purge** action they must have the respective permission for that object (key or secret) in the key vault access policy.</span></span> <span data-ttu-id="3f623-176">Ve výchozím nastavení **mazání** oprávnění nebyla přidána do zásad přístupu k trezoru klíčů, když "vše" zástupce se používá k udělení oprávnění pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="3f623-176">By default, the **purge** permission is not added to a key vault's access policy when the 'all' shortcut is used to grant all permissions to a user.</span></span> <span data-ttu-id="3f623-177">Je třeba explicitně udělit **mazání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="3f623-177">You must explicitly grant **purge** permission.</span></span> <span data-ttu-id="3f623-178">Například následující příkaz uděluje user@contoso.com oprávnění k provádění různých operací na klíče ve *ContosoVault* včetně **mazání**.</span><span class="sxs-lookup"><span data-stu-id="3f623-178">For example, the following command grants user@contoso.com permission to perform several operations on keys in *ContosoVault* including **purge**.</span></span>

#### <a name="set-a-key-vault-access-policy"></a><span data-ttu-id="3f623-179">Nastavit zásady přístupu k trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="3f623-179">Set a key vault access policy</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> <span data-ttu-id="3f623-180">Pokud máte existující trezor klíčů, který má právě měl konfigurace soft odstranění povoleno, nemusí mít **obnovit** a **mazání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="3f623-180">If you have an existing key vault that has just had soft-delete enabled, you may not have **recover** and **purge** permissions.</span></span>

#### <a name="secrets"></a><span data-ttu-id="3f623-181">Tajné kódy</span><span class="sxs-lookup"><span data-stu-id="3f623-181">Secrets</span></span>

<span data-ttu-id="3f623-182">Podobně jako klíče jsou tajných klíčů v trezoru klíčů provozovat na s vlastní příkazy.</span><span class="sxs-lookup"><span data-stu-id="3f623-182">Like keys, secrets in a key vault are operated on with their own commands.</span></span> <span data-ttu-id="3f623-183">Následující, jsou příkazy pro odstranění, výpis, obnovení a mazání tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="3f623-183">Following, are the commands for deleting, listing, recovering, and purging secrets.</span></span>

- <span data-ttu-id="3f623-184">Odstranění tajného klíče s názvem SQLPassword:</span><span class="sxs-lookup"><span data-stu-id="3f623-184">Delete a secret named SQLPassword:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- <span data-ttu-id="3f623-185">Zobrazí seznam všech odstraněných tajných klíčů v trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="3f623-185">List all deleted secrets in a key vault:</span></span> 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- <span data-ttu-id="3f623-186">Tajný klíč v odstraněném stavu obnovení:</span><span class="sxs-lookup"><span data-stu-id="3f623-186">Recover a secret in the deleted state:</span></span> 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- <span data-ttu-id="3f623-187">Vyprázdnění tajný klíč v odstraněném stavu:</span><span class="sxs-lookup"><span data-stu-id="3f623-187">Purge a secret in deleted state:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
><span data-ttu-id="3f623-188">Vymazání tajného klíče se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="3f623-188">Purging a secret will permanently delete it, meaning it will not be recoverable.</span></span>

## <a name="purging-and-key-vaults"></a><span data-ttu-id="3f623-189">Trezory mazání a klíč</span><span class="sxs-lookup"><span data-stu-id="3f623-189">Purging and key vaults</span></span>

### <a name="key-vault-objects"></a><span data-ttu-id="3f623-190">Objekty trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="3f623-190">Key vault objects</span></span>

<span data-ttu-id="3f623-191">Vymazání klíč, tajný klíč nebo certifikát se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="3f623-191">Purging a key, secret or, certificate will permanently delete it, meaning it will not be recoverable.</span></span> <span data-ttu-id="3f623-192">Trezor klíčů, který obsahuje odstraněný objekt se ale zachovají stejně jako všechny ostatní objekty v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="3f623-192">The key vault that contained the deleted object will however remain intact as will all other objects in the key vault.</span></span> 

### <a name="key-vaults-as-containers"></a><span data-ttu-id="3f623-193">Klíč trezory jako kontejnery</span><span class="sxs-lookup"><span data-stu-id="3f623-193">Key vaults as containers</span></span>
<span data-ttu-id="3f623-194">Když se vyprazdňují trezoru klíčů, veškerý její obsah, včetně klíče a tajné klíče, certifikáty, se trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="3f623-194">When a key vault is purged, all of its contents, including keys, secrets, and certificates, are permanently deleted.</span></span> <span data-ttu-id="3f623-195">K vyprázdnění trezoru klíčů, použijte `Remove-AzureRmKeyVault` příkaz s možností `-InRemovedState` a zadáním umístění odstraněného klíče trezoru se `-Location location` argument.</span><span class="sxs-lookup"><span data-stu-id="3f623-195">To purge a key vault, use the `Remove-AzureRmKeyVault` command with the option `-InRemovedState` and by specifying the location of the deleted key vault with the `-Location location` argument.</span></span> <span data-ttu-id="3f623-196">Můžete najít umístění odstraněného trezoru pomocí příkazu `Get-AzureRmKeyVault -InRemovedState`.</span><span class="sxs-lookup"><span data-stu-id="3f623-196">You can find the location of a deleted vault using the command `Get-AzureRmKeyVault -InRemovedState`.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
><span data-ttu-id="3f623-197">Vymazání trezoru klíčů se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="3f623-197">Purging a key vault will permanently delete it, meaning it will not be recoverable.</span></span>

### <a name="purge-permissions-required"></a><span data-ttu-id="3f623-198">Vyprázdnění oprávněních</span><span class="sxs-lookup"><span data-stu-id="3f623-198">Purge permissions required</span></span>
- <span data-ttu-id="3f623-199">K vyprázdnění odstraněné trezoru klíčů, tak, aby trezoru a veškerý jeho obsah jsou trvale odstraněny, musí uživatel oprávnění k provedení *Microsoft.KeyVault/locations/deletedVaults/purge/action* operaci.</span><span class="sxs-lookup"><span data-stu-id="3f623-199">To purge a deleted key vault, such that the vault and all its contents are permanently removed, the user needs RBAC permission to perform a *Microsoft.KeyVault/locations/deletedVaults/purge/action* operation.</span></span> 
- <span data-ttu-id="3f623-200">Pro zobrazení seznamu odstraněný klíč, trezor uživatel potřebuje oprávnění k provedení *Microsoft.KeyVault/deletedVaults/read* oprávnění.</span><span class="sxs-lookup"><span data-stu-id="3f623-200">To list the deleted key, the vault a user needs RBAC permission to perform *Microsoft.KeyVault/deletedVaults/read* permission.</span></span> 
- <span data-ttu-id="3f623-201">Pouze správce předplatného ve výchozím nastavení má tato oprávnění.</span><span class="sxs-lookup"><span data-stu-id="3f623-201">By default only a subscription administrator has these permissions.</span></span> 

### <a name="scheduled-purge"></a><span data-ttu-id="3f623-202">Naplánované vyprázdnění</span><span class="sxs-lookup"><span data-stu-id="3f623-202">Scheduled purge</span></span>

<span data-ttu-id="3f623-203">Výpis vaše objekty odstraněné trezoru klíčů zobrazí, kdy mají schedled vyprázdní pomocí Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3f623-203">Listing your deleted key vault objects shows when they are schedled to be purged by Key Vault.</span></span> <span data-ttu-id="3f623-204">*Naplánované datum vyprázdnění* pole označuje, když objekt trezoru klíčů se trvale odstraní, pokud nebyla provedena žádná akce.</span><span class="sxs-lookup"><span data-stu-id="3f623-204">The *Scheduled Purge Date* field indicates when a key vault object will be permanently deleted, if no action is taken.</span></span> <span data-ttu-id="3f623-205">Ve výchozím nastavení Doba uchování pro objekt odstraněného trezoru klíčů je 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="3f623-205">By default, the retention period for a deleted key vault object is 90 days.</span></span>

>[!NOTE]
><span data-ttu-id="3f623-206">Objekt vymazány trezoru, aktivuje její *naplánované datum vyprázdnění* pole, je trvale odstraněn.</span><span class="sxs-lookup"><span data-stu-id="3f623-206">A purged vault object, triggered by its *Scheduled Purge Date* field, is permanently deleted.</span></span> <span data-ttu-id="3f623-207">Není použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="3f623-207">It is not recoverable.</span></span>

## <a name="other-resources"></a><span data-ttu-id="3f623-208">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="3f623-208">Other resources</span></span>

- <span data-ttu-id="3f623-209">Přehled funkce obnovitelného odstranění Key Vault najdete v tématu [Přehled konfigurace soft odstranění Azure Key Vault](key-vault-ovw-soft-delete.md).</span><span class="sxs-lookup"><span data-stu-id="3f623-209">For an overview of Key Vault's soft-delete feature, see [Azure Key Vault soft-delete overview](key-vault-ovw-soft-delete.md).</span></span>
- <span data-ttu-id="3f623-210">Obecné informace o použití Azure Key Vault najdete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3f623-210">For a general overview of Azure Key Vault usage, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>

