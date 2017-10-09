---
title: "aaaHow toouse hello sendgrid vám umožňuje e-mailovou službu (PHP) | Microsoft Docs"
description: "Zjistěte, jak odeslat e-mail s hello sendgrid vám umožňuje e-mailovou službu v Azure. Ukázky kódu jsou vytvořeny v jazyce PHP."
documentationcenter: php
services: 
manager: sendgrid
editor: mollybos
author: thinkingserious
ms.assetid: 51a9928a-4c9e-4b0a-aca8-9a408aeb6f47
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 10/30/2014
ms.author: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com
ms.openlocfilehash: 0076e56dc185cb8f52e629395e7d2c143cb5cfa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-sendgrid-email-service-from-php"></a>Jak tooUse hello sendgrid vám umožňuje e-mailovou službu z PHP
Tato příručka ukazuje, jak tooperform běžné úlohy programování s hello sendgrid vám umožňuje e-mailové služby v Azure. Ukázky Hello jsou zapsány v jazyce PHP.
Hello pokryté scénáře zahrnují **vytváření e-mailu**, **odesílání e-mailu**, a **přidávání příloh**. Další informace o sendgrid vám umožňuje a odesílání e-mailu, najdete v části hello [další kroky](#next-steps) části.

## <a name="what-is-hello-sendgrid-email-service"></a>Co je hello sendgrid vám umožňuje e-mailovou službu?
Je sendgrid vám umožňuje [cloudový e-mailovou službu] poskytuje spolehlivé [doručování e-mailem transakční], škálovatelnost a analýzu v reálném čase společně s flexibilní rozhraní API, které umožňují snadnou vlastní integrace. Obvyklé scénáře použití sendgrid vám umožňuje patří:

* Automatické odesílání toocustomers potvrzení
* Správa distribučních seznamů pro odesílání zákazníkům měsíční e letáků a speciálních nabídek
* Shromažďování metriky v reálném čase pro takové věci, jako e-mailu blokovaný a odezvy zákazníka
* Generování sestav toohelp identifikovat trendy
* Předávání dotazy zákazníků
* E-mailových oznámení z vaší aplikace

Další informace najdete v tématu [https://sendgrid.com][https://sendgrid.com].

## <a name="create-a-sendgrid-account"></a>Vytvoření účtu sendgrid vám umožňuje
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Pomocí sendgrid vám umožňuje z vaší aplikace PHP
Použití sendgrid vám umožňuje v aplikaci Azure PHP vyžaduje žádná speciální konfigurace nebo kódování. Protože služby sendgrid vám umožňuje se k němu v přesně hello stejným způsobem jako z cloudových aplikací, jak můžete z místní aplikace.

## <a name="how-to-send-an-email"></a>Postupy: odeslání e-mailu
Můžete odeslat e-mailu pomocí protokolu SMTP nebo hello webového rozhraní API poskytované sendgrid vám umožňuje.

### <a name="smtp-api"></a>ROZHRANÍ API SMTP
toosend e-mailu pomocí hello rozhraní API SMTP Sendgridu, použijte *poštovní Swift modul*, na základě součástí knihovny pro odesílání e-mailů z aplikací PHP. Si můžete stáhnout hello *poštovní Swift modul* knihovny z [http://swiftmailer.org/download] [ http://swiftmailer.org/download] v5.3.0 (použijte [autora] tooinstall SWIFT poštovní modul). Odesílání e-mailu pomocí hello knihovně zahrnuje vytvoření instancí <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_poštovní modul</span>, a <span class="auto-style2">Swift\_zpráv </span> třídy, nastavení příslušných vlastností a volání <span class="auto-style2">Swift\_Mailer::send</span> metoda.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create hello body of hello message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of hello email
      * If hello receiver is able tooview html emails then only hello html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name tooAppear');
     // Email recipients
     $too= array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';

     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach hello body of hello email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');

     // send message
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out too'.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>Web API
Použít pro PHP [curl funkce] [ curl function] toosend e-mailu pomocí webového rozhraní API sendgrid vám umožňuje hello.

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );

     $request = $url.'api/mail.send.json';

     // Generate curl request
     $session = curl_init($request);

     // Tell curl toouse HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);

     // Tell curl that this is hello body of hello POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

     // Tell curl not tooreturn headers, but do return hello response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

     // obtain response
     $response = curl_exec($session);
     curl_close($session);

     // print everything out
     print_r($response);

Sendgrid vám umožňuje na webového rozhraní API je velmi podobné tooa rozhraní REST API, i když není skutečně rozhraní RESTful API protože ve většině volání, i GET a POST příkazy zaměnitelné.

## <a name="how-to-add-an-attachment"></a>Postupy: Přidání přílohy
### <a name="smtp-api"></a>ROZHRANÍ API SMTP
Odesílání přílohy pomocí hello SMTP API zahrnuje jeden další řádek kódu toohello ukázkový skript pro e-mailu s poštovní Swift modul.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create hello body of hello message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of hello email
      * If hello reciever is able tooview html emails then only hello html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name tooAppear');

     // Email recipients
     $too= array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';

     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach hello body of hello email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));

     // send message
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out too'.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

Hello další řádek kódu vypadá takto:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Tento řádek kódu volání hello attach – metoda na <span class="auto-style2">Swift\_zpráva</span> objektu a používá statickou metodu <span class="auto-style2">fromPath</span> na <span class="auto-style2">Swift\_přílohy</span>třídy tooget a připojte soubor tooa zprávu.

### <a name="web-api"></a>Web API
Odeslání přílohy pomocí hello webového rozhraní API je velmi podobné toosending, k e-mailu pomocí webového rozhraní API hello. Všimněte si však, že v následujícím příkladu hello hello parametr pole musí obsahovat tento element:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Příklad:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';

     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> hello HTML </p>',
         'text' => 'hello plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );

     print_r($params);

     $request = $url.'api/mail.send.json';

     // Generate curl request
     $session = curl_init($request);

     // Tell curl toouse HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);

     // Tell curl that this is hello body of hello POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

     // Tell curl not tooreturn headers, but do return hello response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

     // obtain response
     $response = curl_exec($session);
     curl_close($session);

     // print everything out
     print_r($response);

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a>Postupy: použití filtrů tooEnable zápatí, sledování a analýzy
Sendgrid vám umožňuje poskytuje další e-mailové funkce prostřednictvím použití hello 'filtrů'. Jsou to nastavení, které mohou být přidány tooan e-mailové zprávě povolit specifické funkce, například povolení sledování klikněte na tlačítko, Google analytics, sledování, předplatné a tak dále.

Filtry lze použité tooa zpráv pomocí vlastnosti filtry hello. Každý filtr je zadána hodnota hash obsahující nastavení pro konkrétní filtru. Následující příklad povolí filtr hello zápatí a určuje, že textová zpráva, která bude připojeno toohello dolní hello e-mailové zprávy.
V tomto příkladu použijeme [sendgrid vám umožňuje php knihovny].
Použití [autora] tooinstall knihovny:

    php composer.phar require sendgrid/sendgrid 2.1.1

Příklad:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // hello list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request tooSendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify hello names of hello recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of hello above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have
     // enabled hello footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // hello subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy hello user here. If
     // a sender list IS specified above, this email address becomes irrelevant.
     $too= 'john@contoso.com';

     # Create hello body of hello message (a plain-text and an HTML version).
     # text is your plain-text email
     # html is your html version of hello email
     # if hello receiver is able tooview html emails then only hello html
     # email will be displayed

     /*
      * Note hello variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment toocall you at -time- EST toodiscuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html>
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     toocall you at -time- EST toodiscuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach hello body of hello email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello hello služby sendgrid vám umožňuje e-mailů, použijte tyto odkazy toolearn Další.

* Dokumentace sendgrid vám umožňuje: <https://sendgrid.com/docs>
* Knihovna sendgrid vám umožňuje PHP: <https://github.com/sendgrid/sendgrid-php>
* Speciální nabídka sendgrid vám umožňuje Azure zákazníků: <https://sendgrid.com/windowsazure.html>

Další informace najdete v tématu taky hello [středisku pro vývojáře PHP](/develop/php/).

[https://sendgrid.com]: https://sendgrid.com
[https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
[special offer]: https://www.sendgrid.com/windowsazure.html
[Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
[http://swiftmailer.org/download]: http://swiftmailer.org/download
[curl function]: http://php.net/curl
[cloudový e-mailovou službu]: https://sendgrid.com/email-solutions
[doručování e-mailem transakční]: https://sendgrid.com/transactional-email
[sendgrid vám umožňuje php knihovny]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
[autora]: https://getcomposer.org/download/
