---
title: "Jak používat Twilio pro hlasový a serveru SMS (Ruby) | Microsoft Docs"
description: "Naučte se telefonní hovor a odeslání zprávy SMS službou Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořeny v Ruby."
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
ms.openlocfilehash: 69e50e7fe0e1f302e96c309878b8dea6869dff4a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a><span data-ttu-id="4ae64-104">Jak používat Twilio pro hlasový a možnosti serveru SMS v Ruby</span><span class="sxs-lookup"><span data-stu-id="4ae64-104">How to Use Twilio for Voice and SMS Capabilities in Ruby</span></span>
<span data-ttu-id="4ae64-105">Tato příručka ukazuje, jak provádět běžné úkoly programování službou Twilio rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="4ae64-105">This guide demonstrates how to perform common programming tasks with the Twilio API service on Azure.</span></span> <span data-ttu-id="4ae64-106">Pokryté scénáře zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS).</span><span class="sxs-lookup"><span data-stu-id="4ae64-106">The scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="4ae64-107">Další informace o Twilio a používání hlasového a SMS ve svých aplikacích najdete v tématu [další kroky](#NextSteps) části.</span><span class="sxs-lookup"><span data-stu-id="4ae64-107">For more information on Twilio and using voice and SMS in your applications, see the [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="4ae64-108"><a id="WhatIs"></a>Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="4ae64-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="4ae64-109">Twilio je telefonickou rozhraní API webové služby, který vám umožní používat existující webové jazyky a dovednosti pro sestavení hlasu a aplikace serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="4ae64-109">Twilio is a telephony web-service API that lets you use your existing web languages and skills to build voice and SMS applications.</span></span> <span data-ttu-id="4ae64-110">Twilio je služba třetích stran (ne funkcí platformy Azure a produkt společnosti Microsoft).</span><span class="sxs-lookup"><span data-stu-id="4ae64-110">Twilio is a third-party service (not an Azure feature and not a Microsoft product).</span></span>

<span data-ttu-id="4ae64-111">**Hlasové Twilio** umožňuje aplikacím zkontrolujte a příjem telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="4ae64-111">**Twilio Voice** allows your applications to make and receive phone calls.</span></span> <span data-ttu-id="4ae64-112">**Twilio SMS** umožňuje aplikacím zkontrolujte a přijímat zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="4ae64-112">**Twilio SMS** allows your applications to make and receive SMS messages.</span></span> <span data-ttu-id="4ae64-113">**Klient Twilio** umožňuje aplikace tak, aby hlasové komunikace pomocí existujícího připojení k Internetu, včetně mobilních připojení.</span><span class="sxs-lookup"><span data-stu-id="4ae64-113">**Twilio Client** allows your applications to enable voice communication using existing Internet connections, including mobile connections.</span></span>

## <span data-ttu-id="4ae64-114"><a id="Pricing"></a>Ceny Twilio a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="4ae64-114"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="4ae64-115">Informace o cenách Twilio je k dispozici na [Twilio ceny][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="4ae64-115">Information about Twilio pricing is available at [Twilio Pricing][twilio_pricing].</span></span> <span data-ttu-id="4ae64-116">Přijímat Azure zákazníků [speciální nabídka][special_offer]: volné platební 1000 textů nebo 1000 příchozí minut.</span><span class="sxs-lookup"><span data-stu-id="4ae64-116">Azure customers receive a [special offer][special_offer]: a free credit of 1000 texts or 1000 inbound minutes.</span></span> <span data-ttu-id="4ae64-117">Pokud chcete zaregistrovat v rámci této nabídky nebo získat další informace, navštivte [http://ahoy.twilio.com/azure][special_offer].</span><span class="sxs-lookup"><span data-stu-id="4ae64-117">To sign up for this offer or get more information, please visit [http://ahoy.twilio.com/azure][special_offer].</span></span>  

## <span data-ttu-id="4ae64-118"><a id="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="4ae64-118"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="4ae64-119">Rozhraní API Twilio je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ae64-119">The Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="4ae64-120">Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="4ae64-120">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

### <span data-ttu-id="4ae64-121"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="4ae64-121"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="4ae64-122">TwiML je sada pokynů formátu XML, které informovat Twilio postup zpracování hovoru nebo SMS.</span><span class="sxs-lookup"><span data-stu-id="4ae64-122">TwiML is a set of XML-based instructions that inform Twilio of how to process a call or SMS.</span></span>

<span data-ttu-id="4ae64-123">Jako příklad následující TwiML by převést text **Hello, World** k rozpoznávání řeči.</span><span class="sxs-lookup"><span data-stu-id="4ae64-123">As an example, the following TwiML would convert the text **Hello World** to speech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

<span data-ttu-id="4ae64-124">Mají všechny dokumenty TwiML `<Response>` jako kořenový element.</span><span class="sxs-lookup"><span data-stu-id="4ae64-124">All TwiML documents have `<Response>` as their root element.</span></span> <span data-ttu-id="4ae64-125">Z tohoto místa používáte Twilio příkazy k definování chování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ae64-125">From there, you use Twilio Verbs to define the behavior of your application.</span></span>

### <span data-ttu-id="4ae64-126"><a id="Verbs"></a>Příkazy TwiML</span><span class="sxs-lookup"><span data-stu-id="4ae64-126"><a id="Verbs"></a>TwiML Verbs</span></span>
<span data-ttu-id="4ae64-127">Příkazy Twilio jsou značky XML, které informují Twilio, co **provést**.</span><span class="sxs-lookup"><span data-stu-id="4ae64-127">Twilio Verbs are XML tags that tell Twilio what to **do**.</span></span> <span data-ttu-id="4ae64-128">Například  **&lt;indikované&gt;**  příkaz dá pokyn Twilio zpráva zvukově dodržujeme volání.</span><span class="sxs-lookup"><span data-stu-id="4ae64-128">For example, the **&lt;Say&gt;** verb instructs Twilio to audibly deliver a message on a call.</span></span> 

<span data-ttu-id="4ae64-129">Následuje seznam operací Twilio.</span><span class="sxs-lookup"><span data-stu-id="4ae64-129">The following is a list of Twilio verbs.</span></span>

* <span data-ttu-id="4ae64-130">**&lt;Vytočit&gt;**: připojí volající na jiný telefon.</span><span class="sxs-lookup"><span data-stu-id="4ae64-130">**&lt;Dial&gt;**: Connects the caller to another phone.</span></span>
* <span data-ttu-id="4ae64-131">**&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu.</span><span class="sxs-lookup"><span data-stu-id="4ae64-131">**&lt;Gather&gt;**: Collects numeric digits entered on the telephone keypad.</span></span>
* <span data-ttu-id="4ae64-132">**&lt;Zavěšení&gt;**: ukončí volání.</span><span class="sxs-lookup"><span data-stu-id="4ae64-132">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="4ae64-133">**&lt;Přehrání&gt;**: přehraje zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="4ae64-133">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="4ae64-134">**&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.</span><span class="sxs-lookup"><span data-stu-id="4ae64-134">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="4ae64-135">**&lt;Záznam&gt;**: zaznamenává hlasové volajícího a vrátí adresu URL souboru, který obsahuje záznamu.</span><span class="sxs-lookup"><span data-stu-id="4ae64-135">**&lt;Record&gt;**: Records the caller's voice and returns a URL of a file that contains the recording.</span></span>
* <span data-ttu-id="4ae64-136">**&lt;Přesměrování&gt;**: předá řízení hovoru nebo SMS TwiML na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="4ae64-136">**&lt;Redirect&gt;**: Transfers control of a call or SMS to the TwiML at a different URL.</span></span>
* <span data-ttu-id="4ae64-137">**&lt;Odmítnout&gt;**: odmítne příchozí volání na vaše číslo Twilio bez fakturace můžete</span><span class="sxs-lookup"><span data-stu-id="4ae64-137">**&lt;Reject&gt;**: Rejects an incoming call to your Twilio number without billing you</span></span>
* <span data-ttu-id="4ae64-138">**&lt;Řekněme&gt;**: Převod převede textu na řeč, že z volání.</span><span class="sxs-lookup"><span data-stu-id="4ae64-138">**&lt;Say&gt;**: Converts text to speech that is made on a call.</span></span>
* <span data-ttu-id="4ae64-139">**&lt;SMS&gt;**: odešle zprávu SMS.</span><span class="sxs-lookup"><span data-stu-id="4ae64-139">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

<span data-ttu-id="4ae64-140">Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="4ae64-140">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="4ae64-141">Další informace o rozhraní API Twilio najdete v tématu [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="4ae64-141">For additional information about the Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="4ae64-142"><a id="CreateAccount"></a>Vytvoření účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="4ae64-142"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="4ae64-143">Když budete chtít získat účet Twilio, zaregistrujte si v [zkuste Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="4ae64-143">When you're ready to get a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="4ae64-144">Můžete začít s bezplatný účet a upgradujte si účet později.</span><span class="sxs-lookup"><span data-stu-id="4ae64-144">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="4ae64-145">Když zaregistrujete k účtu Twilio, získáte bezplatné telefonní číslo pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4ae64-145">When you sign up for a Twilio account, you'll get a free phone number for your application.</span></span> <span data-ttu-id="4ae64-146">Také budete dostávat účet SID a tokenu ověřování.</span><span class="sxs-lookup"><span data-stu-id="4ae64-146">You'll also receive an account SID and an auth token.</span></span> <span data-ttu-id="4ae64-147">Jak bude potřeba k volání rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="4ae64-147">Both will be needed to make Twilio API calls.</span></span> <span data-ttu-id="4ae64-148">Aby se zabránilo neoprávněnému přístupu ke svému účtu, zabezpečit ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="4ae64-148">To prevent unauthorized access to your account, keep your authentication token secure.</span></span> <span data-ttu-id="4ae64-149">SID účtu a ověření tokenu jsou viditelná na [stránku účtu Twilio][twilio_account], v polích s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="4ae64-149">Your account SID and auth token are viewable at the [Twilio account page][twilio_account], in the fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

### <span data-ttu-id="4ae64-150"><a id="VerifyPhoneNumbers"></a>Zkontrolujte telefonní čísla</span><span class="sxs-lookup"><span data-stu-id="4ae64-150"><a id="VerifyPhoneNumbers"></a>Verify Phone Numbers</span></span>
<span data-ttu-id="4ae64-151">Kromě číslo platí při Twilio, můžete si taky ověřit čísla (tj. vaše mobilní telefon nebo Domovská telefonní číslo) řízení pro použití v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="4ae64-151">In addition to the number you are given by Twilio, you can also verify numbers that you control (i.e. your cell phone or home phone number) for use in your applications.</span></span> 

<span data-ttu-id="4ae64-152">Informace o tom, jak ověření telefonního čísla najdete v tématu [spravovat čísla][verify_phone].</span><span class="sxs-lookup"><span data-stu-id="4ae64-152">For information on how to verify a phone number, see [Manage Numbers][verify_phone].</span></span>

## <span data-ttu-id="4ae64-153"><a id="create_app"></a>Vytvoření Ruby aplikace</span><span class="sxs-lookup"><span data-stu-id="4ae64-153"><a id="create_app"></a>Create a Ruby Application</span></span>
<span data-ttu-id="4ae64-154">Ruby aplikace, která používá službu Twilio a běží v Azure je nejsou jiné než jiná Ruby aplikace, která používá službu Twilio.</span><span class="sxs-lookup"><span data-stu-id="4ae64-154">A Ruby application that uses the Twilio service and is running in Azure is no different than any other Ruby application that uses the Twilio service.</span></span> <span data-ttu-id="4ae64-155">Při Twilio služby jsou dosáhl standardu RESTful a lze volat z Ruby několika způsoby, v tomto článku se soustředí na použití služeb Twilio s [Twilio pomocné knihovny pro Ruby][twilio_ruby].</span><span class="sxs-lookup"><span data-stu-id="4ae64-155">While Twilio services are RESTful and can be called from Ruby in several ways, this article will focus on how to use Twilio services with [Twilio helper library for Ruby][twilio_ruby].</span></span>

<span data-ttu-id="4ae64-156">První, [nastavení nového virtuálního počítače Azure Linux] [ azure_vm_setup] tak, aby fungoval jako hostitele pro nový Ruby webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ae64-156">First, [set-up a new Azure Linux VM][azure_vm_setup] to act as a host for your new Ruby web application.</span></span> <span data-ttu-id="4ae64-157">Kroky týkající se vytváření aplikace, které právě vytvořeny virtuální počítač ignorujte.</span><span class="sxs-lookup"><span data-stu-id="4ae64-157">Ignore the steps involving the creation of a Rails app, just set-up the VM.</span></span> <span data-ttu-id="4ae64-158">Ujistěte se, že vytvoříte koncový bod s externí port 80 a interní port 5000.</span><span class="sxs-lookup"><span data-stu-id="4ae64-158">Make sure you create an Endpoint with an external port of 80 and an internal port of 5000.</span></span>

<span data-ttu-id="4ae64-159">V následujících příkladech použijeme [Sinatra][sinatra], velmi jednoduché webové rozhraní pro Ruby.</span><span class="sxs-lookup"><span data-stu-id="4ae64-159">In the examples below, we will be using [Sinatra][sinatra], a very simple web framework for Ruby.</span></span> <span data-ttu-id="4ae64-160">Ale určitě můžete Twilio pomocné knihovny pro Ruby s jakékoli jiné webové rozhraní, včetně Ruby, na které.</span><span class="sxs-lookup"><span data-stu-id="4ae64-160">But you can certainly use the Twilio helper library for Ruby with any other web framework, including Ruby on Rails.</span></span>

<span data-ttu-id="4ae64-161">SSH do nového virtuálního počítače a vytvořte adresář pro nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ae64-161">SSH into your new VM and create a directory for your new app.</span></span> <span data-ttu-id="4ae64-162">V tomto adresáři vytvořte soubor s názvem Gemfile a zkopírujte do ní následující kód:</span><span class="sxs-lookup"><span data-stu-id="4ae64-162">Inside that directory, create a file called Gemfile and copy the following code into it:</span></span>

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

<span data-ttu-id="4ae64-163">Na příkazovém řádku spusťte `bundle install`.</span><span class="sxs-lookup"><span data-stu-id="4ae64-163">On the command line run `bundle install`.</span></span> <span data-ttu-id="4ae64-164">Dojde k instalaci závislostí uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="4ae64-164">This will install the dependencies above.</span></span> <span data-ttu-id="4ae64-165">Dále vytvořte soubor s názvem `web.rb`.</span><span class="sxs-lookup"><span data-stu-id="4ae64-165">Next create a file called `web.rb`.</span></span> <span data-ttu-id="4ae64-166">Bude jím, kde je umístěn kódu pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4ae64-166">This will be where the code for your web app lives.</span></span> <span data-ttu-id="4ae64-167">Vložte do něj následující kód:</span><span class="sxs-lookup"><span data-stu-id="4ae64-167">Paste the following code into it:</span></span>

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

<span data-ttu-id="4ae64-168">V tomto okamžiku byste měli mít možnost spustit příkaz `ruby web.rb -p 5000`.</span><span class="sxs-lookup"><span data-stu-id="4ae64-168">At this point you should be able the run the command `ruby web.rb -p 5000`.</span></span> <span data-ttu-id="4ae64-169">To bude roztočení malé webový server na portu 5000.</span><span class="sxs-lookup"><span data-stu-id="4ae64-169">This will spin-up a small web server on port 5000.</span></span> <span data-ttu-id="4ae64-170">Nyní byste měli mít vyhledejte tuto aplikaci v prohlížeči návštěvou URL jste nastavení pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="4ae64-170">You should be able to browse to this app in your browser by visiting the URL you set-up for your Azure VM.</span></span> <span data-ttu-id="4ae64-171">Jakmile se můžete dostat vaší webové aplikace v prohlížeči, jste připravení začít vytvářet aplikace Twilio.</span><span class="sxs-lookup"><span data-stu-id="4ae64-171">Once you can reach your web app in the browser, you're ready to start building a Twilio app.</span></span>

## <span data-ttu-id="4ae64-172"><a id="configure_app"></a>Konfigurace aplikace k používání Twilio</span><span class="sxs-lookup"><span data-stu-id="4ae64-172"><a id="configure_app"></a>Configure Your Application to Use Twilio</span></span>
<span data-ttu-id="4ae64-173">Můžete nakonfigurovat webové aplikace na používání knihovny, Twilio aktualizací vaší `Gemfile` zahrnout tento řádek:</span><span class="sxs-lookup"><span data-stu-id="4ae64-173">You can configure your web app to use the Twilio library by updating your `Gemfile` to include this line:</span></span>

    gem 'twilio-ruby'

<span data-ttu-id="4ae64-174">Na příkazovém řádku spusťte `bundle install`.</span><span class="sxs-lookup"><span data-stu-id="4ae64-174">On the command line, run `bundle install`.</span></span> <span data-ttu-id="4ae64-175">Nyní otevřete `web.rb` a včetně tento řádek v horní části:</span><span class="sxs-lookup"><span data-stu-id="4ae64-175">Now open `web.rb` and including this line at the top:</span></span>

    require 'twilio-ruby'

<span data-ttu-id="4ae64-176">Jste nyní vše připraveno na používání knihovny Twilio pomocné rutiny pro Ruby ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4ae64-176">You're now all set to use the Twilio helper library for Ruby in your web app.</span></span>

## <span data-ttu-id="4ae64-177"><a id="howto_make_call"></a>Postupy: volání odchozí</span><span class="sxs-lookup"><span data-stu-id="4ae64-177"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="4ae64-178">Následující ukazuje, jak provést odchozí volání.</span><span class="sxs-lookup"><span data-stu-id="4ae64-178">The following shows how to make an outgoing call.</span></span> <span data-ttu-id="4ae64-179">Klíčové koncepty zahrnují používat Twilio pomocné knihovny pro Ruby pro volání rozhraní REST API a vykreslování TwiML.</span><span class="sxs-lookup"><span data-stu-id="4ae64-179">Key concepts include using the Twilio helper library for Ruby to make REST API calls and rendering TwiML.</span></span> <span data-ttu-id="4ae64-180">Dosaďte svoje hodnoty **z** a **k** telefonních čísel a ujistěte se, abyste ověřili **z** telefonní číslo pro svůj účet Twilio před spuštěním kódu.</span><span class="sxs-lookup"><span data-stu-id="4ae64-180">Substitute your values for the **From** and **To** phone numbers, and ensure that you verify the **From** phone number for your Twilio account prior to running the code.</span></span>

<span data-ttu-id="4ae64-181">Přidejte tuto funkci k `web.md`:</span><span class="sxs-lookup"><span data-stu-id="4ae64-181">Add this function to `web.md`:</span></span>

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";

    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);

      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end

<span data-ttu-id="4ae64-182">Pokud otevřete až `http://yourdomain.cloudapp.net/make_call` v prohlížeči, který aktivuje volání rozhraní API Twilio telefonní hovor.</span><span class="sxs-lookup"><span data-stu-id="4ae64-182">If you open-up `http://yourdomain.cloudapp.net/make_call` in a browser, that will trigger the call to the Twilio API to make the phone call.</span></span> <span data-ttu-id="4ae64-183">První dva parametry v `client.account.calls.create` poměrně není potřeba vysvětlovat: počet volání je `from` a počet volání je `to`.</span><span class="sxs-lookup"><span data-stu-id="4ae64-183">The first two parameters in `client.account.calls.create` are fairly self-explanatory: the number the call is `from` and the number the call is `to`.</span></span> 

<span data-ttu-id="4ae64-184">Třetí parametr (`url`) je adresu URL, kterou chcete-li získat pokyny k tomu, co udělat po připojení volání požadavky Twilio.</span><span class="sxs-lookup"><span data-stu-id="4ae64-184">The third parameter (`url`) is the URL that Twilio requests to get instructions on what to do once the call is connected.</span></span> <span data-ttu-id="4ae64-185">V tomto případě jsme nastavení a adresy URL (`http://yourdomain.cloudapp.net`), vrátí jednoduché TwiML dokument a používá `<Say>` příkaz dělat některé převod textu na řeč a uvést "Hello opic", aby osoba, která přijímá volání.</span><span class="sxs-lookup"><span data-stu-id="4ae64-185">In this case we set-up a URL (`http://yourdomain.cloudapp.net`) that returns a simple TwiML document and uses the `<Say>` verb to do some text-to-speech and say "Hello Monkey" to the person recieving the call.</span></span>

## <span data-ttu-id="4ae64-186"><a id="howto_recieve_sms"></a>Postupy: Receive zpráva SMS</span><span class="sxs-lookup"><span data-stu-id="4ae64-186"><a id="howto_recieve_sms"></a>How to: Recieve an SMS message</span></span>
<span data-ttu-id="4ae64-187">V předchozím příkladu jsme spustili **odchozí** telefonního hovoru.</span><span class="sxs-lookup"><span data-stu-id="4ae64-187">In the previous example we initiated an **outgoing** phone call.</span></span> <span data-ttu-id="4ae64-188">Tento čas, použijeme telefonní číslo, které jste nám během Twilio dali procesu registrace **příchozí** zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="4ae64-188">This time, let's use the phone number that Twilio gave us during sign-up to process an **incoming** SMS message.</span></span>

<span data-ttu-id="4ae64-189">První, přihlaste se k vaší [řídicí panel Twilio][twilio_account].</span><span class="sxs-lookup"><span data-stu-id="4ae64-189">First, log-in to your [Twilio dashboard][twilio_account].</span></span> <span data-ttu-id="4ae64-190">Klikněte na "Čísla" v horním navigaci a potom klikněte na číslo Twilio, byla zadaná.</span><span class="sxs-lookup"><span data-stu-id="4ae64-190">Click on "Numbers" in the top nav and then click on the Twilio number you were provided.</span></span> <span data-ttu-id="4ae64-191">Zobrazí se dvou adres URL, která se dají konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="4ae64-191">You'll see two URLs that you can configure.</span></span> <span data-ttu-id="4ae64-192">Adresa URL požadavku na adrese URL žádosti hlasu a serveru služby SMS.</span><span class="sxs-lookup"><span data-stu-id="4ae64-192">A Voice Request URL and an SMS Request URL.</span></span> <span data-ttu-id="4ae64-193">Tyto jsou adresy URL, které volá Twilio pokaždé, když se provádí telefonní hovor nebo serveru služby SMS je odeslaný na vaše číslo.</span><span class="sxs-lookup"><span data-stu-id="4ae64-193">These are the URLs that Twilio calls whenever a phone call is made or an SMS is sent to your number.</span></span> <span data-ttu-id="4ae64-194">Adresy URL se také označuje jako "web háky".</span><span class="sxs-lookup"><span data-stu-id="4ae64-194">The URLs are also known as "web hooks".</span></span>

<span data-ttu-id="4ae64-195">Rádi bychom se zpracovává příchozí zprávy SMS, tak umožňuje aktualizovat adresu URL `http://yourdomain.cloudapp.net/sms_url`.</span><span class="sxs-lookup"><span data-stu-id="4ae64-195">We would like to process incoming SMS messages, so let's update the URL to `http://yourdomain.cloudapp.net/sms_url`.</span></span> <span data-ttu-id="4ae64-196">Pokračujte a klikněte na tlačítko Uložit změny v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="4ae64-196">Go ahead and click Save Changes at the bottom of the page.</span></span> <span data-ttu-id="4ae64-197">Nyní, zpět na `web.rb` umožňuje programu naše aplikace pro zpracování toto:</span><span class="sxs-lookup"><span data-stu-id="4ae64-197">Now, back in `web.rb` let's program our application to handle this:</span></span>

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

<span data-ttu-id="4ae64-198">Po provedení změny, zajistěte, aby na restartování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ae64-198">After making the change, make sure to re-start your web app.</span></span> <span data-ttu-id="4ae64-199">Teď vyjměte telefonu a odeslat zprávu SMS na vaše číslo Twilio.</span><span class="sxs-lookup"><span data-stu-id="4ae64-199">Now, take out your phone and send an SMS to your Twilio number.</span></span> <span data-ttu-id="4ae64-200">Měli byste rychle získat odpověď serveru SMS, která říká "blogu Hey, Děkujeme za příkazem ping!</span><span class="sxs-lookup"><span data-stu-id="4ae64-200">You should promptly get an SMS response that says "Hey, thanks for the ping!</span></span> <span data-ttu-id="4ae64-201">Twilio a Azure rock! ".</span><span class="sxs-lookup"><span data-stu-id="4ae64-201">Twilio and Azure rock!".</span></span>

## <span data-ttu-id="4ae64-202"><a id="additional_services"></a>Postupy: použití další Twilio služeb</span><span class="sxs-lookup"><span data-stu-id="4ae64-202"><a id="additional_services"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="4ae64-203">Kromě příkladů tady uvedené nabízí Twilio rozhraní API založené na webu, který můžete využít další funkce Twilio vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="4ae64-203">In addition to the examples shown here, Twilio offers web-based APIs that you can use to leverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="4ae64-204">Úplné podrobnosti najdete v tématu [dokumentaci k rozhraní API Twilio][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="4ae64-204">For full details, see the [Twilio API documentation][twilio_api_documentation].</span></span>

### <span data-ttu-id="4ae64-205"><a id="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="4ae64-205"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="4ae64-206">Teď, když jste se naučili základy služby Twilio, postupujte podle následujících odkazech na další informace:</span><span class="sxs-lookup"><span data-stu-id="4ae64-206">Now that you've learned the basics of the Twilio service, follow these links to learn more:</span></span>

* <span data-ttu-id="4ae64-207">[Pokyny pro zabezpečení Twilio][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="4ae64-207">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="4ae64-208">[Twilio HowTos a ukázkový kód][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="4ae64-208">[Twilio HowTos and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="4ae64-209">[Kurzy Twilio rychlý start][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="4ae64-209">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span> 
* <span data-ttu-id="4ae64-210">[Twilio na Githubu][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="4ae64-210">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="4ae64-211">[Obraťte se na podporu Twilio][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="4ae64-211">[Talk to Twilio Support][twilio_support]</span></span>

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
