---
title: "Nejčastější dotazy (FAQ) – Azure AD B2C | Microsoft Docs"
description: "Časté otázky k Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: saeeda
manager: krassk
editor: bryanla
ms.assetid: ed33c2ca-76d0-442a-abb1-8b7b7bb92d6a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeeda
ms.openlocfilehash: e8b28bc9ccc12b280b1746272519bd4c9ea9e4a4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-ad-b2c-frequently-asked-questions-faq"></a>Azure AD B2C: Nejčastější dotazy (FAQ) 
Tato stránka odpovědi časté otázky o Azure Active Directory (Azure AD) B2C. Kontrolovat zpět aktualizací.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Můžete použít funkce Azure AD B2C v mé existující, na základě zaměstnanec klienta Azure AD?
Azure AD a Azure AD B2C samostatný produkt nabídky a nemohou být nainstalovány ve stejném klientovi.  Klient služby Azure AD představuje organizace.  Klient služby Azure AD B2C reprezentuje kolekci identit pro použití s aplikacemi předávajících stran.  Pomocí vlastních zásad (ve verzi public preview) můžete vytvořit federaci Azure AD B2C ke službě Azure AD povolení ověřování zaměstnanci v organizaci.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Lze použít Azure AD B2C zajistit přihlášení prostřednictvím sociální sítě (Facebook a Google +) do Office 365?
Azure AD B2C nelze použít k ověřování uživatelů pro Microsoft Office 365.  Azure AD je řešení společnosti Microsoft pro správu přístupu zaměstnanců k aplikacím SaaS a má funkcí navržených pro tento účel třeba licencování a podmíněného přístupu.  Azure AD B2C poskytuje platformu správy identit a přístupu pro vytváření webových a mobilních aplikací.  Pokud Azure AD B2C je nakonfigurovaná pro federaci do klienta služby Azure AD, klient Azure AD spravuje přístup zaměstnanců k aplikacím, které jsou závislé na Azure AD B2C.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Jaké jsou místní účty v Azure AD B2C? Jak budou liší od pracovní nebo školní účty ve službě Azure AD?
V klientovi služby Azure AD, které uživatelům patřícím do klienta Přihlaste se pomocí e-mailovou adresu ve tvaru `<xyz>@<tenant domain>`.  `<tenant domain>` Je jedním z ověřených domén v klientovi nebo počáteční `<...>.onmicrosoft.com` domény. Tento typ účtu je pracovní nebo školní účet.

V klienta Azure AD B2C, většina aplikací, aby uživatel Přihlaste se pomocí jakékoli libovolné e-mailovou adresu (například joe@comcast.net, bob@gmail.com, sarah@contoso.com, nebo jim@live.com). Tento typ účtu je místní účet.  Také podporujeme libovolný uživatelská jména jako místní účty (například Jan, Roberta, sarah nebo jima). Vyberte jednu z těchto dvou typů místní účet konfigurací Azure AD B2C na portálu Azure.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Sociální identity poskytovatelů, kteří je podporují nyní? Ty, které plánujete podporovat v budoucnu?
Momentálně podporujeme sítě Facebook, Google +, LinkedIn, Amazon, služby Twitter (preview), WeChat (preview), Weibo (preview) a QQ (Preview). Přidáme podporou dalších zprostředkovatelů oblíbených sociálních identity na základě poptávky zákazníka.

Azure AD B2C je také přidána podpora pro [vlastní zásady](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview-custom).  Tyto [vlastní zásady](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview-custom) umožňuje vývojářům vytvářet své vlastní zásady, s všechny zprostředkovatele identity, která podporuje [OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) nebo SAML. 

Začínáme s vlastními zásadami Vyzkoušejte naše [vlastní zásady starter pack](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack).

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Můžete nakonfigurovat oborů, které chcete shromažďovat další informace o příjemci od různých poskytovatelů sociálních identity?
Ne, ale tato funkce je v našem plán. Výchozí obory, použít pro naše podporované sadu poskytovatelů sociálních identit jsou:

* Facebook: e-mailu
* Google +: e-mailu
* Účet Microsoft: openid e-mailový profil
* Amazon: profil
* LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Má Moje aplikace můžou běžet na Azure pro jeho pracovat s Azure AD B2C?
Ne, je možné hostovat aplikace kdekoli (v cloudu nebo místní). Všechny potřebné k interakci s Azure AD B2C je možnost odesílat a přijímat požadavky HTTP na veřejně přístupný koncové body.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Mám několik klientů Azure AD B2C. Jak můžete spravovat jejich na portálu Azure?
Před otevřením v levé nabídce portálu Azure, Azure AD B2C', je nutné přepnout do adresáře, které chcete spravovat.  Přepněte adresáře kliknutím na vaši identitu v pravém horním rohu stránky na portálu Azure, a potom vyberte, že adresář v rozevíracím seznamu, který se zobrazí.  Krok za krokem s obrázky, naleznete v části [přejděte do nastavení Azure AD B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Jak přizpůsobit ověřovacích e-mailů (obsah a "z:" pole) poslal Azure AD B2C?
Můžete použít [firemního brandingu funkce](../active-directory/active-directory-add-company-branding.md) přizpůsobit obsah ověřovacích e-mailů. Konkrétně lze přizpůsobit tyto dva prvky e-mailu:

* **Banner s logem**: zobrazí vpravo dole.
* **Barva pozadí**: zobrazeny nahoře.

    ![Snímek obrazovky vlastní ověřovací e-mail](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Podpis e-mailu obsahuje název klienta B2C, který jste zadali při prvním vytvoření klienta B2C. Můžete změnit název podle těchto pokynů:

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com/) jako Správce předplatného.
1. Přejděte do svého klienta B2C.
1. Klikněte na kartu **KONFIGUROVAT**.
1. Změna **název** pole v části **vlastnosti Directory** části.
1. V dolní části stránky klikněte na **Uložit**.

Aktuálně neexistuje žádný způsob, jak změnit "z:" na e-mailu. Hlasovat o [feedback.azure.com](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails) vás zajímá přizpůsobení textu ověřovací e-mail.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Jak mohu migrovat mé existující uživatelská jména, hesla a profilů z databáze do Azure AD B2C?
Azure AD Graph API můžete použít k zápisu nástroj pro migraci. Najdete v článku [rozhraní Graph API ukázkový](active-directory-b2c-devquickstarts-graph-dotnet.md) podrobnosti.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Jaké zásady hesel se používá pro místní účty v Azure AD B2C?
Zásady hesel Azure AD B2C pro místní účty je na základě zásad pro Azure AD. Azure AD B2C je registrace, registrace nebo přihlášení a heslo resetovat zásady využívá sílu "silné" heslo a nemá vypršení platnosti hesla. Pro čtení [zásady hesel služby Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx) další podrobnosti.

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Můžete použít Azure AD Connect pro migraci uživatelských identit, které jsou uložené na můj místní služby Active Directory k Azure AD B2C?
Ne, Azure AD Connect není určená pro práci s Azure AD B2C. Zvažte použití [rozhraní Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) pro migraci uživatele.

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Můžete otevřít mé aplikace do Azure AD B2C stránky v rámci elementu iFrame?
Ne, z důvodů zabezpečení Azure AD B2C stránky nelze otevřít v rámci elementu iFrame.  Naše služba komunikuje s prohlížečem, který se má vložené rámce.  Komunita zabezpečení v obecné vlastnosti a specifikace OAUTH2 se nedoporučujeme používání prvky IFrame pro činnosti identity z důvodu riziko opěry pro klikněte na tlačítko.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Funguje s CRM systémů, jako jsou Microsoft Dynamics Azure AD B2C?
Integrace s Microsoft Dynamics 365 Portal je k dispozici.  V tématu [konfigurace portálu 365 Dynamics pro ověřování pomocí Azure AD B2C](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/portals/azure-ad-b2c).

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Nemá Azure AD B2C práce s místní SharePoint 2016 nebo starší?
Azure AD B2C není určena pro SharePoint externí sdílení partnera scénář; v tématu [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) místo.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Použít Azure AD B2C nebo B2B ke správě externí identity?
Přečtěte si tento článek o [externí identity](../active-directory/active-directory-b2b-compare-external-identities.md) Další informace o použití funkce vhodné pro vaše scénáře externí identity.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Jaké generování sestav a auditování funkce poskytuje Azure AD B2C? Budou stejné jako v Azure AD Premium?
Ne, Azure AD B2C nepodporuje stejnou sadu sestavy jako Azure AD Premium. Ale existuje mnoho commonalities:

* Přihlášení sestavy poskytují záznam každé přihlášení s omezenou podrobnosti.
* Sestavy auditu jsou k dispozici na portálu Azure v rámci Azure Active Directory > protokoly auditu aktivity > vyberte B2C a použít filtry podle potřeby. Aktivita správce jak aktivity aplikací jsou popsané. 
* Sestavy využití, pokrývajících počet uživatelů, počet přihlášení a objem MFA je k dispozici na [API pro vytváření sestav využití](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-usage-reporting-api)

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Možné lokalizovat uživatelského rozhraní stránky obsluhuje Azure AD B2C? Jaké jazyky jsou podporovány?
Ano!  Přečtěte si informace o [jazyk přizpůsobení](active-directory-b2c-reference-language-customization.md), což je ve verzi public preview.  Poskytujeme překladů pro 36 jazyků a můžete přepsat libovolný řetězec tak, aby vyhovovala vašim potřebám.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Můžete použít vlastní adresy URL na stránkách Moje registrace a přihlášení, které jsou obsluhovány pomocí Azure AD B2C Například můžete změnit adresu URL z login.microsoftonline.com login.contoso.com?
Aktuálně nepodporuje. Tato funkce je v našem plán. Ověření vaší doméně v **domén** karta na portálu Azure classic není dosažení tohoto cíle.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Jak se odstraním klienta Azure AD B2C?
Použijte následující postup odstranění vašeho klienta Azure AD B2C:

1. Postupujte podle těchto kroků [přejděte do nastavení Azure AD B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na portálu Azure.
1. Přejděte na **aplikace**, **zprostředkovatelů Identity**, a **všechny zásady** a odstraňte všechny položky v každé z nich.
1. Nyní se přihlásit k [portál Azure classic](https://manage.windowsazure.com/) jako správce předplatného. (Použijte stejný pracovní nebo školní účet nebo stejný účet Microsoft, který jste použili při registraci k Azure.)
1. Přejděte na rozšíření Active Directory na levé straně a klikněte na svého klienta B2C.
1. Klikněte **uživatelé** kartě.
1. Vyberte každého uživatele v zapnout (vyloučení, který jste přihlášeni jako správce předplatného uživatele). Klikněte na tlačítko **odstranit** v dolní části stránky a klikněte na tlačítko **Ano** po zobrazení výzvy.
1. Klikněte **aplikace** kartě.
1. Vyberte **aplikace Moje společnost vlastní** v **zobrazit** pole rozevíracího seznamu a klikněte na označit kontroly.
1. Volá se aplikace **aplikace b2c rozšíření**. Klikněte na tlačítko **odstranit** v dolní části stránky a klikněte na tlačítko **Ano** po zobrazení výzvy.
1. Znovu přejděte do rozšíření Active Directory a vyberte svého klienta B2C.
1. Klikněte na tlačítko **odstranit** v dolní části stránky. Dokončete proces, postupujte podle pokynů na obrazovce.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Můžete získat Azure AD B2C jako součást sady Enterprise Mobility Suite?
Ne, Azure AD B2C je průběžnými platbami služby Azure a není součástí sady Enterprise Mobility Suite.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Jak mohu ohlásit problémy s Azure AD B2C?
V tématu [soubor žádosti o podporu pro Azure Active Directory B2C](active-directory-b2c-support.md).
