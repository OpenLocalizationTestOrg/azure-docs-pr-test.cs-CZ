---
title: "Postup telefonní hovor z Twilio (PHP) | Microsoft Docs"
description: "Naučte se telefonní hovor a odeslání zprávy SMS službou Twilio rozhraní API v Azure. Ukázky jsou pro aplikace PHP."
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
ms.openlocfilehash: f35450ace02727ddf392dbbe857b934a45ee022a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a><span data-ttu-id="b1743-104">Postup telefonní hovor pomocí Twilio v aplikaci PHP v Azure</span><span class="sxs-lookup"><span data-stu-id="b1743-104">How to Make a Phone Call Using Twilio in a PHP Application on Azure</span></span>
<span data-ttu-id="b1743-105">Následující příklad ukazuje, jak Twilio můžete využít volání z webové stránky PHP hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="b1743-105">The following example shows you how you can use Twilio to make a call from a PHP web page hosted in Azure.</span></span> <span data-ttu-id="b1743-106">Výsledné aplikace vyzve uživatele, hodnoty telefonní hovor, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="b1743-106">The resulting application will prompt the user for phone call values, as shown in the following screen shot.</span></span>

![Azure volání formulář s využitím Twilio a PHP][twilio_php]

<span data-ttu-id="b1743-108">Budete muset následujícím postupem použít kód v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="b1743-108">You'll need to do the following to use the code in this topic:</span></span>

1. <span data-ttu-id="b1743-109">Získání účtu Twilio a ověřování tokenu z vaší [Twilio konzoly][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="b1743-109">Acquire a Twilio account and authentication token from your [Twilio Console][twilio_console].</span></span> <span data-ttu-id="b1743-110">Začít s Twilio vyhodnotit ceny v [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="b1743-110">To get started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="b1743-111">Můžete si zaregistrovat zkušební účet v [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="b1743-111">You can sign up for a trial account at [https://www.twilio.com/try-twilio][try_twilio].</span></span>
2. <span data-ttu-id="b1743-112">Získat [Twilio knihovny pro jazyk PHP](https://github.com/twilio/twilio-php) nebo ji nainstalovat jako balíček HRUŠKAMI.</span><span class="sxs-lookup"><span data-stu-id="b1743-112">Obtain the [Twilio library for PHP](https://github.com/twilio/twilio-php) or install it as a PEAR package.</span></span> <span data-ttu-id="b1743-113">Další informace najdete v tématu [souboru readme](https://github.com/twilio/twilio-php/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="b1743-113">For more information, see the [readme file](https://github.com/twilio/twilio-php/blob/master/README.md).</span></span>
3. <span data-ttu-id="b1743-114">Nainstalujte si Azure SDK pro jazyk PHP.</span><span class="sxs-lookup"><span data-stu-id="b1743-114">Install the Azure SDK for PHP.</span></span> <span data-ttu-id="b1743-115">Přehled sady SDK a pokyny k instalaci ji najdete v tématu [nastavit sadu Azure SDK pro jazyk PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span><span class="sxs-lookup"><span data-stu-id="b1743-115">For an overview of the SDK and instructions on installing it, see [Set up the Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="b1743-116">Vytvoření webového formuláře pro volání</span><span class="sxs-lookup"><span data-stu-id="b1743-116">Create a web form for making a call</span></span>
<span data-ttu-id="b1743-117">Následující kód HTML ukazuje, jak vytvořit webovou stránku (**callform.html**), načte data uživatele pro volání:</span><span class="sxs-lookup"><span data-stu-id="b1743-117">The following HTML code shows how to build a web page (**callform.html**) that retrieves user data for making a call:</span></span>

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
        <td><input name="callText" size="100" type="text" value="Hello. This is the call text. Good bye."></td>
      </tr>
      <tr>
        <td colspan="2"><input type="submit" value="Make this call"></td>
      </tr>
    </table>
  </form><br>
</body>
</html>
```

## <a name="create-the-code-to-make-the-call"></a><span data-ttu-id="b1743-118">Vytvoření kódu pro volání</span><span class="sxs-lookup"><span data-stu-id="b1743-118">Create the code to make the call</span></span>
<span data-ttu-id="b1743-119">Následující kód ukazuje, jak sestavit **makecall.php**, která je volána, když uživatel odešle formulář zobrazuje **callform.html**.</span><span class="sxs-lookup"><span data-stu-id="b1743-119">The following code shows how to build **makecall.php**, which is called when the user submits the form displayed by **callform.html**.</span></span> <span data-ttu-id="b1743-120">Následující kód vytvoří zprávu volání a vygeneruje volání.</span><span class="sxs-lookup"><span data-stu-id="b1743-120">The code shown below creates the call message and generates the call.</span></span> <span data-ttu-id="b1743-121">Navíc je nutné používat svůj účet Twilio a ověřování tokenu z [Twilio konzoly] [ twilio_console] místo zástupné hodnoty přiřazené **$sid** a **$token** v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="b1743-121">Also, be sure to use your Twilio account and authentication token from the [Twilio Console][twilio_console] instead of the placeholder values assigned to **$sid** and **$token** in the code below.</span></span>

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

<span data-ttu-id="b1743-122">Kromě vytváření volání **makecall.php** zobrazí některá metadata volání tak, jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="b1743-122">In addition to making the call, **makecall.php** displays some call metadata, as is shown in the image below.</span></span> <span data-ttu-id="b1743-123">Další informace o metadatech volání najdete v tématu [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span><span class="sxs-lookup"><span data-stu-id="b1743-123">For more information about call metadata, see [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span></span>

![Azure volání odpovědi pomocí Twilio a PHP][twilio_php_response]

## <a name="run-the-application"></a><span data-ttu-id="b1743-125">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b1743-125">Run the application</span></span>
<span data-ttu-id="b1743-126">Dalším krokem je pro nasazení aplikace na weby Azure.</span><span class="sxs-lookup"><span data-stu-id="b1743-126">The next step is to deploy your application to Azure Websites.</span></span> <span data-ttu-id="b1743-127">V následujících článcích obsahovat informace o vytvoření webu a nasazení kódu s Git, FTP nebo WebMatrix (i když nejsou všechny informace v jednotlivých článků je relevantní):</span><span class="sxs-lookup"><span data-stu-id="b1743-127">The following articles contain the information for creating a website and deploying your code with Git, FTP, or WebMatrix (though not all information in each article is relevant):</span></span>

* [<span data-ttu-id="b1743-128">Vytvoření webu PHP MySQL Azure a nasadit pomocí Git</span><span class="sxs-lookup"><span data-stu-id="b1743-128">Create a PHP-MySQL Azure Web Site and deploy using Git</span></span>](app-service-web/web-sites-php-mysql-deploy-use-git.md)
* [<span data-ttu-id="b1743-129">Vytvoření webu PHP MySQL Azure a nasadit pomocí protokolu FTP</span><span class="sxs-lookup"><span data-stu-id="b1743-129">Create a PHP-MySQL Azure Web Site and Deploy Using FTP</span></span>](app-service-web/web-sites-php-mysql-deploy-use-ftp.md)

## <a name="next-steps"></a><span data-ttu-id="b1743-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b1743-130">Next steps</span></span>
<span data-ttu-id="b1743-131">Tento kód byl poskytnut tak, aby zobrazovalo základních funkcí pomocí Twilio v jazyce PHP v Azure.</span><span class="sxs-lookup"><span data-stu-id="b1743-131">This code was provided to show you basic functionality using Twilio in PHP on Azure.</span></span> <span data-ttu-id="b1743-132">Před nasazením do Azure v produkčním prostředí, můžete přidat další zpracování chyb a další funkce.</span><span class="sxs-lookup"><span data-stu-id="b1743-132">Before deploying to Azure in production, you may want to add more error handling or other features.</span></span> <span data-ttu-id="b1743-133">Například:</span><span class="sxs-lookup"><span data-stu-id="b1743-133">For example:</span></span>

* <span data-ttu-id="b1743-134">Místo použití webového formuláře, můžete použít objekty BLOB úložiště Azure nebo databázi SQL pro ukládání telefonních čísel a volání text.</span><span class="sxs-lookup"><span data-stu-id="b1743-134">Instead of using a web form, you could use Azure storage blobs or SQL Database to store phone numbers and call text.</span></span> <span data-ttu-id="b1743-135">Informace o použití objektů BLOB služby Azure storage v jazyce PHP najdete v tématu [pomocí Azure Storage s aplikací PHP][howto_blob_storage_php].</span><span class="sxs-lookup"><span data-stu-id="b1743-135">For information about using Azure storage blobs in PHP, see [Using Azure Storage with PHP Applications][howto_blob_storage_php].</span></span> <span data-ttu-id="b1743-136">Informace o používání databáze SQL v jazyce PHP najdete v tématu [pomocí SQL Database pomocí aplikace PHP][howto_sql_azure_php].</span><span class="sxs-lookup"><span data-stu-id="b1743-136">For information about using SQL Database in PHP, see [Using SQL Database with PHP Applications][howto_sql_azure_php].</span></span>
* <span data-ttu-id="b1743-137">**Makecall.php** kód používá URL poskytnutou Twilio ([http://twimlets.com/message][twimlet_message_url]) můžete zadat odpověď Twilio Markup Language (TwiML), které informují Twilio postup při volání.</span><span class="sxs-lookup"><span data-stu-id="b1743-137">The **makecall.php** code uses Twilio-provided URL ([http://twimlets.com/message][twimlet_message_url]) to provide a Twilio Markup Language (TwiML) response that informs Twilio how to proceed with the call.</span></span> <span data-ttu-id="b1743-138">Například může obsahovat TwiML, která je vrácena `<Say>` akci, která je výsledkem text použitém k příjemce volání.</span><span class="sxs-lookup"><span data-stu-id="b1743-138">For example, the TwiML that is returned can contain a `<Say>` verb that results in text being spoken to the call recipient.</span></span> <span data-ttu-id="b1743-139">Místo použití URL poskytnutou Twilio, může vytvořit vlastní služba neodpoví na požadavek na Twilio; Další informace najdete v tématu [jak Twilio použijte pro hlasový a možnosti serveru SMS v jazyce PHP][howto_twilio_voice_sms_php].</span><span class="sxs-lookup"><span data-stu-id="b1743-139">Instead of using the Twilio-provided URL, you could build your own service to respond to Twilio's request; for more information, see [How to Use Twilio for Voice and SMS Capabilities in PHP][howto_twilio_voice_sms_php].</span></span> <span data-ttu-id="b1743-140">Další informace o TwiML lze najít na [http://www.twilio.com/docs/api/twiml][twiml]a další informace o `<Say>` a ostatní operace Twilio naleznete na adrese [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="b1743-140">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about `<Say>` and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="b1743-141">Přečtěte si pokyny zabezpečení Twilio v [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="b1743-141">Read the Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="b1743-142">Další informace o Twilio najdete v tématu [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="b1743-142">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="b1743-143">Viz také</span><span class="sxs-lookup"><span data-stu-id="b1743-143">See Also</span></span>
* [<span data-ttu-id="b1743-144">Jak používat Twilio pro hlasový a možnosti serveru SMS v jazyce PHP</span><span class="sxs-lookup"><span data-stu-id="b1743-144">How to Use Twilio for Voice and SMS Capabilities in PHP</span></span>](partner-twilio-php-how-to-use-voice-sms.md)

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
