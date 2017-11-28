---
title: aaaTemplates
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
ms.openlocfilehash: 0149f0c7473e5a4b952905bc8217582b58db2a0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="templates"></a><span data-ttu-id="e14a9-103">Šablony</span><span class="sxs-lookup"><span data-stu-id="e14a9-103">Templates</span></span>
## <a name="overview"></a><span data-ttu-id="e14a9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e14a9-104">Overview</span></span>
<span data-ttu-id="e14a9-105">Šablony povolit klienta aplikace toospecify hello přesný formátu hello oznámení, že ho chce tooreceive.</span><span class="sxs-lookup"><span data-stu-id="e14a9-105">Templates enable a client application toospecify hello exact format of hello notifications it wants tooreceive.</span></span> <span data-ttu-id="e14a9-106">Pomocí šablon, aplikace můžou realizovat několik různých výhod, jako třeba hello následující:</span><span class="sxs-lookup"><span data-stu-id="e14a9-106">Using templates, an app can realize several different benefits, including hello following :</span></span>

* <span data-ttu-id="e14a9-107">Back-end bez ohledu na platformy</span><span class="sxs-lookup"><span data-stu-id="e14a9-107">A platform-agnostic backend</span></span>
* <span data-ttu-id="e14a9-108">Přizpůsobené oznámení</span><span class="sxs-lookup"><span data-stu-id="e14a9-108">Personalized notifications</span></span>
* <span data-ttu-id="e14a9-109">Nezávislost verze klienta</span><span class="sxs-lookup"><span data-stu-id="e14a9-109">Client-version independence</span></span>
* <span data-ttu-id="e14a9-110">Snadno lokalizace</span><span class="sxs-lookup"><span data-stu-id="e14a9-110">Easy localization</span></span>

<span data-ttu-id="e14a9-111">Tato část obsahuje dva podrobné příklady, jak toouse šablony toosend bez ohledu na platformu oznámení cílení na všechna zařízení napříč platformami a toopersonalize vysílání oznámení tooeach zařízení.</span><span class="sxs-lookup"><span data-stu-id="e14a9-111">This section provides two in-depth examples of how toouse templates toosend platform-agnostic notifications targeting all your devices across platforms, and toopersonalize broadcast notification tooeach device.</span></span>

## <a name="using-templates-cross-platform"></a><span data-ttu-id="e14a9-112">Pomocí šablony a platformy</span><span class="sxs-lookup"><span data-stu-id="e14a9-112">Using templates cross-platform</span></span>
<span data-ttu-id="e14a9-113">Hello standardním způsobem toosend nabízených oznámení je toosend pro každý oznámení, že toobe pošle služby oznámení konkrétní datové části tooplatform, (WNS, APNS).</span><span class="sxs-lookup"><span data-stu-id="e14a9-113">hello standard way toosend push notifications is toosend, for each notification that is toobe sent, a specific payload tooplatform notification services (WNS, APNS).</span></span> <span data-ttu-id="e14a9-114">Například toosend výstrahy tooAPNS, datová část hello je objekt Json hello následující formulář:</span><span class="sxs-lookup"><span data-stu-id="e14a9-114">For example, toosend an alert tooAPNS, hello payload is a Json object of hello following form:</span></span>

    {"aps": {"alert" : "Hello!" }}

<span data-ttu-id="e14a9-115">toosend podobná informační zpráva v aplikaci pro Windows Store, hello datovou část XML je následující:</span><span class="sxs-lookup"><span data-stu-id="e14a9-115">toosend a similar toast message on a Windows Store application, hello XML payload is as follows:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">Hello!</text>
        </binding>
      </visual>
    </toast>

<span data-ttu-id="e14a9-116">Podobně jako datové části můžete vytvořit pro platformy (Android) GCM a MPNS (Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="e14a9-116">You can create similar payloads for MPNS (Windows Phone) and GCM (Android) platforms.</span></span>

<span data-ttu-id="e14a9-117">Tento požadavek vynutí hello aplikace back-end tooproduce různých datových částí pro každou platformu a efektivně je zodpovědná za součástí prezentační vrstvy hello hello aplikace hello back-end.</span><span class="sxs-lookup"><span data-stu-id="e14a9-117">This requirement forces hello app backend tooproduce different payloads for each platform, and effectively makes hello backend responsible for part of hello presentation layer of hello app.</span></span> <span data-ttu-id="e14a9-118">Některé aspekty patří lokalizace a grafické rozložení (hlavně u aplikací pro Windows Store, které obsahují oznámení pro různé typy dlaždic).</span><span class="sxs-lookup"><span data-stu-id="e14a9-118">Some concerns include localization and graphical layouts (especially for Windows Store apps that include notifications for various types of tiles).</span></span>

<span data-ttu-id="e14a9-119">funkce šablony Hello Notification Hubs umožňuje aplikace toocreate speciální registrace klienta, nazývá šablona registrace, které obsahují, kromě toohello sadu značky, šablony.</span><span class="sxs-lookup"><span data-stu-id="e14a9-119">hello Notification Hubs template feature enables a client app toocreate special registrations, called template registrations, which include, in addition toohello set of tags, a template.</span></span> <span data-ttu-id="e14a9-120">Hello Notification Hubs šablony funkce umožňuje tooassociate aplikace klientské zařízení se šablonami, zda pracujete s instalací (doporučeno) nebo registrací.</span><span class="sxs-lookup"><span data-stu-id="e14a9-120">hello Notification Hubs template feature enables a client app tooassociate devices with templates whether you are working with Installations (preferred) or Registrations.</span></span> <span data-ttu-id="e14a9-121">Zadána hello předcházející datové části Příklady hello jenom informace o nezávislé na platformě je hello skutečné výstrahy zpráva (Hello!).</span><span class="sxs-lookup"><span data-stu-id="e14a9-121">Given hello preceding payload examples, hello only platform-independent information is hello actual alert message (Hello!).</span></span> <span data-ttu-id="e14a9-122">Šablona je sada pokynů pro hello centra oznámení na tom, jak tooformat nezávislé na platformě zprávy pro registraci hello tohoto konkrétního klienta aplikace.</span><span class="sxs-lookup"><span data-stu-id="e14a9-122">A template is a set of instructions for hello Notification Hub on how tooformat a platform-independent message for hello registration of that specific client app.</span></span> <span data-ttu-id="e14a9-123">V předchozím příkladu hello, nezávislé zpráva hello platformy je jedinou vlastností: **zpráva = Hello!**.</span><span class="sxs-lookup"><span data-stu-id="e14a9-123">In hello preceding example, hello platform independent message is a single property: **message = Hello!**.</span></span>

<span data-ttu-id="e14a9-124">Hello následující obrázek znázorňuje hello výše proces:</span><span class="sxs-lookup"><span data-stu-id="e14a9-124">hello following picture illustrates hello above process:</span></span>

![](./media/notification-hubs-templates/notification-hubs-hello.png)

<span data-ttu-id="e14a9-125">Hello šablonu pro registraci aplikace klienta iOS hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="e14a9-125">hello template for hello iOS client app registration is as follows:</span></span>

    {"aps": {"alert": "$(message)"}}

<span data-ttu-id="e14a9-126">Hello odpovídající šablonu pro klientskou aplikaci pro Windows Store hello je:</span><span class="sxs-lookup"><span data-stu-id="e14a9-126">hello corresponding template for hello Windows Store client app is:</span></span>

    <toast>
        <visual>
            <binding template=\"ToastText01\">
                <text id=\"1\">$(message)</text>
            </binding>
        </visual>
    </toast>

<span data-ttu-id="e14a9-127">Všimněte si, že hello skutečné zpráva nahradí hello výraz $(zprávy).</span><span class="sxs-lookup"><span data-stu-id="e14a9-127">Notice that hello actual message is substituted for hello expression $(message).</span></span> <span data-ttu-id="e14a9-128">Tento výraz dá pokyn hello centra oznámení pokaždé, když odešle zprávu toothis konkrétní registrace, toobuild zprávu, která odpovídá jeho a přepínačů v hodnotě běžné hello.</span><span class="sxs-lookup"><span data-stu-id="e14a9-128">This expression instructs hello Notification Hub, whenever it sends a message toothis particular registration, toobuild a message that follows it and switches in hello common value.</span></span>

<span data-ttu-id="e14a9-129">Pokud pracujete s modelem instalace, obsahuje klíč "šablony" hello instalace JSON více šablon.</span><span class="sxs-lookup"><span data-stu-id="e14a9-129">If you are working with Installation model, hello installation “templates” key holds a JSON of multiple templates.</span></span> <span data-ttu-id="e14a9-130">Pokud pracujete s modelem registrace, klientská aplikace hello můžete vytvořit více registrace v pořadí toouse několik šablon; například šablonu pro oznámení a šablonu pro dlaždici aktualizací.</span><span class="sxs-lookup"><span data-stu-id="e14a9-130">If you are working with Registration model, hello client application can create multiple registrations in order toouse multiple templates; for example, a template for alert messages and a template for tile updates.</span></span> <span data-ttu-id="e14a9-131">Klientské aplikace můžete také kombinovat nativní registrace (registrace s žádnou šablonu) a šablony registrace.</span><span class="sxs-lookup"><span data-stu-id="e14a9-131">Client applications can also mix native registrations (registrations with no template) and template registrations.</span></span>

<span data-ttu-id="e14a9-132">Hello centra oznámení odešle jedno oznámení ke každé šabloně bez ohledu zda patří toohello stejné klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="e14a9-132">hello Notification Hub sends one notification for each template without considering whether they belong toohello same client app.</span></span> <span data-ttu-id="e14a9-133">Toto chování může být použité tootranslate nezávislé na platformě oznámení do další oznámení.</span><span class="sxs-lookup"><span data-stu-id="e14a9-133">This behavior can be used tootranslate platform-independent notifications into more notifications.</span></span> <span data-ttu-id="e14a9-134">Například hello stejné toohello nezávislé zpráva platformy, které Centrum oznámení můžete bezproblémově přeložit v informační výstrahy a aktualizace dlaždice, bez nutnosti hello back-end toobe kroky.</span><span class="sxs-lookup"><span data-stu-id="e14a9-134">For example, hello same platform independent message toohello Notification Hub can be seamlessly translated in a toast alert and a tile update, without requiring hello backend toobe aware of it.</span></span> <span data-ttu-id="e14a9-135">Všimněte si, že některé platformy (například iOS) může sbalit více toohello oznámení stejného zařízení, pokud jsou odesílány v krátké době.</span><span class="sxs-lookup"><span data-stu-id="e14a9-135">Note that some platforms (for example, iOS) might collapse multiple notifications toohello same device if they are sent in a short period of time.</span></span>

## <a name="using-templates-for-personalization"></a><span data-ttu-id="e14a9-136">Pomocí šablon pro přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="e14a9-136">Using templates for personalization</span></span>
<span data-ttu-id="e14a9-137">Další výhody toousing šablony je hello možnost toouse Notification Hubs tooperform za registraci přizpůsobení oznámení.</span><span class="sxs-lookup"><span data-stu-id="e14a9-137">Another advantage toousing templates is hello ability toouse Notification Hubs tooperform per-registration personalization of notifications.</span></span> <span data-ttu-id="e14a9-138">Představte si třeba počasí aplikaci, která zobrazuje dlaždici hello počasí na určité místo.</span><span class="sxs-lookup"><span data-stu-id="e14a9-138">For example, consider a weather app that displays a tile with hello weather conditions at a specific location.</span></span> <span data-ttu-id="e14a9-139">Uživatel může zvolit c nebo Fahrenheita stupňů a jednoho nebo pětidenního prognózy.</span><span class="sxs-lookup"><span data-stu-id="e14a9-139">A user can choose between Celsius or Fahrenheit degrees, and a single or five-day forecast.</span></span> <span data-ttu-id="e14a9-140">Pomocí šablon, každou instalaci klienta aplikace si můžou zaregistrovat hello formátu potřebném (1 den Celsius, 1 den Fahrenheita, 5 dní c, 5 dní Fahrenheita kurzy), a odeslat zprávu, která obsahuje všechny informace o hello požadované toofill ty back-end hello šablony (například pět dní prognózy s c a Fahrenheita stupních).</span><span class="sxs-lookup"><span data-stu-id="e14a9-140">Using templates, each client app installation can register for hello format required (1-day Celsius, 1-day Fahrenheit, 5-days Celsius, 5-days Fahrenheit), and have hello backend send a single message that contains all hello information required toofill those templates (for example, a five-day forecast with Celsius and Fahrenheit degrees).</span></span>

<span data-ttu-id="e14a9-141">Hello šablony pro hello jeden den prognózy s c teploty vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="e14a9-141">hello template for hello one-day forecast with Celsius temperatures is as follows:</span></span>

    <tile>
      <visual>
        <binding template="TileWideSmallImageAndText04">
          <image id="1" src="$(day1_image)" alt="alt text"/>
          <text id="1">Seattle, WA</text>
          <text id="2">$(day1_tempC)</text>
        </binding>  
      </visual>
    </tile>

<span data-ttu-id="e14a9-142">zpráva byla odeslána toohello Hello centra oznámení obsahuje všechny hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="e14a9-142">hello message sent toohello Notification Hub contains all hello following properties:</span></span>

<table border="1">

<tr><td><span data-ttu-id="e14a9-143">day1_image</span><span class="sxs-lookup"><span data-stu-id="e14a9-143">day1_image</span></span></td><td><span data-ttu-id="e14a9-144">day2_image</span><span class="sxs-lookup"><span data-stu-id="e14a9-144">day2_image</span></span></td><td><span data-ttu-id="e14a9-145">day3_image</span><span class="sxs-lookup"><span data-stu-id="e14a9-145">day3_image</span></span></td><td><span data-ttu-id="e14a9-146">day4_image</span><span class="sxs-lookup"><span data-stu-id="e14a9-146">day4_image</span></span></td><td><span data-ttu-id="e14a9-147">day5_image</span><span class="sxs-lookup"><span data-stu-id="e14a9-147">day5_image</span></span></td></tr>

<tr><td><span data-ttu-id="e14a9-148">day1_tempC</span><span class="sxs-lookup"><span data-stu-id="e14a9-148">day1_tempC</span></span></td><td><span data-ttu-id="e14a9-149">day2_tempC</span><span class="sxs-lookup"><span data-stu-id="e14a9-149">day2_tempC</span></span></td><td><span data-ttu-id="e14a9-150">day3_tempC</span><span class="sxs-lookup"><span data-stu-id="e14a9-150">day3_tempC</span></span></td><td><span data-ttu-id="e14a9-151">day4_tempC</span><span class="sxs-lookup"><span data-stu-id="e14a9-151">day4_tempC</span></span></td><td><span data-ttu-id="e14a9-152">day5_tempC</span><span class="sxs-lookup"><span data-stu-id="e14a9-152">day5_tempC</span></span></td></tr>

<tr><td><span data-ttu-id="e14a9-153">day1_tempF</span><span class="sxs-lookup"><span data-stu-id="e14a9-153">day1_tempF</span></span></td><td><span data-ttu-id="e14a9-154">day2_tempF</span><span class="sxs-lookup"><span data-stu-id="e14a9-154">day2_tempF</span></span></td><td><span data-ttu-id="e14a9-155">day3_tempF</span><span class="sxs-lookup"><span data-stu-id="e14a9-155">day3_tempF</span></span></td><td><span data-ttu-id="e14a9-156">day4_tempF</span><span class="sxs-lookup"><span data-stu-id="e14a9-156">day4_tempF</span></span></td><td><span data-ttu-id="e14a9-157">day5_tempF</span><span class="sxs-lookup"><span data-stu-id="e14a9-157">day5_tempF</span></span></td></tr>
</table><br/>

<span data-ttu-id="e14a9-158">Pomocí tohoto vzoru back-end hello pouze odešle do jedné zprávy bez nutnosti toostore přizpůsobení konkrétní možnosti pro uživatele aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e14a9-158">By using this pattern, hello backend only sends a single message without having toostore specific personalization options for hello app users.</span></span> <span data-ttu-id="e14a9-159">Hello následující obrázek znázorňuje tento scénář:</span><span class="sxs-lookup"><span data-stu-id="e14a9-159">hello following picture illustrates this scenario:</span></span>

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-tooregister-templates"></a><span data-ttu-id="e14a9-160">Jak tooregister šablony</span><span class="sxs-lookup"><span data-stu-id="e14a9-160">How tooregister templates</span></span>
<span data-ttu-id="e14a9-161">tooregister s šablony pomocí modelu instalace hello (doporučeno) nebo hello modelu registrace najdete v části [registrace správu](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="e14a9-161">tooregister with templates using hello Installation model (preferred), or hello Registration model, see [Registration Management](notification-hubs-push-notification-registration-management.md).</span></span>

## <a name="template-expression-language"></a><span data-ttu-id="e14a9-162">Výraz jazyka šablony</span><span class="sxs-lookup"><span data-stu-id="e14a9-162">Template expression language</span></span>
<span data-ttu-id="e14a9-163">Šablony jsou omezené tooXML nebo formáty dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="e14a9-163">Templates are limited tooXML or JSON document formats.</span></span> <span data-ttu-id="e14a9-164">Navíc můžete pouze umístit výrazy konkrétní místech; například atributy uzlu nebo hodnoty pro formát XML řetězec hodnoty vlastností pro formát JSON.</span><span class="sxs-lookup"><span data-stu-id="e14a9-164">Also, you can only place expressions in particular places; for example, node attributes or values for XML, string property values for JSON.</span></span>

<span data-ttu-id="e14a9-165">Hello následující tabulka uvádí hello jazyk povoleny v rámci šablon:</span><span class="sxs-lookup"><span data-stu-id="e14a9-165">hello following table shows hello language allowed in templates:</span></span>

| <span data-ttu-id="e14a9-166">výraz</span><span class="sxs-lookup"><span data-stu-id="e14a9-166">Expression</span></span> | <span data-ttu-id="e14a9-167">Popis</span><span class="sxs-lookup"><span data-stu-id="e14a9-167">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e14a9-168">$(prop)</span><span class="sxs-lookup"><span data-stu-id="e14a9-168">$(prop)</span></span> |<span data-ttu-id="e14a9-169">Vlastnost referenčního tooan události se zadaným názvem hello.</span><span class="sxs-lookup"><span data-stu-id="e14a9-169">Reference tooan event property with hello given name.</span></span> <span data-ttu-id="e14a9-170">Názvy vlastností nerozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="e14a9-170">Property names are not case-sensitive.</span></span> <span data-ttu-id="e14a9-171">Tento výraz přeloží do hello vlastnost textové hodnoty nebo na prázdný řetězec, pokud vlastnost hello není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e14a9-171">This expression resolves into hello property’s text value or into an empty string if hello property is not present.</span></span> |
| <span data-ttu-id="e14a9-172">$(prop, n)</span><span class="sxs-lookup"><span data-stu-id="e14a9-172">$(prop, n)</span></span> |<span data-ttu-id="e14a9-173">Jak je vyšší, ale hello textu je explicitně oříznut n znaků, například $(název, 20) klipy hello obsah hello název vlastnosti v 20 znaků.</span><span class="sxs-lookup"><span data-stu-id="e14a9-173">As above, but hello text is explicitly clipped at n characters, for example $(title, 20) clips hello contents of hello title property at 20 characters.</span></span> |
| <span data-ttu-id="e14a9-174">. (prop, n)</span><span class="sxs-lookup"><span data-stu-id="e14a9-174">.(prop, n)</span></span> |<span data-ttu-id="e14a9-175">Jako vyšší, ale hello je text konci se třemi tečkami, jako je oříznut.</span><span class="sxs-lookup"><span data-stu-id="e14a9-175">As above, but hello text is suffixed with three dots as it is clipped.</span></span> <span data-ttu-id="e14a9-176">Celková velikost Hello hello oříznut řetězec a příponu hello nepřesahuje n znaků.</span><span class="sxs-lookup"><span data-stu-id="e14a9-176">hello total size of hello clipped string and hello suffix does not exceed n characters.</span></span> <span data-ttu-id="e14a9-177">. (title, 20) s vstupní vlastností "Toto je název řádku hello" výsledků v **Toto je název hello...**</span><span class="sxs-lookup"><span data-stu-id="e14a9-177">.(title, 20) with an input property of “This is hello title line” results in **This is hello title...**</span></span> |
| <span data-ttu-id="e14a9-178">%(Prop)</span><span class="sxs-lookup"><span data-stu-id="e14a9-178">%(prop)</span></span> |<span data-ttu-id="e14a9-179">Podobně jako too$(name) s tím rozdílem, že výstup hello je kódovaný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="e14a9-179">Similar too$(name) except that hello output is URI-encoded.</span></span> |
| <span data-ttu-id="e14a9-180">#(prop)</span><span class="sxs-lookup"><span data-stu-id="e14a9-180">#(prop)</span></span> |<span data-ttu-id="e14a9-181">Používá se v šablony JSON (například pro iOS a Android šablony).</span><span class="sxs-lookup"><span data-stu-id="e14a9-181">Used in JSON templates (for example, for iOS and Android templates).</span></span><br><br><span data-ttu-id="e14a9-182">Tato funkce funguje přesně hello stejné jako $(prop) dříve zadán, s výjimkou při používány šablony JSON (například Apple šablony).</span><span class="sxs-lookup"><span data-stu-id="e14a9-182">This function works exactly hello same as $(prop) previously specified, except when used in JSON templates (for example, Apple templates).</span></span> <span data-ttu-id="e14a9-183">V tomto případě, pokud není tato funkce obklopená "{','}" (například myJsonProperty: '#(název)'), a vyhodnocuje tooa číslo ve formátu Javascript, například regexp: (0 &#124; (&#91; 1-9 &#93; &#91; 0-9 & #93 ;*))(\. &#91; 0-9 &#93; +)? ((e &#124; E) (+ &#124;-)? &#91; 0-9 &#93; +)?, pak hello výstup JSON je číslo.</span><span class="sxs-lookup"><span data-stu-id="e14a9-183">In this case, if this function is not surrounded by “{‘,’}” (for example, ‘myJsonProperty’ : ‘#(name)’), and it evaluates tooa number in Javascript format, for example, regexp: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*))(\.&#91;0-9&#93;+)?((e&#124;E)(+&#124;-)?&#91;0-9&#93;+)?, then hello output JSON is a number.</span></span><br><br><span data-ttu-id="e14a9-184">Například "oznámení" BADGE ":"#(název), se změní na "oznámení": 40 (a ne 40).</span><span class="sxs-lookup"><span data-stu-id="e14a9-184">For example, ‘badge : ‘#(name)’ becomes ‘badge’ : 40 (and not ‘40‘).</span></span> |
| <span data-ttu-id="e14a9-185">'text' nebo "text"</span><span class="sxs-lookup"><span data-stu-id="e14a9-185">‘text’ or “text”</span></span> |<span data-ttu-id="e14a9-186">Literál.</span><span class="sxs-lookup"><span data-stu-id="e14a9-186">A literal.</span></span> <span data-ttu-id="e14a9-187">Literály obsahovat libovolný text v jednoduchých nebo dvojitých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="e14a9-187">Literals contain arbitrary text enclosed in single or double quotes.</span></span> |
| <span data-ttu-id="e14a9-188">Výraz1 + Výraz2</span><span class="sxs-lookup"><span data-stu-id="e14a9-188">expr1 + expr2</span></span> |<span data-ttu-id="e14a9-189">operátor řetězení Hello propojení dvou výrazů do jednoho řetězce.</span><span class="sxs-lookup"><span data-stu-id="e14a9-189">hello concatenation operator joining two expressions into a single string.</span></span> |

<span data-ttu-id="e14a9-190">výrazy Hello může být libovolná z předchozích forms hello.</span><span class="sxs-lookup"><span data-stu-id="e14a9-190">hello expressions can be any of hello preceding forms.</span></span>

<span data-ttu-id="e14a9-191">Pokud používáte zřetězení, musí být uzavřena hello celý výraz s {}.</span><span class="sxs-lookup"><span data-stu-id="e14a9-191">When using concatenation, hello entire expression must be surrounded with {}.</span></span> <span data-ttu-id="e14a9-192">Například {$(prop) + '-' + $(prop2)}.</span><span class="sxs-lookup"><span data-stu-id="e14a9-192">For example, {$(prop) + ‘ - ’ + $(prop2)}.</span></span> |

<span data-ttu-id="e14a9-193">Například následující hello není platné šablony XML:</span><span class="sxs-lookup"><span data-stu-id="e14a9-193">For example, hello following is not a valid XML template:</span></span>

    <tile>
      <visual>
        <binding $(property)>
          <text id="1">Seattle, WA</text>
        </binding>  
      </visual>
    </tile>


<span data-ttu-id="e14a9-194">Jak je vysvětleno výše při použití zřetězení, výrazy musí být uzavřen do složených závorek.</span><span class="sxs-lookup"><span data-stu-id="e14a9-194">As explained above, when using concatenation, expressions must be wrapped in curly brackets.</span></span> <span data-ttu-id="e14a9-195">Například:</span><span class="sxs-lookup"><span data-stu-id="e14a9-195">For example:</span></span>

    <tile>
      <visual>
        <binding template="ToastText01">
          <text id="1">{'Hi, ' + $(name)}</text>
        </binding>  
      </visual>
    </tile>

