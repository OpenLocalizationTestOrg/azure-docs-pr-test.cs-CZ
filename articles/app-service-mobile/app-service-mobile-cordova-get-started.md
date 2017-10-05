---
title: "Vytvoření aplikace Cordova v Azure App Service Mobile Apps | Dokumentace Microsoftu"
description: "V tomto kurzu začnete používat back-endy mobilních aplikací Azure pro vývoj na platformě Apache Cordova."
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
ms.openlocfilehash: b620465cdc3cfa04933dc6e70163fc32aa9a839b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-apache-cordova-app"></a><span data-ttu-id="0fd61-104">Vytvoření aplikace na platformě Apache Cordova</span><span class="sxs-lookup"><span data-stu-id="0fd61-104">Create an Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="0fd61-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="0fd61-105">Overview</span></span>
<span data-ttu-id="0fd61-106">V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu k mobilní aplikaci Apache Cordova pomocí back-endu mobilní aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="0fd61-106">This tutorial shows you how to add a cloud-based backend service to an Apache Cordova mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="0fd61-107">Vytvoříte jak nový back-end mobilní aplikace, tak jednoduchou aplikaci Apache Cordova, která bude představovat *seznam úkolů* a ukládat data do Azure.</span><span class="sxs-lookup"><span data-stu-id="0fd61-107">You create both a new mobile app backend and a simple *Todo list* Apache Cordova app that stores app data in Azure.</span></span>

<span data-ttu-id="0fd61-108">Dokončení tohoto kurzu se předpokládá ve všech dalších kurzech k používání funkce Mobile Apps v Azure App Service pro Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="0fd61-108">Completing this tutorial is a prerequisite for all other Apache Cordova tutorials about using the Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0fd61-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0fd61-109">Prerequisites</span></span>
<span data-ttu-id="0fd61-110">Pro absolvování tohoto kurzu musí být splněné následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="0fd61-110">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="0fd61-111">Počítač se sadou [Visual Studio Community 2017] nebo novější</span><span class="sxs-lookup"><span data-stu-id="0fd61-111">A PC with [Visual Studio Community 2017] or newer.</span></span>
* <span data-ttu-id="0fd61-112">[Visual Studio Tools for Apache Cordova]</span><span class="sxs-lookup"><span data-stu-id="0fd61-112">[Visual Studio Tools for Apache Cordova].</span></span>
* <span data-ttu-id="0fd61-113">[Aktivní účet Azure](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="0fd61-113">An [active Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="0fd61-114">Visual Studio je možné obejít a používat přímo příkazový řádek platformy Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="0fd61-114">You may also bypass Visual Studio and use the Apache Cordova command line directly.</span></span>  <span data-ttu-id="0fd61-115">Použití příkazového řádku je užitečné v případě, že kurz budete absolvovat na počítači Mac.</span><span class="sxs-lookup"><span data-stu-id="0fd61-115">Using the command line is useful when completing the tutorial on a Mac computer.</span></span>  <span data-ttu-id="0fd61-116">Tento kurz se nezabývá kompilací klientských aplikací Apache Cordova pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="0fd61-116">Compiling Apache Cordova client applications using the command line is not covered by this tutorial.</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="0fd61-117">Vytvoření back-endu mobilní aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="0fd61-117">Create an Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[<span data-ttu-id="0fd61-118">Podívejte se na video zobrazující podobný postup.</span><span class="sxs-lookup"><span data-stu-id="0fd61-118">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a><span data-ttu-id="0fd61-119">Konfigurace serverového projektu</span><span class="sxs-lookup"><span data-stu-id="0fd61-119">Configure the server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a><span data-ttu-id="0fd61-120">Stažení a spuštění aplikace Apache Cordova</span><span class="sxs-lookup"><span data-stu-id="0fd61-120">Download and run the Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a><span data-ttu-id="0fd61-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0fd61-121">Next Steps</span></span>
<span data-ttu-id="0fd61-122">Teď když jste dokončili tento kurz, jak rychle začít, přejděte k jednomu z následujících kurzů: </span><span class="sxs-lookup"><span data-stu-id="0fd61-122">Now that you completed this quick start tutorial, move on to one of the following tutorials:</span></span>

* <span data-ttu-id="0fd61-123">[Přidání offline dat](app-service-mobile-cordova-get-started-offline-data.md) do aplikace Apache Cordova</span><span class="sxs-lookup"><span data-stu-id="0fd61-123">[Add Offline Data](app-service-mobile-cordova-get-started-offline-data.md) to your Apache Cordova app.</span></span>
* <span data-ttu-id="0fd61-124">[Přidání ověřování](app-service-mobile-cordova-get-started-users.md) do aplikace Apache Cordova</span><span class="sxs-lookup"><span data-stu-id="0fd61-124">[Add Authentication](app-service-mobile-cordova-get-started-users.md) to your Apache Cordova app.</span></span>
* <span data-ttu-id="0fd61-125">[Přidání nabízených oznámení](app-service-mobile-cordova-get-started-push.md) do aplikace Apache Cordova</span><span class="sxs-lookup"><span data-stu-id="0fd61-125">[Add Push Notifications](app-service-mobile-cordova-get-started-push.md) to your Apache Cordova app.</span></span>

<span data-ttu-id="0fd61-126">Další informace o klíčových konceptech Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0fd61-126">Learn more about key concepts with Azure App Service.</span></span>

* <span data-ttu-id="0fd61-127">[Offline data]</span><span class="sxs-lookup"><span data-stu-id="0fd61-127">[Offline Data]</span></span>
* <span data-ttu-id="0fd61-128">[Ověřování]</span><span class="sxs-lookup"><span data-stu-id="0fd61-128">[Authentication]</span></span>
* <span data-ttu-id="0fd61-129">[Nabízená oznámení]</span><span class="sxs-lookup"><span data-stu-id="0fd61-129">[Push Notifications]</span></span>

<span data-ttu-id="0fd61-130">Zjistěte, jak používat sady SDK.</span><span class="sxs-lookup"><span data-stu-id="0fd61-130">Learn how to use the SDKs.</span></span>

* <span data-ttu-id="0fd61-131">[Apache Cordova SDK]</span><span class="sxs-lookup"><span data-stu-id="0fd61-131">[Apache Cordova SDK]</span></span>
* <span data-ttu-id="0fd61-132">[ASP.NET Server SDK]</span><span class="sxs-lookup"><span data-stu-id="0fd61-132">[ASP.NET Server SDK]</span></span>
* <span data-ttu-id="0fd61-133">[Node.js Server SDK]</span><span class="sxs-lookup"><span data-stu-id="0fd61-133">[Node.js Server SDK]</span></span>

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
<span data-ttu-id="0fd61-134">[Visual Studio Community 2017]: http://www.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="0fd61-134">[Visual Studio Community 2017]: http://www.visualstudio.com/</span></span>
<span data-ttu-id="0fd61-135">[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span><span class="sxs-lookup"><span data-stu-id="0fd61-135">[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span></span>
<span data-ttu-id="0fd61-136">[Offline data]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="0fd61-136">[Offline Data]: app-service-mobile-offline-data-sync.md</span></span>
<span data-ttu-id="0fd61-137">[Ověřování]: app-service-mobile-auth.md</span><span class="sxs-lookup"><span data-stu-id="0fd61-137">[Authentication]: app-service-mobile-auth.md</span></span>
<span data-ttu-id="0fd61-138">[Nabízená oznámení]: ../notification-hubs/notification-hubs-push-notification-overview.md</span><span class="sxs-lookup"><span data-stu-id="0fd61-138">[Push Notifications]: ../notification-hubs/notification-hubs-push-notification-overview.md</span></span>
<span data-ttu-id="0fd61-139">[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md</span><span class="sxs-lookup"><span data-stu-id="0fd61-139">[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md</span></span>
<span data-ttu-id="0fd61-140">[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md</span><span class="sxs-lookup"><span data-stu-id="0fd61-140">[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md</span></span>
<span data-ttu-id="0fd61-141">[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md</span><span class="sxs-lookup"><span data-stu-id="0fd61-141">[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md</span></span>
