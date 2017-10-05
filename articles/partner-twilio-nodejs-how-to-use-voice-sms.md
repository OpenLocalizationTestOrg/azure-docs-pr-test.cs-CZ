---
title: "Použití Twilio pro hlasové, VoIP a zasílání zpráv SMS v Azure"
description: "Naučte se telefonní hovor a odeslání zprávy SMS službou Twilio rozhraní API v Azure. Ukázky kódu jsou vytvořeny v Node.js."
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
ms.openlocfilehash: 44ec97812130d41d75be98fc8e2d846b7cb5c913
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a><span data-ttu-id="a7f5d-104">Použití Twilio pro hlasové, VoIP a zasílání zpráv SMS v Azure</span><span class="sxs-lookup"><span data-stu-id="a7f5d-104">Using Twilio for Voice, VoIP, and SMS Messaging in Azure</span></span>
<span data-ttu-id="a7f5d-105">Tato příručka ukazuje, jak vytvářet aplikace, které komunikují s Twilio a node.js v Azure.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-105">This guide demonstrates how to build apps that communicate with Twilio and node.js on Azure.</span></span>

<a id="whatis"/>

## <a name="what-is-twilio"></a><span data-ttu-id="a7f5d-106">Co je Twilio?</span><span class="sxs-lookup"><span data-stu-id="a7f5d-106">What is Twilio?</span></span>
<span data-ttu-id="a7f5d-107">Twilio je platforma rozhraní API, která usnadňuje vývojářům zkontrolujte a příjem telefonních hovorů, odesílat a přijímat textové zprávy a vložení volání VoIP do mobilní aplikace založené na prohlížeči a nativní.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-107">Twilio is an API platform that makes it easy for developers to make and receive phone calls, send and receive text messages, and embed VoIP calling into browser-based and native mobile applications.</span></span> <span data-ttu-id="a7f5d-108">Pojďme se stručně přenášeny po tom, jak to funguje než začnete.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-108">Let's briefly go over how this works before diving in.</span></span>

### <a name="receiving-calls-and-text-messages"></a><span data-ttu-id="a7f5d-109">Přijímá volání a textové zprávy</span><span class="sxs-lookup"><span data-stu-id="a7f5d-109">Receiving Calls and Text Messages</span></span>
<span data-ttu-id="a7f5d-110">Twilio umožňuje vývojářům [zakoupit programovatelný telefonní čísla] [ purchase_phone] kterého lze odesílat i přijímat volání a textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-110">Twilio allows developers to [purchase programmable phone numbers][purchase_phone] which can be used to both send and receive calls and text messages.</span></span> <span data-ttu-id="a7f5d-111">Když číslo Twilio obdrží příchozí volání nebo text, Twilio odešle vaši webovou aplikaci na HTTP POST nebo GET požadavku, žádostí pokyny o tom, jak zpracovat hovor nebo textovou.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-111">When a Twilio number receives an inbound call or text, Twilio will send your web application an HTTP POST or GET request, asking you for instructions on how to handle the call or text.</span></span> <span data-ttu-id="a7f5d-112">Váš server bude reagovat na požadavek HTTP je Twilio s [TwiML][twiml], jednoduchou sadou značky XML, které obsahují pokyny, jak zpracovat hovor nebo textovou.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-112">Your server will respond to Twilio's HTTP request with [TwiML][twiml], a simple set of XML tags that contain instructions on how to handle a call or text.</span></span> <span data-ttu-id="a7f5d-113">Příklady TwiML jsme se zobrazí pouze za chvíli.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-113">We will see examples of TwiML in just a moment.</span></span>

### <a name="making-calls-and-sending-text-messages"></a><span data-ttu-id="a7f5d-114">Volání a posílat textové zprávy</span><span class="sxs-lookup"><span data-stu-id="a7f5d-114">Making Calls and Sending Text Messages</span></span>
<span data-ttu-id="a7f5d-115">Vývojáře můžou pomocí požadavků protokolu HTTP k Twilio web service API, odeslání textové zprávy nebo zahájit odchozí telefonní hovory.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-115">By making HTTP requests to the Twilio web service API, developers can send text messages or initiate outbound phone calls.</span></span> <span data-ttu-id="a7f5d-116">Pro odchozí volání musí vývojář také zadejte adresu URL, která vrátí TwiML pokyny, jak zpracovat odchozí volání po připojení.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-116">For outbound calls, the developer must also specify a URL that returns TwiML instructions for how to handle the outbound call once it is connected.</span></span>

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a><span data-ttu-id="a7f5d-117">Vložení VoIP možnosti v kódu uživatelského rozhraní (JavaScript, iOS nebo Android)</span><span class="sxs-lookup"><span data-stu-id="a7f5d-117">Embedding VoIP Capabilities in UI code (JavaScript, iOS, or Android)</span></span>
<span data-ttu-id="a7f5d-118">Twilio poskytuje klienta SDK, který můžete upravit všechny plochy webový prohlížeč, aplikace pro iOS nebo Android aplikace na telefon VoIP.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-118">Twilio provides a client-side SDK which can turn any desktop web browser, iOS app, or Android app into a VoIP phone.</span></span> <span data-ttu-id="a7f5d-119">V tomto článku se zaměříme na tom, jak používat VoIP volání v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-119">In this article, we will focus on how to use VoIP calling in the browser.</span></span> <span data-ttu-id="a7f5d-120">Kromě *Twilio JavaScript SDK* spuštěný v prohlížeči, musí se aplikace na straně serveru (naše aplikace node.js) použít k vydání token"funkce" klientovi JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-120">In addition to the *Twilio JavaScript SDK* running in the browser, a server-side application (our node.js application) must be used to issue a "capability token" to the JavaScript client.</span></span> <span data-ttu-id="a7f5d-121">Další informace o používání VoIP s node.js [na blog vývojářů Twilio][voipnode].</span><span class="sxs-lookup"><span data-stu-id="a7f5d-121">You can read more about using VoIP with node.js [on the Twilio dev blog][voipnode].</span></span>

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a><span data-ttu-id="a7f5d-122">Zaregistrovat se k Twilio (Microsoft slevy)</span><span class="sxs-lookup"><span data-stu-id="a7f5d-122">Sign Up For Twilio (Microsoft Discount)</span></span>
<span data-ttu-id="a7f5d-123">Před použitím služby Twilio, musíte nejdřív [zaregistrujte si účet][signup].</span><span class="sxs-lookup"><span data-stu-id="a7f5d-123">Before using Twilio services, you must first [sign up for an account][signup].</span></span> <span data-ttu-id="a7f5d-124">Microsoft Azure zákazníků získávání speciální slevy - [nezapomeňte zaregistrujte si zde][signup]!</span><span class="sxs-lookup"><span data-stu-id="a7f5d-124">Microsoft Azure customers receive a special discount - [be sure to sign up here][signup]!</span></span>

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a><span data-ttu-id="a7f5d-125">Vytvoření a nasazení webu Azure node.js</span><span class="sxs-lookup"><span data-stu-id="a7f5d-125">Create and Deploy a node.js Azure Website</span></span>
<span data-ttu-id="a7f5d-126">Dále musíte vytvořit web node.js spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-126">Next, you will need to create a node.js website running on Azure.</span></span> <span data-ttu-id="a7f5d-127">[Zde se nachází v oficiální dokumentaci tohoto postupu][azure_new_site].</span><span class="sxs-lookup"><span data-stu-id="a7f5d-127">[The official documentation for doing this is located here][azure_new_site].</span></span> <span data-ttu-id="a7f5d-128">Na vysoké úrovni je bude možné následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a7f5d-128">At a high level, you will be doing the following:</span></span>

* <span data-ttu-id="a7f5d-129">Zaregistrujete účet Azure, pokud již nemáte</span><span class="sxs-lookup"><span data-stu-id="a7f5d-129">Signing up for an Azure account, if you don't have one already</span></span>
* <span data-ttu-id="a7f5d-130">Pomocí konzole pro správu Azure k vytvoření nového webu</span><span class="sxs-lookup"><span data-stu-id="a7f5d-130">Using the Azure admin console to create a new website</span></span>
* <span data-ttu-id="a7f5d-131">Přidání podpory zdroj ovládacího prvku (budeme předpokládat, že jste použili git)</span><span class="sxs-lookup"><span data-stu-id="a7f5d-131">Adding source control support (we will assume you used git)</span></span>
* <span data-ttu-id="a7f5d-132">Vytvoření souboru `server.js` s webovou aplikací jednoduché node.js</span><span class="sxs-lookup"><span data-stu-id="a7f5d-132">Creating a file `server.js` with a simple node.js web application</span></span>
* <span data-ttu-id="a7f5d-133">Nasazení této jednoduché aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="a7f5d-133">Deploying this simple application to Azure</span></span>

<a id="twiliomodule"/>

## <a name="configure-the-twilio-module"></a><span data-ttu-id="a7f5d-134">Konfigurace modulu Twilio</span><span class="sxs-lookup"><span data-stu-id="a7f5d-134">Configure the Twilio Module</span></span>
<span data-ttu-id="a7f5d-135">V dalším kroku začneme psát jednoduché node.js aplikace, které využívá rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-135">Next, we will begin to write a simple node.js application which makes use of the Twilio API.</span></span> <span data-ttu-id="a7f5d-136">Než začneme, je potřeba nakonfigurovat naše přihlašovací údaje účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-136">Before we begin, we need to configure our Twilio account credentials.</span></span>

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a><span data-ttu-id="a7f5d-137">Konfigurace přihlašovacích údajů pro Twilio v proměnných prostředí systému</span><span class="sxs-lookup"><span data-stu-id="a7f5d-137">Configuring Twilio Credentials in System Environment Variables</span></span>
<span data-ttu-id="a7f5d-138">Aby bylo možné ověření požadavků na back-end Twilio, potřebujeme naše SID účtu a ověřování tokenem, které slouží jako uživatelské jméno a heslo pro účet naše Twilio.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-138">In order to make authenticated requests against the Twilio back end, we need our account SID and auth token, which function as the username and password set for our Twilio account.</span></span> <span data-ttu-id="a7f5d-139">Nejbezpečnější způsob nakonfigurujete pro použití s modulem uzlu v Azure je prostřednictvím systémové proměnné prostředí, které můžete nastavit přímo v konzole pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-139">The most secure way to configure these for use with the node module in Azure is via system environment variables, which you can set directly in the Azure admin console.</span></span>

<span data-ttu-id="a7f5d-140">Vyberte svůj web node.js a klikněte na odkaz "Konfigurace".</span><span class="sxs-lookup"><span data-stu-id="a7f5d-140">Select your node.js website, and click the "CONFIGURE" link.</span></span>  <span data-ttu-id="a7f5d-141">Posuňte se dolů bit, zobrazí se oblast, kde můžete nastavit vlastnosti konfigurace pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-141">If you scroll down a bit, you will see an area where you can set configuration properties for your application.</span></span>  <span data-ttu-id="a7f5d-142">Zadejte přihlašovací údaje účtu Twilio ([nalezen na konzole Twilio][twilio_console]) jak je znázorněno - zkontrolujte, zda je název `TWILIO_ACCOUNT_SID` a `TWILIO_AUTH_TOKEN`, v uvedeném pořadí:</span><span class="sxs-lookup"><span data-stu-id="a7f5d-142">Enter your Twilio account credentials ([found on your Twilio Console][twilio_console]) as shown - make sure to name them `TWILIO_ACCOUNT_SID` and `TWILIO_AUTH_TOKEN`, respectively:</span></span>

![Konzola pro správu Azure][azure-admin-console]

<span data-ttu-id="a7f5d-144">Po nakonfigurování těchto proměnných restartujte aplikaci v konzole Azure.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-144">Once you have configured these variables, restart your application in the Azure console.</span></span>

### <a name="declaring-the-twilio-module-in-packagejson"></a><span data-ttu-id="a7f5d-145">Deklarování Twilio modul v souboru package.json</span><span class="sxs-lookup"><span data-stu-id="a7f5d-145">Declaring the Twilio module in package.json</span></span>
<span data-ttu-id="a7f5d-146">Dále je nutné vytvořit package.json ke správě závislosti modulu naše uzlu prostřednictvím [npm].</span><span class="sxs-lookup"><span data-stu-id="a7f5d-146">Next, we need to create a package.json to manage our node module dependencies via [npm].</span></span> <span data-ttu-id="a7f5d-147">Na stejné úrovni jako `server.js` jste vytvořili v souboru *Azure/node.js* kurzu, vytvořte soubor s názvem `package.json`.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-147">At the same level as the `server.js` file you created in the *Azure/node.js* tutorial, create a file named `package.json`.</span></span>  <span data-ttu-id="a7f5d-148">Uvnitř tohoto souboru umístíte následující:</span><span class="sxs-lookup"><span data-stu-id="a7f5d-148">Inside this file, place the following:</span></span>

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

<span data-ttu-id="a7f5d-149">Tento modul twilio deklaruje jako závislost, stejně jako oblíbených [Express webová architektura] [ express] a modul EJS šablony.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-149">This declares the twilio module as a dependency, as well as the popular [Express web framework][express] and the EJS template engine.</span></span>  <span data-ttu-id="a7f5d-150">V pořádku teď jsme je vše připraveno – umožňuje napsat kód!</span><span class="sxs-lookup"><span data-stu-id="a7f5d-150">Okay, now we're all set - let's write some code!</span></span>

<a id="makecall"/>

## <a name="make-an-outbound-call"></a><span data-ttu-id="a7f5d-151">Odchozí volání</span><span class="sxs-lookup"><span data-stu-id="a7f5d-151">Make an Outbound Call</span></span>
<span data-ttu-id="a7f5d-152">Umožňuje vytvořit jednoduché formulář, který bude zavolat na číslo, že jsme vyberete.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-152">Let's create a simple form that will place a call to a number we choose.</span></span> <span data-ttu-id="a7f5d-153">Otevřete `server.js`a zadejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-153">Open up `server.js`, and enter the following code.</span></span> <span data-ttu-id="a7f5d-154">Všimněte si, kam se říká "CHANGE_ME" - uveďte název vašeho webu azure existuje:</span><span class="sxs-lookup"><span data-stu-id="a7f5d-154">Note where it says "CHANGE_ME" - put the name of your azure website there:</span></span>

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

// Render an HTML user interface for the application's home page
app.get('/', (request, response) => response.render('index'));

// Handle the form POST to place a call
app.post('/call', (request, response) => {
  var client = twilio(accountSid, authToken);

  client.makeCall({
    // make a call to this number
    to:request.body.number,

    // Change to a Twilio number you bought - see:
    // https://www.twilio.com/console/phone-numbers/incoming
    from:'+15558675309',

    // A URL in our app which generates TwiML
    // Change "CHANGE_ME" to your app's name
    url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});

// Generate TwiML to handle an outbound call
app.post('/outbound_call', (request, response) => {
  var twiml = new twilio.TwimlResponse();

  // Say a message to the call's receiver
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

<span data-ttu-id="a7f5d-155">Dále vytvořte adresář s názvem `views` – v tomto adresáři vytvořte soubor s názvem `index.ejs` s tímto obsahem:</span><span class="sxs-lookup"><span data-stu-id="a7f5d-155">Next, create a directory called `views` - inside this directory, create a file named `index.ejs` with the following contents:</span></span>

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
      <input type="submit" value="Call the number above"/>
  </form>
</body>
</html>
```

<span data-ttu-id="a7f5d-156">Nyní nasazení webu do Azure a otevřete vaším domovem.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-156">Now, deploy your website to Azure and open your home.</span></span> <span data-ttu-id="a7f5d-157">Nyní byste měli mít do textového pole zadejte vaše telefonní číslo a přijímat volání z vaše číslo Twilio!</span><span class="sxs-lookup"><span data-stu-id="a7f5d-157">You should be able to enter your phone number in the text field, and receive a call from your Twilio number!</span></span>

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a><span data-ttu-id="a7f5d-158">Odeslání zprávy SMS</span><span class="sxs-lookup"><span data-stu-id="a7f5d-158">Send an SMS Message</span></span>
<span data-ttu-id="a7f5d-159">Nyní nastavíme uživatelské rozhraní a zpracování logiky formulář pro odeslání textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-159">Now, let's set up a user interface and form handling logic to send a text message.</span></span> <span data-ttu-id="a7f5d-160">Otevřete `server.js`a přidejte následující kód po posledním volání `app.post`:</span><span class="sxs-lookup"><span data-stu-id="a7f5d-160">Open up `server.js`, and add the following code after the last call to `app.post`:</span></span>

```javascript
app.post('/sms', (request, response) => {
  const client = twilio(accountSid, authToken);

  client.sendSms({
      // send a text to this number
      to:request.body.number,

      // A Twilio number you bought - see:
      // https://www.twilio.com/console/phone-numbers/incoming
      from:'+15558675309',

      // The body of the text message
      body: request.body.message

  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});
```

<span data-ttu-id="a7f5d-161">V `views/index.ejs`, přidejte jiný formulář pod první z nich pro odeslání číslo a textové zprávy:</span><span class="sxs-lookup"><span data-stu-id="a7f5d-161">In `views/index.ejs`, add another form under the first one to submit a number and a text message:</span></span>

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message to send" name="message"/>
  <br/>
  <input type="submit" value="Send text to the number above"/>
</form>
```

<span data-ttu-id="a7f5d-162">Znovu nasaďte aplikaci do Azure a má teď nebudete moct odeslání, které tvoří a odeslání textové zprávy sami (nebo některého z vašich přátel nejbližší)!</span><span class="sxs-lookup"><span data-stu-id="a7f5d-162">Re-deploy your application to Azure, and you should now be able to submit that form and send yourself (or any of your closest friends) a text message!</span></span>

<a id="nextsteps"/>

## <a name="next-steps"></a><span data-ttu-id="a7f5d-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7f5d-163">Next Steps</span></span>
<span data-ttu-id="a7f5d-164">Jste nyní se naučili základy používání node.js a Twilio můžete vytvářet aplikace, které komunikují.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-164">You have now learned the basics of using node.js and Twilio to build apps that communicate.</span></span> <span data-ttu-id="a7f5d-165">Ale tyto příklady sotva scratch prostor co je možné pomocí Twilio a node.js.</span><span class="sxs-lookup"><span data-stu-id="a7f5d-165">But these examples barely scratch the surface of what's possible with Twilio and node.js.</span></span> <span data-ttu-id="a7f5d-166">Další informace pomocí node.js Twilio najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="a7f5d-166">For more information using Twilio with node.js, check out the following resources:</span></span>

* <span data-ttu-id="a7f5d-167">[Oficiální modulu dokumentace][docs]</span><span class="sxs-lookup"><span data-stu-id="a7f5d-167">[Official module docs][docs]</span></span>
* <span data-ttu-id="a7f5d-168">[Kurz týkající se VoIP s aplikacemi node.js][voipnode]</span><span class="sxs-lookup"><span data-stu-id="a7f5d-168">[Tutorial on VoIP with node.js applications][voipnode]</span></span>
* <span data-ttu-id="a7f5d-169">[Votr – v reálném čase SMS hlasování aplikace s node.js a CouchDB (tři části)][votr]</span><span class="sxs-lookup"><span data-stu-id="a7f5d-169">[Votr - a real-time SMS voting application with node.js and CouchDB (three parts)][votr]</span></span>
* <span data-ttu-id="a7f5d-170">[Pár programování v prohlížeči s node.js][pair]</span><span class="sxs-lookup"><span data-stu-id="a7f5d-170">[Pair programming in the browser with node.js][pair]</span></span>

<span data-ttu-id="a7f5d-171">Doufáme, že rádi hackerům node.js a Twilio v Azure!</span><span class="sxs-lookup"><span data-stu-id="a7f5d-171">We hope you love hacking node.js and Twilio on Azure!</span></span>

[purchase_phone]: https://www.twilio.com/console/phone-numbers/search
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: app-service-web/app-service-web-get-started-nodejs.md
[twilio_console]: https://www.twilio.com/console
<span data-ttu-id="a7f5d-172">[npm]: http://npmjs.org</span><span class="sxs-lookup"><span data-stu-id="a7f5d-172">[npm]: http://npmjs.org</span></span>
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png
