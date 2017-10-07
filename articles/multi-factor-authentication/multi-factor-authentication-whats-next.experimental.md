---
title: aaaConfigure Azure MFA | Microsoft Docs
description: "Toto je stránka ověřování Azure Multi-Factor hello, která popisuje co toodo vedle pomocí vícefaktorového ověřování.  To zahrnuje sestavy, upozornění na podvod, jednorázové přihlášení, vlastní hlasové zprávy, ukládání do mezipaměti, důvěryhodné IP adresy a aplikaci hesla."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: kgremban
ms.openlocfilehash: db7d87d95b73fed78d3ce599fd03da9692851663
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Konfigurovat nastavení ověřování Azure Multi-Factor Authentication
Tento článek usnadňuje správu ověřování Azure Multi-Factor Authentication teď, když jsou spuštěná.  Vysvětluje různé témata, které vám pomůžou tooget hello naplno Azure Multi-Factor Authentication.  Ne všechny tyto funkce jsou dostupné v každé verzi Azure Multi-Factor Authentication.

| Funkce | Popis | 
|:--- |:--- |
| [Upozornění na podvod](#fraud-alert) |Upozornění na podvod můžete nakonfigurovat a nastavit tak, aby vaši uživatelé mohou zasílat zprávy o tooaccess podvodné pokusy o jejich prostředky. |
| [Jednorázové přihlášení](#one-time-bypass) |Jednorázové přihlášení umožňuje uživatele tooauthenticate jednorázově "vynecháte" služby Multi-Factor authentication. |
| [Vlastní hlasové zprávy](#custom-voice-messages) |Vlastní hlasové zprávy umožňují toouse vlastní záznamy nebo pozdrav pomocí služby Multi-Factor authentication. |
| [Ukládání do mezipaměti](#caching-in-azure-multi-factor-authentication) |Ukládání do mezipaměti můžete tooset za určité časové období, aby automaticky být úspěšné následné pokusy o ověření. |
| [Důvěryhodné IP adresy](#trusted-ips) |Správci spravované nebo federované klienta můžete použít důvěryhodné IP adresy toobypass dvoustupňové ověřování pro uživatele, kteří se přihlašují z místního intranetu hello společnosti. |
| [Hesla aplikací](#app-passwords) |Heslo aplikace umožňuje aplikaci, která není podporující vícefaktorové ověřování služby Multi-Factor authentication toobypass a pokračovat v práci. |
| [Zapamatovat Vícefaktorové ověřování na zapamatovaných zařízeních a prohlížeče](#remember-multi-factor-authentication-for-devices-that-users-trust) |Umožňuje tooremember zařízení pro sadu počet dnů po uživatel se úspěšně přihlásil pomocí vícefaktorového ověřování. |
| [Volitelný ověření metody](#selectable-verification-methods) |Můžete se metody ověřování hello toochoose, které jsou k dispozici pro uživatele toouse. |

## <a name="access-hello-azure-mfa-management-portal"></a>Hello přístup k portálu pro správu Azure MFA

Hello funkcí, které jsou zahrnuté v tomto článku jsou nakonfigurované v hello Azure Multi-Factor Authentication Management Portal. Existují dva způsoby tooaccess hello portálu pro správu MFA prostřednictvím hello portál Azure classic. Hello, nejprve je Správa zprostředkovatel vícefaktorového ověřování. Hello druhou je přes nastavení služby hello vícefaktorového ověřování. 

### <a name="use-an-authentication-provider"></a>Použití zprostředkovatele ověřování

Pokud používáte zprostředkovatel vícefaktorového ověřování na základě spotřeby mfa, použijte tento portál metoda tooaccess hello správy.

tooaccess hello portálu pro správu MFA prostřednictvím poskytovatele Azure Multi-Factor Auth, přihlaste se k hello portál Azure classic jako správce a možnost služby Active Directory vyberte hello. Klikněte na tlačítko hello **zprostředkovatelé vícefaktorového ověřování** kartě, pak vyberte adresář a klikněte na tlačítko hello **spravovat** tlačítko dole v hello.

### <a name="use-hello-mfa-service-settings-page"></a>Na stránce nastavení vícefaktorového ověřování služby hello 

Pokud máte jeden z následujících licencí hello, můžete použít stránku hello nastavení vícefaktorového ověřování služby:
- Azure MFA
- Azure AD Premium 
- Enterprise Mobility + Security

tooaccess hello portálu pro správu MFA přes hello stránky nastavení vícefaktorového ověřování služby, přihlaste se k hello portál Azure classic jako správce a vyberte možnost Active Directory hello. Klikněte na adresáře a pak klikněte na tlačítko hello **konfigurace** kartě. V části služby Multi-Factor authentication hello vyberte **spravovat nastavení služby**. V dolní části hello hello nastavení vícefaktorového ověřování služby stránky, klikněte na tlačítko hello **přejděte toohello portál** odkaz.


## <a name="fraud-alert"></a>Upozornění na podvod
Upozornění na podvod můžete nakonfigurovat a nastavit tak, aby vaši uživatelé mohou zasílat zprávy o tooaccess podvodné pokusy o jejich prostředky.  Uživatelé mohou zasílat zprávy podvod pomocí mobilní aplikace hello nebo přes svůj telefon.

### <a name="set-up-fraud-alert"></a>Nastavení upozornění na podvod
1. Přejděte toohello portálu pro správu MFA podle pokynů hello hello horní části této stránky.
2. V hello Azure Multi-Factor Authentication Management Portal, klikněte na **nastavení** pod hello část konfigurace.
3. V části hello upozornění na podvod části hello nastavení stránky, zkontrolujte hello **uživatelům povolit upozornění na podvod toosubmit** zaškrtávací políčko.
4. Vyberte **Uložit** tooapply změny. 

### <a name="configuration-options"></a>Možnosti konfigurace

- **Blokování uživatele při nahlášení podvodu** – Pokud uživatel sestavy podvod, svůj účet je blokován.
- **Kód podvodu během počáteční s pozdravem tooReport** -uživatelé obvykle stisknutím klávesy # tooconfirm dvoustupňové ověřování. Pokud chtějí tooreport podvodů, jejich zadat kód ještě před stisknutím klávesy #. Tento kód je **0** ve výchozím nastavení, ale můžete jej upravit.

> [!NOTE]
> Společnosti Microsoft výchozí hlasový pozdrav vyzvat uživatele toopress 0# toosubmit upozornění na možný podvod. Pokud chcete toouse kódu než 0, měli zaznamenávat a nahrát vlastní vlastní hlasový pozdrav příslušné pokyny.

![Možnosti upozornění podvod – snímek obrazovky](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="how-users-report-fraud"></a>Jak uživatelé nahlášení podvodu 
Upozornění na podvod mohou být oznámeny dvěma způsoby.  Buď prostřednictvím hello mobilní aplikace nebo prostřednictvím hello phone.  

#### <a name="report-fraud-with-hello-mobile-app"></a>Ohlásit podvod v mobilní aplikaci hello
1. Odeslání ověření tooyour telefon, vyberte ho aplikaci Microsoft Authenticator tooopen hello.
2. Vyberte **Odepřít** na hello oznámení. 
3. Vyberte **nahlášení podvodu**.
4. Zavřít hello aplikace.

#### <a name="report-fraud-with-a-phone"></a>Nahlášení podvodu po telefonu
1. Když ověřovací hovor se dodává v telefonu tooyour, odpovězte na ni.  
2. tooreport podvodů, zadejte kód podvodu hello (výchozí hodnota je 0) a pak hello znak #. Potom budete upozorněni, aby byla odeslána upozornění na možný podvod.
3. Ukončení hello volání.

### <a name="view-fraud-reports"></a>Zobrazit sestavy podvod
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Na levé straně hello vyberte služby Active Directory.
3. V horní části hello, vyberte **zprostředkovatelé vícefaktorového ověřování** tooshow seznam vaši zprostředkovatelé vícefaktorového ověřování.
4. Vyberte poskytovatele služby Multi-Factor Auth a klikněte na tlačítko **spravovat** v hello dolní části stránky hello. Otevře se Hello Azure Multi-Factor Authentication Management Portal.
5. Na hello Azure Multi-Factor Authentication Management Portal, v části sestavy A zobrazení, klikněte na tlačítko **upozornění na podvod**.
6. Zadejte rozsah dat hello chcete tooview v sestavě hello. Můžete také zadat uživatelských jmen, telefonních čísel a stav hello uživatele.
7. Klikněte na tlačítko **spustit** tooshow sestavu upozornění na podvod. Klikněte na tlačítko **exportovat tooCSV** Pokud chcete sestavu tooexport hello.

## <a name="one-time-bypass"></a>Jednorázové přihlášení
Jednorázové přihlášení umožňuje uživatele tooauthenticate jednorázově bez provedení dvoustupňové ověřování. Hello jednorázové přihlášení je dočasné a vyprší po zadaném počtu sekund. V situacích, kde hello mobilní aplikaci nebo phone nepřijímá oznámení nebo telefonní hovor můžete povolit jednorázové přihlášení, uživatel hello přístup hello požadovaného prostředku.

### <a name="create-a-one-time-bypass"></a>Vytvoření jednorázové přihlášení
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Přejděte toohello portálu pro správu MFA podle pokynů hello hello horní části této stránky.
3. Pokud se zobrazí název hello zprostředkovatele Azure MFA na hello ponechaná na ** + ** další tooit, klikněte na tlačítko hello ** + **. Jiný MFA Server replikační skupiny a hello Azure výchozí skupiny jsou zobrazeny. Vyberte příslušné skupiny hello.
4. V části Správa uživatele, vyberte **jednorázové přihlášení**.
5. Na stránce jednorázové přihlášení hello, klikněte na **nové jednorázové přihlášení**.

  ![Cloud](./media/multi-factor-authentication-whats-next/create1.png)

6. Zadejte uživatelské jméno hello, hello trvání hello obejití a hello důvod pro jednorázové přihlášení hello. Klikněte na tlačítko **nepoužívat**.
7. Hello časový limit vstoupí v platnost okamžitě, tak uživatel hello potřebám toosign v před hello jednorázového přihlášení vyprší platnost. 

### <a name="view-hello-one-time-bypass-report"></a>Zobrazení hello jednorázové vynechat sestavy
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Na levé straně hello vyberte služby Active Directory.
3. V horní části hello, vyberte **zprostředkovatelé vícefaktorového ověřování** tooshow seznam vaši zprostředkovatelé vícefaktorového ověřování.
4. Vyberte poskytovatele služby Multi-Factor Auth a klikněte na tlačítko **spravovat** v hello dolní části stránky hello. Otevře se Hello Azure Multi-Factor Authentication Management Portal.
5. Na hello Azure Multi-Factor Authentication Management Portal, na levé straně hello v části sestavy A zobrazení, klikněte na tlačítko **jednorázové přihlášení**.
6. Zadejte rozsah dat hello chcete tooview v sestavě hello. Můžete také zadat uživatelských jmen, telefonních čísel a stav hello uživatele.
7. Klikněte na tlačítko **spustit** tooshow sestavu přeskočení. Klikněte na tlačítko **exportovat tooCSV** Pokud chcete sestavu tooexport hello.

## <a name="custom-voice-messages"></a>Vlastní hlasové zprávy
Vlastní hlasové zprávy umožňují toouse vlastní záznamy nebo přivítání pro dvoustupňové ověření. Můžete vytvořit vlastní hlasové zprávy v přidání tooor tooreplace hello Microsoft záznamy.

Než začnete, nezapomeňte tyto věci:

* Hello podporované formáty souborů jsou ve formátu WAV nebo MP3.
* maximální velikost souboru Hello je 5 MB.
* Ověřování zprávy musí být kratší než 20 sekund. Zprávy, které jsou delší než 20 sekund nemáte uživatelům hello dostatek času toorespond před uplynutím hello ověření časového limitu.

### <a name="set-up-a-custom-message"></a>Nastavit vlastní zprávu

Existují dvě části toocreating vlastní zprávu. Nejprve nahrát uvítací zprávu a pak můžete zapnout pro vaše uživatele.

tooupload vlastní zprávu:

1. Vytvořte vlastní hlasové zprávy pomocí jedné z hello podporované formáty souborů.
2. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
3. Přejděte toohello portálu pro správu MFA podle pokynů hello hello horní části této stránky.
4. V hello Azure Multi-Factor Authentication Management Portal, klikněte na **hlasové zprávy** pod hello část konfigurace.
5. Na hello konfigurace: hlasové zprávy stránky, klikněte na tlačítko **nové hlasové zprávy**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom1.png)
6. Na hello konfigurace: stránka nové hlasové zprávy, klikněte na tlačítko **Správa souborů zvuk**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom2.png)
7. Na hello konfigurace: zvukových souborů stránky, klikněte na tlačítko **nahrát soubor zvuk**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom3.png)
8. Na hello konfigurace: nahrát soubor zvuku, klikněte na tlačítko **Procházet** a přejděte tooyour hlasové zprávy, klikněte na **otevřete**.
9. Přidejte popis a klikněte na tlačítko **nahrát**.
10. Po nahrání hello hlasové zprávy zprávu potvrdí, že jste úspěšně odeslali hello souboru.

tooturn na uvítací zprávu pro vaše uživatele:

1. Na levé straně hello, klikněte na tlačítko **hlasové zprávy**.
2. V části hello části hlasové zprávy, klikněte na **nové hlasové zprávy**.
3. Hello jazyk rozevíracího seznamu vyberte jazyk.
4. Pokud tato zpráva je pro danou aplikaci, zadejte ji do aplikace hello.
5. Z rozevíracího seznamu typu zprávy hello vyberte hello zpráva typu toobe přepisu s novou vlastní zprávu.
6. Z rozevíracího seznamu zvukový soubor hello vyberte v první části hello hello zvukového souboru, který jste nahráli.
7. Klikněte na možnost **Vytvořit**. Zobrazí se zpráva potvrzující, že jste úspěšně vytvořili hlasové zprávy.
    ![Cloud](./media/multi-factor-authentication-whats-next/custom5.png)</center>

## <a name="caching-in-azure-multi-factor-authentication"></a>Ukládání do mezipaměti v Azure Multi-Factor Authentication
Ukládání do mezipaměti můžete tooset za určité časové období, aby automaticky být úspěšné následné pokusy o ověření v tomto časovém období. Ukládání do mezipaměti se používá při místních systémů, jako je například VPN odesílat mnoho žádostí o ověření hello první požadavek stále probíhá. Povolení hello následné žádosti toosucceed automaticky poté, co uživatel hello úspěšné ověření první hello v průběhu. 

Ukládání do mezipaměti není určený toobe použít pro přihlášení tooAzure AD.

### <a name="set-up-caching"></a>Nastavení ukládání do mezipaměti 
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Přejděte toohello portálu pro správu MFA podle pokynů hello hello horní části této stránky.
3. V hello Azure Multi-Factor Authentication Management Portal, klikněte na **ukládání do mezipaměti** pod hello část konfigurace.
4. Klikněte na tlačítko **nová mezipaměť** na ukládání do mezipaměti stránku hello konfigurovat.
5. Vyberte typ mezipaměti hello a počet sekund mezipaměti hello. Klikněte na možnost **Vytvořit**.

<center>![Cloud](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Důvěryhodné IP adresy
Důvěryhodné IP adresy je funkce Azure mfa, správci spravované nebo federované klienta můžete použít toobypass dvoustupňové ověřování. Která se používá pro uživatele, kteří se přihlašují ze společnosti hello místní intranet. Tato funkce je k dispozici hello plnou verzi Azure Multi-Factor Authentication, ne hello bezplatnou verzi pro správce. Podrobnosti o tom, jak tooget hello plnou verzi Azure Multi-Factor Authentication, naleznete v části [Azure Multi-Factor Authentication](multi-factor-authentication.md).

| Typ klientovi Azure AD | Dostupné možnosti důvěryhodných adres IP |
|:--- |:--- |
| Spravované |<li>Určitých rozsahů IP adres – správci mohou určit rozsah IP adres, které mohou obejít dvoustupňové ověřování pro uživatele, kteří se přihlašují ze hello firemní intranet.</li> |
| Federované |<li>Všichni uživatelé federovaný – všechny federovaní uživatelé, kteří se přihlašují v z uvnitř hello organizace bude obejít dvoustupňové ověření pomocí deklarace identity vystavené službou AD FS.</li><br><li>Určitých rozsahů IP adres – správci mohou určit rozsah IP adres, které mohou obejít dvoustupňové ověřování pro uživatele, kteří se přihlašují ze hello firemní intranet. |

To nepoužívat funguje pouze z uvnitř intranetu společnosti. 

**Činnost koncového uživatele v podnikové síti:**

Pokud je zakázáno důvěryhodné IP adresy, dvoustupňové ověření je vyžadována pro toky prohlížeče a hesla aplikací jsou požadovány pro starší aplikace plně funkčního klienta. 

Pokud je povoleno důvěryhodné IP adresy, dvoustupňové ověření je *není* požadované pro toky prohlížeče. Hesla aplikací jsou *není* požadované pro starší pokročilé klientské aplikace za předpokladu, že hello uživatele nebyla již vytvořit heslo aplikace. Jakmile heslo aplikace se používá, zůstane vyžaduje. 

**Činnost koncového uživatele mimo corpnet:**

Zda důvěryhodné IP adresy je povolena, nebo Ne, dvoustupňové ověření je vyžadována pro toky prohlížeče a hesla aplikací jsou požadovány pro starší aplikace plně funkčního klienta. 

### <a name="tooenable-trusted-ips"></a>tooenable důvěryhodné IP adresy
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Přejděte toohello stránky nastavení vícefaktorového ověřování služby podle pokynů hello od začátku hello tohoto článku.
3. Na stránce nastavení služby hello v rámci důvěryhodných adres IP máte dvě možnosti:
   
   * **Pro žádosti od federovaných uživatelů pocházející z mém intranetu** – zaškrtávací políčko hello. Všechny federovaní uživatelé, kteří se přihlašují ze hello podnikové síti vynechat dvoustupňové ověření pomocí deklarace identity vystavené službou AD FS.
   * **Pro žádosti od určitého rozsahu veřejné IP adresy** – zadat pomocí notace CIDR hello textového pole zadejte hello IP adresy. Příklad: xxx.xxx.xxx.0/24 pro IP adresy v rozsahu xxx.xxx.xxx.1 hello – xxx.xxx.xxx.254 nebo xxx.xxx.xxx.xxx/32 pro jednu IP adresu. Můžete zadat až too50 rozsahy IP adres. Uživatelé, kteří se přihlašují z těchto IP adres nepoužívat dvoustupňové ověřování.
4. Klikněte na **Uložit**.
5. Jakmile byly použity hello aktualizace, klikněte na možnost **Zavřít**.

![Důvěryhodné IP adresy](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Hesla aplikací
Některé aplikace, jako je Office 2010 nebo starší a Apple Mail, nepodporují dvoustupňové ověřování. Nejsou nakonfigurované tooaccept druhé ověření. toouse těchto aplikací, musíte místo tradiční heslo toouse "hesla aplikací". heslo aplikace Hello umožňuje toobypass aplikace hello dvoustupňové ověřování a pokračovat v práci.

> [!NOTE]
> Moderní ověřování pro hello klientům Office 2013
> 
> Klienti Office 2013 (včetně Outlook) a novější podporu moderních ověřovacích protokolů a může být povoleno toowork s dvoustupňové ověřování. Po povolení nejsou hesla aplikací vyžaduje pro tyto klienty.  Další informace najdete v tématu [Office 2013 veřejné Preview verze moderního ověřování oznámeno](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### <a name="important-things-tooknow-about-app-passwords"></a>Důležité věci tooknow o heslech aplikací
Tady je seznam důležitých věcí, které byste měli vědět o heslech aplikací.

* Hesla aplikací by měly být zadány v hello vstupní pole jednou za aplikace. Uživatelé nemáte tookeep sledovat jejich a pokaždé, když je zadat.
* Hello vlastní heslo je automaticky vygenerováno a není zadané uživatelem hello. Hello automaticky vygenerované heslo je pro tooguess útočník složitější a je bezpečnější.
* Existuje omezení 40 hesel na uživatele. 
* Aplikace, které mezipaměti hesla a používat ho v místní scénářích mohou začít selhávat, protože heslo aplikace hello není známo mimo id organizace hello. Příkladem je e-mailů Exchange, které jsou na místě, ale hello Archivovaná pošta se nachází v cloudu hello. Hello stejné heslo nebude fungovat.
* Při spuštění služby Multi-Factor authentication, můžete použít heslo s některé neprohlížečových klientů. Pro úlohy správy, nelze používat hesla aplikací. Ujistěte se, vytvoření účtu služby pomocí skriptů prostředí PowerShell silné heslo toorun a nepovolujte tohoto účtu pro dvoustupňové ověření.

> [!WARNING]
> Hesla aplikací nepodporují v hybridních prostředích, kde klienti komunikaci s místní a cloudové Automatická konfigurace koncových bodů. Hesel domény vyžaduje tooauthenticate na místě a hesla aplikací jsou požadované tooauthenticate hello cloudu.

### <a name="naming-guidance-for-app-passwords"></a>Pokyny pro hesla aplikací pro pojmenování
Názvů hesel aplikací by měla odpovídat hello zařízení, na které se používají. Pokud máte přenosný počítač, který má neprohlížečové aplikace, například vytvořit jedno heslo aplikace s názvem přenosný počítač a používat takové heslo aplikace. Pak vytvořte jiné heslo aplikace s názvem Desktop pro hello stejné aplikace ve stolním počítači. 

Společnost Microsoft doporučuje vytvoření jednoho hesla aplikace na zařízení, není jedno heslo aplikace na aplikaci.

### <a name="federated-sso-app-passwords"></a>Hesla aplikací federované (SSO)
Azure AD podporuje federation (jednotné přihlášení) s místními systému Windows Server Active Directory Domain Services (AD DS). Pokud je vaše organizace Federovaná pomocí Azure AD a budete toobe používání ověřování Azure Multi-Factor Authentication, je důležité, abyste hello informace o heslech aplikací. Tato část se týká pouze zákazníků toofederated (SSO).

* Hesla aplikací jsou ověřit pomocí služby Azure AD a proto obcházet federace. Federace se používá aktivně pouze při nastavování hesla aplikací.
* Pro federované uživatele (SSO) jsme nikdy přejděte toohello zprostředkovatele Identity (IdP) na rozdíl od pasivního toku hello. Hello hesla jsou uložena v id organizace hello. Pokud hello uživatel odejde hello společnosti, má tento informace id tooorganizational tooflow pomocí služby DirSync v skutečný čas. Zakázání/odstranění účtu může trvat hodiny toosync toothree, přičemž dojde ke zpoždění zakázání/odstranění hesla aplikace ve službě Azure AD.
* Místní nastavení služby Access Control klienta není dodrženo heslem aplikace.
* Bez ověřování místní protokolování nebo auditování funkce je k dispozici u hesla aplikace.
* Některé pokročilé architektury návrhů může vyžadovat kombinaci organizační uživatelského jména, hesla a hesla aplikací, při použití dvoustupňové ověřování s klienty. I když to závisí na kde ověřují. Pro klienty, kteří se ověřují se místní infrastruktury by použijte organizační uživatelské jméno a heslo. Pro klienty, kteří se ověřují se Azure AD použijete heslo aplikace hello.

  Předpokládejme například, že máte architekturu, která se skládá z těchto instancí:

  * Federujete místní instanci služby Active Directory s Azure AD
  * Používáte Exchange online
  * Používáte-li aplikace Lync, který je konkrétně místní
  * Používáte Azure Multi-Factor Authentication

  ![Ověření](./media/multi-factor-authentication-whats-next/federated.png)

  V těchto případech musí postupujte takto:

  * Při podepisování – v tooLync, použít uživatelské jméno a heslo vaší organizace.
  * Při pokusu o tooaccess hello adresáře prostřednictvím klienta aplikace Outlook, která se připojuje tooExchange online, používejte heslo aplikace.

### <a name="allow-app-password-creation"></a>Povolit vytváření heslo aplikace
Ve výchozím nastavení uživatelé nemůžou vytvářet hesla aplikací, ale můžete povolit funkci hello sami. Uživatelé tooallow hello hesla aplikací toocreate možnost, použijte následující postup hello:

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Přejděte toohello stránky nastavení vícefaktorového ověřování služby podle pokynů hello od začátku hello tohoto článku.
3. Vyberte přepínač hello vedle příliš**povolit uživatelům toocreate aplikace hesla toosign do neprohlížečových aplikací**.

![Vytvoření hesel aplikací](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Vytvoření hesel aplikací
Uživatelé mohou vytvářet hesla aplikací během jejich počáteční registrace. Získá možnost na konci hello hello procesu registrace, která by jim umožnila toocreate hesla aplikací.

Uživatelé mohou také vytvářet hesla aplikací po registraci. Hello mohou vytvářet hesla aplikací tak, že změníte jejich nastavení v hello Azure portal nebo hello portál Office 365. Další informace a podrobné pokyny pro uživatele najdete v tématu [co jsou hesla aplikací v Azure Multi-Factor Authentication](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Pro zařízení, které důvěřují uživatelům zapamatovat Vícefaktorové ověřování
Zapamatování Multi-Factor Authentication na zařízení a prohlížeče, že uživatelé důvěryhodnosti je volné funkce pro všechny uživatele vícefaktorového ověřování. Služba Multi-Factor authentication umožňuje uživatelům tooby průchodu vícefaktorového ověřování pro sadu počet dnů po přihlášení. Tato funkce zlepšuje použitelnost minimalizací hello počet, kolikrát může uživatel provést dvoustupňové ověřování na hello stejné zařízení.

Ale pokud účtu nebo zařízení ohrožení, pak zapamatování MFA pro důvěryhodná zařízení může ovlivnit zabezpečení. Pokud podnikové účty nebo ztráty nebo odcizení důvěryhodné zařízení, je třeba příliš[obnovit Multi-Factor Authentication na všech zařízeních](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Tato akce odvolá hello důvěryhodné stav ze všech zařízení a uživatele hello je požadovaná tooperform dvoustupňové ověření znovu. Můžete také určit, aby uživatelé toorestore MFA na jejich vlastní zařízení hello pokyny v [spravovat nastavení pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted)

### <a name="how-it-works"></a>Jak to funguje

Zapamatování Multi-Factor Authentication funguje tak, že když uživatel kontroluje hello nastavení trvalého souboru cookie v prohlížeči hello "nezobrazovat dotaz dalších **X** dní" pole při přihlášení. Hello uživatel nebude vyzván pro MFA znovu z tohoto prohlížeče do vypršení platnosti souboru cookie hello. Pokud uživatel hello otevře jiný prohlížeč na hello stejné zařízení nebo zruší své soubory cookie jsou výzvami tooverify znovu. 

Hello "nezobrazovat dotaz dalších **X** dní" zaškrtávací políčko se nezobrazí na neprohlížečové aplikace, jestli podporují moderní ověřování. Tyto aplikace použít tokeny obnovení, které poskytují nové přístupové tokeny každou hodinu. Po ověření se token obnovení, Azure AD ověří, zda byla provedena hello poslední čas dvoustupňové ověřování v rámci hello nakonfigurovaný počet dnů. 

Na závěr zapamatování vícefaktorového ověřování na důvěryhodných zařízeních snižuje hello počet ověření na webové aplikace (které obvykle poskytne pokaždé, když). Můžete ale taky zvyšuje hello počet ověření pro klienty moderní ověřování, (což obvykle poskytne každých 90 dní).

> [!NOTE]
>Tato funkce není kompatibilní s hello "Zůstat přihlášeni" funkce služby AD FS, když uživatelé provádět dvoustupňové ověřování pro službu AD FS pomocí Azure MFA serveru hello nebo řešení třetí strany vícefaktorového ověřování. Pokud uživatelé ve službě AD FS vyberte možnost "Zůstat přihlášeni" a také označit jako důvěryhodné zařízení pro MFA, nebudou moct tooverify po vypršení platnosti hello "Zapamatovat MFA" počet dní. Azure AD požadavky čerstvé dvoustupňové ověření, ale služby AD FS vrátí token s hello původní vícefaktorového ověřování deklarací identity a datum namísto provádění dvoustupňové ověření znovu. Tento proces nastaví mimo smyčku ověření mezi Azure AD a služby AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Povolit zapamatovat vícefaktorové ověřování
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Přejděte toohello stránky nastavení vícefaktorového ověřování služby podle pokynů hello od začátku hello tohoto článku.
3. Na stránce nastavení služby hello, v části Správa nastavení zařízení uživatele, zkontrolujte hello **povolit uživatelům tooremember služby Multi-Factor authentication na zařízení, které důvěřují** pole.
   ![Mějte na paměti, zařízení](./media/multi-factor-authentication-whats-next/remember.png)
4. Nastavte hello počet dní, které chcete tooallow hello důvěryhodné zařízení toobypass dvoustupňové ověřování. Výchozí hodnota Hello je 14 dnů.
5. Klikněte na **Uložit**.
6. Klikněte na **Zavřít**.

### <a name="mark-a-device-as-trusted"></a>Označit jako důvěryhodné zařízení

Po povolení této funkce, uživatelé mohou označit zařízení jako důvěryhodné při přihlášení kontrolou **dotaz již nezobrazovat**.

![Dotaz již nezobrazovat – snímek obrazovky](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Volitelný ověření metody
Můžete zvolit, které metody ověřování jsou k dispozici pro vaše uživatele. Hello následující tabulka obsahuje stručný přehled jednotlivých metod.

Když uživatelé zaregistrují svoje účty pro MFA, vybírá jejich metoda upřednostňované ověření mimo hello možnosti, které jste povolili. Hello pokyny k jejich registraci, najdete v článku [nastavit účtu pro dvoustupňové ověření](multi-factor-authentication-end-user-first-time.md)

| Metoda | Popis |
|:--- |:--- |
| Volání toophone |Umístí automatický hlasový hovor. Hello uživatele odpovědi hello hovor a stiskem tlačítka # na tooauthenticate klávesnici telefonu hello. Toto telefonní číslo není synchronizovaná tooon místní služby Active Directory. |
| Text zprávy toophone |Odešle textovou zprávu s ověřovacím kódem. uživatel Hello je výzvami tooeither odpověď toohello text, zprávu s hello ověřovací kód nebo tooenter hello ověřovací kód do hello přihlašovací rozhraní. |
| Oznámení pomocí mobilních aplikací |Odešle nabízená oznámení tooyour telefonu nebo zaregistrovaného zařízení. Hello uživatele zobrazení hello oznámení a vybere **ověřte** toocomplete ověření. <br>je k dispozici pro aplikaci Microsoft Authenticator Hello [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Ověřovací kód z mobilní aplikace |aplikace Microsoft Authenticator Hello generuje každých 30 sekund nový ověřovací kód OATH. Hello uživatel zadá tento ověřovací kód do hello přihlašovací rozhraní.<br>je k dispozici pro aplikaci Microsoft Authenticator Hello [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-tooenabledisable-authentication-methods"></a>Jak tooenable nebo zakázat metody ověřování
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Přejděte toohello stránky nastavení vícefaktorového ověřování služby podle pokynů hello od začátku hello tohoto článku.
3. Na stránce nastavení služby hello, v části Možnosti ověření výběrem nebo zrušením výběru možnosti hello chcete toouse.
   ![Požadované možnosti ověření](./media/multi-factor-authentication-whats-next/authmethods.png)
4. Klikněte na **Uložit**.
5. Klikněte na **Zavřít**.
