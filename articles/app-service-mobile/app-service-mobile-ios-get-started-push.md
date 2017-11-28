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
# <a name="add-push-notifications-tooyour-ios-app"></a><span data-ttu-id="fff2f-103">Přidat tooyour nabízených oznámení aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="fff2f-103">Add Push Notifications tooyour iOS App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="fff2f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="fff2f-104">Overview</span></span>
<span data-ttu-id="fff2f-105">V tomto kurzu přidáte nabízená oznámení toohello [iOS rychlý start] projektu tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="fff2f-105">In this tutorial, you add push notifications toohello [iOS quick start] project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="fff2f-106">Pokud nepoužijete hello stáhli úvodní serverový projekt, bude nutné hello nabízených oznámení v balíčku rozšíření.</span><span class="sxs-lookup"><span data-stu-id="fff2f-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="fff2f-107">V tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="fff2f-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

<span data-ttu-id="fff2f-108">Hello [simulátoru iOS nabízená oznámení nepodporuje](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span><span class="sxs-lookup"><span data-stu-id="fff2f-108">hello [iOS simulator does not support push notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span></span> <span data-ttu-id="fff2f-109">Je třeba fyzickém zařízení iOS a [programu pro vývojáře Apple členství](https://developer.apple.com/programs/ios/).</span><span class="sxs-lookup"><span data-stu-id="fff2f-109">You need a physical iOS device and an [Apple Developer Program membership](https://developer.apple.com/programs/ios/).</span></span>

## <span data-ttu-id="fff2f-110"><a name="configure-hub"></a>Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="fff2f-110"><a name="configure-hub"></a>Configure Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <span data-ttu-id="fff2f-111"><a id="register"></a>Registrace aplikace pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="fff2f-111"><a id="register"></a>Register app for push notifications</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-toosend-push-notifications"></a><span data-ttu-id="fff2f-112">Konfigurace Azure toosend nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="fff2f-112">Configure Azure toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <span data-ttu-id="fff2f-113"><a id="update-server"></a>Aktualizovat back-end toosend nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="fff2f-113"><a id="update-server"></a>Update backend toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <span data-ttu-id="fff2f-114"><a id="add-push"></a>Přidat nabízená oznámení tooapp</span><span class="sxs-lookup"><span data-stu-id="fff2f-114"><a id="add-push"></a>Add push notifications tooapp</span></span>
[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <span data-ttu-id="fff2f-115"><a id="test"></a>Nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="fff2f-115"><a id="test"></a>Test push notifications</span></span>
[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <span data-ttu-id="fff2f-116"><a id="more"></a>Více</span><span class="sxs-lookup"><span data-stu-id="fff2f-116"><a id="more"></a>More</span></span>
* <span data-ttu-id="fff2f-117">Šablony umožňují nabízených oznámení napříč platformami toosend flexibilitu a lokalizované nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="fff2f-117">Templates give you flexibility toosend cross-platform pushes and localized pushes.</span></span> <span data-ttu-id="fff2f-118">[Jak tooUse iOS Klientská knihovna pro Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) ukazuje, jak tooregister šablony.</span><span class="sxs-lookup"><span data-stu-id="fff2f-118">[How tooUse iOS Client Library for Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) shows you how tooregister templates.</span></span>

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS rychlý start]: app-service-mobile-ios-get-started.md
