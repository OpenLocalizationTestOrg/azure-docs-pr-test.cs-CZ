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
# <a name="add-push-notifications-tooyour-android-app"></a>Přidat nabízená oznámení tooyour aplikace Android
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Přehled
V tomto kurzu přidáte nabízená oznámení toohello [Android úvodní] projektu tak, aby nabízených oznámení je odesláno toohello zařízení pokaždé, když vložení záznamu.

Pokud nepoužijete hello stáhli úvodní serverový projekt, třeba hello nabízených oznámení v balíčku rozšíření. Další informace najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Požadavky
Je třeba hello následující:

* IDE, v závislosti na back-end vašeho projektu:

  * [Android Studio](https://developer.android.com/sdk/index.html) Pokud tato aplikace nemá back-end Node.js.
  * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) nebo novější, pokud tato aplikace nemá rozhraní Microsoft .NET back-end.
* Android 2.3 nebo novější, Google úložiště revize 27 nebo novější a služby Google Play 9.0.2 nebo novější pro zasílání zpráv cloudu Firebase.
* Dokončení hello [Android úvodní].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Vytvoření projektu, který podporuje službu Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Konfigurace centra oznámení
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-toosend-push-notifications"></a>Konfigurace Azure toosend nabízená oznámení
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-hello-server-project"></a>Povolte nabízená oznámení pro projekt server hello
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-tooyour-app"></a>Přidat nabízená oznámení tooyour aplikaci
V této části aktualizace klienta aplikace pro Android toohandle nabízených oznámení.

### <a name="verify-android-sdk-version"></a>Ověřit verzi sady SDK pro Android
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Dalším krokem je tooinstall služby Google Play. Google Cloud Messaging má několik minimální rozhraní API úrovně požadavků pro vývoj a testování, které hello **minSdkVersion** musí odpovídat vlastnosti v manifestu hello.

Pokud testujete s starší zařízením, projděte si [nastavit až Google Play Services SDK] toodetermine jak nízkou můžete nastavit tuto hodnotu a správně nastavené.

### <a name="add-google-play-services-toohello-project"></a>Přidání projektu toohello služby Google Play
[!INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Přidání kódu
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-hello-app-against-hello-published-mobile-service"></a>Testování aplikace hello proti hello publikovaná mobilní služby
Hello aplikaci můžete otestovat připojení přímo telefon se systémem Android pomocí kabelu USB, nebo použití virtuálního zařízení v emulátoru hello.

## <a name="next-steps"></a>Další kroky
Teď, když jste dokončili tento kurz, vezměte v úvahu pokračovat na tooone hello následující kurzy:

* [Přidat aplikaci pro Android tooyour ověřování](app-service-mobile-android-get-started-users.md).
  Zjistěte, jak rychlý start tooadd ověřování toohello seznamu úkolů projektu v systému Android pomocí zprostředkovatele identity podporované.
* [Zapnutí offline synchronizace pro svoji aplikaci pro Android](app-service-mobile-android-get-started-offline-data.md).
  Zjistěte, jak tooadd offline podporovat tooyour aplikaci pomocí back-end mobilní aplikace. S offline synchronizací, mohou uživatelé komunikovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.

<!-- URLs -->
[Android úvodní]: app-service-mobile-android-get-started.md

[nastavit až Google Play Services SDK]:https://developers.google.com/android/guides/setup
