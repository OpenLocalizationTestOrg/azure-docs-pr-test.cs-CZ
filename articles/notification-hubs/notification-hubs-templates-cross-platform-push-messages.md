---
title: "Šablony"
description: "Toto téma vysvětluje šablon pro Azure notification hubs."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: a41897bb-5b4b-48b2-bfd5-2e3c65edc37e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 1ca24a4bf08ecdbe1c1e47a931613144309a04a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="templates"></a><span data-ttu-id="46a9a-103">Šablony</span><span class="sxs-lookup"><span data-stu-id="46a9a-103">Templates</span></span>
## <a name="overview"></a><span data-ttu-id="46a9a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="46a9a-104">Overview</span></span>
<span data-ttu-id="46a9a-105">Šablony povolte klientská aplikace k určení přesném formátu, které chce dostávat oznámení.</span><span class="sxs-lookup"><span data-stu-id="46a9a-105">Templates enable a client application to specify the exact format of the notifications it wants to receive.</span></span> <span data-ttu-id="46a9a-106">Pomocí šablon, aplikace můžou realizovat několik různých výhod, včetně následujících:</span><span class="sxs-lookup"><span data-stu-id="46a9a-106">Using templates, an app can realize several different benefits, including the following :</span></span>

* <span data-ttu-id="46a9a-107">Back-end bez ohledu na platformy</span><span class="sxs-lookup"><span data-stu-id="46a9a-107">A platform-agnostic backend</span></span>
* <span data-ttu-id="46a9a-108">Přizpůsobené oznámení</span><span class="sxs-lookup"><span data-stu-id="46a9a-108">Personalized notifications</span></span>
* <span data-ttu-id="46a9a-109">Nezávislost verze klienta</span><span class="sxs-lookup"><span data-stu-id="46a9a-109">Client-version independence</span></span>
* <span data-ttu-id="46a9a-110">Snadno lokalizace</span><span class="sxs-lookup"><span data-stu-id="46a9a-110">Easy localization</span></span>

<span data-ttu-id="46a9a-111">Tato část obsahuje dva podrobné příklady, jak používat šablony k odesílání oznámení bez ohledu na platformu cílení na všechna zařízení napříč platformami a přizpůsobení všesměrového vysílání oznámení pro každé zařízení.</span><span class="sxs-lookup"><span data-stu-id="46a9a-111">This section provides two in-depth examples of how to use templates to send platform-agnostic notifications targeting all your devices across platforms, and to personalize broadcast notification to each device.</span></span>

## <a name="using-templates-cross-platform"></a><span data-ttu-id="46a9a-112">Pomocí šablony a platformy</span><span class="sxs-lookup"><span data-stu-id="46a9a-112">Using templates cross-platform</span></span>
<span data-ttu-id="46a9a-113">Standardní způsob k odesílání nabízených oznámení je odeslání pro jednotlivá oznámení, která má být odeslán konkrétní datové části pro služby oznámení platformy (WNS, APNS).</span><span class="sxs-lookup"><span data-stu-id="46a9a-113">The standard way to send push notifications is to send, for each notification that is to be sent, a specific payload to platform notification services (WNS, APNS).</span></span> <span data-ttu-id="46a9a-114">Pokud chcete odeslat výstrahu do služby APN, například datové části je objekt Json v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="46a9a-114">For example, to send an alert to APNS, the payload is a Json object of the following form:</span></span>

    {"aps": {"alert" : "Hello!" }}

<span data-ttu-id="46a9a-115">Odeslat zprávu podobné informační zprávy v aplikaci pro Windows Store, datovou část XML je následující:</span><span class="sxs-lookup"><span data-stu-id="46a9a-115">To send a similar toast message on a Windows Store application, the XML payload is as follows:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">Hello!</text>
        </binding>
      </visual>
    </toast>

<span data-ttu-id="46a9a-116">Podobně jako datové části můžete vytvořit pro platformy (Android) GCM a MPNS (Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="46a9a-116">You can create similar payloads for MPNS (Windows Phone) and GCM (Android) platforms.</span></span>

<span data-ttu-id="46a9a-117">Tento požadavek vynutí back-end aplikace k vytvoření různých datových částí pro každou platformu a efektivně zajišťuje back-end zodpovědná za součástí prezentační vrstvy aplikace.</span><span class="sxs-lookup"><span data-stu-id="46a9a-117">This requirement forces the app backend to produce different payloads for each platform, and effectively makes the backend responsible for part of the presentation layer of the app.</span></span> <span data-ttu-id="46a9a-118">Některé aspekty patří lokalizace a grafické rozložení (hlavně u aplikací pro Windows Store, které obsahují oznámení pro různé typy dlaždic).</span><span class="sxs-lookup"><span data-stu-id="46a9a-118">Some concerns include localization and graphical layouts (especially for Windows Store apps that include notifications for various types of tiles).</span></span>

<span data-ttu-id="46a9a-119">Funkce šablony Notification Hubs umožňuje klientskou aplikaci vytvořit speciální registrace, nazývá šablona registrace, které obsahují, kromě sadu značky, šablony.</span><span class="sxs-lookup"><span data-stu-id="46a9a-119">The Notification Hubs template feature enables a client app to create special registrations, called template registrations, which include, in addition to the set of tags, a template.</span></span> <span data-ttu-id="46a9a-120">Funkce šablony Notification Hubs umožňuje klientskou aplikaci pro přidružení zařízení se šablonami, zda pracujete s instalací (doporučeno) nebo registrací.</span><span class="sxs-lookup"><span data-stu-id="46a9a-120">The Notification Hubs template feature enables a client app to associate devices with templates whether you are working with Installations (preferred) or Registrations.</span></span> <span data-ttu-id="46a9a-121">Informace pouze nezávislé na platformě zadané v předchozích příkladech datové části, je skutečný zpráva s výstrahou (Hello!).</span><span class="sxs-lookup"><span data-stu-id="46a9a-121">Given the preceding payload examples, the only platform-independent information is the actual alert message (Hello!).</span></span> <span data-ttu-id="46a9a-122">Šablona je sada pokynů pro Centrum oznámení o tom, jak formátování zprávy nezávislé na platformě pro registraci tohoto konkrétního klienta aplikace.</span><span class="sxs-lookup"><span data-stu-id="46a9a-122">A template is a set of instructions for the Notification Hub on how to format a platform-independent message for the registration of that specific client app.</span></span> <span data-ttu-id="46a9a-123">V předchozím příkladu nezávislé zpráva platformy je jedinou vlastností: **zpráva = Hello!**.</span><span class="sxs-lookup"><span data-stu-id="46a9a-123">In the preceding example, the platform independent message is a single property: **message = Hello!**.</span></span>

<span data-ttu-id="46a9a-124">Následující obrázek znázorňuje proces výše:</span><span class="sxs-lookup"><span data-stu-id="46a9a-124">The following picture illustrates the above process:</span></span>

![](./media/notification-hubs-templates/notification-hubs-hello.png)

<span data-ttu-id="46a9a-125">Šablona pro registraci aplikace iOS klienta vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="46a9a-125">The template for the iOS client app registration is as follows:</span></span>

    {"aps": {"alert": "$(message)"}}

<span data-ttu-id="46a9a-126">Odpovídající šablonu pro klientské aplikace Windows Store je:</span><span class="sxs-lookup"><span data-stu-id="46a9a-126">The corresponding template for the Windows Store client app is:</span></span>

    <toast>
        <visual>
            <binding template=\"ToastText01\">
                <text id=\"1\">$(message)</text>
            </binding>
        </visual>
    </toast>

<span data-ttu-id="46a9a-127">Všimněte si, že skutečné zpráva nahradí výraz $(zprávy).</span><span class="sxs-lookup"><span data-stu-id="46a9a-127">Notice that the actual message is substituted for the expression $(message).</span></span> <span data-ttu-id="46a9a-128">Tento výraz dá pokyn centru oznámení pokaždé, když ji odešle zprávu do této konkrétní registraci, k vytvoření zprávu, která odpovídá jeho a přepínačů v hodnotě běžné.</span><span class="sxs-lookup"><span data-stu-id="46a9a-128">This expression instructs the Notification Hub, whenever it sends a message to this particular registration, to build a message that follows it and switches in the common value.</span></span>

<span data-ttu-id="46a9a-129">Pokud pracujete s modelem instalace, instalace "šablony" klíč obsahuje JSON více šablon.</span><span class="sxs-lookup"><span data-stu-id="46a9a-129">If you are working with Installation model, the installation “templates” key holds a JSON of multiple templates.</span></span> <span data-ttu-id="46a9a-130">Pokud pracujete s modelem registrace, klientská aplikace můžete vytvořit více registrace Chcete-li použít několik šablon; například šablonu pro oznámení a šablonu pro dlaždici aktualizací.</span><span class="sxs-lookup"><span data-stu-id="46a9a-130">If you are working with Registration model, the client application can create multiple registrations in order to use multiple templates; for example, a template for alert messages and a template for tile updates.</span></span> <span data-ttu-id="46a9a-131">Klientské aplikace můžete také kombinovat nativní registrace (registrace s žádnou šablonu) a šablony registrace.</span><span class="sxs-lookup"><span data-stu-id="46a9a-131">Client applications can also mix native registrations (registrations with no template) and template registrations.</span></span>

<span data-ttu-id="46a9a-132">Centra oznámení odešle jedno oznámení ke každé šabloně bez ohledu, jestli patří do stejné klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="46a9a-132">The Notification Hub sends one notification for each template without considering whether they belong to the same client app.</span></span> <span data-ttu-id="46a9a-133">Toto chování lze přeložit nezávislé na platformě oznámení do další oznámení.</span><span class="sxs-lookup"><span data-stu-id="46a9a-133">This behavior can be used to translate platform-independent notifications into more notifications.</span></span> <span data-ttu-id="46a9a-134">Například stejnou platformu nezávislé zprávu do centra oznámení můžete bezproblémově přeložit v informační výstrahy a aktualizace dlaždice, bez nutnosti back-end zajímat ho.</span><span class="sxs-lookup"><span data-stu-id="46a9a-134">For example, the same platform independent message to the Notification Hub can be seamlessly translated in a toast alert and a tile update, without requiring the backend to be aware of it.</span></span> <span data-ttu-id="46a9a-135">Všimněte si, že některé platformy (například iOS) může sbalit několik oznámení do stejného zařízení, pokud jsou odesílány v krátké době.</span><span class="sxs-lookup"><span data-stu-id="46a9a-135">Note that some platforms (for example, iOS) might collapse multiple notifications to the same device if they are sent in a short period of time.</span></span>

## <a name="using-templates-for-personalization"></a><span data-ttu-id="46a9a-136">Pomocí šablon pro přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="46a9a-136">Using templates for personalization</span></span>
<span data-ttu-id="46a9a-137">Další výhodou použití šablony je schopnost provádět přizpůsobení za registraci oznámení pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="46a9a-137">Another advantage to using templates is the ability to use Notification Hubs to perform per-registration personalization of notifications.</span></span> <span data-ttu-id="46a9a-138">Představte si třeba počasí aplikaci, která zobrazuje dlaždici počasí na určité místo.</span><span class="sxs-lookup"><span data-stu-id="46a9a-138">For example, consider a weather app that displays a tile with the weather conditions at a specific location.</span></span> <span data-ttu-id="46a9a-139">Uživatel může zvolit c nebo Fahrenheita stupňů a jednoho nebo pětidenního prognózy.</span><span class="sxs-lookup"><span data-stu-id="46a9a-139">A user can choose between Celsius or Fahrenheit degrees, and a single or five-day forecast.</span></span> <span data-ttu-id="46a9a-140">Pomocí šablon, každou instalaci klienta aplikace můžete zaregistrovat pro formát požadované (1 den Celsius, 1 den Fahrenheita, 5 dní c, 5 dní Fahrenheita kurzy), a back-end odeslat zprávu, která obsahuje všechny informace potřebné k vyplnění těchto šablon (například pět dní prognózy s c a Fahrenheita stupních).</span><span class="sxs-lookup"><span data-stu-id="46a9a-140">Using templates, each client app installation can register for the format required (1-day Celsius, 1-day Fahrenheit, 5-days Celsius, 5-days Fahrenheit), and have the backend send a single message that contains all the information required to fill those templates (for example, a five-day forecast with Celsius and Fahrenheit degrees).</span></span>

<span data-ttu-id="46a9a-141">Šablona pro jeden den prognózy s c teploty vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="46a9a-141">The template for the one-day forecast with Celsius temperatures is as follows:</span></span>

    <tile>
      <visual>
        <binding template="TileWideSmallImageAndText04">
          <image id="1" src="$(day1_image)" alt="alt text"/>
          <text id="1">Seattle, WA</text>
          <text id="2">$(day1_tempC)</text>
        </binding>  
      </visual>
    </tile>

<span data-ttu-id="46a9a-142">Zpráva odeslaná do centra oznámení obsahuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="46a9a-142">The message sent to the Notification Hub contains all the following properties:</span></span>

<table border="1">

<tr><td><span data-ttu-id="46a9a-143">day1_image</span><span class="sxs-lookup"><span data-stu-id="46a9a-143">day1_image</span></span></td><td><span data-ttu-id="46a9a-144">day2_image</span><span class="sxs-lookup"><span data-stu-id="46a9a-144">day2_image</span></span></td><td><span data-ttu-id="46a9a-145">day3_image</span><span class="sxs-lookup"><span data-stu-id="46a9a-145">day3_image</span></span></td><td><span data-ttu-id="46a9a-146">day4_image</span><span class="sxs-lookup"><span data-stu-id="46a9a-146">day4_image</span></span></td><td><span data-ttu-id="46a9a-147">day5_image</span><span class="sxs-lookup"><span data-stu-id="46a9a-147">day5_image</span></span></td></tr>

<tr><td><span data-ttu-id="46a9a-148">day1_tempC</span><span class="sxs-lookup"><span data-stu-id="46a9a-148">day1_tempC</span></span></td><td><span data-ttu-id="46a9a-149">day2_tempC</span><span class="sxs-lookup"><span data-stu-id="46a9a-149">day2_tempC</span></span></td><td><span data-ttu-id="46a9a-150">day3_tempC</span><span class="sxs-lookup"><span data-stu-id="46a9a-150">day3_tempC</span></span></td><td><span data-ttu-id="46a9a-151">day4_tempC</span><span class="sxs-lookup"><span data-stu-id="46a9a-151">day4_tempC</span></span></td><td><span data-ttu-id="46a9a-152">day5_tempC</span><span class="sxs-lookup"><span data-stu-id="46a9a-152">day5_tempC</span></span></td></tr>

<tr><td><span data-ttu-id="46a9a-153">day1_tempF</span><span class="sxs-lookup"><span data-stu-id="46a9a-153">day1_tempF</span></span></td><td><span data-ttu-id="46a9a-154">day2_tempF</span><span class="sxs-lookup"><span data-stu-id="46a9a-154">day2_tempF</span></span></td><td><span data-ttu-id="46a9a-155">day3_tempF</span><span class="sxs-lookup"><span data-stu-id="46a9a-155">day3_tempF</span></span></td><td><span data-ttu-id="46a9a-156">day4_tempF</span><span class="sxs-lookup"><span data-stu-id="46a9a-156">day4_tempF</span></span></td><td><span data-ttu-id="46a9a-157">day5_tempF</span><span class="sxs-lookup"><span data-stu-id="46a9a-157">day5_tempF</span></span></td></tr>
</table><br/>

<span data-ttu-id="46a9a-158">Pomocí tohoto vzoru back-end pouze odešle do jedné zprávy bez nutnosti ukládat konkrétní nastavení možnosti pro uživatele aplikace.</span><span class="sxs-lookup"><span data-stu-id="46a9a-158">By using this pattern, the backend only sends a single message without having to store specific personalization options for the app users.</span></span> <span data-ttu-id="46a9a-159">Následující obrázek znázorňuje tento scénář:</span><span class="sxs-lookup"><span data-stu-id="46a9a-159">The following picture illustrates this scenario:</span></span>

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-to-register-templates"></a><span data-ttu-id="46a9a-160">Postup registrace šablony</span><span class="sxs-lookup"><span data-stu-id="46a9a-160">How to register templates</span></span>
<span data-ttu-id="46a9a-161">Zaregistrovat se šablonami pomocí modelu instalace (doporučeno) nebo model registraci, najdete v tématu [registrace správu](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="46a9a-161">To register with templates using the Installation model (preferred), or the Registration model, see [Registration Management](notification-hubs-push-notification-registration-management.md).</span></span>

## <a name="template-expression-language"></a><span data-ttu-id="46a9a-162">Výraz jazyka šablony</span><span class="sxs-lookup"><span data-stu-id="46a9a-162">Template expression language</span></span>
<span data-ttu-id="46a9a-163">Šablony jsou omezeny na formáty XML nebo JSON dokumentů.</span><span class="sxs-lookup"><span data-stu-id="46a9a-163">Templates are limited to XML or JSON document formats.</span></span> <span data-ttu-id="46a9a-164">Navíc můžete pouze umístit výrazy konkrétní místech; například atributy uzlu nebo hodnoty pro formát XML řetězec hodnoty vlastností pro formát JSON.</span><span class="sxs-lookup"><span data-stu-id="46a9a-164">Also, you can only place expressions in particular places; for example, node attributes or values for XML, string property values for JSON.</span></span>

<span data-ttu-id="46a9a-165">V následující tabulce jsou povolené v šablonách jazyk:</span><span class="sxs-lookup"><span data-stu-id="46a9a-165">The following table shows the language allowed in templates:</span></span>

| <span data-ttu-id="46a9a-166">výraz</span><span class="sxs-lookup"><span data-stu-id="46a9a-166">Expression</span></span> | <span data-ttu-id="46a9a-167">Popis</span><span class="sxs-lookup"><span data-stu-id="46a9a-167">Description</span></span> |
| --- | --- |
| <span data-ttu-id="46a9a-168">$(prop)</span><span class="sxs-lookup"><span data-stu-id="46a9a-168">$(prop)</span></span> |<span data-ttu-id="46a9a-169">Odkaz na vlastnost události se zadaným názvem.</span><span class="sxs-lookup"><span data-stu-id="46a9a-169">Reference to an event property with the given name.</span></span> <span data-ttu-id="46a9a-170">Názvy vlastností nerozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="46a9a-170">Property names are not case-sensitive.</span></span> <span data-ttu-id="46a9a-171">Tento výraz přeloží do vlastnosti textové hodnoty nebo na prázdný řetězec, pokud vlastnost není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="46a9a-171">This expression resolves into the property’s text value or into an empty string if the property is not present.</span></span> |
| <span data-ttu-id="46a9a-172">$(prop, n)</span><span class="sxs-lookup"><span data-stu-id="46a9a-172">$(prop, n)</span></span> |<span data-ttu-id="46a9a-173">Jak je uvedeno výše zatímco text elementu je explicitně oříznut n znaků, například $(název, 20) klipy obsah název vlastnosti v 20 znaků.</span><span class="sxs-lookup"><span data-stu-id="46a9a-173">As above, but the text is explicitly clipped at n characters, for example $(title, 20) clips the contents of the title property at 20 characters.</span></span> |
| <span data-ttu-id="46a9a-174">. (prop, n)</span><span class="sxs-lookup"><span data-stu-id="46a9a-174">.(prop, n)</span></span> |<span data-ttu-id="46a9a-175">Jak je uvedeno výše ale text je na konci se třemi tečkami jako je oříznut.</span><span class="sxs-lookup"><span data-stu-id="46a9a-175">As above, but the text is suffixed with three dots as it is clipped.</span></span> <span data-ttu-id="46a9a-176">Celková velikost oříznutí řetězec a příponu nepřesahuje n znaků.</span><span class="sxs-lookup"><span data-stu-id="46a9a-176">The total size of the clipped string and the suffix does not exceed n characters.</span></span> <span data-ttu-id="46a9a-177">. (title, 20) s vstupní vlastností "Toto je název řádku" výsledků v **Toto je název...**</span><span class="sxs-lookup"><span data-stu-id="46a9a-177">.(title, 20) with an input property of “This is the title line” results in **This is the title...**</span></span> |
| <span data-ttu-id="46a9a-178">%(Prop)</span><span class="sxs-lookup"><span data-stu-id="46a9a-178">%(prop)</span></span> |<span data-ttu-id="46a9a-179">Podobně jako u $(name) s tím rozdílem, že výstupem je kódovaný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="46a9a-179">Similar to $(name) except that the output is URI-encoded.</span></span> |
| <span data-ttu-id="46a9a-180">#(prop)</span><span class="sxs-lookup"><span data-stu-id="46a9a-180">#(prop)</span></span> |<span data-ttu-id="46a9a-181">Používá se v šablony JSON (například pro iOS a Android šablony).</span><span class="sxs-lookup"><span data-stu-id="46a9a-181">Used in JSON templates (for example, for iOS and Android templates).</span></span><br><br><span data-ttu-id="46a9a-182">Tato funkce funguje stejně jako $(prop) dříve zadán, s výjimkou při používány šablony JSON (například Apple šablony).</span><span class="sxs-lookup"><span data-stu-id="46a9a-182">This function works exactly the same as $(prop) previously specified, except when used in JSON templates (for example, Apple templates).</span></span> <span data-ttu-id="46a9a-183">V tomto případě, pokud není tato funkce obklopená "{','}" (například myJsonProperty: '#(název).), a se vyhodnocuje na číslo ve formátu Javascript, například regexp: (0 &#124; (&#91; 1-9 &#93; &#91; 0-9 & #93 ;*))(\.&#91; 0-9 &#93; +)? () (e &#124; E) (+ &#124;-)? &#91; 0-9 &#93; +)?, pak výstup JSON je číslo.</span><span class="sxs-lookup"><span data-stu-id="46a9a-183">In this case, if this function is not surrounded by “{‘,’}” (for example, ‘myJsonProperty’ : ‘#(name)’), and it evaluates to a number in Javascript format, for example, regexp: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*))(\.&#91;0-9&#93;+)?((e&#124;E)(+&#124;-)?&#91;0-9&#93;+)?, then the output JSON is a number.</span></span><br><br><span data-ttu-id="46a9a-184">Například "oznámení" BADGE ":"#(název), se změní na "oznámení": 40 (a ne 40).</span><span class="sxs-lookup"><span data-stu-id="46a9a-184">For example, ‘badge : ‘#(name)’ becomes ‘badge’ : 40 (and not ‘40‘).</span></span> |
| <span data-ttu-id="46a9a-185">'text' nebo "text"</span><span class="sxs-lookup"><span data-stu-id="46a9a-185">‘text’ or “text”</span></span> |<span data-ttu-id="46a9a-186">Literál.</span><span class="sxs-lookup"><span data-stu-id="46a9a-186">A literal.</span></span> <span data-ttu-id="46a9a-187">Literály obsahovat libovolný text v jednoduchých nebo dvojitých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="46a9a-187">Literals contain arbitrary text enclosed in single or double quotes.</span></span> |
| <span data-ttu-id="46a9a-188">Výraz1 + Výraz2</span><span class="sxs-lookup"><span data-stu-id="46a9a-188">expr1 + expr2</span></span> |<span data-ttu-id="46a9a-189">Operátor řetězení propojení dvou výrazů do jednoho řetězce.</span><span class="sxs-lookup"><span data-stu-id="46a9a-189">The concatenation operator joining two expressions into a single string.</span></span> |

<span data-ttu-id="46a9a-190">Výraz může být libovolná z předchozí formuláře.</span><span class="sxs-lookup"><span data-stu-id="46a9a-190">The expressions can be any of the preceding forms.</span></span>

<span data-ttu-id="46a9a-191">Pokud používáte zřetězení, musí být uzavřena celý výraz s {}.</span><span class="sxs-lookup"><span data-stu-id="46a9a-191">When using concatenation, the entire expression must be surrounded with {}.</span></span> <span data-ttu-id="46a9a-192">Například {$(prop) + '-' + $(prop2)}.</span><span class="sxs-lookup"><span data-stu-id="46a9a-192">For example, {$(prop) + ‘ - ’ + $(prop2)}.</span></span> |

<span data-ttu-id="46a9a-193">Například následující není platné šablony XML:</span><span class="sxs-lookup"><span data-stu-id="46a9a-193">For example, the following is not a valid XML template:</span></span>

    <tile>
      <visual>
        <binding $(property)>
          <text id="1">Seattle, WA</text>
        </binding>  
      </visual>
    </tile>


<span data-ttu-id="46a9a-194">Jak je vysvětleno výše při použití zřetězení, výrazy musí být uzavřen do složených závorek.</span><span class="sxs-lookup"><span data-stu-id="46a9a-194">As explained above, when using concatenation, expressions must be wrapped in curly brackets.</span></span> <span data-ttu-id="46a9a-195">Například:</span><span class="sxs-lookup"><span data-stu-id="46a9a-195">For example:</span></span>

    <tile>
      <visual>
        <binding template="ToastText01">
          <text id="1">{'Hi, ' + $(name)}</text>
        </binding>  
      </visual>
    </tile>

