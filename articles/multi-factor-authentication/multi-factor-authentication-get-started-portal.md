---
title: "aaaUser portálu pro Azure MFA serveru | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, která popisuje, jak tooget začít s vícefaktorovým Ověřováním Azure a hello uživatelského portálu."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 06b419fa-3507-4980-96a4-d2e3960e1772
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 0e36644c3d780249fb98d5da654e9d267c63561a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="user-portal-for-hello-azure-multi-factor-authentication-server"></a>Portál User portal pro Azure Multi-Factor Authentication Server hello

Hello uživatelský portál je webová stránka IIS, který umožňuje uživatelům tooenroll v Azure Multi-Factor Authentication (MFA) a spravovat své účty. Uživatel může změnit své telefonní číslo, svůj PIN kód nebo zvolte toobypass dvoustupňové ověřování během jejich další přihlášení.

Uživatelé přihlášení toohello portálu user portal pomocí svého obvyklého uživatelského jména a hesla, pak buď dokončí hovor dvoustupňové ověření nebo zodpovědět toocomplete otázky zabezpečení ověřování. Pokud je povolena registrace uživatele, uživatelé nakonfiguruje své telefonní číslo a PIN kód hello prvním podepisují toohello uživatelského portálu.

Portál User portal správci mohou nastavit a udělí oprávnění tooadd nové uživatele a aktualizovat stávající uživatele.

V závislosti na vašem prostředí, může být vhodné toodeploy hello uživatelského portálu na hello stejný server jako Server Azure Multi-Factor Authentication nebo na jiném internetového serveru.

![MFA User Portal](./media/multi-factor-authentication-get-started-portal/portal.png)

> [!NOTE]
> Hello uživatelský portál je dostupná jenom s Multi-Factor Authentication Server. Pokud používáte služby Multi-Factor Authentication v cloudu hello, podívejte se vaši uživatelé toohello [nastavení účtu pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user-first-time.md) nebo [spravovat nastavení pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user-manage-settings.md).

## <a name="install-hello-web-service-sdk"></a>Instalace sady SDK webové služby hello

V obou těchto případech, pokud hello SDK webové služby Azure Multi-Factor Authentication je **není** nainstalovány na hello serveru Azure Multi-Factor Authentication (MFA), dokončení hello následujících kroků.

1. Otevřete konzolu služby Multi-Factor Authentication Server hello.
2. Přejděte toohello **sady SDK webové služby** a vyberte **instalace sady SDK webové služby**.
3. Dokončení hello instalace pomocí výchozího nastavení hello, pokud potřebujete toochange je z nějakého důvodu.
4. Vytvořit vazbu webu toohello certifikát SSL ve službě IIS.

Pokud máte dotazy týkající se konfigurace certifikátu protokolu SSL na server služby IIS, najdete v článku hello [jak tooSet protokolu SSL ve službě IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

Hello sady SDK webové služby musí být zabezpečená certifikátem SSL. Pro tento účel stačí certifikát podepsaný svým držitelem. Importujte certifikát hello do úložiště "Důvěryhodné kořenové certifikační autority" hello hello účtu místního počítače na webovém serveru portálu User Portal hello tak, aby při inicializaci připojení SSL hello důvěřovat certifikátu.

![Nastavení konfigurace MFA Serveru – sada SDK webové služby](./media/multi-factor-authentication-get-started-portal/sdk.png)

## <a name="deploy-hello-user-portal-on-hello-same-server-as-hello-azure-multi-factor-authentication-server"></a>Nasazení uživatelského portálu hello na hello stejný server jako hello Azure Multi-Factor Authentication Server

Hello následující požadavky jsou požadované tooinstall hello uživatelského portálu na hello **stejný server** jako hello Azure Multi-Factor Authentication Server:

* Služba IIS, včetně kompatibility s ASP.NET a IIS 6 meta base (pro IIS 7 a novější).
* Účet s právy správce pro počítač hello a domény, pokud je k dispozici. Hello účet potřebuje skupiny zabezpečení Active Directory toocreate oprávnění.
* Zabezpečte hello uživatelského portálu certifikátem SSL.
* Zabezpečte hello SDK webové služby Azure Multi-Factor Authentication certifikátem SSL.

toodeploy hello uživatele portálu, postupujte podle těchto kroků:

1. Otevřete konzolu hello Azure Multi-Factor Authentication Server, klikněte na tlačítko hello **portálu User Portal** ikonu v hello levé nabídce, pak klikněte na tlačítko **instalovat uživatelský portál**.
2. Dokončení hello instalace pomocí výchozího nastavení hello, pokud potřebujete toochange je z nějakého důvodu.
3. Vytvořit vazbu webu toohello certifikát SSL ve službě IIS

   > [!NOTE]
   > Tento certifikát je obvykle veřejně podepsaný certifikát SSL.

4. Z libovolného počítače otevřete webový prohlížeč a přejděte toohello adresu URL, kam se nainstaloval hello user portal (Příklad: https://mfa.contoso.com/MultiFactorAuth). Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.

![Instalace portálu User Portal pro MFA Server](./media/multi-factor-authentication-get-started-portal/install.png)

Pokud máte dotazy týkající se konfigurace certifikátu protokolu SSL na server služby IIS, najdete v článku hello [jak tooSet protokolu SSL ve službě IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

## <a name="deploy-hello-user-portal-on-a-separate-server"></a>Nasazení hello uživatelský portál na samostatný server

Pokud není server hello se spuštěným systémem Azure Multi-Factor Authentication Server internetového, musíte nainstalovat hello uživatelského portálu na **samostatné internetového serveru**.

Pokud vaše organizace používá hello aplikaci Microsoft Authenticator jako jednu z metod ověřování hello a chcete toodeploy hello uživatelského portálu na vlastním serveru, proveďte hello následující požadavky:

* Použít verze 6.0 nebo vyšší hello Azure Multi-Factor Authentication Server.
* Instalace hello user portal na webovém serveru Internetové spuštěna Internetová informační služba (IIS) 6.x nebo vyšší.
* Při použití IIS 6.x, ASP.NET v2.0.50727 musí být nainstalována, zaregistrován a nastavit příliš**povolené**.
* Pokud používáte IIS 7.x nebo vyšší, vyžaduje se služba IIS, včetně základního ověřování, ASP.NET a kompatibility s IIS 6 meta base.
* Zabezpečte hello uživatelského portálu certifikátem SSL.
* Zabezpečte hello SDK webové služby Azure Multi-Factor Authentication certifikátem SSL.
* Zajistěte, aby že tento uživatelský portál hello můžete připojit toohello SDK webové služby Azure Multi-Factor Authentication přes protokol SSL.
* Ujistěte se, že hello uživatelský portál, můžete ověřovat toohello SDK webové služby Azure Multi-Factor Authentication pomocí pověření hello účtu služby, skupiny zabezpečení "PhoneFactor Admins" hello. Tento účet služby a skupina by měla existovat ve službě Active Directory, pokud hello Azure Multi-Factor Authentication Server běží na serveru připojeném k doméně. Tento účet služby a skupina existují místně na serveru Azure Multi-Factor Authentication Server hello Pokud není připojený k tooa domény.

Instalace uživatelského portálu hello na jiném serveru než hello Azure Multi-Factor Authentication Server vyžaduje hello následující kroky:

1. **Na hello MFA Server**, procházet toohello instalační cestu (Příklad: C:\Program Files\Multi-Factor Authentication Server) a kopírovat soubor hello **MultiFactorAuthenticationUserPortalSetup64** tooa umístění přístupné toohello internetovém serveru, kde bude nainstalovat.
2. **Na webovém serveru Internetové hello**, spusťte instalační soubor hello MultiFactorAuthenticationUserPortalSetup64 jako správce, v případě potřeby změňte hello lokality a pokud byste chtěli změnit hello virtuální adresář tooa krátký název.
3. Vytvořit vazbu webu toohello certifikát SSL ve službě IIS.

   > [!NOTE]
   > Tento certifikát je obvykle veřejně podepsaný certifikát SSL.

4. Procházet příliš**C:\inetpub\wwwroot\MultiFactorAuth**
5. Upravit soubor Web.Config hello v poznámkovém bloku

    * Najít klíč hello **"USE_WEB_SERVICE_SDK"** a změňte **hodnota = "false"** příliš**hodnota = true.**
    * Najít klíč hello **"Klíče WEB_SERVICE_SDK_AUTHENTICATION_USERNAME"** a změňte **hodnota = ""** příliš**hodnota = "Doména\uživatel"** kde doména\uživatel je účtu služby, který je součástí Skupina "PhoneFactor Admins".
    * Najít klíč hello **"WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD"** a změňte **hodnota = ""** příliš**hodnota = "Password"** kde heslo je heslo hello hello služby Účet zadaný v předchozím řádku hello.
    * Najít hodnotu hello **https://www.contoso.com/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx** a změňte adresu URL toohello tento zástupný symbol SDK adresa URL webové služby jsme nainstalovali v kroku 2.
    * Zavřete poznámkový blok a uložte soubor Web.Config hello.

6. Z libovolného počítače otevřete webový prohlížeč a přejděte toohello adresu URL, kam se nainstaloval hello user portal (Příklad: https://mfa.contoso.com/MultiFactorAuth). Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.

Pokud máte dotazy týkající se konfigurace certifikátu protokolu SSL na server služby IIS, najdete v článku hello [jak tooSet protokolu SSL ve službě IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

## <a name="configure-user-portal-settings-in-hello-azure-multi-factor-authentication-server"></a>Konfigurace nastavení uživatelského portálu v Azure Multi-Factor Authentication Server hello

Nyní je nainstalován portál uživatele této hello, je nutné toowork Azure Multi-Factor Authentication Server hello tooconfigure hello portálu.

1. V konzole Azure Multi-Factor Authentication Server hello klikněte hello **portálu User Portal** ikonu. Na kartě nastavení hello, zadejte adresu URL hello toohello portálu user portal v hello **adresa URL uživatelského portálu** textové pole. Pokud je zapnutá funkce e-mailu, je tato adresa URL součástí hello e-mailů, které se odesílají toousers, pokud jsou importované do hello Azure Multi-Factor Authentication Server.
2. Zvolte hello nastavení, které chcete toouse v hello portálu User Portal. Například, pokud mohou uživatelé toochoose své metody ověřování, ujistěte se, který **povolit uživatelům tooselect metoda** je zaškrtnuto, společně s hello metody můžete vybírat.
3. Definovat, kdo by měl být správci v hello **správci** kartě. Můžete vytvořit pomocí zaškrtávacích políček hello a rozevíracích seznamů v oknech, přidat či upravit hello granulární oprávnění správce.

Volitelná konfigurace:
- **Bezpečnostní otázky** -definovat schválené bezpečnostní otázky pro vaše prostředí a hello jazyk, se objeví v.
- **Předané relace** – Můžete konfigurovat integraci portálu User Portal s webem na základě formuláře, který používá MFA.
- **Důvěryhodné IP adresy** -povolit uživatelům tooskip MFA při ověřování ze seznamu Důvěryhodné IP adresy nebo rozsahy.

![Konfigurace portálu User Portal pro MFA Server](./media/multi-factor-authentication-get-started-portal/config.png)

Server Azure Multi-Factor Authentication poskytuje několik možností pro portál user portal hello. Hello následující tabulka obsahuje seznam těchto možností a vysvětlení jejich použití.

| Nastavení uživatelského portálu | Popis |
|:--- |:--- |
| Adresa URL portálu User Portal | Zadejte adresu URL hello kde hello portál hostuje. |
| Primární ověření | Při přihlášení toohello portál, zadejte typ hello toouse ověřování. Ověření Windows, Radius nebo LDAP. |
| Povolit uživatelům toolog v | Povolte uživatelům tooenter uživatelské jméno a heslo na hello přihlašovací stránce portálu User portal hello. Pokud tuto možnost nevyberete, jsou zobrazena šedě hello polí. |
| Povolit zápis uživatele | Povolit uživatele tooenroll ověřování službou Multi-Factor Authentication provedením obrazovku tooa nastavení, která ho požádá o další informace, jako je telefonní číslo. Výzva týkající se záložního telefonu umožní uživatelům toospecify záložní telefonní číslo. Výzva pro třetí strany tokenu OATH umožňuje uživatelům toospecify token OATH třetí strany. |
| Povolit uživatelům tooinitiate jednorázové přihlášení | Povolit uživatelům tooinitiate jednorázové přihlášení. Pokud uživatel nastaví tato možnost, bude trvat vliv hello při příštím přihlášení uživatele hello. Výzva k přihlášení uživatele hello sekund poskytuje pole, takže můžou změnit hello výchozí hodnoty 300 sekund. V opačném hello jednorázové přihlášení je vhodný pouze na 300 sekund. |
| Povolit uživatelům tooselect – metoda | Povolit uživatelům toospecify primární metodu kontaktu. Tento způsob může být telefonní hovor, zpráva SMS, mobilní aplikace nebo token OATH. |
| Povolit uživatelům tooselect jazyk | Povolit uživatelům toochange hello jazyk, který se používá pro hello telefonní hovor, textová zpráva, mobilní aplikace nebo tokenu OATH. |
| Povolit uživatelům tooactivate mobilní aplikace | Povolit uživatelům toogenerate k aktivační kód toocomplete hello procesu aktivace mobilní aplikace používaná u serveru hello.  Můžete také nastavit hello počet zařízení, které jsou aktivací aplikace hello na, mezi 1 a 10. |
| V nouzové situaci použít bezpečnostní otázky | Povolí bezpečnostní otázky, pokud se nezdaří dvoustupňové ověřování. Můžete zadat číslo hello bezpečnostních otázek, které je potřeba správně zodpovědět. |
| Povolit tokenu OATH třetí strany tooassociate uživatelů | Povolit uživatelům toospecify token OATH třetí strany. |
| V nouzové situaci použít token OATH | Povolit pro použití hello token OATH v případě, že dvoustupňové ověření se nepovedlo úspěšně dokončit. Můžete také zadat časový limit relace hello v minutách. |
| Povolit protokolování | Povolte protokolování na portálu user portal hello. Hello soubory protokolu jsou umístěné na: C:\Program Files\Multi-Factor Authentication Server\Logs. |

Tato nastavení budou viditelné toohello uživatele na portálu hello, jakmile jsou povoleny a jsou podepsané toohello uživatelského portálu.

![Nastavení uživatelského portálu](./media/multi-factor-authentication-get-started-portal/portalsettings.png)

### <a name="self-service-user-enrollment"></a>Samoobslužná registrace uživatele

Pokud mají vaši uživatelé toosign a zaregistrovat, je nutné vybrat hello **povolit uživatelům toolog v** a **povolit zápis uživatele** možnosti kartě nastavení hello. Mějte na paměti, že hello nastavení, které vyberete ovlivnit hello přihlášení činnost koncového uživatele.

Například pokud se uživatel přihlásí v toohello user portal pro hello poprvé, budou se pak provádějí toohello stránka Azure Multi-Factor Authentication – nastavení uživatele. V závislosti na tom, jak jste nakonfigurovali ověřování Azure Multi-Factor Authentication, uživatel hello může být schopný tooselect metodu ověření.

Pokud vyberte metodu ověřování hlasový hovor hello nebo byly předem nakonfigurovaná toouse dané metody, stránku hello vyzve uživatele tooenter hello svého primárního telefonního čísla a případně linky. Také může být povoleno tooenter telefonní číslo záložního telefonu.

![Registrace primárního a záložního telefonního čísla](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Pokud uživatel hello požadované toouse kódu PIN při ověření, stránku hello zobrazí výzvu toocreate kód PIN. Po zadání jejich telefonních čísel a čísla PIN (pokud existuje), hello uživatel klikne hello **zavolat mi nyní tooAuthenticate** tlačítko. Provede Azure Multi-Factor Authentication telefonní hovor ověření toohello primární telefonní číslo uživatele. Hello uživatel musí odpovědět hello telefonního hovoru a zadat PIN (pokud existuje) a stiskněte tlačítko # toomove na toohello dalšímu kroku vlastního zápisu procesu hello.

Pokud uživatel hello vybere metoda ověření hello textová zpráva nebo byl předem nakonfigurovaná toouse dané metody, vyzve k stránku hello hello uživatele pro své mobilní telefonní číslo. Pokud uživatel hello požadované toouse kódu PIN při ověření, stránku hello také zobrazí výzvu tooenter kód PIN.  Po zadání svého telefonního čísla a kódu PIN (pokud existuje), hello uživatel klikne hello **mi nyní zprávu SMS tooAuthenticate** tlačítko. Azure Multi-Factor Authentication provede služby SMS ověření toohello mobilní telefon uživatele. Hello uživatel obdrží hello textovou zprávu s jednoho-bránu-heslem (OTP) a potom zprávu odpovědi toohello s tímto OTP a číslem PIN (pokud existuje).

![SMS uživatelského portálu](./media/multi-factor-authentication-get-started-portal/text.png)

Pokud uživatel hello vybere metoda ověření mobilní aplikace hello, stránku hello vyzve hello uživatele tooinstall hello Microsoft Authenticator aplikace na jejich zařízení a vygenerování aktivačního kódu. Po instalaci aplikace hello, hello uživatel kliknutím na tlačítko Generovat aktivační kód hello.

> [!NOTE]
> aplikaci Microsoft Authenticator hello toouse, hello uživatele musíte povolit nabízená oznámení pro jejich zařízení.

Hello stránce se pak zobrazí aktivační kód a adresu URL a čárový. Pokud uživatel hello požadované toouse kódu PIN při ověření, stránku hello kromě zobrazí výzvu tooenter kód PIN. Hello uživatel zadá hello aktivační kód a adresu URL do aplikace Microsoft Authenticator hello nebo používá hello čárový kód skener tooscan hello obrázek čárového kódu a kliknutím na tlačítko aktivovat hello.

Po dokončení aktivace hello hello uživatel klikne hello **ověřit mě nyní** tlačítko. Azure Multi-Factor Authentication provede ověření uživatele toohello mobilní aplikace. Hello uživatel musí zadat PIN (pokud existuje) a stiskněte tlačítko Ověřit hello v jejich toomove mobilní aplikace na toohello dalšímu kroku vlastního zápisu procesu hello.

Pokud správci hello nakonfigurovali hello Azure Multi-Factor Authentication Server toocollect bezpečnostní otázky a odpovědi, hello uživatele je nutno stránky toohello bezpečnostní otázky. Hello uživatel musí vybrat čtyři bezpečnostní otázky a zadat odpovědi tootheir vybrané otázky.

![Bezpečnostní otázky uživatelského portálu](./media/multi-factor-authentication-get-started-portal/secq.png)

Hello samoobslužný zápis je teď dokončený a hello uživatel je přihlášený toohello uživatelského portálu. Uživatelé můžou přihlásit zpět toohello uživatelského portálu v budoucnu toochange hello kdykoli jejich telefonní čísla PIN, metody ověřování a bezpečnostní otázky Pokud je povolena změna své metody jejich správci.

## <a name="next-steps"></a>Další kroky

- [Nasazení hello Azure Multi-Factor Authentication Server webové služby mobilní aplikace](multi-factor-authentication-get-started-server-webservice.md)
