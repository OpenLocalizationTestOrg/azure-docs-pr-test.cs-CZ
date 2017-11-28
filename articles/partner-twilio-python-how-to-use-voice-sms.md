---
title: "Jak používat Twilio pro hlasový a serveru SMS (Python) | Microsoft Docs"
description: "Naučte se telefonní hovor a odeslání zprávy SMS službou Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořené v Pythonu."
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
ms.openlocfilehash: f4a02bb7a7c46e7a0e3c75b870c522eae8294339
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-python"></a><span data-ttu-id="a0d9b-104">Jak používat Twilio pro hlasový a možnosti serveru SMS v Pythonu</span><span class="sxs-lookup"><span data-stu-id="a0d9b-104">How to Use Twilio for Voice and SMS Capabilities in Python</span></span>
<span data-ttu-id="a0d9b-105">Tato příručka ukazuje, jak provádět běžné úkoly programování službou Twilio rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-105">This guide demonstrates how to perform common programming tasks with the Twilio API service on Azure.</span></span> <span data-ttu-id="a0d9b-106">Pokryté scénáře zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS).</span><span class="sxs-lookup"><span data-stu-id="a0d9b-106">The scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="a0d9b-107">Další informace o Twilio a používání hlasového a SMS ve svých aplikacích najdete v tématu [další kroky](#NextSteps) části.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-107">For more information on Twilio and using voice and SMS in your applications, see the [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="a0d9b-108"><a id="WhatIs"></a>Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="a0d9b-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="a0d9b-109">Twilio je pohánějící budoucí obchodní komunikaci, umožňuje vývojářům pro vložení hlasové, VoIP a zasílání zpráv do aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-109">Twilio is powering the future of business communications, enabling developers to embed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="a0d9b-110">Jejich Virtualizovat všechny infrastrukturu potřebnou v prostředí cloudu, globální vystavení prostřednictvím platformy komunikace rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through the Twilio communications API platform.</span></span> <span data-ttu-id="a0d9b-111">Aplikace jsou jednoduché k sestavení a škálovatelné.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-111">Applications are simple to build and scalable.</span></span> <span data-ttu-id="a0d9b-112">Získejte flexibilitu s platím jako vámi přejděte ceny a těžit z cloudu spolehlivost.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-112">Enjoy flexibility with pay-as-you go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="a0d9b-113">**Hlasové Twilio** umožňuje aplikacím zkontrolujte a příjem telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-113">**Twilio Voice** allows your applications to make and receive phone calls.</span></span>
<span data-ttu-id="a0d9b-114">**Twilio SMS** umožňuje aplikaci posílat a přijímat textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-114">**Twilio SMS** enables your application to send and receive text messages.</span></span>
<span data-ttu-id="a0d9b-115">**Klient Twilio** umožňuje provádět volání VoIP z jakékoli telefon, tablet nebo prohlížeče a podporuje WebRTC.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-115">**Twilio Client** allows you to make VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="a0d9b-116"><a id="Pricing"></a>Ceny Twilio a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="a0d9b-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="a0d9b-117">Přijímat Azure zákazníků [speciální nabídka] [ special_offer] 10 Twilio kreditu při upgradu vašeho účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-117">Azure customers receive a [special offer][special_offer] $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="a0d9b-118">Tato Twilio platební je použít pro všechny Twilio využití (10 platební ekvivalentní až 1 000 zpráv serveru SMS odesílání nebo přijímání až 1 000 příchozí hlasové minut, v závislosti na umístění telefonní číslo a zpráva nebo volání cíl).</span><span class="sxs-lookup"><span data-stu-id="a0d9b-118">This Twilio Credit can be applied to any Twilio usage ($10 credit equivalent to sending as many as 1,000 SMS messages or receiving up to 1000 inbound Voice minutes, depending on the location of your phone number and message or call destination).</span></span> <span data-ttu-id="a0d9b-119">To uplatnit [Twilio platební] [ special_offer] a začít.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-119">Redeem this [Twilio credit][special_offer] and get started.</span></span>

<span data-ttu-id="a0d9b-120">Twilio je služba, průběžnými platbami.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="a0d9b-121">Neexistují žádné poplatky nastavení a kdykoli můžete zavřít svůj účet.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-121">There are no set-up fees and you can close your account at any time.</span></span> <span data-ttu-id="a0d9b-122">Můžete najít další podrobnosti o [Twilio ceny][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-122">You can find more details at [Twilio Pricing][twilio_pricing].</span></span>

## <span data-ttu-id="a0d9b-123"><a id="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="a0d9b-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="a0d9b-124">Rozhraní API Twilio je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-124">The Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="a0d9b-125">Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="a0d9b-126">Klíčové aspekty rozhraní API Twilio jsou příkazy Twilio a Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="a0d9b-126">Key aspects of the Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="a0d9b-127"><a id="Verbs"></a>Příkazy Twilio</span><span class="sxs-lookup"><span data-stu-id="a0d9b-127"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="a0d9b-128">Rozhraní API využívá Twilio příkazy; například  **&lt;indikované&gt;**  příkaz dá pokyn Twilio zpráva zvukově dodržujeme volání.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-128">The API makes use of Twilio verbs; for example, the **&lt;Say&gt;** verb instructs Twilio to audibly deliver a message on a call.</span></span>

<span data-ttu-id="a0d9b-129">Následuje seznam operací Twilio.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-129">The following is a list of Twilio verbs.</span></span> <span data-ttu-id="a0d9b-130">Další informace o jiné příkazy a možnosti prostřednictvím [Twilio Markup Language dokumentaci][twiml].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-130">Learn about the other verbs and capabilities via [Twilio Markup Language documentation][twiml].</span></span>

* <span data-ttu-id="a0d9b-131">**&lt;Vytočit&gt;**: připojí volající na jiný telefon.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-131">**&lt;Dial&gt;**: Connects the caller to another phone.</span></span>
* <span data-ttu-id="a0d9b-132">**&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-132">**&lt;Gather&gt;**: Collects numeric digits entered on the telephone keypad.</span></span>
* <span data-ttu-id="a0d9b-133">**&lt;Zavěšení&gt;**: ukončí volání.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="a0d9b-134">**&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-134">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="a0d9b-135">**&lt;Přehrání&gt;**: přehraje zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-135">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="a0d9b-136">**&lt;Fronty&gt;**: přidejte do fronty volající.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-136">**&lt;Queue&gt;**: Add the to a queue of callers.</span></span>
* <span data-ttu-id="a0d9b-137">**&lt;Záznam&gt;**: zaznamenává hlasu volajícího a vrátí adresu URL souboru, který obsahuje záznamu.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-137">**&lt;Record&gt;**: Records the voice of the caller and returns a URL of a file that contains the recording.</span></span>
* <span data-ttu-id="a0d9b-138">**&lt;Přesměrování&gt;**: předá řízení hovoru nebo SMS TwiML na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-138">**&lt;Redirect&gt;**: Transfers control of a call or SMS to the TwiML at a different URL.</span></span>
* <span data-ttu-id="a0d9b-139">**&lt;Odmítnout&gt;**: odmítne příchozí volání na vaše číslo Twilio bez fakturace můžete.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-139">**&lt;Reject&gt;**: Rejects an incoming call to your Twilio number without billing you.</span></span>
* <span data-ttu-id="a0d9b-140">**&lt;Řekněme&gt;**: Převod převede textu na řeč, že z volání.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-140">**&lt;Say&gt;**: Converts text to speech that is made on a call.</span></span>
* <span data-ttu-id="a0d9b-141">**&lt;SMS&gt;**: odešle zprávu SMS.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-141">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="a0d9b-142"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="a0d9b-142"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="a0d9b-143">TwiML je sada založený na jazyce XML pokyny podle Twilio příkazy, které informují Twilio postup zpracování hovoru nebo SMS.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-143">TwiML is a set of XML-based instructions based on the Twilio verbs that inform Twilio of how to process a call or SMS.</span></span>

<span data-ttu-id="a0d9b-144">Jako příklad následující TwiML by převést text **Hello, World** k rozpoznávání řeči.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-144">As an example, the following TwiML would convert the text **Hello World** to speech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

<span data-ttu-id="a0d9b-145">Pokud aplikace zavolá rozhraní API Twilio, jeden z parametrů rozhraní API je adresa URL, který vrátí odpověď TwiML.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-145">When your application calls the Twilio API, one of the API parameters is the URL that returns the TwiML response.</span></span> <span data-ttu-id="a0d9b-146">Pro účely vývoje URL můžete použít zadaný Twilio zajistit TwiML odpovědi použít v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-146">For development purposes, you can use Twilio-provided URLs to provide the TwiML responses used by your applications.</span></span> <span data-ttu-id="a0d9b-147">Může taky hostovat vlastní adresy URL k vytvoření odpovědi TwiML a další možností je použít `TwiMLResponse` objektu.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-147">You could also host your own URLs to produce the TwiML responses, and another option is to use the `TwiMLResponse` object.</span></span>

<span data-ttu-id="a0d9b-148">Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-148">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="a0d9b-149">Další informace o rozhraní API Twilio najdete v tématu [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-149">For additional information about the Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="a0d9b-150"><a id="CreateAccount"></a>Vytvoření účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="a0d9b-150"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="a0d9b-151">Když budete chtít získat účet Twilio, zaregistrujte si v [zkuste Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-151">When you are ready to get a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="a0d9b-152">Můžete začít s bezplatný účet a upgradujte si účet později.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-152">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="a0d9b-153">Když zaregistrujete k účtu Twilio, zobrazí se účet SID a ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-153">When you sign up for a Twilio account, you receive an account SID and an authentication token.</span></span> <span data-ttu-id="a0d9b-154">Jak bude potřeba k volání rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-154">Both will be needed to make Twilio API calls.</span></span> <span data-ttu-id="a0d9b-155">Aby se zabránilo neoprávněnému přístupu ke svému účtu, zabezpečit ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-155">To prevent unauthorized access to your account, keep your authentication token secure.</span></span> <span data-ttu-id="a0d9b-156">SID účtu a ověřovací token lze zobrazit v [Twilio konzoly][twilio_console], v polích s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-156">Your account SID and authentication token are viewable in the [Twilio Console][twilio_console], in the fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="a0d9b-157"><a id="create_app"></a>Vytvoření aplikace Python</span><span class="sxs-lookup"><span data-stu-id="a0d9b-157"><a id="create_app"></a>Create a Python Application</span></span>
<span data-ttu-id="a0d9b-158">Aplikace Python, která používá službu Twilio a běží v Azure je nejsou jiné než jiná aplikace Python, která používá službu Twilio.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-158">A Python application that uses the Twilio service and is running in Azure is no different than any other Python application that uses the Twilio service.</span></span> <span data-ttu-id="a0d9b-159">Při Twilio services jsou založené na REST a lze volat z Pythonu několika způsoby, v tomto článku se soustředí na použití služeb Twilio s [Twilio knihovny pro jazyk Python z webu GitHub][twilio_python].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-159">While Twilio services are REST-based and can be called from Python in several ways, this article will focus on how to use Twilio services with [Twilio library for Python from GitHub][twilio_python].</span></span> <span data-ttu-id="a0d9b-160">Další informace o používání knihovny Twilio pro jazyk Python najdete v tématu [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-160">For more information about using the Twilio library for Python, see [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].</span></span>

<span data-ttu-id="a0d9b-161">První s názvem [nastavení nového virtuálního počítače Azure Linux] [azure_vm_setup] tak, aby fungoval jako hostitel pro Python vaší nové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-161">First, [set-up a new Azure Linux VM][azure_vm_setup] to act as a host for your new Python web application.</span></span> <span data-ttu-id="a0d9b-162">Jakmile je virtuální počítač spuštěn, musíte se ke zveřejnění aplikace na veřejném portu, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-162">Once the Virtual Machine is running, you will need to expose your application on a public port as described below.</span></span>

### <a name="add-an-incoming-rule"></a><span data-ttu-id="a0d9b-163">Přidat příchozí pravidlo</span><span class="sxs-lookup"><span data-stu-id="a0d9b-163">Add An Incoming Rule</span></span>
  1. <span data-ttu-id="a0d9b-164">Přejděte na stránku [skupinu zabezpečení sítě] [azure_nsg].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-164">Go to the [Network Security Group][azure_nsg] page.</span></span>
  2. <span data-ttu-id="a0d9b-165">Vyberte skupinu zabezpečení sítě odpovídající s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-165">Select the Network Security Group that corresponds with your Virtual Machine.</span></span>
  3. <span data-ttu-id="a0d9b-166">Přidat a **odchozí pravidlo** pro **port 80**.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-166">Add and **Outgoing Rule** for **port 80**.</span></span> <span data-ttu-id="a0d9b-167">Ujistěte se, zda povolit příchozí z libovolné adresy.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-167">Be sure to allow incoming from any address.</span></span>

### <a name="set-the-dns-name-label"></a><span data-ttu-id="a0d9b-168">Nastavte Popisek názvu DNS</span><span class="sxs-lookup"><span data-stu-id="a0d9b-168">Set the DNS Name Label</span></span>
  1. <span data-ttu-id="a0d9b-169">Přejděte na stránku [The veřejné IP adresy] [azure_ips].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-169">Go to the [The Public IP Adresses][azure_ips] page.</span></span>
  2. <span data-ttu-id="a0d9b-170">Vyberte veřejnou IP adresu tohoto correspends s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-170">Select the Public IP that correspends with your Virtual Machine.</span></span>
  3. <span data-ttu-id="a0d9b-171">Nastavte **Popisek názvu DNS** v **konfigurace** části.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-171">Set the **DNS Name Label** in the **Configuration** section.</span></span> <span data-ttu-id="a0d9b-172">V případě tohoto příkladu bude vypadat přibližně takto *vaše popisek domény*. centralus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="a0d9b-172">In the case of this example it will look something like this *your-domain-label*.centralus.cloudapp.azure.com</span></span>

<span data-ttu-id="a0d9b-173">Jakmile budete moci připojit přes SSH k virtuálnímu počítači můžete nainstalovat architektury webů podle vašeho výběru (většina známých v Pythonu ve dvou [Flask](http://flask.pocoo.org/) a [Django](https://www.djangoproject.com)).</span><span class="sxs-lookup"><span data-stu-id="a0d9b-173">Once you are able to connect through SSH to the Virtual Machine you can install the Web Framework of your choice (the two most well known in Python being [Flask](http://flask.pocoo.org/) and [Django](https://www.djangoproject.com)).</span></span> <span data-ttu-id="a0d9b-174">Můžete nainstalovat buď z nich právě spuštěním `pip install` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-174">You can install either of them just by running the `pip install` command.</span></span>

<span data-ttu-id="a0d9b-175">Uvědomte si, že jsme nakonfigurovali virtuální počítač povolit přenos jenom na portu 80.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-175">Keep in mind that we configured the Virtual Machine to allow traffic only on port 80.</span></span> <span data-ttu-id="a0d9b-176">Proto nezapomeňte nakonfigurovat aplikaci, aby tento port použít.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-176">So make sure to configure the application to use this port.</span></span>

## <span data-ttu-id="a0d9b-177"><a id="configure_app"></a>Konfigurace aplikace k používání Twilio knihovny</span><span class="sxs-lookup"><span data-stu-id="a0d9b-177"><a id="configure_app"></a>Configure Your Application to Use Twilio Libraries</span></span>
<span data-ttu-id="a0d9b-178">Můžete nakonfigurovat aplikace na používání knihovny, Twilio pro jazyk Python dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="a0d9b-178">You can configure your application to use the Twilio library for Python in two ways:</span></span>

* <span data-ttu-id="a0d9b-179">Nainstalujte knihovnu Twilio pro jazyk Python jako balíček Pip.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-179">Install the Twilio library for Python as a Pip package.</span></span> <span data-ttu-id="a0d9b-180">Může se nainstalovat pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="a0d9b-180">It can be installed with the following commands:</span></span>
   
        $ pip install twilio

    <span data-ttu-id="a0d9b-181">- nebo -</span><span class="sxs-lookup"><span data-stu-id="a0d9b-181">-OR-</span></span>

* <span data-ttu-id="a0d9b-182">Stáhnout Twilio knihovny pro jazyk Python z webu GitHub ([https://github.com/twilio/twilio-python][twilio_python]) a nainstalujte ho takto:</span><span class="sxs-lookup"><span data-stu-id="a0d9b-182">Download the Twilio library for Python from GitHub ([https://github.com/twilio/twilio-python][twilio_python]) and install it like this:</span></span>

        $ python setup.py install

<span data-ttu-id="a0d9b-183">Po instalaci Twilio knihovny pro jazyk Python, můžete pak `import` ho v souborech Python:</span><span class="sxs-lookup"><span data-stu-id="a0d9b-183">Once you have installed the Twilio library for Python, you can then `import` it in your Python files:</span></span>

        import twilio

<span data-ttu-id="a0d9b-184">Další informace najdete v tématu [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-184">For more information, see [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].</span></span>

## <span data-ttu-id="a0d9b-185"><a id="howto_make_call"></a>Postupy: volání odchozí</span><span class="sxs-lookup"><span data-stu-id="a0d9b-185"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="a0d9b-186">Následující ukazuje, jak provést odchozí volání.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-186">The following shows how to make an outgoing call.</span></span> <span data-ttu-id="a0d9b-187">Tento kód také používá zadaný Twilio lokality k vrácení odpovědi Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="a0d9b-187">This code also uses a Twilio-provided site to return the Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="a0d9b-188">Dosaďte svoje hodnoty **from_number** a **to_number** telefonních čísel a ujistěte se, že ověříte **from_number** telefonní číslo pro svůj účet Twilio před spuštěním kódu.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-188">Substitute your values for the **from_number** and **to_number** phone numbers, and ensure that you've verified the **from_number** phone number for your Twilio account before running the code.</span></span>

    from urllib.parse import urlencode

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from_number = "NNNNNNNNNNN"

    # The number of the phone receiving call.
    to_number = "NNNNNNNNNNN"

    # Use the Twilio-provided site for the TwiML response.
    url = "http://twimlets.com/message?"

    # The phone message text.
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url + urlencode({'Message': message}))
    print(call.sid)

<span data-ttu-id="a0d9b-189">Jak je uvedeno, tento kód používá k vrácení odpovědi TwiML zadaný Twilio lokality.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-189">As mentioned, this code uses a Twilio-provided site to return the TwiML response.</span></span> <span data-ttu-id="a0d9b-190">Můžete místo toho použít vlastní funkční web zajistit TwiML odpovědi; Další informace najdete v tématu [jak poskytnout TwiML odpovědí z vaše vlastní webové stránky](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="a0d9b-190">You could instead use your own site to provide the TwiML response; for more information, see [How to Provide TwiML Responses from Your Own Web Site](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="a0d9b-191"><a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS</span><span class="sxs-lookup"><span data-stu-id="a0d9b-191"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="a0d9b-192">Následující ukazuje, jak odeslat zprávu SMS pomocí `TwilioRestClient` třídy.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-192">The following shows how to send an SMS message using the `TwilioRestClient` class.</span></span> <span data-ttu-id="a0d9b-193">**From_number** číslo poskytne Twilio pro zkušebními účty k odesílání zpráv serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-193">The **from_number** number is provided by Twilio for trial accounts to send SMS messages.</span></span> <span data-ttu-id="a0d9b-194">**To_number** číslo musí ověřit účtu Twilio před spuštěním kódu.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-194">The **to_number** number must be verified for your Twilio account before running the code.</span></span>

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    from_number = "NNNNNNNNNNN"  # With trial account, texts can only be sent from your Twilio number.
    to_number = "NNNNNNNNNNN"
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Send the SMS message.
    message = client.messages.create(to=to_number,
                                     from_=from_number,
                                     body=message)

## <span data-ttu-id="a0d9b-195"><a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu</span><span class="sxs-lookup"><span data-stu-id="a0d9b-195"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="a0d9b-196">Pokud vaše aplikace zahájí volání rozhraní API Twilio, odešle Twilio vaši žádost o adresu URL, která zpět TwiML odpověď.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-196">When your application initiates a call to the Twilio API, Twilio will send your request to a URL that is expected to return a TwiML response.</span></span> <span data-ttu-id="a0d9b-197">V předchozím příkladu používá adresu URL poskytnutou Twilio [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-197">The example above uses the Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="a0d9b-198">(Při TwiML je určen pro Twilio, můžete ji zobrazit v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-198">(While TwiML is designed for use by Twilio, you can view it in your browser.</span></span> <span data-ttu-id="a0d9b-199">For example, klikněte na tlačítko [http://twimlets.com/message] [ twimlet_message_url] zobrazíte prázdnou `<Response>` element; například klikněte na tlačítko [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] zobrazíte `<Response>` elementu, který obsahuje `<Say>` elementu.)</span><span class="sxs-lookup"><span data-stu-id="a0d9b-199">For example, click [http://twimlets.com/message][twimlet_message_url] to see an empty `<Response>` element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] to see a `<Response>` element that contains a `<Say>` element.)</span></span>

<span data-ttu-id="a0d9b-200">Aniž byste museli spoléhat na adresu URL pro zadaný Twilio, můžete vytvořit vlastního webu, který vrací odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-200">Instead of relying on the Twilio-provided URL, you can create your own site that returns HTTP responses.</span></span> <span data-ttu-id="a0d9b-201">Můžete vytvořit web v libovolném jazyce, který vrací odpovědí XML; Toto téma předpokládá, že se používá Python vytvořit TwiML.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-201">You can create the site in any language that returns XML responses; this topic assumes you will be using Python to create the TwiML.</span></span>

<span data-ttu-id="a0d9b-202">Následující příklady výstup TwiML odpovědi, která uvádí, že **Hello, World** při volání.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-202">The following examples will output a TwiML response that says **Hello World** on the call.</span></span>

<span data-ttu-id="a0d9b-203">S Flask:</span><span class="sxs-lookup"><span data-stu-id="a0d9b-203">With Flask:</span></span>

    from flask import Response
    @app.route("/")
    def hello():
        xml = '<Response><Say>Hello world.</Say></Response>'
        return Response(xml, mimetype='text/xml')

<span data-ttu-id="a0d9b-204">Pomocí rozhraní Django:</span><span class="sxs-lookup"><span data-stu-id="a0d9b-204">With Django:</span></span>

    from django.http import HttpResponse
    def hello(request):
        xml = '<Response><Say>Hello world.</Say></Response>'
        return HttpResponse(xml, content_type='text/xml')

<span data-ttu-id="a0d9b-205">Jak je vidět na výše uvedeném příkladu, odpověď TwiML je jednoduše dokument XML.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-205">As you can see from the example above, the TwiML response is simply an XML document.</span></span> <span data-ttu-id="a0d9b-206">Knihovna Twilio pro jazyk Python obsahuje třídy, které způsobí vygenerování TwiML za vás.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-206">The Twilio library for Python contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="a0d9b-207">Následující příklad vytvoří ekvivalentní odpovědi jako v příkladu nahoře, ale používá `twiml` modulu v knihovně Twilio pro jazyk Python:</span><span class="sxs-lookup"><span data-stu-id="a0d9b-207">The example below produces the equivalent response as shown above, but uses the `twiml` module in the Twilio library for Python:</span></span>

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

<span data-ttu-id="a0d9b-208">Další informace o TwiML najdete v tématu [https://www.twilio.com/docs/api/twiml][twiml_reference].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-208">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml][twiml_reference].</span></span>

<span data-ttu-id="a0d9b-209">Až budete mít aplikaci Python nastavit na, zadejte TwiML odpovědi, použijte adresu URL aplikace, jako adresa URL předaný do `client.calls.create` metoda.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-209">Once you have your Python application set up to provide TwiML responses, use the URL of the application as the URL passed into the `client.calls.create`  method.</span></span> <span data-ttu-id="a0d9b-210">Například, pokud máte webovou aplikaci s názvem **MyTwiML** nasadit do Azure hostované služby, můžete použít jeho adresa url jako webhooku, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="a0d9b-210">For example, if you have a Web application named **MyTwiML** deployed to an Azure hosted service, you can use its url as webhook as shown in the following example:</span></span>

    from twilio.rest import TwilioRestClient

    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"
    from_number = "NNNNNNNNNNN"
    to_number = "NNNNNNNNNNN"
    url = "http://your-domain-label.centralus.cloudapp.azure.com/MyTwiML/"

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url)
    print(call.sid)

## <span data-ttu-id="a0d9b-211"><a id="AdditionalServices"></a>Postupy: použití další Twilio služeb</span><span class="sxs-lookup"><span data-stu-id="a0d9b-211"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="a0d9b-212">Kromě příkladů tady uvedené nabízí Twilio rozhraní API založené na webu, který můžete využít další funkce Twilio vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="a0d9b-212">In addition to the examples shown here, Twilio offers web-based APIs that you can use to leverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="a0d9b-213">Úplné podrobnosti najdete v tématu [dokumentaci k rozhraní API Twilio][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="a0d9b-213">For full details, see the [Twilio API documentation][twilio_api].</span></span>

## <span data-ttu-id="a0d9b-214"><a id="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="a0d9b-214"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="a0d9b-215">Teď, když jste se naučili základy služby Twilio, postupujte podle následujících odkazech na další informace:</span><span class="sxs-lookup"><span data-stu-id="a0d9b-215">Now that you have learned the basics of the Twilio service, follow these links to learn more:</span></span>

* <span data-ttu-id="a0d9b-216">[Pokyny pro zabezpečení Twilio][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="a0d9b-216">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="a0d9b-217">[Příručky Twilio postupy a příklady kódu][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="a0d9b-217">[Twilio HowTo Guides and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="a0d9b-218">[Kurzy Twilio rychlý start][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="a0d9b-218">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span>
* <span data-ttu-id="a0d9b-219">[Twilio na Githubu][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="a0d9b-219">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="a0d9b-220">[Obraťte se na podporu Twilio][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="a0d9b-220">[Talk to Twilio Support][twilio_support]</span></span>

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
