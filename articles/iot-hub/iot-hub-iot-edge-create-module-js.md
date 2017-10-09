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
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a>Vytvořte modul Azure IoT Edge s Node.js

V tomto kurzu umožňující prezentovat jak toocreate modul pro Azure IoT hrany v JS.

V tomto kurzu jsme provede procesem nastavení prostředí a jak toowrite [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modulu převaděč dat pomocí Azure IoT Edge NPM balíčky nejnovější hello.

## <a name="prerequisites"></a>Požadavky

V této části nastavíte prostředí pro vývoj modulu IoT okraj. Platí tooboth *64bitová verze Windows* a *Linux 64-bit (Ubuntu 14 +)* operační systémy.

je potřeba Hello následující software:
* [Git klienta](https://git-scm.com/downloads).
* [Uzel LTS](https://nodejs.org).
* `npm install -g yo`.
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a>Architektura

platformy Azure IoT Edge Hello výraznou přijme hello [Von Neumannova architektura](https://en.wikipedia.org/wiki/Von_Neumann_architecture). To znamená této architektuře hello celý Azure IoT okraj je systém, který zpracovává vstup a výstup; a každý jednotlivých modul je také velmi malé subsystému vstup a výstup. V tomto kurzu zavedeme hello následující dva moduly:

1. Modul, který obdrží simulované [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signál a převede jej na formátovaný [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.
2. Modul, který vytiskne hello přijata [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.

Hello následující obrázek zobrazuje hello typické end tooend toku dat pro tento projekt.

![Tok dat mezi tři moduly](media/iot-hub-iot-edge-create-module/dataflow.png "vstup: Simulated zakázat modulu; Procesor: Převaděč modulu; Výstup: Modul tiskárny")

## <a name="set-up-hello-environment"></a>Nastavení prostředí hello
Zde jsme vám ukážou, jak tooquickly nastavit prostředí toostart toowrite první modul zakázat převaděč JS.

### <a name="create-module-project"></a>Vytvoření projektu modulu
1. Otevřete okno příkazového řádku, spusťte `yo az-iot-gw-module`.
2. Postupujte podle kroků hello na hello obrazovky toofinish hello inicializace modulu projektu.

### <a name="project-structure"></a>Struktura projektu
Modul projektu JS se skládá z hello následující součásti:

`modules`-hello přizpůsobit JS modulu zdrojové soubory. Nahraďte hello výchozí `sensor.js` a `printer.js` s vlastní soubory modulu.

`app.js`-hello vstupní soubor toostart hello Edge instance.

`gw.config.json`-hello konfigurační soubor toocustomize hello moduly toobe načteny okraj.

`package.json`-hello informace metadat pro modul projektu.

`README.md`-hello základní dokumentace pro modul projekt.


### <a name="package-file"></a>Soubor balíčku

To `package.json` deklaruje všechny hello metadata informace potřebné pro modul projekt, který obsahuje název, verzi, položky, skripty, modulu runtime a vývoj závislosti hello.

Následující fragment kódu ukazuje kód jak tooconfigure pro zakázat převaděč ukázkový projektu.
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


### <a name="entry-file"></a>Vstupní soubor
Hello `app.js` definuje hello způsob tooinitialize hello edge instance. Zde jsme nepotřebují toomake všechny změny.

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

### <a name="interface-of-module"></a>Rozhraní modulu
Modul služby Azure IoT Edge lze považovat procesor dat, jejichž povinnostem je: přijímání vstupu, zpracovat a vytvoření výstupu.

vstup Hello může být data z hardware (např. detektor pohybu), zprávu z ostatních modulů, nebo cokoliv jiného (např. náhodné číslo pravidelně generované časovač).

výstup Hello je podobné toohello vstup, se může spustit chování hardware (např. hello blikat DIODU), zpráva tooother moduly nebo cokoliv jiného (např. tisk toohello konzoly).

Moduly navzájem komunikují pomocí `message` objektu. Hello **obsah** z `message` je bajtové pole, které umožňuje reprezentovat jakýkoli druh dat se vám líbí. **Vlastnosti** jsou také k dispozici v hello `message` a jsou jednoduše mapování řetězec řetězec. Může si můžete představit **vlastnosti** jako hello hlavičky v požadavku HTTP, nebo hello metadata souboru.

Pořadí toodevelop modul Azure IoT Edge v JS, je nutné toocreate nového objektu modulu, který implementuje metody hello požadované `receive()`. V tomto okamžiku můžete také tooimplement hello volitelné `create()` nebo `start()`, nebo `destroy()` také metody. Hello následující fragment kódu ukazuje, že hello generování uživatelského rozhraní modulu objektu JS.

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

### <a name="converter-module"></a>Převaděč modulu
| Vstup                    | Procesor                              | Výstup                 | Zdrojový soubor            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Teplotní datových zpráv | Analýza a vytvořit novou zprávu JSON | Struktura JSON zpráv | `converter.js` |

Tento modul je typické modul Azure IoT okraj. Přijímá zprávy teploty z ostatních modulů (modul hardwarového nebo v tomto případě naše simulované modulu lit); a pak normalizuje uvítací zprávu teploty tooa strukturovaná JSON zprávy (včetně připojování hello ID zprávy, nastavení hello vlastnost jestli potřebujeme tootrigger hello teploty výstrahy a tak dále).

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

### <a name="printer-module"></a>Modul tiskárny
| Vstup                          | Procesor | Výstup                     | Zdrojový soubor          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Jakékoli zprávy z ostatních modulů | Není k dispozici       | Protokolování zpráv tooconsole hello | `printer.js` |

Tento modul je jednoduché, není potřeba vysvětlovat, který výstupy hello přijaté zprávy (vlastnost, obsah) toohello okno terminálu.

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a>Konfigurace
posledním krokem Hello před spuštěním moduly hello je tooconfigure hello Azure IoT okraj a tooestablish hello připojení mezi moduly.

Nejdřív potřebujeme toodeclare naše `node` zavaděče (od Azure IoT Edge podporuje zavaděče různých jazyků), který může být odkazuje jeho `name` v částech hello i později.

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

Jakmile jsme prohlásí, že je naše zavaděče, také potřebujeme toodeclare také naše moduly. Podobné zavaděče hello toodeclaring, že lze také odkazovat podle jejich `name` atribut. Když modul deklarace, potřebujeme toospecify hello zavaděč měli použít (které by se měly hello jeden jsme definovali před) a hello vstupního bodu (by měl být hello normalizovaný název třídy Naše modulu) pro každý modul. Hello `simulated_device` modulu je nativní modul, který je součástí hello Azure IoT Edge core runtime balíčku. Zahrnout `args` v souboru JSON, i když je hello `null`.

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

Na konci hello hello konfigurace naváže připojení hello. Je každé připojení vyjádřená `source` a `sink`. Jejich by měl obě odkazovat modul předem definovaná. zprávu výstup Hello `source` modulu se předají toohello vstup z `sink` modulu.

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

## <a name="running-hello-modules"></a>Spuštěné moduly hello
1. `npm install`
2. `npm start`

Pokud chcete aplikace hello tooterminate, stiskněte klávesu `<Enter>` klíč.

> [!IMPORTANT]
> Není doporučeno toouse Ctrl + C tooterminate hello IoT Edge aplikace. Jako tímto způsobem může způsobit hello proces tooterminate neobvyklým způsobem.
