---
title: "aaaAzure nejčastější dotazy týkající se vícefaktorového ověřování | Microsoft Docs"
description: "Časté otázky a odpovědi související s tooAzure služby Multi-Factor Authentication. Služba Multi-Factor Authentication je metoda ověření identity uživatele, který vyžaduje více než uživatelské jméno a heslo. Poskytuje další vrstvu zabezpečení toouser přihlášení a transakce."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 50bb8ac3-5559-4d8b-a96a-799a74978b14
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c8305cf4c41bf8e9802df192fecdb7e463eff9eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-multi-factor-authentication"></a>Časté otázky k Azure Multi-Factor Authentication
Tyto nejčastější dotazy odpovídá na běžné otázky o Azure Multi-Factor Authentication a použití služby Multi-Factor Authentication hello. Ho je rozdělena do dotazy týkající se služby hello obecně fakturace modely, koncových uživatelů a řešení potíží.

## <a name="general"></a>Obecné
**Otázka: jak Azure Multi-Factor Authentication Server zpracovávat uživatelská data?**

Pomocí aplikace Multi-Factor Authentication Server jsou uložená data uživatele pouze na místní servery hello. Žádná trvalá data se ukládají v cloudu hello. Když uživatel hello provede dvoustupňové ověření, Multi-Factor Authentication Server odešle data toohello cloudové služby Azure Multi-Factor Authentication pro ověřování. Komunikace mezi serverem služby Multi-Factor Authentication a hello cloudové služby Multi-Factor Authentication využívá Secure Sockets Layer (SSL) nebo zabezpečení TLS (Transport Layer) přes port 443 odchozí.

Pokud požadavky na ověření pošlou toohello Cloudová služba, data se shromažďují pro ověřování a použití sestav. Datová pole, které jsou součástí dvoustupňové ověření protokoly jsou následující:

* **Jedinečné ID** (buď název nebo místní služby Multi-Factor Authentication Server ID uživatele)
* **První a poslední název** (volitelné)
* **E-mailová adresa** (volitelné)
* **Telefonní číslo** (při použití telefonní hovor nebo SMS ověřování)
* **Token zařízení** (při použití ověřování mobilní aplikací)
* **Režim ověřování**
* **Výsledek ověřování**
* **Název serveru služby Multi-Factor Authentication**
* **Server Multi-Factor Authentication IP**
* **Klient IP** (Pokud je k dispozici)

volitelná pole Hello se dá nakonfigurovat v aplikaci Multi-Factor Authentication Server.

Hello výsledek ověření (úspěch nebo odmítnutí) a hello důvod pokud mu byl odepřen, se uloží s daty hello ověřování. Tato data jsou k dispozici na ověřování a v sestavách využití.

## <a name="billing"></a>Fakturace
Většina máte dotazy k fakturaci můžete odpovědi tím, že odkazuje tooeither hello [stránce s cenami vícefaktorového ověřování](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) nebo hello dokumentaci o [jak tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md).

**Otázka: je Moje organizace účtovat pro posílání telefonních hovorů hello a textové zprávy, které se používají pro ověřování?**

Ne, se vám neúčtují poplatky pro jednotlivé telefonní hovory umístit nebo textové zprávy odeslané toousers prostřednictvím Azure Multi-Factor Authentication. Pokud používáte poskytovatele MFA podle ověření, se účtují pro každé ověření, ale ne pro metodu hello.

Vaši uživatelé možná budou účtovat pro telefonní hovory hello nebo textové zprávy, které obdrží, podle tootheir osobní telefonní služby.

**Otázka: model fakturace na uživatele hello účtují mi pro všechny povolené uživatele nebo jenom hello ty, které provádí dvoustupňové ověřování?**

Fakturace je založena na hello počet uživatelé nakonfigurovaní toouse ověřování Multi-Factor Authentication, bez ohledu na to, jestli se provedlo dvoustupňové ověření daného měsíce.

**Otázka: jak fakturace služby Multi-Factor Authentication funguje?**

Při vytváření poskytovatele MFA podle uživatele nebo podle ověření předplatného Azure vaší organizace se fakturuje měsíčně na základě využití. Tento model fakturace je podobný bills toohow Azure pro využití virtuálních počítačů a weby.

Při zakoupení předplatného pro Azure Multi-Factor Authentication, vaše organizace jenom platí hello roční poplatky licence pro každého uživatele. Tímto způsobem se účtují licence MFA a Office 365, Azure AD Premium nebo Enterprise Mobility + Security sady. 

Další informace o dostupných možnostech najdete v [jak tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md).

**Otázka: je bezplatnou verzi Azure Multi-Factor Authentication?**

V některých případech Ano.

Služba Multi-Factor Authentication pro správce Azure nabízí podmnožinu funkcí Azure MFA bez nákladů pro přístup k tooMicrosoft online služeb, včetně správce portálů hello Azure a Office 365. Tato nabídka platí jenom správci tooglobal instancí Azure Active Directory, které nemají hello plnou verzi Azure MFA prostřednictvím licenci MFA, sady nebo samostatné zprostředkovatele na základě spotřeby. Pokud správci použít hello bezplatnou verzi, a poté zakoupit plnou verzi Azure MFA, všichni globální správci se zvýšenými oprávněními toohello placené verze automaticky.

Víceúrovňové ověřování pro uživatele služeb Office 365 nabízí podmnožinu funkcí Azure MFA bez nákladů pro přístup k tooOffice 365 služby, včetně Exchange Online a SharePoint Online. Tato nabídka platí toousers, kteří mají licenci Office 365, který je přiřazen, když hello odpovídající instance služby Azure Active Directory nemá hello plnou verzi Azure MFA prostřednictvím licenci MFA, sady nebo samostatné zprostředkovatele na základě spotřeby.

**Otázka: je možné mezi jednotlivé uživatele nebo podle ověření spotřeba modely fakturace kdykoli přepnout Moje organizace?**

Pokud vaše organizace zakoupí MFA jako samostatné služby s fakturace na základě spotřeby, zvolíte Fakturační model, při vytváření poskytovatele MFA. Hello fakturační model nelze změnit po vytvoření poskytovatele MFA. Můžete však odstranit hello zprostředkovatele vícefaktorového ověřování a pak vytvořte jiný model fakturace.

Při vytvoření poskytovatele MFA, může být tooan propojené služby Azure Active Directory (neboli "klienta Azure AD"). Pokud hello aktuálního zprostředkovatele vícefaktorového ověřování klienta tooan propojené služby Azure AD, můžete bezpečně odstranit hello zprostředkovatele vícefaktorového ověřování a vytvoření jednoho klienta propojené toohello stejnou službou Azure AD. Případně pokud jste zakoupili dostatek MFA, Azure AD Premium nebo Enterprise Mobility + Security (EMS) licence toocover všechny uživatele, kteří jsou povolené pro MFA, můžete odstranit zprostředkovatele vícefaktorového ověřování hello úplně.

Pokud je zprostředkovatel vícefaktorového ověřování *není* klienta tooan propojené služby Azure AD, nebo můžete propojit hello nového MFA zprostředkovatele tooa jiné službě Azure AD klienta, nejsou přenést uživatelská nastavení a možnosti konfigurace. Navíc existující servery Azure MFA potřebovat toobe znovu aktivovat pomocí přihlašovacích údajů pro aktivaci vygenerované pomocí hello nového poskytovatele MFA. Opětovná aktivace toolink MFA servery hello je toohello nového poskytovatele MFA neovlivní telefonního hovoru a textové zprávy ověřování, ale oznámení zastaví pro všechny uživatele, dokud se znovu aktivovat mobilní aplikaci hello mobilní aplikace.

Další informace o poskytovatelích MFA v [Začínáme s poskytovatele Azure Multi-Factor Auth](multi-factor-authentication-get-started-auth-provider.md).

**Otázka: je možné Moje organizace přepínat mezi fakturace a předplatných (na základě licencí modelu) na základě spotřeby kdykoli?**

V některých případech Ano.

Pokud má adresáře *uživatelská* poskytovatele ověřování Azure Multi-Factor Authentication, můžete přidat licence MFA. Uživatelé s licencí nezapočítávají v hello na uživatele na základě spotřeby fakturace. Uživatelé bez licence můžete stále povolené pro MFA prostřednictvím poskytovatele MFA hello. Pokud chcete zakoupit a přiřadit licence pro všechny uživatele nakonfigurován toouse služby Multi-Factor Authentication, můžete odstranit hello poskytovatele ověřování Azure Multi-Factor Authentication. Pokud máte více uživatelů než licencí v hello budoucí můžete kdykoli vytvořit jiného poskytovatele MFA podle uživatele.

Pokud má adresáře *za ověřování* poskytovatele ověřování Azure Multi-Factor Authentication, vždy fakturuje se pro každé ověření, pokud je zprostředkovatel vícefaktorového ověřování hello propojené tooyour předplatné. Můžete přiřadit toousers licence MFA, ale stále platit budete pro každý požadavek dvoustupňové ověření, zda pochází od uživatele licence MFA přiřazené nebo ne.

**Otázka: Moje organizace mají toouse a synchronizaci identit toouse Azure Multi-Factor Authentication?**

Pokud vaše organizace používá model fakturace na základě spotřeby, Azure Active Directory je volitelné, ale nejsou vyžadována. Pokud váš poskytovatel MFA není propojené tooan klienta Azure AD, lze nasadit pouze Server Azure Multi-Factor Authentication nebo hello sady SDK Azure Multi-Factor Authentication na místě.

Azure Active Directory není nutná pro model hello licence licence jsou přidány toohello klienta Azure AD při nákupu a přiřadit jim toousers v adresáři hello.

## <a name="manage-and-support-user-accounts"></a>Spravovat a podporovat uživatelské účty

**Otázka: co říct mé uživatelé toodo Pokud nemáte, obdrží odpověď na svůj telefon nebo nemáte svůj telefon s nimi?**

Všichni uživatelé zpravidla nakonfigurovat více než jednu metodu ověřování. Sdělte jim, že tootry znovu se přihlaste, ale vyberte metodu ověření různých na přihlašovací stránku hello.

Můžete zadat odkaz vaši uživatelé toohello [Průvodci odstraňováním potíží koncového uživatele](./end-user/multi-factor-authentication-end-user-troubleshoot.md).


**Otázka: co je třeba udělat, pokud jeden z mých uživatelů nelze načíst v tootheir účtu?**

Tím, že je znovu toogo prostřednictvím procesu registrace hello můžete resetovat hello uživatelský účet. Další informace o [Správa nastavení uživatelů a zařízení s Azure Multi-Factor Authentication v cloudu hello](multi-factor-authentication-manage-users-and-devices.md).

**Otázka: co je třeba udělat, pokud jeden z mých uživatelů ztratí telefonního čísla, která používá hesla aplikací?**

tooprevent neoprávněným přístupem, odstranit všechny hello uživatelské hesla aplikací. Po nahrazení zařízení má uživatel hello, můžete znovu vytvořit hello hesla. Další informace o [Správa nastavení uživatelů a zařízení s Azure Multi-Factor Authentication v cloudu hello](multi-factor-authentication-manage-users-and-devices.md).

**Otázka: Co když uživatel nemůže přihlásit toonon prohlížečových aplikací?**

Pokud vaše organizace stále používá starší verze klientů a [povolené hello používání hesel aplikací](multi-factor-authentication-whats-next.md#app-passwords), pak uživatelům nemůžete se přihlásit toothese starších verzí klientů s uživatelského jména a hesla. Místo toho potřebují příliš[nastavit hesla aplikací](./end-user/multi-factor-authentication-end-user-app-passwords.md). Uživatelé musí vymazat (odstranit) restartujte aplikace hello jejich přihlašovací údaje a pak se přihlaste pomocí svého uživatelského jména a *heslo aplikace* místo hesla pro regulární.

Pokud vaše organizace nemá starší verze klientů, neměli povolit uživatelům toocreate hesla aplikací.

> [!NOTE]
> Moderní ověřování pro klienty Office 2013
>
> Hesla aplikací jsou pouze nezbytné pro aplikace, které nepodporují moderní ověřování. Klienti Office 2013 podporovat protokoly moderní ověřování, ale potřebovat toobe nakonfigurované. Novějších klientů Office automaticky podporují protokoly moderní ověřování. Další informace najdete v tématu hello [oznámení veřejné preview moderní ověřování Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**Otázka: Moji uživatelé prohlašují, že někdy nebudete přijímat textové zprávy hello, nebo jejich odpovědět tootwo způsob textové zprávy, ale hello ověření vyprší časový limit.**

Doručování textové zprávy a obdržení odpovědi v obousměrné služby SMS se nezaručuje, protože nejsou k dispozici neovladatelném faktory, které mohou ovlivnit hello spolehlivost služby hello. Tyto faktory zahrnují hello cílové země, operátora mobilní telefon hello a síly hello signál.

Pokud se uživatelé často dochází k problémům s spolehlivě přijímat textové zprávy, sdělte jim, že metoda hello toouse mobilní aplikace nebo telefonní hovor místo. mobilní aplikace Hello může přijímat oznámení jak přes mobilní služby a připojení Wi-Fi. Kromě toho hello mobilní aplikace může generovat ověřovací kódy, i v případě, že má zařízení hello signál vůbec. je k dispozici pro aplikaci Microsoft Authenticator Hello [Android](http://go.microsoft.com/fwlink/?Linkid=825072), [IOS](http://go.microsoft.com/fwlink/?Linkid=825073), a [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

Pokud musíte použít textové zprávy, doporučujeme pomocí jednosměrné služby SMS, nikoli obousměrné služby SMS, pokud je to možné. Jednosměrné služby SMS je spolehlivější a uživatelům zabrání by docházelo k globální poplatky za služby SMS z odpovídajících tooa textová zpráva, která byla odeslána z jiné země.

**Otázka: je možné změnit hello množství času mé uživatelé mají tooenter hello ověřovací kód z textové zprávy před vypršením hello systému?**

V některých případech Ano. 

Pro jednosměrné služby SMS s Azure MFA serveru verze 7.0 nebo vyšší můžete nakonfigurovat časový limit hello nastavení podle nastavení klíče registru. Po hello cloudové služby MFA odešle hello textovou zprávu, je vrácen toohello MFA Server hello ověřovací kód (nebo jednorázové heslo). Hello MFA Server ukládá hello kód v paměti na 300 sekund ve výchozím nastavení. Pokud hello bez zadání kódu hello před hello předané 300 sekund, jejich ověřování byl odepřen. Použijte tyto kroky toochange hello výchozí časový limit nastavení:

1. Přejděte tooHKLM\Software\Wow6432Node\Positive Networks\PhoneFactor.
2. Vytvořte klíč registru typu DWORD s názvem **pfsvc_pendingSmsTimeoutSeconds** a nastavit hello čas v sekundách, který má hello Azure MFA serveru toostore jednorázového hesla.

>[!TIP] 
>Pokud máte více serverů MFA, zná pouze hello jeden, který zpracovává žádosti o ověření původní hello hello ověřovací kód, který vám byl zaslán toohello uživatele. Když uživatel hello zadá kód hello, hello toovalidate požadavek ověřování, musí se poslat toohello stejný server. Pokud se ověření kódu hello je odeslat tooa jiný server, ověřování hello byl odepřen. 

Pro obousměrné služby SMS s Azure MFA serveru můžete nakonfigurovat nastavení limitu hello v hello portálu pro správu MFA. Pokud uživatelé nejsou v rámci definovaného časového limitu hello odpovídat toohello SMS, jejich ověřování byl odepřen. 

Pro jednosměrné služby SMS s Azure MFA v cloudu hello (včetně hello rozšíření služby AD FS adaptér nebo hello Network Policy Server) nelze nakonfigurovat nastavení limitu hello. Azure AD ukládá hello ověřovací kód po dobu 180 sekund. 

**Otázka: je možné použít hardwarové tokeny s Azure Multi-Factor Authentication Server?**

Pokud používáte Azure Multi-Factor Authentication Server, můžete import tokenů založené na čase, jednorázové heslo (TOTP) otevřete ověřování (OATH) třetí strany a potom je použít pro dvoustupňové ověření.

Můžete použít ActiveIdentity tokeny, které jsou tokeny OATH TOTP, pokud jste v souboru CSV uvést hello tajný klíč a importovat tooAzure aplikace Multi-Factor Authentication Server. Můžete tokeny OATH s Active Directory Federation Services (ADFS), ověřování pomocí formulářů Internet Information Server (IIS) a RADIUS Remote Authentication Dial-In User Service (), tak dlouho, dokud hello klientský systém může přijmout hello uživatelský vstup.

Můžete importovat tokeny OATH TOTP třetích stran s hello následujících formátů:  

- Kontejner přenosné symetrického klíče (PSKC)  
- Sdílený svazek clusteru, pokud soubor hello obsahuje sériové číslo, tajný klíč ve formátu Base 32 a časovém intervalu  

**Otázka: je možné použít Server Azure Multi-Factor Authentication toosecure Terminálové služby?**

Ano, ale pokud používáte Windows Server 2012 R2 nebo novější můžete pouze zabezpečené Terminálové služby pomocí Brána vzdálené plochy (Brána VP).

Změny zabezpečení ve Windows serveru 2012 R2 změnit, jak Server Azure Multi-Factor Authentication připojí balíček zabezpečení toohello místní úřad zabezpečení (LSA) v systému Windows Server 2012 a starší verze. Pro Terminálové služby v systému Windows Server 2012 nebo starší verze, můžete [zabezpečit aplikaci pomocí ověřování systému Windows](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Pokud používáte Windows Server 2012 R2, musíte služby Brána VP.

**Otázka: I MFA serveru nakonfigurovaná ID volajícího, ale Moji uživatelé stále přijímat volání služby Multi-Factor Authentication z anonymní volajícího.**

Při volání služby Multi-Factor Authentication jsou umístění přes veřejnou telefonní síť hello se někdy směrování v poskytovatel, který nepodporuje ID volajícího. Z toho důvodu není zaručena ID volajícího, i když hello systém Multi-Factor Authentication je vždy odešle.

**Otázka: Proč Moji uživatelé probíhá výzvami tooregister své informace o zabezpečení?**
Tady je několik důvodů, že uživatelé mohou být výzvami tooregister své informace zabezpečení:

- Hello uživatel povolen pro MFA jim správce ve službě Azure AD, ale nemá informace o zabezpečení pro svůj účet ještě zaregistrována.
- bylo povoleno Hello uživatele samoobslužné služby ve službě Azure AD pro vytvoření nového hesla. informace o zabezpečení Hello pomůže je obnovit své heslo v budoucnu hello, pokud se někdy v budoucnu zapomněli.
- Hello uživatel získat přístup k aplikaci, která má toorequire zásady podmíněného přístupu MFA a nebyl dříve registrován pro MFA.
- Hello uživatelé registrují zařízení s Azure AD (včetně Azure AD Join) a vaše organizace vyžaduje vícefaktorové ověřování pro registraci zařízení, ale uživatel hello nebyl dříve registrován pro MFA.
- uživatel Hello generuje Windows Hello pro firmy v systému Windows 10 (který vyžaduje vícefaktorové ověřování) a nebyl dříve registrován pro MFA.
- Hello organizace vytvořila a povolené zásady registrace MFA, které bylo použité toohello uživatele.
- uživatel Hello dříve zaregistrovaný pro MFA, ale vybrali metodu ověření, která od správce zakázal. Hello uživatel musí přejít proto registrací MFA znovu tooselect nový výchozí metoda ověření.


## <a name="errors"></a>Chyby
**Otázka: co uživatelé dělat, když se zobrazí chybová zpráva "žádosti o ověření není pro aktivovaný účet", při použití oznámení mobilní aplikace?**

Sdělte jim, že toofollow tento postup tooremove svého účtu z mobilní aplikace hello, poté ji znovu přidejte:

1. Přejděte příliš[profilu Azure portálu](https://account.activedirectory.windowsazure.com/profile/) a přihlaste se pomocí účtu organizace.
2. Vyberte **dalšího ověření zabezpečení**.
3. Odeberte existující účet hello z mobilní aplikace hello.
4. Klikněte na tlačítko **konfigurace**a pak postupujte podle hello pokyny tooreconfigure hello mobilní aplikace.

**Otázka: co by uživatelé dělat, pokud se zobrazí chybová zpráva 0x800434D4L při přihlášení tooa neprohlížečové aplikace?**

Při pokusu o toosign v tooa neprohlížečové aplikace, nainstalována na místním počítači, který nelze použít s účty, které vyžadují dvoustupňové ověření, dojde k chybě 0x800434D4L Hello.

Alternativní řešení této chyby je toohave samostatné uživatelských účtů pro související správu a operace bez oprávnění správce. Později můžete propojit poštovní schránky mezi účtu správce a účet bez oprávnění správce, aby tooOutlook můžete přihlásit pomocí účtu bez oprávnění správce. Další podrobnosti o tomto řešení najdete další informace jak příliš[poskytnout možnost tooopen a zobrazení hello obsah poštovní schránky uživatele hello správce](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Další kroky
Pokud váš dotaz není zodpovězené, nechejte ji prosím v hello komentáře v dolní části hello hello stránky. Nebo, tady jsou některé další možnosti nápovědy:

* Hledání hello [znalostní báze podpory společnosti Microsoft](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) pro řešení toocommon technické problémy.
* Hledání a procházet technické dotazy a odpovědi od komunity hello nebo svůj vlastní dotaz hello zadat [fóra Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).
* Pokud jste zákazník starší verze PhoneFactor a máte nějaké otázky nebo potřebujete pomoc, resetování hesla, použijte hello [resetování hesla](mailto:phonefactorsupport@microsoft.com) odkaz tooopen případu podpory.
* Obraťte se na pracovníky technické podpory prostřednictvím [podpory Azure Multi-Factor Authentication Server (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947). Při kontaktování nám, je užitečné, pokud zahrnete tolik informací o problému nejdříve. Informace, které můžete zadat zahrnují hello stránky, kde jste viděli hello chyba, hello určitý kód chyby, ID hello konkrétní relace a ID hello hello uživatele, který viděli hello chyby.
