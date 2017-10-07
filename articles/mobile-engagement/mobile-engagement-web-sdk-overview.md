---
title: "aaaAzure Přehled sady SDK webové Mobile Engagement | Microsoft Docs"
description: "Hello nejnovější aktualizace a postupy pro hello Web SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 5bbc2fda-0f3f-43d0-a73d-0f2c0f8dc25b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 10/18/2016
ms.author: piyushjo
ms.openlocfilehash: 9e60a232b5eb2c41c405041a88e09d7137563513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-web-sdk"></a><span data-ttu-id="4b738-103">Web Azure Mobile Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="4b738-103">Azure Mobile Engagement Web SDK</span></span>
<span data-ttu-id="4b738-104">Začněte zde pro všechny hello podrobnosti o způsobu toointegrate Azure Mobile Engagement ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b738-104">Start here for all hello details about how toointegrate Azure Mobile Engagement in a web app.</span></span> <span data-ttu-id="4b738-105">Pokud chcete toogive ji zkuste před Začínáme s vlastní webové aplikace, najdete v našich [15 minut kurzu](mobile-engagement-web-app-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4b738-105">If you'd like toogive it a try before getting started with your own web app, see our [15-minute tutorial](mobile-engagement-web-app-get-started.md).</span></span>

## <a name="integration-procedures"></a><span data-ttu-id="4b738-106">Integrace procedury</span><span class="sxs-lookup"><span data-stu-id="4b738-106">Integration procedures</span></span>
1. <span data-ttu-id="4b738-107">Další informace [jak toointegrate Mobile Engagement ve vaší webové aplikaci](mobile-engagement-web-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="4b738-107">Learn [how toointegrate Mobile Engagement in your web app](mobile-engagement-web-integrate-engagement.md).</span></span>
2. <span data-ttu-id="4b738-108">Implementace plánu značky, další [jak toouse hello advanced označování rozhraní API ve vaší webové aplikaci Mobile Engagement](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="4b738-108">For tag plan implementation, learn [how toouse hello advanced Mobile Engagement tagging API in your web app](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="release-notes"></a><span data-ttu-id="4b738-109">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="4b738-109">Release notes</span></span>
### <a name="202-10182016"></a><span data-ttu-id="4b738-110">2.0.2 (10/18/2016)</span><span class="sxs-lookup"><span data-stu-id="4b738-110">2.0.2 (10/18/2016)</span></span>
* <span data-ttu-id="4b738-111">Opravené havárií na privátní procházení (Safari).</span><span class="sxs-lookup"><span data-stu-id="4b738-111">Fixed crash on private browsing (Safari).</span></span>
* <span data-ttu-id="4b738-112">Opravené havárie v prohlížečích se soubory cookie zakázána.</span><span class="sxs-lookup"><span data-stu-id="4b738-112">Fixed crash on browsers with cookies disabled.</span></span>

<span data-ttu-id="4b738-113">Všechny verze, najdete v tématu hello [dokončení poznámky k verzi](mobile-engagement-web-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="4b738-113">For all versions, please see hello [complete release notes](mobile-engagement-web-release-notes.md).</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="4b738-114">Postupy upgradu</span><span class="sxs-lookup"><span data-stu-id="4b738-114">Upgrade procedures</span></span>
### <a name="upgrade-from-121-too200"></a><span data-ttu-id="4b738-115">Upgrade z 1.2.1 too2.0.0</span><span class="sxs-lookup"><span data-stu-id="4b738-115">Upgrade from 1.2.1 too2.0.0</span></span>
<span data-ttu-id="4b738-116">Hello následující části popisují, jak toomigrate integrace sady Mobile Engagement Web SDK ze služby Capptain hello, které nabízí Capptain SAS, tooan aplikace Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="4b738-116">hello following sections describe how toomigrate a Mobile Engagement Web SDK integration from hello Capptain service, offered by Capptain SAS, tooan Azure Mobile Engagement app.</span></span> <span data-ttu-id="4b738-117">Pokud provádíte migraci ze starší verze než 1.2.1, informujte hello Capptain webu toomigrate too1.2.1 nejprve a potom použijte hello následující postupy.</span><span class="sxs-lookup"><span data-stu-id="4b738-117">If you are migrating from a version earlier than 1.2.1, please consult hello Capptain website toomigrate too1.2.1 first, and then apply hello following procedures.</span></span>

<span data-ttu-id="4b738-118">Tuto verzi hello Mobile Engagement Web SDK nepodporuje Samsung inteligentní TV, Opera TV, webOS nebo funkci Reach hello.</span><span class="sxs-lookup"><span data-stu-id="4b738-118">This version of hello Mobile Engagement Web SDK doesn't support Samsung Smart TV, Opera TV, webOS, or hello Reach feature.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4b738-119">Capptain a Azure Mobile Engagement nejsou hello stejnou službu a hello následující postupy zvýrazněte pouze jak toomigrate hello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b738-119">Capptain and Azure Mobile Engagement are not hello same service, and hello following procedures highlight only how toomigrate hello client app.</span></span> <span data-ttu-id="4b738-120">Migrace hello sady SDK pro aplikaci Mobile Engagement webové aplikace hello nebude migrovat data z Capptain serveru tooa Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="4b738-120">Migrating hello Mobile Engagement Web SDK in hello app will not migrate your data from a Capptain server tooa Mobile Engagement server.</span></span>
> 
> 

#### <a name="javascript-files"></a><span data-ttu-id="4b738-121">Soubory jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="4b738-121">JavaScript files</span></span>
<span data-ttu-id="4b738-122">Nahraďte soubor hello capptain-sdk.js hello souboru azure-engagement.js a pak aktualizaci vašeho skriptu importuje odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="4b738-122">Replace hello file capptain-sdk.js with hello file azure-engagement.js, and then update your script imports accordingly.</span></span>

#### <a name="remove-capptain-reach"></a><span data-ttu-id="4b738-123">Odebrat Capptain Reach</span><span class="sxs-lookup"><span data-stu-id="4b738-123">Remove Capptain Reach</span></span>
<span data-ttu-id="4b738-124">Tato verze hello Mobile Engagement Web SDK nepodporuje funkci Reach hello.</span><span class="sxs-lookup"><span data-stu-id="4b738-124">This version of hello Mobile Engagement Web SDK doesn't support hello Reach feature.</span></span> <span data-ttu-id="4b738-125">Pokud mají integrované Capptain Reach do své aplikace, je třeba tooremove ho.</span><span class="sxs-lookup"><span data-stu-id="4b738-125">If you have integrated Capptain Reach into your application, you need tooremove it.</span></span>

<span data-ttu-id="4b738-126">Odebrání stránku hello import dosáhnout šablon stylů CSS a odstranit soubor .css související hello (capptain-reach.css, ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="4b738-126">Remove hello Reach CSS import from your page and delete hello related .css file (capptain-reach.css, by default).</span></span>

<span data-ttu-id="4b738-127">Odstranit následující prostředky Reach hello: hello zavřít bitové kopie (capptain-close.png, ve výchozím nastavení) a ikonu značky hello (capptain ikonu oznámení, ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="4b738-127">Delete hello following Reach resources: hello close image (capptain-close.png, by default) and hello brand icon (capptain-notification-icon, by default).</span></span>

<span data-ttu-id="4b738-128">Odeberte hello dosáhnout uživatelského rozhraní pro oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b738-128">Remove hello Reach UI for in-app notifications.</span></span> <span data-ttu-id="4b738-129">výchozí rozložení Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4b738-129">hello default layout looks like this:</span></span>

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

<span data-ttu-id="4b738-130">Odeberte hello dosáhnout uživatelského rozhraní pro text a webovou oznámení a hlasování.</span><span class="sxs-lookup"><span data-stu-id="4b738-130">Remove hello Reach UI for text and web announcements and polls.</span></span> <span data-ttu-id="4b738-131">výchozí rozložení Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4b738-131">hello default layout looks like this:</span></span>

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

<span data-ttu-id="4b738-132">Odebrat hello `reach` objektu z vaší konfigurace, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="4b738-132">Remove hello `reach` object from your configuration, if it exists.</span></span> <span data-ttu-id="4b738-133">Vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4b738-133">It looks like this:</span></span>

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

<span data-ttu-id="4b738-134">Odeberte jakékoli jiné Reach přizpůsobení, jako např.</span><span class="sxs-lookup"><span data-stu-id="4b738-134">Remove any other Reach customization, such as categories.</span></span>

#### <a name="remove-deprecated-apis"></a><span data-ttu-id="4b738-135">Odeberte nepoužívané rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4b738-135">Remove deprecated APIs</span></span>
<span data-ttu-id="4b738-136">Některé rozhraní API z Capptain jsou zastaralé v hello SDK webové služby Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="4b738-136">Some APIs from Capptain are deprecated in hello Mobile Engagement Web SDK.</span></span>

<span data-ttu-id="4b738-137">Odeberte všechny volání toohello následující rozhraní API: `agent.connect`, `agent.disconnect`, `agent.pause`, a `agent.sendMessageToDevice`.</span><span class="sxs-lookup"><span data-stu-id="4b738-137">Remove any calls toohello following APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, and `agent.sendMessageToDevice`.</span></span>

<span data-ttu-id="4b738-138">Odeberte některé z následujících zpětná volání z konfiguraci Capptain hello: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, a `onPushMessageReceived`.</span><span class="sxs-lookup"><span data-stu-id="4b738-138">Remove any of hello following callbacks from your Capptain configuration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, and `onPushMessageReceived`.</span></span>

#### <a name="configuration"></a><span data-ttu-id="4b738-139">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="4b738-139">Configuration</span></span>
<span data-ttu-id="4b738-140">Mobile Engagement používá připojovací řetězec tooconfigure SDK identifikátory, například identifikátor aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4b738-140">Mobile Engagement uses a connection string tooconfigure SDK identifiers, for example, hello application identifier.</span></span>

<span data-ttu-id="4b738-141">ID aplikace hello nahraďte připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="4b738-141">Replace hello application ID with your connection string.</span></span> <span data-ttu-id="4b738-142">Poznámka: Tento objekt globální hello hello konfigurace sady SDK změny z `capptain` příliš`azureEngagement`.</span><span class="sxs-lookup"><span data-stu-id="4b738-142">Note that hello global object for hello SDK configuration changes from `capptain` too`azureEngagement`.</span></span>

<span data-ttu-id="4b738-143">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="4b738-143">Before migration:</span></span>

    window.capptain = {
      appId: ...,
      [...]
    };

<span data-ttu-id="4b738-144">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="4b738-144">After migration:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

<span data-ttu-id="4b738-145">Hello připojovací řetězec pro vaši aplikaci se zobrazí v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4b738-145">hello connection string for your application is displayed in hello Azure portal.</span></span>

#### <a name="javascript-apis"></a><span data-ttu-id="4b738-146">Rozhraní API jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="4b738-146">JavaScript APIs</span></span>
<span data-ttu-id="4b738-147">globální objekt jazyka JavaScript Hello `window.capptain` byla přejmenována `window.azureEngagement`, ale můžete použít hello `window.engagement` alias pro volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4b738-147">hello global JavaScript object `window.capptain` has been renamed `window.azureEngagement`, but you can use hello `window.engagement` alias for API calls.</span></span> <span data-ttu-id="4b738-148">Konfigurace sady SDK hello toodefine tento alias nelze použít.</span><span class="sxs-lookup"><span data-stu-id="4b738-148">You can't use this alias toodefine hello SDK configuration.</span></span>

<span data-ttu-id="4b738-149">Například `capptain.deviceId` stane `engagement.deviceId`, `capptain.agent.startActivity` stane `engagement.agent.startActivity`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="4b738-149">For instance, `capptain.deviceId` becomes `engagement.deviceId`, `capptain.agent.startActivity` becomes `engagement.agent.startActivity`, and so on.</span></span>

<span data-ttu-id="4b738-150">Pokud jste již spojili dřívější verzi hello Azure Mobile Engagement Web SDK do své aplikace, přečtěte si o [upgrade postupy](mobile-engagement-web-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="4b738-150">If you have already integrated an earlier version of hello Azure Mobile Engagement Web SDK into your application, please read about [upgrade procedures](mobile-engagement-web-upgrade-procedure.md).</span></span>

