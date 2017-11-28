---
title: "aaaHow toomake telefonní hovor z Twilio (PHP) | Microsoft Docs"
description: "Zjistěte, jak toomake telefonní hovor a odesílání SMS zprávy službou hello Twilio rozhraní API v Azure. Ukázky jsou pro aplikace PHP."
documentationcenter: php
services: 
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 44e35adc-be06-4700-beee-8c9e2c20c540
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: e6fecc345bf9ae787d14d533bd8d96b175c2453b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomake-a-phone-call-using-twilio-in-a-php-application-on-azure"></a><span data-ttu-id="fdcd1-104">Jak tooMake Twilio pomocí telefonního hovoru v aplikaci PHP v Azure</span><span class="sxs-lookup"><span data-stu-id="fdcd1-104">How tooMake a Phone Call Using Twilio in a PHP Application on Azure</span></span>
<span data-ttu-id="fdcd1-105">Hello následující příklad ukazuje, je použití Twilio toomake volání z webové stránky PHP hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-105">hello following example shows you how you can use Twilio toomake a call from a PHP web page hosted in Azure.</span></span> <span data-ttu-id="fdcd1-106">Výsledná aplikace Hello vyzve uživatele hello hodnoty telefonní hovor, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-106">hello resulting application will prompt hello user for phone call values, as shown in hello following screen shot.</span></span>

![Azure volání formulář s využitím Twilio a PHP][twilio_php]

<span data-ttu-id="fdcd1-108">Budete potřebovat následující hello toodo toouse hello kód v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="fdcd1-108">You'll need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="fdcd1-109">Získání účtu Twilio a ověřování tokenu z vaší [Twilio konzoly][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="fdcd1-109">Acquire a Twilio account and authentication token from your [Twilio Console][twilio_console].</span></span> <span data-ttu-id="fdcd1-110">začít s Twilio, tooget vyhodnotit ceny v [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="fdcd1-110">tooget started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="fdcd1-111">Můžete si zaregistrovat zkušební účet v [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="fdcd1-111">You can sign up for a trial account at [https://www.twilio.com/try-twilio][try_twilio].</span></span>
2. <span data-ttu-id="fdcd1-112">Získat hello [Twilio knihovny pro jazyk PHP](https://github.com/twilio/twilio-php) nebo ji nainstalovat jako balíček HRUŠKAMI.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-112">Obtain hello [Twilio library for PHP](https://github.com/twilio/twilio-php) or install it as a PEAR package.</span></span> <span data-ttu-id="fdcd1-113">Další informace najdete v tématu hello [souboru readme](https://github.com/twilio/twilio-php/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="fdcd1-113">For more information, see hello [readme file](https://github.com/twilio/twilio-php/blob/master/README.md).</span></span>
3. <span data-ttu-id="fdcd1-114">Nainstalujte hello Azure SDK pro jazyk PHP.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-114">Install hello Azure SDK for PHP.</span></span> <span data-ttu-id="fdcd1-115">Přehled hello SDK a pokyny k instalaci ji najdete v tématu [nastavit hello Azure SDK pro jazyk PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span><span class="sxs-lookup"><span data-stu-id="fdcd1-115">For an overview of hello SDK and instructions on installing it, see [Set up hello Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="fdcd1-116">Vytvoření webového formuláře pro volání</span><span class="sxs-lookup"><span data-stu-id="fdcd1-116">Create a web form for making a call</span></span>
<span data-ttu-id="fdcd1-117">Hello HTML následující kód ukazuje, jak toobuild na webové stránce (**callform.html**), načte data uživatele pro volání:</span><span class="sxs-lookup"><span data-stu-id="fdcd1-117">hello following HTML code shows how toobuild a web page (**callform.html**) that retrieves user data for making a call:</span></span>

```html
<!DOCTYPE html>
<html>
<head>
  <title>Automated call form</title>
</head>
<body>
  <h1>Automated Call Form</h1>
  <p>Fill in all fields and click <b>Make this call</b>.</p>
  <form action="makecall.php" method="post">
    <table>
      <tr>
        <td>To:</td>
        <td><input name="callTo" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>From:</td>
        <td><input name="callFrom" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>Call message:</td>
        <td><input name="callText" size="100" type="text" value="Hello. This is hello call text. Good bye."></td>
      </tr>
      <tr>
        <td colspan="2"><input type="submit" value="Make this call"></td>
      </tr>
    </table>
  </form><br>
</body>
</html>
```

## <a name="create-hello-code-toomake-hello-call"></a><span data-ttu-id="fdcd1-118">Vytvoření hello kód toomake hello volání</span><span class="sxs-lookup"><span data-stu-id="fdcd1-118">Create hello code toomake hello call</span></span>
<span data-ttu-id="fdcd1-119">Následující kód ukazuje, jak Hello toobuild **makecall.php**, která je volána, když hello uživatel odešle formulář hello zobrazuje **callform.html**.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-119">hello following code shows how toobuild **makecall.php**, which is called when hello user submits hello form displayed by **callform.html**.</span></span> <span data-ttu-id="fdcd1-120">Kód Hello vidíte níže vytvoří zprávu volání hello a generuje hello volání.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-120">hello code shown below creates hello call message and generates hello call.</span></span> <span data-ttu-id="fdcd1-121">Být také, zda toouse účtu Twilio a ověřování tokenu z hello [Twilio konzoly] [ twilio_console] místo hello zástupné hodnoty přiřazené příliš**$sid** a **$token** v hello kód níže.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-121">Also, be sure toouse your Twilio account and authentication token from hello [Twilio Console][twilio_console] instead of hello placeholder values assigned too**$sid** and **$token** in hello code below.</span></span>

```html
<html>
<head><title>Making call...</title></head>
<body>
<p>Your call is being made.</p>

<?php
require_once 'path/to/vendor/autoload.php';

$sid   = "your_account_sid";
$token = "your_authentication_token";

$from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
$to_number   = $_POST['callTo'];
$message     = $_POST['callText'];

$client = new Twilio\Rest\Client($sid, $token);

$call = $client->calls->create(
            $to_number,
            $from_number,
            array('url' => http://twimlets.com/message?Message=' . urlencode($message))
        );

echo "Call status: " . $call->status . "<br />";
echo "URI resource: " . $call->uri . "<br />";
?>
</body>
</html>
```

<span data-ttu-id="fdcd1-122">Kromě toho toomaking hello volání **makecall.php** zobrazí některá metadata volání znázorněné v následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-122">In addition toomaking hello call, **makecall.php** displays some call metadata, as is shown in hello image below.</span></span> <span data-ttu-id="fdcd1-123">Další informace o metadatech volání najdete v tématu [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span><span class="sxs-lookup"><span data-stu-id="fdcd1-123">For more information about call metadata, see [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span></span>

![Azure volání odpovědi pomocí Twilio a PHP][twilio_php_response]

## <a name="run-hello-application"></a><span data-ttu-id="fdcd1-125">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="fdcd1-125">Run hello application</span></span>
<span data-ttu-id="fdcd1-126">dalším krokem Hello je toodeploy vaší aplikace tooAzure weby.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-126">hello next step is toodeploy your application tooAzure Websites.</span></span> <span data-ttu-id="fdcd1-127">Hello následující články obsahovat hello informace pro vytvoření webu a nasazení kódu s Git, FTP nebo WebMatrix (i když nejsou všechny informace v jednotlivých článků je relevantní):</span><span class="sxs-lookup"><span data-stu-id="fdcd1-127">hello following articles contain hello information for creating a website and deploying your code with Git, FTP, or WebMatrix (though not all information in each article is relevant):</span></span>

* [<span data-ttu-id="fdcd1-128">Vytvoření webu PHP MySQL Azure a nasadit pomocí Git</span><span class="sxs-lookup"><span data-stu-id="fdcd1-128">Create a PHP-MySQL Azure Web Site and deploy using Git</span></span>](app-service-web/web-sites-php-mysql-deploy-use-git.md)
* [<span data-ttu-id="fdcd1-129">Vytvoření webu PHP MySQL Azure a nasadit pomocí protokolu FTP</span><span class="sxs-lookup"><span data-stu-id="fdcd1-129">Create a PHP-MySQL Azure Web Site and Deploy Using FTP</span></span>](app-service-web/web-sites-php-mysql-deploy-use-ftp.md)

## <a name="next-steps"></a><span data-ttu-id="fdcd1-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fdcd1-130">Next steps</span></span>
<span data-ttu-id="fdcd1-131">Tento kód byl poskytnut tooshow je základní funkce pomocí Twilio v jazyce PHP v Azure.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-131">This code was provided tooshow you basic functionality using Twilio in PHP on Azure.</span></span> <span data-ttu-id="fdcd1-132">Před nasazením tooAzure v produkčním prostředí, můžete chtít tooadd další zpracování chyb a další funkce.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-132">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="fdcd1-133">Například:</span><span class="sxs-lookup"><span data-stu-id="fdcd1-133">For example:</span></span>

* <span data-ttu-id="fdcd1-134">Místo použití webového formuláře, můžete použít objekty BLOB úložiště Azure nebo databázi SQL toostore telefonních čísel a volání text.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-134">Instead of using a web form, you could use Azure storage blobs or SQL Database toostore phone numbers and call text.</span></span> <span data-ttu-id="fdcd1-135">Informace o použití objektů BLOB služby Azure storage v jazyce PHP najdete v tématu [pomocí Azure Storage s aplikací PHP][howto_blob_storage_php].</span><span class="sxs-lookup"><span data-stu-id="fdcd1-135">For information about using Azure storage blobs in PHP, see [Using Azure Storage with PHP Applications][howto_blob_storage_php].</span></span> <span data-ttu-id="fdcd1-136">Informace o používání databáze SQL v jazyce PHP najdete v tématu [pomocí SQL Database pomocí aplikace PHP][howto_sql_azure_php].</span><span class="sxs-lookup"><span data-stu-id="fdcd1-136">For information about using SQL Database in PHP, see [Using SQL Database with PHP Applications][howto_sql_azure_php].</span></span>
* <span data-ttu-id="fdcd1-137">Hello **makecall.php** kód používá URL poskytnutou Twilio ([http://twimlets.com/message][twimlet_message_url]) tooprovide odpovědi Twilio Markup Language (TwiML), která informuje o tom, jak Twilio tooproceed s hello volání.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-137">hello **makecall.php** code uses Twilio-provided URL ([http://twimlets.com/message][twimlet_message_url]) tooprovide a Twilio Markup Language (TwiML) response that informs Twilio how tooproceed with hello call.</span></span> <span data-ttu-id="fdcd1-138">Například může obsahovat hello TwiML, která je vrácena `<Say>` akci, která má za následek textová hodnota mluvené toohello volání příjemce.</span><span class="sxs-lookup"><span data-stu-id="fdcd1-138">For example, hello TwiML that is returned can contain a `<Say>` verb that results in text being spoken toohello call recipient.</span></span> <span data-ttu-id="fdcd1-139">Místo použití hello URL poskytnutou Twilio, je sestavení vlastní tooTwilio toorespond služby požadavek; Další informace najdete v tématu [jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce PHP][howto_twilio_voice_sms_php].</span><span class="sxs-lookup"><span data-stu-id="fdcd1-139">Instead of using hello Twilio-provided URL, you could build your own service toorespond tooTwilio's request; for more information, see [How tooUse Twilio for Voice and SMS Capabilities in PHP][howto_twilio_voice_sms_php].</span></span> <span data-ttu-id="fdcd1-140">Další informace o TwiML lze najít na [http://www.twilio.com/docs/api/twiml][twiml]a další informace o `<Say>` a ostatní operace Twilio naleznete na adrese [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="fdcd1-140">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about `<Say>` and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="fdcd1-141">Přečtěte si pokyny pro zabezpečení Twilio hello v [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="fdcd1-141">Read hello Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="fdcd1-142">Další informace o Twilio najdete v tématu [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="fdcd1-142">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="fdcd1-143">Viz také</span><span class="sxs-lookup"><span data-stu-id="fdcd1-143">See Also</span></span>
* [<span data-ttu-id="fdcd1-144">Jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce PHP</span><span class="sxs-lookup"><span data-stu-id="fdcd1-144">How tooUse Twilio for Voice and SMS Capabilities in PHP</span></span>](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/docs/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
