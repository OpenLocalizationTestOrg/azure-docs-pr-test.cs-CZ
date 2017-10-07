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
# <a name="how-tooget-appsource-certified-for-azure-active-directory"></a><span data-ttu-id="5716a-103">Jak tooget AppSource certifikované pro Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5716a-103">How tooget AppSource Certified for Azure Active Directory</span></span>
<span data-ttu-id="5716a-104">[Microsoft AppSource](https://appsource.microsoft.com/) je cíl pro obchodní uživatelé toodiscover, zkuste a spravovat-obchodní aplikace SaaS (samostatný SaaS a rozšíření tooexisting Microsoft produktech SaaS).</span><span class="sxs-lookup"><span data-stu-id="5716a-104">[Microsoft AppSource](https://appsource.microsoft.com/) is a destination for business users toodiscover, try, and manage line-of-business SaaS applications (standalone SaaS and add-on tooexisting Microsoft SaaS products).</span></span>

<span data-ttu-id="5716a-105">toolist samostatnou aplikaci SaaS na AppSource, aplikace musí přijmout jednotné přihlašování z pracovních účtů v jakémkoli společnosti nebo organizace, která má Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5716a-105">toolist a standalone SaaS application on AppSource, your application must accept single sign-on from work accounts from any company or organization that has Azure Active Directory.</span></span> <span data-ttu-id="5716a-106">Hello přihlašovací proces musí používat hello [OpenID Connect](./active-directory-protocols-openid-connect-code.md) nebo [OAuth 2.0](./active-directory-protocols-oauth-code.md) protokoly.</span><span class="sxs-lookup"><span data-stu-id="5716a-106">hello sign-in process must use hello [OpenID Connect](./active-directory-protocols-openid-connect-code.md) or [OAuth 2.0](./active-directory-protocols-oauth-code.md) protocols.</span></span> <span data-ttu-id="5716a-107">Integrace SAML nebyla přijata AppSource certifikaci.</span><span class="sxs-lookup"><span data-stu-id="5716a-107">SAML integration is not accepted for AppSource certification.</span></span>

## <a name="guides-and-code-samples"></a><span data-ttu-id="5716a-108">Vodítka a ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="5716a-108">Guides and code samples</span></span>
<span data-ttu-id="5716a-109">Pokud chcete toolearn o tom, jak toointegrate vaší aplikace pomocí Azure Active Directory pomocí Open ID připojení, postupujte podle naše příručky a ukázky v hello kódu [Příručka pro vývojáře Azure Active Directory](./active-directory-developers-guide.md#get-started "Začínáme s Azure AD pro vývojáře").</span><span class="sxs-lookup"><span data-stu-id="5716a-109">If you want toolearn about how toointegrate your application with Azure Active Directory using Open ID connect, follow our guides and code samples in hello [Azure Active Directory developer's guide](./active-directory-developers-guide.md#get-started "Get Started with Azure AD for developers").</span></span>

## <a name="multi-tenant-applications"></a><span data-ttu-id="5716a-110">Víceklientským aplikacím</span><span class="sxs-lookup"><span data-stu-id="5716a-110">Multi-tenant applications</span></span>

<span data-ttu-id="5716a-111">Aplikace, která přijímá přihlášení od uživatelů v jakémkoli společnosti nebo organizace, které mají Azure Active Directory bez nutnosti samostatné instanci, konfigurace nebo nasazení se označuje jako *víceklientské aplikace*.</span><span class="sxs-lookup"><span data-stu-id="5716a-111">An application that accepts sign-ins from users from any company or organization that have Azure Active Directory without requiring a separate instance, configuration, or deployment is known as a *multi-tenant application*.</span></span> <span data-ttu-id="5716a-112">AppSource doporučuje, že aplikace implementovat víceklientské tooenable hello *jedním kliknutím* volné zkušební verze.</span><span class="sxs-lookup"><span data-stu-id="5716a-112">AppSource recommends that applications implement multi-tenancy tooenable hello *single-click* free trial experience.</span></span>

<span data-ttu-id="5716a-113">V pořadí tooenable víceklientská architektura ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="5716a-113">In order tooenable multi-tenancy on your application:</span></span>
- <span data-ttu-id="5716a-114">Nastavit `Multi-Tenanted` vlastnost příliš`Yes` na informace o registraci aplikace v hello [portálu Azure](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (ve výchozím nastavení jsou aplikace vytvořené v hello portálu Azure nakonfigurované jako *single klienta*)</span><span class="sxs-lookup"><span data-stu-id="5716a-114">Set `Multi-Tenanted` property too`Yes` on your application registration's information in hello [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (by default, applications created in hello Azure Portal are configured as *single-tenant*)</span></span>
- <span data-ttu-id="5716a-115">Aktualizace vašeho kódu toosend požadavky toohello '`common`se koncový bod (aktualizovat koncový bod hello z *https://login.microsoftonline.com/ {yourtenant}* příliš*https://login.microsoftonline.com/common*)</span><span class="sxs-lookup"><span data-stu-id="5716a-115">Update your code toosend requests toohello '`common`' endpoint (update hello endpoint from *https://login.microsoftonline.com/{yourtenant}* too*https://login.microsoftonline.com/common*)</span></span>
- <span data-ttu-id="5716a-116">Pro některé platformy, jako je ASP.NET, je nutné také tooupdate tooaccept váš kód více vystavitelů</span><span class="sxs-lookup"><span data-stu-id="5716a-116">For some platforms, like ASP.NET, you need also tooupdate your code tooaccept multiple issuers</span></span>

<span data-ttu-id="5716a-117">Další informace o víceklientské najdete v tématu: [jak toosign v jakékoli pomocí uživatele Azure Active Directory (AD) hello vzor aplikací víceklientské](./active-directory-devhowto-multi-tenant-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5716a-117">For more information about multi-tenancy, see: [How toosign in any Azure Active Directory (AD) user using hello multi-tenant application pattern](./active-directory-devhowto-multi-tenant-overview.md).</span></span>

### <a name="single-tenant-applications"></a><span data-ttu-id="5716a-118">Single klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="5716a-118">Single-tenant applications</span></span>
<span data-ttu-id="5716a-119">Aplikace, které přijímají pouze přihlášení z uživatelů definovaných instance služby Azure Active Directory se označují jako *jednoho klienta aplikace*.</span><span class="sxs-lookup"><span data-stu-id="5716a-119">Applications that only accept sign-ins from users of a defined Azure Active Directory instance are known as *single-tenant application*.</span></span> <span data-ttu-id="5716a-120">Externí uživatelé (včetně pracovní nebo školní účty z jiných organizací, nebo osobní účet) se mohou přihlásit v aplikaci klienta jedním tooa po přidání každého uživatele jako *účtu guest* instanci toohello Azure Active Directory aplikace Hello je zaregistrována.</span><span class="sxs-lookup"><span data-stu-id="5716a-120">External users (including Work or School accounts from other organizations, or personal account) can sign in tooa single-tenant application after adding each user as *guest account* toohello Azure Active Directory instance that hello application is registered.</span></span> <span data-ttu-id="5716a-121">Uživatele můžete přidat jako hosta účty tooan Azure Active Directory prostřednictvím hello [ *spolupráce Azure AD B2B* ](../active-directory-b2b-what-is-azure-ad-b2b.md) - a je možné ji provést [programově](../active-directory-b2b-code-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5716a-121">You can add users as guest accounts tooan Azure Active Directory via hello [*Azure AD B2B collaboration*](../active-directory-b2b-what-is-azure-ad-b2b.md) - and it can be done [programatically](../active-directory-b2b-code-samples.md).</span></span> <span data-ttu-id="5716a-122">Když přidáte uživatele jako tooan účet hosta Azure Active Directory, je odeslána e-mailová pozvánka toohello uživateli, který má tooaccept hello pozvánku kliknutím na odkaz hello v e-mailová pozvánka hello.</span><span class="sxs-lookup"><span data-stu-id="5716a-122">When you add a user as guest account tooan Azure Active Directory, an invitation email is sent toohello user, who has tooaccept hello invitation by clicking on hello link in hello invitation email.</span></span> <span data-ttu-id="5716a-123">Pozvánek odesílaných tooan další uživatele v organizaci pozváním, která je také členem hello organizaci partnera poskytujícího prostředky není požadováno tooaccept toosign pozvání v.</span><span class="sxs-lookup"><span data-stu-id="5716a-123">Invitations that are sent tooan additional user in an inviting organization that is also a member of hello partner organization are not required tooaccept an invitation toosign in.</span></span>

<span data-ttu-id="5716a-124">Jednoho klienta aplikace můžete povolit hello *kontaktujte mi* prostředí, ale pokud chcete tooenable hello jedním kliknutím / bezplatné zkušební prostředí, které doporučuje AppSource, povolit více klientů ve vaší aplikaci místo.</span><span class="sxs-lookup"><span data-stu-id="5716a-124">Single-tenant applications can enable hello *Contact Me* experience, but if you want tooenable hello single-click/ free trial experience that AppSource recommends, enable multi-tenancy on your application instead.</span></span>


## <a name="appsource-trial-experiences"></a><span data-ttu-id="5716a-125">AppSource zkušební prostředí</span><span class="sxs-lookup"><span data-stu-id="5716a-125">AppSource trial experiences</span></span>

### <a name="free-trial-customer-led-trial-experience"></a><span data-ttu-id="5716a-126">Bezplatná zkušební verze (vedla zákazníka zkušební prostředí)</span><span class="sxs-lookup"><span data-stu-id="5716a-126">Free Trial (Customer-led trial experience)</span></span> 
<span data-ttu-id="5716a-127">Hello *vedla zákazníka zkušební verze* je hello prostředí, které doporučuje AppSource jako nabízí tooyour aplikace access jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="5716a-127">hello *customer-led trial* is hello experience that AppSource recommends as it offers a single-click access tooyour application.</span></span> <span data-ttu-id="5716a-128">Níže ilustraci toho, jak toto prostředí vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="5716a-128">Below an illustration of how this experience looks like:</span></span><br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li><span data-ttu-id="5716a-129">Uživatel vyhledá aplikace ve AppSource webu</span><span class="sxs-lookup"><span data-stu-id="5716a-129">User finds your application in AppSource Web Site</span></span></li><li><span data-ttu-id="5716a-130">Vybere možnost "bezplatnou zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="5716a-130">Selects ‘Free trial’ option</span></span></li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li><span data-ttu-id="5716a-131">AppSource přesměruje uživatele tooa URL vašeho webu</span><span class="sxs-lookup"><span data-stu-id="5716a-131">AppSource redirects user tooa URL in your web site</span></span></li><li><span data-ttu-id="5716a-132">Webový server spustí hello <i>jednotného přihlašování</i> zpracování automaticky (podle načtení stránky)</span><span class="sxs-lookup"><span data-stu-id="5716a-132">Your web site starts hello <i>single-sign-on</i> process automatically (on page load)</span></span></li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li><span data-ttu-id="5716a-133">Uživatel je přesměrovaného tooMicrosoft přihlašovací stránka</span><span class="sxs-lookup"><span data-stu-id="5716a-133">User is redirected tooMicrosoft Sign-in page</span></span></li><li><span data-ttu-id="5716a-134">Uživatel zadá přihlašovací údaje toosign v</span><span class="sxs-lookup"><span data-stu-id="5716a-134">User provides credentials toosign in</span></span></li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li><span data-ttu-id="5716a-135">Uživatel dává souhlasu pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="5716a-135">User gives consent for your application</span></span></li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li><span data-ttu-id="5716a-136">Dokončení přihlášení a uživatele je přesměrovaného back tooyour webu</span><span class="sxs-lookup"><span data-stu-id="5716a-136">Sign-in completes and user is redirected back tooyour web site</span></span></li><li><span data-ttu-id="5716a-137">Uživatel spustí hello bezplatné zkušební verze</span><span class="sxs-lookup"><span data-stu-id="5716a-137">User starts hello free trial</span></span></li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a><span data-ttu-id="5716a-138">Obraťte se na mě (vedla partnera zkušební prostředí)</span><span class="sxs-lookup"><span data-stu-id="5716a-138">Contact Me (Partner-led trial experience)</span></span>
<span data-ttu-id="5716a-139">Hello *partner zkušební verze* lze použít při ruční nebo dlouhodobé operace musí toohappen tooprovision hello uživatele / společnosti: například vaše aplikace potřebuje tooprovision virtuální počítače, instancí databáze, nebo operace, které trvat toocomplete mnoho času.</span><span class="sxs-lookup"><span data-stu-id="5716a-139">hello *partner trial experience* can be used when a manual or a long-term operation needs toohappen tooprovision hello user/ company: for example, your application needs tooprovision virtual machines, database instances, or operations that take much time toocomplete.</span></span> <span data-ttu-id="5716a-140">V takovém případě po uživatel vybere hello *'žádosti o zkušební verzi,* tlačítko a vyplňování formuláře, AppSource zasílá hello kontaktní údaje uživatele.</span><span class="sxs-lookup"><span data-stu-id="5716a-140">In this case, after user selects hello *'Request Trial'* button and fills out a form, AppSource sends you hello user's contact information.</span></span> <span data-ttu-id="5716a-141">Po přijetí těchto informací, pak zřídíte hello prostředí a odeslat hello pokyny toohello uživatele na tom, jak tooaccess hello zkušební verze:</span><span class="sxs-lookup"><span data-stu-id="5716a-141">Upon receiving this information, you then provision hello environment and send hello instructions toohello user on how tooaccess hello trial experience:</span></span><br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li><span data-ttu-id="5716a-142">Vyhledá uživatele vaší aplikace AppSource webu</span><span class="sxs-lookup"><span data-stu-id="5716a-142">User finds your application in AppSource web site</span></span></li><li><span data-ttu-id="5716a-143">Vybere možnost, obraťte se na mě.</span><span class="sxs-lookup"><span data-stu-id="5716a-143">Selects ‘Contact Me’ option</span></span></li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li><span data-ttu-id="5716a-144">Vyplňování formuláře s kontaktní informace</span><span class="sxs-lookup"><span data-stu-id="5716a-144">Fills out a form with contact information</span></span></li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td><span data-ttu-id="5716a-145">Zobrazí informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="5716a-145">You receive user information</span></span></td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td><span data-ttu-id="5716a-146">Nastavení prostředí</span><span class="sxs-lookup"><span data-stu-id="5716a-146">Setup environment</span></span></td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td><span data-ttu-id="5716a-147">Kontaktujte uživatele s informacemi zkušební verze</span><span class="sxs-lookup"><span data-stu-id="5716a-147">Contact user with trial info</span></span></td>
        </tr>
        </table><br/><br/>
        <ul><li><span data-ttu-id="5716a-148">Zobrazí informace a instalace zkušební verze instance uživatele</span><span class="sxs-lookup"><span data-stu-id="5716a-148">You receive user's information and setup trial instance</span></span></li><li><span data-ttu-id="5716a-149">Odeslat hello hyperlink tooaccess toohello uživatelů vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="5716a-149">You send hello hyperlink tooaccess your application toohello user</span></span></li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li><span data-ttu-id="5716a-150">Uživatel získá přístup k aplikaci a dokončení hello jednotného přihlašování procesu</span><span class="sxs-lookup"><span data-stu-id="5716a-150">User accesses your application and complete hello single-sign-on process</span></span></li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li><span data-ttu-id="5716a-151">Uživatel dává souhlasu pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="5716a-151">User gives consent for your application</span></span></li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li><span data-ttu-id="5716a-152">Dokončení přihlášení a uživatele je přesměrovaného back tooyour webu</span><span class="sxs-lookup"><span data-stu-id="5716a-152">Sign-in completes and user is redirected back tooyour web site</span></span></li><li><span data-ttu-id="5716a-153">Uživatel spustí hello bezplatné zkušební verze</span><span class="sxs-lookup"><span data-stu-id="5716a-153">User starts hello free trial</span></span></li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a><span data-ttu-id="5716a-154">Další informace</span><span class="sxs-lookup"><span data-stu-id="5716a-154">More information</span></span>
<span data-ttu-id="5716a-155">Další informace o hello AppSource zkušební prostředí najdete v tématu [toto video](https://aka.ms/trialexperienceforwebapps).</span><span class="sxs-lookup"><span data-stu-id="5716a-155">For more information about hello AppSource trial experience, see [this video](https://aka.ms/trialexperienceforwebapps).</span></span> 
 
## <a name="next-steps"></a><span data-ttu-id="5716a-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5716a-156">Next Steps</span></span>

- <span data-ttu-id="5716a-157">Další informace o vytváření aplikace, které podporují přihlášení Azure Active Directory, naleznete v části [scénáře ověřování pro Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)</span><span class="sxs-lookup"><span data-stu-id="5716a-157">For more information on building applications that support Azure Active Directory sign-ins, see [Authentication Scenarios for Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)</span></span> 

- <span data-ttu-id="5716a-158">Informace o tom, jak toolist aplikace SaaS v AppSource, přejděte v tématu [informace o partnerovi AppSource](https://appsource.microsoft.com/partners)</span><span class="sxs-lookup"><span data-stu-id="5716a-158">For information on how toolist your SaaS application in AppSource, go see [AppSource Partner Information](https://appsource.microsoft.com/partners)</span></span>


## <a name="get-support"></a><span data-ttu-id="5716a-159">Získat podporu</span><span class="sxs-lookup"><span data-stu-id="5716a-159">Get Support</span></span>
<span data-ttu-id="5716a-160">Integrace služby Azure Active Directory, používáme [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) s podporou tooprovide komunity hello.</span><span class="sxs-lookup"><span data-stu-id="5716a-160">For Azure Active Directory integration, we use [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) with hello community tooprovide support.</span></span> 

<span data-ttu-id="5716a-161">Důrazně doporučujeme nejprve vaše dotazy posílejte na Stack Overflow a procházet existující toosee problémy, pokud někdo požádal váš dotaz před.</span><span class="sxs-lookup"><span data-stu-id="5716a-161">We highly recommend you ask your questions on Stack Overflow first and browse existing issues toosee if someone has asked your question before.</span></span> <span data-ttu-id="5716a-162">Ujistěte se, že jsou označené svoje dotazy nebo připomínky `[azure-active-directory]`.</span><span class="sxs-lookup"><span data-stu-id="5716a-162">Make sure that your questions or comments are tagged with `[azure-active-directory]`.</span></span>

<span data-ttu-id="5716a-163">Použijte hello následující komentáře části tooprovide zpětnou vazbu a Pomozte nám vylepšit a utvářejí náš obsah.</span><span class="sxs-lookup"><span data-stu-id="5716a-163">Use hello following comments section tooprovide feedback and help us refine and shape our content.</span></span>

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->