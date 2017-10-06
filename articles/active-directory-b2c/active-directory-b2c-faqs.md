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
ms.openlocfilehash: f7857299bc3cb9d5fbe58e047818ec56741e0740
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-frequently-asked-questions-faq"></a>Azure AD B2C: Nejčastější dotazy (FAQ) 
Tato stránka odpovědi na nejčastější dotazy týkající hello Azure Active Directory (Azure AD) B2C. Kontrolovat zpět aktualizací.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Můžete použít funkce Azure AD B2C v mé existující, na základě zaměstnanec klienta Azure AD?
Azure AD a Azure AD B2C samostatný produkt nabídky a nemohou být nainstalovány v hello stejné klienta.  Klient služby Azure AD představuje organizace.  Klient služby Azure AD B2C představuje kolekci toobe identity použít s aplikacemi předávajících stran.  Pomocí vlastních zásad (ve verzi public preview) Azure AD B2C můžete vytvořit federaci tooAzure AD povolení ověřování zaměstnanců v organizaci.

### <a name="can-i-use-azure-ad-b2c-tooprovide-social-login-facebook-and-google-into-office-365"></a>Můžete použít Azure AD B2C tooprovide přihlášení prostřednictvím sociální sítě (Facebook a Google +) do Office 365?
Azure AD B2C nelze použít tooauthenticate uživatele pro Microsoft Office 365.  Azure AD je řešení společnosti Microsoft pro správu aplikace tooSaaS přístupu zaměstnanců a má funkcí navržených pro tento účel třeba licencování a podmíněného přístupu.  Azure AD B2C poskytuje platformu správy identit a přístupu pro vytváření webových a mobilních aplikací.  Pokud Azure AD B2C je klient Azure AD tooan nakonfigurované toofederate, klient Azure AD hello spravuje tooapplications přístupu zaměstnanců, která závisí na Azure AD B2C.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Jaké jsou místní účty v Azure AD B2C? Jak budou liší od pracovní nebo školní účty ve službě Azure AD?
V klientovi služby Azure AD, uživatelé, kteří patří toohello klienta Přihlaste se pomocí e-mailovou adresu ve tvaru hello `<xyz>@<tenant domain>`.  Hello `<tenant domain>` mezi hello ověření domény v hello klienta nebo hello počáteční `<...>.onmicrosoft.com` domény. Tento typ účtu je pracovní nebo školní účet.

Ve klienta Azure AD B2C, většinu aplikací má uživatel hello toosign v jakékoli libovolné e-mailovou adresou (například joe@comcast.net, bob@gmail.com, sarah@contoso.com, nebo jim@live.com). Tento typ účtu je místní účet.  Také podporujeme libovolný uživatelská jména jako místní účty (například Jan, Roberta, sarah nebo jima). Vyberte jednu z těchto dvou typů místní účet konfigurací Azure AD B2C ve hello portálu Azure.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-toosupport-in-hello-future"></a>Sociální identity poskytovatelů, kteří je podporují nyní? Ty, které můžete provést plánování toosupport v hello budoucí?
Momentálně podporujeme sítě Facebook, Google +, LinkedIn, Amazon, služby Twitter (preview), WeChat (preview), Weibo (preview) a QQ (Preview). Přidáme podporou dalších zprostředkovatelů oblíbených sociálních identity na základě poptávky zákazníka.

Azure AD B2C je také přidána podpora pro [vlastní zásady](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview-custom).  Tyto [vlastní zásady](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview-custom) povolit vývojáře toocreate své vlastní zásady, aby se všechny zprostředkovatele identity podporující [OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) nebo SAML. 

Začínáme s vlastními zásadami Vyzkoušejte naše [vlastní zásady starter pack](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack).

### <a name="can-i-configure-scopes-toogather-more-information-about-consumers-from-various-social-identity-providers"></a>Je možné nakonfigurovat obory toogather Další informace o příjemci od různých poskytovatelů sociálních identity?
Ne, ale tato funkce je v našem plán. Výchozí rozsahy Hello používá pro naše podporované sadu poskytovatelů sociálních identit jsou:

* Facebook: e-mailu
* Google +: e-mailu
* Účet Microsoft: openid e-mailový profil
* Amazon: profil
* LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-toobe-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Má Moje aplikace fungovat s Azure AD B2C spustit v Azure pro něj toobe?
Ne, je možné hostovat aplikace kdekoli (v hello cloudové nebo místní). Všechny musí toointeract s Azure AD B2C je hello možnost toosend a přijímat požadavky HTTP na veřejně přístupný koncové body.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-hello-azure-portal"></a>Mám několik klientů Azure AD B2C. Jak můžete spravovat jejich na hello portál Azure?
Před otevřením, Azure AD B2C, v levé nabídce hello hello portálu Azure, je nutné přepnout do adresáře hello chcete toomanage.  Přepněte adresáře kliknutím na vaši identitu v hello pravém horním rohu stránky hello portálu Azure, a potom vyberte, že adresář v hello rozevíracího seznamu, který se zobrazí.  Krok za krokem s obrázky, naleznete v části [přejděte tooAzure AD B2C nastavení](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).

### <a name="how-do-i-customize-verification-emails-hello-content-and-hello-from-field-sent-by-azure-ad-b2c"></a>Jak přizpůsobit ověřovacích e-mailů (hello obsahu a hello "z:" pole) poslal Azure AD B2C?
Můžete použít hello [firemního brandingu funkce](../active-directory/active-directory-add-company-branding.md) toocustomize hello obsah ověřovacích e-mailů. Konkrétně lze přizpůsobit tyto dva prvky hello e-mailů:

* **Banner s logem**: Zobrazuje se v hello vpravo dole.
* **Barva pozadí**: zobrazí v horní části hello.

    ![Snímek obrazovky vlastní ověřovací e-mail](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

podpis e-mailu Hello obsahuje klienta B2C hello název, který jste zadali při prvním vytvoření klienta hello B2C. Můžete změnit název hello podle těchto pokynů:

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) jako hello správce předplatného.
1. Přejděte tooyour B2C klienta.
1. Klikněte na tlačítko hello **konfigurace** kartě.
1. Změna hello **název** pole v části hello **vlastnosti Directory** části.
1. Klikněte na tlačítko **Uložit** v hello dolní části stránky hello.

V současné době neexistuje žádný způsob, jak toochange hello "z:" na hello e-mailu. Hlasovat o [feedback.azure.com](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails) vás zajímá přizpůsobení textu hello hello ověření e-mailů.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-tooazure-ad-b2c"></a>Jak můžete migrovat mé existující uživatelská jména, hesla a profilů z mé databáze tooAzure AD B2C?
Toowrite hello Azure AD Graph API můžete použít nástroj pro migraci. V tématu hello [rozhraní Graph API ukázkový](active-directory-b2c-devquickstarts-graph-dotnet.md) podrobnosti.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Jaké zásady hesel se používá pro místní účty v Azure AD B2C?
Hello zásady hesel Azure AD B2C pro místní účty je založena na hello zásady pro Azure AD. Azure AD B2C je registrace, registrace nebo přihlášení a heslo resetovat síly "silné" hesla hello používá zásady a není vypršení platnosti hesla. Čtení hello [zásady hesel služby Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx) další podrobnosti.

### <a name="can-i-use-azure-ad-connect-toomigrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-tooazure-ad-b2c"></a>Můžete použít Azure AD Connect toomigrate uživatelských identit, které jsou uložené na Moje tooAzure místní služby Active Directory AD B2C?
Ne, Azure AD Connect není navrženou toowork s Azure AD B2C. Zvažte použití hello [rozhraní Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) pro migraci uživatele.

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Můžete otevřít mé aplikace do Azure AD B2C stránky v rámci elementu iFrame?
Ne, z důvodů zabezpečení Azure AD B2C stránky nelze otevřít v rámci elementu iFrame.  Naše služba komunikuje s prvky IFRAME tooprohibit hello prohlížeče.  obecně Hello zabezpečení komunity a hello specifikací OAUTH2, nedoporučujeme používání prvky IFrame pro činnosti identity z důvodu toohello riziko opěry pro klikněte na tlačítko.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Funguje s CRM systémů, jako jsou Microsoft Dynamics Azure AD B2C?
Integrace s Microsoft Dynamics 365 Portal je k dispozici.  V tématu [toouse konfigurace Dynamics 365 portálu Azure AD B2C pro ověřování](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/portals/azure-ad-b2c).

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Nemá Azure AD B2C práce s místní SharePoint 2016 nebo starší?
Azure AD B2C není určena pro hello SharePoint externí sdílení partnera scénář; v tématu [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) místo.

### <a name="should-i-use-azure-ad-b2c-or-b2b-toomanage-external-identities"></a>Použít Azure AD B2C nebo B2B toomanage externí identity?
Přečtěte si tento článek o [externí identity](../active-directory/active-directory-b2b-compare-external-identities.md) toolearn informace o použití hello příslušné funkce tooyour scénáře externí identity.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-hello-same-as-in-azure-ad-premium"></a>Jaké generování sestav a auditování funkce poskytuje Azure AD B2C? Se, zda že text hello, stejně jako v Azure AD Premium?
Ne, Azure AD B2C nepodporuje hello stejná sada sestavy jako Azure AD Premium. Ale existuje mnoho commonalities:

* Hello přihlášení sestavy poskytují záznam každé přihlášení s omezenou podrobnosti.
* Sestavy auditu jsou k dispozici v hello portál Azure, v rámci Azure Active Directory > protokoly auditu aktivity > vyberte B2C a použít filtry podle potřeby. Aktivita správce jak aktivity aplikací jsou popsané. 
* Sestavy využití, pokrývajících počet uživatelů, počet přihlášení a objem MFA je k dispozici na [API pro vytváření sestav využití](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-usage-reporting-api)

### <a name="can-i-localize-hello-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Možné lokalizovat hello uživatelského rozhraní stránek, které obsluhuje Azure AD B2C? Jaké jazyky jsou podporovány?
Ano!  Přečtěte si informace o [jazyk přizpůsobení](active-directory-b2c-reference-language-customization.md), což je ve verzi public preview.  Poskytujeme překladů pro jazyky, 36, a můžete přepsat všechny řetězec toosuit vašim potřebám.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-hello-url-from-loginmicrosoftonlinecom-toologincontosocom"></a>Můžete použít vlastní adresy URL na stránkách Moje registrace a přihlášení, které jsou obsluhovány pomocí Azure AD B2C Například můžete změnit adresu URL hello z login.microsoftonline.com toologin.contoso.com?
Aktuálně nepodporuje. Tato funkce je v našem plán. Ověření vaší doméně v hello **domén** karta na portálu Azure classic hello není dosažení tohoto cíle.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Jak se odstraním klienta Azure AD B2C?
Postupujte podle těchto kroků toodelete vašeho klienta Azure AD B2C:

1. Postupujte podle těchto kroků příliš[přejděte tooAzure AD B2C nastavení](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.
1. Přejděte toohello **aplikace**, **zprostředkovatelů Identity**, a **všechny zásady** a odstraňte všechny položky hello v každé z nich.
1. Teď přihlášení toohello [portál Azure classic](https://manage.windowsazure.com/) jako hello správce předplatného. (Použít hello stejný pracovní nebo školní účet nebo hello stejný účet Microsoft, kterou jste použili toosign pro službu Azure.)
1. Přejděte toohello rozšíření Active Directory na levé straně hello a klikněte na svého klienta B2C.
1. Klikněte na tlačítko hello **uživatelé** kartě.
1. Vyberte každého uživatele v zapnout (vyloučení hello správce předplatného uživatele, kterého jste přihlášeni jako). Klikněte na tlačítko **odstranit** v dolní části hello stránku hello a klikněte na tlačítko **Ano** po zobrazení výzvy.
1. Klikněte na tlačítko hello **aplikace** kartě.
1. Vyberte **aplikace Moje společnost vlastní** v hello **zobrazit** pole rozevíracího seznamu a klikněte na tlačítko hello zaškrtnutí.
1. Volá se aplikace **aplikace b2c rozšíření**. Klikněte na tlačítko **odstranit** v dolní části hello stránku hello a klikněte na tlačítko **Ano** po zobrazení výzvy.
1. Znovu přejděte toohello rozšíření Active Directory a vyberte svého klienta B2C.
1. Klikněte na tlačítko **odstranit** v hello dolní části stránky hello. toocomplete hello proces, postupujte podle pokynů hello na úvodní obrazovka.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Můžete získat Azure AD B2C jako součást sady Enterprise Mobility Suite?
Ne, Azure AD B2C je průběžnými platbami služby Azure a není součástí sady Enterprise Mobility Suite.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Jak mohu ohlásit problémy s Azure AD B2C?
V tématu [soubor žádosti o podporu pro Azure Active Directory B2C](active-directory-b2c-support.md).
