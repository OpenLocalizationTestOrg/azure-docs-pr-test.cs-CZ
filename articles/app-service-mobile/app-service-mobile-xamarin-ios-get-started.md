---
title: "Začínáme s Azure App Service Mobile Apps pro aplikace na platformě Xamarin.iOS | Dokumentace Microsoftu"
description: "V tomto kurzu začnete používat Mobile Apps pro vývoj na platformě Xamarin.iOS."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: syntaxc4
ms.openlocfilehash: 8dc965df2cd45366970effb29f246b0045a94717
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-xamarinios-app"></a><span data-ttu-id="1d24c-103">Vytvoření aplikace Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="1d24c-103">Create a Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="1d24c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1d24c-104">Overview</span></span>
<span data-ttu-id="1d24c-105">V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace Xamarin.iOS pomocí back-endu mobilní aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="1d24c-105">This tutorial shows you how to add a cloud-based backend service to a Xamarin.iOS mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="1d24c-106">Vytvoříte nový back-end mobilní aplikace i jednoduchou aplikaci Xamarin.iOS, která bude představovat *seznam úkolů* a bude ukládat data do Azure.</span><span class="sxs-lookup"><span data-stu-id="1d24c-106">You create both a new mobile app backend and a simple *Todo list* Xamarin.iOS app that stores app data in Azure.</span></span>

<span data-ttu-id="1d24c-107">Dokončení tohoto kurzu se předpokládá ve všech dalších kurzech k používání funkce Mobile Apps v Azure App Service pro Xamarin.iOS.</span><span class="sxs-lookup"><span data-stu-id="1d24c-107">Completing this tutorial is a prerequisite for all other Xamarin.iOS tutorials about using the Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d24c-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1d24c-108">Prerequisites</span></span>
<span data-ttu-id="1d24c-109">Pro absolvování tohoto kurzu musí být splněné následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="1d24c-109">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="1d24c-110">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1d24c-110">An active Azure account.</span></span> <span data-ttu-id="1d24c-111">Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních aplikací, které můžete používat i po skončení zkušebního období.</span><span class="sxs-lookup"><span data-stu-id="1d24c-111">If you don't have an account, sign up for an Azure trial and get up to 10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="1d24c-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1d24c-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="1d24c-113">Visual Studio s Xamarinem.</span><span class="sxs-lookup"><span data-stu-id="1d24c-113">Visual Studio with Xamarin.</span></span> <span data-ttu-id="1d24c-114">Pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="1d24c-114">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) for instructions.</span></span>
* <span data-ttu-id="1d24c-115">Počítač Mac s nainstalovaným Xcode verze 7.0 nebo novějším a Xamarin Studio Community.</span><span class="sxs-lookup"><span data-stu-id="1d24c-115">A Mac with Xcode v7.0 or later and Xamarin Studio Community installed.</span></span> <span data-ttu-id="1d24c-116">Přečtěte si témata o [nastavení a instalaci nástrojů Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) a o [nastavení, instalaci a ověření pro uživatele počítačů Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span><span class="sxs-lookup"><span data-stu-id="1d24c-116">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) and [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="1d24c-117">Vytvoření back-endu mobilní aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="1d24c-117">Create an Azure Mobile App backend</span></span>
<span data-ttu-id="1d24c-118">Podle těchto pokynů vytvořte back-end mobilní aplikace:</span><span class="sxs-lookup"><span data-stu-id="1d24c-118">Follow these steps to create a Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a><span data-ttu-id="1d24c-119">Konfigurace serverového projektu</span><span class="sxs-lookup"><span data-stu-id="1d24c-119">Configure the server project</span></span>
<span data-ttu-id="1d24c-120">Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích.</span><span class="sxs-lookup"><span data-stu-id="1d24c-120">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="1d24c-121">Dále si stáhněte serverový projekt pro jednoduchý back-end seznamu úkolů a publikujete ho v Azure.</span><span class="sxs-lookup"><span data-stu-id="1d24c-121">Next, download a server project for a simple "todo list" backend and publish it to Azure.</span></span>

<span data-ttu-id="1d24c-122">Podle následujících kroků nakonfigurujte serverový projekt tak, aby používal buď back-end Node.js, nebo .NET.</span><span class="sxs-lookup"><span data-stu-id="1d24c-122">Follow the following steps to configure the server project to use either the Node.js or .NET backend.</span></span>

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a><span data-ttu-id="1d24c-123">Stáhnutí a spuštění aplikace Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="1d24c-123">Download and run the Xamarin.iOS app</span></span>
1. <span data-ttu-id="1d24c-124">Otevřete v okně prohlížeče [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="1d24c-124">Open the [Azure portal] in a browser window.</span></span>
2. <span data-ttu-id="1d24c-125">V okně nastavení mobilní aplikace klikněte na **Začínáme** > **Xamarin.iOS**.</span><span class="sxs-lookup"><span data-stu-id="1d24c-125">On the settings blade for your Mobile App, click **Get Started** > **Xamarin.iOS**.</span></span> <span data-ttu-id="1d24c-126">V kroku 3 klikněte na možnost **Vytvořit novou aplikaci**, pokud ještě nebyla vybrána.</span><span class="sxs-lookup"><span data-stu-id="1d24c-126">Under step 3, click **Create a new app** if it's not already selected.</span></span>  <span data-ttu-id="1d24c-127">Pak klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="1d24c-127">Next click the **Download** button.</span></span>

      <span data-ttu-id="1d24c-128">Stáhne se klientská aplikace, která se připojí k mobilnímu back-endu.</span><span class="sxs-lookup"><span data-stu-id="1d24c-128">A client application that connects to your mobile backend is downloaded.</span></span> <span data-ttu-id="1d24c-129">Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.</span><span class="sxs-lookup"><span data-stu-id="1d24c-129">Save the compressed project file to your local computer, and make a note of where you save it.</span></span>
3. <span data-ttu-id="1d24c-130">Extrahujte projekt, který jste stáhli, a otevřete jej v nástroji Xamarin Studio (nebo v nástroji Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="1d24c-130">Extract the project that you downloaded, and then open it in Xamarin Studio (or Visual Studio).</span></span>

    ![][9]

    ![][8]
4. <span data-ttu-id="1d24c-131">Stiskněte klávesu F5, aby se projekt sestavil a aplikace se spustila v emulátoru iPhonu.</span><span class="sxs-lookup"><span data-stu-id="1d24c-131">Press the F5 key to build the project and start the app in the iPhone emulator.</span></span>
5. <span data-ttu-id="1d24c-132">Zadejte do aplikace smysluplný text, například *Naučit se Xamarin*, a klikněte na tlačítko **+**.</span><span class="sxs-lookup"><span data-stu-id="1d24c-132">In the app, type meaningful text, such as *Learn Xamarin*, and then click the **+** button.</span></span>

    ![][10]

    <span data-ttu-id="1d24c-133">Data z požadavku se vloží do tabulky TodoItem.</span><span class="sxs-lookup"><span data-stu-id="1d24c-133">Data from the request is inserted into the TodoItem table.</span></span> <span data-ttu-id="1d24c-134">Položky uložené v tabulce se vrátí back-endu mobilní aplikace a v seznamu se zobrazí data.</span><span class="sxs-lookup"><span data-stu-id="1d24c-134">Items stored in the table are returned by the mobile app backend, and the data is displayed in the list.</span></span>

> [!NOTE]
> <span data-ttu-id="1d24c-135">Na kód, který přistupuje k back-endu mobilní aplikace pro dotazování a vkládání dat, se můžete podívat v souboru C# QSTodoService.cs.</span><span class="sxs-lookup"><span data-stu-id="1d24c-135">You can review the code that accesses your mobile app backend to query and insert data in the QSTodoService.cs C# file.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="1d24c-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d24c-136">Next steps</span></span>
* [<span data-ttu-id="1d24c-137">Přidání offline synchronizace do aplikace</span><span class="sxs-lookup"><span data-stu-id="1d24c-137">Add Offline Sync to your app</span></span>](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [<span data-ttu-id="1d24c-138">Přidání ověřování do aplikace</span><span class="sxs-lookup"><span data-stu-id="1d24c-138">Add authentication to your app </span></span>](app-service-mobile-xamarin-ios-get-started-users.md)
* [<span data-ttu-id="1d24c-139">Přidání nabízených oznámení do aplikace Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="1d24c-139">Add push notifications to your Xamarin.Android app</span></span>](app-service-mobile-xamarin-ios-get-started-push.md)
* [<span data-ttu-id="1d24c-140">Jak používat spravovaného klienta pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="1d24c-140">How to use the managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
<span data-ttu-id="1d24c-141">[Azure Portal]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="1d24c-141">[Azure portal]: https://portal.azure.com/</span></span>
