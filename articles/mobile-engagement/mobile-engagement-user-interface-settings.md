---
title: "aaaAzure Mobile Engagement uživatelské rozhraní – nastavení"
description: "Zjistěte, jak toomanage hello globální nastavení aplikace pomocí Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 858f4cb4-14de-4bb5-826f-28cadbfc928b
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 02d4a36c591fc5e097410b7e931d1c9ce81d68d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-hello-global-settings-of-your-application"></a><span data-ttu-id="61335-103">Jak toomanage hello globální nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="61335-103">How toomanage hello global settings of your application</span></span>
<span data-ttu-id="61335-104">Hello **nastavení** možnosti nabídky, které jsou k dispozici pro aplikací měnit, v závislosti na platformě hello hello aplikace a máte pro aplikace hello oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="61335-104">hello **Settings** menu options available for an application vary, depending on hello platform of hello application and hello permissions you have been granted for hello application.</span></span> <span data-ttu-id="61335-105">Nastavení zahrnuje následující hello: Podrobnosti, projekty, nativní oznámení, rychlosti Push, značkou Tag (app-info) a komerční naléhavost.</span><span class="sxs-lookup"><span data-stu-id="61335-105">Settings include hello following: Details, Projects, Native Push, Push Speed, Tag (app info), and Commercial Pressure.</span></span> <span data-ttu-id="61335-106">Hello značky (informace o aplikaci) nabídky hello nastavení oddílu lze spravovat vaše aplikace (pomocí hello SDK) nebo váš back-end (pomocí hello Device API).</span><span class="sxs-lookup"><span data-stu-id="61335-106">hello Tag (app info) menu option of hello Settings section can be managed by your application (using hello SDK) or by your backend (using hello Device API).</span></span> 

> [!NOTE]
> <span data-ttu-id="61335-107">Mnoho oddílů hello **Mobile Engagement** portál uživatelského rozhraní obsahovat hello **zobrazit NÁPOVĚDU k** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="61335-107">Many sections of hello **Mobile Engagement** portal UI contain hello **SHOW HELP** button.</span></span> <span data-ttu-id="61335-108">Stisknutím tohoto tlačítka tooget další kontextové informace o oddílu.</span><span class="sxs-lookup"><span data-stu-id="61335-108">Press this button tooget more contextual information about a section.</span></span>
> 
> 

## <a name="details"></a><span data-ttu-id="61335-109">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="61335-109">Details</span></span>
<span data-ttu-id="61335-110">Můžete toochange hello název a popis vaší aplikace, nadřazeného zobrazení hello aplikace, a oprávnění vaší role.</span><span class="sxs-lookup"><span data-stu-id="61335-110">Allows you toochange hello name and description of your application, view hello owner of your application and your role permissions.</span></span> 

<span data-ttu-id="61335-111">Konfigurace analýzy vám umožní tooview nebo změňte hello den týdnů spustit na a hello dobu uchování ve dnech.</span><span class="sxs-lookup"><span data-stu-id="61335-111">Analytics configuration enables  you tooview or change hello day weeks start on and hello retention time in day(s).</span></span>

  ![settings1][46]

## <a name="projects"></a><span data-ttu-id="61335-113">Projekty</span><span class="sxs-lookup"><span data-stu-id="61335-113">Projects</span></span>
<span data-ttu-id="61335-114">Umožňuje vám tooselect všechny projekty chcete tooappear vaší aplikace v.</span><span class="sxs-lookup"><span data-stu-id="61335-114">Allows you tooselect all projects you want your application tooappear in.</span></span> 

<span data-ttu-id="61335-115">Můžete taky vyhledat projekt a zobrazení hello název, popis, vlastníka a oprávnění vaší role žádné projektu aplikace je součástí.</span><span class="sxs-lookup"><span data-stu-id="61335-115">You can also search for a project and view hello name, description, owner and your role permissions of any project your application is part of.</span></span>

<span data-ttu-id="61335-116">Další informace najdete v tématu: [dokumentace uživatelského rozhraní – Domů][Link 13]</span><span class="sxs-lookup"><span data-stu-id="61335-116">For more information, see: [UI Documentation – Home][Link 13]</span></span>

  ![settings3][48]

## <a name="native-push"></a><span data-ttu-id="61335-118">Nativního nabízení</span><span class="sxs-lookup"><span data-stu-id="61335-118">Native Push</span></span>
<span data-ttu-id="61335-119">Umožňuje vám tooregister nový certifikát nebo odstraňte a existující certifikát pro použití s nativního nabízení.</span><span class="sxs-lookup"><span data-stu-id="61335-119">Allows you tooregister a new certificate or delete and existing certificate for use with native push.</span></span> <span data-ttu-id="61335-120">Nativního nabízení umožňuje Azure Mobile Engagement toopush tooyour aplikaci kdykoli, i když není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="61335-120">Native Push enables Azure Mobile Engagement toopush tooyour application at any time, even when it is not running.</span></span> 

<span data-ttu-id="61335-121">Po zadání přihlašovacích údajů nebo certifikáty pro aspoň jednu službu s nativním nabízením, můžete vybrat "Kdykoliv" při vytváření kampaně Reach a také použití hello "oznamovatelem" parametr v hello PUSH rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="61335-121">After providing credentials or certificates for at least one Native Push service, you can select "Any time" when creating Reach Campaigns, and also use hello "notifier" parameter in hello PUSH API.</span></span>

### <a name="apple-push-notification-service-apns"></a><span data-ttu-id="61335-122">Služby nabízených oznámení Apple (APNS)</span><span class="sxs-lookup"><span data-stu-id="61335-122">Apple Push Notification Service (APNS)</span></span>
<span data-ttu-id="61335-123">tooenable nativní oznámení pomocí služby Apple Push Notification Service hello budete potřebovat tooregister svůj certifikát.</span><span class="sxs-lookup"><span data-stu-id="61335-123">tooenable Native Push using hello Apple Push Notification Service you will need tooregister your certificate.</span></span> <span data-ttu-id="61335-124">Budete potřebovat toospecify hello typu certifikátu jako vývoj (vývoj) nebo produkční (PRODUKČNÍMU).</span><span class="sxs-lookup"><span data-stu-id="61335-124">You will need toospecify hello type of certificate as either development (DEV) or production (PROD).</span></span> <span data-ttu-id="61335-125">Bude nutné pak nahrajte certifikát a hello heslo.</span><span class="sxs-lookup"><span data-stu-id="61335-125">Then you will need upload your certificate and hello password.</span></span>

<span data-ttu-id="61335-126">Další informace najdete v tématu: [dokumentaci k sadě SDK - iOS – jak tooPrepare aplikace pro Apple nabízená oznámení][Link 5]</span><span class="sxs-lookup"><span data-stu-id="61335-126">For more information, see: [SDK Documentation - iOS - How tooPrepare your Application for Apple Push notifications][Link 5]</span></span>

![settings4][49]

### <a name="windows-push-notification-service-wpns"></a><span data-ttu-id="61335-128">Služba nabízených oznámení Windows (WPNS)</span><span class="sxs-lookup"><span data-stu-id="61335-128">Windows Push Notification Service (WPNS)</span></span>
<span data-ttu-id="61335-129">tooenable nativní oznámení pomocí služby oznámení Windows, je nutné zadat přihlašovací údaje pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="61335-129">tooenable Native Push using Windows Notification Service, you must provide your application's credentials.</span></span> <span data-ttu-id="61335-130">Budete potřebovat vaše identifikátor zabezpečení (SID) balíčku a tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="61335-130">You will need your Package security identifier (SID) and your Secret key.</span></span>

![settings5][50]

### <a name="google-cloud-messaging-for-android-gcm"></a><span data-ttu-id="61335-132">Google Cloud Messaging pro Android (GCM)</span><span class="sxs-lookup"><span data-stu-id="61335-132">Google Cloud Messaging for Android (GCM)</span></span>
<span data-ttu-id="61335-133">tooenable nativní oznámení pomocí služby GCM, budete potřebovat pokyny hello toofollow z Google.</span><span class="sxs-lookup"><span data-stu-id="61335-133">tooenable Native Push using GCM, you need toofollow hello instructions from Google.</span></span> <span data-ttu-id="61335-134">Pak musíte vložit jednoduchý klíče rozhraní API serveru nakonfigurovaný bez omezení IP adres.</span><span class="sxs-lookup"><span data-stu-id="61335-134">Then you must paste a server simple API key, configured without IP restrictions.</span></span> <span data-ttu-id="61335-135">Vyžaduje integraci s hello SDK pro Android v1.12.0 +.</span><span class="sxs-lookup"><span data-stu-id="61335-135">Requires integration with hello SDK for Android v1.12.0+.</span></span>

<span data-ttu-id="61335-136">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="61335-136">For more information, see:</span></span> 

* <span data-ttu-id="61335-137">[SDK dokumentace Android jak tooIntegrate GCM][Link 5]</span><span class="sxs-lookup"><span data-stu-id="61335-137">[SDK Documentation Android How tooIntegrate GCM][Link 5]</span></span>
* [<span data-ttu-id="61335-138">Příručka vývojáře Google GCM</span><span class="sxs-lookup"><span data-stu-id="61335-138">Google Developer GCM Guide</span></span>](http://developer.android.com/guide/google/gcm/gs.html)

### <a name="amazon-device-messaging-for-android-adm"></a><span data-ttu-id="61335-139">Zasílání zpráv pro Android (ADM) zařízení Amazon</span><span class="sxs-lookup"><span data-stu-id="61335-139">Amazon Device Messaging for Android (ADM)</span></span>
<span data-ttu-id="61335-140">tooenable nativní oznámení pomocí služby ADM, je nutné zadat Amazon <OAuth credentials> skládající se z ID klienta a tajný klíč klienta (vyžaduje integraci se sadou SDK pro Android v2.1.0 +).</span><span class="sxs-lookup"><span data-stu-id="61335-140">tooenable Native Push using ADM, you must provide Amazon <OAuth credentials> consisting of a Client ID and Client Secret (Requires integration with SDK for Android v2.1.0+).</span></span>

<span data-ttu-id="61335-141">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="61335-141">For more information, see:</span></span> 

* <span data-ttu-id="61335-142">[SDK dokumentace Android jak tooIntegrate ADM][Link 5]</span><span class="sxs-lookup"><span data-stu-id="61335-142">[SDK Documentation Android How tooIntegrate ADM][Link 5]</span></span>
* [<span data-ttu-id="61335-143">ADM dokumentaci pro vývojáře Amazon</span><span class="sxs-lookup"><span data-stu-id="61335-143">Amazon Developer ADM Documentation</span></span>](https://developer.amazon.com/sdk/adm/credentials.html#Getting)

![settings6][51]

## <a name="push-speed"></a><span data-ttu-id="61335-145">Rychlost nabízení</span><span class="sxs-lookup"><span data-stu-id="61335-145">Push Speed</span></span>
<span data-ttu-id="61335-146">Zobrazuje hello aktuální rychlost nabízení svojí aplikace a umožňuje vám toodefine hello rychlost nabízení svojí aplikace.</span><span class="sxs-lookup"><span data-stu-id="61335-146">Shows hello current push speed of your application and allows you toodefine hello push speed of your application.</span></span>

  ![settings7][52]

## <a name="tag-app-info"></a><span data-ttu-id="61335-148">Na tag (app-info)</span><span class="sxs-lookup"><span data-stu-id="61335-148">Tag (app info)</span></span>
![settings11][56]

## <a name="commercial-pressure"></a><span data-ttu-id="61335-150">Komerční naléhavost</span><span class="sxs-lookup"><span data-stu-id="61335-150">Commercial Pressure</span></span>
![settings12][57]

## <a name="see-also"></a><span data-ttu-id="61335-152">Viz také</span><span class="sxs-lookup"><span data-stu-id="61335-152">See also</span></span>
* <span data-ttu-id="61335-153">[Koncepty][Link 6]</span><span class="sxs-lookup"><span data-stu-id="61335-153">[Concepts][Link 6]</span></span>
* <span data-ttu-id="61335-154">[Řešení potíží s příručce služby][Link 24]</span><span class="sxs-lookup"><span data-stu-id="61335-154">[Troubleshooting Guide Service][Link 24]</span></span>

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

