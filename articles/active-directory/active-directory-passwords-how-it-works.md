---
title: "Nabídnout detailní: Azure AD SSPR | Microsoft Docs"
description: "Podrobné informace pro vytvoření nového hesla samoobslužné služby Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 618c5908-5bf6-4f0d-bf88-5168dfb28a88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: c177192bbe69d179a25d174b06a0813ec28e2615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Samoobslužné služby v Azure AD podrobné informace pro vytvoření nového hesla

Jak funguje SSPR? Co tato možnost znamená v hello rozhraní? Pokračujte ve čtení toofind Další informace o hesla pomocí samoobslužné služby Azure AD resetovat.

## <a name="how-does-hello-password-reset-portal-work"></a>Jak resetování hesla hello portálu pracovní

Když uživatel přejde toohello portálu pro resetování hesel, pracovní postup je spuštěna toodetermine:

   * Jak by měl být lokalizovaný stránku hello?
   * Je platný uživatelský účet hello?
   * Jaké organizace hello uživatel nepatří do?
   * Kde je spravované heslo hello uživatele?
   * Je funkce hello uživatel licencovaný toouse hello?


Pročtěte hello kroků toolearn o hello logiku za stránku pro reset hesla hello.

1. Uživatel klikne na hello nemůže získat přístup k účtu odkaz na vaši nebo přejde přímo příliš[https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2. Na základě na hello prohlížeče národního prostředí hello prostředí je vykreslen v příslušné jazykové hello. Hello prostředí resetování hesla je lokalizované do hello stejné jazyky jako podporuje Office 365.
3. Uživatel zadá id uživatele a předá test captcha.
4. Azure AD ověří, zda uživatel hello je možné toouse tuto funkci pomocí tohoto postupu hello následující:
   * Ověří, zda že má tento uživatel hello povolení této funkce a přiřazenou licenci Azure AD.
     * Pokud uživatel hello nemá povolení této funkce nebo přiřazenou licencí, uživatel hello je výzva toocontact jejich tooreset správce své heslo.
   * Ověří, zda že text hello má hello správné výzvy dat definovaných na svůj účet v souladu se zásadami správce.
     * Pokud zásady vyžaduje pouze jednu výzvu, pak je zajištěno, že tento uživatel hello má příslušná data hello definované pro minimálně jeden hello problémy ve hello zásad správce povolené.
       * Pokud není nakonfigurovaný hello uživatele a pak hello uživatele je doporučené toocontact jejich správce tooreset své heslo.
     * Pokud zásady hello vyžaduje dva problémy, pak je zajištěno, že tento uživatel hello má příslušná data hello definované pro alespoň dvě hello problémy ve hello zásad správce povolené.
       * Pokud není nakonfigurovaný hello uživatele, pak jsme hello uživatele je doporučené toocontact jejich správce tooreset své heslo.
   * Kontroluje, pokud je heslo uživatele hello spravovaný místně (federovaný nebo hodnoty hash hesla synchronizovat).
     * Pokud je nasazená zpětný zápis a heslo uživatele hello je spravovaný místně, hello uživatele je povoleno tooproceed tooauthenticate a obnovit své heslo.
     * Pokud není nasazen zpětný zápis a heslo uživatele hello je spravovaný místně, a uživatel hello výzva toocontact jejich správce tooreset své heslo.
5. Pokud je zjištěno, že tento uživatel hello toosuccessfully obnovit své heslo a potom hello uživatel v průvodci procesem resetování hello.

## <a name="authentication-methods"></a>Metody ověřování

Pokud je povolená funkce samoobslužného resetování hesla (SSPR), musíte vybrat alespoň jeden z hello následující možnosti pro metody ověřování. Důrazně doporučujeme vybrat alespoň dvě metody ověřování, tak, aby vaši uživatelé větší flexibilitu.

* E-mail
* Mobilní telefon
* Telefon do kanceláře
* Bezpečnostní otázky

### <a name="what-fields-are-used-in-hello-directory-for-authentication-data"></a>Jaké pole se používají v adresáři hello ověřování dat

* Telefon do kanceláře odpovídá tooOffice Phone
    * Uživatelé jsou nelze tooset toto pole sami, musí být definován správcem
* Mobilní telefon odpovídá tooeither telefon pro ověření (nejsou viditelné veřejně) nebo mobilní telefon (veřejně viditelný)
    * Hello služby nejprve hledá telefon pro ověření, pak spadne zpět tooMobile Phone není-li k dispozici
* Alternativní e-mailová adresa odpovídá tooeither ověřování e-mailu (nejsou viditelné veřejně) nebo alternativní e-mailu
    * Hello služby nejprve hledá ověřování e-mailu a pak selže back tooAlternate e-mailu

Ve výchozím nastavení pouze atributy cloudu hello, telefonní číslo do kanceláře a mobilních telefonních jsou synchronizovány tooyour cloudového adresáře z místního adresáře pro data ověřování.

Uživatelé mohou pouze resetování hesla Pokud mají data obsažená v hello metody ověřování, že správce hello má povoleno a vyžaduje.

Pokud uživatel nechcete, aby se jejich mobilní telefonní číslo toobe viditelné v adresáři hello by stále jako toouse ho pro resetování hesla, ale Správci by neměl naplnit v adresáři hello a hello uživatele by měly naplnit jejich **telefon pro ověření**  atribut prostřednictvím hello [portál pro registraci a resetování hesla](http://aka.ms/ssprsetup). Správci uvidí tyto informace v profilu uživatele hello ale nebude publikován jinde. Pokud účet Azure správce zaregistruje jejich číslo telefonu pro ověření, se importují do pole hello mobilní telefon a je viditelný.

### <a name="number-of-authentication-methods-required"></a>Počet požadované metody ověřování

Tato možnost určuje minimální počet hello dostupné metody ověření uživatel musí projít tooreset hello nebo jejich heslo pro odemknutí a lze nastavit tooeither 1 nebo 2.

Uživatelé mohou toosupply další metody ověřování, pokud jsou povolené správcem hello.

Pokud uživatel nemá hello minimální požadované metody zaregistrovaný, zobrazí se jim chybová stránka, která by je odkazovalo toorequest tooreset správce své heslo.

### <a name="how-secure-are-my-security-questions"></a>Do jaké míry jsou mé bezpečnostní otázky

Pokud chcete použít bezpečnostní otázky, doporučujeme, abyste je používán pomocí jiné metody, které se méně bezpečné než jiné metody vzhledem k tomu, že někteří uživatelé mohou znát odpovědi hello tooanother uživatelé otázky.

> [!NOTE] 
> Bezpečnostní otázky jsou uloženy a s lepším zabezpečením na objekt uživatele v adresáři hello a pouze může být odpovědi uživatele během registrace. Neexistuje žádný způsob pro správce tooread nebo úprava uživatelů otázek a odpovědí.
>

### <a name="security-question-localization"></a>Lokalizace bezpečnostní otázku

Všechny předdefinované otázky, které následují, jsou lokalizovány do hello úplnou sadu Office 365 jazyky podle národního prostředí prohlížeče hello uživatele.

* V jakém městě jste potkali svého manžela nebo manželku (partnera nebo partnerku)?
* V jakém městě se potkali vaši rodiče?
* V jakém městě žije váš nejbližší sourozenec?
* V jakém městě se narodil váš otec?
* V jakém městě jste měli první práci?
* V jakém městě se narodila vaše matka?
* V jakém městě jste byli na Nový rok 2000?
* Jaké je příjmení vašeho oblíbeného učitele ze vysokou hello * školy?
* Co je název univerzity, na kterou můžete použít hello toobut nechodili na ni?
* Jaké je jméno hello hello místě, kde jste měli první svatební hostinu?
* Jaký je oblíbený sport vašeho otce?
* Jaké je vaše oblíbené jídlo?
* Jaké je jméno a příjmení vaší babičky z matčiny strany?
* Jak se za svobodna jmenovala vaše matka?
* Co je narozeninách měsíci a roce vašeho nejstaršího sourozence? (Příklad: listopad 1985)
* Jaké je křestní jméno vašeho nejstaršího sourozence?
* Jaké je jméno a příjmení vašeho dědečka z otcovy strany?
* Jaké je křestní jméno vašeho nejmladšího sourozence?
* Do jaké školy jste chodili v šesté třídě?
* Co nejdříve byl hello a příjmení váš nejlepší přítel v dětství?
* Co nejdříve byl hello a příjmení váš první partner nebo partnerka?
* Jaké bylo příjmení vašeho oblíbeného základní školy učitele hello?
* Jaká byla hello značku a model vašeho prvního auta nebo motorky?
* Jaký byl název hello hello první školy, do které jste chodili?
* Jaký byl název hello hello měla nemocnice, ve kterém jste se narodili?
* Jaký byl název hello hello ulici z dětství bydleli?
* Jaký byl název hello váš hrdina z dětství?
* Jaký byl název hello vašeho oblíbeného plyšáka?
* Jaký byl název hello vaše první domácí zvířátko?
* Jakou jste měli v dětství přezdívku?
* Jaký byl váš oblíbený sport na střední škole?
* Jaká byla vaše první práce?
* Jaké byly hello poslední čtyři číslice vaše telefonní číslo měli v dětství?
* Když jste byli malí, co má toobe vysněné povolání?
* Kdo je hello nejznámější osobnost jste kdy potkali?

### <a name="custom-security-questions"></a>Vlastní bezpečnostní otázky

Vlastní bezpečnostní otázky nejsou lokalizovány pro různá národní prostředí. Všechny vlastní otázky se zobrazují v hello stejný jazyk, které jsou zapsány v rozhraní správce hello i v případě národního prostředí prohlížeče hello uživatele se liší. Pokud potřebujete lokalizovaná otázky, použijte prosím hello předdefinované dotazy.

Maximální délka vlastní bezpečnostní otázku Hello je 200 znaků.

### <a name="security-question-requirements"></a>Požadavky na zabezpečení otázku

* Minimální odpovědí limit pro počet znaků je 3 znaky
* Odpověď maximální limit pro počet znaků je 40 znaků
* Uživatelé nemusí zodpovědět hello stejné otázka více než jednou.
* Uživatelé nemusí poskytovat hello stejné toomore než jednu otázku odpovědět
* Všechny znaková sada může být použité toodefine otázky a odpovědi, včetně znaky kódování Unicode
* Hello počet otázek definovaných musí být větší než nebo roven hodnotě toohello počet požadovaných tooregister otázky

## <a name="registration"></a>Registrace

### <a name="require-users-tooregister-when-signing-in"></a>Vyžadovat tooregister uživatele při přihlášení

Povolením této možnosti vyžaduje, že uživatel, který je povolený pro heslo resetovat registraci k resetování hesla toocomplete hello, pokud se přihlášení pomocí služby Azure AD toosign v jako ty, které následují tooapplications:

* Office 365
* portál Azure
* Přístupový panel
* Federovaným aplikacím
* Vlastních aplikací pomocí služby Azure AD

Zakázáním této funkce bude stále povolit registraci toomanually uživatelé své kontaktní informace navštivte stránky [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) nebo kliknutím hello **zaregistrovat pro resetování hesla** odkazu na hello Karta profil v hello přístupového panelu.

> [!NOTE]
> Uživatel kliknutím na tlačítko Storno nebo zavřením hello okno můžete zavřít portál registrace pro resetování hesel hello ale výzvy pokaždé, když se přihlásit, dokud nebude po dokončení registrace.
>

### <a name="number-of-days-before-users-are-asked-tooreconfirm-their-authentication-information"></a>Počet dní, než budou uživatelé vyzváni tooreconfirm své informace o ověřování

Tato možnost určuje dobu hello mezi nastavení a reconfirming informace o ověřování a je k dispozici, pokud povolíte hello pouze **vyžadují tooregister uživatele při přihlášení** možnost.

Platné hodnoty jsou 0-730 dní s 0 znamená nikdy požádejte uživatele tooreconfirm své informace o ověřování

## <a name="notifications"></a>Oznámení

### <a name="notify-users-on-password-resets"></a>Upozornit uživatele na resetování hesla

Pokud je tato možnost nastavená tooyes, hello uživatele, který je resetování hesla obdrží e-mail s upozorněním, že jejich heslo bylo změněno prostřednictvím hello SSPR portálu tootheir primární a alternativní e-mailové adresy na souboru ve službě Azure AD. Nikdo jiný po upozornění na resetování této události.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Upozornit všechny správce, když jiní správci resetování hesel

Pokud je tato možnost nastavená tooyes, pak **všichni správci** dostávat e-mailovou tootheir primární e-mailovou adresu v souboru ve službě Azure AD, které jim, že jiný správce došlo ke změně hesla pomocí SSPR.

Příklad: Existují čtyři správci v prostředí. Resetuje heslo pomocí SSPR správce "A". Správci B, C a D dostane e-mail výstrahy je této situaci.

## <a name="on-premises-integration"></a>Místní integrace

Pokud jste nainstalovali, nakonfigurovat a povolili Azure AD Connect bude mít další možnosti pro místní integrace.

### <a name="write-back-passwords-tooyour-on-premises-directory"></a>Zpětný zápis hesla tooyour místní adresář

Řídí, zda je povolen zpětný zápis hesla pro tento adresář a pokud zpětného zápisu na, označuje stav hello služby hello místní zpětný zápis. To je užitečné, pokud chcete tootemporarily zpětný zápis hesla hello zakázat bez opětovná konfigurace Azure AD Connect.

* Pokud hello přepínač je sadu tooyes zpětný zápis bude povoleno a federovaných a heslo hash synchronizované uživatelé budou moct tooreset jejich hesla.
* Pokud přepínač hello je sadu toono zpětný zápis bude zakázáno a federovaných a heslo hash synchronizované uživatelé nebudou moct tooreset jejich hesla.

### <a name="allow-users-toounlock-accounts-without-resetting-their-password"></a>Povolit uživatelům toounlock účtů bez resetování hesla

Označuje, zda uživatele, kteří navštěvují portál pro resetování hesel hello by měla být toounlock možnost dané hello svojí místní službě Active Directory účtů bez resetování hesla. Ve výchozím nastavení, Azure AD bude vždy odemknutí účtů při resetování hesla, toto nastavení umožňuje tooseparate těchto dvou operací. 

* Pokud nastavení příliš "Ano", uživatelé budou dané hello možnost tooreset své heslo a odemknout účet hello nebo toounlock bez resetování hesla hello.
* Pokud nastavení příliš "Ne", pak pouze uživatelé budou moct tooperform účtu a resetování hesla kombinované operace odemknutí.

## <a name="network-requirements"></a>Síťové požadavky

### <a name="firewall-rules"></a>Pravidla brány firewall

[Seznam adres URL aplikace Microsoft Office a IP adres](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)

Azure AD Connect verze 1.1.443.0 a vyšší, můžete potřebovat odchozí následující toohello přístup HTTPS
* passwordreset.microsoftonline.com
* servicebus.Windows.NET

Podrobnější přístup, můžete najít hello aktualizovat seznam Microsoft Azure Datacenter rozsahy IP adres, je aktualizovat každou středu a umístí do následující hello vliv pondělí [zde](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="idle-connection-timeouts"></a>Časové limity nečinnosti připojení

Nástroj Hello Azure AD Connect odešle pravidelné příkazy ping nebo udržování otevřených připojení tooServiceBus koncové body tooensure že hello připojení zůstat zachování připojení. By měl hello nástroj zjistit, že jsou se ukončil příliš mnoha připojení, to automaticky zvýší hello frekvence příkazy ping toohello koncového bodu. Hello nejnižší 'příkaz ping intervaly' vyřazuje toois 1 ping každých 60 sekund, ale důrazně doporučujeme, proxy nebo brány firewall umožňovat nečinné připojení toopersist nejméně 2 – 3 minut. * Pro starší verze doporučujeme čtyři minut nebo déle.

## <a name="active-directory-permissions"></a>Oprávnění služby Active Directory

Hello účet zadaný v hello Azure AD Connect nástroj musí mít resetovat heslo, změnit heslo, oprávnění k zápisu na lockoutTime a oprávnění k zápisu na pwdLastSet, rozšířená práva na buď objekt kořenové hello **každou doménu** v této doménové struktuře **nebo** hello uživatele organizační jednotky nechcete toobe v oboru pro SSPR.

Pokud si nejste jisti jaké výše uvedený účet hello odkazuje, otevřete dialogové okno Konfigurace Azure Active Directory Connect hello uživatelského rozhraní a klikněte na možnost hello zobrazení aktuální konfigurace. Hello účet, který že potřebujete toois tooadd oprávnění uvedených v části "Synchronizovat adresáře"

Nastavení těchto oprávnění umožní účtu na služby MA hello pro každou doménovou strukturu toomanage hesla zastoupení uživatelských účtů v dané doménové struktuře. **Pokud neprovedete tooassign tato oprávnění, pak i když zpětný zápis se zobrazí toobe správně nakonfigurovaný, uživatelé dojít k chybám při pokusu o toomanage svých místních hesel z cloudu hello.**

> [!NOTE]
> To může trvat až hodinu tooan nebo více těchto oprávnění tooreplicate tooall objektů v adresáři.
>

tooset až hello příslušná oprávnění pro toooccur zpětný zápis hesla

1. Otevřete pomocí účtu, který má oprávnění pro správu hello příslušné domény Active Directory Users and Computers
2. Z nabídky Zobrazit hello zkontrolujte, zda že je zapnutý pokročilé funkce
3. V levém panelu hello klikněte pravým tlačítkem na objekt hello, který reprezentuje hello kořenové domény hello a vyberte možnost Vlastnosti
    * Klikněte na kartu zabezpečení hello
    * Klikněte na tlačítko Upřesnit.
4. Na kartě hello oprávnění klikněte na tlačítko Přidat
5. Vyberte účet hello, že oprávnění jsou aplikovány příliš (z instalace Azure AD Connect)
6. V toodrop platí hello seznamu vyberte podřízené uživatelské objekty
7. V části oprávnění zaškrtněte políčka hello pro následující hello
    * Trvale platné heslo
    * Resetování hesla
    * Změnit heslo
    * Zápis lockoutTime
    * Zápis pwdLastSet
8. Klikněte na tlačítko použít/OK prostřednictvím tooapply a ukončete všechna otevřená dialogová.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Jak resetování hesla práce B2B uživatelů?
Resetování hesla a změny jsou plně podporovány s všechny konfigurace B2B.  Přečtěte si níže hello tři explicitní B2B případech nepodporuje obnovení hesla.

1. **Uživatelé z partner org s existujícího klienta Azure AD** – Pokud má hello organizace jsou partnerství se existujícího klienta Azure AD, jsme **respektují aktivují zásady resetování hesel jsou povolené v něm**. Pro heslo resetovat toowork, hello partnera organizace jenom potřebám toomake se Azure AD SSPR je povoleno, což je bez dalších poplatků pro zákazníky O365, a lze je povolit pomocí následujících kroků hello v našem [Začínáme se správou hesel ](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) průvodce.
2. **Uživatelé, kteří zaregistrovali do služby pomocí [samoobslužné registrace](active-directory-self-service-signup.md)**  – Pokud organizace hello jsou partnerství se použila hello [samoobslužné registrace](active-directory-self-service-signup.md) funkce tooget do klienta, jsme by jim umožnila resetován s Hello e-mailu, budou zaregistrována.
3. **Uživatelé B2B** – všechny nové uživatele B2B vytvořený nový hello [funkce Azure AD s B2B](active-directory-b2b-what-is-azure-ad-b2b.md) bude také možné tooreset hesla s e-mailu hello se během procesu pozvání hello zaregistrovány.

tootest se přejděte toohttp://passwordreset.microsoftonline.com s jedním z těchto uživatelů partnera. Tak dlouho, dokud budou mít alternativní e-mailu nebo e-mailové ověřování definované, resetování hesla funguje podle očekávání.

## <a name="next-steps"></a>Další kroky

Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel
* [**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
* [**Zpětný zápis hesla**](active-directory-passwords-writeback.md) – Způsob fungování zpětného zápisu hesla v místním adresáři.
* [**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? -Odpovědi tooquestions vždy chtěli tooask
* [**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR

