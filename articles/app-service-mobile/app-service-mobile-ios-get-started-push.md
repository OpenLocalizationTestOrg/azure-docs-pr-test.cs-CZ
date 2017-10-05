---
title: "Přidání nabízených oznámení do aplikace pomocí Azure Mobile Apps pro iOS"
description: "Naučte se používat Azure Mobile Apps k odesílání nabízených oznámení do aplikace pro iOS."
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
ms.openlocfilehash: 08a8c35b89386bd0dbe7bba406a6985a5a0d7eb8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-ios-app"></a><span data-ttu-id="73513-103">Přidání nabízených oznámení do aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="73513-103">Add Push Notifications to your iOS App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="73513-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="73513-104">Overview</span></span>
<span data-ttu-id="73513-105">V tomto kurzu přidáte nabízená oznámení [iOS rychlý start] projektu tak, aby nabízených oznámení se odešle do zařízení pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="73513-105">In this tutorial, you add push notifications to the [iOS quick start] project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="73513-106">Pokud použijete serverový projekt stažené rychlý start, budete potřebovat balíček rozšíření nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="73513-106">If you do not use the downloaded quick start server project, you will need the push notification extension package.</span></span> <span data-ttu-id="73513-107">V tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="73513-107">See [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

<span data-ttu-id="73513-108">[Simulátoru iOS nabízená oznámení nepodporuje](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span><span class="sxs-lookup"><span data-stu-id="73513-108">The [iOS simulator does not support push notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span></span> <span data-ttu-id="73513-109">Je třeba fyzickém zařízení iOS a [programu pro vývojáře Apple členství](https://developer.apple.com/programs/ios/).</span><span class="sxs-lookup"><span data-stu-id="73513-109">You need a physical iOS device and an [Apple Developer Program membership](https://developer.apple.com/programs/ios/).</span></span>

## <span data-ttu-id="73513-110"><a name="configure-hub"></a>Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="73513-110"><a name="configure-hub"></a>Configure Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <span data-ttu-id="73513-111"><a id="register"></a>Registrace aplikace pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="73513-111"><a id="register"></a>Register app for push notifications</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a><span data-ttu-id="73513-112">Konfigurace Azure k odesílání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="73513-112">Configure Azure to send push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <span data-ttu-id="73513-113"><a id="update-server"></a>Aktualizovat back-end k odesílání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="73513-113"><a id="update-server"></a>Update backend to send push notifications</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <span data-ttu-id="73513-114"><a id="add-push"></a>Přidání nabízených oznámení do aplikace</span><span class="sxs-lookup"><span data-stu-id="73513-114"><a id="add-push"></a>Add push notifications to app</span></span>
[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <span data-ttu-id="73513-115"><a id="test"></a>Nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="73513-115"><a id="test"></a>Test push notifications</span></span>
[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <span data-ttu-id="73513-116"><a id="more"></a>Více</span><span class="sxs-lookup"><span data-stu-id="73513-116"><a id="more"></a>More</span></span>
* <span data-ttu-id="73513-117">Šablony poskytují flexibilitu k odesílání nabízených oznámení napříč platformami a lokalizované nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="73513-117">Templates give you flexibility to send cross-platform pushes and localized pushes.</span></span> <span data-ttu-id="73513-118">[Postup použití iOS Klientská knihovna pro Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) ukazuje, jak zaregistrovat šablony.</span><span class="sxs-lookup"><span data-stu-id="73513-118">[How to Use iOS Client Library for Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) shows you how to register templates.</span></span>

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
<span data-ttu-id="73513-119">[iOS rychlý start]: app-service-mobile-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="73513-119">[iOS quick start]: app-service-mobile-ios-get-started.md</span></span>
