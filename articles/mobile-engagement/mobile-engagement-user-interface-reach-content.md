---
title: "Azure Mobile Engagement uživatelské rozhraní - Reach obsahu"
description: "Naučte se spravovat jedinečný obsah s různými typy kampaní nabízených oznámení v Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: add64f06-43c9-475c-8722-51cd00bb844b
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3741a43b74af5846e95e42d8a7b533621e780f2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a><span data-ttu-id="a542c-103">Jak spravovat jedinečný obsah s různými typy kampaní nabízených oznámení</span><span class="sxs-lookup"><span data-stu-id="a542c-103">How to manage the unique content of the different types of push notification campaigns</span></span>
<span data-ttu-id="a542c-104">Části obsahu nové kampaně reach můžete upravovat obsah oznámení, hlasování, datová oznámení a dlaždice (pouze Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="a542c-104">You can use the Content section of a new reach campaign to modify the content of your Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span> <span data-ttu-id="a542c-105">Nastavení obsahu kampaní nabízených je specifické pro daný typ kampaně.</span><span class="sxs-lookup"><span data-stu-id="a542c-105">The content setting of Push campaigns is specific to the type of campaign.</span></span> 

### <a name="content-types"></a><span data-ttu-id="a542c-106">Typy obsahu:</span><span class="sxs-lookup"><span data-stu-id="a542c-106">Content types:</span></span>
* <span data-ttu-id="a542c-107">Oznámení</span><span class="sxs-lookup"><span data-stu-id="a542c-107">Announcements</span></span>
* <span data-ttu-id="a542c-108">Hlasování</span><span class="sxs-lookup"><span data-stu-id="a542c-108">Polls</span></span>
* <span data-ttu-id="a542c-109">Datová oznámení</span><span class="sxs-lookup"><span data-stu-id="a542c-109">Data pushes</span></span>
* <span data-ttu-id="a542c-110">Dlaždice (pouze Windows Phone)</span><span class="sxs-lookup"><span data-stu-id="a542c-110">Tiles (Windows Phone Only)</span></span>

## <a name="content-of-announcements"></a><span data-ttu-id="a542c-111">Obsah oznámení</span><span class="sxs-lookup"><span data-stu-id="a542c-111">Content of Announcements</span></span>
 ![Reach Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a><span data-ttu-id="a542c-113">Zvolte typ sdělení:</span><span class="sxs-lookup"><span data-stu-id="a542c-113">Choose the type of your announcement:</span></span>
* <span data-ttu-id="a542c-114">Pouze oznámení: je jednoduchý standardní oznámení.</span><span class="sxs-lookup"><span data-stu-id="a542c-114">Notification only: It is a simple standard notification.</span></span> <span data-ttu-id="a542c-115">Znamená to, že pokud uživatel klikne na něm, bez dalšího zobrazení se zobrazí, ale jenom akce, které jsou přidružené k ní dojde.</span><span class="sxs-lookup"><span data-stu-id="a542c-115">Meaning that if a user clicks on it, no additional view will appear, but only the action associated to it will occur.</span></span>
* <span data-ttu-id="a542c-116">Text oznámení: je oznámení, že zapojí uživateli Podíváme se na zobrazení textu.</span><span class="sxs-lookup"><span data-stu-id="a542c-116">Text announcement: It is a notification that engages the user to have a look at a text view.</span></span>
* <span data-ttu-id="a542c-117">Sdělení webovém: je oznámení, že zapojí uživateli Podíváme se na webové zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a542c-117">Web announcement: It is a notification that engages the user to have a look at a web view.</span></span>

### <a name="see-also"></a><span data-ttu-id="a542c-118">Viz také</span><span class="sxs-lookup"><span data-stu-id="a542c-118">See also</span></span>
* <span data-ttu-id="a542c-119">[Dosažení – jak Tos – oznámení][Link 3]</span><span class="sxs-lookup"><span data-stu-id="a542c-119">[Reach - How Tos - Announcements][Link 3]</span></span> 

### <a name="about-web-view-announcements"></a><span data-ttu-id="a542c-120">O sděleních ve webovém zobrazení:</span><span class="sxs-lookup"><span data-stu-id="a542c-120">About Web View Announcements:</span></span>
<span data-ttu-id="a542c-121">Výskyty vzoru "{deviceid}" v kódu HTML nebo kód jazyka JavaScript, které tady zadáte, se automaticky nahradí identifikátorem zařízení, se zobrazuje oznámení.</span><span class="sxs-lookup"><span data-stu-id="a542c-121">Occurrences of the pattern "{deviceid}" in the HTML code or JavaScript code you provide here will be automatically replaced by the identifier of the device displaying the announcement.</span></span> <span data-ttu-id="a542c-122">Toto je snadný způsob, jak načíst identifikátory zařízení Azure Mobile Engagement přes externí webovou službu hostovanou na vašem interním systému.</span><span class="sxs-lookup"><span data-stu-id="a542c-122">This is an easy way to retrieve Azure Mobile Engagement device identifiers in an external web service hosted on your back office.</span></span>
<span data-ttu-id="a542c-123">Pokud chcete vytvořit webové zobrazení na celou obrazovku (bez výchozích tlačítek akce a ukončení), můžete použít následující funkce z javascriptového kódu vašeho sdělení ve webovém zobrazení:</span><span class="sxs-lookup"><span data-stu-id="a542c-123">If you want to create a full screen web view (without the default Action and Exit buttons we provide) you can use the following functions from your web view announcement's JavaScript code:</span></span> 

* <span data-ttu-id="a542c-124">provést akci sdělení: ReachContent.actionContent()</span><span class="sxs-lookup"><span data-stu-id="a542c-124">perform the announcement action: ReachContent.actionContent()</span></span>
* <span data-ttu-id="a542c-125">Ukončit ze sdělení: ReachContent.exitContent()</span><span class="sxs-lookup"><span data-stu-id="a542c-125">exit from the announcement: ReachContent.exitContent()</span></span>

### <a name="choose-your-action"></a><span data-ttu-id="a542c-126">Vyberte akci:</span><span class="sxs-lookup"><span data-stu-id="a542c-126">Choose your Action:</span></span>
### <a name="about-action-urls"></a><span data-ttu-id="a542c-127">O adresy URL akce:</span><span class="sxs-lookup"><span data-stu-id="a542c-127">About Action URLs:</span></span>
<span data-ttu-id="a542c-128">Jako adresa URL akce se dá použít libovolná adresa URL, která jde interpretovat operačním systémem zacíleného zařízení.</span><span class="sxs-lookup"><span data-stu-id="a542c-128">Any URL that can be interpreted by a targeted device's operating system can be used as an action URL.</span></span>
<span data-ttu-id="a542c-129">Jako adresu URL akce můžete použít rovněž jakoukoli vyhrazenou adresu URL, kterou vaše aplikace může podporovat (například k přechodu uživatelů na konkrétní obrazovku).</span><span class="sxs-lookup"><span data-stu-id="a542c-129">Any dedicated URL that your application might support (e.g. to make users jump to a particular screen) can also be used as an action URL.</span></span>
<span data-ttu-id="a542c-130">Každý výskyt vzoru {deviceid} se automaticky nahradí identifikátorem zařízení, které provádí akce.</span><span class="sxs-lookup"><span data-stu-id="a542c-130">Each occurrence of the {deviceid} pattern is automatically replaced by the identifier of the device performing the action.</span></span> <span data-ttu-id="a542c-131">To můžete použít k snadnému načtení identifikátorů zařízení Azure Mobile Engagement přes externí webovou službu hostovanou na vašem interním systému.</span><span class="sxs-lookup"><span data-stu-id="a542c-131">This can be used to easily retrieve Azure Mobile Engagement device identifiers via an external web service hosted on your back office.</span></span>

* <span data-ttu-id="a542c-132">**Android a iOS akce**</span><span class="sxs-lookup"><span data-stu-id="a542c-132">**Android + iOS actions**</span></span>
  * <span data-ttu-id="a542c-133">Otevřít webovou stránku</span><span class="sxs-lookup"><span data-stu-id="a542c-133">Open a web page</span></span>
  * <span data-ttu-id="a542c-134">http://\[domény webové lokality\]</span><span class="sxs-lookup"><span data-stu-id="a542c-134">http://\[web-site-domain\]</span></span> 
  * <span data-ttu-id="a542c-135">Příklad: http://www.azure.com</span><span class="sxs-lookup"><span data-stu-id="a542c-135">Example:http://www.azure.com</span></span>
  * <span data-ttu-id="a542c-136">Odeslat e-mail</span><span class="sxs-lookup"><span data-stu-id="a542c-136">Send an e-mail</span></span>
  * <span data-ttu-id="a542c-137">mailto:\[e-mailu-příjemce\]? subjektu =\[subjektu\]& textu =\[zpráv\]</span><span class="sxs-lookup"><span data-stu-id="a542c-137">mailto:\[e-mail-recipient\]?subject=\[subject\]&body=\[message\]</span></span> 
  * <span data-ttu-id="a542c-138">Example:mailto:foo@example.com? subjektu = pozdrav % 20from % 20Azure % 20Mobile % 20Engagement! & textu = 20stuff dobrý %!</span><span class="sxs-lookup"><span data-stu-id="a542c-138">Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!</span></span>
  * <span data-ttu-id="a542c-139">Odeslat SMS</span><span class="sxs-lookup"><span data-stu-id="a542c-139">Send a SMS</span></span>
  * <span data-ttu-id="a542c-140">SMS:\[telefonní číslo\]</span><span class="sxs-lookup"><span data-stu-id="a542c-140">sms:\[phone-number\]</span></span> 
  * <span data-ttu-id="a542c-141">Příklad: sms:2125551212</span><span class="sxs-lookup"><span data-stu-id="a542c-141">Example:sms:2125551212</span></span>
  * <span data-ttu-id="a542c-142">Vytočit telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="a542c-142">Dial a phone number</span></span>
  * <span data-ttu-id="a542c-143">Telefon:\[telefonní číslo\]</span><span class="sxs-lookup"><span data-stu-id="a542c-143">tel:\[phone-number\]</span></span> 
  * <span data-ttu-id="a542c-144">Příklad: tel:2125551212</span><span class="sxs-lookup"><span data-stu-id="a542c-144">Example:tel:2125551212</span></span>
* <span data-ttu-id="a542c-145">**Android pouze akce**</span><span class="sxs-lookup"><span data-stu-id="a542c-145">**Android only actions**</span></span>
  * <span data-ttu-id="a542c-146">Stáhnout aplikaci v obchodě Play</span><span class="sxs-lookup"><span data-stu-id="a542c-146">Download an application on the Play Store</span></span>
  * <span data-ttu-id="a542c-147">Market://details?ID=\[balíček aplikace\]</span><span class="sxs-lookup"><span data-stu-id="a542c-147">market://details?id=\[app package\]</span></span> 
  * <span data-ttu-id="a542c-148">Příklad: market://details?id=com.microsoft.office.word</span><span class="sxs-lookup"><span data-stu-id="a542c-148">Example:market://details?id=com.microsoft.office.word</span></span>
  * <span data-ttu-id="a542c-149">Spustit hledání se zjištěním polohy</span><span class="sxs-lookup"><span data-stu-id="a542c-149">Start a geo-localized search</span></span>
  * <span data-ttu-id="a542c-150">GEO:0, 0? q =\[vyhledávací dotaz.\]</span><span class="sxs-lookup"><span data-stu-id="a542c-150">geo:0,0?q=\[search query\]</span></span> 
  * <span data-ttu-id="a542c-151">Příklad: geo:0, 0? q = starbucks, Paříž</span><span class="sxs-lookup"><span data-stu-id="a542c-151">Example:geo:0,0?q=starbucks,paris</span></span>
* <span data-ttu-id="a542c-152">**iOS pouze akce**</span><span class="sxs-lookup"><span data-stu-id="a542c-152">**iOS only actions**</span></span>
  * <span data-ttu-id="a542c-153">Stáhnout aplikaci v obchodě App Store</span><span class="sxs-lookup"><span data-stu-id="a542c-153">Download an application on the App Store</span></span>
  * <span data-ttu-id="a542c-154">http://iTunes.Apple.com/ [Země] /app/ [název aplikace] /id [id aplikace]? mt = 8</span><span class="sxs-lookup"><span data-stu-id="a542c-154">http://itunes.apple.com/[country]/app/[app name]/id[app id]?mt=8</span></span> 
  * <span data-ttu-id="a542c-155">Příklad: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8</span><span class="sxs-lookup"><span data-stu-id="a542c-155">Example:http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8</span></span>
  * <span data-ttu-id="a542c-156">Akce Windows</span><span class="sxs-lookup"><span data-stu-id="a542c-156">Windows Actions</span></span>
  * <span data-ttu-id="a542c-157">Otevřít webovou stránku</span><span class="sxs-lookup"><span data-stu-id="a542c-157">Open a web page</span></span>
  * <span data-ttu-id="a542c-158">http://\[domény webové lokality\]</span><span class="sxs-lookup"><span data-stu-id="a542c-158">http://\[web-site-domain\]</span></span> 
  * <span data-ttu-id="a542c-159">Příklad: http://www.azure.com</span><span class="sxs-lookup"><span data-stu-id="a542c-159">Example:http://www.azure.com</span></span>
  * <span data-ttu-id="a542c-160">Odeslat e-mail</span><span class="sxs-lookup"><span data-stu-id="a542c-160">Send an e-mail</span></span>
  * <span data-ttu-id="a542c-161">mailto:\[e-mailu-příjemce\]? subjektu =\[subjektu\]& textu =\[zpráv\]</span><span class="sxs-lookup"><span data-stu-id="a542c-161">mailto:\[e-mail-recipient\]?subject=\[subject\]&body=\[message\]</span></span> 
  * <span data-ttu-id="a542c-162">Example:mailto:foo@example.com? subjektu = pozdrav % 20from % 20Azure % 20Mobile % 20Engagement! & textu = 20stuff dobrý %!</span><span class="sxs-lookup"><span data-stu-id="a542c-162">Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!</span></span>
  * <span data-ttu-id="a542c-163">Odeslat SMS (vyžaduje se aplikace Skype pro Store)</span><span class="sxs-lookup"><span data-stu-id="a542c-163">Send a SMS (Skype Store App required)</span></span>
  * <span data-ttu-id="a542c-164">SMS:\[telefonní číslo\]</span><span class="sxs-lookup"><span data-stu-id="a542c-164">sms:\[phone-number\]</span></span> 
  * <span data-ttu-id="a542c-165">Příklad: sms:2125551212</span><span class="sxs-lookup"><span data-stu-id="a542c-165">Example:sms:2125551212</span></span>
  * <span data-ttu-id="a542c-166">Vytočit telefonní číslo (vyžaduje se aplikace Skype pro Store)</span><span class="sxs-lookup"><span data-stu-id="a542c-166">Dial a phone number (Skype Store App required)</span></span>
  * <span data-ttu-id="a542c-167">Telefon:\[telefonní číslo\]</span><span class="sxs-lookup"><span data-stu-id="a542c-167">tel:\[phone-number\]</span></span> 
  * <span data-ttu-id="a542c-168">Příklad: tel:2125551212</span><span class="sxs-lookup"><span data-stu-id="a542c-168">Example:tel:2125551212</span></span>
  * <span data-ttu-id="a542c-169">Stáhnout aplikaci v obchodě Play</span><span class="sxs-lookup"><span data-stu-id="a542c-169">Download an application on the Play Store</span></span>
  * <span data-ttu-id="a542c-170">MS-windows-úložiště: PDP? PFN =\[ID balíčku aplikace\]</span><span class="sxs-lookup"><span data-stu-id="a542c-170">ms-windows-store:PDP?PFN=\[app package ID\]</span></span> 
  * <span data-ttu-id="a542c-171">Příklad: ms-windows-úložiště: PDP? PFN = 4d91298a-07cb-40fb-aecc-4cb5615d53c1</span><span class="sxs-lookup"><span data-stu-id="a542c-171">Example:ms-windows-store:PDP?PFN=4d91298a-07cb-40fb-aecc-4cb5615d53c1</span></span>
  * <span data-ttu-id="a542c-172">Zahájit hledání v Mapách Bing</span><span class="sxs-lookup"><span data-stu-id="a542c-172">Start a bingmaps search</span></span>
  * <span data-ttu-id="a542c-173">bingmaps:? q =\[vyhledávací dotaz.\]</span><span class="sxs-lookup"><span data-stu-id="a542c-173">bingmaps:?q=\[search query\]</span></span> 
  * <span data-ttu-id="a542c-174">Příklad: bingmaps:? q = starbucks, Paříž</span><span class="sxs-lookup"><span data-stu-id="a542c-174">Example:bingmaps:?q=starbucks,paris</span></span>
  * <span data-ttu-id="a542c-175">Použít vlastní schéma</span><span class="sxs-lookup"><span data-stu-id="a542c-175">Use a custom scheme</span></span>
  * <span data-ttu-id="a542c-176">\[vlastní schéma\]://\[parametry vlastního schématu\]</span><span class="sxs-lookup"><span data-stu-id="a542c-176">\[custom scheme\]://\[custom scheme params\]</span></span> 
  * <span data-ttu-id="a542c-177">Příklad: myCustomProtocol://myCustomParams</span><span class="sxs-lookup"><span data-stu-id="a542c-177">Example:myCustomProtocol://myCustomParams</span></span>
  * <span data-ttu-id="a542c-178">Použít data balíčku (vyžaduje se aplikace Store pro rozšíření pro čtení)</span><span class="sxs-lookup"><span data-stu-id="a542c-178">Use a package data (Store App for extension read required)</span></span>
  * <span data-ttu-id="a542c-179">\[Složka\]\[data\].\[ rozšíření\]</span><span class="sxs-lookup"><span data-stu-id="a542c-179">\[folder\]\[data\].\[extension\]</span></span> 
  * <span data-ttu-id="a542c-180">Example:myfolderdata.txt</span><span class="sxs-lookup"><span data-stu-id="a542c-180">Example:myfolderdata.txt</span></span>

### <a name="build-a-tracking-url"></a><span data-ttu-id="a542c-181">Sestavte adresu URL pro sledování:</span><span class="sxs-lookup"><span data-stu-id="a542c-181">Build a Tracking URL:</span></span>
* <span data-ttu-id="a542c-182">Najdete v části "Nastavení" <UI Documentation> pro instrukce k sestavení sledovací adresu URL, která umožní uživatelům stáhnout jednu z jiné aplikace.</span><span class="sxs-lookup"><span data-stu-id="a542c-182">See the “Settings” section of the <UI Documentation> for instruction on building a tracking URL that will allow users to download one of your other applications.</span></span>

### <a name="define-the-texts-of-your-announcement"></a><span data-ttu-id="a542c-183">Definujte texty sdělení.</span><span class="sxs-lookup"><span data-stu-id="a542c-183">Define the texts of your announcement</span></span>
<span data-ttu-id="a542c-184">Zadejte název, obsah a tlačítko texty sdělení.</span><span class="sxs-lookup"><span data-stu-id="a542c-184">Fill in the title, content, and button texts of your announcement.</span></span> <span data-ttu-id="a542c-185">Můžete vybrat cílovou skupinu na základě názorů reach o tom, jak odpověděl uživatelé tuto kampaň budoucí kampaně.</span><span class="sxs-lookup"><span data-stu-id="a542c-185">You can target an audience of a future campaign based on the reach feedback of how users responded to this campaign.</span></span> <span data-ttu-id="a542c-186">Cílení na publikum může být založen na zpětnou vazbu o tom, jestli se tato kampaň právě nabídnutých, zodpovězených, reakcí nebo ukončením.</span><span class="sxs-lookup"><span data-stu-id="a542c-186">Audience targeting can be based on the feedback of whether this campaign was just pushed, replied, actioned, or exited.</span></span>

### <a name="see-also"></a><span data-ttu-id="a542c-187">Viz také</span><span class="sxs-lookup"><span data-stu-id="a542c-187">See also</span></span>
* <span data-ttu-id="a542c-188">[Nové nabízené kritérium dokumentace - Reach - uživatelského rozhraní][Link 28]</span><span class="sxs-lookup"><span data-stu-id="a542c-188">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

## <a name="content-of-polls"></a><span data-ttu-id="a542c-189">Obsah hlasování</span><span class="sxs-lookup"><span data-stu-id="a542c-189">Content of Polls</span></span>
![Reach Content2][31] 

<span data-ttu-id="a542c-191">Zadejte název, popis a tlačítko texty sdělení.</span><span class="sxs-lookup"><span data-stu-id="a542c-191">Fill in the title, description, and button texts of your announcement.</span></span> <span data-ttu-id="a542c-192">Pak přidejte otázky a možnosti pro odpovědi na otázky.</span><span class="sxs-lookup"><span data-stu-id="a542c-192">Then, add questions and choices for the answers to your questions.</span></span>
<span data-ttu-id="a542c-193">Můžete vybrat cílovou skupinu na základě názorů reach o tom, jak odpověděl uživatelé tuto kampaň budoucí kampaně.</span><span class="sxs-lookup"><span data-stu-id="a542c-193">You can target an audience of a future campaign based on the reach feedback of how users responded to this campaign.</span></span> <span data-ttu-id="a542c-194">Cílení na publikum může být založené na tom, jestli se tato kampaň právě nabídnutých, zodpovězených, reakcí nebo ukončením.</span><span class="sxs-lookup"><span data-stu-id="a542c-194">Audience targeting can be based on whether this campaign was just pushed, replied, actioned, or exited.</span></span> <span data-ttu-id="a542c-195">Cílení na publikum může být taky založené na dotazování odpovědí zpětnou vazbu, kde jsou otázka a odpověď volba použít jako kritéria.</span><span class="sxs-lookup"><span data-stu-id="a542c-195">Audience targeting can also be based on Poll answer feedback, where the question and answer choice are used as criteria.</span></span>

### <a name="see-also"></a><span data-ttu-id="a542c-196">Viz také</span><span class="sxs-lookup"><span data-stu-id="a542c-196">See also</span></span>
* <span data-ttu-id="a542c-197">[Nové nabízené kritérium dokumentace - Reach - uživatelského rozhraní][Link 28]</span><span class="sxs-lookup"><span data-stu-id="a542c-197">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

## <a name="content-of-data-pushes"></a><span data-ttu-id="a542c-198">Obsah datová oznámení</span><span class="sxs-lookup"><span data-stu-id="a542c-198">Content of Data Pushes</span></span>
![Reach Content3][32] 

### <a name="choose-the-type-of-your-data"></a><span data-ttu-id="a542c-200">Vyberte typ dat:</span><span class="sxs-lookup"><span data-stu-id="a542c-200">Choose the type of your data:</span></span>
* <span data-ttu-id="a542c-201">Text</span><span class="sxs-lookup"><span data-stu-id="a542c-201">Text</span></span>
* <span data-ttu-id="a542c-202">Binární data</span><span class="sxs-lookup"><span data-stu-id="a542c-202">Binary data</span></span>
* <span data-ttu-id="a542c-203">Data formátu Base64.</span><span class="sxs-lookup"><span data-stu-id="a542c-203">Base64 data</span></span>

### <a name="define-the-content-of-your-data"></a><span data-ttu-id="a542c-204">Definujte obsah dat.</span><span class="sxs-lookup"><span data-stu-id="a542c-204">Define the content of your data</span></span>
* <span data-ttu-id="a542c-205">Pokud jste vybrali nabízet text data, zkopírujte a vložte text do pole "obsah".</span><span class="sxs-lookup"><span data-stu-id="a542c-205">If you selected to push text data, copy and paste the text into the "content" box.</span></span>
* <span data-ttu-id="a542c-206">Pokud jste vybrali nabízet data binární nebo base64, pomocí tlačítka "nahrát soubor" k odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="a542c-206">If you selected to push either binary or base64 data, use the "upload your file" button to upload your file.</span></span>
* <span data-ttu-id="a542c-207">Můžete vybrat cílovou skupinu na základě názorů reach o tom, jak odpověděl uživatelé tuto kampaň budoucí kampaně.</span><span class="sxs-lookup"><span data-stu-id="a542c-207">You can target an audience of a future campaign based on the reach feedback of how users responded to this campaign.</span></span> <span data-ttu-id="a542c-208">Cílení na publikum může být založené na tom, jestli se tato kampaň právě nabídnutých, zodpovězených, reakcí nebo ukončením.</span><span class="sxs-lookup"><span data-stu-id="a542c-208">Audience targeting can be based on whether this campaign was just pushed, replied, actioned, or exited.</span></span>

### <a name="see-also"></a><span data-ttu-id="a542c-209">Viz také</span><span class="sxs-lookup"><span data-stu-id="a542c-209">See also</span></span>
* <span data-ttu-id="a542c-210">[Nové nabízené kritérium dokumentace - Reach - uživatelského rozhraní][Link 28]</span><span class="sxs-lookup"><span data-stu-id="a542c-210">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

## <a name="content-of-tiles-windows-phone-only"></a><span data-ttu-id="a542c-211">Obsah dlaždice (pouze Windows Phone)</span><span class="sxs-lookup"><span data-stu-id="a542c-211">Content of Tiles (Windows Phone only)</span></span>
![Reach Content4][33]

### <a name="define-the-content-of-your-tile"></a><span data-ttu-id="a542c-213">Definujte obsah dlaždice.</span><span class="sxs-lookup"><span data-stu-id="a542c-213">Define the content of your tile</span></span>
<span data-ttu-id="a542c-214">Datová část dlaždice je text, který se zobrazí na dlaždici aplikace na zařízení Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="a542c-214">The tile payload is the text to be displayed in the tile of your app on Windows Phone devices.</span></span>
<span data-ttu-id="a542c-215">Push dlaždice je verze služby Microsoft nabízených oznámení (MPNS) nativního nabízení pro Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="a542c-215">A tile push is the Microsoft Push Notification Service (MPNS) version of a native push for Windows Phone.</span></span> <span data-ttu-id="a542c-216">Typ nabízeného dlaždice je pouze typ push, který nemá odpověď a proto nemůže být cílové skupiny kampaní budoucí založený na výsledky dlaždice nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="a542c-216">The tile push type is the only push type that does not have a response and so the audience of future campaigns can't be built on the results of a tile push campaign.</span></span> 

### <a name="see-also"></a><span data-ttu-id="a542c-217">Viz také</span><span class="sxs-lookup"><span data-stu-id="a542c-217">See also</span></span>
* <span data-ttu-id="a542c-218">[Rozhraní API dokumentace - Reach API - nativního nabízení][Link 4]</span><span class="sxs-lookup"><span data-stu-id="a542c-218">[API Documentation - Reach API - Native Push][Link 4]</span></span>

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

