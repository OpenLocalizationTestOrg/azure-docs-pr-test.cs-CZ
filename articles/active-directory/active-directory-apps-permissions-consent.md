---
title: "Aplikace, oprávnění a vyjádření souhlasu v Azure Active Directory | Dokumentace Microsoftu"
description: "Azure AD Connect integruje vaše místní adresáře do služby Azure Active Directory. To vám umožní tooprovide společnou identitu pro aplikace Office 365, Azure a SaaS integrované s Azure AD."
keywords: "tooAzure Úvod AD, aplikace, co je Azure AD Connect, nainstalovat službu active directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 25897cc4-7687-49b6-b0d5-71f51302b6b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/31/2017
ms.author: billmath
ms.reviewer: jesakowi
ms.custom: oldportal;it-pro;
ms.openlocfilehash: af0c2669199736fdb41e85876a7e3a7064e80770
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a><span data-ttu-id="013d3-105">Aplikace, oprávnění a vyjádření souhlasu v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="013d3-105">Apps, permissions, and consent in Azure Active Directory</span></span>
<span data-ttu-id="013d3-106">V rámci Azure Active Directory můžete přidat adresář tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="013d3-106">Within Azure Active Directory, you can add applications tooyour directory.</span></span>  <span data-ttu-id="013d3-107">aplikace Hello se může lišit v závislosti na typu hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="013d3-107">hello applications can vary depending on hello type of application.</span></span>  <span data-ttu-id="013d3-108">tooview aplikace hello portálu classic, vyberte adresář a vyberte aplikace.</span><span class="sxs-lookup"><span data-stu-id="013d3-108">tooview applications in hello classic portal, select a directory and choose applications.</span></span>

![](media/active-directory-apps-permissions-consent/apps1.png)

> [!IMPORTANT]
> <span data-ttu-id="013d3-109">Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="013d3-109">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span>

## <a name="types-of-apps"></a><span data-ttu-id="013d3-110">Typy aplikací</span><span class="sxs-lookup"><span data-stu-id="013d3-110">Types of apps</span></span>

1. <span data-ttu-id="013d3-111">**Aplikace s jedním tenantem**</span><span class="sxs-lookup"><span data-stu-id="013d3-111">**Single-tenant apps**</span></span> </br>
    - <span data-ttu-id="013d3-112">**Jednoho klienta aplikace** -často označuje tooas-obchodní (LOB) aplikace.</span><span class="sxs-lookup"><span data-stu-id="013d3-112">**Single-tenant apps** - Often referred tooas line-of-business (LOB) apps.</span></span> <span data-ttu-id="013d3-113">Toto je hello případ, kdy někdo v rámci vaší organizace sama vyvinula vlastní aplikaci a mají uživatelé v hello organizace toobe možné toosign v toohello aplikaci.</span><span class="sxs-lookup"><span data-stu-id="013d3-113">This is hello case where someone within your organization develops their own app, and would like users in hello organization toobe able toosign in toohello app.</span></span>
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - <span data-ttu-id="013d3-114">**Proxy aplikace aplikace** – Pokud vystavit místní aplikace s Proxy aplikace Azure AD, aplikace na jednoho klienta je zaregistrován ve vašem klientovi (v toohello přidání Proxy aplikace služby).</span><span class="sxs-lookup"><span data-stu-id="013d3-114">**App Proxy apps** - When you expose an on-prem application with Azure AD App Proxy, a single-tenant app is registered in your tenant (in addition toohello App Proxy service).</span></span> <span data-ttu-id="013d3-115">Taková aplikace reprezentuje vaši místní aplikaci pro veškeré cloudové interakce (například ověřování).</span><span class="sxs-lookup"><span data-stu-id="013d3-115">This app is what represents your on-prem application for all cloud interactions (for example, authentication).</span></span> <span data-ttu-id="013d3-116">(App Proxy vyžaduje Azure AD Basic nebo vyšší.)</span><span class="sxs-lookup"><span data-stu-id="013d3-116">(App Proxy requires Azure AD Basic or higher.)</span></span>


2. <span data-ttu-id="013d3-117">**Aplikace s více tenanty**</span><span class="sxs-lookup"><span data-stu-id="013d3-117">**Multi-tenant apps**</span></span>
    - <span data-ttu-id="013d3-118">**Víceklientské aplikace, které můžete s ostatními souhlasit** – podobné příliš "jednoho klienta aplikace, které vaše organizace sama vyvinula".</span><span class="sxs-lookup"><span data-stu-id="013d3-118">**Multi-tenant apps which others can consent to** - similar too“single-tenant apps that your organization develops”.</span></span> <span data-ttu-id="013d3-119">Hlavní rozdíl Hello (kromě hello logiku hello aplikace) je uživatelé z jiných klientů můžete také souhlas tooand přihlášení toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="013d3-119">hello main difference (besides hello logic in hello app itself) is that users from other tenants can also consent tooand sign in toohello app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - <span data-ttu-id="013d3-120">**Aplikace s více tenanty vyvíjené ostatními, se kterými může Contoso vyjádřit souhlas**.</span><span class="sxs-lookup"><span data-stu-id="013d3-120">**Multi-tenant apps others develop, which Contoso can consent to**.</span></span> <span data-ttu-id="013d3-121">(zkráceně odsouhlasené aplikace) Toto je straně překlopit hello "víceklientské aplikací, které vaše organizace sama vyvinula".</span><span class="sxs-lookup"><span data-stu-id="013d3-121">(Or “consented apps”, for short.) This is hello flip side of “multi-tenant apps your organization develops”.</span></span> <span data-ttu-id="013d3-122">Když jiné organizaci sama vyvinula víceklientské aplikace, můžete uživatele organizaci souhlas toohello aplikace a přihlaste tooit.</span><span class="sxs-lookup"><span data-stu-id="013d3-122">When another organization develops a multi-tenant app, users of your organization can consent toohello app and sign in tooit.</span></span>
    - <span data-ttu-id="013d3-123">**Vlastní aplikace Microsoftu** – Aplikace, které představují služby Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="013d3-123">**Microsoft first-party apps** - Apps that represent Microsoft services.</span></span> <span data-ttu-id="013d3-124">Souhlas doprovází hello skutečnost, že registrace pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="013d3-124">Consent is driven by hello fact that you sign up for hello service.</span></span> <span data-ttu-id="013d3-125">Je někdy speciální UX a logiku pro určité aplikace první strany, která se často používá při vytváření zásady kolem toohello aplikace access.</span><span class="sxs-lookup"><span data-stu-id="013d3-125">There is sometimes special UX and logic for certain first-party apps that is often used when establishing policies around access toohello app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - <span data-ttu-id="013d3-126">**Předběžně integrované aplikace** – aplikace, které jsou k dispozici v hello Azure AD aplikace Galerie, které můžete přidat tooyour directory tooprovide jedním přihlašování (a v některých případech zřizování) toopopular SaaS aplikace.</span><span class="sxs-lookup"><span data-stu-id="013d3-126">**Pre-integrated apps** - Apps available in hello Azure AD App Gallery, which you can add tooyour directory tooprovide single sign-on (and in some cases, provisioning) toopopular SaaS apps.</span></span>
    - <span data-ttu-id="013d3-127">**Jednotné přihlašování Azure AD**: Opravdové jednotné přihlašování pro aplikace, které lze integrovat s Azure AD, prostřednictvím podporovaného přihlašovacího protokolu, jako je SAML 2.0 nebo OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="013d3-127">**Azure AD single sign-on**: “Real” SSO, for apps that can be integrated with Azure AD, through a supported sign-in protocol such as SAML 2.0 or OpenID Connect.</span></span> <span data-ttu-id="013d3-128">Hello Průvodce vás provede procesem jeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="013d3-128">hello wizard walks you through setting it up.</span></span>
    - <span data-ttu-id="013d3-129">**Heslo jednotné přihlašování**: Azure AD bezpečně uloží hello uživatelská pověření pro aplikace hello a hello přihlašovací údaje jsou "vloženy" do hello přihlašovací formulář hello rozšíření prohlížeče přístup k aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="013d3-129">**Password single sign-on**: Azure AD securely stores hello user’s credentials for hello app, and hello credentials are “injected” into hello sign-in form by hello Azure AD App Access browser extension.</span></span> <span data-ttu-id="013d3-130">Označuje se také jako ukládání hesel do trezoru.</span><span class="sxs-lookup"><span data-stu-id="013d3-130">Also known as “password vaulting”.</span></span>

## <a name="permissions"></a><span data-ttu-id="013d3-131">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="013d3-131">Permissions</span></span>

<span data-ttu-id="013d3-132">Při registraci aplikace hello uživatele provedení registrace aplikace hello (tedy hello vývojáře) definuje, které aplikace hello oprávnění potřebuje přístup k a prostředky, ke kterým.</span><span class="sxs-lookup"><span data-stu-id="013d3-132">When an app is registered, hello user performing hello app registration (that is, hello developer) defines which permissions hello app needs access to, and which resources.</span></span> <span data-ttu-id="013d3-133">(hello prostředky jsou, sami definován jako ostatní aplikace). Například někdo vytváření aplikaci čtečky pomocí e-mailu, by stavu, že jejich aplikace vyžaduje oprávnění hello "Přístup k poštovním schránkám jako hello přihlášeného uživatele" hello "Office 365 Exchange Online" prostředků:</span><span class="sxs-lookup"><span data-stu-id="013d3-133">(hello resources are, themselves, defined as other apps.) For example, someone building a mail reader app, would state that their app requires hello “Access mailboxes as hello signed-in user” permission in hello “Office 365 Exchange Online” resource:</span></span>
    
![](media/active-directory-apps-permissions-consent/apps6.png)

<span data-ttu-id="013d3-134">V pořadí pro jednu aplikaci (klient hello) toorequest oprávnění z jiné aplikace (hello prostředků) definuje hello vývojáře hello prostředků aplikace hello oprávnění, která neexistuje.</span><span class="sxs-lookup"><span data-stu-id="013d3-134">In order for one app (hello client) toorequest a certain permission from another app (hello resource), hello developer of hello resource app defines hello permissions that exist.</span></span> <span data-ttu-id="013d3-135">V našem příkladu společnosti Microsoft, vlastníka hello prostředků aplikace "Office 365 Exchange Online" hello definovali oprávnění s názvem "Přístup k poštovním schránkám jako hello přihlášeného uživatele".</span><span class="sxs-lookup"><span data-stu-id="013d3-135">In our example, Microsoft, hello owner of hello “Office 365 Exchange Online” resource app, have defined a permission named “Access mailboxes as hello signed-in user”.</span></span>

<span data-ttu-id="013d3-136">Při definování oprávnění, vývojáři aplikace hello musí definovat, pokud může být souhlas hello oprávnění, nebo jestli vyžaduje souhlas správce.</span><span class="sxs-lookup"><span data-stu-id="013d3-136">When defining permissions, hello app developer must define if hello permission can be consented to, or if it requires admin consent.</span></span> <span data-ttu-id="013d3-137">To umožňuje vývojářům tooconsent tooallow uživatelé na svých vlastních tooapps vyžaduje pouze oprávnění nízká citlivosti, ale vyžadují admins tooconsent toomore citlivé oprávnění.</span><span class="sxs-lookup"><span data-stu-id="013d3-137">This allows developers tooallow users tooconsent on their own tooapps requesting only low-sensitivity permissions, but require admins tooconsent toomore sensitive permissions.</span></span> <span data-ttu-id="013d3-138">Například hello "Azure Active Directory" prostředků aplikace, byla definována, takže uživatelé mohou souhlas tooapps, požaduje omezenými oprávněními jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="013d3-138">For example, hello “Azure Active Directory” resource app, has been defined, so users can consent tooapps, requesting limited read-only permissions.</span></span>  <span data-ttu-id="013d3-139">Pro úplná oprávnění ke čtení a veškerá oprávnění k zápisu však vyžaduje souhlas správce.</span><span class="sxs-lookup"><span data-stu-id="013d3-139">However, admin consent is required for full read permissions, and all write permissions.</span></span>

<span data-ttu-id="013d3-140">Vzhledem k tomu, že se nativní klienti neověřují, aplikace definovaná jako nativní klientská aplikace může požadovat pouze delegovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="013d3-140">Because native clients are not authenticated, an app defined as a native client app can only request delegated permissions.</span></span> <span data-ttu-id="013d3-141">To znamená, že získání tokenu se musí vždy účastnit i skutečný uživatel.</span><span class="sxs-lookup"><span data-stu-id="013d3-141">This means that there must always be an actual user involved when obtaining a token.</span></span> <span data-ttu-id="013d3-142">Webové aplikace a webová rozhraní API (důvěrní klienti) se musí při každém získání přístupového tokenu ověřovat pomocí Azure AD.</span><span class="sxs-lookup"><span data-stu-id="013d3-142">Web apps and web APIs (confidential clients), must always authenticate with Azure AD when getting an access token.</span></span> <span data-ttu-id="013d3-143">Znamená, mají možnost hello požadovat oprávnění jen pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="013d3-143">Meaning they also have hello possibility of requesting app-only permissions.</span></span> <span data-ttu-id="013d3-144">Například jedna služba back-end potřebuje tooauthenticate tooanother back-end službu.</span><span class="sxs-lookup"><span data-stu-id="013d3-144">For example, if one back-end service needs tooauthenticate tooanother back-end service.</span></span> <span data-ttu-id="013d3-145">Aplikace vyžadující oprávnění jen pro aplikace vždy vyžadují souhlas správce.</span><span class="sxs-lookup"><span data-stu-id="013d3-145">Applications requesting app-only permissions always require administrator consent.</span></span>

<span data-ttu-id="013d3-146">Shrňme si to:</span><span class="sxs-lookup"><span data-stu-id="013d3-146">Summarizing:</span></span>



- <span data-ttu-id="013d3-147">Aplikace (klient) stavy hello oprávnění, která potřebuje pro jiné aplikace (prostředky).</span><span class="sxs-lookup"><span data-stu-id="013d3-147">An app (client) states hello permissions it needs for other apps (resources).</span></span>
- <span data-ttu-id="013d3-148">Aplikace (prostředků) určují, jaká oprávnění jsou zveřejněné tooother aplikace (klientů).</span><span class="sxs-lookup"><span data-stu-id="013d3-148">An app (resource) states what permissions are exposed tooother apps (clients).</span></span>
- <span data-ttu-id="013d3-149">Oprávnění může být jen pro aplikace nebo delegované.</span><span class="sxs-lookup"><span data-stu-id="013d3-149">A permission can be an app-only permission, or a delegated permission.</span></span>
- <span data-ttu-id="013d3-150">Delegované oprávnění může být označeno jako „umožňuje souhlas uživatele“ nebo „vyžaduje souhlas správce“.</span><span class="sxs-lookup"><span data-stu-id="013d3-150">A delegated permission can be marked as “allows user consent”, or “requires admin consent”.</span></span>
- <span data-ttu-id="013d3-151">Aplikace se může chovat jako klient (deklarováním potřebuje oprávnění tooa prostředku), jako prostředek (deklarováním oprávnění, která zpřístupňuje) nebo obě.</span><span class="sxs-lookup"><span data-stu-id="013d3-151">An app can behave as a client (by declaring that it needs permissions tooa resource), as a resource (by declaring which permissions it exposes), or as both.</span></span>

## <a name="controls"></a><span data-ttu-id="013d3-152">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="013d3-152">Controls</span></span>

<span data-ttu-id="013d3-153">Hello následuje seznam hello jiný správce kontrolních mechanismů pro toto chování.</span><span class="sxs-lookup"><span data-stu-id="013d3-153">hello following is a list of hello different admin controls available for all this behavior.</span></span> <span data-ttu-id="013d3-154">Dobrý den, správce, které jsou přístupné ovládací prvky portálu classic hello z konfigurace v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="013d3-154">hello admin controls can be accessed in hello classic portal from configure under hello directory.</span></span>

![](media/active-directory-apps-permissions-consent/apps7.png)

<span data-ttu-id="013d3-155">V hello Azure portálu pod **spravovat**, **uživatelská nastavení**.</span><span class="sxs-lookup"><span data-stu-id="013d3-155">In hello Azure portal, under **manage**, **user settings**.</span></span>

![](media/active-directory-apps-permissions-consent/apps11.png)



- <span data-ttu-id="013d3-156">Můžete řídit, jestli uživatelé mohou souhlas tooapps:</span><span class="sxs-lookup"><span data-stu-id="013d3-156">You can control whether users can consent tooapps:</span></span>

<span data-ttu-id="013d3-157">Na portálu classic hello vyberte **uživatelé mohou poskytnout tooaccess oprávnění aplikací svá data.**
![](media/active-directory-apps-permissions-consent/apps8.png)</span><span class="sxs-lookup"><span data-stu-id="013d3-157">In hello classic portal, select **Users may give applications permissions tooaccess their data.**
![](media/active-directory-apps-permissions-consent/apps8.png)</span></span>

<span data-ttu-id="013d3-158">V hello portálu Azure, vyberte **uživatelé můžou aplikace tooaccess svá data**.</span><span class="sxs-lookup"><span data-stu-id="013d3-158">In hello Azure portal, select **users can allow apps tooaccess their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps12.png)



- <span data-ttu-id="013d3-159">Můžete řídit, jestli uživatelé mohou registrovat své vlastní obchodní aplikace jednoho klienta: V hello klasického portálu vyberte **uživatelé mohou přidat integrovaných aplikací.**
![](media/active-directory-apps-permissions-consent/apps9.png)</span><span class="sxs-lookup"><span data-stu-id="013d3-159">You can control whether users can register their own single-tenant LOB apps: In hello classic portal select **Users may add integrated applications.**
![](media/active-directory-apps-permissions-consent/apps9.png)</span></span>

<span data-ttu-id="013d3-160">V hello portálu Azure, vyberte **uživatelé můžou aplikace tooaccess svá data**.</span><span class="sxs-lookup"><span data-stu-id="013d3-160">In hello Azure portal, select **users can allow apps tooaccess their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
><span data-ttu-id="013d3-161">I v případě, že uživatelům umožnit tooregister jednoho klienta obchodní aplikace, existují omezení, které lze registrovat toowhat.</span><span class="sxs-lookup"><span data-stu-id="013d3-161">Even if you do allow users tooregister single-tenant LOB apps, there are limits toowhat can be registered.</span></span>  
><span data-ttu-id="013d3-162">Například vývojáři, kteří nejsou správci adresáře.</span><span class="sxs-lookup"><span data-stu-id="013d3-162">For example, developers who are not directory admins.</span></span>
>
>- <span data-ttu-id="013d3-163">Uživatelé nemohou udělat z aplikace pro jednoho tenanta aplikaci pro více tenantů.</span><span class="sxs-lookup"><span data-stu-id="013d3-163">Users cannot make a single-tenant app a multi-tenant app.</span></span>
>- <span data-ttu-id="013d3-164">Při registraci aplikace LOB jednoho klienta, uživatelé požádat o oprávnění jen aplikace tooother aplikace.</span><span class="sxs-lookup"><span data-stu-id="013d3-164">When registering single-tenant LOB apps, users cannot request app-only permissions tooother apps.</span></span>
>- <span data-ttu-id="013d3-165">Při registraci aplikace LOB jednoho klienta, uživatelé nemůže požádat o aplikace tooother delegovaných oprávnění, pokud tato oprávnění vyžadovat souhlas správce.</span><span class="sxs-lookup"><span data-stu-id="013d3-165">When registering single-tenant LOB apps, users cannot request delegated permissions tooother apps if those permissions require admin consent.</span></span>
>- <span data-ttu-id="013d3-166">Mohou uživatelé provádět tooapps změny, které nejsou vlastníky.</span><span class="sxs-lookup"><span data-stu-id="013d3-166">Users cannot make changes tooapps that they are not owners of.</span></span>



- <span data-ttu-id="013d3-167">Můžete řídit, jestli sami uživatelé mohou přidávat předem integrované aplikace, které používají jednotné přihlašování pomocí hesla (neboli ukládání hesel do trezoru).![](media/active-directory-apps-permissions-consent/apps10.png)</span><span class="sxs-lookup"><span data-stu-id="013d3-167">You can control whether users can themselves add pre-integrated apps that use password SSO (aka “password vaulting”) ![](media/active-directory-apps-permissions-consent/apps10.png)</span></span>



- <span data-ttu-id="013d3-168">Můžete řídit, za jakých podmínek je aplikace přístupná (tj. podmíněný přístup).</span><span class="sxs-lookup"><span data-stu-id="013d3-168">You can control under which conditions applications can be accessed (that is, conditional access).</span></span> <span data-ttu-id="013d3-169">Mějte na paměti, že platí toohello klientská aplikace i toohello prostředků aplikace.</span><span class="sxs-lookup"><span data-stu-id="013d3-169">Be aware this applies both toohello client app and toohello resource app.</span></span> <span data-ttu-id="013d3-170">Ano stát, že nastavit zásady podmíněného přístupu s upozorněním, že tuto aplikaci "Office 365 Exchange Online" hello lze přistupovat pouze z počítačů, které splňují předpisy.</span><span class="sxs-lookup"><span data-stu-id="013d3-170">So, say you set a conditional access policy that says that hello “Office 365 Exchange Online” app can only be accessed from machines, which are compliant.</span></span>  <span data-ttu-id="013d3-171">Tato zásada se nové i když se uživatel pokusí toouse klientské aplikace, který vyžaduje oprávnění tooExchange Online.</span><span class="sxs-lookup"><span data-stu-id="013d3-171">This policy will also kick in when a user attempts toouse a client app which requests permissions tooExchange Online.</span></span>



- <span data-ttu-id="013d3-172">Máte přehled, do kterého byla aplikace svolení tooand ty, které jsou používány.</span><span class="sxs-lookup"><span data-stu-id="013d3-172">You have visibility into which apps have been consented tooand which ones are being used.</span></span>

1.  <span data-ttu-id="013d3-173">Když uživatel souhlasí tooan aplikace, vytvoří se objekt ServicePrincipal v klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="013d3-173">When a user consents tooan app, a ServicePrincipal object is created in hello tenant.</span></span> <span data-ttu-id="013d3-174">Vytvoření ServicePrincipal je součástí sestava auditu hello.</span><span class="sxs-lookup"><span data-stu-id="013d3-174">ServicePrincipal creation is included in hello audit report.</span></span>
2.  <span data-ttu-id="013d3-175">Sestavy aktivit přihlášení uživatele zjistíte, které aplikace hello uživatele je přihlášení k.</span><span class="sxs-lookup"><span data-stu-id="013d3-175">User sign-in activity reports tell you which app hello user is signing in to.</span></span> 

## <a name="example"></a><span data-ttu-id="013d3-176">Příklad</span><span class="sxs-lookup"><span data-stu-id="013d3-176">Example</span></span>

<span data-ttu-id="013d3-177">Jako příklad Podívejme aplikaci "FabrikamMail pro Office 365" hello, která jste si všimli, že uživatelé ve vašem klientovi se přihlašujete k.</span><span class="sxs-lookup"><span data-stu-id="013d3-177">As an example, let’s take hello “FabrikamMail for Office 365” app, which you’ve noticed users in your tenant are signing in to.</span></span> <span data-ttu-id="013d3-178">FabrikamMail je aplikace pro čtení pošty pro Android vytvořená společností Fabrikam, Inc.</span><span class="sxs-lookup"><span data-stu-id="013d3-178">“FabrikamMail” is a mail reader app for Android, published by “Fabrikam, Inc.”.</span></span> <span data-ttu-id="013d3-179">To, které patří do hello "víceklientské aplikace jiné vývoj, které můžete souhlas Contoso".</span><span class="sxs-lookup"><span data-stu-id="013d3-179">This falls into hello “multi-tenant apps other develop, which Contoso can consent to”.</span></span>

<span data-ttu-id="013d3-180">Pokud mají uživatelé povoleno tooconsent, získají vyzvat hello souhlasu při prvním přihlášení:![](media/active-directory-apps-permissions-consent/apps14.png)</span><span class="sxs-lookup"><span data-stu-id="013d3-180">If users are allowed tooconsent, they get a consent prompt hello first time they sign in: ![](media/active-directory-apps-permissions-consent/apps14.png)</span></span>

<span data-ttu-id="013d3-181">"Přístup k vaší poštovní schránky" je hello uživatelsky orientovaný souhlasu řetězec oprávnění "Přístup k poštovním schránkám jako hello přihlášeného uživatele" hello "Office 365 Exchange Online" (Exchange) vystavené.</span><span class="sxs-lookup"><span data-stu-id="013d3-181">“Access your mailboxes” is hello user-facing consent string for hello “Access mailboxes as hello signed-in user” permission exposed by “Office 365 Exchange Online” (that is, Exchange).</span></span>

<span data-ttu-id="013d3-182">Uvidíte hello oprávnění vyhledáním hello ServicePrincipal objekt pro Exchange (hello prostředků), která byla přidána při registraci pro Office 365.</span><span class="sxs-lookup"><span data-stu-id="013d3-182">You can see hello permissions by looking up hello ServicePrincipal object for Exchange (hello resource), which was added when you signed up for Office 365.</span></span> <span data-ttu-id="013d3-183">Objekt ServicePrincipal hello "instance" hello aplikace si můžete představit ve vašem klientovi, který se používá k zaznamenání různé možnosti a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="013d3-183">You can think of hello ServicePrincipal object of an “instance” of hello app in your tenant, which is used for recording different options and configurations.</span></span>  <span data-ttu-id="013d3-184">Můžete to vidět pomocí hello `Get-AzureADServicePrincipal` v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="013d3-184">You can see this by using hello `Get-AzureADServicePrincipal` in PowerShell.</span></span>

    PS C:\> Get-AzureADServicePrincipal -ObjectId 383f7b97-6754-4d3d-9474-3908ebcba1c6 | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : Office 365 Exchange Online
    AppId                     : 00000002-0000-0ff1-ce00-000000000000
    AppOwnerTenantId          : 
    AppRoleAssignmentRequired : False
    AppRoles                  : {...}
    DisplayName               : Microsoft.Exchange
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {...
                                , class OAuth2Permission {
                                  AdminConsentDescription : Allows hello app toohave hello same access toomailboxes as hello signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as hello signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows hello app full access tooyour mailboxes on your behalf.
                                  UserConsentDisplayName  : Access your mailboxes
                                  Value                   : full_access_as_user
                                },
                                ...}
    PasswordCredentials       : {}
    PublisherName             : 
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {00000002-0000-0ff1-ce00-000000000000/outlook.office365.com, 00000002-0000-0ff1-ce00-000000000000/mail.office365.com, 00000002-0000-0ff1-ce00-000000000000/outlook.com, 
                                00000002-0000-0ff1-ce00-000000000000/*.outlook.com...}
    Tags                      : {}

<span data-ttu-id="013d3-185">Souhlas je vyvoláno hello kliknutí na tlačítko "Přijmout".</span><span class="sxs-lookup"><span data-stu-id="013d3-185">Consent is initiated when hello user clicks “Accept”.</span></span> <span data-ttu-id="013d3-186">Nejprve je vytvořen objekt ServicePrincipal pro "FabrikamMail pro Office 365" v klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="013d3-186">First, a ServicePrincipal object for “FabrikamMail for Office 365” is created in hello tenant.</span></span> <span data-ttu-id="013d3-187">Hello ServicePrincipal vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="013d3-187">hello ServicePrincipal looks something like this:</span></span>

    PS C:\> Get-AzureADServicePrincipal -SearchString "FabrikamMail for Office 365" | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : a8b16333-851d-42e8-acd2-eac155849b37
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : FabrikamMail for Office 365
    AppId                     : aba7c072-2267-4031-8960-e7a2db6e0590
    AppOwnerTenantId          : 4a4076e0-a70f-41c6-b819-6f9c4a86df89
    AppRoleAssignmentRequired : False
    AppRoles                  : {}
    DisplayName               : FabrikamMail for Office 365
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {}
    PasswordCredentials       : {}
    PublisherName             : Fabrikam, Inc.
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {aba7c072-2267-4031-8960-e7a2db6e0590}
    Tags                      : {WindowsAzureActiveDirectoryIntegratedApp}

<span data-ttu-id="013d3-188">Souhlas tooan aplikace vytvoří odkaz Oauth2PermissionGrant mezi hello následující:</span><span class="sxs-lookup"><span data-stu-id="013d3-188">Consenting tooan app creates an Oauth2PermissionGrant link between hello following:</span></span>
  
- <span data-ttu-id="013d3-189">objekt uživatele Hello</span><span class="sxs-lookup"><span data-stu-id="013d3-189">hello user object</span></span>
- <span data-ttu-id="013d3-190">klientské aplikace Hello ServicePrincipalName (SPN)</span><span class="sxs-lookup"><span data-stu-id="013d3-190">hello client apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="013d3-191">Hello prostředků aplikace ServicePrincipalName (SPN)</span><span class="sxs-lookup"><span data-stu-id="013d3-191">hello resource apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="013d3-192">oprávnění v hello prostředků aplikace.</span><span class="sxs-lookup"><span data-stu-id="013d3-192">permissions in hello resource app.</span></span>  

<span data-ttu-id="013d3-193">V případě hello FabrikamMail vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="013d3-193">In hello case of FabrikamMail, it looks something like this:</span></span>

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

<span data-ttu-id="013d3-194">(**ClientId** je ID objektu zabezpečení služby je FabrikamMail (hello, ten, který právě nebyl vytvořen), **PrincipalId** je ID objektu uživatelské hello (of hello uživatel, který dá souhlas), **ResourceId**je služba Exchange pro ID objektu zabezpečení, rozsahu je hello oprávnění v systému Exchange, které se souhlas).</span><span class="sxs-lookup"><span data-stu-id="013d3-194">(**ClientId** is FabrikamMail’s service principal object ID (hello one that just got created), **PrincipalId** is hello user object ID (of hello user who consented), **ResourceId** is Exchange’s service principal object ID, Scope is hello permission in Exchange that was consented to).</span></span>

<span data-ttu-id="013d3-195">Pokud uživatelé nejsou povoleny tooconsent, zobrazí se obrazovky s upozorněním, že oprávnění je vyžadováno.</span><span class="sxs-lookup"><span data-stu-id="013d3-195">If users are not allowed tooconsent, they will see a screen that says that permission is required.</span></span>

