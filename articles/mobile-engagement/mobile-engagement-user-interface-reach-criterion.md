---
title: "aaaAzure Mobile Engagement uživatelské rozhraní - dosáhnout kritérium"
description: "Zjistěte, jak toouse cílení kritéria toosend nabízené kampaně tooa vyberte část uživatelů pomocí Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: a4ed03a0-55b1-4dd8-b0bd-c475005afb66
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: d956add1b7edc1d49451596019c5a4dec098d724
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-targeting-criteria-toosend-push-campaigns-tooa-select-subset-of-your-users"></a><span data-ttu-id="b85ea-103">Jak toouse cílení kritéria toosend nabízené kampaně tooa vybrat podmnožinu uživatelů</span><span class="sxs-lookup"><span data-stu-id="b85ea-103">How toouse targeting criteria toosend push campaigns tooa select subset of your users</span></span>
<span data-ttu-id="b85ea-104">Cílení na cílovou skupinu podle konkrétních kritérií pomocí tlačítka "Nová kritéria" hello je jedním z hello nejúčinnějších koncepty v Azure Mobile Engagement, pomáhá odesílat relevantní nabízená oznámení, že zákazníci hello bude odpovídat tooinstead z každého spamu.</span><span class="sxs-lookup"><span data-stu-id="b85ea-104">Targeting your audience by specific criteria with hello "New Criteria" button is one of hello most powerful concepts in Azure Mobile Engagement that helps you send relevant push notifications that hello customers will respond tooinstead of Spamming everyone.</span></span> <span data-ttu-id="b85ea-105">Můžete omezit na základě kritérií standardní cílovou skupinu a simulovat nabízených oznámení toodetermine kolik lidí obdrží oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="b85ea-105">You can limit your audience based on standard criteria and simulate pushes toodetermine how many people will receive hello notification.</span></span>

<span data-ttu-id="b85ea-106">**Viz také:**</span><span class="sxs-lookup"><span data-stu-id="b85ea-106">**See also:**</span></span>

* <span data-ttu-id="b85ea-107">[Kampaně nabízených novou dokumentaci - Reach - uživatelského rozhraní][Link 27]</span><span class="sxs-lookup"><span data-stu-id="b85ea-107">[UI Documentation - Reach - New Push Campaign][Link 27]</span></span>

## <a name="audience-criteria-can-include"></a><span data-ttu-id="b85ea-108">Může zahrnovat kritérií pro cílové skupiny:</span><span class="sxs-lookup"><span data-stu-id="b85ea-108">Audience criteria can include:</span></span>
* <span data-ttu-id="b85ea-109">** Technicals: ** můžete cílit na základě hello stejné technické informace zobrazí v hello analýzy a části monitorování.</span><span class="sxs-lookup"><span data-stu-id="b85ea-109">**Technicals: ** You can target based on hello same technical information you can see in hello Analytics and Monitor sections.</span></span> <span data-ttu-id="b85ea-110">**Viz také:** [uživatelského rozhraní dokumentace - Analytics][Link 15], [dokumentace k uživatelského rozhraní – monitorování][Link 16]</span><span class="sxs-lookup"><span data-stu-id="b85ea-110">**See also:** [UI Documentation - Analytics][Link 15],  [UI Documentation - Monitor][Link 16]</span></span>
* <span data-ttu-id="b85ea-111">**Umístění:** aplikace, které používají "hlášení polohy reálném čase" s geografického vymezení můžete použít jako kritéria tootarget cílovou skupinu z hello GPS umístění geografického umístění.</span><span class="sxs-lookup"><span data-stu-id="b85ea-111">**Location:** Applications that use "Real time location reporting" with Geo-Fencing can use Geo-Location as a criteria tootarget an audience from hello GPS location.</span></span> <span data-ttu-id="b85ea-112">"Opožděné hlášení umístění oblasti" volání také možné použít tootarget cílovou skupinu z umístění hello mobilní telefon ("hlášení polohy reálném čase" a "Opožděné hlášení umístění oblasti" musí být aktivována z hello SDK).</span><span class="sxs-lookup"><span data-stu-id="b85ea-112">"Lazy Area Location Reporting" call also be used tootarget an audience from hello cell phone location ("Real time location reporting" and "Lazy Area Location Reporting" must be activated from hello SDK).</span></span> <span data-ttu-id="b85ea-113">**Viz také:** [dokumentaci k sadě SDK - iOS – integrace][Link 5], [dokumentaci k sadě SDK - Android - integrace][Link 5]</span><span class="sxs-lookup"><span data-stu-id="b85ea-113">**See also:** [SDK Documentation - iOS -  Integration][Link 5], [SDK Documentation - Android -  Integration][Link 5]</span></span>
* <span data-ttu-id="b85ea-114">**Váš názor reach:** , můžete vybrat cílovou skupinu na základě jejich názorů předchozí oznámení reach prostřednictvím reach zpětnou vazbu od oznámení, hlasování a datová oznámení.</span><span class="sxs-lookup"><span data-stu-id="b85ea-114">**Reach Feedback:** You can target your audience based on their feedback from previous reach notifications through reach feedback from Announcements, Polls, and Data Pushes.</span></span> <span data-ttu-id="b85ea-115">To vám umožní toobetter cíl cílovou skupinu po dva nebo tři dosáhnout kampaní, než jste může hello poprvé.</span><span class="sxs-lookup"><span data-stu-id="b85ea-115">This enables you toobetter target your audience after two or three reach campaigns than you could hello first time.</span></span> <span data-ttu-id="b85ea-116">Můžete také použít toofilter uživatelů, kteří již obdržela oznámení o s podobnou obsahu, a to nastavením kampaň tooNOT odeslat toousers, který už dostali konkrétní předchozí kampaně.</span><span class="sxs-lookup"><span data-stu-id="b85ea-116">It can also be used toofilter out users who already received a notification with similar content, by setting a campaign tooNOT be sent toousers who already received a specific previous campaign.</span></span> <span data-ttu-id="b85ea-117">Můžete vyloučit i uživatelé kdo jsou zahrnuty konkrétní kampaň, která je stále aktivní příjem nové oznámení.</span><span class="sxs-lookup"><span data-stu-id="b85ea-117">You can even exclude users who are included a specific campaign that is still active from receiving new Pushes.</span></span> <span data-ttu-id="b85ea-118">**Viz také:** [předávaný obsah dokumentace - Reach - uživatelského rozhraní][Link 29]</span><span class="sxs-lookup"><span data-stu-id="b85ea-118">**See also:** [UI Documentation -  Reach - Push Content][Link 29]</span></span>
* <span data-ttu-id="b85ea-119">**Instalace sledování:** můžete sledovat informace podle nainstalovanou uživatelům vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b85ea-119">**Install Tracking:** You can track information based on where your users installed your App.</span></span> <span data-ttu-id="b85ea-120">**Viz také:** [dokumentace k uživatelského rozhraní – nastavení][Link 20]</span><span class="sxs-lookup"><span data-stu-id="b85ea-120">**See also:** [UI Documentation -  Settings][Link 20]</span></span>
* <span data-ttu-id="b85ea-121">**Profil uživatele:** vám může cíl na základě informací o standardní uživatel a vy můžete cíl založené na informace o hello vlastní aplikaci, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b85ea-121">**User Profile:** You can target based on standard user information and you can target based on hello custom app info that you have created.</span></span> <span data-ttu-id="b85ea-122">To zahrnuje uživatele, kteří jsou aktuálně přihlášeni a uživatele, kteří mají odpověděli konkrétní otázky můžete být vyzváni tooset v hello aplikace místo právě jak se mají odpověděl tooprevious kampaně.</span><span class="sxs-lookup"><span data-stu-id="b85ea-122">This includes users who are currently logged in and users that have answered specific questions you have asked them tooset in hello app itself instead of just how they have responded tooprevious campaigns.</span></span> <span data-ttu-id="b85ea-123">Všechny informace pěkně aplikace definovaná pro vaše aplikace se zobrazí v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="b85ea-123">All of your App Info's defined for your app show up on this list.</span></span>
* <span data-ttu-id="b85ea-124">Segmenty: Můžete také cílový podle segmentů, které jste vytvořili podle konkrétního uživatele chování obsahující více kritérií.</span><span class="sxs-lookup"><span data-stu-id="b85ea-124">Segments: You can also target based on segments that you have created based on specific user behavior containing multiple criteria.</span></span> <span data-ttu-id="b85ea-125">Všechny segmenty definované pro vaše aplikace zobrazí v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="b85ea-125">All of your segments defined for your app show up on this list.</span></span> <span data-ttu-id="b85ea-126">**Viz také:** [dokumentace k uživatelského rozhraní – segmenty][Link 18]</span><span class="sxs-lookup"><span data-stu-id="b85ea-126">**See also:** [UI Documentation -  Segments][Link 18]</span></span>
* <span data-ttu-id="b85ea-127">**Informace o aplikaci:** vlastní značky informace o aplikaci lze vytvořit na základě chování uživatelů tootrack "Nastavení".</span><span class="sxs-lookup"><span data-stu-id="b85ea-127">**App Info:** Custom App Info Tags can be created from “Settings” tootrack user behavior.</span></span> <span data-ttu-id="b85ea-128">**Viz také:** [dokumentace k uživatelského rozhraní – nastavení][Link 20]</span><span class="sxs-lookup"><span data-stu-id="b85ea-128">**See also:** [UI Documentation -  Settings][Link 20]</span></span>

## <a name="example"></a><span data-ttu-id="b85ea-129">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b85ea-129">Example:</span></span>
<span data-ttu-id="b85ea-130">Pokud chcete, aby toopush oznámení pouze toohello dílčí sadu z vašich uživatelů, které jste udělali v aplikaci zakoupit akce.</span><span class="sxs-lookup"><span data-stu-id="b85ea-130">If you want toopush an announcement only toohello sub-set of your users that have performed an in-app purchase action.</span></span>

1. <span data-ttu-id="b85ea-131">Přejděte tooyour stránky nastavení aplikace, vyberte nabídku "Informace o aplikaci" hello a vyberte možnost "Informace o nové aplikace"</span><span class="sxs-lookup"><span data-stu-id="b85ea-131">Go tooyour application settings page, select hello "App info" menu and select "New app info"</span></span>
2. <span data-ttu-id="b85ea-132">Zaregistrovat nové informace o logické aplikaci názvem "inAppPurchase"</span><span class="sxs-lookup"><span data-stu-id="b85ea-132">Register a new Boolean app info called "inAppPurchase"</span></span>
3. <span data-ttu-id="b85ea-133">Aby vaše aplikace nastavit tuto informaci o aplikaci příliš "PRAVDA", v případě, že hello uživatel úspěšně provede nákupy v aplikaci (s použitím sendAppInfo hello ("inAppPurchase",...) funkce)</span><span class="sxs-lookup"><span data-stu-id="b85ea-133">Make your application set this app info too"true" when hello user successfully performs an in-app purchase (by using hello sendAppInfo("inAppPurchase", ...) function)</span></span>
4. <span data-ttu-id="b85ea-134">Pokud nechcete, aby toodo to z vaší aplikace, můžete pomocí rozhraní API pro zařízení hello provádět z váš back-end)</span><span class="sxs-lookup"><span data-stu-id="b85ea-134">If you don't want toodo this from your application, you can do it from your backend by using hello device API)</span></span>
5. <span data-ttu-id="b85ea-135">Pak bude třeba jen toocreate sdělení, pomocí kritéria omezení vaše cílová skupina toousers s "inAppPurchase" nastaven příliš "true")</span><span class="sxs-lookup"><span data-stu-id="b85ea-135">Then, you just need toocreate your announcement, with a criterion limiting your audience toousers having "inAppPurchase" set too"true")</span></span>

> [!NOTE]
> <span data-ttu-id="b85ea-136">Cílení na základě kritérií než značky informace o aplikaci vyžaduje Azure Mobile Engagement toogather informace ze zařízení uživatelů předtím, než nabízené hello je odeslán a proto může dojít k prodlevě.</span><span class="sxs-lookup"><span data-stu-id="b85ea-136">Targeting based on criteria other than app info tags requires Azure Mobile Engagement toogather information from your users' devices before hello push is sent and so can cause a delay.</span></span> <span data-ttu-id="b85ea-137">Konfigurace komplexní nabízených možností (např. aktualizace odznaky) můžete také zpoždění nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="b85ea-137">Complex push configuration options (like updating badges) can also delay pushes.</span></span> <span data-ttu-id="b85ea-138">Použití "jeden snímek" kampaň z hello nabízené rozhraní API je hello absolutní nejrychlejší metodu push v Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="b85ea-138">Using a "one shot" campaign from hello Push API is hello absolute fastest push method in Azure Mobile Engagement.</span></span> <span data-ttu-id="b85ea-139">Pomocí pouze značek informace o aplikaci jako kritéria nabízené kampaně Reach (buď z hello Reach API nebo hello uživatelského rozhraní) je hello další nejrychlejší metodu, protože značky informace o aplikaci se ukládají na straně serveru hello.</span><span class="sxs-lookup"><span data-stu-id="b85ea-139">Using only app info tags as push criteria for a Reach campaign (either from hello Reach API or hello UI) is hello next fastest method since app info tags are stored on hello server side.</span></span> <span data-ttu-id="b85ea-140">Pomocí jiných cílení kritérií pro kampaně nabízených je hello nejvíce metodu push flexibilní ale nejpomalejší, protože Azure Mobile Engagement, kteří mají zařízení hello tooquery v pořadí toosend hello kampaně.</span><span class="sxs-lookup"><span data-stu-id="b85ea-140">Using other targeting criteria for a push campaign is hello most flexible but slowest push method since Azure Mobile Engagement has tooquery hello devices in order toosend hello campaign.</span></span>

![Reach Criterion1][29] 

## <a name="criterion-options-apply-to"></a><span data-ttu-id="b85ea-142">Kritérium možnosti platí pro:</span><span class="sxs-lookup"><span data-stu-id="b85ea-142">Criterion Options Apply to:</span></span>
* <span data-ttu-id="b85ea-143">**Technicals**</span><span class="sxs-lookup"><span data-stu-id="b85ea-143">**Technicals**</span></span>     
* <span data-ttu-id="b85ea-144">Název firmware: název firmwaru</span><span class="sxs-lookup"><span data-stu-id="b85ea-144">Firmware name:    Firmware name</span></span>
* <span data-ttu-id="b85ea-145">Verze firmwaru: verze firmwaru</span><span class="sxs-lookup"><span data-stu-id="b85ea-145">Firmware version:    Firmware version</span></span>
* <span data-ttu-id="b85ea-146">Model zařízení: model zařízení</span><span class="sxs-lookup"><span data-stu-id="b85ea-146">Device model:    Device model</span></span>
* <span data-ttu-id="b85ea-147">Výrobce zařízení: výrobce zařízení</span><span class="sxs-lookup"><span data-stu-id="b85ea-147">Device manufacturer:    Device manufacturer</span></span>
* <span data-ttu-id="b85ea-148">Verze aplikace: verze aplikace</span><span class="sxs-lookup"><span data-stu-id="b85ea-148">Application version:    Application version</span></span>
* <span data-ttu-id="b85ea-149">Poskytovatel název: Název operátora, není definovaná</span><span class="sxs-lookup"><span data-stu-id="b85ea-149">Carrier name:    Carrier name, undefined</span></span>
* <span data-ttu-id="b85ea-150">Poskytovatel země: poskytovatel země, není definovaná</span><span class="sxs-lookup"><span data-stu-id="b85ea-150">Carrier country:    Carrier country, undefined</span></span>
* <span data-ttu-id="b85ea-151">Typ sítě: typ sítě</span><span class="sxs-lookup"><span data-stu-id="b85ea-151">Network type:    Network type</span></span>
* <span data-ttu-id="b85ea-152">Národní prostředí: národní prostředí</span><span class="sxs-lookup"><span data-stu-id="b85ea-152">Locale:    Locale</span></span>
* <span data-ttu-id="b85ea-153">Velikost obrazovky: velikost obrazovky</span><span class="sxs-lookup"><span data-stu-id="b85ea-153">Screen size:    Screen size</span></span>
* <span data-ttu-id="b85ea-154">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="b85ea-154">**Location**</span></span>      
* <span data-ttu-id="b85ea-155">Poslední známá oblasti: polohu zemi, oblast,</span><span class="sxs-lookup"><span data-stu-id="b85ea-155">Last known area:    Country, Region, Locality</span></span>
* <span data-ttu-id="b85ea-156">Geografického vymezení reálném čase: seznam z POIs (název, akce), cyklické bodů zájmu (název, šířky, zeměpisnou délku, protokolu Radius v měřidla)</span><span class="sxs-lookup"><span data-stu-id="b85ea-156">Real time geo-fencing:    List of POIs (Name, Actions), Circular POI (Name, Latitude, Longitude, Radius in meters)</span></span>
* <span data-ttu-id="b85ea-157">**Dosažení zpětné vazby**</span><span class="sxs-lookup"><span data-stu-id="b85ea-157">**Reach feedback**</span></span>     
* <span data-ttu-id="b85ea-158">Zpětná vazba oznámení: oznámení, zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="b85ea-158">Announcement feedback:    Announcement, feedback</span></span>
* <span data-ttu-id="b85ea-159">Dotazování zpětné vazby: dotazování, zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="b85ea-159">Poll feedback:    Poll, feedback</span></span>
* <span data-ttu-id="b85ea-160">Dotazování odpovědí zpětné vazby: dotazování odpovědí zpětnou vazbu, otázku, výběru</span><span class="sxs-lookup"><span data-stu-id="b85ea-160">Poll answer feedback:    Poll answer feedback, question, choice</span></span>
* <span data-ttu-id="b85ea-161">Zpětné vazby dat nabízené: Datová oznámení, zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="b85ea-161">Data Push feedback:    Data Push, feedback</span></span>
* <span data-ttu-id="b85ea-162">**Sledování instalací**</span><span class="sxs-lookup"><span data-stu-id="b85ea-162">**Install Tracking**</span></span>     
* <span data-ttu-id="b85ea-163">Úložiště: Úložiště, není definovaná</span><span class="sxs-lookup"><span data-stu-id="b85ea-163">Store:    Store, Undefined</span></span>
* <span data-ttu-id="b85ea-164">Zdroj: Zdroje, není definovaná</span><span class="sxs-lookup"><span data-stu-id="b85ea-164">Source:    Source, Undefined</span></span>
* <span data-ttu-id="b85ea-165">**Profil uživatele**</span><span class="sxs-lookup"><span data-stu-id="b85ea-165">**User profile**</span></span>     
* <span data-ttu-id="b85ea-166">Pohlaví: muže nebo žena, není definovaná</span><span class="sxs-lookup"><span data-stu-id="b85ea-166">Gender:    male or female, undefined</span></span>
* <span data-ttu-id="b85ea-167">Datum narození: operátor, datum, není definovaná</span><span class="sxs-lookup"><span data-stu-id="b85ea-167">Birth date:    operator, date, undefined</span></span>
* <span data-ttu-id="b85ea-168">Výslovný souhlas: true nebo false, Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="b85ea-168">Opt-in:    true or false, undefined</span></span>
* <span data-ttu-id="b85ea-169">**Informace o aplikaci**</span><span class="sxs-lookup"><span data-stu-id="b85ea-169">**App Info**</span></span>      
* <span data-ttu-id="b85ea-170">Řetězec: Řetězec, není definovaná</span><span class="sxs-lookup"><span data-stu-id="b85ea-170">String:    String, undefined</span></span>
* <span data-ttu-id="b85ea-171">Datum: operátor, datum, není definovaná</span><span class="sxs-lookup"><span data-stu-id="b85ea-171">Date:    operator, date, undefined</span></span>
* <span data-ttu-id="b85ea-172">Celé číslo: číslo, není definována, operátor</span><span class="sxs-lookup"><span data-stu-id="b85ea-172">Integer:    operator, number, undefined</span></span>
* <span data-ttu-id="b85ea-173">Logická hodnota: true nebo false, není definovaná</span><span class="sxs-lookup"><span data-stu-id="b85ea-173">Boolean:    true or false, undefined</span></span>
* <span data-ttu-id="b85ea-174">**Segment**</span><span class="sxs-lookup"><span data-stu-id="b85ea-174">**Segment**</span></span>    
* <span data-ttu-id="b85ea-175">Název segmenty (z rozevíracího seznamu), vyloučení (cíloví uživatelé, kteří nejsou součástí tohoto segmentu).</span><span class="sxs-lookup"><span data-stu-id="b85ea-175">Name of Segments (from dropdown list), Exclusion (target users that are not a part of this segment).</span></span>

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

