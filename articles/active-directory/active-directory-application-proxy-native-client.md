---
title: "nativní klientské aplikace aaaPublish – Azure AD | Microsoft Docs"
description: "Popisuje, jak tooenable nativního klienta aplikace toocommunicate s konektoru Proxy aplikace služby Azure AD tooprovide zabezpečený vzdálený přístup tooyour místní aplikace."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f0cae145-e346-4126-948f-3f699747b96e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 0ed2be217bf992f034d8321d5e66569b4cace24f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-native-client-apps-toointeract-with-proxy-applications"></a><span data-ttu-id="55ce6-103">Jak tooenable nativního klienta aplikace toointeract s proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="55ce6-103">How tooenable native client apps toointeract with proxy Applications</span></span>

<span data-ttu-id="55ce6-104">V aplikacích tooweb přidání Proxy Azure Active Directory aplikace lze také použít toopublish nativní klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="55ce6-104">In addition tooweb applications, Azure Active Directory Application Proxy can also be used toopublish native client apps.</span></span> <span data-ttu-id="55ce6-105">Nativní klientské aplikace se liší od webové aplikace, protože se instalují na zařízení, když webové aplikace jsou dostupné prostřednictvím prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="55ce6-105">Native client apps differ from web apps because they are installed on a device, while web apps are accessed through a browser.</span></span> 

<span data-ttu-id="55ce6-106">Proxy aplikací podporuje nativní klient aplikace přijímá Azure AD, které jsou vystavené tokeny, které se odesílají v hlavičky standardních autorizovat HTTP.</span><span class="sxs-lookup"><span data-stu-id="55ce6-106">Application Proxy supports native client apps by accepting Azure AD issued tokens that are sent in standard Authorize HTTP headers.</span></span>

![Vztah mezi koncovým uživatelům, Azure Active Directory a publikování aplikace](./media/active-directory-application-proxy-native-client/richclientflow.png)

<span data-ttu-id="55ce6-108">Použijte hello knihovna ověřování Azure AD, který se stará o ověřování a podporuje mnoho klienta prostředí, toopublish nativních aplikací.</span><span class="sxs-lookup"><span data-stu-id="55ce6-108">Use hello Azure AD Authentication Library, which takes care of authentication and supports many client environments, toopublish native applications.</span></span> <span data-ttu-id="55ce6-109">Proxy aplikací zapadá do hello [nativní aplikace tooWeb rozhraní API scénář](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="55ce6-109">Application Proxy fits into hello [Native Application tooWeb API scenario](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span></span> <span data-ttu-id="55ce6-110">Tento článek vás provede hello čtyři kroky toopublish nativní aplikace s Proxy aplikace a hello knihovna ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="55ce6-110">This article walks you through hello four steps toopublish a native application with Application Proxy and hello Azure AD Authentication Library.</span></span> 

## <a name="step-1-publish-your-application"></a><span data-ttu-id="55ce6-111">Krok 1: Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="55ce6-111">Step 1: Publish your application</span></span>
<span data-ttu-id="55ce6-112">Publikování aplikace proxy, stejně jako všechny ostatní aplikace a přiřaďte tooaccess uživatelů vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="55ce6-112">Publish your proxy application as you would any other application and assign users tooaccess your application.</span></span> <span data-ttu-id="55ce6-113">Další informace najdete v tématu [publikování aplikací pomocí Proxy aplikace](active-directory-application-proxy-publish.md).</span><span class="sxs-lookup"><span data-stu-id="55ce6-113">For more information, see [Publish applications with Application Proxy](active-directory-application-proxy-publish.md).</span></span>

## <a name="step-2-configure-your-application"></a><span data-ttu-id="55ce6-114">Krok 2: Konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="55ce6-114">Step 2: Configure your application</span></span>
<span data-ttu-id="55ce6-115">Nativní aplikace nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="55ce6-115">Configure your native application as follows:</span></span>

1. <span data-ttu-id="55ce6-116">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="55ce6-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="55ce6-117">Přejděte příliš**Azure Active Directory** > **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="55ce6-117">Navigate too**Azure Active Directory** > **App registrations**.</span></span>
3. <span data-ttu-id="55ce6-118">Vyberte **nové registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="55ce6-118">Select **New application registration**.</span></span>
4. <span data-ttu-id="55ce6-119">Zadejte název pro vaši aplikaci, vyberte **nativní** jako typ aplikace hello a zadejte hello identifikátor URI pro přesměrování pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="55ce6-119">Specify a name for your application, select **Native** as hello application type, and provide hello Redirect URI for your application.</span></span> 

   ![Vytvořit novou registraci aplikace](./media/active-directory-application-proxy-native-client/create.png)
5. <span data-ttu-id="55ce6-121">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="55ce6-121">Select **Create**.</span></span>

<span data-ttu-id="55ce6-122">Podrobné informace o vytváření nové registrace aplikace, najdete v části [integrace aplikací s Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="55ce6-122">For more detailed information about creating a new app registration, see [Integrating applications with Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span></span>


## <a name="step-3-grant-access-tooother-applications"></a><span data-ttu-id="55ce6-123">Krok 3: Udělení přístupu tooother aplikace</span><span class="sxs-lookup"><span data-stu-id="55ce6-123">Step 3: Grant access tooother applications</span></span>
<span data-ttu-id="55ce6-124">Povolte hello nativní aplikace toobe zveřejněné tooother aplikace ve vašem adresáři:</span><span class="sxs-lookup"><span data-stu-id="55ce6-124">Enable hello native application toobe exposed tooother applications in your directory:</span></span>

1. <span data-ttu-id="55ce6-125">Pořád ještě v **registrace aplikace**, vyberte hello nové nativní aplikaci, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="55ce6-125">Still in **App registrations**, select hello new native application that you just created.</span></span>
2. <span data-ttu-id="55ce6-126">Vyberte **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="55ce6-126">Select **Required permissions**.</span></span>
3. <span data-ttu-id="55ce6-127">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="55ce6-127">Select **Add**.</span></span>
4. <span data-ttu-id="55ce6-128">Prvním krokem otevřete hello, **vybrat rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="55ce6-128">Open hello first step, **Select an API**.</span></span>
5. <span data-ttu-id="55ce6-129">Pomocí hello vyhledávání panelu toofind hello aplikace Proxy aplikace, která jste publikovali v první části hello.</span><span class="sxs-lookup"><span data-stu-id="55ce6-129">Use hello search bar toofind hello Application Proxy app that you published in hello first section.</span></span> <span data-ttu-id="55ce6-130">Vyberte aplikaci a pak klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="55ce6-130">Choose that app, then click **Select**.</span></span> 

   ![Vyhledejte hello proxy aplikace](./media/active-directory-application-proxy-native-client/select_api.png)
6. <span data-ttu-id="55ce6-132">Otevřete hello druhý krok, **vyberte oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="55ce6-132">Open hello second step, **Select permissions**.</span></span>
7. <span data-ttu-id="55ce6-133">Použijte nativní aplikace přístup tooyour proxy aplikace hello toogrant zaškrtávací políčko a potom klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="55ce6-133">Use hello checkbox toogrant your native application access tooyour proxy application, then click **Select**.</span></span>

   ![Udělení přístupu tooproxy aplikace](./media/active-directory-application-proxy-native-client/select_perms.png)
8. <span data-ttu-id="55ce6-135">Vyberte **provádí**.</span><span class="sxs-lookup"><span data-stu-id="55ce6-135">Select **Done**.</span></span>


## <a name="step-4-edit-hello-active-directory-authentication-library"></a><span data-ttu-id="55ce6-136">Krok 4: Úprava hello knihovna ověřování Active Directory</span><span class="sxs-lookup"><span data-stu-id="55ce6-136">Step 4: Edit hello Active Directory Authentication Library</span></span>
<span data-ttu-id="55ce6-137">Upravte kód nativní aplikace hello v kontextu ověřování hello hello Active Directory Authentication Library (ADAL) tooinclude hello následující text:</span><span class="sxs-lookup"><span data-stu-id="55ce6-137">Edit hello native application code in hello authentication context of hello Active Directory Authentication Library (ADAL) tooinclude hello following text:</span></span>

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of hello Native app>",
        new Uri("<Redirect Uri of hello Native App>"),
        PromptBehavior.Never);

//Use hello Access Token tooaccess hello Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

<span data-ttu-id="55ce6-138">Hello proměnné v hello ukázkový kód by měla být nahrazená následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="55ce6-138">hello variables in hello sample code should be replaced as follows:</span></span>

* <span data-ttu-id="55ce6-139">**ID klienta** lze nalézt v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="55ce6-139">**Tenant ID** can be found in hello Azure portal.</span></span> <span data-ttu-id="55ce6-140">Přejděte příliš**Azure Active Directory** > **vlastnosti** a kopírování hello ID adresáře.</span><span class="sxs-lookup"><span data-stu-id="55ce6-140">Navigate too**Azure Active Directory** > **Properties** and copy hello Directory ID.</span></span> 
* <span data-ttu-id="55ce6-141">**Externí adresu URL** hello front-end adresa URL, jste zadali v hello Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="55ce6-141">**External URL** is hello front-end URL you entered in hello Proxy Application.</span></span> <span data-ttu-id="55ce6-142">toofind, tato hodnota, přejděte toohello **proxy aplikace** části hello proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="55ce6-142">toofind this value, navigate toohello **Application proxy** section of hello proxy app.</span></span>
* <span data-ttu-id="55ce6-143">**ID aplikace** z hello nativní aplikace naleznete na hello **vlastnosti** stránku hello nativní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="55ce6-143">**App ID** of hello native app can be found on hello **Properties** page of hello native application.</span></span>
* <span data-ttu-id="55ce6-144">**Identifikátor URI přesměrování nativní aplikace hello** naleznete na hello **identifikátory URI přesměrování** stránku hello nativní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="55ce6-144">**Redirect URI of hello native app** can be found on hello **Redirect URIs** page of hello native application.</span></span>


## <a name="see-also"></a><span data-ttu-id="55ce6-145">Viz také</span><span class="sxs-lookup"><span data-stu-id="55ce6-145">See also</span></span>

<span data-ttu-id="55ce6-146">Další informace o toku nativní aplikace hello najdete v tématu [tooweb nativní aplikace API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span><span class="sxs-lookup"><span data-stu-id="55ce6-146">For more information about hello native application flow, see [Native application tooweb API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span></span>

<span data-ttu-id="55ce6-147">Hello nejnovější novinky a aktualizace, najdete na naší hello [blogu Proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)</span><span class="sxs-lookup"><span data-stu-id="55ce6-147">For hello latest news and updates, check out hello [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span></span>
