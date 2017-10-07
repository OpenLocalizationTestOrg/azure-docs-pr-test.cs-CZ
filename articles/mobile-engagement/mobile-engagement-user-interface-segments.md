---
title: "aaaAzure Mobile Engagement uživatelské rozhraní – segmenty"
description: "Zjistěte, jak toocreate a správa segmentů uživatelů tooidentify vzorce pomocí Azure Mobile Engagement"
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
ms.openlocfilehash: bb214c45d05ebfbf243978658a7e331d4a7c6e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-segments-of-users-tooidentify-usage-patterns"></a><span data-ttu-id="a2bea-103">Jak toocreate a správa segmentů uživatelů tooidentify vzorce</span><span class="sxs-lookup"><span data-stu-id="a2bea-103">How toocreate and manage segments of users tooidentify usage patterns</span></span>
<span data-ttu-id="a2bea-104">Tento článek popisuje hello **SEGMENTY** kartě hello **Mobile Engagement** portálu.</span><span class="sxs-lookup"><span data-stu-id="a2bea-104">This article describes hello **SEGMENTS** tab of hello **Mobile Engagement** portal.</span></span> <span data-ttu-id="a2bea-105">Použít hello **Mobile Engagement** portálu toomonitor a spravovat své mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2bea-105">You use hello **Mobile Engagement** portal toomonitor and manage your mobile apps.</span></span>

<span data-ttu-id="a2bea-106">část segmenty Hello hello uživatelského rozhraní můžete toowork na segmentace uživatelů na základě hello různé chování a analýz, které můžete získat z aplikace hello a můžete také přistupovat prostřednictvím hello segmenty rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2bea-106">hello Segments section of hello UI allows you toowork on segmenting your users based on hello different behavior and analytics that you can get from hello application and can also access through hello Segments API.</span></span> <span data-ttu-id="a2bea-107">Segmenty se vypočítávají nejprve 24 hodin po jejich vytvoření a jejich jsou přepočítávány každých 24 hodin na základě hello nejnovější analytics informací.</span><span class="sxs-lookup"><span data-stu-id="a2bea-107">Segments are first computed 24 hours after they are created, and they are recomputed every 24 hours based on hello latest analytics information.</span></span> <span data-ttu-id="a2bea-108">Jakmile se počítá segment, zobrazuje graf "Den tooday historie" každý den.</span><span class="sxs-lookup"><span data-stu-id="a2bea-108">Once a segment is calculated, it displays a "Day tooday history" chart each day.</span></span>

> [!NOTE]
> <span data-ttu-id="a2bea-109">Mnoho oddílů hello **Mobile Engagement** portál uživatelského rozhraní obsahovat hello **zobrazit NÁPOVĚDU k** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a2bea-109">Many sections of hello **Mobile Engagement** portal UI contain hello **SHOW HELP** button.</span></span> <span data-ttu-id="a2bea-110">Stisknutím tohoto tlačítka tooget další kontextové informace o oddílu.</span><span class="sxs-lookup"><span data-stu-id="a2bea-110">Press this button tooget more contextual information about a section.</span></span>
> 
> 

## <a name="create-segments"></a><span data-ttu-id="a2bea-111">Vytvoření segmentů</span><span class="sxs-lookup"><span data-stu-id="a2bea-111">Create Segments</span></span>
<span data-ttu-id="a2bea-112">Můžete vytvořit na základě až too10 kritéria na určitou dobu až too60 dní v hello segment minulosti z oddílu analytics hello.</span><span class="sxs-lookup"><span data-stu-id="a2bea-112">You can create a segment based on up too10 criteria on a specific period up too60 days in hello past from hello analytics section.</span></span> <span data-ttu-id="a2bea-113">Můžete například vytvořit segment podle hello lidé, kteří mají zobrazit určité stránky nebo vyhledávat konkrétní obsahu v rámci vaší aplikace v rámci hello posledních 10 dnů.</span><span class="sxs-lookup"><span data-stu-id="a2bea-113">For example, you can create a segment based on hello people who have viewed certain pages or searched for specific content within your app within hello last 10 days.</span></span> <span data-ttu-id="a2bea-114">Tyto informace jsou k dispozici v části analytics hello.</span><span class="sxs-lookup"><span data-stu-id="a2bea-114">This information is available in hello analytics section.</span></span> <span data-ttu-id="a2bea-115">Ano, můžete ho použít toocreate segment a pak nastavit nabízených oznámení tootarget tuto podmnožinu uživatelů tooget je toocome back toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2bea-115">So, you can use it toocreate a segment, and then set up a push notification tootarget this subset of users tooget them toocome back toohello application.</span></span> 

> [!NOTE]
> <span data-ttu-id="a2bea-116">Jakmile je vypočítána segment, nemůže být upraven; může být pouze klonovat (zkopírovaný) nebo zničení (odstraněno).</span><span class="sxs-lookup"><span data-stu-id="a2bea-116">Once a segment has been calculated, it cannot be edited; it can only be cloned (copied) or destroyed (deleted).</span></span> <span data-ttu-id="a2bea-117">Segment můžete klonovat v rámci hello stejnou aplikaci (s hello stejné AppID), a můžete ho také klonovat do jiných aplikací (s jinou AppID).</span><span class="sxs-lookup"><span data-stu-id="a2bea-117">A segment can be cloned within hello same application (with hello same AppID), and it can also be cloned into other applications (with a different AppID).</span></span> 

 ![segments1][35] 

## <a name="examples-segments"></a><span data-ttu-id="a2bea-119">Příklady segmenty</span><span class="sxs-lookup"><span data-stu-id="a2bea-119">Examples Segments</span></span>
 ![segments2][36]

<span data-ttu-id="a2bea-121">Segmenty umožní koncovým uživatelům hello toosegment vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2bea-121">Segments allow you toosegment hello end-users of your application.</span></span>
<span data-ttu-id="a2bea-122">Segmentace vaši uživatelé je důležité marketingové strategie.</span><span class="sxs-lookup"><span data-stu-id="a2bea-122">Segmenting your users is an important marketing strategy.</span></span> <span data-ttu-id="a2bea-123">Azure Mobile Engagement vám umožní tooget historických dat a vytvořte vlastní segmenty.</span><span class="sxs-lookup"><span data-stu-id="a2bea-123">Azure Mobile Engagement allows you tooget historic data and create custom segments.</span></span> <span data-ttu-id="a2bea-124">Tento výkonný nástroj umožňuje toolearn o zákazníkům ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a2bea-124">This powerful tool enables you toolearn about your customers’ experience in your application.</span></span> <span data-ttu-id="a2bea-125">Můžete snadno analyzovat segmenty a použít jako cíle nabízené segmenty.</span><span class="sxs-lookup"><span data-stu-id="a2bea-125">You can easily analyze your segments and use your segments as push targets.</span></span>
<span data-ttu-id="a2bea-126">Je běžné případ použití, které chcete toosend nabízená oznámení tooencourage vaši koncoví uživatelé toorate aplikace v úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="a2bea-126">A common use-case is that you want toosend a push a notification tooencourage your end-users toorate your application in hello store.</span></span> <span data-ttu-id="a2bea-127">Místo odesílání oznámení tooall koncovým uživatelům, můžete vytvořit segment, který byste zadat jenom uživatelé, kteří použili aplikaci každý den pro hello poslední měsíc a neměla skvělé uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="a2bea-127">Rather than sending a notification tooall your end-users, you can create a segment that would specify only users that have used your application every day for hello last month and have had a great user experience.</span></span> <span data-ttu-id="a2bea-128">Jakmile odešlete méně, vysoce cílové nabízená oznámení, získáte lepší návratnost investic.</span><span class="sxs-lookup"><span data-stu-id="a2bea-128">When you send fewer, highly targeted push notifications, you get a better ROI.</span></span>

 ![segments3][37]

### <a name="segments-you-can-create-based-on-hello-major-azure-mobile-engagement-elements"></a><span data-ttu-id="a2bea-130">Segmenty, které můžete vytvořit podle hello důležité elementy: Azure Mobile Engagement:</span><span class="sxs-lookup"><span data-stu-id="a2bea-130">Segments you can create based on hello major Azure Mobile Engagement elements:</span></span>
* <span data-ttu-id="a2bea-131">Události: vytvoření segment tohoto cíle jeden konkrétní události hello aplikace, která došlo k více než dvakrát týdně.</span><span class="sxs-lookup"><span data-stu-id="a2bea-131">Event: create a segment that targets one specific event of hello application that happened more than twice a week.</span></span> 
* <span data-ttu-id="a2bea-132">Relace: vytvoření segment uživatelů, kteří použili aplikaci hello více než 5 výskyty minulého týdne.</span><span class="sxs-lookup"><span data-stu-id="a2bea-132">Session: create a segment of users that have used hello application more than 5 times last week.</span></span>
* <span data-ttu-id="a2bea-133">Aktivita: vytvořte segment uživatelů, kteří použili jednu stránku nebo obsah vyšší nebo nižší než 10 čas poslední měsíc.</span><span class="sxs-lookup"><span data-stu-id="a2bea-133">Activity: create a segment of users that have used one page or content more or less than 10 time last month.</span></span>
* <span data-ttu-id="a2bea-134">Úlohu: vytvořte segment uživatelů, kteří dokončili úlohy maximálně dvakrát denně.</span><span class="sxs-lookup"><span data-stu-id="a2bea-134">Job: create a segment of users that have completed a job more than twice a day.</span></span>
* <span data-ttu-id="a2bea-135">Havárií: vytvořte segment všech uživatelů hello, které má havárie více než 10 výskyty minulého týdne.</span><span class="sxs-lookup"><span data-stu-id="a2bea-135">Crash: create a segment of all hello users that have had a crash more than 10 times last week.</span></span> <span data-ttu-id="a2bea-136">(Tento segment omluva nebo i kupónů může push!)</span><span class="sxs-lookup"><span data-stu-id="a2bea-136">(You could push this segment with an apology or even a coupon!)</span></span>
* <span data-ttu-id="a2bea-137">Chyba: vytvořte segment všechny hello uživatele, kteří mají došlo k chybě víc než 100krát poslední 3 dny.</span><span class="sxs-lookup"><span data-stu-id="a2bea-137">Error: create a segment of all hello users that have had an error more than 100 times last 3 days.</span></span>
* <span data-ttu-id="a2bea-138">Informace o aplikaci: vytvořte segment, který cílí vlastní informace o aplikaci, která nastala v průběhu hello posledních 25 dnů.</span><span class="sxs-lookup"><span data-stu-id="a2bea-138">App Info: create a segment that targets a custom App Info that happened during hello last 25 days.</span></span>
  
  ![segments4][38]

<span data-ttu-id="a2bea-140">toouse Segment s optimálně, musíte provést vlastní integrace hello SDK v aplikaci s označování plánu značek "Informace o aplikaci".</span><span class="sxs-lookup"><span data-stu-id="a2bea-140">toouse Segment in an optimal way, you must have done a customized integration of hello SDK in your application with a tagging plan of "App Info" tags.</span></span>
<span data-ttu-id="a2bea-141">Pak přejděte toohello domovskou stránku hello rozhraní, vyberte hello aplikace a klikněte na v oddílu "Segmenty" hello.</span><span class="sxs-lookup"><span data-stu-id="a2bea-141">Then, go toohello home page of hello interface, select hello application you want and click on hello "Segments" section.</span></span>

1. <span data-ttu-id="a2bea-142">Vyberte část "Segmenty" hello.</span><span class="sxs-lookup"><span data-stu-id="a2bea-142">Select hello "Segments" section.</span></span>
2. <span data-ttu-id="a2bea-143">Klikněte na hello "Nový segment" tlačítko toocreate nový segment.</span><span class="sxs-lookup"><span data-stu-id="a2bea-143">Click on hello "New segment" button toocreate a new segment.</span></span>

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a><span data-ttu-id="a2bea-144">Skutečné životnosti příklad: Vytvoření jednoduché segmentu na základě informací "Relace"</span><span class="sxs-lookup"><span data-stu-id="a2bea-144">Real Life Example: Create a simple segment based on "Session" information</span></span>
<span data-ttu-id="a2bea-145">Vytvořte segment všichni hello koncoví uživatelé, kteří použili aplikaci alespoň 50 časy v hello minulého týdne.</span><span class="sxs-lookup"><span data-stu-id="a2bea-145">Create a segment of all hello end-users that have used your app at least 50 times in hello last week.</span></span> <span data-ttu-id="a2bea-146">Z tohoto místa najít pouze hello uživatelů, kteří mají stráví nejméně 30 sekund ve vaší aplikaci na relaci.</span><span class="sxs-lookup"><span data-stu-id="a2bea-146">From there, find only hello end-users that have spent at least 30 seconds in your app per session.</span></span> <span data-ttu-id="a2bea-147">Zobrazí všechny hello koncoví uživatelé, kteří mají pozitivní zkušenost ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a2bea-147">This will show all hello end-users who have a positive experience in your app.</span></span> <span data-ttu-id="a2bea-148">Potom hello segment vytvořili může být použité toopush tooask koncoví uživatelé toothese oznámení je toorate aplikace v rámci hello uložit.</span><span class="sxs-lookup"><span data-stu-id="a2bea-148">Then, hello segment created could be used toopush a notification toothese end-users tooask them toorate your app in hello store.</span></span>

 ![segments5][39]

1. <span data-ttu-id="a2bea-150">Pojmenujte segment v pořadí toofind rychle v seznamu "Segment" hello.</span><span class="sxs-lookup"><span data-stu-id="a2bea-150">Give your segment a name in order toofind it quickly in hello "Segment" list.</span></span>
2. <span data-ttu-id="a2bea-151">Klikněte na hello tlačítko "Vytvořit".</span><span class="sxs-lookup"><span data-stu-id="a2bea-151">Click on hello "Create" button.</span></span>
   
   ![segments6][40]

<span data-ttu-id="a2bea-153">Vyberte relaci.</span><span class="sxs-lookup"><span data-stu-id="a2bea-153">Select Session.</span></span>

 ![segments7][41]

1. <span data-ttu-id="a2bea-155">Vyberte dobu hello "Poslední týden".</span><span class="sxs-lookup"><span data-stu-id="a2bea-155">Select hello period of "Last week".</span></span>
2. <span data-ttu-id="a2bea-156">Klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="a2bea-156">Click next.</span></span>
   
   ![segments8][42]
3. <span data-ttu-id="a2bea-158">Hello vyberte operátor, který je relevantní mezi seznamu hello: =; ≥, ≤.</span><span class="sxs-lookup"><span data-stu-id="a2bea-158">Select hello Operator that is relevant among hello list: =; ≥, ≤.</span></span>
4. <span data-ttu-id="a2bea-159">Zadejte počet chcete hello.</span><span class="sxs-lookup"><span data-stu-id="a2bea-159">Enter hello Count you want.</span></span>
5. <span data-ttu-id="a2bea-160">Vyberte hello výskyt chcete.</span><span class="sxs-lookup"><span data-stu-id="a2bea-160">Select hello Occurrence you want.</span></span> 
6. <span data-ttu-id="a2bea-161">Klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="a2bea-161">Click next.</span></span>
   <span data-ttu-id="a2bea-162">V tomto příkladu je nastaven tak, že přes hello poslední týden, shodu uživatele, kteří provedli alespoň 50 relací.</span><span class="sxs-lookup"><span data-stu-id="a2bea-162">This example is set so that over hello last week, match users that have made at least 50 sessions.</span></span>
   
   ![segments9][43]

<span data-ttu-id="a2bea-164">Pro hello relace segmentace můžete hello délka relace jako kritérium.</span><span class="sxs-lookup"><span data-stu-id="a2bea-164">For hello Session segmentation, you can choose hello length per session as a criteria.</span></span>

1. <span data-ttu-id="a2bea-165">Vyberte ze seznamu hello hello operátor.</span><span class="sxs-lookup"><span data-stu-id="a2bea-165">Select hello Operator from hello list.</span></span>
2. <span data-ttu-id="a2bea-166">Zadejte hello délka relace.</span><span class="sxs-lookup"><span data-stu-id="a2bea-166">Provide hello Length per session.</span></span>
3. <span data-ttu-id="a2bea-167">Klikněte na Další.</span><span class="sxs-lookup"><span data-stu-id="a2bea-167">Click Next.</span></span>
   <span data-ttu-id="a2bea-168">V tomto příkladu se říká, že na všech hello relací, které byly segmentované v oddílu hello výskyt, vyberte pouze hello uživatele, které strávily déle než 30 sekund na relaci.</span><span class="sxs-lookup"><span data-stu-id="a2bea-168">In this example, it says that over all hello sessions that have been segmented on hello occurrence section, select only hello users that have spent more than 30 seconds per session.</span></span>
   
   ![segments10][44]

<span data-ttu-id="a2bea-170">Název kritériem v pořadí tooretrieve bude v hello dokončit Trychtýřový a klikněte na tlačítko Dokončit.</span><span class="sxs-lookup"><span data-stu-id="a2bea-170">Name your criterion in order tooretrieve it in hello complete funnel, and click Finish.</span></span>

 ![segments11][45]

<span data-ttu-id="a2bea-172">Po dokončení nastavení kritériem se zobrazí v segmentu hello trychtýřového grafu.</span><span class="sxs-lookup"><span data-stu-id="a2bea-172">When you have finished setting up your criterion, it will appear in hello segment funnel.</span></span>
<span data-ttu-id="a2bea-173">Protože segment je na základě analýzy dat, se vypočítávají segmenty jednou za den.</span><span class="sxs-lookup"><span data-stu-id="a2bea-173">Because a segment is based on analytics data, segments are computed once per day.</span></span>
<span data-ttu-id="a2bea-174">V tomto příkladu 47,7 % celkový koncoví uživatelé hello shodná hello kritérium.</span><span class="sxs-lookup"><span data-stu-id="a2bea-174">In this example, 47,7% of hello total end-users matched hello criterion.</span></span> <span data-ttu-id="a2bea-175">Měly by být hello uživatelů, kteří mají měl kvalitní a bude se pravděpodobně tooprovide vyšší hodnocení, pokud je nabízená oznámení výzvou toorate hello aplikaci ve hello store.</span><span class="sxs-lookup"><span data-stu-id="a2bea-175">They should be hello users who have had a good experience and will be likely tooprovide a higher rating if you push them a notification asking them toorate hello app in hello store.</span></span>

## <a name="see-also"></a><span data-ttu-id="a2bea-176">Viz také</span><span class="sxs-lookup"><span data-stu-id="a2bea-176">See also</span></span>
* <span data-ttu-id="a2bea-177">[Koncepty][Link 6]</span><span class="sxs-lookup"><span data-stu-id="a2bea-177">[Concepts][Link 6]</span></span>
* <span data-ttu-id="a2bea-178">[Řešení potíží s příručce služby][Link 24]</span><span class="sxs-lookup"><span data-stu-id="a2bea-178">[Troubleshooting Guide Service][Link 24]</span></span>

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

