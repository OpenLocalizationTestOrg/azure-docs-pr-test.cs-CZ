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
# <a name="add-push-notifications-to-your-android-app"></a>Přidání nabízených oznámení do vaší aplikace pro Android
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Přehled
V tomto kurzu přidáte nabízená oznámení [Android úvodní] projektu tak, aby nabízených oznámení se odešle do zařízení pokaždé, když vložení záznamu.

Pokud použijete serverový projekt stažené rychlý start, je třeba balíček rozšíření nabízená oznámení. Další informace najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Požadavky
Budete potřebovat následující:

* IDE, v závislosti na back-end vašeho projektu:

  * [Android Studio](https://developer.android.com/sdk/index.html) Pokud tato aplikace nemá back-end Node.js.
  * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) nebo novější, pokud tato aplikace nemá rozhraní Microsoft .NET back-end.
* Android 2.3 nebo novější, Google úložiště revize 27 nebo novější a služby Google Play 9.0.2 nebo novější pro zasílání zpráv cloudu Firebase.
* Dokončení [Android úvodní].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Vytvoření projektu, který podporuje službu Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Konfigurace centra oznámení
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurace Azure k odesílání nabízených oznámení
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Povolte nabízená oznámení pro serverový projekt
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Přidání nabízených oznámení do aplikace
V této části že aktualizujete aplikaci Android klienta ke zpracování nabízených oznámení.

### <a name="verify-android-sdk-version"></a>Ověřit verzi sady SDK pro Android
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Dalším krokem je instalace služby Google Play. Google Cloud Messaging má několik minimální rozhraní API úrovně požadavků pro vývoj a testování, která **minSdkVersion** musí odpovídat vlastnosti v manifestu.

Pokud testujete s starší zařízením, projděte si [nastavit až Google Play Services SDK] určit, jak nedostatek můžete nastavit hodnotu a správně nastavené.

### <a name="add-google-play-services-to-the-project"></a>Přidejte do projektu služby Google Play
[!INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Přidání kódu
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Testování aplikací proti publikované mobilní služby
Aplikaci můžete otestovat připojení přímo telefon se systémem Android pomocí kabelu USB, nebo použití virtuálního zařízení v emulátoru.

## <a name="next-steps"></a>Další kroky
Teď, když jste dokončili tento kurz, vezměte v úvahu pokračovat na jednu z následujících kurzů:

* [Přidání ověřování do aplikace pro Android](app-service-mobile-android-get-started-users.md).
  Postup přidání ověřování do seznamu úkolů projektu pro rychlý start v systému Android pomocí zprostředkovatele identity podporované.
* [Zapnutí offline synchronizace pro svoji aplikaci pro Android](app-service-mobile-android-get-started-offline-data.md).
  Zjistěte, jak přidat podporu offline režimu do aplikace pomocí back-end mobilní aplikace. S offline synchronizací, mohou uživatelé komunikovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.

<!-- URLs -->
[Android úvodní]: app-service-mobile-android-get-started.md

[nastavit až Google Play Services SDK]:https://developers.google.com/android/guides/setup
