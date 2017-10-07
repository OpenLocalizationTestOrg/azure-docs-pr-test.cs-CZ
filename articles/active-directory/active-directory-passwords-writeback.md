---
title: "Azure AD SSPR se zpětným zápisem hesel | Microsoft Docs"
description: "Pomocí Azure AD a Azure AD Connect toowriteback hesla tooon místní adresář"
services: active-directory
keywords: "Správa hesel služby Active directory, správou hesel Azure AD samoobslužném resetování hesla služby"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 6a1dd964a51b4f3b5b0be303b722ab6deb4a6110
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="password-writeback-overview"></a>Přehled zpětný zápis hesla

Umožňuje zpětný zápis hesla že můžete hesla toowrite tooconfigure Azure AD zpátky tooyou místní služby Active Directory. Odebere hello nutné tooset nahoru a spravovat složitá místním řešením pro resetování hesla pomocí samoobslužné služby, a poskytuje pohodlný způsob cloudu pro vaše uživatele tooreset svých místních hesel kdekoli budou. Zpětný zápis hesla je součástí [Azure Active Directory Connect](./connect/active-directory-aadconnect.md) , lze povolit a používat aktuální odběratele Premium [edice služby Azure Active Directory](active-directory-editions.md).

Zpětný zápis hesla poskytuje následující funkce hello

* **Nula zpětnou vazbu zpoždění** -zpětný zápis hesla je asynchronní operace. Uživatelé jsou okamžitě upozorněni, pokud své heslo nesplňuje zásady nebo se nepodařilo toobe resetovat nebo změnit z jakéhokoli důvodu.
* **Podporuje resetování hesla pro uživatele, kteří používají službu AD FS nebo jiné technologie federation** -s zpětný zápis hesla, tak dlouho, dokud hello federované uživatelské účty jsou synchronizovány do vašeho klienta Azure AD jsou možné toomanage své místní AD hesla z cloudu hello.
* **Podporuje resetování hesla pro uživatele, kteří používají [synchronizace hodnot hash hesel](./connect/active-directory-aadconnectsync-implement-password-synchronization.md)**  – Pokud služba resetování hesel hello zjistí, že synchronizované uživatelský účet je povolený pro synchronizace hodnot hash hesel, nemůžeme resetovat obou tohoto účtu místní a cloudové heslo současně.
* **Změna hesla z hello podporuje přístup k panelu a Office 365** – když federovaný nebo heslo synchronizováno uživatelé přijde toochange vypršela platnost, nebo jiný vypršela platnost hesla, jsme zápisu těchto prostředí back tooyour místní AD hesla.
* **Podporuje zápis zpět hesla, když správce obnovit je z hello portál Azure** – vždy, když správce obnoví heslo uživatele v hello [portál Azure](https://portal.azure.com), pokud je Federovaná tohoto uživatele nebo heslo synchronizován, vytvoříme hello heslo Dobrý den, správce vybere na vaše místní AD, také. Aktuálně nejsou podporované v hello portálu pro správu Office.
* **Vynucuje místní zásady hesel AD** – Pokud uživatel resetuje heslo, zajišťujeme, že splňuje místní zásady služby Active Directory před potvrzením ho toothat adresáře. To zahrnuje historie, složitost, stáří, filtrů hesla a další omezení heslo, které jste definovali ve vaší místní AD.
* **Nevyžaduje žádná pravidla brány firewall pro příchozí** -zpětný zápis hesla předávání přes Azure Service Bus používá jako základní komunikační kanál, což znamená, že nemáte tooopen žádné příchozí porty v bráně firewall pro tuto funkci toowork.
* **Není podporována pro uživatelské účty, které existují v rámci chráněné skupiny ve službě Active Directory vaší místní** – Další informace o chráněné skupiny, najdete v tématu [chráněné účty a skupiny ve službě Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>Jak funguje zpětný zápis hesla

Při synchronizaci hodnotu hash hesla nebo federované uživatele dodává tooreset nebo změnit své heslo v cloudu hello, dojde k následujícímu hello:

1. Jsme zkontrolujte toosee, jaký typ uživatele hello heslo má.
    * Pokud vidíte hello heslo je spravované místně
        * Jsme zkontrolujte, zda hello je služba zpětný zápis a spuštěna, pokud se jedná, jsme hello uživateli umožnit pokračovat
        * Pokud není služba zpětný zápis hello nahoru, jsme informace hello uživatele, že nyní nelze resetovat svoje heslo
2. V dalším kroku hello uživatele předá hello příslušný ověřovací brány a dosáhne úvodní obrazovka heslo resetovat.
3. Hello uživatel vybere nové heslo a potvrdí ho.
4. Po kliknutí na tlačítko Odeslat, jsou šifrovány heslo jako prostý text hello s symetrický klíč, který jste vytvořili během procesu instalace hello zpětný zápis.
5. Po šifrování hello heslo, budeme její zahrnutí do datové části, která se odešlou přes režimu HTTPS kanál tooyour specifické pro klienta služby bus relay (který také nastavíme pro vás během procesu instalace hello zpětný zápis). Tato předávání je chráněn náhodně generované heslo, které zná pouze místní instalace.
6. Jakmile uvítací zprávu dosáhne služby service bus, hello koncový bod resetování hesla automaticky probudí a uvidí, že má čekající žádost o resetování.
7. Hello služba pak hledá uživatele hello dotyčném pomocí atribut kotvy hello cloudu. Pro tento vyhledávací toosucceed:

    * objekt uživatele Hello musí existovat v hello AD prostoru konektoru
    * objekt uživatele Hello musí být propojená toohello odpovídající objektu MV
    * objekt uživatele Hello musí být propojená toohello odpovídající objekt konektoru AAD.
    * Hello odkaz z tooMV objektu AD konektor musí mít hello synchronizační pravidlo `Microsoft.InfromADUserAccountEnabled.xxx` na odkaz hello. <br> <br>
    Při volání hello se dodává z cloudu hello, používá modul Synchronizace hello hello cloudAnchor atribut toolook až objektu prostoru konektoru AAD hello, postupuje podle objektu MV back toohello odkaz hello a pak následuje hello odkaz back toohello AD objektu. Vzhledem k tomu může být více objektů AD (více doménovými strukturami) pro stejného uživatele, hello synchronizační modul spoléhá na hello hello `Microsoft.InfromADUserAccountEnabled.xxx` odkaz toopick hello opravte jednu.

    > [!Note]
    > V důsledku této logiky Azure AD Connect musí být schopný toocommunicate s hello emulátoru primárního řadiče domény pro toowork zpětný zápis hesla. Pokud potřebujete tooenable tomto ručně, můžete se připojit Azure AD Connect toohello emulátoru primárního řadiče domény kliknutím pravým tlačítkem na hello **vlastnosti** synchronizace konektoru služby Active Directory hello, pak výběrem **konfigurace oddíly adresáře**. Odtud, vyhledejte hello **nastavení připojení řadiče domény** části a hello políčko s názvem **používat jenom řadiče domény upřednostňované**. I v případě hello, že řadič domény není emulátor primárního řadiče domény, Azure AD Connect se pokusí tooconnect toohello primárního řadiče domény pro zpětný zápis hesla.

8. Jakmile hello uživatelský účet nenajde, jsme pokus tooreset hello heslo přímo v doménové struktuře AD odpovídající hello.
9. Pokud hello operace nastavení hesla úspěšné, jsme informace hello uživatele, že jejich heslo bylo změněno.
    > [!NOTE]
    > V případě hello synchronizované tooAzure AD po hello uživatelské heslo pomocí synchronizace hesel, je pravděpodobné, že zásady hesel místní hello je nižší než zásady hesel cloudové hello. V takovém případě jsme stále vynucovat zásady místního ať hello je a místo toho povolit heslo hash synchronizace toosynchronize hello hash toto heslo. To zajistí, že vaše místní zásady se vynucují v cloudu hello, bez ohledu Pokud použijete heslo synchronizace nebo federování tooprovide jednotného přihlašování.

10. Pokud heslo hello nastavit operace selže, jsme vrátit hello při toohello uživatele a dát jim zkuste to znovu.
    * operace Hello může selhat z důvodu následující hello
        * Služba Hello se dolů
        * Hello heslo, které vybral nesplňuje zásady organizace
        * Nemohli jsme najít uživatele hello v hello místní AD

    Máme konkrétní zpráva pro mnohá z těchto případů a informace hello uživatele, co mohou provádět tooresolve hello problém.

## <a name="configuring-password-writeback"></a>Konfigurace zpětný zápis hesla

Doporučujeme použít funkci Automatické aktualizace hello [Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md) Pokud chcete, aby toouse zpětný zápis hesla.

DirSync a Azure AD Sync již nejsou podporované znamená povolení článku hello zpětný zápis hesla [upgradu z nástroje DirSync a Azure AD Sync](connect/active-directory-aadconnect-dirsync-deprecated.md) obsahuje další informace o toohelp s váš přechod.

Hello níže uvedený postup předpokládá jste již nakonfigurovali Azure AD Connect ve vašem prostředí pomocí hello [Express](./connect/active-directory-aadconnect-get-started-express.md) nebo [vlastní](./connect/active-directory-aadconnect-get-started-custom.md) nastavení.

1. tooconfigure a povolit zpětný zápis hesla přihlásit server tooyour Azure AD Connect a spusťte hello **Azure AD Connect** Průvodce konfigurací.
2. Na úvodní obrazovce hello klikněte na tlačítko **konfigurace**.
3. Na hello další úlohy obrazovce klikněte na tlačítko **přizpůsobit možnosti synchronizace** a potom zvolte **Další**.
4. Na obrazovce tooAzure AD připojení hello zadejte přihlašovací údaje globálního správce a vyberte **Další**.
5. Na hello připojení adresáře a doménu a organizační jednotky filtrování obrazovky můžete **Další**.
6. Na obrazovce volitelných funkcí hello políčko hello vedle příliš**zpětný zápis hesla** a klikněte na tlačítko **Další**.
   ![Povolení zpětného zápisu hesla ve službě Azure AD Connect][Writeback]
7. Na obrazovce připraveno tooconfigure hello klikněte na tlačítko **konfigurace** a počkejte toocomplete proces hello.
8. Až se zobrazí konfigurace dokončení můžete kliknout na **ukončení**

## <a name="licensing-requirements-for-password-writeback"></a>Licenční požadavky pro zpětný zápis hesla

Informace týkající se licencování, najdete v tématu hello [licence potřebné pro zpětný zápis hesla](active-directory-passwords-licensing.md#licenses-required-for-password-writeback) nebo hello následující weby

* [Azure Active Directory ceny lokality](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Zabezpečené produktivní Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-supported-for-password-writeback"></a>Místní režimy ověřování, které jsou podporované pro zpětný zápis hesla

Zpětný zápis hesla funguje pro následující typy heslo uživatele hello:

* **Uživatelé cloudové**: zpětný zápis hesla nelze použít v této situaci, protože neexistuje žádné místní heslo
* **Synchronizovat hesla uživatelů**: podporované zpětný zápis hesla
* **Federovaní uživatelé**: podporované zpětný zápis hesla
* **Předávací ověřování uživatelů**: podporované zpětný zápis hesla

### <a name="user-and-admin-operations-supported-for-password-writeback"></a>Operace uživatele a správce podporované pro zpětný zápis hesla

Hesla jsou zapsány zpět ve všech hello následující situace:

* **Podporované operace koncového uživatele**
  * Všechny koncového uživatele samoobslužné služby dobrovolném změnit operace hesla
  * Všechny koncového uživatele samoobslužné služby vynutit změnu hesla operace (například vypršení platnosti hesla)
  * Všechny koncových uživatelů samoobslužného resetování hesel pocházející z hello [portálu pro resetování hesel](https://passwordreset.microsoftonline.com)
* **Podporované operace správce**
  * Všechny dobrovolném samoobslužné služby správce změnit operace hesla
  * Všechny správce samoobslužné služby vynutit změnu hesla operace (například vypršení platnosti hesla)
  * Jakékoli správce samoobslužného resetování hesel pocházející z hello [portálu pro resetování hesel](https://passwordreset.microsoftonline.com)
  * Všechny iniciované správcem koncového uživatele heslo resetovat z hello [portál Azure classic](https://manage.windowsazure.com)
  * Všechny iniciované správcem koncového uživatele heslo resetovat z hello [portálu Azure](https://portal.azure.com)

### <a name="user-and-admin-operations-not-supported-for-password-writeback"></a>Operace uživatele a správce není podporováno pro zpětný zápis hesla

Hesla jsou **není** napsaných v některé z hello následující situace:

* **Nepodporovaná operace koncového uživatele**
  * Všichni koncoví uživatelé resetovat vlastní heslo pomocí prostředí PowerShell v1, v2 nebo hello Azure AD Graph API
* **Nepodporovaná operace správce**
  * Všechny iniciované správcem koncového uživatele heslo resetovat z hello [portálu pro správu Office](https://portal.office.com)
  * Žádné z prostředí PowerShell v1, v2 nebo hello Azure AD Graph API pro vytvoření nového hesla iniciované správcem koncového uživatele

Při pracujeme tooremove tato omezení konkrétní časové osy, kterou jsme můžete sdílet ještě nemáme.

## <a name="password-writeback-security-model"></a>Model zabezpečení zpětný zápis hesla

Zpětný zápis hesla je vysoce zabezpečených služba.  tooensure je chránit vaše informace, jsme povolit model vrstvené 4 zabezpečení, který je popsán níže.

* **Předávání přes service-bus konkrétního klienta**
  * Při nastavování služby hello nastavíme předávání sběrnice služby konkrétního klienta, který je chráněný pomocí náhodně generované silné heslo, které společnost Microsoft nikdy nemá přístup k.
* **Uzamčení dolů, kryptograficky silné heslo šifrovacího klíče**
  * Po vytvoření hello service bus relay vytvoříme silné heslo hello tooencrypt používáme jako pochází přes přenosu hello symetrického klíče. Tento klíč je umístěn pouze v úložišti tajný vaší společnosti v hello cloudu, což je výraznou uzamčené a auditovat, stejně jako jakékoli jiné heslo v adresáři hello.
* **Oborový standard TLS**
  1. Pokud heslo resetovat nebo změnit operace probíhá v cloudu hello, jsme trvat heslo jako prostý text hello a šifrování pomocí veřejného klíče.
  2. Jsme pak umístit, do zprávy protokolu HTTPS, která se posílají přes šifrovaný kanál pomocí společnosti Microsoft SSL certifikátů tooyour service bus relay.
  3. Po uvítací zprávu dorazí do služby Service Bus, vaše místní agent probudí nahoru a ověřuje pomocí sběrnice tooService hello silné heslo, který byl vytvořen dříve.
  4. Místní agent převezme hello šifrované zprávy, dešifruje ji pomocí hello soukromého klíče, který jsme vygenerovat.
  5. Místní agent pokusí tooset hello hesla prostřednictvím hello AD DS SetPassword rozhraní API.
     * Tento krok je co nám umožňují tooenforce služby AD místní zásady hesel (složitost, stáří, historie, filtry atd.) v cloudu hello.
* **Zásady vypršení platnosti zpráv** 
  * Pokud uvítací zprávu nachází v Service Bus, protože vaše místní služba není spuštěna, bude vypršení časového limitu a odeberou po několik na minut tooincrease zabezpečení ještě víc.

### <a name="password-writeback-encryption-details"></a>Podrobnosti o šifrování zpětný zápis hesla

kroky pro šifrování Hello prochází žádost o resetování hesla, až uživatel odešle, než dorazí v místním prostředí, tooensure maximální služby spolehlivost a zabezpečení jsou popsané níže.

* **Krok 1 – heslo šifrování pomocí klíče RSA 2048 bitů** – když uživatel odešle heslo toobe, zapisovat zpátky tooon místní, nejdřív hello odeslaná heslo samotné je šifrován klíčem RSA 2048 bitů.
* **Krok 2 – šifrování na úrovni balíčku s AES-GCM** - pak celý balíček hello (heslo + požadovaná metadata) jsou šifrována pomocí standardu AES-GCM. Každý, kdo má přímý přístup toohello základní sběrnice kanálu zabrání zobrazení/manipulaci s obsahem hello.
* **Krok 3 – veškerá komunikace probíhá přes protokol TLS / SSL** -veškerá komunikace hello se sběrnice probíhá kanál SSL/TLS. To zabezpečuje hello obsah od neoprávněným 3. stran.
* **Automatickou výměnu klíče každých šest měsíců** – automaticky každých 6 měsíců, nebo pokaždé, když zpětný zápis hesla je zakázána a znovu povolena na Azure AD Connect, jsme výměny tyto klíče tooensure maximální služby zabezpečení a zabezpečení.

### <a name="password-writeback-bandwidth-usage"></a>Využití šířky pásma zpětný zápis hesla

Zpětný zápis hesla je služba malou šířkou pásma, která odešle požadavky zpět toohello místní agent pouze za hello následující okolnosti:

1. Dvě zprávy po povolení nebo zakázání funkce hello přes Azure AD Connect.
2. Tak dlouho, dokud se službou hello, je odeslána zpráva jeden každých pět minut jako služba prezenčního signálu.
3. Dvě zprávy, které se odesílají každý čas odeslání nové heslo
    * První zprávu jako operace žádosti tooperform hello
    * Druhou zprávu, která obsahuje hello výsledek operace hello a jsou odeslány hello následující okolnosti:
        * Pokaždé, když nové heslo je odesíláno během resetování hesla pomocí samoobslužné služby uživatele.
        * Pokaždé, když nové heslo je odesíláno během operace změny hesla uživatele.
        * Pokaždé, když nové heslo je odesíláno během spouštěná správce uživatelské heslo resetovat (z hello pouze portálů Azure správce).

#### <a name="message-size-and-bandwidth-considerations"></a>Aspekty zpráva velikost a šířky pásma

Hello velikost jednotlivých zpráv hello popsané výše je obvykle v části 1 kb, i v rámci extrémně zatížení, hello heslo zpětný zápis samotnou službu spotřebovává několik kilobity za sekundu šířky pásma. Vzhledem k tomu, že každá zpráva se odeslat v reálném čase, pouze v případě, že operace aktualizace a heslo a totiž tak malá velikost zprávy hello využití šířky pásma hello hello zpětný zápis funkce efektivně je příliš malá toohave všech skutečných měřitelný dopad.

## <a name="next-steps"></a>Další kroky

Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel
* [**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden
* [**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? -Odpovědi tooquestions vždy chtěli tooask
* [**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR

[Writeback]: ./media/active-directory-passwords-writeback/enablepasswordwriteback.png "Povolení zpětného zápisu hesla ve službě Azure AD Connect"
