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
# <a name="azure-mobile-engagement-web-sdk"></a>Web Azure Mobile Engagement SDK
Začněte zde pro všechny hello podrobnosti o způsobu toointegrate Azure Mobile Engagement ve webové aplikaci. Pokud chcete toogive ji zkuste před Začínáme s vlastní webové aplikace, najdete v našich [15 minut kurzu](mobile-engagement-web-app-get-started.md).

## <a name="integration-procedures"></a>Integrace procedury
1. Další informace [jak toointegrate Mobile Engagement ve vaší webové aplikaci](mobile-engagement-web-integrate-engagement.md).
2. Implementace plánu značky, další [jak toouse hello advanced označování rozhraní API ve vaší webové aplikaci Mobile Engagement](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Poznámky k verzi
### <a name="202-10182016"></a>2.0.2 (10/18/2016)
* Opravené havárií na privátní procházení (Safari).
* Opravené havárie v prohlížečích se soubory cookie zakázána.

Všechny verze, najdete v tématu hello [dokončení poznámky k verzi](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Postupy upgradu
### <a name="upgrade-from-121-too200"></a>Upgrade z 1.2.1 too2.0.0
Hello následující části popisují, jak toomigrate integrace sady Mobile Engagement Web SDK ze služby Capptain hello, které nabízí Capptain SAS, tooan aplikace Azure Mobile Engagement. Pokud provádíte migraci ze starší verze než 1.2.1, informujte hello Capptain webu toomigrate too1.2.1 nejprve a potom použijte hello následující postupy.

Tuto verzi hello Mobile Engagement Web SDK nepodporuje Samsung inteligentní TV, Opera TV, webOS nebo funkci Reach hello.

> [!IMPORTANT]
> Capptain a Azure Mobile Engagement nejsou hello stejnou službu a hello následující postupy zvýrazněte pouze jak toomigrate hello klientskou aplikaci. Migrace hello sady SDK pro aplikaci Mobile Engagement webové aplikace hello nebude migrovat data z Capptain serveru tooa Mobile Engagement.
> 
> 

#### <a name="javascript-files"></a>Soubory jazyka JavaScript
Nahraďte soubor hello capptain-sdk.js hello souboru azure-engagement.js a pak aktualizaci vašeho skriptu importuje odpovídajícím způsobem.

#### <a name="remove-capptain-reach"></a>Odebrat Capptain Reach
Tato verze hello Mobile Engagement Web SDK nepodporuje funkci Reach hello. Pokud mají integrované Capptain Reach do své aplikace, je třeba tooremove ho.

Odebrání stránku hello import dosáhnout šablon stylů CSS a odstranit soubor .css související hello (capptain-reach.css, ve výchozím nastavení).

Odstranit následující prostředky Reach hello: hello zavřít bitové kopie (capptain-close.png, ve výchozím nastavení) a ikonu značky hello (capptain ikonu oznámení, ve výchozím nastavení).

Odeberte hello dosáhnout uživatelského rozhraní pro oznámení v aplikaci. výchozí rozložení Hello vypadá takto:

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

Odeberte hello dosáhnout uživatelského rozhraní pro text a webovou oznámení a hlasování. výchozí rozložení Hello vypadá takto:

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

Odebrat hello `reach` objektu z vaší konfigurace, pokud existuje. Vypadá takto:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Odeberte jakékoli jiné Reach přizpůsobení, jako např.

#### <a name="remove-deprecated-apis"></a>Odeberte nepoužívané rozhraní API
Některé rozhraní API z Capptain jsou zastaralé v hello SDK webové služby Mobile Engagement.

Odeberte všechny volání toohello následující rozhraní API: `agent.connect`, `agent.disconnect`, `agent.pause`, a `agent.sendMessageToDevice`.

Odeberte některé z následujících zpětná volání z konfiguraci Capptain hello: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, a `onPushMessageReceived`.

#### <a name="configuration"></a>Konfigurace
Mobile Engagement používá připojovací řetězec tooconfigure SDK identifikátory, například identifikátor aplikace hello.

ID aplikace hello nahraďte připojovací řetězec. Poznámka: Tento objekt globální hello hello konfigurace sady SDK změny z `capptain` příliš`azureEngagement`.

Před migrací:

    window.capptain = {
      appId: ...,
      [...]
    };

Po dokončení migrace:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

Hello připojovací řetězec pro vaši aplikaci se zobrazí v hello portálu Azure.

#### <a name="javascript-apis"></a>Rozhraní API jazyka JavaScript
globální objekt jazyka JavaScript Hello `window.capptain` byla přejmenována `window.azureEngagement`, ale můžete použít hello `window.engagement` alias pro volání rozhraní API. Konfigurace sady SDK hello toodefine tento alias nelze použít.

Například `capptain.deviceId` stane `engagement.deviceId`, `capptain.agent.startActivity` stane `engagement.agent.startActivity`a tak dále.

Pokud jste již spojili dřívější verzi hello Azure Mobile Engagement Web SDK do své aplikace, přečtěte si o [upgrade postupy](mobile-engagement-web-upgrade-procedure.md).

