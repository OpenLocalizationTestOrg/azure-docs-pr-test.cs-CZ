---
title: "Přidání nabízených oznámení do vaší aplikace pomocí mobilních aplikací pro Android | Microsoft Docs"
description: "Naučte se používat Mobile Apps k odesílání nabízených oznámení do aplikace pro Android."
services: app-service\mobile
documentationcenter: android
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: 9058ed6d-e871-4179-86af-0092d0ca09d3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/12/2016
ms.author: glenga
ms.openlocfilehash: b89e9af55342d5d7473d848956996f846250b4b5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-android-app"></a><span data-ttu-id="14dc8-103">Přidání nabízených oznámení do vaší aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="14dc8-103">Add push notifications to your Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="14dc8-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="14dc8-104">Overview</span></span>
<span data-ttu-id="14dc8-105">V tomto kurzu přidáte nabízená oznámení [Android úvodní] projektu tak, aby nabízených oznámení se odešle do zařízení pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="14dc8-105">In this tutorial, you add push notifications to the [Android quick start] project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="14dc8-106">Pokud použijete serverový projekt stažené rychlý start, je třeba balíček rozšíření nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="14dc8-106">If you do not use the downloaded quick start server project, you need the push notification extension package.</span></span> <span data-ttu-id="14dc8-107">Další informace najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="14dc8-107">For more information, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="14dc8-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="14dc8-108">Prerequisites</span></span>
<span data-ttu-id="14dc8-109">Budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="14dc8-109">You need the following:</span></span>

* <span data-ttu-id="14dc8-110">IDE, v závislosti na back-end vašeho projektu:</span><span class="sxs-lookup"><span data-stu-id="14dc8-110">An IDE, depending on your project's back end:</span></span>

  * <span data-ttu-id="14dc8-111">[Android Studio](https://developer.android.com/sdk/index.html) Pokud tato aplikace nemá back-end Node.js.</span><span class="sxs-lookup"><span data-stu-id="14dc8-111">[Android Studio](https://developer.android.com/sdk/index.html) if this app has a Node.js back end.</span></span>
  * <span data-ttu-id="14dc8-112">[Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) nebo novější, pokud tato aplikace nemá rozhraní Microsoft .NET back-end.</span><span class="sxs-lookup"><span data-stu-id="14dc8-112">[Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) or later if this app has a Microsoft .NET back end.</span></span>
* <span data-ttu-id="14dc8-113">Android 2.3 nebo novější, Google úložiště revize 27 nebo novější a služby Google Play 9.0.2 nebo novější pro zasílání zpráv cloudu Firebase.</span><span class="sxs-lookup"><span data-stu-id="14dc8-113">Android 2.3 or later, Google Repository revision 27 or later, and Google Play Services 9.0.2 or later for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="14dc8-114">Dokončení [Android úvodní].</span><span class="sxs-lookup"><span data-stu-id="14dc8-114">Complete the [Android quick start].</span></span>

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a><span data-ttu-id="14dc8-115">Vytvoření projektu, který podporuje službu Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="14dc8-115">Create a project that supports Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a><span data-ttu-id="14dc8-116">Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="14dc8-116">Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a><span data-ttu-id="14dc8-117">Konfigurace Azure k odesílání nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="14dc8-117">Configure Azure to send push notifications</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a><span data-ttu-id="14dc8-118">Povolte nabízená oznámení pro serverový projekt</span><span class="sxs-lookup"><span data-stu-id="14dc8-118">Enable push notifications for the server project</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a><span data-ttu-id="14dc8-119">Přidání nabízených oznámení do aplikace</span><span class="sxs-lookup"><span data-stu-id="14dc8-119">Add push notifications to your app</span></span>
<span data-ttu-id="14dc8-120">V této části že aktualizujete aplikaci Android klienta ke zpracování nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="14dc8-120">In this section, you update your client Android app to handle push notifications.</span></span>

### <a name="verify-android-sdk-version"></a><span data-ttu-id="14dc8-121">Ověřit verzi sady SDK pro Android</span><span class="sxs-lookup"><span data-stu-id="14dc8-121">Verify Android SDK version</span></span>
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

<span data-ttu-id="14dc8-122">Dalším krokem je instalace služby Google Play.</span><span class="sxs-lookup"><span data-stu-id="14dc8-122">Your next step is to install Google Play services.</span></span> <span data-ttu-id="14dc8-123">Google Cloud Messaging má několik minimální rozhraní API úrovně požadavků pro vývoj a testování, která **minSdkVersion** musí odpovídat vlastnosti v manifestu.</span><span class="sxs-lookup"><span data-stu-id="14dc8-123">Google Cloud Messaging has some minimum API level requirements for development and testing, which the **minSdkVersion** property in the manifest must conform to.</span></span>

<span data-ttu-id="14dc8-124">Pokud testujete s starší zařízením, projděte si [nastavit až Google Play Services SDK] určit, jak nedostatek můžete nastavit hodnotu a správně nastavené.</span><span class="sxs-lookup"><span data-stu-id="14dc8-124">If you are testing with an older device, consult [Set Up Google Play Services SDK] to determine how low you can set this value, and set it appropriately.</span></span>

### <a name="add-google-play-services-to-the-project"></a><span data-ttu-id="14dc8-125">Přidejte do projektu služby Google Play</span><span class="sxs-lookup"><span data-stu-id="14dc8-125">Add Google Play services to the project</span></span>
[!INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a><span data-ttu-id="14dc8-126">Přidání kódu</span><span class="sxs-lookup"><span data-stu-id="14dc8-126">Add code</span></span>
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a><span data-ttu-id="14dc8-127">Testování aplikací proti publikované mobilní služby</span><span class="sxs-lookup"><span data-stu-id="14dc8-127">Test the app against the published mobile service</span></span>
<span data-ttu-id="14dc8-128">Aplikaci můžete otestovat připojení přímo telefon se systémem Android pomocí kabelu USB, nebo použití virtuálního zařízení v emulátoru.</span><span class="sxs-lookup"><span data-stu-id="14dc8-128">You can test the app by directly attaching an Android phone with a USB cable, or by using a virtual device in the emulator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14dc8-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14dc8-129">Next steps</span></span>
<span data-ttu-id="14dc8-130">Teď, když jste dokončili tento kurz, vezměte v úvahu pokračovat na jednu z následujících kurzů:</span><span class="sxs-lookup"><span data-stu-id="14dc8-130">Now that you completed this tutorial, consider continuing on to one of the following tutorials:</span></span>

* <span data-ttu-id="14dc8-131">[Přidání ověřování do aplikace pro Android](app-service-mobile-android-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="14dc8-131">[Add authentication to your Android app](app-service-mobile-android-get-started-users.md).</span></span>
  <span data-ttu-id="14dc8-132">Postup přidání ověřování do seznamu úkolů projektu pro rychlý start v systému Android pomocí zprostředkovatele identity podporované.</span><span class="sxs-lookup"><span data-stu-id="14dc8-132">Learn how to add authentication to the todolist quickstart project on Android using a supported identity provider.</span></span>
* <span data-ttu-id="14dc8-133">[Zapnutí offline synchronizace pro svoji aplikaci pro Android](app-service-mobile-android-get-started-offline-data.md).</span><span class="sxs-lookup"><span data-stu-id="14dc8-133">[Enable offline sync for your Android app](app-service-mobile-android-get-started-offline-data.md).</span></span>
  <span data-ttu-id="14dc8-134">Zjistěte, jak přidat podporu offline režimu do aplikace pomocí back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="14dc8-134">Learn how to add offline support to your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="14dc8-135">S offline synchronizací, mohou uživatelé komunikovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="14dc8-135">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- URLs -->
<span data-ttu-id="14dc8-136">[Android úvodní]: app-service-mobile-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="14dc8-136">[Android quick start]: app-service-mobile-android-get-started.md</span></span>

<span data-ttu-id="14dc8-137">[nastavit až Google Play Services SDK]:https://developers.google.com/android/guides/setup</span><span class="sxs-lookup"><span data-stu-id="14dc8-137">[Set Up Google Play Services SDK]:https://developers.google.com/android/guides/setup</span></span>
