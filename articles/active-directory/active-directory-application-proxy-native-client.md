---
title: "Publikování aplikací pro nativní klient – Azure AD | Microsoft Docs"
description: "Popisuje postup povolení aplikace nativního klienta ke komunikaci s konektor Proxy aplikace Azure AD poskytnout zabezpečený vzdálený přístup k místní aplikace."
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
ms.openlocfilehash: bdaa5af6ff5331bc310499586615b48a864c3c5e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a><span data-ttu-id="f4e56-103">Postup povolení nativního klienta aplikace pro interakci s proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="f4e56-103">How to enable native client apps to interact with proxy Applications</span></span>

<span data-ttu-id="f4e56-104">Kromě webovým aplikacím Proxy aplikace služby Active Directory Azure také slouží k publikování aplikací pro nativní klient systému.</span><span class="sxs-lookup"><span data-stu-id="f4e56-104">In addition to web applications, Azure Active Directory Application Proxy can also be used to publish native client apps.</span></span> <span data-ttu-id="f4e56-105">Nativní klientské aplikace se liší od webové aplikace, protože se instalují na zařízení, když webové aplikace jsou dostupné prostřednictvím prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f4e56-105">Native client apps differ from web apps because they are installed on a device, while web apps are accessed through a browser.</span></span> 

<span data-ttu-id="f4e56-106">Proxy aplikací podporuje nativní klient aplikace přijímá Azure AD, které jsou vystavené tokeny, které se odesílají v hlavičky standardních autorizovat HTTP.</span><span class="sxs-lookup"><span data-stu-id="f4e56-106">Application Proxy supports native client apps by accepting Azure AD issued tokens that are sent in standard Authorize HTTP headers.</span></span>

![Vztah mezi koncovým uživatelům, Azure Active Directory a publikování aplikace](./media/active-directory-application-proxy-native-client/richclientflow.png)

<span data-ttu-id="f4e56-108">Knihovnu pro ověřování Azure AD, který se stará o ověřování a podporuje mnoho klienta prostředí, použijte k publikování nativních aplikací.</span><span class="sxs-lookup"><span data-stu-id="f4e56-108">Use the Azure AD Authentication Library, which takes care of authentication and supports many client environments, to publish native applications.</span></span> <span data-ttu-id="f4e56-109">Proxy aplikací zapadá do [nativní aplikace za účelem scénáře webového rozhraní API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="f4e56-109">Application Proxy fits into the [Native Application to Web API scenario](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span></span> <span data-ttu-id="f4e56-110">Tento článek vás provede čtyři kroky k publikování nativní aplikace s Proxy aplikace a knihovna ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f4e56-110">This article walks you through the four steps to publish a native application with Application Proxy and the Azure AD Authentication Library.</span></span> 

## <a name="step-1-publish-your-application"></a><span data-ttu-id="f4e56-111">Krok 1: Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="f4e56-111">Step 1: Publish your application</span></span>
<span data-ttu-id="f4e56-112">Publikování aplikace proxy, stejně jako všechny ostatní aplikace a přiřadit uživatelům přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f4e56-112">Publish your proxy application as you would any other application and assign users to access your application.</span></span> <span data-ttu-id="f4e56-113">Další informace najdete v tématu [publikování aplikací pomocí Proxy aplikace](active-directory-application-proxy-publish.md).</span><span class="sxs-lookup"><span data-stu-id="f4e56-113">For more information, see [Publish applications with Application Proxy](active-directory-application-proxy-publish.md).</span></span>

## <a name="step-2-configure-your-application"></a><span data-ttu-id="f4e56-114">Krok 2: Konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="f4e56-114">Step 2: Configure your application</span></span>
<span data-ttu-id="f4e56-115">Nativní aplikace nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f4e56-115">Configure your native application as follows:</span></span>

1. <span data-ttu-id="f4e56-116">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f4e56-116">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f4e56-117">Přejděte na **Azure Active Directory** > **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f4e56-117">Navigate to **Azure Active Directory** > **App registrations**.</span></span>
3. <span data-ttu-id="f4e56-118">Vyberte **nové registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f4e56-118">Select **New application registration**.</span></span>
4. <span data-ttu-id="f4e56-119">Zadejte název pro vaši aplikaci, vyberte **nativní** jako typ aplikace a zadejte identifikátor URI přesměrování pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f4e56-119">Specify a name for your application, select **Native** as the application type, and provide the Redirect URI for your application.</span></span> 

   ![Vytvořit novou registraci aplikace](./media/active-directory-application-proxy-native-client/create.png)
5. <span data-ttu-id="f4e56-121">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f4e56-121">Select **Create**.</span></span>

<span data-ttu-id="f4e56-122">Podrobné informace o vytváření nové registrace aplikace, najdete v části [integrace aplikací s Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="f4e56-122">For more detailed information about creating a new app registration, see [Integrating applications with Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span></span>


## <a name="step-3-grant-access-to-other-applications"></a><span data-ttu-id="f4e56-123">Krok 3: Udělení přístupu k ostatním aplikacím</span><span class="sxs-lookup"><span data-stu-id="f4e56-123">Step 3: Grant access to other applications</span></span>
<span data-ttu-id="f4e56-124">Povolte nativní aplikace, které mají být exponovány k ostatním aplikacím ve vašem adresáři:</span><span class="sxs-lookup"><span data-stu-id="f4e56-124">Enable the native application to be exposed to other applications in your directory:</span></span>

1. <span data-ttu-id="f4e56-125">Pořád ještě v **registrace aplikace**, vyberte novou nativní aplikaci, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f4e56-125">Still in **App registrations**, select the new native application that you just created.</span></span>
2. <span data-ttu-id="f4e56-126">Vyberte **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="f4e56-126">Select **Required permissions**.</span></span>
3. <span data-ttu-id="f4e56-127">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f4e56-127">Select **Add**.</span></span>
4. <span data-ttu-id="f4e56-128">Prvním krokem, otevřete **vybrat rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="f4e56-128">Open the first step, **Select an API**.</span></span>
5. <span data-ttu-id="f4e56-129">Pomocí panelu vyhledávání můžete najít aplikaci Proxy aplikace, kterou jste publikovali v první části.</span><span class="sxs-lookup"><span data-stu-id="f4e56-129">Use the search bar to find the Application Proxy app that you published in the first section.</span></span> <span data-ttu-id="f4e56-130">Vyberte aplikaci a pak klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="f4e56-130">Choose that app, then click **Select**.</span></span> 

   ![Vyhledejte aplikaci, proxy](./media/active-directory-application-proxy-native-client/select_api.png)
6. <span data-ttu-id="f4e56-132">Otevřete druhý krok **vyberte oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="f4e56-132">Open the second step, **Select permissions**.</span></span>
7. <span data-ttu-id="f4e56-133">Zaškrtnutím políčka pomocí zajišťují vašim nativní aplikace přístup k aplikaci proxy a pak klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="f4e56-133">Use the checkbox to grant your native application access to your proxy application, then click **Select**.</span></span>

   ![Udělit přístup k proxy aplikace](./media/active-directory-application-proxy-native-client/select_perms.png)
8. <span data-ttu-id="f4e56-135">Vyberte **provádí**.</span><span class="sxs-lookup"><span data-stu-id="f4e56-135">Select **Done**.</span></span>


## <a name="step-4-edit-the-active-directory-authentication-library"></a><span data-ttu-id="f4e56-136">Krok 4: Úprava knihovna ověřování Active Directory</span><span class="sxs-lookup"><span data-stu-id="f4e56-136">Step 4: Edit the Active Directory Authentication Library</span></span>
<span data-ttu-id="f4e56-137">Upravte kód nativní aplikace v rámci ověřování služby Active Directory Authentication Library (ADAL) zahrnout následující text:</span><span class="sxs-lookup"><span data-stu-id="f4e56-137">Edit the native application code in the authentication context of the Active Directory Authentication Library (ADAL) to include the following text:</span></span>

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

<span data-ttu-id="f4e56-138">Proměnné v ukázkovém kódu by měla být nahrazená následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f4e56-138">The variables in the sample code should be replaced as follows:</span></span>

* <span data-ttu-id="f4e56-139">**ID klienta** naleznete na webu Azure portal.</span><span class="sxs-lookup"><span data-stu-id="f4e56-139">**Tenant ID** can be found in the Azure portal.</span></span> <span data-ttu-id="f4e56-140">Přejděte na **Azure Active Directory** > **vlastnosti** a zkopírujte ID adresáře.</span><span class="sxs-lookup"><span data-stu-id="f4e56-140">Navigate to **Azure Active Directory** > **Properties** and copy the Directory ID.</span></span> 
* <span data-ttu-id="f4e56-141">**Externí adresu URL** je front-end adresa URL jste zadali v aplikaci Proxy.</span><span class="sxs-lookup"><span data-stu-id="f4e56-141">**External URL** is the front-end URL you entered in the Proxy Application.</span></span> <span data-ttu-id="f4e56-142">Chcete-li tuto hodnotu najít, přejděte na **proxy aplikace** části proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4e56-142">To find this value, navigate to the **Application proxy** section of the proxy app.</span></span>
* <span data-ttu-id="f4e56-143">**ID aplikace** nativní aplikace naleznete na **vlastnosti** stránka nativní aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4e56-143">**App ID** of the native app can be found on the **Properties** page of the native application.</span></span>
* <span data-ttu-id="f4e56-144">**Identifikátor URI přesměrování nativní aplikace** naleznete na **identifikátory URI přesměrování** stránka nativní aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4e56-144">**Redirect URI of the native app** can be found on the **Redirect URIs** page of the native application.</span></span>


## <a name="see-also"></a><span data-ttu-id="f4e56-145">Viz také</span><span class="sxs-lookup"><span data-stu-id="f4e56-145">See also</span></span>

<span data-ttu-id="f4e56-146">Další informace o toku nativní aplikace najdete v tématu [nativní aplikace za účelem webového rozhraní API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span><span class="sxs-lookup"><span data-stu-id="f4e56-146">For more information about the native application flow, see [Native application to web API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span></span>

<span data-ttu-id="f4e56-147">Nejnovější novinky a aktualizace naleznete na [blogu proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)</span><span class="sxs-lookup"><span data-stu-id="f4e56-147">For the latest news and updates, check out the [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span></span>
