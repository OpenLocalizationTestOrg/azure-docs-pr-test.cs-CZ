---
title: "aaaHow tooUse Twilio pro hlasový a serveru SMS (PHP) | Microsoft Docs"
description: "Zjistěte, jak toomake telefonní hovor a odesílání SMS zprávy službou hello Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořeny v jazyce PHP."
documentationcenter: php
services: 
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 007f22e3-ac75-4868-8315-da000c2e0dd0
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 9354df8de694826a0ff7ea92620ec4d7e5c2fd70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-php"></a><span data-ttu-id="0851b-104">Jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce PHP</span><span class="sxs-lookup"><span data-stu-id="0851b-104">How tooUse Twilio for Voice and SMS Capabilities in PHP</span></span>
<span data-ttu-id="0851b-105">Tato příručka ukazuje, jak služba tooperform běžné úlohy programování s hello Twilio rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="0851b-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="0851b-106">pokryté scénáře Hello zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS).</span><span class="sxs-lookup"><span data-stu-id="0851b-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="0851b-107">Další informace o Twilio a použití hlasu a SMS ve svých aplikacích najdete v tématu hello [další kroky](#NextSteps) části.</span><span class="sxs-lookup"><span data-stu-id="0851b-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="0851b-108"><a id="WhatIs"></a>Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="0851b-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="0851b-109">Twilio je pohánějící budoucí hello firmy komunikace, povolení vývojáři tooembed hlasu VoIP a zasílání zpráv do aplikace.</span><span class="sxs-lookup"><span data-stu-id="0851b-109">Twilio is powering hello future of business communications, enabling developers tooembed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="0851b-110">Jejich Virtualizovat všechny infrastrukturu potřebnou v prostředí cloudu, globální vystavení prostřednictvím platformy hello Twilio komunikace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0851b-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through hello Twilio communications API platform.</span></span> <span data-ttu-id="0851b-111">Aplikace jsou jednoduché toobuild a škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="0851b-111">Applications are simple toobuild and scalable.</span></span> <span data-ttu-id="0851b-112">Získejte flexibilitu s platím jako vámi přejděte ceny a těžit z cloudu spolehlivost.</span><span class="sxs-lookup"><span data-stu-id="0851b-112">Enjoy flexibility with pay-as-you go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="0851b-113">**Hlasové Twilio** umožňuje toomake vaší aplikace a příjem telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="0851b-113">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="0851b-114">**Twilio SMS** umožňuje toosend vaší aplikace a přijímat textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="0851b-114">**Twilio SMS** enables your application toosend and receive text messages.</span></span> <span data-ttu-id="0851b-115">**Klient Twilio** vám umožní toomake VoIP volání z jakékoli telefon, tablet nebo prohlížeče a podporuje WebRTC.</span><span class="sxs-lookup"><span data-stu-id="0851b-115">**Twilio Client** allows you toomake VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="0851b-116"><a id="Pricing"></a>Ceny Twilio a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="0851b-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="0851b-117">Přijímat Azure zákazníků [speciální nabídka](http://www.twilio.com/azure): bezplatnou 10 Twilio kreditu při upgradu vašeho účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="0851b-117">Azure customers receive a [special offer](http://www.twilio.com/azure): complimentary $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="0851b-118">Tato Twilio platební můžou být použité tooany Twilio využití (10 platební ekvivalentní toosending až 1 000 zpráv serveru SMS nebo příjem až too1000 příchozí hlasové minut v závislosti na umístění hello vaše telefonní číslo a zpráva nebo volání cíle).</span><span class="sxs-lookup"><span data-stu-id="0851b-118">This Twilio Credit can be applied tooany Twilio usage ($10 credit equivalent toosending as many as 1,000 SMS messages or receiving up too1000 inbound Voice minutes, depending on hello location of your phone number and message or call destination).</span></span> <span data-ttu-id="0851b-119">Uplatnit tento platební Twilio a začít v: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span><span class="sxs-lookup"><span data-stu-id="0851b-119">Redeem this Twilio credit and get started at: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span></span>

<span data-ttu-id="0851b-120">Twilio je služba, průběžnými platbami.</span><span class="sxs-lookup"><span data-stu-id="0851b-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="0851b-121">Neexistují žádné poplatky nastavení a kdykoli můžete zavřít svůj účet.</span><span class="sxs-lookup"><span data-stu-id="0851b-121">There are no set-up fees and you can close your account at any time.</span></span> <span data-ttu-id="0851b-122">Můžete najít další podrobnosti o [Twilio ceny][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="0851b-122">You can find more details at [Twilio Pricing][twilio_pricing].</span></span>

## <span data-ttu-id="0851b-123"><a id="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="0851b-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="0851b-124">Hello Twilio rozhraní API je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="0851b-124">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="0851b-125">Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="0851b-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="0851b-126">Klíčové aspekty hello Twilio API jsou příkazy Twilio a Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="0851b-126">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="0851b-127"><a id="Verbs"></a>Příkazy Twilio</span><span class="sxs-lookup"><span data-stu-id="0851b-127"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="0851b-128">Hello API využívá Twilio příkazy; například hello  **&lt;indikované&gt;**  příkaz dá pokyn Twilio tooaudibly doručit zprávu na volání.</span><span class="sxs-lookup"><span data-stu-id="0851b-128">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="0851b-129">Hello následuje seznam operací Twilio.</span><span class="sxs-lookup"><span data-stu-id="0851b-129">hello following is a list of Twilio verbs.</span></span> <span data-ttu-id="0851b-130">Další informace o hello jiné příkazy a možnosti prostřednictvím [Twilio Markup Language dokumentaci](http://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="0851b-130">Learn about hello other verbs and capabilities via [Twilio Markup Language documentation](http://www.twilio.com/docs/api/twiml).</span></span>

* <span data-ttu-id="0851b-131">**&lt;Vytočit&gt;**: připojí hello volající tooanother phone.</span><span class="sxs-lookup"><span data-stu-id="0851b-131">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="0851b-132">**&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu hello.</span><span class="sxs-lookup"><span data-stu-id="0851b-132">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="0851b-133">**&lt;Zavěšení&gt;**: ukončí volání.</span><span class="sxs-lookup"><span data-stu-id="0851b-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="0851b-134">**&lt;Přehrání&gt;**: přehraje zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="0851b-134">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="0851b-135">**&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.</span><span class="sxs-lookup"><span data-stu-id="0851b-135">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="0851b-136">**&lt;Záznam&gt;**: zaznamenává hlasové hello volajícího a vrátí adresu URL souboru, který obsahuje záznam hello.</span><span class="sxs-lookup"><span data-stu-id="0851b-136">**&lt;Record&gt;**: Records hello caller's voice and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="0851b-137">**&lt;Přesměrování&gt;**: přenáší kontrolu nad telefonického hovoru nebo SMS toohello TwiML na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="0851b-137">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="0851b-138">**&lt;Odmítnout&gt;**: odmítne na příchozí volání číslo Twilio tooyour bez fakturace můžete</span><span class="sxs-lookup"><span data-stu-id="0851b-138">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you</span></span>
* <span data-ttu-id="0851b-139">**&lt;Řekněme&gt;**: toospeech převede text, že z volání.</span><span class="sxs-lookup"><span data-stu-id="0851b-139">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="0851b-140">**&lt;SMS&gt;**: odešle zprávu SMS.</span><span class="sxs-lookup"><span data-stu-id="0851b-140">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="0851b-141"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="0851b-141"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="0851b-142">TwiML je sada založený na jazyce XML pokyny podle hello Twilio příkazy, které informovat o tom, jak Twilio tooprocess hovoru nebo SMS.</span><span class="sxs-lookup"><span data-stu-id="0851b-142">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="0851b-143">Jako příklad by hello následující TwiML převést hello text **Hello, World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="0851b-143">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

<span data-ttu-id="0851b-144">Pokud vaše aplikace volání hello Twilio rozhraní API, jeden z parametrů hello rozhraní API je hello adresu URL, který vrátí odpověď TwiML hello.</span><span class="sxs-lookup"><span data-stu-id="0851b-144">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="0851b-145">Pro účely vývoje můžete použít adresy URL zadané Twilio tooprovide hello TwiML odpovědí použít v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="0851b-145">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="0851b-146">Může taky hostovat vlastní adresy URL tooproduce hello TwiML odpovědí, a další možností je toouse hello **TwiMLResponse** objektu.</span><span class="sxs-lookup"><span data-stu-id="0851b-146">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello **TwiMLResponse** object.</span></span>

<span data-ttu-id="0851b-147">Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="0851b-147">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="0851b-148">Další informace o hello Twilio API najdete v tématu [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="0851b-148">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="0851b-149"><a id="CreateAccount"></a>Vytvoření účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="0851b-149"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="0851b-150">Když jste připravené tooget Twilio účet, zaregistrovat na [zkuste Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="0851b-150">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="0851b-151">Můžete začít s bezplatný účet a upgradujte si účet později.</span><span class="sxs-lookup"><span data-stu-id="0851b-151">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="0851b-152">Když se zaregistrujete k účtu Twilio, obdržíte ID účtu a ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="0851b-152">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="0851b-153">Jak bude volání rozhraní API Twilio potřebné toomake.</span><span class="sxs-lookup"><span data-stu-id="0851b-153">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="0851b-154">tooprevent neoprávněný přístup k účtu tooyour, ověřovací token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0851b-154">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="0851b-155">ID účtu a ověřování tokenu je možné zobrazit v hello [stránku účtu Twilio][twilio_account]v hello pole s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0851b-155">Your account ID and authentication token are viewable at hello [Twilio account page][twilio_account], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="0851b-156"><a id="create_app"></a>Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="0851b-156"><a id="create_app"></a>Create a PHP Application</span></span>
<span data-ttu-id="0851b-157">Aplikace PHP, která používá službu hello Twilio a běží v Azure je nejsou jiné než jiná aplikace PHP, která používá službu Twilio hello.</span><span class="sxs-lookup"><span data-stu-id="0851b-157">A PHP application that uses hello Twilio service and is running in Azure is no different than any other PHP application that uses hello Twilio service.</span></span> <span data-ttu-id="0851b-158">Při Twilio services jsou založené na REST a lze volat z PHP několika způsoby, v tomto článku se soustředí na jak toouse Twilio služeb s [Twilio knihovny pro jazyk PHP z Githubu][twilio_php].</span><span class="sxs-lookup"><span data-stu-id="0851b-158">While Twilio services are REST-based and can be called from PHP in several ways, this article will focus on how toouse Twilio services with [Twilio library for PHP from GitHub][twilio_php].</span></span> <span data-ttu-id="0851b-159">Další informace o používání knihovny hello Twilio pro jazyk PHP najdete v tématu [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span><span class="sxs-lookup"><span data-stu-id="0851b-159">For more information about using hello Twilio library for PHP, see [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span></span>

<span data-ttu-id="0851b-160">Podrobné pokyny pro vytváření a nasazování tooAzure Twilio nebo PHP aplikace jsou k dispozici v [jak tooMake Twilio pomocí telefonního hovoru v aplikaci PHP v Azure][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="0851b-160">Detailed instructions for building and deploying a Twilio/PHP application tooAzure are available at [How tooMake a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="0851b-161"><a id="configure_app"></a>Konfigurace knihovny Twilio tooUse vaše aplikace</span><span class="sxs-lookup"><span data-stu-id="0851b-161"><a id="configure_app"></a>Configure Your Application tooUse Twilio Libraries</span></span>
<span data-ttu-id="0851b-162">Vaše aplikace toouse hello Twilio knihovna pro jazyk PHP můžete nakonfigurovat dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="0851b-162">You can configure your application toouse hello Twilio library for PHP in two ways:</span></span>

1. <span data-ttu-id="0851b-163">Stáhnout hello Twilio knihovny pro jazyk PHP z webu GitHub ([https://github.com/twilio/twilio-php][twilio_php]) a přidejte hello **služby** directory tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="0851b-163">Download hello Twilio library for PHP from GitHub ([https://github.com/twilio/twilio-php][twilio_php]) and add hello **Services** directory tooyour application.</span></span>
   
    <span data-ttu-id="0851b-164">- nebo -</span><span class="sxs-lookup"><span data-stu-id="0851b-164">-OR-</span></span>
2. <span data-ttu-id="0851b-165">Nainstalujte hello Twilio knihovny pro jazyk PHP jako balíček HRUŠKAMI.</span><span class="sxs-lookup"><span data-stu-id="0851b-165">Install hello Twilio library for PHP as a PEAR package.</span></span> <span data-ttu-id="0851b-166">Dají se instalovat s hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0851b-166">It can be installed with hello following commands:</span></span>
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

<span data-ttu-id="0851b-167">Po instalaci hello Twilio knihovny pro jazyk PHP, poté můžete přidat **require_once** příkaz v horní části hello vaší PHP soubory tooreference hello knihovně:</span><span class="sxs-lookup"><span data-stu-id="0851b-167">Once you have installed hello Twilio library for PHP, you can then add a **require_once** statement at hello top of your PHP files tooreference hello library:</span></span>

        require_once 'Services/Twilio.php';

<span data-ttu-id="0851b-168">Další informace najdete v tématu [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span><span class="sxs-lookup"><span data-stu-id="0851b-168">For more information, see [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span></span>

## <span data-ttu-id="0851b-169"><a id="howto_make_call"></a>Postupy: volání odchozí</span><span class="sxs-lookup"><span data-stu-id="0851b-169"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="0851b-170">Hello následující ukazuje, jak toomake odchozí volat pomocí hello **Services_Twilio** třídy.</span><span class="sxs-lookup"><span data-stu-id="0851b-170">hello following shows how toomake an outgoing call using hello **Services_Twilio** class.</span></span> <span data-ttu-id="0851b-171">Tento kód také používá zadaný Twilio lokality tooreturn hello odpovědi Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="0851b-171">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="0851b-172">Dosaďte svoje hodnoty hello **z** a **k** telefonních čísel a ujistěte se, abyste ověřili hello **z** telefonní číslo kódu hello předchozí toorunning účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="0851b-172">Substitute your values for hello **From** and **To** phone numbers, and ensure that you verify hello **From** phone number for your Twilio account prior toorunning hello code.</span></span>

    // Include hello Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // hello number of hello phone initiating hello hello call.
    $from_number = "NNNNNNNNNNN";

    // hello number of hello phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use hello Twilio-provided site for hello TwiML response.
    $url = "http://twimlets.com/message";

    // hello phone message text.
    $message = "Hello world.";

    // Create hello call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make hello call.
    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

<span data-ttu-id="0851b-173">Jak je uvedeno, tento kód používá zadaný Twilio lokality tooreturn hello TwiML odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0851b-173">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="0851b-174">Místo toho můžete použít vlastní lokality tooprovide hello TwiML odpovědi; Další informace najdete v tématu [jak tooProvide TwiML odpovědí z vaše vlastní webové stránky](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="0851b-174">You could instead use your own site tooprovide hello TwiML response; for more information, see [How tooProvide TwiML Responses from Your Own Web Site](#howto_provide_twiml_responses).</span></span>

* <span data-ttu-id="0851b-175">**Poznámka:**: tootroubleshoot chyby ověření certifikátu SSL, najdete v části [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span><span class="sxs-lookup"><span data-stu-id="0851b-175">**Note**: tootroubleshoot SSL certificate validation errors, see [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span></span> 

## <span data-ttu-id="0851b-176"><a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS</span><span class="sxs-lookup"><span data-stu-id="0851b-176"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="0851b-177">Hello následující ukazuje, jak toosend zprávu SMS pomocí hello **Services_Twilio** třídy.</span><span class="sxs-lookup"><span data-stu-id="0851b-177">hello following shows how toosend an SMS message using hello **Services_Twilio** class.</span></span> <span data-ttu-id="0851b-178">Hello **z** číslo poskytne Twilio pro zkušební verzi účty toosend zpráv serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="0851b-178">hello **From** number is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="0851b-179">Hello **k** číslo musí ověřit kódu hello předchozí toorunning účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="0851b-179">hello **To** number must be verified for your Twilio account prior toorunning hello code.</span></span>

    // Include hello Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create hello call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send hello SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <span data-ttu-id="0851b-180"><a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu</span><span class="sxs-lookup"><span data-stu-id="0851b-180"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="0851b-181">Pokud vaše aplikace zahájí toohello volání rozhraní API Twilio, budou odeslány Twilio tooa adresu URL vašeho požadavku, která je očekáván tooreturn TwiML odpověď.</span><span class="sxs-lookup"><span data-stu-id="0851b-181">When your application initiates a call toohello Twilio API, Twilio will send your request tooa URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="0851b-182">výše uvedený příklad Hello používá URL poskytnutou Twilio hello [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="0851b-182">hello example above uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="0851b-183">(Při TwiML je určen pro Twilio, můžete zobrazit hello v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="0851b-183">(While TwiML is designed for use by Twilio, you can view hello it in your browser.</span></span> <span data-ttu-id="0851b-184">Klikněte například na [http://twimlets.com/message] [ twimlet_message_url] toosee prázdnou `<Response>` element; například klikněte na tlačítko [http://twimlets.com/message? Zprávy % 5B0 %5 D = Hello % 20World] [ twimlet_message_url_hello_world] toosee `<Response>` elementu, který obsahuje `<Say>` elementu.)</span><span class="sxs-lookup"><span data-stu-id="0851b-184">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty `<Response>` element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] toosee a `<Response>` element that contains a `<Say>` element.)</span></span>

<span data-ttu-id="0851b-185">Aniž byste museli spoléhat na URL poskytnutou Twilio hello, můžete vytvořit vlastního webu, který vrací odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="0851b-185">Instead of relying on hello Twilio-provided URL, you can create your own site that returns HTTP responses.</span></span> <span data-ttu-id="0851b-186">Můžete vytvořit hello lokality v libovolném jazyce, který vrací odpovědí XML; Toto téma předpokládá, že budete používat PHP toocreate hello TwiML.</span><span class="sxs-lookup"><span data-stu-id="0851b-186">You can create hello site in any language that returns XML responses; this topic assumes you'll be using PHP toocreate hello TwiML.</span></span>

<span data-ttu-id="0851b-187">Hello následující stránky PHP za následek TwiML odpovědi, která uvádí, že **Hello, World** při volání hello.</span><span class="sxs-lookup"><span data-stu-id="0851b-187">hello following PHP page results in a TwiML response that says **Hello World** on hello call.</span></span>

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

<span data-ttu-id="0851b-188">Jak je vidět z výše uvedeném příkladu hello hello TwiML odpovědi je jednoduše dokument XML.</span><span class="sxs-lookup"><span data-stu-id="0851b-188">As you can see from hello example above, hello TwiML response is simply an XML document.</span></span> <span data-ttu-id="0851b-189">Knihovna Hello Twilio pro jazyk PHP obsahuje třídy, které způsobí vygenerování TwiML za vás.</span><span class="sxs-lookup"><span data-stu-id="0851b-189">hello Twilio library for PHP contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="0851b-190">Následující příklad Hello vytvoří ekvivalentní odpovědi hello jako v příkladu nahoře, ale používá hello **služby\_Twilio\_Twiml** – třída v hello knihovně Twilio pro PHP:</span><span class="sxs-lookup"><span data-stu-id="0851b-190">hello example below produces hello equivalent response as shown above, but uses hello **Services\_Twilio\_Twiml** class in hello Twilio library for PHP:</span></span>

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

<span data-ttu-id="0851b-191">Další informace o TwiML najdete v tématu [https://www.twilio.com/docs/api/twiml][twiml_reference].</span><span class="sxs-lookup"><span data-stu-id="0851b-191">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml][twiml_reference].</span></span> 

<span data-ttu-id="0851b-192">Jakmile máte stránku PHP, nastavení tooprovide TwiML odpovědi, použijte hello URL stránky PHP hello jako adresa URL předaná do hello hello `Services_Twilio->account->calls->create` metoda.</span><span class="sxs-lookup"><span data-stu-id="0851b-192">Once you have your PHP page set up tooprovide TwiML responses, use hello URL of hello PHP page as hello URL passed into hello  `Services_Twilio->account->calls->create`  method.</span></span> <span data-ttu-id="0851b-193">Například, pokud máte webovou aplikaci s názvem **MyTwiML** nasazené tooan Azure hostovaná služba a je hello název stránky PHP hello **mytwiml.php**, adresa URL může být předán příliš hello **Services_ Twilio -> účet -> volání -> vytvořit** jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="0851b-193">For example, if you have a Web application named **MyTwiML** deployed tooan Azure hosted service, and hello name of hello PHP page is **mytwiml.php**, hello URL can be passed too **Services_Twilio->account->calls->create**  as shown in hello following example:</span></span>

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // hello phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

<span data-ttu-id="0851b-194">Další informace o používání Twilio v Azure s PHP najdete v tématu [jak tooMake Twilio pomocí telefonního hovoru v aplikaci PHP v Azure][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="0851b-194">For additional information about using Twilio in Azure with PHP, see [How tooMake a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="0851b-195"><a id="AdditionalServices"></a>Postupy: použití další Twilio služeb</span><span class="sxs-lookup"><span data-stu-id="0851b-195"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="0851b-196">Kromě toho toohello příklady zobrazeny zde, že twilio nabízí webové rozhraní API, které můžete použít další funkce Twilio tooleverage z vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="0851b-196">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="0851b-197">Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API Twilio][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="0851b-197">For full details, see hello [Twilio API documentation][twilio_api_documentation].</span></span>

## <span data-ttu-id="0851b-198"><a id="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="0851b-198"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="0851b-199">Teď, když jste se naučili základy hello hello Twilio služby, použijte tyto odkazy toolearn Další:</span><span class="sxs-lookup"><span data-stu-id="0851b-199">Now that you've learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="0851b-200">[Pokyny pro zabezpečení Twilio][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="0851b-200">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="0851b-201">[Twilio postupy a příklady kódu][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="0851b-201">[Twilio HowTo's and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="0851b-202">[Kurzy Twilio rychlý start][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="0851b-202">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span> 
* <span data-ttu-id="0851b-203">[Twilio na Githubu][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="0851b-203">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="0851b-204">[Komunikovat tooTwilio podpory][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="0851b-204">[Talk tooTwilio Support][twilio_support]</span></span>

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_api_service]: https://api.twilio.com
[howto_phonecall_php]: partner-twilio-php-make-phone-call.md
[twilio_voice_request]: https://www.twilio.com/docs/api/twiml/twilio_request
[twilio_sms_request]: https://www.twilio.com/docs/api/twiml/sms/twilio_request
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
