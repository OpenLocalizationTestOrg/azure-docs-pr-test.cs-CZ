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
# <a name="add-push-notifications-tooyour-xamarinandroid-app"></a>Přidat nabízená oznámení tooyour Xamarin.Android aplikaci
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Přehled
V tomto kurzu přidáte nabízená oznámení toohello [Xamarin.Android úvodní](app-service-mobile-windows-store-dotnet-get-started.md) projektu tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.

Pokud nepoužijete hello stáhli úvodní serverový projekt, bude nutné hello nabízených oznámení v balíčku rozšíření. V tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Další informace.

## <a name="prerequisites"></a>Požadavky
Tento kurz vyžaduje hello následující:

* Aktivní účet Google. Můžete si zaregistrovat účet Google na webu [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
* [Komponenta klienta zasílání zpráv cloudu Google](http://components.xamarin.com/view/GCMClient/).

## <a name="configure-hub"></a>Konfigurace centra oznámení
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Povolit Firebase cloudu zasílání zpráv
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-toosend-push-requests"></a>Konfigurace Azure toosend nabízené požadavků
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a id="update-server"></a>Aktualizovat hello serveru projektu toosend nabízená oznámení
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>Konfigurace projektu hello klienta pro nabízená oznámení
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>Přidat nabízená oznámení kód tooyour aplikaci
[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Nabízená oznámení v aplikaci
Hello aplikaci můžete otestovat pomocí virtuálního zařízení v emulátoru hello. Existují další konfigurační kroky potřebné při spuštění v emulátoru.

1. Ujistěte se, že nasazujete tooor ladění na virtuální zařízení, které se má nastavit jako cíl hello rozhraní Google API, jak je vidět níže ve Správci hello virtuální zařízení Android (AVD).
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)
2. Kliknutím na Přidat zařízení Android toohello účet Google **aplikace** > **nastavení** > **přidejte účet**, postupujte podle pokynů hello.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)
3. Spusťte aplikaci seznamu úkolů hello jako před a vložit novou položku úkolů. Tentokrát ikonu oznámení se zobrazí v oznamovací oblasti hello. Můžete otevřít hello oznámení nástroj drawer tooview hello textu v plném znění hello oznámení.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
