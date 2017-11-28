---
title: "aaaGet začít s Azure App Service Mobile Apps pro aplikace pro Xamarin.iOS | Microsoft Docs"
description: "Využijte tento kurz tooget začít s pomocí Mobile Apps pro vývoj na platformě Xamarin.iOS."
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
ms.openlocfilehash: 524c5ac4d8a29d7cb858f74132aad5d6e2201d02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinios-app"></a><span data-ttu-id="f0e9f-103">Vytvoření aplikace Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="f0e9f-103">Create a Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="f0e9f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f0e9f-104">Overview</span></span>
<span data-ttu-id="f0e9f-105">Tento kurz ukazuje, jak tooadd back-end cloudové služby mobilní aplikace Xamarin.iOS tooa pomocí back-end mobilní aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-105">This tutorial shows you how tooadd a cloud-based backend service tooa Xamarin.iOS mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="f0e9f-106">Vytvoříte nový back-end mobilní aplikace i jednoduchou aplikaci Xamarin.iOS, která bude představovat *seznam úkolů* a bude ukládat data do Azure.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-106">You create both a new mobile app backend and a simple *Todo list* Xamarin.iOS app that stores app data in Azure.</span></span>

<span data-ttu-id="f0e9f-107">Dokončení tohoto kurzu je předpokladem pro Xamarin.iOS o všech dalších kurzech k používání funkce Mobile Apps hello v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-107">Completing this tutorial is a prerequisite for all other Xamarin.iOS tutorials about using hello Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0e9f-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f0e9f-108">Prerequisites</span></span>
<span data-ttu-id="f0e9f-109">toocomplete tohoto kurzu budete potřebovat hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="f0e9f-109">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="f0e9f-110">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-110">An active Azure account.</span></span> <span data-ttu-id="f0e9f-111">Pokud nemáte účet, zaregistrovat zkušební verzi Azure a zprovoznění too10 bezplatné mobilní aplikace, které můžete používat i po skončení zkušebního období.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-111">If you don't have an account, sign up for an Azure trial and get up too10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="f0e9f-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f0e9f-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="f0e9f-113">Visual Studio s Xamarinem.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-113">Visual Studio with Xamarin.</span></span> <span data-ttu-id="f0e9f-114">Pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0e9f-114">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) for instructions.</span></span>
* <span data-ttu-id="f0e9f-115">Počítač Mac s nainstalovaným Xcode verze 7.0 nebo novějším a Xamarin Studio Community.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-115">A Mac with Xcode v7.0 or later and Xamarin Studio Community installed.</span></span> <span data-ttu-id="f0e9f-116">Přečtěte si témata o [nastavení a instalaci nástrojů Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) a o [nastavení, instalaci a ověření pro uživatele počítačů Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span><span class="sxs-lookup"><span data-stu-id="f0e9f-116">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) and [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="f0e9f-117">Vytvoření back-endu mobilní aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="f0e9f-117">Create an Azure Mobile App backend</span></span>
<span data-ttu-id="f0e9f-118">Postupujte podle těchto kroků toocreate back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-118">Follow these steps toocreate a Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-hello-server-project"></a><span data-ttu-id="f0e9f-119">Konfigurace projektu server hello</span><span class="sxs-lookup"><span data-stu-id="f0e9f-119">Configure hello server project</span></span>
<span data-ttu-id="f0e9f-120">Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-120">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="f0e9f-121">V dalším kroku stáhnout serverový projekt pro jednoduchý "seznam úkolů" back-end a publikujete ho v tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-121">Next, download a server project for a simple "todo list" backend and publish it tooAzure.</span></span>

<span data-ttu-id="f0e9f-122">Postupujte podle následujících kroků tooconfigure hello serveru projektu toouse hello buď hello Node.js, nebo .NET back-end.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-122">Follow hello following steps tooconfigure hello server project toouse either hello Node.js or .NET backend.</span></span>

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinios-app"></a><span data-ttu-id="f0e9f-123">Stažení a spuštění aplikace Xamarin.iOS hello</span><span class="sxs-lookup"><span data-stu-id="f0e9f-123">Download and run hello Xamarin.iOS app</span></span>
1. <span data-ttu-id="f0e9f-124">Otevřete hello [portál Azure] v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-124">Open hello [Azure portal] in a browser window.</span></span>
2. <span data-ttu-id="f0e9f-125">V okně Nastavení hello mobilní aplikace, klikněte na tlačítko **Začínáme** > **Xamarin.iOS**.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-125">On hello settings blade for your Mobile App, click **Get Started** > **Xamarin.iOS**.</span></span> <span data-ttu-id="f0e9f-126">V kroku 3 klikněte na možnost **Vytvořit novou aplikaci**, pokud ještě nebyla vybrána.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-126">Under step 3, click **Create a new app** if it's not already selected.</span></span>  <span data-ttu-id="f0e9f-127">Pak klikněte na hello **Stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-127">Next click hello **Download** button.</span></span>

      <span data-ttu-id="f0e9f-128">Je Stáhnout klientskou aplikaci, která se připojuje tooyour mobilního back-endu.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-128">A client application that connects tooyour mobile backend is downloaded.</span></span> <span data-ttu-id="f0e9f-129">Uložte hello komprimovaný soubor projektu do místního počítače a poznamenejte si kam jste jej uložili.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-129">Save hello compressed project file to your local computer, and make a note of where you save it.</span></span>
3. <span data-ttu-id="f0e9f-130">Extrahujte hello projekt, který jste stáhli a otevřete jej v nástroji Xamarin Studio (nebo Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="f0e9f-130">Extract hello project that you downloaded, and then open it in Xamarin Studio (or Visual Studio).</span></span>

    ![][9]

    ![][8]
4. <span data-ttu-id="f0e9f-131">Stiskněte klávesu hello F5 klíče toobuild hello projektu a spusťte hello aplikaci v emulátoru Iphonu hello.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-131">Press hello F5 key toobuild hello project and start hello app in hello iPhone emulator.</span></span>
5. <span data-ttu-id="f0e9f-132">Hello aplikace zadejte smysluplný text, například *naučit se Xamarin*a potom klikněte na hello  **+**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-132">In hello app, type meaningful text, such as *Learn Xamarin*, and then click hello **+** button.</span></span>

    ![][10]

    <span data-ttu-id="f0e9f-133">Data z požadavku hello je vloženy do tabulky TodoItem hello.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-133">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="f0e9f-134">Položky uložené v tabulce hello se vrátí pomocí back-end mobilní aplikace hello a data se zobrazí v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-134">Items stored in hello table are returned by hello mobile app backend, and the data is displayed in hello list.</span></span>

> [!NOTE]
> <span data-ttu-id="f0e9f-135">Můžete zkontrolovat hello kód, který má přístup k vaší tooquery back-end mobilní aplikace a vkládání dat v souboru C# QSTodoService.cs hello.</span><span class="sxs-lookup"><span data-stu-id="f0e9f-135">You can review hello code that accesses your mobile app backend tooquery and insert data in hello QSTodoService.cs C# file.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="f0e9f-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0e9f-136">Next steps</span></span>
* [<span data-ttu-id="f0e9f-137">Přidat aplikaci tooyour Offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="f0e9f-137">Add Offline Sync tooyour app</span></span>](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [<span data-ttu-id="f0e9f-138">Přidat aplikaci tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="f0e9f-138">Add authentication tooyour app </span></span>](app-service-mobile-xamarin-ios-get-started-users.md)
* [<span data-ttu-id="f0e9f-139">Přidat nabízená oznámení tooyour Xamarin.Android aplikaci</span><span class="sxs-lookup"><span data-stu-id="f0e9f-139">Add push notifications tooyour Xamarin.Android app</span></span>](app-service-mobile-xamarin-ios-get-started-push.md)
* [<span data-ttu-id="f0e9f-140">Jak toouse hello spravovat klienta pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="f0e9f-140">How toouse hello managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)

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
[portál Azure]: https://portal.azure.com/
