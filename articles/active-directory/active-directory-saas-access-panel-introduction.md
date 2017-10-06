---
title: "aaaWhat je hello přístupového panelu v Azure Active Directory? | Dokumentace Microsoftu"
description: "Zjistěte, jak toouse variace hello přistupovat k panelu (webový prohlížeč, aplikace pro Android, aplikace iPhone a iPad) tooaccess aplikací SaaS."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 800be6a69f13978c5b88e2fe28a416d4b763656c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-access-panel"></a>Co je hello přístupový panel?

Hello přístupový panel je webový portál. Umožňuje, aby uživatele, který má pracovní nebo školní účet v Azure Active Directory tooview a spusťte cloudové aplikace, že správce Azure AD udělil přístup. Můžete také použít skupiny samoobslužné služby a možnosti správy aplikace prostřednictvím hello přístupového panelu.

Hello přístupový panel je oddělená od hello portál Azure a nemá jste to vy toohave předplatné Azure.

![Přístupový panel][1]

Hello přístupový panel umožňuje tooedit některá z nastavení profilu, včetně hello možnost:

- Změna hesla hello přidružené pracovní nebo školní účet

- Upravit nastavení resetování hesla

- Upravit kontaktu a předvoleb nastavení související toomulti-factor authentication (pro účty, které byly požadované toouse ho správce)

- Zobrazit účet, který podrobnosti, jako je například ID uživatele, alternativní e-mailu a mobile a office telefonní čísla a zařízení

- Zobrazení a spusťte cloudové aplikace, které hello správce Azure AD je udělil přístup. Další informace o hello panel přístupu z hlediska uživatelů hello najdete pomocí hello přístupového panelu. 

- Samoobslužná správa skupin. Správce hello přesněji řečeno, můžete vytvořit a spravovat skupiny zabezpečení a žádosti o členství ve skupinách zabezpečení ve službě Azure AD. Další informace najdete v tématu [Samoobslužná správa skupin uživatelů ve službě Azure AD](active-directory-accessmanagement-self-service-group-management.md) a [spravovat vaše skupiny](active-directory-manage-groups.md).




## <a name="accessing-hello-access-panel"></a>Přístup k hello přístupového panelu

Když přejdete hello následující adresu URL ve webovém prohlížeči se můžete dostat hello přístupový panel:`http://myapps.microsoft.com`

Pokud máte vlastní branding nakonfigurovaný pro přihlašovací stránku, můžete načíst branding připojením end toohello domény vaší organizace hello adresy URL:`http://myapps.microsoft.com/<your domain>.com`

V takovém případě můžete použít libovolný název aktivní nebo ověřené domény, který byl nakonfigurován na portálu Azure.

![Název domény Wingtip Toys][2]  

Je nutné toodistribute hello URL tooall uživatelů, kteří se přihlašují tooapplications, které jsou integrované s Azure AD.

## <a name="authentication"></a>Authentication

tooreach hello přístupového panelu, musíte být ověřeni přes pracovní nebo školní účet ve službě Azure AD. Může být ověřené tooAzure AD přímo. Případně pokud organizace nakonfiguroval federace pomocí služby Active Directory Federation Services (AD FS) nebo jiných technologií, vám může být ověřen pomocí Windows Server Active Directory.

Pokud máte předplatné Azure nebo Office 365 a používáte hello portál Azure nebo aplikace Office 365, uvidíte hello seznam aplikací bez přihlášení znovu. Pokud jste nejsou ověřené jsou výzvami toosign v pomocí hello uživatelské jméno a heslo pro svůj účet ve službě Azure AD. Pokud vaše organizace má nakonfigurovali federaci, zadejte uživatelské jméno hello je dostačující.

Pokud jste se ověřili, můžete pracovat s hello aplikace, které správce má integrované s adresářem hello. jak zjistit, toointegrate aplikací s Azure AD, toolearn [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Požadavky na webový prohlížeč

Minimálně hello přístupový panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS. Pro uživatele toobe hello přihlášení tooapplications prostřednictvím založené na heslech jednotné přihlašování (SSO) musí být nainstalován rozšíření panely hello přístup v prohlížeči. rozšíření Hello je stažen automaticky, když vyberete aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.

rozšíření panelu přístup Hello je aktuálně dostupné pro Internet Explorer 8 a novější, okraj a Chrome, Firefox prohlížeče.

## <a name="mobile-app-support"></a>Podpora mobilních aplikací

tým služby Azure Active Directory Hello publikuje hello mé mobilní aplikace aplikace. Při instalaci aplikace hello se můžete přihlásit toopassword jednotného přihlašování k aplikacím na zařízení iOS a Android.

> [!NOTE]
> Můžete přihlásit tooapplications podporující federace se službou Azure AD (včetně Salesforce, Google Apps, Dropbox, pole, Concur, Workday, Office 365 a více než 70 dalších) prakticky libovolný webový prohlížeč na libovolném zařízení, bez nutnosti modul plug-in nebo mobilní aplikace. Všechny ostatní [přístup k panelu prostředí](https://myapps.microsoft.com/) také nevyžadují hello mé mobilní aplikace toobe aplikace používat na mobilních zařízeních.
>
>

### <a name="my-apps-for-android"></a>Moje aplikace pro Android

Moje aplikace pro Android je podporována v jakékoli zařízení s Androidem se systémem Android verze 4.1 nebo novější.  
Je k dispozici v hello [obchodu Google Play](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![Moje aplikace pro Android][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>Moje aplikace pro zařízení iPhone a iPad

Moje aplikace pro iOS je podporována na jakékoli zařízení iPhone nebo iPad, který používá verzi iOS 7 nebo novější.  
Je k dispozici v hello [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![Moje aplikace pro iOS][4]    



## <a name="managed-browser-for-my-apps"></a>Spravovaný prohlížeč pro Moje aplikace

Moje aplikace je taky integrovaná v hello Intune Managed Browser. Hello Intune Managed Browser pro iOS a zařízení se systémem Android hraje důležitou roli v zajistíte, že jejího zabezpečení dat na mobilních zařízeních. Umožňuje bezpečně a přejděte na webové stránky, které může obsahovat informace o společnosti a poskytuje zabezpečené procházení webu prostředí.  
Pro vás rychlý přístup toomy aplikace na domovské stránce Managed Browser a ve své záložky budete méně klikne tooreach libovolné aplikace tooaccess.

Je k dispozici v hello [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) a [Google Play Storu](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![Mananged prohlížeče pro Moje aplikace][5]    





## <a name="tips-for-testing-hello-user-experience"></a>Tipy pro testování hello činnost koncového uživatele

Pokud jste správcem Azure a jste přihlášeni toohello portálu Azure pomocí účtu v adresáři hello, jste automaticky přihlášeni toohello přístupový panel jako aktuálního účtu. V takovém případě se zobrazí všechny aplikace, které byly přiřazeny tooyou.

**tootest jako *různých* uživatelský účet:**

1. V nabídce hello uživatele v pravém horním rohu hello hello portál Azure nebo hello přístupového panelu a pak vyberte **Odhlásit**. 
2. Přejděte toohello [přístupový panel](http://myapps.microsoft.com).
3. Na přihlašovací stránku hello, typ hello uživatelské jméno a heslo pro účet hello ve vašem adresáři chcete tootest.


## <a name="starting-applications"></a>Spuštění aplikace

Několik typů aplikací, se může zobrazit na panel přístupu hello.

### <a name="office-365-applications"></a>Aplikace Office 365

Pokud vaše organizace používá Office 365 aplikace a máte licenci pro ně, zobrazí se aplikace hello Office 365 na přístupový panel.

Když kliknete dlaždici aplikace pro aplikaci Office 365, jste přesměrovaného toohello aplikace a automaticky přihlášeni.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Společnost Microsoft a třetích stran aplikace, které jsou nakonfigurované pomocí jednotného přihlašování na základě federation

Správce můžete přidat aplikace v hello části hello portál Azure Active Directory s režimem jednotné přihlašování hello nastavit příliš**Azure AD jednotné přihlašování**. Tyto aplikace lze zobrazit pouze pokud vám správce udělil explicitně přístup toohello aplikace.

Když kliknete na dlaždici pro jednu z těchto aplikací, jsou přesměrování a automaticky přihlášeni v toohello aplikaci.

### <a name="password-based-sso-without-identity-provisioning"></a>Založené na heslech jednotné přihlašování bez zřizování identity

Správce můžete přidat aplikace v hello části hello portál Azure Active Directory s režimem jednotné přihlašování hello nastavit příliš**založené na heslech jednotné přihlašování**. Všechny aplikace, které jsou nakonfigurované v tomto režimu mohou prohlížet všichni uživatelé v adresáři hello.

Hello poprvé, můžete kliknutím na některou dlaždici pro jednu z těchto aplikací, jsou výzvami tooinstall hello heslo jednotného přihlašování k modulu plug-in pro aplikace Internet Explorer nebo Chrome. Hello instalace může vyžadovat jste toorestart webového prohlížeče. Když vraťte toohello přístupového panelu a klikněte na dlaždici aplikace hello znovu, budete vyzváni k uživatelské jméno a heslo pro aplikaci hello. Když zadáte uživatelské jméno a heslo, tyto přihlašovací údaje jsou bezpečně uložena a propojit tooyour účet ve službě Azure AD.

Hello příštím kliknutí na dlaždici aplikace hello se automaticky přihlásíte toohello aplikace.  
Nemáte tooenter vaše přihlašovací údaje znovu a nebo nainstalovat hello heslo jednotného přihlašování k modulu plug-in.

Pokud vaše přihlašovací údaje se v hello cílová třetích stran aplikace změnily, je nutné také aktualizovat svoje přihlašovací údaje, které jsou uložené ve službě Azure AD. 

**tooupdate přihlašovací údaje:**

1. Vyberte ikonu hello na dlaždici aplikace hello.
2. Vyberte **aktualizovat přihlašovací údaje** tooreenter hello uživatelské jméno a heslo pro hello aplikace.


### <a name="password-based-sso-with-identity-provisioning"></a>Založené na heslech jednotného přihlašování k při zřizování identity

Správce můžete přidat aplikace v hello **služby Active Directory** části hello portál Azure s režimem jednotné přihlašování hello nastavit příliš**založené na heslech jednotné přihlašování**, společně s zřizování identity.

Hello poprvé, kliknete dlaždici aplikace pro jednu z těchto aplikací, jsou hello výzvami tooinstall **heslo jednotného přihlašování k modulu plug-in pro aplikace Internet Explorer nebo Chrome**. Hello instalace může vyžadovat jste toorestart webového prohlížeče.  
Když vraťte toohello přístupového panelu a klikněte na dlaždici aplikace hello znovu, se automaticky přihlásíte toohello aplikace.

Některé aplikace může vyžadovat toochange jste heslo na hello první přihlášení. Pokud vaše přihlašovací údaje se v hello cílová třetích stran aplikace změnily, je nutné také aktualizovat hello přihlašovací údaje, které jsou uložené ve službě Azure AD. 

**tooupdate přihlašovací údaje:**

1. Vyberte ikonu hello na dlaždici aplikace hello.
2. Vyberte **aktualizovat přihlašovací údaje** tooreenter hello uživatelské jméno a heslo pro hello aplikace.


### <a name="application-with-existing-sso-solutions"></a>Aplikace s stávajících řešení pro jednotné přihlašování

tooconfigure jednotné přihlašování pro aplikace hello portál Azure poskytuje třetí možnost jako **existující jednotné přihlašování**. Tato možnost umožňuje váš správce toocreate tooan aplikaci na odkaz a umístěte ji na hello panel přístupu pro vybraného uživatele.

Například pokud aplikace je nakonfigurovaná tooauthenticate uživatelů pomocí služby AD FS 2.0, Správce může použít hello **existující jednotné přihlašování** možnost toocreate tooit odkaz na panel přístupu hello. Při přístupu k hello odkaz, se ověřují pomocí služby AD FS 2.0 nebo poskytuje ať stávající aplikace hello řešení jednotného přihlašování.


## <a name="next-steps"></a>Další kroky

- toosee seznam všech témat, které jsou související tooapplication management, najdete v části hello [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md).
 
- toolearn jak toointegrate SaaS aplikace do služby Azure AD, najdete v části hello [seznam kurzů aplikace SaaS toointegrate](active-directory-saas-tutorial-list.md).
 
- toolearn Další informace o správě aplikací s Azure AD, najdete v části hello [přihlašování toosingle úvod a správa přístupu k aplikaci pomocí Azure Active Directory](active-directory-appssoaccess-whatis.md).
 
- toolearn Další informace o zřizování uživatelů, najdete v části [automatizace zřizování uživatelů a rušení zajištění tooSaaS aplikace](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
