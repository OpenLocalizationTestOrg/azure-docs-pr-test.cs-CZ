---
title: "aaaMFA serveru se službou AD FS ve Windows serveru | Microsoft Docs"
description: "Tento článek popisuje, jak tooget pracovat s Azure Multi-Factor Authentication a AD FS v systému Windows Server 2012 R2 a 2016."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 57208068-1e55-45b6-840f-fdcd13723074
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/29/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 656785abcc63a020add765a86670b488a3b84b51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-toowork-with-ad-fs-in-windows-server"></a>Konfigurace serveru Azure Multi-Factor Authentication Server toowork se službou AD FS v systému Windows Server
Pokud pomocí služby Active Directory Federation Services (AD FS) a chcete toosecure cloudové nebo místní prostředky, můžete nakonfigurovat toowork Azure Multi-Factor Authentication Server se službou AD FS. Tato konfigurace aktivuje u citlivých koncových bodů dvoustupňové ověřování.

V tomto článku se podíváme na to, jak používat Azure Multi-Factor Authentication Server s AD FS v systému Windows Server 2012 R2 nebo Windows Server 2016. Další informace najdete v tématu o příliš[zabezpečený cloud a místní prostředky pomocí serveru Azure Multi-Factor Authentication Server se službou AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md).

## <a name="secure-windows-server-ad-fs-with-azure-multi-factor-authentication-server"></a>Zabezpečení Windows Serveru AD FS pomocí Azure Multi-Factor Authentication Serveru
Při instalaci Azure Multi-Factor Authentication serveru máte hello následující možnosti:

* Nainstalovat Azure Multi-Factor Authentication Server lokálně na hello stejném serveru jako AD FS
* Nainstalujte Azure Multi-Factor Authentication adaptér hello místně na hello serveru služby AD FS a pak nainstalujte aplikace Multi-Factor Authentication Server na jiném počítači

Než začnete, nezapomeňte hello následující informace:

* Nejste požadované tooinstall Azure Multi-Factor Authentication Server na serveru služby AD FS. Nicméně je nutné nainstalovat adaptér služby Multi-Factor Authentication hello pro službu AD FS na Windows Server 2012 R2 nebo Windows Server 2016, na kterém běží služba AD FS. Hello serveru můžete nainstalovat do jiného počítače, pokud se jedná o podporovanou verzi a samostatně nainstalovat hello adaptér AD FS na federačním serveru služby AD FS. Hello najdete následující postupy toolearn jak tooinstall hello adaptéru samostatně.
* Pokud vaše organizace používá textové zprávy nebo metody ověření mobilní aplikace, hello řetězce definované v nastavení společnosti obsahují zástupný text, <$*název_aplikace*$>. V MFA Serveru verze 7.1 můžete zadat název aplikace, který nahrazuje tento zástupný text. V v7.0 nebo starší není tento zástupný text automaticky nahradit při použití hello AD FS adapter. Pro tyto starší verze odeberte zástupný symbol hello z příslušných řetězců hello při zabezpečení služby AD FS.
* Hello účet, který používáte toosign v musí mít skupiny zabezpečení toocreate práva uživatele ve službě Active Directory.
* Průvodce instalací Hello služby Multi-Factor Authentication AD FS vytvoří skupinu zabezpečení s názvem PhoneFactor Admins ve vaší instanci služby Active Directory. Pak přidá účet služby hello služby AD FS vaší federační služby toothis skupiny. Na vašem řadiči domény ověřte, že hello skupiny PhoneFactor Admins opravdu vytvořila a že účet služby hello AD FS je členem této skupiny. V případě potřeby ručně přidejte hello služby AD FS služby účet toohello skupiny PhoneFactor Admins ve vašem řadiči domény.
* Informace o instalaci hello sady SDK webové služby s hello uživatelského portálu, přečtěte si informace o [nasazení hello uživatelského portálu pro Azure Multi-Factor Authentication Server.](multi-factor-authentication-get-started-portal.md)

### <a name="install-azure-multi-factor-authentication-server-locally-on-hello-ad-fs-server"></a>Nainstalovat Azure Multi-Factor Authentication Server lokálně na serveru služby AD FS hello
1. Stáhněte a nainstalujte Azure Multi-Factor Authentication Server na server AD FS. Informace o instalaci se dočtete v tématu [Začínáme s Azure Multi-Factor Authentication Serverem](multi-factor-authentication-get-started-server.md).
2. V konzole pro správu Azure Multi-Factor Authentication Server hello, klikněte na tlačítko hello **služby AD FS** ikonu. Vyberte možnosti hello **povolit zápis uživatele** a **povolit uživatelům tooselect metoda**.
3. Vyberte jakékoli další možnosti, které byste chtěli toospecify pro vaši organizaci.
4. Klikněte na **Instalovat adaptér služby AD FS**.
   
   <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>

5. Pokud se zobrazí okno hello služby Active Directory, která znamená dvě věci. Je počítač připojený k tooa domény a hello konfigurace služby Active Directory pro zabezpečení komunikace mezi hello AD FS adapterem a hello službou Multi-Factor Authentication je nekompletní. Klikněte na tlačítko **Další** tooautomatically dokončení této konfigurace, nebo vyberte hello **vynechat automatickou konfiguraci služby Active Directory a konfigurovat nastavení ručně** zaškrtávací políčko. Klikněte na **Další**.
6. Pokud se zobrazí místní skupiny windows hello, to znamená dvě věci. Váš počítač není připojený k tooa domény a hello konfigurace místní skupiny pro zabezpečení komunikace mezi hello AD FS adapterem a hello službou Multi-Factor Authentication je nekompletní. Klikněte na tlačítko **Další** tooautomatically dokončení této konfigurace, nebo vyberte hello **vynechat automatickou konfiguraci místní skupiny a konfigurovat nastavení ručně** zaškrtávací políčko. Klikněte na **Další**.
7. V Průvodci hello instalace, klikněte na tlačítko **Další**. Azure Multi-Factor Authentication Server vytvoří hello skupiny PhoneFactor Admins a přidá hello služby AD FS služby účet toohello skupiny PhoneFactor Admins.
   <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Na hello **spustit instalační program** klikněte na tlačítko **Další**.
9. V programu hello služby Multi-Factor Authentication AD FS adapteru klikněte na tlačítko **Další**.
10. Klikněte na tlačítko **Zavřít** po dokončení instalace hello.
11. Když hello adaptér nainstalovaný, je nutné zaregistrovat se službou AD FS. Otevřete prostředí Windows PowerShell a spusťte následující příkaz hello:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
    <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. toouse vaše nově zaregistrovaný adaptér upravit hello globální zásady ověřování ve službě AD FS. V konzole pro správu hello AD FS přejděte toohello **zásady ověřování** uzlu. V hello **Multi-Factor Authentication** klikněte na tlačítko hello **upravit** propojit další toohello **globální nastavení** části. V hello **upravit globální zásady ověřování** vyberte **Multi-Factor Authentication** jako dodatečnou metodu ověřování a pak klikněte na tlačítko **OK**. Hello adaptér se zaregistruje jako WindowsAzureMultiFactorAuthentication. Restartujte službu hello AD FS pro hello registrace tootake vliv.

<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

V tomto okamžiku aplikace Multi-Factor Authentication Server je nastavený toobe toouse poskytovatele další ověřování se službou AD FS.

## <a name="install-a-standalone-instance-of-hello-ad-fs-adapter-by-using-hello-web-service-sdk"></a>Instalace samostatné instance adaptéru hello služby AD FS pomocí hello sady SDK webové služby
1. Hello sady SDK webové služby nainstalujte na server hello, které běží aplikace Multi-Factor Authentication Server.
2. Následující hello kopírování souborů z hello \Program Files\Multi-server toohello directory Multi-Factor Authentication Server, na který budete tooinstall hello AD FS adapteru:
   * MultiFactorAuthenticationAdfsAdapterSetup64.msi
   * Register-MultiFactorAuthenticationAdfsAdapter.ps1
   * Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
   * MultiFactorAuthenticationAdfsAdapter.config
3. Spusťte instalační soubor soubory MultiFactorAuthenticationAdfsAdapterSetup64.msi hello.
4. V programu hello služby Multi-Factor Authentication AD FS adapteru klikněte na tlačítko **Další** toostart hello instalace.
5. Klikněte na tlačítko **Zavřít** po dokončení instalace hello.

## <a name="edit-hello-multifactorauthenticationadfsadapterconfig-file"></a>Upravte soubor MultiFactorAuthenticationAdfsAdapter.config hello
Postupujte podle těchto kroků tooedit hello MultiFactorAuthenticationAdfsAdapter.config souboru:

1. Sada hello **UseWebServiceSdk** uzlu příliš**true**.  
2. Nastavit hodnotu hello **WebServiceSdkUrl** toohello URL hello SDK webové služby Multi-Factor Authentication. Příklad: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx*, kde *certificatename* je hello název vašeho certifikátu.  
3. Upravte skript Register-MultiFactorAuthenticationAdfsAdapter.ps1 hello přidáním *- ConfigurationFilePath &lt;cesta&gt;*  toohello konec hello `Register-AdfsAuthenticationProvider` příkaz, kde  *&lt;cesta&gt;*  je toohello hello úplné cestě k souboru MultiFactorAuthenticationAdfsAdapter.config.

### <a name="configure-hello-web-service-sdk-with-a-username-and-password"></a>Konfigurace hello sady SDK webové služby pomocí uživatelského jména a hesla
Existují dvě možnosti pro konfiguraci hello sady SDK webové služby. Hello je nejdřív pomocí uživatelského jména a hesla, hello druhou je pomocí klientského certifikátu. Postupujte podle těchto kroků pro první možnost hello nebo přeskočit pro hello druhý.  

1. Nastavit hodnotu hello **WebServiceSdkUsername** tooan účet, který je členem skupiny zabezpečení PhoneFactor Admins hello. Použití hello &lt;domény&gt;&#92;&lt; uživatelské jméno&gt; formátu.  
2. Nastavit hodnotu hello **WebServiceSdkPassword** toohello příslušné heslo k účtu.

### <a name="configure-hello-web-service-sdk-with-a-client-certificate"></a>Konfigurace hello sady SDK webové služby pomocí certifikátu klienta
Pokud nechcete, aby toouse uživatelského jména a hesla, postupujte podle těchto kroků tooconfigure hello sady SDK webové služby pomocí certifikátu klienta.

1. Získejte certifikát klienta z certifikační autority pro server hello se systémem hello sady SDK webové služby. Zjistěte, jak příliš[získat klientské certifikáty](https://technet.microsoft.com/library/cc770328.aspx).  
2. Import hello klienta certifikátu toohello osobní úložiště certifikátů místního počítače na hello serveru, který běží hello sady SDK webové služby. Ujistěte se, že veřejný certifikát této certifikační autority hello je v úložišti certifikátů důvěryhodných kořenových certifikátů.  
3. Exportujte hello veřejné a soukromé klíče souboru .pfx tooa certifikátu klienta hello.  
4. Exportujte hello veřejný klíč v souboru .cer tooa ve formátu Base64.  
5. Ve Správci serveru ověřte, zda že je nainstalovaná funkce této hello \Web Server\Security\IIS webového serveru (IIS) ověřování pomocí mapování certifikátu klienta. Pokud nainstalovaná není, vyberte **přidat role a funkce** tooadd tuto funkci.  
6. Ve Správci služby IIS poklikejte na **Editor konfigurací** hello webu, která obsahuje virtuální adresář sady SDK webové služby hello. Je důležité tooselect hello webu, ne hello virtuální adresář.  
7. Přejděte toohello **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** části.  
8. Sada povoleno příliš**true**.  
9. Nastavte položku oneToOneCertificateMappingsEnabled příliš**true**.  
10. Klikněte na tlačítko hello **...**  tlačítko Další toooneToOneMappings a potom klikněte na hello **přidat** odkaz.  
11. Otevřete soubor .cer Base64 hello, které jste dříve exportovali. Odeberte řetězce *-----BEGIN CERTIFICATE-----* a *-----END CERTIFICATE-----* a veškerá zalomení řádků. Zkopírujte výsledný řetězec hello.  
12. Sada certifikát toohello řetězec zkopírovaný v předchozím kroku hello.  
13. Sada povoleno příliš**true**.  
14. Nastavit účet tooan uživatelské jméno, který je členem skupiny zabezpečení PhoneFactor Admins hello. Použití hello &lt;domény&gt;&#92;&lt; uživatelské jméno&gt; formátu.  
15. Nastavit hello heslo toohello příslušné heslo k účtu a pak zavřete Editor konfigurací.  
16. Klikněte na tlačítko hello **použít** odkaz.  
17. V hello virtuální adresář sady SDK webové služby, klikněte dvakrát na **ověřování**.  
18. Ověřte, zda je příliš nastavena zosobnění technologie ASP.NET a základní ověřování**povoleno**, a zda jsou všechny ostatní položky nastavena příliš**zakázané**.  
19. V hello virtuální adresář sady SDK webové služby, klikněte dvakrát na **nastavení SSL**.  
20. Nastavte klientské certifikáty příliš**přijmout**a potom klikněte na **použít**.  
21. Zkopírujte soubor .pfx hello jste exportovali starší toohello serveru se systémem hello adaptér AD FS.  
22. Import hello .pfx souboru toohello osobní úložiště certifikátů místního počítače.  
23. Klikněte pravým tlačítkem a vyberte **spravovat privátní klíče**a potom udělit přístup pro čtení toohello účet používá toosign ve službě toohello AD FS.  
24. Otevření hello klientský certifikát a zkopírujte hello kryptografický otisk z hello **podrobnosti** kartě.  
25. V souboru MultiFactorAuthenticationAdfsAdapter.config hello nastavte **WebServiceSdkCertificateThumbprint** toohello řetězec zkopírovaný v předchozím kroku hello.  

Nakonec tooregister hello adaptéru, spusťte hello \Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 skriptu v prostředí PowerShell. Hello adaptér se zaregistruje jako WindowsAzureMultiFactorAuthentication. Restartujte službu hello AD FS pro hello registrace tootake vliv.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Zabezpečení prostředků Azure AD pomocí služby AD FS
toosecure vaše prostředek v cloudu, nastavit pravidlo deklarací identity, aby Active Directory Federation Services vysílá hello multipleauthn deklarací, když uživatel provede dvoustupňové ověření úspěšně. Tento požadavek je předán na tooAzure AD. Použijte tento postup toowalk prostřednictvím hello kroky:

1. Otevřete správu služby AD FS.
2. Na levé straně hello vyberte **vztahy důvěryhodnosti předávající strany**.
3. Klikněte pravým tlačítkem na **Platforma identit Microsoft Office 365** a vyberte **Upravit pravidla deklarací identity**.

   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)

4. V pravidlech transformace vystavení klikněte na **Přidat pravidlo**.

   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)

5. V průvodci Přidat transformované deklarace identity pravidlo text hello, vyberte **předávat nebo filtrovat příchozí deklarace identity** z hello rozevíracího seznamu a klikněte na tlačítko **Další**.

   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)

6. Pojmenujte pravidlo.
7. Vyberte **odkazy na metody ověřování** jako hello příchozí typ deklarace identity.
8. Vyberte **Předávat všechny hodnoty deklarací identity**.
    ![	Průvodce přidáním pravidla – deklarace identity transformace](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)
9. Klikněte na **Dokončit**. Zavřete konzolu hello AD FS.

## <a name="related-topics"></a>Související témata
Poradce při potížích, najdete v části hello [Azure Multi-Factor Authentication nejčastější dotazy](multi-factor-authentication-faq.md)
