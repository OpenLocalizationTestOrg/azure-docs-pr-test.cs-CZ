---
title: "aaaHow tooUse Twilio pro hlasový a serveru SMS (Ruby) | Microsoft Docs"
description: "Zjistěte, jak toomake telefonní hovor a odesílání SMS zprávy službou hello Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořeny v Ruby."
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
ms.openlocfilehash: aca5ccb4663ff03c9c1f39c848469415f06dfb45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Jak tooUse Twilio pro hlasový a možnosti serveru SMS v Ruby
Tato příručka ukazuje, jak služba tooperform běžné úlohy programování s hello Twilio rozhraní API v Azure. pokryté scénáře Hello zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS). Další informace o Twilio a použití hlasu a SMS ve svých aplikacích najdete v tématu hello [další kroky](#NextSteps) části.

## <a id="WhatIs"></a>Co je Twilio?
Twilio je telefonickou rozhraní API webové služby, které umožňuje použít existující webové jazyky a dovednosti toobuild hlas a aplikace serveru SMS. Twilio je služba třetích stran (ne funkcí platformy Azure a produkt společnosti Microsoft).

**Hlasové Twilio** umožňuje toomake vaší aplikace a příjem telefonních hovorů. **Twilio SMS** umožňuje toomake vaší aplikace a přijímat zprávy SMS. **Klient Twilio** umožňuje aplikace tooenable hlasové komunikaci pomocí existujícího připojení k Internetu, včetně mobilních připojení.

## <a id="Pricing"></a>Ceny Twilio a speciálních nabídek
Informace o cenách Twilio je k dispozici na [Twilio ceny][twilio_pricing]. Přijímat Azure zákazníků [speciální nabídka][special_offer]: volné platební 1000 textů nebo 1000 příchozí minut. toosign pro toto nabízejí nebo získat další informace, navštivte [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Koncepty
Hello Twilio rozhraní API je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace. Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML je sada pokynů formátu XML, které informovat o tom, jak Twilio tooprocess hovoru nebo SMS.

Jako příklad by hello následující TwiML převést hello text **Hello, World** toospeech.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Mají všechny dokumenty TwiML `<Response>` jako kořenový element. Zde můžete použít příkazy Twilio toodefine hello chování vaší aplikace.

### <a id="Verbs"></a>Příkazy TwiML
Příkazy Twilio jsou značky XML, které informují Twilio, co příliš**provést**. Například hello  **&lt;indikované&gt;**  příkaz dá pokyn Twilio tooaudibly doručit zprávu na volání. 

Hello následuje seznam operací Twilio.

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

Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml]. Další informace o hello Twilio API najdete v tématu [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Vytvoření účtu Twilio
Když jste připravené tooget Twilio účet, zaregistrovat na [zkuste Twilio][try_twilio]. Můžete začít s bezplatný účet a upgradujte si účet později.

Když zaregistrujete k účtu Twilio, získáte bezplatné telefonní číslo pro vaši aplikaci. Také budete dostávat účet SID a tokenu ověřování. Jak bude volání rozhraní API Twilio potřebné toomake. tooprevent neoprávněný přístup k účtu tooyour, ověřovací token zabezpečení. SID účtu a ověření tokenu jsou viditelná na hello [stránku účtu Twilio][twilio_account]v hello pole s názvem bez přípony **SID účtu** a **ověřování TOKENU** , v uvedeném pořadí.

### <a id="VerifyPhoneNumbers"></a>Zkontrolujte telefonní čísla
V přidání toohello číslo, které jsou uvedeny ve Twilio můžete si taky ověřit čísla řízení (tj. vaše mobilní telefon nebo Domovská telefonní číslo) pro použití v aplikacích. 

Informace o tom najdete v části tooverify telefonní číslo, [spravovat čísla][verify_phone].

## <a id="create_app"></a>Vytvoření Ruby aplikace
Poznámky Ruby aplikace, která používá službu hello Twilio a běží v Azure je nejsou jiné než jiná Ruby aplikace, která používá službu Twilio hello. Při Twilio služby jsou dosáhl standardu RESTful a lze volat z Ruby několika způsoby, v tomto článku se soustředí na jak toouse Twilio služeb s [Twilio pomocné knihovny pro Ruby][twilio_ruby].

První, [nastavení nového virtuálního počítače Azure Linux] [ azure_vm_setup] tooact jako hostitele pro nový Ruby webové aplikace. Ignorujte hello kroky zahrnující hello vytvoření aplikace které právě hello nastavení virtuálního počítače. Ujistěte se, že vytvoříte koncový bod s externí port 80 a interní port 5000.

V následujících příkladech hello, použijeme [Sinatra][sinatra], velmi jednoduché webové rozhraní pro Ruby. Ale určitě můžete hello Twilio pomocné knihovny pro Ruby s jakékoli jiné webové rozhraní, včetně Ruby, na které.

SSH do nového virtuálního počítače a vytvořte adresář pro nové aplikace. V tomto adresáři vytvořte soubor s názvem hello Gemfile a zkopírujte do ní následující kód:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Na příkazovém řádku hello spustit `bundle install`. Nainstaluje hello závislostí uvedených výše. Dále vytvořte soubor s názvem `web.rb`. Bude jím, kde je umístěn hello kódu pro webovou aplikaci. Vložte následující kód do ní hello:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

V tomto okamžiku by měl být schopný hello spustit hello příkaz `ruby web.rb -p 5000`. To bude roztočení malé webový server na portu 5000. Byste měli mít toobrowse toothis aplikace v prohlížeči návštěvou hello URL nastavení pro virtuální počítač Azure. Jakmile se můžete dostat vaší webové aplikace v prohlížeči hello, jste připravené toostart vytváření aplikace Twilio.

## <a id="configure_app"></a>Nakonfigurujte si aplikace tooUse Twilio
Můžete nakonfigurovat své webové aplikace toouse hello Twilio knihovny aktualizace vašeho `Gemfile` tooinclude tento řádek:

    gem 'twilio-ruby'

Na příkazovém řádku hello spusťte `bundle install`. Nyní otevřete `web.rb` a včetně tento řádek v horní části hello:

    require 'twilio-ruby'

Jste nyní všechny nastavit toouse hello Twilio pomocné knihovny pro Ruby ve vaší webové aplikaci.

## <a id="howto_make_call"></a>Postupy: volání odchozí
Hello následující ukazuje, jak volat toomake odchozí. Klíčové koncepty zahrnují pomocí hello Twilio pomocné knihovny pro poznámky Ruby toomake, které volání rozhraní REST API a vykreslování TwiML. Dosaďte svoje hodnoty hello **z** a **k** telefonních čísel a ujistěte se, abyste ověřili hello **z** telefonní číslo kódu hello předchozí toorunning účtu Twilio.

Přidejte tuto funkci příliš`web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # hello number of hello phone initiating hello hello call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # hello number of hello phone receiving call.
    too= "NNNNNNNNNNN";

    # Use hello Twilio-provided site for hello TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";

    get '/make_call' do
      # Create hello call client.
      client = Twilio::REST::Client.new(sid, token);

      # Make hello call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end

Pokud otevřete až `http://yourdomain.cloudapp.net/make_call` v prohlížeči, který aktivuje hello volání toohello Twilio API toomake hello telefonního hovoru. Hello první dva parametry v `client.account.calls.create` poměrně není potřeba vysvětlovat: počet volání hello hello je `from` a počet volání hello hello je `to`. 

třetí parametr Hello (`url`) je adresa URL hello že Twilio požadavky tooget pokyny, které toodo hello volání po připojení. V tomto případě jsme nastavení a adresy URL (`http://yourdomain.cloudapp.net`), vrátí jednoduché TwiML dokument a používá hello `<Say>` toodo příkaz některé převod textu na řeč a vyslovte "Hello opic" toohello osoba, která přijímá hello volání.

## <a id="howto_recieve_sms"></a>Postupy: Receive zpráva SMS
V předchozím příkladu hello jsme spustili **odchozí** telefonního hovoru. Tento čas, použijeme hello telefonní číslo, které Twilio jste nám dali během registrace tooprocess **příchozí** zprávy SMS.

První, přihlášení tooyour [řídicí panel Twilio][twilio_account]. Klikněte na "Čísla" v horním nav hello a potom klikněte na hello Twilio číslo, které byly poskytnuté. Zobrazí se dvou adres URL, která se dají konfigurovat. Adresa URL požadavku na adrese URL žádosti hlasu a serveru služby SMS. Tyto jsou hello adresy URL, které se provádí volání Twilio vždy, když telefonní hovor nebo serveru služby SMS je odeslaných tooyour číslo. adresy URL Hello se také označuje jako "web háky".

Rádi bychom znali tooprocess příchozí zprávy SMS, takže umožňuje aktualizovat adresu URL hello příliš`http://yourdomain.cloudapp.net/sms_url`. Pokračujte a klikněte na tlačítko Uložit změny v hello dolní části stránky hello. Nyní, zpět na `web.rb` umožňuje programu naše aplikace toohandle toto:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for hello ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Po provedení změny hello, zkontrolujte, zda toore-start vaší webové aplikace. Teď vyjměte telefonu a odeslat služby SMS tooyour Twilio číslo. Měli byste rychle získat odpověď serveru SMS, která říká "blogu Hey, Děkujeme, že hello ping! Twilio a Azure rock! ".

## <a id="additional_services"></a>Postupy: použití další Twilio služeb
Kromě toho toohello příklady zobrazeny zde, že twilio nabízí webové rozhraní API, které můžete použít další funkce Twilio tooleverage z vaše aplikace Azure. Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API Twilio][twilio_api_documentation].

### <a id="NextSteps"></a>Další kroky
Teď, když jste se naučili základy hello hello Twilio služby, použijte tyto odkazy toolearn Další:

* [Pokyny pro zabezpečení Twilio][twilio_security_guidelines]
* [Twilio HowTos a ukázkový kód][twilio_howtos]
* [Kurzy Twilio rychlý start][twilio_quickstarts] 
* [Twilio na Githubu][twilio_on_github]
* [Komunikovat tooTwilio podpory][twilio_support]

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
