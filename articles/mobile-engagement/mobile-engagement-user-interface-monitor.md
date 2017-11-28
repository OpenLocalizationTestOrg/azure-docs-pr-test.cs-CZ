---
title: "aaaAzure Mobile Engagement uživatelské rozhraní – monitorování"
description: "Zjistěte, jak toomonitor v reálném čase data o vaší aplikaci pomocí Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: b91ad89a-b89d-4377-abb0-cc2d16a2836d
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3a581e4166bc88e6ee7aa784d4047c94533685b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-real-time-data-about-your-application"></a><span data-ttu-id="9c49a-103">Jak toomonitor v reálném čase dat o aplikaci</span><span class="sxs-lookup"><span data-stu-id="9c49a-103">How toomonitor real-time data about your application</span></span>
<span data-ttu-id="9c49a-104">Tento článek popisuje hello **monitorování** kartě hello **Mobile Engagement** portálu.</span><span class="sxs-lookup"><span data-stu-id="9c49a-104">This article describes hello **MONITOR** tab of hello **Mobile Engagement** portal.</span></span> <span data-ttu-id="9c49a-105">Použít hello **Mobile Engagement** portálu toomonitor a spravovat své mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c49a-105">You use hello **Mobile Engagement** portal toomonitor and manage your mobile apps.</span></span> <span data-ttu-id="9c49a-106">Všimněte si, že toostart hello portálu musíte nejprve toocreate **Azure Mobile Engagement** účtu.</span><span class="sxs-lookup"><span data-stu-id="9c49a-106">Note that toostart using hello portal you first need toocreate an **Azure Mobile Engagement** account.</span></span> 

<span data-ttu-id="9c49a-107">Hello monitorování části hello uživatelského rozhraní poskytuje informace pro analýzu v reálném čase a umožňuje vám tooset výstrahy, když se dosáhne mezní hodnoty pro většinu hello stejné informace, které jsou k dispozici v minulosti v hello [ANALYTICS](mobile-engagement-user-interface-analytics.md) část Hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9c49a-107">hello Monitor section of hello UI provides real-time analytics information and allows you tooset alerts when thresholds are reached for most of hello same information that is available historically in hello [ANALYTICS](mobile-engagement-user-interface-analytics.md) section of hello UI.</span></span> <span data-ttu-id="9c49a-108">V tématu hello **Glosář** část v hello [koncepty](http://go.microsoft.com/fwlink/?LinkId=525555) tématu definice termínů a zkratek v analýzy a monitorování (například následující hello: aktivního uživatele, nový uživatel uchovávají uživatele, relace, Uživatel cesta grafu, uživatelé mapy, sledování adres URL, trendy, aktivity, událostí, úlohy, chyba, doplňující informace, havárií a App-info).</span><span class="sxs-lookup"><span data-stu-id="9c49a-108">See hello **Glossary** section in hello [Concepts](http://go.microsoft.com/fwlink/?LinkId=525555) topic for definitions of terms and abbreviations in Analytics and Monitoring (such as hello following: Active User, New user, Retained User, Session, User Path Graph, Users Map, Tracking URLs, Trends, Activity, Event, Job, Error, Extra Info, Crash, and App-info).</span></span>

> [!NOTE]
> <span data-ttu-id="9c49a-109">Mnoho oddílů hello **Mobile Engagement** portál uživatelského rozhraní obsahovat hello **zobrazit NÁPOVĚDU k** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9c49a-109">Many sections of hello **Mobile Engagement** portal UI contain hello **SHOW HELP** button.</span></span> <span data-ttu-id="9c49a-110">Stisknutím tohoto tlačítka tooget další kontextové informace o oddílu.</span><span class="sxs-lookup"><span data-stu-id="9c49a-110">Press this button tooget more contextual information about a section.</span></span>
> 
> 

## <a name="monitor---sessions-jobs-events-errors-and-crashes"></a><span data-ttu-id="9c49a-111">Monitorování – relací, úlohy, události, chyb a dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="9c49a-111">Monitor - Sessions, Jobs, Events, Errors, and Crashes</span></span>
<span data-ttu-id="9c49a-112">Zobrazí počet uživatelů, kteří jsou aktuálně v relaci a na konkrétní obrazovky nebo provádění určitých akcí.</span><span class="sxs-lookup"><span data-stu-id="9c49a-112">You can see how many users are currently in session and on specific screens or doing specific actions.</span></span> <span data-ttu-id="9c49a-113">Můžete zobrazit aktivitu uživatelů rozdělené podle relací, úlohy, události, chyb a dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="9c49a-113">You can view user activity divided by Sessions, Jobs, Events, Errors, and Crashes.</span></span> <span data-ttu-id="9c49a-114">Můžete zobrazit aktuální informace o hello a zobrazovat informace o hello z hello poslední hodinu dne nebo týdne.</span><span class="sxs-lookup"><span data-stu-id="9c49a-114">You can see hello current information and show hello information from hello last hour, day, or week.</span></span> <span data-ttu-id="9c49a-115">Můžete zobrazit všechny informace hello v každé kategorii nebo seřadit hello konkrétní relace, úlohy, události, chyby a havárií.</span><span class="sxs-lookup"><span data-stu-id="9c49a-115">You can see all of hello information in each category or sort by hello specific Session, Job, Event, Error, and Crash.</span></span>  <span data-ttu-id="9c49a-116">Monitorování za provozu je užitečné toouse během události třeba toosee kampaň nabízených, pokud dojde uptick v akci správné po odeslání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="9c49a-116">Live monitoring is helpful toouse during events such as a Push campaign toosee if there is an uptick in action right after you send your Push notification.</span></span>

![Monitor1][14]  

## <a name="troubleshooting-with-monitor---events---details"></a><span data-ttu-id="9c49a-118">Řešení potíží s monitorování - události – podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9c49a-118">Troubleshooting with Monitor - Events - Details</span></span>
<span data-ttu-id="9c49a-119">Generování událost v aplikaci z testovací zařízení a hledání v monitorování - události – podrobnosti je jedním z nejjednodušší toofind způsoby hello ID zařízení pro testovací zařízení a tooconfirm této integrace Azure Mobile Engagement Analytics, monitorování, a Segmenty pracuje z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c49a-119">Generating an event in your application from your test device and finding it in Monitor - Events - Details is one of hello easiest ways toofind your Device ID for your test device and tooconfirm that Azure Mobile Engagement integration of Analytics, Monitoring, and Segments is working from your application.</span></span> <span data-ttu-id="9c49a-120">Až budete mít hello zařízení ID testovací zařízení, můžete ho přidat tooyour testovací zařízení v "Moje zařízení – účet".</span><span class="sxs-lookup"><span data-stu-id="9c49a-120">Once you have hello Device ID of your test device, you can add it tooyour test devices in “My Account - Devices”.</span></span> <span data-ttu-id="9c49a-121">Pokud nelze generovat událost, ujistěte se, že Azure Mobile Engagement je správně integrovaný do vaší aplikace Android nebo iOS/Web/Windows/Windows Phone pomocí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="9c49a-121">If you can't generate an event, make sure that Azure Mobile Engagement is correctly integrated in your Android/iOS/Web/Windows/Windows Phone app with hello SDK.</span></span>

<span data-ttu-id="9c49a-122">Další informace najdete v tématu: [dokumentaci k sadě SDK][Link 5]</span><span class="sxs-lookup"><span data-stu-id="9c49a-122">For more information, see:  [SDK Documentation][Link 5]</span></span>

![Monitor2][15]  

## <a name="troubleshooting-with-monitor---crashes---details"></a><span data-ttu-id="9c49a-124">Řešení potíží s monitorováním - spadne – podrobnosti</span><span class="sxs-lookup"><span data-stu-id="9c49a-124">Troubleshooting with Monitor - Crashes - Details</span></span>
<span data-ttu-id="9c49a-125">O aplikaci z monitorování - havárií - podrobnosti toohelp určení příčiny chyb aplikace můžete zkontrolovat informace o havárii.</span><span class="sxs-lookup"><span data-stu-id="9c49a-125">You can review crash information about your app from Monitor - Crashes - Details toohelp determine why your app is crashing.</span></span> <span data-ttu-id="9c49a-126">Vyhledání by také známé problémy s každou verzi hello SDK v hello poznámky k verzi pro každou verzi hello SDK pro Android nebo iOS/Web/Windows/Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="9c49a-126">You should also look up known issues with each version of hello SDK in hello release notes for each version of hello SDK for Android/iOS/Web/Windows/Windows Phone.</span></span>

<span data-ttu-id="9c49a-127">Další informace najdete v tématu: [dokumentaci k sadě SDK – poznámky k verzi][Link 5]</span><span class="sxs-lookup"><span data-stu-id="9c49a-127">For more information, see:  [SDK Documentation - Release notes][Link 5]</span></span>

![Monitor3][16]

## <a name="monitor---alerts"></a><span data-ttu-id="9c49a-129">Monitorování – výstrahy</span><span class="sxs-lookup"><span data-stu-id="9c49a-129">Monitor - Alerts</span></span>
<span data-ttu-id="9c49a-130">Můžete také zadat podmínek pro výstrahy, které se budou automaticky posílat tooyou prostřednictvím e-mailu nebo pomocí rychlé zprávy.</span><span class="sxs-lookup"><span data-stu-id="9c49a-130">You can also specify conditions for Alerts that will be automatically sent tooyou via e-mail or instant message.</span></span> <span data-ttu-id="9c49a-131">(Všechny služby vyhovující protokolu XMPP, například Google Talk nebo Apple iChat nejsou podporovány.) Výstrahy jsou založené na předem definované detekce prahová hodnota větší než (>) nebo menší než (<) určitý počet relací, úlohy, události, chyby nebo dojde k chybě za sekundu, minutu nebo hodinu.</span><span class="sxs-lookup"><span data-stu-id="9c49a-131">(Any XMPP-compliant services like Google's GTalk or Apple's iChat are supported.) Alerts are based on a pre-defined detection threshold greater than (>) or less than (<) a specific number of Sessions, Jobs, Events, Errors, or Crashes per second, minute, or hour.</span></span> <span data-ttu-id="9c49a-132">Výstrahy můžete sledovat všechny aktivity daného typu nebo právě sledovat konkrétní úlohy, události nebo chyba činnost.</span><span class="sxs-lookup"><span data-stu-id="9c49a-132">Alerts can monitor all activities of a given type, or just monitor a specific Job, Event, or Error activity.</span></span> 

<span data-ttu-id="9c49a-133">Můžete také zadat minimální rychlost detekce, což je minimální množství minut, po které dojde k oddělení dvou oznámení hello stejné výstrahy toomake jistotu, že při aktivaci upozornění nikdy nedostanete vyšší než 1 oznámení za intervalu zadaném hello.</span><span class="sxs-lookup"><span data-stu-id="9c49a-133">You can also specify a Minimum Detection Rate, which is hello minimum amount of minutes that will separate two notifications for hello same alert toomake sure that when your alert is triggered, you will never receive more than 1 notification per interval specified.</span></span>

![Monitor4][17]

## <a name="see-also"></a><span data-ttu-id="9c49a-135">Viz také</span><span class="sxs-lookup"><span data-stu-id="9c49a-135">See also</span></span>
* <span data-ttu-id="9c49a-136">[Koncepty][Link 6]</span><span class="sxs-lookup"><span data-stu-id="9c49a-136">[Concepts][Link 6]</span></span>
* <span data-ttu-id="9c49a-137">[Řešení potíží s příručce služby][Link 24]</span><span class="sxs-lookup"><span data-stu-id="9c49a-137">[Troubleshooting Guide Service][Link 24]</span></span>

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
