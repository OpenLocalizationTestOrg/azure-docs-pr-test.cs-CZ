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
# <a name="how-toouse-key-vault-soft-delete-with-powershell"></a>Jak toouse Key Vault obnovitelného odstranění pomocí prostředí PowerShell

Azure Key Vault obnovitelného odstranění funkce umožňuje obnovení odstraněné trezory a objekty trezoru. Konkrétně konfigurace soft odstranění adresy hello následující scénáře:

- Podpora pro obnovitelné odstranění trezoru klíčů
- Podpora pro obnovitelné odstranění trezoru klíčů objektů; klíče, tajné údaje a certifikáty

## <a name="prerequisites"></a>Požadavky

- Prostředí Azure PowerShell 4.0.0 nebo novější – Pokud nepoužíváte již je tento instalační program, nainstalujte prostředí Azure PowerShell a přidružit ho ke svému předplatnému Azure, najdete [jak tooinstall a konfigurace prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview). 

>[!NOTE]
> Zastaralou verzi naše klíč trezoru PowerShell výstupní formátování souboru, který je **může** ho možné zavést do vašeho prostředí místo hello správnou verzi. Jsme se očekává, že aktualizovanou verzi prostředí PowerShell toocontain hello oprava pro výstup hello formátování a aktualizuje v tomto tématu v daném čase. Hello aktuální řešení, by měl narazíte na potíže formátování je:
> - Použití hello následující dotaz, pokud si všimnete nevidíte hello konfigurace soft odstranění povolena vlastnost popsaných v tomto tématu: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.


Informace o konkrétní refernece Key Vault pro prostředí PowerShell najdete v tématu [Azure Key Vault PowerShell odkaz](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

## <a name="required-permissions"></a>Požadovaná oprávnění

Key Vault operace jsou samostatně spravované prostřednictvím oprávnění k přístupu na základě rolí k řízení (RBAC) následujícím způsobem:

| Operace | Popis | Oprávnění uživatele |
|:--|:--|:--|
|Seznam|Seznamy odstraňovat trezorů klíčů.|Microsoft.KeyVault/deletedVaults/read|
|Zotavit|Obnoví odstraněné trezoru klíčů.|Microsoft.KeyVault/vaults/write|
|Vyprázdnit|Trvale odstraní odstraněné trezoru klíčů a veškerý jeho obsah.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Další informace o oprávněních a řízení přístupu najdete v tématu [zabezpečit váš trezor klíčů](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Povolení konfigurace soft odstranění

možnost toorecover toobe odstraněné trezoru klíčů nebo objekty uložené v klíč trezoru, musíte nejdřív povolit konfigurace soft odstranění tohoto klíče trezoru.

### <a name="existing-key-vault"></a>Existující trezor klíčů

Pro existující trezor klíčů s názvem ContosoVault následujícím způsobem povolte obnovitelného odstranění. 

>[!NOTE]
>Aktuálně je třeba toouse Azure Resource Manager prostředků manipulaci s toodirectly zápisu hello *enableSoftDelete* toohello vlastnost prostředku Key Vault.

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Nový trezor klíčů

Povolení konfigurace soft odstranění pro nového trezoru klíčů se provádí v okamžiku vytvoření přidáním hello konfigurace soft odstranění povolit příznak tooyour vytvoření příkazu.

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Ověřte povolování konfigurace soft odstranění

tooverify, který trezoru klíčů má konfigurace soft odstranit povolený, spusťte hello *získat* příkazů a vyhledejte hello "Soft odstranit povolen?" atribut a jeho nastavení true nebo false.

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Odstranění trezoru klíčů chráněn konfigurace soft odstranění

Hello příkaz toodelete (nebo odebrat) zůstane trezoru klíčů hello stejné, ale její změny chování v závislosti na tom, jestli jste povolili obnovitelného odstranění nebo ne.

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
>Pokud spustíte předchozí příkaz hello pro trezor klíčů, který nemá konfigurace soft odstranění povoleno, bude trvale odstranit tento trezor klíčů a veškerý jeho obsah bez jakékoli možnosti pro obnovení.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Jak odstranit soft chrání vaše trezorů klíčů

S konfigurace soft odstranění povoleno:

- Při odstranění trezoru klíčů není odebrán z jeho skupin prostředků a je umístěn v vyhrazené oboru názvů, který je pouze přidružené hello umístění, kde se vytvořila. 
- Objekty odstraněné klíč trezoru, jako jsou nedostupné klíče, tajných klíčů a certifikátů a tak zůstanou při jejich obsahující trezoru klíčů je ve stavu hello odstranit. 
- název DNS Hello trezoru klíčů v odstraněném stavu je stále vyhrazené, proto nelze vytvořit nový trezor klíčů se stejným názvem.  

Může zobrazit trezorů klíčů stavu deleted, spojené s vaším předplatným pomocí hello následující příkaz:

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

Hello *ID prostředku* v hello výstup odkazuje toohello původní ID prostředku trezoru. Vzhledem k tomu, že tento trezor klíčů je teď ve stavu deleted, neexistuje žádný prostředek s ID tohoto zdroje. Hello *Id* pole lze použít tooidentify hello prostředků při obnovení nebo vyprazdňování. Hello *naplánované datum vyprázdnění* pole označuje, když se trvale odstraní trezoru hello (Vymazat) Pokud nebyla provedena žádná akce pro tento trezor odstraněné. Hello výchozí uchování období, využité toocalculate hello *naplánované Vyprázdnit data*, je 90 dnů.

## <a name="recovering-a-key-vault"></a>Obnovení trezoru klíčů

toorecover trezoru klíčů, musíte název trezoru klíčů hello toospecify, skupinu prostředků a umístění. Poznámka: hello umístění a skupiny prostředků hello hello odstranit trezor klíčů, jako je třeba tyto klíče trezoru procesu obnovení.

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

Když je obnovena trezoru klíčů, výsledkem hello je nový prostředek s ID hello trezoru klíčů původního zdroje. Pokud byla odebrána skupina prostředků hello kde existovaly hello trezoru klíčů, musí před trezoru klíčů hello lze obnovit vytvořit novou skupinu prostředků se stejným názvem.

## <a name="key-vault-objects-and-soft-delete"></a>Key Vault objekty a konfigurace soft odstranění

Pro klíč 'ContosoFirstKey' v trezoru klíčů s názvem 'ContosoVault' s konfigurace soft odstranění povoleno, zde je způsob odstranili byste klíči.

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

S vaší povolené pro obnovitelného odstranění trezoru klíčů zobrazí odstraněný klíč i stejně, jako je odstraněn s výjimkou, že při explicitně seznamu nebo načtení odstraněné klíčů. Většinu operací na klíč v hello odstranit stavu se nezdaří s výjimkou výpis odstraněný klíč, obnovení nebo vyprazdňování ho. 

Například toorequest toolist odstranit klíčů v trezoru klíčů, použijte následující příkaz hello:

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Přechodový stav. 

Pokud odstraníte klíč v trezoru klíčů s konfigurace soft odstranění povolené, může trvat několik sekund, než toocomplete přechod hello. Při tomto přechod stavu může zdát, klíči hello není v aktivním stavu hello nebo hello odstranit stavu. Tento příkaz zobrazí seznam všech odstraněných klíčů v trezoru klíčů s názvem 'ContosoVault'.

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

### <a name="using-soft-delete-with-key-vault-objects"></a>Pomocí konfigurace soft odstranění trezoru klíčů objekty

Jenom jako trezorů klíčů, odstraněný klíč tajný klíč nebo certifikát zůstane v odstraněném stavu pro až dny too90 Pokud jej obnovit, nebo ji vymazat. 

#### <a name="keys"></a>Klíče

toorecover odstraněný klíč:

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

toopermanently odstranit klíč:

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
>Vymazání klíč se trvale odstraní, což znamená, že nebude použitelná pro obnovení.

Hello **obnovit** a **mazání** akce mají své vlastní oprávnění v zásadách přístupu k trezoru klíčů. Pro uživatele nebo službu hlavní toobe možné tooexecute **obnovit** nebo **mazání** akce hello příslušných oprávnění pro tento objekt (klíč nebo tajný klíč) musí mít v zásadách přístupu hello trezoru klíčů. Ve výchozím nastavení, hello **mazání** oprávnění nebyla přidána zásady přístupu tooa trezoru klíčů, po hello "vše" zástupce použité toogrant všechny uživatelské tooa oprávnění. Je třeba explicitně udělit **mazání** oprávnění. Například hello následující příkaz uděluje user@contoso.com oprávnění tooperform několik operací na klíče ve *ContosoVault* včetně **mazání**.

#### <a name="set-a-key-vault-access-policy"></a>Nastavit zásady přístupu k trezoru klíčů

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Pokud máte existující trezor klíčů, který má právě měl konfigurace soft odstranění povoleno, nemusí mít **obnovit** a **mazání** oprávnění.

#### <a name="secrets"></a>Tajné kódy

Podobně jako klíče jsou tajných klíčů v trezoru klíčů provozovat na s vlastní příkazy. Následující, jsou hello příkazy pro odstranění, výpis, obnovení a mazání tajných klíčů.

- Odstranění tajného klíče s názvem SQLPassword: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- Zobrazí seznam všech odstraněných tajných klíčů v trezoru klíčů: 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- Tajný klíč v hello odstranit stavu obnovení: 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- Vyprázdnění tajný klíč v odstraněném stavu: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
>Vymazání tajného klíče se trvale odstraní, což znamená, že nebude použitelná pro obnovení.

## <a name="purging-and-key-vaults"></a>Trezory mazání a klíč

### <a name="key-vault-objects"></a>Objekty trezoru klíčů

Vymazání klíč, tajný klíč nebo certifikát se trvale odstraní, což znamená, že nebude použitelná pro obnovení. Hello trezoru klíčů, který obsahuje hello odstranit objekt se ale zachovají stejně jako všechny ostatní objekty v trezoru klíčů hello. 

### <a name="key-vaults-as-containers"></a>Klíč trezory jako kontejnery
Když se vyprazdňují trezoru klíčů, veškerý její obsah, včetně klíče a tajné klíče, certifikáty, se trvale odstraní. toopurge trezoru klíčů, použijte hello `Remove-AzureRmKeyVault` příkaz s možností hello `-InRemovedState` a zadáním umístění hello trezor klíčů odstranit hello se hello `-Location location` argument. Můžete vyhledat umístění hello odstraněné úložiště pomocí příkazu hello `Get-AzureRmKeyVault -InRemovedState`.

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
>Vymazání trezoru klíčů se trvale odstraní, což znamená, že nebude použitelná pro obnovení.

### <a name="purge-permissions-required"></a>Vyprázdnění oprávněních
- toopurge odstraněné trezoru klíčů, tak, aby hello trezoru a veškerý jeho obsah jsou trvale odstraněny, hello uživatel potřebuje tooperform oprávnění RBAC *Microsoft.KeyVault/locations/deletedVaults/purge/action* operaci. 
- toolist hello odstranit klíč trezoru hello uživatel potřebuje tooperform oprávnění RBAC *Microsoft.KeyVault/deletedVaults/read* oprávnění. 
- Pouze správce předplatného ve výchozím nastavení má tato oprávnění. 

### <a name="scheduled-purge"></a>Naplánované vyprázdnění

Výpis vaše objekty odstraněné trezoru klíčů zobrazuje, když jsou odstraněna podle Key Vault toobe schedled. Hello *naplánované datum vyprázdnění* pole označuje, když objekt trezoru klíčů se trvale odstraní, pokud nebyla provedena žádná akce. Ve výchozím nastavení hello doba uchování pro objekt odstraněného trezoru klíčů je 90 dnů.

>[!NOTE]
>Objekt vymazány trezoru, aktivuje její *naplánované datum vyprázdnění* pole, je trvale odstraněn. Není použitelná pro obnovení.

## <a name="other-resources"></a>Další prostředky

- Přehled funkce obnovitelného odstranění Key Vault najdete v tématu [Přehled konfigurace soft odstranění Azure Key Vault](key-vault-ovw-soft-delete.md).
- Obecné informace o použití Azure Key Vault najdete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md).

