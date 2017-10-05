---
title: "Jak zjistit, jaké jednotného přihlašování metodu použít | Microsoft Docs"
description: "Pochopit režimy jeden přihlašování podporované službou Azure AD a jak vybrat volbě pro aplikace, které vás zajímají."
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
ms.openlocfilehash: 80f4a965920fec9cb578c1bee235c7857f37431e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-determine-what-single-sign-on-method-to-use"></a><span data-ttu-id="f68ba-103">Jak zjistit, jaké jednotného přihlašování metoda se má použít</span><span class="sxs-lookup"><span data-stu-id="f68ba-103">How to determine what single-sign on method to use</span></span>

<span data-ttu-id="f68ba-104">Tento článek vám pomůže lépe porozumět režimy jeden přihlašování podporované službou Azure AD a jak vybrat volbě pro aplikace, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="f68ba-104">This article help you to understand the single sign-on modes supported by Azure AD and how to pick which one to choose for the application you are interested in.</span></span>

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a><span data-ttu-id="f68ba-105">Jednotné přihlašování a zřizování režimy podporované typy určitou aplikací</span><span class="sxs-lookup"><span data-stu-id="f68ba-105">Single sign-on and provisioning modes supported by specific application types</span></span>

<span data-ttu-id="f68ba-106">Následující tabulka popisuje různé jednotné přihlašování a zřizování režimy podporované každou z výše uvedených typů aplikací.</span><span class="sxs-lookup"><span data-stu-id="f68ba-106">The table below describes the different single sign-on and provisioning modes supported by each of the above application types.</span></span> <span data-ttu-id="f68ba-107">Tato tabulka vám pomůže vám pomůže lépe porozumět aplikaci, pro kterou je nutné přidat na podporu dosažení určitého cíle.</span><span class="sxs-lookup"><span data-stu-id="f68ba-107">You can use this table to help you to understand which application you need to add to support a specific goal.</span></span>

  ![Tabulka typů Asie a Tichomoří](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a><span data-ttu-id="f68ba-109">Jak vybrat jediný režim přihlášení</span><span class="sxs-lookup"><span data-stu-id="f68ba-109">How to choose a single sign-on mode</span></span>

<span data-ttu-id="f68ba-110">U podporovaných **jednotného přihlašování** režimy pro aplikace, Azure AD jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="f68ba-110">The supported **single sign-on** modes for Azure AD applications are listed below.</span></span>

-   <span data-ttu-id="f68ba-111">**Azure AD jednotné přihlašování zakázáno** – zvolte Azure AD jednotné přihlašování zakázáno **režimu přihlašování** Pokud ještě nejsou připraveny k integraci tuto aplikaci pomocí jednotného přihlašování s Azure AD, nebo ho jednoduše testování</span><span class="sxs-lookup"><span data-stu-id="f68ba-111">**Azure AD single sign-on disabled** – choose Azure AD single sign-on disabled **single sign-on mode** if you are not yet ready to integrate this application with single sign-on with Azure AD, or are simply testing it out</span></span>

-   <span data-ttu-id="f68ba-112">**Propojené přihlášení** – zvolte [propojené přihlášení](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud máte aplikaci, která je již připojen k existujícího jeden přihlašování řešení, nebo pokud chcete publikovat jednoduchý odkaz pro uživatele v jejich [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) nebo [Spouštěč aplikace Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span><span class="sxs-lookup"><span data-stu-id="f68ba-112">**Linked Sign-on** – choose the [Linked Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if you have an application that is already connected with an existing single sign-on solution, or if you just want to publish a simple link for your users in their [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) or [Office 365 application launcher](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span></span>

-   <span data-ttu-id="f68ba-113">**Založené na heslech přihlašování** – zvolte [založené na heslech přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud vaše aplikace vykreslí pole HTML uživatelské jméno a heslo a chcete uložit tento uživatelské jméno a heslo do přehrány do aplikace později</span><span class="sxs-lookup"><span data-stu-id="f68ba-113">**Password-based Sign-on** – choose the [Password-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if your application renders an HTML username and password field and you want to store that username and password securely to be replayed to the application later</span></span>

-   <span data-ttu-id="f68ba-114">**Na základě SAML přihlašování** – zvolte [na základě SAML přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) jednotné přihlašování v režimu, pokud vaše aplikace podporuje protokoly SAML nebo OpenID Connect, nebo chcete být schopni mapování uživatelů na konkrétní aplikaci role na základě pravidel definování v deklaracích identity vaší SAML *(**Poznámka:** tato možnost není k dispozici, když je proxy aplikací nakonfigurovaný pro aplikaci) *</span><span class="sxs-lookup"><span data-stu-id="f68ba-114">**SAML-based Sign-on** – choose the [SAML-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single-sign on mode if your application supports the SAML or OpenID Connect protocols, or you want to be able to map users to specific application roles based on rules you define in your SAML claims *(**Note:** this option is not available when the application proxy is configured for an application)*</span></span>

-   <span data-ttu-id="f68ba-115">**Na základě záhlaví přihlašování** – tuto možnost vyberte, [na základě záhlaví přihlašování](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) jediný přihlašování režim Pokud máte aplikace pomocí PingAccess, který podporuje hlavičky protokolu HTTP na základě ověřování, které chcete provést jednotné přihlašování k *(**Poznámka:** tato možnost je dostupná pouze v případě proxy aplikací a PingAccess konfigurován pro aplikaci) *</span><span class="sxs-lookup"><span data-stu-id="f68ba-115">**Header-based Sign-on** – choose this [Header-based Sign-on](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) single sign-on mode if you have an application using PingAccess that supports HTTP-header based authentication that you wish to perform single-sign on to *(**Note:** this option is only available when the application proxy and PingAccess is configured for an application)*</span></span>

-   <span data-ttu-id="f68ba-116">**Integrované ověřování systému Windows** – zvolte [integrované ověřování systému Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) jednotné přihlašování v režimu při vystavení, kterou chcete provést jednotné přihlašování k aplikaci WIA místní *(* *Poznámka:** tato možnost je dostupná, pouze pokud je nakonfigurován proxy aplikace pro aplikaci) *</span><span class="sxs-lookup"><span data-stu-id="f68ba-116">**Integrated Windows Authentication** – choose the [Integrated Windows Authentication](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) single-sign on mode when exposing an on-premises WIA application that you wish to perform single-sign on to *(**Note:** this option is only available when the application proxy is configured for an application)*</span></span>

## <a name="single-sign-on-modes-for-custom-developed-applications"></a><span data-ttu-id="f68ba-117">Režimy jeden přihlašování pro aplikace vyvinuté vlastní</span><span class="sxs-lookup"><span data-stu-id="f68ba-117">Single sign-on modes for custom-developed applications</span></span>

<span data-ttu-id="f68ba-118">Aplikací mít vlastní vyvinuté pomocí [zákaznických aplikace](#_Custom-Developed_Applications) prostředí podporovat i další jeden přihlašování režimy nejsou uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="f68ba-118">Applications you have custom developed through the [Custom-developed application](#_Custom-Developed_Applications) experience also support additional single sign-on modes not listed above.</span></span> <span data-ttu-id="f68ba-119">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="f68ba-119">These include:</span></span>

-   <span data-ttu-id="f68ba-120">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="f68ba-120">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) based sign-on</span></span>

-   <span data-ttu-id="f68ba-121">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="f68ba-121">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) based sign-on</span></span>

-   <span data-ttu-id="f68ba-122">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="f68ba-122">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) based sign-on</span></span>

-   <span data-ttu-id="f68ba-123">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="f68ba-123">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) based sign-on</span></span>

<span data-ttu-id="f68ba-124">Pro čtení [Příručka pro vývojáře Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) Další informace o tom, jak vytvořit vyvinuté vlastní aplikaci, která podporuje tyto režimy jednom přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f68ba-124">Read the [Azure Active Directory developer’s guide](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) to learn more about how to create a custom-developed application which supports these single sign-on modes.</span></span>

## <a name="how-to-set-an-applications-single-sign-on-mode"></a><span data-ttu-id="f68ba-125">Postup nastavení aplikace jediný přihlašování režim</span><span class="sxs-lookup"><span data-stu-id="f68ba-125">How to set an application’s single sign-on mode</span></span>

<span data-ttu-id="f68ba-126">Nastavení aplikace **jednotného přihlašování** režimu, postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="f68ba-126">To set an application’s **single sign-on** mode, follow the instructions below:</span></span>

1.  <span data-ttu-id="f68ba-127">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="f68ba-127">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="f68ba-128">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="f68ba-128">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f68ba-129">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="f68ba-129">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f68ba-130">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f68ba-130">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f68ba-131">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="f68ba-131">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="f68ba-132">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="f68ba-132">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="f68ba-133">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f68ba-133">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="f68ba-134">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="f68ba-134">Once the application loads, click **Single sign-on** from the application’s left hand navigation menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f68ba-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f68ba-135">Next steps</span></span>
[<span data-ttu-id="f68ba-136">Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="f68ba-136">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

