---
title: aaaCreate aplikace pro Android v Azure App Service Mobile Apps | Microsoft Docs
description: "Postupujte podle tohoto kurzu tooget začít s pomocí back-EndY mobilní aplikace Azure pro vývoj pro Android"
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34a9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 0af85a3a4de9fc265976bbe3f34d73effc3807df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-android-app"></a><span data-ttu-id="7e455-103">Vytvoření aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="7e455-103">Create an Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="7e455-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="7e455-104">Overview</span></span>
<span data-ttu-id="7e455-105">Tento kurz ukazuje, jak tooadd back-end cloudové služby tooan mobilních aplikací systému Android pomocí back-end mobilní aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="7e455-105">This tutorial shows you how tooadd a cloud-based backend service tooan Android mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="7e455-106">Vytvoříte jak nový back-end mobilní aplikace, tak jednoduchou aplikaci pro Android, která bude představovat *seznam úkolů* a ukládat data do Azure.</span><span class="sxs-lookup"><span data-stu-id="7e455-106">You will create both a new mobile app backend and a simple *Todo list* Android app that stores app data in Azure.</span></span>

<span data-ttu-id="7e455-107">Dokončení tohoto kurzu je předpokladem pro všechny ostatní Android kurzech k používání funkce Mobile Apps hello v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="7e455-107">Completing this tutorial is a prerequisite for all other Android tutorials about using hello Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e455-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7e455-108">Prerequisites</span></span>
<span data-ttu-id="7e455-109">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="7e455-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="7e455-110">[Android Developer Tools](https://developer.android.com/sdk/index.html), které zahrnují integrované vývojové prostředí Android Studio hello a nejnovější platformu Android hello.</span><span class="sxs-lookup"><span data-stu-id="7e455-110">[Android Developer Tools](https://developer.android.com/sdk/index.html), which includes hello Android Studio integrated development environment, and hello latest Android platform.</span></span>
* <span data-ttu-id="7e455-111">Azure Mobile Android SDK, která je automaticky odkazuje jako na součást projektu pro rychlý start hello, který si stáhnete.</span><span class="sxs-lookup"><span data-stu-id="7e455-111">Azure Mobile Android SDK, which is automatically referenced as part of hello quickstart project you download.</span></span>
* <span data-ttu-id="7e455-112">[Aktivní účet Azure](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="7e455-112">An [active Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="create-a-new-azure-mobile-app-backend"></a><span data-ttu-id="7e455-113">Vytvoření nového back-endu mobilní aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="7e455-113">Create a new Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-hello-server-project"></a><span data-ttu-id="7e455-114">Konfigurace projektu server hello</span><span class="sxs-lookup"><span data-stu-id="7e455-114">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-android-app"></a><span data-ttu-id="7e455-115">Stáhněte a spusťte aplikaci pro Android hello</span><span class="sxs-lookup"><span data-stu-id="7e455-115">Download and run hello Android app</span></span>
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
