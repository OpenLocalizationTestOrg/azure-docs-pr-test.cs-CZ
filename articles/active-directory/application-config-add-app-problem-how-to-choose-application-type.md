---
title: "Po přidání aplikace zadejte aplikaci, pro kterou toouse toochoose aaaHow | Microsoft Docs"
description: "Pochopení hello podporované typy aplikací můžete integrovat se službou Azure AD a jejich související možnosti konfigurace"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 46e3672e7f5048b0fa54171f0fc169362c9d5ac6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochoose-which-application-type-toouse-when-adding-an-application"></a><span data-ttu-id="f0ceb-103">Jak toochoose, která aplikace zadejte toouse při přidání aplikace</span><span class="sxs-lookup"><span data-stu-id="f0ceb-103">How toochoose which application type toouse when adding an application</span></span>

<span data-ttu-id="f0ceb-104">Tento článek vám pomůže toounderstand hello čtyři hlavní typy aplikací, které lze integrovat s Azure AD:</span><span class="sxs-lookup"><span data-stu-id="f0ceb-104">This article help you toounderstand hello four main types of applications you can integrate with Azure AD:</span></span>

* <span data-ttu-id="f0ceb-105">Co je podporováno každou z nich</span><span class="sxs-lookup"><span data-stu-id="f0ceb-105">What is supported by each of them</span></span>
* <span data-ttu-id="f0ceb-106">Proč můžete vybrat aplikaci, pro kterou</span><span class="sxs-lookup"><span data-stu-id="f0ceb-106">Why you might choose which application</span></span>
* <span data-ttu-id="f0ceb-107">Jak tooconfigure vlastnosti základní tyto aplikace, jako například jak budou uživatelé **zřízený**, nebo co **jednotného přihlašování** toouse technologie.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-107">How tooconfigure those application’s core properties, like how users are **provisioned**, or what **single sign-on** technology toouse.</span></span>

## <a name="supported-application-types-in-azure-ad"></a><span data-ttu-id="f0ceb-108">Typy podporovaných aplikací ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0ceb-108">Supported application types in Azure AD</span></span>

<span data-ttu-id="f0ceb-109">Azure AD podporuje čtyři hlavní typy aplikací, které můžete přidat pomocí hello **přidat** funkce najít v části **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-109">Azure AD supports four main application types that you can add using hello **Add** feature found under **Enterprise Applications**.</span></span> <span data-ttu-id="f0ceb-110">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="f0ceb-110">These include:</span></span>

-   <span data-ttu-id="f0ceb-111">**Galerii aplikací Azure AD** – aplikaci, která jsou předem integrované pro jednotné přihlašování s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-111">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

-   <span data-ttu-id="f0ceb-112">**Aplikace Proxy aplikace** – aplikace běžící v místním prostředí, které mají tooprovide zabezpečené jednotného přihlašování tooexternally.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-112">**Application Proxy Applications** – An application running in your on-premises environment that you want tooprovide secure single-sign on tooexternally.</span></span>

-   <span data-ttu-id="f0ceb-113">**Aplikace vyvinuté vlastní** – aplikace, které vaše organizace si přeje toodevelop na hello platformy vývoj aplikací Azure AD, ale který ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-113">**Custom-developed Applications** – An application that your organization wishes toodevelop on hello Azure AD Application Development Platform, but that may not exist yet.</span></span>

-   <span data-ttu-id="f0ceb-114">**Aplikace bez Galerie** – přineste si vlastní aplikace!</span><span class="sxs-lookup"><span data-stu-id="f0ceb-114">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="f0ceb-115">Všechny webový odkaz, který má být nebo všechny aplikace, která vykreslí pole uživatelské jméno a heslo, podporuje protokoly SAML nebo OpenID Connect, nebo podporuje SCIM, který chcete toointegrate pro jednotné přihlašování s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-115">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish toointegrate for single sign-on with Azure AD.</span></span>

## <a name="features-and-capabilities-supported-by-all-hello-above-application-types"></a><span data-ttu-id="f0ceb-116">Funkce a možnosti nepodporuje všechny hello výše typy aplikací</span><span class="sxs-lookup"><span data-stu-id="f0ceb-116">Features and capabilities supported by all hello above application types</span></span>

<span data-ttu-id="f0ceb-117">Hello následující funkce jsou podporovány všechny hello výše 4 typy aplikací ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="f0ceb-117">hello following features are supported by any of hello above 4 application types in Azure AD:</span></span>

-   <span data-ttu-id="f0ceb-118">**Rychlý start** – zprovoznění aplikace rychle podle [kroky jednoduché nasazení](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span><span class="sxs-lookup"><span data-stu-id="f0ceb-118">**Quick start** – get going with an application quickly by following [simple deployment steps](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span></span>

-   <span data-ttu-id="f0ceb-119">**Obecné vlastnosti správy** – získání [přímé odkazu deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) tooan aplikace [přizpůsobit hello branding](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) aplikace, nebo [vypnout aplikaci hello](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-119">**General properties management** – get a [direct deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) tooan application, [customize hello branding](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) of an application, or [disable hello application](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) for all users.</span></span>

-   <span data-ttu-id="f0ceb-120">**Správa uživatelů a skupin** – [přiřadit](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) nebo [odebrat](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) aplikace tooan uživatelů a skupin a volitelně přiřadit hello konkrétní aplikaci rolí tyto uživatele a skupiny mají přístup k</span><span class="sxs-lookup"><span data-stu-id="f0ceb-120">**User and group management** – [assign](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) or [remove](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) users and groups tooan application, and optionally assign hello specific application roles these users and groups have access to</span></span>

-   <span data-ttu-id="f0ceb-121">**Přístup k aplikaci Samoobslužné služby** – povolit vaši uživatelé toorequest [přístup k aplikaci Samoobslužné služby](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooan aplikaci z jejich přístup k aplikaci panely buď přidáním aplikace přímo nebo [ připojení ke skupině povoleno samoobslužné služby](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), volitelně vyžadující schválení obchodní podél hello způsob</span><span class="sxs-lookup"><span data-stu-id="f0ceb-121">**Self-service application access** – enable your users toorequest [self-service application access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooan application from their Application Access Panels either by adding an application directly or [joining a self-service enabled group](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), optionally requiring business approval along hello way</span></span>

-   <span data-ttu-id="f0ceb-122">**Protokoly přihlášení** – najdete v části [všechny hello přihlášení tooan aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), nebo všechny aplikace</span><span class="sxs-lookup"><span data-stu-id="f0ceb-122">**Sign-in logs** – see [all hello sign-ins tooan application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), or all your applications</span></span>

-   <span data-ttu-id="f0ceb-123">**Protokoly auditu** – viz [podrobné protokoly auditu o aplikaci tooan úpravy](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), nebo tooall aplikace</span><span class="sxs-lookup"><span data-stu-id="f0ceb-123">**Audit logs** – see [detailed audit logs about modifications tooan application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), or tooall your applications</span></span>

-   <span data-ttu-id="f0ceb-124">**Podmíněné a na základě riziko přístup** – nastavte výkonné [pravidel přístupu na základě podmínky](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) , se vynutí při pokusí toosign v konkrétní aplikaci tooa</span><span class="sxs-lookup"><span data-stu-id="f0ceb-124">**Conditional and risk-based access** – set powerful [condition-based access rules](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) that are enforced when users attempt toosign in tooa specific application</span></span>

-   <span data-ttu-id="f0ceb-125">**Zobrazení oprávnění** – zobrazit všechny hello [OAuth2 oprávnění](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) má aplikace přístup tooin adresáře z jednoho místa</span><span class="sxs-lookup"><span data-stu-id="f0ceb-125">**Permissions view** – view any of hello [OAuth2 permissions](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) an application has access tooin your directory from a single place</span></span>

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a><span data-ttu-id="f0ceb-126">Jednotné přihlašování a zřizování režimy podporované typy určitou aplikací</span><span class="sxs-lookup"><span data-stu-id="f0ceb-126">Single sign-on and provisioning modes supported by specific application types</span></span>

<span data-ttu-id="f0ceb-127">Následující tabulka Hello popisuje hello různých jednotné přihlašování a zřizování režimy podporované každou hello výše typy aplikací.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-127">hello table below describes hello different single sign-on and provisioning modes supported by each of hello above application types.</span></span> <span data-ttu-id="f0ceb-128">Můžete použít tuto tabulku toohelp toounderstand aplikaci, pro kterou je třeba tooadd toosupport dosažení určitého cíle.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-128">You can use this table toohelp you toounderstand which application you need tooadd toosupport a specific goal.</span></span>

  ![Tabulka typů aplikací](./media/application-tables/table1.png)

## <a name="how-toochoose-a-single-sign-on-mode"></a><span data-ttu-id="f0ceb-130">Jak toochoose jediný režim přihlášení</span><span class="sxs-lookup"><span data-stu-id="f0ceb-130">How toochoose a single sign-on mode</span></span>

<span data-ttu-id="f0ceb-131">Hello podporované **jednotného přihlašování** režimy pro aplikace, Azure AD jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-131">hello supported **single sign-on** modes for Azure AD applications are listed below.</span></span>

-   <span data-ttu-id="f0ceb-132">**Azure AD jednotné přihlašování zakázáno** – zvolte Azure AD jednotné přihlašování zakázáno **režimu přihlašování** Pokud ještě není připraven toointegrate tuto aplikaci pomocí jednotného přihlašování s Azure AD, nebo ho jednoduše testování</span><span class="sxs-lookup"><span data-stu-id="f0ceb-132">**Azure AD single sign-on disabled** – choose Azure AD single sign-on disabled **single sign-on mode** if you are not yet ready toointegrate this application with single sign-on with Azure AD, or are simply testing it out</span></span>

-   <span data-ttu-id="f0ceb-133">**Propojené přihlášení** – zvolte hello [propojené přihlášení](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud máte aplikaci, která je již připojen k existujícího jeden přihlašování řešení, nebo pokud chcete toopublish jednoduchý odkaz pro uživatele v jejich [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) nebo [Spouštěč aplikace Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span><span class="sxs-lookup"><span data-stu-id="f0ceb-133">**Linked Sign-on** – choose hello [Linked Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if you have an application that is already connected with an existing single sign-on solution, or if you just want toopublish a simple link for your users in their [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) or [Office 365 application launcher](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span></span>

-   <span data-ttu-id="f0ceb-134">**Založené na heslech přihlašování** – zvolte hello [založené na heslech přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud vaše aplikace vykreslí pole HTML uživatelské jméno a heslo a chcete toostore, uživatelské jméno a heslo bezpečně toobe přehrány toohello aplikaci později.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-134">**Password-based Sign-on** – choose hello [Password-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if your application renders an HTML username and password field and you want toostore that username and password securely toobe replayed toohello application later</span></span>

-   <span data-ttu-id="f0ceb-135">**Na základě SAML přihlašování** – zvolte hello [na základě SAML přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) na základě rolí toospecific aplikace jednotného přihlašování režim, pokud vaše aplikace podporuje protokoly hello SAML nebo OpenID Connect, nebo chcete, aby uživatelé moct toomap toobe v pravidlech definujete v vaší SAML deklarací *</span><span class="sxs-lookup"><span data-stu-id="f0ceb-135">**SAML-based Sign-on** – choose hello [SAML-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single-sign on mode if your application supports hello SAML or OpenID Connect protocols, or you want toobe able toomap users toospecific application roles based on rules you define in your SAML claims *</span></span>

   >[!NOTE]
   ><span data-ttu-id="f0ceb-136">Tato možnost není k dispozici, pokud proxy aplikace hello je nakonfigurovaný pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-136">This option is not available when hello application proxy is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="f0ceb-137">**Na základě záhlaví přihlašování** – tuto možnost vyberte, [na základě záhlaví přihlašování](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) jediný přihlašování režim Pokud máte aplikace pomocí PingAccess, který podporuje hlavičky protokolu HTTP na základě ověřování, které chcete tooperform jednotné přihlašování na příliš</span><span class="sxs-lookup"><span data-stu-id="f0ceb-137">**Header-based Sign-on** – choose this [Header-based Sign-on](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) single sign-on mode if you have an application using PingAccess that supports HTTP-header based authentication that you wish tooperform single-sign on too</span></span>

   >[!NOTE]
   ><span data-ttu-id="f0ceb-138">Tato možnost je dostupná, pouze pokud proxy aplikace hello a PingAccess nakonfigurovaný pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-138">This option is only available when hello application proxy and PingAccess is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="f0ceb-139">**Integrované ověřování systému Windows** – zvolte hello [integrované ověřování systému Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) jednotné přihlašování v režimu při vystavení místní WIA aplikaci, která chcete tooperform jednotné přihlašování na příliš</span><span class="sxs-lookup"><span data-stu-id="f0ceb-139">**Integrated Windows Authentication** – choose hello [Integrated Windows Authentication](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) single-sign on mode when exposing an on-premises WIA application that you wish tooperform single-sign on too</span></span>

   >[!NOTE]
   ><span data-ttu-id="f0ceb-140">Tato možnost je dostupná, pouze pokud proxy aplikace hello je nakonfigurovaný pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-140">This option is only available when hello application proxy is configured for an application.</span></span>
   >
   >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a><span data-ttu-id="f0ceb-141">Režimy jeden přihlašování pro aplikace vyvinuté vlastní</span><span class="sxs-lookup"><span data-stu-id="f0ceb-141">Single sign-on modes for custom-developed applications</span></span>

<span data-ttu-id="f0ceb-142">Aplikací mít vlastní vyvinuté pomocí hello [zákaznických aplikace](#_Custom-Developed_Applications) prostředí podporovat i další jeden přihlašování režimy nejsou uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-142">Applications you have custom developed through hello [Custom-developed application](#_Custom-Developed_Applications) experience also support additional single sign-on modes not listed above.</span></span> <span data-ttu-id="f0ceb-143">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="f0ceb-143">These include:</span></span>

-   <span data-ttu-id="f0ceb-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="f0ceb-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) based sign-on</span></span>

-   <span data-ttu-id="f0ceb-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="f0ceb-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) based sign-on</span></span>

-   <span data-ttu-id="f0ceb-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="f0ceb-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) based sign-on</span></span>

-   <span data-ttu-id="f0ceb-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="f0ceb-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) based sign-on</span></span>

<span data-ttu-id="f0ceb-148">Čtení hello [Příručka pro vývojáře Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn informace o tom, jak toocreate vyvinuté vlastní aplikaci, která podporuje tyto jednotné přihlašování režimy.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-148">Read hello [Azure Active Directory developer’s guide](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn more about how toocreate a custom-developed application which supports these single sign-on modes.</span></span>

## <a name="how-tooset-an-applications-single-sign-on-mode"></a><span data-ttu-id="f0ceb-149">Jak je jeden tooset aplikace přihlašování v režimu</span><span class="sxs-lookup"><span data-stu-id="f0ceb-149">How tooset an application’s single sign-on mode</span></span>

<span data-ttu-id="f0ceb-150">tooset aplikace na **jednotného přihlašování** režimu hello postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="f0ceb-150">tooset an application’s **single sign-on** mode, follow hello instructions below:</span></span>

1.  <span data-ttu-id="f0ceb-151">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="f0ceb-151">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="f0ceb-152">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-152">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f0ceb-153">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-153">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f0ceb-154">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-154">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f0ceb-155">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-155">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="f0ceb-156">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="f0ceb-156">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="f0ceb-157">Vyberte hello aplikaci, pro které chcete tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-157">Select hello application for which you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="f0ceb-158">Po načtení hello aplikace, klikněte na **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-158">Once hello application loads, click **Single sign-on** from hello application’s left hand navigation menu.</span></span>

## <a name="how-toochoose-a-provisioning-mode"></a><span data-ttu-id="f0ceb-159">Jak toochoose režim zřizování</span><span class="sxs-lookup"><span data-stu-id="f0ceb-159">How toochoose a provisioning mode</span></span>

-   <span data-ttu-id="f0ceb-160">**Ruční zřizování** – zvolte hello [ruční](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) režimu zřizování, pokud máte existující účty, nebo chcete toomanage účty pro tuto aplikaci mimo Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-160">**Manual Provisioning** – choose hello [Manual](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) provisioning mode if you have existing accounts, or wish toomanage accounts for this application outside of Azure AD.</span></span>

-   <span data-ttu-id="f0ceb-161">**Automatické zřizování** – zvolte hello [automatické](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **režim zřizování** Pokud chcete zřizování tooenable automatické založené na rozhraní API nebo zrušte zřizování uživatelských účtů toothis aplikace</span><span class="sxs-lookup"><span data-stu-id="f0ceb-161">**Automatic Provisioning** – choose hello [Automatic](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **provisioning mode** if you want tooenable automatic API-based provisioning and/or de-provisioning of user accounts toothis application</span></span> 

   >[!NOTE]
   ><span data-ttu-id="f0ceb-162">Tato možnost je dostupná pouze pro aplikace v rámci hello **vybrané** kategorie hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="f0ceb-162">This option is available only for applications within hello **featured** category of hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span></span>
   >
   >

-   <span data-ttu-id="f0ceb-163">**Na základě SCIM automatické zřizování** – použít [na základě SCIM automatické zřizování](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) Pokud vaše aplikace podporuje protokol hello SCIM pro zjišťování toousers změny a skupin, které jsou automaticky vygenerované změny aplikace tooany integrované s Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0ceb-163">**SCIM-based Automatic Provisioning** – use [SCIM-based Automatic Provisioning](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) if your application supports hello SCIM protocol for detecting changes toousers and groups, which are automatically emitted for changes tooany application integrated with Azure AD</span></span> 

   >[!NOTE]
   ><span data-ttu-id="f0ceb-164">Tato možnost není uvedena jako konkrétní režim zřizování, ale je povolená ve výchozím nastavení pro všechny aplikace, které jsou integrované s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-164">This option is not listed as a specific provisioning mode, but is enabled by default for all applications that are integrated with Azure AD.</span></span>
   >
   >

## <a name="how-tooset-an-applications-provisioning-mode"></a><span data-ttu-id="f0ceb-165">Jak je tooset aplikace režimu zřizování</span><span class="sxs-lookup"><span data-stu-id="f0ceb-165">How tooset an application’s provisioning mode</span></span>

<span data-ttu-id="f0ceb-166">tooset aplikace na **zřizování** režimu hello postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="f0ceb-166">tooset an application’s **provisioning** mode, follow hello instructions below:</span></span>

<span data-ttu-id="f0ceb-167">tooset aplikace na **jednotného přihlašování** režimu hello postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="f0ceb-167">tooset an application’s **single sign-on** mode, follow hello instructions below:</span></span>

1.  <span data-ttu-id="f0ceb-168">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="f0ceb-168">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="f0ceb-169">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-169">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f0ceb-170">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-170">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f0ceb-171">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-171">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f0ceb-172">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-172">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="f0ceb-173">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="f0ceb-173">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="f0ceb-174">Vyberte hello aplikaci, pro které chcete tooconfigure zřizování.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-174">Select hello application for which you want tooconfigure provisioning.</span></span>

7.  <span data-ttu-id="f0ceb-175">Po načtení hello aplikace, klikněte na **zřizování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="f0ceb-175">Once hello application loads, click **Provisioning** from hello application’s left hand navigation menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0ceb-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0ceb-176">Next steps</span></span>
[<span data-ttu-id="f0ceb-177">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f0ceb-177">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
