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
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a><span data-ttu-id="492ab-104">Použití Twilio pro hlasové, VoIP a zasílání zpráv SMS v Azure</span><span class="sxs-lookup"><span data-stu-id="492ab-104">Using Twilio for Voice, VoIP, and SMS Messaging in Azure</span></span>
<span data-ttu-id="492ab-105">Tato příručka ukazuje, jak toobuild aplikace, které komunikují Twilio a node.js v Azure.</span><span class="sxs-lookup"><span data-stu-id="492ab-105">This guide demonstrates how toobuild apps that communicate with Twilio and node.js on Azure.</span></span>

<a id="whatis"/>

## <a name="what-is-twilio"></a><span data-ttu-id="492ab-106">Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="492ab-106">What is Twilio?</span></span>
<span data-ttu-id="492ab-107">Twilio je platforma rozhraní API, která usnadňuje vývojářům toomake a příjem telefonních hovorů, odesílat a přijímat textové zprávy a vložit VoIP volání do založené na prohlížeči, nativními mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="492ab-107">Twilio is an API platform that makes it easy for developers toomake and receive phone calls, send and receive text messages, and embed VoIP calling into browser-based and native mobile applications.</span></span> <span data-ttu-id="492ab-108">Pojďme se stručně přenášeny po tom, jak to funguje než začnete.</span><span class="sxs-lookup"><span data-stu-id="492ab-108">Let's briefly go over how this works before diving in.</span></span>

### <a name="receiving-calls-and-text-messages"></a><span data-ttu-id="492ab-109">Přijímá volání a textové zprávy</span><span class="sxs-lookup"><span data-stu-id="492ab-109">Receiving Calls and Text Messages</span></span>
<span data-ttu-id="492ab-110">Twilio umožňuje vývojářům příliš[zakoupit programovatelný telefonní čísla] [ purchase_phone] může být použité tooboth odesílat a přijímat volání a textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="492ab-110">Twilio allows developers too[purchase programmable phone numbers][purchase_phone] which can be used tooboth send and receive calls and text messages.</span></span> <span data-ttu-id="492ab-111">Když číslo Twilio obdrží příchozí volání nebo text, Twilio odešle vaši webovou aplikaci na HTTP POST nebo GET požadavku, žádostí pokyny na tom, jak toohandle hello hovor nebo textovou.</span><span class="sxs-lookup"><span data-stu-id="492ab-111">When a Twilio number receives an inbound call or text, Twilio will send your web application an HTTP POST or GET request, asking you for instructions on how toohandle hello call or text.</span></span> <span data-ttu-id="492ab-112">Váš server bude reagovat na tooTwilio požadavek HTTP s [TwiML][twiml], jednoduchou sadou značky XML, které obsahují pokyny, jak toohandle volání nebo text.</span><span class="sxs-lookup"><span data-stu-id="492ab-112">Your server will respond tooTwilio's HTTP request with [TwiML][twiml], a simple set of XML tags that contain instructions on how toohandle a call or text.</span></span> <span data-ttu-id="492ab-113">Příklady TwiML jsme se zobrazí pouze za chvíli.</span><span class="sxs-lookup"><span data-stu-id="492ab-113">We will see examples of TwiML in just a moment.</span></span>

### <a name="making-calls-and-sending-text-messages"></a><span data-ttu-id="492ab-114">Volání a posílat textové zprávy</span><span class="sxs-lookup"><span data-stu-id="492ab-114">Making Calls and Sending Text Messages</span></span>
<span data-ttu-id="492ab-115">Vývojáře můžou tím, že toohello požadavky HTTP Twilio web service API, odeslání textové zprávy nebo zahájit odchozí telefonní hovory.</span><span class="sxs-lookup"><span data-stu-id="492ab-115">By making HTTP requests toohello Twilio web service API, developers can send text messages or initiate outbound phone calls.</span></span> <span data-ttu-id="492ab-116">Pro odchozí volání hello vývojáře nutné také zadat adresu URL, která vrátí TwiML pokyny, jak volat toohandle hello odchozí po připojení.</span><span class="sxs-lookup"><span data-stu-id="492ab-116">For outbound calls, hello developer must also specify a URL that returns TwiML instructions for how toohandle hello outbound call once it is connected.</span></span>

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a><span data-ttu-id="492ab-117">Vložení VoIP možnosti v kódu uživatelského rozhraní (JavaScript, iOS nebo Android)</span><span class="sxs-lookup"><span data-stu-id="492ab-117">Embedding VoIP Capabilities in UI code (JavaScript, iOS, or Android)</span></span>
<span data-ttu-id="492ab-118">Twilio poskytuje klienta SDK, který můžete upravit všechny plochy webový prohlížeč, aplikace pro iOS nebo Android aplikace na telefon VoIP.</span><span class="sxs-lookup"><span data-stu-id="492ab-118">Twilio provides a client-side SDK which can turn any desktop web browser, iOS app, or Android app into a VoIP phone.</span></span> <span data-ttu-id="492ab-119">V tomto článku se zaměříme na postupy toouse VoIP volání v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="492ab-119">In this article, we will focus on how toouse VoIP calling in hello browser.</span></span> <span data-ttu-id="492ab-120">V přidání toohello *Twilio JavaScript SDK* spuštěný v prohlížeči hello, aplikace na straně serveru (naše aplikace node.js) musí být použité tooissue toohello "schopností token" JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="492ab-120">In addition toohello *Twilio JavaScript SDK* running in hello browser, a server-side application (our node.js application) must be used tooissue a "capability token" toohello JavaScript client.</span></span> <span data-ttu-id="492ab-121">Další informace o používání VoIP s node.js [na blog vývojářů Twilio hello][voipnode].</span><span class="sxs-lookup"><span data-stu-id="492ab-121">You can read more about using VoIP with node.js [on hello Twilio dev blog][voipnode].</span></span>

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a><span data-ttu-id="492ab-122">Zaregistrovat se k Twilio (Microsoft slevy)</span><span class="sxs-lookup"><span data-stu-id="492ab-122">Sign Up For Twilio (Microsoft Discount)</span></span>
<span data-ttu-id="492ab-123">Před použitím služby Twilio, musíte nejdřív [zaregistrujte si účet][signup].</span><span class="sxs-lookup"><span data-stu-id="492ab-123">Before using Twilio services, you must first [sign up for an account][signup].</span></span> <span data-ttu-id="492ab-124">Microsoft Azure zákazníků získávání speciální slevy - [být jisti toosign vytvořit tady][signup]!</span><span class="sxs-lookup"><span data-stu-id="492ab-124">Microsoft Azure customers receive a special discount - [be sure toosign up here][signup]!</span></span>

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a><span data-ttu-id="492ab-125">Vytvoření a nasazení webu Azure node.js</span><span class="sxs-lookup"><span data-stu-id="492ab-125">Create and Deploy a node.js Azure Website</span></span>
<span data-ttu-id="492ab-126">V dalším kroku budete potřebovat toocreate webu node.js spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="492ab-126">Next, you will need toocreate a node.js website running on Azure.</span></span> <span data-ttu-id="492ab-127">[Díky tomuto Hello oficiální dokumentaci se nachází zde][azure_new_site].</span><span class="sxs-lookup"><span data-stu-id="492ab-127">[hello official documentation for doing this is located here][azure_new_site].</span></span> <span data-ttu-id="492ab-128">Na vysoké úrovni bude to hello následující:</span><span class="sxs-lookup"><span data-stu-id="492ab-128">At a high level, you will be doing hello following:</span></span>

* <span data-ttu-id="492ab-129">Zaregistrujete účet Azure, pokud již nemáte</span><span class="sxs-lookup"><span data-stu-id="492ab-129">Signing up for an Azure account, if you don't have one already</span></span>
* <span data-ttu-id="492ab-130">Pomocí hello Azure správce konzoly toocreate nového webu</span><span class="sxs-lookup"><span data-stu-id="492ab-130">Using hello Azure admin console toocreate a new website</span></span>
* <span data-ttu-id="492ab-131">Přidání podpory zdroj ovládacího prvku (budeme předpokládat, že jste použili git)</span><span class="sxs-lookup"><span data-stu-id="492ab-131">Adding source control support (we will assume you used git)</span></span>
* <span data-ttu-id="492ab-132">Vytvoření souboru `server.js` s webovou aplikací jednoduché node.js</span><span class="sxs-lookup"><span data-stu-id="492ab-132">Creating a file `server.js` with a simple node.js web application</span></span>
* <span data-ttu-id="492ab-133">Nasazení této tooAzure jednoduchou aplikaci</span><span class="sxs-lookup"><span data-stu-id="492ab-133">Deploying this simple application tooAzure</span></span>

<a id="twiliomodule"/>

## <a name="configure-hello-twilio-module"></a><span data-ttu-id="492ab-134">Konfigurace hello Twilio modulu</span><span class="sxs-lookup"><span data-stu-id="492ab-134">Configure hello Twilio Module</span></span>
<span data-ttu-id="492ab-135">V dalším kroku začneme toowrite aplikace jednoduché node.js, díky čemuž použití hello Twilio rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="492ab-135">Next, we will begin toowrite a simple node.js application which makes use of hello Twilio API.</span></span> <span data-ttu-id="492ab-136">Než můžeme začít, potřebujeme tooconfigure naše přihlašovací údaje účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="492ab-136">Before we begin, we need tooconfigure our Twilio account credentials.</span></span>

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a><span data-ttu-id="492ab-137">Konfigurace přihlašovacích údajů pro Twilio v proměnných prostředí systému</span><span class="sxs-lookup"><span data-stu-id="492ab-137">Configuring Twilio Credentials in System Environment Variables</span></span>
<span data-ttu-id="492ab-138">V pořadí toomake ověření požadavků na back-end hello Twilio potřebujeme naše SID účtu a ověřování tokenem, které slouží jako hello uživatelské jméno a heslo pro účet naše Twilio.</span><span class="sxs-lookup"><span data-stu-id="492ab-138">In order toomake authenticated requests against hello Twilio back end, we need our account SID and auth token, which function as hello username and password set for our Twilio account.</span></span> <span data-ttu-id="492ab-139">nejbezpečnější způsob tooconfigure Hello tyto pro použití s modulem uzlu hello v Azure je prostřednictvím systémové proměnné prostředí, které můžete nastavit přímo v konzole pro správu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="492ab-139">hello most secure way tooconfigure these for use with hello node module in Azure is via system environment variables, which you can set directly in hello Azure admin console.</span></span>

<span data-ttu-id="492ab-140">Vyberte svůj web node.js a klikněte na odkaz "Konfigurace" hello.</span><span class="sxs-lookup"><span data-stu-id="492ab-140">Select your node.js website, and click hello "CONFIGURE" link.</span></span>  <span data-ttu-id="492ab-141">Posuňte se dolů bit, zobrazí se oblast, kde můžete nastavit vlastnosti konfigurace pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="492ab-141">If you scroll down a bit, you will see an area where you can set configuration properties for your application.</span></span>  <span data-ttu-id="492ab-142">Zadejte přihlašovací údaje účtu Twilio ([nalezen na konzole Twilio][twilio_console]) jak je znázorněno – Ujistěte se, že tooname je `TWILIO_ACCOUNT_SID` a `TWILIO_AUTH_TOKEN`, v uvedeném pořadí:</span><span class="sxs-lookup"><span data-stu-id="492ab-142">Enter your Twilio account credentials ([found on your Twilio Console][twilio_console]) as shown - make sure tooname them `TWILIO_ACCOUNT_SID` and `TWILIO_AUTH_TOKEN`, respectively:</span></span>

![Konzola pro správu Azure][azure-admin-console]

<span data-ttu-id="492ab-144">Po nakonfigurování těchto proměnných restartování aplikace v hello konzoly Azure.</span><span class="sxs-lookup"><span data-stu-id="492ab-144">Once you have configured these variables, restart your application in hello Azure console.</span></span>

### <a name="declaring-hello-twilio-module-in-packagejson"></a><span data-ttu-id="492ab-145">Deklarování hello Twilio modul v souboru package.json</span><span class="sxs-lookup"><span data-stu-id="492ab-145">Declaring hello Twilio module in package.json</span></span>
<span data-ttu-id="492ab-146">Dále je třeba toocreate package.json toomanage naše závislosti modulu uzlu prostřednictvím [npm].</span><span class="sxs-lookup"><span data-stu-id="492ab-146">Next, we need toocreate a package.json toomanage our node module dependencies via [npm].</span></span> <span data-ttu-id="492ab-147">V hello na stejné úrovni jako hello `server.js` soubor vytvořený v hello *Azure/node.js* kurzu, vytvořte soubor s názvem `package.json`.</span><span class="sxs-lookup"><span data-stu-id="492ab-147">At hello same level as hello `server.js` file you created in hello *Azure/node.js* tutorial, create a file named `package.json`.</span></span>  <span data-ttu-id="492ab-148">Uvnitř tohoto souboru umístěte hello následující:</span><span class="sxs-lookup"><span data-stu-id="492ab-148">Inside this file, place hello following:</span></span>

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

<span data-ttu-id="492ab-149">To deklaruje hello twilio modulu jako závislostí, jakož i hello oblíbených [Express webová architektura] [ express] a modulu šablon EJS hello.</span><span class="sxs-lookup"><span data-stu-id="492ab-149">This declares hello twilio module as a dependency, as well as hello popular [Express web framework][express] and hello EJS template engine.</span></span>  <span data-ttu-id="492ab-150">V pořádku teď jsme je vše připraveno – umožňuje napsat kód!</span><span class="sxs-lookup"><span data-stu-id="492ab-150">Okay, now we're all set - let's write some code!</span></span>

<a id="makecall"/>

## <a name="make-an-outbound-call"></a><span data-ttu-id="492ab-151">Odchozí volání</span><span class="sxs-lookup"><span data-stu-id="492ab-151">Make an Outbound Call</span></span>
<span data-ttu-id="492ab-152">Umožňuje vytvořit jednoduché formulář, který umístí číslo tooa volání, který jsme zvolili.</span><span class="sxs-lookup"><span data-stu-id="492ab-152">Let's create a simple form that will place a call tooa number we choose.</span></span> <span data-ttu-id="492ab-153">Otevřete `server.js`a zadejte následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="492ab-153">Open up `server.js`, and enter hello following code.</span></span> <span data-ttu-id="492ab-154">Všimněte si, kam se říká "CHANGE_ME" - put hello název vašeho webu azure existuje:</span><span class="sxs-lookup"><span data-stu-id="492ab-154">Note where it says "CHANGE_ME" - put hello name of your azure website there:</span></span>

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

<span data-ttu-id="492ab-155">Dále vytvořte adresář s názvem `views` – v tomto adresáři vytvořte soubor s názvem `index.ejs` s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="492ab-155">Next, create a directory called `views` - inside this directory, create a file named `index.ejs` with hello following contents:</span></span>

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

<span data-ttu-id="492ab-156">Teď nasaďte tooAzure vašeho webu a otevřete vaším domovem.</span><span class="sxs-lookup"><span data-stu-id="492ab-156">Now, deploy your website tooAzure and open your home.</span></span> <span data-ttu-id="492ab-157">By měl být schopný tooenter vaše telefonní číslo do pole text hello a přijímat volání z vaše číslo Twilio!</span><span class="sxs-lookup"><span data-stu-id="492ab-157">You should be able tooenter your phone number in hello text field, and receive a call from your Twilio number!</span></span>

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a><span data-ttu-id="492ab-158">Odeslání zprávy SMS</span><span class="sxs-lookup"><span data-stu-id="492ab-158">Send an SMS Message</span></span>
<span data-ttu-id="492ab-159">Nyní nastavíme uživatelské rozhraní a formulář zpracování logiky toosend textovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="492ab-159">Now, let's set up a user interface and form handling logic toosend a text message.</span></span> <span data-ttu-id="492ab-160">Otevřete `server.js`a přidejte následující kód po posledním volání hello příliš hello`app.post`:</span><span class="sxs-lookup"><span data-stu-id="492ab-160">Open up `server.js`, and add hello following code after hello last call too`app.post`:</span></span>

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

<span data-ttu-id="492ab-161">V `views/index.ejs`, přidejte jiný formulář pod hello první z nich toosubmit číslo a textové zprávy:</span><span class="sxs-lookup"><span data-stu-id="492ab-161">In `views/index.ejs`, add another form under hello first one toosubmit a number and a text message:</span></span>

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message toosend" name="message"/>
  <br/>
  <input type="submit" value="Send text toohello number above"/>
</form>
```

<span data-ttu-id="492ab-162">Znovu nasaďte tooAzure vaší aplikace a nyní byste měli mít toosubmit, která tvoří a odeslání textové zprávy sami (nebo některého z vašich přátel nejbližší)!</span><span class="sxs-lookup"><span data-stu-id="492ab-162">Re-deploy your application tooAzure, and you should now be able toosubmit that form and send yourself (or any of your closest friends) a text message!</span></span>

<a id="nextsteps"/>

## <a name="next-steps"></a><span data-ttu-id="492ab-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="492ab-163">Next Steps</span></span>
<span data-ttu-id="492ab-164">Jste nyní se naučili základy hello pomocí node.js a Twilio toobuild aplikace, které komunikují.</span><span class="sxs-lookup"><span data-stu-id="492ab-164">You have now learned hello basics of using node.js and Twilio toobuild apps that communicate.</span></span> <span data-ttu-id="492ab-165">Ale tyto příklady sotva scratch prostor hello co je možné pomocí Twilio a node.js.</span><span class="sxs-lookup"><span data-stu-id="492ab-165">But these examples barely scratch hello surface of what's possible with Twilio and node.js.</span></span> <span data-ttu-id="492ab-166">Další informace pomocí node.js Twilio projděte si hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="492ab-166">For more information using Twilio with node.js, check out hello following resources:</span></span>

* <span data-ttu-id="492ab-167">[Oficiální modulu dokumentace][docs]</span><span class="sxs-lookup"><span data-stu-id="492ab-167">[Official module docs][docs]</span></span>
* <span data-ttu-id="492ab-168">[Kurz týkající se VoIP s aplikacemi node.js][voipnode]</span><span class="sxs-lookup"><span data-stu-id="492ab-168">[Tutorial on VoIP with node.js applications][voipnode]</span></span>
* <span data-ttu-id="492ab-169">[Votr – v reálném čase SMS hlasování aplikace s node.js a CouchDB (tři části)][votr]</span><span class="sxs-lookup"><span data-stu-id="492ab-169">[Votr - a real-time SMS voting application with node.js and CouchDB (three parts)][votr]</span></span>
* <span data-ttu-id="492ab-170">[Pár programování v prohlížeči hello s node.js][pair]</span><span class="sxs-lookup"><span data-stu-id="492ab-170">[Pair programming in hello browser with node.js][pair]</span></span>

<span data-ttu-id="492ab-171">Doufáme, že rádi hackerům node.js a Twilio v Azure!</span><span class="sxs-lookup"><span data-stu-id="492ab-171">We hope you love hacking node.js and Twilio on Azure!</span></span>

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
