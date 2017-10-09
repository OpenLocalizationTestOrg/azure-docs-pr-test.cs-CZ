---
ms.assetid: 
title: "aaaAzure klíč trezoru – jak toouse obnovitelného odstranění pomocí prostředí PowerShell"
description: "Pomocí prostředí PowerShell výstřižků kódu případu příklady konfigurace soft odstranění"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bruceper
ms.openlocfilehash: 4968b700a14f764ea1be7de2bf3697664f255f95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-key-vault-soft-delete-with-powershell"></a><span data-ttu-id="bf3f1-103">Jak toouse Key Vault obnovitelného odstranění pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="bf3f1-103">How toouse Key Vault soft-delete with PowerShell</span></span>

<span data-ttu-id="bf3f1-104">Azure Key Vault obnovitelného odstranění funkce umožňuje obnovení odstraněné trezory a objekty trezoru.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-104">Azure Key Vault's soft delete feature allows recovery of deleted vaults and vault objects.</span></span> <span data-ttu-id="bf3f1-105">Konkrétně konfigurace soft odstranění adresy hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-105">Specifically, soft-delete addresses hello following scenarios:</span></span>

- <span data-ttu-id="bf3f1-106">Podpora pro obnovitelné odstranění trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="bf3f1-106">Support for recoverable deletion of a key vault</span></span>
- <span data-ttu-id="bf3f1-107">Podpora pro obnovitelné odstranění trezoru klíčů objektů; klíče, tajné údaje a certifikáty</span><span class="sxs-lookup"><span data-stu-id="bf3f1-107">Support for recoverable deletion of key vault objects; keys, secrets, and, certificates</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf3f1-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bf3f1-108">Prerequisites</span></span>

- <span data-ttu-id="bf3f1-109">Prostředí Azure PowerShell 4.0.0 nebo novější – Pokud nepoužíváte již je tento instalační program, nainstalujte prostředí Azure PowerShell a přidružit ho ke svému předplatnému Azure, najdete [jak tooinstall a konfigurace prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bf3f1-109">Azure PowerShell 4.0.0 or later - If you don't have this already setup, install Azure PowerShell and associate it with your Azure subscription, see [How tooinstall and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span> 

>[!NOTE]
> <span data-ttu-id="bf3f1-110">Zastaralou verzi naše klíč trezoru PowerShell výstupní formátování souboru, který je **může** ho možné zavést do vašeho prostředí místo hello správnou verzi.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-110">There is an outdated version of our Key Vault PowerShell output formatting file that **may** be loaded into your environment instead of hello correct version.</span></span> <span data-ttu-id="bf3f1-111">Jsme se očekává, že aktualizovanou verzi prostředí PowerShell toocontain hello oprava pro výstup hello formátování a aktualizuje v tomto tématu v daném čase.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-111">We are anticipating an updated version of PowerShell toocontain hello needed correction for hello output formatting and will update this topic at that time.</span></span> <span data-ttu-id="bf3f1-112">Hello aktuální řešení, by měl narazíte na potíže formátování je:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-112">hello current workaround, should you encounter this formatting problem, is:</span></span>
> - <span data-ttu-id="bf3f1-113">Použití hello následující dotaz, pokud si všimnete nevidíte hello konfigurace soft odstranění povolena vlastnost popsaných v tomto tématu: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-113">Use hello following query if you notice you're not seeing hello soft-delete enabled property described in this topic: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span></span>


<span data-ttu-id="bf3f1-114">Informace o konkrétní refernece Key Vault pro prostředí PowerShell najdete v tématu [Azure Key Vault PowerShell odkaz](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span><span class="sxs-lookup"><span data-stu-id="bf3f1-114">For Key Vault specific refernece information for PowerShell, see [Azure Key Vault PowerShell reference](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span></span>

## <a name="required-permissions"></a><span data-ttu-id="bf3f1-115">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="bf3f1-115">Required permissions</span></span>

<span data-ttu-id="bf3f1-116">Key Vault operace jsou samostatně spravované prostřednictvím oprávnění k přístupu na základě rolí k řízení (RBAC) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-116">Key Vault operations are separately managed via role-based access control (RBAC) permissions as follows:</span></span>

| <span data-ttu-id="bf3f1-117">Operace</span><span class="sxs-lookup"><span data-stu-id="bf3f1-117">Operation</span></span> | <span data-ttu-id="bf3f1-118">Popis</span><span class="sxs-lookup"><span data-stu-id="bf3f1-118">Description</span></span> | <span data-ttu-id="bf3f1-119">Oprávnění uživatele</span><span class="sxs-lookup"><span data-stu-id="bf3f1-119">User permission</span></span> |
|:--|:--|:--|
|<span data-ttu-id="bf3f1-120">Seznam</span><span class="sxs-lookup"><span data-stu-id="bf3f1-120">List</span></span>|<span data-ttu-id="bf3f1-121">Seznamy odstraňovat trezorů klíčů.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-121">Lists deleted key vaults.</span></span>|<span data-ttu-id="bf3f1-122">Microsoft.KeyVault/deletedVaults/read</span><span class="sxs-lookup"><span data-stu-id="bf3f1-122">Microsoft.KeyVault/deletedVaults/read</span></span>|
|<span data-ttu-id="bf3f1-123">Zotavit</span><span class="sxs-lookup"><span data-stu-id="bf3f1-123">Recover</span></span>|<span data-ttu-id="bf3f1-124">Obnoví odstraněné trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-124">Restores a deleted key vault.</span></span>|<span data-ttu-id="bf3f1-125">Microsoft.KeyVault/vaults/write</span><span class="sxs-lookup"><span data-stu-id="bf3f1-125">Microsoft.KeyVault/vaults/write</span></span>|
|<span data-ttu-id="bf3f1-126">Vyprázdnit</span><span class="sxs-lookup"><span data-stu-id="bf3f1-126">Purge</span></span>|<span data-ttu-id="bf3f1-127">Trvale odstraní odstraněné trezoru klíčů a veškerý jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-127">Permanently removes a deleted key vault and all its contents.</span></span>|<span data-ttu-id="bf3f1-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span><span class="sxs-lookup"><span data-stu-id="bf3f1-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span></span>|

<span data-ttu-id="bf3f1-129">Další informace o oprávněních a řízení přístupu najdete v tématu [zabezpečit váš trezor klíčů](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="bf3f1-129">For more information on permissions and access control, see [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>

## <a name="enabling-soft-delete"></a><span data-ttu-id="bf3f1-130">Povolení konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="bf3f1-130">Enabling soft-delete</span></span>

<span data-ttu-id="bf3f1-131">možnost toorecover toobe odstraněné trezoru klíčů nebo objekty uložené v klíč trezoru, musíte nejdřív povolit konfigurace soft odstranění tohoto klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-131">toobe able toorecover a deleted key vault or objects stored in a key vault, you must first enable soft-delete for that key vault.</span></span>

### <a name="existing-key-vault"></a><span data-ttu-id="bf3f1-132">Existující trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="bf3f1-132">Existing key vault</span></span>

<span data-ttu-id="bf3f1-133">Pro existující trezor klíčů s názvem ContosoVault následujícím způsobem povolte obnovitelného odstranění.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-133">For an existing key vault named ContosoVault, enable soft-delete as follows.</span></span> 

>[!NOTE]
><span data-ttu-id="bf3f1-134">Aktuálně je třeba toouse Azure Resource Manager prostředků manipulaci s toodirectly zápisu hello *enableSoftDelete* toohello vlastnost prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-134">Currently you need toouse Azure Resource Manager resource manipulation toodirectly write hello *enableSoftDelete* property toohello Key Vault resource.</span></span>

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a><span data-ttu-id="bf3f1-135">Nový trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="bf3f1-135">New key vault</span></span>

<span data-ttu-id="bf3f1-136">Povolení konfigurace soft odstranění pro nového trezoru klíčů se provádí v okamžiku vytvoření přidáním hello konfigurace soft odstranění povolit příznak tooyour vytvoření příkazu.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-136">Enabling soft-delete for a new key vault is done at creation time by adding hello soft-delete enable flag tooyour create command.</span></span>

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a><span data-ttu-id="bf3f1-137">Ověřte povolování konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="bf3f1-137">Verify soft-delete enablement</span></span>

<span data-ttu-id="bf3f1-138">tooverify, který trezoru klíčů má konfigurace soft odstranit povolený, spusťte hello *získat* příkazů a vyhledejte hello "Soft odstranit povolen?"</span><span class="sxs-lookup"><span data-stu-id="bf3f1-138">tooverify that a key vault has soft-delete enabled, run hello *get* command and look for hello 'Soft Delete Enabled?'</span></span> <span data-ttu-id="bf3f1-139">atribut a jeho nastavení true nebo false.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-139">attribute and its setting, true or false.</span></span>

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a><span data-ttu-id="bf3f1-140">Odstranění trezoru klíčů chráněn konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="bf3f1-140">Deleting a key vault protected by soft-delete</span></span>

<span data-ttu-id="bf3f1-141">Hello příkaz toodelete (nebo odebrat) zůstane trezoru klíčů hello stejné, ale její změny chování v závislosti na tom, jestli jste povolili obnovitelného odstranění nebo ne.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-141">hello command toodelete (or remove) a key vault remains hello same, but its behavior changes depending on whether you have enabled soft-delete or not.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
><span data-ttu-id="bf3f1-142">Pokud spustíte předchozí příkaz hello pro trezor klíčů, který nemá konfigurace soft odstranění povoleno, bude trvale odstranit tento trezor klíčů a veškerý jeho obsah bez jakékoli možnosti pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-142">If you run hello previous command for a key vault that does not have soft-delete enabled, you will permanently delete this key vault and all its content without any options for recovery.</span></span>

### <a name="how-soft-delete-protects-your-key-vaults"></a><span data-ttu-id="bf3f1-143">Jak odstranit soft chrání vaše trezorů klíčů</span><span class="sxs-lookup"><span data-stu-id="bf3f1-143">How soft-delete protects your key vaults</span></span>

<span data-ttu-id="bf3f1-144">S konfigurace soft odstranění povoleno:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-144">With soft-delete enabled:</span></span>

- <span data-ttu-id="bf3f1-145">Při odstranění trezoru klíčů není odebrán z jeho skupin prostředků a je umístěn v vyhrazené oboru názvů, který je pouze přidružené hello umístění, kde se vytvořila.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-145">When a key vault is deleted, it is removed from its resource group and placed in a reserved namespace that is only associated with hello location where it was created.</span></span> 
- <span data-ttu-id="bf3f1-146">Objekty odstraněné klíč trezoru, jako jsou nedostupné klíče, tajných klíčů a certifikátů a tak zůstanou při jejich obsahující trezoru klíčů je ve stavu hello odstranit.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-146">Objects in a deleted key vault, such as keys, secrets and, certificates, are inaccessible and remain so while their containing key vault is in hello deleted state.</span></span> 
- <span data-ttu-id="bf3f1-147">název DNS Hello trezoru klíčů v odstraněném stavu je stále vyhrazené, proto nelze vytvořit nový trezor klíčů se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-147">hello DNS name for a key vault in a deleted state is still reserved so, a new key vault with same name cannot be created.</span></span>  

<span data-ttu-id="bf3f1-148">Může zobrazit trezorů klíčů stavu deleted, spojené s vaším předplatným pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-148">You may view deleted state key vaults, associated with your subscription, using hello following command:</span></span>

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

<span data-ttu-id="bf3f1-149">Hello *ID prostředku* v hello výstup odkazuje toohello původní ID prostředku trezoru.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-149">hello *Resource ID* in hello output refers toohello original resource ID of this vault.</span></span> <span data-ttu-id="bf3f1-150">Vzhledem k tomu, že tento trezor klíčů je teď ve stavu deleted, neexistuje žádný prostředek s ID tohoto zdroje.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-150">Since this key vault is now in a deleted state, no resource exists with that resource ID.</span></span> <span data-ttu-id="bf3f1-151">Hello *Id* pole lze použít tooidentify hello prostředků při obnovení nebo vyprazdňování.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-151">hello *Id* field can be used tooidentify hello resource when recovering, or purging.</span></span> <span data-ttu-id="bf3f1-152">Hello *naplánované datum vyprázdnění* pole označuje, když se trvale odstraní trezoru hello (Vymazat) Pokud nebyla provedena žádná akce pro tento trezor odstraněné.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-152">hello *Scheduled Purge Date* field indicates when hello vault will be permanently deleted (purged) if no action is taken for this deleted vault.</span></span> <span data-ttu-id="bf3f1-153">Hello výchozí uchování období, využité toocalculate hello *naplánované Vyprázdnit data*, je 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-153">hello default retention period, used toocalculate hello *Scheduled Purge Date*, is 90 days.</span></span>

## <a name="recovering-a-key-vault"></a><span data-ttu-id="bf3f1-154">Obnovení trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="bf3f1-154">Recovering a key vault</span></span>

<span data-ttu-id="bf3f1-155">toorecover trezoru klíčů, musíte název trezoru klíčů hello toospecify, skupinu prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-155">toorecover a key vault, you need toospecify hello key vault name, resource group, and location.</span></span> <span data-ttu-id="bf3f1-156">Poznámka: hello umístění a skupiny prostředků hello hello odstranit trezor klíčů, jako je třeba tyto klíče trezoru procesu obnovení.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-156">Note hello location and hello resource group of hello deleted key vault as you need these for a key vault recovery process.</span></span>

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

<span data-ttu-id="bf3f1-157">Když je obnovena trezoru klíčů, výsledkem hello je nový prostředek s ID hello trezoru klíčů původního zdroje.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-157">When a key vault is recovered, hello result is a new resource with hello key vault's original resource ID.</span></span> <span data-ttu-id="bf3f1-158">Pokud byla odebrána skupina prostředků hello kde existovaly hello trezoru klíčů, musí před trezoru klíčů hello lze obnovit vytvořit novou skupinu prostředků se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-158">If hello resource group where hello key vault existed has been removed, a new resource group with same name must be created before hello key vault can be recovered.</span></span>

## <a name="key-vault-objects-and-soft-delete"></a><span data-ttu-id="bf3f1-159">Key Vault objekty a konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="bf3f1-159">Key Vault objects and soft-delete</span></span>

<span data-ttu-id="bf3f1-160">Pro klíč 'ContosoFirstKey' v trezoru klíčů s názvem 'ContosoVault' s konfigurace soft odstranění povoleno, zde je způsob odstranili byste klíči.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-160">For a key, 'ContosoFirstKey', in a key vault named 'ContosoVault' with soft-delete enabled, here's how you would delete that key.</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="bf3f1-161">S vaší povolené pro obnovitelného odstranění trezoru klíčů zobrazí odstraněný klíč i stejně, jako je odstraněn s výjimkou, že při explicitně seznamu nebo načtení odstraněné klíčů.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-161">With your key vault enabled for soft-delete, a deleted key still appears like it's deleted except, when you explicitly list or retrieve deleted keys.</span></span> <span data-ttu-id="bf3f1-162">Většinu operací na klíč v hello odstranit stavu se nezdaří s výjimkou výpis odstraněný klíč, obnovení nebo vyprazdňování ho.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-162">Most operations on a key in hello deleted state will fail except for listing a deleted key, recovering it or purging it.</span></span> 

<span data-ttu-id="bf3f1-163">Například toorequest toolist odstranit klíčů v trezoru klíčů, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-163">For example, toorequest toolist deleted keys in a key vault, use hello following command:</span></span>

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a><span data-ttu-id="bf3f1-164">Přechodový stav.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-164">Transition state</span></span> 

<span data-ttu-id="bf3f1-165">Pokud odstraníte klíč v trezoru klíčů s konfigurace soft odstranění povolené, může trvat několik sekund, než toocomplete přechod hello.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-165">When you delete a key in a key vault with soft-delete enabled, it may take a few seconds for hello transition toocomplete.</span></span> <span data-ttu-id="bf3f1-166">Při tomto přechod stavu může zdát, klíči hello není v aktivním stavu hello nebo hello odstranit stavu.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-166">During this transition state, it may appear that hello key is not in hello active state or hello deleted state.</span></span> <span data-ttu-id="bf3f1-167">Tento příkaz zobrazí seznam všech odstraněných klíčů v trezoru klíčů s názvem 'ContosoVault'.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-167">This command will list all deleted keys in your key vault named 'ContosoVault'.</span></span>

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

### <a name="using-soft-delete-with-key-vault-objects"></a><span data-ttu-id="bf3f1-168">Pomocí konfigurace soft odstranění trezoru klíčů objekty</span><span class="sxs-lookup"><span data-stu-id="bf3f1-168">Using soft-delete with key vault objects</span></span>

<span data-ttu-id="bf3f1-169">Jenom jako trezorů klíčů, odstraněný klíč tajný klíč nebo certifikát zůstane v odstraněném stavu pro až dny too90 Pokud jej obnovit, nebo ji vymazat.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-169">Just like key vaults, a deleted key, secret or, certificate will remain in deleted state for up too90 days unless you recover it or purge it.</span></span> 

#### <a name="keys"></a><span data-ttu-id="bf3f1-170">Klíče</span><span class="sxs-lookup"><span data-stu-id="bf3f1-170">Keys</span></span>

<span data-ttu-id="bf3f1-171">toorecover odstraněný klíč:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-171">toorecover a deleted key:</span></span>

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="bf3f1-172">toopermanently odstranit klíč:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-172">toopermanently delete a key:</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
><span data-ttu-id="bf3f1-173">Vymazání klíč se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-173">Purging a key will permanently delete it, meaning it will not be recoverable.</span></span>

<span data-ttu-id="bf3f1-174">Hello **obnovit** a **mazání** akce mají své vlastní oprávnění v zásadách přístupu k trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-174">hello **recover** and **purge** actions have their own permissions associated in a key vault access policy.</span></span> <span data-ttu-id="bf3f1-175">Pro uživatele nebo službu hlavní toobe možné tooexecute **obnovit** nebo **mazání** akce hello příslušných oprávnění pro tento objekt (klíč nebo tajný klíč) musí mít v zásadách přístupu hello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-175">For a user or service principal toobe able tooexecute a **recover** or **purge** action they must have hello respective permission for that object (key or secret) in hello key vault access policy.</span></span> <span data-ttu-id="bf3f1-176">Ve výchozím nastavení, hello **mazání** oprávnění nebyla přidána zásady přístupu tooa trezoru klíčů, po hello "vše" zástupce použité toogrant všechny uživatelské tooa oprávnění.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-176">By default, hello **purge** permission is not added tooa key vault's access policy when hello 'all' shortcut is used toogrant all permissions tooa user.</span></span> <span data-ttu-id="bf3f1-177">Je třeba explicitně udělit **mazání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-177">You must explicitly grant **purge** permission.</span></span> <span data-ttu-id="bf3f1-178">Například hello následující příkaz uděluje user@contoso.com oprávnění tooperform několik operací na klíče ve *ContosoVault* včetně **mazání**.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-178">For example, hello following command grants user@contoso.com permission tooperform several operations on keys in *ContosoVault* including **purge**.</span></span>

#### <a name="set-a-key-vault-access-policy"></a><span data-ttu-id="bf3f1-179">Nastavit zásady přístupu k trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="bf3f1-179">Set a key vault access policy</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> <span data-ttu-id="bf3f1-180">Pokud máte existující trezor klíčů, který má právě měl konfigurace soft odstranění povoleno, nemusí mít **obnovit** a **mazání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-180">If you have an existing key vault that has just had soft-delete enabled, you may not have **recover** and **purge** permissions.</span></span>

#### <a name="secrets"></a><span data-ttu-id="bf3f1-181">Tajné kódy</span><span class="sxs-lookup"><span data-stu-id="bf3f1-181">Secrets</span></span>

<span data-ttu-id="bf3f1-182">Podobně jako klíče jsou tajných klíčů v trezoru klíčů provozovat na s vlastní příkazy.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-182">Like keys, secrets in a key vault are operated on with their own commands.</span></span> <span data-ttu-id="bf3f1-183">Následující, jsou hello příkazy pro odstranění, výpis, obnovení a mazání tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-183">Following, are hello commands for deleting, listing, recovering, and purging secrets.</span></span>

- <span data-ttu-id="bf3f1-184">Odstranění tajného klíče s názvem SQLPassword:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-184">Delete a secret named SQLPassword:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- <span data-ttu-id="bf3f1-185">Zobrazí seznam všech odstraněných tajných klíčů v trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-185">List all deleted secrets in a key vault:</span></span> 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- <span data-ttu-id="bf3f1-186">Tajný klíč v hello odstranit stavu obnovení:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-186">Recover a secret in hello deleted state:</span></span> 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- <span data-ttu-id="bf3f1-187">Vyprázdnění tajný klíč v odstraněném stavu:</span><span class="sxs-lookup"><span data-stu-id="bf3f1-187">Purge a secret in deleted state:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
><span data-ttu-id="bf3f1-188">Vymazání tajného klíče se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-188">Purging a secret will permanently delete it, meaning it will not be recoverable.</span></span>

## <a name="purging-and-key-vaults"></a><span data-ttu-id="bf3f1-189">Trezory mazání a klíč</span><span class="sxs-lookup"><span data-stu-id="bf3f1-189">Purging and key vaults</span></span>

### <a name="key-vault-objects"></a><span data-ttu-id="bf3f1-190">Objekty trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="bf3f1-190">Key vault objects</span></span>

<span data-ttu-id="bf3f1-191">Vymazání klíč, tajný klíč nebo certifikát se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-191">Purging a key, secret or, certificate will permanently delete it, meaning it will not be recoverable.</span></span> <span data-ttu-id="bf3f1-192">Hello trezoru klíčů, který obsahuje hello odstranit objekt se ale zachovají stejně jako všechny ostatní objekty v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-192">hello key vault that contained hello deleted object will however remain intact as will all other objects in hello key vault.</span></span> 

### <a name="key-vaults-as-containers"></a><span data-ttu-id="bf3f1-193">Klíč trezory jako kontejnery</span><span class="sxs-lookup"><span data-stu-id="bf3f1-193">Key vaults as containers</span></span>
<span data-ttu-id="bf3f1-194">Když se vyprazdňují trezoru klíčů, veškerý její obsah, včetně klíče a tajné klíče, certifikáty, se trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-194">When a key vault is purged, all of its contents, including keys, secrets, and certificates, are permanently deleted.</span></span> <span data-ttu-id="bf3f1-195">toopurge trezoru klíčů, použijte hello `Remove-AzureRmKeyVault` příkaz s možností hello `-InRemovedState` a zadáním umístění hello trezor klíčů odstranit hello se hello `-Location location` argument.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-195">toopurge a key vault, use hello `Remove-AzureRmKeyVault` command with hello option `-InRemovedState` and by specifying hello location of hello deleted key vault with hello `-Location location` argument.</span></span> <span data-ttu-id="bf3f1-196">Můžete vyhledat umístění hello odstraněné úložiště pomocí příkazu hello `Get-AzureRmKeyVault -InRemovedState`.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-196">You can find hello location of a deleted vault using hello command `Get-AzureRmKeyVault -InRemovedState`.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
><span data-ttu-id="bf3f1-197">Vymazání trezoru klíčů se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-197">Purging a key vault will permanently delete it, meaning it will not be recoverable.</span></span>

### <a name="purge-permissions-required"></a><span data-ttu-id="bf3f1-198">Vyprázdnění oprávněních</span><span class="sxs-lookup"><span data-stu-id="bf3f1-198">Purge permissions required</span></span>
- <span data-ttu-id="bf3f1-199">toopurge odstraněné trezoru klíčů, tak, aby hello trezoru a veškerý jeho obsah jsou trvale odstraněny, hello uživatel potřebuje tooperform oprávnění RBAC *Microsoft.KeyVault/locations/deletedVaults/purge/action* operaci.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-199">toopurge a deleted key vault, such that hello vault and all its contents are permanently removed, hello user needs RBAC permission tooperform a *Microsoft.KeyVault/locations/deletedVaults/purge/action* operation.</span></span> 
- <span data-ttu-id="bf3f1-200">toolist hello odstranit klíč trezoru hello uživatel potřebuje tooperform oprávnění RBAC *Microsoft.KeyVault/deletedVaults/read* oprávnění.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-200">toolist hello deleted key, hello vault a user needs RBAC permission tooperform *Microsoft.KeyVault/deletedVaults/read* permission.</span></span> 
- <span data-ttu-id="bf3f1-201">Pouze správce předplatného ve výchozím nastavení má tato oprávnění.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-201">By default only a subscription administrator has these permissions.</span></span> 

### <a name="scheduled-purge"></a><span data-ttu-id="bf3f1-202">Naplánované vyprázdnění</span><span class="sxs-lookup"><span data-stu-id="bf3f1-202">Scheduled purge</span></span>

<span data-ttu-id="bf3f1-203">Výpis vaše objekty odstraněné trezoru klíčů zobrazuje, když jsou odstraněna podle Key Vault toobe schedled.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-203">Listing your deleted key vault objects shows when they are schedled toobe purged by Key Vault.</span></span> <span data-ttu-id="bf3f1-204">Hello *naplánované datum vyprázdnění* pole označuje, když objekt trezoru klíčů se trvale odstraní, pokud nebyla provedena žádná akce.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-204">hello *Scheduled Purge Date* field indicates when a key vault object will be permanently deleted, if no action is taken.</span></span> <span data-ttu-id="bf3f1-205">Ve výchozím nastavení hello doba uchování pro objekt odstraněného trezoru klíčů je 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-205">By default, hello retention period for a deleted key vault object is 90 days.</span></span>

>[!NOTE]
><span data-ttu-id="bf3f1-206">Objekt vymazány trezoru, aktivuje její *naplánované datum vyprázdnění* pole, je trvale odstraněn.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-206">A purged vault object, triggered by its *Scheduled Purge Date* field, is permanently deleted.</span></span> <span data-ttu-id="bf3f1-207">Není použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="bf3f1-207">It is not recoverable.</span></span>

## <a name="other-resources"></a><span data-ttu-id="bf3f1-208">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="bf3f1-208">Other resources</span></span>

- <span data-ttu-id="bf3f1-209">Přehled funkce obnovitelného odstranění Key Vault najdete v tématu [Přehled konfigurace soft odstranění Azure Key Vault](key-vault-ovw-soft-delete.md).</span><span class="sxs-lookup"><span data-stu-id="bf3f1-209">For an overview of Key Vault's soft-delete feature, see [Azure Key Vault soft-delete overview](key-vault-ovw-soft-delete.md).</span></span>
- <span data-ttu-id="bf3f1-210">Obecné informace o použití Azure Key Vault najdete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bf3f1-210">For a general overview of Azure Key Vault usage, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>

