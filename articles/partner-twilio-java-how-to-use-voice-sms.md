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
# <a name="how-toouse-twilio-for-voice-and-sms-capabilities-in-java"></a>Jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce Java
Tato příručka ukazuje, jak služba tooperform běžné úlohy programování s hello Twilio rozhraní API v Azure. pokryté scénáře Hello zahrnují uskutečnění telefonního hovoru a odesílání zprávy služby krátké zprávy (SMS). Další informace o Twilio a použití hlasu a SMS ve svých aplikacích najdete v tématu hello [další kroky](#NextSteps) části.

## <a id="WhatIs"></a>Co je Twilio?
Twilio je telefonickou rozhraní API webové služby, které umožňuje použít existující webové jazyky a dovednosti toobuild hlas a aplikace serveru SMS. Twilio je služba třetích stran (ne funkcí platformy Azure a produkt společnosti Microsoft).

**Hlasové Twilio** umožňuje toomake vaší aplikace a příjem telefonních hovorů. **Twilio SMS** umožňuje toomake vaší aplikace a přijímat zprávy SMS. **Klient Twilio** umožňuje aplikace tooenable hlasové komunikaci pomocí existujícího připojení k Internetu, včetně mobilních připojení.

## <a id="Pricing"></a>Ceny Twilio a speciálních nabídek
Informace o cenách Twilio je k dispozici na [Twilio ceny][twilio_pricing]. Přijímat Azure zákazníků [speciální nabídka][special_offer]: volné platební 1000 textů nebo 1000 příchozí minut. toosign pro toto nabízejí nebo získat další informace, navštivte [http://ahoy.twilio.com/azure][special_offer].

## <a id="Concepts"></a>Koncepty
Hello Twilio rozhraní API je rozhraní RESTful API, která poskytuje hlas a funkce serveru SMS pro aplikace. Klientské knihovny jsou k dispozici v několika jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny][twilio_libraries].

Klíčové aspekty hello Twilio API jsou příkazy Twilio a Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Příkazy Twilio
Hello API využívá Twilio příkazy; například hello ** &lt;indikované&gt; ** příkaz dá pokyn Twilio tooaudibly doručit zprávu na volání.

Hello následuje seznam operací Twilio.

* **&lt;Vytočit&gt;**: připojí hello volající tooanother phone.
* **&lt;Shromážděte&gt;**: shromažďuje číslice zadané na klávesnici telefonu hello.
* **&lt;Zavěšení&gt;**: ukončí volání.
* **&lt;Přehrání&gt;**: přehraje zvukový soubor.
* **&lt;Fronty&gt;**: přidejte hello tooa frontu volající.
* **&lt;Pozastavení&gt;**: bezobslužná čeká na zadaném počtu sekund.
* **&lt;Záznam&gt;**: zaznamenává hlasové hello volajícího a vrátí adresu URL souboru, který obsahuje záznam hello.
* **&lt;Přesměrování&gt;**: přenáší kontrolu nad telefonického hovoru nebo SMS toohello TwiML na jinou adresu URL.
* **&lt;Odmítnout&gt;**: odmítne na příchozí volání bez fakturace můžete tooyour Twilio číslo.
* **&lt;Řekněme&gt;**: toospeech převede text, že z volání.
* **&lt;SMS&gt;**: odešle zprávu SMS.

### <a id="TwiML"></a>TwiML
TwiML je sada založený na jazyce XML pokyny podle hello Twilio příkazy, které informovat o tom, jak Twilio tooprocess hovoru nebo SMS.

Jako příklad by hello následující TwiML převést hello text **Hello, World!** toospeech.

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

Pokud vaše aplikace volání hello Twilio rozhraní API, jeden z parametrů hello rozhraní API je hello adresu URL, který vrátí odpověď TwiML hello. Pro účely vývoje můžete použít adresy URL zadané Twilio tooprovide hello TwiML odpovědí použít v aplikacích. Může taky hostovat vlastní adresy URL tooproduce hello TwiML odpovědí, a další možností je toouse hello **TwiMLResponse** objektu.

Další informace o Twilio příkazy, jejich atributů a TwiML najdete v tématu [TwiML][twiml]. Další informace o hello Twilio API najdete v tématu [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Vytvoření účtu Twilio
Když jste připravené tooget Twilio účet, zaregistrovat na [zkuste Twilio][try_twilio]. Můžete začít s bezplatný účet a upgradujte si účet později.

Když se zaregistrujete k účtu Twilio, obdržíte ID účtu a ověřovací token. Jak bude volání rozhraní API Twilio potřebné toomake. tooprevent neoprávněný přístup k účtu tooyour, ověřovací token zabezpečení. ID účtu a ověřování tokenu je možné zobrazit v hello [Twilio konzoly][twilio_console]v hello pole s názvem bez přípony **SID účtu** a **ověřování TOKENU**, v uvedeném pořadí.

## <a id="create_app"></a>Vytvořit aplikaci Java
1. Získat hello Twilio JAR a přidejte ho tooyour vaše nasazení sestavení WAR a cesta sestavení Java. V [https://github.com/twilio/twilio-java][twilio_java], můžete stáhnout hello Githubu zdroje a vytvořit vlastní JAR nebo stáhnout předdefinovaných JAR (s nebo bez závislosti).
2. Zkontrolujte vaše JDK **cacerts** úložiště klíčů obsahuje hello certifikát Equifax zabezpečení certifikační autority s MD5 otisk prstu 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello sériové číslo je 35:DE:F4:CF a hello SHA1 otisk prstu se D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Toto je hello certifikát certifikační autority certifikátu pro hello [https://api.twilio.com] [ twilio_api_service] služba, která je volána, když používáte rozhraní API Twilio. Informace o zajištění vaší JDK **cacerts** úložiště klíčů obsahuje hello správný certifikát certifikační Autority najdete v tématu [přidání certifikátu toohello úložiště certifikátů certifikační Autority Java][add_ca_cert].

Podrobné pokyny pro jazyk Java pomocí hello Twilio klientské knihovny jsou k dispozici na [jak tooMake Twilio pomocí telefonního hovoru v aplikaci Java v Azure][howto_phonecall_java].

## <a id="configure_app"></a>Konfigurace knihovny Twilio tooUse vaše aplikace
V kódu, můžete přidat **importovat** příkazy v horní části hello zdrojových souborů pro hello Twilio balíčky nebo třídy, které chcete toouse ve vaší aplikaci.

Pro zdrojové soubory Java:

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

Pro zdrojové soubory Java serveru stránky (JSP):

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
V závislosti na tom, které balíčky Twilio nebo třídy chcete toouse, vaše **importovat** příkazy se můžou lišit.

## <a id="howto_make_call"></a>Postupy: volání odchozí
Hello následující ukazuje, jak toomake odchozí volat pomocí hello **volání** třídy. Tento kód také používá zadaný Twilio lokality tooreturn hello odpovědi Twilio Markup Language (TwiML). Dosaďte svoje hodnoty hello **z** a **k** telefonních čísel a ujistěte se, abyste ověřili hello **z** telefonní číslo kódu hello předchozí toorunning účtu Twilio.

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

Další informace o parametrech hello předaná toohello **Call.creator** metodu, najdete v části [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Jak je uvedeno, tento kód používá zadaný Twilio lokality tooreturn hello TwiML odpovědi. Místo toho můžete použít vlastní lokality tooprovide hello TwiML odpovědi; Další informace najdete v tématu [jak tooProvide TwiML odpovědi v aplikaci Java v Azure](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Postupy: odeslání zprávy SMS
Hello následující ukazuje, jak toosend zprávu SMS pomocí hello **zpráva** třídy. Hello **z** číslo, **4155992671**, zajišťuje Twilio pro zkušební verzi účty toosend zpráv serveru SMS. Hello **k** číslo musí ověřit kódu hello předchozí toorunning účtu Twilio.

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

Další informace o parametrech hello předaná toohello **Message.creator** metodu, najdete v části [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Postupy: poskytování TwiML odpovědí z vlastního webu
Pokud vaše aplikace zahájí volání toohello Twilio rozhraní API, například prostřednictvím hello **CallCreator.create** metoda, odešle Twilio tooa adresu URL vašeho požadavku, která je očekávané tooreturn TwiML odpověď. výše uvedený příklad Hello používá URL poskytnutou Twilio hello [http://twimlets.com/message][twimlet_message_url]. (Při TwiML je určen k použití pomocí webové služby, můžete zobrazit hello TwiML v prohlížeči. Klikněte například na [http://twimlets.com/message] [ twimlet_message_url] toosee prázdnou ** &lt;odpovědi&gt; ** element; například klikněte na tlačítko [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21] [ twimlet_message_url_hello_world] toosee ** &lt;odpovědi&gt; ** elementu, který obsahuje ** &lt;indikované &gt; ** elementu.)

Aniž byste museli spoléhat na URL poskytnutou Twilio hello, můžete vytvořit vlastního webu adresu URL, která vrací odpovědi HTTP. Můžete vytvořit hello lokality v libovolném jazyce, který vrací odpovědi HTTP; Toto téma předpokládá, že budete hostování hello adresu URL, na stránce JSP.

Hello následující stránka JSP za následek TwiML odpovědi, která uvádí, že **Hello, World!** Při volání hello.

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

následující výsledky stránky JSP v TwiML odpovědi s upozorněním, část textu Hello má několik pozastaví a uvádí informace o verzi rozhraní API Twilio hello a název Azure role hello.

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

Hello **ApiVersion** parametr je k dispozici v žádostech o hlasové Twilio (ne požadavků SMS). parametry k dispozici požadavku hello toosee pro hlasové Twilio a žádosti o služby SMS, najdete v části <https://www.twilio.com/docs/api/twiml/twilio_request> a <https://www.twilio.com/docs/api/twiml/sms/twilio_request >, v uvedeném pořadí. Hello **RoleName** proměnná prostředí je k dispozici jako součást nasazení služby Azure. (Pokud budete chtít tooadd vlastní proměnné prostředí, takže se mohou být zachyceny z **System.getenv**, najdete v části proměnných prostředí hello na [ostatní nastavení konfigurace Role] [misc_role_config_settings].)

Jakmile máte stránku JSP nastavit tooprovide TwiML odpovědi, použijte adresu URL hello hello JSP stránky, jako adresa URL předaná do hello hello **Call.creator** metoda. Například pokud máte webovou aplikaci s názvem tooan MyTwiML nasadit Azure hostované služby a hello název stránky JSP hello je mytwiml.jsp, adresa URL hello lze předat příliš**Call.creator** jak je znázorněno v následující hello:

```java
    // Declare tooand From numbers and hello URL of your JSP page
    PhoneNumber too= new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, tooand URL values
    // then make hello call by executing hello create() method
    Call.creator(to, from, uri).create();
```

Další možnost reagovat s TwiML je prostřednictvím hello **VoiceResponse** třída, která je k dispozici v hello **com.twilio.twiml** balíčku.

Další informace o používání Twilio v Azure s Java najdete v tématu [jak tooMake Twilio pomocí telefonního hovoru v aplikaci Java v Azure][howto_phonecall_java].

## <a id="AdditionalServices"></a>Postupy: použití další Twilio služeb
Kromě toho toohello příklady zobrazeny zde, že twilio nabízí webové rozhraní API, které můžete použít další funkce Twilio tooleverage z vaše aplikace Azure. Úplné podrobnosti najdete v tématu hello [dokumentaci k rozhraní API Twilio][twilio_api_documentation].

## <a id="NextSteps"></a>Další kroky
Teď, když jste se naučili základy hello hello Twilio služby, použijte tyto odkazy toolearn Další:

* [Pokyny pro zabezpečení Twilio][twilio_security_guidelines]
* [Twilio postupy a příklady kódu][twilio_howtos]
* [Kurzy Twilio rychlý start][twilio_quickstarts]
* [Twilio na Githubu][twilio_on_github]
* [Komunikovat tooTwilio podpory][twilio_support]

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
