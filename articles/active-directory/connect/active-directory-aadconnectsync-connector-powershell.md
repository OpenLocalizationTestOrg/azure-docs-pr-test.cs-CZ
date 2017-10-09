---
title: aaaPowerShell konektor | Microsoft Docs
description: "Tento článek popisuje, jak tooconfigure Microsoft Windows PowerShell Connector."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6dba8e34-a874-4ff0-90bc-bd2b0a4199b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 44ff6b1f53283000b72e15f861e0f86c21afe12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-powershell-connector-technical-reference"></a>Technické informace o konektoru služby Windows PowerShell
Tento článek popisuje hello konektor služby Windows PowerShell. Hello článek se týká toohello následující produkty:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manageru 2010 R2 (FIM2010R2)
  * Musíte použít opravu hotfix 4.1.3671.0 nebo novější [KB3092178](https://support.microsoft.com/kb/3092178).

Pro MIM2016 a FIM2010R2, je k dispozici ke stažení z hello hello konektor [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-hello-powershell-connector"></a>Přehled hello PowerShell Connector
Hello PowerShell Connector umožňuje toointegrate hello synchronizační služba s externími systémy, které nabízí rozhraními API založenými na prostředí Windows PowerShell. konektor Hello poskytuje most mezi hello možnosti agenta pro správu na základě volání extensible connectivity hello 2 (ECMA2) framework a prostředí Windows PowerShell. Další informace o hello ECMA framework najdete v tématu hello [Extensible Connectivity 2.2 agenta managementu](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Požadavky
Než použijete hello konektor, zkontrolujte, zda že máte na serveru pro synchronizaci hello hello následující:

* 4.5.2 rozhraní Microsoft .NET Framework nebo novější
* Prostředí Windows PowerShell 2.0, 3.0 nebo 4.0

Hello zásady provádění na serveru služby synchronizace hello musí být nakonfigurované tooallow hello konektor toorun prostředí Windows PowerShell skripty. Pokud hello skripty hello konektor spustí jsou digitálně podepsané, nakonfigurujte zásady spouštění hello spuštěním tohoto příkazu:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Vytvořit nový konektor
toocreate konektor prostředí Windows PowerShell v hello synchronizační služby, je nutné zadat řadu skriptů prostředí Windows PowerShell, které jsou spouštěny hello kroky požadoval hello synchronizační služba. V závislosti na zdroj dat hello připojit tooand hello funkce, kterou požadujete se liší hello skripty, které je nutné implementovat. V této části jsou popsané všechny hello skripty, které se dají implementovat a když jsou povinné.

Hello prostředí Windows PowerShell connector je navržená tak toostore hello skriptů uvnitř databáze služby synchronizace hello. I když je možné toorun skripty, které jsou uloženy v systému souborů hello, je snazší textu hello tooinsert každý skript přímo v konfiguraci toohello konektoru.

tooCreate konektor prostředí PowerShell v **synchronizační služba** vyberte **agenta pro správu** a **vytvořit**. Vyberte hello **prostředí PowerShell (Microsoft)** konektor.

![Vytvořit konektor](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Připojení
Zadejte parametry konfigurace pro připojení tooa vzdáleného systému. Tyto hodnoty jsou bezpečně uložené hello synchronizační služby a provedené skriptů prostředí Windows PowerShell k dispozici tooyour při spuštění konektoru hello.

![Připojení](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Můžete nakonfigurovat následující parametry připojení hello:

**Připojení**

| Parametr | Výchozí hodnota | Účel |
| --- | --- | --- |
| Server |<Blank> |Název serveru, který hello konektor musí připojit k. |
| Domény |<Blank> |Doména hello toostore pověření pro použití při spuštění konektoru hello. |
| Uživatel |<Blank> |Uživatelské jméno hello toostore pověření pro použití při spuštění konektoru hello. |
| Heslo |<Blank> |Heslo hello toostore pověření pro použití při spuštění konektoru hello. |
| Zosobnit účet konektoru |False |V případě hodnoty true hello synchronizační služba běží v kontextu hello hello pověření hello skriptů prostředí Windows PowerShell. Pokud je to možné, doporučujeme tuto hello **$Credentials** parametr se předává tooeach skriptu se používá namísto zosobnění. Další informace o další oprávnění, které jsou potřeba toouse tuto možnost, najdete v části [další konfiguraci pro zosobnění](#additional-configuration-for-impersonation). |
| Načíst profil uživatele při zosobnění |False |Dá pokyn profilu uživatele hello tooload Windows hello konektor přihlašovacích údajů během zosobnění. Pokud hello zosobněným uživatelem má cestovní profil, konektor hello nenačte hello cestovní profil. Další informace o další oprávnění, které jsou potřeba toouse tento parametr, najdete v části [další konfiguraci pro zosobnění](#additional-configuration-for-impersonation). |
| Typ přihlášení při zosobnění |Žádný |Typ přihlášení během zosobnění. Další informace najdete v tématu hello [dwLogonType] [ dw] dokumentaci. |
| Pouze podepsaných skriptů |False |V případě hodnoty true hello prostředí Windows PowerShell connector ověří, že každý skript má platný digitální podpis. Pokud je hodnota false, zajistěte, aby byl server synchronizační služba hello zásady spouštění prostředí Windows PowerShell RemoteSigned nebo bez omezení. |

**Běžné modulu**  
Hello konektor umožňuje toostore sdílený modul prostředí Windows PowerShell v konfiguraci hello. Je-li konektor hello spouští skript, hello je modul prostředí Windows PowerShell extrahovat toohello systému souborů tak, aby jej bylo možné importovat každý skript.

Pro Import, Export a synchronizace hesel skripty je běžné modulu hello složka MAData extrahované toohello konektor. Schéma, ověřování, hierarchie a oddílu zjišťování skriptů běžné modul hello je složka extrahované toohello % TEMP %. V obou případech extrahovat hello společný modul s názvem skriptu podle nastavení toohello běžný název modulu skriptu.

tooload modul volá FIMPowerShellConnectorModule.psm1 ze složky MAData hello, použijte následující příkaz hello:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

tooload modul volá FIMPowerShellConnectorModule.psm1 ze složky % TEMP % hello, použijte následující příkaz hello:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Ověření parametru**  
Hello ověření skriptu je volitelné skript prostředí Windows PowerShell, které můžou být použité tooensure, zda jsou parametry konfigurace konektoru poskytovat správce hello platná. Ověřující server, přihlašovací údaje pro připojení a připojení parametry jsou běžné použití hello ověření skriptu. Hello ověření skriptu je volána po hello následující karty a dialogová okna jsou upravit:

* Připojení
* Globální parametry
* Oddíl konfigurace

ověření skriptu Hello obdrží hello z konektoru hello následující parametry:

| Name (Název) | Datový typ | Popis |
| --- | --- | --- |
| ConfigParameterPage |[ConfigParameterPage][cpp] |na kartě Konfigurace Hello nebo dialog, který aktivuje požadavek na ověření hello. |
| ConfigParameters |[Kolekci KeyedCollection] [ keyk] [řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro hello konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce hello zadali na kartě připojení hello. |

Hello ověření skriptu by měl vrátit jeden kanál toohello ParameterValidationResult objektu.

**Zjištění schématu**  
Hello skript zjišťování schématu je povinný. Tento skript vrátí hello typy objektů, atributy a atribut omezení, že hello synchronizační služba používá při konfiguraci pravidla toku atributu. Hello skript schématu zjišťování se spustí při vytváření konektoru a naplní hello konektor schématu. Používá se také podle hello aktualizovat schéma akce v hello Synchronization Service Manager.

skript zjišťování schématu Hello obdrží hello z konektoru hello následující parametry:

| Name (Název) | Datový typ | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection] [ keyk] [řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro hello konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce hello zadali na kartě připojení hello. |

skript Hello musí vracet jedné [schématu] [ schema] objekt toohello kanálu. objekt schématu Hello se skládá z [elementu SchemaType] [ schemaT] objekty, které reprezentují typy objektů (například: uživatelé a skupiny). kolekce má objekt elementu SchemaType Hello [SchemaAttribute] [ schemaA] objekty, které představují hello atributy (například: křestní jméno, příjmení a poštovní adresa) typu hello.

**Další parametry**  
Kromě toho toohello standardního nastavení konfigurace, můžete definovat další vlastní konfigurační nastavení, která jsou konkrétní toohello instanci hello konektor. Tyto parametry mohou být zadané v hello konektor, oddíl nebo kroku spuštění úrovně a k němu přistupovat z hello relevantní skript prostředí Windows PowerShell. Vlastní nastavení konfigurace mohou být uloženy v databázi synchronizační služby hello ve formátu prostého textu nebo může být zašifrovaná. Hello synchronizační služba automaticky šifruje a dešifruje zabezpečenou konfiguraci nastavení, pokud je potřeba.

nastavení vlastní konfigurace toospecify, samostatné hello název každého parametru čárkou (,).

nastavení vlastní konfigurace tooaccess ze skriptu, musí přípona hello název podtržítko ( \_ ) a obor hello parametru hello (globální, oddíl nebo RunStep). Například tooaccess hello parametr FileName globální, použijte tento fragment kódu:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Možnosti
Karta Možnosti Hello hello Návrhář agenta správy definuje chování hello a funkce konektoru hello. Výběr Hello provedené na této kartě nelze upravovat, když byl vytvořen konektor hello. Tato tabulka uvádí nastavení schopností hello.

![Možnosti](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

| Schopnost | Popis |
| --- | --- |
| [Rozlišující název stylu][dnstyle] |Označuje, zda hello konektor podporuje rozlišujícími názvy, a pokud ano, jaké stylu. |
| [Typ exportu][exportT] |Určuje typ hello objektů, které jsou uvedené toohello skripty pro Export. <li>AttributeReplace – zahrnuje hello úplnou sadu hodnot pro atribut s více hodnotami, změní se atribut hello.</li><li>AttributeUpdate – zahrnuje pouze hello rozdílů tooa více hodnot atributů, změní se atribut hello.</li><li>MultivaluedReferenceAttributeUpdate – obsahuje úplnou sadu hodnot pro více hodnot atributů – referenční dokumentace a rozdílů pouze pro atributy typu odkaz s více hodnotami.</li><li>ObjectReplace – obsahuje všechny atributy pro objekt, pokud žádné změny atributů</li> |
| [Normalizaci dat][DataNorm] |Dá pokyn hello synchronizační služba toonormalize ukotvení atributy, než jsou k dispozici tooscripts. |
| [Objekt potvrzení][oconf] |Nakonfiguruje hello čekající na vyřízení import chování v hello synchronizační služba. <li>Normální – výchozí chování, která očekává všechny toobe exportovaný změny potvrzeny prostřednictvím import</li><li>NoDeleteConfirmation – když je odstraněn objekt, neexistuje žádný čekajícího importu vygenerovat.</li><li>NoAddAndDeleteConfirmation – při vytvoření nebo odstranění objektu není žádná čekajícího importu vygenerovat.</li> |
| Používat jako kotvu rozlišující název |Pokud hello rozlišující název styl nastaven tooLDAP, atribut kotvy hello hello konektoru místo je také hello rozlišující název. |
| Souběžných operací několik konektorů |Pokud je zaškrtnuto, můžete současně spustit více konektorů prostředí Windows PowerShell. |
| Oddíly |Pokud je zaškrtnuto, hello konektor podporuje víc oddílů a zjišťování oddílu. |
| Hierarchie |Pokud je zaškrtnuto, hello konektor podporuje hierarchickou strukturu styl LDAP. |
| Povolit Import |Pokud je zaškrtnuto, hello konektor importuje data prostřednictvím import skriptů. |
| Povolit rozdílový Import |Pokud je zaškrtnuto, můžete konektor hello požadavku rozdílů z hello importovat skripty. |
| Povolit Export |Pokud je zaškrtnuto, konektor hello exportuje data prostřednictvím export skripty. |
| Povolit úplnou Export |Pokud je zaškrtnuto, exportujte hello skripty podpory export prostoru konektoru celý hello. toouse, které tuto možnost povolit Export musí také kontrolovat. |
| Žádné hodnoty odkaz při průchodu první exportu |Pokud je zaškrtnuto, exportují se v druhé fázi exportu atributy typu odkaz. |
| Povolit přejmenování objektu |Pokud je zaškrtnuto, můžete upravit rozlišujícími názvy. |
| Přidat jako nahradit odstranění |Pokud je zaškrtnuto, delete přidáte operace jsou exportovány jako jeden nahrazení. |
| Povolit operace s heslem |Pokud je zaškrtnuto, jsou podporovány skripty synchronizace hesel. |
| Povolit Export hesla při prvním průchodu |Pokud je zaškrtnuto, exportují se po vytvoření objektu hello hesla nastavit během zřizování. |

### <a name="global-parameters"></a>Globální parametry
Karta Hello globální parametry v hello Návrhář agenta správy umožňuje tooconfigure hello prostředí Windows PowerShell skripty, které jsou spuštěny konektorem hello. Můžete také nakonfigurovat globální hodnoty pro vlastní nastavení, které jsou definované na kartě připojení hello.

**Oddíl zjišťování**  
Oddíl je samostatný obor názvů v rámci jedné sdílené schématu. Například všechny domény ve službě Active Directory je oddílu v rámci jedné doménové struktuře. Oddíl je hello logické seskupení pro import a export operace. Import a Export mít oddílu, jako v tomto kontextu se stane kontextu a všechny operace. Oddíly jsou by měl toorepresent hierarchie v protokolu LDAP. Hello rozlišující název oddílu se používá v importu tooverify všechny vrácené objekty jsou v rámci oboru hello oddílu. rozlišující název oddílu Hello se také používá při zřizování z hello metaverse toohello konektoru místo toodetermine hello oddílu, který objekt by měly být přidružené během exportu.

skript zjišťování oddílu Hello obdrží hello z konektoru hello následující parametry:

| Name (Název) | Datový typ | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro hello konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce hello zadali na kartě připojení hello. |

Hello skriptu musí vracet buď jediný [oddílu] [ part] objekt nebo seznam [T] oddílu objekty toohello kanálu.

**Zjištění hierarchie**  
skript zjišťování hierarchie Hello se používá pouze v případě hello rozlišující název stylu schopností LDAP. skript Hello je použité tooallow jste toobrowse a vyberte sadu kontejnery, která je považováno za v nebo mimo rozsah pro import a export operace. Hello skriptu by měl poskytovat pouze seznam uzlů, které jsou přímo podřízené hello kořenový uzel zadaný toohello skriptu.

skript zjišťování hierarchie Hello obdrží hello z konektoru hello následující parametry:

| Name (Název) | Datový typ | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro hello konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce hello zadali na kartě připojení hello. |
| ParentNode |[HierarchyNode][hn] |Hello kořenový uzel hierarchie hello v rámci které hello skriptu by měla vrátit přímé podřízené objekty. |

skript Hello musí vrátit buď jeden podřízený objekt HierarchyNode nebo seznam [T] podřízené HierarchyNode objekty toohello kanálu.

#### <a name="import"></a>Import
Konektory, které podporují operace importu musí implementovat tři skripty.

**Zahájení importu**  
Hello zahájení importu spuštění skriptu na začátku hello na import spustit krok. Během tohoto kroku můžete určit připojení toohello zdrojového systému a proveďte přípravné kroky a pak import dat z hello připojený systém.

Hello zahájení importu skriptu přijímá následující parametry z konektoru hello hello:

| Name (Název) | Datový typ | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro hello konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce hello zadali na kartě připojení hello. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informuje o hello typ importu spustit (Rozdílová nebo úplné), oddíl, hierarchie, vodoznak, velikost očekávané stránky hello skriptu. |
| Typy |[Schéma][schema] |Schéma pro hello prostoru konektoru, který není importován. |

skript Hello musí vracet jedné [OpenImportConnectionResults] [ oicres] objektu toohello kanálu, například:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Umožňuje importovat Data**  
Hello importu dat skript se nazývá konektorem hello, dokud hello skriptu určuje, že neexistuje žádná další data tooimport. konektor Hello prostředí Windows PowerShell má velikost stránky objektů 9 999. Pokud váš skript vrátí víc než 9 999 objekty pro import, musí podporovat stránkování. zpřístupňuje Hello konektor, vlastnost vlastních dat, které můžete použít úložiště tooa vodoznak, tak, aby každý čas hello importovat data skriptu je volána, váš skript obnoví importu objektů, kde bylo přerušeno.

Hello importu dat skriptu obdrží hello z konektoru hello následující parametry:

| Name (Název) | Datový typ | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro hello konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce hello zadali na kartě připojení hello. |
| GetImportEntriesRunStep |[ImportRunStep][irs] |Blokování hello vodoznak (CustomData), který se dá použít během stránkové importy a importuje delta. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informuje o hello typ importu spustit (Rozdílová nebo úplné), oddíl, hierarchie, vodoznak, velikost očekávané stránky hello skriptu. |
| Typy |[Schéma][schema] |Schéma pro hello prostoru konektoru, který není importován. |

Hello skriptu pro import dat musíte napsat seznam [[CSEntryChange][csec]] objekt toohello kanálu. Tato kolekce se skládá z CSEntryChange atributy, které představují každý importovaný objekt. Během spuštění úplný Import tuto kolekci měli kompletní CSEntryChange objektů, které mají všechny atributy pro každý objekt. Během rozdílový Import by měly obsahovat hello CSEntryChange objekt buď hello atribut úrovně rozdíly pro každý objekt tooimport nebo dokončení reprezentace hello objekty, které se změnily (nahraďte režim).

**End Import**  
Na závěr hello hello importu spustit hello End importovat skriptu běží. Tento skript by měl provádět všechny úlohy nezbytné čištění (například zavřít připojení toosystems a reakce toofailures).

Hello end importního skriptu obdrží hello z konektoru hello následující parametry:

| Name (Název) | Datový typ | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro hello konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce hello zadali na kartě připojení hello. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informuje o hello typ importu spustit (Rozdílová nebo úplné), oddíl, hierarchie, vodoznak, velikost očekávané stránky hello skriptu. |
| CloseImportConnectionRunStep |[CloseImportConnectionRunStep][cecrs] |Informuje o hello import bylo ukončeno z důvodu hello hello skriptu. |

skript Hello musí vracet jedné [CloseImportConnectionResults] [ cicres] objektu toohello kanálu, například:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Export
Architektura import identické toohello hello konektoru, konektory, které podporují Export musí implementovat tři skripty.

**Zahájení exportu**  
Hello začít export spuštění skriptu na začátku hello na krok exportu. Během tohoto kroku můžete určit připojení toohello zdrojového systému a proveďte všechny přípravné kroky před exportem dat toohello připojený systém.

Hello začít export skriptu přijímá následující parametry z konektoru hello hello:

| Name (Název) | Datový typ | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro hello konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce hello zadali na kartě připojení hello. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informuje o hello typ oddílu export spustit (Rozdílová nebo úplné), hierarchie a velikost očekávané stránky hello skriptu. |
| Typy |[Schéma][schema] |Schéma pro hello prostoru konektoru, která je exportována. |

Hello skriptu by neměla vrátit všechny výstup toohello kanálu.

**Export dat**  
Hello synchronizační služba volá hello exportovat Data skriptu jako tolikrát, kolikrát je nutné tooprocess všechny čekající exporty. Pokud má prostor konektoru hello další čekající exporty než hello velikost stránky konektoru, hello hello exportu dat skriptu může volat vícekrát a pravděpodobně vícekrát pro stejný objekt.

Hello export dat skriptu obdrží hello z konektoru hello následující parametry:

| Name (Název) | Datový typ | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro hello konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce hello zadali na kartě připojení hello. |
| CSEntries |IList[CSEntryChange][csec] |Seznam všech prostoru konektoru hello objekty s čekající exporty toobe zpracovat během této relace. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informuje o hello typ oddílu export spustit (Rozdílová nebo úplné), hierarchie a velikost očekávané stránky hello skriptu. |
| Typy |[Schéma][schema] |Schéma pro hello prostoru konektoru, která je exportována. |

Hello skripty pro export dat musí vracet [PutExportEntriesResults] [ peeres] objekt toohello kanálu. Tento objekt nemusí tooinclude výsledek informace pro každý exportovaný konektor, pokud dojde k chybě nebo atribut změnu toohello ukotvení. Například tooreturn PutExportEntriesResults objekt toohello kanál:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**End Export**  
Na závěr hello hello exportu spusťte, hello exportovat End toorun skriptu. Tento skript by měl provádět všechny úlohy nezbytné čištění (například zavřít připojení toosystems a reakce toofailures).

skripty pro export end Hello obdrží hello z konektoru hello následující parametry:

| Name (Název) | Datový typ | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro hello konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce hello zadali na kartě připojení hello. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informuje o hello typ oddílu export spustit (Rozdílová nebo úplné), hierarchie a velikost očekávané stránky hello skriptu. |
| CloseExportConnectionRunStep |[CloseExportConnectionRunStep][cecrs] |Informuje o hello exportu byla ukončena z důvodu hello hello skriptu. |

Hello skriptu by neměla vrátit všechny výstup toohello kanálu.

#### <a name="password-synchronization"></a>Synchronizace hesel
Konektory prostředí Windows PowerShell můžete použít jako cíl pro změny nebo resetování hesla.

skript heslo Hello obdrží hello z konektoru hello následující parametry:

| Name (Název) | Datový typ | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro hello konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce hello zadali na kartě připojení hello. |
| Oddíl |[Oddíl][part] |Oddíl adresáře, který hello CSEntry je v. |
| CSEntry |[CSEntry][cse] |Položka místa konektoru pro hello objekt, který je přijala změny hesla nebo resetování. |
| Typ operace |Řetězec |Značí, zda text hello operaci provést obnovení (**SetPassword**) nebo ke změně (**Změna hesla byla**). |
| PasswordOptions |[PasswordOptions][pwdopt] |Příznaky, které určují hello určené chování pro resetování hesla. Tento parametr je k dispozici, pokud je typ operace pouze **SetPassword**. |
| Původní_heslo |Řetězec |Vyplní objekt hello staré heslo pro změny hesla. Tento parametr je k dispozici, pokud je typ operace pouze **Změna hesla byla**. |
| Nové_heslo |Řetězec |Vyplní objekt hello nové heslo, který musí nastavit hello skriptu. |

Hello skriptu heslo není očekávaný tooreturn zřetězením příkazů Windows Powershellu toohello žádné výsledky. Pokud dojde k chybě ve skriptu hello heslo, mají hello skriptu vyvolat jeden z následujících výjimek tooinform hello synchronizační služba o problému hello hello:

* [PasswordPolicyViolationException] [ pwdex1] – vygeneruje se, pokud hello heslo nesplňuje zásady hesel hello systému hello připojen.
* [PasswordIllFormedException] [ pwdex2] – vygeneruje se, pokud heslo hello není přijatelné pro hello připojený systém.
* [PasswordExtension] [ pwdex3] – vyvolána pro všechny chyby ve skriptu hello heslo.

## <a name="sample-connectors"></a>Ukázka konektory
Úplný přehled hello k dispozici ukázkový konektory, najdete v části [Windows PowerShell Connector ukázka konektor kolekce][samp].

## <a name="other-notes"></a>Další poznámky
### <a name="additional-configuration-for-impersonation"></a>Další konfiguraci pro zosobnění
Udělit hello uživatele, který je zosobněnou hello následující oprávnění na serveru služby synchronizace hello:

Přístup pro čtení toohello následující klíče registru:

* HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
* HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

toodetermine hello identifikátor zabezpečení (SID) účtu služby synchronizační služba hello, spusťte následující příkazy prostředí PowerShell hello:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Přístup pro čtení toohello následující složky systému souborů:

* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\Extensions
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\MaData\\{Názevkonektoru}

Nahraďte zástupný symbol hello {Názevkonektoru} hello název konektoru hello prostředí Windows PowerShell.

## <a name="troubleshooting"></a>Řešení potíží
* Informace o tom, jak protokolování tooenable tootroubleshoot hello konektoru najdete v tématu hello [jak tooEnable trasování ETW pro konektory](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
