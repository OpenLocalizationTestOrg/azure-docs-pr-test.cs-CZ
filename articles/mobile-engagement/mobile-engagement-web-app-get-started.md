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
# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Začínáme s Azure Mobile Engagementem pro Web Apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak Azure Mobile Engagement toounderstand toouse používání webové aplikace.

> [!NOTE]
> Služba Azure Mobile Engagement Hello vyřadí března 2018 a je aktuálně pouze k dispozici tooexisting zákazníků. Další informace najdete v tématu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Tento kurz vyžaduje hello následující:

* Visual Studio 2015 nebo jiný editor podle vaší volby
* [Sada Web SDK](http://aka.ms/P7b453)

Tato sada SDK webové je ve verzi Preview a pouze podporuje Analytics momentálně hello a odesílání nabízených oznámení prohlížeče nebo v aplikaci ještě nepodporuje. 

> [!NOTE]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).
> 
> 

## <a name="setup-mobile-engagement-for-your-web-app"></a>Nastavení Mobile Engagementu pro vaši webovou aplikaci
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Připojit vaše aplikace toohello Mobile Engagement back-end
V tomto kurzu uvede "základní integraci", což je hello minimální sadu požadovanou toocollect data.

Vytvoříme základní webové aplikace s integrace hello toodemonstrate sady Visual Studio, i když se všechny webové aplikace vytvořené mimo Visual Studio také můžete provést kroky pro hello. 

### <a name="create-a-new-web-app"></a>Vytvoření nové webové aplikace
Hello následující postup předpokládá použití hello sady Visual Studio 2015 když hello postup je podobný jako ve starších verzích sady Visual Studio. 

1. Spuštění sady Visual Studio a v hello **Domů** obrazovku, vyberte **nový projekt**.
2. V místní nabídce hello vyberte **webové** -> **webové aplikace ASP.Net**. Vyplňte aplikace hello **název**, **umístění** a **název řešení**a potom klikněte na **OK**.
3. V hello **vyberte šablonu** místní, vyberte **prázdný** pod **šablony ASP.Net 4.5** a klikněte na tlačítko **OK**. 

Nyní jste vytvořili nový prázdný projekt webové aplikace do které budeme integrovat hello sada SDK webové služby Azure Mobile Engagement.

### <a name="connect-your-app-toomobile-engagement-backend"></a>Připojit vaše tooMobile Engagement back-end aplikace
1. Vytvořit novou složku s názvem **javascript** ve vašem řešení a přidejte soubor webové SDK JS hello **azure engagement.js** do ní. 
2. Přidat nový soubor s názvem **main.js** v této složce javascript s hello následující kód. Ověřte, zda tooupdate hello připojovací řetězec. To `azureEngagement` objekt se bude použité tooaccess sady SDK webové metody. 
   
        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };
   
    ![Visual Studio se soubory JS][1]

## <a name="enable-real-time-monitoring"></a>Povolení sledování v reálném čase
V pořadí toostart odesílat data a zajistit, že hello uživatelé jsou aktivní musíte odeslat alespoň jednu aktivitu toohello Mobile Engagement back-end. Aktivita v kontextu hello webové aplikace je na webové stránce. 

1. Vytvořit novou stránku s názvem **home.html** v řešení a sadu ho jako hello výchozí stránky pro webovou aplikaci. 
2. Zahrnout hello dvě JavaScripty, kterou jsme přidali dříve na této stránce přidáním následující hello v rámci značky textu hello. 
   
        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>
3. Aktualizovat hello textu značky toocall EngagementAgent na `startActivity` – metoda
   
        <body onload="engagement.agent.startActivity('Home')">
4. Stránka **home.html** bude vypadat takhle:
   
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

## <a name="connect-app-with-real-time-monitoring"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

  ![][2]

## <a name="extend-analytics"></a>Rozšířená analýza
Zde jsou všechny metody hello Web SDK, která můžete použít k analýze aktuálně k dispozici:

1. Aktivity / webové stránky:
   
        engagement.agent.startActivity(name);
        engagement.agent.endActivity();
2. Události
   
        engagement.agent.sendEvent(name, extras);
3. Chyby
   
        engagement.agent.sendError(name, extras);
4. Úlohy
   
        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

