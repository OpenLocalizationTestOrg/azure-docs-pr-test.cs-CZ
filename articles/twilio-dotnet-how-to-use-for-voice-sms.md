---
title: "Jak používat Twilio pro hlasový a serveru SMS (.NET) | Microsoft Docs"
description: "Naučte se telefonní hovor a odeslání zprávy SMS službou Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořené v rozhraní .NET."
services: 
documentationcenter: .net
author: devinrader
manager: twilio
editor: 
ms.assetid: 74d4f3c9-f1cb-4968-b744-36b32cd0e834
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/24/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: 1442e3af26ae87e645cf207228ed1197b2afdd4d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-from-azure"></a><span data-ttu-id="f4525-104">Jak používat Twilio pro hlasový a možnosti služby SMS z Azure</span><span class="sxs-lookup"><span data-stu-id="f4525-104">How to use Twilio for voice and SMS capabilities from Azure</span></span>
<span data-ttu-id="f4525-105">Tato příručka ukazuje, jak provádět běžné úkoly programování službou Twilio rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="f4525-105">This guide demonstrates how to perform common programming tasks with the Twilio API service on Azure.</span></span> <span data-ttu-id="f4525-106">Pokryté scénáře zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS).</span><span class="sxs-lookup"><span data-stu-id="f4525-106">The scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="f4525-107">Další informace o Twilio a používání hlasového a SMS ve svých aplikacích najdete v tématu [další kroky](#NextSteps) části.</span><span class="sxs-lookup"><span data-stu-id="f4525-107">For more information on Twilio and using voice and SMS in your applications, see the [Next steps](#NextSteps) section.</span></span>

## <span data-ttu-id="f4525-108"><a id="WhatIs"></a>Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="f4525-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="f4525-109">Twilio je pohánějící budoucí obchodní komunikaci, umožňuje vývojářům pro vložení hlasové, VoIP a zasílání zpráv do aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4525-109">Twilio is powering the future of business communications, enabling developers to embed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="f4525-110">Jejich Virtualizovat všechny infrastrukturu potřebnou v prostředí cloudu, globální vystavení prostřednictvím platformy komunikace rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="f4525-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through the Twilio communications API platform.</span></span> <span data-ttu-id="f4525-111">Aplikace jsou jednoduché k sestavení a škálovatelné.</span><span class="sxs-lookup"><span data-stu-id="f4525-111">Applications are simple to build and scalable.</span></span> <span data-ttu-id="f4525-112">Získejte flexibilitu s průběžnými platbami ceny a využívat cloudové spolehlivost.</span><span class="sxs-lookup"><span data-stu-id="f4525-112">Enjoy flexibility with pay-as-you-go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="f4525-113">**Hlasové Twilio** umožňuje aplikacím zkontrolujte a příjem telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="f4525-113">**Twilio Voice** allows your applications to make and receive phone calls.</span></span> <span data-ttu-id="f4525-114">**Twilio SMS** umožňuje vaší aplikace odesílat a přijímat zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="f4525-114">**Twilio SMS** enables your applications to send and receive SMS messages.</span></span> <span data-ttu-id="f4525-115">**Klient Twilio** umožňuje provádět volání VoIP z jakékoli telefon, tablet nebo prohlížeče a podporuje WebRTC.</span><span class="sxs-lookup"><span data-stu-id="f4525-115">**Twilio Client** allows you to make VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="f4525-116"><a id="Pricing"></a>Ceny Twilio a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="f4525-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="f4525-117">Přijímat Azure zákazníků [speciální nabídka](http://www.twilio.com/azure): bezplatnou 10 Twilio kreditu při upgradu vašeho účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="f4525-117">Azure customers receive a [special offer](http://www.twilio.com/azure): complimentary $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="f4525-118">Tato Twilio platební je použít pro všechny Twilio využití (10 platební ekvivalentní až 1 000 zpráv serveru SMS odesílání nebo přijímání až 1 000 příchozí hlasové minut, v závislosti na umístění telefonní číslo a zpráva nebo volání cíl).</span><span class="sxs-lookup"><span data-stu-id="f4525-118">This Twilio Credit can be applied to any Twilio usage ($10 credit equivalent to sending as many as 1,000 SMS messages or receiving up to 1000 inbound Voice minutes, depending on the location of your phone number and message or call destination).</span></span> <span data-ttu-id="f4525-119">Uplatnit tento platební Twilio a začít na [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span><span class="sxs-lookup"><span data-stu-id="f4525-119">Redeem this Twilio credit and get started at [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span></span>

<span data-ttu-id="f4525-120">Twilio je služba, průběžnými platbami.</span><span class="sxs-lookup"><span data-stu-id="f4525-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="f4525-121">Neexistují žádné poplatky nastavení, a kdykoli můžete zavřít svůj účet.</span><span class="sxs-lookup"><span data-stu-id="f4525-121">There are no set-up fees, and you can close your account at any time.</span></span> <span data-ttu-id="f4525-122">Můžete najít další podrobnosti o [Twilio ceny](http://www.twilio.com/voice/pricing).</span><span class="sxs-lookup"><span data-stu-id="f4525-122">You can find more details at [Twilio Pricing](http://www.twilio.com/voice/pricing).</span></span>

## <span data-ttu-id="f4525-123"><a id="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="f4525-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="f4525-124">Rozhraní API Twilio je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4525-124">The Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="f4525-125">Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="f4525-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="f4525-126">Klíčové aspekty rozhraní API Twilio jsou příkazy Twilio a Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="f4525-126">Key aspects of the Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="f4525-127"><a id="Verbs"></a>Příkazy Twilio</span><span class="sxs-lookup"><span data-stu-id="f4525-127"><a id="Verbs"></a>Twilio verbs</span></span>
<span data-ttu-id="f4525-128">Rozhraní API využívá Twilio příkazy; například  **&lt;indikované&gt;**  příkaz dá pokyn Twilio zpráva zvukově dodržujeme volání.</span><span class="sxs-lookup"><span data-stu-id="f4525-128">The API makes use of Twilio verbs; for example, the **&lt;Say&gt;** verb instructs Twilio to audibly deliver a message on a call.</span></span>

<span data-ttu-id="f4525-129">Následuje seznam operací Twilio.</span><span class="sxs-lookup"><span data-stu-id="f4525-129">The following is a list of Twilio verbs.</span></span>  <span data-ttu-id="f4525-130">Další informace o jiné příkazy a možnosti prostřednictvím [Twilio Markup Language dokumentaci](http://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="f4525-130">Learn about the other verbs and capabilities via [Twilio Markup Language documentation](http://www.twilio.com/docs/api/twiml).</span></span>

* <span data-ttu-id="f4525-131">**&lt;Vytočit&gt;**: připojí volající na jiný telefon.</span><span class="sxs-lookup"><span data-stu-id="f4525-131">**&lt;Dial&gt;**: Connects the caller to another phone.</span></span>
* <span data-ttu-id="f4525-132">**&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu.</span><span class="sxs-lookup"><span data-stu-id="f4525-132">**&lt;Gather&gt;**: Collects numeric digits entered on the telephone keypad.</span></span>
* <span data-ttu-id="f4525-133">**&lt;Zavěšení&gt;**: ukončí volání.</span><span class="sxs-lookup"><span data-stu-id="f4525-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="f4525-134">**&lt;Přehrání&gt;**: přehraje zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="f4525-134">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="f4525-135">**&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.</span><span class="sxs-lookup"><span data-stu-id="f4525-135">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="f4525-136">**&lt;Záznam&gt;**: zaznamenává hlasové volajícího a vrátí adresu URL souboru, který obsahuje záznamu.</span><span class="sxs-lookup"><span data-stu-id="f4525-136">**&lt;Record&gt;**: Records the caller's voice and returns an URL of a file that contains the recording.</span></span>
* <span data-ttu-id="f4525-137">**&lt;Přesměrování&gt;**: předá řízení hovoru nebo SMS TwiML na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f4525-137">**&lt;Redirect&gt;**: Transfers control of a call or SMS to the TwiML at a different URL.</span></span>
* <span data-ttu-id="f4525-138">**&lt;Odmítnout&gt;**: odmítne příchozí volání na vaše číslo Twilio bez fakturace můžete</span><span class="sxs-lookup"><span data-stu-id="f4525-138">**&lt;Reject&gt;**: Rejects an incoming call to your Twilio number without billing you</span></span>
* <span data-ttu-id="f4525-139">**&lt;Řekněme&gt;**: Převod převede textu na řeč, že z volání.</span><span class="sxs-lookup"><span data-stu-id="f4525-139">**&lt;Say&gt;**: Converts text to speech that is made on a call.</span></span>
* <span data-ttu-id="f4525-140">**&lt;SMS&gt;**: odešle zprávu SMS.</span><span class="sxs-lookup"><span data-stu-id="f4525-140">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="f4525-141"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="f4525-141"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="f4525-142">TwiML je sada založený na jazyce XML pokyny podle Twilio příkazy, které informují Twilio postup zpracování hovoru nebo SMS.</span><span class="sxs-lookup"><span data-stu-id="f4525-142">TwiML is a set of XML-based instructions based on the Twilio verbs that inform Twilio of how to process a call or SMS.</span></span>

<span data-ttu-id="f4525-143">Jako příklad následující TwiML by převést text **Hello, World** k rozpoznávání řeči.</span><span class="sxs-lookup"><span data-stu-id="f4525-143">As an example, the following TwiML would convert the text **Hello World** to speech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

<span data-ttu-id="f4525-144">Pokud aplikace zavolá rozhraní API Twilio, jeden z parametrů rozhraní API je adresa URL, který vrátí odpověď TwiML.</span><span class="sxs-lookup"><span data-stu-id="f4525-144">When your application calls the Twilio API, one of the API parameters is the URL that returns the TwiML response.</span></span> <span data-ttu-id="f4525-145">Pro účely vývoje URL můžete použít zadaný Twilio zajistit TwiML odpovědi použít v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="f4525-145">For development purposes, you can use Twilio-provided URLs to provide the TwiML responses used by your applications.</span></span> <span data-ttu-id="f4525-146">Může taky hostovat vlastní adresy URL k vytvoření odpovědi TwiML a další možností je použít **TwiMLResponse** objektu.</span><span class="sxs-lookup"><span data-stu-id="f4525-146">You could also host your own URLs to produce the TwiML responses, and another option is to use the **TwiMLResponse** object.</span></span>

<span data-ttu-id="f4525-147">Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="f4525-147">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="f4525-148">Další informace o rozhraní API Twilio najdete v tématu [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="f4525-148">For additional information about the Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="f4525-149"><a id="CreateAccount"></a>Vytvoření účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="f4525-149"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="f4525-150">Když budete chtít získat účet Twilio, zaregistrujte si v [zkuste Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="f4525-150">When you're ready to get a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="f4525-151">Můžete začít s bezplatný účet a upgradujte si účet později.</span><span class="sxs-lookup"><span data-stu-id="f4525-151">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="f4525-152">Když se zaregistrujete k účtu Twilio, obdržíte ID účtu a ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="f4525-152">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="f4525-153">Jak bude potřeba k volání rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="f4525-153">Both will be needed to make Twilio API calls.</span></span> <span data-ttu-id="f4525-154">Aby se zabránilo neoprávněnému přístupu ke svému účtu, zabezpečit ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="f4525-154">To prevent unauthorized access to your account, keep your authentication token secure.</span></span> <span data-ttu-id="f4525-155">ID účtu a ověřování tokenu je možné zobrazit na [stránku účtu Twilio][twilio_account], v polích s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="f4525-155">Your account ID and authentication token are viewable at the [Twilio account page][twilio_account], in the fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="f4525-156"><a id="create_app"></a>Vytvoření aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="f4525-156"><a id="create_app"></a>Create an Azure Application</span></span>
<span data-ttu-id="f4525-157">Aplikaci Azure, který je hostitelem aplikace Twilio povoleno se neliší od všech aplikací Azure.</span><span class="sxs-lookup"><span data-stu-id="f4525-157">An Azure application that hosts a Twilio enabled application is no different from any other Azure application.</span></span> <span data-ttu-id="f4525-158">Přidání knihovny Twilio .NET a konfigurovat role, kterou chcete použít na knihovny Twilio .NET.</span><span class="sxs-lookup"><span data-stu-id="f4525-158">You add the Twilio .NET library and configure the role to use the Twilio .NET libraries.</span></span>
<span data-ttu-id="f4525-159">Informace o vytváření počáteční projektu Azure najdete v tématu [vytvoření projektu Azure pomocí sady Visual Studio][vs_project].</span><span class="sxs-lookup"><span data-stu-id="f4525-159">For information on creating an initial Azure project, see [Creating an Azure project with Visual Studio][vs_project].</span></span>

## <span data-ttu-id="f4525-160"><a id="configure_app"></a>Nakonfigurujte si aplikace pro používání knihovny Twilio</span><span class="sxs-lookup"><span data-stu-id="f4525-160"><a id="configure_app"></a>Configure Your Application to use Twilio Libraries</span></span>
<span data-ttu-id="f4525-161">Twilio poskytuje sadu pomocné knihovny .NET, které balí různé aspekty Twilio zajistit způsoby jednoduchý a snadno pracovat s Twilio REST API a Twilio klienta vygenerovat TwiML odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f4525-161">Twilio provides a set of .NET helper libraries that wrap various aspects of Twilio to provide simple and easy ways to interact with the Twilio REST API and Twilio Client to generate TwiML responses.</span></span>

<span data-ttu-id="f4525-162">Twilio obsahuje pět knihoven pro vývojáře .NET:</span><span class="sxs-lookup"><span data-stu-id="f4525-162">Twilio provides five libraries for .NET developers:</span></span>
<span data-ttu-id="f4525-163">Knihovna</span><span class="sxs-lookup"><span data-stu-id="f4525-163">Library</span></span>|<span data-ttu-id="f4525-164">Popis</span><span class="sxs-lookup"><span data-stu-id="f4525-164">Description</span></span>
---|---
<span data-ttu-id="f4525-165">Twilio.API</span><span class="sxs-lookup"><span data-stu-id="f4525-165">Twilio.API</span></span>|<span data-ttu-id="f4525-166">Základní knihovna Twilio, které zabaluje rozhraní REST API Twilio v popisný knihovny .NET.</span><span class="sxs-lookup"><span data-stu-id="f4525-166">The core Twilio library that wraps the Twilio REST API in a friendly .NET library.</span></span> <span data-ttu-id="f4525-167">Tato knihovna je k dispozici pro rozhraní .NET, Silverlight a Windows Phone 7.</span><span class="sxs-lookup"><span data-stu-id="f4525-167">This library is available for .NET, Silverlight, and Windows Phone 7.</span></span>
<span data-ttu-id="f4525-168">Twilio.TwiML</span><span class="sxs-lookup"><span data-stu-id="f4525-168">Twilio.TwiML</span></span>|<span data-ttu-id="f4525-169">Poskytuje rozhraní .NET popisný způsob generování kódu TwiML.</span><span class="sxs-lookup"><span data-stu-id="f4525-169">Provides a .NET friendly way to generate TwiML markup.</span></span>
<span data-ttu-id="f4525-170">Twilio.MVC</span><span class="sxs-lookup"><span data-stu-id="f4525-170">Twilio.MVC</span></span>|<span data-ttu-id="f4525-171">Pro vývojáře, kteří používají rozhraní ASP.NET MVC zahrnuje tato knihovna TwilioController, TwiML ActionResult a atribut ověření žádosti.</span><span class="sxs-lookup"><span data-stu-id="f4525-171">For developers using ASP.NET MVC, this library includes a TwilioController, TwiML ActionResult, and request validation attribute.</span></span>
<span data-ttu-id="f4525-172">Twilio.WebMatrix</span><span class="sxs-lookup"><span data-stu-id="f4525-172">Twilio.WebMatrix</span></span>|<span data-ttu-id="f4525-173">Tato knihovna pro vývojáře, kteří používají nástroj pro vývoj volné WebMatrix společnosti Microsoft, obsahuje Pomocníci syntaxe Razor pro různé akce Twilio.</span><span class="sxs-lookup"><span data-stu-id="f4525-173">For developers using Microsoft's free WebMatrix development tool, this library contains Razor syntax helpers for various Twilio actions.</span></span>
<span data-ttu-id="f4525-174">Twilio.Client.Capability</span><span class="sxs-lookup"><span data-stu-id="f4525-174">Twilio.Client.Capability</span></span>|<span data-ttu-id="f4525-175">Obsahuje tokenů generátor schopností pro použití s Twilio klienta JavaScript SDK.</span><span class="sxs-lookup"><span data-stu-id="f4525-175">Contains the Capability token generator for use with the Twilio Client JavaScript SDK.</span></span>

<span data-ttu-id="f4525-176">Upozorňujeme, že všechny knihovny vyžaduje rozhraní .NET 3.5, Silverlight 4 nebo Windows Phone 7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f4525-176">Note that all libraries require .NET 3.5, Silverlight 4, or Windows Phone 7 or later.</span></span>

<span data-ttu-id="f4525-177">Ukázky uvedené v tomto průvodci v knihovně Twilio.API.</span><span class="sxs-lookup"><span data-stu-id="f4525-177">The samples provided in this guide use the Twilio.API library.</span></span>

<span data-ttu-id="f4525-178">Může být v knihovnách [nainstalovat pomocí rozšíření Správce balíčků NuGet](http://www.twilio.com/docs/csharp/install) k dispozici pro Visual Studio 2010 až 2015.</span><span class="sxs-lookup"><span data-stu-id="f4525-178">The libraries can be [installed using the NuGet package manager extension](http://www.twilio.com/docs/csharp/install) available for Visual Studio 2010 up to 2015.</span></span>  <span data-ttu-id="f4525-179">Zdrojový kód je hostované na [Githubu][twilio_github_repo], což zahrnuje Wiki, který obsahuje kompletní dokumentaci pro používání knihovny.</span><span class="sxs-lookup"><span data-stu-id="f4525-179">The source code is hosted on [GitHub][twilio_github_repo], which includes a Wiki that contains complete documentation for using the libraries.</span></span>

<span data-ttu-id="f4525-180">Ve výchozím nastavení nainstaluje Microsoft Visual Studio 2010 verze 1.2 NuGet.</span><span class="sxs-lookup"><span data-stu-id="f4525-180">By default, Microsoft Visual Studio 2010 installs version 1.2 of NuGet.</span></span> <span data-ttu-id="f4525-181">Instalace knihovny Twilio vyžaduje verzi 1.6 NuGet nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="f4525-181">Installing the Twilio libraries requires version 1.6 of NuGet or higher.</span></span> <span data-ttu-id="f4525-182">Informace o instalaci nebo aktualizaci NuGet najdete v tématu [http://nuget.org/][nuget].</span><span class="sxs-lookup"><span data-stu-id="f4525-182">For information on installing or updating NuGet, see [http://nuget.org/][nuget].</span></span>

> [!NOTE]
> <span data-ttu-id="f4525-183">K instalaci nejnovější verze balíčku NuGet, musíte nejprve odinstalovat načíst verzi pomocí Správce rozšíření Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4525-183">To install the latest version of NuGet, you must first uninstall the loaded version using the Visual Studio Extension Manager.</span></span> <span data-ttu-id="f4525-184">To pokud chcete udělat, musíte Visual Studio spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="f4525-184">To do so, you must run Visual Studio as administrator.</span></span> <span data-ttu-id="f4525-185">Jinak je zakázána na tlačítko odinstalovat.</span><span class="sxs-lookup"><span data-stu-id="f4525-185">Otherwise, the Uninstall button is disabled.</span></span>
>
>

### <span data-ttu-id="f4525-186"><a id="use_nuget"></a>Přidání knihovny Twilio do projektu sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f4525-186"><a id="use_nuget"></a>To add the Twilio libraries to your Visual Studio project:</span></span>
1. <span data-ttu-id="f4525-187">Otevřete řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4525-187">Open your solution in Visual Studio.</span></span>
2. <span data-ttu-id="f4525-188">Klikněte pravým tlačítkem na **odkazy**.</span><span class="sxs-lookup"><span data-stu-id="f4525-188">Right-click **References**.</span></span>
3. <span data-ttu-id="f4525-189">Klikněte na tlačítko **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="f4525-189">Click **Manage NuGet Packages...**</span></span>
4. <span data-ttu-id="f4525-190">Klikněte na tlačítko **Online**.</span><span class="sxs-lookup"><span data-stu-id="f4525-190">Click **Online**.</span></span>
5. <span data-ttu-id="f4525-191">Zadejte do vyhledávacího pole online *twilio*.</span><span class="sxs-lookup"><span data-stu-id="f4525-191">In the search online box, type *twilio*.</span></span>
6. <span data-ttu-id="f4525-192">Klikněte na tlačítko **nainstalovat** na balíček Twilio.</span><span class="sxs-lookup"><span data-stu-id="f4525-192">Click **Install** on the Twilio package.</span></span>

## <span data-ttu-id="f4525-193"><a id="howto_make_call"></a>Postupy: volání odchozí</span><span class="sxs-lookup"><span data-stu-id="f4525-193"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="f4525-194">Následující ukazuje, jak zajistit odchozí volání pomocí **CallResource** třídy.</span><span class="sxs-lookup"><span data-stu-id="f4525-194">The following shows how to make an outgoing call using the **CallResource** class.</span></span> <span data-ttu-id="f4525-195">Tento kód také používá zadaný Twilio lokality k vrácení odpovědi Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="f4525-195">This code also uses a Twilio-provided site to return the Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="f4525-196">Dosaďte svoje hodnoty **k** a **z** telefonních čísel a ujistěte se, abyste ověřili **z** telefonní číslo pro svůj účet Twilio před spuštěním kódu.</span><span class="sxs-lookup"><span data-stu-id="f4525-196">Substitute your values for the **to** and **from** phone numbers, and ensure that you verify the **from** phone number for your Twilio account before running the code.</span></span>

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize the TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    var url = "http://twimlets.com/message";
    url = $"{url}?Message%5B0%5D=Hello%20World";

    // Set the call From, To, and URL values to use for the call.
    // This sample uses the sandbox number provided by
    // Twilio to make the call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri(url));
        }

<span data-ttu-id="f4525-197">Další informace o parametry předané do **CallResource.Create** metodu, najdete v části [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span><span class="sxs-lookup"><span data-stu-id="f4525-197">For more information about the parameters passed in to the **CallResource.Create** method, see [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span></span>

<span data-ttu-id="f4525-198">Jak je uvedeno, tento kód používá k vrácení odpovědi TwiML zadaný Twilio lokality.</span><span class="sxs-lookup"><span data-stu-id="f4525-198">As mentioned, this code uses a Twilio-provided site to return the TwiML response.</span></span> <span data-ttu-id="f4525-199">Místo toho můžete použít vlastní funkční web zajistit TwiML odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f4525-199">You could instead use your own site to provide the TwiML response.</span></span> <span data-ttu-id="f4525-200">Další informace najdete v tématu [postup: Zadejte TwiML odpovědí z vlastního webu](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="f4525-200">For more information, see [How to: Provide TwiML responses from your own website](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="f4525-201"><a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS</span><span class="sxs-lookup"><span data-stu-id="f4525-201"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="f4525-202">Následující snímek obrazovky ukazuje, jak odeslat zprávu SMS pomocí **MessageResource** třídy.</span><span class="sxs-lookup"><span data-stu-id="f4525-202">The following screenshot shows how to send an SMS message using the **MessageResource**  class.</span></span> <span data-ttu-id="f4525-203">**z** číslo poskytne Twilio pro zkušebními účty k odesílání zpráv serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="f4525-203">The **from** number is provided by Twilio for trial accounts to send SMS messages.</span></span> <span data-ttu-id="f4525-204">**k** musí ověřit číslo účtu Twilio můžete před spuštěním kódu.</span><span class="sxs-lookup"><span data-stu-id="f4525-204">The **to** number must be verified for your Twilio account before you run the code.</span></span>

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize the TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    try
    {
        // Send an SMS message.
        var message = MessageResource.Create(
            to: new PhoneNumber("+12069419717"),
            from: new PhoneNumber("+14155992671"),
            body: "This is my SMS message.");
    }
    catch (TwilioException ex)
    {
        // An exception occurred making the REST call
        Console.WriteLine(ex.Message);
    }

## <span data-ttu-id="f4525-205"><a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu</span><span class="sxs-lookup"><span data-stu-id="f4525-205"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own website</span></span>
<span data-ttu-id="f4525-206">Pokud vaše aplikace zahájí volání rozhraní API Twilio – například prostřednictvím **CallResource.Create** metoda - Twilio odešle žádost na adresu URL, kterou je očekáván vrátí odpověď TwiML.</span><span class="sxs-lookup"><span data-stu-id="f4525-206">When your application initiates a call to the Twilio API - for example, via the **CallResource.Create** method - Twilio sends your request to an URL that is expected to return a TwiML response.</span></span> <span data-ttu-id="f4525-207">V příkladu v [postupy: volání odchozí](#howto_make_call) používá adresu URL poskytnutou Twilio [http://twimlets.com/message] [ twimlet_message_url] vrátit odpověď.</span><span class="sxs-lookup"><span data-stu-id="f4525-207">The example in [How to: Make an outgoing call](#howto_make_call) uses the Twilio-provided URL [http://twimlets.com/message][twimlet_message_url] to return the response.</span></span>

> [!NOTE]
> <span data-ttu-id="f4525-208">Při TwiML je určen k použití pomocí webové služby, můžete zobrazit TwiML v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f4525-208">While TwiML is designed for use by web services, you can view the TwiML in your browser.</span></span> <span data-ttu-id="f4525-209">Klikněte například na [http://twimlets.com/message] [ twimlet_message_url] zobrazíte prázdnou &lt;odpovědi&gt; element; například klikněte na tlačítko [http://twimlets.com/message? Zpráva % 5B0 %5 D = Hello % 20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) zobrazíte &lt;odpovědi&gt; elementu, který obsahuje &lt;indikované&gt; element.</span><span class="sxs-lookup"><span data-stu-id="f4525-209">For example, click [http://twimlets.com/message][twimlet_message_url] to see an empty &lt;Response&gt; element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) to see a &lt;Response&gt; element that contains a &lt;Say&gt; element.</span></span>
>
>

<span data-ttu-id="f4525-210">Aniž byste museli spoléhat na adresu URL pro zadaný Twilio, můžete vytvořit vlastního webu adresu URL, která vrací odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="f4525-210">Instead of relying on the Twilio-provided URL, you can create your own URL site that returns HTTP responses.</span></span> <span data-ttu-id="f4525-211">Můžete vytvořit web v libovolném jazyce, který vrací odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="f4525-211">You can create the site in any language that returns HTTP responses.</span></span> <span data-ttu-id="f4525-212">Toto téma předpokládá, že budete hostování adresu URL z obecné obslužné rutiny ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f4525-212">This topic assumes you'll be hosting the URL from an ASP.NET generic handler.</span></span>

<span data-ttu-id="f4525-213">Obslužná rutina následující ASP.NET sestavuje TwiML odpovědi, která uvádí, že **Hello, World** při volání.</span><span class="sxs-lookup"><span data-stu-id="f4525-213">The following ASP.NET Handler crafts a TwiML response that says **Hello World** on the call.</span></span>

    using System.Text;
    using System.Web;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {
            public void ProcessRequest(HttpContext context)
            {
                const string twiMLResponse =
                    "<Response><Say>Hello World.</Say></Response>";
                
                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.ContentEncoding = Encoding.UTF8;
                context.Response.Write(twiMLResponse);
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }
    
<span data-ttu-id="f4525-214">Jak je vidět na výše uvedeném příkladu, odpověď TwiML je jednoduše dokument XML.</span><span class="sxs-lookup"><span data-stu-id="f4525-214">As you can see from the example above, the TwiML response is simply an XML document.</span></span> <span data-ttu-id="f4525-215">Knihovna Twilio.TwiML obsahuje třídy, které způsobí vygenerování TwiML za vás.</span><span class="sxs-lookup"><span data-stu-id="f4525-215">The Twilio.TwiML library contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="f4525-216">Následující příklad vytvoří ekvivalentní odpovědi jako v příkladu nahoře, ale používá **VoiceResponse** třídy.</span><span class="sxs-lookup"><span data-stu-id="f4525-216">The example below produces the equivalent response as shown above, but uses the **VoiceResponse** class.</span></span>

    using System.Web;
    using Twilio.TwiML;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {

            public void ProcessRequest(HttpContext context)
            {
                var twiml = new VoiceResponse();
                twiml.Say("Hello World.");

                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.Write(twiml.ToString());
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }

<span data-ttu-id="f4525-217">Další informace o TwiML najdete v tématu [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="f4525-217">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).</span></span>

<span data-ttu-id="f4525-218">Jakmile jste nastavili způsob, jak poskytnout TwiML odpovědí, může předat tuto adresu URL k **CallResource.Create** metoda.</span><span class="sxs-lookup"><span data-stu-id="f4525-218">Once you have set up a way to provide TwiML responses, you can pass that URL to the **CallResource.Create** method.</span></span> <span data-ttu-id="f4525-219">Například pokud máte webovou aplikaci s názvem MyTwiML nasadit do cloudové služby Azure a názvem vaší obslužné rutiny ASP.NET je mytwiml.ashx, adresa URL se dá předat do **CallResource.Create** jak znázorňuje následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="f4525-219">For example, if you have a web application named MyTwiML deployed to an Azure cloud service, and the name of your ASP.NET Handler is mytwiml.ashx, the URL can be passed to **CallResource.Create** as shown in the following code sample:</span></span>

    // This sample uses the sandbox number provided by Twilio to make the call.
    // Place the call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
        }

<span data-ttu-id="f4525-220">Další informace o používání Twilio v Azure s ASP.NET najdete v tématu [postup telefonní hovor ve webové roli v Azure pomocí Twilio][howto_phonecall_dotnet].</span><span class="sxs-lookup"><span data-stu-id="f4525-220">For additional information about using Twilio on Azure with ASP.NET, see [How to make a phone call using Twilio in a web role on Azure][howto_phonecall_dotnet].</span></span>

[!INCLUDE [twilio-additional-services-and-next-steps](../includes/twilio-additional-services-and-next-steps.md)]

[howto_phonecall_dotnet]: partner-twilio-cloud-services-dotnet-phone-call-web-role.md

[twimlet_message_url]: http://twimlets.com/message

[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls

[vs_project]:http://msdn.microsoft.com/library/windowsazure/ee405487.aspx
[nuget]:http://nuget.org/
[twilio_github_repo]:https://github.com/twilio/twilio-csharp

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
