---
title: "Vytvořit modul Azure IoT Edge s Javou | Microsoft Docs"
description: "V tomto kurzu umožňující prezentovat jak napsat modulu převaděč dat zakázat pomocí nejnovější balíčky Azure IoT Edge Maven."
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
ms.openlocfilehash: 0c430272225d79737baec2be15ed7c93991cdeac
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-java"></a><span data-ttu-id="90076-103">Vytvořit modul Azure IoT Edge s Javou</span><span class="sxs-lookup"><span data-stu-id="90076-103">Create an Azure IoT Edge Module with Java</span></span>

<span data-ttu-id="90076-104">V tomto kurzu hodnotí, jak jeden může vytvořit modul pro Azure IoT hrany v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="90076-104">This tutorial showcases how one might build a module for Azure IoT Edge in Java.</span></span>

<span data-ttu-id="90076-105">V tomto kurzu jsme provede procesem nastavení prostředí a jak napsat [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modulu převaděč dat pomocí nejnovější balíčky Azure IoT Edge Maven.</span><span class="sxs-lookup"><span data-stu-id="90076-105">In this tutorial, we will walk through environment setup and how to write a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using the latest Azure IoT Edge Maven packages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90076-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="90076-106">Prerequisites</span></span>

<span data-ttu-id="90076-107">V této části bude nastavení prostředí pro vývoj modulu IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="90076-107">In this section, you will set up your environment for IoT Edge module development.</span></span> <span data-ttu-id="90076-108">Platí pro obě *64bitová verze Windows* a *64-bit Linux (8 Ubuntu/Debian)* operační systémy.</span><span class="sxs-lookup"><span data-stu-id="90076-108">It applies to both *64-bit Windows* and *64-bit Linux (Ubuntu/Debian 8)* operating systems.</span></span>

<span data-ttu-id="90076-109">Je vyžadován následující software:</span><span class="sxs-lookup"><span data-stu-id="90076-109">The following software is required:</span></span>

* <span data-ttu-id="90076-110">[Git klienta](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="90076-110">[Git Client](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="90076-111">[**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="90076-111">[**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="90076-112">[Maven](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="90076-112">[Maven](https://maven.apache.org/install.html).</span></span>

<span data-ttu-id="90076-113">Otevřete okno příkazového řádku terminálu a klonovat úložiště v následující:</span><span class="sxs-lookup"><span data-stu-id="90076-113">Open a command-line terminal window and clone the following repository:</span></span>

1. <span data-ttu-id="90076-114">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span><span class="sxs-lookup"><span data-stu-id="90076-114">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span></span>
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a><span data-ttu-id="90076-115">Přehled architektury</span><span class="sxs-lookup"><span data-stu-id="90076-115">Overall architecture</span></span>

<span data-ttu-id="90076-116">Platformy Azure IoT Edge výraznou přijme [Von Neumannova architektura](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span><span class="sxs-lookup"><span data-stu-id="90076-116">The Azure IoT Edge platform heavily adopts the [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span></span> <span data-ttu-id="90076-117">Které znamená, že se celý Azure IoT Edge architektura systému, který zpracovává vstup a výstup; a každý jednotlivých modul je také velmi malé subsystému vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="90076-117">Which means that the entire Azure IoT Edge architecture is a system which processes input and produces output; and that each individual module is also a tiny input-output subsystem.</span></span> <span data-ttu-id="90076-118">V tomto kurzu jsme vás seznámí následující dva moduly:</span><span class="sxs-lookup"><span data-stu-id="90076-118">In this tutorial, we will introduce the following two modules:</span></span>

1. <span data-ttu-id="90076-119">Modul, který obdrží simulované [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signál a převede jej na formátovaný [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.</span><span class="sxs-lookup"><span data-stu-id="90076-119">A module which receives a simulated [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signal and converts it into a formatted [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>
2. <span data-ttu-id="90076-120">Modul, který vytiskne přijatého [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.</span><span class="sxs-lookup"><span data-stu-id="90076-120">A module which prints the received [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>

<span data-ttu-id="90076-121">Následující obrázek zobrazuje typické toku začátku do konce dat pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="90076-121">The following image displays the typical end-to-end dataflow for this project:</span></span>

<span data-ttu-id="90076-122">![Tok dat mezi tři moduly](media/iot-hub-iot-edge-create-module/dataflow.png "vstup: Simulated zakázat modulu; Procesor: Převaděč modulu; Výstup: Modul tiskárny")</span><span class="sxs-lookup"><span data-stu-id="90076-122">![Dataflow between three modules](media/iot-hub-iot-edge-create-module/dataflow.png "Input: Simulated BLE Module; Processor: Converter Module; Output: Printer Module")</span></span>

## <a name="understanding-the-code"></a><span data-ttu-id="90076-123">Pochopení kódu</span><span class="sxs-lookup"><span data-stu-id="90076-123">Understanding the code</span></span>

### <a name="maven-project-structure"></a><span data-ttu-id="90076-124">Struktura projekt maven</span><span class="sxs-lookup"><span data-stu-id="90076-124">Maven project structure</span></span>

<span data-ttu-id="90076-125">Vzhledem k tomu, že Azure IoT Edge balíčky jsou založená na Maven, je potřeba vytvořit typické struktura projekt Maven, který obsahuje `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="90076-125">Since Azure IoT Edge packages are based on Maven, we need to create a typical Maven project structure, which contains a `pom.xml` file.</span></span>

<span data-ttu-id="90076-126">POM dědí z `com.microsoft.azure.gateway.gateway-module-base` balíček, který deklaruje všechny závislé součásti, které modul projekt, která zahrnuje binární soubory modulu runtime, cesta k souboru konfigurace brány a chování při spuštění.</span><span class="sxs-lookup"><span data-stu-id="90076-126">The POM inherits from the `com.microsoft.azure.gateway.gateway-module-base` package, which declares all of the dependencies needed by a module project which includes the runtime binaries, the gateway configuration file path, and the execution behavior.</span></span> <span data-ttu-id="90076-127">To nám uloží velké množství času a eliminovat potřebu zápisu a opakovaně přepisování stovky řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="90076-127">This saves us lots of time and eliminate the need to write and rewrite hundreds of lines of code over and over again.</span></span>

<span data-ttu-id="90076-128">Je potřeba aktualizovat soubor pom.xml deklarace požadované závislosti nebo moduly plug-in a název konfiguračního souboru pro použití naše modulem, jak je znázorněno v následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="90076-128">We need to update the pom.xml file by declaring the required dependencies/plugins and the name of the configuration file to be used by our module as shown in the following code snippet.</span></span>

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

  <!-- Set the filename of the Azure IoT Edge configuration located
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

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a><span data-ttu-id="90076-129">Základní znalosti o modul Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="90076-129">Basic understanding of an Azure IoT Edge module</span></span>

<span data-ttu-id="90076-130">Modul služby Azure IoT Edge lze považovat procesor dat, jejichž povinnostem je: přijímání vstupu, zpracovat a vytvoření výstupu.</span><span class="sxs-lookup"><span data-stu-id="90076-130">You can treat an Azure IoT Edge module as a data processor whose job is to: receive input, process it, and produce output.</span></span>

<span data-ttu-id="90076-131">Vstup může být data z hardware (např. detektor pohybu), zprávu z ostatních modulů, nebo cokoliv jiného (např. náhodné číslo pravidelně generované časovač).</span><span class="sxs-lookup"><span data-stu-id="90076-131">The input might be data from hardware (like a motion detector), a message from other modules, or anything else (like a random number generated periodically by a timer).</span></span>

<span data-ttu-id="90076-132">Výstup bude vypadat na vstup, se může spustit chování hardware (např. blikající LED), zprávu, která z ostatních modulů, nebo cokoliv jiného (např. tisk do konzoly).</span><span class="sxs-lookup"><span data-stu-id="90076-132">The output is similar to the input, it could trigger hardware behavior (like the blinking LED), a message to other modules, or anything else (like printing to the console).</span></span>

<span data-ttu-id="90076-133">Moduly navzájem komunikují pomocí `com.microsoft.azure.gateway.messaging.Message` třídy.</span><span class="sxs-lookup"><span data-stu-id="90076-133">Modules communicate with each other using `com.microsoft.azure.gateway.messaging.Message` class.</span></span> <span data-ttu-id="90076-134">**Obsahu** z `Message` je bajtové pole, která umožňuje představující jakýkoli druh dat se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="90076-134">The **Content** of a `Message` is a byte array which is capable of representing any kind of data you like.</span></span> <span data-ttu-id="90076-135">**Vlastnosti** jsou také k dispozici v `Message` a jsou jednoduše mapování řetězec řetězec.</span><span class="sxs-lookup"><span data-stu-id="90076-135">**Properties** are also available in the `Message` and are simply a string-to-string mapping.</span></span> <span data-ttu-id="90076-136">Může si můžete představit **vlastnosti** jako hlavičky v požadavku HTTP nebo metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="90076-136">You may think of **Properties** as the headers in an HTTP request, or the metadata of a file.</span></span>

<span data-ttu-id="90076-137">Chcete-li vytvořit modul Azure IoT Edge v jazyce Java, je potřeba vytvořit novou třídu modul, který dědí z `com.microsoft.azure.gateway.core.GatewayModule` a implementovat požadované abstraktní metody `receive()` a `destroy()`.</span><span class="sxs-lookup"><span data-stu-id="90076-137">In order to develop an Azure IoT Edge module in Java, you need to create a new module class which inherits from `com.microsoft.azure.gateway.core.GatewayModule` and implement the required abstract methods `receive()` and `destroy()`.</span></span> <span data-ttu-id="90076-138">V tomto okamžiku můžete také implementovat volitelné `start()` nebo `create()` také metody.</span><span class="sxs-lookup"><span data-stu-id="90076-138">At this point, you may also choose to implement the optional `start()` or `create()` methods as well.</span></span> <span data-ttu-id="90076-139">Následující fragment kódu ukazuje, jak začít pracovat, vytváření modul Azure IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="90076-139">The following code snippet shows you how to get started authoring an Azure IoT Edge module.</span></span>

```java
import com.microsoft.azure.gateway.core.Broker;
import com.microsoft.azure.gateway.core.GatewayModule;
import com.microsoft.azure.gateway.messaging.Message;

public class MyEdgeModule extends GatewayModule {
  public MyEdgeModule(long address, Broker broker, String configuration) {
    /* Let the GatewayModule do the dirty work of initialization. It's also
       a good time to parse your own configuration defined in Azure IoT Edge
       configuration file (typically ./src/main/resources/gateway/gw-config.json) */
    super(address, broker, configuration);
  }

  @Override
  public void start() {
    /* Acquire the resources you need. If you don't
       need any resources, you may omit this method. */
  }

  @Override
  public void destroy() {
    /* It's time to release all resources. This method is required. */
  }

  @Override
  public void receive(Message message) {
    /* Logic to process the input message. This method is required. */
    // ...
    /* Use publish() method to do the output. You are
       allowed to publish your new Message instance. */
    this.publish(message);
  }
}
```

### <a name="converter-module"></a><span data-ttu-id="90076-140">Převaděč modulu</span><span class="sxs-lookup"><span data-stu-id="90076-140">Converter module</span></span>

| <span data-ttu-id="90076-141">Vstup</span><span class="sxs-lookup"><span data-stu-id="90076-141">Input</span></span>                    | <span data-ttu-id="90076-142">Procesor</span><span class="sxs-lookup"><span data-stu-id="90076-142">Processor</span></span>                              | <span data-ttu-id="90076-143">Výstup</span><span class="sxs-lookup"><span data-stu-id="90076-143">Output</span></span>                 | <span data-ttu-id="90076-144">Zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="90076-144">Source File</span></span>            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| <span data-ttu-id="90076-145">Teplotní datových zpráv</span><span class="sxs-lookup"><span data-stu-id="90076-145">Temperature data message</span></span> | <span data-ttu-id="90076-146">Analýza a vytvořit novou zprávu JSON</span><span class="sxs-lookup"><span data-stu-id="90076-146">Parse and construct a new JSON message</span></span> | <span data-ttu-id="90076-147">Struktura JSON zpráv</span><span class="sxs-lookup"><span data-stu-id="90076-147">Structure JSON message</span></span> | `ConverterModule.java` |

<span data-ttu-id="90076-148">Tento modul je typické modul Azure IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="90076-148">This module is a typical Azure IoT Edge module.</span></span> <span data-ttu-id="90076-149">Přijímá zprávy teploty z ostatních modulů (modul hardwarového nebo v tomto případě naše simulované modulu lit); a pak normalizuje teploty zprávu ve zprávě strukturovaných JSON (včetně připojování ID zprávy, nastavení vlastnosti jestli musíme teploty výstrahu aktivovat a tak dále).</span><span class="sxs-lookup"><span data-stu-id="90076-149">It accepts temperature messages from other modules (a hardware module, or in this case our simulated BLE module); and then normalizes the temperature message in to a structured JSON message (including appending the message ID, setting the property of whether we need to trigger the temperature alert, and so on).</span></span>

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

### <a name="printer-module"></a><span data-ttu-id="90076-150">Modul tiskárny</span><span class="sxs-lookup"><span data-stu-id="90076-150">Printer module</span></span>

| <span data-ttu-id="90076-151">Vstup</span><span class="sxs-lookup"><span data-stu-id="90076-151">Input</span></span>                          | <span data-ttu-id="90076-152">Procesor</span><span class="sxs-lookup"><span data-stu-id="90076-152">Processor</span></span> | <span data-ttu-id="90076-153">Výstup</span><span class="sxs-lookup"><span data-stu-id="90076-153">Output</span></span>                     | <span data-ttu-id="90076-154">Zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="90076-154">Source File</span></span>          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| <span data-ttu-id="90076-155">Jakékoli zprávy z ostatních modulů</span><span class="sxs-lookup"><span data-stu-id="90076-155">Any message from other modules</span></span> | <span data-ttu-id="90076-156">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="90076-156">N/A</span></span>       | <span data-ttu-id="90076-157">Přihlaste se do konzoly zprávy</span><span class="sxs-lookup"><span data-stu-id="90076-157">Log the message to console</span></span> | `PrinterModule.java` |

<span data-ttu-id="90076-158">Toto je jednoduchá, není potřeba vysvětlovat, modul, který výstupy přijatých zpráv do okna terminálu.</span><span class="sxs-lookup"><span data-stu-id="90076-158">This is a simple, self-explanatory, module which outputs the received messages to the terminal window.</span></span>

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a><span data-ttu-id="90076-159">Konfigurace Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="90076-159">Azure IoT Edge configuration</span></span>

<span data-ttu-id="90076-160">V posledním kroku před spuštěním moduly je ke konfiguraci Azure IoT okraj a k navázání připojení mezi moduly.</span><span class="sxs-lookup"><span data-stu-id="90076-160">The final step before running the modules is to configure the Azure IoT Edge and to establish the connections between modules.</span></span>

<span data-ttu-id="90076-161">Nejprve je potřeba deklarovat naše zavaděč Java (od Azure IoT Edge podporuje zavaděče různých jazyků), který může být odkazuje jeho `name` v částech i později.</span><span class="sxs-lookup"><span data-stu-id="90076-161">First we need to declare our Java loader (since Azure IoT Edge supports loaders of different languages) which could be referenced by its `name` in the sections afterward.</span></span>

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

<span data-ttu-id="90076-162">Jakmile jsme prohlásí, že je naše zavaděče, bude také musíme deklarovat také naše moduly.</span><span class="sxs-lookup"><span data-stu-id="90076-162">Once we have declared our loaders, we will also need to declare our modules as well.</span></span> <span data-ttu-id="90076-163">Podobně jako deklarace zavaděče, že lze také odkazovat podle jejich `name` atribut.</span><span class="sxs-lookup"><span data-stu-id="90076-163">Similar to declaring the loaders, they can also be referenced by their `name` attribute.</span></span> <span data-ttu-id="90076-164">Když modul deklarace, musíme určit zavaděč, měla by používat (který by měl být jeden jsme definovali před) a vstupní bod (musí být normalizovaný název třídy Naše modulu) pro každý modul.</span><span class="sxs-lookup"><span data-stu-id="90076-164">When declaring a module, we need to specify the loader it should use (which should be the one we defined before) and the entry-point (should be the normalized class name of our module) for each module.</span></span> <span data-ttu-id="90076-165">`simulated_device` Modul je nativní modul, který je zahrnutý v balíčku Azure IoT Edge core runtime.</span><span class="sxs-lookup"><span data-stu-id="90076-165">The `simulated_device` module is a native module which is included in the Azure IoT Edge core runtime package.</span></span> <span data-ttu-id="90076-166">Vždy by měla obsahovat vaše `args` v kódu JSON souboru i v případě, že je `null`.</span><span class="sxs-lookup"><span data-stu-id="90076-166">You should always include `args` in the JSON file even if it is `null`.</span></span>

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

<span data-ttu-id="90076-167">Na konci konfigurace naváže připojení.</span><span class="sxs-lookup"><span data-stu-id="90076-167">At the end of the configuration, we establish the connections.</span></span> <span data-ttu-id="90076-168">Je každé připojení vyjádřená `source` a `sink`.</span><span class="sxs-lookup"><span data-stu-id="90076-168">Each connection is expressed by `source` and `sink`.</span></span> <span data-ttu-id="90076-169">Jejich by měl obě odkazovat modul předem definovaná.</span><span class="sxs-lookup"><span data-stu-id="90076-169">They should both reference a pre-defined module.</span></span> <span data-ttu-id="90076-170">Výstup zprávu `source` modulu se předají na vstup `sink` modulu.</span><span class="sxs-lookup"><span data-stu-id="90076-170">The output message of `source` module will be forwarded to the input of `sink` module.</span></span>

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

## <a name="running-the-modules"></a><span data-ttu-id="90076-171">Spuštění moduly</span><span class="sxs-lookup"><span data-stu-id="90076-171">Running the modules</span></span>

<span data-ttu-id="90076-172">Použití `mvn package` k sestavení všechno, co do `target/` složky.</span><span class="sxs-lookup"><span data-stu-id="90076-172">Use `mvn package` to build everything into the `target/` folder.</span></span> <span data-ttu-id="90076-173">`mvn clean package`je také vhodné pro čistá sestavení.</span><span class="sxs-lookup"><span data-stu-id="90076-173">`mvn clean package` is also recommended for a clean build.</span></span>

<span data-ttu-id="90076-174">Použití `mvn exec:exec` ke spuštění na Azure IoT okraj a jste měli zjistit že se ke konzole s pevnou sazbou vytištěn teploty data a všechny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="90076-174">Use `mvn exec:exec` to run the Azure IoT Edge and you should observe that the temperature data and all the properties are printed to the console at a fixed rate.</span></span>

<span data-ttu-id="90076-175">Pokud chcete aplikaci ukončit, stiskněte klávesu `<Enter>` klíč.</span><span class="sxs-lookup"><span data-stu-id="90076-175">If you want to terminate the application, press `<Enter>` key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="90076-176">Není doporučeno používat kombinace kláves Ctrl + C k ukončení aplikace IoT hraniční brány.</span><span class="sxs-lookup"><span data-stu-id="90076-176">It is not recommended to use Ctrl + C to terminate the IoT Edge gateway application.</span></span> <span data-ttu-id="90076-177">Jak to může způsobit neobvyklým způsobem ukončení procesu.</span><span class="sxs-lookup"><span data-stu-id="90076-177">As this may cause the process to terminate abnormally.</span></span>

