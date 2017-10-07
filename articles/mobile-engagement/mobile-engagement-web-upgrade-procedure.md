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
# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Postupy upgradu Azure Mobile Engagement Web SDK
Pokud jste již spojili dřívější verzi hello sada SDK webové služby Azure Mobile Engagementu do vaší webové aplikaci, musíte tooconsider hello následující body při upgradu hello SDK.

Pokud jste přeskočen více verzí hello SDK webové služby Mobile Engagement, bude pravděpodobně nutné toocomplete několika postupů během procesu upgradu hello. Například, pokud migrujete z 1.4.0 too1.6.0, první postupujte podle hello postupy tooupgrade z 1.4.0 too1.5.0. Potom postupujte podle postupů tooupgrade hello z 1.5.0 too1.6.0.

Podle toho, která verze upgradu, nahraďte všechny předchozí verze souboru hello azure-engagement.js hello nejnovější verzi souboru hello.

## <a name="upgrade-from-121-too200"></a>Upgrade z 1.2.1 too2.0.0
Tato část popisuje, jak toomigrate integrace sady Mobile Engagement Web SDK ze služby Capptain hello, které nabízí Capptain SAS, tooan aplikace Azure Mobile Engagement. Pokud provádíte migraci ze starší verze, prosím naleznete hello Capptain webu toofirst migrovat too1.2.1 a potom použijte následující postupy hello.

Tuto verzi hello Mobile Engagement Web SDK nepodporuje Samsung inteligentní TV, Opera TV, webOS nebo funkci Reach hello.

> [!IMPORTANT]
> Capptain a Azure Mobile Engagement nejsou hello stejnou službu. Následující postup Hello označuje pouze jak toomigrate hello klientskou aplikaci. Migrace hello sady SDK pro aplikaci Mobile Engagement webové aplikace hello nebude migrovat data z Capptain serveru tooa Mobile Engagement.
> 
> 

### <a name="javascript-files"></a>Soubory jazyka JavaScript
Nahraďte soubor hello capptain-sdk.js hello souboru azure-engagement.js a pak aktualizaci vašeho skriptu importuje odpovídajícím způsobem.

### <a name="remove-capptain-reach"></a>Odebrat Capptain Reach
Tato verze hello Mobile Engagement Web SDK nepodporuje funkci Reach hello. Pokud jste Capptain Reach integrovaná do aplikace, je třeba tooremove ho.

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

### <a name="remove-deprecated-apis"></a>Odeberte nepoužívané rozhraní API
Některé rozhraní API z Capptain jsou zastaralé v hello SDK webové služby Mobile Engagement.

Odeberte všechny volání toohello následující rozhraní API: `agent.connect`, `agent.disconnect`, `agent.pause`, a `agent.sendMessageToDevice`.

Odeberte všechny instance hello následujících zpětná volání z konfiguraci Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, a `onPushMessageReceived`.

### <a name="configuration"></a>Konfigurace
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

### <a name="javascript-apis"></a>Rozhraní API jazyka JavaScript
globální objekt jazyka JavaScript Hello `window.capptain` byla přejmenována `window.azureEngagement` ale můžete použít hello `window.engagement` alias pro volání rozhraní API. Hello alias toodefine hello SDK konfigurace nelze použít.

Například `capptain.deviceId` stane `engagement.deviceId`, `capptain.agent.startActivity` stane `engagement.agent.startActivity`a tak dále.

