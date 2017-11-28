---
title: "aaaGet Začínáme s Azure Mobile Apps pro aplikace pro Xamarin.Android"
description: "Postupujte podle tohoto kurzu tooget spuštění pomocí Azure Mobile Apps pro vývoj Xamarin Android"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 38710660d9328fe3c068efca972f76aa8b6e049b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinandroid-app"></a><span data-ttu-id="d8930-103">Vytvoření aplikace Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="d8930-103">Create a Xamarin.Android App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="d8930-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="d8930-104">Overview</span></span>
<span data-ttu-id="d8930-105">Tento kurz ukazuje, jak tooadd back-end cloudové služby tooa aplikace Xamarin.Android.</span><span class="sxs-lookup"><span data-stu-id="d8930-105">This tutorial shows you how tooadd a cloud-based backend service tooa Xamarin.Android app.</span></span> <span data-ttu-id="d8930-106">Další informace najdete v tématu [Co jsou Mobile Apps](app-service-mobile-value-prop.md).</span><span class="sxs-lookup"><span data-stu-id="d8930-106">For more information, see [What are Mobile Apps](app-service-mobile-value-prop.md).</span></span>

<span data-ttu-id="d8930-107">Zde je snímek obrazovky aplikace hello dokončena:</span><span class="sxs-lookup"><span data-stu-id="d8930-107">A screenshot from hello completed app is below:</span></span>

![][0]

<span data-ttu-id="d8930-108">Ve všech dalších kurzech k používání funkce Mobile Apps pro Xamarin.Android se předpokládá dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d8930-108">Completing this tutorial is a prerequisite for all other Mobile Apps tutorials for Xamarin.Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8930-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d8930-109">Prerequisites</span></span>
<span data-ttu-id="d8930-110">toocomplete tohoto kurzu budete potřebovat hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="d8930-110">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="d8930-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d8930-111">An active Azure account.</span></span> <span data-ttu-id="d8930-112">Pokud nemáte účet, zaregistrovat zkušební verzi Azure a zprovoznění too10 bezplatných mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="d8930-112">If you don't have an account, sign up for an Azure trial and get up too10 free Mobile Apps.</span></span> <span data-ttu-id="d8930-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d8930-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="d8930-114">Visual Studio s Xamarinem.</span><span class="sxs-lookup"><span data-stu-id="d8930-114">Visual Studio with Xamarin.</span></span> <span data-ttu-id="d8930-115">Pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8930-115">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) for instructions.</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="d8930-116">Vytvoření back-endu mobilní aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="d8930-116">Create an Azure Mobile App backend</span></span>
<span data-ttu-id="d8930-117">Postupujte podle těchto kroků toocreate back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8930-117">Follow these steps toocreate a Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

<span data-ttu-id="d8930-118">Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích.</span><span class="sxs-lookup"><span data-stu-id="d8930-118">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="d8930-119">V dalším kroku stáhnout serverový projekt pro jednoduchý "seznam úkolů" back-end a publikujete ho v tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d8930-119">Next, download a server project for a simple "todo list" backend and publish it tooAzure.</span></span>

## <a name="configure-hello-server-project"></a><span data-ttu-id="d8930-120">Konfigurace projektu server hello</span><span class="sxs-lookup"><span data-stu-id="d8930-120">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinandroid-app"></a><span data-ttu-id="d8930-121">Stažení a spuštění aplikace Xamarin.Android hello</span><span class="sxs-lookup"><span data-stu-id="d8930-121">Download and run hello Xamarin.Android app</span></span>
1. <span data-ttu-id="d8930-122">V části **stáhnout a spustit projekt Xamarin.Android**, klikněte na tlačítko hello **Stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d8930-122">Under **Download and run your Xamarin.Android project**, click hello **Download** button.</span></span>

      <span data-ttu-id="d8930-123">Uložte hello projektu komprimovaný soubor tooyour místního počítače a poznamenejte si kam jste jej uložili.</span><span class="sxs-lookup"><span data-stu-id="d8930-123">Save hello compressed project file tooyour local computer, and make a note of where you save it.</span></span>
2. <span data-ttu-id="d8930-124">Stiskněte klávesu hello **F5** klíče toobuild hello projektu a spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="d8930-124">Press hello **F5** key toobuild hello project and start hello app.</span></span>
3. <span data-ttu-id="d8930-125">Hello aplikace zadejte smysluplný text, například *hello dokončení kurzu* a pak klikněte na tlačítko hello **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d8930-125">In hello app, type meaningful text, such as *Complete hello tutorial* and then click hello **Add** button.</span></span>

    ![][10]

    <span data-ttu-id="d8930-126">Data z požadavku hello je vloženy do tabulky TodoItem hello.</span><span class="sxs-lookup"><span data-stu-id="d8930-126">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="d8930-127">Položky uložené v tabulce hello se vrátí pomocí back-end mobilní aplikace hello a data se zobrazí v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="d8930-127">Items stored in hello table are returned by hello mobile app backend, and the data appears in hello list.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d8930-128">Můžete zkontrolovat hello kód, který má přístup k vaší tooquery back-end mobilní aplikace a vkládání dat, který se nachází v souboru C# ToDoActivity.cs hello.</span><span class="sxs-lookup"><span data-stu-id="d8930-128">You can review hello code that accesses your mobile app backend tooquery and insert data, which is found in hello ToDoActivity.cs C# file.</span></span>
   >
   >

## <a name="next-steps"></a><span data-ttu-id="d8930-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8930-129">Next steps</span></span>
* [<span data-ttu-id="d8930-130">Přidat aplikaci tooyour Offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="d8930-130">Add Offline Sync tooyour app</span></span>](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [<span data-ttu-id="d8930-131">Přidat aplikaci tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="d8930-131">Add authentication tooyour app </span></span>](app-service-mobile-xamarin-android-get-started-users.md)
* [<span data-ttu-id="d8930-132">Přidat nabízená oznámení tooyour Xamarin.Android aplikaci</span><span class="sxs-lookup"><span data-stu-id="d8930-132">Add push notifications tooyour Xamarin.Android app</span></span>](app-service-mobile-xamarin-android-get-started-push.md)
* [<span data-ttu-id="d8930-133">Jak toouse hello spravovat klienta pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="d8930-133">How toouse hello managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
