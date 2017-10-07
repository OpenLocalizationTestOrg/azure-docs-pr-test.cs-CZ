---
title: "aaaVPN integraci s Azure MFA pomocí serveru NPS rozšíření | Microsoft Docs"
description: "Tento článek popisuje integraci infrastrukturu sítě VPN s Azure MFA pomocí hello serveru NPS (Network Policy Server) rozšíření pro Microsoft Azure."
services: active-directory
keywords: "Azure MFA integrovat sítě VPN, Azure Active Directory, Network Policy Server rozšíření"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 5e120f7633121385d9cc5d7bec97ecaa1ec7cf19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-vpn-infrastructure-with-azure-multi-factor-authentication-mfa-using-hello-network-policy-server-nps-extension-for-azure"></a>Integrovat infrastrukturu sítě VPN s Azure Multi-Factor Authentication (MFA) pomocí rozšíření hello serveru NPS (Network Policy Server) pro Azure.

## <a name="overview"></a>Přehled

Hello rozšíření služby NPS (Network Policy Server) pro Azure organizacím umožňuje ověření klienta RADIUS Remote Authentication Dial-In User Service () toosafeguard pomocí cloudové [Azure Multi-Factor Authentication (MFA)](multi-factor-authentication-get-started-server-rdg.md), který nabízí dvoustupňové ověřování.

Tento článek obsahuje pokyny k integraci s Azure MFA serveru NPS infrastruktury hello pomocí hello NPS rozšíření pro Azure tooenable zabezpečené dvoustupňové ověřování pro uživatele pokoušející tooconnect tooyour síti pomocí sítě VPN. 

Hello síťové zásady a přístup služby (NPS) poskytuje organizace hello následující možnosti:

* Zadejte centrální umístění pro správu hello a řízení toospecify požadavky sítě, který se připojí, denní jsou připojení povolena, jakou dobu trvání hello připojení a hello úroveň zabezpečení, musí klienti používat tooconnect a tak dále. Místo zadat tyto zásady na každém serveru sítě VPN nebo brány vzdálené plochy (RD), tyto zásady můžete zadat jednou v centrálním umístění. Hello protokolu RADIUS slouží tooprovide hello centralizované ověřování, autorizaci a monitorování (AAA). 
* Stanovit a vynutit zásady stavu klienta Network Access Protection (NAP), které určují, jestli zařízení mají neomezený nebo omezený přístup k prostředkům toonetwork.
* Zadejte znamená tooenforce ověřování a autorizace pro přístup k podporující too802.1x bezdrátové přístupové body a přepínače sítě Ethernet.    

Další informace najdete v tématu [Server NPS (Network Policy Server)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top). 

tooenhance zabezpečení a zajistit vysokou úroveň dodržování předpisů, můžete integrovat organizace NPS s tooensure Azure MFA, která uživatelé používají dvoustupňové ověření toobe schopný připojit toohello virtuálního portu na serveru sítě VPN hello. Pro toobe uživatelům udělen přístup musí zadat, že jejich kombinace uživatelského jména a hesla s informacemi, které hello uživatel má v jejich řízení. Tyto informace musí být důvěryhodný a duplikovaná není snadno, například číslo mobilního telefonu, číslo pevné, aplikace na mobilní zařízení a tak dále.

Předchozí toohello dostupnost hello NPS rozšíření pro Azure, zákazníky, kteří si přáli tooimplement dvoustupňové ověřování pro integraci serveru NPS a prostředí Azure MFA měl tooconfigure a udržovat v místním prostředí hello jako samostatný Server MFA popsané v Brána vzdálené plochy a pomocí protokolu RADIUS serveru Azure Multi-Factor Authentication Server.

dostupnost Hello hello NPS rozšíření pro Azure nyní umožňuje organizacím hello volba toodeploy do řešení vícefaktorového ověřování pro místní nebo cloudové MFA řešení toosecure RADIUS ověření klienta.
 
## <a name="authentication-flow"></a>Tok ověřování
Když se uživatel připojí tooa virtuálního portu na serveru VPN, musí je nejdřív ověřit pomocí různých protokolů, které umožňuje hello používat kombinaci uživatelské jméno a heslo a metody ověřování založené na certifikátu. 

V tooauthenticating přidání a ověření identity uživatelé musí mít hello příslušné oprávnění. V jednoduché implementace jsou přímo na objekty uživatele služby Active Directory hello nastavte tato oprávnění dial-in, které umožňují přístup. 

 ![Vlastnosti uživatele.](./media/nps-extension-vpn/image1.png)

Každý server sítě VPN pro jednoduché implementace, uděluje nebo odmítne přístup na základě zásad, které jsou definované na každém serveru místní VPN.

V implementacích větší a větší škálovatelnost hello zásady této udělit nebo odepřít přístup k síti VPN jsou centralizované na servery RADIUS. V takovém případě hello VPN server funguje jako přístup serveru (klienta RADIUS), který předává žádosti o připojení a server RADIUS tooa zprávy účtu. toohello virtuálního portu tooconnect na hello serveru VPN, uživatelé musí být ověřeny a splňovat podmínky hello definované centrálně na servery RADIUS. 

Když hello NPS rozšíření pro Azure je integrována hello NPS, tok úspěšné ověření hello vypadá takto:

1. server sítě VPN Hello obdrží požadavek na ověření od uživatele sítě VPN, který zahrnuje hello uživatelské jméno a heslo tooconnect tooa prostředků, jako je například relaci vzdálené plochy. 
2. Funguje jako klienta protokolu RADIUS, VPN server převede hello požadavek tooa RADIUS tato zpráva a odešle hello zprávy (heslo je zašifrováno) serveru RADIUS (NPS) toohello nainstalovanou hello rozšíření serveru NPS. 
3. Hello uživatelské jméno a heslo kombinace bude ověřen ve službě Active Directory. Pokud hello uživatelské jméno / heslo je nesprávné, hello RADIUS Server odešle zprávu o odepření přístupu. 
4. Pokud všechny podmínky jako zadané v hello žádost o připojení serveru NPS a zásady sítě jsou splněny (například čas, den nebo skupiny omezeními pro členství vyplývajícími), hello NPS rozšíření aktivuje žádost o sekundárního ověřování s Azure MFA. 
5. Azure MFA komunikuje se službou Azure Active Directory, načte podrobnosti hello uživatele a provede hello sekundárního ověřování metodou hello nakonfiguroval hello uživatel (textová zpráva, mobilní aplikace a tak dále). 
6. Po úspěšné hello ověřovací test MFA Azure MFA komunikuje hello výsledek toohello NPS rozšíření.
7. Po pokusu o připojení hello je ověřen a oprávnění, odešle server NPS hello nainstalovanou hello rozšíření RADIUS s potvrzením přístupu zpráva toohello server VPN (klienta RADIUS).
8. Hello uživateli je udělen přístup toohello virtuálního portu na serveru VPN a vytvoří šifrované tunelové propojení VPN.

## <a name="prerequisites"></a>Požadavky
Tato část podrobně hello předpoklady nezbytné před integrace Azure MFA s hello Brána vzdálené plochy. Než začnete, musíte mít hello následující požadavky splněny.

* Infrastruktura sítě VPN.
* Role Síťové zásady a přístup služby NPS)
* Licence Azure MFA
* Software Windows serveru
* Knihovny
* Azure AD synchronizována s místní AD 
* ID identifikátor GUID služby Azure Active Directory

### <a name="vpn-infrastructure"></a>Infrastruktura sítě VPN.
Tento článek předpokládá, že máte pracovní infrastrukturu sítě VPN pomocí Microsoft Windows Server 2016 na místě a tento server VPN hello není aktuálně nakonfigurovaný tooforward připojení požadavky tooa RADIUS server. V tomto průvodci můžete nakonfigurovat hello VPN infrastruktury toouse centrální server RADIUS.

Pokud nemáte pracovní infrastruktury na místě, můžete rychle vytvořit této infrastruktury podle pokynů hello součástí množství kurzy instalační program VPN, které můžete najít na hello Microsoft a weby třetích stran. 

### <a name="network-policy-and-access-services-nps-role"></a>Role Síťové zásady a přístup služby NPS)

Hello službu role NPS poskytuje funkce klienta a serveru RADIUS hello. Tento článek předpokládá, že jste nainstalovali role serveru NPS hello na členský server nebo řadič domény ve vašem prostředí. Pro konfiguraci sítě VPN v tomto průvodci můžete nakonfigurovat protokolu RADIUS. Instalace role serveru NPS hello na serveru _jiných_ než je na serveru VPN.

Informace o instalaci role serveru NPS hello služby Windows Server 2012 nebo vyšší, najdete v části [nainstalovat Server zásad stavu NAP](https://technet.microsoft.com/library/dd296890.aspx). Zásady NAP (Network Access) je ve Windows serveru 2016 zastaralá. Popis osvědčené postupy pro server NPS, včetně doporučení tooinstall hello NPS na řadiči domény najdete v části [osvědčené postupy pro server NPS](https://technet.microsoft.com/library/cc771746).

### <a name="licenses"></a>Licence

Vyžaduje se licenci pro Azure MFA, která je k dispozici prostřednictvím Azure AD Premium, Enterprise Mobility plus Security (EMS) nebo předplatné vícefaktorového ověřování. Další informace najdete v tématu [jak tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md). Pro účely testování můžete použít zkušební verzi předplatného.

### <a name="software"></a>Software

Hello NPS rozšíření vyžaduje Windows Server 2008 R2 SP1 nebo novějším s nainstalovanou službou role NPS hello. Všechny kroky hello v této příručce se provádí pomocí systému Windows Server 2016.

### <a name="libraries"></a>Knihovny

vyžadují se následující dvě knihovny Hello:

* [Distribuovatelné balíčky Visual C++ pro Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
* _Microsoft Azure Active Directory modul pro prostředí Windows PowerShell verze 1.1.166.0_ nebo vyšší. Nejnovější verze hello a pokyny k instalaci najdete v tématu [Microsoft Azure Active Directory PowerShell modulu verze historie verzí](https://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx).

Tyto knihovny nejsou zabalené s hello NPS rozšíření instalační soubory (verze 0.9.1.2), i přes existující dokumentace, která neurčí jinak. Minimálně je třeba nainstalovat Distribuovatelné balíčky Visual C++ hello pro Visual Studio 2013. Hello Microsoft Azure Active Directory modul pro prostředí Windows PowerShell je nainstalován, pokud ještě není přítomný, prostřednictvím konfigurační skript, které spustíte jako součást procesu instalace hello. Není bez nutnosti tooinstall tento modul předem, pokud již není nainstalována.

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Active Directory synchronizována s místní služby Active Directory 

toouse hello NPS rozšíření místní uživatelé musí být synchronizovaná s Azure Active Directory a povolené pro službu Multi-Factor Authentication. Tato příručka předpokládá, že jsou synchronizována místních uživatelů s Azure Active Directory pomocí služby AD Connect. Pokyny k povolení uživatelů pro MFA jsou uvedeny níže.
Informace o Azure AD connect, najdete v části [integraci místních adresářů se službou Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>ID identifikátor GUID služby Azure Active Directory 
tooinstall hello NPS, musíte tooknow hello GUID hello Azure Active Directory. Pokyny pro hledání hello GUID hello Azure Active Directory najdete v další části hello.

## <a name="configure-radius-for-vpn-connections"></a>Konfigurace protokolu RADIUS pro připojení k síti VPN

Pokud jste nainstalovali role serveru NPS hello na členském serveru, potřebujete tooconfigure tooauthenticate a autorizovat klienta VPN, žádosti o připojení k síti VPN. 

V této části předpokládá, že jste nainstalovali roli serveru zásad sítě hello, ale nenakonfigurovali ho pro použití ve vaší infrastruktuře.

>[!NOTE]
>Pokud už máte pracovní serveru VPN, který používá centralizovaný server RADIUS pro ověřování, můžete tuto část přeskočit.
>

### <a name="register-server-in-active-directory"></a>Registrace serveru ve službě Active Directory
toofunction správně v tomto scénáři, musí hello NPS server toobe zaregistrován ve službě Active Directory.

1. Otevřete správce serveru.
2. Ve Správci serveru klikněte na tlačítko **nástroje**a potom klikněte na **Network Policy Server**. 
3. V konzole hello Network Policy Server, klikněte pravým tlačítkem na **server NPS (místní)**a potom klikněte na **server zaregistrovat ve službě Active Directory**. Klikněte na tlačítko **OK** dvakrát.

 ![Server NPS](./media/nps-extension-vpn/image2.png)

4. Ponechte hello konzoly otevřené pro další postup hello.

### <a name="use-wizard-tooconfigure-radius-server"></a>Server RADIUS tooconfigure použití Průvodce
Můžete použít standardní (založené na průvodci) nebo serveru RADIUS hello tooconfigure možnost pokročilou konfiguraci. V této části se předpokládá použití hello možnosti založené na průvodci standardní konfiguraci hello.

1. V konzole Network Policy Server hello klikněte **server NPS (místní)**.
2. V části standardní konfiguraci, vyberte **Server RADIUS pro vytáčená nebo VPN připojení**a potom klikněte na **konfigurace sítě VPN nebo telefonické**.

 ![Konfigurace sítě VPN](./media/nps-extension-vpn/image3.png)

3. Hello vyberte telefonicky nebo virtuální privátní typ síťového připojení vyberte na stránce, **připojení k virtuální privátní síti**a klikněte na tlačítko **Další**.

 ![Virtuální privátní síť](./media/nps-extension-vpn/image4.png)

4. Na stránce zadejte vytáčená nebo VPN Server hello, klikněte na **přidat**.
5. V hello **klienta RADIUS nové** dialogové okno, zadejte popisný název, zadejte hello rozlišitelný název nebo IP adresu serveru VPN hello a zadejte sdílený tajný heslo. Ujistěte se, toto sdílené tajné heslo dlouhá a složitá. Toto heslo si poznamenejte, budete potřebovat pro kroků v další části hello.

 ![Nový klient protokolu RADIUS](./media/nps-extension-vpn/image5.png)

6. Klikněte na tlačítko **OK**a potom **Další**.
7. Na hello **konfigurovat metody ověřování** přijměte výchozí výběr hello (šifrované ověření verze 2 (MS-CHAPv2) nebo zvolte jinou možnost a klikněte na tlačítko **Další**.

  >[!NOTE]
  >Při konfiguraci protokolu EAP (Extensible Authentication), musíte použít MS CHAPv2 nebo protokolem PEAP. Žádné jiné EAP je podporována.
 
8. Na stránce zadat skupiny uživatelů hello, klikněte na **přidat** a vyberte příslušné skupiny, pokud existuje. Jinak ponechte hello výběr prázdné toogrant přístup tooall uživatele.

 ![Určete skupiny uživatelů](./media/nps-extension-vpn/image7.png)

9. Klikněte na **Další**.
10. Na stránce zadat filtry IP adres hello, klikněte na **Další**.
11. Na stránce zadat nastavení šifrování hello přijmout hello výchozí nastavení a klikněte na tlačítko **Další**.

 ![Zadejte možnost Encryption](./media/nps-extension-vpn/image8.png)

12. Na hello zadejte název sféry, ponechejte pole hello název prázdné, přijměte výchozí nastavení hello a klikněte na tlačítko **Další**.

 ![Zadejte název sféry](./media/nps-extension-vpn/image9.png)

13. Hello dokončení nové telefonické nebo stránku klienti připojení k virtuální privátní síti a protokolu RADIUS, klikněte na tlačítko **Dokončit**.

 ![Dokončení připojení](./media/nps-extension-vpn/image10.png)

### <a name="verify-radius-configuration"></a>Ověření konfigurace RADIUS
Tato část podrobně hello konfigurace, které jste vytvořili pomocí Průvodce hello.

1. Na serveru NPS hello v hello konzoly serveru NPS (místní počítač), rozbalte klientů RADIUS a vyberte **klientů RADIUS**.
2. V podokně podrobností hello klikněte pravým tlačítkem na klienta protokolu RADIUS hello vytvořené pomocí průvodce a klikněte na tlačítko **vlastnosti**. Hello vlastnosti pro vašeho klienta protokolu RADIUS (hello VPN server) by měl být podobné toothose vidíte níže.

 ![Vlastnosti sítě VPN](./media/nps-extension-vpn/image11.png)

3. Klikněte na tlačítko **zrušit**.
4. Na serveru NPS hello v hello konzoly serveru NPS (místní počítač), rozbalte položku **zásady**a vyberte **zásady vyžádání nového připojení**. Měli byste vidět hello připojení k síti VPN zásad, která se podobá následující obrázek hello.

 ![Žádosti o připojení](./media/nps-extension-vpn/image12.png)

5. V části zásady, vyberte **zásady sítě**. Měli byste zásadu připojení virtuální privátní sítě (VPN), která se podobá následující obrázek hello.

 ![Vlastnosti sítě](./media/nps-extension-vpn/image13.png)

## <a name="configure-vpn-server-toouse-radius-authentication"></a>Nakonfigurujte ověřování RADIUS toouse Server sítě VPN
V této části můžete nakonfigurovat ověřování RADIUS toouse server VPN hello. V této části se předpokládá, že máte funkční konfigurace serveru VPN, ale nenakonfigurovali ověřování RADIUS toouse server VPN hello. Po dokončení konfigurace serveru VPN hello, potvrďte, že konfiguraci funguje podle očekávání.

>[!NOTE]
>Pokud již máte funkční konfigurace serveru VPN, který používá ověřování pomocí protokolu RADIUS, můžete tuto část přeskočit.
>

### <a name="configure-authentication-provider"></a>Konfigurace zprostředkovatele ověřování
1. Na serveru VPN hello otevřete Správce serveru.
2. Ve Správci serveru klikněte na tlačítko **nástroje**a potom **směrování a vzdálený přístup**.
3. V konzole Směrování a vzdálený přístup hello, klikněte pravým tlačítkem na  **\[název serveru\] (místní)**a potom klikněte na **vlastnosti**.

 ![Směrování a vzdálený přístup](./media/nps-extension-vpn/image14.png)
 
4. V hello **[název serveru} (místní) vlastnosti** dialogové okno pole, klikněte na tlačítko hello **zabezpečení** kartě. 
5. Na hello **zabezpečení** karta, v rámci zprostředkovatele ověřování, klikněte na tlačítko **ověřování pomocí protokolu RADIUS**a potom **konfigurace**.

 ![Ověřování RADIUS](./media/nps-extension-vpn/image15.png)
 
6. V dialogovém okně ověřování pomocí protokolu RADIUS hello, klikněte na tlačítko **přidat**.
7. V hello přidat Server RADIUS v názvu serveru, přidejte hello název nebo hello IP adresa serveru RADIUS hello, které jste nakonfigurovali v předchozím oddílu hello.
8. V sdílený tajný klíč, klikněte na **změnu** a přidejte hello sdílené tajné heslo, které jste vytvořili a poznamenali dříve.
9. V časový limit (v sekundách), změňte hodnotu tooa hello hodnotu mezi **30** a **60**. Toto je nezbytné tooallow dostatek času toocomplete hello druhý ověřovací faktor.
 
 ![Přidat RADIUS Server](./media/nps-extension-vpn/image16.png)
 
10. Klikněte na tlačítko **OK** dokud nedokončíte zavřít všechna dialogová okna.

### <a name="test-vpn-connectivity"></a>Test připojení VPN
V této části potvrdíte, že klient VPN hello ověří a autorizuje serverem RADIUS hello pokusíte-li tooconnect tooVPN virtuálního portu. V této části se předpokládá, že používáte Windows 10 jako klient VPN. 

>[!NOTE]
>Pokud už nakonfigurované server VPN toohello tooconnect klienta VPN a uložení hello nastavení, můžete přeskočit související tooconfiguring hello kroky a ukládání objekt připojení VPN.
>

1. Na klientském počítači sítě VPN, klikněte na tlačítko **spustit**a potom **nastavení** (ozubené kolečko ikona).
2. V okně nastavení klikněte na tlačítko **síť a Internet**.
3. Klikněte na tlačítko **VPN**.
4. Klikněte na tlačítko **přidat připojení VPN**.
5. V přidat připojení VPN, zadejte Windows (předdefinovaná) jako hello poskytovatel VPN a pak dokončení hello zbývající pole, podle potřeby a klikněte na tlačítko **Uložit**. 

 ![Přidat připojení VPN](./media/nps-extension-vpn/image17.png)
 
6. Otevřete hello **Centrum sítí a sdílení** v Ovládacích panelech.
7. Klikněte na tlačítko **změnit nastavení adaptéru**.

 ![Změnit nastavení adaptéru](./media/nps-extension-vpn/image18.png)

8. Klikněte pravým tlačítkem na připojení sítě VPN hello a klikněte na položku Vlastnosti. 

 ![Vlastnosti sítě VPN](./media/nps-extension-vpn/image19.png)

9. V hello VPN dialogové okno Vlastnosti, klikněte na tlačítko hello **zabezpečení** kartě. 
10. Na kartě zabezpečení hello zkontrolujte pouze **Microsoft CHAP Version 2 (MS-CHAP v2)** je vybrána a klikněte na tlačítko OK.

 ![Povolit protokoly](./media/nps-extension-vpn/image20.png)

11. Klikněte pravým tlačítkem na připojení k síti VPN hello a klikněte na tlačítko **Connect**.
12. Na stránce nastavení hello, klikněte na tlačítko **Connect**.

Úspěšné připojení se zobrazí v hello protokolu zabezpečení na serveru RADIUS hello jako události 6272 ID, jak je uvedeno níže.

 ![Vlastnosti události](./media/nps-extension-vpn/image21.png)

## <a name="troubleshoot-guide"></a>Řešení potíží s Průvodce
Předpokládejme, že konfiguraci VPN fungovala předtím, než jste nakonfigurovali hello VPN serveru toouse centralizovaný server RADIUS pro ověřování a autorizaci. V takovém případě je pravděpodobné, že hello problém může být způsobeno nesprávnou konfiguraci hello RADIUS Server nebo hello použití neplatného uživatelského jména nebo hesla. Například pokud používáte alternativní přípona UPN hello hello uživatelské jméno, hello pokus o přihlášení může selhat (byste měli používat stejný název účtu pro dosažení co nejlepších výsledků hello). 

Tyto problémy toostart ideální místo je protokolů událostí zabezpečení hello tooexamine na tootroubleshoot hello serveru RADIUS. hledání toosave doba pro události, lze použít hello na základě rolí síťové zásady a přístup k serveru vlastní zobrazení v prohlížeči událostí, jak je vidět níže. ID události 6273 označuje událostí, kde hello Network Policy Server byl odepřen přístup tooa uživatele. 

 ![Prohlížeč událostí](./media/nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a>Konfigurace služby Multi-Factor Authentication
Tato část obsahuje pokyny pro povolení uživatelů pro MFA a pro nastavení účtů pro dvoustupňové ověření. 

### <a name="enable-multi-factor-authentication"></a>Povolení vícefaktorového ověřování
V této části je povolit účty Azure AD pro MFA. Použití hello **portálu classic** tooenable uživatelů pro MFA. 

1. Otevřete prohlížeč a přejděte příliš[https://manage.windowsazure.com](https://manage.windowsazure.com). 
2. Přihlaste se jako správce hello.
3. Hello portálu, v levém navigačním hello, klikněte na tlačítko **služby ACTIVE DIRECTORY**.

 ![Výchozí adresář](./media/nps-extension-vpn/image23.png)

4. Hello název sloupce, klikněte na tlačítko **výchozí adresář** (nebo jiného adresáře, podle potřeby).
5. Na stránce hello rychlý Start, klikněte na **konfigurace**.

 ![Konfigurace výchozích](./media/nps-extension-vpn/image24.png)

6. Na stránce konfigurace hello, posuňte se dolů a v části hello vícefaktorového ověřování, klikněte na tlačítko **spravovat nastavení služby**.

 ![Spravovat nastavení vícefaktorového ověřování](./media/nps-extension-vpn/image25.png)
 
7. Na stránce služby Multi-Factor authentication hello zkontrolujte hello výchozího nastavení a pak klikněte na tlačítko **uživatelé**. 

 ![Uživatelé vícefaktorového ověřování](./media/nps-extension-vpn/image26.png)
 
8. Na stránce hello uživatele, vyberte uživatele hello chcete tooenable pro MFA a pak klikněte na tlačítko **povolit**.

 ![Vlastnosti](./media/nps-extension-vpn/image27.png)
 
9. Po zobrazení výzvy klikněte na tlačítko **povolit Multi-Factor auth**.

 ![Povolit MFA](./media/nps-extension-vpn/image28.png)
 
10. Klikněte na **Zavřít**. 
11. Obnovte stránku hello. Hello stav MFA je změněné tooEnabled.

Informace o tom najdete v části Uživatelé tooenable pro službu Multi-Factor Authentication, [Začínáme s Azure Multi-Factor Authentication v cloudu hello](multi-factor-authentication-get-started-cloud.md). 

### <a name="configure-accounts-for-two-step-verification"></a>Konfigurace účtů pro dvoustupňové ověření
Jakmile účet povolen pro MFA, nebudou se uživatelé moct toosign v tooresources řídí podle zásad vícefaktorového ověřování hello, dokud úspěšně nakonfigurovali toouse důvěryhodné zařízení pro druhý faktor ověřování hello nutnosti použít dvoustupňové ověřování.

V této části nakonfigurujete důvěryhodné zařízení pro použití s dvoustupňové ověřování. Existuje několik možností pro tooconfigure je k dispozici tyto včetně hello následující:

* **Mobilní aplikace**. Nainstalujte aplikaci Microsoft Authenticator hello zařízení Windows Phone, Android nebo iOS. V závislosti na zásadách vaší organizace, jsou aplikace hello požadované toouse v jednom ze dvou režimů: přijímání oznámení pro ověření (oznámení se posune tooyour zařízení) nebo použít ověřovací kód (je nutné tooenter ověření kódu, který aktualizuje každých 30 sekund). 
* **Mobilní telefonní hovor nebo textovou**. Můžete buď zobrazí automatizovaného telefonního hovoru nebo textové zprávy. S možností hello telefonní hovor odpovězte hello volání a stiskněte klávesu tooauthenticate znak # hello. S možností text hello můžete buď odpovědět toohello textová zpráva nebo zadejte ověřovací kód hello do hello přihlašovací rozhraní.
* **Office telefonní hovor**. Tento proces je hello stejné, jako je popsán pro automatické telefonické hovory výše.

Postupujte podle těchto pokynů pro nastavení zařízení toouse hello mobilní aplikace tooreceive nabízených oznámení pro ověření.

1. Přihlaste se příliš[https://aka.ms/mfasetup](https://aka.ms/mfasetup) nebo v jakékoli lokalitě, jako [https://portal.azure.com](https://portal.azure.com), kde požadované tooauthenticate pomocí svých přihlašovacích údajů povolit MFA. 
2. Po přihlášení pomocí uživatelského jména a hesla, se zobrazí obrazovka, která zobrazí výzvu tooset hello účet pro další ověření zabezpečení.

 ![Další zabezpečení](./media/nps-extension-vpn/image29.png)

3. Klikněte na tlačítko **nyní nastavit**.
4. Na stránce pro hello další bezpečnostní ověření vyberte typ kontaktu (telefon pro ověření, telefonní číslo do kanceláře nebo mobilní aplikace). Poté vyberte zemi nebo oblast a vyberte metodu. Metoda Hello se liší podle kontaktní typ, který jste vybrali. Například pokud se rozhodnete mobilní aplikace, můžete vybrat zda tooreceive oznámení pro ověření nebo toouse ověřovací kód. Hello kroky, které následují předpokládá můžete zvolit **mobilní aplikace** jako hello typ kontaktu.

 ![Ověření telefonu](./media/nps-extension-vpn/image30.png)

5. Vyberte mobilní aplikace, klikněte na **dostávat oznámení pro ověření**a potom **nastavit**. 

 ![Ověření mobilní aplikace](./media/nps-extension-vpn/image31.png)
 
6. Pokud jste tak ještě neučinili, nainstalujte hello authenticator mobilní aplikaci na vašem zařízení. 
7. Postupujte podle pokynů hello v hello tooscan mobilní aplikace hello uvedené čárový kód nebo hello informace zadat ručně a pak klikněte na tlačítko **provádí**.

 ![Konfigurace mobilních aplikací](./media/nps-extension-vpn/image32.png)

8. Na stránce ověření hello dodatečné zabezpečení, klikněte na **kontaktujte mi** a zařízení tooyour toonotification odeslané odpovědi.
9. Na stránce pro hello další bezpečnostní ověření, zadejte číslo kontaktu v případě, že ztratit přístup toohello mobilní aplikace a klikněte na **Další**.

 ![Číslo mobilního telefonu](./media/nps-extension-vpn/image33.png)
 
10. Na hello dalšího ověření zabezpečení, klikněte na tlačítko **provádí**.

Hello zařízení je teď nakonfigurovaná tooprovide druhé metody ověřování. Informace o nastavení účtů pro dvoustupňové ověření najdete v tématu [nastavit účtu pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user-first-time.md).

## <a name="install-and-configure-nps-extension"></a>Instalace a konfigurace serveru NPS rozšíření

Tato část obsahuje pokyny pro konfiguraci sítě VPN toouse Azure MFA pro ověřování klientů se hello serveru VPN.

Po instalaci a konfiguraci serveru NPS rozšíření hello, všechny ověřování klienta na základě protokolu RADIUS, které je tento server zpracovávat je požadovaná toouse Azure MFA. Není-li všichni uživatelé sítě VPN jsou zaregistrované v Azure MFA, můžete nastavit jiný protokolu RADIUS serveru tooauthenticate uživatelů, kteří nejsou nakonfigurované toouse vícefaktorového ověřování. Nebo můžete vytvořit položku registru, která umožňuje uživatelům němuž tooprovide druhý ověřovací faktor, pouze v případě, že jsou zaregistrovaná ve službě vícefaktorového ověřování. 

Vytvořte novou řetězcovou hodnotu s názvem _REQUIRE_USER_MATCH v HKLM\SOFTWARE\Microsoft\AzureMfa_a nastavte tooTRUE hello hodnotu nebo hodnotu NEPRAVDA. 

 ![Vyžadovat porovnání uživatele](./media/nps-extension-vpn/image34.png)
 
Pokud hodnota hello je sada tooTRUE nebo není nastavený, jsou všechny požadavky na ověření ověřovací test MFA tooan subjektu. Pokud tooFALSE je nastavena hodnota hello, MFA problémy jsou vydávány pouze toousers, které jsou zaregistrované v MFA. Používejte pouze hello FALSE nastavení v provozním prostředí nebo při testování během po dobu registrace.

### <a name="acquire-azure-active-directory-guid-id"></a>Získání ID identifikátor GUID služby Azure Active Directory

Jako součást konfigurace hello hello rozšíření serveru NPS potřebujete přihlašovací údaje správce toosupply a hello Azure Active Directory ID pro vašeho tenanta Azure AD. Hello kroky ukazují, jak ID tooget hello klienta.

1. Přihlaste se toohello portál Azure na [https://portal.azure.com](https://portal.azure.com) jako globální správce hello hello Azure klienta.
2. V levé navigační hello, klikněte na tlačítko hello **Azure Active Directory** ikonu.
3. Klikněte na **Vlastnosti**.
4. toocopy ID adresáře toohello schránky, vyberte hello **kopie** ikonu.
 
 ![ID adresáře](./media/nps-extension-vpn/image35.png)

### <a name="install-hello-nps-extension"></a>Instalace serveru NPS rozšíření hello
Hello rozšíření NPS musí toobe nainstalovat na server, který má hello síťové zásady a nainstalovanou rolí služby přístupu (NPS) a která slouží jako server RADIUS hello při návrhu. Neinstalujte hello NPS rozšíření na vašem serveru vzdálené plochy.

1. Stáhněte si rozšíření NPS hello z [https://aka.ms/npsmfa](https://aka.ms/npsmfa). 
2. Kopírování hello instalační program spustitelný soubor (NpsExtnForAzureMfaInstaller.exe) toohello serveru NPS.
3. Na serveru NPS hello, dvakrát klikněte na **NpsExtnForAzureMfaInstaller.exe**. Pokud se zobrazí výzva, klikněte na tlačítko **spustit**.
4. V hello NPS rozšíření pro dialogové okno Azure MFA, zkontrolujte hello licenční podmínky pro software, zkontrolujte **souhlasím toohello licenční podmínky a ujednání**a klikněte na tlačítko **nainstalovat**.

 ![Rozšíření serveru NPS](./media/nps-extension-vpn/image36.png)
 
5. V hello NPS rozšíření pro dialogové okno Azure MFA, klikněte na **Zavřít**.  

 ![Instalační program úspěšně](./media/nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-hello-nps-extension-using-a-powershell-script"></a>Konfigurace certifikátů pro použití s příponou hello NPS pomocí skriptu prostředí PowerShell
tooensure zabezpečení komunikace a záruku, musíte tooconfigure certifikáty pro použití rozšířením hello serveru NPS. součásti serveru NPS Hello zahrnují skript prostředí Windows PowerShell, který konfiguruje certifikát podepsaný svým držitelem pro použití se serverem NPS. 

Hello skript provede hello následující akce:

* Vytvoří certifikát podepsaný svým držitelem
* Přidruží veřejný klíč certifikátu tooservice hlavní na Azure AD
* Úložiště hello certifikátu v úložišti místního počítače hello
* Uděluje přístup k certifikátu toohello privátní klíče toohello síťového uživatele
* Restartuje službu serveru zásad sítě

Pokud chcete toouse vlastní certifikáty, vyžadují tooassociate hello veřejné váš certifikát toohello služby zásadu na Azure AD a tak dále.
skript hello toouse, zadejte hello rozšíření pomocí svých přihlašovacích údajů pro správu Azure Active Directory a hello ID klienta Azure Active Directory jste zkopírovali dříve. Spusťte skript hello na každý server NPS instalujete rozšíření NPS hello.

1. Otevřete řádku prostředí Windows PowerShell pro správu.
2. Zadejte v příkazovém prostředí PowerShell hello _cd 'c:\Program Files\Microsoft\AzureMfa\Config'_a stiskněte klávesu **ENTER**.
3. Typ _.\AzureMfsNpsExtnConfigSetup.ps1_a stiskněte klávesu **ENTER**. 
 * Hello skript kontroluje toosee, pokud je nainstalován modul Azure Active Directory PowerShell hello. Pokud není nainstalována, skript hello nainstaluje modul hello za vás.
 
 ![PowerShell](./media/nps-extension-vpn/image38.png)
 
4. Po hello skript ověřuje hello instalace modulu prostředí PowerShell text hello, zobrazí dialogové okno modul Azure Active Directory PowerShell hello. V dialogovém okně hello, zadejte přihlašovací údaje Správce služby Azure AD a heslo a klikněte na tlačítko **přihlášení**. 
 
 ![Přihlášení k prostředí PowerShell](./media/nps-extension-vpn/image39.png)
 
5. Po zobrazení výzvy, vložte ID klienta hello předtím zkopírovali toohello schránky a stiskněte klávesu **ENTER**. 

 ![ID tenanta](./media/nps-extension-vpn/image40.png)

6. skript Hello vytvoří certifikát podepsaný svým držitelem a provede další změny v konfiguraci. výstup Hello je jako hello obrázku vidíte níže.

 ![Vlastní certifikát podepsaný](./media/nps-extension-vpn/image41.png)

7. Restartujte hello server.
 
### <a name="verify-configuration"></a>Ověření konfigurace
Konfigurace hello tooverify, musíte tooestablish nové připojení VPN se serverem VPN. Po úspěšně zadávat svoje přihlašovací údaje pro primární ověřování, hello připojení k síti VPN vyčká hello sekundárního ověřování toosucceed hello připojení, jak je uvedeno níže. 

 ![Ověření konfigurace](./media/nps-extension-vpn/image42.png)

Pokud jste úspěšně ověřit pomocí metody hello sekundární ověření, které jste dříve nakonfigurovali v Azure MFA, jste připojené toohello prostředků. Ale hello sekundárního ověřování neproběhne úspěšně, je odepřen přístup tooresource. 

V příkladu hello níže hello ověřovací aplikace na Windows phone je použité tooprovide hello sekundární ověřování.

 ![Ověření účtu.](./media/nps-extension-vpn/image43.png)

Jakmile jste úspěšně ověřen pomocí metody sekundární hello, mají přístup toohello virtuálního portu na serveru sítě VPN hello. Protože jste požadované toouse metodu sekundární ověřování pomocí mobilní aplikace na důvěryhodném zařízení, hello protokolu v procesu je ale bezpečnější, než se bude používat jenom uživatelské jméno nebo kombinace hesla.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Zobrazit protokoly Prohlížeče událostí pro události úspěšného přihlášení
tooview hello událostí úspěšného přihlášení v protokolech prohlížeče událostí systému Windows hello, můžete použít následující protokolu o zabezpečení Windows hello tooquery příkaz prostředí Windows PowerShell na serveru NPS hello hello.

tooquery událostí úspěšného přihlášení v protokolech prohlížeče událostí zabezpečení hello, použijte následující příkaz, hello
* _Get-WinEvent. – název protokolu zabezpečení_ | kde {$_.ID - eq '6272'} | FL 

 ![Prohlížeč událostí zabezpečení](./media/nps-extension-vpn/image44.png)
 
Také můžete zobrazit protokol zabezpečení hello nebo hello síťové zásady a přístup ke službě vlastní zobrazení, jak je uvedeno níže:

 ![Přístup k síti zásad](./media/nps-extension-vpn/image45.png)

Na serveru hello nainstalovanou hello NPS rozšíření pro Azure MFA, můžete najít protokoly Prohlížeče událostí aplikace konkrétní toohello rozšíření v **aplikace a služby Logs\Microsoft\AzureMfa**. 

* _Get-WinEvent. – název protokolu zabezpečení_ | kde {$_.ID - eq '6272'} | FL

 ![Počet událostí](./media/nps-extension-vpn/image46.png)

## <a name="troubleshoot-guide"></a>Řešení potíží s Průvodce
Pokud konfigurace hello nefungují podle očekávání, je vhodné místo toostart tootroubleshoot tooverify, který hello uživatele je nakonfigurované toouse Azure MFA. Uživatel hello připojit příliš[https://portal.azure.com](https://portal.azure.com). Když se zobrazí výzva k sekundárního ověřování a můžete úspěšně ověřit, že můžete eliminovat nesprávnou konfiguraci Azure MFA.

Pokud Azure MFA funguje pro hello jiní uživatelé, přečtěte si relevantní protokoly událostí hello. Mezi ně patří protokoly událostí zabezpečení, bránu provozní a Azure MFA hello, které jsou popsané v předchozí části hello. 

Dole je příklad výstupu zobrazuje událost přihlášení se nezdařilo (událost ID 6273) protokolu zabezpečení:

 ![Protokolu zabezpečení](./media/nps-extension-vpn/image47.png)

Zde jsou související události z protokolů AzureMFA hello:

 ![Protokoly Azure MFA](./media/nps-extension-vpn/image48.png)

řešení potíží s tooperform rozšířené možnosti, prohlédněte hello NPS formátu soubory protokolů databáze nainstalovanou hello službu NPS. Tyto soubory protokolu se vytvoří v _%SystemRoot%\System32\Logs_ složky jako soubory text oddělený čárkami. Popis těchto souborů protokolu najdete v tématu [interpretovat NPS formátu soubory protokolů databáze](https://technet.microsoft.com/library/cc771748.aspx). 

Hello položky v těchto protokolových souborech jsou těžko toointerpret bez jejich importování do tabulkového procesoru nebo databázi. Můžete najít číslo z online tooassist analyzátory služby ověřování v Internetu v interpretace hello soubory protokolu. Níže je výstup hello jednoho takové ke stažení [shareware aplikace](http://www.deepsoftware.com/iasviewer): 

 ![Shareware aplikace](./media/nps-extension-vpn/image49.png)

Nakonec pro další možnosti řešení potíží, můžete pomocí analyzátoru protokolu například Wireshark nebo [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). Hello následující obrázek z Wireshark zobrazí hello RADIUS zprávy mezi hello VPN a serverem NPS hello.

 ![Microsoft Message Analyzer](./media/nps-extension-vpn/image50.png)

Další informace najdete v tématu [vaší stávající infrastruktury pro server NPS integrovat Azure Multi-Factor Authentication](multi-factor-authentication-nps-extension.md).  

## <a name="next-steps"></a>Další kroky
[Jak tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)

[Brána vzdálené plochy Azure Multi-Factor Authentication Server pomocí protokolu RADIUS](multi-factor-authentication-get-started-server-rdg.md)

[Integrace místních adresářů se službou Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md)

