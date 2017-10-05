---
ms.assetid: 
title: "Azure Key Vault - Použití obnovitelného odstranění pomocí rozhraní příkazového řádku"
description: "Pomocí rozhraní příkazového řádku výstřižků kódu případu příklady konfigurace soft odstranění"
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: bruceper
ms.openlocfilehash: 3ee2c5dfb99d734cde25894174466b8e49823c67
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-key-vault-soft-delete-with-cli"></a><span data-ttu-id="fa20e-103">Jak používat Key Vault konfigurace soft odstranění pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="fa20e-103">How to use Key Vault soft-delete with CLI</span></span>

<span data-ttu-id="fa20e-104">Azure Key Vault obnovitelného odstranění funkce umožňuje obnovení odstraněné trezory a objekty trezoru.</span><span class="sxs-lookup"><span data-stu-id="fa20e-104">Azure Key Vault's soft delete feature allows recovery of deleted vaults and vault objects.</span></span> <span data-ttu-id="fa20e-105">Konkrétně obnovitelného odstranění adresy následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="fa20e-105">Specifically, soft-delete addresses the following scenarios:</span></span>

- <span data-ttu-id="fa20e-106">Podpora pro obnovitelné odstranění trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="fa20e-106">Support for recoverable deletion of a key vault</span></span>
- <span data-ttu-id="fa20e-107">Podpora pro obnovitelné odstranění trezoru klíčů objektů; klíče, tajné údaje a certifikáty</span><span class="sxs-lookup"><span data-stu-id="fa20e-107">Support for recoverable deletion of key vault objects; keys, secrets, and, certificates</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa20e-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fa20e-108">Prerequisites</span></span>

- <span data-ttu-id="fa20e-109">Rozhraní příkazového řádku Azure 2.0 - Pokud nemáte tento instalační program pro vaše prostředí, najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku 2.0](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="fa20e-109">Azure CLI 2.0 - If you don't have this setup for your environment, see [Manage Key Vault using CLI 2.0](key-vault-manage-with-cli2.md).</span></span>

<span data-ttu-id="fa20e-110">Odkaz na konkrétní informace Key Vault pro rozhraní příkazového řádku najdete v tématu [Azure CLI 2.0 Key Vault odkaz](https://docs.microsoft.com/cli/azure/keyvault).</span><span class="sxs-lookup"><span data-stu-id="fa20e-110">For Key Vault specific reference information for CLI, see [Azure CLI 2.0 Key Vault reference](https://docs.microsoft.com/cli/azure/keyvault).</span></span>

## <a name="required-permissions"></a><span data-ttu-id="fa20e-111">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="fa20e-111">Required permissions</span></span>

<span data-ttu-id="fa20e-112">Key Vault operace jsou samostatně spravované prostřednictvím oprávnění k přístupu na základě rolí k řízení (RBAC) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fa20e-112">Key Vault operations are separately managed via role-based access control (RBAC) permissions as follows:</span></span>

| <span data-ttu-id="fa20e-113">Operace</span><span class="sxs-lookup"><span data-stu-id="fa20e-113">Operation</span></span> | <span data-ttu-id="fa20e-114">Popis</span><span class="sxs-lookup"><span data-stu-id="fa20e-114">Description</span></span> | <span data-ttu-id="fa20e-115">Oprávnění uživatele</span><span class="sxs-lookup"><span data-stu-id="fa20e-115">User permission</span></span> |
|:--|:--|:--|
|<span data-ttu-id="fa20e-116">Seznam</span><span class="sxs-lookup"><span data-stu-id="fa20e-116">List</span></span>|<span data-ttu-id="fa20e-117">Seznamy odstraňovat trezorů klíčů.</span><span class="sxs-lookup"><span data-stu-id="fa20e-117">Lists deleted key vaults.</span></span>|<span data-ttu-id="fa20e-118">Microsoft.KeyVault/deletedVaults/read</span><span class="sxs-lookup"><span data-stu-id="fa20e-118">Microsoft.KeyVault/deletedVaults/read</span></span>|
|<span data-ttu-id="fa20e-119">Zotavit</span><span class="sxs-lookup"><span data-stu-id="fa20e-119">Recover</span></span>|<span data-ttu-id="fa20e-120">Obnoví odstraněné trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="fa20e-120">Restores a deleted key vault.</span></span>|<span data-ttu-id="fa20e-121">Microsoft.KeyVault/vaults/write</span><span class="sxs-lookup"><span data-stu-id="fa20e-121">Microsoft.KeyVault/vaults/write</span></span>|
|<span data-ttu-id="fa20e-122">Vyprázdnit</span><span class="sxs-lookup"><span data-stu-id="fa20e-122">Purge</span></span>|<span data-ttu-id="fa20e-123">Trvale odstraní odstraněné trezoru klíčů a veškerý jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="fa20e-123">Permanently removes a deleted key vault and all its contents.</span></span>|<span data-ttu-id="fa20e-124">Microsoft.KeyVault/locations/deletedVaults/purge/action</span><span class="sxs-lookup"><span data-stu-id="fa20e-124">Microsoft.KeyVault/locations/deletedVaults/purge/action</span></span>|

<span data-ttu-id="fa20e-125">Další informace o oprávněních a řízení přístupu najdete v tématu [zabezpečit váš trezor klíčů](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="fa20e-125">For more information on permissions and access control, see [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>

## <a name="enabling-soft-delete"></a><span data-ttu-id="fa20e-126">Povolení konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="fa20e-126">Enabling soft-delete</span></span>

<span data-ttu-id="fa20e-127">Abyste mohli obnovit odstraněné trezoru klíčů nebo objekty uložené v trezoru klíčů, musíte nejdřív povolit konfigurace soft odstranění tohoto klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="fa20e-127">To be able to recover a deleted key vault or objects stored in a key vault, you must first enable soft-delete for that key vault.</span></span>

### <a name="existing-key-vault"></a><span data-ttu-id="fa20e-128">Existující trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="fa20e-128">Existing key vault</span></span>

<span data-ttu-id="fa20e-129">Pro existující trezor klíčů s názvem ContosoVault následujícím způsobem povolte obnovitelného odstranění.</span><span class="sxs-lookup"><span data-stu-id="fa20e-129">For an existing key vault named ContosoVault, enable soft-delete as follows.</span></span> 

>[!NOTE]
><span data-ttu-id="fa20e-130">Aktuálně je nutné použít zpracování prostředků Azure Resource Manager zápis přímo *enableSoftDelete* vlastnost prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="fa20e-130">Currently you need to use Azure Resource Manager resource manipulation to directly write the *enableSoftDelete* property to the Key Vault resource.</span></span>

```azurecli
az resource update --id $(az keyvault show --name ContosoVault -o tsv | awk '{print $1}') --set properties.enableSoftDelete=true
```

### <a name="new-key-vault"></a><span data-ttu-id="fa20e-131">Nový trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="fa20e-131">New key vault</span></span>

<span data-ttu-id="fa20e-132">Povolení konfigurace soft odstranění pro nového trezoru klíčů se provádí v okamžiku vytvoření přidáním příznak konfigurace soft odstranění povolit, aby vaše vytvoření příkazu.</span><span class="sxs-lookup"><span data-stu-id="fa20e-132">Enabling soft-delete for a new key vault is done at creation time by adding the soft-delete enable flag to your create command.</span></span>

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --enable-soft-delete true --location westus
```

### <a name="verify-soft-delete-enablement"></a><span data-ttu-id="fa20e-133">Ověřte povolování konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="fa20e-133">Verify soft-delete enablement</span></span>

<span data-ttu-id="fa20e-134">Ověřte, že trezoru klíčů má konfigurace soft odstranění povolena, spusťte *zobrazit* příkazů a podívejte se 'logicky odstranit povolen?"</span><span class="sxs-lookup"><span data-stu-id="fa20e-134">To verify that a key vault has soft-delete enabled, run the *show* command and look for the 'Soft Delete Enabled?'</span></span> <span data-ttu-id="fa20e-135">atribut a jeho nastavení true nebo false.</span><span class="sxs-lookup"><span data-stu-id="fa20e-135">attribute and its setting, true or false.</span></span>

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a><span data-ttu-id="fa20e-136">Odstranění trezoru klíčů chráněn konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="fa20e-136">Deleting a key vault protected by soft-delete</span></span>

<span data-ttu-id="fa20e-137">Příkaz k odstranění (nebo odebrání) trezoru klíčů zůstane stejný, ale její chování mění v závislosti na tom, jestli jste povolili obnovitelného odstranění nebo ne.</span><span class="sxs-lookup"><span data-stu-id="fa20e-137">The command to delete (or remove) a key vault remains the same, but its behavior changes depending on whether you have enabled soft-delete or not.</span></span>

```azurecli
az keyvault delete --name ContosoVault
```

> [!IMPORTANT]
><span data-ttu-id="fa20e-138">Pokud spustíte předchozí příkaz pro trezor klíčů, který nemá konfigurace soft odstranění povoleno, bude trvale odstranit tento trezor klíčů a veškerý jeho obsah bez jakékoli možnosti pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="fa20e-138">If you run the previous command for a key vault that does not have soft-delete enabled, you will permanently delete this key vault and all its content without any options for recovery.</span></span>

### <a name="how-soft-delete-protects-your-key-vaults"></a><span data-ttu-id="fa20e-139">Jak odstranit soft chrání vaše trezorů klíčů</span><span class="sxs-lookup"><span data-stu-id="fa20e-139">How soft-delete protects your key vaults</span></span>

<span data-ttu-id="fa20e-140">S konfigurace soft odstranění povoleno:</span><span class="sxs-lookup"><span data-stu-id="fa20e-140">With soft-delete enabled:</span></span>

- <span data-ttu-id="fa20e-141">Při odstranění trezoru klíčů není odebrán z jeho skupin prostředků a je umístěn v vyhrazené oboru názvů, který je pouze přidružená k umístění, kde se vytvořila.</span><span class="sxs-lookup"><span data-stu-id="fa20e-141">When a key vault is deleted, it is removed from its resource group and placed in a reserved namespace that is only associated with the location where it was created.</span></span> 
- <span data-ttu-id="fa20e-142">Objekty odstraněné klíč trezoru, jako jsou nedostupné klíče, tajných klíčů a certifikátů a tak zůstanou při jejich obsahující trezoru klíčů je ve stavu deleted.</span><span class="sxs-lookup"><span data-stu-id="fa20e-142">Objects in a deleted key vault, such as keys, secrets and, certificates, are inaccessible and remain so while their containing key vault is in the deleted state.</span></span> 
- <span data-ttu-id="fa20e-143">Název DNS pro trezoru klíčů v odstraněném stavu je stále vyhrazené proto nelze vytvořit nový trezor klíčů se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="fa20e-143">The DNS name for a key vault in a deleted state is still reserved so, a new key vault with same name cannot be created.</span></span>  

<span data-ttu-id="fa20e-144">Můžete si zobrazit trezorů klíčů stavu deleted, spojené s vaším předplatným, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="fa20e-144">You may view deleted state key vaults, associated with your subscription, using the following command:</span></span>

```azurecli
az keyvault list-deleted
```

<span data-ttu-id="fa20e-145">*ID prostředku* ve výstupu odkazuje na původní ID prostředku trezoru.</span><span class="sxs-lookup"><span data-stu-id="fa20e-145">The *Resource ID* in the output refers to the original resource ID of this vault.</span></span> <span data-ttu-id="fa20e-146">Vzhledem k tomu, že tento trezor klíčů je teď ve stavu deleted, neexistuje žádný prostředek s ID tohoto zdroje.</span><span class="sxs-lookup"><span data-stu-id="fa20e-146">Since this key vault is now in a deleted state, no resource exists with that resource ID.</span></span> <span data-ttu-id="fa20e-147">*Id* pole lze použít k identifikaci prostředků při obnovení nebo vyprazdňování.</span><span class="sxs-lookup"><span data-stu-id="fa20e-147">The *Id* field can be used to identify the resource when recovering, or purging.</span></span> <span data-ttu-id="fa20e-148">*Naplánované datum vyprázdnění* pole určuje, kdy se trvale odstraní trezoru (Vymazat) Pokud nebyla provedena žádná akce pro tento trezor odstraněné.</span><span class="sxs-lookup"><span data-stu-id="fa20e-148">The *Scheduled Purge Date* field indicates when the vault will be permanently deleted (purged) if no action is taken for this deleted vault.</span></span> <span data-ttu-id="fa20e-149">Výchozí dobu uchování, používá k výpočtu *naplánované Vyprázdnit data*, je 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="fa20e-149">The default retention period, used to calculate the *Scheduled Purge Date*, is 90 days.</span></span>

## <a name="recovering-a-key-vault"></a><span data-ttu-id="fa20e-150">Obnovení trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="fa20e-150">Recovering a key vault</span></span>

<span data-ttu-id="fa20e-151">Pokud chcete obnovit trezoru klíčů, je třeba zadat název trezoru klíčů, skupinu prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="fa20e-151">To recover a key vault, you need to specify the key vault name, resource group, and location.</span></span> <span data-ttu-id="fa20e-152">Všimněte si umístění a skupiny prostředků odstraněné trezoru klíčů, jako je třeba tyto klíče trezoru procesu obnovení.</span><span class="sxs-lookup"><span data-stu-id="fa20e-152">Note the location and the resource group of the deleted key vault as you need these for a key vault recovery process.</span></span>

```azurecli
az keyvault recover --location westus --name ContosoVault
```

<span data-ttu-id="fa20e-153">Když je obnovena trezoru klíčů, výsledkem je nový prostředek s ID trezoru klíčů původního zdroje.</span><span class="sxs-lookup"><span data-stu-id="fa20e-153">When a key vault is recovered, the result is a new resource with the key vault's original resource ID.</span></span> <span data-ttu-id="fa20e-154">Pokud byl odebrán skupině prostředků, které existovalo trezoru klíčů, musí před trezoru klíčů lze obnovit vytvořit novou skupinu prostředků se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="fa20e-154">If the resource group where the key vault existed has been removed, a new resource group with same name must be created before the key vault can be recovered.</span></span>

## <a name="key-vault-objects-and-soft-delete"></a><span data-ttu-id="fa20e-155">Key Vault objekty a konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="fa20e-155">Key Vault objects and soft-delete</span></span>

<span data-ttu-id="fa20e-156">Pro klíč 'ContosoFirstKey' v trezoru klíčů s názvem 'ContosoVault' s konfigurace soft odstranění povoleno, zde je způsob odstranili byste klíči.</span><span class="sxs-lookup"><span data-stu-id="fa20e-156">For a key, 'ContosoFirstKey', in a key vault named 'ContosoVault' with soft-delete enabled, here's how you would delete that key.</span></span>

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

<span data-ttu-id="fa20e-157">S vaší povolené pro obnovitelného odstranění trezoru klíčů zobrazí odstraněný klíč i stejně, jako je odstraněn s výjimkou, že při explicitně seznamu nebo načtení odstraněné klíčů.</span><span class="sxs-lookup"><span data-stu-id="fa20e-157">With your key vault enabled for soft-delete, a deleted key still appears like it's deleted except, when you explicitly list or retrieve deleted keys.</span></span> <span data-ttu-id="fa20e-158">Většinu operací pro klíč ve stavu deleted se nezdaří s výjimkou výpis odstraněný klíč, obnovení nebo vyprazdňování ho.</span><span class="sxs-lookup"><span data-stu-id="fa20e-158">Most operations on a key in the deleted state will fail except for listing a deleted key, recovering it or purging it.</span></span> 

<span data-ttu-id="fa20e-159">Například požadavek na seznamu odstranit klíčů v trezoru klíčů, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fa20e-159">For example, to request to list deleted keys in a key vault, use the following command:</span></span>

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a><span data-ttu-id="fa20e-160">Přechodový stav.</span><span class="sxs-lookup"><span data-stu-id="fa20e-160">Transition state</span></span> 

<span data-ttu-id="fa20e-161">Pokud odstraníte klíč v trezoru klíčů s konfigurace soft odstranění povolené, může trvat několik sekund pro přechod k dokončení.</span><span class="sxs-lookup"><span data-stu-id="fa20e-161">When you delete a key in a key vault with soft-delete enabled, it may take a few seconds for the transition to complete.</span></span> <span data-ttu-id="fa20e-162">Při tomto přechod stavu může zdát, že klíč není v aktivním stavu nebo odstraněném stavu.</span><span class="sxs-lookup"><span data-stu-id="fa20e-162">During this transition state, it may appear that the key is not in the active state or the deleted state.</span></span> <span data-ttu-id="fa20e-163">Tento příkaz zobrazí seznam všech odstraněných klíčů v trezoru klíčů s názvem 'ContosoVault'.</span><span class="sxs-lookup"><span data-stu-id="fa20e-163">This command will list all deleted keys in your key vault named 'ContosoVault'.</span></span>

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="using-soft-delete-with-key-vault-objects"></a><span data-ttu-id="fa20e-164">Pomocí konfigurace soft odstranění trezoru klíčů objekty</span><span class="sxs-lookup"><span data-stu-id="fa20e-164">Using soft-delete with key vault objects</span></span>

<span data-ttu-id="fa20e-165">Jenom jako trezorů klíčů, odstraněný klíč tajný klíč nebo certifikát zůstane ve stavu deleted po dobu 90 dnů Pokud jej obnovit, nebo ji vymazat.</span><span class="sxs-lookup"><span data-stu-id="fa20e-165">Just like key vaults, a deleted key, secret or, certificate will remain in deleted state for up to 90 days unless you recover it or purge it.</span></span> 

#### <a name="keys"></a><span data-ttu-id="fa20e-166">Klíče</span><span class="sxs-lookup"><span data-stu-id="fa20e-166">Keys</span></span>

<span data-ttu-id="fa20e-167">K obnovení odstraněné klíče:</span><span class="sxs-lookup"><span data-stu-id="fa20e-167">To recover a deleted key:</span></span>

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

<span data-ttu-id="fa20e-168">Trvale odstranit klíč:</span><span class="sxs-lookup"><span data-stu-id="fa20e-168">To permanently delete a key:</span></span>

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

>[!NOTE]
><span data-ttu-id="fa20e-169">Vymazání klíč se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="fa20e-169">Purging a key will permanently delete it, meaning it will not be recoverable.</span></span>

<span data-ttu-id="fa20e-170">**Obnovit** a **mazání** akce mají své vlastní oprávnění v zásadách přístupu k trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="fa20e-170">The **recover** and **purge** actions have their own permissions associated in a key vault access policy.</span></span> <span data-ttu-id="fa20e-171">Pro uživatele nebo instanční objekt by mohl spustit **obnovit** nebo **mazání** akce musí mít odpovídající oprávnění pro tento objekt (klíč nebo tajný klíč) v zásadách přístupu trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="fa20e-171">For a user or service principal to be able to execute a **recover** or **purge** action they must have the respective permission for that object (key or secret) in the key vault access policy.</span></span> <span data-ttu-id="fa20e-172">Ve výchozím nastavení **mazání** oprávnění nebyla přidána do zásad přístupu k trezoru klíčů, když "vše" zástupce se používá k udělení oprávnění pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="fa20e-172">By default, the **purge** permission is not added to a key vault's access policy when the 'all' shortcut is used to grant all permissions to a user.</span></span> <span data-ttu-id="fa20e-173">Je třeba explicitně udělit **mazání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fa20e-173">You must explicitly grant **purge** permission.</span></span> <span data-ttu-id="fa20e-174">Například následující příkaz uděluje user@contoso.com oprávnění k provádění různých operací na klíče ve *ContosoVault* včetně **mazání**.</span><span class="sxs-lookup"><span data-stu-id="fa20e-174">For example, the following command grants user@contoso.com permission to perform several operations on keys in *ContosoVault* including **purge**.</span></span>

#### <a name="set-a-key-vault-access-policy"></a><span data-ttu-id="fa20e-175">Nastavit zásady přístupu k trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="fa20e-175">Set a key vault access policy</span></span>

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> <span data-ttu-id="fa20e-176">Pokud máte existující trezor klíčů, který má právě měl konfigurace soft odstranění povoleno, nemusí mít **obnovit** a **mazání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fa20e-176">If you have an existing key vault that has just had soft-delete enabled, you may not have **recover** and **purge** permissions.</span></span>

#### <a name="secrets"></a><span data-ttu-id="fa20e-177">Tajné kódy</span><span class="sxs-lookup"><span data-stu-id="fa20e-177">Secrets</span></span>

<span data-ttu-id="fa20e-178">Podobně jako klíče jsou tajných klíčů v trezoru klíčů provozovat na s vlastní příkazy.</span><span class="sxs-lookup"><span data-stu-id="fa20e-178">Like keys, secrets in a key vault are operated on with their own commands.</span></span> <span data-ttu-id="fa20e-179">Následující, jsou příkazy pro odstranění, výpis, obnovení a mazání tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="fa20e-179">Following, are the commands for deleting, listing, recovering, and purging secrets.</span></span>

- <span data-ttu-id="fa20e-180">Odstranění tajného klíče s názvem SQLPassword:</span><span class="sxs-lookup"><span data-stu-id="fa20e-180">Delete a secret named SQLPassword:</span></span> 
```azurecli
az keyvault secret delete --vault-name ContosoVault -name SQLPassword
```

- <span data-ttu-id="fa20e-181">Zobrazí seznam všech odstraněných tajných klíčů v trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="fa20e-181">List all deleted secrets in a key vault:</span></span> 
```azurecli
az keyvault secret list-deleted --vault-name ContosoVault
```

- <span data-ttu-id="fa20e-182">Tajný klíč v odstraněném stavu obnovení:</span><span class="sxs-lookup"><span data-stu-id="fa20e-182">Recover a secret in the deleted state:</span></span> 
```azurecli
az keyvault secret recover --name SQLPassword --vault-name ContosoVault
```

- <span data-ttu-id="fa20e-183">Vyprázdnění tajný klíč v odstraněném stavu:</span><span class="sxs-lookup"><span data-stu-id="fa20e-183">Purge a secret in deleted state:</span></span> 
```azurecli
az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
```

>[!NOTE]
><span data-ttu-id="fa20e-184">Vymazání tajného klíče se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="fa20e-184">Purging a secret will permanently delete it, meaning it will not be recoverable.</span></span>

## <a name="purging-and-key-vaults"></a><span data-ttu-id="fa20e-185">Trezory mazání a klíč</span><span class="sxs-lookup"><span data-stu-id="fa20e-185">Purging and key vaults</span></span>

### <a name="key-vault-objects"></a><span data-ttu-id="fa20e-186">Objekty trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="fa20e-186">Key vault objects</span></span>

<span data-ttu-id="fa20e-187">Vymazání klíč, tajný klíč nebo certifikát se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="fa20e-187">Purging a key, secret or, certificate will permanently delete it, meaning it will not be recoverable.</span></span> <span data-ttu-id="fa20e-188">Trezor klíčů, který obsahuje odstraněný objekt se ale zachovají stejně jako všechny ostatní objekty v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="fa20e-188">The key vault that contained the deleted object will however remain intact as will all other objects in the key vault.</span></span> 

### <a name="key-vaults-as-containers"></a><span data-ttu-id="fa20e-189">Klíč trezory jako kontejnery</span><span class="sxs-lookup"><span data-stu-id="fa20e-189">Key vaults as containers</span></span>
<span data-ttu-id="fa20e-190">Když se vyprazdňují trezoru klíčů, veškerý její obsah, včetně klíče a tajné klíče, certifikáty, se trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="fa20e-190">When a key vault is purged, all of its contents, including keys, secrets, and certificates, are permanently deleted.</span></span> <span data-ttu-id="fa20e-191">K vyprázdnění trezoru klíčů, použijte `az keyvault purge` příkaz.</span><span class="sxs-lookup"><span data-stu-id="fa20e-191">To purge a key vault, use the `az keyvault purge` command.</span></span> <span data-ttu-id="fa20e-192">Můžete vyhledat umístění trezorů vaše předplatné odstraněné klíčů pomocí příkazu `az keyvault list-deleted`.</span><span class="sxs-lookup"><span data-stu-id="fa20e-192">You can find the location your subscription's deleted key vaults using the command `az keyvault list-deleted`.</span></span>

```azurecli
az keyvault purge --location westus --name ContosoVault
```

>[!NOTE]
><span data-ttu-id="fa20e-193">Vymazání trezoru klíčů se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="fa20e-193">Purging a key vault will permanently delete it, meaning it will not be recoverable.</span></span>

### <a name="purge-permissions-required"></a><span data-ttu-id="fa20e-194">Vyprázdnění oprávněních</span><span class="sxs-lookup"><span data-stu-id="fa20e-194">Purge permissions required</span></span>
- <span data-ttu-id="fa20e-195">K vyprázdnění odstraněné trezoru klíčů, tak, aby trezoru a veškerý jeho obsah jsou trvale odstraněny, musí uživatel oprávnění k provedení *Microsoft.KeyVault/locations/deletedVaults/purge/action* operaci.</span><span class="sxs-lookup"><span data-stu-id="fa20e-195">To purge a deleted key vault, such that the vault and all its contents are permanently removed, the user needs RBAC permission to perform a *Microsoft.KeyVault/locations/deletedVaults/purge/action* operation.</span></span> 
- <span data-ttu-id="fa20e-196">Pro zobrazení seznamu odstraněný klíč, trezor uživatel potřebuje oprávnění k provedení *Microsoft.KeyVault/deletedVaults/read* oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fa20e-196">To list the deleted key, the vault a user needs RBAC permission to perform *Microsoft.KeyVault/deletedVaults/read* permission.</span></span> 
- <span data-ttu-id="fa20e-197">Pouze správce předplatného ve výchozím nastavení má tato oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fa20e-197">By default only a subscription administrator has these permissions.</span></span> 

### <a name="scheduled-purge"></a><span data-ttu-id="fa20e-198">Naplánované vyprázdnění</span><span class="sxs-lookup"><span data-stu-id="fa20e-198">Scheduled purge</span></span>

<span data-ttu-id="fa20e-199">Výpis vaše objekty odstraněné trezoru klíčů zobrazí, kdy mají schedled vyprázdní pomocí Key Vault.</span><span class="sxs-lookup"><span data-stu-id="fa20e-199">Listing your deleted key vault objects shows when they are schedled to be purged by Key Vault.</span></span> <span data-ttu-id="fa20e-200">*Naplánované datum vyprázdnění* pole označuje, když objekt trezoru klíčů se trvale odstraní, pokud nebyla provedena žádná akce.</span><span class="sxs-lookup"><span data-stu-id="fa20e-200">The *Scheduled Purge Date* field indicates when a key vault object will be permanently deleted, if no action is taken.</span></span> <span data-ttu-id="fa20e-201">Ve výchozím nastavení Doba uchování pro objekt odstraněného trezoru klíčů je 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="fa20e-201">By default, the retention period for a deleted key vault object is 90 days.</span></span>

>[!NOTE]
><span data-ttu-id="fa20e-202">Objekt vymazány trezoru, aktivuje její *naplánované datum vyprázdnění* pole, je trvale odstraněn.</span><span class="sxs-lookup"><span data-stu-id="fa20e-202">A purged vault object, triggered by its *Scheduled Purge Date* field, is permanently deleted.</span></span> <span data-ttu-id="fa20e-203">Není použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="fa20e-203">It is not recoverable.</span></span>

## <a name="other-resources"></a><span data-ttu-id="fa20e-204">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="fa20e-204">Other resources</span></span>

- <span data-ttu-id="fa20e-205">Přehled funkce obnovitelného odstranění Key Vault najdete v tématu [Přehled konfigurace soft odstranění Azure Key Vault](key-vault-ovw-soft-delete.md).</span><span class="sxs-lookup"><span data-stu-id="fa20e-205">For an overview of Key Vault's soft-delete feature, see [Azure Key Vault soft-delete overview](key-vault-ovw-soft-delete.md).</span></span>
- <span data-ttu-id="fa20e-206">Obecné informace o použití Azure Key Vault najdete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fa20e-206">For a general overview of Azure Key Vault usage, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>

