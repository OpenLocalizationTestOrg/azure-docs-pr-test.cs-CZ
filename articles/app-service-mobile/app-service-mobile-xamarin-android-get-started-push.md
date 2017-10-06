---
title: "aplikace Xamarin.Android aaaAdd nabízená oznámení tooyour | Microsoft Docs"
description: "Zjistěte, jak toouse Azure App Service a Azure Notification Hubs toosend nabízená oznámení aplikace Xamarin.Android tooyour"
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: c93d1d0cae06ab15e3e3e5c4b342808b2cd49113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-xamarinandroid-app"></a><span data-ttu-id="f898f-103">Přidat nabízená oznámení tooyour Xamarin.Android aplikaci</span><span class="sxs-lookup"><span data-stu-id="f898f-103">Add push notifications tooyour Xamarin.Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="f898f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f898f-104">Overview</span></span>
<span data-ttu-id="f898f-105">V tomto kurzu přidáte nabízená oznámení toohello [Xamarin.Android úvodní](app-service-mobile-windows-store-dotnet-get-started.md) projektu tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.</span><span class="sxs-lookup"><span data-stu-id="f898f-105">In this tutorial, you add push notifications toohello [Xamarin.Android quick start](app-service-mobile-windows-store-dotnet-get-started.md) project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="f898f-106">Pokud nepoužijete hello stáhli úvodní serverový projekt, bude nutné hello nabízených oznámení v balíčku rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f898f-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="f898f-107">V tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="f898f-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f898f-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f898f-108">Prerequisites</span></span>
<span data-ttu-id="f898f-109">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="f898f-109">This tutorial requires hello following:</span></span>

* <span data-ttu-id="f898f-110">Aktivní účet Google.</span><span class="sxs-lookup"><span data-stu-id="f898f-110">An active Google account.</span></span> <span data-ttu-id="f898f-111">Můžete si zaregistrovat účet Google na webu [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span><span class="sxs-lookup"><span data-stu-id="f898f-111">You can sign up for a Google account at [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span></span>
* <span data-ttu-id="f898f-112">[Komponenta klienta zasílání zpráv cloudu Google](http://components.xamarin.com/view/GCMClient/).</span><span class="sxs-lookup"><span data-stu-id="f898f-112">[Google Cloud Messaging Client Component](http://components.xamarin.com/view/GCMClient/).</span></span>

## <span data-ttu-id="f898f-113"><a name="configure-hub"></a>Konfigurace centra oznámení</span><span class="sxs-lookup"><span data-stu-id="f898f-113"><a name="configure-hub"></a>Configure a Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <span data-ttu-id="f898f-114"><a id="register"></a>Povolit Firebase cloudu zasílání zpráv</span><span class="sxs-lookup"><span data-stu-id="f898f-114"><a id="register"></a>Enable Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-toosend-push-requests"></a><span data-ttu-id="f898f-115">Konfigurace Azure toosend nabízené požadavků</span><span class="sxs-lookup"><span data-stu-id="f898f-115">Configure Azure toosend push requests</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <span data-ttu-id="f898f-116"><a id="update-server"></a>Aktualizovat hello serveru projektu toosend nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="f898f-116"><a id="update-server"></a>Update hello server project toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <span data-ttu-id="f898f-117"><a id="configure-app"></a>Konfigurace projektu hello klienta pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="f898f-117"><a id="configure-app"></a>Configure hello client project for push notifications</span></span>
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <span data-ttu-id="f898f-118"><a id="add-push"></a>Přidat nabízená oznámení kód tooyour aplikaci</span><span class="sxs-lookup"><span data-stu-id="f898f-118"><a id="add-push"></a>Add push notifications code tooyour app</span></span>
[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <span data-ttu-id="f898f-119"><a name="test"></a>Nabízená oznámení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="f898f-119"><a name="test"></a>Test push notifications in your app</span></span>
<span data-ttu-id="f898f-120">Hello aplikaci můžete otestovat pomocí virtuálního zařízení v emulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="f898f-120">You can test hello app by using a virtual device in hello emulator.</span></span> <span data-ttu-id="f898f-121">Existují další konfigurační kroky potřebné při spuštění v emulátoru.</span><span class="sxs-lookup"><span data-stu-id="f898f-121">There are additional configuration steps required when running on an emulator.</span></span>

1. <span data-ttu-id="f898f-122">Ujistěte se, že nasazujete tooor ladění na virtuální zařízení, které se má nastavit jako cíl hello rozhraní Google API, jak je vidět níže ve Správci hello virtuální zařízení Android (AVD).</span><span class="sxs-lookup"><span data-stu-id="f898f-122">Make sure that you are deploying tooor debugging on a virtual device that has Google APIs set as hello target, as shown below in hello Android Virtual Device (AVD) manager.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)
2. <span data-ttu-id="f898f-123">Kliknutím na Přidat zařízení Android toohello účet Google **aplikace** > **nastavení** > **přidejte účet**, postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="f898f-123">Add a Google account toohello Android device by clicking **Apps** > **Settings** > **Add account**, then follow hello prompts.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)
3. <span data-ttu-id="f898f-124">Spusťte aplikaci seznamu úkolů hello jako před a vložit novou položku úkolů.</span><span class="sxs-lookup"><span data-stu-id="f898f-124">Run hello todolist app as before and insert a new todo item.</span></span> <span data-ttu-id="f898f-125">Tentokrát ikonu oznámení se zobrazí v oznamovací oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="f898f-125">This time, a notification icon is displayed in hello notification area.</span></span> <span data-ttu-id="f898f-126">Můžete otevřít hello oznámení nástroj drawer tooview hello textu v plném znění hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="f898f-126">You can open hello notification drawer tooview hello full text of hello notification.</span></span>
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
