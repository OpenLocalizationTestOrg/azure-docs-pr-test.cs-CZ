---
title: "Vytvořte modul Azure IoT Edge s Node.js | Microsoft Docs"
description: "V tomto kurzu umožňující prezentovat jak napsat modulu převaděč dat zakázat pomocí nejnovější Azure IoT Edge NPM balíčky a Yeoman generátor."
services: iot-hub
author: sushi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: article
ms.date: 06/28/2017
ms.author: sushi
ms.openlocfilehash: ba466f47e157d805600c41fa3d84ed5a0363969c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a><span data-ttu-id="a61fe-103">Vytvořte modul Azure IoT Edge s Node.js</span><span class="sxs-lookup"><span data-stu-id="a61fe-103">Create an Azure IoT Edge Module with Node.js</span></span>

<span data-ttu-id="a61fe-104">V tomto kurzu umožňující prezentovat vytvoření modulu pro Azure IoT hrany v JS.</span><span class="sxs-lookup"><span data-stu-id="a61fe-104">This tutorial showcases how to create a module for Azure IoT Edge in JS.</span></span>

<span data-ttu-id="a61fe-105">V tomto kurzu jsme provede procesem nastavení prostředí a jak napsat [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modulu převaděč dat pomocí nejnovější balíčky Azure IoT Edge NPM.</span><span class="sxs-lookup"><span data-stu-id="a61fe-105">In this tutorial, we walk through environment setup and how to write a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using the latest Azure IoT Edge NPM packages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a61fe-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a61fe-106">Prerequisites</span></span>

<span data-ttu-id="a61fe-107">V této části nastavíte prostředí pro vývoj modulu IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="a61fe-107">In this section, you set up your environment for IoT Edge module development.</span></span> <span data-ttu-id="a61fe-108">Platí pro obě *64bitová verze Windows* a *Linux 64-bit (Ubuntu 14 +)* operační systémy.</span><span class="sxs-lookup"><span data-stu-id="a61fe-108">It applies to both *64-bit Windows* and *64-bit Linux (Ubuntu 14+)* operating systems.</span></span>

<span data-ttu-id="a61fe-109">Je vyžadován následující software:</span><span class="sxs-lookup"><span data-stu-id="a61fe-109">The following software is required:</span></span>
* <span data-ttu-id="a61fe-110">[Git klienta](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="a61fe-110">[Git Client](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="a61fe-111">[Uzel LTS](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="a61fe-111">[Node LTS](https://nodejs.org).</span></span>
* <span data-ttu-id="a61fe-112">`npm install -g yo`.</span><span class="sxs-lookup"><span data-stu-id="a61fe-112">`npm install -g yo`.</span></span>
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a><span data-ttu-id="a61fe-113">Architektura</span><span class="sxs-lookup"><span data-stu-id="a61fe-113">Architecture</span></span>

<span data-ttu-id="a61fe-114">Platformy Azure IoT Edge výraznou přijme [Von Neumannova architektura](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span><span class="sxs-lookup"><span data-stu-id="a61fe-114">The Azure IoT Edge platform heavily adopts the [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span></span> <span data-ttu-id="a61fe-115">Což znamená, že celou architekturu Azure IoT okraj je systém, který zpracovává vstup a výstup; a každý jednotlivých modul je také velmi malé subsystému vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="a61fe-115">Which means that the entire Azure IoT Edge architecture is a system that processes input and produces output; and that each individual module is also a tiny input-output subsystem.</span></span> <span data-ttu-id="a61fe-116">V tomto kurzu zavedeme následující dva moduly:</span><span class="sxs-lookup"><span data-stu-id="a61fe-116">In this tutorial, we introduce the following two modules:</span></span>

1. <span data-ttu-id="a61fe-117">Modul, který obdrží simulované [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signál a převede jej na formátovaný [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.</span><span class="sxs-lookup"><span data-stu-id="a61fe-117">A module that receives a simulated [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signal and converts it into a formatted [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>
2. <span data-ttu-id="a61fe-118">Modul, který vytiskne přijatého [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.</span><span class="sxs-lookup"><span data-stu-id="a61fe-118">A module that prints the received [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>

<span data-ttu-id="a61fe-119">Následující obrázek zobrazuje typické toku koncová dat pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="a61fe-119">The following image displays the typical end to end dataflow for this project:</span></span>

<span data-ttu-id="a61fe-120">![Tok dat mezi tři moduly](media/iot-hub-iot-edge-create-module/dataflow.png "vstup: Simulated zakázat modulu; Procesor: Převaděč modulu; Výstup: Modul tiskárny")</span><span class="sxs-lookup"><span data-stu-id="a61fe-120">![Dataflow between three modules](media/iot-hub-iot-edge-create-module/dataflow.png "Input: Simulated BLE Module; Processor: Converter Module; Output: Printer Module")</span></span>

## <a name="set-up-the-environment"></a><span data-ttu-id="a61fe-121">Nastavení prostředí</span><span class="sxs-lookup"><span data-stu-id="a61fe-121">Set up the environment</span></span>
<span data-ttu-id="a61fe-122">Níže budeme ukazují, jak rychle nastavit prostředí pro začít psát první modul zakázat převaděč JS.</span><span class="sxs-lookup"><span data-stu-id="a61fe-122">Below we show you how to quickly set up environment to start to write your first BLE converter module with JS.</span></span>

### <a name="create-module-project"></a><span data-ttu-id="a61fe-123">Vytvoření projektu modulu</span><span class="sxs-lookup"><span data-stu-id="a61fe-123">Create module project</span></span>
1. <span data-ttu-id="a61fe-124">Otevřete okno příkazového řádku, spusťte `yo az-iot-gw-module`.</span><span class="sxs-lookup"><span data-stu-id="a61fe-124">Open a command-line window, run `yo az-iot-gw-module`.</span></span>
2. <span data-ttu-id="a61fe-125">Podle pokynů na obrazovce na dokončení inicializace modulu projektu.</span><span class="sxs-lookup"><span data-stu-id="a61fe-125">Follow the steps on the screen to finish the initialization of your module project.</span></span>

### <a name="project-structure"></a><span data-ttu-id="a61fe-126">Struktura projektu</span><span class="sxs-lookup"><span data-stu-id="a61fe-126">Project structure</span></span>
<span data-ttu-id="a61fe-127">Modul projektu JS se skládá z následujících součástí:</span><span class="sxs-lookup"><span data-stu-id="a61fe-127">A JS module project consists of the following components:</span></span>

<span data-ttu-id="a61fe-128">`modules`-Přizpůsobené JS modulu zdrojové soubory.</span><span class="sxs-lookup"><span data-stu-id="a61fe-128">`modules` - The customized JS module source files.</span></span> <span data-ttu-id="a61fe-129">Nahraďte výchozí `sensor.js` a `printer.js` s vlastní soubory modulu.</span><span class="sxs-lookup"><span data-stu-id="a61fe-129">Replace the default `sensor.js` and `printer.js` with your own module files.</span></span>

<span data-ttu-id="a61fe-130">`app.js`-Soubor položku Spustit instance okraj.</span><span class="sxs-lookup"><span data-stu-id="a61fe-130">`app.js` - The entry file to start the Edge instance.</span></span>

<span data-ttu-id="a61fe-131">`gw.config.json`Chcete-li přizpůsobit moduly, které mají být načteny Edge – konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="a61fe-131">`gw.config.json` - The configuration file to customize the modules to be loaded by Edge.</span></span>

<span data-ttu-id="a61fe-132">`package.json`-Informace metadat pro modul projekt.</span><span class="sxs-lookup"><span data-stu-id="a61fe-132">`package.json` - The metadata information for module project.</span></span>

<span data-ttu-id="a61fe-133">`README.md`-Základní dokumentace pro modul projekt.</span><span class="sxs-lookup"><span data-stu-id="a61fe-133">`README.md` - The basic documentation for module project.</span></span>


### <a name="package-file"></a><span data-ttu-id="a61fe-134">Soubor balíčku</span><span class="sxs-lookup"><span data-stu-id="a61fe-134">Package file</span></span>

<span data-ttu-id="a61fe-135">To `package.json` deklaruje všechny informace potřebné pro modul projekt, který obsahuje název, verzi, položky, skripty, modulu runtime a vývoj závislosti metadat.</span><span class="sxs-lookup"><span data-stu-id="a61fe-135">This `package.json` declares all the metadata information needed by a module project that includes the name, version, entry, scripts, runtime, and development dependencies.</span></span>

<span data-ttu-id="a61fe-136">Následující fragment kódu ukazuje, jak nakonfigurovat pro zakázat převaděč ukázkový projekt.</span><span class="sxs-lookup"><span data-stu-id="a61fe-136">Following code snippet shows how to configure for BLE converter sample project.</span></span>
```json
{
  "name": "converter",
  "version": "1.0.0",
  "description": "BLE data converter sample for Azure IoT Edge.",
  "repository": {
    "type": "git",
    "url": "https://github.com/Azure-Samples/iot-edge-samples"
  },
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "author": "Microsoft Corporation",
  "license": "MIT",
  "dependencies": {
  },
  "devDependencies": {
    "azure-iot-gateway": "~1.1.3"
  }
}
```


### <a name="entry-file"></a><span data-ttu-id="a61fe-137">Vstupní soubor</span><span class="sxs-lookup"><span data-stu-id="a61fe-137">Entry file</span></span>
<span data-ttu-id="a61fe-138">`app.js` Definuje způsob k inicializaci instance okraj.</span><span class="sxs-lookup"><span data-stu-id="a61fe-138">The `app.js` defines the way to initialize the edge instance.</span></span> <span data-ttu-id="a61fe-139">Zde jsme není nutné provést změnu.</span><span class="sxs-lookup"><span data-stu-id="a61fe-139">Here we don't need to make any change.</span></span>

```javascript
(function() {
  'use strict';

  const Gateway = require('azure-iot-gateway');
  let config_path = './gw.config.json';

  // node app.js
  if (process.argv.length < 2) {
    throw 'Calling pattern should be node app.js.';
  }

  const gw = new Gateway(config_path);
  gw.run();
})();
```

### <a name="interface-of-module"></a><span data-ttu-id="a61fe-140">Rozhraní modulu</span><span class="sxs-lookup"><span data-stu-id="a61fe-140">Interface of module</span></span>
<span data-ttu-id="a61fe-141">Modul služby Azure IoT Edge lze považovat procesor dat, jejichž povinnostem je: přijímání vstupu, zpracovat a vytvoření výstupu.</span><span class="sxs-lookup"><span data-stu-id="a61fe-141">You can treat an Azure IoT Edge module as a data processor whose job is to: receive input, process it, and produce output.</span></span>

<span data-ttu-id="a61fe-142">Vstup může být data z hardware (např. detektor pohybu), zprávu z ostatních modulů, nebo cokoliv jiného (např. náhodné číslo pravidelně generované časovač).</span><span class="sxs-lookup"><span data-stu-id="a61fe-142">The input might be data from hardware (like a motion detector), a message from other modules, or anything else (like a random number generated periodically by a timer).</span></span>

<span data-ttu-id="a61fe-143">Výstup bude vypadat na vstup, se může spustit chování hardware (např. blikající LED), zprávu, která z ostatních modulů, nebo cokoliv jiného (např. tisk do konzoly).</span><span class="sxs-lookup"><span data-stu-id="a61fe-143">The output is similar to the input, it could trigger hardware behavior (like the blinking LED), a message to other modules, or anything else (like printing to the console).</span></span>

<span data-ttu-id="a61fe-144">Moduly navzájem komunikují pomocí `message` objektu.</span><span class="sxs-lookup"><span data-stu-id="a61fe-144">Modules communicate with each other using `message` object.</span></span> <span data-ttu-id="a61fe-145">**Obsah** z `message` je bajtové pole, které umožňuje reprezentovat jakýkoli druh dat se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="a61fe-145">The **content** of a `message` is a byte array that is capable of representing any kind of data you like.</span></span> <span data-ttu-id="a61fe-146">**Vlastnosti** jsou také k dispozici v `message` a jsou jednoduše mapování řetězec řetězec.</span><span class="sxs-lookup"><span data-stu-id="a61fe-146">**Properties** are also available in the `message` and are simply a string-to-string mapping.</span></span> <span data-ttu-id="a61fe-147">Může si můžete představit **vlastnosti** jako hlavičky v požadavku HTTP nebo metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="a61fe-147">You may think of **properties** as the headers in an HTTP request, or the metadata of a file.</span></span>

<span data-ttu-id="a61fe-148">Chcete-li vytvořit modul Azure IoT Edge v JS, budete muset vytvořit nový objekt modul, který implementuje požadované metody `receive()`.</span><span class="sxs-lookup"><span data-stu-id="a61fe-148">In order to develop an Azure IoT Edge module in JS, you need to create a new module object that implements the required methods `receive()`.</span></span> <span data-ttu-id="a61fe-149">V tomto okamžiku můžete také implementovat volitelné `create()` nebo `start()`, nebo `destroy()` také metody.</span><span class="sxs-lookup"><span data-stu-id="a61fe-149">At this point, you may also choose to implement the optional `create()` or `start()`, or `destroy()` methods as well.</span></span> <span data-ttu-id="a61fe-150">Následující fragment kódu ukazuje generování uživatelského rozhraní modulu objektu JS.</span><span class="sxs-lookup"><span data-stu-id="a61fe-150">The following code snippet shows you the scaffolding of JS module object.</span></span>

```javascript
'use strict';

module.exports = {
  broker: null,
  configuration: null,

  create: function (broker, configuration) {
    // Default implementation.
    this.broker = broker;
    this.configuration = configuration;

    return true;
  },

  start: function () {
    // Produce
  },

  receive: function (message) {
    // Consume
  },

  destroy: function () {
  }
};
```

### <a name="converter-module"></a><span data-ttu-id="a61fe-151">Převaděč modulu</span><span class="sxs-lookup"><span data-stu-id="a61fe-151">Converter module</span></span>
| <span data-ttu-id="a61fe-152">Vstup</span><span class="sxs-lookup"><span data-stu-id="a61fe-152">Input</span></span>                    | <span data-ttu-id="a61fe-153">Procesor</span><span class="sxs-lookup"><span data-stu-id="a61fe-153">Processor</span></span>                              | <span data-ttu-id="a61fe-154">Výstup</span><span class="sxs-lookup"><span data-stu-id="a61fe-154">Output</span></span>                 | <span data-ttu-id="a61fe-155">Zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="a61fe-155">Source File</span></span>            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| <span data-ttu-id="a61fe-156">Teplotní datových zpráv</span><span class="sxs-lookup"><span data-stu-id="a61fe-156">Temperature data message</span></span> | <span data-ttu-id="a61fe-157">Analýza a vytvořit novou zprávu JSON</span><span class="sxs-lookup"><span data-stu-id="a61fe-157">Parse and construct a new JSON message</span></span> | <span data-ttu-id="a61fe-158">Struktura JSON zpráv</span><span class="sxs-lookup"><span data-stu-id="a61fe-158">Structure JSON message</span></span> | `converter.js` |

<span data-ttu-id="a61fe-159">Tento modul je typické modul Azure IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="a61fe-159">This module is a typical Azure IoT Edge module.</span></span> <span data-ttu-id="a61fe-160">Přijímá zprávy teploty z ostatních modulů (modul hardwarového nebo v tomto případě naše simulované modulu lit); a pak normalizuje teploty zprávu ve zprávě strukturovaných JSON (včetně připojování ID zprávy, nastavení vlastnosti jestli musíme teploty výstrahu aktivovat a tak dále).</span><span class="sxs-lookup"><span data-stu-id="a61fe-160">It accepts temperature messages from other modules (a hardware module, or in this case our simulated BLE module); and then normalizes the temperature message in to a structured JSON message (including appending the message ID, setting the property of whether we need to trigger the temperature alert, and so on).</span></span>

```javascript
receive: function (message) {
  // Initialize the messageCount in global object at first time.
  if (!global.messageCount) {
    global.messageCount = 0;
  }

  // Read the content and properties objects from message.
  let rawContent = JSON.parse(Buffer.from(message.content).toString('utf8'));
  let rawProperties = message.properties;

  // Generate new properties object.
  let newProperties = {
    source: rawProperties.source,
    macAddress: rawProperties.macAddress,
    temperatureAlert: rawContent.temperature > 30 ? 'true' : 'false'
  };

  // Generate new content object.
  let newContent = {
    deviceId: 'Intel NUC Gateway',
    messageId: ++global.messageCount,
    temperature: rawContent.temperature
  };

  // Publish the new message to broker.
  this.broker.publish(
    {
      properties: newProperties,
      content: new Uint8Array(Buffer.from(JSON.stringify(newContent), 'utf8'))
    }
  );
},
```

### <a name="printer-module"></a><span data-ttu-id="a61fe-161">Modul tiskárny</span><span class="sxs-lookup"><span data-stu-id="a61fe-161">Printer module</span></span>
| <span data-ttu-id="a61fe-162">Vstup</span><span class="sxs-lookup"><span data-stu-id="a61fe-162">Input</span></span>                          | <span data-ttu-id="a61fe-163">Procesor</span><span class="sxs-lookup"><span data-stu-id="a61fe-163">Processor</span></span> | <span data-ttu-id="a61fe-164">Výstup</span><span class="sxs-lookup"><span data-stu-id="a61fe-164">Output</span></span>                     | <span data-ttu-id="a61fe-165">Zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="a61fe-165">Source File</span></span>          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| <span data-ttu-id="a61fe-166">Jakékoli zprávy z ostatních modulů</span><span class="sxs-lookup"><span data-stu-id="a61fe-166">Any message from other modules</span></span> | <span data-ttu-id="a61fe-167">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="a61fe-167">N/A</span></span>       | <span data-ttu-id="a61fe-168">Přihlaste se do konzoly zprávy</span><span class="sxs-lookup"><span data-stu-id="a61fe-168">Log the message to console</span></span> | `printer.js` |

<span data-ttu-id="a61fe-169">Tento modul je jednoduché, není potřeba vysvětlovat, který výstupy přijatých zpráv (vlastnost, obsah) do okna terminálu.</span><span class="sxs-lookup"><span data-stu-id="a61fe-169">This module is simple, self-explanatory, which outputs the received messages(property, content) to the terminal window.</span></span>

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a><span data-ttu-id="a61fe-170">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a61fe-170">Configuration</span></span>
<span data-ttu-id="a61fe-171">V posledním kroku před spuštěním moduly je ke konfiguraci Azure IoT okraj a k navázání připojení mezi moduly.</span><span class="sxs-lookup"><span data-stu-id="a61fe-171">The final step before running the modules is to configure the Azure IoT Edge and to establish the connections between modules.</span></span>

<span data-ttu-id="a61fe-172">Nejprve je potřeba deklarovat naše `node` zavaděče (od Azure IoT Edge podporuje zavaděče různých jazyků), který může být odkazuje jeho `name` v částech i později.</span><span class="sxs-lookup"><span data-stu-id="a61fe-172">First we need to declare our `node` loader (since Azure IoT Edge supports loaders of different languages) which could be referenced by its `name` in the sections afterward.</span></span>

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

<span data-ttu-id="a61fe-173">Jakmile jsme prohlásí, že je naše zavaděče, potřebujeme také deklarovat také naše moduly.</span><span class="sxs-lookup"><span data-stu-id="a61fe-173">Once we have declared our loaders, we also need to declare our modules as well.</span></span> <span data-ttu-id="a61fe-174">Podobně jako deklarace zavaděče, že lze také odkazovat podle jejich `name` atribut.</span><span class="sxs-lookup"><span data-stu-id="a61fe-174">Similar to declaring the loaders, they can also be referenced by their `name` attribute.</span></span> <span data-ttu-id="a61fe-175">Když modul deklarace, musíme určit zavaděč, měla by používat (který by měl být jeden jsme definovali před) a vstupní bod (musí být normalizovaný název třídy Naše modulu) pro každý modul.</span><span class="sxs-lookup"><span data-stu-id="a61fe-175">When declaring a module, we need to specify the loader it should use (which should be the one we defined before) and the entry-point (should be the normalized class name of our module) for each module.</span></span> <span data-ttu-id="a61fe-176">`simulated_device` Modulu je nativní modul, který je součástí balíčku Azure IoT Edge core runtime.</span><span class="sxs-lookup"><span data-stu-id="a61fe-176">The `simulated_device` module is a native module that is included in the Azure IoT Edge core runtime package.</span></span> <span data-ttu-id="a61fe-177">Zahrnout `args` v kódu JSON souboru i v případě, že je `null`.</span><span class="sxs-lookup"><span data-stu-id="a61fe-177">Include `args` in the JSON file even if it is `null`.</span></span>

```json
"modules": [
  {
    "name": "simulated_device",
    "loader": {
      "name": "native",
      "entrypoint": {
        "module.path": "simulated_device"
      }
    },
    "args": {
      "macAddress": "01:02:03:03:02:01",
      "messagePeriod": 500
    }
  },
  {
    "name": "converter",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/converter.js"
      }
    },
    "args": null
  },
  {
    "name": "printer",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/printer.js"
      }
    },
    "args": null
  }
]
```

<span data-ttu-id="a61fe-178">Na konci konfigurace naváže připojení.</span><span class="sxs-lookup"><span data-stu-id="a61fe-178">At the end of the configuration, we establish the connections.</span></span> <span data-ttu-id="a61fe-179">Je každé připojení vyjádřená `source` a `sink`.</span><span class="sxs-lookup"><span data-stu-id="a61fe-179">Each connection is expressed by `source` and `sink`.</span></span> <span data-ttu-id="a61fe-180">Jejich by měl obě odkazovat modul předem definovaná.</span><span class="sxs-lookup"><span data-stu-id="a61fe-180">They should both reference a pre-defined module.</span></span> <span data-ttu-id="a61fe-181">Výstup zprávu `source` modulu se předají na vstup `sink` modulu.</span><span class="sxs-lookup"><span data-stu-id="a61fe-181">The output message of `source` module is forwarded to the input of `sink` module.</span></span>

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "printer"
  }
]
```

## <a name="running-the-modules"></a><span data-ttu-id="a61fe-182">Spuštění moduly</span><span class="sxs-lookup"><span data-stu-id="a61fe-182">Running the modules</span></span>
1. `npm install`
2. `npm start`

<span data-ttu-id="a61fe-183">Pokud chcete aplikaci ukončit, stiskněte klávesu `<Enter>` klíč.</span><span class="sxs-lookup"><span data-stu-id="a61fe-183">If you want to terminate the application, press `<Enter>` key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a61fe-184">Není doporučeno používat kombinace kláves Ctrl + C k ukončení aplikace IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="a61fe-184">It is not recommended to use Ctrl + C to terminate the IoT Edge application.</span></span> <span data-ttu-id="a61fe-185">Jako tímto způsobem může způsobit neobvyklým způsobem ukončení procesu.</span><span class="sxs-lookup"><span data-stu-id="a61fe-185">As this way may cause the process to terminate abnormally.</span></span>
