---
ms.assetid: 
title: "aaaAzure klíč trezoru – jak toouse logicky odstranit pomocí rozhraní příkazového řádku"
description: "Pomocí rozhraní příkazového řádku výstřižků kódu případu příklady konfigurace soft odstranění"
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: bruceper
ms.openlocfilehash: 672f5210ab119c244ca712f0bb80b653b50ea79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-key-vault-soft-delete-with-cli"></a><span data-ttu-id="1bff8-103">Jak toouse Key Vault obnovitelného odstranění pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="1bff8-103">How toouse Key Vault soft-delete with CLI</span></span>

<span data-ttu-id="1bff8-104">Azure Key Vault obnovitelného odstranění funkce umožňuje obnovení odstraněné trezory a objekty trezoru.</span><span class="sxs-lookup"><span data-stu-id="1bff8-104">Azure Key Vault's soft delete feature allows recovery of deleted vaults and vault objects.</span></span> <span data-ttu-id="1bff8-105">Konkrétně konfigurace soft odstranění adresy hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="1bff8-105">Specifically, soft-delete addresses hello following scenarios:</span></span>

- <span data-ttu-id="1bff8-106">Podpora pro obnovitelné odstranění trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="1bff8-106">Support for recoverable deletion of a key vault</span></span>
- <span data-ttu-id="1bff8-107">Podpora pro obnovitelné odstranění trezoru klíčů objektů; klíče, tajné údaje a certifikáty</span><span class="sxs-lookup"><span data-stu-id="1bff8-107">Support for recoverable deletion of key vault objects; keys, secrets, and, certificates</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1bff8-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1bff8-108">Prerequisites</span></span>

- <span data-ttu-id="1bff8-109">Rozhraní příkazového řádku Azure 2.0 - Pokud nemáte tento instalační program pro vaše prostředí, najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku 2.0](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="1bff8-109">Azure CLI 2.0 - If you don't have this setup for your environment, see [Manage Key Vault using CLI 2.0](key-vault-manage-with-cli2.md).</span></span>

<span data-ttu-id="1bff8-110">Odkaz na konkrétní informace Key Vault pro rozhraní příkazového řádku najdete v tématu [Azure CLI 2.0 Key Vault odkaz](https://docs.microsoft.com/cli/azure/keyvault).</span><span class="sxs-lookup"><span data-stu-id="1bff8-110">For Key Vault specific reference information for CLI, see [Azure CLI 2.0 Key Vault reference](https://docs.microsoft.com/cli/azure/keyvault).</span></span>

## <a name="required-permissions"></a><span data-ttu-id="1bff8-111">Požadovaná oprávnění</span><span class="sxs-lookup"><span data-stu-id="1bff8-111">Required permissions</span></span>

<span data-ttu-id="1bff8-112">Key Vault operace jsou samostatně spravované prostřednictvím oprávnění k přístupu na základě rolí k řízení (RBAC) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1bff8-112">Key Vault operations are separately managed via role-based access control (RBAC) permissions as follows:</span></span>

| <span data-ttu-id="1bff8-113">Operace</span><span class="sxs-lookup"><span data-stu-id="1bff8-113">Operation</span></span> | <span data-ttu-id="1bff8-114">Popis</span><span class="sxs-lookup"><span data-stu-id="1bff8-114">Description</span></span> | <span data-ttu-id="1bff8-115">Oprávnění uživatele</span><span class="sxs-lookup"><span data-stu-id="1bff8-115">User permission</span></span> |
|:--|:--|:--|
|<span data-ttu-id="1bff8-116">Seznam</span><span class="sxs-lookup"><span data-stu-id="1bff8-116">List</span></span>|<span data-ttu-id="1bff8-117">Seznamy odstraňovat trezorů klíčů.</span><span class="sxs-lookup"><span data-stu-id="1bff8-117">Lists deleted key vaults.</span></span>|<span data-ttu-id="1bff8-118">Microsoft.KeyVault/deletedVaults/read</span><span class="sxs-lookup"><span data-stu-id="1bff8-118">Microsoft.KeyVault/deletedVaults/read</span></span>|
|<span data-ttu-id="1bff8-119">Zotavit</span><span class="sxs-lookup"><span data-stu-id="1bff8-119">Recover</span></span>|<span data-ttu-id="1bff8-120">Obnoví odstraněné trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="1bff8-120">Restores a deleted key vault.</span></span>|<span data-ttu-id="1bff8-121">Microsoft.KeyVault/vaults/write</span><span class="sxs-lookup"><span data-stu-id="1bff8-121">Microsoft.KeyVault/vaults/write</span></span>|
|<span data-ttu-id="1bff8-122">Vyprázdnit</span><span class="sxs-lookup"><span data-stu-id="1bff8-122">Purge</span></span>|<span data-ttu-id="1bff8-123">Trvale odstraní odstraněné trezoru klíčů a veškerý jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="1bff8-123">Permanently removes a deleted key vault and all its contents.</span></span>|<span data-ttu-id="1bff8-124">Microsoft.KeyVault/locations/deletedVaults/purge/action</span><span class="sxs-lookup"><span data-stu-id="1bff8-124">Microsoft.KeyVault/locations/deletedVaults/purge/action</span></span>|

<span data-ttu-id="1bff8-125">Další informace o oprávněních a řízení přístupu najdete v tématu [zabezpečit váš trezor klíčů](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="1bff8-125">For more information on permissions and access control, see [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>

## <a name="enabling-soft-delete"></a><span data-ttu-id="1bff8-126">Povolení konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="1bff8-126">Enabling soft-delete</span></span>

<span data-ttu-id="1bff8-127">možnost toorecover toobe odstraněné trezoru klíčů nebo objekty uložené v klíč trezoru, musíte nejdřív povolit konfigurace soft odstranění tohoto klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="1bff8-127">toobe able toorecover a deleted key vault or objects stored in a key vault, you must first enable soft-delete for that key vault.</span></span>

### <a name="existing-key-vault"></a><span data-ttu-id="1bff8-128">Existující trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="1bff8-128">Existing key vault</span></span>

<span data-ttu-id="1bff8-129">Pro existující trezor klíčů s názvem ContosoVault následujícím způsobem povolte obnovitelného odstranění.</span><span class="sxs-lookup"><span data-stu-id="1bff8-129">For an existing key vault named ContosoVault, enable soft-delete as follows.</span></span> 

>[!NOTE]
><span data-ttu-id="1bff8-130">Aktuálně je třeba toouse Azure Resource Manager prostředků manipulaci s toodirectly zápisu hello *enableSoftDelete* toohello vlastnost prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1bff8-130">Currently you need toouse Azure Resource Manager resource manipulation toodirectly write hello *enableSoftDelete* property toohello Key Vault resource.</span></span>

```azurecli
az resource update --id $(az keyvault show --name ContosoVault -o tsv | awk '{print $1}') --set properties.enableSoftDelete=true
```

### <a name="new-key-vault"></a><span data-ttu-id="1bff8-131">Nový trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="1bff8-131">New key vault</span></span>

<span data-ttu-id="1bff8-132">Povolení konfigurace soft odstranění pro nového trezoru klíčů se provádí v okamžiku vytvoření přidáním hello konfigurace soft odstranění povolit příznak tooyour vytvoření příkazu.</span><span class="sxs-lookup"><span data-stu-id="1bff8-132">Enabling soft-delete for a new key vault is done at creation time by adding hello soft-delete enable flag tooyour create command.</span></span>

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --enable-soft-delete true --location westus
```

### <a name="verify-soft-delete-enablement"></a><span data-ttu-id="1bff8-133">Ověřte povolování konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="1bff8-133">Verify soft-delete enablement</span></span>

<span data-ttu-id="1bff8-134">tooverify, který trezoru klíčů má konfigurace soft odstranit povolený, spusťte hello *zobrazit* příkazů a vyhledejte hello "Soft odstranit povolen?"</span><span class="sxs-lookup"><span data-stu-id="1bff8-134">tooverify that a key vault has soft-delete enabled, run hello *show* command and look for hello 'Soft Delete Enabled?'</span></span> <span data-ttu-id="1bff8-135">atribut a jeho nastavení true nebo false.</span><span class="sxs-lookup"><span data-stu-id="1bff8-135">attribute and its setting, true or false.</span></span>

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a><span data-ttu-id="1bff8-136">Odstranění trezoru klíčů chráněn konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="1bff8-136">Deleting a key vault protected by soft-delete</span></span>

<span data-ttu-id="1bff8-137">Hello příkaz toodelete (nebo odebrat) zůstane trezoru klíčů hello stejné, ale její změny chování v závislosti na tom, jestli jste povolili obnovitelného odstranění nebo ne.</span><span class="sxs-lookup"><span data-stu-id="1bff8-137">hello command toodelete (or remove) a key vault remains hello same, but its behavior changes depending on whether you have enabled soft-delete or not.</span></span>

```azurecli
az keyvault delete --name ContosoVault
```

> [!IMPORTANT]
><span data-ttu-id="1bff8-138">Pokud spustíte předchozí příkaz hello pro trezor klíčů, který nemá konfigurace soft odstranění povoleno, bude trvale odstranit tento trezor klíčů a veškerý jeho obsah bez jakékoli možnosti pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="1bff8-138">If you run hello previous command for a key vault that does not have soft-delete enabled, you will permanently delete this key vault and all its content without any options for recovery.</span></span>

### <a name="how-soft-delete-protects-your-key-vaults"></a><span data-ttu-id="1bff8-139">Jak odstranit soft chrání vaše trezorů klíčů</span><span class="sxs-lookup"><span data-stu-id="1bff8-139">How soft-delete protects your key vaults</span></span>

<span data-ttu-id="1bff8-140">S konfigurace soft odstranění povoleno:</span><span class="sxs-lookup"><span data-stu-id="1bff8-140">With soft-delete enabled:</span></span>

- <span data-ttu-id="1bff8-141">Při odstranění trezoru klíčů není odebrán z jeho skupin prostředků a je umístěn v vyhrazené oboru názvů, který je pouze přidružené hello umístění, kde se vytvořila.</span><span class="sxs-lookup"><span data-stu-id="1bff8-141">When a key vault is deleted, it is removed from its resource group and placed in a reserved namespace that is only associated with hello location where it was created.</span></span> 
- <span data-ttu-id="1bff8-142">Objekty odstraněné klíč trezoru, jako jsou nedostupné klíče, tajných klíčů a certifikátů a tak zůstanou při jejich obsahující trezoru klíčů je ve stavu hello odstranit.</span><span class="sxs-lookup"><span data-stu-id="1bff8-142">Objects in a deleted key vault, such as keys, secrets and, certificates, are inaccessible and remain so while their containing key vault is in hello deleted state.</span></span> 
- <span data-ttu-id="1bff8-143">název DNS Hello trezoru klíčů v odstraněném stavu je stále vyhrazené, proto nelze vytvořit nový trezor klíčů se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="1bff8-143">hello DNS name for a key vault in a deleted state is still reserved so, a new key vault with same name cannot be created.</span></span>  

<span data-ttu-id="1bff8-144">Může zobrazit trezorů klíčů stavu deleted, spojené s vaším předplatným pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1bff8-144">You may view deleted state key vaults, associated with your subscription, using hello following command:</span></span>

```azurecli
az keyvault list-deleted
```

<span data-ttu-id="1bff8-145">Hello *ID prostředku* v hello výstup odkazuje toohello původní ID prostředku trezoru.</span><span class="sxs-lookup"><span data-stu-id="1bff8-145">hello *Resource ID* in hello output refers toohello original resource ID of this vault.</span></span> <span data-ttu-id="1bff8-146">Vzhledem k tomu, že tento trezor klíčů je teď ve stavu deleted, neexistuje žádný prostředek s ID tohoto zdroje.</span><span class="sxs-lookup"><span data-stu-id="1bff8-146">Since this key vault is now in a deleted state, no resource exists with that resource ID.</span></span> <span data-ttu-id="1bff8-147">Hello *Id* pole lze použít tooidentify hello prostředků při obnovení nebo vyprazdňování.</span><span class="sxs-lookup"><span data-stu-id="1bff8-147">hello *Id* field can be used tooidentify hello resource when recovering, or purging.</span></span> <span data-ttu-id="1bff8-148">Hello *naplánované datum vyprázdnění* pole označuje, když se trvale odstraní trezoru hello (Vymazat) Pokud nebyla provedena žádná akce pro tento trezor odstraněné.</span><span class="sxs-lookup"><span data-stu-id="1bff8-148">hello *Scheduled Purge Date* field indicates when hello vault will be permanently deleted (purged) if no action is taken for this deleted vault.</span></span> <span data-ttu-id="1bff8-149">Hello výchozí uchování období, využité toocalculate hello *naplánované Vyprázdnit data*, je 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="1bff8-149">hello default retention period, used toocalculate hello *Scheduled Purge Date*, is 90 days.</span></span>

## <a name="recovering-a-key-vault"></a><span data-ttu-id="1bff8-150">Obnovení trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="1bff8-150">Recovering a key vault</span></span>

<span data-ttu-id="1bff8-151">toorecover trezoru klíčů, musíte název trezoru klíčů hello toospecify, skupinu prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="1bff8-151">toorecover a key vault, you need toospecify hello key vault name, resource group, and location.</span></span> <span data-ttu-id="1bff8-152">Poznámka: hello umístění a skupiny prostředků hello hello odstranit trezor klíčů, jako je třeba tyto klíče trezoru procesu obnovení.</span><span class="sxs-lookup"><span data-stu-id="1bff8-152">Note hello location and hello resource group of hello deleted key vault as you need these for a key vault recovery process.</span></span>

```azurecli
az keyvault recover --location westus --name ContosoVault
```

<span data-ttu-id="1bff8-153">Když je obnovena trezoru klíčů, výsledkem hello je nový prostředek s ID hello trezoru klíčů původního zdroje.</span><span class="sxs-lookup"><span data-stu-id="1bff8-153">When a key vault is recovered, hello result is a new resource with hello key vault's original resource ID.</span></span> <span data-ttu-id="1bff8-154">Pokud byla odebrána skupina prostředků hello kde existovaly hello trezoru klíčů, musí před trezoru klíčů hello lze obnovit vytvořit novou skupinu prostředků se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="1bff8-154">If hello resource group where hello key vault existed has been removed, a new resource group with same name must be created before hello key vault can be recovered.</span></span>

## <a name="key-vault-objects-and-soft-delete"></a><span data-ttu-id="1bff8-155">Key Vault objekty a konfigurace soft odstranění</span><span class="sxs-lookup"><span data-stu-id="1bff8-155">Key Vault objects and soft-delete</span></span>

<span data-ttu-id="1bff8-156">Pro klíč 'ContosoFirstKey' v trezoru klíčů s názvem 'ContosoVault' s konfigurace soft odstranění povoleno, zde je způsob odstranili byste klíči.</span><span class="sxs-lookup"><span data-stu-id="1bff8-156">For a key, 'ContosoFirstKey', in a key vault named 'ContosoVault' with soft-delete enabled, here's how you would delete that key.</span></span>

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

<span data-ttu-id="1bff8-157">S vaší povolené pro obnovitelného odstranění trezoru klíčů zobrazí odstraněný klíč i stejně, jako je odstraněn s výjimkou, že při explicitně seznamu nebo načtení odstraněné klíčů.</span><span class="sxs-lookup"><span data-stu-id="1bff8-157">With your key vault enabled for soft-delete, a deleted key still appears like it's deleted except, when you explicitly list or retrieve deleted keys.</span></span> <span data-ttu-id="1bff8-158">Většinu operací na klíč v hello odstranit stavu se nezdaří s výjimkou výpis odstraněný klíč, obnovení nebo vyprazdňování ho.</span><span class="sxs-lookup"><span data-stu-id="1bff8-158">Most operations on a key in hello deleted state will fail except for listing a deleted key, recovering it or purging it.</span></span> 

<span data-ttu-id="1bff8-159">Například toorequest toolist odstranit klíčů v trezoru klíčů, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="1bff8-159">For example, toorequest toolist deleted keys in a key vault, use hello following command:</span></span>

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a><span data-ttu-id="1bff8-160">Přechodový stav.</span><span class="sxs-lookup"><span data-stu-id="1bff8-160">Transition state</span></span> 

<span data-ttu-id="1bff8-161">Pokud odstraníte klíč v trezoru klíčů s konfigurace soft odstranění povolené, může trvat několik sekund, než toocomplete přechod hello.</span><span class="sxs-lookup"><span data-stu-id="1bff8-161">When you delete a key in a key vault with soft-delete enabled, it may take a few seconds for hello transition toocomplete.</span></span> <span data-ttu-id="1bff8-162">Při tomto přechod stavu může zdát, klíči hello není v aktivním stavu hello nebo hello odstranit stavu.</span><span class="sxs-lookup"><span data-stu-id="1bff8-162">During this transition state, it may appear that hello key is not in hello active state or hello deleted state.</span></span> <span data-ttu-id="1bff8-163">Tento příkaz zobrazí seznam všech odstraněných klíčů v trezoru klíčů s názvem 'ContosoVault'.</span><span class="sxs-lookup"><span data-stu-id="1bff8-163">This command will list all deleted keys in your key vault named 'ContosoVault'.</span></span>

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="using-soft-delete-with-key-vault-objects"></a><span data-ttu-id="1bff8-164">Pomocí konfigurace soft odstranění trezoru klíčů objekty</span><span class="sxs-lookup"><span data-stu-id="1bff8-164">Using soft-delete with key vault objects</span></span>

<span data-ttu-id="1bff8-165">Jenom jako trezorů klíčů, odstraněný klíč tajný klíč nebo certifikát zůstane v odstraněném stavu pro až dny too90 Pokud jej obnovit, nebo ji vymazat.</span><span class="sxs-lookup"><span data-stu-id="1bff8-165">Just like key vaults, a deleted key, secret or, certificate will remain in deleted state for up too90 days unless you recover it or purge it.</span></span> 

#### <a name="keys"></a><span data-ttu-id="1bff8-166">Klíče</span><span class="sxs-lookup"><span data-stu-id="1bff8-166">Keys</span></span>

<span data-ttu-id="1bff8-167">toorecover odstraněný klíč:</span><span class="sxs-lookup"><span data-stu-id="1bff8-167">toorecover a deleted key:</span></span>

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

<span data-ttu-id="1bff8-168">toopermanently odstranit klíč:</span><span class="sxs-lookup"><span data-stu-id="1bff8-168">toopermanently delete a key:</span></span>

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

>[!NOTE]
><span data-ttu-id="1bff8-169">Vymazání klíč se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="1bff8-169">Purging a key will permanently delete it, meaning it will not be recoverable.</span></span>

<span data-ttu-id="1bff8-170">Hello **obnovit** a **mazání** akce mají své vlastní oprávnění v zásadách přístupu k trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="1bff8-170">hello **recover** and **purge** actions have their own permissions associated in a key vault access policy.</span></span> <span data-ttu-id="1bff8-171">Pro uživatele nebo službu hlavní toobe možné tooexecute **obnovit** nebo **mazání** akce hello příslušných oprávnění pro tento objekt (klíč nebo tajný klíč) musí mít v zásadách přístupu hello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="1bff8-171">For a user or service principal toobe able tooexecute a **recover** or **purge** action they must have hello respective permission for that object (key or secret) in hello key vault access policy.</span></span> <span data-ttu-id="1bff8-172">Ve výchozím nastavení, hello **mazání** oprávnění nebyla přidána zásady přístupu tooa trezoru klíčů, po hello "vše" zástupce použité toogrant všechny uživatelské tooa oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1bff8-172">By default, hello **purge** permission is not added tooa key vault's access policy when hello 'all' shortcut is used toogrant all permissions tooa user.</span></span> <span data-ttu-id="1bff8-173">Je třeba explicitně udělit **mazání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1bff8-173">You must explicitly grant **purge** permission.</span></span> <span data-ttu-id="1bff8-174">Například hello následující příkaz uděluje user@contoso.com oprávnění tooperform několik operací na klíče ve *ContosoVault* včetně **mazání**.</span><span class="sxs-lookup"><span data-stu-id="1bff8-174">For example, hello following command grants user@contoso.com permission tooperform several operations on keys in *ContosoVault* including **purge**.</span></span>

#### <a name="set-a-key-vault-access-policy"></a><span data-ttu-id="1bff8-175">Nastavit zásady přístupu k trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="1bff8-175">Set a key vault access policy</span></span>

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> <span data-ttu-id="1bff8-176">Pokud máte existující trezor klíčů, který má právě měl konfigurace soft odstranění povoleno, nemusí mít **obnovit** a **mazání** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1bff8-176">If you have an existing key vault that has just had soft-delete enabled, you may not have **recover** and **purge** permissions.</span></span>

#### <a name="secrets"></a><span data-ttu-id="1bff8-177">Tajné kódy</span><span class="sxs-lookup"><span data-stu-id="1bff8-177">Secrets</span></span>

<span data-ttu-id="1bff8-178">Podobně jako klíče jsou tajných klíčů v trezoru klíčů provozovat na s vlastní příkazy.</span><span class="sxs-lookup"><span data-stu-id="1bff8-178">Like keys, secrets in a key vault are operated on with their own commands.</span></span> <span data-ttu-id="1bff8-179">Následující, jsou hello příkazy pro odstranění, výpis, obnovení a mazání tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="1bff8-179">Following, are hello commands for deleting, listing, recovering, and purging secrets.</span></span>

- <span data-ttu-id="1bff8-180">Odstranění tajného klíče s názvem SQLPassword:</span><span class="sxs-lookup"><span data-stu-id="1bff8-180">Delete a secret named SQLPassword:</span></span> 
```azurecli
az keyvault secret delete --vault-name ContosoVault -name SQLPassword
```

- <span data-ttu-id="1bff8-181">Zobrazí seznam všech odstraněných tajných klíčů v trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="1bff8-181">List all deleted secrets in a key vault:</span></span> 
```azurecli
az keyvault secret list-deleted --vault-name ContosoVault
```

- <span data-ttu-id="1bff8-182">Tajný klíč v hello odstranit stavu obnovení:</span><span class="sxs-lookup"><span data-stu-id="1bff8-182">Recover a secret in hello deleted state:</span></span> 
```azurecli
az keyvault secret recover --name SQLPassword --vault-name ContosoVault
```

- <span data-ttu-id="1bff8-183">Vyprázdnění tajný klíč v odstraněném stavu:</span><span class="sxs-lookup"><span data-stu-id="1bff8-183">Purge a secret in deleted state:</span></span> 
```azurecli
az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
```

>[!NOTE]
><span data-ttu-id="1bff8-184">Vymazání tajného klíče se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="1bff8-184">Purging a secret will permanently delete it, meaning it will not be recoverable.</span></span>

## <a name="purging-and-key-vaults"></a><span data-ttu-id="1bff8-185">Trezory mazání a klíč</span><span class="sxs-lookup"><span data-stu-id="1bff8-185">Purging and key vaults</span></span>

### <a name="key-vault-objects"></a><span data-ttu-id="1bff8-186">Objekty trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="1bff8-186">Key vault objects</span></span>

<span data-ttu-id="1bff8-187">Vymazání klíč, tajný klíč nebo certifikát se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="1bff8-187">Purging a key, secret or, certificate will permanently delete it, meaning it will not be recoverable.</span></span> <span data-ttu-id="1bff8-188">Hello trezoru klíčů, který obsahuje hello odstranit objekt se ale zachovají stejně jako všechny ostatní objekty v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="1bff8-188">hello key vault that contained hello deleted object will however remain intact as will all other objects in hello key vault.</span></span> 

### <a name="key-vaults-as-containers"></a><span data-ttu-id="1bff8-189">Klíč trezory jako kontejnery</span><span class="sxs-lookup"><span data-stu-id="1bff8-189">Key vaults as containers</span></span>
<span data-ttu-id="1bff8-190">Když se vyprazdňují trezoru klíčů, veškerý její obsah, včetně klíče a tajné klíče, certifikáty, se trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="1bff8-190">When a key vault is purged, all of its contents, including keys, secrets, and certificates, are permanently deleted.</span></span> <span data-ttu-id="1bff8-191">toopurge trezoru klíčů, použijte hello `az keyvault purge` příkaz.</span><span class="sxs-lookup"><span data-stu-id="1bff8-191">toopurge a key vault, use hello `az keyvault purge` command.</span></span> <span data-ttu-id="1bff8-192">Můžete najít umístění hello trezorů vaše předplatné odstraněné klíčů pomocí příkazu hello `az keyvault list-deleted`.</span><span class="sxs-lookup"><span data-stu-id="1bff8-192">You can find hello location your subscription's deleted key vaults using hello command `az keyvault list-deleted`.</span></span>

```azurecli
az keyvault purge --location westus --name ContosoVault
```

>[!NOTE]
><span data-ttu-id="1bff8-193">Vymazání trezoru klíčů se trvale odstraní, což znamená, že nebude použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="1bff8-193">Purging a key vault will permanently delete it, meaning it will not be recoverable.</span></span>

### <a name="purge-permissions-required"></a><span data-ttu-id="1bff8-194">Vyprázdnění oprávněních</span><span class="sxs-lookup"><span data-stu-id="1bff8-194">Purge permissions required</span></span>
- <span data-ttu-id="1bff8-195">toopurge odstraněné trezoru klíčů, tak, aby hello trezoru a veškerý jeho obsah jsou trvale odstraněny, hello uživatel potřebuje tooperform oprávnění RBAC *Microsoft.KeyVault/locations/deletedVaults/purge/action* operaci.</span><span class="sxs-lookup"><span data-stu-id="1bff8-195">toopurge a deleted key vault, such that hello vault and all its contents are permanently removed, hello user needs RBAC permission tooperform a *Microsoft.KeyVault/locations/deletedVaults/purge/action* operation.</span></span> 
- <span data-ttu-id="1bff8-196">toolist hello odstranit klíč trezoru hello uživatel potřebuje tooperform oprávnění RBAC *Microsoft.KeyVault/deletedVaults/read* oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1bff8-196">toolist hello deleted key, hello vault a user needs RBAC permission tooperform *Microsoft.KeyVault/deletedVaults/read* permission.</span></span> 
- <span data-ttu-id="1bff8-197">Pouze správce předplatného ve výchozím nastavení má tato oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1bff8-197">By default only a subscription administrator has these permissions.</span></span> 

### <a name="scheduled-purge"></a><span data-ttu-id="1bff8-198">Naplánované vyprázdnění</span><span class="sxs-lookup"><span data-stu-id="1bff8-198">Scheduled purge</span></span>

<span data-ttu-id="1bff8-199">Výpis vaše objekty odstraněné trezoru klíčů zobrazuje, když jsou odstraněna podle Key Vault toobe schedled.</span><span class="sxs-lookup"><span data-stu-id="1bff8-199">Listing your deleted key vault objects shows when they are schedled toobe purged by Key Vault.</span></span> <span data-ttu-id="1bff8-200">Hello *naplánované datum vyprázdnění* pole označuje, když objekt trezoru klíčů se trvale odstraní, pokud nebyla provedena žádná akce.</span><span class="sxs-lookup"><span data-stu-id="1bff8-200">hello *Scheduled Purge Date* field indicates when a key vault object will be permanently deleted, if no action is taken.</span></span> <span data-ttu-id="1bff8-201">Ve výchozím nastavení hello doba uchování pro objekt odstraněného trezoru klíčů je 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="1bff8-201">By default, hello retention period for a deleted key vault object is 90 days.</span></span>

>[!NOTE]
><span data-ttu-id="1bff8-202">Objekt vymazány trezoru, aktivuje její *naplánované datum vyprázdnění* pole, je trvale odstraněn.</span><span class="sxs-lookup"><span data-stu-id="1bff8-202">A purged vault object, triggered by its *Scheduled Purge Date* field, is permanently deleted.</span></span> <span data-ttu-id="1bff8-203">Není použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="1bff8-203">It is not recoverable.</span></span>

## <a name="other-resources"></a><span data-ttu-id="1bff8-204">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="1bff8-204">Other resources</span></span>

- <span data-ttu-id="1bff8-205">Přehled funkce obnovitelného odstranění Key Vault najdete v tématu [Přehled konfigurace soft odstranění Azure Key Vault](key-vault-ovw-soft-delete.md).</span><span class="sxs-lookup"><span data-stu-id="1bff8-205">For an overview of Key Vault's soft-delete feature, see [Azure Key Vault soft-delete overview](key-vault-ovw-soft-delete.md).</span></span>
- <span data-ttu-id="1bff8-206">Obecné informace o použití Azure Key Vault najdete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1bff8-206">For a general overview of Azure Key Vault usage, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>

