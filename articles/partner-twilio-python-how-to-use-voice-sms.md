---
title: "aaaHow tooUse Twilio pro hlasový a serveru SMS (Python) | Microsoft Docs"
description: "Zjistěte, jak toomake telefonní hovor a odesílání SMS zprávy službou hello Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořené v Pythonu."
services: 
documentationcenter: python
author: devinrader
manager: twilio
editor: 
ms.assetid: 561bc75b-4ac4-40ba-bcba-48e901f27cc3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: 1f16a107d95c94589b8d61a0971c5b62d639c571
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-python"></a><span data-ttu-id="50a08-104">Jak tooUse Twilio pro hlasový a možnosti serveru SMS v Pythonu</span><span class="sxs-lookup"><span data-stu-id="50a08-104">How tooUse Twilio for Voice and SMS Capabilities in Python</span></span>
<span data-ttu-id="50a08-105">Tato příručka ukazuje, jak služba tooperform běžné úlohy programování s hello Twilio rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="50a08-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="50a08-106">pokryté scénáře Hello zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS).</span><span class="sxs-lookup"><span data-stu-id="50a08-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="50a08-107">Další informace o Twilio a použití hlasu a SMS ve svých aplikacích najdete v tématu hello [další kroky](#NextSteps) části.</span><span class="sxs-lookup"><span data-stu-id="50a08-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="50a08-108"><a id="WhatIs"></a>Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="50a08-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="50a08-109">Twilio je pohánějící budoucí hello firmy komunikace, povolení vývojáři tooembed hlasu VoIP a zasílání zpráv do aplikace.</span><span class="sxs-lookup"><span data-stu-id="50a08-109">Twilio is powering hello future of business communications, enabling developers tooembed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="50a08-110">Jejich Virtualizovat všechny infrastrukturu potřebnou v prostředí cloudu, globální vystavení prostřednictvím platformy hello Twilio komunikace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="50a08-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through hello Twilio communications API platform.</span></span> <span data-ttu-id="50a08-111">Aplikace jsou jednoduché toobuild a škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="50a08-111">Applications are simple toobuild and scalable.</span></span> <span data-ttu-id="50a08-112">Získejte flexibilitu s platím jako vámi přejděte ceny a těžit z cloudu spolehlivost.</span><span class="sxs-lookup"><span data-stu-id="50a08-112">Enjoy flexibility with pay-as-you go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="50a08-113">**Hlasové Twilio** umožňuje toomake vaší aplikace a příjem telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="50a08-113">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span>
<span data-ttu-id="50a08-114">**Twilio SMS** umožňuje toosend vaší aplikace a přijímat textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="50a08-114">**Twilio SMS** enables your application toosend and receive text messages.</span></span>
<span data-ttu-id="50a08-115">**Klient Twilio** vám umožní toomake VoIP volání z jakékoli telefon, tablet nebo prohlížeče a podporuje WebRTC.</span><span class="sxs-lookup"><span data-stu-id="50a08-115">**Twilio Client** allows you toomake VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="50a08-116"><a id="Pricing"></a>Ceny Twilio a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="50a08-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="50a08-117">Přijímat Azure zákazníků [speciální nabídka] [ special_offer] 10 Twilio kreditu při upgradu vašeho účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="50a08-117">Azure customers receive a [special offer][special_offer] $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="50a08-118">Tato Twilio platební můžou být použité tooany Twilio využití (10 platební ekvivalentní toosending až 1 000 zpráv serveru SMS nebo příjem až too1000 příchozí hlasové minut v závislosti na umístění hello vaše telefonní číslo a zpráva nebo volání cíle).</span><span class="sxs-lookup"><span data-stu-id="50a08-118">This Twilio Credit can be applied tooany Twilio usage ($10 credit equivalent toosending as many as 1,000 SMS messages or receiving up too1000 inbound Voice minutes, depending on hello location of your phone number and message or call destination).</span></span> <span data-ttu-id="50a08-119">To uplatnit [Twilio platební] [ special_offer] a začít.</span><span class="sxs-lookup"><span data-stu-id="50a08-119">Redeem this [Twilio credit][special_offer] and get started.</span></span>

<span data-ttu-id="50a08-120">Twilio je služba, průběžnými platbami.</span><span class="sxs-lookup"><span data-stu-id="50a08-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="50a08-121">Neexistují žádné poplatky nastavení a kdykoli můžete zavřít svůj účet.</span><span class="sxs-lookup"><span data-stu-id="50a08-121">There are no set-up fees and you can close your account at any time.</span></span> <span data-ttu-id="50a08-122">Můžete najít další podrobnosti o [Twilio ceny][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="50a08-122">You can find more details at [Twilio Pricing][twilio_pricing].</span></span>

## <span data-ttu-id="50a08-123"><a id="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="50a08-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="50a08-124">Hello Twilio rozhraní API je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="50a08-124">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="50a08-125">Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="50a08-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="50a08-126">Klíčové aspekty hello Twilio API jsou příkazy Twilio a Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="50a08-126">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="50a08-127"><a id="Verbs"></a>Příkazy Twilio</span><span class="sxs-lookup"><span data-stu-id="50a08-127"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="50a08-128">Hello API využívá Twilio příkazy; například hello  **&lt;indikované&gt;**  příkaz dá pokyn Twilio tooaudibly doručit zprávu na volání.</span><span class="sxs-lookup"><span data-stu-id="50a08-128">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="50a08-129">Hello následuje seznam operací Twilio.</span><span class="sxs-lookup"><span data-stu-id="50a08-129">hello following is a list of Twilio verbs.</span></span> <span data-ttu-id="50a08-130">Další informace o hello jiné příkazy a možnosti prostřednictvím [Twilio Markup Language dokumentaci][twiml].</span><span class="sxs-lookup"><span data-stu-id="50a08-130">Learn about hello other verbs and capabilities via [Twilio Markup Language documentation][twiml].</span></span>

* <span data-ttu-id="50a08-131">**&lt;Vytočit&gt;**: připojí hello volající tooanother phone.</span><span class="sxs-lookup"><span data-stu-id="50a08-131">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="50a08-132">**&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu hello.</span><span class="sxs-lookup"><span data-stu-id="50a08-132">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="50a08-133">**&lt;Zavěšení&gt;**: ukončí volání.</span><span class="sxs-lookup"><span data-stu-id="50a08-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="50a08-134">**&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.</span><span class="sxs-lookup"><span data-stu-id="50a08-134">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="50a08-135">**&lt;Přehrání&gt;**: přehraje zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="50a08-135">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="50a08-136">**&lt;Fronty&gt;**: přidejte hello tooa frontu volající.</span><span class="sxs-lookup"><span data-stu-id="50a08-136">**&lt;Queue&gt;**: Add hello tooa queue of callers.</span></span>
* <span data-ttu-id="50a08-137">**&lt;Záznam&gt;**: zaznamenává hello hlasové hello volajícího a vrátí adresu URL souboru, který obsahuje záznam hello.</span><span class="sxs-lookup"><span data-stu-id="50a08-137">**&lt;Record&gt;**: Records hello voice of hello caller and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="50a08-138">**&lt;Přesměrování&gt;**: přenáší kontrolu nad telefonického hovoru nebo SMS toohello TwiML na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="50a08-138">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="50a08-139">**&lt;Odmítnout&gt;**: odmítne na příchozí volání bez fakturace můžete tooyour Twilio číslo.</span><span class="sxs-lookup"><span data-stu-id="50a08-139">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you.</span></span>
* <span data-ttu-id="50a08-140">**&lt;Řekněme&gt;**: toospeech převede text, že z volání.</span><span class="sxs-lookup"><span data-stu-id="50a08-140">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="50a08-141">**&lt;SMS&gt;**: odešle zprávu SMS.</span><span class="sxs-lookup"><span data-stu-id="50a08-141">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="50a08-142"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="50a08-142"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="50a08-143">TwiML je sada založený na jazyce XML pokyny podle hello Twilio příkazy, které informovat o tom, jak Twilio tooprocess hovoru nebo SMS.</span><span class="sxs-lookup"><span data-stu-id="50a08-143">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="50a08-144">Jako příklad by hello následující TwiML převést hello text **Hello, World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="50a08-144">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

<span data-ttu-id="50a08-145">Pokud vaše aplikace volání hello Twilio rozhraní API, jeden z parametrů hello rozhraní API je hello adresu URL, který vrátí odpověď TwiML hello.</span><span class="sxs-lookup"><span data-stu-id="50a08-145">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="50a08-146">Pro účely vývoje můžete použít adresy URL zadané Twilio tooprovide hello TwiML odpovědí použít v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="50a08-146">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="50a08-147">Může taky hostovat vlastní adresy URL tooproduce hello TwiML odpovědí, a další možností je toouse hello `TwiMLResponse` objektu.</span><span class="sxs-lookup"><span data-stu-id="50a08-147">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello `TwiMLResponse` object.</span></span>

<span data-ttu-id="50a08-148">Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="50a08-148">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="50a08-149">Další informace o hello Twilio API najdete v tématu [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="50a08-149">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="50a08-150"><a id="CreateAccount"></a>Vytvoření účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="50a08-150"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="50a08-151">Po připravené tooget účtu Twilio zaregistrovat na [zkuste Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="50a08-151">When you are ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="50a08-152">Můžete začít s bezplatný účet a upgradujte si účet později.</span><span class="sxs-lookup"><span data-stu-id="50a08-152">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="50a08-153">Když zaregistrujete k účtu Twilio, zobrazí se účet SID a ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="50a08-153">When you sign up for a Twilio account, you receive an account SID and an authentication token.</span></span> <span data-ttu-id="50a08-154">Jak bude volání rozhraní API Twilio potřebné toomake.</span><span class="sxs-lookup"><span data-stu-id="50a08-154">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="50a08-155">tooprevent neoprávněný přístup k účtu tooyour, ověřovací token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="50a08-155">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="50a08-156">SID účtu a ověřovací token lze zobrazit v hello [Twilio konzoly][twilio_console]v hello pole s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="50a08-156">Your account SID and authentication token are viewable in hello [Twilio Console][twilio_console], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="50a08-157"><a id="create_app"></a>Vytvoření aplikace Python</span><span class="sxs-lookup"><span data-stu-id="50a08-157"><a id="create_app"></a>Create a Python Application</span></span>
<span data-ttu-id="50a08-158">Aplikace Python, která používá službu hello Twilio a běží v Azure je nejsou jiné než jiná aplikace Python, která používá službu Twilio hello.</span><span class="sxs-lookup"><span data-stu-id="50a08-158">A Python application that uses hello Twilio service and is running in Azure is no different than any other Python application that uses hello Twilio service.</span></span> <span data-ttu-id="50a08-159">Při Twilio services jsou založené na REST a lze volat z Pythonu několika způsoby, v tomto článku se soustředí na jak toouse Twilio služeb s [Twilio knihovny pro jazyk Python z webu GitHub][twilio_python].</span><span class="sxs-lookup"><span data-stu-id="50a08-159">While Twilio services are REST-based and can be called from Python in several ways, this article will focus on how toouse Twilio services with [Twilio library for Python from GitHub][twilio_python].</span></span> <span data-ttu-id="50a08-160">Další informace o používání knihovny hello Twilio pro jazyk Python najdete v tématu [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].</span><span class="sxs-lookup"><span data-stu-id="50a08-160">For more information about using hello Twilio library for Python, see [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].</span></span>

<span data-ttu-id="50a08-161">První s názvem [nastavení nového virtuálního počítače Azure Linux] [azure_vm_setup] tooact jako hostitel pro novou webovou aplikaci Python.</span><span class="sxs-lookup"><span data-stu-id="50a08-161">First, [set-up a new Azure Linux VM][azure_vm_setup] tooact as a host for your new Python web application.</span></span> <span data-ttu-id="50a08-162">Jakmile hello je spuštěný virtuální počítač, budete potřebovat tooexpose vaší aplikace na veřejném portu jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="50a08-162">Once hello Virtual Machine is running, you will need tooexpose your application on a public port as described below.</span></span>

### <a name="add-an-incoming-rule"></a><span data-ttu-id="50a08-163">Přidat příchozí pravidlo</span><span class="sxs-lookup"><span data-stu-id="50a08-163">Add An Incoming Rule</span></span>
  1. <span data-ttu-id="50a08-164">Přejděte toohello [skupinu zabezpečení sítě] [azure_nsg] stránky.</span><span class="sxs-lookup"><span data-stu-id="50a08-164">Go toohello [Network Security Group][azure_nsg] page.</span></span>
  2. <span data-ttu-id="50a08-165">Vyberte hello skupinu zabezpečení sítě odpovídající s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="50a08-165">Select hello Network Security Group that corresponds with your Virtual Machine.</span></span>
  3. <span data-ttu-id="50a08-166">Přidat a **odchozí pravidlo** pro **port 80**.</span><span class="sxs-lookup"><span data-stu-id="50a08-166">Add and **Outgoing Rule** for **port 80**.</span></span> <span data-ttu-id="50a08-167">Být jisti tooallow příchozí z libovolné adresy.</span><span class="sxs-lookup"><span data-stu-id="50a08-167">Be sure tooallow incoming from any address.</span></span>

### <a name="set-hello-dns-name-label"></a><span data-ttu-id="50a08-168">Nastavit hello Popisek názvu DNS</span><span class="sxs-lookup"><span data-stu-id="50a08-168">Set hello DNS Name Label</span></span>
  1. <span data-ttu-id="50a08-169">Přejděte toohello [hello veřejné IP adresy] [azure_ips] stránky.</span><span class="sxs-lookup"><span data-stu-id="50a08-169">Go toohello [hello Public IP Adresses][azure_ips] page.</span></span>
  2. <span data-ttu-id="50a08-170">Vyberte hello veřejná IP adresa tohoto correspends s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="50a08-170">Select hello Public IP that correspends with your Virtual Machine.</span></span>
  3. <span data-ttu-id="50a08-171">Sada hello **Popisek názvu DNS** v hello **konfigurace** části.</span><span class="sxs-lookup"><span data-stu-id="50a08-171">Set hello **DNS Name Label** in hello **Configuration** section.</span></span> <span data-ttu-id="50a08-172">V případě hello tohoto příkladu bude vypadat přibližně takto *vaše popisek domény*. centralus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="50a08-172">In hello case of this example it will look something like this *your-domain-label*.centralus.cloudapp.azure.com</span></span>

<span data-ttu-id="50a08-173">Jakmile se vám daří tooconnect prostřednictvím SSH toohello virtuálního počítače můžete nainstalovat hello webová architektura podle vašeho výběru (hello nejvíce známých v Pythonu ve dvou [Flask](http://flask.pocoo.org/) a [Django](https://www.djangoproject.com)).</span><span class="sxs-lookup"><span data-stu-id="50a08-173">Once you are able tooconnect through SSH toohello Virtual Machine you can install hello Web Framework of your choice (hello two most well known in Python being [Flask](http://flask.pocoo.org/) and [Django](https://www.djangoproject.com)).</span></span> <span data-ttu-id="50a08-174">Můžete nainstalovat buď z nich právě spuštěním hello `pip install` příkaz.</span><span class="sxs-lookup"><span data-stu-id="50a08-174">You can install either of them just by running hello `pip install` command.</span></span>

<span data-ttu-id="50a08-175">Uvědomte si, že jsme nakonfigurovali přenosy tooallow virtuálních počítačů hello pouze na portu 80.</span><span class="sxs-lookup"><span data-stu-id="50a08-175">Keep in mind that we configured hello Virtual Machine tooallow traffic only on port 80.</span></span> <span data-ttu-id="50a08-176">Proto ujistěte se, že toouse aplikace hello tooconfigure tento port.</span><span class="sxs-lookup"><span data-stu-id="50a08-176">So make sure tooconfigure hello application toouse this port.</span></span>

## <span data-ttu-id="50a08-177"><a id="configure_app"></a>Konfigurace knihovny Twilio tooUse vaše aplikace</span><span class="sxs-lookup"><span data-stu-id="50a08-177"><a id="configure_app"></a>Configure Your Application tooUse Twilio Libraries</span></span>
<span data-ttu-id="50a08-178">Vaše aplikace toouse hello Twilio knihovna pro jazyk Python můžete nakonfigurovat dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="50a08-178">You can configure your application toouse hello Twilio library for Python in two ways:</span></span>

* <span data-ttu-id="50a08-179">Jako balíček Pip nainstalujte hello Twilio knihovny pro jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="50a08-179">Install hello Twilio library for Python as a Pip package.</span></span> <span data-ttu-id="50a08-180">Dají se instalovat s hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="50a08-180">It can be installed with hello following commands:</span></span>
   
        $ pip install twilio

    <span data-ttu-id="50a08-181">- nebo -</span><span class="sxs-lookup"><span data-stu-id="50a08-181">-OR-</span></span>

* <span data-ttu-id="50a08-182">Stáhnout hello Twilio knihovny pro jazyk Python z webu GitHub ([https://github.com/twilio/twilio-python][twilio_python]) a nainstalujte ho takto:</span><span class="sxs-lookup"><span data-stu-id="50a08-182">Download hello Twilio library for Python from GitHub ([https://github.com/twilio/twilio-python][twilio_python]) and install it like this:</span></span>

        $ python setup.py install

<span data-ttu-id="50a08-183">Po instalaci hello Twilio knihovny pro jazyk Python, můžete pak `import` ho v souborech Python:</span><span class="sxs-lookup"><span data-stu-id="50a08-183">Once you have installed hello Twilio library for Python, you can then `import` it in your Python files:</span></span>

        import twilio

<span data-ttu-id="50a08-184">Další informace najdete v tématu [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].</span><span class="sxs-lookup"><span data-stu-id="50a08-184">For more information, see [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].</span></span>

## <span data-ttu-id="50a08-185"><a id="howto_make_call"></a>Postupy: volání odchozí</span><span class="sxs-lookup"><span data-stu-id="50a08-185"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="50a08-186">Hello následující ukazuje, jak volat toomake odchozí.</span><span class="sxs-lookup"><span data-stu-id="50a08-186">hello following shows how toomake an outgoing call.</span></span> <span data-ttu-id="50a08-187">Tento kód také používá zadaný Twilio lokality tooreturn hello odpovědi Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="50a08-187">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="50a08-188">Dosaďte svoje hodnoty hello **from_number** a **to_number** telefonních čísel a ujistěte se, že ověříte hello **from_number** telefonní číslo pro svůj účet Twilio Před spuštěním kódu hello.</span><span class="sxs-lookup"><span data-stu-id="50a08-188">Substitute your values for hello **from_number** and **to_number** phone numbers, and ensure that you've verified hello **from_number** phone number for your Twilio account before running hello code.</span></span>

    from urllib.parse import urlencode

    # Import hello Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    # hello number of hello phone initiating hello hello call.
    # This should either be a Twilio number or a number that you've verified
    from_number = "NNNNNNNNNNN"

    # hello number of hello phone receiving call.
    to_number = "NNNNNNNNNNN"

    # Use hello Twilio-provided site for hello TwiML response.
    url = "http://twimlets.com/message?"

    # hello phone message text.
    message = "Hello world."

    # Initialize hello Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make hello call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url + urlencode({'Message': message}))
    print(call.sid)

<span data-ttu-id="50a08-189">Jak je uvedeno, tento kód používá zadaný Twilio lokality tooreturn hello TwiML odpovědi.</span><span class="sxs-lookup"><span data-stu-id="50a08-189">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="50a08-190">Místo toho můžete použít vlastní lokality tooprovide hello TwiML odpovědi; Další informace najdete v tématu [jak tooProvide TwiML odpovědí z vaše vlastní webové stránky](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="50a08-190">You could instead use your own site tooprovide hello TwiML response; for more information, see [How tooProvide TwiML Responses from Your Own Web Site](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="50a08-191"><a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS</span><span class="sxs-lookup"><span data-stu-id="50a08-191"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="50a08-192">Hello následující ukazuje, jak toosend zprávu SMS pomocí hello `TwilioRestClient` třídy.</span><span class="sxs-lookup"><span data-stu-id="50a08-192">hello following shows how toosend an SMS message using hello `TwilioRestClient` class.</span></span> <span data-ttu-id="50a08-193">Hello **from_number** číslo poskytne Twilio pro zkušební verzi účty toosend zpráv serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="50a08-193">hello **from_number** number is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="50a08-194">Hello **to_number** číslo musí ověřit účtu Twilio před spuštěním kódu hello.</span><span class="sxs-lookup"><span data-stu-id="50a08-194">hello **to_number** number must be verified for your Twilio account before running hello code.</span></span>

    # Import hello Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    from_number = "NNNNNNNNNNN"  # With trial account, texts can only be sent from your Twilio number.
    to_number = "NNNNNNNNNNN"
    message = "Hello world."

    # Initialize hello Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Send hello SMS message.
    message = client.messages.create(to=to_number,
                                     from_=from_number,
                                     body=message)

## <span data-ttu-id="50a08-195"><a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu</span><span class="sxs-lookup"><span data-stu-id="50a08-195"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="50a08-196">Pokud vaše aplikace zahájí toohello volání rozhraní API Twilio, budou odeslány Twilio tooa adresu URL vašeho požadavku, která je očekáván tooreturn TwiML odpověď.</span><span class="sxs-lookup"><span data-stu-id="50a08-196">When your application initiates a call toohello Twilio API, Twilio will send your request tooa URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="50a08-197">výše uvedený příklad Hello používá URL poskytnutou Twilio hello [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="50a08-197">hello example above uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="50a08-198">(Při TwiML je určen pro Twilio, můžete ji zobrazit v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="50a08-198">(While TwiML is designed for use by Twilio, you can view it in your browser.</span></span> <span data-ttu-id="50a08-199">Klikněte například na [http://twimlets.com/message] [ twimlet_message_url] toosee prázdnou `<Response>` element; například klikněte na tlačítko [http://twimlets.com/message? Zprávy % 5B0 %5 D = Hello % 20World] [ twimlet_message_url_hello_world] toosee `<Response>` elementu, který obsahuje `<Say>` elementu.)</span><span class="sxs-lookup"><span data-stu-id="50a08-199">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty `<Response>` element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] toosee a `<Response>` element that contains a `<Say>` element.)</span></span>

<span data-ttu-id="50a08-200">Aniž byste museli spoléhat na URL poskytnutou Twilio hello, můžete vytvořit vlastního webu, který vrací odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="50a08-200">Instead of relying on hello Twilio-provided URL, you can create your own site that returns HTTP responses.</span></span> <span data-ttu-id="50a08-201">Můžete vytvořit hello lokality v libovolném jazyce, který vrací odpovědí XML; Toto téma předpokládá, že se používá Python toocreate hello TwiML.</span><span class="sxs-lookup"><span data-stu-id="50a08-201">You can create hello site in any language that returns XML responses; this topic assumes you will be using Python toocreate hello TwiML.</span></span>

<span data-ttu-id="50a08-202">Hello následující příklady výstup TwiML odpovědi, která uvádí, že **Hello, World** při volání hello.</span><span class="sxs-lookup"><span data-stu-id="50a08-202">hello following examples will output a TwiML response that says **Hello World** on hello call.</span></span>

<span data-ttu-id="50a08-203">S Flask:</span><span class="sxs-lookup"><span data-stu-id="50a08-203">With Flask:</span></span>

    from flask import Response
    @app.route("/")
    def hello():
        xml = '<Response><Say>Hello world.</Say></Response>'
        return Response(xml, mimetype='text/xml')

<span data-ttu-id="50a08-204">Pomocí rozhraní Django:</span><span class="sxs-lookup"><span data-stu-id="50a08-204">With Django:</span></span>

    from django.http import HttpResponse
    def hello(request):
        xml = '<Response><Say>Hello world.</Say></Response>'
        return HttpResponse(xml, content_type='text/xml')

<span data-ttu-id="50a08-205">Jak je vidět z výše uvedeném příkladu hello hello TwiML odpovědi je jednoduše dokument XML.</span><span class="sxs-lookup"><span data-stu-id="50a08-205">As you can see from hello example above, hello TwiML response is simply an XML document.</span></span> <span data-ttu-id="50a08-206">Knihovna Hello Twilio pro jazyk Python obsahuje třídy, které způsobí vygenerování TwiML za vás.</span><span class="sxs-lookup"><span data-stu-id="50a08-206">hello Twilio library for Python contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="50a08-207">Hello následující příklad vytvoří ekvivalentní odpovědi hello jako v příkladu nahoře, ale používá hello `twiml` modulu v hello knihovně Twilio pro jazyk Python:</span><span class="sxs-lookup"><span data-stu-id="50a08-207">hello example below produces hello equivalent response as shown above, but uses hello `twiml` module in hello Twilio library for Python:</span></span>

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

<span data-ttu-id="50a08-208">Další informace o TwiML najdete v tématu [https://www.twilio.com/docs/api/twiml][twiml_reference].</span><span class="sxs-lookup"><span data-stu-id="50a08-208">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml][twiml_reference].</span></span>

<span data-ttu-id="50a08-209">Až budete mít aplikaci Python nastavit tooprovide TwiML odpovědi, použijte adresu URL hello hello aplikace jako adresa URL předaná do hello hello `client.calls.create` metoda.</span><span class="sxs-lookup"><span data-stu-id="50a08-209">Once you have your Python application set up tooprovide TwiML responses, use hello URL of hello application as hello URL passed into hello `client.calls.create`  method.</span></span> <span data-ttu-id="50a08-210">Například, pokud máte webovou aplikaci s názvem **MyTwiML** nasazené tooan Azure hostovanou službu, můžete použít jeho adresa url jako webhooku jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="50a08-210">For example, if you have a Web application named **MyTwiML** deployed tooan Azure hosted service, you can use its url as webhook as shown in hello following example:</span></span>

    from twilio.rest import TwilioRestClient

    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"
    from_number = "NNNNNNNNNNN"
    to_number = "NNNNNNNNNNN"
    url = "http://your-domain-label.centralus.cloudapp.azure.com/MyTwiML/"

    # Initialize hello Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make hello call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url)
    print(call.sid)

## <span data-ttu-id="50a08-211"><a id="AdditionalServices"></a>Postupy: použití další Twilio služeb</span><span class="sxs-lookup"><span data-stu-id="50a08-211"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="50a08-212">Kromě toho toohello příklady zobrazeny zde, že twilio nabízí webové rozhraní API, které můžete použít další funkce Twilio tooleverage z vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="50a08-212">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="50a08-213">Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API Twilio][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="50a08-213">For full details, see hello [Twilio API documentation][twilio_api].</span></span>

## <span data-ttu-id="50a08-214"><a id="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="50a08-214"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="50a08-215">Teď, když jste se naučili základy hello hello Twilio služby, použijte tyto odkazy toolearn Další:</span><span class="sxs-lookup"><span data-stu-id="50a08-215">Now that you have learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="50a08-216">[Pokyny pro zabezpečení Twilio][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="50a08-216">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="50a08-217">[Příručky Twilio postupy a příklady kódu][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="50a08-217">[Twilio HowTo Guides and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="50a08-218">[Kurzy Twilio rychlý start][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="50a08-218">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span>
* <span data-ttu-id="50a08-219">[Twilio na Githubu][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="50a08-219">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="50a08-220">[Komunikovat tooTwilio podpory][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="50a08-220">[Talk tooTwilio Support][twilio_support]</span></span>

[special_offer]: http://ahoy.twilio.com/azure
[twilio_python]: https://github.com/twilio/twilio-python
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-python/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-python/blob/master/README.md

[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
