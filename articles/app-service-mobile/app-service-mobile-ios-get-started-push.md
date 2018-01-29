---
title: "Přidání nabízených oznámení do aplikace pomocí Azure Mobile Apps pro iOS"
description: "Naučte se používat Azure Mobile Apps k odesílání nabízených oznámení do aplikace pro iOS."
services: app-service\mobile
documentationcenter: ios
manager: crdun
editor: 
author: conceptdev
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/10/2016
ms.author: crdun
ms.openlocfilehash: 063926a5a98aeab62e191d372831a00c29b9f6d7
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="add-push-notifications-to-your-ios-app"></a>Přidání nabízených oznámení do aplikace pro iOS
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Přehled
V tomto kurzu přidáte nabízená oznámení [iOS rychlý start] projektu tak, aby nabízených oznámení se odešle do zařízení pokaždé, když vložení záznamu.

Pokud použijete serverový projekt stažené rychlý start, budete potřebovat balíček rozšíření nabízená oznámení. V tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.

[Simulátoru iOS nabízená oznámení nepodporuje](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Je třeba fyzickém zařízení iOS a [programu pro vývojáře Apple členství](https://developer.apple.com/programs/ios/).

## <a name="configure-hub"></a>Konfigurace centra oznámení
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Registrace aplikace pro nabízená oznámení
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurace Azure k odesílání nabízených oznámení
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a id="update-server"></a>Aktualizovat back-end k odesílání nabízených oznámení
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Přidání nabízených oznámení do aplikace
[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Nabízená oznámení
[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a id="more"></a>Více
* Šablony poskytují flexibilitu k odesílání nabízených oznámení napříč platformami a lokalizované nabízených oznámení. [Postup použití iOS Klientská knihovna pro Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) ukazuje, jak zaregistrovat šablony.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS rychlý start]: app-service-mobile-ios-get-started.md
