---
title: "aaaAzure Mobile Engagement uživatelské rozhraní – kampaně Reach"
description: "Laern jak toocreate a spravovat kampaní nabízených oznámení pomocí Azure Mobile Engagement"
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
ms.openlocfilehash: 825e550ace63a34d1a90b10fa976a61eb15a6d04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-push-notification-campaigns"></a><span data-ttu-id="03916-103">Jak toocreate a spravovat kampaní nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="03916-103">How toocreate and manage push notification campaigns</span></span>
<span data-ttu-id="03916-104">Můžete se komplexní vzorcem hello Reach části hello uživatelského rozhraní toocreate novou kampaň nabízených tím, že poskytuje všechny hello informace, které budete potřebovat toosend nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="03916-104">You can use hello Reach section of hello UI toocreate a new Push campaign with a complex formula by providing all hello information you need toosend a push notification.</span></span> <span data-ttu-id="03916-105">Hello se možnosti nabízené kampaně mírně lišit v závislosti na typech kampaň hello čtyři: oznámení, hlasování, datová oznámení a dlaždice (pouze Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="03916-105">hello options of a Push campaign vary slightly depending on hello four campaign types: Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span>

### <a name="option-applies-to"></a><span data-ttu-id="03916-106">Možnost se vztahuje na:</span><span class="sxs-lookup"><span data-stu-id="03916-106">Option Applies to:</span></span>
* <span data-ttu-id="03916-107">Jazyky: Všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="03916-107">Languages:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="03916-108">Kampaň: Všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="03916-108">Campaign:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="03916-109">Upozornění: Oznámení, hlasování</span><span class="sxs-lookup"><span data-stu-id="03916-109">Notification:     Announcements, Polls</span></span>
* <span data-ttu-id="03916-110">Obsah: Jedinečný pro každý typ kampaně</span><span class="sxs-lookup"><span data-stu-id="03916-110">Content:    Unique for each campaign type</span></span>
* <span data-ttu-id="03916-111">Cílové skupiny: Všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="03916-111">Audience:     All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="03916-112">Časový rámec: oznámení, hlasování, dlaždice</span><span class="sxs-lookup"><span data-stu-id="03916-112">Time frame:     Announcements, Polls, Tiles</span></span>
* <span data-ttu-id="03916-113">Test: Všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="03916-113">Test:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>

![Reach Campaign1][20]

## <a name="languages"></a><span data-ttu-id="03916-115">Jazyky</span><span class="sxs-lookup"><span data-stu-id="03916-115">Languages</span></span>
<span data-ttu-id="03916-116">Můžete použít toosend hello jazyky rozevírací nabídky jinou verzi vaší nabízené toodevices nastavený toouse různé jazyky.</span><span class="sxs-lookup"><span data-stu-id="03916-116">You can use hello Languages drop-down menu toosend a different version of your Push toodevices that are set toouse different languages.</span></span> <span data-ttu-id="03916-117">Ve výchozím nastavení všechna zařízení obdrží hello stejný bez ohledu na to, v jakém jazyce jsou nastavená toouse Push.</span><span class="sxs-lookup"><span data-stu-id="03916-117">By default, all devices will receive hello same Push regardless of what language they are set toouse.</span></span> <span data-ttu-id="03916-118">Uživatelé s jiným jazykem jejich zařízení sady tooa obdrží hello výchozí jazykovou verzi hello Push.</span><span class="sxs-lookup"><span data-stu-id="03916-118">Users with their device set tooa different language will receive hello Default Language version of hello Push.</span></span> <span data-ttu-id="03916-119">Mnoho možností kampaň nabízených hello povolit toospecify alternativní obsah pro každý hello další jazyky, které vyberete.</span><span class="sxs-lookup"><span data-stu-id="03916-119">Many of hello push campaign options allow you toospecify alternate content for each of hello additional languages you select.</span></span> 

![Reach Campaign2][21]

### <a name="language-differences-apply-to"></a><span data-ttu-id="03916-121">Jazyk rozdíly platí pro:</span><span class="sxs-lookup"><span data-stu-id="03916-121">Language differences apply to:</span></span>
* <span data-ttu-id="03916-122">Jazyky: Jedinečný jazyky může být vybraný v přidání toohello výchozí jazyk</span><span class="sxs-lookup"><span data-stu-id="03916-122">Languages:    Unique languages may be selected in addition toohello default language</span></span>
* <span data-ttu-id="03916-123">Kampaň: Stejný pro všechny jazyky</span><span class="sxs-lookup"><span data-stu-id="03916-123">Campaign:    Same for all languages</span></span>
* <span data-ttu-id="03916-124">Oznámení: Jedinečný pro každý jazyk kromě toohello výchozí jazyk</span><span class="sxs-lookup"><span data-stu-id="03916-124">Notification:    Unique for each language in addition toohello default language</span></span>
* <span data-ttu-id="03916-125">Obsah: Jedinečný pro každý jazyk kromě toohello výchozí jazyk</span><span class="sxs-lookup"><span data-stu-id="03916-125">Content:    Unique for each language in addition toohello default language</span></span>
* <span data-ttu-id="03916-126">Cílová skupina: Lze filtrovat podle kritérium samostatné jazyk</span><span class="sxs-lookup"><span data-stu-id="03916-126">Audience:     May be filtered by a separate language criterion</span></span>
* <span data-ttu-id="03916-127">Časový rámec: stejný pro všechny jazyky</span><span class="sxs-lookup"><span data-stu-id="03916-127">Time frame:     Same for all languages</span></span>
* <span data-ttu-id="03916-128">Test: Mohou být odeslány tooeach jazyk v čase</span><span class="sxs-lookup"><span data-stu-id="03916-128">Test:    May be sent tooeach language at a time</span></span>

### <a name="supported-languages"></a><span data-ttu-id="03916-129">Podporované jazyky:</span><span class="sxs-lookup"><span data-stu-id="03916-129">Supported Languages:</span></span>
* <span data-ttu-id="03916-130">Arabština (ar)</span><span class="sxs-lookup"><span data-stu-id="03916-130">Arabic (ar)</span></span> 
* <span data-ttu-id="03916-131">Bulharština (bg)</span><span class="sxs-lookup"><span data-stu-id="03916-131">Bulgarian (bg)</span></span> 
* <span data-ttu-id="03916-132">Katalánštinu (ca)</span><span class="sxs-lookup"><span data-stu-id="03916-132">Catalan (ca)</span></span> 
* <span data-ttu-id="03916-133">Čínština (zh)</span><span class="sxs-lookup"><span data-stu-id="03916-133">Chinese (zh)</span></span> 
* <span data-ttu-id="03916-134">Chorvatština (hr)</span><span class="sxs-lookup"><span data-stu-id="03916-134">Croatian (hr)</span></span> 
* <span data-ttu-id="03916-135">Czech (cs)</span><span class="sxs-lookup"><span data-stu-id="03916-135">Czech (cs)</span></span> 
* <span data-ttu-id="03916-136">Dánština (da)</span><span class="sxs-lookup"><span data-stu-id="03916-136">Danish (da)</span></span> 
* <span data-ttu-id="03916-137">Holandština (nl)</span><span class="sxs-lookup"><span data-stu-id="03916-137">Dutch (nl)</span></span> 
* <span data-ttu-id="03916-138">Angličtina (en)</span><span class="sxs-lookup"><span data-stu-id="03916-138">English (en)</span></span> 
* <span data-ttu-id="03916-139">Finština (fi)</span><span class="sxs-lookup"><span data-stu-id="03916-139">Finnish (fi)</span></span> 
* <span data-ttu-id="03916-140">Francouzština (fr)</span><span class="sxs-lookup"><span data-stu-id="03916-140">French (fr)</span></span> 
* <span data-ttu-id="03916-141">Němčina (de)</span><span class="sxs-lookup"><span data-stu-id="03916-141">German (de)</span></span> 
* <span data-ttu-id="03916-142">Řečtina (el)</span><span class="sxs-lookup"><span data-stu-id="03916-142">Greek (el)</span></span> 
* <span data-ttu-id="03916-143">Hebrejština (mu)</span><span class="sxs-lookup"><span data-stu-id="03916-143">Hebrew (he)</span></span> 
* <span data-ttu-id="03916-144">Hindská (dobrý den)</span><span class="sxs-lookup"><span data-stu-id="03916-144">Hindi (hi)</span></span> 
* <span data-ttu-id="03916-145">Maďarština (hu)</span><span class="sxs-lookup"><span data-stu-id="03916-145">Hungarian (hu)</span></span> 
* <span data-ttu-id="03916-146">Indonéština (id)</span><span class="sxs-lookup"><span data-stu-id="03916-146">Indonesian (id)</span></span> 
* <span data-ttu-id="03916-147">Italština (it)</span><span class="sxs-lookup"><span data-stu-id="03916-147">Italian (it)</span></span> 
* <span data-ttu-id="03916-148">Japonština (ja)</span><span class="sxs-lookup"><span data-stu-id="03916-148">Japanese (ja)</span></span> 
* <span data-ttu-id="03916-149">Korejština (ko)</span><span class="sxs-lookup"><span data-stu-id="03916-149">Korean (ko)</span></span> 
* <span data-ttu-id="03916-150">Lotyština (lv)</span><span class="sxs-lookup"><span data-stu-id="03916-150">Latvian (lv)</span></span> 
* <span data-ttu-id="03916-151">Litevština (lt)</span><span class="sxs-lookup"><span data-stu-id="03916-151">Lithuanian (lt)</span></span> 
* <span data-ttu-id="03916-152">Malajština (macrolanguage) (ms)</span><span class="sxs-lookup"><span data-stu-id="03916-152">Malay (macrolanguage) (ms)</span></span> 
* <span data-ttu-id="03916-153">Norština, Bokmål (nb)</span><span class="sxs-lookup"><span data-stu-id="03916-153">Norwegian Bokmål (nb)</span></span> 
* <span data-ttu-id="03916-154">Polština (pl)</span><span class="sxs-lookup"><span data-stu-id="03916-154">Polish (pl)</span></span> 
* <span data-ttu-id="03916-155">Portugalština (pt)</span><span class="sxs-lookup"><span data-stu-id="03916-155">Portuguese (pt)</span></span> 
* <span data-ttu-id="03916-156">Rumunština (ro)</span><span class="sxs-lookup"><span data-stu-id="03916-156">Romanian (ro)</span></span> 
* <span data-ttu-id="03916-157">Ruština (ru)</span><span class="sxs-lookup"><span data-stu-id="03916-157">Russian (ru)</span></span> 
* <span data-ttu-id="03916-158">Srbština (sr)</span><span class="sxs-lookup"><span data-stu-id="03916-158">Serbian (sr)</span></span> 
* <span data-ttu-id="03916-159">Slovenština (sk)</span><span class="sxs-lookup"><span data-stu-id="03916-159">Slovak (sk)</span></span> 
* <span data-ttu-id="03916-160">Slovinština (sl)</span><span class="sxs-lookup"><span data-stu-id="03916-160">Slovenian (sl)</span></span> 
* <span data-ttu-id="03916-161">Španělština (es)</span><span class="sxs-lookup"><span data-stu-id="03916-161">Spanish (es)</span></span> 
* <span data-ttu-id="03916-162">Švédština (sv)</span><span class="sxs-lookup"><span data-stu-id="03916-162">Swedish (sv)</span></span> 
* <span data-ttu-id="03916-163">Tagalogština (tl)</span><span class="sxs-lookup"><span data-stu-id="03916-163">Tagalog (tl)</span></span> 
* <span data-ttu-id="03916-164">Thajštině (tou)</span><span class="sxs-lookup"><span data-stu-id="03916-164">Thai (th)</span></span> 
* <span data-ttu-id="03916-165">Turečtina (tr)</span><span class="sxs-lookup"><span data-stu-id="03916-165">Turkish (tr)</span></span> 
* <span data-ttu-id="03916-166">Ukrajinská (Velká Británie)</span><span class="sxs-lookup"><span data-stu-id="03916-166">Ukrainian (uk)</span></span> 
* <span data-ttu-id="03916-167">Vietnamštině (vi)</span><span class="sxs-lookup"><span data-stu-id="03916-167">Vietnamese (vi)</span></span> 

## <a name="campaign"></a><span data-ttu-id="03916-168">Kampaň</span><span class="sxs-lookup"><span data-stu-id="03916-168">Campaign</span></span>
<span data-ttu-id="03916-169">Můžete hello kampaň části tooset hello názvů a kategorie kampaně také jako kdyby plánování tooignore hello cílovou skupinu části nabízené kampaně a místo toho odeslat tuto kampaň prostřednictvím hello Reach API (a některé prvky s nízkou úrovní hello Push rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="03916-169">You can use hello Campaign section tooset hello name and category of your campaign as well as if you plan tooignore hello audience section of a Push campaign and send this campaign via hello Reach API (and some elements with hello low level Push API) instead.</span></span> <span data-ttu-id="03916-170">Kategorie lze použít s vlastní oznámení šablony toocontrol v aplikaci oznámení na základě předdefinovaných nastavení.</span><span class="sxs-lookup"><span data-stu-id="03916-170">Categories can be used with a custom notification template toocontrol in-app notifications based on predefined settings.</span></span> <span data-ttu-id="03916-171">Můžete získat seznam existujících "kategorií" prostřednictvím hello Reach API.</span><span class="sxs-lookup"><span data-stu-id="03916-171">You can get a list of your existing “Categories” via hello Reach API.</span></span>

> [!WARNING]
> <span data-ttu-id="03916-172">Pokud použijete možnost "Ignorovat cílovou skupinu, nabízení se odešle toousers prostřednictvím rozhraní API hello" hello v části "Kampaně" hello kampaně Reach hello kampaň bude neodesílal automaticky, budete potřebovat toosend ručně pomocí hello Reach API.</span><span class="sxs-lookup"><span data-stu-id="03916-172">If you use hello "Ignore Audience, push will be sent toousers via hello API" option in hello "Campaign" section of a Reach campaign, hello campaign will NOT automatically send, you will need toosend it manually via hello Reach API.</span></span>

![Reach Campaign3][22]

### <a name="option-applies-to"></a><span data-ttu-id="03916-174">Možnost se vztahuje na:</span><span class="sxs-lookup"><span data-stu-id="03916-174">Option Applies to:</span></span>
* <span data-ttu-id="03916-175">Název: všechny</span><span class="sxs-lookup"><span data-stu-id="03916-175">Name:    All</span></span>
* <span data-ttu-id="03916-176">Kategorie: Oznámení, hlasování</span><span class="sxs-lookup"><span data-stu-id="03916-176">Category:    Announcements, Polls</span></span>
* <span data-ttu-id="03916-177">Ignorovat cílovou skupinu, nabízení se odešle toousers prostřednictvím hello rozhraní API: všechny</span><span class="sxs-lookup"><span data-stu-id="03916-177">Ignore Audience, push will be sent toousers via hello API:    All</span></span>

## <a name="notification"></a><span data-ttu-id="03916-178">Oznámení</span><span class="sxs-lookup"><span data-stu-id="03916-178">Notification</span></span>
<span data-ttu-id="03916-179">Základní nastavení hello oznámení části tooset můžete použít pro vaše včetně nabízené: hello název hello nabízená instalace, uvítací zprávu, bitovou kopii v aplikaci, nebo pokud je zprávy.</span><span class="sxs-lookup"><span data-stu-id="03916-179">You can use hello Notification section tooset basic settings for your push including: hello title of hello Push, hello message, an in-app image, or if it is dismissible.</span></span> <span data-ttu-id="03916-180">Mnoho nastavení oznámení jsou konkrétní toohello platformu zařízení.</span><span class="sxs-lookup"><span data-stu-id="03916-180">Many notification settings are specific toohello platform of your device.</span></span> <span data-ttu-id="03916-181">Můžete určit, zda vaše nabízení se odešle "aplikace" nebo "mimo aplikaci" nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="03916-181">You can select whether your push will be sent "in app" or "out of app" or both.</span></span> <span data-ttu-id="03916-182">(Nezapomeňte, že uživatelé můžou "výslovný souhlas" nebo "výslovný nesouhlas s" aplikace"z" nabízených oznámení v hello operačního systému úrovni na svých zařízeních a bude Azure Mobile Engagement není možné toooverride se toto nastavení.</span><span class="sxs-lookup"><span data-stu-id="03916-182">(Remember that users can "opt-in" or "opt-out" of "out of app" Pushes at hello Operating System level on their devices, and Azure Mobile Engagement will not be able toooverride this setting.</span></span> <span data-ttu-id="03916-183">Nezapomeňte taky, že hello Reach API zpracovává "v aplikaci" a "mimo aplikaci" nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="03916-183">Also remember that hello Reach API handles "in app" and "out of app" Pushes.</span></span> <span data-ttu-id="03916-184">"Hello Push rozhraní API může být použité toohandle"mimo aplikaci"příliš nabízených oznámení.) Oznámení lze přizpůsobit s obrázky a obsah HTML, včetně přímých odkazů pro propojování mimo vaší aplikace nebo tooanother umístění ve vaší aplikaci (Android SDK 2.1.0 nebo novější záměrné kategorií vyžaduje).</span><span class="sxs-lookup"><span data-stu-id="03916-184">hello Push API can be used toohandle "out of app" pushes too.) Pushes can be customized with pictures or HTML content, including deep links for linking outside of your App or tooanother location in your App (Android SDK 2.1.0 or later intent categories required).</span></span> <span data-ttu-id="03916-185">Můžete změnit hello ikony nebo iOS oznámení "BADGE" a odeslání text nebo webového obsahu (místní okno s html, adresa URL odkaz tooanother umístění obsahu uvnitř nebo vně aplikace hello).</span><span class="sxs-lookup"><span data-stu-id="03916-185">You can change hello icon or iOS badge, and send either text or web content (a popup with html content, URL link tooanother location either inside or outside of hello app).</span></span> <span data-ttu-id="03916-186">Můžete také prozvonit zařízení se systémem Android nebo zavibrovat s hello Push.</span><span class="sxs-lookup"><span data-stu-id="03916-186">You can also make Android devices ring or vibrate with hello Push.</span></span> <span data-ttu-id="03916-187">(Nezapomeňte, že je budou potřebovat hello správná oprávnění sady SDK v systémem Android manifest souboru tooring nebo zavibrovat zařízením.) Není aktuálně žádné oborový standard pro Android "velký obrázek" velikostí, protože velikost obrazovky se liší na každé zařízení, ale 400 × 100 obrázky fungovat na téměř jakoukoli velikosti obrazovky.</span><span class="sxs-lookup"><span data-stu-id="03916-187">(Remember that you will need hello correct SDK permissions in your Android manifest file tooring or vibrate a device.) There is currently no industry standard for Android "Big Picture" sizes, since screen sizes are different on every device, but 400x100 pictures work on almost any screen size.</span></span>

### <a name="delivery-types"></a><span data-ttu-id="03916-188">Typy doručení:</span><span class="sxs-lookup"><span data-stu-id="03916-188">Delivery Types:</span></span>
* <span data-ttu-id="03916-189">Jen mimo aplikaci: hello oznámení budou doručeny, pokud uživatel hello nepoužívá aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="03916-189">Out of app only: hello notification will be delivered when hello user does not use hello application.</span></span>
* <span data-ttu-id="03916-190">Hello mimo pouze oznámení aplikace vyžaduje certifikát od společnosti Apple nebo Google (certifikát služby APN nebo GCM).</span><span class="sxs-lookup"><span data-stu-id="03916-190">hello out of app only notification requires a certificate from Apple or Google (APNS or GCM certificate).</span></span>
* <span data-ttu-id="03916-191">V aplikaci jenom: hello oznámení se zobrazí pouze při spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="03916-191">In-app only: hello notification appears only when hello application is running.</span></span>
* <span data-ttu-id="03916-192">oznámení Hello používá hello Capptain doručení systému tooreach hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="03916-192">hello notification uses hello Capptain delivery system tooreach hello user.</span></span> <span data-ttu-id="03916-193">Můžete plně přizpůsobit hello visual rozložení nebo zobrazení vaší push.</span><span class="sxs-lookup"><span data-stu-id="03916-193">You can fully customize hello visual layout/display of your push.</span></span>
* <span data-ttu-id="03916-194">Kdykoli: Tuto možnost zajistí, že odesílat oznámení, která buď hello aplikace běží, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="03916-194">Anytime: This option ensures that you send a notification either hello application is running or not.</span></span>

![Reach Campaign4][23]

### <a name="option-applies-to"></a><span data-ttu-id="03916-196">Možnost se vztahuje na:</span><span class="sxs-lookup"><span data-stu-id="03916-196">Option Applies to:</span></span>
* <span data-ttu-id="03916-197">Upozornění: Oznámení, hlasování</span><span class="sxs-lookup"><span data-stu-id="03916-197">Notification:     Announcements, Polls</span></span>

## <a name="content"></a><span data-ttu-id="03916-198">Obsah</span><span class="sxs-lookup"><span data-stu-id="03916-198">Content</span></span>
<span data-ttu-id="03916-199">Můžete použít hello obsahu části toomodify hello obsah oznámení, hlasování, datová oznámení a dlaždice (pouze Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="03916-199">You can use hello Content section toomodify hello content of your Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span> <span data-ttu-id="03916-200">nastavení obsah Hello kampaní nabízených je typ konkrétní toohello kampaně.</span><span class="sxs-lookup"><span data-stu-id="03916-200">hello Content setting of Push campaigns is specific toohello type of campaign.</span></span> 

### <a name="see-also"></a><span data-ttu-id="03916-201">Viz také</span><span class="sxs-lookup"><span data-stu-id="03916-201">See also</span></span>
* <span data-ttu-id="03916-202">[Dokumentace k uživatelského rozhraní – dosáhnout – Push obsahu][Link 29]</span><span class="sxs-lookup"><span data-stu-id="03916-202">[UI Documentation - Reach - Push Content][Link 29]</span></span>

![Reach Campaign5][24]

## <a name="audience"></a><span data-ttu-id="03916-204">Cílová skupina</span><span class="sxs-lookup"><span data-stu-id="03916-204">Audience</span></span>
<span data-ttu-id="03916-205">Můžete hello cílovou skupinu části toodefine standardní seznam položek toolimit kampaně nebo omezení kampaň na základě přizpůsobené kritérií.</span><span class="sxs-lookup"><span data-stu-id="03916-205">You can use hello Audience section toodefine a standard list of items toolimit your campaign or limits your campaign based on customized criteria.</span></span> <span data-ttu-id="03916-206">Hello standardní sadu možností tooLimit cílovou skupinu můžete toopush tooeither starý, nebo nový uživatele nebo pouze uživatele nativního nabízení.</span><span class="sxs-lookup"><span data-stu-id="03916-206">hello standard set of options tooLimit your Audience allows you toopush tooeither new or old users or native push users only.</span></span> <span data-ttu-id="03916-207">Můžete také nastavit číslo hello toolimit kvótu uživatelů, kteří obdrží nabízená hello.</span><span class="sxs-lookup"><span data-stu-id="03916-207">You can also set a quota toolimit hello number of users who receive hello push.</span></span> <span data-ttu-id="03916-208">Můžete ručně upravit hello výraz jak kampaň je filtrovaná tooinclude jeden nebo více uživatelů tootarget kritérium.</span><span class="sxs-lookup"><span data-stu-id="03916-208">You can manually Edit hello expression for how your campaign is filtered tooinclude one or more criterion tootarget users.</span></span> <span data-ttu-id="03916-209">Výraz cílové skupiny, můžete zadat ručně.</span><span class="sxs-lookup"><span data-stu-id="03916-209">You can manually type an audience expression.</span></span> <span data-ttu-id="03916-210">Takové výraz musí explicitně definujte hello vztah mezi kritérii.</span><span class="sxs-lookup"><span data-stu-id="03916-210">Such an expression must explicitly define hello relation between criteria.</span></span> <span data-ttu-id="03916-211">Kritérium se popisuje identifikátor, který musí začínat velkým písmenem a nesmí obsahovat mezery.</span><span class="sxs-lookup"><span data-stu-id="03916-211">A criterion is described by an identifier that must start with a capital letter and cannot contain spaces.</span></span> <span data-ttu-id="03916-212">Hello vztah mezi hello kritéria lze popsat pomocí 'a', 'nebo', 'není operátory a také '(',')'.</span><span class="sxs-lookup"><span data-stu-id="03916-212">hello relation between hello criteria can be described using 'and', 'or', 'not' operators as well as '(', ')'.</span></span> <span data-ttu-id="03916-213">Příklad: "Criterion1 nebo (Criterion1 a není Criterion2)".</span><span class="sxs-lookup"><span data-stu-id="03916-213">Example: "Criterion1 or (Criterion1 and not Criterion2)".</span></span>

> [!NOTE]
> <span data-ttu-id="03916-214">S velké cílovou skupinu součástí kampaně, hello cílení kontroly na straně serveru může být pomalé, obzvláště pokud se pokusíte toostart hello více kampaně na stejný čas.</span><span class="sxs-lookup"><span data-stu-id="03916-214">With a large audience included in campaigns, hello server side targeting scan can be slow, especially if you attempt toostart multiple campaigns at hello same time.</span></span>

* <span data-ttu-id="03916-215">Pokud je to možné spusťte pouze jeden kampaň najednou.</span><span class="sxs-lookup"><span data-stu-id="03916-215">If possible, only start one campaign at a time.</span></span>
* <span data-ttu-id="03916-216">V nejvíce, hello pouze počáteční čtyři kampaně v čase.</span><span class="sxs-lookup"><span data-stu-id="03916-216">At hello most, only start four campaigns at a time.</span></span>
* <span data-ttu-id="03916-217">Push jen aktivní uživatele tooyour (zaškrtávací políčko "zaujmout jen uživatele, kteří se dají oslovit pomocí nativního nabízení" a "Zaujmout jen aktivní uživatele"), aby pouze kteří stále mít nainstalovanou aplikaci hello a použít ho uživatelé budou potřebovat toobe zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="03916-217">Push only tooyour active users (checkbox "Engage only users who can be reached using Native Push" and "Engage only active users") so that only your users who still have hello app installed and use it will need toobe scanned.</span></span>
  <span data-ttu-id="03916-218">Jakmile cílová skupina je definována, můžete použít hello simulovat tlačítko toofind se počet uživatelů, kteří obdrží tato Push.</span><span class="sxs-lookup"><span data-stu-id="03916-218">Once your audience is defined, you can use hello simulate button toofind out how many users will receive this Push.</span></span> <span data-ttu-id="03916-219">To bude výpočetní hello počet známých uživatelů potenciálně cílem touto cílovou skupinou (jde o odhad vycházející z náhodného vzorku uživatelů).</span><span class="sxs-lookup"><span data-stu-id="03916-219">This will compute hello number of known users potentially targeted by this audience (this is an estimate based on a random sample of users).</span></span> <span data-ttu-id="03916-220">Uvědomte si, že součástí této cílové skupiny jsou i uživatelé, kteří odinstalovali hello aplikace, ale nelze získat přístup.</span><span class="sxs-lookup"><span data-stu-id="03916-220">Be aware that users who have uninstalled hello application are also part of this audience, but cannot be reached.</span></span>

### <a name="see-also"></a><span data-ttu-id="03916-221">Viz také</span><span class="sxs-lookup"><span data-stu-id="03916-221">See also</span></span>
* <span data-ttu-id="03916-222">[Nové nabízené kritérium dokumentace - Reach - uživatelského rozhraní][Link 28]</span><span class="sxs-lookup"><span data-stu-id="03916-222">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

![Reach Campaign6][25]

### <a name="edit-expression"></a><span data-ttu-id="03916-224">Upravit výraz</span><span class="sxs-lookup"><span data-stu-id="03916-224">Edit expression</span></span>
![Reach Campaign7][26]

### <a name="limit-your-audience-option-applies-to"></a><span data-ttu-id="03916-226">Omezení, které vaše cílové skupiny možnost se vztahuje na:</span><span class="sxs-lookup"><span data-stu-id="03916-226">Limit your audience option applies to:</span></span>
* <span data-ttu-id="03916-227">Zaujmout jen podmnožinu uživatelů: všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="03916-227">Engage only a subset of users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="03916-228">Zaujmout jen staré uživatele: všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="03916-228">Engage only old users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="03916-229">Zaujmout jen nové uživatele: všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="03916-229">Engage only new users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="03916-230">Zaujmout jen neaktivní uživatele: oznámení, hlasování, dlaždice</span><span class="sxs-lookup"><span data-stu-id="03916-230">Engage only idle users:    Announcements, Polls, Tiles</span></span>
* <span data-ttu-id="03916-231">Zaujmout jen aktivní uživatele: všechny (oznámení, hlasování, datová oznámení, dlaždice)</span><span class="sxs-lookup"><span data-stu-id="03916-231">Engage only active users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="03916-232">Zaujmout jen uživatele, kteří se dají oslovit pomocí nativního nabízení: oznámení, hlasování</span><span class="sxs-lookup"><span data-stu-id="03916-232">Engage only users who can be reached using Native Push:     Announcements, Polls</span></span>

## <a name="time-frame"></a><span data-ttu-id="03916-233">Časového rámce</span><span class="sxs-lookup"><span data-stu-id="03916-233">Time Frame</span></span>
<span data-ttu-id="03916-234">Při nabízení hello se odešle nebo můžete nechat hello časového rámce prázdné toostart hello kampaň okamžitě můžete tooset části hello časového rámce.</span><span class="sxs-lookup"><span data-stu-id="03916-234">You can use hello Time Frame section tooset when hello push will be sent or you can leave hello time frame blank toostart hello campaign immediately.</span></span> <span data-ttu-id="03916-235">Mějte na paměti, že pomocí hello koncoví uživatelé časové pásmo může spustit hello kampaň dříve, než očekávat pro koncové uživatele v Asii a odesílat malé dávky oznámení najednou, dokud všechny časových pásem v hello world shodu hello časového rámce nastavit kampaně za den.</span><span class="sxs-lookup"><span data-stu-id="03916-235">Remember that using hello end-users' time zone may start hello campaign a day earlier than you expect for your end-users in Asia and send small batches of pushes at a time until all time zones in hello world match hello time frame set for your campaign.</span></span> <span data-ttu-id="03916-236">Pomocí hello koncoví uživatelé časové pásmo může také způsobit zpoždění v kampaně vzhledem k tomu, že má toorequest čas hello z telefonu hello před zahájením nabízené hello.</span><span class="sxs-lookup"><span data-stu-id="03916-236">Using hello end users' time zone can also cause delays in campaigns since it has toorequest hello time from hello phone before starting hello push.</span></span>

> [!NOTE]
> <span data-ttu-id="03916-237">Kampaně bez koncové datum může ukládat do mezipaměti nabízených oznámení místně a stále je zobrazit po můžete ručně dokončení kampaně.</span><span class="sxs-lookup"><span data-stu-id="03916-237">Campaigns without an end date can cache pushes locally and still display them after you manually complete campaigns.</span></span> <span data-ttu-id="03916-238">tooavoid toto chování, konkrétní koncový čas pro kampaně.</span><span class="sxs-lookup"><span data-stu-id="03916-238">tooavoid this behavior, specific an end time for campaigns.</span></span>

### <a name="see-also"></a><span data-ttu-id="03916-239">Viz také</span><span class="sxs-lookup"><span data-stu-id="03916-239">See also</span></span>
* <span data-ttu-id="03916-240">[Dosažení – jak Tos – plánování][Link 3]</span><span class="sxs-lookup"><span data-stu-id="03916-240">[Reach - How Tos – Scheduling][Link 3]</span></span> 

![Reach Campaign8][27]

### <a name="settings-apply-to"></a><span data-ttu-id="03916-242">Nastavení se vztahují na:</span><span class="sxs-lookup"><span data-stu-id="03916-242">Settings Apply to:</span></span>
* <span data-ttu-id="03916-243">Časový rámec: oznámení, hlasování, dlaždice</span><span class="sxs-lookup"><span data-stu-id="03916-243">Time frame:     Announcements, Polls, Tiles</span></span>

## <a name="test"></a><span data-ttu-id="03916-244">Test</span><span class="sxs-lookup"><span data-stu-id="03916-244">Test</span></span>
<span data-ttu-id="03916-245">Hello testovací část toosend můžete použít tento nabízené tooyour vlastní test zařízení před uložením hello kampaně.</span><span class="sxs-lookup"><span data-stu-id="03916-245">You can use hello Test section toosend this push tooyour own test device before saving hello campaign.</span></span> <span data-ttu-id="03916-246">Pokud jste nakonfigurovali všechny vlastní jazyků pro tuto kampaň, můžete otestovat nabízená hello v jednotlivé jazyky.</span><span class="sxs-lookup"><span data-stu-id="03916-246">If you have configured any custom languages for this campaign, you can test hello push in each language.</span></span> <span data-ttu-id="03916-247">Můžete nastavit testovací zařízení z "Můj účet".</span><span class="sxs-lookup"><span data-stu-id="03916-247">You can setup a test device from “My Account”.</span></span>

> [!NOTE]
> <span data-ttu-id="03916-248">Žádné na straně serveru, který data se protokolují, když použijete tlačítko hello příliš "test" nabízených oznámení, data se protokolují pouze pro skutečné nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="03916-248">No server side data is logged when you use hello button too"test" pushes, data is only logged for real push campaigns.</span></span>

### <a name="see-also"></a><span data-ttu-id="03916-249">Viz také</span><span class="sxs-lookup"><span data-stu-id="03916-249">See also</span></span>
* <span data-ttu-id="03916-250">[Dokumentace k uživatelského rozhraní – Můj účet][Link 14]</span><span class="sxs-lookup"><span data-stu-id="03916-250">[UI Documentation - My Account][Link 14]</span></span>

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

