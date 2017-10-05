---
title: "Azure Mobile Engagement uživatelské rozhraní – segmenty"
description: "Postup vytvoření a správa segmentů uživatelů pro identifikaci vzorů využití pomocí Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6a4f2205-4a3c-406e-a04f-5e6f2a36653f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 087f4a1fef420abe9669f8dfe2b84c7a847ce263
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a><span data-ttu-id="7a5db-103">Postup vytvoření a správa segmentů uživatelů pro identifikaci vzorů využití</span><span class="sxs-lookup"><span data-stu-id="7a5db-103">How to create and manage segments of users to identify usage patterns</span></span>
<span data-ttu-id="7a5db-104">Tento článek popisuje **SEGMENTY** kartě **Mobile Engagement** portálu.</span><span class="sxs-lookup"><span data-stu-id="7a5db-104">This article describes the **SEGMENTS** tab of the **Mobile Engagement** portal.</span></span> <span data-ttu-id="7a5db-105">Můžete použít **Mobile Engagement** portálu ke sledování a správě mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="7a5db-105">You use the **Mobile Engagement** portal to monitor and manage your mobile apps.</span></span>

<span data-ttu-id="7a5db-106">Části segmenty rozhraní umožňuje pracovat na segmentace uživatelů na základě různých chování a analýz, které můžete získat z aplikace a můžete také přistupovat prostřednictvím rozhraní API segmenty.</span><span class="sxs-lookup"><span data-stu-id="7a5db-106">The Segments section of the UI allows you to work on segmenting your users based on the different behavior and analytics that you can get from the application and can also access through the Segments API.</span></span> <span data-ttu-id="7a5db-107">Segmenty se vypočítávají nejprve 24 hodin po jejich vytvoření a jejich jsou přepočítávány každých 24 hodin na základě nejnovější analytics informací.</span><span class="sxs-lookup"><span data-stu-id="7a5db-107">Segments are first computed 24 hours after they are created, and they are recomputed every 24 hours based on the latest analytics information.</span></span> <span data-ttu-id="7a5db-108">Jakmile se počítá segment, zobrazuje graf "Dne do historie den" každý den.</span><span class="sxs-lookup"><span data-stu-id="7a5db-108">Once a segment is calculated, it displays a "Day to day history" chart each day.</span></span>

> [!NOTE]
> <span data-ttu-id="7a5db-109">Mnoho části **Mobile Engagement** portál obsahovat uživatelského rozhraní **zobrazit NÁPOVĚDU k** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7a5db-109">Many sections of the **Mobile Engagement** portal UI contain the **SHOW HELP** button.</span></span> <span data-ttu-id="7a5db-110">Chcete-li získat další kontextové informace o oddílu na toto tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7a5db-110">Press this button to get more contextual information about a section.</span></span>
> 
> 

## <a name="create-segments"></a><span data-ttu-id="7a5db-111">Vytvoření segmentů</span><span class="sxs-lookup"><span data-stu-id="7a5db-111">Create Segments</span></span>
<span data-ttu-id="7a5db-112">Můžete vytvořit na základě až 10 kritérií na určitou dobu až na 60 dnů v minulosti z části analytics segment.</span><span class="sxs-lookup"><span data-stu-id="7a5db-112">You can create a segment based on up to 10 criteria on a specific period up to 60 days in the past from the analytics section.</span></span> <span data-ttu-id="7a5db-113">Můžete například vytvořit segment podle osob, které se mají zobrazit určité stránky nebo vyhledávat konkrétní obsahu v aplikaci během posledních 10 dnů.</span><span class="sxs-lookup"><span data-stu-id="7a5db-113">For example, you can create a segment based on the people who have viewed certain pages or searched for specific content within your app within the last 10 days.</span></span> <span data-ttu-id="7a5db-114">Tyto informace jsou k dispozici v části analytics.</span><span class="sxs-lookup"><span data-stu-id="7a5db-114">This information is available in the analytics section.</span></span> <span data-ttu-id="7a5db-115">Ano můžete ho vytvořit segment, a pak nastavte nabízených oznámení pro tuto podmnožinu uživatelů potřebujete, aby se vraťte k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7a5db-115">So, you can use it to create a segment, and then set up a push notification to target this subset of users to get them to come back to the application.</span></span> 

> [!NOTE]
> <span data-ttu-id="7a5db-116">Jakmile je vypočítána segment, nemůže být upraven; může být pouze klonovat (zkopírovaný) nebo zničení (odstraněno).</span><span class="sxs-lookup"><span data-stu-id="7a5db-116">Once a segment has been calculated, it cannot be edited; it can only be cloned (copied) or destroyed (deleted).</span></span> <span data-ttu-id="7a5db-117">Segment můžete klonovat stejné aplikace (se stejným ID aplikace), a můžete ho také klonovat do jiných aplikací (s jinou AppID).</span><span class="sxs-lookup"><span data-stu-id="7a5db-117">A segment can be cloned within the same application (with the same AppID), and it can also be cloned into other applications (with a different AppID).</span></span> 

 ![segments1][35] 

## <a name="examples-segments"></a><span data-ttu-id="7a5db-119">Příklady segmenty</span><span class="sxs-lookup"><span data-stu-id="7a5db-119">Examples Segments</span></span>
 ![segments2][36]

<span data-ttu-id="7a5db-121">Segmenty umožní segmentovat koncovým uživatelům vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7a5db-121">Segments allow you to segment the end-users of your application.</span></span>
<span data-ttu-id="7a5db-122">Segmentace vaši uživatelé je důležité marketingové strategie.</span><span class="sxs-lookup"><span data-stu-id="7a5db-122">Segmenting your users is an important marketing strategy.</span></span> <span data-ttu-id="7a5db-123">Azure Mobile Engagement vám umožňuje získat historických dat a vytvořte vlastní segmenty.</span><span class="sxs-lookup"><span data-stu-id="7a5db-123">Azure Mobile Engagement allows you to get historic data and create custom segments.</span></span> <span data-ttu-id="7a5db-124">Tento výkonný nástroj umožňuje další informace o zákazníkům ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7a5db-124">This powerful tool enables you to learn about your customers’ experience in your application.</span></span> <span data-ttu-id="7a5db-125">Můžete snadno analyzovat segmenty a použít jako cíle nabízené segmenty.</span><span class="sxs-lookup"><span data-stu-id="7a5db-125">You can easily analyze your segments and use your segments as push targets.</span></span>
<span data-ttu-id="7a5db-126">Běžné případ použití je chcete odesílat oznámení oznámení na podporu koncovým uživatelům, aby hodnocení aplikace v úložišti.</span><span class="sxs-lookup"><span data-stu-id="7a5db-126">A common use-case is that you want to send a push a notification to encourage your end-users to rate your application in the store.</span></span> <span data-ttu-id="7a5db-127">Místo odesílání oznámení do všichni vaši koncoví uživatelé, můžete vytvořit segment, který byste zadat jenom uživatelé, kteří použili aplikaci každý den pro poslední měsíc a neměla skvělé uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="7a5db-127">Rather than sending a notification to all your end-users, you can create a segment that would specify only users that have used your application every day for the last month and have had a great user experience.</span></span> <span data-ttu-id="7a5db-128">Jakmile odešlete méně, vysoce cílové nabízená oznámení, získáte lepší návratnost investic.</span><span class="sxs-lookup"><span data-stu-id="7a5db-128">When you send fewer, highly targeted push notifications, you get a better ROI.</span></span>

 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a><span data-ttu-id="7a5db-130">Segmenty, které můžete vytvořit na základě hlavní Azure Mobile Engagement prvků:</span><span class="sxs-lookup"><span data-stu-id="7a5db-130">Segments you can create based on the major Azure Mobile Engagement elements:</span></span>
* <span data-ttu-id="7a5db-131">Události: vytvoření segment tohoto cíle jeden konkrétní události aplikace, která došlo k více než dvakrát týdně.</span><span class="sxs-lookup"><span data-stu-id="7a5db-131">Event: create a segment that targets one specific event of the application that happened more than twice a week.</span></span> 
* <span data-ttu-id="7a5db-132">Relace: vytvoření segment uživatelů, kteří použili aplikaci více než 5 výskyty minulého týdne.</span><span class="sxs-lookup"><span data-stu-id="7a5db-132">Session: create a segment of users that have used the application more than 5 times last week.</span></span>
* <span data-ttu-id="7a5db-133">Aktivita: vytvořte segment uživatelů, kteří použili jednu stránku nebo obsah vyšší nebo nižší než 10 čas poslední měsíc.</span><span class="sxs-lookup"><span data-stu-id="7a5db-133">Activity: create a segment of users that have used one page or content more or less than 10 time last month.</span></span>
* <span data-ttu-id="7a5db-134">Úlohu: vytvořte segment uživatelů, kteří dokončili úlohy maximálně dvakrát denně.</span><span class="sxs-lookup"><span data-stu-id="7a5db-134">Job: create a segment of users that have completed a job more than twice a day.</span></span>
* <span data-ttu-id="7a5db-135">Havárií: vytvořte segment všechny uživatele, kteří předtím havárie více než 10 výskyty minulého týdne.</span><span class="sxs-lookup"><span data-stu-id="7a5db-135">Crash: create a segment of all the users that have had a crash more than 10 times last week.</span></span> <span data-ttu-id="7a5db-136">(Tento segment omluva nebo i kupónů může push!)</span><span class="sxs-lookup"><span data-stu-id="7a5db-136">(You could push this segment with an apology or even a coupon!)</span></span>
* <span data-ttu-id="7a5db-137">Chyba: vytvořte segment všechny uživatele, kteří mají došlo k chybě víc než 100krát poslední 3 dny.</span><span class="sxs-lookup"><span data-stu-id="7a5db-137">Error: create a segment of all the users that have had an error more than 100 times last 3 days.</span></span>
* <span data-ttu-id="7a5db-138">Informace o aplikaci: vytvořte segment, který cílí vlastní informace o aplikaci, kterým došlo během posledních 25 dnů.</span><span class="sxs-lookup"><span data-stu-id="7a5db-138">App Info: create a segment that targets a custom App Info that happened during the last 25 days.</span></span>
  
  ![segments4][38]

<span data-ttu-id="7a5db-140">Pokud chcete použít Segment s optimálně, musíte provést vlastní integrace sady SDK v aplikaci s označování plánu značek "Informace o aplikaci".</span><span class="sxs-lookup"><span data-stu-id="7a5db-140">To use Segment in an optimal way, you must have done a customized integration of the SDK in your application with a tagging plan of "App Info" tags.</span></span>
<span data-ttu-id="7a5db-141">Potom přejděte na domovskou stránku rozhraní, vyberte aplikaci a klikněte na v části "Segmenty".</span><span class="sxs-lookup"><span data-stu-id="7a5db-141">Then, go to the home page of the interface, select the application you want and click on the "Segments" section.</span></span>

1. <span data-ttu-id="7a5db-142">Vyberte v části "Segmenty".</span><span class="sxs-lookup"><span data-stu-id="7a5db-142">Select the "Segments" section.</span></span>
2. <span data-ttu-id="7a5db-143">Klikněte na "nový segment" tlačítko vytvořte nový segment.</span><span class="sxs-lookup"><span data-stu-id="7a5db-143">Click on the "New segment" button to create a new segment.</span></span>

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a><span data-ttu-id="7a5db-144">Skutečné životnosti příklad: Vytvoření jednoduché segmentu na základě informací "Relace"</span><span class="sxs-lookup"><span data-stu-id="7a5db-144">Real Life Example: Create a simple segment based on "Session" information</span></span>
<span data-ttu-id="7a5db-145">Vytvořte segment všichni koncoví uživatelé, kteří použili aplikaci alespoň 50 časy během posledního týdne.</span><span class="sxs-lookup"><span data-stu-id="7a5db-145">Create a segment of all the end-users that have used your app at least 50 times in the last week.</span></span> <span data-ttu-id="7a5db-146">Z tohoto místa najít pouze koncovým uživatelům, kteří mají stráví nejméně 30 sekund ve vaší aplikaci na relaci.</span><span class="sxs-lookup"><span data-stu-id="7a5db-146">From there, find only the end-users that have spent at least 30 seconds in your app per session.</span></span> <span data-ttu-id="7a5db-147">Zobrazí všichni koncoví uživatelé, kteří mají pozitivní zkušenost ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7a5db-147">This will show all the end-users who have a positive experience in your app.</span></span> <span data-ttu-id="7a5db-148">Potom segment vytvořili může nabízená oznámení pro tyto koncové uživatele a požádat ho o hodnocení aplikace v úložišti.</span><span class="sxs-lookup"><span data-stu-id="7a5db-148">Then, the segment created could be used to push a notification to these end-users to ask them to rate your app in the store.</span></span>

 ![segments5][39]

1. <span data-ttu-id="7a5db-150">Chcete-li rychle najít v seznamu "Segment" Zadejte název segment.</span><span class="sxs-lookup"><span data-stu-id="7a5db-150">Give your segment a name in order to find it quickly in the "Segment" list.</span></span>
2. <span data-ttu-id="7a5db-151">Klikněte na tlačítko "Vytvořit".</span><span class="sxs-lookup"><span data-stu-id="7a5db-151">Click on the "Create" button.</span></span>
   
   ![segments6][40]

<span data-ttu-id="7a5db-153">Vyberte relaci.</span><span class="sxs-lookup"><span data-stu-id="7a5db-153">Select Session.</span></span>

 ![segments7][41]

1. <span data-ttu-id="7a5db-155">Vyberte dobu "Poslední týden".</span><span class="sxs-lookup"><span data-stu-id="7a5db-155">Select the period of "Last week".</span></span>
2. <span data-ttu-id="7a5db-156">Klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="7a5db-156">Click next.</span></span>
   
   ![segments8][42]
3. <span data-ttu-id="7a5db-158">Vyberte operátor, který je v seznamu relevantní: =; ≥, ≤.</span><span class="sxs-lookup"><span data-stu-id="7a5db-158">Select the Operator that is relevant among the list: =; ≥, ≤.</span></span>
4. <span data-ttu-id="7a5db-159">Zadejte počet chcete.</span><span class="sxs-lookup"><span data-stu-id="7a5db-159">Enter the Count you want.</span></span>
5. <span data-ttu-id="7a5db-160">Vyberte, chcete, aby výskyt.</span><span class="sxs-lookup"><span data-stu-id="7a5db-160">Select the Occurrence you want.</span></span> 
6. <span data-ttu-id="7a5db-161">Klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="7a5db-161">Click next.</span></span>
   <span data-ttu-id="7a5db-162">V tomto příkladu je nastaven tak přes poslední týden, odpovídající uživatelé, kteří provedli alespoň 50 relací.</span><span class="sxs-lookup"><span data-stu-id="7a5db-162">This example is set so that over the last week, match users that have made at least 50 sessions.</span></span>
   
   ![segments9][43]

<span data-ttu-id="7a5db-164">V případě segmentace relace můžete délka relace jako kritérium.</span><span class="sxs-lookup"><span data-stu-id="7a5db-164">For the Session segmentation, you can choose the length per session as a criteria.</span></span>

1. <span data-ttu-id="7a5db-165">Vyberte operátor, ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="7a5db-165">Select the Operator from the list.</span></span>
2. <span data-ttu-id="7a5db-166">Zadejte délku na relaci.</span><span class="sxs-lookup"><span data-stu-id="7a5db-166">Provide the Length per session.</span></span>
3. <span data-ttu-id="7a5db-167">Klikněte na Další.</span><span class="sxs-lookup"><span data-stu-id="7a5db-167">Click Next.</span></span>
   <span data-ttu-id="7a5db-168">V tomto příkladu se říká, že přes všechny relace, byla segmentované v oddílu výskyt, vyberte pouze uživatelé, kteří mají stráví déle než 30 sekund na relaci.</span><span class="sxs-lookup"><span data-stu-id="7a5db-168">In this example, it says that over all the sessions that have been segmented on the occurrence section, select only the users that have spent more than 30 seconds per session.</span></span>
   
   ![segments10][44]

<span data-ttu-id="7a5db-170">Chcete-li načíst trychtýře. úplný název kritériem a klikněte na tlačítko Dokončit.</span><span class="sxs-lookup"><span data-stu-id="7a5db-170">Name your criterion in order to retrieve it in the complete funnel, and click Finish.</span></span>

 ![segments11][45]

<span data-ttu-id="7a5db-172">Po dokončení nastavení kritériem se zobrazí v segmentu trychtýřového grafu.</span><span class="sxs-lookup"><span data-stu-id="7a5db-172">When you have finished setting up your criterion, it will appear in the segment funnel.</span></span>
<span data-ttu-id="7a5db-173">Protože segment je na základě analýzy dat, se vypočítávají segmenty jednou za den.</span><span class="sxs-lookup"><span data-stu-id="7a5db-173">Because a segment is based on analytics data, segments are computed once per day.</span></span>
<span data-ttu-id="7a5db-174">V tomto příkladu 47,7 % celkový koncoví uživatelé shodná s kritériem.</span><span class="sxs-lookup"><span data-stu-id="7a5db-174">In this example, 47,7% of the total end-users matched the criterion.</span></span> <span data-ttu-id="7a5db-175">Měly by být uživatele, kteří mají dobrou prostředí a bude může zajistit vyšší hodnocení, pokud je nabízená oznámení, které je vyzývají k hodnocení aplikace v úložišti.</span><span class="sxs-lookup"><span data-stu-id="7a5db-175">They should be the users who have had a good experience and will be likely to provide a higher rating if you push them a notification asking them to rate the app in the store.</span></span>

## <a name="see-also"></a><span data-ttu-id="7a5db-176">Viz také</span><span class="sxs-lookup"><span data-stu-id="7a5db-176">See also</span></span>
* <span data-ttu-id="7a5db-177">[Koncepty][Link 6]</span><span class="sxs-lookup"><span data-stu-id="7a5db-177">[Concepts][Link 6]</span></span>
* <span data-ttu-id="7a5db-178">[Řešení potíží s příručce služby][Link 24]</span><span class="sxs-lookup"><span data-stu-id="7a5db-178">[Troubleshooting Guide Service][Link 24]</span></span>

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

