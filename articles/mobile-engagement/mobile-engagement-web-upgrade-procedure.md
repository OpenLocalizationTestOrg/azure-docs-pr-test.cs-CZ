---
title: "postupy upgradu aaaAzure SDK webové služby Mobile Engagement | Microsoft Docs"
description: "Hello nejnovější aktualizace a postupy pro hello Web SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a20529b4-ec8d-4503-8ae9-09b5f0846d5b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: a2df65904c6b56584ce6588ed26a9b79f3aa27ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a><span data-ttu-id="edb97-103">Postupy upgradu Azure Mobile Engagement Web SDK</span><span class="sxs-lookup"><span data-stu-id="edb97-103">Azure Mobile Engagement Web SDK upgrade procedures</span></span>
<span data-ttu-id="edb97-104">Pokud jste již spojili dřívější verzi hello sada SDK webové služby Azure Mobile Engagementu do vaší webové aplikaci, musíte tooconsider hello následující body při upgradu hello SDK.</span><span class="sxs-lookup"><span data-stu-id="edb97-104">If you have already integrated an earlier version of hello Azure Mobile Engagement Web SDK into your web application, you need tooconsider hello following points when you upgrade hello SDK.</span></span>

<span data-ttu-id="edb97-105">Pokud jste přeskočen více verzí hello SDK webové služby Mobile Engagement, bude pravděpodobně nutné toocomplete několika postupů během procesu upgradu hello.</span><span class="sxs-lookup"><span data-stu-id="edb97-105">If you skipped multiple versions of hello Mobile Engagement Web SDK, you might need toocomplete several procedures during hello upgrade process.</span></span> <span data-ttu-id="edb97-106">Například, pokud migrujete z 1.4.0 too1.6.0, první postupujte podle hello postupy tooupgrade z 1.4.0 too1.5.0.</span><span class="sxs-lookup"><span data-stu-id="edb97-106">For example, if you migrate from 1.4.0 too1.6.0, first follow hello procedures tooupgrade from 1.4.0 too1.5.0.</span></span> <span data-ttu-id="edb97-107">Potom postupujte podle postupů tooupgrade hello z 1.5.0 too1.6.0.</span><span class="sxs-lookup"><span data-stu-id="edb97-107">Then, follow hello procedures tooupgrade from 1.5.0 too1.6.0.</span></span>

<span data-ttu-id="edb97-108">Podle toho, která verze upgradu, nahraďte všechny předchozí verze souboru hello azure-engagement.js hello nejnovější verzi souboru hello.</span><span class="sxs-lookup"><span data-stu-id="edb97-108">Whichever version you upgrade from, replace any earlier version of hello file azure-engagement.js with hello latest version of hello file.</span></span>

## <a name="upgrade-from-121-too200"></a><span data-ttu-id="edb97-109">Upgrade z 1.2.1 too2.0.0</span><span class="sxs-lookup"><span data-stu-id="edb97-109">Upgrade from 1.2.1 too2.0.0</span></span>
<span data-ttu-id="edb97-110">Tato část popisuje, jak toomigrate integrace sady Mobile Engagement Web SDK ze služby Capptain hello, které nabízí Capptain SAS, tooan aplikace Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="edb97-110">This section describes how toomigrate a Mobile Engagement Web SDK integration from hello Capptain service, offered by Capptain SAS, tooan Azure Mobile Engagement app.</span></span> <span data-ttu-id="edb97-111">Pokud provádíte migraci ze starší verze, prosím naleznete hello Capptain webu toofirst migrovat too1.2.1 a potom použijte následující postupy hello.</span><span class="sxs-lookup"><span data-stu-id="edb97-111">If you are migrating from an earlier version, please consult hello Capptain website toofirst migrate too1.2.1, and then apply hello following procedures.</span></span>

<span data-ttu-id="edb97-112">Tuto verzi hello Mobile Engagement Web SDK nepodporuje Samsung inteligentní TV, Opera TV, webOS nebo funkci Reach hello.</span><span class="sxs-lookup"><span data-stu-id="edb97-112">This version of hello Mobile Engagement Web SDK doesn't support Samsung Smart TV, Opera TV, webOS, or hello Reach feature.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="edb97-113">Capptain a Azure Mobile Engagement nejsou hello stejnou službu.</span><span class="sxs-lookup"><span data-stu-id="edb97-113">Capptain and Azure Mobile Engagement are not hello same service.</span></span> <span data-ttu-id="edb97-114">Následující postup Hello označuje pouze jak toomigrate hello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="edb97-114">hello following procedure highlights only how toomigrate hello client app.</span></span> <span data-ttu-id="edb97-115">Migrace hello sady SDK pro aplikaci Mobile Engagement webové aplikace hello nebude migrovat data z Capptain serveru tooa Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="edb97-115">Migrating hello Mobile Engagement Web SDK in hello app will not migrate your data from a Capptain server tooa Mobile Engagement server.</span></span>
> 
> 

### <a name="javascript-files"></a><span data-ttu-id="edb97-116">Soubory jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="edb97-116">JavaScript files</span></span>
<span data-ttu-id="edb97-117">Nahraďte soubor hello capptain-sdk.js hello souboru azure-engagement.js a pak aktualizaci vašeho skriptu importuje odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="edb97-117">Replace hello file capptain-sdk.js with hello file azure-engagement.js, and then update your script imports accordingly.</span></span>

### <a name="remove-capptain-reach"></a><span data-ttu-id="edb97-118">Odebrat Capptain Reach</span><span class="sxs-lookup"><span data-stu-id="edb97-118">Remove Capptain Reach</span></span>
<span data-ttu-id="edb97-119">Tato verze hello Mobile Engagement Web SDK nepodporuje funkci Reach hello.</span><span class="sxs-lookup"><span data-stu-id="edb97-119">This version of hello Mobile Engagement Web SDK doesn't support hello Reach feature.</span></span> <span data-ttu-id="edb97-120">Pokud jste Capptain Reach integrovaná do aplikace, je třeba tooremove ho.</span><span class="sxs-lookup"><span data-stu-id="edb97-120">If you integrated Capptain Reach into your application, you need tooremove it.</span></span>

<span data-ttu-id="edb97-121">Odebrání stránku hello import dosáhnout šablon stylů CSS a odstranit soubor .css související hello (capptain-reach.css, ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="edb97-121">Remove hello Reach CSS import from your page and delete hello related .css file (capptain-reach.css, by default).</span></span>

<span data-ttu-id="edb97-122">Odstranit následující prostředky Reach hello: hello zavřít bitové kopie (capptain-close.png, ve výchozím nastavení) a ikonu značky hello (capptain ikonu oznámení, ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="edb97-122">Delete hello following Reach resources: hello close image (capptain-close.png, by default) and hello brand icon (capptain-notification-icon, by default).</span></span>

<span data-ttu-id="edb97-123">Odeberte hello dosáhnout uživatelského rozhraní pro oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="edb97-123">Remove hello Reach UI for in-app notifications.</span></span> <span data-ttu-id="edb97-124">výchozí rozložení Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="edb97-124">hello default layout looks like this:</span></span>

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

<span data-ttu-id="edb97-125">Odeberte hello dosáhnout uživatelského rozhraní pro text a webovou oznámení a hlasování.</span><span class="sxs-lookup"><span data-stu-id="edb97-125">Remove hello Reach UI for text and web announcements and polls.</span></span> <span data-ttu-id="edb97-126">výchozí rozložení Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="edb97-126">hello default layout looks like this:</span></span>

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

<span data-ttu-id="edb97-127">Odebrat hello `reach` objektu z vaší konfigurace, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="edb97-127">Remove hello `reach` object from your configuration, if it exists.</span></span> <span data-ttu-id="edb97-128">Vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="edb97-128">It looks like this:</span></span>

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

<span data-ttu-id="edb97-129">Odeberte jakékoli jiné Reach přizpůsobení, jako např.</span><span class="sxs-lookup"><span data-stu-id="edb97-129">Remove any other Reach customization, such as categories.</span></span>

### <a name="remove-deprecated-apis"></a><span data-ttu-id="edb97-130">Odeberte nepoužívané rozhraní API</span><span class="sxs-lookup"><span data-stu-id="edb97-130">Remove deprecated APIs</span></span>
<span data-ttu-id="edb97-131">Některé rozhraní API z Capptain jsou zastaralé v hello SDK webové služby Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="edb97-131">Some APIs from Capptain are deprecated in hello Mobile Engagement Web SDK.</span></span>

<span data-ttu-id="edb97-132">Odeberte všechny volání toohello následující rozhraní API: `agent.connect`, `agent.disconnect`, `agent.pause`, a `agent.sendMessageToDevice`.</span><span class="sxs-lookup"><span data-stu-id="edb97-132">Remove any calls toohello following APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, and `agent.sendMessageToDevice`.</span></span>

<span data-ttu-id="edb97-133">Odeberte všechny instance hello následujících zpětná volání z konfiguraci Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, a `onPushMessageReceived`.</span><span class="sxs-lookup"><span data-stu-id="edb97-133">Remove any instances of hello following callbacks from your Capptain configuration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, and `onPushMessageReceived`.</span></span>

### <a name="configuration"></a><span data-ttu-id="edb97-134">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="edb97-134">Configuration</span></span>
<span data-ttu-id="edb97-135">Mobile Engagement používá připojovací řetězec tooconfigure SDK identifikátory, například identifikátor aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="edb97-135">Mobile Engagement uses a connection string tooconfigure SDK identifiers, for example, hello application identifier.</span></span>

<span data-ttu-id="edb97-136">ID aplikace hello nahraďte připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="edb97-136">Replace hello application ID with your connection string.</span></span> <span data-ttu-id="edb97-137">Poznámka: Tento objekt globální hello hello konfigurace sady SDK změny z `capptain` příliš`azureEngagement`.</span><span class="sxs-lookup"><span data-stu-id="edb97-137">Note that hello global object for hello SDK configuration changes from `capptain` too`azureEngagement`.</span></span>

<span data-ttu-id="edb97-138">Před migrací:</span><span class="sxs-lookup"><span data-stu-id="edb97-138">Before migration:</span></span>

    window.capptain = {
      appId: ...,
      [...]
    };

<span data-ttu-id="edb97-139">Po dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="edb97-139">After migration:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

<span data-ttu-id="edb97-140">Hello připojovací řetězec pro vaši aplikaci se zobrazí v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="edb97-140">hello connection string for your application is displayed in hello Azure Portal.</span></span>

### <a name="javascript-apis"></a><span data-ttu-id="edb97-141">Rozhraní API jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="edb97-141">JavaScript APIs</span></span>
<span data-ttu-id="edb97-142">globální objekt jazyka JavaScript Hello `window.capptain` byla přejmenována `window.azureEngagement` ale můžete použít hello `window.engagement` alias pro volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="edb97-142">hello global JavaScript object `window.capptain` has been renamed `window.azureEngagement` but you can use hello `window.engagement` alias for API calls.</span></span> <span data-ttu-id="edb97-143">Hello alias toodefine hello SDK konfigurace nelze použít.</span><span class="sxs-lookup"><span data-stu-id="edb97-143">You can't use hello alias toodefine hello SDK configuration.</span></span>

<span data-ttu-id="edb97-144">Například `capptain.deviceId` stane `engagement.deviceId`, `capptain.agent.startActivity` stane `engagement.agent.startActivity`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="edb97-144">For instance, `capptain.deviceId` becomes `engagement.deviceId`, `capptain.agent.startActivity` becomes `engagement.agent.startActivity`, and so on.</span></span>

