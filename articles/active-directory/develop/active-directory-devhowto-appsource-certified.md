---
title: "aaaHow tooget AppSource certifikované pro Azure Active Directory | Microsoft Docs"
description: "Podrobnosti o tom, jak tooget aplikace AppSource certifikované pro Azure Active Directory."
services: active-directory
documentationcenter: 
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 21206407-49f8-4c0b-84d1-c25e17cd4183
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/03/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: e9f07e9220afcba1120b987122fe770fe5225eed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-appsource-certified-for-azure-active-directory"></a>Jak tooget AppSource certifikované pro Azure Active Directory
[Microsoft AppSource](https://appsource.microsoft.com/) je cíl pro obchodní uživatelé toodiscover, zkuste a spravovat-obchodní aplikace SaaS (samostatný SaaS a rozšíření tooexisting Microsoft produktech SaaS).

toolist samostatnou aplikaci SaaS na AppSource, aplikace musí přijmout jednotné přihlašování z pracovních účtů v jakémkoli společnosti nebo organizace, která má Azure Active Directory. Hello přihlašovací proces musí používat hello [OpenID Connect](./active-directory-protocols-openid-connect-code.md) nebo [OAuth 2.0](./active-directory-protocols-oauth-code.md) protokoly. Integrace SAML nebyla přijata AppSource certifikaci.

## <a name="guides-and-code-samples"></a>Vodítka a ukázky kódu
Pokud chcete toolearn o tom, jak toointegrate vaší aplikace pomocí Azure Active Directory pomocí Open ID připojení, postupujte podle naše příručky a ukázky v hello kódu [Příručka pro vývojáře Azure Active Directory](./active-directory-developers-guide.md#get-started "Začínáme s Azure AD pro vývojáře").

## <a name="multi-tenant-applications"></a>Víceklientským aplikacím

Aplikace, která přijímá přihlášení od uživatelů v jakémkoli společnosti nebo organizace, které mají Azure Active Directory bez nutnosti samostatné instanci, konfigurace nebo nasazení se označuje jako *víceklientské aplikace*. AppSource doporučuje, že aplikace implementovat víceklientské tooenable hello *jedním kliknutím* volné zkušební verze.

V pořadí tooenable víceklientská architektura ve vaší aplikaci:
- Nastavit `Multi-Tenanted` vlastnost příliš`Yes` na informace o registraci aplikace v hello [portálu Azure](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (ve výchozím nastavení jsou aplikace vytvořené v hello portálu Azure nakonfigurované jako *single klienta*)
- Aktualizace vašeho kódu toosend požadavky toohello '`common`se koncový bod (aktualizovat koncový bod hello z *https://login.microsoftonline.com/ {yourtenant}* příliš*https://login.microsoftonline.com/common*)
- Pro některé platformy, jako je ASP.NET, je nutné také tooupdate tooaccept váš kód více vystavitelů

Další informace o víceklientské najdete v tématu: [jak toosign v jakékoli pomocí uživatele Azure Active Directory (AD) hello vzor aplikací víceklientské](./active-directory-devhowto-multi-tenant-overview.md).

### <a name="single-tenant-applications"></a>Single klientské aplikace
Aplikace, které přijímají pouze přihlášení z uživatelů definovaných instance služby Azure Active Directory se označují jako *jednoho klienta aplikace*. Externí uživatelé (včetně pracovní nebo školní účty z jiných organizací, nebo osobní účet) se mohou přihlásit v aplikaci klienta jedním tooa po přidání každého uživatele jako *účtu guest* instanci toohello Azure Active Directory aplikace Hello je zaregistrována. Uživatele můžete přidat jako hosta účty tooan Azure Active Directory prostřednictvím hello [ *spolupráce Azure AD B2B* ](../active-directory-b2b-what-is-azure-ad-b2b.md) - a je možné ji provést [programově](../active-directory-b2b-code-samples.md). Když přidáte uživatele jako tooan účet hosta Azure Active Directory, je odeslána e-mailová pozvánka toohello uživateli, který má tooaccept hello pozvánku kliknutím na odkaz hello v e-mailová pozvánka hello. Pozvánek odesílaných tooan další uživatele v organizaci pozváním, která je také členem hello organizaci partnera poskytujícího prostředky není požadováno tooaccept toosign pozvání v.

Jednoho klienta aplikace můžete povolit hello *kontaktujte mi* prostředí, ale pokud chcete tooenable hello jedním kliknutím / bezplatné zkušební prostředí, které doporučuje AppSource, povolit více klientů ve vaší aplikaci místo.


## <a name="appsource-trial-experiences"></a>AppSource zkušební prostředí

### <a name="free-trial-customer-led-trial-experience"></a>Bezplatná zkušební verze (vedla zákazníka zkušební prostředí) 
Hello *vedla zákazníka zkušební verze* je hello prostředí, které doporučuje AppSource jako nabízí tooyour aplikace access jedním kliknutím. Níže ilustraci toho, jak toto prostředí vypadá takto:<br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li>Uživatel vyhledá aplikace ve AppSource webu</li><li>Vybere možnost "bezplatnou zkušební verzi.</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li>AppSource přesměruje uživatele tooa URL vašeho webu</li><li>Webový server spustí hello <i>jednotného přihlašování</i> zpracování automaticky (podle načtení stránky)</li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li>Uživatel je přesměrovaného tooMicrosoft přihlašovací stránka</li><li>Uživatel zadá přihlašovací údaje toosign v</li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li>Uživatel dává souhlasu pro vaši aplikaci</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Dokončení přihlášení a uživatele je přesměrovaného back tooyour webu</li><li>Uživatel spustí hello bezplatné zkušební verze</li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a>Obraťte se na mě (vedla partnera zkušební prostředí)
Hello *partner zkušební verze* lze použít při ruční nebo dlouhodobé operace musí toohappen tooprovision hello uživatele / společnosti: například vaše aplikace potřebuje tooprovision virtuální počítače, instancí databáze, nebo operace, které trvat toocomplete mnoho času. V takovém případě po uživatel vybere hello *'žádosti o zkušební verzi,* tlačítko a vyplňování formuláře, AppSource zasílá hello kontaktní údaje uživatele. Po přijetí těchto informací, pak zřídíte hello prostředí a odeslat hello pokyny toohello uživatele na tom, jak tooaccess hello zkušební verze:<br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li>Vyhledá uživatele vaší aplikace AppSource webu</li><li>Vybere možnost, obraťte se na mě.</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li>Vyplňování formuláře s kontaktní informace</li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td>Zobrazí informace o uživateli</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td>Nastavení prostředí</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td>Kontaktujte uživatele s informacemi zkušební verze</td>
        </tr>
        </table><br/><br/>
        <ul><li>Zobrazí informace a instalace zkušební verze instance uživatele</li><li>Odeslat hello hyperlink tooaccess toohello uživatelů vaší aplikace</li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li>Uživatel získá přístup k aplikaci a dokončení hello jednotného přihlašování procesu</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li>Uživatel dává souhlasu pro vaši aplikaci</li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Dokončení přihlášení a uživatele je přesměrovaného back tooyour webu</li><li>Uživatel spustí hello bezplatné zkušební verze</li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a>Další informace
Další informace o hello AppSource zkušební prostředí najdete v tématu [toto video](https://aka.ms/trialexperienceforwebapps). 
 
## <a name="next-steps"></a>Další kroky

- Další informace o vytváření aplikace, které podporují přihlášení Azure Active Directory, naleznete v části [scénáře ověřování pro Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios) 

- Informace o tom, jak toolist aplikace SaaS v AppSource, přejděte v tématu [informace o partnerovi AppSource](https://appsource.microsoft.com/partners)


## <a name="get-support"></a>Získat podporu
Integrace služby Azure Active Directory, používáme [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) s podporou tooprovide komunity hello. 

Důrazně doporučujeme nejprve vaše dotazy posílejte na Stack Overflow a procházet existující toosee problémy, pokud někdo požádal váš dotaz před. Ujistěte se, že jsou označené svoje dotazy nebo připomínky `[azure-active-directory]`.

Použijte hello následující komentáře části tooprovide zpětnou vazbu a Pomozte nám vylepšit a utvářejí náš obsah.

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->