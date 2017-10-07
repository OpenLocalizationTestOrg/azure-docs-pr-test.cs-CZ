---
title: "odolnost aaaIdentity synchronizace a duplicitní atribut | Microsoft Docs"
description: "Nové chování jak toohandle objekty s konflikty UPN nebo ProxyAddress během synchronizace adresářů přes Azure AD Connect."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 537a92b7-7a84-4c89-88b0-9bce0eacd931
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.openlocfilehash: e27dcbf9d71f83fa9566cae2fd99350297d1cd9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Synchronizace identit a odolnost duplicitních atributů
Duplicitní atribut odolnosti je funkce v Azure Active Directory, který bude eliminovat třecí způsobené **UserPrincipalName** a **ProxyAddress** konfliktu při spuštění jeden společnosti Microsoft Nástroje pro synchronizaci.

Tyto dva atributy se obecně vyžadují toobe jedinečná napříč všemi **uživatele**, **skupiny**, nebo **kontaktujte** objekty v daném klienta Azure Active Directory.

> [!NOTE]
> Pouze uživatelé, může mít názvy UPN.
> 
> 

Hello nové chování, které tato funkce umožňuje je v hello cloudové části hello synchronizace kanálu, a proto je klient lhostejné a relevantní pro synchronizaci produktů společnosti Microsoft včetně Azure AD Connect, DirSync a MIM + konektor. Obecný pojem "sync client" Hello se používá v této dokumentu toorepresent kterékoli z těchto produktů.

## <a name="current-behavior"></a>Aktuální chování
Pokud dojde pokusu o tooprovision nový objekt s hodnotou UPN nebo ProxyAddress, která porušuje toto omezení jedinečnosti, Azure Active Directory blokuje tento objekt z vytváří. Podobně pokud je aktualizován objekt jedinečný UPN nebo ProxyAddress, hello se aktualizace nezdaří. Hello zřizování pokus nebo aktualizace se pokus o synchronizaci klientem hello při každém exportu cyklu a pokračuje toofail, dokud hello konflikt bude vyřešen. Při každém pokusu o se vygeneruje chyba sestav e-mailu a zaznamenána chyba synchronizace klientem hello.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Chování při odolnosti duplicitní atribut
Místo zcela selhání tooprovision nebo aktualizovat objekt s duplicitní atribut Azure Active Directory "umístí do karantény" hello duplicitní atribut, který by způsobila porušení omezení jedinečnosti hello. Pokud tento atribut je požadován pro zřizování, jako jsou UserPrincipalName, služba hello přiřadí hodnotu zástupného symbolu. Formát Hello dočasné hodnoty  
"***<OriginalPrefix>+ < 4DigitNumber > @<InitialTenantDomain>. onmicrosoft.com***".  
Pokud atribut hello není potřeba, jako například **ProxyAddress**, Azure Active Directory jednoduše umístí do karantény hello konflikt atribut a pokračuje v hello objekt vytvoření nebo aktualizace.

Při umístění do karantény hello atribut, se odešlou informace o hello konflikt v hello používá e-mailu sestavy stejné chyby v původním chování hello. Však tyto informace se zobrazí pouze v chybách hello jednou, když se stane hello umístění do karantény, ho nebude dále, že toobe zaznamenána v budoucnosti e-mailů. Navíc vzhledem k tomu hello exportu pro tento objekt byl úspěšný, hello synchronizace klienta neprotokoluje chybu a nepodporuje opakování hello vytvořit a operace při následné synchronizaci cykly aktualizací.

toosupport, toto chování nový atribut byla úspěšně přidána toohello uživatele, skupiny a obraťte se na objekt třídy:  
**DirSyncProvisioningErrors**

To je více hodnotami atributu, který se hello použité toostore konfliktní se atributy, které by způsobila porušení omezení jedinečnosti hello se přidaly normálně. Časovač úlohy na pozadí byla povolena v Azure Active Directory, který spouští každou hodinu toolook duplicitní atribut konflikty, které byly vyřešeny a automaticky odebere hello atributy dotyčném z karantény.

### <a name="enabling-duplicate-attribute-resiliency"></a>Povolíte odolnost duplicitní atribut
Duplicitní atribut odolnosti bude hello nové chování napříč všech klientech služby Azure Active Directory. Na bude ve výchozím nastavení pro všechny klienty, kteří povolena synchronizace pro hello poprvé 22. srpna 2016 nebo novější. Klienty, kteří povolena synchronizace předchozí toothis datum bude mít povolena v dávkách funkce hello. Zavádění tohoto řešení bude zahájena v září 2016 a e-mailových oznámení bude odeslán tooeach klienta technické oznámení kontaktu s hello určité datum, kdy bude hello funkce povolena.

> [!NOTE]
> Jakmile je zapnutý duplicitní atribut odolnosti nelze zakázat.

toocheck, pokud je povolená funkce hello pro vašeho klienta, můžete tak učinit stahování hello nejnovější verzi hello modul Azure Active Directory PowerShell a spuštěním:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

> [!NOTE]
> Už můžete sadu MsolDirSyncFeature rutiny tooproactively povolit hello duplicitní atribut odolnosti funkce předtím, než je zapnutý pro vašeho klienta. Funkce hello možné tootest toobe, budete potřebovat toocreate nového klienta Azure Active Directory.

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Identifikaci objektů s DirSyncProvisioningErrors
Aktuálně dvěma způsoby tooidentify objekty, které mají tyto chyby z důvodu konfliktu vlastnost tooduplicate, Azure Active Directory PowerShell a hello portálu pro správu Office 365. Existují plány tooextend tooadditional portál na základě vytváření sestav v budoucnu hello.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
Pro hello rutiny prostředí PowerShell v tomto tématu platí následující hello:

* Všechny hello následující rutiny jsou velká a malá písmena.
* Hello **– ErrorCategory PropertyConflict** musí být zahrnut. Aktuálně nejsou žádné jiné typy **ErrorCategory**, ale může prodloužit v budoucnu hello.

Nejprve Začínáme spuštěním **Connect-MsolService** a zadání přihlašovacích údajů správce klienta.

Pak použijte hello následující rutiny a operátory tooview chyby různými způsoby:

1. [Zobrazit všechny](#see-all)
2. [Podle typu vlastnosti](#by-property-type)
3. [Podle konfliktní hodnoty](#by-conflicting-value)
4. [Pomocí vyhledávání řetězec](#using-a-string-search)
5. [Seřadit](#sorted)
6. [V omezené množství nebo všechny](#in-a-limited-quantity-or-all)

#### <a name="see-all"></a>Zobrazit všechno
Po připojení toosee obecnější seznam atributů chyby v klientovi hello zřizování spusťte:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Tímto se vytvoří výsledek jako hello následující:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  

#### <a name="by-property-type"></a>Podle typu vlastnosti
chyby toosee podle typu vlastnost přidat hello **- PropertyName** příznak s hello **UserPrincipalName** nebo **ProxyAddresses** argument:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Nebo

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Podle konfliktní hodnoty
toosee chyby týkající se určité vlastnosti tooa přidat hello **- PropertyValue** příznak (**- PropertyName** musí použít také při přidávání tento příznak):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`

#### <a name="using-a-string-search"></a>Pomocí vyhledávání řetězec
toodo široký řetězec hledání použijte hello **- SearchString** příznak. Dá se použít samostatně ze všech hello výše příznaky, s výjimkou hello **- ErrorCategory PropertyConflict**, který bude vždy požadován:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>V omezené množství nebo všechny
1. **Shluku <Int>**  může být použit toolimit hello dotazu tooa konkrétní počet hodnot.
2. **Všechny** lze použít tooensure jsou načteny všechny výsledky v případě hello, zda existuje velký počet chyb.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Portál správy Office 365
Chyby synchronizace adresáře můžete zobrazit v Centru pro správu Office 365 hello. Hello sestavy v portálu pouze zobrazí hello Office 365 **uživatele** objekty, které mají tyto chyby. Nezobrazuje informace o konflikty mezi **skupiny** a **kontakty**.

![Aktivní uživatelé](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "aktivní uživatelé")

Pokyny jak tooview chyby synchronizace adresáře v Office 365 Dobrý den, správce center, najdete v části [identifikovat chyby synchronizace adresáře v Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).

### <a name="identity-synchronization-error-report"></a>Sestava chyby synchronizace identit
Pokud je objekt s duplicitní atribut konflikt s toto nové chování, které je součástí oznámení se odesílají hello standardní sestavy chyby synchronizace identit e-mailu, toohello technické oznámení kontaktu pro klienta hello. Však dojde důležité změně toto chování. V posledních hello informace o konflikt duplicitní atribut by být součástí každých dalších chybách až po vyřešení konfliktu hello. S toto nové chování hello chyba oznámení pro danou konflikt vyskytovat pouze jednou - ve hello dobu, kdy hello konfliktní atributu je v karanténě.

Tady je příklad jaké hello e-mailové oznámení pro konflikt ProxyAddress vypadá:  
    ![Aktivní uživatelé](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "aktivní uživatelé")  

## <a name="resolving-conflicts"></a>Řešení konfliktů
Řešení potíží s svoji strategii a řešení pro tyto chyby by neměl lišit od hello způsob duplicitní atribut chyby byly zpracovány v posledních hello. Hello jediným rozdílem je, že změny úlohy časovače hello prostřednictvím klienta hello na straně služby tooautomatically hello přidání atributu hello v objektu správné otázky toohello po vyřešení konfliktu hello.

Hello následující článek popisuje různé strategie pro odstraňování potíží a řešení: [duplicitní nebo neplatné atributy zabránit synchronizaci adresářů v Office 365](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Známé problémy
Žádná z těchto známých problémů způsobí snížení dat výpadku nebo služby. Několik z nich jsou estetické, ostatní způsobit standard "*před odolnosti*" toobe chyby duplicitní atribut vyvolána místo umístění do karantény hello konflikt atribut a druhý způsobí, že některé chyby toorequire velmi Ruční oprava up.

**Základní chování:**

1. Objektů s určitým atributem konfigurace pokračovat tooreceive export chyby jako názvem na rozdíl od toohello duplicitní atributy v karanténě.  
   Například:
   
    a. Po vytvoření nového uživatele ve službě AD s název UPN  **Joe@contoso.com**  a ProxyAddress**smtp:Joe@contoso.com**
   
    b. Hello vlastnosti tohoto objektu v konfliktu s existující skupiny, kde je ProxyAddress  **SMTP:Joe@contoso.com** .
   
    c. Při exportu **ProxyAddress konflikt** místo nutnosti hello konflikt atributy v karanténě, je vržena chyba. operace Hello je opakovat při každém cyklu následná synchronizace, jako by byl před povolením funkce hello odolnost proti chybám.
2. Pokud se vytvoří dvě skupiny místní s hello stejná adresa SMTP, jeden selže tooprovision na první pokus o hello s duplicitní standard **ProxyAddress** chyba. Ale duplicitní hodnota hello správně v karanténě po hello příštím synchronizačním cyklu.

**Sestavy portálu Office**:

1. Hello Podrobná chybová zpráva pro dva objekty v sadě konflikt UPN je hello stejné. To znamená, že budou obě neměla jejich UPN změnit / do karantény, když ve skutečnosti jenom jeden z nich měl žádná data změnit.
2. Hello Podrobná chybová zpráva pro konflikt UPN ukazuje hello nesprávný displayName pro uživatele, kteří byli jejich UPN, změnit nebo v karanténě. Například:
   
    a. **Uživatel A** synchronizuje si první s **UPN = User@contoso.com** .
   
    b. **Uživatel B** je pokus o toobe synchronizovaný až další s **UPN = User@contoso.com** .
   
    c. **Uživatele B** UPN mění příliš **User1234@contoso.onmicrosoft.com**  a  **User@contoso.com**  je přidána příliš**DirSyncProvisioningErrors**.
   
    d. Hello chybovou zprávu pro **uživatel B** by měl označuje, že **uživatel A** již  **User@contoso.com**  podle názvu UPN, ale zobrazuje **uživatel B** vlastní displayName.

**Sestava chyby synchronizace identit**:

odkaz Hello *postup tooresolve tento problém* je nesprávný:  
    ![Aktivní uživatelé](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "aktivní uživatelé")  

By měl příliš bodu[https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).

## <a name="see-also"></a>Viz také
* [Synchronizace služby Azure AD Connect](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
* [Identifikovat chyby synchronizace adresáře v Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)

