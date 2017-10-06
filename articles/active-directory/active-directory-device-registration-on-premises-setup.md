---
title: "aaaSetting si místní podmíněný přístup pomocí registrace zařízení služby Azure Active Directory | Microsoft Docs"
description: "Podrobný průvodce tooenabling podmíněného přístupu tooon místní aplikace pomocí služby Active Directory Federation Services (AD FS) v systému Windows Server 2012 R2."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6ae9df8b-31fe-4d72-9181-cf50cfebbf05
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 808e3b96873102aebae667153f588dcdb205e884
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-on-premises-conditional-access-by-using-azure-active-directory-device-registration"></a>Nastavení místního podmíněného přístupu pomocí registrace zařízení služby Azure Active Directory
Pokud požadujete uživatelé tooworkplace připojení k jejich osobních zařízení toohello služby registrace zařízení služby Azure Active Directory (Azure AD), svá zařízení může být označený jako známé tooyour organizace. Toto je podrobný návod pro povolení podmíněného přístupu tooon místní aplikace pomocí služby Active Directory Federation Services (AD FS) v systému Windows Server 2012 R2.

> [!NOTE]
> Office 365 licence nebo licenci Azure AD Premium je potřeba při použití zařízení, která jsou zaregistrována v zásady podmíněného přístupu služby registrace zařízení služby Azure Active Directory. Mezi ně patří zásady, které vynucuje služby AD FS v místních prostředků.
> 
> Další informace o scénářích hello podmíněného přístupu pro místní prostředky najdete v tématu [připojení tooworkplace z libovolného zařízení pro jednotné přihlašování a bezproblémové dvouúrovňové ověřování napříč podnikovými aplikacemi](https://technet.microsoft.com/library/dn280945.aspx).

Tyto možnosti jsou k dispozici toocustomers, kdo zakoupit licenci Azure Active Directory Premium.

## <a name="supported-devices"></a>Podporovaná zařízení
* Zařízení připojených k doméně systému Windows 7
* Zařízení s Windows 8.1 osobní a připojený k doméně
* iOS 6 nebo novější pro prohlížeč Safari hello
* Android 4.0 nebo novější, Samsung GS3 nebo telefony novější, Samsung Galaxy Poznámka 2 nebo novější tablety

## <a name="scenario-prerequisites"></a>Předpoklady pro scénář
* Předplatné tooOffice 365 nebo Azure Active Directory Premium
* Klient služby Azure Active Directory
* Windows Server Active Directory (Windows Server 2008 nebo novější)
* Aktualizované schéma v systému Windows Server 2012 R2
* Licence pro Azure Active Directory Premium
* Windows Server 2012 R2 Federation Services, nakonfigurované pro jednotné přihlašování tooAzure AD
* Proxy webových aplikací systému Windows Server 2012 R2 
* Microsoft Azure Active Directory Connect (Azure AD Connect) [(stažení Azure AD Connect)](http://www.microsoft.com/en-us/download/details.aspx?id=47594)
* Ověřené domény

## <a name="known-issues-in-this-release"></a>Známé problémy v této verzi
* Zásady podmíněného přístupu na základě zařízení vyžadují zařízení objekt zpětného zápisu tooActive adresáře ze služby Azure Active Directory. Může to trvat až toothree hodin pro zařízení objekty toobe zapsány zpět tooActive adresáře.
* zařízení se systémem iOS 7 vždycky dotáže hello uživatele tooselect certifikátu během ověření klientského certifikátu.
* Některé verze systému iOS 8 před iOS 8.3 nefungují.

## <a name="scenario-assumptions"></a>Předpoklady pro scénář
Tento scénář předpokládá, že máte hybridní prostředí skládající se z klienta služby Azure AD a místní Active Directory. Tyto klienty by měl být připojen službou Azure AD Connect, s ověřenou doménu a se službou AD FS pro jednotné přihlašování. Použijte následující toohelp kontrolní seznam konfigurace prostředí podle požadavků toohello hello.

## <a name="checklist-prerequisites-for-conditional-access-scenario"></a>Kontrolní seznam: Požadavky pro scénář podmíněného přístupu
Klientovi služby Azure AD Connect s vaší místní instancí Active Directory.

## <a name="configure-azure-active-directory-device-registration-service"></a>Konfigurace služby Azure Active Directory device registration service
Pomocí tohoto průvodce toodeploy a konfigurace služby registrace zařízení hello Azure Active Directory pro vaši organizaci.

Tato příručka předpokládá, že jste nakonfigurovali Windows Server Active Directory a mít předplatné tooMicrosoft Azure Active Directory. Viz hello požadavky popsané výše.

klienta toodeploy hello služby registrace zařízení služby Azure Active Directory s Azure Active Directory, dokončení hello úlohy v následujícím kontrolním seznamu udělejte v pořadí hello. Při použití odkazu přejdete tooa koncepční téma, vrátí kontrolní seznam toothis i později, tak, že můžete pokračovat hello zbývajících úloh. Některé úlohy zahrnují krok ověření scénář, který vám může pomoct potvrďte, zda text hello krok byl úspěšně dokončen.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Část 1: Registrace zařízení povolení Azure Active Directory
Postupujte podle kroků hello v tooenable hello kontrolní seznam a nakonfigurovat služby hello Azure Active Directory device registration service.

| Úkol | Referenční informace | 
| --- | --- |
| Povolte registraci zařízení ve vaší službě Azure Active Directory klienta tooallow zařízení toojoin hello. Ve výchozím nastavení není povolené ověřování Azure Multi-Factor Authentication pro službu hello. Doporučujeme však používat ověřování Multi-Factor Authentication při registraci zařízení. Před povolením vícefaktorového ověřování ve službě Active Directory registrace, ujistěte se, že služba AD FS je nakonfigurován pro poskytovatele služby Multi-Factor Authentication. |[Povolení registrace zařízení služby Azure Active Directory](active-directory-device-registration-get-started.md)| 
|Zařízení zjistit služby registrace zařízení služby Azure Active Directory tak, že vyhledá známých záznamů DNS. Nakonfigurujte záznamy DNS vaší společnosti, aby zařízení můžete zjistit služby registrace zařízení služby Azure Active Directory. |[Konfigurace zjišťování nástroje registrace zařízení služby Azure Active Directory](active-directory-device-registration-get-started.md)| 


## <a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Část 2: Nasazení a konfigurace Windows serveru 2012 R2 Active Directory Federation Services a nastavte vztah federace se službou Azure AD

| Úkol | Referenční informace |
| --- | --- |
| Nasazení služby Active Directory Domain Services s rozšířeními schématu systému Windows Server 2012 R2 hello. Není nutné tooupgrade všechny vaše tooWindows řadiče domény Server 2012 R2. Jediným požadavkem hello je upgrade schématu Hello. |[Upgradu vašeho schématu Active Directory Domain Services](#upgrade-your-active-directory-domain-services-schema) |
| Zařízení zjistit služby registrace zařízení služby Azure Active Directory tak, že vyhledá známých záznamů DNS. Nakonfigurujte záznamy DNS vaší společnosti, aby zařízení můžete zjistit služby registrace zařízení služby Azure Active Directory. |[Příprava zařízení podporu služby Active Directory](#prepare-your-active-directory-to-support-devices) |

## <a name="part-3-enable-device-writeback-in-azure-ad"></a>Část 3: Zpětný zápis zařízení povolit ve službě Azure AD
| Úkol | Referenční informace |
| --- | --- |
| Dokončení druhé části "Povolení zpětného zápisu zařízení v Azure AD Connect." Po dokončení je návratový toothis průvodce. |[Povolení zpětného zápisu zařízení v Azure AD Connect](#upgrade-your-active-directory-domain-services-schema) |

## <a name="optional-part-4-enable-multi-factor-authentication"></a>[Nepovinné] Část 4: Povolení služby Multi-Factor Authentication
Důrazně doporučujeme nakonfigurovat jednu hello několik možností pro službu Multi-Factor Authentication. Pokud chcete, aby toorequire služby Multi-Factor Authentication, přečtěte si téma [zvolte hello řešení zabezpečení Multi-Factor Authentication pro vás](../multi-factor-authentication/multi-factor-authentication-get-started.md). Obsahuje popis jednotlivých řešení a propojí toohelp nakonfigurujete hello řešení podle svého výběru.

## <a name="part-5-verification"></a>Část 5: ověření
nasazení Hello je nyní dokončen a můžete vyzkoušet některé scénáře. Použijte následující odkazy tooexperiment službou hello a seznámit se s jeho funkce hello.

| Úkol | Referenční informace |
| --- | --- |
| Připojení k síti na pracovišti tooyour některé zařízení pomocí služby Azure Active Directory device registration service. Toho se můžete zapojit iOS, Windows a zařízení se systémem Android. |[Připojení k síti na pracovišti tooyour zařízení pomocí služby Azure Active Directory device registration service](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Zobrazit a povolit nebo zakázat registrovaná zařízení pomocí portálu správce hello. V této úloze zobrazit některé registrovaná zařízení pomocí portálu správce hello. |[Přehled služby registrace zařízení služby Azure Active Directory](active-directory-device-registration-get-started.md) |
| Ověřte, že objekty zařízení, zapíšou se zpět z Azure Active Directory tooWindows Server Active Directory. |[Ověřte, že registrovaná zařízení jsou zapsány zpět tooActive adresáře](#verify-registered-devices-are-written-back-to-active-directory) |
| Teď, když uživatelé mohou registrovat svá zařízení, můžete vytvořit aplikaci ve službě AD FS, které umožní pouze k registrovaným zařízením zásady přístupu. V této úloze vytvoříte pravidlo přístupu aplikace a vlastní zprávu o odepření přístupu. |[Vytvoření zásad přístupu aplikace a vlastní zprávu při odepření přístupu](#create-an-application-access-policy-and-custom-access-denied-message) |

## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Integraci služby Azure Active Directory s místní služby Active Directory
Tento krok umožňuje integrovat klientovi Azure AD vaší služby Active Directory v místě pomocí Azure AD Connect. I když hello kroky jsou k dispozici v hello portál Azure classic, poznamenejte si žádné zvláštní pokyny, které jsou uvedené v této části.

1. Přihlaste se toohello portál Azure classic pomocí účtu, který je globálním správcem ve službě Azure AD.
2. V levém podokně hello vyberte **služby Active Directory**.
3. Na hello **Directory** vyberte adresáře.
4. Vyberte hello **integrace adresáře** kartě.
5. V části hello **nasadit a spravovat** , postupujte podle kroků 1 až 3 toointegrate Azure Active Directory s místním adresářem.
   
   1. Přidání domény.
   2. Nainstalujte a spusťte Azure AD Connect pomocí pokynů hello [vlastní instalace Azure AD Connect](connect/active-directory-aadconnect-get-started-custom.md).
   3. Ověření a Správa synchronizace adresářů. Jeden přihlašování pokyny jsou k dispozici v tomto kroku.
   
   Kromě toho konfigurace federace se službou AD FS, jak je uvedeno v [vlastní instalace Azure AD Connect](connect/active-directory-aadconnect-get-started-custom.md).

## <a name="upgrade-your-active-directory-domain-services-schema"></a>Upgradu vašeho schématu Active Directory Domain Services
> [!NOTE]
> Po upgradu schéma služby Active Directory, proces hello je nevratný. Doporučujeme nejprve provést hello upgrade v testovacím prostředí.
> 

1. Přihlaste se tooyour řadič domény pomocí účtu, který má oprávnění správce schématu i správce podnikové sítě.
2. Kopírování hello **[media] \support\adprep** adresář a podadresáře tooone řadičích domény služby Active Directory (kde **[media]** je hello cesta toohello Windows Server 2012 R2 instalačního média ).
4. Z příkazového řádku, přejděte toohello **adprep** adresáře a spusťte **adprep.exe/forestprep**. Postupujte podle hello pokynů toocomplete hello schématu upgradu.

## <a name="prepare-your-active-directory-toosupport-devices"></a>Příprava zařízení toosupport služby Active Directory
> [!NOTE]
> Jedná se o jednorázovou operaci, musíte spustit tooprepare zařízení toosupport doménové struktury služby Active Directory. toocomplete tento postup vám musí být podepsané pomocí oprávnění správce rozlehlé sítě a doménové struktury služby Active Directory musí mít hello schématu Windows serveru 2012 R2.
> 


### <a name="prepare-your-active-directory-forest"></a>Příprava doménové struktury služby Active Directory
1. Na federačním serveru, otevřete příkazové okno prostředí Windows PowerShell a zadejte **inicializovat ADDeviceRegistration**. 
2. Po zobrazení výzvy k **ServiceAccountName**, zadejte hello název účtu služby hello jste vybrali jako hello účet služby pro službu AD FS. Pokud se jedná o účet gMSA, zadejte účet hello ve hello **domain\accountname$** formátu. Pro účet domény, použijte formát hello **domain\accountname**.

### <a name="enable-device-authentication-in-ad-fs"></a>Povolit ověřování zařízení ve službě AD FS
1. Na federačním serveru, otevřete konzolu pro správu hello služby AD FS a přejděte příliš**služby AD FS** > **zásady ověřování**.
2. Na hello **akce** podokně, vyberte **upravit globální primární ověřování**.
3. Zkontrolujte **povolit ověřování zařízení**a potom vyberte **OK**.
4. Ve výchozím nastavení služba AD FS pravidelně odebere nepoužívané zařízení ze služby Active Directory. Při použití služby Azure Active Directory device registration service, aby zařízení můžete spravovat v Azure, zakažte tuto úlohu.

### <a name="disable-unused-device-cleanup"></a>Zakázání nepoužívaných zařízení čištění
Na federačním serveru, otevřete příkazové okno prostředí Windows PowerShell a zadejte **Set AdfsDeviceRegistration - MaximumInactiveDays 0**.

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Příprava Azure AD Connect pro zpětný zápis zařízení.
Dokončení část 1: Příprava Azure AD Connect.

## <a name="join-devices-tooyour-workplace-by-using-azure-active-directory-device-registration-service"></a>Připojení k síti na pracovišti tooyour zařízení pomocí služby Azure Active Directory device registration service

### <a name="join-an-ios-device-by-using-azure-active-directory-device-registration"></a>Připojení zařízení s iOS pomocí registrace zařízení služby Azure Active Directory
Registrace zařízení služby Azure Active Directory používá proces registrace hello bezdrátového profilu pro zařízení s iOS. Tento proces začíná, když uživatel hello připojí adresu URL registračního profilu toohello s Safari. Formát adresy URL Hello vypadá takto:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

V takovém případě `yourdomainname` je hello název domény, který jste nakonfigurovali v Azure Active Directory. Například pokud je název vaší domény contoso.com, adresa URL hello vypadá takto:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Nejsou mnoha různými způsoby toocommunicate uživatelé tooyour tuto adresu URL. Například jedna metoda, doporučujeme je publikování tuto adresu URL ve zprávě o odepření přístupu vlastní aplikaci ve službě AD FS. Tyto informace je popsaná v následující části hello [vytvoření zásad přístupu aplikace a vlastní zprávu při odepření přístupu](#create-an-application-access-policy-and-custom-access-denied-message).

### <a name="join-a-windows-81-device-by-using-azure-active-directory-device-registration"></a>Připojení zařízení s Windows 8.1 pomocí registrace zařízení služby Azure Active Directory
1. Na zařízení s Windows 8.1, vyberte **nastavení počítače** > **sítě** > **síti na pracovišti**.
2. Zadejte uživatelské jméno ve formátu UPN. například  **dan@contoso.com** .
3. Vyberte **připojení**.
4. Pokud budete vyzváni, přihlaste se pomocí přihlašovacích údajů. Hello zařízení je teď připojené.

### <a name="join-a-windows-7-device-by-using-azure-active-directory-device-registration"></a>Připojení zařízení s Windows 7 pomocí registrace zařízení služby Azure Active Directory
zařízení připojená k doméně tooregister Windows 7, je nutné balíček softwaru registrace zařízení toodeploy hello. Hello softwarový balíček je volána síti na pracovišti připojit k systému Windows pro systém Windows 7 a jeho k dispozici ke stažení na hello [webu Microsoft Connect](https://connect.microsoft.com/site1164). 

Pokyny o tom, jak toouse hello balíček jsou k dispozici v [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="verify-that-registered-devices-are-written-back-tooactive-directory"></a>Ověřte, že registrovaná zařízení, zapíšou se zpět tooActive adresáře
Můžete zobrazit a ověřit, že vaše zařízení objekty byla zapsána zpět tooyour služby Active Directory pomocí nástroje LDP.exe nebo ADSI Edit. Obě možnosti jsou dostupné pomocí nástroje pro správu služby Active Directory hello.

Ve výchozím nastavení, objekty zařízení, které jsou zapsány zpět z Azure Active Directory jsou umístěny v hello stejné domény jako svou farmu AD FS.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Vytvoření zásad přístupu aplikace a vlastní zprávu při odepření přístupu
Vezměte v úvahu hello následující scénář: vytvoření aplikace vztahu důvěryhodnosti předávající strany ve službě AD FS a nakonfigurovat autorizační pravidlo vystavování, která umožňuje pouze k registrovaným zařízením. Nyní jsou jenom zařízení, které jsou registrovány povolená tooaccess hello aplikace. 

toomake ho snadno toohello aplikace uživatelům toogain přístup, nakonfigurujte vlastní zprávu odepření přístupu, která obsahuje pokyny, jak toojoin jejich zařízení. Nyní mají vaši uživatelé tooregister bezproblémové způsob jejich zařízení, získají přístup k aplikaci.

Hello následující kroky ukazují, jak tooimplement tento scénář.

> [!NOTE]
> V této části se předpokládá, že jste již nakonfigurovali vztahu důvěryhodnosti předávající strany pro aplikaci ve službě AD FS.
> 

1. Otevřete hello nástroj konzoly MMC AD FS a pak vyberte **služby AD FS** > **vztahy důvěryhodnosti** > **vztahy důvěryhodnosti předávající strany**.
2. Vyhledejte toowhich hello aplikace, které bude použito toto nové pravidlo přístupu. Klikněte pravým tlačítkem na hello aplikace a pak vyberte **upravit pravidla deklarací identity**.
3. Vyberte hello **autorizační pravidla vystavování** a pak vyberte **přidat pravidlo**.
4. Z hello **pravidlo deklarace identity** šablony rozevíracího seznamu, vyberte **povolení nebo odmítnutí uživatele na příchozí deklarace identity základě**. Potom vyberte **Další**.
5. V hello **název pravidla deklarací** zadejte **povolení přístupu z registrovaných zařízení**.
6. Z hello **typ příchozí deklarace** rozevíracího seznamu vyberte **je registrovaný uživatel**.
7. V hello **hodnota příchozí deklarace** zadejte **true**.
8. Vyberte hello **toousers povolení přístupu s touto příchozí deklarací identity** přepínač.
9. Vyberte **Dokončit**a potom vyberte **použít**.
10. Odeberte všechna pravidla, která jsou mírnější než hello pravidlo, které jste vytvořili. Například odstranit výchozí pravidlo hello **tooall povolit přístup uživatelům**.

Aplikace je teď nakonfigurovaná tooallow přístup jenom v případě, že uživatel hello pochází ze zařízení, aby zaregistrované a připojený k síti na pracovišti toohello. Pro pokročilejší přístup zásady, najdete v části [řízení rizik pomocí dodatečného vícefaktorového ověřování citlivých aplikací](https://technet.microsoft.com/library/dn280949.aspx).

Dále je nutné nakonfigurovat vlastní chybové zprávy pro vaši aplikaci. chybová zpráva Hello upozorní uživatele na pracovišti toohello jejich zařízení bylo musí připojit, předtím, než získají přístup k aplikaci hello. Zpráva o odepření přístupu vlastní aplikaci můžete vytvořit pomocí vlastních HTML a prostředí PowerShell.

Na federačním serveru otevřete příkazové okno prostředí PowerShell a zadejte následující příkaz hello. Části hello příkazu nahraďte položky, které jsou specifické tooyour systému:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Zařízení je nutné zaregistrovat si před přístupem k této aplikaci.

**Pokud používáte zařízení se systémem iOS, vyberte tento odkaz toojoin zařízení**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Připojte tento iOS zařízení tooyour síti na pracovišti.

Pokud používáte zařízení s Windows 8.1, můžete svoje zařízení připojíte výběrem **nastavení počítače**> **sítě** > **síti na pracovišti**.

V předchozích příkazů, hello **název vztahu důvěryhodnosti předávající strany** je hello název vaší aplikace objekt vztahu důvěryhodnosti předávající strany ve službě AD FS.
A **vase_domena.com** je hello název domény, který jste nakonfigurovali v Azure Active Directory (například contoso.com).
Být jisti tooremove žádné zalomení (pokud existuje) z hello HTML obsahu, že předáváte toohello **Set-AdfsRelyingPartyWebContent** rutiny.

Nyní když uživatelé přístup k aplikaci ze zařízení, které není registrován u hello služby Azure Active Directory device registration service, uvidí stránky, který vypadá podobně jako toohello následující snímek obrazovky.

![Snímek obrazovky chybu, když uživatelé nezaregistrovali svoje zařízení s Azure AD](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)


