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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-from-azure"></a>Jak toouse Twilio pro hlasový a možnosti služby SMS z Azure
Tato příručka ukazuje, jak služba tooperform běžné úlohy programování s hello Twilio rozhraní API v Azure. pokryté scénáře Hello zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS). Další informace o Twilio a použití hlasu a SMS ve svých aplikacích najdete v tématu hello [další kroky](#NextSteps) části.

## <a id="WhatIs"></a>Co je Twilio?
Twilio je pohánějící budoucí hello firmy komunikace, povolení vývojáři tooembed hlasu VoIP a zasílání zpráv do aplikace. Jejich Virtualizovat všechny infrastrukturu potřebnou v prostředí cloudu, globální vystavení prostřednictvím platformy hello Twilio komunikace rozhraní API. Aplikace jsou jednoduché toobuild a škálovatelnost. Získejte flexibilitu s průběžnými platbami ceny a využívat cloudové spolehlivost.

**Hlasové Twilio** umožňuje toomake vaší aplikace a příjem telefonních hovorů. **Twilio SMS** umožňuje toosend vaší aplikace a přijímat zprávy SMS. **Klient Twilio** vám umožní toomake VoIP volání z jakékoli telefon, tablet nebo prohlížeče a podporuje WebRTC.

## <a id="Pricing"></a>Ceny Twilio a speciálních nabídek
Přijímat Azure zákazníků [speciální nabídka](http://www.twilio.com/azure): bezplatnou 10 Twilio kreditu při upgradu vašeho účtu Twilio. Tato Twilio platební můžou být použité tooany Twilio využití (10 platební ekvivalentní toosending až 1 000 zpráv serveru SMS nebo příjem až too1000 příchozí hlasové minut v závislosti na umístění hello vaše telefonní číslo a zpráva nebo volání cíle). Uplatnit tento platební Twilio a začít na [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio je služba, průběžnými platbami. Neexistují žádné poplatky nastavení, a kdykoli můžete zavřít svůj účet. Můžete najít další podrobnosti o [Twilio ceny](http://www.twilio.com/voice/pricing).

## <a id="Concepts"></a>Koncepty
Hello Twilio rozhraní API je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace. Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].

Klíčové aspekty hello Twilio API jsou příkazy Twilio a Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Příkazy Twilio
Hello API využívá Twilio příkazy; například hello  **&lt;indikované&gt;**  příkaz dá pokyn Twilio tooaudibly doručit zprávu na volání.

Hello následuje seznam operací Twilio.  Další informace o hello jiné příkazy a možnosti prostřednictvím [Twilio Markup Language dokumentaci](http://www.twilio.com/docs/api/twiml).

* **&lt;Vytočit&gt;**: připojí hello volající tooanother phone.
* **&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu hello.
* **&lt;Zavěšení&gt;**: ukončí volání.
* **&lt;Přehrání&gt;**: přehraje zvukový soubor.
* **&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.
* **&lt;Záznam&gt;**: zaznamenává hlasové hello volajícího a vrátí adresu URL souboru, který obsahuje záznam hello.
* **&lt;Přesměrování&gt;**: přenáší kontrolu nad telefonického hovoru nebo SMS toohello TwiML na jinou adresu URL.
* **&lt;Odmítnout&gt;**: odmítne na příchozí volání číslo Twilio tooyour bez fakturace můžete
* **&lt;Řekněme&gt;**: toospeech převede text, že z volání.
* **&lt;SMS&gt;**: odešle zprávu SMS.

### <a id="TwiML"></a>TwiML
TwiML je sada založený na jazyce XML pokyny podle hello Twilio příkazy, které informovat o tom, jak Twilio tooprocess hovoru nebo SMS.

Jako příklad by hello následující TwiML převést hello text **Hello, World** toospeech.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

Pokud vaše aplikace volání hello Twilio rozhraní API, jeden z parametrů hello rozhraní API je hello adresu URL, který vrátí odpověď TwiML hello. Pro účely vývoje můžete použít adresy URL zadané Twilio tooprovide hello TwiML odpovědí použít v aplikacích. Může taky hostovat vlastní adresy URL tooproduce hello TwiML odpovědí, a další možností je toouse hello **TwiMLResponse** objektu.

Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml]. Další informace o hello Twilio API najdete v tématu [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Vytvoření účtu Twilio
Když jste připravené tooget Twilio účet, zaregistrovat na [zkuste Twilio][try_twilio]. Můžete začít s bezplatný účet a upgradujte si účet později.

Když se zaregistrujete k účtu Twilio, obdržíte ID účtu a ověřovací token. Jak bude volání rozhraní API Twilio potřebné toomake. tooprevent neoprávněný přístup k účtu tooyour, ověřovací token zabezpečení. ID účtu a ověřování tokenu je možné zobrazit v hello [stránku účtu Twilio][twilio_account]v hello pole s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.

## <a id="create_app"></a>Vytvoření aplikace Azure
Aplikaci Azure, který je hostitelem aplikace Twilio povoleno se neliší od všech aplikací Azure. Přidejte knihovny Twilio .NET hello a nakonfigurujte hello role toouse hello Twilio .NET knihovny.
Informace o vytváření počáteční projektu Azure najdete v tématu [vytvoření projektu Azure pomocí sady Visual Studio][vs_project].

## <a id="configure_app"></a>Konfigurace knihovny Twilio toouse vaše aplikace
Twilio poskytuje sadu knihovny .NET pomocné rutiny, které balí různé aspekty Twilio tooprovide jednoduchý a snadno způsoby toointeract s hello Twilio REST API a Twilio klienta toogenerate TwiML odpovědi.

Twilio obsahuje pět knihoven pro vývojáře .NET:
Knihovna|Popis
---|---
Twilio.API|Hello základní Twilio knihovny, která zabalí hello Twilio REST API v popisný knihovny .NET. Tato knihovna je k dispozici pro rozhraní .NET, Silverlight a Windows Phone 7.
Twilio.TwiML|Poskytuje rozhraní .NET popisný způsob toogenerate TwiML značky.
Twilio.MVC|Pro vývojáře, kteří používají rozhraní ASP.NET MVC zahrnuje tato knihovna TwilioController, TwiML ActionResult a atribut ověření žádosti.
Twilio.WebMatrix|Tato knihovna pro vývojáře, kteří používají nástroj pro vývoj volné WebMatrix společnosti Microsoft, obsahuje Pomocníci syntaxe Razor pro různé akce Twilio.
Twilio.Client.Capability|Obsahuje tokenů generátor hello schopností pro použití s hello Twilio klienta JavaScript SDK.

Upozorňujeme, že všechny knihovny vyžaduje rozhraní .NET 3.5, Silverlight 4 nebo Windows Phone 7 nebo novější.

Ukázky Hello uvedené v tomto průvodci použijte knihovnu Twilio.API hello.

může být Hello knihovny [nainstalovat pomocí rozšíření Správce balíčků NuGet hello](http://www.twilio.com/docs/csharp/install) k dispozici pro Visual Studio 2010 až too2015.  Hello zdrojový kód je hostované na [Githubu][twilio_github_repo], což zahrnuje Wiki, který obsahuje kompletní dokumentaci pro používání knihovny hello.

Ve výchozím nastavení nainstaluje Microsoft Visual Studio 2010 verze 1.2 NuGet. Instalace hello Twilio knihovny vyžaduje verzi 1.6 NuGet nebo vyšší. Informace o instalaci nebo aktualizaci NuGet najdete v tématu [http://nuget.org/][nuget].

> [!NOTE]
> tooinstall hello nejnovější verze balíčku NuGet, musíte nejprve odinstalovat hello načíst verzi pomocí hello Správce rozšíření Visual Studio. toodo Ano, musíte Visual Studio spustit jako správce. V opačném hello k dispozici tlačítko odinstalovat.
>
>

### <a id="use_nuget"></a>tooadd hello Twilio knihovny tooyour projektu sady Visual Studio:
1. Otevřete řešení v sadě Visual Studio.
2. Klikněte pravým tlačítkem na **odkazy**.
3. Klikněte na tlačítko **spravovat balíčky NuGet...**
4. Klikněte na tlačítko **Online**.
5. Zadejte online pole hledání hello *twilio*.
6. Klikněte na tlačítko **nainstalovat** na hello Twilio balíčku.

## <a id="howto_make_call"></a>Postupy: volání odchozí
Hello následující ukazuje, jak toomake odchozí volat pomocí hello **CallResource** třídy. Tento kód také používá zadaný Twilio lokality tooreturn hello odpovědi Twilio Markup Language (TwiML). Dosaďte svoje hodnoty hello **k** a **z** telefonních čísel a ujistěte se, abyste ověřili hello **z** telefonní číslo pro svůj účet Twilio před spuštěním kódu hello.

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

Další informace o parametrech hello předaná toohello **CallResource.Create** metodu, najdete v části [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Jak je uvedeno, tento kód používá zadaný Twilio lokality tooreturn hello TwiML odpovědi. Místo toho můžete použít vlastní lokality tooprovide hello TwiML odpovědi. Další informace najdete v tématu [postup: Zadejte TwiML odpovědí z vlastního webu](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS
Hello následující snímek obrazovky ukazuje, jak toosend zprávu SMS pomocí hello **MessageResource** třídy. Hello **z** číslo poskytne Twilio pro zkušební verzi účty toosend zpráv serveru SMS. Hello **k** musí ověřit číslo účtu Twilio můžete před spuštěním kódu hello.

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

## <a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu
Pokud vaše aplikace iniciuje toohello volání rozhraní API Twilio – například prostřednictvím hello **CallResource.Create** metoda - Twilio odešle tooan adresu URL vašeho požadavku, která je očekávané tooreturn TwiML odpověď. Příklad Hello v [postupy: volání odchozích](#howto_make_call) používá hello URL poskytnutou Twilio [http://twimlets.com/message] [ twimlet_message_url] tooreturn hello odpovědi.

> [!NOTE]
> Při TwiML je určen k použití pomocí webové služby, můžete zobrazit hello TwiML v prohlížeči. Klikněte například na [http://twimlets.com/message] [ twimlet_message_url] toosee prázdnou &lt;odpovědi&gt; element; například klikněte na tlačítko [http://twimlets.com/message ? Zpráva % 5B0 %5 D = Hello % 20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) toosee &lt;odpovědi&gt; elementu, který obsahuje &lt;indikované&gt; element.
>
>

Aniž byste museli spoléhat na URL poskytnutou Twilio hello, můžete vytvořit vlastního webu adresu URL, která vrací odpovědi HTTP. Hello lokality můžete vytvořit v libovolném jazyce, který vrací odpovědi HTTP. Toto téma předpokládá, že budete hostování hello URL z obecné obslužné rutiny ASP.NET.

následující popisovač ASP.NET Hello sestavuje TwiML odpovědi, která uvádí, že **Hello, World** při volání hello.

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
    
Jak je vidět z výše uvedeném příkladu hello hello TwiML odpovědi je jednoduše dokument XML. Knihovna Twilio.TwiML Hello obsahuje třídy, které způsobí vygenerování TwiML za vás. Hello následující příklad vytvoří ekvivalentní odpovědi hello jako v příkladu nahoře, ale používá hello **VoiceResponse** třídy.

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

Další informace o TwiML najdete v tématu [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).

Jakmile jste nastavili způsob tooprovide TwiML odpovědi, můžete předat tuto adresu URL toohello **CallResource.Create** metoda. Například pokud máte webovou aplikaci s názvem tooan MyTwiML nasazení cloudové služby Azure a hello názvem vaší obslužné rutiny ASP.NET je mytwiml.ashx, adresa URL hello lze předat příliš**CallResource.Create** jak je znázorněno v následujícím kódu hello Ukázka:

    // This sample uses hello sandbox number provided by Twilio toomake hello call.
    // Place hello call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
        }

Další informace o používání Twilio v Azure s ASP.NET najdete v tématu [jak toomake telefonní hovor pomocí Twilio ve webové roli v Azure][howto_phonecall_dotnet].

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
