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
# <a name="how-toomake-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Jak tooMake Twilio pomocí telefonního hovoru v aplikaci PHP v Azure
Hello následující příklad ukazuje, je použití Twilio toomake volání z webové stránky PHP hostované v Azure. Výsledná aplikace Hello vyzve uživatele hello hodnoty telefonní hovor, jak ukazuje následující snímek obrazovky hello.

![Azure volání formulář s využitím Twilio a PHP][twilio_php]

Budete potřebovat následující hello toodo toouse hello kód v tomto tématu:

1. Získání účtu Twilio a ověřování tokenu z vaší [Twilio konzoly][twilio_console]. začít s Twilio, tooget vyhodnotit ceny v [http://www.twilio.com/pricing][twilio_pricing]. Můžete si zaregistrovat zkušební účet v [https://www.twilio.com/try-twilio][try_twilio].
2. Získat hello [Twilio knihovny pro jazyk PHP](https://github.com/twilio/twilio-php) nebo ji nainstalovat jako balíček HRUŠKAMI. Další informace najdete v tématu hello [souboru readme](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Nainstalujte hello Azure SDK pro jazyk PHP. Přehled hello SDK a pokyny k instalaci ji najdete v tématu [nastavit hello Azure SDK pro jazyk PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)

## <a name="create-a-web-form-for-making-a-call"></a>Vytvoření webového formuláře pro volání
Hello HTML následující kód ukazuje, jak toobuild na webové stránce (**callform.html**), načte data uživatele pro volání:

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

## <a name="create-hello-code-toomake-hello-call"></a>Vytvoření hello kód toomake hello volání
Následující kód ukazuje, jak Hello toobuild **makecall.php**, která je volána, když hello uživatel odešle formulář hello zobrazuje **callform.html**. Kód Hello vidíte níže vytvoří zprávu volání hello a generuje hello volání. Být také, zda toouse účtu Twilio a ověřování tokenu z hello [Twilio konzoly] [ twilio_console] místo hello zástupné hodnoty přiřazené příliš**$sid** a **$token** v hello kód níže.

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

Kromě toho toomaking hello volání **makecall.php** zobrazí některá metadata volání znázorněné v následující obrázek hello. Další informace o metadatech volání najdete v tématu [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Azure volání odpovědi pomocí Twilio a PHP][twilio_php_response]

## <a name="run-hello-application"></a>Spuštění aplikace hello
dalším krokem Hello je toodeploy vaší aplikace tooAzure weby. Hello následující články obsahovat hello informace pro vytvoření webu a nasazení kódu s Git, FTP nebo WebMatrix (i když nejsou všechny informace v jednotlivých článků je relevantní):

* [Vytvoření webu PHP MySQL Azure a nasadit pomocí Git](app-service-web/web-sites-php-mysql-deploy-use-git.md)
* [Vytvoření webu PHP MySQL Azure a nasadit pomocí protokolu FTP](app-service-web/web-sites-php-mysql-deploy-use-ftp.md)

## <a name="next-steps"></a>Další kroky
Tento kód byl poskytnut tooshow je základní funkce pomocí Twilio v jazyce PHP v Azure. Před nasazením tooAzure v produkčním prostředí, můžete chtít tooadd další zpracování chyb a další funkce. Například:

* Místo použití webového formuláře, můžete použít objekty BLOB úložiště Azure nebo databázi SQL toostore telefonních čísel a volání text. Informace o použití objektů BLOB služby Azure storage v jazyce PHP najdete v tématu [pomocí Azure Storage s aplikací PHP][howto_blob_storage_php]. Informace o používání databáze SQL v jazyce PHP najdete v tématu [pomocí SQL Database pomocí aplikace PHP][howto_sql_azure_php].
* Hello **makecall.php** kód používá URL poskytnutou Twilio ([http://twimlets.com/message][twimlet_message_url]) tooprovide odpovědi Twilio Markup Language (TwiML), která informuje o tom, jak Twilio tooproceed s hello volání. Například může obsahovat hello TwiML, která je vrácena `<Say>` akci, která má za následek textová hodnota mluvené toohello volání příjemce. Místo použití hello URL poskytnutou Twilio, je sestavení vlastní tooTwilio toorespond služby požadavek; Další informace najdete v tématu [jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce PHP][howto_twilio_voice_sms_php]. Další informace o TwiML lze najít na [http://www.twilio.com/docs/api/twiml][twiml]a další informace o `<Say>` a ostatní operace Twilio naleznete na adrese [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Přečtěte si pokyny pro zabezpečení Twilio hello v [https://www.twilio.com/docs/security][twilio_docs_security].

Další informace o Twilio najdete v tématu [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Viz také
* [Jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce PHP](partner-twilio-php-how-to-use-voice-sms.md)

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
