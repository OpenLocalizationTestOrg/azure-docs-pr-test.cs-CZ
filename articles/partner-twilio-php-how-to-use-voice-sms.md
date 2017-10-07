---
title: "aaaHow tooUse Twilio pro hlasový a serveru SMS (PHP) | Microsoft Docs"
description: "Zjistěte, jak toomake telefonní hovor a odesílání SMS zprávy službou hello Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořeny v jazyce PHP."
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
ms.openlocfilehash: 9354df8de694826a0ff7ea92620ec4d7e5c2fd70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-php"></a>Jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce PHP
Tato příručka ukazuje, jak služba tooperform běžné úlohy programování s hello Twilio rozhraní API v Azure. pokryté scénáře Hello zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS). Další informace o Twilio a použití hlasu a SMS ve svých aplikacích najdete v tématu hello [další kroky](#NextSteps) části.

## <a id="WhatIs"></a>Co je Twilio?
Twilio je pohánějící budoucí hello firmy komunikace, povolení vývojáři tooembed hlasu VoIP a zasílání zpráv do aplikace. Jejich Virtualizovat všechny infrastrukturu potřebnou v prostředí cloudu, globální vystavení prostřednictvím platformy hello Twilio komunikace rozhraní API. Aplikace jsou jednoduché toobuild a škálovatelnost. Získejte flexibilitu s platím jako vámi přejděte ceny a těžit z cloudu spolehlivost.

**Hlasové Twilio** umožňuje toomake vaší aplikace a příjem telefonních hovorů. **Twilio SMS** umožňuje toosend vaší aplikace a přijímat textové zprávy. **Klient Twilio** vám umožní toomake VoIP volání z jakékoli telefon, tablet nebo prohlížeče a podporuje WebRTC.

## <a id="Pricing"></a>Ceny Twilio a speciálních nabídek
Přijímat Azure zákazníků [speciální nabídka](http://www.twilio.com/azure): bezplatnou 10 Twilio kreditu při upgradu vašeho účtu Twilio. Tato Twilio platební můžou být použité tooany Twilio využití (10 platební ekvivalentní toosending až 1 000 zpráv serveru SMS nebo příjem až too1000 příchozí hlasové minut v závislosti na umístění hello vaše telefonní číslo a zpráva nebo volání cíle). Uplatnit tento platební Twilio a začít v: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio je služba, průběžnými platbami. Neexistují žádné poplatky nastavení a kdykoli můžete zavřít svůj účet. Můžete najít další podrobnosti o [Twilio ceny][twilio_pricing].

## <a id="Concepts"></a>Koncepty
Hello Twilio rozhraní API je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace. Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].

Klíčové aspekty hello Twilio API jsou příkazy Twilio a Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Příkazy Twilio
Hello API využívá Twilio příkazy; například hello  **&lt;indikované&gt;**  příkaz dá pokyn Twilio tooaudibly doručit zprávu na volání.

Hello následuje seznam operací Twilio. Další informace o hello jiné příkazy a možnosti prostřednictvím [Twilio Markup Language dokumentaci](http://www.twilio.com/docs/api/twiml).

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

## <a id="create_app"></a>Vytvoření aplikace PHP
Aplikace PHP, která používá službu hello Twilio a běží v Azure je nejsou jiné než jiná aplikace PHP, která používá službu Twilio hello. Při Twilio services jsou založené na REST a lze volat z PHP několika způsoby, v tomto článku se soustředí na jak toouse Twilio služeb s [Twilio knihovny pro jazyk PHP z Githubu][twilio_php]. Další informace o používání knihovny hello Twilio pro jazyk PHP najdete v tématu [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Podrobné pokyny pro vytváření a nasazování tooAzure Twilio nebo PHP aplikace jsou k dispozici v [jak tooMake Twilio pomocí telefonního hovoru v aplikaci PHP v Azure][howto_phonecall_php].

## <a id="configure_app"></a>Konfigurace knihovny Twilio tooUse vaše aplikace
Vaše aplikace toouse hello Twilio knihovna pro jazyk PHP můžete nakonfigurovat dvěma způsoby:

1. Stáhnout hello Twilio knihovny pro jazyk PHP z webu GitHub ([https://github.com/twilio/twilio-php][twilio_php]) a přidejte hello **služby** directory tooyour aplikace.
   
    - nebo -
2. Nainstalujte hello Twilio knihovny pro jazyk PHP jako balíček HRUŠKAMI. Dají se instalovat s hello následující příkazy:
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Po instalaci hello Twilio knihovny pro jazyk PHP, poté můžete přidat **require_once** příkaz v horní části hello vaší PHP soubory tooreference hello knihovně:

        require_once 'Services/Twilio.php';

Další informace najdete v tématu [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Postupy: volání odchozí
Hello následující ukazuje, jak toomake odchozí volat pomocí hello **Services_Twilio** třídy. Tento kód také používá zadaný Twilio lokality tooreturn hello odpovědi Twilio Markup Language (TwiML). Dosaďte svoje hodnoty hello **z** a **k** telefonních čísel a ujistěte se, abyste ověřili hello **z** telefonní číslo kódu hello předchozí toorunning účtu Twilio.

    // Include hello Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // hello number of hello phone initiating hello hello call.
    $from_number = "NNNNNNNNNNN";

    // hello number of hello phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use hello Twilio-provided site for hello TwiML response.
    $url = "http://twimlets.com/message";

    // hello phone message text.
    $message = "Hello world.";

    // Create hello call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make hello call.
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

Jak je uvedeno, tento kód používá zadaný Twilio lokality tooreturn hello TwiML odpovědi. Místo toho můžete použít vlastní lokality tooprovide hello TwiML odpovědi; Další informace najdete v tématu [jak tooProvide TwiML odpovědí z vaše vlastní webové stránky](#howto_provide_twiml_responses).

* **Poznámka:**: tootroubleshoot chyby ověření certifikátu SSL, najdete v části [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation] 

## <a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS
Hello následující ukazuje, jak toosend zprávu SMS pomocí hello **Services_Twilio** třídy. Hello **z** číslo poskytne Twilio pro zkušební verzi účty toosend zpráv serveru SMS. Hello **k** číslo musí ověřit kódu hello předchozí toorunning účtu Twilio.

    // Include hello Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create hello call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send hello SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu
Pokud vaše aplikace zahájí toohello volání rozhraní API Twilio, budou odeslány Twilio tooa adresu URL vašeho požadavku, která je očekáván tooreturn TwiML odpověď. výše uvedený příklad Hello používá URL poskytnutou Twilio hello [http://twimlets.com/message][twimlet_message_url]. (Při TwiML je určen pro Twilio, můžete zobrazit hello v prohlížeči. Klikněte například na [http://twimlets.com/message] [ twimlet_message_url] toosee prázdnou `<Response>` element; například klikněte na tlačítko [http://twimlets.com/message? Zprávy % 5B0 %5 D = Hello % 20World] [ twimlet_message_url_hello_world] toosee `<Response>` elementu, který obsahuje `<Say>` elementu.)

Aniž byste museli spoléhat na URL poskytnutou Twilio hello, můžete vytvořit vlastního webu, který vrací odpovědi HTTP. Můžete vytvořit hello lokality v libovolném jazyce, který vrací odpovědí XML; Toto téma předpokládá, že budete používat PHP toocreate hello TwiML.

Hello následující stránky PHP za následek TwiML odpovědi, která uvádí, že **Hello, World** při volání hello.

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

Jak je vidět z výše uvedeném příkladu hello hello TwiML odpovědi je jednoduše dokument XML. Knihovna Hello Twilio pro jazyk PHP obsahuje třídy, které způsobí vygenerování TwiML za vás. Následující příklad Hello vytvoří ekvivalentní odpovědi hello jako v příkladu nahoře, ale používá hello **služby\_Twilio\_Twiml** – třída v hello knihovně Twilio pro PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Další informace o TwiML najdete v tématu [https://www.twilio.com/docs/api/twiml][twiml_reference]. 

Jakmile máte stránku PHP, nastavení tooprovide TwiML odpovědi, použijte hello URL stránky PHP hello jako adresa URL předaná do hello hello `Services_Twilio->account->calls->create` metoda. Například, pokud máte webovou aplikaci s názvem **MyTwiML** nasazené tooan Azure hostovaná služba a je hello název stránky PHP hello **mytwiml.php**, adresa URL může být předán příliš hello **Services_ Twilio -> účet -> volání -> vytvořit** jak ukazuje následující příklad hello:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // hello phone message text.
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

Další informace o používání Twilio v Azure s PHP najdete v tématu [jak tooMake Twilio pomocí telefonního hovoru v aplikaci PHP v Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Postupy: použití další Twilio služeb
Kromě toho toohello příklady zobrazeny zde, že twilio nabízí webové rozhraní API, které můžete použít další funkce Twilio tooleverage z vaše aplikace Azure. Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API Twilio][twilio_api_documentation].

## <a id="NextSteps"></a>Další kroky
Teď, když jste se naučili základy hello hello Twilio služby, použijte tyto odkazy toolearn Další:

* [Pokyny pro zabezpečení Twilio][twilio_security_guidelines]
* [Twilio postupy a příklady kódu][twilio_howtos]
* [Kurzy Twilio rychlý start][twilio_quickstarts] 
* [Twilio na Githubu][twilio_on_github]
* [Komunikovat tooTwilio podpory][twilio_support]

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
