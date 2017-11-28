---
title: "Začínáme s Azure Mobile Engagementem pro Web Apps | Dokumentace Microsoftu"
description: "Naučte se používat Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro Web Apps."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04afe53a-4caf-4c80-bd75-20cc630cd75c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: hero-article
ms.date: 06/01/2016
ms.author: piyushjo
ms.openlocfilehash: abcb04e4e0a3ae4fdba3a4ded20b3846ac3b21e6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a><span data-ttu-id="82b30-103">Začínáme s Azure Mobile Engagementem pro Web Apps</span><span class="sxs-lookup"><span data-stu-id="82b30-103">Get started with Azure Mobile Engagement for Web Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="82b30-104">V tomto tématu se dozvíte, jak můžete pomocí Azure Mobile Engagementu porozumět využití své webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="82b30-104">This topic shows you how to use Azure Mobile Engagement to understand your Web App usage.</span></span>

> [!NOTE]
> <span data-ttu-id="82b30-105">Službu Azure Mobile Engagement vyřadíme z provozu v březnu 2018. V současnosti je dostupná jenom pro stávající zákazníky.</span><span class="sxs-lookup"><span data-stu-id="82b30-105">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="82b30-106">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="82b30-106">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="82b30-107">V tomto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="82b30-107">This tutorial requires the following:</span></span>

* <span data-ttu-id="82b30-108">Visual Studio 2015 nebo jiný editor podle vaší volby</span><span class="sxs-lookup"><span data-stu-id="82b30-108">Visual Studio 2015 or any other editor of your choice</span></span>
* [<span data-ttu-id="82b30-109">Sada Web SDK</span><span class="sxs-lookup"><span data-stu-id="82b30-109">Web SDK</span></span>](http://aka.ms/P7b453)

<span data-ttu-id="82b30-110">Sada Web SDK je ve verzi Preview, takže zatím podporuje jenom analytické funkce, ale ne odesílání nabízených oznámení v prohlížeči nebo v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="82b30-110">This Web SDK is in Preview and only supports Analytics at the moment and doesn't support sending browser or in-app push notifications yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="82b30-111">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="82b30-111">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="82b30-112">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="82b30-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="82b30-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).</span><span class="sxs-lookup"><span data-stu-id="82b30-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).</span></span>
> 
> 

## <a name="setup-mobile-engagement-for-your-web-app"></a><span data-ttu-id="82b30-114">Nastavení Mobile Engagementu pro vaši webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="82b30-114">Setup Mobile Engagement for your Web app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="82b30-115"><a id="connecting-app"></a>Připojení aplikace k back-endu Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="82b30-115"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="82b30-116">V tomto kurzu si představíme „základní integraci“, tedy minimální sadu, která je zapotřebí pro shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="82b30-116">This tutorial presents a "basic integration," which is the minimal set required to collect data.</span></span>

<span data-ttu-id="82b30-117">Pomocí sady Visual Studio vytvoříme základní webovou aplikaci, na které si předvedeme integraci, i když uvedený postup můžete provést také s libovolnou webovou aplikací vytvořenou mimo sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="82b30-117">We will create a basic web app with Visual Studio to demonstrate the integration though you can follow the steps with any web application created outside of Visual Studio also.</span></span> 

### <a name="create-a-new-web-app"></a><span data-ttu-id="82b30-118">Vytvoření nové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="82b30-118">Create a new Web App</span></span>
<span data-ttu-id="82b30-119">Následující postup předpokládá použití sady Visual Studio 2015, ačkoliv kroky se podobají předchozím verzím sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="82b30-119">The following steps assume the use of Visual Studio 2015 though the steps are similar in earlier versions of Visual Studio.</span></span> 

1. <span data-ttu-id="82b30-120">Spusťte Visual Studio a na obrazovce **Domů** vyberte **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="82b30-120">Start Visual Studio, and in the **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="82b30-121">V místním okně vyberte **Web** -> **Webová aplikace ASP.Net**.</span><span class="sxs-lookup"><span data-stu-id="82b30-121">In the pop-up, select **Web** -> **ASP.Net Web Application**.</span></span> <span data-ttu-id="82b30-122">Vyplňte **Název** aplikace, **Umístění** **Název řešení** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="82b30-122">Fill in the app **Name**, **Location** and  **Solution name**, and then click **OK**.</span></span>
3. <span data-ttu-id="82b30-123">V místním okně **Vyberte šablonu** zvolte v části **Šablony ASP.Net 4.5** možnost **Prázdná** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="82b30-123">In the **Select a template** popup, select **Empty** under **ASP.Net 4.5 Templates** and click **OK**.</span></span> 

<span data-ttu-id="82b30-124">Vytvořili jste nový prázdný projekt webové aplikace, do kterého budeme integrovat sadu Azure Mobile Engagement Web SDK.</span><span class="sxs-lookup"><span data-stu-id="82b30-124">You have now created a new blank Web App project into which we will integrate the Azure Mobile Engagement Web SDK.</span></span>

### <a name="connect-your-app-to-mobile-engagement-backend"></a><span data-ttu-id="82b30-125">Připojení aplikace k back-endu Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="82b30-125">Connect your app to Mobile Engagement backend</span></span>
1. <span data-ttu-id="82b30-126">Vytvořte ve svém řešení novou složku s názvem **javascript** a přidejte do ní soubor JS sady Web SDK **azure-engagement.js**.</span><span class="sxs-lookup"><span data-stu-id="82b30-126">Create a new folder called **javascript** in your solution and add the Web SDK JS file **azure-engagement.js** into it.</span></span> 
2. <span data-ttu-id="82b30-127">Do této složky javascript přidejte nový soubor **main.js** s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="82b30-127">Add a new file called **main.js** in this javascript folder with the following code.</span></span> <span data-ttu-id="82b30-128">Nezapomeňte aktualizovat připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="82b30-128">Make sure to update the connection string.</span></span> <span data-ttu-id="82b30-129">Tento objekt `azureEngagement` se bude používat k přístupu k metodám sady Web SDK.</span><span class="sxs-lookup"><span data-stu-id="82b30-129">This `azureEngagement` object will be used to access Web SDK methods.</span></span> 
   
        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };
   
    ![Visual Studio se soubory JS][1]

## <a name="enable-real-time-monitoring"></a><span data-ttu-id="82b30-131">Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="82b30-131">Enable real-time monitoring</span></span>
<span data-ttu-id="82b30-132">Pokud chcete začít odesílat data a zajistit, aby byli uživatelé aktivní, musíte odeslat alespoň jednu aktivitu do rozhraní back-end Mobile Engagementu.</span><span class="sxs-lookup"><span data-stu-id="82b30-132">In order to start sending data and ensuring that the users are active, you must send at least one Activity to the Mobile Engagement backend.</span></span> <span data-ttu-id="82b30-133">Aktivita v kontextu webové aplikace znamená webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="82b30-133">An activity in the context of a web app is a web page.</span></span> 

1. <span data-ttu-id="82b30-134">Vytvořte ve svém řešení novou stránku s názvem **home.html** a nastavte ji jako úvodní stránku své webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="82b30-134">Create a new page called **home.html** in your solution and set it as the starting page for your web app.</span></span> 
2. <span data-ttu-id="82b30-135">Do této stránky zahrňte dva kódy JavaScript, které jsme přidali předtím, a to tak, že do značky body přidáte následující kód.</span><span class="sxs-lookup"><span data-stu-id="82b30-135">Include the two javascripts we added earlier in this page by adding the following within the body tag.</span></span> 
   
        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>
3. <span data-ttu-id="82b30-136">Aktualizace značky body, aby volala metodu `startActivity` programu EngagementAgent</span><span class="sxs-lookup"><span data-stu-id="82b30-136">Update the body tag to call EngagementAgent's `startActivity` method</span></span>
   
        <body onload="engagement.agent.startActivity('Home')">
4. <span data-ttu-id="82b30-137">Stránka **home.html** bude vypadat takhle:</span><span class="sxs-lookup"><span data-stu-id="82b30-137">Here is what your **home.html** will look like</span></span>
   
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

## <a name="connect-app-with-real-time-monitoring"></a><span data-ttu-id="82b30-138">Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="82b30-138">Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

  ![][2]

## <a name="extend-analytics"></a><span data-ttu-id="82b30-139">Rozšířená analýza</span><span class="sxs-lookup"><span data-stu-id="82b30-139">Extend analytics</span></span>
<span data-ttu-id="82b30-140">Tady jsou všechny metody, které jsou v současné době v sadě Web SDK dostupné a které můžete použít k analýze:</span><span class="sxs-lookup"><span data-stu-id="82b30-140">Here are all the methods currently available with Web SDK that you can use for analytics:</span></span>

1. <span data-ttu-id="82b30-141">Aktivity / webové stránky:</span><span class="sxs-lookup"><span data-stu-id="82b30-141">Activities/Web pages:</span></span>
   
        engagement.agent.startActivity(name);
        engagement.agent.endActivity();
2. <span data-ttu-id="82b30-142">Události</span><span class="sxs-lookup"><span data-stu-id="82b30-142">Events</span></span>
   
        engagement.agent.sendEvent(name, extras);
3. <span data-ttu-id="82b30-143">Chyby</span><span class="sxs-lookup"><span data-stu-id="82b30-143">Errors</span></span>
   
        engagement.agent.sendError(name, extras);
4. <span data-ttu-id="82b30-144">Úlohy</span><span class="sxs-lookup"><span data-stu-id="82b30-144">Jobs</span></span>
   
        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

