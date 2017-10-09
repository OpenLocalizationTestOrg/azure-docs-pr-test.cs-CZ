---
title: "aaaAdd nabízená oznámení tooyour aplikace pro Android s Mobile Apps | Microsoft Docs"
description: "Zjistěte, jak toouse Mobile Apps toosend nabízená oznámení tooyour aplikace pro Android."
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
ms.openlocfilehash: dcfb8463b395904b4bd0bf9bde2f71f259894066
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-android-app"></a><span data-ttu-id="b5859-103">Přidat nabízená oznámení tooyour aplikace Android</span><span class="sxs-lookup"><span data-stu-id="b5859-103">Add push notifications tooyour Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="b5859-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b5859-104">Overview</span></span>
<span data-ttu-id="b5859-105">V tomto kurzu přidáte nabízená oznámení toohello [Android úvodní] projektu tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="b5859-105">In this tutorial, you add push notifications toohello [Android quick start] project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="b5859-106">Pokud nepoužijete hello stáhli úvodní serverový projekt, třeba hello nabízených oznámení v balíčku rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b5859-106">If you do not use hello downloaded quick start server project, you need hello push notification extension package.</span></span> <span data-ttu-id="b5859-107">Další informace najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="b5859-107">For more information, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5859-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b5859-108">Prerequisites</span></span>
<span data-ttu-id="b5859-109">Je třeba hello následující:</span><span class="sxs-lookup"><span data-stu-id="b5859-109">You need hello following:</span></span>

* <span data-ttu-id="b5859-110">IDE, v závislosti na back-end vašeho projektu:</span><span class="sxs-lookup"><span data-stu-id="b5859-110">An IDE, depending on your project's back end:</span></span>

  * <span data-ttu-id="b5859-111">[Android Studio](https://developer.android.com/sdk/index.html) Pokud tato aplikace nemá back-end Node.js.</span><span class="sxs-lookup"><span data-stu-id="b5859-111">[Android Studio](https://developer.android.com/sdk/index.html) if this app has a Node.js back end.</span></span>
  * <span data-ttu-id="b5859-112">[Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) nebo novější, pokud tato aplikace nemá rozhraní Microsoft .NET back-end.</span><span class="sxs-lookup"><span data-stu-id="b5859-112">[Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) or later if this app has a Microsoft .NET back end.</span></span>
* <span data-ttu-id="b5859-113">Android 2.3 nebo novější, Google úložiště revize 27 nebo novější a služby Google Play 9.0.2 nebo novější pro zasílání zpráv cloudu Firebase.</span><span class="sxs-lookup"><span data-stu-id="b5859-113">Android 2.3 or later, Google Repository revision 27 or later, and Google Play Services 9.0.2 or later for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="b5859-114">Dokončení hello [Android úvodní].</span><span class="sxs-lookup"><span data-stu-id="b5859-114">Complete hello [Android quick start].</span></span>

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a><span data-ttu-id="b5859-115">Vytvoření projektu, který podporuje službu Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="b5859-115">Create a project that supports Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a><span data-ttu-id="b5859-116">Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="b5859-116">Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-toosend-push-notifications"></a><span data-ttu-id="b5859-117">Konfigurace Azure toosend nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="b5859-117">Configure Azure toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-hello-server-project"></a><span data-ttu-id="b5859-118">Povolte nabízená oznámení pro projekt server hello</span><span class="sxs-lookup"><span data-stu-id="b5859-118">Enable push notifications for hello server project</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-tooyour-app"></a><span data-ttu-id="b5859-119">Přidat nabízená oznámení tooyour aplikaci</span><span class="sxs-lookup"><span data-stu-id="b5859-119">Add push notifications tooyour app</span></span>
<span data-ttu-id="b5859-120">V této části aktualizace klienta aplikace pro Android toohandle nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="b5859-120">In this section, you update your client Android app toohandle push notifications.</span></span>

### <a name="verify-android-sdk-version"></a><span data-ttu-id="b5859-121">Ověřit verzi sady SDK pro Android</span><span class="sxs-lookup"><span data-stu-id="b5859-121">Verify Android SDK version</span></span>
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

<span data-ttu-id="b5859-122">Dalším krokem je tooinstall služby Google Play.</span><span class="sxs-lookup"><span data-stu-id="b5859-122">Your next step is tooinstall Google Play services.</span></span> <span data-ttu-id="b5859-123">Google Cloud Messaging má několik minimální rozhraní API úrovně požadavků pro vývoj a testování, které hello **minSdkVersion** musí odpovídat vlastnosti v manifestu hello.</span><span class="sxs-lookup"><span data-stu-id="b5859-123">Google Cloud Messaging has some minimum API level requirements for development and testing, which hello **minSdkVersion** property in hello manifest must conform to.</span></span>

<span data-ttu-id="b5859-124">Pokud testujete s starší zařízením, projděte si [nastavit až Google Play Services SDK] toodetermine jak nízkou můžete nastavit tuto hodnotu a správně nastavené.</span><span class="sxs-lookup"><span data-stu-id="b5859-124">If you are testing with an older device, consult [Set Up Google Play Services SDK] toodetermine how low you can set this value, and set it appropriately.</span></span>

### <a name="add-google-play-services-toohello-project"></a><span data-ttu-id="b5859-125">Přidání projektu toohello služby Google Play</span><span class="sxs-lookup"><span data-stu-id="b5859-125">Add Google Play services toohello project</span></span>
[!INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a><span data-ttu-id="b5859-126">Přidání kódu</span><span class="sxs-lookup"><span data-stu-id="b5859-126">Add code</span></span>
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-hello-app-against-hello-published-mobile-service"></a><span data-ttu-id="b5859-127">Testování aplikace hello proti hello publikovaná mobilní služby</span><span class="sxs-lookup"><span data-stu-id="b5859-127">Test hello app against hello published mobile service</span></span>
<span data-ttu-id="b5859-128">Hello aplikaci můžete otestovat připojení přímo telefon se systémem Android pomocí kabelu USB, nebo použití virtuálního zařízení v emulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="b5859-128">You can test hello app by directly attaching an Android phone with a USB cable, or by using a virtual device in hello emulator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5859-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5859-129">Next steps</span></span>
<span data-ttu-id="b5859-130">Teď, když jste dokončili tento kurz, vezměte v úvahu pokračovat na tooone hello následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="b5859-130">Now that you completed this tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* <span data-ttu-id="b5859-131">[Přidat aplikaci pro Android tooyour ověřování](app-service-mobile-android-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="b5859-131">[Add authentication tooyour Android app](app-service-mobile-android-get-started-users.md).</span></span>
  <span data-ttu-id="b5859-132">Zjistěte, jak rychlý start tooadd ověřování toohello seznamu úkolů projektu v systému Android pomocí zprostředkovatele identity podporované.</span><span class="sxs-lookup"><span data-stu-id="b5859-132">Learn how tooadd authentication toohello todolist quickstart project on Android using a supported identity provider.</span></span>
* <span data-ttu-id="b5859-133">[Zapnutí offline synchronizace pro svoji aplikaci pro Android](app-service-mobile-android-get-started-offline-data.md).</span><span class="sxs-lookup"><span data-stu-id="b5859-133">[Enable offline sync for your Android app](app-service-mobile-android-get-started-offline-data.md).</span></span>
  <span data-ttu-id="b5859-134">Zjistěte, jak tooadd offline podporovat tooyour aplikaci pomocí back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="b5859-134">Learn how tooadd offline support tooyour app by using a Mobile Apps back end.</span></span> <span data-ttu-id="b5859-135">S offline synchronizací, mohou uživatelé komunikovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="b5859-135">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- URLs -->
[Android úvodní]: app-service-mobile-android-get-started.md

[nastavit až Google Play Services SDK]:https://developers.google.com/android/guides/setup
