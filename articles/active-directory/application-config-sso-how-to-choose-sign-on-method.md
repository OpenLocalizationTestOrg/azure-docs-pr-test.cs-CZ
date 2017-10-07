---
title: "aaaHow toodetermine jaké jednotného přihlašování metoda toouse | Microsoft Docs"
description: "Pochopení hello jeden přihlašování režimy podporované službou Azure AD a jak toopick které jeden toochoose pro hello aplikace vás zajímá."
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
ms.openlocfilehash: 64f0bc1dc8281d1ab8222fd50eaceaf710704886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetermine-what-single-sign-on-method-toouse"></a><span data-ttu-id="e20c0-103">Jak toodetermine jaké jednotného přihlašování toouse – metoda</span><span class="sxs-lookup"><span data-stu-id="e20c0-103">How toodetermine what single-sign on method toouse</span></span>

<span data-ttu-id="e20c0-104">Tento článek vám pomůže toounderstand hello jeden přihlašování režimy podporované službou Azure AD a jak toopick které jeden toochoose pro hello aplikace vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="e20c0-104">This article help you toounderstand hello single sign-on modes supported by Azure AD and how toopick which one toochoose for hello application you are interested in.</span></span>

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a><span data-ttu-id="e20c0-105">Jednotné přihlašování a zřizování režimy podporované typy určitou aplikací</span><span class="sxs-lookup"><span data-stu-id="e20c0-105">Single sign-on and provisioning modes supported by specific application types</span></span>

<span data-ttu-id="e20c0-106">Následující tabulka Hello popisuje hello různých jednotné přihlašování a zřizování režimy podporované každou hello výše typy aplikací.</span><span class="sxs-lookup"><span data-stu-id="e20c0-106">hello table below describes hello different single sign-on and provisioning modes supported by each of hello above application types.</span></span> <span data-ttu-id="e20c0-107">Můžete použít tuto tabulku toohelp toounderstand aplikaci, pro kterou je třeba tooadd toosupport dosažení určitého cíle.</span><span class="sxs-lookup"><span data-stu-id="e20c0-107">You can use this table toohelp you toounderstand which application you need tooadd toosupport a specific goal.</span></span>

  ![Tabulka typů Asie a Tichomoří](./media/application-tables/table1.png)

## <a name="how-toochoose-a-single-sign-on-mode"></a><span data-ttu-id="e20c0-109">Jak toochoose jediný režim přihlášení</span><span class="sxs-lookup"><span data-stu-id="e20c0-109">How toochoose a single sign-on mode</span></span>

<span data-ttu-id="e20c0-110">Hello podporované **jednotného přihlašování** režimy pro aplikace, Azure AD jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="e20c0-110">hello supported **single sign-on** modes for Azure AD applications are listed below.</span></span>

-   <span data-ttu-id="e20c0-111">**Azure AD jednotné přihlašování zakázáno** – zvolte Azure AD jednotné přihlašování zakázáno **režimu přihlašování** Pokud ještě není připraven toointegrate tuto aplikaci pomocí jednotného přihlašování s Azure AD, nebo ho jednoduše testování</span><span class="sxs-lookup"><span data-stu-id="e20c0-111">**Azure AD single sign-on disabled** – choose Azure AD single sign-on disabled **single sign-on mode** if you are not yet ready toointegrate this application with single sign-on with Azure AD, or are simply testing it out</span></span>

-   <span data-ttu-id="e20c0-112">**Propojené přihlášení** – zvolte hello [propojené přihlášení](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud máte aplikaci, která je již připojen k existujícího jeden přihlašování řešení, nebo pokud chcete toopublish jednoduchý odkaz pro uživatele v jejich [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) nebo [Spouštěč aplikace Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span><span class="sxs-lookup"><span data-stu-id="e20c0-112">**Linked Sign-on** – choose hello [Linked Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if you have an application that is already connected with an existing single sign-on solution, or if you just want toopublish a simple link for your users in their [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) or [Office 365 application launcher](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span></span>

-   <span data-ttu-id="e20c0-113">**Založené na heslech přihlašování** – zvolte hello [založené na heslech přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud vaše aplikace vykreslí pole HTML uživatelské jméno a heslo a chcete toostore, uživatelské jméno a heslo bezpečně toobe přehrány toohello aplikaci později.</span><span class="sxs-lookup"><span data-stu-id="e20c0-113">**Password-based Sign-on** – choose hello [Password-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if your application renders an HTML username and password field and you want toostore that username and password securely toobe replayed toohello application later</span></span>

-   <span data-ttu-id="e20c0-114">**Na základě SAML přihlašování** – zvolte hello [na základě SAML přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) na základě rolí toospecific aplikace jednotného přihlašování režim, pokud vaše aplikace podporuje protokoly hello SAML nebo OpenID Connect, nebo chcete, aby uživatelé moct toomap toobe v pravidlech definujete v vaší SAML deklarací *(**Poznámka:** tato možnost není k dispozici, pokud proxy aplikace hello je nakonfigurovaný pro aplikaci) *</span><span class="sxs-lookup"><span data-stu-id="e20c0-114">**SAML-based Sign-on** – choose hello [SAML-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single-sign on mode if your application supports hello SAML or OpenID Connect protocols, or you want toobe able toomap users toospecific application roles based on rules you define in your SAML claims *(**Note:** this option is not available when hello application proxy is configured for an application)*</span></span>

-   <span data-ttu-id="e20c0-115">**Na základě záhlaví přihlašování** – tuto možnost vyberte, [na základě záhlaví přihlašování](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) jediný přihlašování režim Pokud máte aplikace pomocí PingAccess, který podporuje hlavičky protokolu HTTP na základě ověřování, které chcete tooperform jednotné přihlašování na příliš *(**Poznámka:** tato možnost je dostupná pouze v případě proxy aplikace hello a PingAccess konfigurován pro aplikaci) *</span><span class="sxs-lookup"><span data-stu-id="e20c0-115">**Header-based Sign-on** – choose this [Header-based Sign-on](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) single sign-on mode if you have an application using PingAccess that supports HTTP-header based authentication that you wish tooperform single-sign on too*(**Note:** this option is only available when hello application proxy and PingAccess is configured for an application)*</span></span>

-   <span data-ttu-id="e20c0-116">**Integrované ověřování systému Windows** – zvolte hello [integrované ověřování systému Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) jednotné přihlašování v režimu při vystavení místní WIA aplikaci, která chcete tooperform jednotné přihlašování na příliš*()*  *Poznámka:** tato možnost je dostupná, pouze pokud proxy aplikace hello je nakonfigurovaný pro aplikaci) *</span><span class="sxs-lookup"><span data-stu-id="e20c0-116">**Integrated Windows Authentication** – choose hello [Integrated Windows Authentication](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) single-sign on mode when exposing an on-premises WIA application that you wish tooperform single-sign on too*(**Note:** this option is only available when hello application proxy is configured for an application)*</span></span>

## <a name="single-sign-on-modes-for-custom-developed-applications"></a><span data-ttu-id="e20c0-117">Režimy jeden přihlašování pro aplikace vyvinuté vlastní</span><span class="sxs-lookup"><span data-stu-id="e20c0-117">Single sign-on modes for custom-developed applications</span></span>

<span data-ttu-id="e20c0-118">Aplikací mít vlastní vyvinuté pomocí hello [zákaznických aplikace](#_Custom-Developed_Applications) prostředí podporovat i další jeden přihlašování režimy nejsou uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="e20c0-118">Applications you have custom developed through hello [Custom-developed application](#_Custom-Developed_Applications) experience also support additional single sign-on modes not listed above.</span></span> <span data-ttu-id="e20c0-119">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="e20c0-119">These include:</span></span>

-   <span data-ttu-id="e20c0-120">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="e20c0-120">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) based sign-on</span></span>

-   <span data-ttu-id="e20c0-121">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="e20c0-121">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) based sign-on</span></span>

-   <span data-ttu-id="e20c0-122">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="e20c0-122">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) based sign-on</span></span>

-   <span data-ttu-id="e20c0-123">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) na základě přihlášení</span><span class="sxs-lookup"><span data-stu-id="e20c0-123">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) based sign-on</span></span>

<span data-ttu-id="e20c0-124">Čtení hello [Příručka pro vývojáře Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn informace o tom, jak toocreate vyvinuté vlastní aplikaci, která podporuje tyto jednotné přihlašování režimy.</span><span class="sxs-lookup"><span data-stu-id="e20c0-124">Read hello [Azure Active Directory developer’s guide](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn more about how toocreate a custom-developed application which supports these single sign-on modes.</span></span>

## <a name="how-tooset-an-applications-single-sign-on-mode"></a><span data-ttu-id="e20c0-125">Jak je jeden tooset aplikace přihlašování v režimu</span><span class="sxs-lookup"><span data-stu-id="e20c0-125">How tooset an application’s single sign-on mode</span></span>

<span data-ttu-id="e20c0-126">tooset aplikace na **jednotného přihlašování** režimu hello postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="e20c0-126">tooset an application’s **single sign-on** mode, follow hello instructions below:</span></span>

1.  <span data-ttu-id="e20c0-127">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="e20c0-127">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="e20c0-128">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="e20c0-128">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="e20c0-129">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="e20c0-129">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="e20c0-130">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="e20c0-130">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="e20c0-131">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="e20c0-131">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="e20c0-132">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="e20c0-132">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="e20c0-133">Vyberte hello aplikaci tooconfigure jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e20c0-133">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="e20c0-134">Po načtení hello aplikace, klikněte na **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="e20c0-134">Once hello application loads, click **Single sign-on** from hello application’s left hand navigation menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e20c0-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e20c0-135">Next steps</span></span>
[<span data-ttu-id="e20c0-136">Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="e20c0-136">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

