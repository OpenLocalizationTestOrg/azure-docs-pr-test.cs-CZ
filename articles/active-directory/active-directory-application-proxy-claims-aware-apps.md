---
title: Deklaracemi identity aplikace - Proxy aplikace Azure AD | Microsoft Docs
description: "Jak publikovat místní aplikace ASP.NET, které přijímat deklarace identity služby AD FS pro vaši uživatelé zabezpečený vzdálený přístup."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 91e6211b-fe6a-42c6-bdb3-1fff0312db15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: 5784222608b01509fc4ff84b1a8792cbcfea89e6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a><span data-ttu-id="4cf2a-103">Práce s deklaracemi identity aplikace v Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="4cf2a-103">Working with claims-aware apps in Application Proxy</span></span>
<span data-ttu-id="4cf2a-104">[Deklaracemi identity aplikace](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) provést přesměrování na Security Token Service (STS).</span><span class="sxs-lookup"><span data-stu-id="4cf2a-104">[Claims-aware apps](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) perform a redirection to the Security Token Service (STS).</span></span> <span data-ttu-id="4cf2a-105">Služba tokenů zabezpečení požadavky přihlašovacích údajů od uživatele za token a pak přesměruje uživatele na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-105">The STS requests credentials from the user in exchange for a token and then redirects the user to the application.</span></span> <span data-ttu-id="4cf2a-106">Existuje několik způsobů, jak povolit Proxy aplikace pro práci s těmito přesměrování.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-106">There are a few ways to enable Application Proxy to work with these redirects.</span></span> <span data-ttu-id="4cf2a-107">Tento článek slouží ke konfiguraci nasazení pro deklaracemi identity aplikace.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-107">Use this article to configure your deployment for claims-aware apps.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="4cf2a-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4cf2a-108">Prerequisites</span></span>
<span data-ttu-id="4cf2a-109">Zkontrolujte, že služba tokenů zabezpečení, které aplikace deklaracemi přesměruje na je k dispozici mimo vaší místní síti.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-109">Make sure that the STS that the claims-aware app redirects to is available outside of your on-premises network.</span></span> <span data-ttu-id="4cf2a-110">Můžete zpřístupnit službu STS vystavení přes proxy server nebo tím, že se mimo připojení.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-110">You can make the STS available by exposing it through a proxy or by allowing outside connections.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="4cf2a-111">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="4cf2a-111">Publish your application</span></span>

1. <span data-ttu-id="4cf2a-112">Publikování aplikace podle pokynů v tématu [publikování aplikací pomocí Proxy aplikace](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4cf2a-112">Publish your application according to the instructions described in [Publish applications with Application Proxy](application-proxy-publish-azure-portal.md).</span></span>
2. <span data-ttu-id="4cf2a-113">Přejděte na stránku aplikace v portálu a vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-113">Navigate to the application page in the portal and select **Single sign-on**.</span></span>
3. <span data-ttu-id="4cf2a-114">Pokud jste zvolili **Azure Active Directory** jako vaše **metoda předběžného ověření**, vyberte **Azure AD jednotné přihlašování zakázáno** jako vaše **interní metodu ověřování**.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-114">If you chose **Azure Active Directory** as your **Preauthentication Method**, select **Azure AD single sign-on disabled** as your **Internal Authentication Method**.</span></span> <span data-ttu-id="4cf2a-115">Pokud jste zvolili **průchozí** jako vaše **metoda předběžného ověření**, nemusíte nic nezmění.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-115">If you chose **Passthrough** as your **Preauthentication Method**, you don't need to change anything.</span></span>

## <a name="configure-adfs"></a><span data-ttu-id="4cf2a-116">Konfigurace služby AD FS</span><span class="sxs-lookup"><span data-stu-id="4cf2a-116">Configure ADFS</span></span>

<span data-ttu-id="4cf2a-117">Služba AD FS můžete nakonfigurovat pro deklaracemi identity aplikace v jednom ze dvou způsobů.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-117">You can configure ADFS for claims-aware apps in one of two ways.</span></span> <span data-ttu-id="4cf2a-118">První je pomocí vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-118">The first is by using custom domains.</span></span> <span data-ttu-id="4cf2a-119">Druhá je s WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-119">The second is with WS-Federation.</span></span> 

### <a name="option-1-custom-domains"></a><span data-ttu-id="4cf2a-120">Možnost 1: Vlastní domény</span><span class="sxs-lookup"><span data-stu-id="4cf2a-120">Option 1: Custom domains</span></span>

<span data-ttu-id="4cf2a-121">Pokud všechny interní adresy URL pro vaše aplikace jsou plně kvalifikované názvy domény (FQDN), pak můžete nakonfigurovat [vlastní domény](active-directory-application-proxy-custom-domains.md) pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-121">If all the internal URLs for your applications are fully qualified domain names (FQDNs), then you can configure [custom domains](active-directory-application-proxy-custom-domains.md) for your applications.</span></span> <span data-ttu-id="4cf2a-122">Použijte vlastní domény k vytvoření externí adresy URL, které jsou stejné jako interní adresy URL.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-122">Use the custom domains to create external URLs that are the same as the internal URLs.</span></span> <span data-ttu-id="4cf2a-123">Pokud vaše externí adresy URL neodpovídají vaše interní adresy URL, přesměrování služby tokenů zabezpečení fungovat, zda jsou vaši uživatelé místní nebo vzdálené.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-123">When your external URLs match your internal URLs, then the STS redirections work whether your users are on-premises or remote.</span></span> 

### <a name="option-2-ws-federation"></a><span data-ttu-id="4cf2a-124">Možnost 2: WS-Federation</span><span class="sxs-lookup"><span data-stu-id="4cf2a-124">Option 2: WS-Federation</span></span>

1. <span data-ttu-id="4cf2a-125">Otevřete modul Správa služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-125">Open ADFS Management.</span></span>
2. <span data-ttu-id="4cf2a-126">Přejděte na **vztahy důvěryhodnosti předávající strany**, klikněte pravým tlačítkem na aplikaci, kterou publikujete pomocí Proxy aplikace a zvolte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-126">Go to **Relying Party Trusts**, right-click on the app you are publishing with Application Proxy, and choose **Properties**.</span></span>  

   ![Vztahy důvěryhodnosti předávající strany, klikněte pravým tlačítkem na název aplikace – snímek obrazovky](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. <span data-ttu-id="4cf2a-128">Na **koncové body** v části **typ koncového bodu**, vyberte **WS-Federation**.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-128">On the **Endpoints** tab, under **Endpoint type**, select **WS-Federation**.</span></span>
4. <span data-ttu-id="4cf2a-129">V části **důvěryhodné adresy URL**, zadejte adresu URL, které jste zadali v Proxy aplikace v rámci **externí adresu URL** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4cf2a-129">Under **Trusted URL**, enter the URL you entered in the Application Proxy under **External URL** and click **OK**.</span></span>  

   ![Přidání koncového bodu - nastavit důvěryhodné adresy URL hodnotu – snímek obrazovky](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a><span data-ttu-id="4cf2a-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4cf2a-131">Next steps</span></span>
* <span data-ttu-id="4cf2a-132">[Povolit jednotné přihlašování na](application-proxy-sso-overview.md) pro aplikace, které nejsou pracujícím s deklaracemi</span><span class="sxs-lookup"><span data-stu-id="4cf2a-132">[Enable single-sign on](application-proxy-sso-overview.md) for applications that aren't claims-aware</span></span>
* [<span data-ttu-id="4cf2a-133">Povolení nativního klienta pro interakci s proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="4cf2a-133">Enable native client apps to interact with proxy applications</span></span>](active-directory-application-proxy-native-client.md)


