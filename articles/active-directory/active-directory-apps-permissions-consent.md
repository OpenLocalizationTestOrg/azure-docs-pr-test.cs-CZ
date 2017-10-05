---
title: "Aplikace, oprávnění a vyjádření souhlasu v Azure Active Directory | Dokumentace Microsoftu"
description: "Azure AD Connect integruje vaše místní adresáře do služby Azure Active Directory. To umožní poskytovat společnou identitu pro aplikace Office 365, Azure a SaaS integrované s Azure AD."
keywords: "úvod k Azure AD, aplikace, co je Azure AD Connect, instalace active directory"
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
ms.openlocfilehash: 6f6baf5e1538fb280a899065c64ca5688473c04a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a><span data-ttu-id="dff74-105">Aplikace, oprávnění a vyjádření souhlasu v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dff74-105">Apps, permissions, and consent in Azure Active Directory</span></span>
<span data-ttu-id="dff74-106">V rámci Azure Active Directory můžete do svého adresáře přidávat aplikace.</span><span class="sxs-lookup"><span data-stu-id="dff74-106">Within Azure Active Directory, you can add applications to your directory.</span></span>  <span data-ttu-id="dff74-107">Aplikace se mohou lišit v závislosti na typu aplikace.</span><span class="sxs-lookup"><span data-stu-id="dff74-107">The applications can vary depending on the type of application.</span></span>  <span data-ttu-id="dff74-108">Pokud chcete zobrazit aplikace na portálu Classic, vyberte adresář a zvolte Aplikace.</span><span class="sxs-lookup"><span data-stu-id="dff74-108">To view applications in the classic portal, select a directory and choose applications.</span></span>

![](media/active-directory-apps-permissions-consent/apps1.png)

> [!IMPORTANT]
> <span data-ttu-id="dff74-109">Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.</span><span class="sxs-lookup"><span data-stu-id="dff74-109">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span>

## <a name="types-of-apps"></a><span data-ttu-id="dff74-110">Typy aplikací</span><span class="sxs-lookup"><span data-stu-id="dff74-110">Types of apps</span></span>

1. <span data-ttu-id="dff74-111">**Aplikace s jedním tenantem**</span><span class="sxs-lookup"><span data-stu-id="dff74-111">**Single-tenant apps**</span></span> </br>
    - <span data-ttu-id="dff74-112">**Aplikace s jedním tenantem** – Často označované jako obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="dff74-112">**Single-tenant apps** - Often referred to as line-of-business (LOB) apps.</span></span> <span data-ttu-id="dff74-113">To je případ, kdy někdo v rámci vaší organizace vytvoří vlastní aplikaci a chce umožnit uživatelům v organizaci přihlásit se k ní.</span><span class="sxs-lookup"><span data-stu-id="dff74-113">This is the case where someone within your organization develops their own app, and would like users in the organization to be able to sign in to the app.</span></span>
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - <span data-ttu-id="dff74-114">**Aplikace App Proxy** – Pokud zveřejňujete místní aplikaci pomocí Azure AD App Proxy, zaregistruje se ve vašem tenantovi aplikace s jedním tenantem (vedle služby App Proxy).</span><span class="sxs-lookup"><span data-stu-id="dff74-114">**App Proxy apps** - When you expose an on-prem application with Azure AD App Proxy, a single-tenant app is registered in your tenant (in addition to the App Proxy service).</span></span> <span data-ttu-id="dff74-115">Taková aplikace reprezentuje vaši místní aplikaci pro veškeré cloudové interakce (například ověřování).</span><span class="sxs-lookup"><span data-stu-id="dff74-115">This app is what represents your on-prem application for all cloud interactions (for example, authentication).</span></span> <span data-ttu-id="dff74-116">(App Proxy vyžaduje Azure AD Basic nebo vyšší.)</span><span class="sxs-lookup"><span data-stu-id="dff74-116">(App Proxy requires Azure AD Basic or higher.)</span></span>


2. <span data-ttu-id="dff74-117">**Aplikace s více tenanty**</span><span class="sxs-lookup"><span data-stu-id="dff74-117">**Multi-tenant apps**</span></span>
    - <span data-ttu-id="dff74-118">**Aplikace s více tenanty, se kterými mohou ostatní vyjádřit souhlas** – Podobné jako aplikace s jedním tenantem, které vyvíjí vaše organizace.</span><span class="sxs-lookup"><span data-stu-id="dff74-118">**Multi-tenant apps which others can consent to** - similar to “single-tenant apps that your organization develops”.</span></span> <span data-ttu-id="dff74-119">Hlavní rozdíl (kromě samotné logiky aplikace) je, že uživatelé z jiných tenantů mohou také vyjádřit souhlas s aplikací a přihlašovat se k ní.</span><span class="sxs-lookup"><span data-stu-id="dff74-119">The main difference (besides the logic in the app itself) is that users from other tenants can also consent to and sign in to the app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - <span data-ttu-id="dff74-120">**Aplikace s více tenanty vyvíjené ostatními, se kterými může Contoso vyjádřit souhlas**.</span><span class="sxs-lookup"><span data-stu-id="dff74-120">**Multi-tenant apps others develop, which Contoso can consent to**.</span></span> <span data-ttu-id="dff74-121">(zkráceně odsouhlasené aplikace) Jedná se o pravý opak aplikací s více tenanty, které vyvíjí vaše organizace.</span><span class="sxs-lookup"><span data-stu-id="dff74-121">(Or “consented apps”, for short.) This is the flip side of “multi-tenant apps your organization develops”.</span></span> <span data-ttu-id="dff74-122">Když některá jiná organizace vytvoří aplikaci s více tenanty, uživatelé vaší organizace s ní budou moci vyjádřit souhlas a přihlašovat se k ní.</span><span class="sxs-lookup"><span data-stu-id="dff74-122">When another organization develops a multi-tenant app, users of your organization can consent to the app and sign in to it.</span></span>
    - <span data-ttu-id="dff74-123">**Vlastní aplikace Microsoftu** – Aplikace, které představují služby Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="dff74-123">**Microsoft first-party apps** - Apps that represent Microsoft services.</span></span> <span data-ttu-id="dff74-124">Vyjádřením souhlasu se rozumí registrace do služby.</span><span class="sxs-lookup"><span data-stu-id="dff74-124">Consent is driven by the fact that you sign up for the service.</span></span> <span data-ttu-id="dff74-125">Některé vlastní aplikace mohou obsahovat speciální uživatelské prostředí a logiku, které často slouží k vytváření zásad kolem přístupu k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dff74-125">There is sometimes special UX and logic for certain first-party apps that is often used when establishing policies around access to the app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - <span data-ttu-id="dff74-126">**Předem integrované aplikace** – Aplikace dostupné v Azure AD App Gallery, které můžete přidat do svého adresáře pro poskytování jednotného přihlašování k oblíbeným aplikacím SaaS (v některých případech i jejich zřizování).</span><span class="sxs-lookup"><span data-stu-id="dff74-126">**Pre-integrated apps** - Apps available in the Azure AD App Gallery, which you can add to your directory to provide single sign-on (and in some cases, provisioning) to popular SaaS apps.</span></span>
    - <span data-ttu-id="dff74-127">**Jednotné přihlašování Azure AD**: Opravdové jednotné přihlašování pro aplikace, které lze integrovat s Azure AD, prostřednictvím podporovaného přihlašovacího protokolu, jako je SAML 2.0 nebo OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="dff74-127">**Azure AD single sign-on**: “Real” SSO, for apps that can be integrated with Azure AD, through a supported sign-in protocol such as SAML 2.0 or OpenID Connect.</span></span> <span data-ttu-id="dff74-128">Průvodce vás provede procesem jeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="dff74-128">The wizard walks you through setting it up.</span></span>
    - <span data-ttu-id="dff74-129">**Jednotné přihlašování pomocí hesla**: Azure AD bezpečně ukládá přihlašovací údaje uživatele k aplikaci. Rozšíření prohlížeče Azure AD App Access následně přihlašovací údaje vkládá do přihlašovacího formuláře.</span><span class="sxs-lookup"><span data-stu-id="dff74-129">**Password single sign-on**: Azure AD securely stores the user’s credentials for the app, and the credentials are “injected” into the sign-in form by the Azure AD App Access browser extension.</span></span> <span data-ttu-id="dff74-130">Označuje se také jako ukládání hesel do trezoru.</span><span class="sxs-lookup"><span data-stu-id="dff74-130">Also known as “password vaulting”.</span></span>

## <a name="permissions"></a><span data-ttu-id="dff74-131">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="dff74-131">Permissions</span></span>

<span data-ttu-id="dff74-132">Při registraci aplikace uživatel, který registraci aplikace provádí (tedy vývojář), definuje, k jakým oprávněním a prostředkům potřebuje aplikace přístup.</span><span class="sxs-lookup"><span data-stu-id="dff74-132">When an app is registered, the user performing the app registration (that is, the developer) defines which permissions the app needs access to, and which resources.</span></span> <span data-ttu-id="dff74-133">(Prostředky samotné jsou definované jako ostatní aplikace.) Například někdo, kdo vytváří aplikaci pro čtení pošty, by uvedl, že jeho aplikace požaduje oprávnění „Access mailboxes as the signed-in user“ (Přístup k poštovním schránkám jménem přihlášeného uživatele) v prostředku „Office 365 Exchange Online“:</span><span class="sxs-lookup"><span data-stu-id="dff74-133">(The resources are, themselves, defined as other apps.) For example, someone building a mail reader app, would state that their app requires the “Access mailboxes as the signed-in user” permission in the “Office 365 Exchange Online” resource:</span></span>
    
![](media/active-directory-apps-permissions-consent/apps6.png)

<span data-ttu-id="dff74-134">Aby mohla jedna aplikace (klient) požadovat určitá oprávnění z jiné aplikace (prostředku), vývojář aplikace prostředku definuje existující oprávnění.</span><span class="sxs-lookup"><span data-stu-id="dff74-134">In order for one app (the client) to request a certain permission from another app (the resource), the developer of the resource app defines the permissions that exist.</span></span> <span data-ttu-id="dff74-135">V našem příkladu Microsoft, vlastník aplikace prostředku „Office 365 Exchange Online“, definoval oprávnění s názvem „Access mailboxes as the signed-in user“ (Přístup k poštovním schránkám jménem přihlášeného uživatele).</span><span class="sxs-lookup"><span data-stu-id="dff74-135">In our example, Microsoft, the owner of the “Office 365 Exchange Online” resource app, have defined a permission named “Access mailboxes as the signed-in user”.</span></span>

<span data-ttu-id="dff74-136">Při definování oprávnění musí vývojář aplikace definovat, jestli je možné s oprávněním vyjádřit souhlas, nebo jestli vyžaduje souhlas správce.</span><span class="sxs-lookup"><span data-stu-id="dff74-136">When defining permissions, the app developer must define if the permission can be consented to, or if it requires admin consent.</span></span> <span data-ttu-id="dff74-137">Vývojáři tak mohou umožnit uživatelům samostatně vyjádřit souhlas s aplikacemi, které požadují méně citlivá oprávnění, a zároveň vyžadovat souhlas správce, pokud jde o citlivějšími oprávněními.</span><span class="sxs-lookup"><span data-stu-id="dff74-137">This allows developers to allow users to consent on their own to apps requesting only low-sensitivity permissions, but require admins to consent to more sensitive permissions.</span></span> <span data-ttu-id="dff74-138">Například aplikace prostředku Azure Active Directory je definována tak, že aby uživatelé mohli vyjadřovat souhlas s aplikacemi, vyžaduje omezená oprávnění pouze pro čtení.</span><span class="sxs-lookup"><span data-stu-id="dff74-138">For example, the “Azure Active Directory” resource app, has been defined, so users can consent to apps, requesting limited read-only permissions.</span></span>  <span data-ttu-id="dff74-139">Pro úplná oprávnění ke čtení a veškerá oprávnění k zápisu však vyžaduje souhlas správce.</span><span class="sxs-lookup"><span data-stu-id="dff74-139">However, admin consent is required for full read permissions, and all write permissions.</span></span>

<span data-ttu-id="dff74-140">Vzhledem k tomu, že se nativní klienti neověřují, aplikace definovaná jako nativní klientská aplikace může požadovat pouze delegovaná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="dff74-140">Because native clients are not authenticated, an app defined as a native client app can only request delegated permissions.</span></span> <span data-ttu-id="dff74-141">To znamená, že získání tokenu se musí vždy účastnit i skutečný uživatel.</span><span class="sxs-lookup"><span data-stu-id="dff74-141">This means that there must always be an actual user involved when obtaining a token.</span></span> <span data-ttu-id="dff74-142">Webové aplikace a webová rozhraní API (důvěrní klienti) se musí při každém získání přístupového tokenu ověřovat pomocí Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dff74-142">Web apps and web APIs (confidential clients), must always authenticate with Azure AD when getting an access token.</span></span> <span data-ttu-id="dff74-143">To znamená, že mají také možnost požadovat oprávnění jen pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="dff74-143">Meaning they also have the possibility of requesting app-only permissions.</span></span> <span data-ttu-id="dff74-144">Pokud se například jedna služba back-end potřebuje ověřit v jiné službě back-end.</span><span class="sxs-lookup"><span data-stu-id="dff74-144">For example, if one back-end service needs to authenticate to another back-end service.</span></span> <span data-ttu-id="dff74-145">Aplikace vyžadující oprávnění jen pro aplikace vždy vyžadují souhlas správce.</span><span class="sxs-lookup"><span data-stu-id="dff74-145">Applications requesting app-only permissions always require administrator consent.</span></span>

<span data-ttu-id="dff74-146">Shrňme si to:</span><span class="sxs-lookup"><span data-stu-id="dff74-146">Summarizing:</span></span>



- <span data-ttu-id="dff74-147">Aplikace (klient) uvádí pro ostatní aplikace (prostředky), jaká oprávnění potřebuje.</span><span class="sxs-lookup"><span data-stu-id="dff74-147">An app (client) states the permissions it needs for other apps (resources).</span></span>
- <span data-ttu-id="dff74-148">Aplikace (prostředek) uvádí pro ostatní aplikace (klienty), jaká oprávnění vystavuje.</span><span class="sxs-lookup"><span data-stu-id="dff74-148">An app (resource) states what permissions are exposed to other apps (clients).</span></span>
- <span data-ttu-id="dff74-149">Oprávnění může být jen pro aplikace nebo delegované.</span><span class="sxs-lookup"><span data-stu-id="dff74-149">A permission can be an app-only permission, or a delegated permission.</span></span>
- <span data-ttu-id="dff74-150">Delegované oprávnění může být označeno jako „umožňuje souhlas uživatele“ nebo „vyžaduje souhlas správce“.</span><span class="sxs-lookup"><span data-stu-id="dff74-150">A delegated permission can be marked as “allows user consent”, or “requires admin consent”.</span></span>
- <span data-ttu-id="dff74-151">Aplikace se může chovat jako klient (prohlášením, že potřebuje oprávnění k prostředku), jako prostředek (prohlášením, jaká oprávnění vystavuje), nebo jako obojí.</span><span class="sxs-lookup"><span data-stu-id="dff74-151">An app can behave as a client (by declaring that it needs permissions to a resource), as a resource (by declaring which permissions it exposes), or as both.</span></span>

## <a name="controls"></a><span data-ttu-id="dff74-152">Ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="dff74-152">Controls</span></span>

<span data-ttu-id="dff74-153">Následuje seznam ovládacích prvků správce, které jsou k dispozici pro toto chování.</span><span class="sxs-lookup"><span data-stu-id="dff74-153">The following is a list of the different admin controls available for all this behavior.</span></span> <span data-ttu-id="dff74-154">Ovládací prvky správce jsou přístupné v adresáři na portálu Classic pod možností Konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="dff74-154">The admin controls can be accessed in the classic portal from configure under the directory.</span></span>

![](media/active-directory-apps-permissions-consent/apps7.png)

<span data-ttu-id="dff74-155">Na webu Azure Portal v části **Spravovat**, **Uživatelské nastavení**.</span><span class="sxs-lookup"><span data-stu-id="dff74-155">In the Azure portal, under **manage**, **user settings**.</span></span>

![](media/active-directory-apps-permissions-consent/apps11.png)



- <span data-ttu-id="dff74-156">Můžete řídit, jestli uživatelé mohou souhlasit s aplikacemi:</span><span class="sxs-lookup"><span data-stu-id="dff74-156">You can control whether users can consent to apps:</span></span>

<span data-ttu-id="dff74-157">Na portálu Classic vyberte možnost „**Users may give applications permissions to access their data**“ (Uživatelé mohou udělovat aplikacím oprávnění k přístupu ke svým datům).
![](media/active-directory-apps-permissions-consent/apps8.png)</span><span class="sxs-lookup"><span data-stu-id="dff74-157">In the classic portal, select **Users may give applications permissions to access their data.**
![](media/active-directory-apps-permissions-consent/apps8.png)</span></span>

<span data-ttu-id="dff74-158">Na webu Azure Portal vyberte **Uživatelé můžou povolit aplikacím přístup ke svým datům**.</span><span class="sxs-lookup"><span data-stu-id="dff74-158">In the Azure portal, select **users can allow apps to access their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps12.png)



- <span data-ttu-id="dff74-159">Můžete řídit, jestli uživatelé mohou registrovat své vlastní obchodní aplikace s jedním tenantem: Na portálu Classic vyberte možnost „**Users may add integrated applications**“ (Uživatelé mohou přidávat integrované aplikace).
![](media/active-directory-apps-permissions-consent/apps9.png)</span><span class="sxs-lookup"><span data-stu-id="dff74-159">You can control whether users can register their own single-tenant LOB apps: In the classic portal select **Users may add integrated applications.**
![](media/active-directory-apps-permissions-consent/apps9.png)</span></span>

<span data-ttu-id="dff74-160">Na webu Azure Portal vyberte **Uživatelé můžou povolit aplikacím přístup ke svým datům**.</span><span class="sxs-lookup"><span data-stu-id="dff74-160">In the Azure portal, select **users can allow apps to access their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
><span data-ttu-id="dff74-161">I v případě, že uživatelům umožníte registrovat obchodní aplikace pro jednoho tenanta, existují omezení ohledně toho, co mohou registrovat.</span><span class="sxs-lookup"><span data-stu-id="dff74-161">Even if you do allow users to register single-tenant LOB apps, there are limits to what can be registered.</span></span>  
><span data-ttu-id="dff74-162">Například vývojáři, kteří nejsou správci adresáře.</span><span class="sxs-lookup"><span data-stu-id="dff74-162">For example, developers who are not directory admins.</span></span>
>
>- <span data-ttu-id="dff74-163">Uživatelé nemohou udělat z aplikace pro jednoho tenanta aplikaci pro více tenantů.</span><span class="sxs-lookup"><span data-stu-id="dff74-163">Users cannot make a single-tenant app a multi-tenant app.</span></span>
>- <span data-ttu-id="dff74-164">Při registraci obchodní aplikace pro jednoho tenanta uživatelé nemohou požadovat oprávnění jen pro aplikace k jiným aplikacím.</span><span class="sxs-lookup"><span data-stu-id="dff74-164">When registering single-tenant LOB apps, users cannot request app-only permissions to other apps.</span></span>
>- <span data-ttu-id="dff74-165">Při registraci obchodní aplikace pro jednoho tenanta uživatelé nemohou požadovat delegovaná oprávnění k jiným aplikacím, pokud taková oprávnění vyžadují souhlas správce.</span><span class="sxs-lookup"><span data-stu-id="dff74-165">When registering single-tenant LOB apps, users cannot request delegated permissions to other apps if those permissions require admin consent.</span></span>
>- <span data-ttu-id="dff74-166">Uživatelé nemohou provádět změny aplikací, pokud nejsou jejich vlastníky.</span><span class="sxs-lookup"><span data-stu-id="dff74-166">Users cannot make changes to apps that they are not owners of.</span></span>



- <span data-ttu-id="dff74-167">Můžete řídit, jestli sami uživatelé mohou přidávat předem integrované aplikace, které používají jednotné přihlašování pomocí hesla (neboli ukládání hesel do trezoru).![](media/active-directory-apps-permissions-consent/apps10.png)</span><span class="sxs-lookup"><span data-stu-id="dff74-167">You can control whether users can themselves add pre-integrated apps that use password SSO (aka “password vaulting”) ![](media/active-directory-apps-permissions-consent/apps10.png)</span></span>



- <span data-ttu-id="dff74-168">Můžete řídit, za jakých podmínek je aplikace přístupná (tj. podmíněný přístup).</span><span class="sxs-lookup"><span data-stu-id="dff74-168">You can control under which conditions applications can be accessed (that is, conditional access).</span></span> <span data-ttu-id="dff74-169">Mějte na paměti, že to platí pro klientské aplikace i pro aplikace prostředků.</span><span class="sxs-lookup"><span data-stu-id="dff74-169">Be aware this applies both to the client app and to the resource app.</span></span> <span data-ttu-id="dff74-170">Řekněme třeba, že jste nastavili zásadu podmíněného přístupu, která říká, že aplikace Office 365 Exchange Online je přístupná pouze ze zařízení dodržujících předpisy.</span><span class="sxs-lookup"><span data-stu-id="dff74-170">So, say you set a conditional access policy that says that the “Office 365 Exchange Online” app can only be accessed from machines, which are compliant.</span></span>  <span data-ttu-id="dff74-171">Tato zásada se uplatní také ve chvíli, kdy se uživatel pokusí použít klientskou aplikaci, která požaduje oprávnění k Exchange Online.</span><span class="sxs-lookup"><span data-stu-id="dff74-171">This policy will also kick in when a user attempts to use a client app which requests permissions to Exchange Online.</span></span>



- <span data-ttu-id="dff74-172">Máte přehled o odsouhlasených a používaných aplikacích.</span><span class="sxs-lookup"><span data-stu-id="dff74-172">You have visibility into which apps have been consented to and which ones are being used.</span></span>

1.  <span data-ttu-id="dff74-173">Když uživatel vyjádří souhlas s aplikací, v tenantovi se vytvoří objekt ServicePrincipal.</span><span class="sxs-lookup"><span data-stu-id="dff74-173">When a user consents to an app, a ServicePrincipal object is created in the tenant.</span></span> <span data-ttu-id="dff74-174">Vytvoření objektu ServicePrincipal je součástí sestavy auditu.</span><span class="sxs-lookup"><span data-stu-id="dff74-174">ServicePrincipal creation is included in the audit report.</span></span>
2.  <span data-ttu-id="dff74-175">Sestavy aktivit přihlašování uživatele vám řeknou, ke které aplikaci se uživatel přihlašuje.</span><span class="sxs-lookup"><span data-stu-id="dff74-175">User sign-in activity reports tell you which app the user is signing in to.</span></span> 

## <a name="example"></a><span data-ttu-id="dff74-176">Příklad</span><span class="sxs-lookup"><span data-stu-id="dff74-176">Example</span></span>

<span data-ttu-id="dff74-177">Jako příklad si vezměme aplikaci FabrikamMail for Office 365, protože jste si všimli, že se k ní připojují uživatelé ve vašem tenantovi.</span><span class="sxs-lookup"><span data-stu-id="dff74-177">As an example, let’s take the “FabrikamMail for Office 365” app, which you’ve noticed users in your tenant are signing in to.</span></span> <span data-ttu-id="dff74-178">FabrikamMail je aplikace pro čtení pošty pro Android vytvořená společností Fabrikam, Inc.</span><span class="sxs-lookup"><span data-stu-id="dff74-178">“FabrikamMail” is a mail reader app for Android, published by “Fabrikam, Inc.”.</span></span> <span data-ttu-id="dff74-179">Spadá do kategorie Aplikace s více tenanty vyvíjené ostatními, se kterými může Contoso vyjádřit souhlas.</span><span class="sxs-lookup"><span data-stu-id="dff74-179">This falls into the “multi-tenant apps other develop, which Contoso can consent to”.</span></span>

<span data-ttu-id="dff74-180">Pokud je uživatelům dovoleno vyjadřovat souhlas, při prvním přihlášení se jim zobrazí dialogové okno pro vyjádření souhlasu: ![](media/active-directory-apps-permissions-consent/apps14.png)</span><span class="sxs-lookup"><span data-stu-id="dff74-180">If users are allowed to consent, they get a consent prompt the first time they sign in: ![](media/active-directory-apps-permissions-consent/apps14.png)</span></span>

<span data-ttu-id="dff74-181">„Access your mailboxes“ (Přístup k vašim poštovním schránkám) je řetězec souhlasu zobrazený uživateli pro oprávnění „Access mailboxes as the signed-in user“ (Přístup k poštovním schránkám jménem přihlášeného uživatele), které vystavuje Office 365 Exchange Online (tedy Exchange).</span><span class="sxs-lookup"><span data-stu-id="dff74-181">“Access your mailboxes” is the user-facing consent string for the “Access mailboxes as the signed-in user” permission exposed by “Office 365 Exchange Online” (that is, Exchange).</span></span>

<span data-ttu-id="dff74-182">Oprávnění můžete zobrazit vyhledáním objektu ServicePricipal pro Exchange (prostředek), který se přidal, když jste se zaregistrovali k Office 365.</span><span class="sxs-lookup"><span data-stu-id="dff74-182">You can see the permissions by looking up the ServicePrincipal object for Exchange (the resource), which was added when you signed up for Office 365.</span></span> <span data-ttu-id="dff74-183">Objekt ServicePrincipal si můžete představit jako instanci aplikace ve vašem tenantovi, která slouží k zaznamenávání různých možností a konfigurací.</span><span class="sxs-lookup"><span data-stu-id="dff74-183">You can think of the ServicePrincipal object of an “instance” of the app in your tenant, which is used for recording different options and configurations.</span></span>  <span data-ttu-id="dff74-184">Můžete to zobrazit pomocí rutiny prostředí PowerShell `Get-AzureADServicePrincipal`.</span><span class="sxs-lookup"><span data-stu-id="dff74-184">You can see this by using the `Get-AzureADServicePrincipal` in PowerShell.</span></span>

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
                                  AdminConsentDescription : Allows the app to have the same access to mailboxes as the signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as the signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows the app full access to your mailboxes on your behalf.
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

<span data-ttu-id="dff74-185">Souhlas začne platit, když uživatel klikne na Přijmout.</span><span class="sxs-lookup"><span data-stu-id="dff74-185">Consent is initiated when the user clicks “Accept”.</span></span> <span data-ttu-id="dff74-186">Nejprve se v tenantovi vytvoří objekt ServicePrincipal pro aplikaci FabrikamMail for Office 365.</span><span class="sxs-lookup"><span data-stu-id="dff74-186">First, a ServicePrincipal object for “FabrikamMail for Office 365” is created in the tenant.</span></span> <span data-ttu-id="dff74-187">Objekt ServicePrincipal vypadá nějak takto:</span><span class="sxs-lookup"><span data-stu-id="dff74-187">The ServicePrincipal looks something like this:</span></span>

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

<span data-ttu-id="dff74-188">Souhlas s aplikací vytvoří propojení Oauth2PermissionGrant mezi následujícími položkami:</span><span class="sxs-lookup"><span data-stu-id="dff74-188">Consenting to an app creates an Oauth2PermissionGrant link between the following:</span></span>
  
- <span data-ttu-id="dff74-189">objekt uživatele</span><span class="sxs-lookup"><span data-stu-id="dff74-189">the user object</span></span>
- <span data-ttu-id="dff74-190">ServicePrincipalName (hlavní název služby – SPN) klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="dff74-190">the client apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="dff74-191">ServicePrincipalName (hlavní název služby – SPN) aplikace prostředku</span><span class="sxs-lookup"><span data-stu-id="dff74-191">the resource apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="dff74-192">oprávnění v aplikaci prostředku</span><span class="sxs-lookup"><span data-stu-id="dff74-192">permissions in the resource app.</span></span>  

<span data-ttu-id="dff74-193">V případě FabrikamMail to vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="dff74-193">In the case of FabrikamMail, it looks something like this:</span></span>

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

<span data-ttu-id="dff74-194">(**ClientId** je ID instančního objektu aplikace FabrikamMail (který se právě vytvořil), **PrincipalId** je ID objektu uživatele (který vyjádřil souhlas), **ResourceId** je ID instančního objektu aplikace Exchange, Scope je oprávnění v aplikaci Exchange, se kterým se souhlasilo).</span><span class="sxs-lookup"><span data-stu-id="dff74-194">(**ClientId** is FabrikamMail’s service principal object ID (the one that just got created), **PrincipalId** is the user object ID (of the user who consented), **ResourceId** is Exchange’s service principal object ID, Scope is the permission in Exchange that was consented to).</span></span>

<span data-ttu-id="dff74-195">Pokud uživatelům není povoleno vyjadřovat souhlas, zobrazí se jim obrazovka s upozorněním, že je vyžadováno oprávnění.</span><span class="sxs-lookup"><span data-stu-id="dff74-195">If users are not allowed to consent, they will see a screen that says that permission is required.</span></span>

