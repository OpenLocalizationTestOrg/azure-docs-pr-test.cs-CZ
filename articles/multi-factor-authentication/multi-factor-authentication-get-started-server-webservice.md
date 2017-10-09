---
title: "aaaAzure webové služby mobilní aplikace MFA Server | Microsoft Docs"
description: "aplikace Microsoft Authenticator Hello nabízí možnost dalšího ověření out-of-band.  To umožňuje hello MFA server toouse nabízená oznámení toousers."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 6c8d6fcc-70f4-4da4-9610-c76d66635b8b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 4175b68fcbf85ec3fd53d8edf4e07306c75a4c71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Povolení ověření přes mobilní aplikaci se serverem Azure Multi-Factor Authentication

aplikace Microsoft Authenticator Hello nabízí možnost dalšího ověření out-of-band. Namísto automatizovaného telefonního hovoru nebo SMS toohello uživatele při přihlášení, doručí Azure Multi-Factor Authentication aplikaci Microsoft Authenticator toohello oznámení na tablet nebo smartphone uživatele hello. Hello uživatel jednoduše klepne na **ověřte** (nebo zadá kód PIN a klepne na "Ověřit") v hello toocomplete aplikace jejich přihlášení.

Použití mobilní aplikace pro dvojstupňové ověřování se upřednostňuje v případě, že je telefonní příjem nespolehlivý. Pokud používáte aplikace hello jako generátor tokenu OATH, nevyžaduje jakékoli připojení, sítě nebo Internetu.

V závislosti na vašem prostředí, může být vhodné webové služby mobilní aplikace toodeploy hello na hello stejný server jako Server Azure Multi-Factor Authentication nebo na jiném internetového serveru.

## <a name="requirements"></a>Požadavky

aplikace Microsoft Authenticator hello toouse hello vyžadují se následující věci tak, aby hello aplikace mohla úspěšně komunikovat s webovou službou mobilní aplikace:

* Azure Multi-Factor Authentication Server verze 6.0 nebo vyšší
* Instalace webové služby mobilní aplikace na internetovém webovém serveru s [Internetovou informační službou Microsoft® (IIS) IIS 7.x nebo vyšší](http://www.iis.net/)
* Nainstalovaná, zaregistrované a nastavit tooAllowed položku ASP.NET v4.0.30319
* Požadované role služby zahrnují technologie ASP.NET a kompatibilitu metabáze služby IIS 6
* Webová služba mobilní aplikace je přístupná přes veřejnou adresu URL.
* Webová služba mobilní aplikace je zabezpečena certifikátem SSL.
* Nainstalujte hello SDK webové služby Azure Multi-Factor Authentication ve službě IIS 7.x nebo vyšší na hello **stejný server jako hello Azure Multi-Factor Authentication Server**
* Hello SDK webové služby Azure Multi-Factor Authentication je zabezpečená certifikátem SSL.
* Webové služby mobilní aplikace můžete připojit toohello SDK webové služby Azure Multi-Factor Authentication přes protokol SSL
* Webové služby mobilní aplikace, můžete ověřovat toohello Azure MFA Web Service SDK pomocí pověření účtu služby, který je členem skupiny zabezpečení "PhoneFactor Admins" hello hello. Tento účet služby a skupina existují ve službě Active Directory, pokud je hello Azure Multi-Factor Authentication Server na serveru připojeném k doméně. Tento účet služby a skupina existují místně na serveru Azure Multi-Factor Authentication Server hello Pokud není připojený k tooa domény.

## <a name="install-hello-mobile-app-web-service"></a>Instalace webové služby mobilní aplikace hello

Před instalací webové služby mobilní aplikace hello, nezapomeňte hello následující podrobnosti:

* Potřebujete účet služby, který je součástí skupiny PhoneFactor Admins. Tento účet může být stejný jako jeden hello použitý k instalaci portálu User Portal hello hello.
* Je užitečné tooopen webový prohlížeč na webovém serveru Internetové hello a přejděte na adresu URL toohello hello sady SDK webové služby, který byl zadán do souboru web.config hello. Pokud prohlížeč hello můžete získat úspěšně toohello webové služby, se musí výzvu k zadání pověření. Zadejte hello uživatelské jméno a heslo, které jste zadali do souboru web.config hello úplně stejně jako v souboru hello. Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.
* Pokud reverzní proxy server nebo brána firewall se před webovým serverem webové služby mobilní aplikace hello uložený a provádí SSL snižování zátěže, můžete upravit soubor web.config webové služby mobilní aplikace hello, takže hello webové služby mobilní aplikace mohly používat protokol http místo protokolu https. Šifrování SSL je stále vyžadují z hello mobilní aplikace toohello brány firewall nebo reverznímu proxy serveru. Přidejte následující klíče toohello hello \<appSettings\> části:

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-hello-web-service-sdk"></a>Instalace sady SDK webové služby hello

V obou těchto případech, pokud hello SDK webové služby Azure Multi-Factor Authentication je **není** nainstalovány na hello serveru Azure Multi-Factor Authentication (MFA), dokončení hello následujících kroků.

1. Otevřete konzolu služby Multi-Factor Authentication Server hello.
2. Přejděte toohello **sady SDK webové služby** a vyberte **instalace sady SDK webové služby**.
3. Dokončení hello instalace pomocí výchozího nastavení hello, pokud potřebujete toochange je z nějakého důvodu.
4. Vytvořit vazbu webu toohello certifikát SSL ve službě IIS.

Pokud máte dotazy týkající se konfigurace certifikátu protokolu SSL na server služby IIS, najdete v článku hello [jak tooSet protokolu SSL ve službě IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

Hello sady SDK webové služby musí být zabezpečená certifikátem SSL. Pro tento účel stačí certifikát podepsaný svým držitelem. Importujte certifikát hello do úložiště "Důvěryhodné kořenové certifikační autority" hello hello účtu místního počítače na webovém serveru portálu User Portal hello tak, aby při inicializaci připojení SSL hello důvěřovat certifikátu.

![Nastavení konfigurace MFA Serveru – sada SDK webové služby](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)

### <a name="install-hello-service"></a>Nainstalujte službu hello

1. **Na hello MFA Server**, procházet toohello Instalační cesta.
2. Přejděte toohello složku, kde hello Azure MFA serveru je nainstalovaná hello výchozí je **C:\Program Files\Azure Multi-Factor Authentication**.
3. Vyhledejte soubor instalace hello **MultiFactorAuthenticationMobileAppWebServiceSetup64**. Pokud je hello server **není** internetového kopie hello instalace souboru toohello internetovém serveru.
4. Pokud hello je MFA Server **není** internetového přepínač toohello **internetovém serveru**.
5. Spustit hello **MultiFactorAuthenticationMobileAppWebServiceSetup64** nainstalujte soubor jako správce, změňte hello lokality v případě potřeby a pokud byste chtěli změnit hello virtuální adresář tooa krátký název.
6. Po dokončení instalace hello, procházet příliš**C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService** (nebo odpovídajícího adresáře podle názvu virtuálního adresáře hello) a upravte soubor Web.Config hello.

   * Najít klíč hello **"Klíče WEB_SERVICE_SDK_AUTHENTICATION_USERNAME"** a změňte **hodnota = ""** příliš**hodnota = "Doména\uživatel"** kde doména\uživatel je účtu služby, který je součástí Skupina "PhoneFactor Admins".
   * Najít klíč hello **"WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD"** a změňte **hodnota = ""** příliš**hodnota = "Password"** kde heslo je heslo hello hello služby Účet zadaný v předchozím řádku hello.
   * Najde hello **nastavení pfMobile App Web Service_pfwssdk_PfWsSdk** nastavení a změňte hodnotu hello z **http://localhost: 4898/pfwssdk.asmx** toohello adresa URL sady SDK webové služby (Příklad: https://mfa.contoso.com/ MultiFactorAuthWebServiceSdk/PfWsSdk.asmx).
   * Zavřete poznámkový blok a uložte soubor Web.Config hello.

   > [!NOTE]
   > Vzhledem k tomu, že je pro toto připojení používá protokol SSL, musíte odkázat hello sadu Web Service SDK pomocí **plně kvalifikovaný název domény (FQDN)** a **není IP adresa**. Hello certifikát SSL by byl vydán pro hello plně kvalifikovaný název domény a hello adresa URL se musí shodovat hello názvem na certifikátu hello.

7. Pokud hello web, který webové služby mobilní aplikace byla nainstalována v části nebyla již byla svázána s veřejně podepsaný certifikát, nainstalujte hello certifikát na hello server, otevřete Správce služby IIS a vazby webu toohello hello certifikátu.
8. Z libovolného počítače otevřete webový prohlížeč a přejděte na adresu URL toohello nainstalovanou webové služby mobilní aplikace (Příklad: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService). Ujistěte se, že se nezobrazí žádná varování nebo chyby týkající se certifikátu.

## <a name="configure-hello-mobile-app-settings-in-hello-azure-multi-factor-authentication-server"></a>Konfigurovat nastavení mobilní aplikace hello v Azure Multi-Factor Authentication Server hello

Teď je nainstalované webové služby mobilní aplikace hello musíte toowork Azure Multi-Factor Authentication Server hello tooconfigure hello portálu.

1. V konzole aplikace Multi-Factor Authentication Server hello klikněte na ikonu portálu pro uživatele hello. Pokud jsou uživatelé toocontrol své metody ověřování, zkontrolujte **mobilní aplikace** na kartě nastavení hello, v části **povolit uživatelům tooselect metoda**. Bez povolení této funkce, koncoví uživatelé jsou požadované toocontact aktivací toocomplete HelpDesk pro hello mobilní aplikace.
2. Zkontrolujte hello **povolit uživatelům tooactivate mobilní aplikace** pole.
3. Zkontrolujte hello **povolit zápis uživatele** pole.
4. Klikněte na tlačítko hello **mobilní aplikace** ikonu.
5. Zadejte adresu URL hello používá s hello virtuálního adresáře, který byl vytvořen při instalaci MultiFactorAuthenticationMobileAppWebServiceSetup64 (Příklad: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService/) v poli hello  **Adresa URL služby Mobile App Web:**.
6. Naplnění hello **název účtu** pole s hello firmy nebo organizace toodisplay název v hello mobilních aplikací pro tento účet.
   ![Konfigurace MFA Serveru – nastavení mobilní aplikace](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)

## <a name="next-steps"></a>Další kroky

- [Pokročilé scénáře se službou Azure Multi-Factor Authentication a sítěmi VPN třetích stran](multi-factor-authentication-advanced-vpn-configurations.md).