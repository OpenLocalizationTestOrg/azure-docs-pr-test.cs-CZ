---
title: "aaaAzure Active Directory s B2B spolupráce nejčastější dotazy | Microsoft Docs"
description: "Získejte odpovědi toofrequently kladené dotazy týkající se spolupráce Azure Active Directory s B2B."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 4412bbc65274ff01782db81dfcc8818a6362ea7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Spolupráce Azure Active Directory s B2B nejčastější dotazy

Tyto nejčastější dotazy (FAQ) o spolupráci Azure Active Directory (Azure AD) business-to-business (B2B) jsou pravidelně aktualizované tooinclude nová témata.

### <a name="is-azure-ad-b2b-collaboration-available-in-hello-azure-classic-portal"></a>Je k dispozici v hello portál Azure classic spolupráce Azure AD B2B?
Ne. Funkce spolupráce Azure AD s B2B jsou k dispozici pouze v hello [portál Azure](https://portal.azure.com) a v hello [přístupový Panel](https://myapps.microsoft.com/). 

### <a name="can-we-customize-our-sign-in-page-so-it-is-more-intuitive-for-our-b2b-collaboration-guest-users"></a>Jsme naše přihlašovací stránka přizpůsobit, tak, aby byl pro naše uživatele typu Host spolupráce B2B intuitivnější?
Absolutně! V tématu naše [příspěvku na blogu o této funkci](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). Další informace o tom, jak toocustomize vaší organizace je přihlašovací stránky najdete v tématu [přidání firemního brandingu toosign v a přístupového panelu](active-directory-add-company-branding.md).

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>Mohou uživatelé spolupráce B2B, získat přístup k SharePoint Online a OneDrive?
Ano. Je však hello toosearch možnost pro existující uživatele typu Host ve službě SharePoint Online pomocí výběr osob hello **vypnout** ve výchozím nastavení. nastavit tooturn na toosearch hello možnost pro existující uživatele typu Host, **ShowPeoplePickerSuggestionsForGuestUsers** příliš**na**. Toto nastavení můžete zapnout na buď na úrovni klienta hello nebo na úrovni kolekce webů hello. Toto nastavení můžete změnit pomocí rutiny Set-SPOTenant a Set-SPOSite hello. Pomocí těchto rutin může hledat členy v adresáři hello, všechny existující uživatele typu Host. Změny v obor klienta hello neovlivňují weby SharePoint Online, které již byly zřízeny.

### <a name="is-hello-csv-upload-feature-still-supported"></a>Je hello CSV odeslat funkce stále podporovány?
Ano. Další informace o používání funkce nahrávání souborů .csv hello najdete v tématu [této ukázce prostředí PowerShell](active-directory-b2b-code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>Jak můžete přizpůsobit Moje pozvánku e-mailů?
Téměř všechny údaje o procesu hello pozvánky můžete přizpůsobit pomocí hello [B2B pozvánku rozhraní API](active-directory-b2b-api.md).

### <a name="can-an-invited-external-user-leave-hello-organization-after-being-invited"></a>Můžete ponechat pozvané externího uživatele organizaci hello po pozvat?
Hello pozváním organizace správce může odstranit uživatel guest spolupráce B2B z adresáře, ale uživatel guest hello nemůžete opustit hello pozvání adresáře organizace samy o sobě. 

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Můžete resetovat uživatele typu Host jejich metoda služby Multi-Factor authentication?
Ano. Uživatele typu Host můžete resetovat jejich hello metoda služby Multi-Factor authentication, které stejný způsobem že běžní uživatelé provést.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Které organizace je zodpovědná za licence služby Multi-Factor authentication?
Hello pozváním organizace provádí ověřování Multi-Factor authentication. Hello pozvání organizace musí zajistit, že má organizace hello dostatek licencí pro své B2B uživatele, kteří používají službu Multi-Factor authentication.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Co když partnerské organizace už má nastavení služby Multi-Factor authentication? Můžete jsme důvěřovat jejich služby Multi-Factor authentication a nechcete použít vlastní vícefaktorové ověřování?
Tato funkce je plánovaná pro budoucí použití, která pak můžete vybrat konkrétní partnery tooexclude z vícefaktorového ověřování (hello pozváním organizace).

### <a name="how-can-i-use-delayed-invitations"></a>Jak můžete použít zpožděné pozvánek?
Organizace může mají uživatelé spolupráce tooadd B2B, je podle potřeby zřizovat tooapplications a pak odeslat pozvánky. Můžete použít hello B2B spolupráce pozvánku k rozhraní API toocustomize hello pracovního postupu registrace.

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Můžete vytvořit uživatele guest správce s omezeními?
Jistě. Další informace najdete v tématu [role tooa uživatelé hosta přidání](active-directory-users-assign-role-azure-portal.md).

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-tooaccess-hello-azure-portal"></a>Umožňuje spolupráci Azure AD B2B s B2B uživatelé tooaccess hello portál Azure?
Pokud uživatel není přiřazen hello role Globální správce nebo správce s omezeními, nebudou uživatelé spolupráce B2B vyžadují toohello přístup k portálu Azure. Uživatelé spolupráce B2B, s přiřazenou hello role Globální správce nebo správce s omezeními můžete přístup k portálu hello. Navíc pokud uživatel hosta, který není přiřazen jedna z těchto rolí správce má přístup k portálu hello, hello uživatel může být schopný tooaccess na určité části hello prostředí. role uživatele guest Hello má některá oprávnění v adresáři hello.

### <a name="can-i-block-access-toohello-azure-portal-for-guest-users"></a>Můžete blokovat přístup toohello portál Azure pro uživatele typu Host?
Ano! Když nakonfigurujete tuto zásadu, být opatrní tooavoid omylem blokování přístupu toomembers a správci.
tooblock uživatel guest pro přístup k toohello [portál Azure](https://portal.azure.com), použijte zásady podmíněného přístupu v hello rozhraní API pro model nasazení classic systému Windows Azure:
1. Upravit hello **všichni uživatelé** tak, aby obsahoval pouze členové skupiny.
  ![Upravit hello skupiny – snímek obrazovky](media/active-directory-b2b-faq/modify-all-users-group.png)
2. Vytvoření dynamické skupiny, která obsahuje uživatele typu Host.
  ![Vytvoření skupiny – snímek obrazovky](media/active-directory-b2b-faq/group-with-guest-users.png)
3. Nastavte podmíněného přístupu zásad tooblock hosta uživatelům přístup k portálu hello, jak je znázorněno v následující hello video:
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Podporuje spolupráci Azure AD B2B Multi-Factor authentication a příjemce e-mailové účty?
Ano. Vícefaktorové ověřování a příjemce e-mailové účty jsou podporované pro spolupráci Azure AD B2B.

### <a name="do-you-plan-toosupport-password-reset-for-azure-ad-b2b-collaboration-users"></a>Máte v plánu toosupport resetování hesla pro uživatele spolupráce Azure AD B2B?
Ano. Tady jsou důležité podrobnosti hello pro samoobslužné resetování hesla (SSPR) pro uživatele B2B, který jste pozvánku, od organizace partnera:
 
* SSPR dojde pouze v hello identity klienta hello B2B uživatele.
* Pokud hello identity klienta se účet Microsoft, použije se hello účtu Microsoft SSPR mechanismus.
* Pokud je hello identitu klienta v běhu (JIT) nebo "virální" klienta, se budou odesílat e-mailu s heslo resetovat.
* U jiných klientů je pro uživatele B2B následovaný standardní proces SSPR hello. Jako člen SSPR B2B uživatelům, v kontextu hello hello prostředku je blokována klientů. 

### <a name="is-password-reset-available-for-guest-users-in-a-just-in-time-jit-or-viral-tenant-who-accepted-invitations-with-a-work-or-school-email-address-but-who-didnt-have-a-pre-existing-azure-ad-account"></a>Je resetování hesla k dispozici pro uživatele typu Host v v běhu (JIT) nebo "virální" klientů, kteří přijali pozvánek s pracovním nebo školním e-mailovou adresu, ale který neměly existující účet Azure AD?
Ano. A heslo resetovat poštu lze odesílat své heslo umožňuje uživateli tooreset v hello JIT klientů.

### <a name="does-microsoft-dynamics-crm-provide-online-support-for-azure-ad-b2b-collaboration"></a>Poskytuje Microsoft Dynamics CRM online podpory společnosti Microsoft pro spolupráci Azure AD B2B?
V současné době Microsoft Dynamics CRM neposkytuje online podporu pro spolupráci Azure AD B2B. Ale plánujeme toosupport to v budoucnu hello.

### <a name="what-is-hello-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Co je hello životnost počátečního hesla pro nově vytvořeného uživatele spolupráce B2B?
Azure AD má pevnou sadu znaků, síly hesla a požadavky uzamčení účtu, které platí stejnou měrou tooall Azure AD cloud uživatelské účty. Cloud uživatelské účty jsou účty, které nejsou Federovaná pomocí jiného poskytovatele identity, jako například 
* Účet Microsoft
* Facebook
* Služba Active Directory Federation Services
* Jiného klienta cloudu (pro spolupráci B2B)

Pro federované účty zásady hesel závisí na hello zásady, které jsou použity v nastavení účtu Microsoft hello místní klientů a hello uživatele.

### <a name="an-organization-might-want-toohave-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-hello-presence-of-hello-identity-provider-claim-hello-correct-model-toouse"></a>Organizace může být vhodné toohave různých dojde ve svých aplikacích pro uživatele klienta a uživatele typu Host. Je k dispozici standardní pokyny pro tento? Je hello přítomnost hello identity poskytovatele deklarací identity hello správné modelu toouse?
 Uživatel typu Host můžete použít všechny tooauthenticate zprostředkovatele identity. Další informace najdete v tématu [vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md). Použití hello **UserType** vlastnost toodetermine uživatelské prostředí. Hello **UserType** deklarace identity není aktuálně součástí hello token. Aplikace by měly používat hello rozhraní Graph API tooquery hello adresář pro uživatele hello a tooget hello UserType.

### <a name="where-can-i-find-a-b2b-collaboration-community-tooshare-solutions-and-toosubmit-ideas"></a>Kde lze najít tooshare komunity spolupráce B2B řešení a nápady toosubmit?
Nemůžeme se neustále naslouchání tooyour zpětnou vazbu, tooimprove spolupráce B2B. Jsme tooshare můžete pozvat uživatele scénáře, osvědčené postupy a co se vám líbí o spolupráci Azure AD B2B. Připojit k diskuse hello v hello [Microsoft technická komunita](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
 
Můžeme také pozvat toosubmit můžete nápady a hlasů pro budoucí funkce na [nápady spolupráce B2B](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas).

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-hello-user-is-just-ready-toogo-or-does-hello-user-always-have-tooclick-through-toohello-redemption-url"></a>Můžete odeslat požadavek, který je automaticky uplatněny, který hello uživatel je právě "připraveni toogo"? Nebo uživatel hello vždy tooclick prostřednictvím adresy URL uplatnění toohello?
Pozvánek odesílaných uživatelem v hello pozvání organizace, který je taky členem skupiny organizace partnera hello nevyžadují, aby se uživatelem B2B hello.

Doporučujeme, abyste pozvání jednoho uživatele z hello partnerské organizace toojoin hello pozvání organizace. [Přidání této role uživatele guest toohello pozval vás v organizaci poskytující prostředky hello](active-directory-b2b-add-guest-to-role.md). Tohoto uživatele můžete pozvat jiných uživatelů v organizaci partnera hello pomocí přihlášení hello uživatelského rozhraní, skriptů prostředí PowerShell nebo rozhraní API. Potom B2B spolupráce uživatele z dané organizace nejsou požadované tooredeem své pozvánky.

### <a name="how-does-b2b-collaboration-work-when-hello-invited-partner-is-using-federation-tooadd-their-own-on-premises-authentication"></a>Spolupráce B2B funkce když hello pozvat partnera používá federační tooadd vlastní místního ověřování
Pokud má klient služby Azure AD, která je Federovaná hello partnera toohello místní infrastrukturu ověřování, místního jednotné přihlašování (SSO) automaticky dosáhnout. Pokud hello partner nemá klient služby Azure AD, vytvoří se účet služby Azure AD pro nové uživatele. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>Ačkoli Azure AD B2B nepřijal gmail.com a outlook.com e-mailové adresy a že B2C byla použita pro tyto typy účtů?
Jsme se odebrat hello rozdíly mezi B2B a spolupráce obchodní společnosti (B2C) z hlediska, které jsou podporovány identity. identity Hello používá není funkční důvod toochoose mezi použitím B2B a použitím B2C. Informace o výběru vaše možnosti spolupráce naleznete v tématu [spolupráce B2B porovnat a B2C v Azure Active Directory](active-directory-b2b-compare-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Jaké aplikace a služby podporují uživatele typu Host Azure B2B?
Aplikace všechny integrované s Active Directory Azure podporují uživatele typu Host Azure B2B. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>Jsme Pokud našich partnerů nemají služby Multi-Factor authentication vynutit službu Multi-Factor authentication pro uživatele typu Host B2B?
Ano. Další informace najdete v tématu [podmíněného přístupu pro uživatele spolupráce B2B](active-directory-b2b-mfa-instructions.md).

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>Ve službě SharePoint můžete definovat seznam aplikace "Povolit" nebo "deny" pro externí uživatele. Můžete jsme to udělat v Azure?
Ano. Podporuje spolupráce Azure AD s B2B povolit seznamy a seznamy zakázat. 

### <a name="what-licenses-do-we-need-toouse-azure-ad-b2b"></a>Co dělat, licence potřebujeme toouse Azure AD B2B?
Informace o co licence vaše organizace potřebuje toouse Azure AD B2B, najdete v části [spolupráce Azure Active Directory s B2B licencování pokyny](active-directory-b2b-licensing.md).

### <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Azure AD správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [elementy Hello hello e-mailová pozvánka pro spolupráci B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Řešení potíží s spolupráce Azure AD B2B](active-directory-b2b-troubleshooting.md)
* [Spolupráce B2B Azure AD rozhraní API a přizpůsobení](active-directory-b2b-api.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Rejstřík článků o správě aplikací ve službě Azure AD](active-directory-apps-index.md)
