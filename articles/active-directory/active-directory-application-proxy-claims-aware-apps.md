---
title: "aplikace využívající technologii aaaClaims - Proxy aplikace Azure AD | Microsoft Docs"
description: "Jak toopublish místní aplikace ASP.NET, které přijímat deklarace identity služby AD FS pro vaši uživatelé zabezpečený vzdálený přístup."
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
ms.openlocfilehash: 7be633225de700226c7c94815eb91b3de2b61cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a><span data-ttu-id="d011e-103">Práce s deklaracemi identity aplikace v Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="d011e-103">Working with claims-aware apps in Application Proxy</span></span>
<span data-ttu-id="d011e-104">[Deklaracemi identity aplikace](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) provádět přesměrování toohello tokenu služby zabezpečení (STS).</span><span class="sxs-lookup"><span data-stu-id="d011e-104">[Claims-aware apps](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) perform a redirection toohello Security Token Service (STS).</span></span> <span data-ttu-id="d011e-105">Služba tokenů zabezpečení Hello požadavky přihlašovací údaje od uživatele hello za token a pak přesměruje hello uživatelská toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d011e-105">hello STS requests credentials from hello user in exchange for a token and then redirects hello user toohello application.</span></span> <span data-ttu-id="d011e-106">Existuje několik způsobů tooenable Proxy aplikace toowork s tyto přesměrování.</span><span class="sxs-lookup"><span data-stu-id="d011e-106">There are a few ways tooenable Application Proxy toowork with these redirects.</span></span> <span data-ttu-id="d011e-107">Použijte tento článek tooconfigure nasazení pro deklaracemi identity aplikace.</span><span class="sxs-lookup"><span data-stu-id="d011e-107">Use this article tooconfigure your deployment for claims-aware apps.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="d011e-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d011e-108">Prerequisites</span></span>
<span data-ttu-id="d011e-109">Ujistěte se, že tento hello služby tokenů zabezpečení, která hello deklaracemi identity aplikace přesměruje toois dostupné v místní síti.</span><span class="sxs-lookup"><span data-stu-id="d011e-109">Make sure that hello STS that hello claims-aware app redirects toois available outside of your on-premises network.</span></span> <span data-ttu-id="d011e-110">Můžete zpřístupnit hello STS vystavení prostřednictvím proxy serveru nebo tím, že se mimo připojení.</span><span class="sxs-lookup"><span data-stu-id="d011e-110">You can make hello STS available by exposing it through a proxy or by allowing outside connections.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="d011e-111">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="d011e-111">Publish your application</span></span>

1. <span data-ttu-id="d011e-112">Publikování aplikace podle toohello pokynů v tématu [publikování aplikací pomocí Proxy aplikace](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d011e-112">Publish your application according toohello instructions described in [Publish applications with Application Proxy](application-proxy-publish-azure-portal.md).</span></span>
2. <span data-ttu-id="d011e-113">Přejděte toohello stránka aplikace v portálu a vyberte možnost hello **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d011e-113">Navigate toohello application page in hello portal and select **Single sign-on**.</span></span>
3. <span data-ttu-id="d011e-114">Pokud jste zvolili **Azure Active Directory** jako vaše **metoda předběžného ověření**, vyberte **Azure AD jednotné přihlašování zakázáno** jako vaše **interní metodu ověřování**.</span><span class="sxs-lookup"><span data-stu-id="d011e-114">If you chose **Azure Active Directory** as your **Preauthentication Method**, select **Azure AD single sign-on disabled** as your **Internal Authentication Method**.</span></span> <span data-ttu-id="d011e-115">Pokud jste zvolili **průchozí** jako vaše **metoda předběžného ověření**, nepotřebujete toochange cokoli.</span><span class="sxs-lookup"><span data-stu-id="d011e-115">If you chose **Passthrough** as your **Preauthentication Method**, you don't need toochange anything.</span></span>

## <a name="configure-adfs"></a><span data-ttu-id="d011e-116">Konfigurace služby AD FS</span><span class="sxs-lookup"><span data-stu-id="d011e-116">Configure ADFS</span></span>

<span data-ttu-id="d011e-117">Služba AD FS můžete nakonfigurovat pro deklaracemi identity aplikace v jednom ze dvou způsobů.</span><span class="sxs-lookup"><span data-stu-id="d011e-117">You can configure ADFS for claims-aware apps in one of two ways.</span></span> <span data-ttu-id="d011e-118">Hello je nejdřív pomocí vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="d011e-118">hello first is by using custom domains.</span></span> <span data-ttu-id="d011e-119">Hello druhou je s WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="d011e-119">hello second is with WS-Federation.</span></span> 

### <a name="option-1-custom-domains"></a><span data-ttu-id="d011e-120">Možnost 1: Vlastní domény</span><span class="sxs-lookup"><span data-stu-id="d011e-120">Option 1: Custom domains</span></span>

<span data-ttu-id="d011e-121">Pokud všechny hello interní adresy URL pro vaše aplikace jsou plně kvalifikované názvy domény (FQDN), pak můžete nakonfigurovat [vlastní domény](active-directory-application-proxy-custom-domains.md) pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="d011e-121">If all hello internal URLs for your applications are fully qualified domain names (FQDNs), then you can configure [custom domains](active-directory-application-proxy-custom-domains.md) for your applications.</span></span> <span data-ttu-id="d011e-122">Použití hello vlastní domény toocreate externí adresy URL, které jsou hello stejná hodnota jako hello interní adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d011e-122">Use hello custom domains toocreate external URLs that are hello same as hello internal URLs.</span></span> <span data-ttu-id="d011e-123">Při vašem externí adresy URL odpovídat vaše interní adresy URL, přesměrování služby tokenů zabezpečení hello fungovat, zda jsou vaši uživatelé místní nebo vzdálené.</span><span class="sxs-lookup"><span data-stu-id="d011e-123">When your external URLs match your internal URLs, then hello STS redirections work whether your users are on-premises or remote.</span></span> 

### <a name="option-2-ws-federation"></a><span data-ttu-id="d011e-124">Možnost 2: WS-Federation</span><span class="sxs-lookup"><span data-stu-id="d011e-124">Option 2: WS-Federation</span></span>

1. <span data-ttu-id="d011e-125">Otevřete modul Správa služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="d011e-125">Open ADFS Management.</span></span>
2. <span data-ttu-id="d011e-126">Přejděte příliš**vztahy důvěryhodnosti předávající strany**, klikněte pravým tlačítkem na hello aplikace publikujete pomocí Proxy aplikace a zvolte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="d011e-126">Go too**Relying Party Trusts**, right-click on hello app you are publishing with Application Proxy, and choose **Properties**.</span></span>  

   ![Vztahy důvěryhodnosti předávající strany, klikněte pravým tlačítkem na název aplikace – snímek obrazovky](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. <span data-ttu-id="d011e-128">Na hello **koncové body** v části **typ koncového bodu**, vyberte **WS-Federation**.</span><span class="sxs-lookup"><span data-stu-id="d011e-128">On hello **Endpoints** tab, under **Endpoint type**, select **WS-Federation**.</span></span>
4. <span data-ttu-id="d011e-129">V části **důvěryhodné adresy URL**, zadejte adresu URL hello jste zadali v hello Proxy aplikace pod **externí adresu URL** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d011e-129">Under **Trusted URL**, enter hello URL you entered in hello Application Proxy under **External URL** and click **OK**.</span></span>  

   ![Přidání koncového bodu - nastavit důvěryhodné adresy URL hodnotu – snímek obrazovky](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a><span data-ttu-id="d011e-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d011e-131">Next steps</span></span>
* <span data-ttu-id="d011e-132">[Povolit jednotné přihlašování na](application-proxy-sso-overview.md) pro aplikace, které nejsou pracujícím s deklaracemi</span><span class="sxs-lookup"><span data-stu-id="d011e-132">[Enable single-sign on](application-proxy-sso-overview.md) for applications that aren't claims-aware</span></span>
* [<span data-ttu-id="d011e-133">Povolení nativního klienta aplikace toointeract s proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="d011e-133">Enable native client apps toointeract with proxy applications</span></span>](active-directory-application-proxy-native-client.md)


