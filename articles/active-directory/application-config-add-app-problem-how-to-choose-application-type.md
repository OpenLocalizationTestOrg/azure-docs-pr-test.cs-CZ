---
title: "Jak vybrat typů aplikací, které se mají použít při přidávání aplikace | Microsoft Docs"
description: "Podporované typy aplikací můžete integrovat se službou Azure AD pochopit a jejich související možnosti konfigurace"
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
ms.openlocfilehash: e0d41d1933531c2c633613bcbc1bbcbf075d6a69
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-choose-which-application-type-to-use-when-adding-an-application"></a><span data-ttu-id="eea86-103">Jak vybrat typů aplikací, které se mají použít při přidávání aplikace</span><span class="sxs-lookup"><span data-stu-id="eea86-103">How to choose which application type to use when adding an application</span></span>

<span data-ttu-id="eea86-104">Tento článek vám pomůže lépe porozumět čtyři hlavní typy aplikací, které lze integrovat s Azure AD:</span><span class="sxs-lookup"><span data-stu-id="eea86-104">This article help you to understand the four main types of applications you can integrate with Azure AD:</span></span>

* <span data-ttu-id="eea86-105">Co je podporováno každou z nich</span><span class="sxs-lookup"><span data-stu-id="eea86-105">What is supported by each of them</span></span>
* <span data-ttu-id="eea86-106">Proč můžete vybrat aplikaci, pro kterou</span><span class="sxs-lookup"><span data-stu-id="eea86-106">Why you might choose which application</span></span>
* <span data-ttu-id="eea86-107">Postup konfigurace vlastností základní těchto aplikací, jako je jak budou uživatelé **zřízený**, nebo co **jednotného přihlašování** technologie používat.</span><span class="sxs-lookup"><span data-stu-id="eea86-107">How to configure those application’s core properties, like how users are **provisioned**, or what **single sign-on** technology to use.</span></span>

## <a name="supported-application-types-in-azure-ad"></a><span data-ttu-id="eea86-108">Typy podporovaných aplikací ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="eea86-108">Supported application types in Azure AD</span></span>

<span data-ttu-id="eea86-109">Azure AD podporuje čtyři typy hlavní aplikace, které můžete přidat pomocí **přidat** funkce najít v části **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="eea86-109">Azure AD supports four main application types that you can add using the **Add** feature found under **Enterprise Applications**.</span></span> <span data-ttu-id="eea86-110">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="eea86-110">These include:</span></span>

-   <span data-ttu-id="eea86-111">**Galerii aplikací Azure AD** – aplikaci, která jsou předem integrované pro jednotné přihlašování s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eea86-111">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

-   <span data-ttu-id="eea86-112">**Aplikace Proxy aplikace** – aplikace běžící v místním prostředí, které byste chtěli poskytnout zabezpečený jednotné přihlašování k externě.</span><span class="sxs-lookup"><span data-stu-id="eea86-112">**Application Proxy Applications** – An application running in your on-premises environment that you want to provide secure single-sign on to externally.</span></span>

-   <span data-ttu-id="eea86-113">**Aplikace vyvinuté vlastní** – aplikaci, kterou vaše organizace chce vývoji na platformu vývoj aplikací Azure AD, ale který ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="eea86-113">**Custom-developed Applications** – An application that your organization wishes to develop on the Azure AD Application Development Platform, but that may not exist yet.</span></span>

-   <span data-ttu-id="eea86-114">**Aplikace bez Galerie** – přineste si vlastní aplikace!</span><span class="sxs-lookup"><span data-stu-id="eea86-114">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="eea86-115">Všechny webový odkaz, který má být nebo všechny aplikace, která vykreslí pole uživatelské jméno a heslo, podporuje protokoly SAML nebo OpenID Connect, nebo podporuje SCIM, který chcete integrovat pro jednotné přihlašování s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eea86-115">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish to integrate for single sign-on with Azure AD.</span></span>

## <a name="features-and-capabilities-supported-by-all-the-above-application-types"></a><span data-ttu-id="eea86-116">Funkce a možnosti nepodporuje všechny výše uvedené typy aplikací</span><span class="sxs-lookup"><span data-stu-id="eea86-116">Features and capabilities supported by all the above application types</span></span>

<span data-ttu-id="eea86-117">Žádné z výše uvedených typů 4 aplikací ve službě Azure AD jsou podporovány následující funkce:</span><span class="sxs-lookup"><span data-stu-id="eea86-117">The following features are supported by any of the above 4 application types in Azure AD:</span></span>

-   <span data-ttu-id="eea86-118">**Rychlý start** – zprovoznění aplikace rychle podle [kroky jednoduché nasazení](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span><span class="sxs-lookup"><span data-stu-id="eea86-118">**Quick start** – get going with an application quickly by following [simple deployment steps](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span></span>

-   <span data-ttu-id="eea86-119">**Obecné vlastnosti správy** – získání [přímé odkazu deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) aplikace, [přizpůsobit branding](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) aplikace, nebo [zakázání aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="eea86-119">**General properties management** – get a [direct deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) to an application, [customize the branding](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) of an application, or [disable the application](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) for all users.</span></span>

-   <span data-ttu-id="eea86-120">**Správa uživatelů a skupin** – [přiřadit](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) nebo [odebrat](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) uživatelů a skupin k aplikaci a volitelně přiřadit role konkrétní aplikaci tito uživatelé a skupiny mají přístup k</span><span class="sxs-lookup"><span data-stu-id="eea86-120">**User and group management** – [assign](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) or [remove](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) users and groups to an application, and optionally assign the specific application roles these users and groups have access to</span></span>

-   <span data-ttu-id="eea86-121">**Přístup k aplikaci Samoobslužné služby** – mohou uživatelé požádat o [přístup k aplikaci Samoobslužné služby](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) z jejich panelů přístupu aplikace buď pomocí přidání aplikace přímo k aplikaci nebo [připojení ke skupině povoleno samoobslužné služby](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), volitelně vyžadující schválení obchodní na cestě</span><span class="sxs-lookup"><span data-stu-id="eea86-121">**Self-service application access** – enable your users to request [self-service application access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) to an application from their Application Access Panels either by adding an application directly or [joining a self-service enabled group](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), optionally requiring business approval along the way</span></span>

-   <span data-ttu-id="eea86-122">**Protokoly přihlášení** – najdete v části [všechny přihlášení k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), nebo všechny aplikace</span><span class="sxs-lookup"><span data-stu-id="eea86-122">**Sign-in logs** – see [all the sign-ins to an application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), or all your applications</span></span>

-   <span data-ttu-id="eea86-123">**Protokoly auditu** – viz [podrobné protokoly auditu o změny aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), nebo pro všechny aplikace</span><span class="sxs-lookup"><span data-stu-id="eea86-123">**Audit logs** – see [detailed audit logs about modifications to an application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), or to all your applications</span></span>

-   <span data-ttu-id="eea86-124">**Podmíněné a na základě riziko přístup** – nastavte výkonné [pravidel přístupu na základě podmínky](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) , se vynucují, když se uživatelé pokusí přihlásit na konkrétní aplikaci</span><span class="sxs-lookup"><span data-stu-id="eea86-124">**Conditional and risk-based access** – set powerful [condition-based access rules](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) that are enforced when users attempt to sign in to a specific application</span></span>

-   <span data-ttu-id="eea86-125">**Zobrazení oprávnění** – zobrazit žádné z [OAuth2 oprávnění](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) má aplikace přístup k ve vašem adresáři z jedné umístit</span><span class="sxs-lookup"><span data-stu-id="eea86-125">**Permissions view** – view any of the [OAuth2 permissions](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) an application has access to in your directory from a single place</span></span>

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a><span data-ttu-id="eea86-126">Jednotné přihlašování a zřizování režimy podporované typy určitou aplikací</span><span class="sxs-lookup"><span data-stu-id="eea86-126">Single sign-on and provisioning modes supported by specific application types</span></span>

<span data-ttu-id="eea86-127">Následující tabulka popisuje různé jednotné přihlašování a zřizování režimy podporované každou z výše uvedených typů aplikací.</span><span class="sxs-lookup"><span data-stu-id="eea86-127">The table below describes the different single sign-on and provisioning modes supported by each of the above application types.</span></span> <span data-ttu-id="eea86-128">Tato tabulka vám pomůže vám pomůže lépe porozumět aplikaci, pro kterou je nutné přidat na podporu dosažení určitého cíle.</span><span class="sxs-lookup"><span data-stu-id="eea86-128">You can use this table to help you to understand which application you need to add to support a specific goal.</span></span>

  ![Tabulka typů aplikací](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a><span data-ttu-id="eea86-130">Jak vybrat jediný režim přihlášení</span><span class="sxs-lookup"><span data-stu-id="eea86-130">How to choose a single sign-on mode</span></span>

<span data-ttu-id="eea86-131">U podporovaných **jednotného přihlašování** režimy pro aplikace, Azure AD jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="eea86-131">The supported **single sign-on** modes for Azure AD applications are listed below.</span></span>

-   <span data-ttu-id="eea86-132">**Azure AD jednotné přihlašování zakázáno** – zvolte Azure AD jednotné přihlašování zakázáno **režimu přihlašování** Pokud ještě nejsou připraveny k integraci tuto aplikaci pomocí jednotného přihlašování s Azure AD, nebo ho jednoduše testování</span><span class="sxs-lookup"><span data-stu-id="eea86-132">**Azure AD single sign-on disabled** – choose Azure AD single sign-on disabled **single sign-on mode** if you are not yet ready to integrate this application with single sign-on with Azure AD, or are simply testing it out</span></span>

-   <span data-ttu-id="eea86-133">**Propojené přihlášení** – zvolte [propojené přihlášení](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud máte aplikaci, která je již připojen k existujícího jeden přihlašování řešení, nebo pokud chcete publikovat jednoduchý odkaz pro uživatele v jejich [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) nebo [Spouštěč aplikace Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span><span class="sxs-lookup"><span data-stu-id="eea86-133">**Linked Sign-on** – choose the [Linked Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if you have an application that is already connected with an existing single sign-on solution, or if you just want to publish a simple link for your users in their [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) or [Office 365 application launcher](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span></span>

-   <span data-ttu-id="eea86-134">**Založené na heslech přihlašování** – zvolte [založené na heslech přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud vaše aplikace vykreslí pole HTML uživatelské jméno a heslo a chcete uložit tento uživatelské jméno a heslo do přehrány do aplikace později</span><span class="sxs-lookup"><span data-stu-id="eea86-134">**Password-based Sign-on** – choose the [Password-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if your application renders an HTML username and password field and you want to store that username and password securely to be replayed to the application later</span></span>

-   <span data-ttu-id="eea86-135">**Na základě SAML přihlašování** – zvolte [na základě SAML přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) jednotné přihlašování v režimu, pokud vaše aplikace podporuje protokoly SAML nebo OpenID Connect, nebo chcete být schopni mapování uživatelů na konkrétní aplikaci role na základě pravidel, můžete definovat vaše SAML deklaracemi identity *</span><span class="sxs-lookup"><span data-stu-id="eea86-135">**SAML-based Sign-on** – choose the [SAML-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single-sign on mode if your application supports the SAML or OpenID Connect protocols, or you want to be able to map users to specific application roles based on rules you define in your SAML claims *</span></span>

   >[!NOTE]
   ><span data-ttu-id="eea86-136">Tato možnost není k dispozici, když je proxy aplikací nakonfigurovaný pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eea86-136">This option is not available when the application proxy is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="eea86-137">**Na základě záhlaví přihlašování** – tuto možnost vyberte, [na základě záhlaví přihlašování](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) jediný přihlašování režim Pokud máte aplikace pomocí PingAccess, který podporuje hlavičky protokolu HTTP na základě ověřování, které chcete provést jednotné přihlašování k</span><span class="sxs-lookup"><span data-stu-id="eea86-137">**Header-based Sign-on** – choose this [Header-based Sign-on](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) single sign-on mode if you have an application using PingAccess that supports HTTP-header based authentication that you wish to perform single-sign on to</span></span> 

   >[!NOTE]
   ><span data-ttu-id="eea86-138">Tato možnost je dostupná, pouze pokud proxy aplikace a PingAccess nakonfigurovaný pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eea86-138">This option is only available when the application proxy and PingAccess is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="eea86-139">**Integrované ověřování systému Windows** – zvolte [integrované ověřování systému Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) jednotné přihlašování v režimu při vystavení, kterou chcete provést jednotné přihlašování k aplikaci WIA místní</span><span class="sxs-lookup"><span data-stu-id="eea86-139">**Integrated Windows Authentication** – choose the [Integrated Windows Authentication](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) single-sign on mode when exposing an on-premises WIA application that you wish to perform single-sign on to</span></span> 

   >[!NOTE]
   ><span data-ttu-id="eea86-140">Tato možnost je dostupná, pouze pokud je nakonfigurován proxy aplikace pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eea86-140">This option is only available when the application proxy is configured for an application.</span></span>
   >
   >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a><span data-ttu-id="eea86-141">Režimy jeden přihlašování pro aplikace vyvinuté vlastní</span><span class="sxs-lookup"><span data-stu-id="eea86-141">Single sign-on modes for custom-developed applications</span></span>

<span data-ttu-id="eea86-142">Aplikací mít vlastní vyvinuté pomocí [zákaznických aplikace](#_Custom-Developed_Applications) prostředí podporovat i další jeden přihlašování režimy nejsou uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="eea86-142">Applications you have custom developed through the [Custom-developed application](#_Custom-Developed_Applications) experience also support additional single sign-on modes not listed above.</span></span> <span data-ttu-id="eea86-143">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="eea86-143">These include:</span></span>

-   <span data-ttu-id="eea86-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="eea86-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) based sign-on</span></span>

-   <span data-ttu-id="eea86-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="eea86-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) based sign-on</span></span>

-   <span data-ttu-id="eea86-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="eea86-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) based sign-on</span></span>

-   <span data-ttu-id="eea86-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="eea86-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) based sign-on</span></span>

<span data-ttu-id="eea86-148">Pro čtení [Příručka pro vývojáře Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) Další informace o tom, jak vytvořit vyvinuté vlastní aplikaci, která podporuje tyto režimy jednom přihlášení.</span><span class="sxs-lookup"><span data-stu-id="eea86-148">Read the [Azure Active Directory developer’s guide](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) to learn more about how to create a custom-developed application which supports these single sign-on modes.</span></span>

## <a name="how-to-set-an-applications-single-sign-on-mode"></a><span data-ttu-id="eea86-149">Postup nastavení aplikace jediný přihlašování režim</span><span class="sxs-lookup"><span data-stu-id="eea86-149">How to set an application’s single sign-on mode</span></span>

<span data-ttu-id="eea86-150">Nastavení aplikace **jednotného přihlašování** režimu, postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="eea86-150">To set an application’s **single sign-on** mode, follow the instructions below:</span></span>

1.  <span data-ttu-id="eea86-151">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="eea86-151">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="eea86-152">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="eea86-152">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eea86-153">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eea86-153">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eea86-154">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="eea86-154">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eea86-155">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="eea86-155">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="eea86-156">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="eea86-156">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="eea86-157">Vyberte aplikaci, pro který chcete nakonfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eea86-157">Select the application for which you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="eea86-158">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="eea86-158">Once the application loads, click **Single sign-on** from the application’s left hand navigation menu.</span></span>

## <a name="how-to-choose-a-provisioning-mode"></a><span data-ttu-id="eea86-159">Jak vybrat režim zřizování</span><span class="sxs-lookup"><span data-stu-id="eea86-159">How to choose a provisioning mode</span></span>

-   <span data-ttu-id="eea86-160">**Ruční zřizování** – zvolte [ruční](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) režimu zřizování, pokud máte existující účty, nebo chcete spravovat účty pro tuto aplikaci mimo Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eea86-160">**Manual Provisioning** – choose the [Manual](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) provisioning mode if you have existing accounts, or wish to manage accounts for this application outside of Azure AD.</span></span>

-   <span data-ttu-id="eea86-161">**Automatické zřizování** – zvolte [automatické](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **režim zřizování** Pokud chcete povolit automatické zřizování na základě rozhraní API a jeho rušení uživatelských účtů do této aplikace</span><span class="sxs-lookup"><span data-stu-id="eea86-161">**Automatic Provisioning** – choose the [Automatic](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **provisioning mode** if you want to enable automatic API-based provisioning and/or de-provisioning of user accounts to this application</span></span> 

   >[!NOTE]
   ><span data-ttu-id="eea86-162">Tato možnost je dostupná pouze pro aplikace v rámci **vybrané** kategorii [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="eea86-162">This option is available only for applications within the **featured** category of the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span></span>
   >
   >

-   <span data-ttu-id="eea86-163">**Na základě SCIM automatické zřizování** – použít [na základě SCIM automatické zřizování](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) Pokud vaše aplikace podporuje protokol SCIM pro zjišťování změny uživatelů a skupin, které jsou automaticky vygenerované změny všech aplikací integrované s Azure AD</span><span class="sxs-lookup"><span data-stu-id="eea86-163">**SCIM-based Automatic Provisioning** – use [SCIM-based Automatic Provisioning](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) if your application supports the SCIM protocol for detecting changes to users and groups, which are automatically emitted for changes to any application integrated with Azure AD</span></span> 

   >[!NOTE]
   ><span data-ttu-id="eea86-164">Tato možnost není uvedena jako konkrétní režim zřizování, ale je povolená ve výchozím nastavení pro všechny aplikace, které jsou integrované s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eea86-164">This option is not listed as a specific provisioning mode, but is enabled by default for all applications that are integrated with Azure AD.</span></span>
   >
   >

## <a name="how-to-set-an-applications-provisioning-mode"></a><span data-ttu-id="eea86-165">Postup nastavení aplikace je režimu zřizování</span><span class="sxs-lookup"><span data-stu-id="eea86-165">How to set an application’s provisioning mode</span></span>

<span data-ttu-id="eea86-166">Chcete-li nastavit aplikace **zřizování** režimu, postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="eea86-166">To set an application’s **provisioning** mode, follow the instructions below:</span></span>

<span data-ttu-id="eea86-167">Nastavení aplikace **jednotného přihlašování** režimu, postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="eea86-167">To set an application’s **single sign-on** mode, follow the instructions below:</span></span>

1.  <span data-ttu-id="eea86-168">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="eea86-168">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="eea86-169">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="eea86-169">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eea86-170">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eea86-170">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eea86-171">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="eea86-171">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eea86-172">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="eea86-172">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="eea86-173">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="eea86-173">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="eea86-174">Vyberte aplikaci, pro kterou chcete provést konfiguraci zřizování.</span><span class="sxs-lookup"><span data-stu-id="eea86-174">Select the application for which you want to configure provisioning.</span></span>

7.  <span data-ttu-id="eea86-175">Po načtení aplikace, klikněte na **zřizování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="eea86-175">Once the application loads, click **Provisioning** from the application’s left hand navigation menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eea86-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eea86-176">Next steps</span></span>
[<span data-ttu-id="eea86-177">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eea86-177">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
