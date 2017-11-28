---
title: "na základě aaaToken ověřování (HTTP/2) pro služby APN v Azure Notification Hubs | Microsoft Docs"
description: "Toto téma vysvětluje, jak tooleverage hello nový token ověřování pro služby APN"
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
ms.openlocfilehash: 3353d7f16033ce0b68edec9ee9aeb98f47faa1fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="token-based-http2-authentication-for-apns"></a><span data-ttu-id="a4ebc-103">Ověřování na základě tokenu (HTTP/2) pro služby APN</span><span class="sxs-lookup"><span data-stu-id="a4ebc-103">Token-based (HTTP/2) Authentication for APNS</span></span>
## <a name="overview"></a><span data-ttu-id="a4ebc-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="a4ebc-104">Overview</span></span>
<span data-ttu-id="a4ebc-105">Tento článek podrobně popisuje, jak ověřování na základě toouse hello nový APNS HTTP/2 protokol s tokenem.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-105">This article details how toouse hello new APNS HTTP/2 protocol with token based authentication.</span></span>

<span data-ttu-id="a4ebc-106">pomocí nového protokolu hello Hello klíčové výhody patří:</span><span class="sxs-lookup"><span data-stu-id="a4ebc-106">hello key benefits of using hello new protocol include:</span></span>
-   <span data-ttu-id="a4ebc-107">Generování tokenů je poměrně jednoduchý volné (porovnání toocertificates)</span><span class="sxs-lookup"><span data-stu-id="a4ebc-107">Token generation is relatively hassle free (compared toocertificates)</span></span>
-   <span data-ttu-id="a4ebc-108">Žádná další data vypršení – jsou v ovládacím prvku ověřovací tokeny a jejich odvolání</span><span class="sxs-lookup"><span data-stu-id="a4ebc-108">No more expiry dates – you are in control of your authentication tokens and their revocation</span></span>
-   <span data-ttu-id="a4ebc-109">Datové části teď může být až too4 KB</span><span class="sxs-lookup"><span data-stu-id="a4ebc-109">Payloads can now be up too4 KB</span></span>
- <span data-ttu-id="a4ebc-110">Synchronní zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="a4ebc-110">Synchronous feedback</span></span>
-   <span data-ttu-id="a4ebc-111">Jste nejnovější protokol společnosti Apple – certifikátů dál používat hello binární protokol, který je označen pro vyřazení</span><span class="sxs-lookup"><span data-stu-id="a4ebc-111">You’re on Apple’s latest protocol – certificates still use hello binary protocol, which is marked for deprecation</span></span>

<span data-ttu-id="a4ebc-112">Pomocí tohoto nového mechanismu lze provést ve dvou krocích za pár minut:</span><span class="sxs-lookup"><span data-stu-id="a4ebc-112">Using this new mechanism can be done in two steps in a few minutes:</span></span>
1.  <span data-ttu-id="a4ebc-113">Získat informace potřebné hello z portálu Apple Developer účtu hello</span><span class="sxs-lookup"><span data-stu-id="a4ebc-113">Obtain hello necessary information from hello Apple Developer Account portal</span></span>
2.  <span data-ttu-id="a4ebc-114">Konfigurace centra oznámení s informací o novém hello</span><span class="sxs-lookup"><span data-stu-id="a4ebc-114">Configure your notification hub with hello new information</span></span>

<span data-ttu-id="a4ebc-115">Centra oznámení je nyní všechny sady toouse hello nového ověřování systému pomocí služby APNS.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-115">Notification Hubs is now all set toouse hello new authentication system with APNS.</span></span> 

<span data-ttu-id="a4ebc-116">Pamatujte, že pokud jste migrovali ze pomocí přihlašovacích údajů certifikát služby APN:</span><span class="sxs-lookup"><span data-stu-id="a4ebc-116">Note that if you migrated from using certificate credentials for APNS:</span></span>
- <span data-ttu-id="a4ebc-117">token vlastnosti Hello přepsat svůj certifikát v našem systému</span><span class="sxs-lookup"><span data-stu-id="a4ebc-117">hello token properties overwrite your certificate in our system,</span></span>
- <span data-ttu-id="a4ebc-118">ale aplikace stále tooreceive oznámení bez problémů.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-118">but your application continues tooreceive notifications seamlessly.</span></span>

## <a name="obtaining-authentication-information-from-apple"></a><span data-ttu-id="a4ebc-119">Získávání informací o ověření od společnosti Apple</span><span class="sxs-lookup"><span data-stu-id="a4ebc-119">Obtaining authentication information from Apple</span></span>
<span data-ttu-id="a4ebc-120">ověřování na základě tokenu tooenable, je třeba hello následující vlastnosti z vašeho účtu Apple Developer:</span><span class="sxs-lookup"><span data-stu-id="a4ebc-120">tooenable token-based authentication, you need hello following properties from your Apple Developer Account:</span></span>
### <a name="key-identifier"></a><span data-ttu-id="a4ebc-121">Identifikátor klíče</span><span class="sxs-lookup"><span data-stu-id="a4ebc-121">Key Identifier</span></span>
<span data-ttu-id="a4ebc-122">Nelze získat identifikátor klíče Hello ze stránky "Klíče" hello ve vašem účtu vývojáře Apple</span><span class="sxs-lookup"><span data-stu-id="a4ebc-122">hello key identifier can be obtained from hello "Keys" page in your Apple Developer Account</span></span>

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a><span data-ttu-id="a4ebc-123">Identifikátor aplikace & název aplikace</span><span class="sxs-lookup"><span data-stu-id="a4ebc-123">Application Identifier & Application Name</span></span>
<span data-ttu-id="a4ebc-124">název aplikace Hello je k dispozici prostřednictvím stránku hello ID aplikace hello vývojářský účet.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-124">hello application name is available via hello App IDs page in hello Developer Account.</span></span> 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

<span data-ttu-id="a4ebc-125">identifikátor aplikace Hello je k dispozici prostřednictvím hello stránce s podrobnostmi o členství v hello vývojářský účet.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-125">hello application identifier is available via hello membership details page in hello Developer Account.</span></span>
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a><span data-ttu-id="a4ebc-126">Ověřovací token</span><span class="sxs-lookup"><span data-stu-id="a4ebc-126">Authentication token</span></span>
<span data-ttu-id="a4ebc-127">Hello ověřovací token si můžete stáhnout po vygenerování tokenu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-127">hello authentication token can be downloaded after you generate a token for your application.</span></span> <span data-ttu-id="a4ebc-128">Podrobnosti o tom, jak toogenerate tento token, naleznete v příliš[dokumentaci pro vývojáře Apple](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).</span><span class="sxs-lookup"><span data-stu-id="a4ebc-128">For details on how toogenerate this token, refer too[Apple’s Developer documentation](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).</span></span>

## <a name="configuring-your-notification-hub-toouse-token-based-authentication"></a><span data-ttu-id="a4ebc-129">Konfigurace oznámení centra toouse na základě tokenu ověřování</span><span class="sxs-lookup"><span data-stu-id="a4ebc-129">Configuring your notification hub toouse token-based authentication</span></span>
### <a name="configure-via-hello-azure-portal"></a><span data-ttu-id="a4ebc-130">Nakonfigurovat přes hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a4ebc-130">Configure via hello Azure portal</span></span>
<span data-ttu-id="a4ebc-131">tooenable token ověřování hello portálu, přihlášení toohello portál Azure a přejděte tooyour centra oznámení > oznámení služby > panely služby APN.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-131">tooenable token based authentication in hello portal, log in toohello Azure portal and go tooyour Notification Hub > Notification Services > APNS panel.</span></span> 

<span data-ttu-id="a4ebc-132">Existuje nový vlastnost – *režim ověřování*.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-132">There is a new property – *Authentication Mode*.</span></span> <span data-ttu-id="a4ebc-133">Výběr Token vám umožní tooupdate vašeho centra s všech hello relevantní token vlastností.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-133">Selecting Token allows you tooupdate your hub with all hello relevant token properties.</span></span>

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- <span data-ttu-id="a4ebc-134">Zadejte vlastnosti hello, který jste získali z účtu vývojáře Apple</span><span class="sxs-lookup"><span data-stu-id="a4ebc-134">Enter hello properties you retrieved from your Apple developer account,</span></span> 
- <span data-ttu-id="a4ebc-135">Zvolte režim vaší aplikace (produkční nebo izolovaného prostoru)</span><span class="sxs-lookup"><span data-stu-id="a4ebc-135">choose your application mode (Production or Sandbox)</span></span> 
- <span data-ttu-id="a4ebc-136">Klikněte na Uložit tooupdate vaše přihlašovací údaje APNS.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-136">click Save tooupdate your APNS credentials.</span></span> 

### <a name="configure-via-management-api-rest"></a><span data-ttu-id="a4ebc-137">Nakonfigurovat přes rozhraní API (REST) pro správu</span><span class="sxs-lookup"><span data-stu-id="a4ebc-137">Configure via Management API (REST)</span></span>

<span data-ttu-id="a4ebc-138">Můžete použít naše [rozhraní API pro správu](https://msdn.microsoft.com/library/azure/dn495827.aspx) tooupdate oznámení centra toouse na základě tokenu ověřování.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-138">You can use our [management APIs](https://msdn.microsoft.com/library/azure/dn495827.aspx) tooupdate your notification hub toouse token-based authentication.</span></span>
<span data-ttu-id="a4ebc-139">V závislosti na tom, jestli je hello aplikaci, kterou konfigurujete izolovaného prostoru nebo produkční aplikace (zadané v účtu vývojáře Apple) použijte jednu z hello odpovídající koncové body:</span><span class="sxs-lookup"><span data-stu-id="a4ebc-139">Depending on whether hello application you’re configuring is a Sandbox or Production app (specified in your Apple Developer Account), use one of hello corresponding endpoints:</span></span>

- <span data-ttu-id="a4ebc-140">Koncový bod izolovaného prostoru: [https://api.development.push.apple.com:443, 3 nebo zařízení](https://api.development.push.apple.com:443/3/device)</span><span class="sxs-lookup"><span data-stu-id="a4ebc-140">Sandbox Endpoint: [https://api.development.push.apple.com:443/3/device](https://api.development.push.apple.com:443/3/device)</span></span>
- <span data-ttu-id="a4ebc-141">Koncový bod produkční: [https://api.push.apple.com:443, 3 nebo zařízení](https://api.push.apple.com:443/3/device)</span><span class="sxs-lookup"><span data-stu-id="a4ebc-141">Production Endpoint: [https://api.push.apple.com:443/3/device](https://api.push.apple.com:443/3/device)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a4ebc-142">Tokeny ověřování vyžaduje verzi rozhraní API z: **2017-04 nebo novější**.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-142">Token-based authentication requires an API version of: **2017-04 or later**.</span></span>
> 
> 

<span data-ttu-id="a4ebc-143">Tady je příklad tooupdate požadavek PUT rozbočovač s ověřováním na základě tokenu:</span><span class="sxs-lookup"><span data-stu-id="a4ebc-143">Here’s an example of a PUT request tooupdate a hub with token-based authentication:</span></span>


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
        

### <a name="configure-via-hello-net-sdk"></a><span data-ttu-id="a4ebc-144">Nakonfigurovat přes hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="a4ebc-144">Configure via hello .NET SDK</span></span>
<span data-ttu-id="a4ebc-145">Můžete nakonfigurovat vaše centra toouse tokenu ověřování založené na našem [nejnovější verzi klienta SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).</span><span class="sxs-lookup"><span data-stu-id="a4ebc-145">You can configure your hub toouse token based authentication using our [latest client SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).</span></span> 

<span data-ttu-id="a4ebc-146">Zde je ukázka kódu ilustrující hello správné použití:</span><span class="sxs-lookup"><span data-stu-id="a4ebc-146">Here’s a code sample illustrating hello correct usage:</span></span>


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH tooYOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-toousing-certificate-based-authentication"></a><span data-ttu-id="a4ebc-147">Ověřování pomocí certifikátů navrácení toousing</span><span class="sxs-lookup"><span data-stu-id="a4ebc-147">Reverting toousing certificate-based authentication</span></span>
<span data-ttu-id="a4ebc-148">Při ověřování založené na certifikátech toousing čas můžete obnovit pomocí jakékoli předchozí hello certifikátu metoda a předávání místo hello token vlastností.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-148">You can revert at any time toousing certificate-based authentication by using any preceding method and passing hello certificate instead of hello token properties.</span></span> <span data-ttu-id="a4ebc-149">Aby akce přepíše hello dříve uložené přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="a4ebc-149">That action overwrites hello previously stored credentials.</span></span>
