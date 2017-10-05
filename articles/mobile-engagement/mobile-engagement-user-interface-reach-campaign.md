---
title: "Azure Mobile Engagement uživatelské rozhraní – kampaně Reach"
description: "Laern jak vytvářet a spravovat nabízených oznámení kampaně pomocí Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2fe124a2-a86f-4136-81ba-a9d298ec798a
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: fc88db8db11d1ed12fa95c2087c9a32b21bf4de5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-manage-push-notification-campaigns"></a><span data-ttu-id="12d36-103">Jak vytvořit a spravovat kampaní nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="12d36-103">How to create and manage push notification campaigns</span></span>
<span data-ttu-id="12d36-104">V části Reach UI slouží k vytvoření nové kampaně nabízených s komplexní vzorec tím, že poskytuje všechny informace, které potřebujete k odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="12d36-104">You can use the Reach section of the UI to create a new Push campaign with a complex formula by providing all the information you need to send a push notification.</span></span> <span data-ttu-id="12d36-105">Možnosti nabízené kampaně mírně lišit v závislosti na typech čtyři kampaň: oznámení, hlasování, datová oznámení a dlaždice (pouze Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="12d36-105">The options of a Push campaign vary slightly depending on the four campaign types: Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span>

### <a name="option-applies-to"></a><span data-ttu-id="12d36-106">Možnost se vztahuje na:</span><span class="sxs-lookup"><span data-stu-id="12d36-106">Option Applies to:</span></span>
* <span data-ttu-id="12d36-107">Jazyky: Všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="12d36-107">Languages:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="12d36-108">Kampaň: Všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="12d36-108">Campaign:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="12d36-109">Upozornění: Oznámení, hlasování</span><span class="sxs-lookup"><span data-stu-id="12d36-109">Notification:     Announcements, Polls</span></span>
* <span data-ttu-id="12d36-110">Obsah: Jedinečný pro každý typ kampaně</span><span class="sxs-lookup"><span data-stu-id="12d36-110">Content:    Unique for each campaign type</span></span>
* <span data-ttu-id="12d36-111">Cílové skupiny: Všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="12d36-111">Audience:     All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="12d36-112">Časový rámec: oznámení, hlasování, dlaždice</span><span class="sxs-lookup"><span data-stu-id="12d36-112">Time frame:     Announcements, Polls, Tiles</span></span>
* <span data-ttu-id="12d36-113">Test: Všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="12d36-113">Test:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>

![Reach Campaign1][20]

## <a name="languages"></a><span data-ttu-id="12d36-115">Jazyky</span><span class="sxs-lookup"><span data-stu-id="12d36-115">Languages</span></span>
<span data-ttu-id="12d36-116">Rozevírací nabídky jazyků můžete použít k odeslání jinou verzi vaší nabízené do zařízení, které jsou nastavené na používají různé jazyky.</span><span class="sxs-lookup"><span data-stu-id="12d36-116">You can use the Languages drop-down menu to send a different version of your Push to devices that are set to use different languages.</span></span> <span data-ttu-id="12d36-117">Ve výchozím nastavení všechna zařízení se zobrazí stejné nabízeného oznámení bez ohledu na to, v jakém jazyce jsou nastaveny na používání.</span><span class="sxs-lookup"><span data-stu-id="12d36-117">By default, all devices will receive the same Push regardless of what language they are set to use.</span></span> <span data-ttu-id="12d36-118">Uživatelé s jejich zařízení nastaven jiný jazyk, se zobrazí výchozí jazykovou verzi nabízeného oznámení.</span><span class="sxs-lookup"><span data-stu-id="12d36-118">Users with their device set to a different language will receive the Default Language version of the Push.</span></span> <span data-ttu-id="12d36-119">Mnohé z možností kampaň nabízených umožňují zadat alternativní obsah pro každý další jazyky, které vyberete.</span><span class="sxs-lookup"><span data-stu-id="12d36-119">Many of the push campaign options allow you to specify alternate content for each of the additional languages you select.</span></span> 

![Reach Campaign2][21]

### <a name="language-differences-apply-to"></a><span data-ttu-id="12d36-121">Jazyk rozdíly platí pro:</span><span class="sxs-lookup"><span data-stu-id="12d36-121">Language differences apply to:</span></span>
* <span data-ttu-id="12d36-122">Jazyky: Jedinečný jazyky může být vybraný kromě výchozí jazyk</span><span class="sxs-lookup"><span data-stu-id="12d36-122">Languages:    Unique languages may be selected in addition to the default language</span></span>
* <span data-ttu-id="12d36-123">Kampaň: Stejný pro všechny jazyky</span><span class="sxs-lookup"><span data-stu-id="12d36-123">Campaign:    Same for all languages</span></span>
* <span data-ttu-id="12d36-124">Oznámení: Jedinečný pro každý jazyk kromě výchozí jazyk</span><span class="sxs-lookup"><span data-stu-id="12d36-124">Notification:    Unique for each language in addition to the default language</span></span>
* <span data-ttu-id="12d36-125">Obsah: Jedinečný pro každý jazyk kromě výchozí jazyk</span><span class="sxs-lookup"><span data-stu-id="12d36-125">Content:    Unique for each language in addition to the default language</span></span>
* <span data-ttu-id="12d36-126">Cílová skupina: Lze filtrovat podle kritérium samostatné jazyk</span><span class="sxs-lookup"><span data-stu-id="12d36-126">Audience:     May be filtered by a separate language criterion</span></span>
* <span data-ttu-id="12d36-127">Časový rámec: stejný pro všechny jazyky</span><span class="sxs-lookup"><span data-stu-id="12d36-127">Time frame:     Same for all languages</span></span>
* <span data-ttu-id="12d36-128">Test: Mohou být odeslány na každý jazyk v čase</span><span class="sxs-lookup"><span data-stu-id="12d36-128">Test:    May be sent to each language at a time</span></span>

### <a name="supported-languages"></a><span data-ttu-id="12d36-129">Podporované jazyky:</span><span class="sxs-lookup"><span data-stu-id="12d36-129">Supported Languages:</span></span>
* <span data-ttu-id="12d36-130">Arabština (ar)</span><span class="sxs-lookup"><span data-stu-id="12d36-130">Arabic (ar)</span></span> 
* <span data-ttu-id="12d36-131">Bulharština (bg)</span><span class="sxs-lookup"><span data-stu-id="12d36-131">Bulgarian (bg)</span></span> 
* <span data-ttu-id="12d36-132">Katalánštinu (ca)</span><span class="sxs-lookup"><span data-stu-id="12d36-132">Catalan (ca)</span></span> 
* <span data-ttu-id="12d36-133">Čínština (zh)</span><span class="sxs-lookup"><span data-stu-id="12d36-133">Chinese (zh)</span></span> 
* <span data-ttu-id="12d36-134">Chorvatština (hr)</span><span class="sxs-lookup"><span data-stu-id="12d36-134">Croatian (hr)</span></span> 
* <span data-ttu-id="12d36-135">Czech (cs)</span><span class="sxs-lookup"><span data-stu-id="12d36-135">Czech (cs)</span></span> 
* <span data-ttu-id="12d36-136">Dánština (da)</span><span class="sxs-lookup"><span data-stu-id="12d36-136">Danish (da)</span></span> 
* <span data-ttu-id="12d36-137">Holandština (nl)</span><span class="sxs-lookup"><span data-stu-id="12d36-137">Dutch (nl)</span></span> 
* <span data-ttu-id="12d36-138">Angličtina (en)</span><span class="sxs-lookup"><span data-stu-id="12d36-138">English (en)</span></span> 
* <span data-ttu-id="12d36-139">Finština (fi)</span><span class="sxs-lookup"><span data-stu-id="12d36-139">Finnish (fi)</span></span> 
* <span data-ttu-id="12d36-140">Francouzština (fr)</span><span class="sxs-lookup"><span data-stu-id="12d36-140">French (fr)</span></span> 
* <span data-ttu-id="12d36-141">Němčina (de)</span><span class="sxs-lookup"><span data-stu-id="12d36-141">German (de)</span></span> 
* <span data-ttu-id="12d36-142">Řečtina (el)</span><span class="sxs-lookup"><span data-stu-id="12d36-142">Greek (el)</span></span> 
* <span data-ttu-id="12d36-143">Hebrejština (mu)</span><span class="sxs-lookup"><span data-stu-id="12d36-143">Hebrew (he)</span></span> 
* <span data-ttu-id="12d36-144">Hindská (dobrý den)</span><span class="sxs-lookup"><span data-stu-id="12d36-144">Hindi (hi)</span></span> 
* <span data-ttu-id="12d36-145">Maďarština (hu)</span><span class="sxs-lookup"><span data-stu-id="12d36-145">Hungarian (hu)</span></span> 
* <span data-ttu-id="12d36-146">Indonéština (id)</span><span class="sxs-lookup"><span data-stu-id="12d36-146">Indonesian (id)</span></span> 
* <span data-ttu-id="12d36-147">Italština (it)</span><span class="sxs-lookup"><span data-stu-id="12d36-147">Italian (it)</span></span> 
* <span data-ttu-id="12d36-148">Japonština (ja)</span><span class="sxs-lookup"><span data-stu-id="12d36-148">Japanese (ja)</span></span> 
* <span data-ttu-id="12d36-149">Korejština (ko)</span><span class="sxs-lookup"><span data-stu-id="12d36-149">Korean (ko)</span></span> 
* <span data-ttu-id="12d36-150">Lotyština (lv)</span><span class="sxs-lookup"><span data-stu-id="12d36-150">Latvian (lv)</span></span> 
* <span data-ttu-id="12d36-151">Litevština (lt)</span><span class="sxs-lookup"><span data-stu-id="12d36-151">Lithuanian (lt)</span></span> 
* <span data-ttu-id="12d36-152">Malajština (macrolanguage) (ms)</span><span class="sxs-lookup"><span data-stu-id="12d36-152">Malay (macrolanguage) (ms)</span></span> 
* <span data-ttu-id="12d36-153">Norština, Bokmål (nb)</span><span class="sxs-lookup"><span data-stu-id="12d36-153">Norwegian Bokmål (nb)</span></span> 
* <span data-ttu-id="12d36-154">Polština (pl)</span><span class="sxs-lookup"><span data-stu-id="12d36-154">Polish (pl)</span></span> 
* <span data-ttu-id="12d36-155">Portugalština (pt)</span><span class="sxs-lookup"><span data-stu-id="12d36-155">Portuguese (pt)</span></span> 
* <span data-ttu-id="12d36-156">Rumunština (ro)</span><span class="sxs-lookup"><span data-stu-id="12d36-156">Romanian (ro)</span></span> 
* <span data-ttu-id="12d36-157">Ruština (ru)</span><span class="sxs-lookup"><span data-stu-id="12d36-157">Russian (ru)</span></span> 
* <span data-ttu-id="12d36-158">Srbština (sr)</span><span class="sxs-lookup"><span data-stu-id="12d36-158">Serbian (sr)</span></span> 
* <span data-ttu-id="12d36-159">Slovenština (sk)</span><span class="sxs-lookup"><span data-stu-id="12d36-159">Slovak (sk)</span></span> 
* <span data-ttu-id="12d36-160">Slovinština (sl)</span><span class="sxs-lookup"><span data-stu-id="12d36-160">Slovenian (sl)</span></span> 
* <span data-ttu-id="12d36-161">Španělština (es)</span><span class="sxs-lookup"><span data-stu-id="12d36-161">Spanish (es)</span></span> 
* <span data-ttu-id="12d36-162">Švédština (sv)</span><span class="sxs-lookup"><span data-stu-id="12d36-162">Swedish (sv)</span></span> 
* <span data-ttu-id="12d36-163">Tagalogština (tl)</span><span class="sxs-lookup"><span data-stu-id="12d36-163">Tagalog (tl)</span></span> 
* <span data-ttu-id="12d36-164">Thajštině (tou)</span><span class="sxs-lookup"><span data-stu-id="12d36-164">Thai (th)</span></span> 
* <span data-ttu-id="12d36-165">Turečtina (tr)</span><span class="sxs-lookup"><span data-stu-id="12d36-165">Turkish (tr)</span></span> 
* <span data-ttu-id="12d36-166">Ukrajinská (Velká Británie)</span><span class="sxs-lookup"><span data-stu-id="12d36-166">Ukrainian (uk)</span></span> 
* <span data-ttu-id="12d36-167">Vietnamštině (vi)</span><span class="sxs-lookup"><span data-stu-id="12d36-167">Vietnamese (vi)</span></span> 

## <a name="campaign"></a><span data-ttu-id="12d36-168">Kampaň</span><span class="sxs-lookup"><span data-stu-id="12d36-168">Campaign</span></span>
<span data-ttu-id="12d36-169">V části kampaň můžete použít k nastavení názvu a kategorie kampaně také jako, pokud budete chtít ignorovat cílovou skupinu části nabízené kampaně a místo toho odeslat tuto kampaň prostřednictvím rozhraní Reach API (a některé prvky s nízkou úrovní Push rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="12d36-169">You can use the Campaign section to set the name and category of your campaign as well as if you plan to ignore the audience section of a Push campaign and send this campaign via the Reach API (and some elements with the low level Push API) instead.</span></span> <span data-ttu-id="12d36-170">Kategorie lze použít s vlastní oznámení šablonu pro oznámení ovládacího prvku v aplikaci na základě předdefinovaných nastavení.</span><span class="sxs-lookup"><span data-stu-id="12d36-170">Categories can be used with a custom notification template to control in-app notifications based on predefined settings.</span></span> <span data-ttu-id="12d36-171">Můžete získat seznam existujících "kategorií" prostřednictvím rozhraní Reach API.</span><span class="sxs-lookup"><span data-stu-id="12d36-171">You can get a list of your existing “Categories” via the Reach API.</span></span>

> [!WARNING]
> <span data-ttu-id="12d36-172">Pokud použijete možnost "Ignorovat cílovou skupinu, nabízení se se uživatelům odešle přes rozhraní API" v části "Kampaň" kampaně Reach, kampaň se neodesílal automaticky, musíte ručně odesílání prostřednictvím rozhraní Reach API.</span><span class="sxs-lookup"><span data-stu-id="12d36-172">If you use the "Ignore Audience, push will be sent to users via the API" option in the "Campaign" section of a Reach campaign, the campaign will NOT automatically send, you will need to send it manually via the Reach API.</span></span>

![Reach Campaign3][22]

### <a name="option-applies-to"></a><span data-ttu-id="12d36-174">Možnost se vztahuje na:</span><span class="sxs-lookup"><span data-stu-id="12d36-174">Option Applies to:</span></span>
* <span data-ttu-id="12d36-175">Název: všechny</span><span class="sxs-lookup"><span data-stu-id="12d36-175">Name:    All</span></span>
* <span data-ttu-id="12d36-176">Kategorie: Oznámení, hlasování</span><span class="sxs-lookup"><span data-stu-id="12d36-176">Category:    Announcements, Polls</span></span>
* <span data-ttu-id="12d36-177">Ignorovat cílovou skupinu, nabízení se odešle uživatelům prostřednictvím rozhraní API: všechny</span><span class="sxs-lookup"><span data-stu-id="12d36-177">Ignore Audience, push will be sent to users via the API:    All</span></span>

## <a name="notification"></a><span data-ttu-id="12d36-178">Oznámení</span><span class="sxs-lookup"><span data-stu-id="12d36-178">Notification</span></span>
<span data-ttu-id="12d36-179">Část oznámení můžete nastavit základní nastavení pro vaše včetně nabízené: název nabízeného oznámení, zprávu, bitovou kopii v aplikaci, nebo pokud je zprávy.</span><span class="sxs-lookup"><span data-stu-id="12d36-179">You can use the Notification section to set basic settings for your push including: The title of the Push, the message, an in-app image, or if it is dismissible.</span></span> <span data-ttu-id="12d36-180">Mnoho nastavení oznámení jsou specifické pro platformu zařízení.</span><span class="sxs-lookup"><span data-stu-id="12d36-180">Many notification settings are specific to the platform of your device.</span></span> <span data-ttu-id="12d36-181">Můžete určit, zda vaše nabízení se odešle "aplikace" nebo "mimo aplikaci" nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="12d36-181">You can select whether your push will be sent "in app" or "out of app" or both.</span></span> <span data-ttu-id="12d36-182">(Nezapomeňte, že uživatelé můžou "výslovný souhlas" nebo "výslovný nesouhlas s" aplikace"z" nabízených oznámení na operační systém úrovni na svých zařízeních a Azure Mobile Engagement nebudete moci toto nastavení přepsat.</span><span class="sxs-lookup"><span data-stu-id="12d36-182">(Remember that users can "opt-in" or "opt-out" of "out of app" Pushes at the Operating System level on their devices, and Azure Mobile Engagement will not be able to override this setting.</span></span> <span data-ttu-id="12d36-183">Nezapomeňte taky, že rozhraní Reach API zpracovává "v aplikaci" a "mimo aplikaci" nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="12d36-183">Also remember that the Reach API handles "in app" and "out of app" Pushes.</span></span> <span data-ttu-id="12d36-184">Rozhraní API nabízené slouží ke zpracování nabízených oznámení "mimo aplikaci" příliš.) Oznámení lze přizpůsobit s obrázky a obsah HTML, včetně přímých odkazů pro propojování mimo aplikaci nebo na jiné místo v aplikaci (Android SDK 2.1.0 nebo novější záměrné kategorií vyžaduje).</span><span class="sxs-lookup"><span data-stu-id="12d36-184">The Push API can be used to handle "out of app" pushes too.) Pushes can be customized with pictures or HTML content, including deep links for linking outside of your App or to another location in your App (Android SDK 2.1.0 or later intent categories required).</span></span> <span data-ttu-id="12d36-185">Můžete změnit oznámení "BADGE" ikony nebo iOS a odeslání text nebo webového obsahu (místní okno s html, adresa URL odkaz na obsah do jiného umístění uvnitř nebo vně aplikace).</span><span class="sxs-lookup"><span data-stu-id="12d36-185">You can change the icon or iOS badge, and send either text or web content (a popup with html content, URL link to another location either inside or outside of the app).</span></span> <span data-ttu-id="12d36-186">Můžete také prozvonit zařízení se systémem Android nebo zavibrovat s nabízeného oznámení.</span><span class="sxs-lookup"><span data-stu-id="12d36-186">You can also make Android devices ring or vibrate with the Push.</span></span> <span data-ttu-id="12d36-187">(Nezapomeňte, že budete potřebovat správné oprávnění sady SDK v systémem Android manifest souboru prstence nebo zavibrovat zařízením.) Není aktuálně žádné oborový standard pro Android "velký obrázek" velikostí, protože velikost obrazovky se liší na každé zařízení, ale 400 × 100 obrázky fungovat na téměř jakoukoli velikosti obrazovky.</span><span class="sxs-lookup"><span data-stu-id="12d36-187">(Remember that you will need the correct SDK permissions in your Android manifest file to ring or vibrate a device.) There is currently no industry standard for Android "Big Picture" sizes, since screen sizes are different on every device, but 400x100 pictures work on almost any screen size.</span></span>

### <a name="delivery-types"></a><span data-ttu-id="12d36-188">Typy doručení:</span><span class="sxs-lookup"><span data-stu-id="12d36-188">Delivery Types:</span></span>
* <span data-ttu-id="12d36-189">Jen mimo aplikaci: oznámení budou doručeny, pokud uživatel nepoužívá aplikaci.</span><span class="sxs-lookup"><span data-stu-id="12d36-189">Out of app only: the notification will be delivered when the user does not use the application.</span></span>
* <span data-ttu-id="12d36-190">Mimo pouze oznámení aplikace vyžaduje certifikát od společnosti Apple nebo Google (certifikát služby APN nebo GCM).</span><span class="sxs-lookup"><span data-stu-id="12d36-190">The out of app only notification requires a certificate from Apple or Google (APNS or GCM certificate).</span></span>
* <span data-ttu-id="12d36-191">V aplikaci jenom: oznámení se zobrazí, jenom když je aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="12d36-191">In-app only: The notification appears only when the application is running.</span></span>
* <span data-ttu-id="12d36-192">Oznámení používá systém doručení Capptain spojit uživatele.</span><span class="sxs-lookup"><span data-stu-id="12d36-192">The notification uses the Capptain delivery system to reach the user.</span></span> <span data-ttu-id="12d36-193">Visual rozložení nebo zobrazení vaší nabízené můžete plně přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="12d36-193">You can fully customize the visual layout/display of your push.</span></span>
* <span data-ttu-id="12d36-194">Kdykoli: Tuto možnost zajistí, že odesílat oznámení, která buď aplikace běží, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="12d36-194">Anytime: This option ensures that you send a notification either the application is running or not.</span></span>

![Reach Campaign4][23]

### <a name="option-applies-to"></a><span data-ttu-id="12d36-196">Možnost se vztahuje na:</span><span class="sxs-lookup"><span data-stu-id="12d36-196">Option Applies to:</span></span>
* <span data-ttu-id="12d36-197">Upozornění: Oznámení, hlasování</span><span class="sxs-lookup"><span data-stu-id="12d36-197">Notification:     Announcements, Polls</span></span>

## <a name="content"></a><span data-ttu-id="12d36-198">Obsah</span><span class="sxs-lookup"><span data-stu-id="12d36-198">Content</span></span>
<span data-ttu-id="12d36-199">Části obsahu můžete upravovat obsah oznámení, hlasování, datová oznámení a dlaždice (pouze Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="12d36-199">You can use the Content section to modify the content of your Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span> <span data-ttu-id="12d36-200">Nastavení obsahu kampaní nabízených je specifické pro daný typ kampaně.</span><span class="sxs-lookup"><span data-stu-id="12d36-200">The Content setting of Push campaigns is specific to the type of campaign.</span></span> 

### <a name="see-also"></a><span data-ttu-id="12d36-201">Viz také</span><span class="sxs-lookup"><span data-stu-id="12d36-201">See also</span></span>
* <span data-ttu-id="12d36-202">[Dokumentace k uživatelského rozhraní – dosáhnout – Push obsahu][Link 29]</span><span class="sxs-lookup"><span data-stu-id="12d36-202">[UI Documentation - Reach - Push Content][Link 29]</span></span>

![Reach Campaign5][24]

## <a name="audience"></a><span data-ttu-id="12d36-204">Cílová skupina</span><span class="sxs-lookup"><span data-stu-id="12d36-204">Audience</span></span>
<span data-ttu-id="12d36-205">V části cílové skupiny můžete definovat standardní seznam položek, které mají omezit kampaně nebo omezení kampaň na základě přizpůsobené kritérií.</span><span class="sxs-lookup"><span data-stu-id="12d36-205">You can use the Audience section to define a standard list of items to limit your campaign or limits your campaign based on customized criteria.</span></span> <span data-ttu-id="12d36-206">Standardní sada možností pro omezení cílovou skupinu umožňuje odešlete do nové nebo staré uživatele nebo pouze uživatele nativního nabízení.</span><span class="sxs-lookup"><span data-stu-id="12d36-206">The standard set of options to Limit your Audience allows you to push to either new or old users or native push users only.</span></span> <span data-ttu-id="12d36-207">Můžete také nastavit kvótu omezit počet uživatelů, kteří obdrží nabízeného oznámení.</span><span class="sxs-lookup"><span data-stu-id="12d36-207">You can also set a quota to limit the number of users who receive the push.</span></span> <span data-ttu-id="12d36-208">Výraz pro filtrování kampaň zahrnout jeden nebo více kritéria pro cílové uživatele můžete ručně upravit.</span><span class="sxs-lookup"><span data-stu-id="12d36-208">You can manually Edit the expression for how your campaign is filtered to include one or more criterion to target users.</span></span> <span data-ttu-id="12d36-209">Výraz cílové skupiny, můžete zadat ručně.</span><span class="sxs-lookup"><span data-stu-id="12d36-209">You can manually type an audience expression.</span></span> <span data-ttu-id="12d36-210">Takové výraz musí explicitně definovat vztah mezi kritérii.</span><span class="sxs-lookup"><span data-stu-id="12d36-210">Such an expression must explicitly define the relation between criteria.</span></span> <span data-ttu-id="12d36-211">Kritérium se popisuje identifikátor, který musí začínat velkým písmenem a nesmí obsahovat mezery.</span><span class="sxs-lookup"><span data-stu-id="12d36-211">A criterion is described by an identifier that must start with a capital letter and cannot contain spaces.</span></span> <span data-ttu-id="12d36-212">Vztah mezi kritéria lze popsat pomocí 'a', 'nebo', 'není operátory a také '(',')'.</span><span class="sxs-lookup"><span data-stu-id="12d36-212">The relation between the criteria can be described using 'and', 'or', 'not' operators as well as '(', ')'.</span></span> <span data-ttu-id="12d36-213">Příklad: "Criterion1 nebo (Criterion1 a není Criterion2)".</span><span class="sxs-lookup"><span data-stu-id="12d36-213">Example: "Criterion1 or (Criterion1 and not Criterion2)".</span></span>

> [!NOTE]
> <span data-ttu-id="12d36-214">S velké cílovou skupinu součástí kampaně může být na straně serveru cílení na kontrolu pomalé, obzvláště pokud se pokusíte spustit několik kampaní ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="12d36-214">With a large audience included in campaigns, the server side targeting scan can be slow, especially if you attempt to start multiple campaigns at the same time.</span></span>

* <span data-ttu-id="12d36-215">Pokud je to možné spusťte pouze jeden kampaň najednou.</span><span class="sxs-lookup"><span data-stu-id="12d36-215">If possible, only start one campaign at a time.</span></span>
* <span data-ttu-id="12d36-216">Nejvýše spusťte pouze čtyři kampaně v čase.</span><span class="sxs-lookup"><span data-stu-id="12d36-216">At the most, only start four campaigns at a time.</span></span>
* <span data-ttu-id="12d36-217">Push jenom na aktivní uživatele (zaškrtávací políčko "zaujmout jen uživatele, kteří se dají oslovit pomocí nativního nabízení" a "Zaujmout jen aktivní uživatele"), aby pouze vaši uživatelé, kteří stále mít nainstalovanou aplikaci a použít ho bude muset být kontrolována.</span><span class="sxs-lookup"><span data-stu-id="12d36-217">Push only to your active users (checkbox "Engage only users who can be reached using Native Push" and "Engage only active users") so that only your users who still have the app installed and use it will need to be scanned.</span></span>
  <span data-ttu-id="12d36-218">Jakmile je definovány cílovou skupinu, můžete zjistit počet uživatelů, kteří obdrží tato nabízená tlačítko Simulovat.</span><span class="sxs-lookup"><span data-stu-id="12d36-218">Once your audience is defined, you can use the simulate button to find out how many users will receive this Push.</span></span> <span data-ttu-id="12d36-219">To bude vypočte se počet známých uživatelů potenciálně cílem touto cílovou skupinou (jde o odhad vycházející z náhodného vzorku uživatelů).</span><span class="sxs-lookup"><span data-stu-id="12d36-219">This will compute the number of known users potentially targeted by this audience (this is an estimate based on a random sample of users).</span></span> <span data-ttu-id="12d36-220">Uvědomte si, že součástí této cílové skupiny jsou i uživatelé, kteří aplikaci odinstalovali, ale není dostupný.</span><span class="sxs-lookup"><span data-stu-id="12d36-220">Be aware that users who have uninstalled the application are also part of this audience, but cannot be reached.</span></span>

### <a name="see-also"></a><span data-ttu-id="12d36-221">Viz také</span><span class="sxs-lookup"><span data-stu-id="12d36-221">See also</span></span>
* <span data-ttu-id="12d36-222">[Nové nabízené kritérium dokumentace - Reach - uživatelského rozhraní][Link 28]</span><span class="sxs-lookup"><span data-stu-id="12d36-222">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

![Reach Campaign6][25]

### <a name="edit-expression"></a><span data-ttu-id="12d36-224">Upravit výraz</span><span class="sxs-lookup"><span data-stu-id="12d36-224">Edit expression</span></span>
![Reach Campaign7][26]

### <a name="limit-your-audience-option-applies-to"></a><span data-ttu-id="12d36-226">Omezení, které vaše cílové skupiny možnost se vztahuje na:</span><span class="sxs-lookup"><span data-stu-id="12d36-226">Limit your audience option applies to:</span></span>
* <span data-ttu-id="12d36-227">Zaujmout jen podmnožinu uživatelů: všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="12d36-227">Engage only a subset of users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="12d36-228">Zaujmout jen staré uživatele: všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="12d36-228">Engage only old users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="12d36-229">Zaujmout jen nové uživatele: všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="12d36-229">Engage only new users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="12d36-230">Zaujmout jen neaktivní uživatele: oznámení, hlasování, dlaždice</span><span class="sxs-lookup"><span data-stu-id="12d36-230">Engage only idle users:    Announcements, Polls, Tiles</span></span>
* <span data-ttu-id="12d36-231">Zaujmout jen aktivní uživatele: všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="12d36-231">Engage only active users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="12d36-232">Zaujmout jen uživatele, kteří se dají oslovit pomocí nativního nabízení: oznámení, hlasování</span><span class="sxs-lookup"><span data-stu-id="12d36-232">Engage only users who can be reached using Native Push:     Announcements, Polls</span></span>

## <a name="time-frame"></a><span data-ttu-id="12d36-233">Časového rámce</span><span class="sxs-lookup"><span data-stu-id="12d36-233">Time Frame</span></span>
<span data-ttu-id="12d36-234">Nastavení, když bude zasláno nabízeného oznámení nebo časový rámec můžete nechat prázdné pro okamžité spuštění kampaň můžete části časového rámce.</span><span class="sxs-lookup"><span data-stu-id="12d36-234">You can use the Time Frame section to set when the push will be sent or you can leave the time frame blank to start the campaign immediately.</span></span> <span data-ttu-id="12d36-235">Mějte na paměti, že pomocí koncovým uživatelům se časové pásmo může spusťte kampaň dříve, než očekávat pro koncové uživatele v Asii a odesílat malé dávky oznámení najednou, dokud všechny časových pásem na světě odpovídat časového rámce, nastavte pro kampaň za den.</span><span class="sxs-lookup"><span data-stu-id="12d36-235">Remember that using the end-users' time zone may start the campaign a day earlier than you expect for your end-users in Asia and send small batches of pushes at a time until all time zones in the world match the time frame set for your campaign.</span></span> <span data-ttu-id="12d36-236">Pomocí koncoví uživatelé časové pásmo může také způsobit zpoždění v kampaně vzhledem k tomu, že má k vyžádání čas z telefonu před zahájením nabízeného oznámení.</span><span class="sxs-lookup"><span data-stu-id="12d36-236">Using the end users' time zone can also cause delays in campaigns since it has to request the time from the phone before starting the push.</span></span>

> [!NOTE]
> <span data-ttu-id="12d36-237">Kampaně bez koncové datum může ukládat do mezipaměti nabízených oznámení místně a stále je zobrazit po můžete ručně dokončení kampaně.</span><span class="sxs-lookup"><span data-stu-id="12d36-237">Campaigns without an end date can cache pushes locally and still display them after you manually complete campaigns.</span></span> <span data-ttu-id="12d36-238">Aby se zabránilo toto chování konkrétní koncový čas pro kampaně.</span><span class="sxs-lookup"><span data-stu-id="12d36-238">To avoid this behavior, specific an end time for campaigns.</span></span>

### <a name="see-also"></a><span data-ttu-id="12d36-239">Viz také</span><span class="sxs-lookup"><span data-stu-id="12d36-239">See also</span></span>
* <span data-ttu-id="12d36-240">[Dosažení – jak Tos – plánování][Link 3]</span><span class="sxs-lookup"><span data-stu-id="12d36-240">[Reach - How Tos – Scheduling][Link 3]</span></span> 

![Reach Campaign8][27]

### <a name="settings-apply-to"></a><span data-ttu-id="12d36-242">Nastavení se vztahují na:</span><span class="sxs-lookup"><span data-stu-id="12d36-242">Settings Apply to:</span></span>
* <span data-ttu-id="12d36-243">Časový rámec: oznámení, hlasování, dlaždice</span><span class="sxs-lookup"><span data-stu-id="12d36-243">Time frame:     Announcements, Polls, Tiles</span></span>

## <a name="test"></a><span data-ttu-id="12d36-244">Test</span><span class="sxs-lookup"><span data-stu-id="12d36-244">Test</span></span>
<span data-ttu-id="12d36-245">Testovací část vám pomůže tento nabízené poslat testovací zařízení před uložením kampaně.</span><span class="sxs-lookup"><span data-stu-id="12d36-245">You can use the Test section to send this push to your own test device before saving the campaign.</span></span> <span data-ttu-id="12d36-246">Pokud jste nakonfigurovali všechny vlastní jazyků pro tuto kampaň, můžete otestovat nabízeného oznámení v jednotlivé jazyky.</span><span class="sxs-lookup"><span data-stu-id="12d36-246">If you have configured any custom languages for this campaign, you can test the push in each language.</span></span> <span data-ttu-id="12d36-247">Můžete nastavit testovací zařízení z "Můj účet".</span><span class="sxs-lookup"><span data-stu-id="12d36-247">You can setup a test device from “My Account”.</span></span>

> [!NOTE]
> <span data-ttu-id="12d36-248">Žádné na straně serveru, který data se protokolují, když použijete tlačítko pro "test" nabízených oznámení, data se protokolují pouze pro skutečné nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="12d36-248">No server side data is logged when you use the button to "test" pushes, data is only logged for real push campaigns.</span></span>

### <a name="see-also"></a><span data-ttu-id="12d36-249">Viz také</span><span class="sxs-lookup"><span data-stu-id="12d36-249">See also</span></span>
* <span data-ttu-id="12d36-250">[Dokumentace k uživatelského rozhraní – Můj účet][Link 14]</span><span class="sxs-lookup"><span data-stu-id="12d36-250">[UI Documentation - My Account][Link 14]</span></span>

![Reach Campaign9][28]

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

