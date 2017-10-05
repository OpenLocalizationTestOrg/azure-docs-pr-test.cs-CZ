---
title: "Přehled sady SDK webové Azure Mobile Engagement | Microsoft Docs"
description: "Nejnovější aktualizace a postupy pro Web SDK pro Azure Mobile Engagement"
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
ms.openlocfilehash: 770a83131a3e661771db50b22ce7de25b2d541cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement-web-sdk"></a><span data-ttu-id="af332-103">Web Azure Mobile Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="af332-103">Azure Mobile Engagement Web SDK</span></span>
<span data-ttu-id="af332-104">Podrobné informace o tom, jak integrovat Azure Mobile Engagement ve webové aplikaci, začněte zde.</span><span class="sxs-lookup"><span data-stu-id="af332-104">Start here for all the details about how to integrate Azure Mobile Engagement in a web app.</span></span> <span data-ttu-id="af332-105">Pokud chcete a vyzkoušejte ho před Začínáme s webovou aplikaci najdete v tématu naše [15 minut kurzu](mobile-engagement-web-app-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="af332-105">If you'd like to give it a try before getting started with your own web app, see our [15-minute tutorial](mobile-engagement-web-app-get-started.md).</span></span>

## <a name="integration-procedures"></a><span data-ttu-id="af332-106">Integrace procedury</span><span class="sxs-lookup"><span data-stu-id="af332-106">Integration procedures</span></span>
1. <span data-ttu-id="af332-107">Další informace [jak integrovat Mobile Engagement ve vaší webové aplikaci](mobile-engagement-web-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="af332-107">Learn [how to integrate Mobile Engagement in your web app](mobile-engagement-web-integrate-engagement.md).</span></span>
2. <span data-ttu-id="af332-108">Implementace plánu značky, další [jak používat rozšířené Mobile Engagement označování rozhraní API ve vaší webové aplikaci](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="af332-108">For tag plan implementation, learn [how to use the advanced Mobile Engagement tagging API in your web app](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="release-notes"></a><span data-ttu-id="af332-109">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="af332-109">Release notes</span></span>
### <a name="202-10182016"></a><span data-ttu-id="af332-110">2.0.2 (10/18/2016)</span><span class="sxs-lookup"><span data-stu-id="af332-110">2.0.2 (10/18/2016)</span></span>
* <span data-ttu-id="af332-111">Opravené havárií na privátní procházení (Safari).</span><span class="sxs-lookup"><span data-stu-id="af332-111">Fixed crash on private browsing (Safari).</span></span>
* <span data-ttu-id="af332-112">Opravené havárie v prohlížečích se soubory cookie zakázána.</span><span class="sxs-lookup"><span data-stu-id="af332-112">Fixed crash on browsers with cookies disabled.</span></span>

<span data-ttu-id="af332-113">Všechny verze, najdete v tématu [dokončení poznámky k verzi](mobile-engagement-web-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="af332-113">For all versions, please see the [complete release notes](mobile-engagement-web-release-notes.md).</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="af332-114">Postupy upgradu</span><span class="sxs-lookup"><span data-stu-id="af332-114">Upgrade procedures</span></span>
### <a name="upgrade-from-121-to-200"></a><span data-ttu-id="af332-115">Upgrade z 1.2.1 na 2.0.0</span><span class="sxs-lookup"><span data-stu-id="af332-115">Upgrade from 1.2.1 to 2.0.0</span></span>
<span data-ttu-id="af332-116">Následující části popisují, jak migrovat integrace sady Mobile Engagement webového SDK z Capptain služby, které nabízí Capptain SAS, do aplikace Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="af332-116">The following sections describe how to migrate a Mobile Engagement Web SDK integration from the Capptain service, offered by Capptain SAS, to an Azure Mobile Engagement app.</span></span> <span data-ttu-id="af332-117">Pokud provádíte migraci z verze starší než 1.2.1, přečtěte si nejdřív přenést 1.2.1 Capptain web a potom použijte následující postupy.</span><span class="sxs-lookup"><span data-stu-id="af332-117">If you are migrating from a version earlier than 1.2.1, please consult the Capptain website to migrate to 1.2.1 first, and then apply the following procedures.</span></span>

<span data-ttu-id="af332-118">Tato verze sady SDK Mobile Engagement webové nepodporuje Samsung inteligentní TV, Opera TV, webOS nebo funkci Reach.</span><span class="sxs-lookup"><span data-stu-id="af332-118">This version of the Mobile Engagement Web SDK doesn't support Samsung Smart TV, Opera TV, webOS, or the Reach feature.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="af332-119">Capptain a Azure Mobile Engagement nejsou stejnou službu a následujících postupů zvýrazněte pouze to, jak migrovat klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="af332-119">Capptain and Azure Mobile Engagement are not the same service, and the following procedures highlight only how to migrate the client app.</span></span> <span data-ttu-id="af332-120">Migrace webových Mobile Engagement SDK do aplikace nebude přenést data ze serveru Capptain na serveru Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="af332-120">Migrating the Mobile Engagement Web SDK in the app will not migrate your data from a Capptain server to a Mobile Engagement server.</span></span>
> 
> 

#### <a name="javascript-files"></a><span data-ttu-id="af332-121">Soubory jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="af332-121">JavaScript files</span></span>
<span data-ttu-id="af332-122">Nahraďte soubor capptain-sdk.js souboru azure-engagement.js a aktualizujte vaše skriptu importy odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="af332-122">Replace the file capptain-sdk.js with the file azure-engagement.js, and then update your script imports accordingly.</span></span>

#### <a name="remove-capptain-reach"></a><span data-ttu-id="af332-123">Odebrat Capptain Reach</span><span class="sxs-lookup"><span data-stu-id="af332-123">Remove Capptain Reach</span></span>
<span data-ttu-id="af332-124">Tato verze sady SDK Mobile Engagement webové nepodporuje funkci Reach.</span><span class="sxs-lookup"><span data-stu-id="af332-124">This version of the Mobile Engagement Web SDK doesn't support the Reach feature.</span></span> <span data-ttu-id="af332-125">Pokud mají integrované Capptain Reach do své aplikace, budete muset odebrat.</span><span class="sxs-lookup"><span data-stu-id="af332-125">If you have integrated Capptain Reach into your application, you need to remove it.</span></span>

<span data-ttu-id="af332-126">Odebrání stránku import dosáhnout šablon stylů CSS a odstraňte soubor .css související (capptain-reach.css, ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="af332-126">Remove the Reach CSS import from your page and delete the related .css file (capptain-reach.css, by default).</span></span>

<span data-ttu-id="af332-127">Odstraňte následující prostředky Reach: zavřít bitové kopie (capptain-close.png, ve výchozím nastavení) a na ikonu značky (capptain ikonu oznámení, ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="af332-127">Delete the following Reach resources: the close image (capptain-close.png, by default) and the brand icon (capptain-notification-icon, by default).</span></span>

<span data-ttu-id="af332-128">Odeberte dosáhnout uživatelského rozhraní pro oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af332-128">Remove the Reach UI for in-app notifications.</span></span> <span data-ttu-id="af332-129">Výchozí rozložení vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="af332-129">The default layout looks like this:</span></span>

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

<span data-ttu-id="af332-130">Odeberte dosáhnout uživatelského rozhraní pro text a webovou oznámení a hlasování.</span><span class="sxs-lookup"><span data-stu-id="af332-130">Remove the Reach UI for text and web announcements and polls.</span></span> <span data-ttu-id="af332-131">Výchozí rozložení vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="af332-131">The default layout looks like this:</span></span>

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

<span data-ttu-id="af332-132">Odeberte `reach` objektu z vaší konfigurace, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="af332-132">Remove the `reach` object from your configuration, if it exists.</span></span> <span data-ttu-id="af332-133">Vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="af332-133">It looks like this:</span></span>

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

<span data-ttu-id="af332-134">Odeberte jakékoli jiné Reach přizpůsobení, jako např.</span><span class="sxs-lookup"><span data-stu-id="af332-134">Remove any other Reach customization, such as categories.</span></span>

#### <a name="remove-deprecated-apis"></a><span data-ttu-id="af332-135">Odeberte nepoužívané rozhraní API</span><span class="sxs-lookup"><span data-stu-id="af332-135">Remove deprecated APIs</span></span>
<span data-ttu-id="af332-136">Některé rozhraní API z Capptain jsou zastaralé v sadě SDK webové Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="af332-136">Some APIs from Capptain are deprecated in the Mobile Engagement Web SDK.</span></span>

<span data-ttu-id="af332-137">Odeberte všechny volání pro následující rozhraní API: `agent.connect`, `agent.disconnect`, `agent.pause`, a `agent.sendMessageToDevice`.</span><span class="sxs-lookup"><span data-stu-id="af332-137">Remove any calls to the following APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, and `agent.sendMessageToDevice`.</span></span>

<span data-ttu-id="af332-138">Odeberte některé z následujících zpětná volání z konfiguraci Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, a `onPushMessageReceived`.</span><span class="sxs-lookup"><span data-stu-id="af332-138">Remove any of the following callbacks from your Capptain configuration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, and `onPushMessageReceived`.</span></span>

#### <a name="configuration"></a><span data-ttu-id="af332-139">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="af332-139">Configuration</span></span>
<span data-ttu-id="af332-140">Mobile Engagement používá připojovací řetězec konfigurace identifikátory SDK, například identifikátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="af332-140">Mobile Engagement uses a connection string to configure SDK identifiers, for example, the application identifier.</span></span>

<span data-ttu-id="af332-141">ID aplikace nahraďte připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="af332-141">Replace the application ID with your connection string.</span></span> <span data-ttu-id="af332-142">Všimněte si, že objekt globální konfigurace sady SDK se změní z `capptain` k `azureEngagement`.</span><span class="sxs-lookup"><span data-stu-id="af332-142">Note that the global object for the SDK configuration changes from `capptain` to `azureEngagement`.</span></span>

<span data-ttu-id="af332-143">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="af332-143">Before migration:</span></span>

    window.capptain = {
      appId: ...,
      [...]
    };

<span data-ttu-id="af332-144">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="af332-144">After migration:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

<span data-ttu-id="af332-145">Připojovací řetězec pro vaši aplikaci se zobrazí na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="af332-145">The connection string for your application is displayed in the Azure portal.</span></span>

#### <a name="javascript-apis"></a><span data-ttu-id="af332-146">Rozhraní API jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="af332-146">JavaScript APIs</span></span>
<span data-ttu-id="af332-147">Globální objekt jazyka JavaScript `window.capptain` byla přejmenována `window.azureEngagement`, ale můžete použít `window.engagement` alias pro volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="af332-147">The global JavaScript object `window.capptain` has been renamed `window.azureEngagement`, but you can use the `window.engagement` alias for API calls.</span></span> <span data-ttu-id="af332-148">Tento alias nelze použít k definování konfigurace sady SDK.</span><span class="sxs-lookup"><span data-stu-id="af332-148">You can't use this alias to define the SDK configuration.</span></span>

<span data-ttu-id="af332-149">Například `capptain.deviceId` stane `engagement.deviceId`, `capptain.agent.startActivity` stane `engagement.agent.startActivity`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="af332-149">For instance, `capptain.deviceId` becomes `engagement.deviceId`, `capptain.agent.startActivity` becomes `engagement.agent.startActivity`, and so on.</span></span>

<span data-ttu-id="af332-150">Pokud jste již spojili dřívější verzi webové sady Azure Mobile Engagement SDK do své aplikace, přečtěte si o [upgrade postupy](mobile-engagement-web-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="af332-150">If you have already integrated an earlier version of the Azure Mobile Engagement Web SDK into your application, please read about [upgrade procedures](mobile-engagement-web-upgrade-procedure.md).</span></span>

