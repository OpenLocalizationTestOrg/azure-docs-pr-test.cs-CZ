---
title: "aaaUsing Twilio pro hlasové, VoIP a zasílání zpráv SMS v Azure"
description: "Zjistěte, jak toomake telefonní hovor a odesílání SMS zprávy službou hello Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořeny v Node.js."
services: 
documentationcenter: nodejs
author: devinrader
manager: wpickett
editor: 
ms.assetid: f558cbbd-13d2-416f-b9b1-33a99c426af9
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/25/2014
ms.author: wpickett
ms.openlocfilehash: 6c44d60e217fcdf51e69fd2a8197f979afbb507a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Použití Twilio pro hlasové, VoIP a zasílání zpráv SMS v Azure
Tato příručka ukazuje, jak toobuild aplikace, které komunikují Twilio a node.js v Azure.

<a id="whatis"/>

## <a name="what-is-twilio"></a>Co je Twilio?
Twilio je platforma rozhraní API, která usnadňuje vývojářům toomake a příjem telefonních hovorů, odesílat a přijímat textové zprávy a vložit VoIP volání do založené na prohlížeči, nativními mobilní aplikace. Pojďme se stručně přenášeny po tom, jak to funguje než začnete.

### <a name="receiving-calls-and-text-messages"></a>Přijímá volání a textové zprávy
Twilio umožňuje vývojářům příliš[zakoupit programovatelný telefonní čísla] [ purchase_phone] může být použité tooboth odesílat a přijímat volání a textové zprávy. Když číslo Twilio obdrží příchozí volání nebo text, Twilio odešle vaši webovou aplikaci na HTTP POST nebo GET požadavku, žádostí pokyny na tom, jak toohandle hello hovor nebo textovou. Váš server bude reagovat na tooTwilio požadavek HTTP s [TwiML][twiml], jednoduchou sadou značky XML, které obsahují pokyny, jak toohandle volání nebo text. Příklady TwiML jsme se zobrazí pouze za chvíli.

### <a name="making-calls-and-sending-text-messages"></a>Volání a posílat textové zprávy
Vývojáře můžou tím, že toohello požadavky HTTP Twilio web service API, odeslání textové zprávy nebo zahájit odchozí telefonní hovory. Pro odchozí volání hello vývojáře nutné také zadat adresu URL, která vrátí TwiML pokyny, jak volat toohandle hello odchozí po připojení.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Vložení VoIP možnosti v kódu uživatelského rozhraní (JavaScript, iOS nebo Android)
Twilio poskytuje klienta SDK, který můžete upravit všechny plochy webový prohlížeč, aplikace pro iOS nebo Android aplikace na telefon VoIP. V tomto článku se zaměříme na postupy toouse VoIP volání v prohlížeči hello. V přidání toohello *Twilio JavaScript SDK* spuštěný v prohlížeči hello, aplikace na straně serveru (naše aplikace node.js) musí být použité tooissue toohello "schopností token" JavaScript klienta. Další informace o používání VoIP s node.js [na blog vývojářů Twilio hello][voipnode].

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a>Zaregistrovat se k Twilio (Microsoft slevy)
Před použitím služby Twilio, musíte nejdřív [zaregistrujte si účet][signup]. Microsoft Azure zákazníků získávání speciální slevy - [být jisti toosign vytvořit tady][signup]!

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a>Vytvoření a nasazení webu Azure node.js
V dalším kroku budete potřebovat toocreate webu node.js spuštěné v Azure. [Díky tomuto Hello oficiální dokumentaci se nachází zde][azure_new_site]. Na vysoké úrovni bude to hello následující:

* Zaregistrujete účet Azure, pokud již nemáte
* Pomocí hello Azure správce konzoly toocreate nového webu
* Přidání podpory zdroj ovládacího prvku (budeme předpokládat, že jste použili git)
* Vytvoření souboru `server.js` s webovou aplikací jednoduché node.js
* Nasazení této tooAzure jednoduchou aplikaci

<a id="twiliomodule"/>

## <a name="configure-hello-twilio-module"></a>Konfigurace hello Twilio modulu
V dalším kroku začneme toowrite aplikace jednoduché node.js, díky čemuž použití hello Twilio rozhraní API. Než můžeme začít, potřebujeme tooconfigure naše přihlašovací údaje účtu Twilio.

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Konfigurace přihlašovacích údajů pro Twilio v proměnných prostředí systému
V pořadí toomake ověření požadavků na back-end hello Twilio potřebujeme naše SID účtu a ověřování tokenem, které slouží jako hello uživatelské jméno a heslo pro účet naše Twilio. nejbezpečnější způsob tooconfigure Hello tyto pro použití s modulem uzlu hello v Azure je prostřednictvím systémové proměnné prostředí, které můžete nastavit přímo v konzole pro správu Azure hello.

Vyberte svůj web node.js a klikněte na odkaz "Konfigurace" hello.  Posuňte se dolů bit, zobrazí se oblast, kde můžete nastavit vlastnosti konfigurace pro vaši aplikaci.  Zadejte přihlašovací údaje účtu Twilio ([nalezen na konzole Twilio][twilio_console]) jak je znázorněno – Ujistěte se, že tooname je `TWILIO_ACCOUNT_SID` a `TWILIO_AUTH_TOKEN`, v uvedeném pořadí:

![Konzola pro správu Azure][azure-admin-console]

Po nakonfigurování těchto proměnných restartování aplikace v hello konzoly Azure.

### <a name="declaring-hello-twilio-module-in-packagejson"></a>Deklarování hello Twilio modul v souboru package.json
Dále je třeba toocreate package.json toomanage naše závislosti modulu uzlu prostřednictvím [npm]. V hello na stejné úrovni jako hello `server.js` soubor vytvořený v hello *Azure/node.js* kurzu, vytvořte soubor s názvem `package.json`.  Uvnitř tohoto souboru umístěte hello následující:

```json
{
  "name": "application-name",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node server"
  },
  "dependencies": {
    "body-parser": "^1.16.1",
    "ejs": "^2.5.5",
    "errorhandler": "^1.5.0",
    "express": "^4.14.1",
    "morgan": "^1.8.1",
    "twilio": "^2.11.1"
  }
}
```

To deklaruje hello twilio modulu jako závislostí, jakož i hello oblíbených [Express webová architektura] [ express] a modulu šablon EJS hello.  V pořádku teď jsme je vše připraveno – umožňuje napsat kód!

<a id="makecall"/>

## <a name="make-an-outbound-call"></a>Odchozí volání
Umožňuje vytvořit jednoduché formulář, který umístí číslo tooa volání, který jsme zvolili. Otevřete `server.js`a zadejte následující kód hello. Všimněte si, kam se říká "CHANGE_ME" - put hello název vašeho webu azure existuje:

```javascript
// Module dependencies
const express = require('express');
const path = require('path');
const http = require('http');
const twilio = require('twilio');
const logger = require('morgan');
const bodyParser = require('body-parser');
const errorHandler = require('errorhandler');
const accountSid = process.env.TWILIO_ACCOUNT_SID;
const authToken = process.env.TWILIO_AUTH_TOKEN;
// Create Express web application
const app = express();

// Express configuration
app.set('port', process.env.PORT || 3000);
app.set('views', __dirname + '/views');
app.set('view engine', 'ejs');
app.use(logger('tiny'));
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
app.use(express.static(path.join(__dirname, 'public')));

if (app.get('env') !== 'production') {
  app.use(errorHandler());
}

// Render an HTML user interface for hello application's home page
app.get('/', (request, response) => response.render('index'));

// Handle hello form POST tooplace a call
app.post('/call', (request, response) => {
  var client = twilio(accountSid, authToken);

  client.makeCall({
    // make a call toothis number
    to:request.body.number,

    // Change tooa Twilio number you bought - see:
    // https://www.twilio.com/console/phone-numbers/incoming
    from:'+15558675309',

    // A URL in our app which generates TwiML
    // Change "CHANGE_ME" tooyour app's name
    url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
  }, () => {
      // Go back toohello home page
      response.redirect('/');
  });
});

// Generate TwiML toohandle an outbound call
app.post('/outbound_call', (request, response) => {
  var twiml = new twilio.TwimlResponse();

  // Say a message toohello call's receiver
  twiml.say('hello - thanks for checking out Twilio and Azure', {
      voice:'woman'
  });

  response.set('Content-Type', 'text/xml');
  response.send(twiml.toString());
});

// Start server
app.listen(app.get('port'), function(){
  console.log(`Express server listening on port ${app.get('port')}`);
});
```

Dále vytvořte adresář s názvem `views` – v tomto adresáři vytvořte soubor s názvem `index.ejs` s hello následující obsah:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Twilio Test</title>
  <style>
    input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
  </style>
</head>
<body>
  <h1>Twilio Test</h1>
  <form action="/call" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input type="submit" value="Call hello number above"/>
  </form>
</body>
</html>
```

Teď nasaďte tooAzure vašeho webu a otevřete vaším domovem. By měl být schopný tooenter vaše telefonní číslo do pole text hello a přijímat volání z vaše číslo Twilio!

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a>Odeslání zprávy SMS
Nyní nastavíme uživatelské rozhraní a formulář zpracování logiky toosend textovou zprávu. Otevřete `server.js`a přidejte následující kód po posledním volání hello příliš hello`app.post`:

```javascript
app.post('/sms', (request, response) => {
  const client = twilio(accountSid, authToken);

  client.sendSms({
      // send a text toothis number
      to:request.body.number,

      // A Twilio number you bought - see:
      // https://www.twilio.com/console/phone-numbers/incoming
      from:'+15558675309',

      // hello body of hello text message
      body: request.body.message

  }, () => {
      // Go back toohello home page
      response.redirect('/');
  });
});
```

V `views/index.ejs`, přidejte jiný formulář pod hello první z nich toosubmit číslo a textové zprávy:

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message toosend" name="message"/>
  <br/>
  <input type="submit" value="Send text toohello number above"/>
</form>
```

Znovu nasaďte tooAzure vaší aplikace a nyní byste měli mít toosubmit, která tvoří a odeslání textové zprávy sami (nebo některého z vašich přátel nejbližší)!

<a id="nextsteps"/>

## <a name="next-steps"></a>Další kroky
Jste nyní se naučili základy hello pomocí node.js a Twilio toobuild aplikace, které komunikují. Ale tyto příklady sotva scratch prostor hello co je možné pomocí Twilio a node.js. Další informace pomocí node.js Twilio projděte si hello následující prostředky:

* [Oficiální modulu dokumentace][docs]
* [Kurz týkající se VoIP s aplikacemi node.js][voipnode]
* [Votr – v reálném čase SMS hlasování aplikace s node.js a CouchDB (tři části)][votr]
* [Pár programování v prohlížeči hello s node.js][pair]

Doufáme, že rádi hackerům node.js a Twilio v Azure!

[purchase_phone]: https://www.twilio.com/console/phone-numbers/search
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: app-service-web/app-service-web-get-started-nodejs.md
[twilio_console]: https://www.twilio.com/console
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png
