---
title: "aaaHow tooUse Twilio pro hlasový a serveru SMS (Java) | Microsoft Docs"
description: "Zjistěte, jak toomake telefonní hovor a odesílání SMS zprávy službou hello Twilio rozhraní API v Azure. Ukázky kódu napsanou v jazyce Java."
services: 
documentationcenter: java
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: f3508965-5527-4255-9d51-5d5f926f4d43
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: a186e2c8e73ced928bd0dec348971034f10ba82c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-java"></a><span data-ttu-id="bd223-104">Jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="bd223-104">How tooUse Twilio for Voice and SMS Capabilities in Java</span></span>
<span data-ttu-id="bd223-105">Tato příručka ukazuje, jak služba tooperform běžné úlohy programování s hello Twilio rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="bd223-105">This guide demonstrates how tooperform common programming tasks with hello Twilio API service on Azure.</span></span> <span data-ttu-id="bd223-106">pokryté scénáře Hello zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS).</span><span class="sxs-lookup"><span data-stu-id="bd223-106">hello scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="bd223-107">Další informace o Twilio a použití hlasu a SMS ve svých aplikacích najdete v tématu hello [další kroky](#NextSteps) části.</span><span class="sxs-lookup"><span data-stu-id="bd223-107">For more information on Twilio and using voice and SMS in your applications, see hello [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="bd223-108"><a id="WhatIs"></a>Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="bd223-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="bd223-109">Twilio je telefonickou rozhraní API webové služby, které umožňuje použít existující webové jazyky a dovednosti toobuild hlas a aplikace serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="bd223-109">Twilio is a telephony web-service API that lets you use your existing web languages and skills toobuild voice and SMS applications.</span></span> <span data-ttu-id="bd223-110">Twilio je služba třetích stran (ne funkcí platformy Azure a produkt společnosti Microsoft).</span><span class="sxs-lookup"><span data-stu-id="bd223-110">Twilio is a third-party service (not an Azure feature and not a Microsoft product).</span></span>

<span data-ttu-id="bd223-111">**Hlasové Twilio** umožňuje toomake vaší aplikace a příjem telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="bd223-111">**Twilio Voice** allows your applications toomake and receive phone calls.</span></span> <span data-ttu-id="bd223-112">**Twilio SMS** umožňuje toomake vaší aplikace a přijímat zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="bd223-112">**Twilio SMS** allows your applications toomake and receive SMS messages.</span></span> <span data-ttu-id="bd223-113">**Klient Twilio** umožňuje aplikace tooenable hlasové komunikaci pomocí existujícího připojení k Internetu, včetně mobilních připojení.</span><span class="sxs-lookup"><span data-stu-id="bd223-113">**Twilio Client** allows your applications tooenable voice communication using existing Internet connections, including mobile connections.</span></span>

## <span data-ttu-id="bd223-114"><a id="Pricing"></a>Ceny Twilio a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="bd223-114"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="bd223-115">Informace o cenách Twilio je k dispozici na [Twilio ceny][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="bd223-115">Information about Twilio pricing is available at [Twilio Pricing][twilio_pricing].</span></span> <span data-ttu-id="bd223-116">Přijímat Azure zákazníků [speciální nabídka][special_offer]: volné platební 1000 textů nebo 1000 příchozí minut.</span><span class="sxs-lookup"><span data-stu-id="bd223-116">Azure customers receive a [special offer][special_offer]: a free credit of 1000 texts or 1000 inbound minutes.</span></span> <span data-ttu-id="bd223-117">toosign pro toto nabízejí nebo získat další informace, navštivte [http://ahoy.twilio.com/azure][special_offer].</span><span class="sxs-lookup"><span data-stu-id="bd223-117">toosign up for this offer or get more information, please visit [http://ahoy.twilio.com/azure][special_offer].</span></span>

## <span data-ttu-id="bd223-118"><a id="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="bd223-118"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="bd223-119">Hello Twilio rozhraní API je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd223-119">hello Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="bd223-120">Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="bd223-120">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="bd223-121">Klíčové aspekty hello Twilio API jsou příkazy Twilio a Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="bd223-121">Key aspects of hello Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="bd223-122"><a id="Verbs"></a>Příkazy Twilio</span><span class="sxs-lookup"><span data-stu-id="bd223-122"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="bd223-123">Hello API využívá Twilio příkazy; například hello ** &lt;indikované&gt; ** příkaz dá pokyn Twilio tooaudibly doručit zprávu na volání.</span><span class="sxs-lookup"><span data-stu-id="bd223-123">hello API makes use of Twilio verbs; for example, hello **&lt;Say&gt;** verb instructs Twilio tooaudibly deliver a message on a call.</span></span>

<span data-ttu-id="bd223-124">Hello následuje seznam operací Twilio.</span><span class="sxs-lookup"><span data-stu-id="bd223-124">hello following is a list of Twilio verbs.</span></span>

* <span data-ttu-id="bd223-125">**&lt;Vytočit&gt;**: připojí hello volající tooanother phone.</span><span class="sxs-lookup"><span data-stu-id="bd223-125">**&lt;Dial&gt;**: Connects hello caller tooanother phone.</span></span>
* <span data-ttu-id="bd223-126">**&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu hello.</span><span class="sxs-lookup"><span data-stu-id="bd223-126">**&lt;Gather&gt;**: Collects numeric digits entered on hello telephone keypad.</span></span>
* <span data-ttu-id="bd223-127">**&lt;Zavěšení&gt;**: ukončí volání.</span><span class="sxs-lookup"><span data-stu-id="bd223-127">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="bd223-128">**&lt;Přehrání&gt;**: přehraje zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="bd223-128">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="bd223-129">**&lt;Fronty&gt;**: přidejte hello tooa frontu volající.</span><span class="sxs-lookup"><span data-stu-id="bd223-129">**&lt;Queue&gt;**: Add hello tooa queue of callers.</span></span>
* <span data-ttu-id="bd223-130">**&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.</span><span class="sxs-lookup"><span data-stu-id="bd223-130">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="bd223-131">**&lt;Záznam&gt;**: zaznamenává hlasové hello volajícího a vrátí adresu URL souboru, který obsahuje záznam hello.</span><span class="sxs-lookup"><span data-stu-id="bd223-131">**&lt;Record&gt;**: Records hello caller's voice and returns a URL of a file that contains hello recording.</span></span>
* <span data-ttu-id="bd223-132">**&lt;Přesměrování&gt;**: přenáší kontrolu nad telefonického hovoru nebo SMS toohello TwiML na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="bd223-132">**&lt;Redirect&gt;**: Transfers control of a call or SMS toohello TwiML at a different URL.</span></span>
* <span data-ttu-id="bd223-133">**&lt;Odmítnout&gt;**: odmítne na příchozí volání bez fakturace můžete tooyour Twilio číslo.</span><span class="sxs-lookup"><span data-stu-id="bd223-133">**&lt;Reject&gt;**: Rejects an incoming call tooyour Twilio number without billing you.</span></span>
* <span data-ttu-id="bd223-134">**&lt;Řekněme&gt;**: toospeech převede text, že z volání.</span><span class="sxs-lookup"><span data-stu-id="bd223-134">**&lt;Say&gt;**: Converts text toospeech that is made on a call.</span></span>
* <span data-ttu-id="bd223-135">**&lt;SMS&gt;**: odešle zprávu SMS.</span><span class="sxs-lookup"><span data-stu-id="bd223-135">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="bd223-136"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="bd223-136"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="bd223-137">TwiML je sada založený na jazyce XML pokyny podle hello Twilio příkazy, které informovat o tom, jak Twilio tooprocess hovoru nebo SMS.</span><span class="sxs-lookup"><span data-stu-id="bd223-137">TwiML is a set of XML-based instructions based on hello Twilio verbs that inform Twilio of how tooprocess a call or SMS.</span></span>

<span data-ttu-id="bd223-138">Jako příklad by hello následující TwiML převést hello text **Hello, World!**</span><span class="sxs-lookup"><span data-stu-id="bd223-138">As an example, hello following TwiML would convert hello text **Hello World!**</span></span> <span data-ttu-id="bd223-139">toospeech.</span><span class="sxs-lookup"><span data-stu-id="bd223-139">toospeech.</span></span>

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

<span data-ttu-id="bd223-140">Pokud vaše aplikace volání hello Twilio rozhraní API, jeden z parametrů hello rozhraní API je hello adresu URL, který vrátí odpověď TwiML hello.</span><span class="sxs-lookup"><span data-stu-id="bd223-140">When your application calls hello Twilio API, one of hello API parameters is hello URL that returns hello TwiML response.</span></span> <span data-ttu-id="bd223-141">Pro účely vývoje můžete použít adresy URL zadané Twilio tooprovide hello TwiML odpovědí použít v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="bd223-141">For development purposes, you can use Twilio-provided URLs tooprovide hello TwiML responses used by your applications.</span></span> <span data-ttu-id="bd223-142">Může taky hostovat vlastní adresy URL tooproduce hello TwiML odpovědí, a další možností je toouse hello **TwiMLResponse** objektu.</span><span class="sxs-lookup"><span data-stu-id="bd223-142">You could also host your own URLs tooproduce hello TwiML responses, and another option is toouse hello **TwiMLResponse** object.</span></span>

<span data-ttu-id="bd223-143">Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="bd223-143">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="bd223-144">Další informace o hello Twilio API najdete v tématu [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="bd223-144">For additional information about hello Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="bd223-145"><a id="CreateAccount"></a>Vytvoření účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="bd223-145"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="bd223-146">Když jste připravené tooget Twilio účet, zaregistrovat na [zkuste Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="bd223-146">When you're ready tooget a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="bd223-147">Můžete začít s bezplatný účet a upgradujte si účet později.</span><span class="sxs-lookup"><span data-stu-id="bd223-147">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="bd223-148">Když se zaregistrujete k účtu Twilio, obdržíte ID účtu a ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="bd223-148">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="bd223-149">Jak bude volání rozhraní API Twilio potřebné toomake.</span><span class="sxs-lookup"><span data-stu-id="bd223-149">Both will be needed toomake Twilio API calls.</span></span> <span data-ttu-id="bd223-150">tooprevent neoprávněný přístup k účtu tooyour, ověřovací token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="bd223-150">tooprevent unauthorized access tooyour account, keep your authentication token secure.</span></span> <span data-ttu-id="bd223-151">ID účtu a ověřování tokenu je možné zobrazit v hello [Twilio konzoly][twilio_console]v hello pole s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="bd223-151">Your account ID and authentication token are viewable at hello [Twilio Console][twilio_console], in hello fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="bd223-152"><a id="create_app"></a>Vytvořit aplikaci Java</span><span class="sxs-lookup"><span data-stu-id="bd223-152"><a id="create_app"></a>Create a Java Application</span></span>
1. <span data-ttu-id="bd223-153">Získat hello Twilio JAR a přidejte ho tooyour vaše nasazení sestavení WAR a cesta sestavení Java.</span><span class="sxs-lookup"><span data-stu-id="bd223-153">Obtain hello Twilio JAR and add it tooyour Java build path and your WAR deployment assembly.</span></span> <span data-ttu-id="bd223-154">V [https://github.com/twilio/twilio-java][twilio_java], můžete stáhnout hello Githubu zdroje a vytvořit vlastní JAR nebo stáhnout předdefinovaných JAR (s nebo bez závislosti).</span><span class="sxs-lookup"><span data-stu-id="bd223-154">At [https://github.com/twilio/twilio-java][twilio_java], you can download hello GitHub sources and create your own JAR, or download a pre-built JAR (with or without dependencies).</span></span>
2. <span data-ttu-id="bd223-155">Zkontrolujte vaše JDK **cacerts** úložiště klíčů obsahuje hello certifikát Equifax zabezpečení certifikační autority s MD5 otisk prstu 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello sériové číslo je 35:DE:F4:CF a hello SHA1 otisk prstu se D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A).</span><span class="sxs-lookup"><span data-stu-id="bd223-155">Ensure your JDK's **cacerts** keystore contains hello Equifax Secure Certificate Authority certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello serial number is 35:DE:F4:CF and hello SHA1 fingerprint is D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A).</span></span> <span data-ttu-id="bd223-156">Toto je hello certifikát certifikační autority certifikátu pro hello [https://api.twilio.com] [ twilio_api_service] služba, která je volána, když používáte rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="bd223-156">This is hello certificate authority (CA) certificate for hello [https://api.twilio.com][twilio_api_service] service, which is called when you use Twilio APIs.</span></span> <span data-ttu-id="bd223-157">Informace o zajištění vaší JDK **cacerts** úložiště klíčů obsahuje hello správný certifikát certifikační Autority najdete v tématu [přidání certifikátu toohello úložiště certifikátů certifikační Autority Java][add_ca_cert].</span><span class="sxs-lookup"><span data-stu-id="bd223-157">For information about ensuring your JDK's **cacerts** keystore contains hello correct CA certificate, see [Adding a Certificate toohello Java CA Certificate Store][add_ca_cert].</span></span>

<span data-ttu-id="bd223-158">Podrobné pokyny pro jazyk Java pomocí hello Twilio klientské knihovny jsou k dispozici na [jak tooMake Twilio pomocí telefonního hovoru v aplikaci Java v Azure][howto_phonecall_java].</span><span class="sxs-lookup"><span data-stu-id="bd223-158">Detailed instructions for using hello Twilio client library for Java are available at [How tooMake a Phone Call Using Twilio in a Java Application on Azure][howto_phonecall_java].</span></span>

## <span data-ttu-id="bd223-159"><a id="configure_app"></a>Konfigurace knihovny Twilio tooUse vaše aplikace</span><span class="sxs-lookup"><span data-stu-id="bd223-159"><a id="configure_app"></a>Configure Your Application tooUse Twilio Libraries</span></span>
<span data-ttu-id="bd223-160">V kódu, můžete přidat **importovat** příkazy v horní části hello zdrojových souborů pro hello Twilio balíčky nebo třídy, které chcete toouse ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bd223-160">Within your code, you can add **import** statements at hello top of your source files for hello Twilio packages or classes you want toouse in your application.</span></span>

<span data-ttu-id="bd223-161">Pro zdrojové soubory Java:</span><span class="sxs-lookup"><span data-stu-id="bd223-161">For Java source files:</span></span>

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

<span data-ttu-id="bd223-162">Pro zdrojové soubory Java serveru stránky (JSP):</span><span class="sxs-lookup"><span data-stu-id="bd223-162">For Java Server Page (JSP) source files:</span></span>

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
<span data-ttu-id="bd223-163">V závislosti na tom, které balíčky Twilio nebo třídy chcete toouse, vaše **importovat** příkazy se můžou lišit.</span><span class="sxs-lookup"><span data-stu-id="bd223-163">Depending on which Twilio packages or classes you want toouse, your **import** statements may be different.</span></span>

## <span data-ttu-id="bd223-164"><a id="howto_make_call"></a>Postupy: volání odchozí</span><span class="sxs-lookup"><span data-stu-id="bd223-164"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="bd223-165">Hello následující ukazuje, jak toomake odchozí volat pomocí hello **volání** třídy.</span><span class="sxs-lookup"><span data-stu-id="bd223-165">hello following shows how toomake an outgoing call using hello **Call** class.</span></span> <span data-ttu-id="bd223-166">Tento kód také používá zadaný Twilio lokality tooreturn hello odpovědi Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="bd223-166">This code also uses a Twilio-provided site tooreturn hello Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="bd223-167">Dosaďte svoje hodnoty hello **z** a **k** telefonních čísel a ujistěte se, abyste ověřili hello **z** telefonní číslo kódu hello předchozí toorunning účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="bd223-167">Substitute your values for hello **from** and **to** phone numbers, and ensure that you verify hello **from** phone number for your Twilio account prior toorunning hello code.</span></span>

```java
    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize hello Twilio client.
    Twilio.init(accountSID, authToken);

    // Use hello Twilio-provided site for hello TwiML response.
    URI uri = new URI("http://twimlets.com/message" +
            "?Message%5B0%5D=Hello%20World%21");

    // Declare tooand From numbers
    PhoneNumber too= new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");

    // Create a Call creator passing From, tooand URL values
    // then make hello call by executing hello create() method
    Call.creator(to, from, uri).create();
```

<span data-ttu-id="bd223-168">Další informace o parametrech hello předaná toohello **Call.creator** metodu, najdete v části [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span><span class="sxs-lookup"><span data-stu-id="bd223-168">For more information about hello parameters passed in toohello **Call.creator** method, see [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span></span>

<span data-ttu-id="bd223-169">Jak je uvedeno, tento kód používá zadaný Twilio lokality tooreturn hello TwiML odpovědi.</span><span class="sxs-lookup"><span data-stu-id="bd223-169">As mentioned, this code uses a Twilio-provided site tooreturn hello TwiML response.</span></span> <span data-ttu-id="bd223-170">Místo toho můžete použít vlastní lokality tooprovide hello TwiML odpovědi; Další informace najdete v tématu [jak tooProvide TwiML odpovědi v aplikaci Java v Azure](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="bd223-170">You could instead use your own site tooprovide hello TwiML response; for more information, see [How tooProvide TwiML Responses in a Java Application on Azure](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="bd223-171"><a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS</span><span class="sxs-lookup"><span data-stu-id="bd223-171"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="bd223-172">Hello následující ukazuje, jak toosend zprávu SMS pomocí hello **zpráva** třídy.</span><span class="sxs-lookup"><span data-stu-id="bd223-172">hello following shows how toosend an SMS message using hello **Message** class.</span></span> <span data-ttu-id="bd223-173">Hello **z** číslo, **4155992671**, zajišťuje Twilio pro zkušební verzi účty toosend zpráv serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="bd223-173">hello **from** number, **4155992671**, is provided by Twilio for trial accounts toosend SMS messages.</span></span> <span data-ttu-id="bd223-174">Hello **k** číslo musí ověřit kódu hello předchozí toorunning účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="bd223-174">hello **to** number must be verified for your Twilio account prior toorunning hello code.</span></span>

```java
    // Use your account SID and authentication token instead
    // of hello placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize hello Twilio client.
    Twilio.init(accountSID, authToken);

    // Declare tooand From numbers and hello Body of hello SMS message
    PhoneNumber too= new PhoneNumber("+14159352345"); // Replace with a valid phone number for your account.
    PhoneNumber from = new PhoneNumber("+14158141829"); // Replace with a valid phone number for your account.
    String body = "Where's Wallace?";

    // Create a Message creator passing From, tooand Body values
    // then send hello SMS message by calling hello create() method
    Message sms = Message.creator(to, from, body).create();
```

<span data-ttu-id="bd223-175">Další informace o parametrech hello předaná toohello **Message.creator** metodu, najdete v části [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].</span><span class="sxs-lookup"><span data-stu-id="bd223-175">For more information about hello parameters passed in toohello **Message.creator** method, see [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].</span></span>

## <span data-ttu-id="bd223-176"><a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu</span><span class="sxs-lookup"><span data-stu-id="bd223-176"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="bd223-177">Pokud vaše aplikace zahájí volání toohello Twilio rozhraní API, například prostřednictvím hello **CallCreator.create** metoda, odešle Twilio tooa adresu URL vašeho požadavku, která je očekávané tooreturn TwiML odpověď.</span><span class="sxs-lookup"><span data-stu-id="bd223-177">When your application initiates a call toohello Twilio API, for example via hello **CallCreator.create** method, Twilio will send your request tooa URL that is expected tooreturn a TwiML response.</span></span> <span data-ttu-id="bd223-178">výše uvedený příklad Hello používá URL poskytnutou Twilio hello [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="bd223-178">hello example above uses hello Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="bd223-179">(Při TwiML je určen k použití pomocí webové služby, můžete zobrazit hello TwiML v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="bd223-179">(While TwiML is designed for use by Web services, you can view hello TwiML in your browser.</span></span> <span data-ttu-id="bd223-180">Klikněte například na [http://twimlets.com/message] [ twimlet_message_url] toosee prázdnou ** &lt;odpovědi&gt; ** element; například klikněte na tlačítko [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21] [ twimlet_message_url_hello_world] toosee ** &lt;odpovědi&gt; ** elementu, který obsahuje ** &lt;indikované &gt; ** elementu.)</span><span class="sxs-lookup"><span data-stu-id="bd223-180">For example, click [http://twimlets.com/message][twimlet_message_url] toosee an empty **&lt;Response&gt;** element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21][twimlet_message_url_hello_world] toosee a **&lt;Response&gt;** element that contains a **&lt;Say&gt;** element.)</span></span>

<span data-ttu-id="bd223-181">Aniž byste museli spoléhat na URL poskytnutou Twilio hello, můžete vytvořit vlastního webu adresu URL, která vrací odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="bd223-181">Instead of relying on hello Twilio-provided URL, you can create your own URL site that returns HTTP responses.</span></span> <span data-ttu-id="bd223-182">Můžete vytvořit hello lokality v libovolném jazyce, který vrací odpovědi HTTP; Toto téma předpokládá, že budete hostování hello adresu URL, na stránce JSP.</span><span class="sxs-lookup"><span data-stu-id="bd223-182">You can create hello site in any language that returns HTTP responses; this topic assumes you'll be hosting hello URL in a JSP page.</span></span>

<span data-ttu-id="bd223-183">Hello následující stránka JSP za následek TwiML odpovědi, která uvádí, že **Hello, World!**</span><span class="sxs-lookup"><span data-stu-id="bd223-183">hello following JSP page results in a TwiML response that says **Hello World!**</span></span> <span data-ttu-id="bd223-184">Při volání hello.</span><span class="sxs-lookup"><span data-stu-id="bd223-184">on hello call.</span></span>

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

<span data-ttu-id="bd223-185">následující výsledky stránky JSP v TwiML odpovědi s upozorněním, část textu Hello má několik pozastaví a uvádí informace o verzi rozhraní API Twilio hello a název Azure role hello.</span><span class="sxs-lookup"><span data-stu-id="bd223-185">hello following JSP page results in a TwiML response that says some text, has several pauses, and says information about hello Twilio API version and hello Azure role name.</span></span>

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello from Azure!</Say>
        <Pause></Pause>
        <Say>hello Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>hello Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>
```

<span data-ttu-id="bd223-186">Hello **ApiVersion** parametr je k dispozici v žádostech o hlasové Twilio (ne požadavků SMS).</span><span class="sxs-lookup"><span data-stu-id="bd223-186">hello **ApiVersion** parameter is available in Twilio voice requests (not SMS requests).</span></span> <span data-ttu-id="bd223-187">parametry k dispozici požadavku hello toosee pro hlasové Twilio a žádosti o služby SMS, najdete v části <https://www.twilio.com/docs/api/twiml/twilio_request> a <https://www.twilio.com/docs/api/twiml/sms/twilio_request >, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="bd223-187">toosee hello available request parameters for Twilio voice and SMS requests, see <https://www.twilio.com/docs/api/twiml/twilio_request> and <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, respectively.</span></span> <span data-ttu-id="bd223-188">Hello **RoleName** proměnná prostředí je k dispozici jako součást nasazení služby Azure.</span><span class="sxs-lookup"><span data-stu-id="bd223-188">hello **RoleName** environment variable is available as part of an Azure deployment.</span></span> <span data-ttu-id="bd223-189">(Pokud budete chtít tooadd vlastní proměnné prostředí, takže se mohou být zachyceny z **System.getenv**, najdete v části proměnných prostředí hello na [ostatní nastavení konfigurace Role] [misc_role_config_settings].)</span><span class="sxs-lookup"><span data-stu-id="bd223-189">(If you want tooadd custom environment variables so they could be picked up from **System.getenv**, see hello environment variables section at [Miscellaneous Role Configuration Settings][misc_role_config_settings].)</span></span>

<span data-ttu-id="bd223-190">Jakmile máte stránku JSP nastavit tooprovide TwiML odpovědi, použijte adresu URL hello hello JSP stránky, jako adresa URL předaná do hello hello **Call.creator** metoda.</span><span class="sxs-lookup"><span data-stu-id="bd223-190">Once you have your JSP page set up tooprovide TwiML responses, use hello URL of hello JSP page as hello URL passed into hello **Call.creator** method.</span></span> <span data-ttu-id="bd223-191">Například pokud máte webovou aplikaci s názvem tooan MyTwiML nasadit Azure hostované služby a hello název stránky JSP hello je mytwiml.jsp, adresa URL hello lze předat příliš**Call.creator** jak je znázorněno v následující hello:</span><span class="sxs-lookup"><span data-stu-id="bd223-191">For example, if you have a Web application named MyTwiML deployed tooan Azure hosted service, and hello name of hello JSP page is mytwiml.jsp, hello URL can be passed too**Call.creator** as shown in hello following:</span></span>

```java
    // Declare tooand From numbers and hello URL of your JSP page
    PhoneNumber too= new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, tooand URL values
    // then make hello call by executing hello create() method
    Call.creator(to, from, uri).create();
```

<span data-ttu-id="bd223-192">Další možnost reagovat s TwiML je prostřednictvím hello **VoiceResponse** třída, která je k dispozici v hello **com.twilio.twiml** balíčku.</span><span class="sxs-lookup"><span data-stu-id="bd223-192">Another option for responding with TwiML is via hello **VoiceResponse** class, which is available in hello **com.twilio.twiml** package.</span></span>

<span data-ttu-id="bd223-193">Další informace o používání Twilio v Azure s Java najdete v tématu [jak tooMake Twilio pomocí telefonního hovoru v aplikaci Java v Azure][howto_phonecall_java].</span><span class="sxs-lookup"><span data-stu-id="bd223-193">For additional information about using Twilio in Azure with Java, see [How tooMake a Phone Call Using Twilio in a Java Application on Azure][howto_phonecall_java].</span></span>

## <span data-ttu-id="bd223-194"><a id="AdditionalServices"></a>Postupy: použití další Twilio služeb</span><span class="sxs-lookup"><span data-stu-id="bd223-194"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="bd223-195">Kromě toho toohello příklady zobrazeny zde, že twilio nabízí webové rozhraní API, které můžete použít další funkce Twilio tooleverage z vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="bd223-195">In addition toohello examples shown here, Twilio offers web-based APIs that you can use tooleverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="bd223-196">Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API Twilio][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="bd223-196">For full details, see hello [Twilio API documentation][twilio_api_documentation].</span></span>

## <span data-ttu-id="bd223-197"><a id="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd223-197"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="bd223-198">Teď, když jste se naučili základy hello hello Twilio služby, použijte tyto odkazy toolearn Další:</span><span class="sxs-lookup"><span data-stu-id="bd223-198">Now that you've learned hello basics of hello Twilio service, follow these links toolearn more:</span></span>

* <span data-ttu-id="bd223-199">[Pokyny pro zabezpečení Twilio][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="bd223-199">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="bd223-200">[Twilio postupy a příklady kódu][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="bd223-200">[Twilio HowTo's and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="bd223-201">[Kurzy Twilio rychlý start][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="bd223-201">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span>
* <span data-ttu-id="bd223-202">[Twilio na Githubu][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="bd223-202">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="bd223-203">[Komunikovat tooTwilio podpory][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="bd223-203">[Talk tooTwilio Support][twilio_support]</span></span>

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World%21
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/docs
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
