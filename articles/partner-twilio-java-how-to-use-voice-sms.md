---
title: "Jak používat Twilio pro hlasový a serveru SMS (Java) | Microsoft Docs"
description: "Naučte se telefonní hovor a odeslání zprávy SMS službou Twilio rozhraní API v Azure. Ukázky kódu napsanou v jazyce Java."
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
ms.openlocfilehash: 5a1b2ffa160a31b639605242b651dc8d14e7a01b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a><span data-ttu-id="6654c-104">Jak používat Twilio pro hlasový a možnosti serveru SMS v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="6654c-104">How to Use Twilio for Voice and SMS Capabilities in Java</span></span>
<span data-ttu-id="6654c-105">Tato příručka ukazuje, jak provádět běžné úkoly programování službou Twilio rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="6654c-105">This guide demonstrates how to perform common programming tasks with the Twilio API service on Azure.</span></span> <span data-ttu-id="6654c-106">Pokryté scénáře zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS).</span><span class="sxs-lookup"><span data-stu-id="6654c-106">The scenarios covered include making a phone call and sending a Short Message Service (SMS) message.</span></span> <span data-ttu-id="6654c-107">Další informace o Twilio a používání hlasového a SMS ve svých aplikacích najdete v tématu [další kroky](#NextSteps) části.</span><span class="sxs-lookup"><span data-stu-id="6654c-107">For more information on Twilio and using voice and SMS in your applications, see the [Next Steps](#NextSteps) section.</span></span>

## <span data-ttu-id="6654c-108"><a id="WhatIs"></a>Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="6654c-108"><a id="WhatIs"></a>What is Twilio?</span></span>
<span data-ttu-id="6654c-109">Twilio je telefonickou rozhraní API webové služby, který vám umožní používat existující webové jazyky a dovednosti pro sestavení hlasu a aplikace serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="6654c-109">Twilio is a telephony web-service API that lets you use your existing web languages and skills to build voice and SMS applications.</span></span> <span data-ttu-id="6654c-110">Twilio je služba třetích stran (ne funkcí platformy Azure a produkt společnosti Microsoft).</span><span class="sxs-lookup"><span data-stu-id="6654c-110">Twilio is a third-party service (not an Azure feature and not a Microsoft product).</span></span>

<span data-ttu-id="6654c-111">**Hlasové Twilio** umožňuje aplikacím zkontrolujte a příjem telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="6654c-111">**Twilio Voice** allows your applications to make and receive phone calls.</span></span> <span data-ttu-id="6654c-112">**Twilio SMS** umožňuje aplikacím zkontrolujte a přijímat zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="6654c-112">**Twilio SMS** allows your applications to make and receive SMS messages.</span></span> <span data-ttu-id="6654c-113">**Klient Twilio** umožňuje aplikace tak, aby hlasové komunikace pomocí existujícího připojení k Internetu, včetně mobilních připojení.</span><span class="sxs-lookup"><span data-stu-id="6654c-113">**Twilio Client** allows your applications to enable voice communication using existing Internet connections, including mobile connections.</span></span>

## <span data-ttu-id="6654c-114"><a id="Pricing"></a>Ceny Twilio a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="6654c-114"><a id="Pricing"></a>Twilio Pricing and Special Offers</span></span>
<span data-ttu-id="6654c-115">Informace o cenách Twilio je k dispozici na [Twilio ceny][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="6654c-115">Information about Twilio pricing is available at [Twilio Pricing][twilio_pricing].</span></span> <span data-ttu-id="6654c-116">Přijímat Azure zákazníků [speciální nabídka][special_offer]: volné platební 1000 textů nebo 1000 příchozí minut.</span><span class="sxs-lookup"><span data-stu-id="6654c-116">Azure customers receive a [special offer][special_offer]: a free credit of 1000 texts or 1000 inbound minutes.</span></span> <span data-ttu-id="6654c-117">Pokud chcete zaregistrovat v rámci této nabídky nebo získat další informace, navštivte [http://ahoy.twilio.com/azure][special_offer].</span><span class="sxs-lookup"><span data-stu-id="6654c-117">To sign up for this offer or get more information, please visit [http://ahoy.twilio.com/azure][special_offer].</span></span>

## <span data-ttu-id="6654c-118"><a id="Concepts"></a>Koncepty</span><span class="sxs-lookup"><span data-stu-id="6654c-118"><a id="Concepts"></a>Concepts</span></span>
<span data-ttu-id="6654c-119">Rozhraní API Twilio je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="6654c-119">The Twilio API is a RESTful API that provides voice and SMS functionality for applications.</span></span> <span data-ttu-id="6654c-120">Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].</span><span class="sxs-lookup"><span data-stu-id="6654c-120">Client libraries are available in multiple languages; for a list, see [Twilio API Libraries][twilio_libraries].</span></span>

<span data-ttu-id="6654c-121">Klíčové aspekty rozhraní API Twilio jsou příkazy Twilio a Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="6654c-121">Key aspects of the Twilio API are Twilio verbs and Twilio Markup Language (TwiML).</span></span>

### <span data-ttu-id="6654c-122"><a id="Verbs"></a>Příkazy Twilio</span><span class="sxs-lookup"><span data-stu-id="6654c-122"><a id="Verbs"></a>Twilio Verbs</span></span>
<span data-ttu-id="6654c-123">Rozhraní API využívá Twilio příkazy; například  **&lt;indikované&gt;**  příkaz dá pokyn Twilio zpráva zvukově dodržujeme volání.</span><span class="sxs-lookup"><span data-stu-id="6654c-123">The API makes use of Twilio verbs; for example, the **&lt;Say&gt;** verb instructs Twilio to audibly deliver a message on a call.</span></span>

<span data-ttu-id="6654c-124">Následuje seznam operací Twilio.</span><span class="sxs-lookup"><span data-stu-id="6654c-124">The following is a list of Twilio verbs.</span></span>

* <span data-ttu-id="6654c-125">**&lt;Vytočit&gt;**: připojí volající na jiný telefon.</span><span class="sxs-lookup"><span data-stu-id="6654c-125">**&lt;Dial&gt;**: Connects the caller to another phone.</span></span>
* <span data-ttu-id="6654c-126">**&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu.</span><span class="sxs-lookup"><span data-stu-id="6654c-126">**&lt;Gather&gt;**: Collects numeric digits entered on the telephone keypad.</span></span>
* <span data-ttu-id="6654c-127">**&lt;Zavěšení&gt;**: ukončí volání.</span><span class="sxs-lookup"><span data-stu-id="6654c-127">**&lt;Hangup&gt;**: Ends a call.</span></span>
* <span data-ttu-id="6654c-128">**&lt;Přehrání&gt;**: přehraje zvukový soubor.</span><span class="sxs-lookup"><span data-stu-id="6654c-128">**&lt;Play&gt;**: Plays an audio file.</span></span>
* <span data-ttu-id="6654c-129">**&lt;Fronty&gt;**: přidejte do fronty volající.</span><span class="sxs-lookup"><span data-stu-id="6654c-129">**&lt;Queue&gt;**: Add the to a queue of callers.</span></span>
* <span data-ttu-id="6654c-130">**&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.</span><span class="sxs-lookup"><span data-stu-id="6654c-130">**&lt;Pause&gt;**: Waits silently for a specified number of seconds.</span></span>
* <span data-ttu-id="6654c-131">**&lt;Záznam&gt;**: zaznamenává hlasové volajícího a vrátí adresu URL souboru, který obsahuje záznamu.</span><span class="sxs-lookup"><span data-stu-id="6654c-131">**&lt;Record&gt;**: Records the caller's voice and returns a URL of a file that contains the recording.</span></span>
* <span data-ttu-id="6654c-132">**&lt;Přesměrování&gt;**: předá řízení hovoru nebo SMS TwiML na jinou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6654c-132">**&lt;Redirect&gt;**: Transfers control of a call or SMS to the TwiML at a different URL.</span></span>
* <span data-ttu-id="6654c-133">**&lt;Odmítnout&gt;**: odmítne příchozí volání na vaše číslo Twilio bez fakturace můžete.</span><span class="sxs-lookup"><span data-stu-id="6654c-133">**&lt;Reject&gt;**: Rejects an incoming call to your Twilio number without billing you.</span></span>
* <span data-ttu-id="6654c-134">**&lt;Řekněme&gt;**: Převod převede textu na řeč, že z volání.</span><span class="sxs-lookup"><span data-stu-id="6654c-134">**&lt;Say&gt;**: Converts text to speech that is made on a call.</span></span>
* <span data-ttu-id="6654c-135">**&lt;SMS&gt;**: odešle zprávu SMS.</span><span class="sxs-lookup"><span data-stu-id="6654c-135">**&lt;Sms&gt;**: Sends an SMS message.</span></span>

### <span data-ttu-id="6654c-136"><a id="TwiML"></a>TwiML</span><span class="sxs-lookup"><span data-stu-id="6654c-136"><a id="TwiML"></a>TwiML</span></span>
<span data-ttu-id="6654c-137">TwiML je sada založený na jazyce XML pokyny podle Twilio příkazy, které informují Twilio postup zpracování hovoru nebo SMS.</span><span class="sxs-lookup"><span data-stu-id="6654c-137">TwiML is a set of XML-based instructions based on the Twilio verbs that inform Twilio of how to process a call or SMS.</span></span>

<span data-ttu-id="6654c-138">Jako příklad následující TwiML by převést text **Hello, World!**</span><span class="sxs-lookup"><span data-stu-id="6654c-138">As an example, the following TwiML would convert the text **Hello World!**</span></span> <span data-ttu-id="6654c-139">k rozpoznávání řeči.</span><span class="sxs-lookup"><span data-stu-id="6654c-139">to speech.</span></span>

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

<span data-ttu-id="6654c-140">Pokud aplikace zavolá rozhraní API Twilio, jeden z parametrů rozhraní API je adresa URL, který vrátí odpověď TwiML.</span><span class="sxs-lookup"><span data-stu-id="6654c-140">When your application calls the Twilio API, one of the API parameters is the URL that returns the TwiML response.</span></span> <span data-ttu-id="6654c-141">Pro účely vývoje URL můžete použít zadaný Twilio zajistit TwiML odpovědi použít v aplikacích.</span><span class="sxs-lookup"><span data-stu-id="6654c-141">For development purposes, you can use Twilio-provided URLs to provide the TwiML responses used by your applications.</span></span> <span data-ttu-id="6654c-142">Může taky hostovat vlastní adresy URL k vytvoření odpovědi TwiML a další možností je použít **TwiMLResponse** objektu.</span><span class="sxs-lookup"><span data-stu-id="6654c-142">You could also host your own URLs to produce the TwiML responses, and another option is to use the **TwiMLResponse** object.</span></span>

<span data-ttu-id="6654c-143">Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml].</span><span class="sxs-lookup"><span data-stu-id="6654c-143">For more information about Twilio verbs, their attributes, and TwiML, see [TwiML][twiml].</span></span> <span data-ttu-id="6654c-144">Další informace o rozhraní API Twilio najdete v tématu [Twilio API][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="6654c-144">For additional information about the Twilio API, see [Twilio API][twilio_api].</span></span>

## <span data-ttu-id="6654c-145"><a id="CreateAccount"></a>Vytvoření účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="6654c-145"><a id="CreateAccount"></a>Create a Twilio Account</span></span>
<span data-ttu-id="6654c-146">Když budete chtít získat účet Twilio, zaregistrujte si v [zkuste Twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="6654c-146">When you're ready to get a Twilio account, sign up at [Try Twilio][try_twilio].</span></span> <span data-ttu-id="6654c-147">Můžete začít s bezplatný účet a upgradujte si účet později.</span><span class="sxs-lookup"><span data-stu-id="6654c-147">You can start with a free account, and upgrade your account later.</span></span>

<span data-ttu-id="6654c-148">Když se zaregistrujete k účtu Twilio, obdržíte ID účtu a ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="6654c-148">When you sign up for a Twilio account, you'll receive an account ID and an authentication token.</span></span> <span data-ttu-id="6654c-149">Jak bude potřeba k volání rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="6654c-149">Both will be needed to make Twilio API calls.</span></span> <span data-ttu-id="6654c-150">Aby se zabránilo neoprávněnému přístupu ke svému účtu, zabezpečit ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="6654c-150">To prevent unauthorized access to your account, keep your authentication token secure.</span></span> <span data-ttu-id="6654c-151">ID účtu a ověřování tokenu je možné zobrazit na [Twilio konzoly][twilio_console], v polích s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6654c-151">Your account ID and authentication token are viewable at the [Twilio Console][twilio_console], in the fields labeled **ACCOUNT SID** and **AUTH TOKEN**, respectively.</span></span>

## <span data-ttu-id="6654c-152"><a id="create_app"></a>Vytvořit aplikaci Java</span><span class="sxs-lookup"><span data-stu-id="6654c-152"><a id="create_app"></a>Create a Java Application</span></span>
1. <span data-ttu-id="6654c-153">Získat Twilio JAR a přidat jej do vaší cesta sestavení Java a vaše WAR nasazení sestavení.</span><span class="sxs-lookup"><span data-stu-id="6654c-153">Obtain the Twilio JAR and add it to your Java build path and your WAR deployment assembly.</span></span> <span data-ttu-id="6654c-154">V [https://github.com/twilio/twilio-java][twilio_java], můžete stáhnout zdroje Githubu a vytvořit vlastní JAR nebo stáhnout předdefinovaných JAR (s nebo bez závislosti).</span><span class="sxs-lookup"><span data-stu-id="6654c-154">At [https://github.com/twilio/twilio-java][twilio_java], you can download the GitHub sources and create your own JAR, or download a pre-built JAR (with or without dependencies).</span></span>
2. <span data-ttu-id="6654c-155">Zkontrolujte vaše JDK **cacerts** úložiště klíčů obsahuje certifikát Equifax zabezpečení certifikační autority s MD5 otisk prstu 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (sériové číslo je 35:DE:F4:CF a otisk prstu SHA1 D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A).</span><span class="sxs-lookup"><span data-stu-id="6654c-155">Ensure your JDK's **cacerts** keystore contains the Equifax Secure Certificate Authority certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (the serial number is 35:DE:F4:CF and the SHA1 fingerprint is D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A).</span></span> <span data-ttu-id="6654c-156">Toto je certifikát certifikační autority certifikátu pro [https://api.twilio.com] [ twilio_api_service] služba, která je volána, když používáte rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="6654c-156">This is the certificate authority (CA) certificate for the [https://api.twilio.com][twilio_api_service] service, which is called when you use Twilio APIs.</span></span> <span data-ttu-id="6654c-157">Informace o zajištění vaší JDK **cacerts** úložiště klíčů obsahuje správný certifikát certifikační Autority najdete v tématu [přidání certifikátu do úložiště certifikátů certifikační Autority Java][add_ca_cert].</span><span class="sxs-lookup"><span data-stu-id="6654c-157">For information about ensuring your JDK's **cacerts** keystore contains the correct CA certificate, see [Adding a Certificate to the Java CA Certificate Store][add_ca_cert].</span></span>

<span data-ttu-id="6654c-158">Podrobné pokyny pro používání Twilio klientské knihovny pro jazyk Java jsou k dispozici v [postup proveďte v aplikaci Java v Azure Twilio pomocí telefonního hovoru][howto_phonecall_java].</span><span class="sxs-lookup"><span data-stu-id="6654c-158">Detailed instructions for using the Twilio client library for Java are available at [How to Make a Phone Call Using Twilio in a Java Application on Azure][howto_phonecall_java].</span></span>

## <span data-ttu-id="6654c-159"><a id="configure_app"></a>Konfigurace aplikace k používání Twilio knihovny</span><span class="sxs-lookup"><span data-stu-id="6654c-159"><a id="configure_app"></a>Configure Your Application to Use Twilio Libraries</span></span>
<span data-ttu-id="6654c-160">V kódu, můžete přidat **importovat** příkazy v horní části vaší zdrojové soubory pro Twilio balíčky nebo třídy, kterou chcete použít v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6654c-160">Within your code, you can add **import** statements at the top of your source files for the Twilio packages or classes you want to use in your application.</span></span>

<span data-ttu-id="6654c-161">Pro zdrojové soubory Java:</span><span class="sxs-lookup"><span data-stu-id="6654c-161">For Java source files:</span></span>

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

<span data-ttu-id="6654c-162">Pro zdrojové soubory Java serveru stránky (JSP):</span><span class="sxs-lookup"><span data-stu-id="6654c-162">For Java Server Page (JSP) source files:</span></span>

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
<span data-ttu-id="6654c-163">V závislosti na tom, které balíčky Twilio nebo třídy, kterou chcete použít, vaše **importovat** příkazy se můžou lišit.</span><span class="sxs-lookup"><span data-stu-id="6654c-163">Depending on which Twilio packages or classes you want to use, your **import** statements may be different.</span></span>

## <span data-ttu-id="6654c-164"><a id="howto_make_call"></a>Postupy: volání odchozí</span><span class="sxs-lookup"><span data-stu-id="6654c-164"><a id="howto_make_call"></a>How to: Make an outgoing call</span></span>
<span data-ttu-id="6654c-165">Následující ukazuje, jak zajistit odchozí volání pomocí **volání** třídy.</span><span class="sxs-lookup"><span data-stu-id="6654c-165">The following shows how to make an outgoing call using the **Call** class.</span></span> <span data-ttu-id="6654c-166">Tento kód také používá zadaný Twilio lokality k vrácení odpovědi Twilio Markup Language (TwiML).</span><span class="sxs-lookup"><span data-stu-id="6654c-166">This code also uses a Twilio-provided site to return the Twilio Markup Language (TwiML) response.</span></span> <span data-ttu-id="6654c-167">Dosaďte svoje hodnoty **z** a **k** telefonních čísel a ujistěte se, abyste ověřili **z** telefonní číslo pro svůj účet Twilio před spuštěním kódu.</span><span class="sxs-lookup"><span data-stu-id="6654c-167">Substitute your values for the **from** and **to** phone numbers, and ensure that you verify the **from** phone number for your Twilio account prior to running the code.</span></span>

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    URI uri = new URI("http://twimlets.com/message" +
            "?Message%5B0%5D=Hello%20World%21");

    // Declare To and From numbers
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

<span data-ttu-id="6654c-168">Další informace o parametry předané do **Call.creator** metodu, najdete v části [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span><span class="sxs-lookup"><span data-stu-id="6654c-168">For more information about the parameters passed in to the **Call.creator** method, see [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].</span></span>

<span data-ttu-id="6654c-169">Jak je uvedeno, tento kód používá k vrácení odpovědi TwiML zadaný Twilio lokality.</span><span class="sxs-lookup"><span data-stu-id="6654c-169">As mentioned, this code uses a Twilio-provided site to return the TwiML response.</span></span> <span data-ttu-id="6654c-170">Můžete místo toho použít vlastní funkční web zajistit TwiML odpovědi; Další informace najdete v tématu [jak poskytnout TwiML odpovědi v aplikaci Java v Azure](#howto_provide_twiml_responses).</span><span class="sxs-lookup"><span data-stu-id="6654c-170">You could instead use your own site to provide the TwiML response; for more information, see [How to Provide TwiML Responses in a Java Application on Azure](#howto_provide_twiml_responses).</span></span>

## <span data-ttu-id="6654c-171"><a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS</span><span class="sxs-lookup"><span data-stu-id="6654c-171"><a id="howto_send_sms"></a>How to: Send an SMS message</span></span>
<span data-ttu-id="6654c-172">Následující ukazuje, jak odeslat zprávu SMS pomocí **zpráva** třídy.</span><span class="sxs-lookup"><span data-stu-id="6654c-172">The following shows how to send an SMS message using the **Message** class.</span></span> <span data-ttu-id="6654c-173">**z** číslo, **4155992671**, zajišťuje Twilio pro zkušebními účty k odesílání zpráv serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="6654c-173">The **from** number, **4155992671**, is provided by Twilio for trial accounts to send SMS messages.</span></span> <span data-ttu-id="6654c-174">**k** číslo musí být ověřena pro váš účet Twilio před spuštěním kódu.</span><span class="sxs-lookup"><span data-stu-id="6654c-174">The **to** number must be verified for your Twilio account prior to running the code.</span></span>

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Declare To and From numbers and the Body of the SMS message
    PhoneNumber to = new PhoneNumber("+14159352345"); // Replace with a valid phone number for your account.
    PhoneNumber from = new PhoneNumber("+14158141829"); // Replace with a valid phone number for your account.
    String body = "Where's Wallace?";

    // Create a Message creator passing From, To and Body values
    // then send the SMS message by calling the create() method
    Message sms = Message.creator(to, from, body).create();
```

<span data-ttu-id="6654c-175">Další informace o parametry předané do **Message.creator** metodu, najdete v části [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].</span><span class="sxs-lookup"><span data-stu-id="6654c-175">For more information about the parameters passed in to the **Message.creator** method, see [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].</span></span>

## <span data-ttu-id="6654c-176"><a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu</span><span class="sxs-lookup"><span data-stu-id="6654c-176"><a id="howto_provide_twiml_responses"></a>How to: Provide TwiML Responses from your own Website</span></span>
<span data-ttu-id="6654c-177">Pokud vaše aplikace zahájí volání rozhraní API Twilio například prostřednictvím **CallCreator.create** metody Twilio odešlete žádost na adresu URL, kterou je očekáván vrátí odpověď TwiML.</span><span class="sxs-lookup"><span data-stu-id="6654c-177">When your application initiates a call to the Twilio API, for example via the **CallCreator.create** method, Twilio will send your request to a URL that is expected to return a TwiML response.</span></span> <span data-ttu-id="6654c-178">V předchozím příkladu používá adresu URL poskytnutou Twilio [http://twimlets.com/message][twimlet_message_url].</span><span class="sxs-lookup"><span data-stu-id="6654c-178">The example above uses the Twilio-provided URL [http://twimlets.com/message][twimlet_message_url].</span></span> <span data-ttu-id="6654c-179">(Při TwiML je určen k použití pomocí webové služby, můžete zobrazit TwiML v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6654c-179">(While TwiML is designed for use by Web services, you can view the TwiML in your browser.</span></span> <span data-ttu-id="6654c-180">For example, klikněte na tlačítko [http://twimlets.com/message] [ twimlet_message_url] zobrazíte prázdnou  **&lt;odpovědi&gt;**  element; například klikněte na tlačítko [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21] [ twimlet_message_url_hello_world] zobrazíte  **&lt;odpovědi&gt;**  elementu, který obsahuje  **&lt;indikované&gt;**  elementu.)</span><span class="sxs-lookup"><span data-stu-id="6654c-180">For example, click [http://twimlets.com/message][twimlet_message_url] to see an empty **&lt;Response&gt;** element; as another example, click [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21][twimlet_message_url_hello_world] to see a **&lt;Response&gt;** element that contains a **&lt;Say&gt;** element.)</span></span>

<span data-ttu-id="6654c-181">Aniž byste museli spoléhat na adresu URL pro zadaný Twilio, můžete vytvořit vlastního webu adresu URL, která vrací odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="6654c-181">Instead of relying on the Twilio-provided URL, you can create your own URL site that returns HTTP responses.</span></span> <span data-ttu-id="6654c-182">Můžete vytvořit web v libovolném jazyce, který vrací odpovědi HTTP; Toto téma předpokládá, že budete hostování adresu URL na stránce JSP.</span><span class="sxs-lookup"><span data-stu-id="6654c-182">You can create the site in any language that returns HTTP responses; this topic assumes you'll be hosting the URL in a JSP page.</span></span>

<span data-ttu-id="6654c-183">Na následující stránce JSP výsledkem TwiML odpovědi, která uvádí, že **Hello, World!**</span><span class="sxs-lookup"><span data-stu-id="6654c-183">The following JSP page results in a TwiML response that says **Hello World!**</span></span> <span data-ttu-id="6654c-184">Při volání.</span><span class="sxs-lookup"><span data-stu-id="6654c-184">on the call.</span></span>

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

<span data-ttu-id="6654c-185">Na následující stránce JSP výsledkem TwiML odpověď, který je uveden text, má několik pozastaví a uvádí informace o verzi Twilio API a název Azure role.</span><span class="sxs-lookup"><span data-stu-id="6654c-185">The following JSP page results in a TwiML response that says some text, has several pauses, and says information about the Twilio API version and the Azure role name.</span></span>

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello from Azure!</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>
```

<span data-ttu-id="6654c-186">**ApiVersion** parametr je k dispozici v žádostech o hlasové Twilio (ne požadavků SMS).</span><span class="sxs-lookup"><span data-stu-id="6654c-186">The **ApiVersion** parameter is available in Twilio voice requests (not SMS requests).</span></span> <span data-ttu-id="6654c-187">Parametry žádosti k dispozici pro hlasové Twilio a žádosti o služby SMS najdete v sekci <https://www.twilio.com/docs/api/twiml/twilio_request> a <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6654c-187">To see the available request parameters for Twilio voice and SMS requests, see <https://www.twilio.com/docs/api/twiml/twilio_request> and <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, respectively.</span></span> <span data-ttu-id="6654c-188">**RoleName** proměnná prostředí je k dispozici jako součást nasazení služby Azure.</span><span class="sxs-lookup"><span data-stu-id="6654c-188">The **RoleName** environment variable is available as part of an Azure deployment.</span></span> <span data-ttu-id="6654c-189">(Pokud chcete přidat vlastní proměnné prostředí, aby se může být zachyceny z **System.getenv**, naleznete v části proměnných prostředí v [ostatní nastavení konfigurace Role][misc_role_config_settings].)</span><span class="sxs-lookup"><span data-stu-id="6654c-189">(If you want to add custom environment variables so they could be picked up from **System.getenv**, see the environment variables section at [Miscellaneous Role Configuration Settings][misc_role_config_settings].)</span></span>

<span data-ttu-id="6654c-190">Jakmile máte stránku JSP nastavit na, zadejte TwiML odpovědi, použijte adresu URL stránky JSP jako adresu URL předaný do **Call.creator** metoda.</span><span class="sxs-lookup"><span data-stu-id="6654c-190">Once you have your JSP page set up to provide TwiML responses, use the URL of the JSP page as the URL passed into the **Call.creator** method.</span></span> <span data-ttu-id="6654c-191">Například pokud máte webovou aplikaci s názvem MyTwiML nasadit do Azure hostované služby a název stránky JSP je mytwiml.jsp, adresa URL se dá předat do **Call.creator** jak je znázorněno v následujícím:</span><span class="sxs-lookup"><span data-stu-id="6654c-191">For example, if you have a Web application named MyTwiML deployed to an Azure hosted service, and the name of the JSP page is mytwiml.jsp, the URL can be passed to **Call.creator** as shown in the following:</span></span>

```java
    // Declare To and From numbers and the URL of your JSP page
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

<span data-ttu-id="6654c-192">Další možnost reagovat s TwiML je prostřednictvím **VoiceResponse** třída, která je k dispozici v **com.twilio.twiml** balíčku.</span><span class="sxs-lookup"><span data-stu-id="6654c-192">Another option for responding with TwiML is via the **VoiceResponse** class, which is available in the **com.twilio.twiml** package.</span></span>

<span data-ttu-id="6654c-193">Další informace o používání Twilio v Azure s Java najdete v tématu [postup proveďte v aplikaci Java v Azure Twilio pomocí telefonního hovoru][howto_phonecall_java].</span><span class="sxs-lookup"><span data-stu-id="6654c-193">For additional information about using Twilio in Azure with Java, see [How to Make a Phone Call Using Twilio in a Java Application on Azure][howto_phonecall_java].</span></span>

## <span data-ttu-id="6654c-194"><a id="AdditionalServices"></a>Postupy: použití další Twilio služeb</span><span class="sxs-lookup"><span data-stu-id="6654c-194"><a id="AdditionalServices"></a>How to: Use Additional Twilio Services</span></span>
<span data-ttu-id="6654c-195">Kromě příkladů tady uvedené nabízí Twilio rozhraní API založené na webu, který můžete využít další funkce Twilio vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="6654c-195">In addition to the examples shown here, Twilio offers web-based APIs that you can use to leverage additional Twilio functionality from your Azure application.</span></span> <span data-ttu-id="6654c-196">Úplné podrobnosti najdete v tématu [dokumentaci k rozhraní API Twilio][twilio_api_documentation].</span><span class="sxs-lookup"><span data-stu-id="6654c-196">For full details, see the [Twilio API documentation][twilio_api_documentation].</span></span>

## <span data-ttu-id="6654c-197"><a id="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="6654c-197"><a id="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="6654c-198">Teď, když jste se naučili základy služby Twilio, postupujte podle následujících odkazech na další informace:</span><span class="sxs-lookup"><span data-stu-id="6654c-198">Now that you've learned the basics of the Twilio service, follow these links to learn more:</span></span>

* <span data-ttu-id="6654c-199">[Pokyny pro zabezpečení Twilio][twilio_security_guidelines]</span><span class="sxs-lookup"><span data-stu-id="6654c-199">[Twilio Security Guidelines][twilio_security_guidelines]</span></span>
* <span data-ttu-id="6654c-200">[Twilio postupy a příklady kódu][twilio_howtos]</span><span class="sxs-lookup"><span data-stu-id="6654c-200">[Twilio HowTo's and Example Code][twilio_howtos]</span></span>
* <span data-ttu-id="6654c-201">[Kurzy Twilio rychlý start][twilio_quickstarts]</span><span class="sxs-lookup"><span data-stu-id="6654c-201">[Twilio Quickstart Tutorials][twilio_quickstarts]</span></span>
* <span data-ttu-id="6654c-202">[Twilio na Githubu][twilio_on_github]</span><span class="sxs-lookup"><span data-stu-id="6654c-202">[Twilio on GitHub][twilio_on_github]</span></span>
* <span data-ttu-id="6654c-203">[Obraťte se na podporu Twilio][twilio_support]</span><span class="sxs-lookup"><span data-stu-id="6654c-203">[Talk to Twilio Support][twilio_support]</span></span>

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
