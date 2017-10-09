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
# <a name="how-toouse-hello-sendgrid-email-service-from-php"></a><span data-ttu-id="3cf7e-104">Jak tooUse hello sendgrid vám umožňuje e-mailovou službu z PHP</span><span class="sxs-lookup"><span data-stu-id="3cf7e-104">How tooUse hello SendGrid Email Service from PHP</span></span>
<span data-ttu-id="3cf7e-105">Tato příručka ukazuje, jak tooperform běžné úlohy programování s hello sendgrid vám umožňuje e-mailové služby v Azure.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-105">This guide demonstrates how tooperform common programming tasks with hello SendGrid email service on Azure.</span></span> <span data-ttu-id="3cf7e-106">Ukázky Hello jsou zapsány v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-106">hello samples are written in PHP.</span></span>
<span data-ttu-id="3cf7e-107">Hello pokryté scénáře zahrnují **vytváření e-mailu**, **odesílání e-mailu**, a **přidávání příloh**.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-107">hello scenarios covered include **constructing email**, **sending email**, and **adding attachments**.</span></span> <span data-ttu-id="3cf7e-108">Další informace o sendgrid vám umožňuje a odesílání e-mailu, najdete v části hello [další kroky](#next-steps) části.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-108">For more information on SendGrid and sending email, see hello [Next Steps](#next-steps) section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="3cf7e-109">Co je hello sendgrid vám umožňuje e-mailovou službu?</span><span class="sxs-lookup"><span data-stu-id="3cf7e-109">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="3cf7e-110">Je sendgrid vám umožňuje [cloudový e-mailovou službu] poskytuje spolehlivé [doručování e-mailem transakční], škálovatelnost a analýzu v reálném čase společně s flexibilní rozhraní API, které umožňují snadnou vlastní integrace.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="3cf7e-111">Obvyklé scénáře použití sendgrid vám umožňuje patří:</span><span class="sxs-lookup"><span data-stu-id="3cf7e-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="3cf7e-112">Automatické odesílání toocustomers potvrzení</span><span class="sxs-lookup"><span data-stu-id="3cf7e-112">Automatically sending receipts toocustomers</span></span>
* <span data-ttu-id="3cf7e-113">Správa distribučních seznamů pro odesílání zákazníkům měsíční e letáků a speciálních nabídek</span><span class="sxs-lookup"><span data-stu-id="3cf7e-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="3cf7e-114">Shromažďování metriky v reálném čase pro takové věci, jako e-mailu blokovaný a odezvy zákazníka</span><span class="sxs-lookup"><span data-stu-id="3cf7e-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="3cf7e-115">Generování sestav toohelp identifikovat trendy</span><span class="sxs-lookup"><span data-stu-id="3cf7e-115">Generating reports toohelp identify trends</span></span>
* <span data-ttu-id="3cf7e-116">Předávání dotazy zákazníků</span><span class="sxs-lookup"><span data-stu-id="3cf7e-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="3cf7e-117">E-mailových oznámení z vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="3cf7e-117">Email notifications from your application</span></span>

<span data-ttu-id="3cf7e-118">Další informace najdete v tématu [https://sendgrid.com][https://sendgrid.com].</span><span class="sxs-lookup"><span data-stu-id="3cf7e-118">For more information, see [https://sendgrid.com][https://sendgrid.com].</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="3cf7e-119">Vytvoření účtu sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="3cf7e-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a><span data-ttu-id="3cf7e-120">Pomocí sendgrid vám umožňuje z vaší aplikace PHP</span><span class="sxs-lookup"><span data-stu-id="3cf7e-120">Using SendGrid from your PHP Application</span></span>
<span data-ttu-id="3cf7e-121">Použití sendgrid vám umožňuje v aplikaci Azure PHP vyžaduje žádná speciální konfigurace nebo kódování.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-121">Using SendGrid in an Azure PHP application requires no special configuration or coding.</span></span> <span data-ttu-id="3cf7e-122">Protože služby sendgrid vám umožňuje se k němu v přesně hello stejným způsobem jako z cloudových aplikací, jak můžete z místní aplikace.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-122">Because SendGrid is a service, it can be accessed in exactly hello same way from a cloud application as it can from an on-premises application.</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="3cf7e-123">Postupy: odeslání e-mailu</span><span class="sxs-lookup"><span data-stu-id="3cf7e-123">How to: Send an Email</span></span>
<span data-ttu-id="3cf7e-124">Můžete odeslat e-mailu pomocí protokolu SMTP nebo hello webového rozhraní API poskytované sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-124">You can send email using either SMTP or hello Web API provided by SendGrid.</span></span>

### <a name="smtp-api"></a><span data-ttu-id="3cf7e-125">ROZHRANÍ API SMTP</span><span class="sxs-lookup"><span data-stu-id="3cf7e-125">SMTP API</span></span>
<span data-ttu-id="3cf7e-126">toosend e-mailu pomocí hello rozhraní API SMTP Sendgridu, použijte *poštovní Swift modul*, na základě součástí knihovny pro odesílání e-mailů z aplikací PHP.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-126">toosend email using hello SendGrid SMTP API, use *Swift Mailer*, a component-based library for sending emails from PHP applications.</span></span> <span data-ttu-id="3cf7e-127">Si můžete stáhnout hello *poštovní Swift modul* knihovny z [http://swiftmailer.org/download] [ http://swiftmailer.org/download] v5.3.0 (použijte [autora] tooinstall SWIFT poštovní modul).</span><span class="sxs-lookup"><span data-stu-id="3cf7e-127">You can download hello *Swift Mailer* library from [http://swiftmailer.org/download][http://swiftmailer.org/download] v5.3.0 (use [Composer] tooinstall Swift Mailer).</span></span> <span data-ttu-id="3cf7e-128">Odesílání e-mailu pomocí hello knihovně zahrnuje vytvoření instancí <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_poštovní modul</span>, a <span class="auto-style2">Swift\_zpráv </span> třídy, nastavení příslušných vlastností a volání <span class="auto-style2">Swift\_Mailer::send</span> metoda.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-128">Sending email with hello library involves creating instances of the <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_Mailer</span>, and <span class="auto-style2">Swift\_Message</span> classes, setting the appropriate properties, and calling the <span class="auto-style2">Swift\_Mailer::send</span> method.</span></span>

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

### <a name="web-api"></a><span data-ttu-id="3cf7e-129">Web API</span><span class="sxs-lookup"><span data-stu-id="3cf7e-129">Web API</span></span>
<span data-ttu-id="3cf7e-130">Použít pro PHP [curl funkce] [ curl function] toosend e-mailu pomocí webového rozhraní API sendgrid vám umožňuje hello.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-130">Use PHP's [curl function][curl function] toosend email using hello SendGrid Web API.</span></span>

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

<span data-ttu-id="3cf7e-131">Sendgrid vám umožňuje na webového rozhraní API je velmi podobné tooa rozhraní REST API, i když není skutečně rozhraní RESTful API protože ve většině volání, i GET a POST příkazy zaměnitelné.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-131">SendGrid's Web API is very similar tooa REST API, though it is not truly a RESTful API since, in most calls, both GET and POST verbs can be used interchangeably.</span></span>

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="3cf7e-132">Postupy: Přidání přílohy</span><span class="sxs-lookup"><span data-stu-id="3cf7e-132">How to: Add an Attachment</span></span>
### <a name="smtp-api"></a><span data-ttu-id="3cf7e-133">ROZHRANÍ API SMTP</span><span class="sxs-lookup"><span data-stu-id="3cf7e-133">SMTP API</span></span>
<span data-ttu-id="3cf7e-134">Odesílání přílohy pomocí hello SMTP API zahrnuje jeden další řádek kódu toohello ukázkový skript pro e-mailu s poštovní Swift modul.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-134">Sending an attachment using hello SMTP API involves one additional line of code toohello example script for sending an email with Swift Mailer.</span></span>

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

<span data-ttu-id="3cf7e-135">Hello další řádek kódu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="3cf7e-135">hello additional line of code is as follows:</span></span>

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

<span data-ttu-id="3cf7e-136">Tento řádek kódu volání hello attach – metoda na <span class="auto-style2">Swift\_zpráva</span> objektu a používá statickou metodu <span class="auto-style2">fromPath</span> na <span class="auto-style2">Swift\_přílohy</span>třídy tooget a připojte soubor tooa zprávu.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-136">This line of code calls hello attach method on the <span class="auto-style2">Swift\_Message</span> object and uses static method <span class="auto-style2">fromPath</span> on the <span class="auto-style2">Swift\_Attachment</span> class tooget and attach a file tooa message.</span></span>

### <a name="web-api"></a><span data-ttu-id="3cf7e-137">Web API</span><span class="sxs-lookup"><span data-stu-id="3cf7e-137">Web API</span></span>
<span data-ttu-id="3cf7e-138">Odeslání přílohy pomocí hello webového rozhraní API je velmi podobné toosending, k e-mailu pomocí webového rozhraní API hello.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-138">Sending an attachment using hello Web API is very similar toosending an email using hello Web API.</span></span> <span data-ttu-id="3cf7e-139">Všimněte si však, že v následujícím příkladu hello hello parametr pole musí obsahovat tento element:</span><span class="sxs-lookup"><span data-stu-id="3cf7e-139">However, note that in hello example that follows, hello parameter array must contain this element:</span></span>

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

<span data-ttu-id="3cf7e-140">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3cf7e-140">Example:</span></span>

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

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a><span data-ttu-id="3cf7e-141">Postupy: použití filtrů tooEnable zápatí, sledování a analýzy</span><span class="sxs-lookup"><span data-stu-id="3cf7e-141">How to: Use Filters tooEnable Footers, Tracking, and Analytics</span></span>
<span data-ttu-id="3cf7e-142">Sendgrid vám umožňuje poskytuje další e-mailové funkce prostřednictvím použití hello 'filtrů'.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-142">SendGrid provides additional email functionality through hello use of 'filters'.</span></span> <span data-ttu-id="3cf7e-143">Jsou to nastavení, které mohou být přidány tooan e-mailové zprávě povolit specifické funkce, například povolení sledování klikněte na tlačítko, Google analytics, sledování, předplatné a tak dále.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-143">These are settings that can be added tooan email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span>

<span data-ttu-id="3cf7e-144">Filtry lze použité tooa zpráv pomocí vlastnosti filtry hello.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-144">Filters can be applied tooa message by using hello filters property.</span></span> <span data-ttu-id="3cf7e-145">Každý filtr je zadána hodnota hash obsahující nastavení pro konkrétní filtru.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-145">Each filter is specified by a hash containing filter-specific settings.</span></span> <span data-ttu-id="3cf7e-146">Následující příklad povolí filtr hello zápatí a určuje, že textová zpráva, která bude připojeno toohello dolní hello e-mailové zprávy.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-146">The following example enables hello footer filter and specifies a text message that will be appended toohello bottom of hello email message.</span></span>
<span data-ttu-id="3cf7e-147">V tomto příkladu použijeme [sendgrid vám umožňuje php knihovny].</span><span class="sxs-lookup"><span data-stu-id="3cf7e-147">For this example we will use [sendgrid-php library].</span></span>
<span data-ttu-id="3cf7e-148">Použití [autora] tooinstall knihovny:</span><span class="sxs-lookup"><span data-stu-id="3cf7e-148">Use [Composer] tooinstall library:</span></span>

    php composer.phar require sendgrid/sendgrid 2.1.1

<span data-ttu-id="3cf7e-149">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3cf7e-149">Example:</span></span>    

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

## <a name="next-steps"></a><span data-ttu-id="3cf7e-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3cf7e-150">Next Steps</span></span>
<span data-ttu-id="3cf7e-151">Teď, když jste se naučili základy hello hello služby sendgrid vám umožňuje e-mailů, použijte tyto odkazy toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="3cf7e-151">Now that you've learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="3cf7e-152">Dokumentace sendgrid vám umožňuje: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="3cf7e-152">SendGrid documentation: <https://sendgrid.com/docs></span></span>
* <span data-ttu-id="3cf7e-153">Knihovna sendgrid vám umožňuje PHP: <https://github.com/sendgrid/sendgrid-php></span><span class="sxs-lookup"><span data-stu-id="3cf7e-153">SendGrid PHP library: <https://github.com/sendgrid/sendgrid-php></span></span>
* <span data-ttu-id="3cf7e-154">Speciální nabídka sendgrid vám umožňuje Azure zákazníků: <https://sendgrid.com/windowsazure.html></span><span class="sxs-lookup"><span data-stu-id="3cf7e-154">SendGrid special offer for Azure customers: <https://sendgrid.com/windowsazure.html></span></span>

<span data-ttu-id="3cf7e-155">Další informace najdete v tématu taky hello [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="3cf7e-155">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

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
