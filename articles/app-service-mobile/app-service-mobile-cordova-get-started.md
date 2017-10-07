---
title: aaaCreate aplikace Cordova v Azure App Service Mobile Apps | Microsoft Docs
description: "Postupujte podle tohoto kurzu tooget začít s pomocí back-EndY mobilní aplikace Azure pro vývoj pro Apache Cordova"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
tags: 
keywords: "cordova, javascript, mobilní, klient"
ms.assetid: 0b08fc12-0a80-42d3-9cc1-9b3f8d3e3a3f
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: hero-article
ms.date: 07/07/2017
ms.author: glenga
ms.openlocfilehash: 4981cbc0686c15230019ac9f30495f30cbf2c791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-cordova-app"></a>Vytvoření aplikace na platformě Apache Cordova
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak tooadd back-end cloudové služby mobilní aplikace Apache Cordova tooan pomocí back-end mobilní aplikace Azure.  Vytvoříte jak nový back-end mobilní aplikace, tak jednoduchou aplikaci Apache Cordova, která bude představovat *seznam úkolů* a ukládat data do Azure.

Dokončení tohoto kurzu je předpokladem pro Apache Cordova o všech dalších kurzech k používání funkce Mobile Apps hello v Azure App Service.

## <a name="prerequisites"></a>Požadavky
toocomplete tohoto kurzu budete potřebovat hello následující požadavky:

* Počítač se sadou [Visual Studio Community 2017] nebo novější
* [Visual Studio Tools for Apache Cordova]
* [Aktivní účet Azure](https://azure.microsoft.com/pricing/free-trial/)

Také můžete vynechat Visual Studio a používat přímo hello Apache Cordova příkazového řádku.  Pomocí příkazového řádku hello je užitečné, když dokončení kurzu hello na počítači Mac.  Kompilování klientských aplikací Apache Cordova pomocí příkazového řádku hello není předmětem tohoto kurzu.

## <a name="create-an-azure-mobile-app-backend"></a>Vytvoření back-endu mobilní aplikace Azure
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Podívejte se na video zobrazující podobný postup.](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-hello-server-project"></a>Konfigurace projektu server hello
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-apache-cordova-app"></a>Stažení a spuštění aplikace Apache Cordova hello
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Další kroky
Teď, když jste dokončili tento úvodní kurz, přesunete na tooone hello následující kurzy:

* [Přidání dat do offline režimu](app-service-mobile-cordova-get-started-offline-data.md) tooyour aplikace Apache Cordova.
* [Přidání ověřování](app-service-mobile-cordova-get-started-users.md) tooyour aplikace Apache Cordova.
* [Přidat nabízená oznámení](app-service-mobile-cordova-get-started-push.md) tooyour aplikace Apache Cordova.

Další informace o klíčových konceptech Azure App Service

* [Offline data]
* [Ověřování]
* [Nabízená oznámení]

Zjistěte, jak toouse hello sady SDK.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2017]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Offline data]: app-service-mobile-offline-data-sync.md
[Ověřování]: app-service-mobile-auth.md
[Nabízená oznámení]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
