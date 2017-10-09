---
title: aaaCreate modul Azure IoT Edge s Node.js | Microsoft Docs
description: "V tomto kurzu umožňující prezentovat jak toowrite pomocí modulu zakázat data převaděč hello nejnovější Azure IoT Edge NPM balíčky a Yeoman generátor."
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
ms.openlocfilehash: d3e696b5a310377ffb8e99998ff0714bf7c0bb41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a><span data-ttu-id="39fb5-103">Vytvořte modul Azure IoT Edge s Node.js</span><span class="sxs-lookup"><span data-stu-id="39fb5-103">Create an Azure IoT Edge Module with Node.js</span></span>

<span data-ttu-id="39fb5-104">V tomto kurzu umožňující prezentovat jak toocreate modul pro Azure IoT hrany v JS.</span><span class="sxs-lookup"><span data-stu-id="39fb5-104">This tutorial showcases how toocreate a module for Azure IoT Edge in JS.</span></span>

<span data-ttu-id="39fb5-105">V tomto kurzu jsme provede procesem nastavení prostředí a jak toowrite [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modulu převaděč dat pomocí Azure IoT Edge NPM balíčky nejnovější hello.</span><span class="sxs-lookup"><span data-stu-id="39fb5-105">In this tutorial, we walk through environment setup and how toowrite a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using hello latest Azure IoT Edge NPM packages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="39fb5-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="39fb5-106">Prerequisites</span></span>

<span data-ttu-id="39fb5-107">V této části nastavíte prostředí pro vývoj modulu IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="39fb5-107">In this section, you set up your environment for IoT Edge module development.</span></span> <span data-ttu-id="39fb5-108">Platí tooboth *64bitová verze Windows* a *Linux 64-bit (Ubuntu 14 +)* operační systémy.</span><span class="sxs-lookup"><span data-stu-id="39fb5-108">It applies tooboth *64-bit Windows* and *64-bit Linux (Ubuntu 14+)* operating systems.</span></span>

<span data-ttu-id="39fb5-109">je potřeba Hello následující software:</span><span class="sxs-lookup"><span data-stu-id="39fb5-109">hello following software is required:</span></span>
* <span data-ttu-id="39fb5-110">[Git klienta](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="39fb5-110">[Git Client](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="39fb5-111">[Uzel LTS](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="39fb5-111">[Node LTS](https://nodejs.org).</span></span>
* <span data-ttu-id="39fb5-112">`npm install -g yo`.</span><span class="sxs-lookup"><span data-stu-id="39fb5-112">`npm install -g yo`.</span></span>
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a><span data-ttu-id="39fb5-113">Architektura</span><span class="sxs-lookup"><span data-stu-id="39fb5-113">Architecture</span></span>

<span data-ttu-id="39fb5-114">platformy Azure IoT Edge Hello výraznou přijme hello [Von Neumannova architektura](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span><span class="sxs-lookup"><span data-stu-id="39fb5-114">hello Azure IoT Edge platform heavily adopts hello [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span></span> <span data-ttu-id="39fb5-115">To znamená této architektuře hello celý Azure IoT okraj je systém, který zpracovává vstup a výstup; a každý jednotlivých modul je také velmi malé subsystému vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="39fb5-115">Which means that hello entire Azure IoT Edge architecture is a system that processes input and produces output; and that each individual module is also a tiny input-output subsystem.</span></span> <span data-ttu-id="39fb5-116">V tomto kurzu zavedeme hello následující dva moduly:</span><span class="sxs-lookup"><span data-stu-id="39fb5-116">In this tutorial, we introduce hello following two modules:</span></span>

1. <span data-ttu-id="39fb5-117">Modul, který obdrží simulované [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signál a převede jej na formátovaný [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.</span><span class="sxs-lookup"><span data-stu-id="39fb5-117">A module that receives a simulated [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signal and converts it into a formatted [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>
2. <span data-ttu-id="39fb5-118">Modul, který vytiskne hello přijata [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.</span><span class="sxs-lookup"><span data-stu-id="39fb5-118">A module that prints hello received [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>

<span data-ttu-id="39fb5-119">Hello následující obrázek zobrazuje hello typické end tooend toku dat pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="39fb5-119">hello following image displays hello typical end tooend dataflow for this project:</span></span>

<span data-ttu-id="39fb5-120">![Tok dat mezi tři moduly](media/iot-hub-iot-edge-create-module/dataflow.png "vstup: Simulated zakázat modulu; Procesor: Převaděč modulu; Výstup: Modul tiskárny")</span><span class="sxs-lookup"><span data-stu-id="39fb5-120">![Dataflow between three modules](media/iot-hub-iot-edge-create-module/dataflow.png "Input: Simulated BLE Module; Processor: Converter Module; Output: Printer Module")</span></span>

## <a name="set-up-hello-environment"></a><span data-ttu-id="39fb5-121">Nastavení prostředí hello</span><span class="sxs-lookup"><span data-stu-id="39fb5-121">Set up hello environment</span></span>
<span data-ttu-id="39fb5-122">Zde jsme vám ukážou, jak tooquickly nastavit prostředí toostart toowrite první modul zakázat převaděč JS.</span><span class="sxs-lookup"><span data-stu-id="39fb5-122">Below we show you how tooquickly set up environment toostart toowrite your first BLE converter module with JS.</span></span>

### <a name="create-module-project"></a><span data-ttu-id="39fb5-123">Vytvoření projektu modulu</span><span class="sxs-lookup"><span data-stu-id="39fb5-123">Create module project</span></span>
1. <span data-ttu-id="39fb5-124">Otevřete okno příkazového řádku, spusťte `yo az-iot-gw-module`.</span><span class="sxs-lookup"><span data-stu-id="39fb5-124">Open a command-line window, run `yo az-iot-gw-module`.</span></span>
2. <span data-ttu-id="39fb5-125">Postupujte podle kroků hello na hello obrazovky toofinish hello inicializace modulu projektu.</span><span class="sxs-lookup"><span data-stu-id="39fb5-125">Follow hello steps on hello screen toofinish hello initialization of your module project.</span></span>

### <a name="project-structure"></a><span data-ttu-id="39fb5-126">Struktura projektu</span><span class="sxs-lookup"><span data-stu-id="39fb5-126">Project structure</span></span>
<span data-ttu-id="39fb5-127">Modul projektu JS se skládá z hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="39fb5-127">A JS module project consists of hello following components:</span></span>

<span data-ttu-id="39fb5-128">`modules`-hello přizpůsobit JS modulu zdrojové soubory.</span><span class="sxs-lookup"><span data-stu-id="39fb5-128">`modules` - hello customized JS module source files.</span></span> <span data-ttu-id="39fb5-129">Nahraďte hello výchozí `sensor.js` a `printer.js` s vlastní soubory modulu.</span><span class="sxs-lookup"><span data-stu-id="39fb5-129">Replace hello default `sensor.js` and `printer.js` with your own module files.</span></span>

<span data-ttu-id="39fb5-130">`app.js`-hello vstupní soubor toostart hello Edge instance.</span><span class="sxs-lookup"><span data-stu-id="39fb5-130">`app.js` - hello entry file toostart hello Edge instance.</span></span>

<span data-ttu-id="39fb5-131">`gw.config.json`-hello konfigurační soubor toocustomize hello moduly toobe načteny okraj.</span><span class="sxs-lookup"><span data-stu-id="39fb5-131">`gw.config.json` - hello configuration file toocustomize hello modules toobe loaded by Edge.</span></span>

<span data-ttu-id="39fb5-132">`package.json`-hello informace metadat pro modul projektu.</span><span class="sxs-lookup"><span data-stu-id="39fb5-132">`package.json` - hello metadata information for module project.</span></span>

<span data-ttu-id="39fb5-133">`README.md`-hello základní dokumentace pro modul projekt.</span><span class="sxs-lookup"><span data-stu-id="39fb5-133">`README.md` - hello basic documentation for module project.</span></span>


### <a name="package-file"></a><span data-ttu-id="39fb5-134">Soubor balíčku</span><span class="sxs-lookup"><span data-stu-id="39fb5-134">Package file</span></span>

<span data-ttu-id="39fb5-135">To `package.json` deklaruje všechny hello metadata informace potřebné pro modul projekt, který obsahuje název, verzi, položky, skripty, modulu runtime a vývoj závislosti hello.</span><span class="sxs-lookup"><span data-stu-id="39fb5-135">This `package.json` declares all hello metadata information needed by a module project that includes hello name, version, entry, scripts, runtime, and development dependencies.</span></span>

<span data-ttu-id="39fb5-136">Následující fragment kódu ukazuje kód jak tooconfigure pro zakázat převaděč ukázkový projektu.</span><span class="sxs-lookup"><span data-stu-id="39fb5-136">Following code snippet shows how tooconfigure for BLE converter sample project.</span></span>
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


### <a name="entry-file"></a><span data-ttu-id="39fb5-137">Vstupní soubor</span><span class="sxs-lookup"><span data-stu-id="39fb5-137">Entry file</span></span>
<span data-ttu-id="39fb5-138">Hello `app.js` definuje hello způsob tooinitialize hello edge instance.</span><span class="sxs-lookup"><span data-stu-id="39fb5-138">hello `app.js` defines hello way tooinitialize hello edge instance.</span></span> <span data-ttu-id="39fb5-139">Zde jsme nepotřebují toomake všechny změny.</span><span class="sxs-lookup"><span data-stu-id="39fb5-139">Here we don't need toomake any change.</span></span>

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

### <a name="interface-of-module"></a><span data-ttu-id="39fb5-140">Rozhraní modulu</span><span class="sxs-lookup"><span data-stu-id="39fb5-140">Interface of module</span></span>
<span data-ttu-id="39fb5-141">Modul služby Azure IoT Edge lze považovat procesor dat, jejichž povinnostem je: přijímání vstupu, zpracovat a vytvoření výstupu.</span><span class="sxs-lookup"><span data-stu-id="39fb5-141">You can treat an Azure IoT Edge module as a data processor whose job is to: receive input, process it, and produce output.</span></span>

<span data-ttu-id="39fb5-142">vstup Hello může být data z hardware (např. detektor pohybu), zprávu z ostatních modulů, nebo cokoliv jiného (např. náhodné číslo pravidelně generované časovač).</span><span class="sxs-lookup"><span data-stu-id="39fb5-142">hello input might be data from hardware (like a motion detector), a message from other modules, or anything else (like a random number generated periodically by a timer).</span></span>

<span data-ttu-id="39fb5-143">výstup Hello je podobné toohello vstup, se může spustit chování hardware (např. hello blikat DIODU), zpráva tooother moduly nebo cokoliv jiného (např. tisk toohello konzoly).</span><span class="sxs-lookup"><span data-stu-id="39fb5-143">hello output is similar toohello input, it could trigger hardware behavior (like hello blinking LED), a message tooother modules, or anything else (like printing toohello console).</span></span>

<span data-ttu-id="39fb5-144">Moduly navzájem komunikují pomocí `message` objektu.</span><span class="sxs-lookup"><span data-stu-id="39fb5-144">Modules communicate with each other using `message` object.</span></span> <span data-ttu-id="39fb5-145">Hello **obsah** z `message` je bajtové pole, které umožňuje reprezentovat jakýkoli druh dat se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="39fb5-145">hello **content** of a `message` is a byte array that is capable of representing any kind of data you like.</span></span> <span data-ttu-id="39fb5-146">**Vlastnosti** jsou také k dispozici v hello `message` a jsou jednoduše mapování řetězec řetězec.</span><span class="sxs-lookup"><span data-stu-id="39fb5-146">**Properties** are also available in hello `message` and are simply a string-to-string mapping.</span></span> <span data-ttu-id="39fb5-147">Může si můžete představit **vlastnosti** jako hello hlavičky v požadavku HTTP, nebo hello metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="39fb5-147">You may think of **properties** as hello headers in an HTTP request, or hello metadata of a file.</span></span>

<span data-ttu-id="39fb5-148">Pořadí toodevelop modul Azure IoT Edge v JS, je nutné toocreate nového objektu modulu, který implementuje metody hello požadované `receive()`.</span><span class="sxs-lookup"><span data-stu-id="39fb5-148">In order toodevelop an Azure IoT Edge module in JS, you need toocreate a new module object that implements hello required methods `receive()`.</span></span> <span data-ttu-id="39fb5-149">V tomto okamžiku můžete také tooimplement hello volitelné `create()` nebo `start()`, nebo `destroy()` také metody.</span><span class="sxs-lookup"><span data-stu-id="39fb5-149">At this point, you may also choose tooimplement hello optional `create()` or `start()`, or `destroy()` methods as well.</span></span> <span data-ttu-id="39fb5-150">Hello následující fragment kódu ukazuje, že hello generování uživatelského rozhraní modulu objektu JS.</span><span class="sxs-lookup"><span data-stu-id="39fb5-150">hello following code snippet shows you hello scaffolding of JS module object.</span></span>

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

### <a name="converter-module"></a><span data-ttu-id="39fb5-151">Převaděč modulu</span><span class="sxs-lookup"><span data-stu-id="39fb5-151">Converter module</span></span>
| <span data-ttu-id="39fb5-152">Vstup</span><span class="sxs-lookup"><span data-stu-id="39fb5-152">Input</span></span>                    | <span data-ttu-id="39fb5-153">Procesor</span><span class="sxs-lookup"><span data-stu-id="39fb5-153">Processor</span></span>                              | <span data-ttu-id="39fb5-154">Výstup</span><span class="sxs-lookup"><span data-stu-id="39fb5-154">Output</span></span>                 | <span data-ttu-id="39fb5-155">Zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="39fb5-155">Source File</span></span>            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| <span data-ttu-id="39fb5-156">Teplotní datových zpráv</span><span class="sxs-lookup"><span data-stu-id="39fb5-156">Temperature data message</span></span> | <span data-ttu-id="39fb5-157">Analýza a vytvořit novou zprávu JSON</span><span class="sxs-lookup"><span data-stu-id="39fb5-157">Parse and construct a new JSON message</span></span> | <span data-ttu-id="39fb5-158">Struktura JSON zpráv</span><span class="sxs-lookup"><span data-stu-id="39fb5-158">Structure JSON message</span></span> | `converter.js` |

<span data-ttu-id="39fb5-159">Tento modul je typické modul Azure IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="39fb5-159">This module is a typical Azure IoT Edge module.</span></span> <span data-ttu-id="39fb5-160">Přijímá zprávy teploty z ostatních modulů (modul hardwarového nebo v tomto případě naše simulované modulu lit); a pak normalizuje uvítací zprávu teploty tooa strukturovaná JSON zprávy (včetně připojování hello ID zprávy, nastavení hello vlastnost jestli potřebujeme tootrigger hello teploty výstrahy a tak dále).</span><span class="sxs-lookup"><span data-stu-id="39fb5-160">It accepts temperature messages from other modules (a hardware module, or in this case our simulated BLE module); and then normalizes hello temperature message in tooa structured JSON message (including appending hello message ID, setting hello property of whether we need tootrigger hello temperature alert, and so on).</span></span>

```javascript
receive: function (message) {
  // Initialize hello messageCount in global object at first time.
  if (!global.messageCount) {
    global.messageCount = 0;
  }

  // Read hello content and properties objects from message.
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

  // Publish hello new message toobroker.
  this.broker.publish(
    {
      properties: newProperties,
      content: new Uint8Array(Buffer.from(JSON.stringify(newContent), 'utf8'))
    }
  );
},
```

### <a name="printer-module"></a><span data-ttu-id="39fb5-161">Modul tiskárny</span><span class="sxs-lookup"><span data-stu-id="39fb5-161">Printer module</span></span>
| <span data-ttu-id="39fb5-162">Vstup</span><span class="sxs-lookup"><span data-stu-id="39fb5-162">Input</span></span>                          | <span data-ttu-id="39fb5-163">Procesor</span><span class="sxs-lookup"><span data-stu-id="39fb5-163">Processor</span></span> | <span data-ttu-id="39fb5-164">Výstup</span><span class="sxs-lookup"><span data-stu-id="39fb5-164">Output</span></span>                     | <span data-ttu-id="39fb5-165">Zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="39fb5-165">Source File</span></span>          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| <span data-ttu-id="39fb5-166">Jakékoli zprávy z ostatních modulů</span><span class="sxs-lookup"><span data-stu-id="39fb5-166">Any message from other modules</span></span> | <span data-ttu-id="39fb5-167">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="39fb5-167">N/A</span></span>       | <span data-ttu-id="39fb5-168">Protokolování zpráv tooconsole hello</span><span class="sxs-lookup"><span data-stu-id="39fb5-168">Log hello message tooconsole</span></span> | `printer.js` |

<span data-ttu-id="39fb5-169">Tento modul je jednoduché, není potřeba vysvětlovat, který výstupy hello přijaté zprávy (vlastnost, obsah) toohello okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="39fb5-169">This module is simple, self-explanatory, which outputs hello received messages(property, content) toohello terminal window.</span></span>

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a><span data-ttu-id="39fb5-170">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="39fb5-170">Configuration</span></span>
<span data-ttu-id="39fb5-171">posledním krokem Hello před spuštěním moduly hello je tooconfigure hello Azure IoT okraj a tooestablish hello připojení mezi moduly.</span><span class="sxs-lookup"><span data-stu-id="39fb5-171">hello final step before running hello modules is tooconfigure hello Azure IoT Edge and tooestablish hello connections between modules.</span></span>

<span data-ttu-id="39fb5-172">Nejdřív potřebujeme toodeclare naše `node` zavaděče (od Azure IoT Edge podporuje zavaděče různých jazyků), který může být odkazuje jeho `name` v částech hello i později.</span><span class="sxs-lookup"><span data-stu-id="39fb5-172">First we need toodeclare our `node` loader (since Azure IoT Edge supports loaders of different languages) which could be referenced by its `name` in hello sections afterward.</span></span>

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

<span data-ttu-id="39fb5-173">Jakmile jsme prohlásí, že je naše zavaděče, také potřebujeme toodeclare také naše moduly.</span><span class="sxs-lookup"><span data-stu-id="39fb5-173">Once we have declared our loaders, we also need toodeclare our modules as well.</span></span> <span data-ttu-id="39fb5-174">Podobné zavaděče hello toodeclaring, že lze také odkazovat podle jejich `name` atribut.</span><span class="sxs-lookup"><span data-stu-id="39fb5-174">Similar toodeclaring hello loaders, they can also be referenced by their `name` attribute.</span></span> <span data-ttu-id="39fb5-175">Když modul deklarace, potřebujeme toospecify hello zavaděč měli použít (které by se měly hello jeden jsme definovali před) a hello vstupního bodu (by měl být hello normalizovaný název třídy Naše modulu) pro každý modul.</span><span class="sxs-lookup"><span data-stu-id="39fb5-175">When declaring a module, we need toospecify hello loader it should use (which should be hello one we defined before) and hello entry-point (should be hello normalized class name of our module) for each module.</span></span> <span data-ttu-id="39fb5-176">Hello `simulated_device` modulu je nativní modul, který je součástí hello Azure IoT Edge core runtime balíčku.</span><span class="sxs-lookup"><span data-stu-id="39fb5-176">hello `simulated_device` module is a native module that is included in hello Azure IoT Edge core runtime package.</span></span> <span data-ttu-id="39fb5-177">Zahrnout `args` v souboru JSON, i když je hello `null`.</span><span class="sxs-lookup"><span data-stu-id="39fb5-177">Include `args` in hello JSON file even if it is `null`.</span></span>

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

<span data-ttu-id="39fb5-178">Na konci hello hello konfigurace naváže připojení hello.</span><span class="sxs-lookup"><span data-stu-id="39fb5-178">At hello end of hello configuration, we establish hello connections.</span></span> <span data-ttu-id="39fb5-179">Je každé připojení vyjádřená `source` a `sink`.</span><span class="sxs-lookup"><span data-stu-id="39fb5-179">Each connection is expressed by `source` and `sink`.</span></span> <span data-ttu-id="39fb5-180">Jejich by měl obě odkazovat modul předem definovaná.</span><span class="sxs-lookup"><span data-stu-id="39fb5-180">They should both reference a pre-defined module.</span></span> <span data-ttu-id="39fb5-181">zprávu výstup Hello `source` modulu se předají toohello vstup z `sink` modulu.</span><span class="sxs-lookup"><span data-stu-id="39fb5-181">hello output message of `source` module is forwarded toohello input of `sink` module.</span></span>

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

## <a name="running-hello-modules"></a><span data-ttu-id="39fb5-182">Spuštěné moduly hello</span><span class="sxs-lookup"><span data-stu-id="39fb5-182">Running hello modules</span></span>
1. `npm install`
2. `npm start`

<span data-ttu-id="39fb5-183">Pokud chcete aplikace hello tooterminate, stiskněte klávesu `<Enter>` klíč.</span><span class="sxs-lookup"><span data-stu-id="39fb5-183">If you want tooterminate hello application, press `<Enter>` key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="39fb5-184">Není doporučeno toouse Ctrl + C tooterminate hello IoT Edge aplikace.</span><span class="sxs-lookup"><span data-stu-id="39fb5-184">It is not recommended toouse Ctrl + C tooterminate hello IoT Edge application.</span></span> <span data-ttu-id="39fb5-185">Jako tímto způsobem může způsobit hello proces tooterminate neobvyklým způsobem.</span><span class="sxs-lookup"><span data-stu-id="39fb5-185">As this way may cause hello process tooterminate abnormally.</span></span>
