---
title: "aaaAzure protokolování v Key Vault | Microsoft Docs"
description: "Použijte tento kurz toohelp Začínáme s Azure Key Vault protokolování."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 43f96a2b-3af8-4adc-9344-bc6041fface8
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 38a173297948748bef45e3d857c06b50b3e21e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-logging"></a>Protokolování v Azure Key Vault
Azure Key Vault je dostupný ve většině oblastí. Další informace najdete v tématu hello [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Úvod
Po vytvoření jednoho nebo více trezorů klíčů, budete pravděpodobně chtít toomonitor jak a kdy váš klíč trezory jsou přístupu a kým. Toho docílíte povolením protokolování pro Key Vault, které ukládá informace v zadaném účtu úložiště Azure. Nový kontejner s názvem **insights-logs-auditevent** je automaticky vytvořený pro zadaný účet úložiště a ten samý účet úložiště můžete použít pro shromažďování protokolů více trezorů klíčů.

Informace o protokolování můžete přístup maximálně, operace 10 minut po hello klíč trezoru. Ve většině případů to bude rychlejší.  Je to tooyou toomanage protokolů v účtu úložiště:

* Pomocí protokolů toosecure metody řízení přístupu standardní Azure tak, že omezíte, kdo má přístup.
* Odstranění protokolů, které již nechcete tookeep ve vašem účtu úložiště.

Použijte tento kurz toohelp Začínáme s Azure Key Vault protokolování, toocreate účtu úložiště povolit protokolování a interpretací hello shromážděných informací.  

> [!NOTE]
> Tento kurz neobsahuje pokyny, jak toocreate klíče trezory, klíčů nebo tajných klíčů. Další informace naleznete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md) Pokyny pro rozhraní příkazového řádku pro různé platformy naleznete v [tomto ekvivalentním kurzu](key-vault-manage-with-cli2.md).
>
> V současné době nelze Azure Key Vault konfigurovat v hello portálu Azure. Místo toho použijte tyto pokyny pro Azure PowerShell.
>
>

Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).

## <a name="prerequisites"></a>Požadavky
toocomplete tento kurz, musíte mít hello následující:

* Existující trezor klíčů, který již používáte.  
* Azure PowerShell v **minimální verzi 1.0.1**. tooinstall prostředí Azure PowerShell a přidružit ho ke svému předplatnému Azure, najdete v části [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). Pokud jste již Azure PowerShell nainstalovali a neznáte hello verzi, z konzoly Azure PowerShell text hello, zadejte `(Get-Module azure -ListAvailable).Version`.  
* Dostatečné úložiště v Azure pro vaše protokoly Key Vault.

## <a id="connect"></a>Připojit tooyour odběrů
Spusťte relaci prostředí Azure PowerShell a přihlaste tooyour účet Azure s hello následující příkaz:  

    Login-AzureRmAccount

V okně hello automaticky otevírané okno prohlížeče zadejte účet Azure uživatelské jméno a heslo. Azure PowerShell získá všechna předplatná hello, které jsou spojeny s tímto účtem a ve výchozím nastavení, používá hello první z nich.

Pokud máte více předplatných, může být toospecify předplatné, které bylo použité toocreate Azure Key Vault. Zadejte hello následující toosee hello předplatných pro váš účet:

    Get-AzureRmSubscription

Potom toospecify hello odběr, který je spojen s trezoru klíčů, který budete protokolovat, zadejte:

    Set-AzureRmContext -SubscriptionId <subscription ID>

> [!NOTE]
> Jedná se důležitý a obzvláště užitečný krok, pokud máte víc předplatných přidružených vašemu účtu. Pokud tento krok se přeskočí, může se zobrazit k chybě tooregister Microsoft.Insights.
>   
>

Další informace o konfiguraci Azure Powershellu najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a id="storage"></a>Vytvoření nového účtu úložiště pro protokoly
Přestože je možné použít existující účet úložiště pro svoje protokoly, vytvoříme nový účet úložiště, který bude vyhrazený tooKey protokoly trezoru. Abychom si usnadnili práci při máme toospecify, to později, uložíme hello podrobnosti do proměnné s názvem **sa**.

Pro další usnadnění správy použijeme také hello stejné skupině prostředků jako ten, který obsahuje náš trezor klíčů hello. Z hello [kurz Začínáme](key-vault-get-started.md), tato skupina prostředků je s názvem **ContosoResourceGroup** a budeme pokračovat v umístění východní Asie toouse hello. Tyto hodnoty nahraďte příslušnými vlastními hodnotami:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'


> [!NOTE]
> Pokud se rozhodnete toouse stávající účet úložiště, musíte použít hello stejné předplatné jako váš trezor klíčů a musí používat model nasazení Resource Manager hello, nikoli hello model nasazení Classic.
>
>

## <a id="identify"></a>Identifikujte trezor klíčů hello pro svoje protokoly
V našem kurzu Začínáme byl název trezoru klíčů **ContosoKeyVault**, budeme tedy pokračovat toouse, zadejte název a uložte hello podrobnosti do proměnné s názvem **kv**:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Povolení protokolování
tooenable protokolování pro Key Vault použijeme rutinu Set-AzureRmDiagnosticSetting hello, společně s hello proměnné jsme vytvořili pro naše nový účet úložiště a náš trezor klíčů. Také nastavíme hello **-povolit** příznak příliš**$true** a nastavte hello kategorie tooAuditEvent (hello jediná kategorie pro protokolování v Key Vault),:

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

výstup Hello zahrnuje:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


To potvrdí, že je váš trezor klíčů, ukládá informace o účtu úložiště tooyour nyní povoleno protokolování.

Volitelně můžete pro své protokoly nastavit také zásady uchovávání informací tak, aby se starší protokoly automaticky odstraňovaly. Například nastavit pomocí zásad uchovávání **- RetentionEnabled** příznak příliš**$true** a nastavte **- RetentionInDays** parametr příliš**90** , aby protokoly, které jsou starší než 90 dní automaticky odstraní.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Co je protokolováno:

* Protokolují se všechny ověřené požadavky REST API, což zahrnuje i neúspěšné požadavky v důsledku neoprávněného přístupu, systémových chyb nebo chybných požadavků.
* Operace na hello klíč trezoru sebe, což zahrnuje vytváření, odstraňování, nastavení zásad přístupu trezoru klíčů, a aktualizaci atributů trezoru klíčů, jako značky.
* Operace u klíčů a tajných klíčů v trezoru klíčů hello, včetně vytváření, úpravy nebo odstranění těchto klíčů nebo tajných klíčů; operace jako jsou podepsání, ověření, šifrování, dešifrování, zabalení a rozbalení klíčů, získat tajné klíče, výpis klíčů a tajných klíčů a jejich verze.
* Neověřené požadavky, které skončí odpovědí 401 – Neoprávněno. Například požadavky, které nemají nosný token, jsou poškozené nebo jejichž platnost vypršela, nebo mají neplatný token.  

## <a id="access"></a>Přístup k protokolům
Protokoly trezoru klíčů se ukládají do hello **insights-logs-auditevent** kontejneru v účtu úložiště hello jste zadali. toolist všech objektů BLOB hello v tomto kontejneru, zadejte:

Nejprve vytvořte proměnnou pro název kontejneru hello. Bude se používat v rámci hello zbytek hello ukázce.

    $container = 'insights-logs-auditevent'

toolist všech objektů BLOB hello v tomto kontejneru, zadejte:

    Get-AzureStorageBlob -Container $container -Context $sa.Context
Hello výstup bude vypadat něco podobné toothis:

**Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**

**Název**

- - -
**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json****

Jak je vidět na výpisu, objekty BLOB hello podle zásady vytváření názvů: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

Hello hodnoty data a času používají UTC.

Vzhledem k hello stejný účet úložiště můžou být použité toocollect protokoly pro více zdrojů, hello úplné ID prostředku v hello název objektu blob je velmi užitečné tooaccess nebo objekty BLOB jenom hello stahování, které potřebujete. Ale předtím, než se podíváme na to, budete nejprve nabídneme jak toodownload všechny hello objekty BLOB.

Nejprve vytvořte složku toodownload hello objekty BLOB. Například:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Poté získejte seznam všech objektů blob:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Tento seznam prostřednictvím 'Get-AzureStorageBlobContent' toodownload hello objekty BLOB přesměrováním do našich cílovou složku:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Když spustíte tento druhý příkaz, hello  **/**  oddělovač v názvech objektů blob hello vytvořit úplnou strukturu složek v části hello cílovou složku a tato struktura bude použité toodownload úložiště hello objektů BLOB a jako soubory.

tooselectively stáhnout objekty BLOB, použijte zástupné znaky. Například:

* Pokud máte více trezorů klíčů a chcete toodownload protokoly pro právě jeden trezor klíčů, s názvem CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
* Pokud máte více skupin prostředků a chcete toodownload protokoly pro jenom jedné skupiny prostředků, použijte `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
* V případě potřeby toodownload všechny hello protokoly pro měsíc leden 2016 hello pomocí `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Jste nyní připraven toostart prohlížení co je v hello protokoly. Ale předtím, další dva parametry Get-AzureRmDiagnosticSetting, může být nutné tooknow:

* tooquery hello stav nastavení diagnostiky pro prostředek vašeho trezoru klíčů:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`
* toodisable protokolování pro prostředek vašeho trezoru klíčů:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`

## <a id="interpret"></a>Interpretace protokolů služby Key Vault
Jednotlivé objekty blob jsou uloženy jako text ve formátu JSON blob. Toto je příklad položky protokolu ze spuštění příkazu `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


Hello následující tabulka uvádí hello názvy polí a popisy.

| Název pole | Popis |
| --- | --- |
| time |Datum a čas (UTC). |
| resourceId |ID prostředku Azure Resource Manageru Pro protokoly Key Vault je to vždy ID prostředku Key Vault hello |
| operationName |Název hello operace, jak je uvedeno v další tabulce hello. |
| operationVersion |Toto je verze hello REST API požadovaná klientem hello. |
| category |Pro protokoly Key Vault je AuditEvent hello jeden, k dispozici hodnota. |
| resultType |Výsledek požadavku REST API. |
| resultSignature |Stav HTTP. |
| resultDescription |Další popis výsledku hello, pokud je k dispozici. |
| durationMs |Čas, kdy trvalo požadavku REST API hello tooservice, v milisekundách. Nezahrnuje latenci sítě hello, takže čas hello, naměřený na straně klienta hello se může lišit. |
| callerIpAddress |IP adresa klienta hello, který vytvořil požadavek hello. |
| correlationId |Volitelný GUID, který hello klienta můžete předat toocorrelate protokoly na straně klienta s protokoly na straně služby (Key Vault). |
| identity |Identita z tokenu hello, který byl předložený při provádění požadavku REST API hello. Obvykle to je položka „user“, „service principal“ nebo kombinace „user+appId“ jako v případě požadavku pocházejícího z rutiny Azure PowerShellu. |
| properties |Toto pole bude obsahovat různé informace v závislosti na hello operaci (operationName). Ve většině případů obsahuje informace o klientovi (hello řetězec useragent předaný klientem hello), hello přesný identifikátor URI požadavku REST API a stavový kód HTTP. Kromě toho pokud je jako výsledek požadavku (například KeyCreate nebo VaultGet) vrácen objekt bude také obsahovat hello URI klíče (jako "id"), identifikátor URI trezoru nebo identifikátor URI tajného klíče. |

Hello **operationName** hodnoty polí jsou ve formátu ObjectVerb. Například:

* Všechny operace trezoru klíčů mít hello ' trezoru`<action>`' formátu, například `VaultGet` a `VaultCreate`.
* Všechny operace nad klíči jsou hello se klíč`<action>`' formátu, například `KeySign` a `KeyList`.
* Všechny operace nad tajnými klíči mít hello "tajný klíč`<action>`' formátu, například `SecretGet` a `SecretListVersions`.

Hello následující tabulka uvádí hello operationName a odpovídajících příkazů REST API.

| operationName | Příkaz REST API |
| --- | --- |
| Authentication |Přes koncový bod služby Azure Active Directory |
| VaultGet |[Získání informací o trezoru klíčů](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx) |
| VaultPut |[Vytvoření nebo aktualizace trezoru klíčů](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx) |
| VaultDelete |[Odstranění trezoru klíčů](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx) |
| VaultPatch |[Aktualizace trezoru klíčů](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| VaultList |[Výpis všech trezorů klíčů ve skupině prostředků](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx) |
| KeyCreate |[Vytvoření klíče](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx) |
| KeyGet |[Získání informací o klíči](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx) |
| KeyImport |[Import klíče do trezoru](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx) |
| KeyBackup |[Zálohování klíče](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx). |
| KeyDelete |[Odstranění klíče](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx) |
| KeyRestore |[Obnovení klíče](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx) |
| KeySign |[Podpis klíčem](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx) |
| KeyVerify |[Ověření pomocí klíče](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx) |
| KeyWrap |[Zabalení klíče](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx) |
| KeyUnwrap |[Rozbalení klíče](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx) |
| KeyEncrypt |[Šifrování pomocí klíče](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx) |
| KeyDecrypt |[Dešifrování pomocí klíče](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx) |
| KeyUpdate |[Aktualizace klíče](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx) |
| KeyList |[Seznam hello klíčů v trezoru](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx) |
| KeyListVersions |[Seznam verzí hello klíče](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx) |
| SecretSet |[Vytvoření tajného kódu](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx) |
| SecretGet |[Získání tajného kódu](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx) |
| SecretUpdate |[Aktualizace tajného kódu](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx) |
| SecretDelete |[Odstranění tajného kódu](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx) |
| SecretList |[Výpis tajných kódů v trezoru](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx) |
| SecretListVersions |[Výpis verzí tajného kódu](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx) |

## <a id="loganalytics"></a>Použití Log Analytics

V tooreview analýzy protokolů, Azure Key Vault AuditEvent protokolů můžete použít hello Azure Key Vault řešení. Další informace, včetně jak tooset to, najdete v části [řešení Azure Key Vault ve analýzy protokolů](../log-analytics/log-analytics-azure-key-vault.md). Tento článek také obsahuje pokyny, pokud potřebujete toomigrate z hello staré Key Vault řešení nabídnuté během analýzy protokolů hello preview, kde je první směrovat vaše protokoly tooan účet úložiště Azure a nakonfigurován tooread analýzy protokolů z ní.

## <a id="next"></a>Další kroky
Chcete-li používat Azure Key Vault ve webové aplikaci, podívejte se na kurz [Použití Azure Key Vault z webové aplikace](key-vault-use-from-web-application.md).

Programátorské reference najdete v části [hello Příručka pro vývojáře Azure Key Vault](key-vault-developers-guide.md).

Seznam rutin Azure PowerShellu 1.0 pro Azure Key Vault naleznete v tématu [Rutiny Azure Key Vault](/powershell/module/azurerm.keyvault/#key_vault).

Kurz týkající se střídání klíče a protokolu auditování s Azure Key Vault, najdete v části [jak toosetup Key Vault s end tooend klíčů otočení a auditování](key-vault-key-rotation-log-monitoring.md).
