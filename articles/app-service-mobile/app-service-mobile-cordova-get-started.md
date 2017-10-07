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
# <a name="create-an-apache-cordova-app"></a><span data-ttu-id="b70ec-104">Vytvoření aplikace na platformě Apache Cordova</span><span class="sxs-lookup"><span data-stu-id="b70ec-104">Create an Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="b70ec-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="b70ec-105">Overview</span></span>
<span data-ttu-id="b70ec-106">Tento kurz ukazuje, jak tooadd back-end cloudové služby mobilní aplikace Apache Cordova tooan pomocí back-end mobilní aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="b70ec-106">This tutorial shows you how tooadd a cloud-based backend service tooan Apache Cordova mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="b70ec-107">Vytvoříte jak nový back-end mobilní aplikace, tak jednoduchou aplikaci Apache Cordova, která bude představovat *seznam úkolů* a ukládat data do Azure.</span><span class="sxs-lookup"><span data-stu-id="b70ec-107">You create both a new mobile app backend and a simple *Todo list* Apache Cordova app that stores app data in Azure.</span></span>

<span data-ttu-id="b70ec-108">Dokončení tohoto kurzu je předpokladem pro Apache Cordova o všech dalších kurzech k používání funkce Mobile Apps hello v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b70ec-108">Completing this tutorial is a prerequisite for all other Apache Cordova tutorials about using hello Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b70ec-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b70ec-109">Prerequisites</span></span>
<span data-ttu-id="b70ec-110">toocomplete tohoto kurzu budete potřebovat hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="b70ec-110">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="b70ec-111">Počítač se sadou [Visual Studio Community 2017] nebo novější</span><span class="sxs-lookup"><span data-stu-id="b70ec-111">A PC with [Visual Studio Community 2017] or newer.</span></span>
* <span data-ttu-id="b70ec-112">[Visual Studio Tools for Apache Cordova]</span><span class="sxs-lookup"><span data-stu-id="b70ec-112">[Visual Studio Tools for Apache Cordova].</span></span>
* <span data-ttu-id="b70ec-113">[Aktivní účet Azure](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="b70ec-113">An [active Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="b70ec-114">Také můžete vynechat Visual Studio a používat přímo hello Apache Cordova příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="b70ec-114">You may also bypass Visual Studio and use hello Apache Cordova command line directly.</span></span>  <span data-ttu-id="b70ec-115">Pomocí příkazového řádku hello je užitečné, když dokončení kurzu hello na počítači Mac.</span><span class="sxs-lookup"><span data-stu-id="b70ec-115">Using hello command line is useful when completing hello tutorial on a Mac computer.</span></span>  <span data-ttu-id="b70ec-116">Kompilování klientských aplikací Apache Cordova pomocí příkazového řádku hello není předmětem tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b70ec-116">Compiling Apache Cordova client applications using hello command line is not covered by this tutorial.</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="b70ec-117">Vytvoření back-endu mobilní aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="b70ec-117">Create an Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[<span data-ttu-id="b70ec-118">Podívejte se na video zobrazující podobný postup.</span><span class="sxs-lookup"><span data-stu-id="b70ec-118">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-hello-server-project"></a><span data-ttu-id="b70ec-119">Konfigurace projektu server hello</span><span class="sxs-lookup"><span data-stu-id="b70ec-119">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-apache-cordova-app"></a><span data-ttu-id="b70ec-120">Stažení a spuštění aplikace Apache Cordova hello</span><span class="sxs-lookup"><span data-stu-id="b70ec-120">Download and run hello Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a><span data-ttu-id="b70ec-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b70ec-121">Next Steps</span></span>
<span data-ttu-id="b70ec-122">Teď, když jste dokončili tento úvodní kurz, přesunete na tooone hello následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="b70ec-122">Now that you completed this quick start tutorial, move on tooone of hello following tutorials:</span></span>

* <span data-ttu-id="b70ec-123">[Přidání dat do offline režimu](app-service-mobile-cordova-get-started-offline-data.md) tooyour aplikace Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="b70ec-123">[Add Offline Data](app-service-mobile-cordova-get-started-offline-data.md) tooyour Apache Cordova app.</span></span>
* <span data-ttu-id="b70ec-124">[Přidání ověřování](app-service-mobile-cordova-get-started-users.md) tooyour aplikace Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="b70ec-124">[Add Authentication](app-service-mobile-cordova-get-started-users.md) tooyour Apache Cordova app.</span></span>
* <span data-ttu-id="b70ec-125">[Přidat nabízená oznámení](app-service-mobile-cordova-get-started-push.md) tooyour aplikace Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="b70ec-125">[Add Push Notifications](app-service-mobile-cordova-get-started-push.md) tooyour Apache Cordova app.</span></span>

<span data-ttu-id="b70ec-126">Další informace o klíčových konceptech Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b70ec-126">Learn more about key concepts with Azure App Service.</span></span>

* <span data-ttu-id="b70ec-127">[Offline data]</span><span class="sxs-lookup"><span data-stu-id="b70ec-127">[Offline Data]</span></span>
* <span data-ttu-id="b70ec-128">[Ověřování]</span><span class="sxs-lookup"><span data-stu-id="b70ec-128">[Authentication]</span></span>
* <span data-ttu-id="b70ec-129">[Nabízená oznámení]</span><span class="sxs-lookup"><span data-stu-id="b70ec-129">[Push Notifications]</span></span>

<span data-ttu-id="b70ec-130">Zjistěte, jak toouse hello sady SDK.</span><span class="sxs-lookup"><span data-stu-id="b70ec-130">Learn how toouse hello SDKs.</span></span>

* <span data-ttu-id="b70ec-131">[Apache Cordova SDK]</span><span class="sxs-lookup"><span data-stu-id="b70ec-131">[Apache Cordova SDK]</span></span>
* <span data-ttu-id="b70ec-132">[ASP.NET Server SDK]</span><span class="sxs-lookup"><span data-stu-id="b70ec-132">[ASP.NET Server SDK]</span></span>
* <span data-ttu-id="b70ec-133">[Node.js Server SDK]</span><span class="sxs-lookup"><span data-stu-id="b70ec-133">[Node.js Server SDK]</span></span>

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
