---
title: "aaaSensitive Data - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "způsoby zmírnění hrozeb v hello nástroj modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 8a18f43af439241ba193ccf668971ddc4655355f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-sensitive-data--mitigations"></a>Zabezpečení rámce: Citlivá Data | Způsoby zmírnění rizik 
| Produktům a službám | Článek |
| --------------- | ------- |
| **Počítač hranice vztahů důvěryhodnosti** | <ul><li>[Ujistěte se, že binární soubory jsou zamaskované pokud obsahují citlivé informace](#binaries-info)</li><li>[Zvažte použití šifrované souborů EFS (Encrypting File System) je použité tooprotect důvěrných dat specifických pro uživatele](#efs-user)</li><li>[Zajistěte, aby byla zašifrovaná citlivá data ukládaná hello aplikace v systému souborů hello](#filesystem)</li></ul> | 
| **Webové aplikace** | <ul><li>[Ujistěte se, že není v prohlížeči hello mezipaměti citlivého obsahu](#cache-browser)</li><li>[Šifrování oddíly konfiguračních souborů webové aplikace, které obsahují citlivá data](#encrypt-data)</li><li>[Explicitně zakážete atributu HTML hello automatického dokončování v citlivých formulářů a vstupy](#autocomplete-input)</li><li>[Ujistěte se, že citlivá data zobrazí na obrazovce uživatele hello zakryté hvězdičkami](#data-mask)</li></ul> | 
| **Database** | <ul><li>[Implementace dynamická data maskování bez ohrožení citlivých dat toolimit privilegovaný uživatelů](#dynamic-users)</li><li>[Ujistěte se, že hesla jsou uložena ve formátu hashe](#salted-hash)</li><li>[Musí být šifrovaný citlivá data v databázi sloupců](#db-encrypted)</li><li>[Ujistěte se, že je povolené šifrování tuto úroveň databáze (TDE)](#tde-enabled)</li><li>[Zajistěte, aby byly šifrované zálohování databáze](#backup)</li></ul> | 
| **Webové rozhraní API** | <ul><li>[Ujistěte se, že relevantní tooWeb citlivá data, které rozhraní API není uložen v úložišti prohlížeče](#api-browser)</li></ul> | 
| Azure Documentdb | <ul><li>[Šifrování citlivých dat, které jsou uložené v DocumentDB](#encrypt-docdb)</li></ul> | 
| **Hranice vztahů důvěryhodnosti virtuálních počítačů Azure IaaS** | <ul><li>[Použití Azure Disk Encryption tooencrypt disky používat virtuální počítače](#disk-vm)</li></ul> | 
| **Hranice vztahů důvěryhodnosti Service Fabric** | <ul><li>[Šifrování tajných klíčů v aplikace Service Fabric](#fabric-apps)</li></ul> | 
| **Dynamics CRM** | <ul><li>[Provedení modelování zabezpečení a použít obchodní jednotky nebo týmy případě požadavku](#modeling-teams)</li><li>[Minimalizovat funkce tooshare přístupu na kritické entity](#entities)</li><li>[Uživatelé Train na hello rizika spojená s hello funkce sdílené složky Dynamics CRM a postupy dobrý zabezpečení](#good-practices)</li><li>[Zahrnout pravidla standardy vývoj proscribing zobrazující podrobnosti konfigurace ve správě výjimek](#exception-mgmt)</li></ul> | 
| **Azure Storage** | <ul><li>[Používat šifrování služby úložiště Azure (SSE) dat v klidovém stavu (Preview)](#sse-preview)</li><li>[Pomocí šifrování na straně klienta toostore citlivá data ve službě Azure Storage](#client-storage)</li></ul> | 
| **Mobilního klienta** | <ul><li>[Šifrování velká a malá písmena nebo PII dat zapsaných toophones místní úložiště](#pii-phones)</li><li>[Před distribucí uživatelům tooend obfuskováním generovaného binárních souborů](#binaries-end)</li></ul> | 
| **WCF** | <ul><li>[Sada clientCredentialType tooCertificate nebo systému Windows](#cert)</li><li>[Není povolen režim zabezpečení WCF](#security)</li></ul> | 

## <a id="binaries-info"></a>Ujistěte se, že binární soubory jsou zamaskované pokud obsahují citlivé informace

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Počítač hranice vztahů důvěryhodnosti | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Ujistěte se, že binární soubory jsou zamaskované pokud obsahují citlivé informace, například obchodních tajemství, citlivé obchodní logiky, která by měla není vrátit zpět. Toto je toostop zpětnou analýzu sestavení. Nástroje, jako `CryptoObfuscator` mohou být použity pro tento účel. |

## <a id="efs-user"></a>Zvažte použití šifrované souborů EFS (Encrypting File System) je použité tooprotect důvěrných dat specifických pro uživatele

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Počítač hranice vztahů důvěryhodnosti | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Zvažte použití šifrované souborů EFS (Encrypting File System) je použité tooprotect důvěrné uživatelská data z nežádoucí s počítačem toohello fyzický přístup. |

## <a id="filesystem"></a>Zajistěte, aby byla zašifrovaná citlivá data ukládaná hello aplikace v systému souborů hello

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Počítač hranice vztahů důvěryhodnosti | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Zajistěte, aby byla zašifrovaná citlivá data ukládaná hello aplikace v systému souborů hello (např. pomocí rozhraní DPAPI), pokud systém souborů EFS se nedá vynutit. |

## <a id="cache-browser"></a>Ujistěte se, že není v prohlížeči hello mezipaměti citlivého obsahu

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, webových formulářů, MVC5, MVC6 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Prohlížeče můžete ukládat informace pro účely ukládání do mezipaměti a historie. Tyto soubory uložené v mezipaměti jsou uloženy ve složce, jako je složka dočasných souborů Internetu hello v případě hello aplikace Internet Explorer. Když tyto stránek se označují znovu, je hello prohlížeče zobrazí ze své mezipaměti. Pokud je citlivých informací zobrazených toohello uživatele (například jejich adresu, údajů z platební karty, číslo sociálního pojištění nebo uživatelské jméno), pak tato informace může být uložené v mezipaměti prohlížeče a proto získat prostřednictvím zkoumání mezipaměti hello prohlížeče nebo jednoduše stisknutím hello prohlížeči na tlačítko "Zpět". Nastavte hodnotu hlavičky cache-control odpovědi příliš "Ne úložiště" pro všechny stránky. |

### <a name="example"></a>Příklad
```XML
<configuration>
  <system.webServer>
   <httpProtocol>
    <customHeaders>
        <add name="Cache-Control" value="no-cache" />
        <add name="Pragma" value="no-cache" />
        <add name="Expires" value="-1" />
    </customHeaders>
  </httpProtocol>
 </system.webServer>
</configuration>
```

### <a name="example"></a>Příklad
Tato možnost může být implementovaná pomocí filtru. Následující příklad mohou být použity: 
```C#
public override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            if (filterContext == null || (filterContext.HttpContext != null && filterContext.HttpContext.Response != null && filterContext.HttpContext.Response.IsRequestBeingRedirected))
            {
                //// Since this is MVC pipeline, this should never be null.
                return;
            }

            var attributes = filterContext.ActionDescriptor.GetCustomAttributes(typeof(System.Web.Mvc.OutputCacheAttribute), false);
            if (attributes == null || **Attributes**.Count() == 0)
            {
                filterContext.HttpContext.Response.Cache.SetNoStore();
                filterContext.HttpContext.Response.Cache.SetCacheability(HttpCacheability.NoCache);
                filterContext.HttpContext.Response.Cache.SetExpires(DateTime.UtcNow.AddHours(-1));
                if (!filterContext.IsChildAction)
                {
                    filterContext.HttpContext.Response.AppendHeader("Pragma", "no-cache");
                }
            }

            base.OnActionExecuting(filterContext);
        }
``` 

## <a id="encrypt-data"></a>Šifrování oddíly konfiguračních souborů webové aplikace, které obsahují citlivá data

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Postupy: Šifrování konfigurační oddíly funkce v technologii ASP.NET 2.0 pomocí rozhraní DPAPI](https://msdn.microsoft.com/library/ff647398.aspx), [určení konfigurace poskytovatele chráněné](https://msdn.microsoft.com/library/68ze1hb2.aspx), [tajné klíče aplikace tooprotect pomocí Azure Key Vault](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Kroky** | Konfigurační soubory, jako je například hello souboru Web.config, appSettings.JSON určený se často používá toohold citlivé informace, včetně uživatelská jména, hesla, databázové připojovací řetězce a šifrovací klíče. Pokud není chránit tyto informace, vaše aplikace je snadno napadnutelný tooattackers nebo uživatelé se zlými úmysly získání citlivé informace, jako je například účet uživatelská jména a hesla, názvy databáze a názvy serverů. Podle typu nasazení hello (azure nebo místní), zašifrujte hello citlivé části konfigurační soubory pomocí rozhraní DPAPI nebo služby, jako je Azure Key Vault. |

## <a id="autocomplete-input"></a>Explicitně zakážete atributu HTML hello automatického dokončování v citlivých formulářů a vstupy

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN: automatického dokončování atribut](http://msdn.microsoft.com/library/ms533486(VS.85).aspx), [pomocí automatického dokončování ve formátu HTML](http://msdn.microsoft.com/library/ms533032.aspx), [zabezpečení čištění HTML](http://technet.microsoft.com/security/bulletin/MS10-071), [Autocomplete., znovu?](http://blog.mindedsecurity.com/2011/10/autocompleteagain.html) |
| **Kroky** | Hello automatického dokončování atribut určuje, zda formuláře by měl mít automatického dokončování zapnout nebo vypnout. Pokud automatického dokončování je zapnutý, hodnot automaticky dokončení hello prohlížeče na základě hodnot tohoto hello uživatel zadá před. Například při zadání nové jméno a heslo ve formuláři a odeslání formuláře hello, hello prohlížeče zobrazí, pokud má být uložen hello heslo. Po tomto datu, když se zobrazí hello formuláře, hello jméno a heslo se vyplní automaticky, nebo jsou dokončit, protože je zadán název hello. Útočník s místního přístupu může získat hesla v nešifrovaném textu hello z mezipaměti prohlížeče hello. Ve výchozím nastavení je povolená funkce automatického dokončování a musí explicitně zakázat. |

### <a name="example"></a>Příklad
```C#
<form action="Login.aspx" method="post " autocomplete="off" >
      Social Security Number: <input type="text" name="ssn" />
      <input type="submit" value="Submit" />    
</form>
```

## <a id="data-mask"></a>Ujistěte se, že citlivá data zobrazí na obrazovce uživatele hello zakryté hvězdičkami

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Citlivá data, jako jsou hesla, čísla platebních karet, číslo sociálního pojištění atd by měl maskovat, kdy se zobrazí na úvodní obrazovka. Toto je tooprevent Neautorizováno pracovníky z přístup k datům hello (například procházení osazení hesla, zobrazení SSN čísel uživatelů pracovníky technické podpory). Zajistěte, aby tyto datové prvky nejsou viditelné ve formátu prostého textu a jsou správně maskování. Tato akce nemá toobe postaráno při přijímání je jako vstup (např., Typ vstupu = "password") a také zobrazení zpět na úvodní obrazovka (například zobrazení pouze hello poslední 4 číslice čísla platební karty hello). |

## <a id="dynamic-users"></a>Implementace dynamická data maskování bez ohrožení citlivých dat toolimit privilegovaný uživatelů

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | SQL Azure a místní |
| **Atributy**              | MsSQL2016 verze - V12, verze SQL - SQL |
| **Odkazy**              | [Dynamická Data maskování](https://msdn.microsoft.com/library/mt130841) |
| **Kroky** | Hello dynamické maskování dat se používá toolimit úniku citlivých dat, brání uživatelům, kteří by neměl mít přístup k datům toohello v jejich zobrazení. Dynamická data maskování není cílem tooprevent databáze uživatelům připojovat přímo toohello databáze a spuštění vyčerpávající dotazů, které zveřejňují kusy hello citlivá data. Dynamická data maskování je funkce zabezpečení systému SQL Server doplňkové tooother (auditování, šifrování, zabezpečení na úrovni řádků...) a toouse důrazně doporučujeme tuto funkci ve spojení s nimi kromě v pořadí toobetter chránit hello citlivá data v hello databáze. Upozorňujeme, že tato funkce je podporována pouze tak, že počínaje 2016 systému SQL Server a databáze SQL Azure. |

## <a id="salted-hash"></a>Ujistěte se, že hesla jsou uložena ve formátu hashe

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Pomocí rozhraní API technologie .NET kryptografických Hashing heslo](http://docs.asp.net/en/latest/security/data-protection/consumer-apis/password-hashing.html) |
| **Kroky** | Hesla by neměl být uložena ve vlastní uživatelské úložiště databáze. Hodnoty hash hesla by měly být uložené s hodnoty řetězce salt místo. Zajistěte, aby hello salt pro uživatele hello je vždy jedinečný a použijete b-crypt, s-crypt nebo PBKDF2 před ukládání hello heslo, minimální pracovní Multi-Factor iterace počet 150 000 cyklu tooeliminate hello možnost hrubou silou.| 

## <a id="db-encrypted"></a>Musí být šifrovaný citlivá data v databázi sloupců

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Verze SQL – všechny |
| **Odkazy**              | [Šifrování důvěrných osobních údajů v systému SQL server](https://technet.microsoft.com/library/ff848751(v=sql.105).aspx), [postupy: šifrování sloupec dat v systému SQL Server](https://msdn.microsoft.com/library/ms179331), [šifrovat pomocí certifikátu](https://msdn.microsoft.com/library/ms188061) |
| **Kroky** | Citlivá data, třeba čísla platebních karet má toobe šifrované v databázi hello. Data mohou být šifrována pomocí šifrování na úrovni sloupce nebo funkcí aplikace pomocí funkcí šifrování hello. |

## <a id="tde-enabled"></a>Ujistěte se, že je povolené šifrování tuto úroveň databáze (TDE)

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Principy SQL serveru transparentní šifrování dat (šifrování TDE)](https://technet.microsoft.com/library/bb934049(v=sql.105).aspx) |
| **Kroky** | Transparentní šifrování šifrování dat (TDE) funkci v aplikaci SQL server pomáhá v šifrování citlivých dat v databázi a chrání hello klíče, které jsou používané tooencrypt hello data s certifikátem. Každý, kdo bez klíče hello zabrání pomocí dat hello. Šifrování TDE chrání data "v klidu", což znamená hello data a soubory protokolu. Poskytuje možnost toocomply hello s mnoha právní a pokyny uvedenými v různých odvětví. |

## <a id="backup"></a>Zajistěte, aby byly šifrované zálohování databáze

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | SQL Azure a místní |
| **Atributy**              | MsSQL2014 verze - V12, verze SQL - SQL |
| **Odkazy**              | [Šifrování zálohování databáze SQL](https://msdn.microsoft.com/library/dn449489) |
| **Kroky** | Při vytváření zálohy má systém SQL Server hello možnost tooencrypt hello data. Zadáním hello šifrovací algoritmus a modul pro šifrování hello (certifikát nebo asymetrický klíč) při vytváření zálohy, můžete jeden vytvořit šifrovaný záložní soubor. |

## <a id="api-browser"></a>Ujistěte se, že relevantní tooWeb citlivá data, které rozhraní API není uložen v úložišti prohlížeče

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC 5, 6 MVC |
| **Atributy**              | Poskytovatel Identity identity zprostředkovatel – AD FS, – Azure AD |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>V některých implementacích citlivé artefakty relevantní tooWeb rozhraní API ověřování jsou uloženy v prohlížeči místní úložiště. Například artefakty ověřování Azure AD, například adal.idtoken, adal.nonce.idtoken, adal.access.token.key, adal.token.keys, adal.state.login, adal.session.state, adal.expiration.key atd.</p><p>Všechny tyto artefakty jsou k dispozici i po odhlášení nebo zavření prohlížeče. Pokud nežádoucí osoba získá přístup toothese artefakty, si můžete je znovu použijte tooaccess hello chráněné prostředky (API). Ujistěte se, že všechny citlivé artefakty související tooWeb rozhraní API není uložena v prohlížeči úložiště. V případech, kdy je nevyhnutelné použít úložiště na straně klienta (například jedné stránky aplikace (SPA), využívat implicitní OpenIdConnect/OAuth toky potřebovat toostore přístupové tokeny místně), použijte možnosti úložiště s nemají trvalost. například přednost SessionStorage tooLocalStorage.</p>| 

### <a name="example"></a>Příklad
Hello následující fragment kódu jazyka JavaScript je z knihovny vlastního ověřování, která ukládá artefakty ověřování v místní úložiště. Je nutno tyto implementace. 
```javascript
ns.AuthHelper.Authenticate = function () {
window.config = {
instance: 'https://login.microsoftonline.com/',
tenant: ns.Configurations.Tenant,
clientId: ns.Configurations.AADApplicationClientID,
postLogoutRedirectUri: window.location.origin,
cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
};
```

## <a id="encrypt-docdb"></a>Šifrování citlivých dat, které jsou uloženy v databázi systému Cosmos

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Documentdb | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Před uložením v dokumentu DB šifrování citlivých dat na úrovni aplikace nebo uložit všechny citlivá data v jiné řešení úložiště, jako je Azure Storage nebo Azure SQL| 

## <a id="disk-vm"></a>Použití Azure Disk Encryption tooencrypt disky používat virtuální počítače

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Hranice vztahů důvěryhodnosti virtuálních počítačů Azure IaaS | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Pomocí Azure Disk Encryption tooencrypt disky používat virtuální počítače](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) |
| **Kroky** | <p>Azure Disk Encryption je nová funkce, která je aktuálně ve verzi preview. Tato funkce umožňuje tooencrypt hello OS disky a datové disky použít podle IaaS virtuálního počítače. Pro systém Windows jsou šifrované jednotky hello pomocí standardních technologií šifrování nástroje BitLocker. Pro systémy Linux jsou disky hello šifrované pomocí hello DM-Crypt technologie. Toto je integrovaná s Azure Key Vault tooallow jste toocontrol a správu hello disku šifrovacích klíčů. Hello řešení Azure Disk Encryption podporuje hello následující tři scénáře šifrování zákazníka:</p><ul><li>Povolte šifrování na nové virtuální počítače IaaS vytvořené zákazníka šifrované soubory virtuálního pevného disku a zadaný zákazníka šifrovací klíče, které jsou uložené v Azure Key Vault.</li><li>Povolte šifrování na nové virtuální počítače IaaS vytvořené z hello Azure Marketplace.</li><li>Povolte šifrování na existující virtuální počítače IaaS už běží v Azure.</li></ul>| 

## <a id="fabric-apps"></a>Šifrování tajných klíčů v aplikace Service Fabric

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Hranice vztahů důvěryhodnosti Service Fabric | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Prostředí – Azure |
| **Odkazy**              | [Správa tajných klíčů v aplikace Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-application-secret-management/) |
| **Kroky** | Tajné klíče může být žádné citlivé informace, jako je například úložiště připojovací řetězce, hesla nebo jiné hodnoty, které by neměly být zpracovány v prostém textu. Použití Azure Key Vault toomanage klíče a tajné klíče v service fabric aplikace. |

## <a id="modeling-teams"></a>Provedení modelování zabezpečení a použít obchodní jednotky nebo týmy případě požadavku

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Dynamics CRM | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Provedení modelování zabezpečení a použít obchodní jednotky nebo týmy případě požadavku |

## <a id="entities"></a>Minimalizovat funkce tooshare přístupu na kritické entity

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Dynamics CRM | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Minimalizovat funkce tooshare přístupu na kritické entity |

## <a id="good-practices"></a>Uživatelé Train na hello rizika spojená s hello funkce sdílené složky Dynamics CRM a postupy dobrý zabezpečení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Dynamics CRM | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Uživatelé Train na hello rizika spojená s hello funkce sdílené složky Dynamics CRM a postupy dobrý zabezpečení |

## <a id="exception-mgmt"></a>Zahrnout pravidla standardy vývoj proscribing zobrazující podrobnosti konfigurace ve správě výjimek

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Dynamics CRM | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Zahrnout pravidla standardy vývoj proscribing zobrazuje podrobnosti konfigurace ve správě výjimka mimo vývoj. Testování pro tento jako součást recenze kódu nebo pravidelné kontroly.|

## <a id="sse-preview"></a>Používat šifrování služby úložiště Azure (SSE) dat v klidovém stavu (Preview)

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | StorageType – objekt Blob |
| **Odkazy**              | [Šifrování služby úložiště Azure pro Data v klidovém stavu (Preview)](https://azure.microsoft.com/documentation/articles/storage-service-encryption/) |
| **Kroky** | <p>Azure Storage Service šifrování (SSE) pro Data v klidovém stavu pomáhá chránit a chrání vaše data toomeet zabezpečení organizace a dodržování předpisů závazky. Pomocí této funkce Azure Storage automaticky toostorage předchozí toopersisting vaše data šifruje a dešifruje předchozí tooretrieval. Dobrý den šifrování, dešifrování a klíč správy je zcela transparentní toousers. SSE platí pouze tooblock BLOB, objekty BLOB stránky a doplňovací objekty BLOB. Hello dalších typů dat, včetně tabulek, front a soubory, nebudou šifrována.</p><p>Šifrování a dešifrování pracovního postupu:</p><ul><li>Zákazník Hello povoluje šifrování na účet úložiště hello</li><li>Když zákazník hello zapíše nové úložiště dat (PUT objektů Blob, PUT bloku, PUT stránky atd.) tooBlob; každém zápisu je zašifrovaná pomocí šifrování AES 256 bitů, jeden z hello nejsilnější bloku šifry k dispozici</li><li>Když zákazník hello potřebuje tooaccess dat (získání objektu Blob atd.), jsou data automaticky dešifrována před vrácením toohello uživatele</li><li>Pokud šifrování je zakázáno, nové zápisy již nejsou zašifrované a existující šifrovaná data zůstává zašifrovaný, dokud přepsaná uživatelem hello. Při šifrování je povoleno, bude se šifrovat úložiště tooBlob zápisy. Stav Hello dat s uživatelem hello při přepínání mezi povolení nebo zákaz šifrování pro účet úložiště hello nezmění.</li><li>Všechny šifrovací klíče jsou uložené, zašifrovaná a spravován společností Microsoft</li></ul><p>Upozorňujeme, že v tomto okamžiku hello klíče pro šifrování hello spravuje Microsoft. Společnost Microsoft generuje hello klíče původně a spravovat hello zabezpečeného úložiště hello klíče, jakož i regulární otočení hello podle definice interní zásady Microsoft. V budoucnu hello, zákazníkům získáte možnost toomanage hello vlastní > šifrovacích klíčů a zadejte cestu migrace z klíče spravované Microsoftem spravované toocustomer klíče.</p>| 

## <a id="client-storage"></a>Pomocí šifrování na straně klienta toostore citlivá data ve službě Azure Storage

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Šifrování na straně klienta a Azure Key Vault pro Microsoft Azure Storage](https://azure.microsoft.com/documentation/articles/storage-client-side-encryption/), [kurz: šifrování a dešifrování objektů BLOB v úložišti Microsoft Azure pomocí Azure Key Vault](https://azure.microsoft.com/documentation/articles/storage-encrypt-decrypt-blobs-key-vault/), [bezpečné ukládání dat v Azure Blob Úložiště s příponami Azure šifrování](https://blogs.msdn.microsoft.com/partnercatalystteam/2015/06/17/storing-data-securely-in-azure-blob-storage-with-azure-encryption-extensions/) |
| **Kroky** | <p>Hello Klientská knihovna pro úložiště Azure pro balíček Nuget pro rozhraní .NET podporuje šifrování dat v rámci klientské aplikace před nahráním tooAzure úložiště a dešifrování dat při stahování toohello klienta. Hello knihovna také podporuje integraci s Azure Key Vault pro správu klíčů účtu úložiště. Zde je stručný popis toho, jak funguje šifrování na straně klienta:</p><ul><li>Klient Azure Storage Hello SDK vygeneruje obsahu šifrovací klíč (CEK), což je použití jednoho bránu symetrický klíč</li><li>Data zákazníků se šifruje pomocí této CEK</li><li>Hello CEK je vnořena (šifrované) pomocí hello klíče šifrovacího klíče (KEK). Hello KEK je identifikovaná identifikátorem klíče a může být pár asymetrických klíčů nebo symetrického klíče a může být spravovaný místně nebo uložené v Azure Key Vault. Hello klienta úložiště, samotné nikdy přístup toohello KEK. Vyvolá právě hello klíče zabalení algoritmus, který zajišťuje Key Vault. Zákazníci můžete zvolit toouse vlastního zprostředkovatele pro klíč zabalení/rozbalování Pokud chtějí</li><li>Hello šifrovaná data se pak nahrán toohello služby Azure Storage. Zkontrolujte hello odkazy v části odkazy hello podrobnosti implementace nižší úrovně.</li></ul>|

## <a id="pii-phones"></a>Šifrování velká a malá písmena nebo PII dat zapsaných toophones místní úložiště

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Mobilního klienta | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, Xamarin  |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Správa nastavení a funkcí v zařízeních pomocí zásad Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/manage-settings-and-features-on-your-devices-with-microsoft-intune-policies#create-a-configuration-policy), [osobní služby řetězce klíčů](https://components.xamarin.com/view/square.valet) |
| **Kroky** | <p>Pokud aplikace hello zapisuje citlivé informace, jako je PII uživatele (e-mailu, telefonní číslo, křestní jméno, příjmení a předvolby atd.) – v systému souborů pro mobilní, pak jej by se měla šifrovat před zápisem toohello místního systému souborů. Pokud aplikace hello podniková aplikace, které byste měli prozkoumejte hello možnost publikování aplikací pomocí služby Windows Intune.</p>|

### <a name="example"></a>Příklad
Intune můžete nakonfigurovat následující zásady toosafeguard citlivá data zabezpečení: 
```C#
Require encryption on mobile device    
Require encryption on storage cards
Allow screen capture
```

### <a name="example"></a>Příklad
Pokud aplikace hello není podniková aplikace a pak použijte platforma poskytovaná úložiště klíčů, řetězce klíčů toostore šifrovací klíče, pomocí které kryptografické operace může provést v systému souborů hello. Následující kód fragment kódu ukazuje, jak tooaccess klíče z řetězce klíčů pomocí xamarin: 
```C#
        protected static string EncryptionKey
        {
            get
            {
                if (String.IsNullOrEmpty(_Key))
                {
                    var query = new SecRecord(SecKind.GenericPassword);
                    query.Service = NSBundle.MainBundle.BundleIdentifier;
                    query.Account = "UniqueID";

                    NSData uniqueId = SecKeyChain.QueryAsData(query);
                    if (uniqueId == null)
                    {
                        query.ValueData = NSData.FromString(System.Guid.NewGuid().ToString());
                        var err = SecKeyChain.Add(query);
                        _Key = query.ValueData.ToString();
                    }
                    else
                    {
                        _Key = uniqueId.ToString();
                    }
                }

                return _Key;
            }
        }
```

## <a id="binaries-end"></a>Před distribucí uživatelům tooend obfuskováním generovaného binárních souborů

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Mobilního klienta | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Kryptografické maskováním pro .net](http://www.ssware.com/cryptoobfuscator/obfuscator-net.htm) |
| **Kroky** | Vygenerovaný binárních souborů (sestavení v rámci apk) by měl být zkomolené toostop zpětnou analýzu sestavení. Nástroje, jako `CryptoObfuscator` mohou být použity pro tento účel. |

## <a id="cert"></a>Sada clientCredentialType tooCertificate nebo systému Windows

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Rozhraní .NET framework 3 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Obohacení](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | Pomocí UsernameToken heslo jako prostý text přes nezašifrované kanály zpřístupní tooattackers hello heslo, kdo, můžete zachytit hello protokolu SOAP zprávy. Poskytovatelé služeb, který použít hello UsernameToken může přijmout hesla odesílána v podobě prostého textu. Odesílání nezašifrovaná hesla přes nezašifrované kanály můžou zpřístupnit tooattackers hello přihlašovacích údajů, kdo, můžete zachytit uvítací zprávu protokolu SOAP. | 

### <a name="example"></a>Příklad
Hello následující konfigurace poskytovatele služeb WCF používá hello UsernameToken: 
```
<security mode="Message"> 
<message clientCredentialType="UserName" />
``` 
Nastavte clientCredentialType tooCertificate nebo systému Windows. 

## <a id="security"></a>Není povolen režim zabezpečení WCF

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecná rozhraní .NET Framework 3 |
| **Atributy**              | Zabezpečení režimu – přenosu, režimu zabezpečení – zpráva |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html), [časopis CoDe Základy zabezpečení WCF](http://www.codemag.com/article/0611051) |
| **Kroky** | Nebylo definováno žádné zabezpečení přenosu nebo zprávy. Aplikace, které přenosu zpráv bez přenos nebo zpráva, že zabezpečení nemůže zaručit hello integrity nebo důvěrnosti zpráv hello. Vazby WCF zabezpečení nastavena tooNone, jsou zakázané zabezpečení přenosu a zprávy. |

### <a name="example"></a>Příklad
Hello následující konfiguraci nastaví tooNone režim zabezpečení hello. 
```
<system.serviceModel> 
  <bindings> 
    <wsHttpBinding> 
      <binding name=""MyBinding""> 
        <security mode=""None""/> 
      </binding> 
  </bindings> 
</system.serviceModel> 
```

### <a name="example"></a>Příklad
Režim zabezpečení přes všechny vazby služeb jsou pět režimy možné zabezpečení: 
* Žádné. Vypne zabezpečení. 
* Přenos. Používá přenosu zabezpečení pro vzájemné ověřování a zpráva ochranu. 
* Zpráva. Zabezpečení zpráv se používá pro vzájemné ověřování a zpráva ochranu. 
* Obě. Umožňuje toosupply nastavení pro přenos a zabezpečení na úrovni zpráv (MSMQ pouze podporuje to). 
* TransportWithMessageCredential. Přihlašovací údaje se předávají uvítací zprávu a ochranu zprávy a ověření serveru jsou poskytovány hello přenosové vrstvy. 
* TransportCredentialOnly. Pověření klienta se dokončila s hello transportní vrstvy a použije se žádná zpráva ochrana. Pomocí přenosu a zpráva zabezpečení tooprotect hello integrity a důvěrnosti zpráv. Konfigurace Hello níže informuje hello služby toouse zabezpečení přenosu s přihlašovacími údaji zprávy.
```
<system.serviceModel>
  <bindings>
    <wsHttpBinding>
    <binding name=""MyBinding""> 
    <security mode=""TransportWithMessageCredential""/> 
    <message clientCredentialType=""Windows""/> 
    </binding> 
  </bindings> 
</system.serviceModel> 
```
