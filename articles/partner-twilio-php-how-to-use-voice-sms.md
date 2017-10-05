---
title: "Jak používat Twilio pro hlasový a serveru SMS (PHP) | Microsoft Docs"
description: "Naučte se telefonní hovor a odeslání zprávy SMS službou Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořeny v jazyce PHP."
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
ms.openlocfilehash: bd50eac7390e8639f77894689388e6926cdb619c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a><span data-ttu-id="67384-104">Jak používat Twilio pro hlasový a možnosti serveru SMS v jazyce PHP</span><span class="sxs-lookup"><span data-stu-id="67384-104">How to Use Twilio for Voice and SMS Capabilities in PHP</span></span>
<span data-ttu-id="67384-105">Tato příručka ukazuje, jak provádět běžné úkoly programování službou Twilio rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="67384-105">This guide demonstrates how to perform common programming tasks with the Twilio API service on Azure.</span></span> <span data-ttu-id="67384-106">Pokryté scénáře zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS).</span><span class="sxs-lookup"><span data-stu-id="67384-106">The scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="67384-107">Další informace o Twilio a používání hlasového a SMS ve svých aplikacích najdete v tématu [další kroky](#NextSteps) části.</span><span class="sxs-lookup"><span data-stu-id="67384-107">For more information on Twilio and using voice and SMS in your applications, see the [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="67384-108"><a id="WhatIs"></a>Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="67384-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="67384-109">Twilio je pohánějící budoucí obchodní komunikaci, umožňuje vývojářům pro vložení hlasové, VoIP a zasílání zpráv do aplikace.</span><span class="sxs-lookup"><span data-stu-id="67384-109">Twilio is powering the future of business communications, enabling developers to embed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="67384-110">Jejich Virtualizovat všechny infrastrukturu potřebnou v prostředí cloudu, globální vystavení prostřednictvím platformy komunikace rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="67384-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through the Twilio communications API platform.</span></span> <span data-ttu-id="67384-111">Aplikace jsou jednoduché k sestavení a škálovatelné.</span><span class="sxs-lookup"><span data-stu-id="67384-111">Applications are simple to build and scalable.</span></span> <span data-ttu-id="67384-112">Získejte flexibilitu s platím jako vámi přejděte ceny a těžit z cloudu spolehlivost.</span><span class="sxs-lookup"><span data-stu-id="67384-112">Enjoy flexibility with pay-as-you go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="67384-113">**Hlasové Twilio** umožňuje aplikacím zkontrolujte a příjem telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="67384-113">**Twilio Voice** allows your applications to make and receive phone calls.</span></span> <span data-ttu-id="67384-114">**Twilio SMS** umožňuje aplikaci posílat a přijímat textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="67384-114">**Twilio SMS** enables your application to send and receive text messages.</span></span> <span data-ttu-id="67384-115">**Klient Twilio** umožňuje provádět volání VoIP z jakékoli telefon, tablet nebo prohlížeče a podporuje WebRTC.</span><span class="sxs-lookup"><span data-stu-id="67384-115">**Twilio Client** allows you to make VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="67384-116"><a id="Pricing"></a>Ceny Twilio a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="67384-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="67384-117">Přijímat Azure zákazníků [speciální nabídka](http://www.twilio.com/azure): bezplatnou 10 Twilio kreditu při upgradu vašeho účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="67384-117">Azure customers receive a [special offer](http://www.twilio.com/azure): complimentary $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="67384-118">Tato Twilio platební je použít pro všechny Twilio využití (10 platební ekvivalentní až 1 000 zpráv serveru SMS odesílání nebo přijímání až 1 000 příchozí hlasové minut, v závislosti na umístění telefonní číslo a zpráva nebo volání cíl).</span><span class="sxs-lookup"><span data-stu-id="67384-118">This Twilio Credit can be applied to any Twilio usage ($10 credit equivalent to sending as many as 1,000 SMS messages or receiving up to 1000 inbound Voice minutes, depending on the location of your phone number and message or call destination).</span></span> <span data-ttu-id="67384-119">Uplatnit tento platební Twilio a začít v: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span><span class="sxs-lookup"><span data-stu-id="67384-119">Redeem this Twilio credit and get started at: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span></span>

<span data-ttu-id="67384-120">Twilio je služba, průběžnými platbami.</span><span class="sxs-lookup"><span data-stu-id="67384-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="67384-121">Neexistují žádné poplatky nastavení a kdykoli můžete zavřít svůj účet.</span><span class="sxs-lookup"><span data-stu-id="67384-121">There are no set-up fees and you can close your account at any time.</span></span> <span data-ttu-id="67384-122">Můžete najít další podrobnosti o [Twilio ceny][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="67384-122">You can find more details at [Twilio Pricing][twilio_pricing].</span></span>

## <span data-ttu-id="67384-123"><a id="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="67384-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="67384-124">Rozhraní API Twilio je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="67384-124">The Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="67384-125">Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="67384-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="67384-126">Klíčové aspekty rozhraní API Twilio jsou příkazy Twilio a Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="67384-126">Key aspects of the Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="67384-127"><a id="Verbs"></a>Příkazy Twilio</span><span class="sxs-lookup"><span data-stu-id="67384-127"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="67384-128">Rozhraní API využívá Twilio příkazy; například  **&lt;indikované&gt;**  příkaz dá pokyn Twilio zpráva zvukově dodržujeme volání.</span><span class="sxs-lookup"><span data-stu-id="67384-128">The API makes use of Twilio verbs; for example, the **&lt;Say&gt;** verb instructs Twilio to audibly deliver a message on a call.</span></span>

<span data-ttu-id="67384-129">Následuje seznam operací Twilio.</span><span class="sxs-lookup"><span data-stu-id="67384-129">The following is a list of Twilio verbs.</span></span> <span data-ttu-id="67384-130">Další informace o jiné příkazy a možnosti prostřednictvím [Twilio Markup Language dokumentaci](http://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="67384-130">Learn about the other verbs and capabilities via [Twilio Markup Language documentation](http://www.twilio.com/docs/api/twiml).</span></span>

* <span data-ttu-id="67384-131">**&lt;Vytočit&gt;**: připojí volající na jiný telefon.</span><span class="sxs-lookup"><span data-stu-id="67384-131">**&lt;Dial&gt;**: Connects the caller to another phone.</span></span>
* <span data-ttu-id="67384-132">**&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu.</span><span class="sxs-lookup"><span data-stu-id="67384-132">**&lt;Gather&gt;**: Collects numeric digits entered on the telephone keypad.</span></span>
* <span data-ttu-id="67384-133">**&lt;Zavěšení&gt;**: ukončí volání.</span><span class="sxs-lookup"><span data-stu-id="67384-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="67384-134">**&lt;Přehrání&gt;**: přehraje zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="67384-134">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="67384-135">**&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.</span><span class="sxs-lookup"><span data-stu-id="67384-135">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="67384-136">**&lt;Záznam&gt;**: zaznamenává hlasové volajícího a vrátí adresu URL souboru, který obsahuje záznamu.</span><span class="sxs-lookup"><span data-stu-id="67384-136">**&lt;Record&gt;**: Records the caller's voice and returns a URL of a file that contains the recording.</span></span>
* <span data-ttu-id="67384-137">**&lt;Přesměrování&gt;**: předá řízení hovoru nebo SMS TwiML na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="67384-137">**&lt;Redirect&gt;**: Transfers control of a call or SMS to the TwiML at a different URL.</span></span>
* <span data-ttu-id="67384-138">**&lt;Odmítnout&gt;**: odmítne příchozí volání na vaše číslo Twilio bez fakturace můžete</span><span class="sxs-lookup"><span data-stu-id="67384-138">**&lt;Reject&gt;**: Rejects an incoming call to your Twilio number without billing you</span></span>
* <span data-ttu-id="67384-139">**&lt;Řekněme&gt;**: Převod převede textu na řeč, že z volání.</span><span class="sxs-lookup"><span data-stu-id="67384-139">**&lt;Say&gt;**: Converts text to speech that is made on a call.</span></span>
* <span data-ttu-id="67384-140">**&lt;SMS&gt;**: odešle zprávu SMS.</span><span class="sxs-lookup"><span data-stu-id="67384-140">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="67384-141"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="67384-141"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="67384-142">TwiML je sada založený na jazyce XML pokyny podle Twilio příkazy, které informují Twilio postup zpracování hovoru nebo SMS.</span><span class="sxs-lookup"><span data-stu-id="67384-142">TwiML is a set of XML-based instructions based on the Twilio verbs that inform Twilio of how to process a call or SMS.</span></span>

<span data-ttu-id="67384-143">Jako příklad následující TwiML by převést text **Hello, World** k rozpoznávání řeči.</span><span class="sxs-lookup"><span data-stu-id="67384-143">As an example, the following TwiML would convert the text **Hello World** to speech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

<span data-ttu-id="67384-144">Pokud aplikace zavolá rozhraní API Twilio, jeden z parametrů rozhraní API je adresa URL, který vrátí odpověď TwiML.</span><span class="sxs-lookup"><span data-stu-id="67384-144">When your application calls the Twilio API, one of the API parameters is the URL that returns the TwiML response.</span></span> <span data-ttu-id="67384-145">Pro účely vývoje URL můžete použít zadaný Twilio zajistit TwiML odpovědi použít v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="67384-145">For development purposes, you can use Twilio-provided URLs to provide the TwiML responses used by your applications.</span></span> <span data-ttu-id="67384-146">Může taky hostovat vlastní adresy URL k vytvoření odpovědi TwiML a další možností je použít **TwiMLResponse** objektu.</span><span class="sxs-lookup"><span data-stu-id="67384-146">You could also host your own URLs to produce the TwiML responses, and another option is to use the **TwiMLResponse** object.</span></span>

<span data-ttu-id="67384-147">Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="67384-147">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="67384-148">Další informace o rozhraní API Twilio najdete v tématu [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="67384-148">For additional information about the Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="67384-149"><a id="CreateAccount"></a>Vytvoření účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="67384-149"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="67384-150">Když budete chtít získat účet Twilio, zaregistrujte si v [zkuste Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="67384-150">When you're ready to get a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="67384-151">Můžete začít s bezplatný účet a upgradujte si účet později.</span><span class="sxs-lookup"><span data-stu-id="67384-151">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="67384-152">Když se zaregistrujete k účtu Twilio, obdržíte ID účtu a ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="67384-152">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="67384-153">Jak bude potřeba k volání rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="67384-153">Both will be needed to make Twilio API calls.</span></span> <span data-ttu-id="67384-154">Aby se zabránilo neoprávněnému přístupu ke svému účtu, zabezpečit ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="67384-154">To prevent unauthorized access to your account, keep your authentication token secure.</span></span> <span data-ttu-id="67384-155">ID účtu a ověřování tokenu je možné zobrazit na [stránku účtu Twilio][twilio_account], v polích s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="67384-155">Your account ID and authentication token are viewable at the [Twilio account page][twilio_account], in the fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="67384-156"><a id="create_app"></a>Vytvoření aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="67384-156"><a id="create_app"></a>Create a PHP Application</span></span>
<span data-ttu-id="67384-157">Aplikace PHP, která používá službu Twilio a běží v Azure je nejsou jiné než jiná aplikace PHP, která používá službu Twilio.</span><span class="sxs-lookup"><span data-stu-id="67384-157">A PHP application that uses the Twilio service and is running in Azure is no different than any other PHP application that uses the Twilio service.</span></span> <span data-ttu-id="67384-158">Při Twilio services jsou založené na REST a lze volat z PHP několika způsoby, v tomto článku se soustředí na použití služeb Twilio s [Twilio knihovny pro jazyk PHP z Githubu][twilio_php].</span><span class="sxs-lookup"><span data-stu-id="67384-158">While Twilio services are REST-based and can be called from PHP in several ways, this article will focus on how to use Twilio services with [Twilio library for PHP from GitHub][twilio_php].</span></span> <span data-ttu-id="67384-159">Další informace o používání knihovny Twilio pro jazyk PHP najdete v tématu [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span><span class="sxs-lookup"><span data-stu-id="67384-159">For more information about using the Twilio library for PHP, see [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].</span></span>

<span data-ttu-id="67384-160">Podrobné pokyny pro vytváření a nasazování aplikace Twilio nebo PHP do Azure jsou k dispozici na [postup proveďte Twilio pomocí telefonního hovoru v aplikaci PHP v Azure][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="67384-160">Detailed instructions for building and deploying a Twilio/PHP application to Azure are available at [How to Make a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="67384-161"><a id="configure_app"></a>Konfigurace aplikace k používání Twilio knihovny</span><span class="sxs-lookup"><span data-stu-id="67384-161"><a id="configure_app"></a>Configure Your Application to Use Twilio Libraries</span></span>
<span data-ttu-id="67384-162">Můžete nakonfigurovat aplikace na používání knihovny, Twilio pro jazyk PHP dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="67384-162">You can configure your application to use the Twilio library for PHP in two ways:</span></span>

1. <span data-ttu-id="67384-163">Stáhnout Twilio knihovny pro jazyk PHP z webu GitHub ([https://github.com/twilio/twilio-php][twilio_php]) a přidejte **služby** adresář do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="67384-163">Download the Twilio library for PHP from GitHub ([https://github.com/twilio/twilio-php][twilio_php]) and add the **Services** directory to your application.</span></span>
   
    <span data-ttu-id="67384-164">- nebo -</span><span class="sxs-lookup"><span data-stu-id="67384-164">-OR-</span></span>
2. <span data-ttu-id="67384-165">Nainstalujte knihovnu Twilio pro jazyk PHP jako balíček HRUŠKAMI.</span><span class="sxs-lookup"><span data-stu-id="67384-165">Install the Twilio library for PHP as a PEAR package.</span></span> <span data-ttu-id="67384-166">Může se nainstalovat pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="67384-166">It can be installed with the following commands:</span></span>
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

<span data-ttu-id="67384-167">Po instalaci Twilio knihovny pro jazyk PHP, poté můžete přidat **require_once** příkaz v horní části PHP soubory k odkazování knihovny:</span><span class="sxs-lookup"><span data-stu-id="67384-167">Once you have installed the Twilio library for PHP, you can then add a **require_once** statement at the top of your PHP files to reference the library:</span></span>

        require_once 'Services/Twilio.php';

<span data-ttu-id="67384-168">Další informace najdete v tématu [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span><span class="sxs-lookup"><span data-stu-id="67384-168">For more information, see [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].</span></span>

## <span data-ttu-id="67384-169"><a id="howto_make_call"></a>Postupy: volání odchozí</span><span class="sxs-lookup"><span data-stu-id="67384-169"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="67384-170">Následující ukazuje, jak zajistit odchozí volání pomocí **Services_Twilio** třídy.</span><span class="sxs-lookup"><span data-stu-id="67384-170">The following shows how to make an outgoing call using the **Services_Twilio** class.</span></span> <span data-ttu-id="67384-171">Tento kód také používá zadaný Twilio lokality k vrácení odpovědi Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="67384-171">This code also uses a Twilio-provided site to return the Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="67384-172">Dosaďte svoje hodnoty **z** a **k** telefonních čísel a ujistěte se, abyste ověřili **z** telefonní číslo pro svůj účet Twilio před spuštěním kódu.</span><span class="sxs-lookup"><span data-stu-id="67384-172">Substitute your values for the **From** and **To** phone numbers, and ensure that you verify the **From** phone number for your Twilio account prior to running the code.</span></span>

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
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

<span data-ttu-id="67384-173">Jak je uvedeno, tento kód používá k vrácení odpovědi TwiML zadaný Twilio lokality.</span><span class="sxs-lookup"><span data-stu-id="67384-173">As mentioned, this code uses a Twilio-provided site to return the TwiML response.</span></span> <span data-ttu-id="67384-174">Můžete místo toho použít vlastní funkční web zajistit TwiML odpovědi; Další informace najdete v tématu [jak poskytnout TwiML odpovědí z vaše vlastní webové stránky](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="67384-174">You could instead use your own site to provide the TwiML response; for more information, see [How to Provide TwiML Responses from Your Own Web Site](#howto_provide_twiml_responses).</span></span>

* <span data-ttu-id="67384-175">**Poznámka:**: Chcete-li vyřešit chyby ověření certifikátu SSL, najdete v části [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span><span class="sxs-lookup"><span data-stu-id="67384-175">**Note**: To troubleshoot SSL certificate validation errors, see [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]</span></span> 

## <span data-ttu-id="67384-176"><a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS</span><span class="sxs-lookup"><span data-stu-id="67384-176"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="67384-177">Následující ukazuje, jak odeslat zprávu SMS pomocí **Services_Twilio** třídy.</span><span class="sxs-lookup"><span data-stu-id="67384-177">The following shows how to send an SMS message using the **Services_Twilio** class.</span></span> <span data-ttu-id="67384-178">**z** číslo poskytne Twilio pro zkušebními účty k odesílání zpráv serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="67384-178">The **From** number is provided by Twilio for trial accounts to send SMS messages.</span></span> <span data-ttu-id="67384-179">**k** číslo musí být ověřena pro váš účet Twilio před spuštěním kódu.</span><span class="sxs-lookup"><span data-stu-id="67384-179">The **To** number must be verified for your Twilio account prior to running the code.</span></span>

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <span data-ttu-id="67384-180"><a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu</span><span class="sxs-lookup"><span data-stu-id="67384-180"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="67384-181">Pokud vaše aplikace zahájí volání rozhraní API Twilio, odešle Twilio vaši žádost o adresu URL, která zpět TwiML odpověď.</span><span class="sxs-lookup"><span data-stu-id="67384-181">When your application initiates a call to the Twilio API, Twilio will send your request to a URL that is expected to return a TwiML response.</span></span> <span data-ttu-id="67384-182">V předchozím příkladu používá adresu URL poskytnutou Twilio [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="67384-182">The example above uses the Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="67384-183">(Při TwiML je určen pro Twilio, můžete zobrazit it v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="67384-183">(While TwiML is designed for use by Twilio, you can view the it in your browser.</span></span> <span data-ttu-id="67384-184">For example, klikněte na tlačítko [http://twimlets.com/message] [ twimlet_message_url] zobrazíte prázdnou `<Response>` element; například klikněte na tlačítko [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] zobrazíte `<Response>` elementu, který obsahuje `<Say>` elementu.)</span><span class="sxs-lookup"><span data-stu-id="67384-184">For example, click [http://twimlets.com/message][twimlet_message_url] to see an empty `<Response>` element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] to see a `<Response>` element that contains a `<Say>` element.)</span></span>

<span data-ttu-id="67384-185">Aniž byste museli spoléhat na adresu URL pro zadaný Twilio, můžete vytvořit vlastního webu, který vrací odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="67384-185">Instead of relying on the Twilio-provided URL, you can create your own site that returns HTTP responses.</span></span> <span data-ttu-id="67384-186">Můžete vytvořit web v libovolném jazyce, který vrací odpovědí XML; Toto téma předpokládá, že budete používat PHP k vytvoření TwiML.</span><span class="sxs-lookup"><span data-stu-id="67384-186">You can create the site in any language that returns XML responses; this topic assumes you'll be using PHP to create the TwiML.</span></span>

<span data-ttu-id="67384-187">Na následující stránce PHP výsledkem TwiML odpovědi, která uvádí, že **Hello, World** při volání.</span><span class="sxs-lookup"><span data-stu-id="67384-187">The following PHP page results in a TwiML response that says **Hello World** on the call.</span></span>

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

<span data-ttu-id="67384-188">Jak je vidět na výše uvedeném příkladu, odpověď TwiML je jednoduše dokument XML.</span><span class="sxs-lookup"><span data-stu-id="67384-188">As you can see from the example above, the TwiML response is simply an XML document.</span></span> <span data-ttu-id="67384-189">Knihovna Twilio pro jazyk PHP obsahuje třídy, které způsobí vygenerování TwiML za vás.</span><span class="sxs-lookup"><span data-stu-id="67384-189">The Twilio library for PHP contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="67384-190">Následující příklad vytvoří ekvivalentní odpovědi jako v příkladu nahoře, ale používá **služby\_Twilio\_Twiml** – třída v knihovně Twilio pro jazyk PHP:</span><span class="sxs-lookup"><span data-stu-id="67384-190">The example below produces the equivalent response as shown above, but uses the **Services\_Twilio\_Twiml** class in the Twilio library for PHP:</span></span>

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

<span data-ttu-id="67384-191">Další informace o TwiML najdete v tématu [https://www.twilio.com/docs/api/twiml][twiml_reference].</span><span class="sxs-lookup"><span data-stu-id="67384-191">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml][twiml_reference].</span></span> 

<span data-ttu-id="67384-192">Jakmile máte stránku PHP nastavit na, zadejte TwiML odpovědi, použijte adresu URL stránky PHP jako adresu URL předaný do `Services_Twilio->account->calls->create` metoda.</span><span class="sxs-lookup"><span data-stu-id="67384-192">Once you have your PHP page set up to provide TwiML responses, use the URL of the PHP page as the URL passed into the  `Services_Twilio->account->calls->create`  method.</span></span> <span data-ttu-id="67384-193">Například, pokud máte webovou aplikaci s názvem **MyTwiML** nasadit do Azure hostovaná služba a název stránky PHP je **mytwiml.php**, adresa URL se dá předat do **Services_Twilio -> účet -> volání -> vytvořit** jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="67384-193">For example, if you have a Web application named **MyTwiML** deployed to an Azure hosted service, and the name of the PHP page is **mytwiml.php**, the URL can be passed to  **Services_Twilio->account->calls->create**  as shown in the following example:</span></span>

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
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

<span data-ttu-id="67384-194">Další informace o používání Twilio v Azure s PHP najdete v tématu [postup proveďte Twilio pomocí telefonního hovoru v aplikaci PHP v Azure][howto_phonecall_php].</span><span class="sxs-lookup"><span data-stu-id="67384-194">For additional information about using Twilio in Azure with PHP, see [How to Make a Phone Call Using Twilio in a PHP Application on Azure][howto_phonecall_php].</span></span>

## <span data-ttu-id="67384-195"><a id="AdditionalServices"></a>Postupy: použití další Twilio služeb</span><span class="sxs-lookup"><span data-stu-id="67384-195"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="67384-196">Kromě příkladů tady uvedené nabízí Twilio rozhraní API založené na webu, který můžete využít další funkce Twilio vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="67384-196">In addition to the examples shown here, Twilio offers web-based APIs that you can use to leverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="67384-197">Úplné podrobnosti najdete v tématu [dokumentaci k rozhraní API Twilio][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="67384-197">For full details, see the [Twilio API documentation][twilio_api_documentation].</span></span>

## <span data-ttu-id="67384-198"><a id="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="67384-198"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="67384-199">Teď, když jste se naučili základy služby Twilio, postupujte podle následujících odkazech na další informace:</span><span class="sxs-lookup"><span data-stu-id="67384-199">Now that you've learned the basics of the Twilio service, follow these links to learn more:</span></span>

* <span data-ttu-id="67384-200">[Pokyny pro zabezpečení Twilio][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="67384-200">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="67384-201">[Twilio postupy a příklady kódu][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="67384-201">[Twilio HowTo's and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="67384-202">[Kurzy Twilio rychlý start][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="67384-202">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span> 
* <span data-ttu-id="67384-203">[Twilio na Githubu][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="67384-203">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="67384-204">[Obraťte se na podporu Twilio][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="67384-204">[Talk to Twilio Support][twilio_support]</span></span>

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
