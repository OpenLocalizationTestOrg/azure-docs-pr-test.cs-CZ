---
title: aaaCreate modul Azure IoT Edge s Javou | Microsoft Docs
description: "V tomto kurzu hodnotí, jak toowrite pomocí modulu zakázat data převaděč hello nejnovější balíčky Azure IoT Edge Maven."
services: iot-hub
author: junyi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 06/28/2017
ms.author: junyi
ms.openlocfilehash: abb560933d13d133ae9a1da08b503d5735b230e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-iot-edge-module-with-java"></a>Vytvořit modul Azure IoT Edge s Javou

V tomto kurzu hodnotí, jak jeden může vytvořit modul pro Azure IoT hrany v jazyce Java.

V tomto kurzu jsme provede procesem nastavení prostředí a jak toowrite [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modulu převaděč dat pomocí Azure IoT Edge Maven balíčky nejnovější hello.

## <a name="prerequisites"></a>Požadavky

V této části bude nastavení prostředí pro vývoj modulu IoT okraj. Platí tooboth *64bitová verze Windows* a *64-bit Linux (8 Ubuntu/Debian)* operační systémy.

je potřeba Hello následující software:

* [Git klienta](https://git-scm.com/downloads).
* [**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* [Maven](https://maven.apache.org/install.html).

Otevřete příkazového řádku terminálu okno a klonování hello následující úložiště:

1. `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a>Přehled architektury

platformy Azure IoT Edge Hello výraznou přijme hello [Von Neumannova architektura](https://en.wikipedia.org/wiki/Von_Neumann_architecture). To znamená této architektuře hello celý Azure IoT okraj je systém, který zpracovává vstup a výstup; a každý jednotlivých modul je také velmi malé subsystému vstup a výstup. V tomto kurzu jsme vás seznámí hello následující dva moduly:

1. Modul, který obdrží simulované [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signál a převede jej na formátovaný [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.
2. Modul, který vytiskne hello přijata [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.

Hello následující obrázek zobrazuje typické začátku do konce toku hello dat pro tento projekt.

![Tok dat mezi tři moduly](media/iot-hub-iot-edge-create-module/dataflow.png "vstup: Simulated zakázat modulu; Procesor: Převaděč modulu; Výstup: Modul tiskárny")

## <a name="understanding-hello-code"></a>Pochopení kódu hello

### <a name="maven-project-structure"></a>Struktura projekt maven

Vzhledem k tomu, že Azure IoT Edge balíčky jsou založená na Maven, potřebujeme toocreate typické struktura projekt Maven, který obsahuje `pom.xml` souboru.

Hello POM dědí z hello `com.microsoft.azure.gateway.gateway-module-base` balíček, který deklaruje všechny hello závislosti, které modul projekt, která zahrnuje hello binární soubory modulu runtime, cesta ke konfiguračnímu souboru hello brány a chování při spuštění hello. To nám uloží velké množství času a eliminovat potřeba toowrite hello a opakovaně přepisování stovky řádků kódu.

Potřebujeme tooupdate soubor pom.xml deklarováním hello požadované závislosti nebo modulů plug-in a hello název hello konfigurační soubor toobe používané naše modulu, jak je znázorněno v následujícím fragmentu kódu hello.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!-- Inherit from parent -->
  <parent>
    <groupId>com.microsoft.azure.gateway</groupId>
    <artifactId>gateway-module-base</artifactId>
    <version>1.0.1</version>
  </parent>
  
  <groupId>com.microsoft.azure.gateway</groupId>
  <artifactId>ble-converter</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>

  <!-- Set hello filename of hello Azure IoT Edge configuration located
       under ./src/main/resources/gateway/ which is used in parent -->
  <properties>
    <gw.config.fileName>gw-config.json</gw.config.fileName>
  </properties>

  <!-- Re-declare dependencies used in parent -->
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.gateway</groupId>
      <artifactId>gateway-java-binding</artifactId>
    </dependency>
    <dependency>
      <groupId>${dependency.runtime.group}</groupId>
      <artifactId>${dependency.runtime.name}</artifactId>
    </dependency>
  </dependencies>

  <!-- Re-declare plugins used in parent -->
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a>Základní znalosti o modul Azure IoT Edge

Modul služby Azure IoT Edge lze považovat procesor dat, jejichž povinnostem je: přijímání vstupu, zpracovat a vytvoření výstupu.

vstup Hello může být data z hardware (např. detektor pohybu), zprávu z ostatních modulů, nebo cokoliv jiného (např. náhodné číslo pravidelně generované časovač).

výstup Hello je podobné toohello vstup, se může spustit chování hardware (např. hello blikat DIODU), zpráva tooother moduly nebo cokoliv jiného (např. tisk toohello konzoly).

Moduly navzájem komunikují pomocí `com.microsoft.azure.gateway.messaging.Message` třídy. Hello **obsahu** z `Message` je bajtové pole, která umožňuje představující jakýkoli druh dat se vám líbí. **Vlastnosti** jsou také k dispozici v hello `Message` a jsou jednoduše mapování řetězec řetězec. Může si můžete představit **vlastnosti** jako hello hlavičky v požadavku HTTP, nebo hello metadata souboru.

V pořadí toodevelop modul Azure IoT Edge v jazyce Java, budete potřebovat toocreate novou třídu modul, který dědí z `com.microsoft.azure.gateway.core.GatewayModule` a implementaci abstraktní metody hello požadované `receive()` a `destroy()`. V tomto okamžiku můžete také tooimplement hello volitelné `start()` nebo `create()` také metody. Hello následující fragment kódu ukazuje, jak tooget zahájeno vytváření modul Azure IoT okraj.

```java
import com.microsoft.azure.gateway.core.Broker;
import com.microsoft.azure.gateway.core.GatewayModule;
import com.microsoft.azure.gateway.messaging.Message;

public class MyEdgeModule extends GatewayModule {
  public MyEdgeModule(long address, Broker broker, String configuration) {
    /* Let hello GatewayModule do hello dirty work of initialization. It's also
       a good time tooparse your own configuration defined in Azure IoT Edge
       configuration file (typically ./src/main/resources/gateway/gw-config.json) */
    super(address, broker, configuration);
  }

  @Override
  public void start() {
    /* Acquire hello resources you need. If you don't
       need any resources, you may omit this method. */
  }

  @Override
  public void destroy() {
    /* It's time toorelease all resources. This method is required. */
  }

  @Override
  public void receive(Message message) {
    /* Logic tooprocess hello input message. This method is required. */
    // ...
    /* Use publish() method toodo hello output. You are
       allowed toopublish your new Message instance. */
    this.publish(message);
  }
}
```

### <a name="converter-module"></a>Převaděč modulu

| Vstup                    | Procesor                              | Výstup                 | Zdrojový soubor            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Teplotní datových zpráv | Analýza a vytvořit novou zprávu JSON | Struktura JSON zpráv | `ConverterModule.java` |

Tento modul je typické modul Azure IoT okraj. Přijímá zprávy teploty z ostatních modulů (modul hardwarového nebo v tomto případě naše simulované modulu lit); a pak normalizuje uvítací zprávu teploty tooa strukturovaná JSON zprávy (včetně připojování hello ID zprávy, nastavení hello vlastnost jestli potřebujeme tootrigger hello teploty výstrahy a tak dále).

```java
@Override
public void receive(Message message) {
  try {
    JSONObject messageFromBle = new JSONObject(new String(message.getContent()));
    double temperature = messageFromBle.getDouble("temperature");
    Map<String, String> inputProperties = message.getProperties();

    HashMap<String, String> properties = new HashMap<>();
    properties.put("source", inputProperties.get("source"));
    properties.put("macAddress", inputProperties.get("macAddress"));
    properties.put("temperatureAlert", temperature > 30 ? "true" : "false");

    String content = String.format(
        "{ \"deviceId\": \"Intel NUC Gateway\", \"messageId\": %d, \"temperature\": %f }",
        ++this.messageCount, temperature);

    this.publish(new Message(content.getBytes(), properties));
  } catch (Exception ex) {
    ex.printStackTrace();
  }
}
```

### <a name="printer-module"></a>Modul tiskárny

| Vstup                          | Procesor | Výstup                     | Zdrojový soubor          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Jakékoli zprávy z ostatních modulů | Není k dispozici       | Protokolování zpráv tooconsole hello | `PrinterModule.java` |

Toto je jednoduchá, není potřeba vysvětlovat, modul, který výstupy hello přijaté zprávy toohello okno terminálu.

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a>Konfigurace Azure IoT Edge

posledním krokem Hello před spuštěním moduly hello je tooconfigure hello Azure IoT okraj a tooestablish hello připojení mezi moduly.

Nejprve je třeba toodeclare naše zavaděč Java (od Azure IoT Edge podporuje zavaděče různých jazyků), který může být odkazuje jeho `name` v částech hello i později.

```json
"loaders": [{
  "type": "java",
  "name": "java",
  "configuration": {
    "jvm.options": {
      "library.path": "./"
    }
  }
}]
```

Jakmile jsme prohlásí, že je naše zavaděče, bude také potřebujeme toodeclare také naše moduly. Podobné zavaděče hello toodeclaring, že lze také odkazovat podle jejich `name` atribut. Když modul deklarace, potřebujeme toospecify hello zavaděč měli použít (které by se měly hello jeden jsme definovali před) a hello vstupního bodu (by měl být hello normalizovaný název třídy Naše modulu) pro každý modul. Hello `simulated_device` modul je nativní modul, který je součástí hello Azure IoT Edge core runtime balíčku. Vždy by měla obsahovat vaše `args` v souboru JSON, i když je hello `null`.

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
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/ConverterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  },
  {
    "name": "print",
    "loader": {
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/PrinterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
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
    "sink": "print"
  }
]
```

## <a name="running-hello-modules"></a>Spuštěné moduly hello

Použití `mvn package` toobuild všechno, co do hello `target/` složky. `mvn clean package`je také vhodné pro čistá sestavení.

Použití `mvn exec:exec` toorun hello Azure IoT okraj a měli zjistit, zda jsou hello teploty data a všechny vlastnosti hello konzoly na tištěných toohello s pevnou sazbou.

Pokud chcete aplikace hello tooterminate, stiskněte klávesu `<Enter>` klíč.

> [!IMPORTANT]
> Není doporučeno toouse Ctrl + C tooterminate hello IoT hraniční brány aplikace. Jak to může způsobit hello proces tooterminate neobvyklým způsobem.

