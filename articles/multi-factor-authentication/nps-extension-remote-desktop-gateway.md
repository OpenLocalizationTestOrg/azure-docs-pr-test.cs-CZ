---
title: "aaaRemote brána plochy integraci s Azure MFA serveru NPS rozšíření | Microsoft Docs"
description: "Tento článek popisuje, integrace infrastruktury Brána vzdálené plochy s Azure MFA pomocí hello serveru NPS (Network Policy Server) rozšíření pro Microsoft Azure."
services: active-directory
keywords: "Azure MFA, integraci služby Brána vzdálené plochy, Azure Active Directory, Network Policy Server rozšíření"
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
ms.openlocfilehash: ae5f6864416582bd82b787005ca787988d23a813
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="integrate-your-remote-desktop-gateway-infrastructure-using-hello-network-policy-server-nps-extension-and-azure-ad"></a>Integrace infrastruktury Brána vzdálené plochy pomocí rozšíření hello serveru NPS (Network Policy Server) a Azure AD

Tento článek obsahuje podrobné informace pro integraci infrastruktury Brána vzdálené plochy Azure Multi-Factor Authentication (MFA) pomocí hello serveru NPS (Network Policy Server) rozšíření pro Microsoft Azure. 

Hello rozšíření služby NPS (Network Policy Server) pro Azure umožňuje zákazníkům toosafeguard RADIUS Remote Authentication Dial-In User Service () ověřování klientů pomocí Azure je založená na cloudu [Multi-Factor Authentication (MFA)](multi-factor-authentication.md). Toto řešení poskytuje dvoustupňové ověřování pro přidání druhé vrstvy zabezpečení toouser přihlášení a transakce.

Tento článek obsahuje podrobné pokyny k integraci s Azure MFA hello NPS infrastruktury pomocí hello NPS rozšíření pro Azure. To umožňuje zabezpečeného ověřování pro uživatele pokoušející toolog na tooa Brána vzdálené plochy. 

Hello síťové zásady a přístup služby (NPS) poskytuje organizace hello možnost toodo hello následující:
* Definujte centrální umístění pro správu hello a řízení požadavků sítě tak, že zadáte, kdo se může připojit, hello denní jsou připojení povolena, jakou dobu trvání připojení a hello úroveň zabezpečení, musí klienti používat tooconnect a tak dále. Místo zadání těchto zásad na každý server sítě VPN nebo brány vzdálené plochy (RD), tyto zásady můžete zadat jednou v centrálním umístění. Hello protokolu RADIUS poskytuje hello centralizované ověřování, autorizaci a monitorování (AAA). 
* Stanovit a vynutit zásady stavu klienta Network Access Protection (NAP), které určují, jestli zařízení mají neomezený nebo omezený přístup k prostředkům toonetwork.
* Zadejte znamená tooenforce ověřování a autorizace pro přístup k podporující too802.1x bezdrátové přístupové body a přepínače sítě Ethernet.    

Obvykle organizace používat toosimplify RADIUS serveru NPS () a centralizovat správu hello VPN zásady. Mnoho organizací ale také použít NPS toosimplify a centralizovat správu hello zásady autorizace připojení plochy vzdálené ploše (VP CAP). 

Organizace můžete také integrovat zabezpečení tooenhance Azure MFA serveru NPS a zajistit vysokou úroveň dodržování předpisů. To pomáhá zajistit, že uživatelé vytvořit dvoustupňové ověření toolog na toohello Brána vzdálené plochy. Pro toobe uživatelům udělen přístup musí zadat, že jejich kombinace uživatelského jména a hesla s informacemi, které hello uživatel má v jejich řízení. Tyto informace musí být důvěryhodný a duplikovaná není snadno, například číslo mobilního telefonu, číslo pevné, aplikace na mobilní zařízení a tak dále.

Předchozí toohello dostupnost hello NPS rozšíření pro Azure, zákazníky, kteří si přáli tooimplement dvoustupňové ověřování pro integraci serveru NPS a prostředí Azure MFA měl tooconfigure a udržovat v místním prostředí hello jako samostatný Server MFA popsané v [Brána vzdálené plochy a pomocí protokolu RADIUS serveru Azure Multi-Factor Authentication Server](multi-factor-authentication-get-started-server-rdg.md).

dostupnost Hello hello NPS rozšíření pro Azure nyní umožňuje organizacím hello volba toodeploy do řešení vícefaktorového ověřování pro místní nebo cloudové MFA řešení toosecure RADIUS ověření klienta.

## <a name="authentication-flow"></a>Tok ověřování

Pro uživatele toobe udělen přístup k prostředkům toonetwork přes Brána vzdálené plochy že musí splňovat podmínky hello zadané v jedné VP zásady autorizace připojení (RD CAP) a zásady pro prostředek autorizace jeden vzdálené ploše (VP pro autorizaci prostředků). VP CAP k vzdálené ploše zadejte, který je autorizovaný tooconnect tooRD brány. Zásady VP pro autorizaci prostředků zadejte hello síťovým prostředkům, jako je vzdálené plochy nebo vzdálené aplikace, je tento uživatel hello povoleno tooconnect toothrough hello Brána VP. 

Bránu VP může být nakonfigurované toouse úložiště centrální zásady pro VP CAP k vzdálené ploše. Zásady VP pro autorizaci prostředků nelze použít centrální zásady, jak se zpracovávají na hello Brána VP. Příkladem toouse Brána VP nakonfigurované zásady centrálního úložiště pro VP CAP k vzdálené ploše je server NPS tooanother klienta RADIUS, který slouží jako úložiště hello centrální zásady.

Když hello NPS rozšíření pro Azure je integrována hello NPS a Brána vzdálené plochy, tok úspěšné ověření hello vypadá takto:

1. server služby Brána vzdálené plochy Hello obdrží požadavek na ověření z prostředku tooa tooconnect uživatele vzdálené plochy, jako je například relaci vzdálené plochy. Funguje jako klienta protokolu RADIUS, serveru brány vzdálené plochy hello převede hello požadavek tooa RADIUS tato zpráva a odešle serveru RADIUS (NPS) toohello zpráva hello nainstalovanou hello NPS rozšíření. 
2. Hello uživatelské jméno a kombinace hesla je ověřit ve službě Active Directory a hello uživatel ověřen.
3. Pokud všechny podmínky zadané v hello hello žádost o připojení serveru NPS a zásady sítě hello splnění (například čas, den nebo skupiny omezeními pro členství vyplývajícími), hello NPS rozšíření aktivuje žádost o sekundárního ověřování s Azure MFA. 
4. Azure MFA komunikuje s Azure AD, načte podrobnosti hello uživatele a provede hello sekundárního ověřování metodou hello nakonfiguroval hello uživatel (textová zpráva, mobilní aplikace a tak dále). 
5. Po úspěšné hello ověřovací test MFA Azure MFA komunikuje hello výsledek toohello NPS rozšíření.
6. server NPS Hello nainstalovanou hello rozšíření odešle zprávu povolení přístupu protokolu RADIUS pro server brány vzdálené plochy toohello zásady CAP k vzdálené ploše hello.
7. Hello uživateli je udělen přístup toohello požadovaný prostředek sítě prostřednictvím hello Brána VP.

## <a name="prerequisites"></a>Požadavky
Tato část podrobně hello předpoklady nezbytné před integrace Azure MFA s hello Brána vzdálené plochy. Než začnete, musíte mít hello následující požadavky splněny.  

* Vzdálené infrastruktury služby plocha (RDS)
* Licence Azure MFA
* Software Windows serveru
* Role Síťové zásady a přístup služby NPS)
* Azure AD synchronizována s místní AD 
* ID identifikátor GUID služby Azure Active Directory

### <a name="remote-desktop-services-rds-infrastructure"></a>Vzdálené infrastruktury služby plocha (RDS)
Musíte mít funkční infrastrukturu služby Vzdálená plocha (RDS) na místě. Pokud ho použít nechcete, potom můžete rychle vytvořit tuto infrastrukturu v Azure pomocí následujících hello rychlý úvodní šablony: [vytvoření kolekce relací vzdálené plochy nasazení](https://github.com/Azure/azure-quickstart-templates/tree/ad20c78b36d8e1246f96bb0e7a8741db481f957f/rds-deployment). 

Pokud chcete vytvořit toomanually místní RDS infrastruktury rychle pro testovací účely, postupujte podle hello kroky toodeploy jeden. 
**Další informace**: [nasazení vzdálené plochy s Azure rychlý start](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-in-azure) a [nasazení infrastruktury základní RDS](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure). 

### <a name="licenses"></a>Licence
Vyžaduje se licenci pro Azure MFA, která je k dispozici prostřednictvím Azure AD Premium, Enterprise Mobility plus Security (EMS) nebo předplatné vícefaktorového ověřování. Další informace najdete v tématu [jak tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md). Pro účely testování můžete použít zkušební verzi předplatného.

### <a name="software"></a>Software
Hello NPS rozšíření vyžaduje Windows Server 2008 R2 SP1 nebo novějším s nainstalovanou službou role NPS hello. Všechny kroky hello v této části se provádí pomocí systému Windows Server 2016.

### <a name="network-policy-and-access-services-nps-role"></a>Role Síťové zásady a přístup služby NPS)
Hello službu role NPS poskytuje hello protokolu RADIUS serveru a klienta, funkce a také zásady přístupu k síti služby stavu. Musí být nainstalovaná tato role na alespoň dva počítače ve vaší infrastruktuře: hello Brána vzdálené plochy a druhý členský server nebo řadič domény. Ve výchozím nastavení hello role již existuje v hello počítač nakonfigurovaný jako hello Brána vzdálené plochy.  Musíte také nainstalovat hello NPS role na alespoň na jiném počítači, jako jsou řadiče domény nebo členském serveru.

Informace o instalaci role serveru NPS hello služby Windows Server 2012 nebo starší, najdete v části [nainstalovat Server zásad stavu NAP](https://technet.microsoft.com/library/dd296890.aspx). Popis osvědčené postupy pro server NPS, včetně doporučení tooinstall hello NPS na řadiči domény najdete v části [osvědčené postupy pro server NPS](https://technet.microsoft.com/library/cc771746).

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Active Directory synchronizována s místní služby Active Directory 
toouse hello NPS rozšíření místní uživatelé musí být synchronizovaná s Azure AD a povolené pro MFA. V této části se předpokládá, že jsou synchronizována místních uživatelů s Azure AD pomocí služby AD Connect. Informace o Azure AD connect, najdete v části [integraci místních adresářů se službou Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>ID identifikátor GUID služby Azure Active Directory
tooinstall NPS, potřebujete tooknow hello GUID hello Azure AD. Pokyny k hledání hello GUID hello Azure AD jsou uvedeny níže.

## <a name="configure-multi-factor-authentication"></a>Konfigurace služby Multi-Factor Authentication 
Tato část obsahuje pokyny k integraci Azure MFA s hello Brána vzdálené plochy. Jako správce musíte nakonfigurovat službu Azure MFA hello předtím, než mohou uživatelé registrovat svoje zařízení Multi-Factor a aplikace.

Postupujte podle kroků hello v [Začínáme s Azure Multi-Factor Authentication v cloudu hello](multi-factor-authentication-get-started-cloud.md) tooenable vícefaktorového ověřování pro uživatele Azure AD. 

### <a name="configure-accounts-for-two-step-verification"></a>Konfigurace účtů pro dvoustupňové ověření
Jakmile účet povolen pro MFA, nemůžete se přihlásit tooresources řídí hello zásad vícefaktorového ověřování dokud úspěšně jste nakonfigurovali důvěryhodné zařízení toouse hello druhý ověřovací faktor mít ověřeného pomocí dvoustupňového ověření.

Postupujte podle kroků hello v [co Azure Multi-Factor Authentication znamená pro mě nejlepší?](./end-user/multi-factor-authentication-end-user.md) toounderstand a správně nakonfigurovat zařízení pro MFA s vaším uživatelským účtem.

## <a name="install-and-configure-nps-extension"></a>Instalace a konfigurace serveru NPS rozšíření
Tato část obsahuje pokyny pro konfiguraci vzdálené plochy infrastruktury toouse Azure MFA pro ověřování klientů se hello Brána vzdálené plochy.

### <a name="acquire-azure-active-directory-guid-id"></a>Získání ID identifikátor GUID služby Azure Active Directory

Jako součást konfigurace hello hello rozšíření serveru NPS potřebujete přihlašovací údaje správce toosupply a ID hello Azure AD pro vašeho tenanta Azure AD. Hello následující kroky ukazují, jak ID tooget hello klienta.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) jako globální správce hello hello Azure klienta.
2. V levé navigační hello, vyberte hello **Azure Active Directory** ikonu.
3. Vyberte **vlastnosti**.
4. V okně Vlastnosti hello vedle hello ID adresáře, klikněte na tlačítko hello **kopie** ikonu, jak je uvedeno níže, toocopy hello ID tooclipboard.

 ![Vlastnosti](./media/nps-extension-remote-desktop-gateway/image1.png)

### <a name="install-hello-nps-extension"></a>Instalace serveru NPS rozšíření hello
Nainstalujte rozšíření hello NPS na server, který má hello síťové zásady a nainstalovanou rolí služby NPS přístup (Server). To funguje jako server RADIUS hello pro váš návrh. 

>[!Important]
> Ujistěte se, že instalujete hello NPS rozšíření není na serveru služby Brána vzdálené plochy.
> 

1. Stáhnout hello [NPS rozšíření](https://aka.ms/npsmfa). 
2. Kopírování hello instalační program spustitelný soubor (NpsExtnForAzureMfaInstaller.exe) toohello serveru NPS.
3. Na serveru NPS hello, dvakrát klikněte na **NpsExtnForAzureMfaInstaller.exe**. Pokud se zobrazí výzva, klikněte na tlačítko **spustit**.
4. V hello NPS rozšíření pro dialogové okno Azure MFA, zkontrolujte hello licenční podmínky pro software, zkontrolujte **souhlasím toohello licenční podmínky a ujednání**a klikněte na tlačítko **nainstalovat**.
 
  ![Nastavení Azure MFA](./media/nps-extension-remote-desktop-gateway/image2.png)

5. V hello NPS rozšíření pro dialogové okno Azure MFA klikněte na tlačítko Zavřít. 

  ![Server NPS rozšíření pro Azure MFA](./media/nps-extension-remote-desktop-gateway/image3.png)

### <a name="configure-certificates-for-use-with-hello-nps-extension-using-a-powershell-script"></a>Konfigurace certifikátů pro použití s příponou hello NPS pomocí skriptu prostředí PowerShell
Dále musíte tooconfigure certifikáty používané hello NPS rozšíření tooensure zabezpečení komunikace a záruky. součásti serveru NPS Hello zahrnují skript prostředí Windows PowerShell, který konfiguruje certifikát podepsaný svým držitelem pro použití se serverem NPS. 

Hello skript provede hello následující akce:

* Vytvoří certifikát podepsaný svým držitelem
* Přidruží veřejný klíč certifikátu tooservice hlavní na Azure AD
* Úložiště hello certifikátu v úložišti místního počítače hello
* Uděluje přístup k certifikátu toohello privátní klíče toohello síťového uživatele
* Restartuje službu serveru zásad sítě

Pokud chcete toouse vlastní certifikáty, vyžadují tooassociate hello veřejné váš certifikát toohello služby zásadu na Azure AD a tak dále.

skript hello toouse, poskytovat hello rozšíření pomocí svých přihlašovacích údajů správce Azure AD a hello ID klienta Azure AD, který jste zkopírovali dříve. Spusťte skript hello na každý server NPS, do které jste nainstalovali rozšíření NPS hello. Potom hello následující:

1. Otevřete řádku prostředí Windows PowerShell pro správu.
2. Zadejte v příkazovém prostředí PowerShell hello **cd 'c:\Program Files\Microsoft\AzureMfa\Config'**a stiskněte klávesu **ENTER**.
3. Typ _.\AzureMfsNpsExtnConfigSetup.ps1_a stiskněte klávesu **ENTER**. Hello skript kontroluje toosee, pokud je nainstalován modul Azure Active Directory PowerShell hello. Pokud není nainstalovaná, nainstaluje hello skriptu hello modulu.

  ![Azure AD PowerShell](./media/nps-extension-remote-desktop-gateway/image4.png)
  
4. Po hello skript ověřuje hello instalace modulu prostředí PowerShell text hello, zobrazí dialogové okno modul Azure Active Directory PowerShell hello. V dialogovém okně hello, zadejte přihlašovací údaje Správce služby Azure AD a heslo a klikněte na tlačítko **přihlásit**.

  ![Otevřete prostředí Powershell účet](./media/nps-extension-remote-desktop-gateway/image5.png)

5. Po zobrazení výzvy, vložte ID klienta hello předtím zkopírovali toohello schránky a stiskněte klávesu **ENTER**.

  ![Zadejte ID klienta](./media/nps-extension-remote-desktop-gateway/image6.png)

6. skript Hello vytvoří certifikát podepsaný svým držitelem a provede další změny v konfiguraci. výstup Hello by měl být jako hello obrázku vidíte níže.

  ![Certifikát podepsaný sám sebou](./media/nps-extension-remote-desktop-gateway/image7.png)

## <a name="configure-nps-components-on-remote-desktop-gateway"></a>Konfigurace součástí serveru NPS na Brána vzdálené plochy
V této části nakonfigurujete zásady autorizace připojení hello Brána vzdálené plochy a další nastavení protokolu RADIUS.

tok ověřování Hello vyžaduje, že zprávy protokolu RADIUS se vyměňují mezi hello Brána vzdálené plochy a hello serveru NPS nainstalovanou hello serveru NPS. To znamená, že musíte nakonfigurovat nastavení klientů RADIUS na Brána vzdálené plochy a hello server NPS, kde je nainstalován server NPS rozšíření hello. 

### <a name="configure-remote-desktop-gateway-connection-authorization-policies-toouse-central-store"></a>Konfigurace služby Brána vzdálené plochy připojení autorizace zásady toouse centrální úložiště
Zásady autorizace připojení ke vzdálené ploše (VP CAP) zadejte hello požadavky pro připojování tooa serveru brány vzdálené plochy. VP CAP k vzdálené ploše lze ukládat místně (výchozí), nebo mohou být uloženy v centrální úložiště CAP k vzdálené ploše, které běží server NPS. tooconfigure integrace Azure MFA s vzdálené plochy, musíte použít hello toospecify centrální úložiště.

1. Otevřete na serveru služby Brána VP hello **správce serveru**. 
2. Hello nabídce klikněte na **nástroje**, bod příliš**služby Vzdálená plocha**a potom klikněte na **Správce brány vzdálené plochy**.

  ![Vzdálená plocha](./media/nps-extension-remote-desktop-gateway/image8.png)

3. V hello Správce brány vzdálené plochy, klikněte pravým tlačítkem na  **\[název serveru\] (místní)**a klikněte na tlačítko **vlastnosti**.

  ![Název serveru](./media/nps-extension-remote-desktop-gateway/image9.png)

4. V dialogové okno Vlastnosti hello, vyberte hello **VP CAP** kartě úložiště.
5. Na kartě úložiště CAP k vzdálené ploše hello vyberte **centrálního serveru NPS**. 
6. V hello **zadejte název nebo IP adresu pro hello serveru NPS** pole, typ hello IP adresu nebo název serveru hello serveru, kam jste nainstalovali rozšíření NPS hello.

  ![Zadejte název nebo IP adresu](./media/nps-extension-remote-desktop-gateway/image10.png)
  
7. Klikněte na tlačítko **Přidat**.
8. V hello **sdílený tajný klíč** dialogové okno, zadejte sdílený tajný klíč a pak klikněte na tlačítko **OK**. Ujistěte se, zaznamenejte tento sdílený tajný klíč a bezpečně uložit záznam hello.

 >[!NOTE]
 >Sdílený tajný klíč slouží tooestablish vztah důvěryhodnosti mezi klienty a servery RADIUS hello. Vytvořte heslo dlouhá a složitá.
 >

 ![Sdílený tajný klíč](./media/nps-extension-remote-desktop-gateway/image11.png)

9. Klikněte na tlačítko **OK** dialogové okno tooclose hello.

### <a name="configure-radius-timeout-value-on-remote-desktop-gateway-nps"></a>Hodnota časového limitu protokolu RADIUS nakonfigurovat na NPS brány vzdálené plochy
existuje tooensure je čas přihlašovací údaje uživatelů toovalidate, dvoustupňové ověření, příjem odpovědí a reagovat tooRADIUS zprávy, je hodnota časového limitu protokolu RADIUS potřebné tooadjust hello.

1. Na serveru služby Brána VP hello, ve Správci serveru klikněte na tlačítko **nástroje**a potom klikněte na **Network Policy Server**. 
2. V hello **server NPS (místní)** rozbalte **klienti a servery RADIUS**a vyberte **vzdálený Server RADIUS**.

 ![Vzdálený Server RADIUS](./media/nps-extension-remote-desktop-gateway/image12.png)

3. V podokně podrobností hello klikněte dvakrát na **skupiny serverů brány TS**.

 >[!NOTE]
 >Této skupiny serverů RADIUS byla vytvořena, když jste nakonfigurovali hello centrální server pro zásady serveru NPS. Hello Brána VP předává server toothis zprávy protokolu RADIUS nebo skupiny serverů, pokud je více než jeden ve skupině hello.
 >

4. V hello **vlastností skupiny serverů brány TS** dialogové okno, vyberte hello IP adresu nebo název hello serveru NPS nakonfigurovat toostore VP CAP k vzdálené ploše a potom klikněte na **upravit**. 

 ![Skupiny serverů brány TS](./media/nps-extension-remote-desktop-gateway/image13.png)

5. V hello **upravit Server RADIUS** dialogové okno, vyberte hello **Vyrovnávání zatížení** kartě.
6. V hello **Vyrovnávání zatížení** na kartě hello **počet sekund bez odpovědi, než je žádost považována za zrušených** pole, změňte výchozí hodnotu hello z 3 tooa hodnotu v rozmezí 30 až 60 sekund.
7. V hello **počet sekund mezi požadavky, když je server identifikován jako nedostupný** pole, změňte výchozí hodnotu hello 30 sekund tooa hodnotu, která je rovna tooor větší než hodnota hello jste zadali v předchozím kroku hello.

 ![Upravit Radius Server](./media/nps-extension-remote-desktop-gateway/image14.png)

8.  Klikněte na tlačítko OK, dva časy tooclose hello dialogových oken.

### <a name="verify-connection-request-policies"></a>Ověření zásad vyžádání nového připojení 
Ve výchozím nastavení při konfiguraci brány VP toouse hello úložiště centrální zásady pro zásady autorizace připojení ke hello Brána VP je nakonfigurované tooforward CAP požadavky toohello NPS server. server NPS Hello s příponou Azure MFA hello nainstalovaný, procesy žádost o přístup protokolu RADIUS hello. Hello následující kroky vám ukážou, jak zásady požadavku tooverify hello výchozí připojení. 

1. Na hello Brána VP v hello konzoly serveru NPS (místní počítač), rozbalte položku **zásady**a vyberte **zásady vyžádání nového připojení**.
2. Klikněte pravým tlačítkem na **zásady vyžádání nového připojení**a dvakrát klikněte na **ZÁSADA AUTORIZACE brány TS**.
3. V hello **ZÁSADA AUTORIZACE brány TS vlastnosti** dialogovém okně klikněte na hello **nastavení** kartě.
4. Na **nastavení** karta, v části Přesměrování požadavku na připojení, klikněte na tlačítko **ověřování**. Klient protokolu RADIUS je nakonfigurované tooforward požadavky na ověření.

 ![Nastavení ověřování](./media/nps-extension-remote-desktop-gateway/image15.png)
 
5. Klikněte na tlačítko **zrušit**. 

## <a name="configure-nps-on-hello-server-where-hello-nps-extension-is-installed"></a>Konfigurace serveru NPS v hello serveru s nainstalovanou hello rozšíření serveru NPS
server NPS Hello kde hello NPS rozšíření je nainstalovaná potřebám toobe možné tooexchange RADIUS zprávy s hello serveru NPS na hello Brána vzdálené plochy. tooenable tuto zprávu exchange, musíte tooconfigure hello NPS součásti na hello serveru s nainstalovanou hello služby rozšíření serveru NPS. 

### <a name="register-server-in-active-directory"></a>Registrace serveru ve službě Active Directory
toofunction správně v tomto scénáři, musí hello NPS server toobe zaregistrován ve službě Active Directory.

1. Otevřete **správce serveru**.
2. Ve Správci serveru klikněte na tlačítko **nástroje**a potom klikněte na **Network Policy Server**. 
3. V konzole hello Network Policy Server, klikněte pravým tlačítkem na **server NPS (místní)**a potom klikněte na **server zaregistrovat ve službě Active Directory**. 
4. Klikněte na tlačítko **OK** dvakrát.

 ![Registrace serveru ve službě Active Directory](./media/nps-extension-remote-desktop-gateway/image16.png)

5. Ponechte hello konzoly otevřené pro další postup hello.

### <a name="create-and-configure-radius-client"></a>Vytvoření a konfigurace klienta protokolu RADIUS 
Hello Brána vzdálené plochy musí toobe nakonfigurovaný jako server NPS toohello klienta RADIUS. 

1. Na serveru NPS hello, kde je nainstalován hello rozšíření serveru NPS, v hello **server NPS (místní)** konzoly, klikněte pravým tlačítkem na **klientů RADIUS** a klikněte na tlačítko **nový**.

 ![Noví klienti RADIUS](./media/nps-extension-remote-desktop-gateway/image17.png)

2. V hello **klienta RADIUS nové** dialogové okno pole, zadejte popisný název, jako třeba _brány_, a hello IP adresu nebo název DNS serveru brány vzdálené plochy hello. 
3. V hello **sdílený tajný klíč** a hello **potvrzení sdíleného tajného klíče** pole, zadejte hello stejný tajný klíč, který jste používali před.

 ![Název a adresu](./media/nps-extension-remote-desktop-gateway/image18.png)

4. Klikněte na tlačítko **OK** dialogové okno Nový RADIUS klienta tooclose hello.

### <a name="configure-network-policy"></a>Konfigurace zásad sítě
Server NPS hello odvolání s hello rozšíření Azure MFA je hello určené zásady centrálního úložiště pro hello zásady autorizace připojení (CAP). Proto musíte tooimplement CAP u hello NPS serveru tooauthorize platné připojení požadavků.  

1. V konzole serveru NPS (místní) hello rozbalte **zásady**a klikněte na tlačítko **zásady sítě**.
2. Klikněte pravým tlačítkem na **serverů pro přístup k připojení tooother**a klikněte na tlačítko **duplicitní zásady**. 

 ![Duplicitní zásad](./media/nps-extension-remote-desktop-gateway/image19.png)

3. Klikněte pravým tlačítkem na **serverů pro přístup k připojení kopie tooother**a klikněte na tlačítko **vlastnosti**.

 ![Vlastnosti sítě](./media/nps-extension-remote-desktop-gateway/image20.png)

4. V hello **serverů pro přístup k připojení kopie tooother** dialogové okno, v název zásady, zadejte vhodný název, například **RDG_CAP**. Zkontrolujte **povolené zásady**a vyberte **udělit přístup**. Volitelně můžete v typu přístup k síti, vyberte **Brána vzdálené plochy**, nebo můžete nechat jej jako **nespecifikovaný**.

 ![Kopii připojení](./media/nps-extension-remote-desktop-gateway/image21.png)

5.  Klikněte na tlačítko hello **omezení** a zkontrolujte **tooconnect klientům povolit bez vyjednávání metodu ověřování**.

 ![Povolit klientům tooconnect](./media/nps-extension-remote-desktop-gateway/image22.png)

6. Volitelně klikněte na hello **podmínky** kartě a přidat podmínky, které musí být splněny pro hello připojení toobe oprávnění, například členství v určité skupině systému Windows.

 ![Podmínky](./media/nps-extension-remote-desktop-gateway/image23.png)

7. Klikněte na **OK**. Po výzvami tooview hello odpovídající téma nápovědy, klikněte na tlačítko **ne**.
8. Zkontrolujte, že nové zásady hello horní části seznamu hello, že je zásada hello povolena, a udělí přístup.

 ![Zásady sítě](./media/nps-extension-remote-desktop-gateway/image24.png)

## <a name="verify-configuration"></a>Ověření konfigurace
tooverify hello konfigurace, musíte toolog na hello Brána vzdálené plochy s vhodný klientem RDP. Zda toouse být účet, který je povolen na základě vaší zásady autorizace připojení a je povolený pro Azure MFA. 

Jak je vidět v následující hello obrázek, můžete použít hello **Remote Desktop Web Access** stránky.

![Remote Desktop Web Access](./media/nps-extension-remote-desktop-gateway/image25.png)

Po úspěšně zadávat svoje přihlašovací údaje pro primární ověřování, hello připojení vzdálené plochy dialogové okno zobrazuje stav Inicializace vzdáleného připojení, jak je uvedeno níže. 

Pokud jste úspěšně ověřit hello sekundární metodu ověřování, které jste dříve nakonfigurovali v Azure MFA, jste připojené toohello prostředků. Ale hello sekundárního ověřování neproběhne úspěšně, je odepřen přístup tooresource. 

![Inicializace vzdáleného připojení](./media/nps-extension-remote-desktop-gateway/image26.png)

V příkladu hello níže hello ověřovací aplikace na Windows phone je použité tooprovide hello sekundární ověřování.

![Účty](./media/nps-extension-remote-desktop-gateway/image27.png)

Jakmile úspěšně jste se ověřili pomocí hello sekundární metodu ověřování, se protokolují do hello Brána vzdálené plochy jako normální. Ale vzhledem k tomu, že jste požadované toouse metodu sekundární ověřování pomocí mobilní aplikace na důvěryhodném zařízení, procesu přihlašování hello je bezpečnější, než by byla, jinak hodnota.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Zobrazit protokoly Prohlížeče událostí pro události úspěšného přihlášení
tooview hello úspěšné přihlášení události v protokolech prohlížeče událostí systému Windows hello, můžete použít následující protokoly zabezpečení systému Windows a prostředí Windows PowerShell příkaz tooquery hello Terminálová služba systému Windows hello.

tooquery úspěšné přihlášení události v provozních protokolech hello brány _(událost prohlížeč událostí\protokoly aplikací a služeb Logs\Microsoft\Windows\TerminalServices-Gateway\Operational_, použijte hello následující příkazy:

* _Get-WinEvent. – název protokolu Microsoft-Windows-TerminalServices brány nebo provozní_ | kde {$_.ID - eq '300'} | FL 
* Tento příkaz zobrazí události systému Windows, které se zobrazí uživatelské hello splněny požadavky na zásady autorizace prostředků (VP pro autorizaci prostředků) a byl udělen přístup.

![Prohlížeč událostí zobrazení](./media/nps-extension-remote-desktop-gateway/image28.png)

* _Get-WinEvent. – název protokolu Microsoft-Windows-TerminalServices brány nebo provozní_ | kde {$_.ID - eq '200'} | FL
* Tento příkaz zobrazí hello události, které se zobrazí, když uživatel splněny požadavky na zásady autorizace připojení.

![Ověření připojení](./media/nps-extension-remote-desktop-gateway/image29.png)

Můžete také zobrazit tomuto protokolu a filtru na události ID, 300 a 200. tooquery událostí úspěšného přihlášení v protokolech prohlížeče událostí hello zabezpečení, použijte následující příkaz hello:

* _Get-WinEvent. – název protokolu zabezpečení_ | kde {$_.ID - eq '6272'} | FL 
* Tento příkaz lze spustit ve buď hello centrální server NPS nebo hello serveru služby Brána VP. 

![Události úspěšného přihlášení](./media/nps-extension-remote-desktop-gateway/image30.png)

Také můžete zobrazit protokol zabezpečení hello nebo hello síťové zásady a přístup ke službě vlastní zobrazení, jak je uvedeno níže:

![Služba Síťové zásady a přístup](./media/nps-extension-remote-desktop-gateway/image31.png)

Na serveru hello nainstalovanou hello NPS rozšíření pro Azure MFA, můžete najít protokoly Prohlížeče událostí aplikace konkrétní toohello rozšíření v _aplikace a služby Logs\Microsoft\AzureMfa_. 

![Aplikace protokoly Prohlížeče událostí](./media/nps-extension-remote-desktop-gateway/image32.png)

## <a name="troubleshoot-guide"></a>Řešení potíží s Průvodce

Pokud konfigurace hello nefungují podle očekávání, hello první místní toostart tootroubleshoot je tooverify, který hello uživatelů nakonfigurovanou toouse Azure MFA. Uživatel hello připojit toohello [portál Azure](https://portal.azure.com). Pokud se zobrazí výzva k sekundární ověření uživatelů a můžete úspěšně ověřit, že můžete eliminovat nesprávnou konfiguraci Azure MFA.

Pokud Azure MFA funguje pro hello jiní uživatelé, přečtěte si relevantní protokoly událostí hello. Mezi ně patří protokoly událostí zabezpečení, bránu provozní a Azure MFA hello, které jsou popsané v předchozí části hello. 

Níže je příklad výstupu zobrazuje událost přihlášení se nezdařilo (událost ID 6273) protokolu zabezpečení.

![Událost přihlášení se nezdařilo](./media/nps-extension-remote-desktop-gateway/image33.png)

Zde jsou související události z protokolů AzureMFA hello:

![Protokolů Azure MFA](./media/nps-extension-remote-desktop-gateway/image34.png)

řešení potíží s tooperform rozšířené možnosti, prohlédněte hello NPS formátu soubory protokolů databáze nainstalovanou hello službu NPS. Tyto soubory protokolu se vytvoří v _%SystemRoot%\System32\Logs_ složky jako soubory text oddělený čárkami. 

Popis těchto souborů protokolu najdete v tématu [interpretovat NPS formátu soubory protokolů databáze](https://technet.microsoft.com/library/cc771748.aspx). Hello položky v těchto protokolových souborech může být obtížné toointerpret bez jejich importování do tabulkového procesoru nebo databázi. Můžete najít několik analyzátory služby ověřování v Internetu online tooassist v interpretace hello soubory protokolu. 

Hello následující obrázek ukazuje výstup hello jednoho takové ke stažení [shareware aplikace](http://www.deepsoftware.com/iasviewer). 

![Shareware aplikace](./media/nps-extension-remote-desktop-gateway/image35.png)

Nakonec pro další možnosti řešení potíží, můžete pomocí analyzátoru protokolu, takové [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). 

Hello obrázek níže z Microsoft Message Analyzer ukazuje síťový provoz filtrováno protokolu RADIUS, který obsahuje uživatelské jméno hello **Contoso\AliceC**.

![Microsoft Message Analyzer](./media/nps-extension-remote-desktop-gateway/image36.png)

## <a name="next-steps"></a>Další kroky
[Jak tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)

[Brána vzdálené plochy Azure Multi-Factor Authentication Server pomocí protokolu RADIUS](multi-factor-authentication-get-started-server-rdg.md)

[Integrace místních adresářů se službou Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md)
