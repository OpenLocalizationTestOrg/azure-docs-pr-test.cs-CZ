---
title: "Azure AD Connect: Řešení potíží s chybami při synchronizaci | Microsoft Docs"
description: "Vysvětluje, jak k chybám tootroubleshoot během synchronizace se službou Azure AD Connect."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 2209d5ce-0a64-447b-be3a-6f06d47995f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: af262dfe95d686e34697454c0dfe8b4a6d8693c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-errors-during-synchronization"></a>Řešení potíží s chybami při synchronizaci
Při synchronizaci dat identity z Windows Server Active Directory (AD DS) tooAzure služby Active Directory (Azure AD), může dojít k chybám. Tento článek obsahuje přehled různých typů chyb synchronizace, některé hello možných scénářů, které způsobují tyto chyby a potenciální chyby hello toofix způsoby. Tento článek obsahuje hello běžné typy chyb a nemusí zahrnovat všechny možné chyby hello.

 Tento článek předpokládá hello čtečka je obeznámeni s hello základní [návrh koncepty Azure AD a Azure AD Connect](active-directory-aadconnect-design-concepts.md).

Hello nejnovější verzi služby Azure AD Connect \(srpna 2016 nebo vyšší\), sestavy chyb synchronizace je k dispozici v hello [portálu Azure](https://aka.ms/aadconnecthealth) v rámci služby Azure AD Connect Health pro synchronizaci.

Od 1. září 2016 [Azure Active Directory duplicitní atribut odolnosti](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) funkce bude k dispozici ve výchozím nastavení pro všechny hello *nové* Azure Active Directory klientů. Tato funkce budou automaticky povolené pro existující klienty v nadcházejících měsících hello.

Azure AD Connect provede 3 typy operací z udržuje v synchronizaci adresářů hello: Import a synchronizace a exportu. Chyby může provádět všechny operace hello. Tento článek zaměřuje především na chyby během exportu tooAzure AD.

## <a name="errors-during-export-tooazure-ad"></a>Chyby při exportu tooAzure AD
Následující část popisuje různé typy synchronizace hello chybách, ke kterým může dojít během hello export operaci tooAzure AD pomocí konektoru služby Azure AD. Tento konektor lze identifikovat podle formát názvu hello se "contoso. *onmicrosoft.com*".
Tuto operaci hello vyskytly chyby při exportu tooAzure AD \(přidání, aktualizace, odstranění atd.\) pokus o Azure AD Connect \(synchronizační modul\) v Azure Active Directory se nezdařilo.

![Přehled chyby exportu](./media/active-directory-aadconnect-troubleshoot-sync-errors/Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Chyby neshoda typů dat.
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Popis
* Pokud Azure AD Connect \(synchronizační modul\) dá pokyn Azure objektů služby Active Directory tooadd nebo aktualizace, Azure AD odpovídá hello příchozí objekt, který používá hello **sourceAnchor** atribut toohello  **immutableId** atributů objektů ve službě Azure AD. Toto porovnání se nazývá **pevný odpovídat**.
* Pokud Azure AD **nebyly nalezeny** libovolný objekt, který odpovídá hello **immutableId** atribut s hello **sourceAnchor** atribut příchozí objektu hello před zřizováním Nový objekt, přejde zpět toouse hello ProxyAddresses a UserPrincipalName atributy toofind shody. Toto porovnání se nazývá **logicky odpovídat**. Hello logicky shoda se objekty navrženou toomatch již existuje ve službě Azure AD (která pocházejí ve službě Azure AD) s nové objekty hello se přidat/aktualizovat během synchronizace, které představují hello stejné entit (uživatelů, skupin) místně.
* **InvalidSoftMatch** chyba nastane, když hello pevný shodu nebyly nalezeny žádné odpovídající objekt **a** logicky match najde odpovídající objekt, ale tento objekt má hodnotu různých *immutableId* než hello příchozí objekt *SourceAnchor*, které naznačují, že hello odpovídající objekt byl synchronizován s jiným objektem z místní služby Active Directory.

Aby hello logicky shodu toowork, jinými slovy, toobe objekt hello soft shodná s by neměl mít žádnou hodnotu pro hello *immutableId*. Pokud žádný objekt s *immutableId* sady s hodnotou selhává hello pevný shodují, ale vyhovuje požadavkům hello konfigurace soft-match kritéria, hello operace by způsobilo chybu InvalidSoftMatch synchronizace.

Azure Active Directory, které schéma nepovoluje dva nebo více objektů toohave hello stejnou hodnotu hello následující atributy. \(Nejedná se o vyčerpávající seznam.\)

* ProxyAddresses
* UserPrincipalName
* onPremisesSecurityIdentifier
* objectId

> [!NOTE]
> [Azure AD atribut duplicitní atribut odolnosti](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) je také se nasazuje funkce jako hello výchozí chování služby Azure Active Directory.  Tím se sníží hello počet chyb synchronizace pohledu Azure AD Connect (stejně jako ostatní klienty synchronizace) tím, že Azure AD pružnější způsobem hello zpracovává duplicitní ProxyAddresses UserPrincipalName atributy a v místní AD prostředí. Tato funkce nevyřeší chyb duplikace hello. Hello data, je stále toobe pevné. Ale umožňuje zřizování nových objektů, které jsou jinak blokovaná se zřídí kvůli tooduplicated hodnoty ve službě Azure AD. Tím se sníží také hello počet chyb synchronizace vrátil toohello synchronizace klienta.
> Pokud je tato funkce je povoleno pro vašeho klienta, neuvidíte chyby synchronizace InvalidSoftMatch hello viděli při zřizování nových objektů.
>
>

#### <a name="example-scenarios-for-invalidsoftmatch"></a>Příklady scénářů pro InvalidSoftMatch
1. Dva nebo více objektů s hello stejnou hodnotu atributu ProxyAddresses v místní službě Active Directory existuje. Pouze jedna je získávání zřídí ve službě Azure AD.
2. Dva nebo více objektů s hello stejnou hodnotu userPrincipalName v místní službě Active Directory existuje. Pouze jedna je získávání zřídí ve službě Azure AD.
3. Objekt byl přidán v hello místní služby Active Directory s hello stejná hodnota atributu ProxyAddresses jako existující objekt ve službě Azure Active Directory. Hello objektu přidat místní není získávání zřízený v Azure Active Directory.
4. Objekt je přidaná do místní služby Active Directory s hello stejná hodnota atributu userPrincipalName jako účet ve službě Azure Active Directory. objekt Hello není získávání zřízený v Azure Active Directory.
5. Synchronizovaná účet byl přesunut z doménové struktury A tooForest B. Azure AD Connect (synchronizační stroj) se používá hello toocompute atribut ObjectGUID SourceAnchor. Po přesunutí hello doménové struktury hodnota hello hello SourceAnchor se liší. Nový objekt Hello (z doménové struktury B) selhává toosync s hello existující objekt ve službě Azure AD.
6. Objekt synchronizoval získali omylem odstraněna z místní služby Active Directory a nový objekt, který byl vytvořen ve službě Active Directory pro hello stejné entity (například uživatele) bez odstranění hello účet ve službě Azure Active Directory. nový účet Hello selže toosync s hello existující objekt služby Azure AD.
7. Azure AD Connect byla odinstalovat a znovu nainstalujete. Během opětovné instalace hello jste vybrali jiný atribut jako hello SourceAnchor. Všechny hello objekty, které měl dříve synchronizovány zastavena, synchronizuje s chybou InvalidSoftMatch.

#### <a name="example-case"></a>Příklad případ:
1. **Bob Smith** je synchronizoval uživatele v Azure Active Directory z v místní službě Active Directory *contoso.com*
2. Bob Smith **UserPrincipalName** je nastaven jako  **bobs@contoso.com** .
3. **"abcdefghijklmnopqrstuv =="** je hello **SourceAnchor** vypočítána Azure AD Connect s použitím Bob Smith **objectGUID** z místní služby Active Directory, která je hello **immutableId** Bob Smith v Azure Active Directory.
4. Bob má také následující hodnoty pro hello **proxyAddresses** atribut:
   * smtp:bobs@contoso.com
   * smtp:bob.smith@contoso.com
   * **smtp:bob@contoso.com**
5. Nový uživatel **Bob Taylora**, se přidá toohello místní služby Active Directory.
6. Bob Taylora **UserPrincipalName** je nastaven jako  **bobt@contoso.com** .
7. **"abcdefghijkl0123456789 ==" "** je hello **sourceAnchor** vypočítána Azure AD Connect s použitím Bob Taylora **objectGUID** z na místní služby Active Directory. Bob Taylora objekt má zatím není synchronizovat tooAzure služby Active Directory.
8. Bob Taylora má následující hodnoty pro atribut proxyAddresses hello hello
   * smtp:bobt@contoso.com
   * smtp:bob.taylor@contoso.com
   * **smtp:bob@contoso.com**
9. Během synchronizace Azure AD Connect rozpozná hello přidání Roberta Taylora v místní službě Active Directory a požádejte Azure AD toomake hello stejnou změnu.
10. Azure AD se nejprve provést pevný shodu. To znamená, bude hledat, pokud je jakýkoli objekt s hello immutableId rovna příliš "abcdefghijkl0123456789 ==". Pevné shoda se nezdaří, žádný další objekt ve službě Azure AD bude mít tento immutableId.
11. Azure AD se potom pokusí toosoft-match Bob Taylora. To znamená bude hledat, pokud je jakýkoli objekt s rovna toohello proxyAddresses tří hodnot, včetněsmtp:bob@contoso.com
12. Azure AD se najít objekt Bob Smith toomatch hello konfigurace soft-match kritéria. Ale tento objekt má hodnotu hello immutableId = "abcdefghijklmnopqrstuv ==". Určuje, která tento objekt byl synchronizované z jiného objektu z místní služby Active Directory. Proto Azure AD nejde konfigurace soft-match tyto objekty a má za následek **InvalidSoftMatch** došlo k chybě synchronizace.

#### <a name="how-toofix-invalidsoftmatch-error"></a>Jak toofix InvalidSoftMatch chyba
Hello nejběžnějším důvodem hello InvalidSoftMatch chyba je dva objekty s jinou SourceAnchor \(immutableId\) mají stejnou hodnotu pro ProxyAddresses nebo UserPrincipalName hello atributy, které se používají při hello hello proces konfigurace soft-match na Azure AD. V pořadí toofix hello neplatný logicky shody

1. Identifikujte hello duplicitní proxyAddresses, userPrincipalName nebo jinou hodnotu atributu, která je příčinou chyby hello. Také zjistit, jaké dva \(nebo více\) objekty jsou součástí hello konflikt. Hello sestav generovaných [Azure AD Connect Health pro synchronizaci](https://aka.ms/aadchsyncerrors) můžete identifikovat hello dva objekty.
2. Určete, který objekt by měly pokračovat ve toohave hello duplicitní hodnota a který objekt neměli.
3. Odeberte hello duplicitní hodnotu z hello objektu, který by měl není tuto hodnotu. Všimněte si, že byste měli změnit v hello adresáři, kde se objekt hello pochází z hello. V některých případech může být nutné toodelete jeden z objektů hello v konfliktu.
4. Pokud jste provedli hello změnit v hello místní AD, umožní Azure AD Connect sync hello změnit.

Upozorňujeme, že synchronizace chybách v rámci Azure AD Connect Health pro synchronizaci se aktualizuje každých 30 minut a patří i chyby hello z hello poslední pokus o synchronizaci.

> [!NOTE]
> ImmutableId, podle definice neměli měnit v hello životnosti objektu hello. Pokud u některých scénářů hello v paměti z hello výše seznamu nebylo nakonfigurováno Azure AD Connect, může dojít v situaci, kde Azure AD Connect vypočítá jinou hodnotu hello SourceAnchor pro objekt hello AD, představuje hello stejné entity (stejného uživatele nebo skupiny nebo kontaktu atd.), má existující objekt AD Azure chcete pomocí toocontinue.
>
>

#### <a name="related-articles"></a>Související články
* [Neplatná nebo duplicitní atributy zabránit synchronizaci adresářů v Office 365](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Popis
Když se Azure AD pokusí toosoft shodu dva objekty, je možné, že dva objekty různé "typ objektu" (například uživatelem, skupinou, obraťte se na atd.) mají stejné hodnoty pro atributy hello používá tooperform hello logicky shodu hello. Jako duplikace těchto atributů není povolena ve službě Azure AD, hello operaci může způsobit chyby synchronizace "ObjectTypeMismatch".

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Ukázkové scénáře ObjectTypeMismatch došlo k chybě
* V Office 365 se vytvoří skupinu zabezpečení e-mailu povolen. Správce přidá nového uživatele nebo kontaktujte v místní AD (které má nejsou synchronizovány tooAzure AD ještě) s hello stejnou hodnotu pro atribut ProxyAddresses hello jako hello Office 365 skupiny.

#### <a name="example-case"></a>Příklad případu
1. Správce vytvoří novou skupinu zabezpečení e-mailu povolen v Office 365 pro hello daň oddělení a poskytuje e-mailovou adresu jako tax@contoso.com. Tím se přiřadí hello ProxyAddresses atribut pro tuto skupinu s hodnotou hello**smtp:tax@contoso.com**
2. Nový uživatel připojí Contoso.com a účet se vytvoří pro uživatele hello místně s hello proxyAddress jako**smtp:tax@contoso.com**
3. Pokud Azure AD Connect se budou synchronizovat hello nový uživatelský účet, získají hello "ObjectTypeMismatch" Chyba.

#### <a name="how-toofix-objecttypemismatch-error"></a>Jak toofix ObjectTypeMismatch chyba
Nejčastější příčinou Hello hello ObjectTypeMismatch chyby je, že dva objekty jiného typu (uživatelem, skupinou, obraťte se na atd.) mají stejnou hodnotu pro atribut ProxyAddresses hello hello. V pořadí toofix hello ObjectTypeMismatch:

1. Identifikovat hello duplicitní proxyAddresses (nebo jiný atribut) hodnotu to je chyba což způsobilo hello. Také zjistit, jaké dva \(nebo více\) objekty jsou součástí hello konflikt. Hello sestav generovaných [Azure AD Connect Health pro synchronizaci](https://aka.ms/aadchsyncerrors) můžete identifikovat hello dva objekty.
2. Určete, který objekt by měly pokračovat ve toohave hello duplicitní hodnota a který objekt neměli.
3. Odeberte hello duplicitní hodnotu z hello objektu, který by měl není tuto hodnotu. Všimněte si, že byste měli změnit v hello adresáři, kde se objekt hello pochází z hello. V některých případech může být nutné toodelete jeden z objektů hello v konfliktu.
4. Pokud jste provedli hello změnit v hello místní AD, umožní Azure AD Connect sync hello změnit. Synchronizace chybách v rámci Azure AD Connect Health pro synchronizaci aktualizuje každých 30 minut a patří i chyby hello z hello poslední pokus o synchronizaci.

## <a name="duplicate-attributes"></a>Duplicitní atributy
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Popis
Azure Active Directory, které schéma nepovoluje dva nebo více objektů toohave hello stejnou hodnotu hello následující atributy. To je, že každý objekt ve službě Azure AD je vynucené toohave jedinečnou hodnotu z těchto atributů v dané instanci.

* ProxyAddresses
* UserPrincipalName

Pokud Azure AD Connect se pokusí tooadd nový objekt nebo aktualizovat existující objekt s hodnotou hello výše atributy, která je již přiřazen tooanother objektu ve službě Azure Active Directory, výsledkem operace hello "AttributeValueMustBeUnique" hello k chybě synchronizace.

#### <a name="possible-scenarios"></a>Možných scénářů:
1. Duplicitní hodnota je přiřazené tooan již synchronizovaného objektu, který je v konfliktu s jiným objektem synchronizovaná.

#### <a name="example-case"></a>Příklad případ:
1. **Bob Smith** je synchronizoval uživatele v Azure Active Directory z na místní služby Active Directory contoso.com
2. Bob Smith **UserPrincipalName** místní je nastaven jako  **bobs@contoso.com** .
3. Bob má také následující hodnoty pro hello **proxyAddresses** atribut:
   * smtp:bobs@contoso.com
   * smtp:bob.smith@contoso.com
   * **smtp:bob@contoso.com**
4. Nový uživatel **Bob Taylora**, se přidá toohello místní služby Active Directory.
5. Bob Taylora **UserPrincipalName** je nastaven jako  **bobt@contoso.com** .
6. **Bob Taylora** má následující hodnoty pro hello hello **ProxyAddresses** atribut i. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Bob Taylora objektu je úspěšně synchronizovat s Azure AD.
8. Správce se rozhodli Taylora Bob tooupdate **ProxyAddresses** atribut s hello následující hodnotu: i. **smtp:bob@contoso.com**
9. Azure AD se pokusí Taylora Bob tooupdate objektu ve službě Azure AD s hello větší než hodnota, ale že se nezdaří jako hodnota ProxyAddresses je již přiřazen tooBob Smith, výsledkem je chyba "AttributeValueMustBeUnique".

#### <a name="how-toofix-attributevaluemustbeunique-error"></a>Jak toofix AttributeValueMustBeUnique chyba
Hello nejběžnějším důvodem hello AttributeValueMustBeUnique chyba je dva objekty s jinou SourceAnchor \(immutableId\) mají stejnou hodnotu pro hello ProxyAddresses nebo UserPrincipalName atributy hello. V pořadí toofix AttributeValueMustBeUnique chyba

1. Identifikujte hello duplicitní proxyAddresses, userPrincipalName nebo jinou hodnotu atributu, která je příčinou chyby hello. Také zjistit, jaké dva \(nebo více\) objekty jsou součástí hello konflikt. Hello sestav generovaných [Azure AD Connect Health pro synchronizaci](https://aka.ms/aadchsyncerrors) můžete identifikovat hello dva objekty.
2. Určete, který objekt by měly pokračovat ve toohave hello duplicitní hodnota a který objekt neměli.
3. Odeberte hello duplicitní hodnotu z hello objektu, který by měl není tuto hodnotu. Všimněte si, že byste měli změnit v hello adresáři, kde se objekt hello pochází z hello. V některých případech může být nutné toodelete jeden z objektů hello v konfliktu.
4. Pokud jste provedli hello změnit v hello místní AD, umožní Azure AD Connect sync hello změnit pro tooget chyba hello pevné.

#### <a name="related-articles"></a>Související články
-[Neplatná nebo duplicitní atributy zabránit synchronizaci adresářů v Office 365](https://support.microsoft.com/en-us/kb/2647098)

## <a name="data-validation-failures"></a>Ověřování dat
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Popis
Azure Active Directory vynucuje různé omezení hello samotná data před povolením této data toobe zapisovat do adresáře hello. Toto je tooensure, který koncoví uživatelé získávají hello nejlepší možný prostředí při použití hello aplikace, které jsou závislé na tato data.

#### <a name="scenarios"></a>Scénáře
a. Hodnota atributu UserPrincipalName Hello obsahuje neplatné znaky.
b. atribut UserPrincipalName Hello nedodrží hello požadovaný formát.

#### <a name="how-toofix-identitydatavalidationfailed-error"></a>Jak toofix IdentityDataValidationFailed chyba
a. Zkontrolujte, zda že má tento atribut userPrincipalName hello podporované znaky a požadovaný formát.

#### <a name="related-articles"></a>Související články
* [Příprava tooprovision uživateli prostřednictvím tooOffice synchronizace adresáře 365](https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)

### <a name="federateddomainchangeerror"></a>FederatedDomainChangeError
#### <a name="description"></a>Popis
Toto je zvláštní případ, jejímž výsledkem **"FederatedDomainChangeError"** při změně hello příponu UserPrincipalName uživatele z jedné federovanou doménu tooanother federované domény došlo k chybě synchronizace.

#### <a name="scenarios"></a>Scénáře
Pro synchronizované uživatele bylo změněno hello UserPrincipalName příponu z jedné federovanou doménu tooanother federované domény místně. Například *UserPrincipalName = bob@contoso.com*  změnila příliš*UserPrincipalName = bob@fabrikam.com* .

#### <a name="example"></a>Příklad
1. Bob Smith, účtu pro Contoso.com, získá přidat jako nový uživatel ve službě Active Directory s hello UserPrincipalNamebob@contoso.com
2. Bob přesune tooa různých dělení contoso.com názvem Fabrikam.com a jeho UserPrincipalName je změněny.toobob@fabrikam.com
3. Domény contoso.com a fabrikam.com jsou federovaných domén v Azure Active Directory.
4. UserPrincipalName Boba neaktualizuje a výsledkem chyba synchronizace "FederatedDomainChangeError".

#### <a name="how-toofix"></a>Jak toofix
Pokud přípona UserPrincipalName uživatele byla aktualizována z bob @**contoso.com** toobob @**fabrikam.com**, kde obě **contoso.com** a **fabrikam.com** jsou **federované domény**, postupujte podle těchto kroků toofix hello – Chyba synchronizace

1. Aktualizovat UserPrincipalName hello uživatele ve službě Azure AD z bob@contoso.com toobob@contoso.onmicrosoft.com. Můžete použít následující příkaz prostředí PowerShell s hello modul Azure AD PowerShell hello:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`
2. Povolit hello další synchronizační cyklus tooattempt synchronizace. Tento čas synchronizace bude úspěšné a bude aktualizovat hello UserPrincipalName Bob toobob@fabrikam.com podle očekávání.

#### <a name="related-articles"></a>Související články
* [Změny nejsou synchronizovány pomocí nástroje Azure Active Directory Sync hello po změně hello UPN uživatel účet toouse jiné federované domény](https://support.microsoft.com/en-us/help/2669550/changes-aren-t-synced-by-the-azure-active-directory-sync-tool-after-you-change-the-upn-of-a-user-account-to-use-a-different-federated-domain)

## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Popis
Pokud atribut překračuje limit velikosti, délkový limit nebo ve schématu služby Active Directory Azure nastavení limitu počtu povolené hello, operace synchronizace hello výsledkem hello **LargeObject** nebo **ExceededAllowedLength** došlo k chybě synchronizace. Obvykle k této chybě dojde pro hello následující atributy

* userCertificate
* userSMIMECertificate
* thumbnailPhoto
* proxyAddresses

### <a name="possible-scenarios"></a>Možné scénáře
1. Atribut userCertificate Boba ukládá příliš mnoho tooBob přiřazovat certifikáty. To může zahrnovat certifikáty starší, jejichž platnost vypršela. pevný limit Hello je 15 certifikáty. Další informace o tom, jak toohandle LargeObject chyby s userCertificate atribut, prosím odkazovat tooarticle [LargeObject zpracování chyb vzniklých při userCertificate atribut](active-directory-aadconnectsync-largeobjecterror-usercertificate.md).
2. Atribut userSMIMECertificate Boba ukládá příliš mnoho tooBob přiřazovat certifikáty. To může zahrnovat certifikáty starší, jejichž platnost vypršela. pevný limit Hello je 15 certifikáty.
3. Bob thumbnailPhoto nastavit ve službě Active Directory je příliš velký toobe synchronizovat ve službě Azure AD.
4. Při automatické naplňování hello ProxyAddresses atributu ve službě Active Directory objekt má příliš mnoho ProxyAddresses přiřazen.

### <a name="how-toofix"></a>Jak toofix
1. Zkontrolujte, zda je tento atribut hello příčinou chyby hello v rámci hello povolené omezení.

## <a name="related-links"></a>Související odkazy
* [Vyhledat objekty služby Active Directory v Centru správy služby Active Directory](https://technet.microsoft.com/library/dd560661.aspx)
* [Jak tooquery Azure Active Directory pro objekt, který používá Azure Active Directory PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
