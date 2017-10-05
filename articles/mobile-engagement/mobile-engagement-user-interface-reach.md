---
title: "Azure Mobile Engagement uživatelské rozhraní - Reach"
description: "Zjistěte, jak k oslovení uživatelů vaší aplikace pomocí nabízených oznámení pomocí Azure Mobile Engagement"
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
ms.openlocfilehash: ce30456e41ff1a2f4824bcb64246ee115fdd1ef7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a><span data-ttu-id="a4aa0-103">Postup oslovení uživatelů vaší aplikace pomocí nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="a4aa0-103">How to reach out to the users of your application with push notifications</span></span>
<span data-ttu-id="a4aa0-104">Tento článek popisuje **dosáhnout** kartě **Mobile Engagement** portálu.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-104">This article describes the **REACH** tab of the **Mobile Engagement** portal.</span></span> <span data-ttu-id="a4aa0-105">Můžete použít **Mobile Engagement** portálu ke sledování a správě mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-105">You use the **Mobile Engagement** portal to monitor and manage your mobile apps.</span></span> <span data-ttu-id="a4aa0-106">Všimněte si, že pokud chcete začít používat portál musíte nejprve vytvořit **Azure Mobile Engagement** účtu.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-106">Note that to start using the portal you first need to create an **Azure Mobile Engagement** account.</span></span> <span data-ttu-id="a4aa0-107">Další informace najdete v tématu [vytvoření účtu Azure Mobile Engagement](mobile-engagement-create.md).</span><span class="sxs-lookup"><span data-stu-id="a4aa0-107">For more information, see [Create an Azure Mobile Engagement account](mobile-engagement-create.md).</span></span>

<span data-ttu-id="a4aa0-108">Části Reach uživatelské rozhraní je nástroj správy kampaň nabízených kde můžete vytvořit nebo upravit nebo aktivovat nebo dokončit nebo monitorování a získat statistiky kampaní nabízených oznámení a funkce, které lze také přistupovat přes rozhraní Reach API (a některé prvky nízkou úroveň Push rozhraní API) .</span><span class="sxs-lookup"><span data-stu-id="a4aa0-108">The Reach section of the UI is the Push campaign management tool where you can create/edit/activate/finish/monitor and get statistics on Push notification campaigns and features that can also be accessed via the Reach API (and some elements of the low level Push API).</span></span> <span data-ttu-id="a4aa0-109">Mějte na paměti, jestli používáte rozhraní API nebo uživatelského rozhraní, bude potřeba integrovat Azure Mobile Engagement a Reach do své aplikace pro každou platformu sadou SDK, abyste mohli používat kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-109">Remember that whether you are using the APIs or the UI, you will need to integrate both Azure Mobile Engagement and Reach into your application for each platform with the SDK before you can use Reach campaigns.</span></span>

> [!NOTE]
> <span data-ttu-id="a4aa0-110">Mnoho části **Mobile Engagement** portál obsahovat uživatelského rozhraní **zobrazit NÁPOVĚDU k** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-110">Many sections of the **Mobile Engagement** portal UI contain the **SHOW HELP** button.</span></span> <span data-ttu-id="a4aa0-111">Chcete-li získat další kontextové informace o oddílu na toto tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-111">Press this button to get more contextual information about a section.</span></span>
> 
> 

## <a name="four-types-of-push-notifications"></a><span data-ttu-id="a4aa0-112">Čtyři typy nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="a4aa0-112">Four types of Push notifications</span></span>
1. <span data-ttu-id="a4aa0-113">Oznámení – umožňují odesílat reklamní zprávy pro uživatele, kteří přesměruje je to jiné umístění uvnitř vaší aplikace a odešlete je webová stránka nebo úložiště mimo vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-113">Announcements - allow you to send advertising messages to users that redirect them to another location inside your app or to send them to a webpage or store outside of your app.</span></span> 
2. <span data-ttu-id="a4aa0-114">Hlasování – umožňují od koncových uživatelů sbírat informace ve formě odpovědí na otázky.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-114">Polls - allow you to gather information from end users by asking them questions.</span></span>
3. <span data-ttu-id="a4aa0-115">Datová oznámení - umožňují odesílat binární nebo base64 datový soubor.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-115">Data Pushes - allow you to send a binary or base64 data file.</span></span> <span data-ttu-id="a4aa0-116">Informace obsažené v datovém oznámení se odešlou do vaší aplikace změnit aktuální zkušeností uživatelů ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-116">The information contained in a data push is sent to your application to modify your users' current experience in your app.</span></span> <span data-ttu-id="a4aa0-117">Aplikace musí umět zpracovat data v datovém oznámení.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-117">Your application needs to be able to process the data in a data push.</span></span>

## <a name="campaign-details"></a><span data-ttu-id="a4aa0-118">Podrobnosti kampaně</span><span class="sxs-lookup"><span data-stu-id="a4aa0-118">Campaign Details</span></span>
<span data-ttu-id="a4aa0-119">Můžete upravit, klonování, odstranit nebo aktivovat kampaní, které nebyly ještě aktivována podržením ukazatele nad jejich názvy nebo můžete kliknout na k jejich otevření.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-119">You can edit, clone, delete, or activate campaigns that have not been activated yet by hovering over their names or you can click to open them.</span></span> <span data-ttu-id="a4aa0-120">Může klonovat kampaní, které již byla aktivována podržením ukazatele nad jejich názvy nebo můžete kliknout na k jejich otevření.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-120">You can clone campaigns that have already been activated by hovering over their names or you can click to open them.</span></span> <span data-ttu-id="a4aa0-121">Jakmile byl aktivován však nelze změnit na kampaň.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-121">However, you can't change a campaign once it has been activated.</span></span>

![Reach1][18]

## <a name="reach-feedback"></a><span data-ttu-id="a4aa0-123">Dosažení zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="a4aa0-123">Reach Feedback</span></span>
<span data-ttu-id="a4aa0-124">Klikněte na **statistiky** a zobrazit podrobnosti kampaně Reach.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-124">Click on **Statistics** to see the details of a Reach campaign.</span></span> <span data-ttu-id="a4aa0-125">**Jednoduché** zobrazení nabízí vizuální znázornění ve formě pruhový graf sloupec o co se stalo po kampaň aktivovala.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-125">The **Simple** view provides a visual representation in the form of a column bar graph about what happened after a campaign was activated.</span></span> <span data-ttu-id="a4aa0-126">**Upřesnit** zobrazení obsahuje podrobnější informace o nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-126">The **Advanced** view provides more granular details about the push campaign.</span></span> <span data-ttu-id="a4aa0-127">Tyto údaje nebudou dostupné, pokud odesíláte zkušební kampaně tj oznámení odeslat testovací zařízení.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-127">These details will not be available if you are sending a test campaign i.e. a push sent to a test device.</span></span> <span data-ttu-id="a4aa0-128">Zde je, jak by měl interpretovat tyto podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="a4aa0-128">Here is how you should interpret these details:</span></span>

1. <span data-ttu-id="a4aa0-129">**Poslat** -určuje počet zpráv nabídnutých do zařízení.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-129">**Pushed** - This specifies the number of messages pushed to the devices.</span></span> <span data-ttu-id="a4aa0-130">Tento počet bude záviset na cílovou skupinu, kterou jste zadali při vytváření nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-130">This number will depend on the target audience you specified while creating the push campaign.</span></span> <span data-ttu-id="a4aa0-131">Pokud nezadáte žádné cílové skupiny, pak tato nabízená bude odesílat pro všechna registrovaná zařízení.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-131">If you do not specify any target audience, then this push will be sent out to all the registered devices.</span></span> <span data-ttu-id="a4aa0-132">Podobně jako všechny ostatní nabízených služeb, jsme není nabízených oznámení přímo do zařízení, ale místo toho je odešlete do příslušné platformy konkrétní služeb nabízených oznámení (PNS - APNS nebo GCM/WNS) tak, aby jejich doručování oznámení do zařízení.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-132">Like all other push services, we do not push the notifications directly to the devices but instead push them to the respective platform specific Push Notification Services (PNS - APNS/GCM/WNS) so that they can deliver the notifications to the devices.</span></span> 
2. <span data-ttu-id="a4aa0-133">**Doručit** -určuje počet zpráv, které jsou úspěšně doručil systém oznámení platformy a zařízení a potvrzené jako přijaté Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-133">**Delivered** - This specifies the number of messages which are successfully delivered by the PNS to the device and acknowledged as received by Mobile Engagement SDK.</span></span> 
   
   <span data-ttu-id="a4aa0-134">*Důvody pro doručené počet je menší než počet stisknutí:*</span><span class="sxs-lookup"><span data-stu-id="a4aa0-134">*Reasons for Delivered count being less than Pushed count:*</span></span>
   
   1. <span data-ttu-id="a4aa0-135">Pokud má uživatel ze zařízení odinstaluje aplikaci, ale systém PNS neví o tom, v době, kdy chcete systém PNS odeslat nabízeného oznámení potom zpráva se zahodí.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-135">If the user has uninstalled the app from the device but the PNS doesn't know about it at the time we send the push to the PNS then the message will be dropped.</span></span>
   2. <span data-ttu-id="a4aa0-136">Pokud zařízení je aplikace, ale jsou v režimu offline se zařízeními pro dlouhou dobu, systém PNS se nepodaří doručit zprávu do zařízení.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-136">If the device has the app but the devices themselves were offline for long periods of time, then the PNS will fail to deliver the message to the device.</span></span> 
   3. <span data-ttu-id="a4aa0-137">Pokud zpráva získat doručit do zařízení, ale v aplikaci Mobile Engagement SDK obsah zprávy nerozpozná, pak zahodí zprávy.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-137">If the message does get delivered to the device but the Mobile Engagement SDK in the app doesn’t recognize the content of the message, then it drops that message.</span></span> <span data-ttu-id="a4aa0-138">Toto může nastat, pokud přizpůsobení oznámení v aplikaci vygeneruje výjimku, která jsme catch SDK a drop zprávy.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-138">This could happen if the customization of the notification in the app generates an exception which we catch in the SDK and drop the message.</span></span> <span data-ttu-id="a4aa0-139">To může také nastat, pokud aplikace na zařízení používá verzi sady Mobile Engagement SDK, která není možné zjistit, novější verze nabízené zprávy odeslané ze platformy, ale toto je pouze v případě, že aplikace byl upgradován po oznámení byl odeslán z t Použí služby platformy.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-139">This could also occur if the app on the device is using a version of the Mobile Engagement SDK which is not able to understand the newer version of the push message sent from the platform but this is only when the app was upgraded after the notification was dispatched from the service platform.</span></span> <span data-ttu-id="a4aa0-140">**Upřesnit** kartě zjistit, kolik zpráv byly vyřazeny.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-140">The **Advanced** tab will tell how many messages were dropped.</span></span> 
   4. <span data-ttu-id="a4aa0-141">Na zařízeních s iOS někdy není získat doručeny zprávy je buď zařízení na nízký stav baterie. nebo pokud aplikace využívá významné množství power při zpracování vzdáleného oznámení.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-141">On iOS devices, messages sometimes do not get delivered if either the device is on low battery or if the app is consuming significant amount of power when processing remote notifications.</span></span> <span data-ttu-id="a4aa0-142">Jedná se o omezení zařízení iOS.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-142">This is a limitation of the iOS devices.</span></span>   
3. <span data-ttu-id="a4aa0-143">**Zobrazí** -určuje počet zpráv, které jsou úspěšně zobrazí uživateli aplikace na zařízení ve formě systémové oznámení nabízených nebo out z – aplikaci v centru oznámení, nebo oznámení v aplikaci v mobilní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-143">**Displayed** - This specifies the number of messages which are successfully shown to the app user on the device in the form of a system push/out-of-app notification in the notification center or an in-app notification within the mobile app.</span></span>  <span data-ttu-id="a4aa0-144">**Upřesnit** na kartě se zjistit, kolik byly systémová oznámení a kolik byly oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-144">The **Advanced** tab will tell you how many were system notifications and how many were in-app notifications.</span></span> 
   
   <span data-ttu-id="a4aa0-145">*Důvody pro zobrazené počet je menší než počet doručené (čekání na který se má zobrazit.)*</span><span class="sxs-lookup"><span data-stu-id="a4aa0-145">*Reasons for Displayed count being less than Delivered count (waiting to be displayed)*</span></span>
   
   1. <span data-ttu-id="a4aa0-146">Pokud tato kampaň oznámení na něm měli koncové datum je možné, že byla doručena oznámení, ale při čas dodán otevřít a zobrazit ji pro uživatele aplikace, je již platnost vypršela, se nikdy zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-146">If the notification campaign had an end date on it then it is possible that the notification was delivered but when the time came to open and display it to the app user, it was already expired so it was never displayed.</span></span>   
   2. <span data-ttu-id="a4aa0-147">Pokud oznámení je oznámení v aplikaci, pak oznámení se zobrazí, jenom když aplikace uživatel otevře aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-147">If the notification is an in-app notification then the notification is only displayed when the app user opens the app.</span></span> <span data-ttu-id="a4aa0-148">V případech, kdy uživatel aplikace neotevřel aplikace sady SDK oznámí, že byla oznámení doručit ale ještě nebyla nezobrazí, dokud otevření aplikace.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-148">In cases where the app user hasn't opened the app, the SDK will report that the notification was delivered but not yet displayed until the app is opened.</span></span> 
   3. <span data-ttu-id="a4aa0-149">Pokud oznámení je oznámení v aplikaci a nakonfigurovat zobrazený na konkrétní aktivity nebo obrazovce pak také oznámení se ohlásí jako doručen, ale ještě není doručit neprojeví, dokud uživatel otevře aplikaci na konkrétní obrazovce.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-149">If the notification is an in-app notification and configured to be shown on a specific activity/screen then also the notification will be reported as delivered but not yet delivered until the user opens the app on a specific screen.</span></span> 
4. <span data-ttu-id="a4aa0-150">**Interakce uživatele** -určuje počet zpráv, které má uživatel aplikace zpracoval s a bude obsahovat zprávy, které jsou zpracované nebo ukončené.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-150">**User Interactions** - This specifies the number of messages which the app user has interacted with and will include the messages which are either actioned or exited.</span></span> 
   
   * <span data-ttu-id="a4aa0-151">*Uživatel aplikace může oznámení v některém z následujících způsobů:*</span><span class="sxs-lookup"><span data-stu-id="a4aa0-151">*The app user can action a notification in either of the following ways:*</span></span>
     
     1. <span data-ttu-id="a4aa0-152">Pokud se oznámení o oznámení systému nebo na více systémů z – aplikaci nebo oznámení v aplikaci, pak aplikace uživatel klikne na oznámení odeslaných jen oznámení.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-152">If the notification is a system/out-of-app notification or an in-app notification sent as notification-only then the app user clicks on the notification.</span></span>
     2. <span data-ttu-id="a4aa0-153">Pokud oznámení je oznámení v aplikaci s text nebo webového zobrazení nebo hlasování aplikace uživatel klikne na tlačítko akce v oznámení.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-153">If the notification is an in-app notification with a text or web-view or polls then the app user clicks on the Action button in the notification.</span></span>
     3. <span data-ttu-id="a4aa0-154">Pokud oznámení je oznámení v aplikaci s webové zobrazení pak aplikace uživatel klikne na adresu URL webové pouze v zobrazení [Android]</span><span class="sxs-lookup"><span data-stu-id="a4aa0-154">If the notification is an in-app notification with a web-view then the app user clicks on a URL in the web view [Android Only]</span></span>
   * <span data-ttu-id="a4aa0-155">*Uživatel aplikace můžete ukončit oznámení v některém z následujících způsobů:*</span><span class="sxs-lookup"><span data-stu-id="a4aa0-155">*The app user can exit a notification in either of the following ways:*</span></span>
     
     1. <span data-ttu-id="a4aa0-156">Klepnutím na tlačítko Zavřít na oznámení přímo.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-156">Clicking the close button on the notification directly.</span></span> 
     2. <span data-ttu-id="a4aa0-157">K načtení rychle nebo odstraněním oznámení.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-157">Swiping away or deleting the notification.</span></span> 
     3. <span data-ttu-id="a4aa0-158">Oznámení v aplikaci s text nebo webového obsahu a dotazuje se obvykle zobrazují uživateli aplikace ve dvou krocích.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-158">In-app notifications with text/web content and polls are typically displayed to the app user in a two-step process.</span></span> <span data-ttu-id="a4aa0-159">Nejprve zobrazí oznámení a po kliknutí na něm, zobrazí se jim následné obsahu text/web/dotazování.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-159">They see a notification first and when they click on it, they see the subsequent text/web/poll content.</span></span> <span data-ttu-id="a4aa0-160">Uživatel aplikace můžete ukončit oznámení v některém z těchto kroků a podrobností v rozšířeném zobrazení zachytí to.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-160">The app user can exit a notification in either of these steps and the details in the Advanced view captures this.</span></span> 
5. <span data-ttu-id="a4aa0-161">**Reakcemi** -určuje počet zpráv, které byly explicitně reakcemi uživatelem aplikace.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-161">**Actioned** - This specifies the number of messages which were explicitly actioned by the app user.</span></span> <span data-ttu-id="a4aa0-162">Toto je počet nejvíce zajímavé, jak to ukazuje, kolik uživatelů aplikace byly zajímají o zprávu, kterou instalováni v oznámení.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-162">This is the most interesting number as this tells how many app users were interested by the message you pushed out in the notification.</span></span> 

> [!NOTE]
> <span data-ttu-id="a4aa0-163">Na iOS a Windows otevřete platformy, pokud má uživatel aplikace a tato kampaň byla kampaň "Kdykoliv" potom je možné, že oba mimo aplikaci a oznámení v aplikaci se zobrazují ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-163">On iOS & Windows platforms, if the user has the app open and the campaign was an "AnyTime" campaign then it is possible that both out of app and in-app notifications are displayed at the same time.</span></span> <span data-ttu-id="a4aa0-164">To může způsobit vyšší, než doručené zobrazené počtu.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-164">This may cause a Displayed count higher than the Delivered.</span></span> <span data-ttu-id="a4aa0-165">Pokud uživatel pracuje nebo akce oznámení a pak i počet uživatelské interakce/Actioned může být větší než doručené.</span><span class="sxs-lookup"><span data-stu-id="a4aa0-165">If the user interacts or actions the notification, then even the User Interactions/Actioned count could be greater than Delivered.</span></span> 
> 
> 

![Reach2][19]

## <a name="see-also"></a><span data-ttu-id="a4aa0-167">Viz také</span><span class="sxs-lookup"><span data-stu-id="a4aa0-167">See also</span></span>
* <span data-ttu-id="a4aa0-168">[Koncepty][Link 6]</span><span class="sxs-lookup"><span data-stu-id="a4aa0-168">[Concepts][Link 6]</span></span>
* <span data-ttu-id="a4aa0-169">[Řešení potíží s příručce služby][Link 24]</span><span class="sxs-lookup"><span data-stu-id="a4aa0-169">[Troubleshooting Guide Service][Link 24]</span></span>

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

