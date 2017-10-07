---
title: "Nejčastější dotazy: Azure AD SSPR | Microsoft Docs"
description: "Nejčastější dotazy o hesla pomocí samoobslužné služby Azure AD resetovat"
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
ms.openlocfilehash: d04a9efeb3b35421aa605cadb2aa25f656a4d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="password-management-frequently-asked-questions"></a>Nejčastější dotazy se správou hesel

Hello následující jsou některé nejčastější dotazy pro všechny věci týkající se toopassword resetovat.

Pokud máte obecný dotaz týkající se Azure AD a samoobslužné služby heslo resetovat, která není zde zodpovězena, můžete požádat hello komunity pro pomoc na hello [fóra Azure Ad](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Členové komunity hello zahrnují Engineers, správce produktu, MVP a hlavě nehodí Odborníci v oblasti IT.

Tyto nejčastější dotazy se rozdělí do hello následující části:

* [**Dotazy o registraci k resetování hesla**](#password-reset-registration)
* [**Dotazy o resetování hesla**](#password-reset)
* [**Dotazy týkající se změna hesla**](#password-change)
* [**Sestavy dotazy týkající se správou hesel**](#password-management-reports)
* [**Dotazy týkající se zpětný zápis hesla**](#password-writeback)

## <a name="password-reset-registration"></a>Registrace pro resetování hesla
* **Otázka: je možné mé uživatelé registrovat svá vlastní data resetování hesla?**

  > **Odpověď:** Ano, jak dlouho, dokud je povoleno obnovení hesla a udělení licence, mohou přejít portálu toohello registraci pro resetování hesla na http://aka.ms/ssprsetup tooregister své informace o ověřování. Uživatelé mohou také registrovat pomocí budete toohello přístupového panelu v http://myapps.microsoft.com, klikněte na kartu profil hello a příkaz hello registrace pro resetování hesla možnost.
  >
  >
* **Otázka: je možné definovat data resetování hesel jménem Moje uživatelů?**

  > **Odpověď:** Ano, můžete tak učinit pomocí Azure AD Connect, prostředí PowerShell, hello [portál Azure](https://portal.azure.com), nebo portál pro správu Office hello. Další informace najdete v článku hello [dat používá Azure AD funkce samoobslužného resetování hesla](active-directory-passwords-data.md).
  >
  >
* **Otázka: je možné synchronizovat data pro bezpečnostní otázky z místně?**

  > **Odpověď:** to není možné ještě dnes.
  >
  >
* **Otázka: je možné zaregistrovat vlastní uživatelé dat tak, že další uživatelé nemohou zobrazit tato data?**

  > **Odpověď:** Ano, když uživatelé zaregistrovat dat pomocí hello registrace portálu pro resetování hesel je uložen do polí privátní ověřování, která viditelné pouze globální správci a hello uživatelem.
    >
    > [!NOTE]
    > Pokud **účet správce Azure** zaregistruje jejich číslo telefonu pro ověření ní se importují také do pole hello mobilní telefon a je viditelný.
    >
  >
  >
* **Otázka: Moji uživatelé mají toobe zaregistrován před použitím resetování hesla?**

  > **Odpověď:** Ne, pokud jejich jménem definujete dostatek informací, ověřování, uživatelé nebudou mít tooregister. Tak dlouho, dokud máte správně naformátovaná data uložená v hello odpovídající pole v adresáři hello resetování hesla funguje.
  >
  >
* **Otázka: je možné synchronizovat nebo nastavte pole Telefon pro ověření, ověření e-mailu nebo alternativní telefon pro ověření hello jménem Moje uživatelů?**

  > **Odpověď:** to není možné ještě dnes.
  >
  >
* **Otázka: jak portálu pro registraci hello vědět, jaké možnosti tooshow Moji uživatelé?**

  > **Odpověď:** resetování hesla hello portálu pro registraci jen ukazuje hello možnosti, které jste povolili pro vaše uživatele. Tyto možnosti se nacházejí v rámci hello zásady resetování hesel uživateli část svého adresáře na kartě konfigurace. Například to znamená, že pokud nepovolíte bezpečnostní otázky, pak uživatelé nebudou moct tooregister pro tuto možnost.
  >
  >
* **Otázka: když se považuje za uživatele registrované?**

  > **A:** uživatel se považuje zaregistrovat pro SSPR, když registrace alespoň hello **počet požadovaných tooreset metody** , kterou jste nastavili v hello [portál Azure](https://portal.azure.com).
  >
  >
## <a name="password-reset"></a>Resetování hesla
* **Otázka: jak dlouho má čekat tooreceive e-mailem, SMS nebo telefonní hovor z resetování hesla?**

  > **Odpověď:** e-mailu, zpráv SMS, a telefonní hovory by měl přicházející do v části jedna minuta, 5-20 sekund normální případem hello.
    >Pokud tento časový rámec neobdrží oznámení hello:
        > * Zkontrolujte své složky s nevyžádanou poštou.
        > * Kontrola hello číslo nebo e-mailu kontaktovaný je hello jeden, které očekáváte.
        > * Zkontrolujte, zda je správně naformátován hello ověřování dat v adresáři hello.
                >     * Příklad: "+ 1 4255551234" nebo "user@contoso.com"
  >
  >
* **Otázka: jaké jazyky jsou podporovány resetování hesla?**

  > **Odpověď:** hello resetování hesla uživatelského rozhraní, zpráv SMS a hlasové hovory jsou lokalizované do hello stejné jazyky, které jsou podporovány v Office 365.
  >
  >
* **Otázka: co částí prostředí resetování hesla hello získat značky po nastavení organizační brandingu v adresáře Moje aktivity karta Konfigurace?**

  > **Odpověď:** portálu pro resetování hesel hello zobrazuje logo vaší organizace a umožňuje vám tooconfigure hello obraťte se na správce odkaz toopoint tooa vlastní e-mailu nebo adresa URL. E-mailu, která se odešlou resetování hesla zahrnuje loga vaší organizace, barvy, název v textu hello hello e-mailů a přizpůsobit z názvu.
  >
  >
* **Otázka: jak můžete naučit Moje uživatele o tom, kde toogo tooreset hesla?**

  > **Odpověď:** toohttps://passwordreset.microsoftonline.com vaši uživatelé můžete odeslat přímo, nebo můžete určit, aby je tooclick hello **nemůže získat přístup k účtu odkaz na vaši** nalezen na libovolné pracovní nebo školní přihlašovací stránce. Tyto odkazy můžete také publikovat v místě snadno dostupný tooyour uživatelů.
  >
  >
* **Otázka: je možné pomocí této stránky z mobilního zařízení?**

  > **Odpověď:** Ano, tato stránka funguje na mobilních zařízeních.
  >
  >
* **Otázka: podporujete odemykání účty místní služby active directory při resetování hesel uživatelů?**

  > **Odpověď:** Ano, pokud uživatel může resetovat své heslo a byla nasazena zpětného zápisu hesla pomocí služby Azure AD Connect, tento uživatelský účet je automaticky odemknout se obnovit své heslo.
  >
  >
* **Otázka: jak můžete integrovat přímo do daného uživatele plochy přihlašování resetování hesla?**

  > **Odpověď:** Pokud jste zákazník Azure AD Premium, můžete nainstalovat Microsoft Identity Manager bez dalších poplatků a nasadit hello místní heslo resetovat řešení toomeet tento požadavek.
  >
  >
* **Otázka: je možné nastavit různé bezpečnostní otázky pro různá národní prostředí?**

  > **Odpověď:** to není možné ještě dnes.
  >
  >
* **Otázka: jak tolik otázek jsme nakonfigurovat pro možnost ověření bezpečnostních otázek hello?**

  > **Odpověď:** můžete nakonfigurovat až too20 vlastní bezpečnostních otázek v hello [portál Azure](https://portal.azure.com).
  >
  >
* **Otázka: jak dlouho může bezpečnostní otázky být?**

  > **Odpověď:** bezpečnostní otázky může být 3 až 200 znaků.
  >
  >
* **Otázka: jak dlouho může odpovědi toosecurity otázky být?**

  > **Odpověď:** odpovědi může mít 3 too40 znaků.
  >
  >
* **Otázka: jsou duplicitní odpovědi toosecurity otázky odmítnuta?**

  > **Odpověď:** Ano, jsme odmítnout otázky toosecurity duplicitní odpovědi.
  >
  >
* **Otázka: může uživatel zaregistrovat stejné bezpečnostní otázku hello více než jednou?**

  > **Odpověď:** Ne, jakmile se uživatel zaregistruje na určitou otázku, se nemusí zaregistrovat pro tento dotaz ještě jednou.
  >
  >
* **Otázka: je možné tooset minimální limit bezpečnostní otázky pro registraci a resetování?**

  > **Odpověď:** Ano, lze nastavit jeden limit pro registraci a druhý pro obnovení. bezpečnostní otázky 3 až 5 může být potřeba k registraci a 3 až 5 může být nutný pro obnovení.
  >
  >
* **Otázka: Pokud uživatel má více než maximální počet otázek požadované tooreset hello registrované, jak jsou bezpečnostní otázky vybrané během obnovení?**

  > **Odpověď:** N bezpečnostní otázky jsou náhodně vybírány mimo hello celkový počet otázek a uživatel zaregistroval, kde N je hello **počet otázek požadované tooreset**. Například pokud má uživatel zaregistrován 5 bezpečnostní otázky, ale jenom 3 jsou požadované tooreset, 3 hello 5 jsou vybrán náhodně a uvedené v resetování. Pokud hello uživatel získá hello odpovědi toohello otázky nesprávný, procesu výběru hello vyskytovat i nadále ražením tooprevent otázku.
  >
  >
* **Otázka: můžete zabránit uživatelům v pokusu o mnohokrát v krátkého časového období pro vytvoření nového hesla?**

  > **Odpověď:** Ano, jsou součástí tooprotect resetování hesla před zneužitím funkce zabezpečení. Uživatelé mohou zkuste jenom 5 resetování pokusů o zadání hesla v rámci jednu hodinu před uzamknutí 24 hodin. Uživatelé pouze mohou zkuste toovalidate telefonní číslo 5krát v rámci jednu hodinu před uzamknutí 24 hodin. Uživatelé mohou pouze pokusí metoda ověření jednotného 5krát v rámci jednu hodinu před uzamknutí 24 hodin.
  >
  >
* **Otázka: jak dlouho měly hello e-mailu a SMS jednorázové heslo platné?**

  > **Odpověď:** hello dobu platnosti relace pro resetování hesla je 105 minut. Od začátku hello hello resetování hesla operace, má uživatel hello 105 minut tooreset své heslo. Hello e-mailu a SMS jednorázové heslo jsou neplatné po vypršení tohoto časového období.
  >
  >

## <a name="password-change"></a>Změna hesla
* **Otázka: kde by měl mé uživatelé toochange hesla?**

  > **A:** uživatelé mohou změnit své heslo kdekoli uvidí jejich profilový obrázek nebo ikonu (stejně jako v nástroji hello pravého horního rohu jejich [Office 365](https://portal.office.com) nebo [přístupový Panel](https://myapps.microsoft.com) dojde. Uživatelé mohou změnit své heslo z hello [stránka přístupového panelu profil](https://account.activedirectory.windowsazure.com/r#/profile). Uživatelé mohou být také vyzváni toochange hesla na přihlašovací obrazovce hello Azure AD automaticky, pokud vypršela platnost hesla. Nakonec mohou uživatelé přejdou toohello [portál změn hesel služby Azure AD](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) přímo Pokud si přejí toochange jejich hesla.
  >
  >
* **Otázka: je možné se svým uživatelům a upozornění v hello portál Office, když vyprší platnost hesla pro místní?**

  > **Odpověď:** to je možné ještě dnes Pokud používáte služby AD FS podle pokynů hello zde: [odesílání deklarace zásady hesel se službou AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396). Pokud používáte synchronizaci hodnoty hash hesla, to není možné ještě dnes. Důvodem je, že jsme není synchronizována zásady hesel z místní, takže není možné, že nám toopost vypršení platnosti oznámení toocloud dojde. V obou případech je také možné příliš[upozorněte uživatele, jejichž hesla se o tooexpire pomocí prostředí PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >

## <a name="password-management-reports"></a>Sestavy správy hesel
* **Otázka: jak dlouho trvá pro data tooshow až na sestavy správy hello heslo?**

  > **Odpověď:** Data by se zobrazit na sestavy správy hello heslo v rámci 5 až 10 minut. Ho některých případech to může trvat až hodinu tooappear tooan.
  >
  >
* **Otázka: jak můžete filtrovat sestavy správy hello heslo?**

  > **Odpověď:** hello heslo správy sestavy můžete filtrovat kliknutím hello lupy malé toohello extrémně napravo od hello popisky sloupců, v horní hello hello sestavy. Pokud chcete toodo bohatší filtrování, můžete stáhnout tooexcel hello sestavy a vytvoření kontingenční tabulky.
  >
  >
* **Otázka: co je hello maximální počet událostí, které jsou uloženy v sestavy správy hello heslo?**

  > **Odpověď:** až too75, 000 heslo resetovat heslo resetovat registrace události nebo ukládají do sestavy správy hello heslo pokrývání uzlů zálohování too30 dnů.  Pracujeme tooexpand toto číslo tooinclude další události.
  >
  >
* **Otázka: jak daleko zpět sestav správy hesel hello přejděte?**

  > **Odpověď:** Správa hesel hello sestavy zobrazit operací, které se provádí v rámci hello posledních 30 dnů. Teď Pokud potřebujete tooarchive tato data můžete stáhnout hello sestavy pravidelně a uložit je do samostatných umístění.
  >
  >
* **Otázka: je maximální počet řádků, které se mohou objevit v sestavy správy hello heslo?**

  > **Odpověď:** Ano, může se zobrazit nesmí být delší než 75 000 řádků na buď hello sestav správy hesel, jestli jsou uvedené v hello uživatelského rozhraní nebo stahování.
  >
  >
* **Otázka: je k dispozici rozhraní API tooaccess hello heslo resetovat nebo registrační data pro generování sestav?**

  > **Odpověď:** Ano, najdete v části hello následující dokumentaci toolearn přístupu hello heslo resetovat vytváření sestav datového proudu.  [Zjistěte, jak tooaccess resetování hesla události vytváření sestav prostřednictvím kódu programu](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Zpětný zápis hesla
* **Otázka: jak zpětný zápis hesla funguje pozadí hello?**

  > **Odpověď:** najdete v části [jak zpětný zápis hesla funguje](active-directory-passwords-writeback.md) pro vysvětlení, co se stane, když zapnete zpětný zápis hesla a jak se data proudí prostřednictvím systému hello zpět do místního prostředí.
  >
  >
* **Otázka: jak dlouho trvá zpětný zápis hesla toowork?  Je k dispozici ke zpoždění synchronizace jako se synchronizací hodnoty hash hesla?**

  > **Odpověď:** zpětný zápis hesla je rychlé. Je synchronní kanál, který funguje zásadně jinak než synchronizaci hodnoty hash hesla. Zpětný zápis hesla umožňuje uživatelům tooget v reálném čase názor hello úspěch své heslo resetovat nebo změnit operaci. Průměrná doba Hello pro úspěšné zpětný zápis hesla je v části 500 ms.
  >
  >
* **Otázka: Pokud Můj účet místní je, jak je můj účet nebo přístup do cloudu zneužitím?**

  > **Odpověď:** Pokud vaše místní ID je zakázaná, cloudu ID nebo přístup budou rovněž zakázány na další interval synchronizace hello prostřednictvím AAD Connect výchozí byt je to každých 30 minut.
  >
  >
* **Otázka: Pokud je omezené Můj účet místní zásady hesel místní služby Active Directory, SSPR orientují tuto zásadu po změně hesla hello?**

  > **Odpověď:** Ano, spoléhá na SSPR a dodržuje hello místní zásady hesel služby AD, včetně typické domény zásady hesel služby AD, a také všechny definované zásady podrobné heslo cílové tooa zadaný uživatel.
  >
  >
* **Otázka: jaký typy účtů zpětný zápis hesla funguje pro?**

  > **Odpověď:** zpětný zápis hesla funguje pro federovaný a synchronizuje hodnoty Hash hesla uživatele.
  >
  >
* **Otázka: zpětný zápis hesla vynutit zásady hesel Moje doména?**

  > **Odpověď:** Ano, zpětný zápis hesla vynucuje stáří hesla, historie, složitost, filtrů a další omezení, můžete mohou zavést na hesla v místní doméně.
  >
  >
* **Otázka: je zpětný zápis hesla zabezpečení?  Jak se může být jistí, že nebude získat hacker I?**

  > **Odpověď:** Ano, jsou zabezpečené zpětný zápis hesla. tooread Další informace o hello čtyři vrstvy zabezpečení implementované hello služba zpětný zápis hesel, podívejte se na hello [model zabezpečení zpětný zápis hesla](active-directory-passwords-writeback.md#password-writeback-security-model) část v tom, jak zpětný zápis hesla funguje.
  >
  >

## <a name="next-steps"></a>Další kroky

Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Správa licencí**](active-directory-passwords-licensing.md) – Konfigurujte licencování Azure AD.
* [**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel
* [**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden
* [**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Zásady**](active-directory-passwords-policy.md) – Pochopte a nastavte zásady hesel Azure AD.
* [**Zpětný zápis hesla**](active-directory-passwords-writeback.md) – Způsob fungování zpětného zápisu hesla v místním adresáři.
* [**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje
* [**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR
