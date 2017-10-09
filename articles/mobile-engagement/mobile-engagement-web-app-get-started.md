---
title: "aaaGet začít s Azure Mobile Engagement pro webové aplikace | Microsoft Docs"
description: "Zjistěte, jak toouse Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro webové aplikace."
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
ms.openlocfilehash: a84c96cac13bf3b85e72aef55da5c91693e1766c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a><span data-ttu-id="3809f-103">Začínáme s Azure Mobile Engagementem pro Web Apps</span><span class="sxs-lookup"><span data-stu-id="3809f-103">Get started with Azure Mobile Engagement for Web Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="3809f-104">Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse používání webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3809f-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your Web App usage.</span></span>

> [!NOTE]
> <span data-ttu-id="3809f-105">Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků.</span><span class="sxs-lookup"><span data-stu-id="3809f-105">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="3809f-106">Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="3809f-106">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="3809f-107">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="3809f-107">This tutorial requires hello following:</span></span>

* <span data-ttu-id="3809f-108">Visual Studio 2015 nebo jiný editor podle vaší volby</span><span class="sxs-lookup"><span data-stu-id="3809f-108">Visual Studio 2015 or any other editor of your choice</span></span>
* [<span data-ttu-id="3809f-109">Sada Web SDK</span><span class="sxs-lookup"><span data-stu-id="3809f-109">Web SDK</span></span>](http://aka.ms/P7b453)

<span data-ttu-id="3809f-110">Tato sada SDK webové je ve verzi Preview a pouze podporuje Analytics momentálně hello a odesílání nabízených oznámení prohlížeče nebo v aplikaci ještě nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="3809f-110">This Web SDK is in Preview and only supports Analytics at hello moment and doesn't support sending browser or in-app push notifications yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="3809f-111">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3809f-111">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="3809f-112">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="3809f-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="3809f-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).</span><span class="sxs-lookup"><span data-stu-id="3809f-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).</span></span>
> 
> 

## <a name="setup-mobile-engagement-for-your-web-app"></a><span data-ttu-id="3809f-114">Nastavení Mobile Engagementu pro vaši webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="3809f-114">Setup Mobile Engagement for your Web app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="3809f-115"><a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end</span><span class="sxs-lookup"><span data-stu-id="3809f-115"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="3809f-116">V tomto kurzu uvede "základní integraci", což je hello minimální sadu požadovanou toocollect data.</span><span class="sxs-lookup"><span data-stu-id="3809f-116">This tutorial presents a "basic integration," which is hello minimal set required toocollect data.</span></span>

<span data-ttu-id="3809f-117">Vytvoříme základní webové aplikace s integrace hello toodemonstrate sady Visual Studio, i když se všechny webové aplikace vytvořené mimo Visual Studio také můžete provést kroky pro hello.</span><span class="sxs-lookup"><span data-stu-id="3809f-117">We will create a basic web app with Visual Studio toodemonstrate hello integration though you can follow hello steps with any web application created outside of Visual Studio also.</span></span> 

### <a name="create-a-new-web-app"></a><span data-ttu-id="3809f-118">Vytvoření nové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="3809f-118">Create a new Web App</span></span>
<span data-ttu-id="3809f-119">Hello následující postup předpokládá použití hello sady Visual Studio 2015 když hello postup je podobný jako ve starších verzích sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3809f-119">hello following steps assume hello use of Visual Studio 2015 though hello steps are similar in earlier versions of Visual Studio.</span></span> 

1. <span data-ttu-id="3809f-120">Spuštění sady Visual Studio a v hello **Domů** obrazovku, vyberte **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="3809f-120">Start Visual Studio, and in hello **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="3809f-121">V místní nabídce hello vyberte **webové** -> **webové aplikace ASP.Net**.</span><span class="sxs-lookup"><span data-stu-id="3809f-121">In hello pop-up, select **Web** -> **ASP.Net Web Application**.</span></span> <span data-ttu-id="3809f-122">Vyplňte aplikace hello **název**, **umístění** a **název řešení**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3809f-122">Fill in hello app **Name**, **Location** and  **Solution name**, and then click **OK**.</span></span>
3. <span data-ttu-id="3809f-123">V hello **vyberte šablonu** místní, vyberte **prázdný** pod **šablony ASP.Net 4.5** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3809f-123">In hello **Select a template** popup, select **Empty** under **ASP.Net 4.5 Templates** and click **OK**.</span></span> 

<span data-ttu-id="3809f-124">Nyní jste vytvořili nový prázdný projekt webové aplikace do které budeme integrovat hello sada SDK webové služby Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="3809f-124">You have now created a new blank Web App project into which we will integrate hello Azure Mobile Engagement Web SDK.</span></span>

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="3809f-125">Připojit vaše tooMobile Engagement back-end aplikace</span><span class="sxs-lookup"><span data-stu-id="3809f-125">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="3809f-126">Vytvořit novou složku s názvem **javascript** ve vašem řešení a přidejte soubor webové SDK JS hello **azure engagement.js** do ní.</span><span class="sxs-lookup"><span data-stu-id="3809f-126">Create a new folder called **javascript** in your solution and add hello Web SDK JS file **azure-engagement.js** into it.</span></span> 
2. <span data-ttu-id="3809f-127">Přidat nový soubor s názvem **main.js** v této složce javascript s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="3809f-127">Add a new file called **main.js** in this javascript folder with hello following code.</span></span> <span data-ttu-id="3809f-128">Ověřte, zda tooupdate hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="3809f-128">Make sure tooupdate hello connection string.</span></span> <span data-ttu-id="3809f-129">To `azureEngagement` objekt se bude použité tooaccess sady SDK webové metody.</span><span class="sxs-lookup"><span data-stu-id="3809f-129">This `azureEngagement` object will be used tooaccess Web SDK methods.</span></span> 
   
        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };
   
    ![Visual Studio se soubory JS][1]

## <a name="enable-real-time-monitoring"></a><span data-ttu-id="3809f-131">Povolení sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="3809f-131">Enable real-time monitoring</span></span>
<span data-ttu-id="3809f-132">V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu aktivitu toohello Mobile Engagement back-end.</span><span class="sxs-lookup"><span data-stu-id="3809f-132">In order toostart sending data and ensuring that hello users are active, you must send at least one Activity toohello Mobile Engagement backend.</span></span> <span data-ttu-id="3809f-133">Aktivita v kontextu hello webové aplikace je na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="3809f-133">An activity in hello context of a web app is a web page.</span></span> 

1. <span data-ttu-id="3809f-134">Vytvořit novou stránku s názvem **home.html** v řešení a sadu ho jako hello výchozí stránky pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3809f-134">Create a new page called **home.html** in your solution and set it as hello starting page for your web app.</span></span> 
2. <span data-ttu-id="3809f-135">Zahrnout hello dvě JavaScripty, kterou jsme přidali dříve na této stránce přidáním následující hello v rámci značky textu hello.</span><span class="sxs-lookup"><span data-stu-id="3809f-135">Include hello two javascripts we added earlier in this page by adding hello following within hello body tag.</span></span> 
   
        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>
3. <span data-ttu-id="3809f-136">Aktualizovat hello textu značky toocall EngagementAgent na `startActivity` – metoda</span><span class="sxs-lookup"><span data-stu-id="3809f-136">Update hello body tag toocall EngagementAgent's `startActivity` method</span></span>
   
        <body onload="engagement.agent.startActivity('Home')">
4. <span data-ttu-id="3809f-137">Stránka **home.html** bude vypadat takhle:</span><span class="sxs-lookup"><span data-stu-id="3809f-137">Here is what your **home.html** will look like</span></span>
   
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

## <a name="connect-app-with-real-time-monitoring"></a><span data-ttu-id="3809f-138">Připojení aplikace se sledováním v reálném čase</span><span class="sxs-lookup"><span data-stu-id="3809f-138">Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

  ![][2]

## <a name="extend-analytics"></a><span data-ttu-id="3809f-139">Rozšířená analýza</span><span class="sxs-lookup"><span data-stu-id="3809f-139">Extend analytics</span></span>
<span data-ttu-id="3809f-140">Zde jsou všechny metody hello Web SDK, která můžete použít k analýze aktuálně k dispozici:</span><span class="sxs-lookup"><span data-stu-id="3809f-140">Here are all hello methods currently available with Web SDK that you can use for analytics:</span></span>

1. <span data-ttu-id="3809f-141">Aktivity / webové stránky:</span><span class="sxs-lookup"><span data-stu-id="3809f-141">Activities/Web pages:</span></span>
   
        engagement.agent.startActivity(name);
        engagement.agent.endActivity();
2. <span data-ttu-id="3809f-142">Události</span><span class="sxs-lookup"><span data-stu-id="3809f-142">Events</span></span>
   
        engagement.agent.sendEvent(name, extras);
3. <span data-ttu-id="3809f-143">Chyby</span><span class="sxs-lookup"><span data-stu-id="3809f-143">Errors</span></span>
   
        engagement.agent.sendError(name, extras);
4. <span data-ttu-id="3809f-144">Úlohy</span><span class="sxs-lookup"><span data-stu-id="3809f-144">Jobs</span></span>
   
        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

