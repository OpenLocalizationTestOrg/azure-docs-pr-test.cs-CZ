---
title: "aaaAdd tooiOS nabízených oznámení aplikace pomocí Azure Mobile Apps"
description: "Zjistěte, jak Azure Mobile Apps toosend toouse nabízená oznámení aplikace iOS tooyour."
services: app-service\mobile
documentationcenter: ios
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/10/2016
ms.author: glenga
ms.openlocfilehash: 11588c56bc8987a71257dbad4fcdebf1b3209b1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-ios-app"></a>Přidat tooyour nabízených oznámení aplikace pro iOS
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Přehled
V tomto kurzu přidáte nabízená oznámení toohello [iOS rychlý start] projektu tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.

Pokud nepoužijete hello stáhli úvodní serverový projekt, bude nutné hello nabízených oznámení v balíčku rozšíření. V tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.

Hello [simulátoru iOS nabízená oznámení nepodporuje](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Je třeba fyzickém zařízení iOS a [programu pro vývojáře Apple členství](https://developer.apple.com/programs/ios/).

## <a name="configure-hub"></a>Konfigurace centra oznámení
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Registrace aplikace pro nabízená oznámení
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-toosend-push-notifications"></a>Konfigurace Azure toosend nabízená oznámení
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a id="update-server"></a>Aktualizovat back-end toosend nabízená oznámení
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Přidat nabízená oznámení tooapp
[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Nabízená oznámení
[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a id="more"></a>Více
* Šablony umožňují nabízených oznámení napříč platformami toosend flexibilitu a lokalizované nabízených oznámení. [Jak tooUse iOS Klientská knihovna pro Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) ukazuje, jak tooregister šablony.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS rychlý start]: app-service-mobile-ios-get-started.md
