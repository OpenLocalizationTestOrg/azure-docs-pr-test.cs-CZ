---
title: "aaaHow tooUse Twilio pro hlasový a serveru SMS (.NET) | Microsoft Docs"
description: "Zjistěte, jak toomake telefonní hovor a odesílání SMS zprávy službou hello Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořené v rozhraní .NET."
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
ms.openlocfilehash: f568da87ef15e9f540fee9674de31e983d4acb6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-from-azure"></a><span data-ttu-id="261df-104">Jak toouse Twilio pro hlasový a možnosti služby SMS z Azure</span><span class="sxs-lookup"><span data-stu-id="261df-104">How toouse Twilio for voice and SMS capabilities from Azure</span></span>
<span data-ttu-id="261df-105">Tato příručka ukazuje, jak služba tooperform běžné úlohy programování s hello Twilio rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="261df-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="261df-106">pokryté scénáře Hello zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS).</span><span class="sxs-lookup"><span data-stu-id="261df-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="261df-107">Další informace o Twilio a použití hlasu a SMS ve svých aplikacích najdete v tématu hello [další kroky](#NextSteps) části.</span><span class="sxs-lookup"><span data-stu-id="261df-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next steps](#NextSteps) section.</span></span>

## <span data-ttu-id="261df-108"><a id="WhatIs"></a>Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="261df-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="261df-109">Twilio je pohánějící budoucí hello firmy komunikace, povolení vývojáři tooembed hlasu VoIP a zasílání zpráv do aplikace.</span><span class="sxs-lookup"><span data-stu-id="261df-109">Twilio is powering hello future of business communications, enabling developers tooembed voice, VoIP, and messaging into applications.</span></span> <span data-ttu-id="261df-110">Jejich Virtualizovat všechny infrastrukturu potřebnou v prostředí cloudu, globální vystavení prostřednictvím platformy hello Twilio komunikace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="261df-110">They virtualize all infrastructure needed in a cloud-based, global environment, exposing it through hello Twilio communications API platform.</span></span> <span data-ttu-id="261df-111">Aplikace jsou jednoduché toobuild a škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="261df-111">Applications are simple toobuild and scalable.</span></span> <span data-ttu-id="261df-112">Získejte flexibilitu s průběžnými platbami ceny a využívat cloudové spolehlivost.</span><span class="sxs-lookup"><span data-stu-id="261df-112">Enjoy flexibility with pay-as-you-go pricing, and benefit from cloud reliability.</span></span>

<span data-ttu-id="261df-113">**Hlasové Twilio** umožňuje toomake vaší aplikace a příjem telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="261df-113">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="261df-114">**Twilio SMS** umožňuje toosend vaší aplikace a přijímat zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="261df-114">**Twilio SMS** enables your applications toosend and receive SMS messages.</span></span> <span data-ttu-id="261df-115">**Klient Twilio** vám umožní toomake VoIP volání z jakékoli telefon, tablet nebo prohlížeče a podporuje WebRTC.</span><span class="sxs-lookup"><span data-stu-id="261df-115">**Twilio Client** allows you toomake VoIP calls from any phone, tablet, or browser and supports WebRTC.</span></span>

## <span data-ttu-id="261df-116"><a id="Pricing"></a>Ceny Twilio a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="261df-116"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="261df-117">Přijímat Azure zákazníků [speciální nabídka](http://www.twilio.com/azure): bezplatnou 10 Twilio kreditu při upgradu vašeho účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="261df-117">Azure customers receive a [special offer](http://www.twilio.com/azure): complimentary $10 of Twilio Credit when you upgrade your Twilio Account.</span></span> <span data-ttu-id="261df-118">Tato Twilio platební můžou být použité tooany Twilio využití (10 platební ekvivalentní toosending až 1 000 zpráv serveru SMS nebo příjem až too1000 příchozí hlasové minut v závislosti na umístění hello vaše telefonní číslo a zpráva nebo volání cíle).</span><span class="sxs-lookup"><span data-stu-id="261df-118">This Twilio Credit can be applied tooany Twilio usage ($10 credit equivalent toosending as many as 1,000 SMS messages or receiving up too1000 inbound Voice minutes, depending on hello location of your phone number and message or call destination).</span></span> <span data-ttu-id="261df-119">Uplatnit tento platební Twilio a začít na [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span><span class="sxs-lookup"><span data-stu-id="261df-119">Redeem this Twilio credit and get started at [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).</span></span>

<span data-ttu-id="261df-120">Twilio je služba, průběžnými platbami.</span><span class="sxs-lookup"><span data-stu-id="261df-120">Twilio is a pay-as-you-go service.</span></span> <span data-ttu-id="261df-121">Neexistují žádné poplatky nastavení, a kdykoli můžete zavřít svůj účet.</span><span class="sxs-lookup"><span data-stu-id="261df-121">There are no set-up fees, and you can close your account at any time.</span></span> <span data-ttu-id="261df-122">Můžete najít další podrobnosti o [Twilio ceny](http://www.twilio.com/voice/pricing).</span><span class="sxs-lookup"><span data-stu-id="261df-122">You can find more details at [Twilio Pricing](http://www.twilio.com/voice/pricing).</span></span>

## <span data-ttu-id="261df-123"><a id="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="261df-123"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="261df-124">Hello Twilio rozhraní API je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="261df-124">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="261df-125">Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="261df-125">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="261df-126">Klíčové aspekty hello Twilio API jsou příkazy Twilio a Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="261df-126">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="261df-127"><a id="Verbs"></a>Příkazy Twilio</span><span class="sxs-lookup"><span data-stu-id="261df-127"><a id="Verbs"></a>Twilio verbs</span></span>
<span data-ttu-id="261df-128">Hello API využívá Twilio příkazy; například hello  **&lt;indikované&gt;**  příkaz dá pokyn Twilio tooaudibly doručit zprávu na volání.</span><span class="sxs-lookup"><span data-stu-id="261df-128">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="261df-129">Hello následuje seznam operací Twilio.</span><span class="sxs-lookup"><span data-stu-id="261df-129">hello following is a list of Twilio verbs.</span></span>  <span data-ttu-id="261df-130">Další informace o hello jiné příkazy a možnosti prostřednictvím [Twilio Markup Language dokumentaci](http://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="261df-130">Learn about hello other verbs and capabilities via [Twilio Markup Language documentation](http://www.twilio.com/docs/api/twiml).</span></span>

* <span data-ttu-id="261df-131">**&lt;Vytočit&gt;**: připojí hello volající tooanother phone.</span><span class="sxs-lookup"><span data-stu-id="261df-131">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="261df-132">**&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu hello.</span><span class="sxs-lookup"><span data-stu-id="261df-132">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="261df-133">**&lt;Zavěšení&gt;**: ukončí volání.</span><span class="sxs-lookup"><span data-stu-id="261df-133">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="261df-134">**&lt;Přehrání&gt;**: přehraje zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="261df-134">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="261df-135">**&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.</span><span class="sxs-lookup"><span data-stu-id="261df-135">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="261df-136">**&lt;Záznam&gt;**: zaznamenává hlasové hello volajícího a vrátí adresu URL souboru, který obsahuje záznam hello.</span><span class="sxs-lookup"><span data-stu-id="261df-136">**&lt;Record&gt;**: Records hello caller's voice and returns an URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="261df-137">**&lt;Přesměrování&gt;**: přenáší kontrolu nad telefonického hovoru nebo SMS toohello TwiML na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="261df-137">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="261df-138">**&lt;Odmítnout&gt;**: odmítne na příchozí volání číslo Twilio tooyour bez fakturace můžete</span><span class="sxs-lookup"><span data-stu-id="261df-138">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you</span></span>
* <span data-ttu-id="261df-139">**&lt;Řekněme&gt;**: toospeech převede text, že z volání.</span><span class="sxs-lookup"><span data-stu-id="261df-139">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="261df-140">**&lt;SMS&gt;**: odešle zprávu SMS.</span><span class="sxs-lookup"><span data-stu-id="261df-140">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="261df-141"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="261df-141"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="261df-142">TwiML je sada založený na jazyce XML pokyny podle hello Twilio příkazy, které informovat o tom, jak Twilio tooprocess hovoru nebo SMS.</span><span class="sxs-lookup"><span data-stu-id="261df-142">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="261df-143">Jako příklad by hello následující TwiML převést hello text **Hello, World** toospeech.</span><span class="sxs-lookup"><span data-stu-id="261df-143">As an example, hello following TwiML would convert hello text **Hello World** toospeech.</span></span>

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

<span data-ttu-id="261df-144">Pokud vaše aplikace volání hello Twilio rozhraní API, jeden z parametrů hello rozhraní API je hello adresu URL, který vrátí odpověď TwiML hello.</span><span class="sxs-lookup"><span data-stu-id="261df-144">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="261df-145">Pro účely vývoje můžete použít adresy URL zadané Twilio tooprovide hello TwiML odpovědí použít v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="261df-145">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="261df-146">Může taky hostovat vlastní adresy URL tooproduce hello TwiML odpovědí, a další možností je toouse hello **TwiMLResponse** objektu.</span><span class="sxs-lookup"><span data-stu-id="261df-146">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello **TwiMLResponse** object.</span></span>

<span data-ttu-id="261df-147">Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="261df-147">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="261df-148">Další informace o hello Twilio API najdete v tématu [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="261df-148">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="261df-149"><a id="CreateAccount"></a>Vytvoření účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="261df-149"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="261df-150">Když jste připravené tooget Twilio účet, zaregistrovat na [zkuste Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="261df-150">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="261df-151">Můžete začít s bezplatný účet a upgradujte si účet později.</span><span class="sxs-lookup"><span data-stu-id="261df-151">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="261df-152">Když se zaregistrujete k účtu Twilio, obdržíte ID účtu a ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="261df-152">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="261df-153">Jak bude volání rozhraní API Twilio potřebné toomake.</span><span class="sxs-lookup"><span data-stu-id="261df-153">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="261df-154">tooprevent neoprávněný přístup k účtu tooyour, ověřovací token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="261df-154">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="261df-155">ID účtu a ověřování tokenu je možné zobrazit v hello [stránku účtu Twilio][twilio_account]v hello pole s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="261df-155">Your account ID and authentication token are viewable at hello [Twilio account page][twilio_account], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="261df-156"><a id="create_app"></a>Vytvoření aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="261df-156"><a id="create_app"></a>Create an Azure Application</span></span>
<span data-ttu-id="261df-157">Aplikaci Azure, který je hostitelem aplikace Twilio povoleno se neliší od všech aplikací Azure.</span><span class="sxs-lookup"><span data-stu-id="261df-157">An Azure application that hosts a Twilio enabled application is no different from any other Azure application.</span></span> <span data-ttu-id="261df-158">Přidejte knihovny Twilio .NET hello a nakonfigurujte hello role toouse hello Twilio .NET knihovny.</span><span class="sxs-lookup"><span data-stu-id="261df-158">You add hello Twilio .NET library and configure hello role toouse hello Twilio .NET libraries.</span></span>
<span data-ttu-id="261df-159">Informace o vytváření počáteční projektu Azure najdete v tématu [vytvoření projektu Azure pomocí sady Visual Studio][vs_project].</span><span class="sxs-lookup"><span data-stu-id="261df-159">For information on creating an initial Azure project, see [Creating an Azure project with Visual Studio][vs_project].</span></span>

## <span data-ttu-id="261df-160"><a id="configure_app"></a>Konfigurace knihovny Twilio toouse vaše aplikace</span><span class="sxs-lookup"><span data-stu-id="261df-160"><a id="configure_app"></a>Configure Your Application toouse Twilio Libraries</span></span>
<span data-ttu-id="261df-161">Twilio poskytuje sadu knihovny .NET pomocné rutiny, které balí různé aspekty Twilio tooprovide jednoduchý a snadno způsoby toointeract s hello Twilio REST API a Twilio klienta toogenerate TwiML odpovědi.</span><span class="sxs-lookup"><span data-stu-id="261df-161">Twilio provides a set of .NET helper libraries that wrap various aspects of Twilio tooprovide simple and easy ways toointeract with hello Twilio REST API and Twilio Client toogenerate TwiML responses.</span></span>

<span data-ttu-id="261df-162">Twilio obsahuje pět knihoven pro vývojáře .NET:</span><span class="sxs-lookup"><span data-stu-id="261df-162">Twilio provides five libraries for .NET developers:</span></span>
<span data-ttu-id="261df-163">Knihovna</span><span class="sxs-lookup"><span data-stu-id="261df-163">Library</span></span>|<span data-ttu-id="261df-164">Popis</span><span class="sxs-lookup"><span data-stu-id="261df-164">Description</span></span>
---|---
<span data-ttu-id="261df-165">Twilio.API</span><span class="sxs-lookup"><span data-stu-id="261df-165">Twilio.API</span></span>|<span data-ttu-id="261df-166">Hello základní Twilio knihovny, která zabalí hello Twilio REST API v popisný knihovny .NET.</span><span class="sxs-lookup"><span data-stu-id="261df-166">hello core Twilio library that wraps hello Twilio REST API in a friendly .NET library.</span></span> <span data-ttu-id="261df-167">Tato knihovna je k dispozici pro rozhraní .NET, Silverlight a Windows Phone 7.</span><span class="sxs-lookup"><span data-stu-id="261df-167">This library is available for .NET, Silverlight, and Windows Phone 7.</span></span>
<span data-ttu-id="261df-168">Twilio.TwiML</span><span class="sxs-lookup"><span data-stu-id="261df-168">Twilio.TwiML</span></span>|<span data-ttu-id="261df-169">Poskytuje rozhraní .NET popisný způsob toogenerate TwiML značky.</span><span class="sxs-lookup"><span data-stu-id="261df-169">Provides a .NET friendly way toogenerate TwiML markup.</span></span>
<span data-ttu-id="261df-170">Twilio.MVC</span><span class="sxs-lookup"><span data-stu-id="261df-170">Twilio.MVC</span></span>|<span data-ttu-id="261df-171">Pro vývojáře, kteří používají rozhraní ASP.NET MVC zahrnuje tato knihovna TwilioController, TwiML ActionResult a atribut ověření žádosti.</span><span class="sxs-lookup"><span data-stu-id="261df-171">For developers using ASP.NET MVC, this library includes a TwilioController, TwiML ActionResult, and request validation attribute.</span></span>
<span data-ttu-id="261df-172">Twilio.WebMatrix</span><span class="sxs-lookup"><span data-stu-id="261df-172">Twilio.WebMatrix</span></span>|<span data-ttu-id="261df-173">Tato knihovna pro vývojáře, kteří používají nástroj pro vývoj volné WebMatrix společnosti Microsoft, obsahuje Pomocníci syntaxe Razor pro různé akce Twilio.</span><span class="sxs-lookup"><span data-stu-id="261df-173">For developers using Microsoft's free WebMatrix development tool, this library contains Razor syntax helpers for various Twilio actions.</span></span>
<span data-ttu-id="261df-174">Twilio.Client.Capability</span><span class="sxs-lookup"><span data-stu-id="261df-174">Twilio.Client.Capability</span></span>|<span data-ttu-id="261df-175">Obsahuje tokenů generátor hello schopností pro použití s hello Twilio klienta JavaScript SDK.</span><span class="sxs-lookup"><span data-stu-id="261df-175">Contains hello Capability token generator for use with hello Twilio Client JavaScript SDK.</span></span>

<span data-ttu-id="261df-176">Upozorňujeme, že všechny knihovny vyžaduje rozhraní .NET 3.5, Silverlight 4 nebo Windows Phone 7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="261df-176">Note that all libraries require .NET 3.5, Silverlight 4, or Windows Phone 7 or later.</span></span>

<span data-ttu-id="261df-177">Ukázky Hello uvedené v tomto průvodci použijte knihovnu Twilio.API hello.</span><span class="sxs-lookup"><span data-stu-id="261df-177">hello samples provided in this guide use hello Twilio.API library.</span></span>

<span data-ttu-id="261df-178">může být Hello knihovny [nainstalovat pomocí rozšíření Správce balíčků NuGet hello](http://www.twilio.com/docs/csharp/install) k dispozici pro Visual Studio 2010 až too2015.</span><span class="sxs-lookup"><span data-stu-id="261df-178">hello libraries can be [installed using hello NuGet package manager extension](http://www.twilio.com/docs/csharp/install) available for Visual Studio 2010 up too2015.</span></span>  <span data-ttu-id="261df-179">Hello zdrojový kód je hostované na [Githubu][twilio_github_repo], což zahrnuje Wiki, který obsahuje kompletní dokumentaci pro používání knihovny hello.</span><span class="sxs-lookup"><span data-stu-id="261df-179">hello source code is hosted on [GitHub][twilio_github_repo], which includes a Wiki that contains complete documentation for using hello libraries.</span></span>

<span data-ttu-id="261df-180">Ve výchozím nastavení nainstaluje Microsoft Visual Studio 2010 verze 1.2 NuGet.</span><span class="sxs-lookup"><span data-stu-id="261df-180">By default, Microsoft Visual Studio 2010 installs version 1.2 of NuGet.</span></span> <span data-ttu-id="261df-181">Instalace hello Twilio knihovny vyžaduje verzi 1.6 NuGet nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="261df-181">Installing hello Twilio libraries requires version 1.6 of NuGet or higher.</span></span> <span data-ttu-id="261df-182">Informace o instalaci nebo aktualizaci NuGet najdete v tématu [http://nuget.org/][nuget].</span><span class="sxs-lookup"><span data-stu-id="261df-182">For information on installing or updating NuGet, see [http://nuget.org/][nuget].</span></span>

> [!NOTE]
> <span data-ttu-id="261df-183">tooinstall hello nejnovější verze balíčku NuGet, musíte nejprve odinstalovat hello načíst verzi pomocí hello Správce rozšíření Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="261df-183">tooinstall hello latest version of NuGet, you must first uninstall hello loaded version using hello Visual Studio Extension Manager.</span></span> <span data-ttu-id="261df-184">toodo Ano, musíte Visual Studio spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="261df-184">toodo so, you must run Visual Studio as administrator.</span></span> <span data-ttu-id="261df-185">V opačném hello k dispozici tlačítko odinstalovat.</span><span class="sxs-lookup"><span data-stu-id="261df-185">Otherwise, hello Uninstall button is disabled.</span></span>
>
>

### <span data-ttu-id="261df-186"><a id="use_nuget"></a>tooadd hello Twilio knihovny tooyour projektu sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="261df-186"><a id="use_nuget"></a>tooadd hello Twilio libraries tooyour Visual Studio project:</span></span>
1. <span data-ttu-id="261df-187">Otevřete řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="261df-187">Open your solution in Visual Studio.</span></span>
2. <span data-ttu-id="261df-188">Klikněte pravým tlačítkem na **odkazy**.</span><span class="sxs-lookup"><span data-stu-id="261df-188">Right-click **References**.</span></span>
3. <span data-ttu-id="261df-189">Klikněte na tlačítko **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="261df-189">Click **Manage NuGet Packages...**</span></span>
4. <span data-ttu-id="261df-190">Klikněte na tlačítko **Online**.</span><span class="sxs-lookup"><span data-stu-id="261df-190">Click **Online**.</span></span>
5. <span data-ttu-id="261df-191">Zadejte online pole hledání hello *twilio*.</span><span class="sxs-lookup"><span data-stu-id="261df-191">In hello search online box, type *twilio*.</span></span>
6. <span data-ttu-id="261df-192">Klikněte na tlačítko **nainstalovat** na hello Twilio balíčku.</span><span class="sxs-lookup"><span data-stu-id="261df-192">Click **Install** on hello Twilio package.</span></span>

## <span data-ttu-id="261df-193"><a id="howto_make_call"></a>Postupy: volání odchozí</span><span class="sxs-lookup"><span data-stu-id="261df-193"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="261df-194">Hello následující ukazuje, jak toomake odchozí volat pomocí hello **CallResource** třídy.</span><span class="sxs-lookup"><span data-stu-id="261df-194">hello following shows how toomake an outgoing call using hello **CallResource** class.</span></span> <span data-ttu-id="261df-195">Tento kód také používá zadaný Twilio lokality tooreturn hello odpovědi Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="261df-195">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="261df-196">Dosaďte svoje hodnoty hello **k** a **z** telefonních čísel a ujistěte se, abyste ověřili hello **z** telefonní číslo pro svůj účet Twilio před spuštěním kódu hello.</span><span class="sxs-lookup"><span data-stu-id="261df-196">Substitute your values for hello **to** and **from** phone numbers, and ensure that you verify hello **from** phone number for your Twilio account before running hello code.</span></span>

    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize hello TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    // Use hello Twilio-provided site for hello TwiML response.
    var url = "http://twimlets.com/message";
    url = $"{url}?Message%5B0%5D=Hello%20World";

    // Set hello call From, To, and URL values toouse for hello call.
    // This sample uses hello sandbox number provided by
    // Twilio toomake hello call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri(url));
        }

<span data-ttu-id="261df-197">Další informace o parametrech hello předaná toohello **CallResource.Create** metodu, najdete v části [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span><span class="sxs-lookup"><span data-stu-id="261df-197">For more information about hello parameters passed in toohello **CallResource.Create** method, see [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span></span>

<span data-ttu-id="261df-198">Jak je uvedeno, tento kód používá zadaný Twilio lokality tooreturn hello TwiML odpovědi.</span><span class="sxs-lookup"><span data-stu-id="261df-198">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="261df-199">Místo toho můžete použít vlastní lokality tooprovide hello TwiML odpovědi.</span><span class="sxs-lookup"><span data-stu-id="261df-199">You could instead use your own site tooprovide hello TwiML response.</span></span> <span data-ttu-id="261df-200">Další informace najdete v tématu [postup: Zadejte TwiML odpovědí z vlastního webu](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="261df-200">For more information, see [How to: Provide TwiML responses from your own website](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="261df-201"><a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS</span><span class="sxs-lookup"><span data-stu-id="261df-201"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="261df-202">Hello následující snímek obrazovky ukazuje, jak toosend zprávu SMS pomocí hello **MessageResource** třídy.</span><span class="sxs-lookup"><span data-stu-id="261df-202">hello following screenshot shows how toosend an SMS message using hello **MessageResource**  class.</span></span> <span data-ttu-id="261df-203">Hello **z** číslo poskytne Twilio pro zkušební verzi účty toosend zpráv serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="261df-203">hello **from** number is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="261df-204">Hello **k** musí ověřit číslo účtu Twilio můžete před spuštěním kódu hello.</span><span class="sxs-lookup"><span data-stu-id="261df-204">hello **to** number must be verified for your Twilio account before you run hello code.</span></span>

    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize hello TwilioClient.
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
        // An exception occurred making hello REST call
        Console.WriteLine(ex.Message);
    }

## <span data-ttu-id="261df-205"><a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu</span><span class="sxs-lookup"><span data-stu-id="261df-205"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own website</span></span>
<span data-ttu-id="261df-206">Pokud vaše aplikace iniciuje toohello volání rozhraní API Twilio – například prostřednictvím hello **CallResource.Create** metoda - Twilio odešle tooan adresu URL vašeho požadavku, která je očekávané tooreturn TwiML odpověď.</span><span class="sxs-lookup"><span data-stu-id="261df-206">When your application initiates a call toohello Twilio API - for example, via hello **CallResource.Create** method - Twilio sends your request tooan URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="261df-207">Příklad Hello v [postupy: volání odchozích](#howto_make_call) používá hello URL poskytnutou Twilio [http://twimlets.com/message] [ twimlet_message_url] tooreturn hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="261df-207">hello example in [How to: Make an outgoing call](#howto_make_call) uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url] tooreturn hello response.</span></span>

> [!NOTE]
> <span data-ttu-id="261df-208">Při TwiML je určen k použití pomocí webové služby, můžete zobrazit hello TwiML v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="261df-208">While TwiML is designed for use by web services, you can view hello TwiML in your browser.</span></span> <span data-ttu-id="261df-209">Klikněte například na [http://twimlets.com/message] [ twimlet_message_url] toosee prázdnou &lt;odpovědi&gt; element; například klikněte na tlačítko [http://twimlets.com/message ? Zpráva % 5B0 %5 D = Hello % 20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) toosee &lt;odpovědi&gt; elementu, který obsahuje &lt;indikované&gt; element.</span><span class="sxs-lookup"><span data-stu-id="261df-209">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty &lt;Response&gt; element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) toosee a &lt;Response&gt; element that contains a &lt;Say&gt; element.</span></span>
>
>

<span data-ttu-id="261df-210">Aniž byste museli spoléhat na URL poskytnutou Twilio hello, můžete vytvořit vlastního webu adresu URL, která vrací odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="261df-210">Instead of relying on hello Twilio-provided URL, you can create your own URL site that returns HTTP responses.</span></span> <span data-ttu-id="261df-211">Hello lokality můžete vytvořit v libovolném jazyce, který vrací odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="261df-211">You can create hello site in any language that returns HTTP responses.</span></span> <span data-ttu-id="261df-212">Toto téma předpokládá, že budete hostování hello URL z obecné obslužné rutiny ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="261df-212">This topic assumes you'll be hosting hello URL from an ASP.NET generic handler.</span></span>

<span data-ttu-id="261df-213">následující popisovač ASP.NET Hello sestavuje TwiML odpovědi, která uvádí, že **Hello, World** při volání hello.</span><span class="sxs-lookup"><span data-stu-id="261df-213">hello following ASP.NET Handler crafts a TwiML response that says **Hello World** on hello call.</span></span>

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
    
<span data-ttu-id="261df-214">Jak je vidět z výše uvedeném příkladu hello hello TwiML odpovědi je jednoduše dokument XML.</span><span class="sxs-lookup"><span data-stu-id="261df-214">As you can see from hello example above, hello TwiML response is simply an XML document.</span></span> <span data-ttu-id="261df-215">Knihovna Twilio.TwiML Hello obsahuje třídy, které způsobí vygenerování TwiML za vás.</span><span class="sxs-lookup"><span data-stu-id="261df-215">hello Twilio.TwiML library contains classes that will generate TwiML for you.</span></span> <span data-ttu-id="261df-216">Hello následující příklad vytvoří ekvivalentní odpovědi hello jako v příkladu nahoře, ale používá hello **VoiceResponse** třídy.</span><span class="sxs-lookup"><span data-stu-id="261df-216">hello example below produces hello equivalent response as shown above, but uses hello **VoiceResponse** class.</span></span>

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

<span data-ttu-id="261df-217">Další informace o TwiML najdete v tématu [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).</span><span class="sxs-lookup"><span data-stu-id="261df-217">For more information about TwiML, see [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).</span></span>

<span data-ttu-id="261df-218">Jakmile jste nastavili způsob tooprovide TwiML odpovědi, můžete předat tuto adresu URL toohello **CallResource.Create** metoda.</span><span class="sxs-lookup"><span data-stu-id="261df-218">Once you have set up a way tooprovide TwiML responses, you can pass that URL toohello **CallResource.Create** method.</span></span> <span data-ttu-id="261df-219">Například pokud máte webovou aplikaci s názvem tooan MyTwiML nasazení cloudové služby Azure a hello názvem vaší obslužné rutiny ASP.NET je mytwiml.ashx, adresa URL hello lze předat příliš**CallResource.Create** jak je znázorněno v následujícím kódu hello Ukázka:</span><span class="sxs-lookup"><span data-stu-id="261df-219">For example, if you have a web application named MyTwiML deployed tooan Azure cloud service, and hello name of your ASP.NET Handler is mytwiml.ashx, hello URL can be passed too**CallResource.Create** as shown in hello following code sample:</span></span>

    // This sample uses hello sandbox number provided by Twilio toomake hello call.
    // Place hello call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
        }

<span data-ttu-id="261df-220">Další informace o používání Twilio v Azure s ASP.NET najdete v tématu [jak toomake telefonní hovor pomocí Twilio ve webové roli v Azure][howto_phonecall_dotnet].</span><span class="sxs-lookup"><span data-stu-id="261df-220">For additional information about using Twilio on Azure with ASP.NET, see [How toomake a phone call using Twilio in a web role on Azure][howto_phonecall_dotnet].</span></span>

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
