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
ms.openlocfilehash: 0fa05ee6a2df13845024e770a82f50ab7f75bafd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Samoobslužné služby v Azure AD podrobné informace pro vytvoření nového hesla

Jak funguje SSPR? Co tato možnost znamená v rozhraní? Materiály pro další informace o hesla pomocí samoobslužné služby Azure AD resetovat.

## <a name="how-does-the-password-reset-portal-work"></a>Jak resetování hesla portálu pracovní

Když uživatel přejde na portál resetovat heslo, pracovní postup je spuštěna Chcete-li zjistit:

   * Jak by měly být lokalizovány stránky?
   * Je platný uživatelský účet?
   * Jaké organizace uživatel nepatří do?
   * Kde je spravované heslo uživatele?
   * Je daný uživatel licenci pro použití funkce?


Přečtěte si další informace o logiku za stránku pro reset hesla níže uvedených kroků.

1. Uživatel klikne na nelze přístup k vašemu účtu odkaz nebo přejde přímo na [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2. Založené na prohlížeči národní prostředí vykresluje v příslušné jazykové možnosti. Prostředí resetování hesla je lokalizované do stejné jazyky jako podporuje Office 365.
3. Uživatel zadá id uživatele a předá test captcha.
4. Azure AD ověří, zda je uživatel moci pomocí této funkce následujícím způsobem:
   * Kontroluje, zda uživatel má povolení této funkce a přiřazenou licenci Azure AD.
     * Pokud uživatel nemá povolení této funkce nebo přiřazenou licenci, je uživatel požádán o obrátit na správce svého resetovat heslo.
   * Kontroluje, zda má uživatel práva challenge dat definovaných na svůj účet v souladu se zásadami správce.
     * Pokud zásady vyžaduje pouze jednu výzvu, pak je je zajistit, že uživatel má příslušná data definované pro minimálně jeden z problémů ve zásad správce povolené.
       * Pokud uživatel není nakonfigurováno, pak uživatel by měl obrátit na správce svého resetovat heslo.
     * Pokud zásady vyžaduje dva problémy, pak je je zajistit, že uživatel má příslušná data definované pro alespoň dvě z problémů ve zásad správce povolené.
       * Pokud uživatel není nakonfigurován, je jsme uživatele doporučujeme na obrátit na správce svého resetovat heslo.
   * Kontroluje, pokud je pro uživatele heslo spravovaný místně (federovaný nebo hodnoty hash hesla synchronizovat).
     * Pokud je nasazený zpětný zápis a heslo uživatele je spravovaný místně, uživatel je moci pokračovat k ověření a obnovit své heslo.
     * Pokud není nasazena zpětný zápis a heslo uživatele je spravovaný místně, je uživatel požádán o obrátit na správce svého resetovat heslo.
5. Pokud je zjištěno, že je uživatel moct úspěšně obnovit své heslo, pak může uživatel v průvodci procesem resetování.

## <a name="authentication-methods"></a>Metody ověřování

Pokud je povolená funkce samoobslužného resetování hesla (SSPR), musíte vybrat alespoň jeden z následujících možností pro metody ověřování. Důrazně doporučujeme vybrat alespoň dvě metody ověřování, tak, aby vaši uživatelé větší flexibilitu.

* E-mail
* Mobilní telefon
* Telefon do kanceláře
* Bezpečnostní otázky

### <a name="what-fields-are-used-in-the-directory-for-authentication-data"></a>Jaké pole se používají v adresáři pro ověřování dat

* Telefon do kanceláře odpovídá telefonní číslo do kanceláře
    * Uživatelé se nemohou tuto možnost nastavíte pole sami musí být definován správcem
* Mobilní telefon odpovídá telefon pro ověření (nejsou viditelné veřejně) nebo mobilní telefon (veřejně viditelný)
    * Služby nejprve hledá telefon pro ověření, pak spadne zpět na mobilní telefon není-li k dispozici
* Alternativní e-mailová adresa odpovídá ověřování e-mailu (nejsou viditelné veřejně) nebo alternativní e-mailu
    * Služby nejprve hledá ověřování e-mailu a pak dojde k navrácení alternativní e-mailu

Ve výchozím nastavení jsou synchronizovány pouze cloudu atributy telefonní číslo do kanceláře a mobilní telefon do cloudového adresáře z vašeho místního adresáře pro data ověřování.

Uživatelé mohou pouze obnovit své heslo, pokud mají data obsažená v metod ověřování, které správce povolil a vyžaduje.

Pokud uživatelé nechcete, aby jejich číslo mobilního telefonu viditelné v adresáři, ale přesto chcete použít pro resetování hesla, Správci by neměl naplnit v adresáři a uživatel by měl naplnit jejich **telefon pro ověření** atribut prostřednictvím [portál pro registraci a resetování hesla](http://aka.ms/ssprsetup). Správci uvidí tyto informace v profilu uživatele, ale nebude publikován jinde. Pokud účet Azure správce zaregistruje jejich číslo telefonu pro ověření, se importují do pole mobilní telefon a je viditelný.

### <a name="number-of-authentication-methods-required"></a>Počet požadované metody ověřování

Tato možnost určuje minimální počet dostupné metody ověření uživatel musí projít resetovat nebo jejich heslo pro odemknutí a můžete nastavit na hodnotu 1 nebo 2.

Uživatelé mohou k poskytování další metody ověřování, pokud jsou povolené správcem.

Pokud uživatel nemá minimální požadované metody zaregistrovaný, zobrazí se jim chybovou stránku, která přesměruje je, aby vyžadovala resetovat heslo správce.

### <a name="how-secure-are-my-security-questions"></a>Do jaké míry jsou mé bezpečnostní otázky

Pokud chcete použít bezpečnostní otázky, doporučujeme, abyste je používán pomocí jiné metody, které se méně bezpečné než jiné metody vzhledem k tomu, že někteří uživatelé mohou znát odpovědi na otázky jiného uživatele.

> [!NOTE] 
> Bezpečnostní otázky jsou uloženy a s lepším zabezpečením na objekt uživatele v adresáři a pouze může být odpovědi uživatele během registrace. Neexistuje žádný způsob pro správce pro čtení nebo úpravy uživatelů otázek a odpovědí.
>

### <a name="security-question-localization"></a>Lokalizace bezpečnostní otázku

Všechny předdefinované otázky, které následují lokalizace do úplnou sadu Office 365 jazyky podle národního prostředí prohlížeče uživatele.

* V jakém městě jste potkali svého manžela nebo manželku (partnera nebo partnerku)?
* V jakém městě se potkali vaši rodiče?
* V jakém městě žije váš nejbližší sourozenec?
* V jakém městě se narodil váš otec?
* V jakém městě jste měli první práci?
* V jakém městě se narodila vaše matka?
* V jakém městě jste byli na Nový rok 2000?
* Jaké je příjmení vašeho oblíbeného učitele ze vysokou * školy?
* Jaký je název univerzity, na kterou jste se hlásili, ale nechodili na ni?
* Jak se jmenuje město, kde jste měli svatební hostinu?
* Jaký je oblíbený sport vašeho otce?
* Jaké je vaše oblíbené jídlo?
* Jaké je jméno a příjmení vaší babičky z matčiny strany?
* Jak se za svobodna jmenovala vaše matka?
* Co je narozeninách měsíci a roce vašeho nejstaršího sourozence? (Příklad: listopad 1985)
* Jaké je křestní jméno vašeho nejstaršího sourozence?
* Jaké je jméno a příjmení vašeho dědečka z otcovy strany?
* Jaké je křestní jméno vašeho nejmladšího sourozence?
* Do jaké školy jste chodili v šesté třídě?
* Jaké měl jméno a příjmení váš nejlepší přítel v dětství?
* Jaké měl jméno a příjmení váš první partner nebo partnerka?
* Jaké bylo příjmení vašeho oblíbeného učitele ze základní školy?
* Jaká byla značka a model vašeho prvního auta nebo motorky?
* Jaký byl název první školy, do které jste chodili?
* Jaký název měla nemocnice, ve které jste se narodili?
* Jak se jmenovala ulice, kde jste v dětství bydleli?
* Jak se jmenoval váš dětský idol?
* Jaké bylo jméno vašeho oblíbeného plyšáka?
* Jak se jmenovalo vaše první domácí zvířátko?
* Jakou jste měli v dětství přezdívku?
* Jaký byl váš oblíbený sport na střední škole?
* Jaká byla vaše první práce?
* Jaké byly poslední čtyři číslice vašeho telefonu v dětství?
* Jaké bylo v dětství vaše vysněné povolání?
* Jakou nejznámější osobnost jste kdy potkali?

### <a name="custom-security-questions"></a>Vlastní bezpečnostní otázky

Vlastní bezpečnostní otázky nejsou lokalizovány pro různá národní prostředí. Všechny vlastní otázky se zobrazují ve stejném jazyce, které jsou zapsány v rozhraní správce i v případě národního prostředí uživatele prohlížeče se liší. Pokud potřebujete lokalizovaná otázky, použijte prosím předdefinované dotazy.

Maximální délka vlastní bezpečnostní otázku je 200 znaků.

### <a name="security-question-requirements"></a>Požadavky na zabezpečení otázku

* Minimální odpovědí limit pro počet znaků je 3 znaky
* Odpověď maximální limit pro počet znaků je 40 znaků
* Uživatelé nemusí odpovězte na stejné otázku více než jednou.
* Uživatelé nemusí poskytovat stejné odpověď na otázku více než jeden
* Všechny znakovou sadu může použít k definování otázky a odpovědi, včetně znaky kódování Unicode
* Počet otázek definovaných musí být větší než nebo rovna počtu otázek vyžadovaných k registraci

## <a name="registration"></a>Registrace

### <a name="require-users-to-register-when-signing-in"></a>Při přihlášení vyžadovat registraci uživatelů

Povolením této možnosti vyžaduje, že uživatel, který je povolen pro resetování hesel k dokončení heslo resetovat registrace, pokud se přihlášení do aplikace pomocí služby Azure AD se přihlásit jako ty, které následují:

* Office 365
* portál Azure
* Přístupový panel
* Federovaným aplikacím
* Vlastních aplikací pomocí služby Azure AD

Zakázáním této funkce stále umožní uživatelům zaregistrovat ručně své kontaktní informace navštivte stránky [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) nebo kliknutím **zaregistrovat pro resetování hesla** v části Karta profil na přístupovém panelu.

> [!NOTE]
> Uživatelé portálu pro registraci resetování hesla můžete zavřít kliknutím na tlačítko Storno nebo zavřením okna ale výzvy pokaždé, když se přihlásit, dokud nebude po dokončení registrace.
>

### <a name="number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Počet dní před vyzváním uživatelů k potvrzení ověřovacích informací

Tato možnost určuje dobu, po mezi nastavení a reconfirming informace o ověřování a je k dispozici, pokud povolíte pouze **vyžadovat, aby registraci při přihlašování uživatelé** možnost.

Platné hodnoty jsou 0-730 dní s 0 znamená nikdy požádejte uživatele o potvrzení jejich informace o ověřování

## <a name="notifications"></a>Oznámení

### <a name="notify-users-on-password-resets"></a>Upozornit uživatele na resetování hesla

Pokud tato možnost nastavená na Ano, uživatele, který je resetování hesla obdrží e-mail s upozorněním, že jejich heslo bylo změněno prostřednictvím portálu SSPR k jejich primární a alternativní e-mailové adresy na souboru ve službě Azure AD. Nikdo jiný po upozornění na resetování této události.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Upozornit všechny správce, když jiní správci resetování hesel

Pokud je tato možnost nastavená na hodnotu Ano, pak **všichni správci** obdrží e-mail na jeho primární e-mailovou adresu v souboru ve službě Azure AD, které jim, že jiný správce došlo ke změně hesla pomocí SSPR.

Příklad: Existují čtyři správci v prostředí. Resetuje heslo pomocí SSPR správce "A". Správci B, C a D dostane e-mail výstrahy je této situaci.

## <a name="on-premises-integration"></a>Místní integrace

Pokud jste nainstalovali, nakonfigurovat a povolili Azure AD Connect bude mít další možnosti pro místní integrace.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Zpětný zápis hesla do místního adresáře

Řídí, zda je povolen zpětný zápis hesla pro tento adresář a pokud zpětného zápisu na, označuje stav zpětného zápisu služby místně. To je užitečné, pokud chcete dočasně zakázat zpětný zápis hesla bez opětovná konfigurace Azure AD Connect.

* Pokud přepínač je nastavena na hodnotu Ano, pak zpětný zápis bude povoleno a federovaných a uživatelé hash synchronizovat hesla bude moct resetovat jejich hesla.
* Pokud přepínač je nastavena na Ne, pak zpětný zápis bude zakázáno a federovaných a heslo hash synchronizované uživatelé nebudou mít k vytvoření nového hesla.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Povolit uživatelům bez resetování hesla pro odemknutí účtů

Označuje, zda uživatelé, kteří navštíví portálu pro resetování hesla by měla mít možnost k odemčení svých místních účtů služby Active Directory bez resetování hesla. Ve výchozím nastavení Azure AD bude vždy odemknutí účtů při resetování hesla, toto nastavení umožňuje oddělit těchto dvou operací. 

* Pokud nastavíte hodnotu "Ano", pak uživatel bude mít možnost obnovit své heslo a odemknout účet, nebo k odemčení bez resetování hesla.
* Pokud nastavena na hodnotu "Ne", pak pouze uživatelé budou moci provést kombinovaný heslo pro obnovení a operaci odemknutí účtu.

## <a name="network-requirements"></a>Síťové požadavky

### <a name="firewall-rules"></a>Pravidla brány firewall

[Seznam adres URL aplikace Microsoft Office a IP adres](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)

Azure AD Connect verze 1.1.443.0 a vyšší, můžete potřebovat HTTPS odchozí přístup k následující
* passwordreset.microsoftonline.com
* servicebus.Windows.NET

Podrobnější přístup, můžete najít aktualizovaný seznam Microsoft Azure Datacenter rozsahy IP adres, se aktualizují každou středu a put platit následující pondělí [zde](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="idle-connection-timeouts"></a>Časové limity nečinnosti připojení

Nástroj Azure AD Connect odešle pravidelné příkazy ping nebo udržování otevřených připojení ke koncovým bodům sběrnice zajistit, že připojení zůstat zachování připojení. Pokud nástroj zjistí, že se ukončuje příliš mnoho připojení, automaticky zvýší frekvenci odesílání příkazu Ping do koncového bodu. Nejnižší vyřazuje 'příkaz ping intervaly' Chcete je 1 ping každých 60 sekund, ale důrazně doporučujeme, aby proxy nebo brány firewall povolit nečinnosti připojení k zachování nejméně 2 – 3 minut. * Pro starší verze doporučujeme čtyři minut nebo déle.

## <a name="active-directory-permissions"></a>Oprávnění služby Active Directory

Účet zadaný v nástroj Azure AD Connect musí mít resetovat heslo, změnit heslo, oprávnění k zápisu na lockoutTime a oprávnění k zápisu na pwdLastSet, rozšířené práv na buď na kořenový objekt **každou doménu** v tom, že doménové struktury **nebo** na uživatele, které chcete být v oboru pro SSPR organizační jednotky.

Pokud si nejste jisti, který účet výše odkazuje, otevřete konfigurace Azure Active Directory Connect uživatelského rozhraní a klikněte na možnost zobrazení aktuální konfigurace. Účet, který je nutné přidat oprávnění je uveden v části "Synchronizovat adresáře"

Nastavení těchto oprávnění umožní účtu služby MA pro každou doménovou strukturu spravovat hesla v zastoupení uživatelských účtů v této doménové struktuře. **Pokud toto přiřazení oprávnění neprovedete, pak i když má být konfigurována správně, zobrazí se zpětný zápis uživatelů dojde k chybám při pokusech o správu svých místních hesel z cloudu.**

> [!NOTE]
> To může trvat až jednu hodinu nebo více těchto oprávnění k replikaci pro všechny objekty ve vašem adresáři.
>

Nastavit příslušná oprávnění pro zpětný zápis hesla hesla

1. Otevřete pomocí účtu, který má oprávnění pro správu příslušné domény Active Directory Users and Computers
2. Z nabídky Zobrazit zkontrolujte, zda že je zapnutý pokročilé funkce
3. V levém panelu klikněte pravým tlačítkem na objekt, který reprezentuje kořen domény a vyberte možnost Vlastnosti
    * Klikněte na kartu zabezpečení
    * Klikněte na tlačítko Upřesnit.
4. Na kartě oprávnění klikněte na tlačítko Přidat
5. Vyberte účet, který oprávnění se používají pro (z instalace Azure AD Connect)
6. V platí pro rozbalení pole vyberte podřízené uživatelské objekty
7. V části oprávnění zaškrtněte políčka pro následující
    * Trvale platné heslo
    * Resetování hesla
    * Změnit heslo
    * Zápis lockoutTime
    * Zápis pwdLastSet
8. Klikněte na tlačítko použít/OK prostřednictvím Pokud chcete použít a ukončete všechna otevřená dialogová.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Jak resetování hesla práce B2B uživatelů?
Resetování hesla a změny jsou plně podporovány s všechny konfigurace B2B.  Přečtěte si níže v tři explicitní případech B2B nepodporuje obnovení hesla.

1. **Uživatelé z partner org s existujícího klienta Azure AD** – Pokud má organizace jsou partnerství se existujícího klienta Azure AD, jsme **respektují aktivují zásady resetování hesel jsou povolené v něm**. Pro heslo resetovat pro práci partnera jenom potřeb organizace a ujistěte se, Azure AD SSPR je povoleno, které je bez dalších poplatků pro zákazníky O365, a lze je povolit pomocí následujících kroků v našem [Začínáme se správou hesel](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords)průvodce.
2. **Uživatelé, kteří zaregistrovali do služby pomocí [samoobslužné registrace](active-directory-self-service-signup.md)**  – Pokud organizace, která jsou partnerství se použité [samoobslužné registrace](active-directory-self-service-signup.md) funkci nahrát do klienta, jsme by jim umožnila resetován s e-mailu, budou registrována.
3. **Uživatelé B2B** – všechny nové uživatele B2B vytvořené pomocí nové [funkce Azure AD s B2B](active-directory-b2b-what-is-azure-ad-b2b.md) bude také moct resetovat vlastní hesla s e-mailu, budou během procesu pozvání zaregistrovány.

Abyste to mohli otestovat, přejděte na http://passwordreset.microsoftonline.com s jedním z těchto uživatelů partnera. Tak dlouho, dokud budou mít alternativní e-mailu nebo e-mailové ověřování definované, resetování hesla funguje podle očekávání.

## <a name="next-steps"></a>Další kroky

Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data**](active-directory-passwords-data.md) – Pochopte požadovaná data a jejich použití pro správu hesel.
* [**Uvedení**](active-directory-passwords-best-practices.md) – Naplánujte a nasaďte pro své uživatele samoobslužné resetování hesla pomocí zde uvedených pokynů.
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
* [**Zpětný zápis hesla**](active-directory-passwords-writeback.md) – Způsob fungování zpětného zápisu hesla v místním adresáři.
* [**Přizpůsobení**](active-directory-passwords-customize.md) – Přizpůsobte vzhled a chování samoobslužného resetování hesla pro vaši společnost.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? – Odpovědi na otázky, na které jste se vždy chtěli zeptat.
* [**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Zjistěte, jak řešit běžné problémy, ke kterým dochází u samoobslužného resetování hesla.

