---
title: "Jak získat AppSource certifikované pro Azure Active Directory | Microsoft Docs"
description: "Podrobnosti o tom, jak získat aplikace AppSource certifikované pro Azure Active Directory."
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
ms.openlocfilehash: d8e2f8fc19ff879e6a7b632f033fd0ed9d77392a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-get-appsource-certified-for-azure-active-directory"></a><span data-ttu-id="98ccc-103">Jak získat AppSource certifikované pro Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="98ccc-103">How to get AppSource Certified for Azure Active Directory</span></span>
<span data-ttu-id="98ccc-104">[Microsoft AppSource](https://appsource.microsoft.com/) je cíl pro firmy uživatelům zjišťovat, zkuste a spravovat-obchodní aplikace SaaS (samostatný SaaS a rozšíření na existující produktů Microsoft SaaS).</span><span class="sxs-lookup"><span data-stu-id="98ccc-104">[Microsoft AppSource](https://appsource.microsoft.com/) is a destination for business users to discover, try, and manage line-of-business SaaS applications (standalone SaaS and add-on to existing Microsoft SaaS products).</span></span>

<span data-ttu-id="98ccc-105">Pro zobrazení seznamu samostatnou aplikaci SaaS na AppSource, musíte aplikace přijmout jednotné přihlašování z pracovních účtů v jakémkoli společnosti nebo organizace, která má Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="98ccc-105">To list a standalone SaaS application on AppSource, your application must accept single sign-on from work accounts from any company or organization that has Azure Active Directory.</span></span> <span data-ttu-id="98ccc-106">Musíte použít proces přihlášení [OpenID Connect](./active-directory-protocols-openid-connect-code.md) nebo [OAuth 2.0](./active-directory-protocols-oauth-code.md) protokoly.</span><span class="sxs-lookup"><span data-stu-id="98ccc-106">The sign-in process must use the [OpenID Connect](./active-directory-protocols-openid-connect-code.md) or [OAuth 2.0](./active-directory-protocols-oauth-code.md) protocols.</span></span> <span data-ttu-id="98ccc-107">Integrace SAML nebyla přijata AppSource certifikaci.</span><span class="sxs-lookup"><span data-stu-id="98ccc-107">SAML integration is not accepted for AppSource certification.</span></span>

## <a name="guides-and-code-samples"></a><span data-ttu-id="98ccc-108">Vodítka a ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="98ccc-108">Guides and code samples</span></span>
<span data-ttu-id="98ccc-109">Pokud chcete další informace o tom, jak integrovat vaší aplikace pomocí Azure Active Directory pomocí Open ID připojení, postupujte podle naše příručky a ukázky v kódu [Příručka pro vývojáře Azure Active Directory](./active-directory-developers-guide.md#get-started "Začínáme s Azure AD pro vývojáře").</span><span class="sxs-lookup"><span data-stu-id="98ccc-109">If you want to learn about how to integrate your application with Azure Active Directory using Open ID connect, follow our guides and code samples in the [Azure Active Directory developer's guide](./active-directory-developers-guide.md#get-started "Get Started with Azure AD for developers").</span></span>

## <a name="multi-tenant-applications"></a><span data-ttu-id="98ccc-110">Víceklientským aplikacím</span><span class="sxs-lookup"><span data-stu-id="98ccc-110">Multi-tenant applications</span></span>

<span data-ttu-id="98ccc-111">Aplikace, která přijímá přihlášení od uživatelů v jakémkoli společnosti nebo organizace, které mají Azure Active Directory bez nutnosti samostatné instanci, konfigurace nebo nasazení se označuje jako *víceklientské aplikace*.</span><span class="sxs-lookup"><span data-stu-id="98ccc-111">An application that accepts sign-ins from users from any company or organization that have Azure Active Directory without requiring a separate instance, configuration, or deployment is known as a *multi-tenant application*.</span></span> <span data-ttu-id="98ccc-112">AppSource doporučuje, že aplikace implementovat víceklientské povolit *jedním kliknutím* volné zkušební verze.</span><span class="sxs-lookup"><span data-stu-id="98ccc-112">AppSource recommends that applications implement multi-tenancy to enable the *single-click* free trial experience.</span></span>

<span data-ttu-id="98ccc-113">Chcete-li povolit více klientů ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="98ccc-113">In order to enable multi-tenancy on your application:</span></span>
- <span data-ttu-id="98ccc-114">Nastavit `Multi-Tenanted` vlastnost `Yes` na informace o registraci aplikace v [portálu Azure](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (ve výchozím nastavení jsou aplikace vytvořené na portálu Azure nakonfigurovaný jako *jednoho klienta*)</span><span class="sxs-lookup"><span data-stu-id="98ccc-114">Set `Multi-Tenanted` property to `Yes` on your application registration's information in the [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (by default, applications created in the Azure Portal are configured as *single-tenant*)</span></span>
- <span data-ttu-id="98ccc-115">Aktualizujte kód a odeslání žádosti o '`common`se koncový bod (aktualizaci koncového bodu z *https://login.microsoftonline.com/ {yourtenant}* k *https://login.microsoftonline.com/common*)</span><span class="sxs-lookup"><span data-stu-id="98ccc-115">Update your code to send requests to the '`common`' endpoint (update the endpoint from *https://login.microsoftonline.com/{yourtenant}* to *https://login.microsoftonline.com/common*)</span></span>
- <span data-ttu-id="98ccc-116">Pro některé platformy, jako je ASP.NET budete muset taky aktualizovat kód tak, aby přijímal více vystavitelů</span><span class="sxs-lookup"><span data-stu-id="98ccc-116">For some platforms, like ASP.NET, you need also to update your code to accept multiple issuers</span></span>

<span data-ttu-id="98ccc-117">Další informace o víceklientské najdete v tématu: [jak se přihlásit žádné uživatele Azure Active Directory (AD) pomocí vzoru víceklientské aplikace](./active-directory-devhowto-multi-tenant-overview.md).</span><span class="sxs-lookup"><span data-stu-id="98ccc-117">For more information about multi-tenancy, see: [How to sign in any Azure Active Directory (AD) user using the multi-tenant application pattern](./active-directory-devhowto-multi-tenant-overview.md).</span></span>

### <a name="single-tenant-applications"></a><span data-ttu-id="98ccc-118">Single klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="98ccc-118">Single-tenant applications</span></span>
<span data-ttu-id="98ccc-119">Aplikace, které přijímají pouze přihlášení z uživatelů definovaných instance služby Azure Active Directory se označují jako *jednoho klienta aplikace*.</span><span class="sxs-lookup"><span data-stu-id="98ccc-119">Applications that only accept sign-ins from users of a defined Azure Active Directory instance are known as *single-tenant application*.</span></span> <span data-ttu-id="98ccc-120">Externí uživatelé (včetně pracovní nebo školní účty z jiných organizací, nebo osobní účet) se můžete přihlásit k aplikaci jednoho klienta po přidání každého uživatele jako *účtu guest* k instanci služby Azure Active Directory, že aplikace je zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="98ccc-120">External users (including Work or School accounts from other organizations, or personal account) can sign in to a single-tenant application after adding each user as *guest account* to the Azure Active Directory instance that the application is registered.</span></span> <span data-ttu-id="98ccc-121">Jako účet hosta. můžete přidat uživatele do služby Azure Active Directory pomocí [ *spolupráce Azure AD B2B* ](../active-directory-b2b-what-is-azure-ad-b2b.md) - a je možné ji provést [programově](../active-directory-b2b-code-samples.md).</span><span class="sxs-lookup"><span data-stu-id="98ccc-121">You can add users as guest accounts to an Azure Active Directory via the [*Azure AD B2B collaboration*](../active-directory-b2b-what-is-azure-ad-b2b.md) - and it can be done [programatically](../active-directory-b2b-code-samples.md).</span></span> <span data-ttu-id="98ccc-122">Když přidáte uživatele jako účet guest do Azure Active Directory, pozvánka e-mail je odeslán uživateli, který má k přijetí pozvánky kliknutím na odkaz v e-mailová pozvánka.</span><span class="sxs-lookup"><span data-stu-id="98ccc-122">When you add a user as guest account to an Azure Active Directory, an invitation email is sent to the user, who has to accept the invitation by clicking on the link in the invitation email.</span></span> <span data-ttu-id="98ccc-123">Pozvánky, které se odesílají do dalšího uživatele v organizaci pozváním, která je také členem partnerské organizace nejsou muset přijmout pozvánku k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="98ccc-123">Invitations that are sent to an additional user in an inviting organization that is also a member of the partner organization are not required to accept an invitation to sign in.</span></span>

<span data-ttu-id="98ccc-124">Můžete povolit jeden klienta aplikace *kontaktujte mi* prostředí, ale pokud chcete povolit jedním kliknutím / bezplatné zkušební prostředí, které doporučuje AppSource, povolte víceklientský ve vaší aplikaci místo.</span><span class="sxs-lookup"><span data-stu-id="98ccc-124">Single-tenant applications can enable the *Contact Me* experience, but if you want to enable the single-click/ free trial experience that AppSource recommends, enable multi-tenancy on your application instead.</span></span>


## <a name="appsource-trial-experiences"></a><span data-ttu-id="98ccc-125">AppSource zkušební prostředí</span><span class="sxs-lookup"><span data-stu-id="98ccc-125">AppSource trial experiences</span></span>

### <a name="free-trial-customer-led-trial-experience"></a><span data-ttu-id="98ccc-126">Bezplatná zkušební verze (vedla zákazníka zkušební prostředí)</span><span class="sxs-lookup"><span data-stu-id="98ccc-126">Free Trial (Customer-led trial experience)</span></span> 
<span data-ttu-id="98ccc-127">*Vedla zákazníka zkušební verze* je prostředí, které doporučuje AppSource jako nabízí jedním kliknutím přístup k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="98ccc-127">The *customer-led trial* is the experience that AppSource recommends as it offers a single-click access to your application.</span></span> <span data-ttu-id="98ccc-128">Níže ilustraci toho, jak toto prostředí vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="98ccc-128">Below an illustration of how this experience looks like:</span></span><br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li><span data-ttu-id="98ccc-129">Uživatel vyhledá aplikace ve AppSource webu</span><span class="sxs-lookup"><span data-stu-id="98ccc-129">User finds your application in AppSource Web Site</span></span></li><li><span data-ttu-id="98ccc-130">Vybere možnost "bezplatnou zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="98ccc-130">Selects ‘Free trial’ option</span></span></li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li><span data-ttu-id="98ccc-131">AppSource přesměruje uživatele na adresu URL vašeho webu</span><span class="sxs-lookup"><span data-stu-id="98ccc-131">AppSource redirects user to a URL in your web site</span></span></li><li><span data-ttu-id="98ccc-132">Váš webový server spustí <i>jednotného přihlašování</i> zpracování automaticky (podle načtení stránky)</span><span class="sxs-lookup"><span data-stu-id="98ccc-132">Your web site starts the <i>single-sign-on</i> process automatically (on page load)</span></span></li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li><span data-ttu-id="98ccc-133">Uživatel je přesměrován na stránku přihlášení Microsoft</span><span class="sxs-lookup"><span data-stu-id="98ccc-133">User is redirected to Microsoft Sign-in page</span></span></li><li><span data-ttu-id="98ccc-134">Uživatel zadá přihlašovací údaje pro přihlášení</span><span class="sxs-lookup"><span data-stu-id="98ccc-134">User provides credentials to sign in</span></span></li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li><span data-ttu-id="98ccc-135">Uživatel dává souhlasu pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="98ccc-135">User gives consent for your application</span></span></li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li><span data-ttu-id="98ccc-136">Přihlášení dokončí a uživatel je přesměrován zpět na webové stránky</span><span class="sxs-lookup"><span data-stu-id="98ccc-136">Sign-in completes and user is redirected back to your web site</span></span></li><li><span data-ttu-id="98ccc-137">Spuštění uživatelem bezplatné zkušební verze</span><span class="sxs-lookup"><span data-stu-id="98ccc-137">User starts the free trial</span></span></li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a><span data-ttu-id="98ccc-138">Obraťte se na mě (vedla partnera zkušební prostředí)</span><span class="sxs-lookup"><span data-stu-id="98ccc-138">Contact Me (Partner-led trial experience)</span></span>
<span data-ttu-id="98ccc-139">*Partner zkušební verze* lze použít při ruční nebo dlouhodobé operaci, musí dojít ke zřízení uživatele / společnosti: například vaše aplikace potřebuje ke zřízení virtuálních počítačů, instancí databáze nebo operace, které trvat dlouho na dokončení.</span><span class="sxs-lookup"><span data-stu-id="98ccc-139">The *partner trial experience* can be used when a manual or a long-term operation needs to happen to provision the user/ company: for example, your application needs to provision virtual machines, database instances, or operations that take much time to complete.</span></span> <span data-ttu-id="98ccc-140">V takovém případě po uživatel vybere *'žádosti o zkušební verzi,* tlačítko a zadá formuláře, AppSource odešle kontaktní údaje uživatele.</span><span class="sxs-lookup"><span data-stu-id="98ccc-140">In this case, after user selects the *'Request Trial'* button and fills out a form, AppSource sends you the user's contact information.</span></span> <span data-ttu-id="98ccc-141">Po přijetí těchto informací, pak zřízení prostředí a odeslat pokyny pro uživatele o tom, jak získat přístup k zkušební prostředí:</span><span class="sxs-lookup"><span data-stu-id="98ccc-141">Upon receiving this information, you then provision the environment and send the instructions to the user on how to access the trial experience:</span></span><br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li><span data-ttu-id="98ccc-142">Vyhledá uživatele vaší aplikace AppSource webu</span><span class="sxs-lookup"><span data-stu-id="98ccc-142">User finds your application in AppSource web site</span></span></li><li><span data-ttu-id="98ccc-143">Vybere možnost, obraťte se na mě.</span><span class="sxs-lookup"><span data-stu-id="98ccc-143">Selects ‘Contact Me’ option</span></span></li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li><span data-ttu-id="98ccc-144">Vyplňování formuláře s kontaktní informace</span><span class="sxs-lookup"><span data-stu-id="98ccc-144">Fills out a form with contact information</span></span></li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td><span data-ttu-id="98ccc-145">Zobrazí informace o uživateli</span><span class="sxs-lookup"><span data-stu-id="98ccc-145">You receive user information</span></span></td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td><span data-ttu-id="98ccc-146">Nastavení prostředí</span><span class="sxs-lookup"><span data-stu-id="98ccc-146">Setup environment</span></span></td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td><span data-ttu-id="98ccc-147">Kontaktujte uživatele s informacemi zkušební verze</span><span class="sxs-lookup"><span data-stu-id="98ccc-147">Contact user with trial info</span></span></td>
        </tr>
        </table><br/><br/>
        <ul><li><span data-ttu-id="98ccc-148">Zobrazí informace a instalace zkušební verze instance uživatele</span><span class="sxs-lookup"><span data-stu-id="98ccc-148">You receive user's information and setup trial instance</span></span></li><li><span data-ttu-id="98ccc-149">Odeslat hypertextový odkaz na přístup k aplikaci pro uživatele</span><span class="sxs-lookup"><span data-stu-id="98ccc-149">You send the hyperlink to access your application to the user</span></span></li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li><span data-ttu-id="98ccc-150">Uživatel přistupuje k aplikaci a dokončíte proces jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="98ccc-150">User accesses your application and complete the single-sign-on process</span></span></li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li><span data-ttu-id="98ccc-151">Uživatel dává souhlasu pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="98ccc-151">User gives consent for your application</span></span></li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li><span data-ttu-id="98ccc-152">Přihlášení dokončí a uživatel je přesměrován zpět na webové stránky</span><span class="sxs-lookup"><span data-stu-id="98ccc-152">Sign-in completes and user is redirected back to your web site</span></span></li><li><span data-ttu-id="98ccc-153">Spuštění uživatelem bezplatné zkušební verze</span><span class="sxs-lookup"><span data-stu-id="98ccc-153">User starts the free trial</span></span></li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a><span data-ttu-id="98ccc-154">Další informace</span><span class="sxs-lookup"><span data-stu-id="98ccc-154">More information</span></span>
<span data-ttu-id="98ccc-155">Další informace o činnosti AppSource zkušební verze najdete v tématu [toto video](https://aka.ms/trialexperienceforwebapps).</span><span class="sxs-lookup"><span data-stu-id="98ccc-155">For more information about the AppSource trial experience, see [this video](https://aka.ms/trialexperienceforwebapps).</span></span> 
 
## <a name="next-steps"></a><span data-ttu-id="98ccc-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="98ccc-156">Next Steps</span></span>

- <span data-ttu-id="98ccc-157">Další informace o vytváření aplikace, které podporují přihlášení Azure Active Directory, naleznete v části [scénáře ověřování pro Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)</span><span class="sxs-lookup"><span data-stu-id="98ccc-157">For more information on building applications that support Azure Active Directory sign-ins, see [Authentication Scenarios for Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)</span></span> 

- <span data-ttu-id="98ccc-158">Informace o tom, jak uvedení aplikace SaaS v AppSource, najdete v tématu [informace o partnerovi AppSource](https://appsource.microsoft.com/partners)</span><span class="sxs-lookup"><span data-stu-id="98ccc-158">For information on how to list your SaaS application in AppSource, go see [AppSource Partner Information](https://appsource.microsoft.com/partners)</span></span>


## <a name="get-support"></a><span data-ttu-id="98ccc-159">Získat podporu</span><span class="sxs-lookup"><span data-stu-id="98ccc-159">Get Support</span></span>
<span data-ttu-id="98ccc-160">Integrace služby Azure Active Directory, používáme [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) s komunitou kvůli zajištění podpory.</span><span class="sxs-lookup"><span data-stu-id="98ccc-160">For Azure Active Directory integration, we use [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) with the community to provide support.</span></span> 

<span data-ttu-id="98ccc-161">Důrazně doporučujeme nejprve vaše dotazy posílejte na Stack Overflow a procházet stávající problémy, které chcete zobrazit, pokud někdo požádal váš dotaz před.</span><span class="sxs-lookup"><span data-stu-id="98ccc-161">We highly recommend you ask your questions on Stack Overflow first and browse existing issues to see if someone has asked your question before.</span></span> <span data-ttu-id="98ccc-162">Ujistěte se, že jsou označené svoje dotazy nebo připomínky `[azure-active-directory]`.</span><span class="sxs-lookup"><span data-stu-id="98ccc-162">Make sure that your questions or comments are tagged with `[azure-active-directory]`.</span></span>

<span data-ttu-id="98ccc-163">Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.</span><span class="sxs-lookup"><span data-stu-id="98ccc-163">Use the following comments section to provide feedback and help us refine and shape our content.</span></span>

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->