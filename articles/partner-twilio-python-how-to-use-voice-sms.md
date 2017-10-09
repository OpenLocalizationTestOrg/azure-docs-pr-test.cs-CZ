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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-python"></a>Jak tooUse Twilio pro hlasový a možnosti serveru SMS v Pythonu
Tato příručka ukazuje, jak služba tooperform běžné úlohy programování s hello Twilio rozhraní API v Azure. pokryté scénáře Hello zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS). Další informace o Twilio a použití hlasu a SMS ve svých aplikacích najdete v tématu hello [další kroky](#NextSteps) části.

## <a id="WhatIs"></a>Co je Twilio?
Twilio je pohánějící budoucí hello firmy komunikace, povolení vývojáři tooembed hlasu VoIP a zasílání zpráv do aplikace. Jejich Virtualizovat všechny infrastrukturu potřebnou v prostředí cloudu, globální vystavení prostřednictvím platformy hello Twilio komunikace rozhraní API. Aplikace jsou jednoduché toobuild a škálovatelnost. Získejte flexibilitu s platím jako vámi přejděte ceny a těžit z cloudu spolehlivost.

**Hlasové Twilio** umožňuje toomake vaší aplikace a příjem telefonních hovorů.
**Twilio SMS** umožňuje toosend vaší aplikace a přijímat textové zprávy.
**Klient Twilio** vám umožní toomake VoIP volání z jakékoli telefon, tablet nebo prohlížeče a podporuje WebRTC.

## <a id="Pricing"></a>Ceny Twilio a speciálních nabídek
Přijímat Azure zákazníků [speciální nabídka] [ special_offer] 10 Twilio kreditu při upgradu vašeho účtu Twilio. Tato Twilio platební můžou být použité tooany Twilio využití (10 platební ekvivalentní toosending až 1 000 zpráv serveru SMS nebo příjem až too1000 příchozí hlasové minut v závislosti na umístění hello vaše telefonní číslo a zpráva nebo volání cíle). To uplatnit [Twilio platební] [ special_offer] a začít.

Twilio je služba, průběžnými platbami. Neexistují žádné poplatky nastavení a kdykoli můžete zavřít svůj účet. Můžete najít další podrobnosti o [Twilio ceny][twilio_pricing].

## <a id="Concepts"></a>Koncepty
Hello Twilio rozhraní API je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace. Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].

Klíčové aspekty hello Twilio API jsou příkazy Twilio a Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Příkazy Twilio
Hello API využívá Twilio příkazy; například hello  **&lt;indikované&gt;**  příkaz dá pokyn Twilio tooaudibly doručit zprávu na volání.

Hello následuje seznam operací Twilio. Další informace o hello jiné příkazy a možnosti prostřednictvím [Twilio Markup Language dokumentaci][twiml].

* **&lt;Vytočit&gt;**: připojí hello volající tooanother phone.
* **&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu hello.
* **&lt;Zavěšení&gt;**: ukončí volání.
* **&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.
* **&lt;Přehrání&gt;**: přehraje zvukový soubor.
* **&lt;Fronty&gt;**: přidejte hello tooa frontu volající.
* **&lt;Záznam&gt;**: zaznamenává hello hlasové hello volajícího a vrátí adresu URL souboru, který obsahuje záznam hello.
* **&lt;Přesměrování&gt;**: přenáší kontrolu nad telefonického hovoru nebo SMS toohello TwiML na jinou adresu URL.
* **&lt;Odmítnout&gt;**: odmítne na příchozí volání bez fakturace můžete tooyour Twilio číslo.
* **&lt;Řekněme&gt;**: toospeech převede text, že z volání.
* **&lt;SMS&gt;**: odešle zprávu SMS.

### <a id="TwiML"></a>TwiML
TwiML je sada založený na jazyce XML pokyny podle hello Twilio příkazy, které informovat o tom, jak Twilio tooprocess hovoru nebo SMS.

Jako příklad by hello následující TwiML převést hello text **Hello, World** toospeech.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

Pokud vaše aplikace volání hello Twilio rozhraní API, jeden z parametrů hello rozhraní API je hello adresu URL, který vrátí odpověď TwiML hello. Pro účely vývoje můžete použít adresy URL zadané Twilio tooprovide hello TwiML odpovědí použít v aplikacích. Může taky hostovat vlastní adresy URL tooproduce hello TwiML odpovědí, a další možností je toouse hello `TwiMLResponse` objektu.

Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml]. Další informace o hello Twilio API najdete v tématu [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Vytvoření účtu Twilio
Po připravené tooget účtu Twilio zaregistrovat na [zkuste Twilio][try_twilio]. Můžete začít s bezplatný účet a upgradujte si účet později.

Když zaregistrujete k účtu Twilio, zobrazí se účet SID a ověřovací token. Jak bude volání rozhraní API Twilio potřebné toomake. tooprevent neoprávněný přístup k účtu tooyour, ověřovací token zabezpečení. SID účtu a ověřovací token lze zobrazit v hello [Twilio konzoly][twilio_console]v hello pole s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.

## <a id="create_app"></a>Vytvoření aplikace Python
Aplikace Python, která používá službu hello Twilio a běží v Azure je nejsou jiné než jiná aplikace Python, která používá službu Twilio hello. Při Twilio services jsou založené na REST a lze volat z Pythonu několika způsoby, v tomto článku se soustředí na jak toouse Twilio služeb s [Twilio knihovny pro jazyk Python z webu GitHub][twilio_python]. Další informace o používání knihovny hello Twilio pro jazyk Python najdete v tématu [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].

První s názvem [nastavení nového virtuálního počítače Azure Linux] [azure_vm_setup] tooact jako hostitel pro novou webovou aplikaci Python. Jakmile hello je spuštěný virtuální počítač, budete potřebovat tooexpose vaší aplikace na veřejném portu jak je popsáno níže.

### <a name="add-an-incoming-rule"></a>Přidat příchozí pravidlo
  1. Přejděte toohello [skupinu zabezpečení sítě] [azure_nsg] stránky.
  2. Vyberte hello skupinu zabezpečení sítě odpovídající s virtuálním počítačem.
  3. Přidat a **odchozí pravidlo** pro **port 80**. Být jisti tooallow příchozí z libovolné adresy.

### <a name="set-hello-dns-name-label"></a>Nastavit hello Popisek názvu DNS
  1. Přejděte toohello [hello veřejné IP adresy] [azure_ips] stránky.
  2. Vyberte hello veřejná IP adresa tohoto correspends s virtuálním počítačem.
  3. Sada hello **Popisek názvu DNS** v hello **konfigurace** části. V případě hello tohoto příkladu bude vypadat přibližně takto *vaše popisek domény*. centralus.cloudapp.azure.com

Jakmile se vám daří tooconnect prostřednictvím SSH toohello virtuálního počítače můžete nainstalovat hello webová architektura podle vašeho výběru (hello nejvíce známých v Pythonu ve dvou [Flask](http://flask.pocoo.org/) a [Django](https://www.djangoproject.com)). Můžete nainstalovat buď z nich právě spuštěním hello `pip install` příkaz.

Uvědomte si, že jsme nakonfigurovali přenosy tooallow virtuálních počítačů hello pouze na portu 80. Proto ujistěte se, že toouse aplikace hello tooconfigure tento port.

## <a id="configure_app"></a>Konfigurace knihovny Twilio tooUse vaše aplikace
Vaše aplikace toouse hello Twilio knihovna pro jazyk Python můžete nakonfigurovat dvěma způsoby:

* Jako balíček Pip nainstalujte hello Twilio knihovny pro jazyk Python. Dají se instalovat s hello následující příkazy:
   
        $ pip install twilio

    - nebo -

* Stáhnout hello Twilio knihovny pro jazyk Python z webu GitHub ([https://github.com/twilio/twilio-python][twilio_python]) a nainstalujte ho takto:

        $ python setup.py install

Po instalaci hello Twilio knihovny pro jazyk Python, můžete pak `import` ho v souborech Python:

        import twilio

Další informace najdete v tématu [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Postupy: volání odchozí
Hello následující ukazuje, jak volat toomake odchozí. Tento kód také používá zadaný Twilio lokality tooreturn hello odpovědi Twilio Markup Language (TwiML). Dosaďte svoje hodnoty hello **from_number** a **to_number** telefonních čísel a ujistěte se, že ověříte hello **from_number** telefonní číslo pro svůj účet Twilio Před spuštěním kódu hello.

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

Jak je uvedeno, tento kód používá zadaný Twilio lokality tooreturn hello TwiML odpovědi. Místo toho můžete použít vlastní lokality tooprovide hello TwiML odpovědi; Další informace najdete v tématu [jak tooProvide TwiML odpovědí z vaše vlastní webové stránky](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS
Hello následující ukazuje, jak toosend zprávu SMS pomocí hello `TwilioRestClient` třídy. Hello **from_number** číslo poskytne Twilio pro zkušební verzi účty toosend zpráv serveru SMS. Hello **to_number** číslo musí ověřit účtu Twilio před spuštěním kódu hello.

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

## <a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu
Pokud vaše aplikace zahájí toohello volání rozhraní API Twilio, budou odeslány Twilio tooa adresu URL vašeho požadavku, která je očekáván tooreturn TwiML odpověď. výše uvedený příklad Hello používá URL poskytnutou Twilio hello [http://twimlets.com/message][twimlet_message_url]. (Při TwiML je určen pro Twilio, můžete ji zobrazit v prohlížeči. Klikněte například na [http://twimlets.com/message] [ twimlet_message_url] toosee prázdnou `<Response>` element; například klikněte na tlačítko [http://twimlets.com/message? Zprávy % 5B0 %5 D = Hello % 20World] [ twimlet_message_url_hello_world] toosee `<Response>` elementu, který obsahuje `<Say>` elementu.)

Aniž byste museli spoléhat na URL poskytnutou Twilio hello, můžete vytvořit vlastního webu, který vrací odpovědi HTTP. Můžete vytvořit hello lokality v libovolném jazyce, který vrací odpovědí XML; Toto téma předpokládá, že se používá Python toocreate hello TwiML.

Hello následující příklady výstup TwiML odpovědi, která uvádí, že **Hello, World** při volání hello.

S Flask:

    from flask import Response
    @app.route("/")
    def hello():
        xml = '<Response><Say>Hello world.</Say></Response>'
        return Response(xml, mimetype='text/xml')

Pomocí rozhraní Django:

    from django.http import HttpResponse
    def hello(request):
        xml = '<Response><Say>Hello world.</Say></Response>'
        return HttpResponse(xml, content_type='text/xml')

Jak je vidět z výše uvedeném příkladu hello hello TwiML odpovědi je jednoduše dokument XML. Knihovna Hello Twilio pro jazyk Python obsahuje třídy, které způsobí vygenerování TwiML za vás. Hello následující příklad vytvoří ekvivalentní odpovědi hello jako v příkladu nahoře, ale používá hello `twiml` modulu v hello knihovně Twilio pro jazyk Python:

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

Další informace o TwiML najdete v tématu [https://www.twilio.com/docs/api/twiml][twiml_reference].

Až budete mít aplikaci Python nastavit tooprovide TwiML odpovědi, použijte adresu URL hello hello aplikace jako adresa URL předaná do hello hello `client.calls.create` metoda. Například, pokud máte webovou aplikaci s názvem **MyTwiML** nasazené tooan Azure hostovanou službu, můžete použít jeho adresa url jako webhooku jak ukazuje následující příklad hello:

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

## <a id="AdditionalServices"></a>Postupy: použití další Twilio služeb
Kromě toho toohello příklady zobrazeny zde, že twilio nabízí webové rozhraní API, které můžete použít další funkce Twilio tooleverage z vaše aplikace Azure. Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API Twilio][twilio_api].

## <a id="NextSteps"></a>Další kroky
Teď, když jste se naučili základy hello hello Twilio služby, použijte tyto odkazy toolearn Další:

* [Pokyny pro zabezpečení Twilio][twilio_security_guidelines]
* [Příručky Twilio postupy a příklady kódu][twilio_howtos]
* [Kurzy Twilio rychlý start][twilio_quickstarts]
* [Twilio na Githubu][twilio_on_github]
* [Komunikovat tooTwilio podpory][twilio_support]

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
