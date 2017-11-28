---
title: "aaaAzure Mobile Engagement uživatelské rozhraní - Reach"
description: "Zjistěte, jak tooreach toohello uživatelů vaší aplikace pomocí nabízených oznámení pomocí Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: d96e2590-08dd-4481-a352-2c18f26a1643
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 40d5162ddeccec82c2c9f5b0d72b4cb10c9ddc38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreach-out-toohello-users-of-your-application-with-push-notifications"></a><span data-ttu-id="38d97-103">Jak tooreach toohello uživatelů vaší aplikace pomocí nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="38d97-103">How tooreach out toohello users of your application with push notifications</span></span>
<span data-ttu-id="38d97-104">Tento článek popisuje hello **dosáhnout** kartě hello **Mobile Engagement** portálu.</span><span class="sxs-lookup"><span data-stu-id="38d97-104">This article describes hello **REACH** tab of hello **Mobile Engagement** portal.</span></span> <span data-ttu-id="38d97-105">Použít hello **Mobile Engagement** portálu toomonitor a spravovat své mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="38d97-105">You use hello **Mobile Engagement** portal toomonitor and manage your mobile apps.</span></span> <span data-ttu-id="38d97-106">Všimněte si, že toostart hello portálu musíte nejprve toocreate **Azure Mobile Engagement** účtu.</span><span class="sxs-lookup"><span data-stu-id="38d97-106">Note that toostart using hello portal you first need toocreate an **Azure Mobile Engagement** account.</span></span> <span data-ttu-id="38d97-107">Další informace najdete v tématu [vytvoření účtu Azure Mobile Engagement](mobile-engagement-create.md).</span><span class="sxs-lookup"><span data-stu-id="38d97-107">For more information, see [Create an Azure Mobile Engagement account](mobile-engagement-create.md).</span></span>

<span data-ttu-id="38d97-108">Hello připojit části hello uživatelského rozhraní je hello nabízené kampaně nástroj pro správu kde můžete vytvořit nebo upravit nebo aktivovat nebo dokončit nebo monitorování a získat statistiky kampaní nabízených oznámení a funkce, které je přístupný také prostřednictvím hello Reach API (a některé prvky hello nízkou úroveň Push rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="38d97-108">hello Reach section of hello UI is hello Push campaign management tool where you can create/edit/activate/finish/monitor and get statistics on Push notification campaigns and features that can also be accessed via hello Reach API (and some elements of hello low level Push API).</span></span> <span data-ttu-id="38d97-109">Mějte na paměti, že ať používáte hello rozhraní API nebo hello uživatelského rozhraní, budete potřebovat toointegrate Azure Mobile Engagement a Reach do své aplikace pro každou platformu s hello SDK, abyste mohli používat kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="38d97-109">Remember that whether you are using hello APIs or hello UI, you will need toointegrate both Azure Mobile Engagement and Reach into your application for each platform with hello SDK before you can use Reach campaigns.</span></span>

> [!NOTE]
> <span data-ttu-id="38d97-110">Mnoho oddílů hello **Mobile Engagement** portál uživatelského rozhraní obsahovat hello **zobrazit NÁPOVĚDU k** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="38d97-110">Many sections of hello **Mobile Engagement** portal UI contain hello **SHOW HELP** button.</span></span> <span data-ttu-id="38d97-111">Stisknutím tohoto tlačítka tooget další kontextové informace o oddílu.</span><span class="sxs-lookup"><span data-stu-id="38d97-111">Press this button tooget more contextual information about a section.</span></span>
> 
> 

## <a name="four-types-of-push-notifications"></a><span data-ttu-id="38d97-112">Čtyři typy nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="38d97-112">Four types of Push notifications</span></span>
1. <span data-ttu-id="38d97-113">Oznámení – umožňují toousers toosend reklamní zprávy, který přesměruje je umístění tooanother uvnitř vaší aplikace nebo toosend je webová stránka tooa nebo uložit mimo vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="38d97-113">Announcements - allow you toosend advertising messages toousers that redirect them tooanother location inside your app or toosend them tooa webpage or store outside of your app.</span></span> 
2. <span data-ttu-id="38d97-114">Hlasování - povolit informace toogather koncovým uživatelům ve formě odpovědí na otázky.</span><span class="sxs-lookup"><span data-stu-id="38d97-114">Polls - allow you toogather information from end users by asking them questions.</span></span>
3. <span data-ttu-id="38d97-115">Datová oznámení - povolit toosend base64 nebo binární datový soubor.</span><span class="sxs-lookup"><span data-stu-id="38d97-115">Data Pushes - allow you toosend a binary or base64 data file.</span></span> <span data-ttu-id="38d97-116">se Hello informace obsažené v datovém oznámení odeslaných tooyour aplikace toomodify aktuální zkušeností uživatelů ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="38d97-116">hello information contained in a data push is sent tooyour application toomodify your users' current experience in your app.</span></span> <span data-ttu-id="38d97-117">Aplikace musí toobe možné tooprocess hello dat v datových oznámeních.</span><span class="sxs-lookup"><span data-stu-id="38d97-117">Your application needs toobe able tooprocess hello data in a data push.</span></span>

## <a name="campaign-details"></a><span data-ttu-id="38d97-118">Podrobnosti kampaně</span><span class="sxs-lookup"><span data-stu-id="38d97-118">Campaign Details</span></span>
<span data-ttu-id="38d97-119">Můžete upravit, klonování, odstranit nebo aktivovat kampaní, které nebyly ještě aktivována podržením ukazatele nad jejich názvy, nebo můžete kliknout na tooopen je.</span><span class="sxs-lookup"><span data-stu-id="38d97-119">You can edit, clone, delete, or activate campaigns that have not been activated yet by hovering over their names or you can click tooopen them.</span></span> <span data-ttu-id="38d97-120">Může klonovat kampaní, které již byla aktivována podržením ukazatele nad jejich názvy nebo můžete kliknout na tooopen je.</span><span class="sxs-lookup"><span data-stu-id="38d97-120">You can clone campaigns that have already been activated by hovering over their names or you can click tooopen them.</span></span> <span data-ttu-id="38d97-121">Jakmile byl aktivován však nelze změnit na kampaň.</span><span class="sxs-lookup"><span data-stu-id="38d97-121">However, you can't change a campaign once it has been activated.</span></span>

![Reach1][18]

## <a name="reach-feedback"></a><span data-ttu-id="38d97-123">Dosažení zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="38d97-123">Reach Feedback</span></span>
<span data-ttu-id="38d97-124">Klikněte na **statistiky** toosee hello podrobnosti kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="38d97-124">Click on **Statistics** toosee hello details of a Reach campaign.</span></span> <span data-ttu-id="38d97-125">Hello **jednoduché** zobrazení nabízí vizuální znázornění hello tvar pruhový graf sloupec o co se stalo po kampaň aktivovala.</span><span class="sxs-lookup"><span data-stu-id="38d97-125">hello **Simple** view provides a visual representation in hello form of a column bar graph about what happened after a campaign was activated.</span></span> <span data-ttu-id="38d97-126">Hello **Upřesnit** zobrazení obsahuje podrobnější informace o hello nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="38d97-126">hello **Advanced** view provides more granular details about hello push campaign.</span></span> <span data-ttu-id="38d97-127">Tyto údaje nebudou dostupné při odesílání zkušební kampaně tj nabízené odeslané tooa testovací zařízení.</span><span class="sxs-lookup"><span data-stu-id="38d97-127">These details will not be available if you are sending a test campaign i.e. a push sent tooa test device.</span></span> <span data-ttu-id="38d97-128">Zde je, jak by měl interpretovat tyto podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="38d97-128">Here is how you should interpret these details:</span></span>

1. <span data-ttu-id="38d97-129">**Poslat** -určuje hello počet zpráv nabídnutých toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="38d97-129">**Pushed** - This specifies hello number of messages pushed toohello devices.</span></span> <span data-ttu-id="38d97-130">Tento počet bude záviset na hello cílovou skupinu, kterou jste zadali při vytváření kampaně nabízených hello.</span><span class="sxs-lookup"><span data-stu-id="38d97-130">This number will depend on hello target audience you specified while creating hello push campaign.</span></span> <span data-ttu-id="38d97-131">Pokud nezadáte žádné cílové skupiny, pak tato nabízená bude odesílat tooall hello zaregistrované zařízení.</span><span class="sxs-lookup"><span data-stu-id="38d97-131">If you do not specify any target audience, then this push will be sent out tooall hello registered devices.</span></span> <span data-ttu-id="38d97-132">Podobně jako všechny ostatní nabízených služeb, jsme není nabízená oznámení hello přímo toohello zařízení, ale místo toho je push příslušné platformy toohello konkrétní služeb nabízených oznámení (PNS - APNS nebo GCM/WNS) tak, aby se doručení oznámení hello toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="38d97-132">Like all other push services, we do not push hello notifications directly toohello devices but instead push them toohello respective platform specific Push Notification Services (PNS - APNS/GCM/WNS) so that they can deliver hello notifications toohello devices.</span></span> 
2. <span data-ttu-id="38d97-133">**Doručit** -určuje hello počet zpráv, které jsou úspěšně doručil hello systém PNS toohello zařízení a potvrzené jako přijaté Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="38d97-133">**Delivered** - This specifies hello number of messages which are successfully delivered by hello PNS toohello device and acknowledged as received by Mobile Engagement SDK.</span></span> 
   
   <span data-ttu-id="38d97-134">*Důvody pro doručené počet je menší než počet stisknutí:*</span><span class="sxs-lookup"><span data-stu-id="38d97-134">*Reasons for Delivered count being less than Pushed count:*</span></span>
   
   1. <span data-ttu-id="38d97-135">Pokud uživatel hello odinstaloval hello aplikace ze zařízení hello ale hello systém PNS neví o něm v době hello pošleme nabízené toohello hello systém PNS bude zrušeno uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="38d97-135">If hello user has uninstalled hello app from hello device but hello PNS doesn't know about it at hello time we send hello push toohello PNS then hello message will be dropped.</span></span>
   2. <span data-ttu-id="38d97-136">Pokud zařízení hello má aplikace hello ale hello zařízeními jsou v režimu offline pro dlouhou dobu, pak hello systém PNS selže toodeliver hello zpráva toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="38d97-136">If hello device has hello app but hello devices themselves were offline for long periods of time, then hello PNS will fail toodeliver hello message toohello device.</span></span> 
   3. <span data-ttu-id="38d97-137">Pokud hello zprávu získat doručit toohello zařízení, ale hello Mobile Engagement SDK v aplikaci hello nerozpoznal hello obsah zprávy hello, pak zahodí zprávy.</span><span class="sxs-lookup"><span data-stu-id="38d97-137">If hello message does get delivered toohello device but hello Mobile Engagement SDK in hello app doesn’t recognize hello content of hello message, then it drops that message.</span></span> <span data-ttu-id="38d97-138">Toto může nastat, pokud hello přizpůsobení hello oznámení v aplikaci hello vygeneruje výjimku, která jsme catch hello SDK a drop uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="38d97-138">This could happen if hello customization of hello notification in hello app generates an exception which we catch in hello SDK and drop hello message.</span></span> <span data-ttu-id="38d97-139">Situace může také nastat, pokud aplikace hello na hello zařízení používáte verzi hello Mobile Engagement SDK, která není možné toounderstand hello novější verzi hello nabízená zpráva odeslaný hello platformy, ale toto je pouze v případě, že aplikace hello byl upgradován po hello oznámení byl odeslán z platformy service hello.</span><span class="sxs-lookup"><span data-stu-id="38d97-139">This could also occur if hello app on hello device is using a version of hello Mobile Engagement SDK which is not able toounderstand hello newer version of hello push message sent from hello platform but this is only when hello app was upgraded after hello notification was dispatched from hello service platform.</span></span> <span data-ttu-id="38d97-140">Hello **Upřesnit** kartě zjistit, kolik zpráv byly vyřazeny.</span><span class="sxs-lookup"><span data-stu-id="38d97-140">hello **Advanced** tab will tell how many messages were dropped.</span></span> 
   4. <span data-ttu-id="38d97-141">Na zařízeních s iOS někdy není získat doručeny zprávy je buď hello zařízení na nízký stav baterie. nebo pokud aplikace hello spotřebovává významné množství power při zpracování vzdáleného oznámení.</span><span class="sxs-lookup"><span data-stu-id="38d97-141">On iOS devices, messages sometimes do not get delivered if either hello device is on low battery or if hello app is consuming significant amount of power when processing remote notifications.</span></span> <span data-ttu-id="38d97-142">Jedná se o omezení hello zařízení iOS.</span><span class="sxs-lookup"><span data-stu-id="38d97-142">This is a limitation of hello iOS devices.</span></span>   
3. <span data-ttu-id="38d97-143">**Zobrazí** -určuje hello počet zpráv, které jsou úspěšně zobrazený toohello aplikace uživatele na zařízení hello hello tvar systémové oznámení nabízených nebo out z – aplikaci v centru oznámení hello nebo oznámení v aplikaci v rámci hello mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="38d97-143">**Displayed** - This specifies hello number of messages which are successfully shown toohello app user on hello device in hello form of a system push/out-of-app notification in hello notification center or an in-app notification within hello mobile app.</span></span>  <span data-ttu-id="38d97-144">Hello **Upřesnit** na kartě se zjistit, kolik byly systémová oznámení a kolik byly oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="38d97-144">hello **Advanced** tab will tell you how many were system notifications and how many were in-app notifications.</span></span> 
   
   <span data-ttu-id="38d97-145">*Důvody pro zobrazené počet je menší než počet doručené (čekání toobe zobrazí)*</span><span class="sxs-lookup"><span data-stu-id="38d97-145">*Reasons for Displayed count being less than Delivered count (waiting toobe displayed)*</span></span>
   
   1. <span data-ttu-id="38d97-146">Pokud kampaň oznámení hello měl koncové datum v jej, pak je možné, že byla doručena hello oznámení, ale když přišel čas hello tooopen a zobrazit ji toohello aplikace uživatele, byl již vypršela, a proto se nikdy zobrazit.</span><span class="sxs-lookup"><span data-stu-id="38d97-146">If hello notification campaign had an end date on it then it is possible that hello notification was delivered but when hello time came tooopen and display it toohello app user, it was already expired so it was never displayed.</span></span>   
   2. <span data-ttu-id="38d97-147">Pokud hello oznámení je oznámení v aplikaci pak hello oznámení se zobrazí pouze když uživatel aplikace hello otevře aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="38d97-147">If hello notification is an in-app notification then hello notification is only displayed when hello app user opens hello app.</span></span> <span data-ttu-id="38d97-148">V případech, kdy uživatel aplikace hello neotevřel aplikace hello hello SDK oznámí, že byla oznámení hello doručit ale ještě nebyla nezobrazí, dokud otevření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="38d97-148">In cases where hello app user hasn't opened hello app, hello SDK will report that hello notification was delivered but not yet displayed until hello app is opened.</span></span> 
   3. <span data-ttu-id="38d97-149">Pokud hello oznámení je oznámení v aplikaci a nakonfigurovat toobe zobrazen na konkrétní aktivity nebo obrazovce pak také hello oznámení se ohlásí jako doručen, ale ještě doručit, dokud není hello uživatel otevře aplikaci hello na konkrétní obrazovce.</span><span class="sxs-lookup"><span data-stu-id="38d97-149">If hello notification is an in-app notification and configured toobe shown on a specific activity/screen then also hello notification will be reported as delivered but not yet delivered until hello user opens hello app on a specific screen.</span></span> 
4. <span data-ttu-id="38d97-150">**Interakce uživatele** -určuje hello počet zpráv, které uživatel aplikace hello má zpracoval s a bude obsahovat hello zprávy, které jsou zpracované nebo ukončené.</span><span class="sxs-lookup"><span data-stu-id="38d97-150">**User Interactions** - This specifies hello number of messages which hello app user has interacted with and will include hello messages which are either actioned or exited.</span></span> 
   
   * <span data-ttu-id="38d97-151">*uživatel aplikace Hello může akce oznámení v některém z následujících způsobů hello:*</span><span class="sxs-lookup"><span data-stu-id="38d97-151">*hello app user can action a notification in either of hello following ways:*</span></span>
     
     1. <span data-ttu-id="38d97-152">Pokud hello oznámení systému nebo na více systémů z – aplikace se oznámení nebo oznámení v aplikaci odesílá jen oznámení potom hello aplikace uživatel klikne na hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="38d97-152">If hello notification is a system/out-of-app notification or an in-app notification sent as notification-only then hello app user clicks on hello notification.</span></span>
     2. <span data-ttu-id="38d97-153">Pokud hello oznámení je oznámení v aplikaci s textem nebo webové zobrazení nebo hlasování pak hello aplikace uživatel klikne na hello tlačítko akce v hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="38d97-153">If hello notification is an in-app notification with a text or web-view or polls then hello app user clicks on hello Action button in hello notification.</span></span>
     3. <span data-ttu-id="38d97-154">Pokud se oznámení hello oznámení v aplikaci s webové zobrazení pak hello aplikace uživatel klikne na na adrese URL hello webové pouze v zobrazení [Android]</span><span class="sxs-lookup"><span data-stu-id="38d97-154">If hello notification is an in-app notification with a web-view then hello app user clicks on a URL in hello web view [Android Only]</span></span>
   * <span data-ttu-id="38d97-155">*uživatel aplikace Hello můžete ukončit oznámení v některém z následujících způsobů hello:*</span><span class="sxs-lookup"><span data-stu-id="38d97-155">*hello app user can exit a notification in either of hello following ways:*</span></span>
     
     1. <span data-ttu-id="38d97-156">Kliknutím na tlačítko Zavřít hello na hello oznámení přímo.</span><span class="sxs-lookup"><span data-stu-id="38d97-156">Clicking hello close button on hello notification directly.</span></span> 
     2. <span data-ttu-id="38d97-157">K načtení rychle nebo odstraněním hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="38d97-157">Swiping away or deleting hello notification.</span></span> 
     3. <span data-ttu-id="38d97-158">Oznámení v aplikaci s text nebo webového obsahu a hlasování jsou obvykle zobrazených toohello aplikace uživatele ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="38d97-158">In-app notifications with text/web content and polls are typically displayed toohello app user in a two-step process.</span></span> <span data-ttu-id="38d97-159">Nejprve zobrazí oznámení a po kliknutí na něm, zobrazí se jim hello následné obsahu text/web/dotazování.</span><span class="sxs-lookup"><span data-stu-id="38d97-159">They see a notification first and when they click on it, they see hello subsequent text/web/poll content.</span></span> <span data-ttu-id="38d97-160">uživatel aplikace Hello můžete ukončit oznámení v některém z těchto kroků a hello podrobností v rozšířeném zobrazení hello zachytí to.</span><span class="sxs-lookup"><span data-stu-id="38d97-160">hello app user can exit a notification in either of these steps and hello details in hello Advanced view captures this.</span></span> 
5. <span data-ttu-id="38d97-161">**Reakcemi** -určuje hello počet zpráv, které byly explicitně reakcemi uživatelem aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="38d97-161">**Actioned** - This specifies hello number of messages which were explicitly actioned by hello app user.</span></span> <span data-ttu-id="38d97-162">Toto je nejvíce zajímavé číslo hello jako to informuje o tom, kolik uživatelů aplikace byly zajímají o uvítací zprávu, kterou instalováni v hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="38d97-162">This is hello most interesting number as this tells how many app users were interested by hello message you pushed out in hello notification.</span></span> 

> [!NOTE]
> <span data-ttu-id="38d97-163">Na iOS & platformy systému Windows, pokud má uživatel hello hello aplikaci otevřete a hello kampaň byla kampaň "Kdykoliv" je možné, že oba mimo oznámení v aplikaci a aplikace se zobrazují v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="38d97-163">On iOS & Windows platforms, if hello user has hello app open and hello campaign was an "AnyTime" campaign then it is possible that both out of app and in-app notifications are displayed at hello same time.</span></span> <span data-ttu-id="38d97-164">To může způsobit vyšší než hello doručené zobrazené počtu.</span><span class="sxs-lookup"><span data-stu-id="38d97-164">This may cause a Displayed count higher than hello Delivered.</span></span> <span data-ttu-id="38d97-165">Pokud uživatel hello pracuje nebo akce hello oznámení, pak i hello uživatelské interakce/Actioned počet může být větší než doručené.</span><span class="sxs-lookup"><span data-stu-id="38d97-165">If hello user interacts or actions hello notification, then even hello User Interactions/Actioned count could be greater than Delivered.</span></span> 
> 
> 

![Reach2][19]

## <a name="see-also"></a><span data-ttu-id="38d97-167">Viz také</span><span class="sxs-lookup"><span data-stu-id="38d97-167">See also</span></span>
* <span data-ttu-id="38d97-168">[Koncepty][Link 6]</span><span class="sxs-lookup"><span data-stu-id="38d97-168">[Concepts][Link 6]</span></span>
* <span data-ttu-id="38d97-169">[Řešení potíží s příručce služby][Link 24]</span><span class="sxs-lookup"><span data-stu-id="38d97-169">[Troubleshooting Guide Service][Link 24]</span></span>

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

