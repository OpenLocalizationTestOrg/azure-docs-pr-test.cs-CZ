---
title: "aaaHow tooUse Twilio pro hlasový a serveru SMS (Ruby) | Microsoft Docs"
description: "Zjistěte, jak toomake telefonní hovor a odesílání SMS zprávy službou hello Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořeny v Ruby."
services: 
documentationcenter: ruby
author: devinrader
manager: twilio
editor: 
ms.assetid: 60e512f6-fa47-47c0-aedc-f19bb72a1158
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 11/25/2014
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: aca5ccb4663ff03c9c1f39c848469415f06dfb45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-ruby"></a><span data-ttu-id="e4f54-104">Jak tooUse Twilio pro hlasový a možnosti serveru SMS v Ruby</span><span class="sxs-lookup"><span data-stu-id="e4f54-104">How tooUse Twilio for Voice and SMS Capabilities in Ruby</span></span>
<span data-ttu-id="e4f54-105">Tato příručka ukazuje, jak služba tooperform běžné úlohy programování s hello Twilio rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="e4f54-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="e4f54-106">pokryté scénáře Hello zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS).</span><span class="sxs-lookup"><span data-stu-id="e4f54-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="e4f54-107">Další informace o Twilio a použití hlasu a SMS ve svých aplikacích najdete v tématu hello [další kroky](#NextSteps) části.</span><span class="sxs-lookup"><span data-stu-id="e4f54-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="e4f54-108"><a id="WhatIs"></a>Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="e4f54-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="e4f54-109">Twilio je telefonickou rozhraní API webové služby, které umožňuje použít existující webové jazyky a dovednosti toobuild hlas a aplikace serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="e4f54-109">Twilio is a telephony web-service API that lets you use your existing web languages and skills toobuild voice and SMS applications.</span></span> <span data-ttu-id="e4f54-110">Twilio je služba třetích stran (ne funkcí platformy Azure a produkt společnosti Microsoft).</span><span class="sxs-lookup"><span data-stu-id="e4f54-110">Twilio is a third-party service (not an Azure feature and not a Microsoft product).</span></span>

<span data-ttu-id="e4f54-111">**Hlasové Twilio** umožňuje toomake vaší aplikace a příjem telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="e4f54-111">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="e4f54-112">**Twilio SMS** umožňuje toomake vaší aplikace a přijímat zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="e4f54-112">**Twilio SMS** allows your applications toomake and receive SMS messages.</span></span> <span data-ttu-id="e4f54-113">**Klient Twilio** umožňuje aplikace tooenable hlasové komunikaci pomocí existujícího připojení k Internetu, včetně mobilních připojení.</span><span class="sxs-lookup"><span data-stu-id="e4f54-113">**Twilio Client** allows your applications tooenable voice communication using existing Internet connections, including mobile connections.</span></span>

## <span data-ttu-id="e4f54-114"><a id="Pricing"></a>Ceny Twilio a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="e4f54-114"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="e4f54-115">Informace o cenách Twilio je k dispozici na [Twilio ceny][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="e4f54-115">Information about Twilio pricing is available at [Twilio Pricing][twilio_pricing].</span></span> <span data-ttu-id="e4f54-116">Přijímat Azure zákazníků [speciální nabídka][special_offer]: volné platební 1000 textů nebo 1000 příchozí minut.</span><span class="sxs-lookup"><span data-stu-id="e4f54-116">Azure customers receive a [special offer][special_offer]: a free credit of 1000 texts or 1000 inbound minutes.</span></span> <span data-ttu-id="e4f54-117">toosign pro toto nabízejí nebo získat další informace, navštivte [http://ahoy.twilio.com/azure][special_offer].</span><span class="sxs-lookup"><span data-stu-id="e4f54-117">toosign up for this offer or get more information, please visit [http://ahoy.twilio.com/azure][special_offer].</span></span>  

## <span data-ttu-id="e4f54-118"><a id="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="e4f54-118"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="e4f54-119">Hello Twilio rozhraní API je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="e4f54-119">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="e4f54-120">Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="e4f54-120">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

### <span data-ttu-id="e4f54-121"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="e4f54-121"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="e4f54-122">TwiML je sada pokynů formátu XML, které informovat o tom, jak Twilio tooprocess hovoru nebo SMS.</span><span class="sxs-lookup"><span data-stu-id="e4f54-122">TwiML is a set of XML-based instructions that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="e4f54-123">Jako příklad by hello následující TwiML převést hello text **Hello, World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="e4f54-123">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

<span data-ttu-id="e4f54-124">Mají všechny dokumenty TwiML `<Response>` jako kořenový element.</span><span class="sxs-lookup"><span data-stu-id="e4f54-124">All TwiML documents have `<Response>` as their root element.</span></span> <span data-ttu-id="e4f54-125">Zde můžete použít příkazy Twilio toodefine hello chování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e4f54-125">From there, you use Twilio Verbs toodefine hello behavior of your application.</span></span>

### <span data-ttu-id="e4f54-126"><a id="Verbs"></a>Příkazy TwiML</span><span class="sxs-lookup"><span data-stu-id="e4f54-126"><a id="Verbs"></a>TwiML Verbs</span></span>
<span data-ttu-id="e4f54-127">Příkazy Twilio jsou značky XML, které informují Twilio, co příliš**provést**.</span><span class="sxs-lookup"><span data-stu-id="e4f54-127">Twilio Verbs are XML tags that tell Twilio what too**do**.</span></span> <span data-ttu-id="e4f54-128">Například hello  **&lt;indikované&gt;**  příkaz dá pokyn Twilio tooaudibly doručit zprávu na volání.</span><span class="sxs-lookup"><span data-stu-id="e4f54-128">For example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span> 

<span data-ttu-id="e4f54-129">Hello následuje seznam operací Twilio.</span><span class="sxs-lookup"><span data-stu-id="e4f54-129">hello following is a list of Twilio verbs.</span></span>

* <span data-ttu-id="e4f54-130">**&lt;Vytočit&gt;**: připojí hello volající tooanother phone.</span><span class="sxs-lookup"><span data-stu-id="e4f54-130">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="e4f54-131">**&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu hello.</span><span class="sxs-lookup"><span data-stu-id="e4f54-131">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="e4f54-132">**&lt;Zavěšení&gt;**: ukončí volání.</span><span class="sxs-lookup"><span data-stu-id="e4f54-132">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="e4f54-133">**&lt;Přehrání&gt;**: přehraje zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="e4f54-133">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="e4f54-134">**&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.</span><span class="sxs-lookup"><span data-stu-id="e4f54-134">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="e4f54-135">**&lt;Záznam&gt;**: zaznamenává hlasové hello volajícího a vrátí adresu URL souboru, který obsahuje záznam hello.</span><span class="sxs-lookup"><span data-stu-id="e4f54-135">**&lt;Record&gt;**: Records hello caller's voice and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="e4f54-136">**&lt;Přesměrování&gt;**: přenáší kontrolu nad telefonického hovoru nebo SMS toohello TwiML na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e4f54-136">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="e4f54-137">**&lt;Odmítnout&gt;**: odmítne na příchozí volání číslo Twilio tooyour bez fakturace můžete</span><span class="sxs-lookup"><span data-stu-id="e4f54-137">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you</span></span>
* <span data-ttu-id="e4f54-138">**&lt;Řekněme&gt;**: toospeech převede text, že z volání.</span><span class="sxs-lookup"><span data-stu-id="e4f54-138">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="e4f54-139">**&lt;SMS&gt;**: odešle zprávu SMS.</span><span class="sxs-lookup"><span data-stu-id="e4f54-139">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

<span data-ttu-id="e4f54-140">Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="e4f54-140">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="e4f54-141">Další informace o hello Twilio API najdete v tématu [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="e4f54-141">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="e4f54-142"><a id="CreateAccount"></a>Vytvoření účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="e4f54-142"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="e4f54-143">Když jste připravené tooget Twilio účet, zaregistrovat na [zkuste Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="e4f54-143">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="e4f54-144">Můžete začít s bezplatný účet a upgradujte si účet později.</span><span class="sxs-lookup"><span data-stu-id="e4f54-144">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="e4f54-145">Když zaregistrujete k účtu Twilio, získáte bezplatné telefonní číslo pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e4f54-145">When you sign up for a Twilio account, you'll get a free phone number for your application.</span></span> <span data-ttu-id="e4f54-146">Také budete dostávat účet SID a tokenu ověřování.</span><span class="sxs-lookup"><span data-stu-id="e4f54-146">You'll also receive an account SID and an auth token.</span></span> <span data-ttu-id="e4f54-147">Jak bude volání rozhraní API Twilio potřebné toomake.</span><span class="sxs-lookup"><span data-stu-id="e4f54-147">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="e4f54-148">tooprevent neoprávněný přístup k účtu tooyour, ověřovací token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e4f54-148">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="e4f54-149">SID účtu a ověření tokenu jsou viditelná na hello [stránku účtu Twilio][twilio_account]v hello pole s názvem bez přípony **SID účtu** a **ověřování TOKENU** , v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="e4f54-149">Your account SID and auth token are viewable at hello [Twilio account page][twilio_account], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

### <span data-ttu-id="e4f54-150"><a id="VerifyPhoneNumbers"></a>Zkontrolujte telefonní čísla</span><span class="sxs-lookup"><span data-stu-id="e4f54-150"><a id="VerifyPhoneNumbers"></a>Verify Phone Numbers</span></span>
<span data-ttu-id="e4f54-151">V přidání toohello číslo, které jsou uvedeny ve Twilio můžete si taky ověřit čísla řízení (tj. vaše mobilní telefon nebo Domovská telefonní číslo) pro použití v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="e4f54-151">In addition toohello number you are given by Twilio, you can also verify numbers that you control (i.e. your cell phone or home phone number) for use in your applications.</span></span> 

<span data-ttu-id="e4f54-152">Informace o tom najdete v části tooverify telefonní číslo, [spravovat čísla][verify_phone].</span><span class="sxs-lookup"><span data-stu-id="e4f54-152">For information on how tooverify a phone number, see [Manage Numbers][verify_phone].</span></span>

## <span data-ttu-id="e4f54-153"><a id="create_app"></a>Vytvoření Ruby aplikace</span><span class="sxs-lookup"><span data-stu-id="e4f54-153"><a id="create_app"></a>Create a Ruby Application</span></span>
<span data-ttu-id="e4f54-154">Poznámky Ruby aplikace, která používá službu hello Twilio a běží v Azure je nejsou jiné než jiná Ruby aplikace, která používá službu Twilio hello.</span><span class="sxs-lookup"><span data-stu-id="e4f54-154">A Ruby application that uses hello Twilio service and is running in Azure is no different than any other Ruby application that uses hello Twilio service.</span></span> <span data-ttu-id="e4f54-155">Při Twilio služby jsou dosáhl standardu RESTful a lze volat z Ruby několika způsoby, v tomto článku se soustředí na jak toouse Twilio služeb s [Twilio pomocné knihovny pro Ruby][twilio_ruby].</span><span class="sxs-lookup"><span data-stu-id="e4f54-155">While Twilio services are RESTful and can be called from Ruby in several ways, this article will focus on how toouse Twilio services with [Twilio helper library for Ruby][twilio_ruby].</span></span>

<span data-ttu-id="e4f54-156">První, [nastavení nového virtuálního počítače Azure Linux] [ azure_vm_setup] tooact jako hostitele pro nový Ruby webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e4f54-156">First, [set-up a new Azure Linux VM][azure_vm_setup] tooact as a host for your new Ruby web application.</span></span> <span data-ttu-id="e4f54-157">Ignorujte hello kroky zahrnující hello vytvoření aplikace které právě hello nastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e4f54-157">Ignore hello steps involving hello creation of a Rails app, just set-up hello VM.</span></span> <span data-ttu-id="e4f54-158">Ujistěte se, že vytvoříte koncový bod s externí port 80 a interní port 5000.</span><span class="sxs-lookup"><span data-stu-id="e4f54-158">Make sure you create an Endpoint with an external port of 80 and an internal port of 5000.</span></span>

<span data-ttu-id="e4f54-159">V následujících příkladech hello, použijeme [Sinatra][sinatra], velmi jednoduché webové rozhraní pro Ruby.</span><span class="sxs-lookup"><span data-stu-id="e4f54-159">In hello examples below, we will be using [Sinatra][sinatra], a very simple web framework for Ruby.</span></span> <span data-ttu-id="e4f54-160">Ale určitě můžete hello Twilio pomocné knihovny pro Ruby s jakékoli jiné webové rozhraní, včetně Ruby, na které.</span><span class="sxs-lookup"><span data-stu-id="e4f54-160">But you can certainly use hello Twilio helper library for Ruby with any other web framework, including Ruby on Rails.</span></span>

<span data-ttu-id="e4f54-161">SSH do nového virtuálního počítače a vytvořte adresář pro nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e4f54-161">SSH into your new VM and create a directory for your new app.</span></span> <span data-ttu-id="e4f54-162">V tomto adresáři vytvořte soubor s názvem hello Gemfile a zkopírujte do ní následující kód:</span><span class="sxs-lookup"><span data-stu-id="e4f54-162">Inside that directory, create a file called Gemfile and copy hello following code into it:</span></span>

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

<span data-ttu-id="e4f54-163">Na příkazovém řádku hello spustit `bundle install`.</span><span class="sxs-lookup"><span data-stu-id="e4f54-163">On hello command line run `bundle install`.</span></span> <span data-ttu-id="e4f54-164">Nainstaluje hello závislostí uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="e4f54-164">This will install hello dependencies above.</span></span> <span data-ttu-id="e4f54-165">Dále vytvořte soubor s názvem `web.rb`.</span><span class="sxs-lookup"><span data-stu-id="e4f54-165">Next create a file called `web.rb`.</span></span> <span data-ttu-id="e4f54-166">Bude jím, kde je umístěn hello kódu pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e4f54-166">This will be where hello code for your web app lives.</span></span> <span data-ttu-id="e4f54-167">Vložte následující kód do ní hello:</span><span class="sxs-lookup"><span data-stu-id="e4f54-167">Paste hello following code into it:</span></span>

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

<span data-ttu-id="e4f54-168">V tomto okamžiku by měl být schopný hello spustit hello příkaz `ruby web.rb -p 5000`.</span><span class="sxs-lookup"><span data-stu-id="e4f54-168">At this point you should be able hello run hello command `ruby web.rb -p 5000`.</span></span> <span data-ttu-id="e4f54-169">To bude roztočení malé webový server na portu 5000.</span><span class="sxs-lookup"><span data-stu-id="e4f54-169">This will spin-up a small web server on port 5000.</span></span> <span data-ttu-id="e4f54-170">Byste měli mít toobrowse toothis aplikace v prohlížeči návštěvou hello URL nastavení pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="e4f54-170">You should be able toobrowse toothis app in your browser by visiting hello URL you set-up for your Azure VM.</span></span> <span data-ttu-id="e4f54-171">Jakmile se můžete dostat vaší webové aplikace v prohlížeči hello, jste připravené toostart vytváření aplikace Twilio.</span><span class="sxs-lookup"><span data-stu-id="e4f54-171">Once you can reach your web app in hello browser, you're ready toostart building a Twilio app.</span></span>

## <span data-ttu-id="e4f54-172"><a id="configure_app"></a>Nakonfigurujte si aplikace tooUse Twilio</span><span class="sxs-lookup"><span data-stu-id="e4f54-172"><a id="configure_app"></a>Configure Your Application tooUse Twilio</span></span>
<span data-ttu-id="e4f54-173">Můžete nakonfigurovat své webové aplikace toouse hello Twilio knihovny aktualizace vašeho `Gemfile` tooinclude tento řádek:</span><span class="sxs-lookup"><span data-stu-id="e4f54-173">You can configure your web app toouse hello Twilio library by updating your `Gemfile` tooinclude this line:</span></span>

    gem 'twilio-ruby'

<span data-ttu-id="e4f54-174">Na příkazovém řádku hello spusťte `bundle install`.</span><span class="sxs-lookup"><span data-stu-id="e4f54-174">On hello command line, run `bundle install`.</span></span> <span data-ttu-id="e4f54-175">Nyní otevřete `web.rb` a včetně tento řádek v horní části hello:</span><span class="sxs-lookup"><span data-stu-id="e4f54-175">Now open `web.rb` and including this line at hello top:</span></span>

    require 'twilio-ruby'

<span data-ttu-id="e4f54-176">Jste nyní všechny nastavit toouse hello Twilio pomocné knihovny pro Ruby ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e4f54-176">You're now all set toouse hello Twilio helper library for Ruby in your web app.</span></span>

## <span data-ttu-id="e4f54-177"><a id="howto_make_call"></a>Postupy: volání odchozí</span><span class="sxs-lookup"><span data-stu-id="e4f54-177"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="e4f54-178">Hello následující ukazuje, jak volat toomake odchozí.</span><span class="sxs-lookup"><span data-stu-id="e4f54-178">hello following shows how toomake an outgoing call.</span></span> <span data-ttu-id="e4f54-179">Klíčové koncepty zahrnují pomocí hello Twilio pomocné knihovny pro poznámky Ruby toomake, které volání rozhraní REST API a vykreslování TwiML.</span><span class="sxs-lookup"><span data-stu-id="e4f54-179">Key concepts include using hello Twilio helper library for Ruby toomake REST API calls and rendering TwiML.</span></span> <span data-ttu-id="e4f54-180">Dosaďte svoje hodnoty hello **z** a **k** telefonních čísel a ujistěte se, abyste ověřili hello **z** telefonní číslo kódu hello předchozí toorunning účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="e4f54-180">Substitute your values for hello **From** and **To** phone numbers, and ensure that you verify hello **From** phone number for your Twilio account prior toorunning hello code.</span></span>

<span data-ttu-id="e4f54-181">Přidejte tuto funkci příliš`web.md`:</span><span class="sxs-lookup"><span data-stu-id="e4f54-181">Add this function too`web.md`:</span></span>

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # hello number of hello phone initiating hello hello call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # hello number of hello phone receiving call.
    too= "NNNNNNNNNNN";

    # Use hello Twilio-provided site for hello TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";

    get '/make_call' do
      # Create hello call client.
      client = Twilio::REST::Client.new(sid, token);

      # Make hello call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end

<span data-ttu-id="e4f54-182">Pokud otevřete až `http://yourdomain.cloudapp.net/make_call` v prohlížeči, který aktivuje hello volání toohello Twilio API toomake hello telefonního hovoru.</span><span class="sxs-lookup"><span data-stu-id="e4f54-182">If you open-up `http://yourdomain.cloudapp.net/make_call` in a browser, that will trigger hello call toohello Twilio API toomake hello phone call.</span></span> <span data-ttu-id="e4f54-183">Hello první dva parametry v `client.account.calls.create` poměrně není potřeba vysvětlovat: počet volání hello hello je `from` a počet volání hello hello je `to`.</span><span class="sxs-lookup"><span data-stu-id="e4f54-183">hello first two parameters in `client.account.calls.create` are fairly self-explanatory: hello number hello call is `from` and hello number hello call is `to`.</span></span> 

<span data-ttu-id="e4f54-184">třetí parametr Hello (`url`) je adresa URL hello že Twilio požadavky tooget pokyny, které toodo hello volání po připojení.</span><span class="sxs-lookup"><span data-stu-id="e4f54-184">hello third parameter (`url`) is hello URL that Twilio requests tooget instructions on what toodo once hello call is connected.</span></span> <span data-ttu-id="e4f54-185">V tomto případě jsme nastavení a adresy URL (`http://yourdomain.cloudapp.net`), vrátí jednoduché TwiML dokument a používá hello `<Say>` toodo příkaz některé převod textu na řeč a vyslovte "Hello opic" toohello osoba, která přijímá hello volání.</span><span class="sxs-lookup"><span data-stu-id="e4f54-185">In this case we set-up a URL (`http://yourdomain.cloudapp.net`) that returns a simple TwiML document and uses hello `<Say>` verb toodo some text-to-speech and say "Hello Monkey" toohello person recieving hello call.</span></span>

## <span data-ttu-id="e4f54-186"><a id="howto_recieve_sms"></a>Postupy: Receive zpráva SMS</span><span class="sxs-lookup"><span data-stu-id="e4f54-186"><a id="howto_recieve_sms"></a>How to: Recieve an SMS message</span></span>
<span data-ttu-id="e4f54-187">V předchozím příkladu hello jsme spustili **odchozí** telefonního hovoru.</span><span class="sxs-lookup"><span data-stu-id="e4f54-187">In hello previous example we initiated an **outgoing** phone call.</span></span> <span data-ttu-id="e4f54-188">Tento čas, použijeme hello telefonní číslo, které Twilio jste nám dali během registrace tooprocess **příchozí** zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="e4f54-188">This time, let's use hello phone number that Twilio gave us during sign-up tooprocess an **incoming** SMS message.</span></span>

<span data-ttu-id="e4f54-189">První, přihlášení tooyour [řídicí panel Twilio][twilio_account].</span><span class="sxs-lookup"><span data-stu-id="e4f54-189">First, log-in tooyour [Twilio dashboard][twilio_account].</span></span> <span data-ttu-id="e4f54-190">Klikněte na "Čísla" v horním nav hello a potom klikněte na hello Twilio číslo, které byly poskytnuté.</span><span class="sxs-lookup"><span data-stu-id="e4f54-190">Click on "Numbers" in hello top nav and then click on hello Twilio number you were provided.</span></span> <span data-ttu-id="e4f54-191">Zobrazí se dvou adres URL, která se dají konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="e4f54-191">You'll see two URLs that you can configure.</span></span> <span data-ttu-id="e4f54-192">Adresa URL požadavku na adrese URL žádosti hlasu a serveru služby SMS.</span><span class="sxs-lookup"><span data-stu-id="e4f54-192">A Voice Request URL and an SMS Request URL.</span></span> <span data-ttu-id="e4f54-193">Tyto jsou hello adresy URL, které se provádí volání Twilio vždy, když telefonní hovor nebo serveru služby SMS je odeslaných tooyour číslo.</span><span class="sxs-lookup"><span data-stu-id="e4f54-193">These are hello URLs that Twilio calls whenever a phone call is made or an SMS is sent tooyour number.</span></span> <span data-ttu-id="e4f54-194">adresy URL Hello se také označuje jako "web háky".</span><span class="sxs-lookup"><span data-stu-id="e4f54-194">hello URLs are also known as "web hooks".</span></span>

<span data-ttu-id="e4f54-195">Rádi bychom znali tooprocess příchozí zprávy SMS, takže umožňuje aktualizovat adresu URL hello příliš`http://yourdomain.cloudapp.net/sms_url`.</span><span class="sxs-lookup"><span data-stu-id="e4f54-195">We would like tooprocess incoming SMS messages, so let's update hello URL too`http://yourdomain.cloudapp.net/sms_url`.</span></span> <span data-ttu-id="e4f54-196">Pokračujte a klikněte na tlačítko Uložit změny v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="e4f54-196">Go ahead and click Save Changes at hello bottom of hello page.</span></span> <span data-ttu-id="e4f54-197">Nyní, zpět na `web.rb` umožňuje programu naše aplikace toohandle toto:</span><span class="sxs-lookup"><span data-stu-id="e4f54-197">Now, back in `web.rb` let's program our application toohandle this:</span></span>

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for hello ping! Twilio and Azure rock!</Message>
       </Response>"
    end

<span data-ttu-id="e4f54-198">Po provedení změny hello, zkontrolujte, zda toore-start vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e4f54-198">After making hello change, make sure toore-start your web app.</span></span> <span data-ttu-id="e4f54-199">Teď vyjměte telefonu a odeslat služby SMS tooyour Twilio číslo.</span><span class="sxs-lookup"><span data-stu-id="e4f54-199">Now, take out your phone and send an SMS tooyour Twilio number.</span></span> <span data-ttu-id="e4f54-200">Měli byste rychle získat odpověď serveru SMS, která říká "blogu Hey, Děkujeme, že hello ping!</span><span class="sxs-lookup"><span data-stu-id="e4f54-200">You should promptly get an SMS response that says "Hey, thanks for hello ping!</span></span> <span data-ttu-id="e4f54-201">Twilio a Azure rock! ".</span><span class="sxs-lookup"><span data-stu-id="e4f54-201">Twilio and Azure rock!".</span></span>

## <span data-ttu-id="e4f54-202"><a id="additional_services"></a>Postupy: použití další Twilio služeb</span><span class="sxs-lookup"><span data-stu-id="e4f54-202"><a id="additional_services"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="e4f54-203">Kromě toho toohello příklady zobrazeny zde, že twilio nabízí webové rozhraní API, které můžete použít další funkce Twilio tooleverage z vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="e4f54-203">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="e4f54-204">Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API Twilio][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="e4f54-204">For full details, see hello [Twilio API documentation][twilio_api_documentation].</span></span>

### <span data-ttu-id="e4f54-205"><a id="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="e4f54-205"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="e4f54-206">Teď, když jste se naučili základy hello hello Twilio služby, použijte tyto odkazy toolearn Další:</span><span class="sxs-lookup"><span data-stu-id="e4f54-206">Now that you've learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="e4f54-207">[Pokyny pro zabezpečení Twilio][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="e4f54-207">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="e4f54-208">[Twilio HowTos a ukázkový kód][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="e4f54-208">[Twilio HowTos and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="e4f54-209">[Kurzy Twilio rychlý start][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="e4f54-209">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span> 
* <span data-ttu-id="e4f54-210">[Twilio na Githubu][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="e4f54-210">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="e4f54-211">[Komunikovat tooTwilio podpory][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="e4f54-211">[Talk tooTwilio Support][twilio_support]</span></span>

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





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
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
