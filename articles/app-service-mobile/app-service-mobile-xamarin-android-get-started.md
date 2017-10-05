---
title: "Začínáme s Azure Mobile Apps u aplikací na platformě Xamarin.Android"
description: "V tomto kurzu začnete používat Azure Mobile Apps pro vývoj na platformě Xamarin.Android."
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
ms.openlocfilehash: 6b41fd8090dd771fc40769c134bad258b3d4bd36
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-xamarinandroid-app"></a><span data-ttu-id="b47d0-103">Vytvoření aplikace Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="b47d0-103">Create a Xamarin.Android App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="b47d0-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b47d0-104">Overview</span></span>
<span data-ttu-id="b47d0-105">V tomto kurzu se dozvíte, jak do aplikace Xamarin.Android přidat cloudovou back-end službu.</span><span class="sxs-lookup"><span data-stu-id="b47d0-105">This tutorial shows you how to add a cloud-based backend service to a Xamarin.Android app.</span></span> <span data-ttu-id="b47d0-106">Další informace najdete v tématu [Co jsou Mobile Apps](app-service-mobile-value-prop.md).</span><span class="sxs-lookup"><span data-stu-id="b47d0-106">For more information, see [What are Mobile Apps](app-service-mobile-value-prop.md).</span></span>

<span data-ttu-id="b47d0-107">Zde je snímek obrazovky dokončené aplikace:</span><span class="sxs-lookup"><span data-stu-id="b47d0-107">A screenshot from the completed app is below:</span></span>

![][0]

<span data-ttu-id="b47d0-108">Ve všech dalších kurzech k používání funkce Mobile Apps pro Xamarin.Android se předpokládá dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b47d0-108">Completing this tutorial is a prerequisite for all other Mobile Apps tutorials for Xamarin.Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b47d0-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b47d0-109">Prerequisites</span></span>
<span data-ttu-id="b47d0-110">Pro absolvování tohoto kurzu musí být splněné následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="b47d0-110">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="b47d0-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="b47d0-111">An active Azure account.</span></span> <span data-ttu-id="b47d0-112">Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="b47d0-112">If you don't have an account, sign up for an Azure trial and get up to 10 free Mobile Apps.</span></span> <span data-ttu-id="b47d0-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b47d0-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="b47d0-114">Visual Studio s Xamarinem.</span><span class="sxs-lookup"><span data-stu-id="b47d0-114">Visual Studio with Xamarin.</span></span> <span data-ttu-id="b47d0-115">Pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="b47d0-115">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) for instructions.</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="b47d0-116">Vytvoření back-endu mobilní aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="b47d0-116">Create an Azure Mobile App backend</span></span>
<span data-ttu-id="b47d0-117">Podle těchto pokynů vytvořte back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="b47d0-117">Follow these steps to create a Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

<span data-ttu-id="b47d0-118">Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích.</span><span class="sxs-lookup"><span data-stu-id="b47d0-118">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="b47d0-119">Dále si stáhněte serverový projekt pro jednoduchý back-end seznamu úkolů a publikujete ho v Azure.</span><span class="sxs-lookup"><span data-stu-id="b47d0-119">Next, download a server project for a simple "todo list" backend and publish it to Azure.</span></span>

## <a name="configure-the-server-project"></a><span data-ttu-id="b47d0-120">Konfigurace serverového projektu</span><span class="sxs-lookup"><span data-stu-id="b47d0-120">Configure the server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a><span data-ttu-id="b47d0-121">Stáhnutí a spuštění aplikace Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="b47d0-121">Download and run the Xamarin.Android app</span></span>
1. <span data-ttu-id="b47d0-122">V části **Stáhnout a spustit projekt Xamarin.Android** klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="b47d0-122">Under **Download and run your Xamarin.Android project**, click the **Download** button.</span></span>

      <span data-ttu-id="b47d0-123">Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.</span><span class="sxs-lookup"><span data-stu-id="b47d0-123">Save the compressed project file to your local computer, and make a note of where you save it.</span></span>
2. <span data-ttu-id="b47d0-124">Stiskněte klávesu **F5**, aby se projekt sestavil a spustil aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b47d0-124">Press the **F5** key to build the project and start the app.</span></span>
3. <span data-ttu-id="b47d0-125">V aplikaci zadejte smysluplný text, například *Dokončit kurz*, a klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="b47d0-125">In the app, type meaningful text, such as *Complete the tutorial* and then click the **Add** button.</span></span>

    ![][10]

    <span data-ttu-id="b47d0-126">Data z požadavku se vloží do tabulky TodoItem.</span><span class="sxs-lookup"><span data-stu-id="b47d0-126">Data from the request is inserted into the TodoItem table.</span></span> <span data-ttu-id="b47d0-127">Položky uložené v tabulce budou vráceny back-endu mobilní aplikace a v seznamu se objeví data.</span><span class="sxs-lookup"><span data-stu-id="b47d0-127">Items stored in the table are returned by the mobile app backend, and the data appears in the list.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b47d0-128">Na kód, který přistupuje k back-endu mobilní aplikace pro dotazování a vkládání dat, se můžete podívat v souboru C# ToDoActivity.cs.</span><span class="sxs-lookup"><span data-stu-id="b47d0-128">You can review the code that accesses your mobile app backend to query and insert data, which is found in the ToDoActivity.cs C# file.</span></span>
   >
   >

## <a name="next-steps"></a><span data-ttu-id="b47d0-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b47d0-129">Next steps</span></span>
* [<span data-ttu-id="b47d0-130">Přidání offline synchronizace do aplikace</span><span class="sxs-lookup"><span data-stu-id="b47d0-130">Add Offline Sync to your app</span></span>](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [<span data-ttu-id="b47d0-131">Přidání ověřování do aplikace</span><span class="sxs-lookup"><span data-stu-id="b47d0-131">Add authentication to your app </span></span>](app-service-mobile-xamarin-android-get-started-users.md)
* [<span data-ttu-id="b47d0-132">Přidání nabízených oznámení do aplikace Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="b47d0-132">Add push notifications to your Xamarin.Android app</span></span>](app-service-mobile-xamarin-android-get-started-push.md)
* [<span data-ttu-id="b47d0-133">Jak používat spravovaného klienta pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="b47d0-133">How to use the managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
