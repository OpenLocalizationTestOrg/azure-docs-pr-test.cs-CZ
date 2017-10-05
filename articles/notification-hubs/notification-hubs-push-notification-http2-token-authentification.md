---
title: "Ověřování na základě tokenu (HTTP/2) pro služby APN v Azure Notification Hubs | Microsoft Docs"
description: "Toto téma vysvětluje, jak můžete využít nové tokenu ověřování pro služby APN"
services: notification-hubs
documentationcenter: .net
author: kpiteira
manager: erikre
editor: 
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 05/17/2017
ms.author: kapiteir
ms.openlocfilehash: 5a21bcd9f12fc3f96b17a556ba15526c35ababe2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="token-based-http2-authentication-for-apns"></a><span data-ttu-id="02a9c-103">Ověřování na základě tokenu (HTTP/2) pro služby APN</span><span class="sxs-lookup"><span data-stu-id="02a9c-103">Token-based (HTTP/2) Authentication for APNS</span></span>
## <a name="overview"></a><span data-ttu-id="02a9c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="02a9c-104">Overview</span></span>
<span data-ttu-id="02a9c-105">Tento článek podrobně popisují pomocí tokenu ověřování založené na nový protokol APNS HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="02a9c-105">This article details how to use the new APNS HTTP/2 protocol with token based authentication.</span></span>

<span data-ttu-id="02a9c-106">Klíčové výhody použití nového protokolu patří:</span><span class="sxs-lookup"><span data-stu-id="02a9c-106">The key benefits of using the new protocol include:</span></span>
-   <span data-ttu-id="02a9c-107">Generování tokenů je poměrně starosti, (ve srovnání s certifikáty)</span><span class="sxs-lookup"><span data-stu-id="02a9c-107">Token generation is relatively hassle free (compared to certificates)</span></span>
-   <span data-ttu-id="02a9c-108">Žádná další data vypršení – jsou v ovládacím prvku ověřovací tokeny a jejich odvolání</span><span class="sxs-lookup"><span data-stu-id="02a9c-108">No more expiry dates – you are in control of your authentication tokens and their revocation</span></span>
-   <span data-ttu-id="02a9c-109">Datové části teď může být až 4 KB</span><span class="sxs-lookup"><span data-stu-id="02a9c-109">Payloads can now be up to 4 KB</span></span>
- <span data-ttu-id="02a9c-110">Synchronní zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="02a9c-110">Synchronous feedback</span></span>
-   <span data-ttu-id="02a9c-111">Jste nejnovější protokol společnosti Apple – certifikátů dál používat binární protokol, který je označen pro vyřazení</span><span class="sxs-lookup"><span data-stu-id="02a9c-111">You’re on Apple’s latest protocol – certificates still use the binary protocol, which is marked for deprecation</span></span>

<span data-ttu-id="02a9c-112">Pomocí tohoto nového mechanismu lze provést ve dvou krocích za pár minut:</span><span class="sxs-lookup"><span data-stu-id="02a9c-112">Using this new mechanism can be done in two steps in a few minutes:</span></span>
1.  <span data-ttu-id="02a9c-113">Získat informace potřebné z portálu Apple Developer účtu</span><span class="sxs-lookup"><span data-stu-id="02a9c-113">Obtain the necessary information from the Apple Developer Account portal</span></span>
2.  <span data-ttu-id="02a9c-114">Konfigurace centra oznámení s novými informacemi</span><span class="sxs-lookup"><span data-stu-id="02a9c-114">Configure your notification hub with the new information</span></span>

<span data-ttu-id="02a9c-115">Centra oznámení je nyní všechny nastaven na nový systém ověřování pomocí služby APN.</span><span class="sxs-lookup"><span data-stu-id="02a9c-115">Notification Hubs is now all set to use the new authentication system with APNS.</span></span> 

<span data-ttu-id="02a9c-116">Pamatujte, že pokud jste migrovali ze pomocí přihlašovacích údajů certifikát služby APN:</span><span class="sxs-lookup"><span data-stu-id="02a9c-116">Note that if you migrated from using certificate credentials for APNS:</span></span>
- <span data-ttu-id="02a9c-117">token vlastnosti přepsat svůj certifikát v našem systému</span><span class="sxs-lookup"><span data-stu-id="02a9c-117">the token properties overwrite your certificate in our system,</span></span>
- <span data-ttu-id="02a9c-118">ale aplikace i nadále bezproblémově přijímat oznámení.</span><span class="sxs-lookup"><span data-stu-id="02a9c-118">but your application continues to receive notifications seamlessly.</span></span>

## <a name="obtaining-authentication-information-from-apple"></a><span data-ttu-id="02a9c-119">Získávání informací o ověření od společnosti Apple</span><span class="sxs-lookup"><span data-stu-id="02a9c-119">Obtaining authentication information from Apple</span></span>
<span data-ttu-id="02a9c-120">Pokud chcete povolit ověřování na základě tokenu, potřebujete následující vlastnosti z vašeho účtu Apple Developer:</span><span class="sxs-lookup"><span data-stu-id="02a9c-120">To enable token-based authentication, you need the following properties from your Apple Developer Account:</span></span>
### <a name="key-identifier"></a><span data-ttu-id="02a9c-121">Identifikátor klíče</span><span class="sxs-lookup"><span data-stu-id="02a9c-121">Key Identifier</span></span>
<span data-ttu-id="02a9c-122">Identifikátor klíče můžete získat na stránce "Klíče" ve vašem účtu vývojáře Apple</span><span class="sxs-lookup"><span data-stu-id="02a9c-122">The key identifier can be obtained from the "Keys" page in your Apple Developer Account</span></span>

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a><span data-ttu-id="02a9c-123">Identifikátor aplikace & název aplikace</span><span class="sxs-lookup"><span data-stu-id="02a9c-123">Application Identifier & Application Name</span></span>
<span data-ttu-id="02a9c-124">Název aplikace je k dispozici prostřednictvím stránky vývojářský účet na ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="02a9c-124">The application name is available via the App IDs page in the Developer Account.</span></span> 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

<span data-ttu-id="02a9c-125">Identifikátor aplikace je k dispozici prostřednictvím stránce s podrobnostmi o členství v vývojářský účet.</span><span class="sxs-lookup"><span data-stu-id="02a9c-125">The application identifier is available via the membership details page in the Developer Account.</span></span>
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a><span data-ttu-id="02a9c-126">Ověřovací token</span><span class="sxs-lookup"><span data-stu-id="02a9c-126">Authentication token</span></span>
<span data-ttu-id="02a9c-127">Ověřovací token si můžete stáhnout po vygenerování tokenu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="02a9c-127">The authentication token can be downloaded after you generate a token for your application.</span></span> <span data-ttu-id="02a9c-128">Podrobnosti o tom, jak generovat tento token [dokumentaci pro vývojáře Apple](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).</span><span class="sxs-lookup"><span data-stu-id="02a9c-128">For details on how to generate this token, refer to [Apple’s Developer documentation](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).</span></span>

## <a name="configuring-your-notification-hub-to-use-token-based-authentication"></a><span data-ttu-id="02a9c-129">Konfigurace centra oznámení pro použití ověřování na základě tokenu</span><span class="sxs-lookup"><span data-stu-id="02a9c-129">Configuring your notification hub to use token-based authentication</span></span>
### <a name="configure-via-the-azure-portal"></a><span data-ttu-id="02a9c-130">Konfigurování prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="02a9c-130">Configure via the Azure portal</span></span>
<span data-ttu-id="02a9c-131">Pokud chcete povolit tokenu ověřování založené na portálu, přihlaste se k portálu Azure a přejděte do vašeho centra oznámení > oznámení služby > panely služby APN.</span><span class="sxs-lookup"><span data-stu-id="02a9c-131">To enable token based authentication in the portal, log in to the Azure portal and go to your Notification Hub > Notification Services > APNS panel.</span></span> 

<span data-ttu-id="02a9c-132">Existuje nový vlastnost – *režim ověřování*.</span><span class="sxs-lookup"><span data-stu-id="02a9c-132">There is a new property – *Authentication Mode*.</span></span> <span data-ttu-id="02a9c-133">Výběr Token umožňuje aktualizovat všechny relevantní tokenu vlastnosti vašeho centra.</span><span class="sxs-lookup"><span data-stu-id="02a9c-133">Selecting Token allows you to update your hub with all the relevant token properties.</span></span>

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- <span data-ttu-id="02a9c-134">Zadejte vlastnosti, které jste získali z vývojářského účtu Apple</span><span class="sxs-lookup"><span data-stu-id="02a9c-134">Enter the properties you retrieved from your Apple developer account,</span></span> 
- <span data-ttu-id="02a9c-135">Zvolte režim vaší aplikace (produkční nebo izolovaného prostoru)</span><span class="sxs-lookup"><span data-stu-id="02a9c-135">choose your application mode (Production or Sandbox)</span></span> 
- <span data-ttu-id="02a9c-136">Kliknutím na Uložit aktualizovat přihlašovací údaje APNS.</span><span class="sxs-lookup"><span data-stu-id="02a9c-136">click Save to update your APNS credentials.</span></span> 

### <a name="configure-via-management-api-rest"></a><span data-ttu-id="02a9c-137">Nakonfigurovat přes rozhraní API (REST) pro správu</span><span class="sxs-lookup"><span data-stu-id="02a9c-137">Configure via Management API (REST)</span></span>

<span data-ttu-id="02a9c-138">Můžete použít naše [rozhraní API pro správu](https://msdn.microsoft.com/library/azure/dn495827.aspx) k aktualizaci centra oznámení pro použití ověřování na základě tokenu.</span><span class="sxs-lookup"><span data-stu-id="02a9c-138">You can use our [management APIs](https://msdn.microsoft.com/library/azure/dn495827.aspx) to update your notification hub to use token-based authentication.</span></span>
<span data-ttu-id="02a9c-139">V závislosti na tom, zda je aplikace, které konfigurujete izolovaného prostoru nebo produkční aplikace (zadané v účtu vývojáře Apple) použijte jednu z odpovídající koncových bodů:</span><span class="sxs-lookup"><span data-stu-id="02a9c-139">Depending on whether the application you’re configuring is a Sandbox or Production app (specified in your Apple Developer Account), use one of the corresponding endpoints:</span></span>

- <span data-ttu-id="02a9c-140">Koncový bod izolovaného prostoru: [https://api.development.push.apple.com:443, 3 nebo zařízení](https://api.development.push.apple.com:443/3/device)</span><span class="sxs-lookup"><span data-stu-id="02a9c-140">Sandbox Endpoint: [https://api.development.push.apple.com:443/3/device](https://api.development.push.apple.com:443/3/device)</span></span>
- <span data-ttu-id="02a9c-141">Koncový bod produkční: [https://api.push.apple.com:443, 3 nebo zařízení](https://api.push.apple.com:443/3/device)</span><span class="sxs-lookup"><span data-stu-id="02a9c-141">Production Endpoint: [https://api.push.apple.com:443/3/device](https://api.push.apple.com:443/3/device)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="02a9c-142">Tokeny ověřování vyžaduje verzi rozhraní API z: **2017-04 nebo novější**.</span><span class="sxs-lookup"><span data-stu-id="02a9c-142">Token-based authentication requires an API version of: **2017-04 or later**.</span></span>
> 
> 

<span data-ttu-id="02a9c-143">Tady je příklad požadavek PUT aktualizovat rozbočovač s ověřováním na základě tokenu:</span><span class="sxs-lookup"><span data-stu-id="02a9c-143">Here’s an example of a PUT request to update a hub with token-based authentication:</span></span>


        PUT https://{namespace}.servicebus.windows.net/{Notification Hub}?api-version=2017-04
          "Properties": {
            "ApnsCredential": {
              "Properties": {
                "KeyId": "<Your Key Id>",
                "Token": "<Your Authentication Token>",
                "AppName": "<Your Application Name>",
                "AppId": "<Your Application Id>",
                "Endpoint":"<Sandbox/Production Endpoint>"
              }
            }
          }
        

### <a name="configure-via-the-net-sdk"></a><span data-ttu-id="02a9c-144">Konfigurovat pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="02a9c-144">Configure via the .NET SDK</span></span>
<span data-ttu-id="02a9c-145">Můžete nakonfigurovat vaše Centrum používat pomocí tokenu ověřování založené na našem [nejnovější verzi klienta SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).</span><span class="sxs-lookup"><span data-stu-id="02a9c-145">You can configure your hub to use token based authentication using our [latest client SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).</span></span> 

<span data-ttu-id="02a9c-146">Zde je ukázka kódu ilustrující správné použití:</span><span class="sxs-lookup"><span data-stu-id="02a9c-146">Here’s a code sample illustrating the correct usage:</span></span>


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH TO YOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-to-using-certificate-based-authentication"></a><span data-ttu-id="02a9c-147">Návrat k použití ověřování na základě certifikátu</span><span class="sxs-lookup"><span data-stu-id="02a9c-147">Reverting to using certificate-based authentication</span></span>
<span data-ttu-id="02a9c-148">Kdykoli můžete vrátit zpět k používání ověřování pomocí certifikátů pomocí jakékoli předchozí metody a předání certifikátu, místo token vlastností.</span><span class="sxs-lookup"><span data-stu-id="02a9c-148">You can revert at any time to using certificate-based authentication by using any preceding method and passing the certificate instead of the token properties.</span></span> <span data-ttu-id="02a9c-149">Tato akce přepíše dříve uložené přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="02a9c-149">That action overwrites the previously stored credentials.</span></span>
