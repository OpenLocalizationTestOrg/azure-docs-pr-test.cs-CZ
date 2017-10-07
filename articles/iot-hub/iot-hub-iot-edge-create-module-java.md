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
# <a name="create-an-azure-iot-edge-module-with-java"></a><span data-ttu-id="e7fb9-103">Vytvořit modul Azure IoT Edge s Javou</span><span class="sxs-lookup"><span data-stu-id="e7fb9-103">Create an Azure IoT Edge Module with Java</span></span>

<span data-ttu-id="e7fb9-104">V tomto kurzu hodnotí, jak jeden může vytvořit modul pro Azure IoT hrany v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-104">This tutorial showcases how one might build a module for Azure IoT Edge in Java.</span></span>

<span data-ttu-id="e7fb9-105">V tomto kurzu jsme provede procesem nastavení prostředí a jak toowrite [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modulu převaděč dat pomocí Azure IoT Edge Maven balíčky nejnovější hello.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-105">In this tutorial, we will walk through environment setup and how toowrite a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using hello latest Azure IoT Edge Maven packages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7fb9-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e7fb9-106">Prerequisites</span></span>

<span data-ttu-id="e7fb9-107">V této části bude nastavení prostředí pro vývoj modulu IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-107">In this section, you will set up your environment for IoT Edge module development.</span></span> <span data-ttu-id="e7fb9-108">Platí tooboth *64bitová verze Windows* a *64-bit Linux (8 Ubuntu/Debian)* operační systémy.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-108">It applies tooboth *64-bit Windows* and *64-bit Linux (Ubuntu/Debian 8)* operating systems.</span></span>

<span data-ttu-id="e7fb9-109">je potřeba Hello následující software:</span><span class="sxs-lookup"><span data-stu-id="e7fb9-109">hello following software is required:</span></span>

* <span data-ttu-id="e7fb9-110">[Git klienta](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="e7fb9-110">[Git Client](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="e7fb9-111">[**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="e7fb9-111">[**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="e7fb9-112">[Maven](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="e7fb9-112">[Maven](https://maven.apache.org/install.html).</span></span>

<span data-ttu-id="e7fb9-113">Otevřete příkazového řádku terminálu okno a klonování hello následující úložiště:</span><span class="sxs-lookup"><span data-stu-id="e7fb9-113">Open a command-line terminal window and clone hello following repository:</span></span>

1. <span data-ttu-id="e7fb9-114">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-114">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span></span>
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a><span data-ttu-id="e7fb9-115">Přehled architektury</span><span class="sxs-lookup"><span data-stu-id="e7fb9-115">Overall architecture</span></span>

<span data-ttu-id="e7fb9-116">platformy Azure IoT Edge Hello výraznou přijme hello [Von Neumannova architektura](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span><span class="sxs-lookup"><span data-stu-id="e7fb9-116">hello Azure IoT Edge platform heavily adopts hello [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span></span> <span data-ttu-id="e7fb9-117">To znamená této architektuře hello celý Azure IoT okraj je systém, který zpracovává vstup a výstup; a každý jednotlivých modul je také velmi malé subsystému vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-117">Which means that hello entire Azure IoT Edge architecture is a system which processes input and produces output; and that each individual module is also a tiny input-output subsystem.</span></span> <span data-ttu-id="e7fb9-118">V tomto kurzu jsme vás seznámí hello následující dva moduly:</span><span class="sxs-lookup"><span data-stu-id="e7fb9-118">In this tutorial, we will introduce hello following two modules:</span></span>

1. <span data-ttu-id="e7fb9-119">Modul, který obdrží simulované [lit](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signál a převede jej na formátovaný [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-119">A module which receives a simulated [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signal and converts it into a formatted [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>
2. <span data-ttu-id="e7fb9-120">Modul, který vytiskne hello přijata [JSON](https://en.wikipedia.org/wiki/JSON) zprávy.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-120">A module which prints hello received [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>

<span data-ttu-id="e7fb9-121">Hello následující obrázek zobrazuje typické začátku do konce toku hello dat pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-121">hello following image displays hello typical end-to-end dataflow for this project:</span></span>

<span data-ttu-id="e7fb9-122">![Tok dat mezi tři moduly](media/iot-hub-iot-edge-create-module/dataflow.png "vstup: Simulated zakázat modulu; Procesor: Převaděč modulu; Výstup: Modul tiskárny")</span><span class="sxs-lookup"><span data-stu-id="e7fb9-122">![Dataflow between three modules](media/iot-hub-iot-edge-create-module/dataflow.png "Input: Simulated BLE Module; Processor: Converter Module; Output: Printer Module")</span></span>

## <a name="understanding-hello-code"></a><span data-ttu-id="e7fb9-123">Pochopení kódu hello</span><span class="sxs-lookup"><span data-stu-id="e7fb9-123">Understanding hello code</span></span>

### <a name="maven-project-structure"></a><span data-ttu-id="e7fb9-124">Struktura projekt maven</span><span class="sxs-lookup"><span data-stu-id="e7fb9-124">Maven project structure</span></span>

<span data-ttu-id="e7fb9-125">Vzhledem k tomu, že Azure IoT Edge balíčky jsou založená na Maven, potřebujeme toocreate typické struktura projekt Maven, který obsahuje `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-125">Since Azure IoT Edge packages are based on Maven, we need toocreate a typical Maven project structure, which contains a `pom.xml` file.</span></span>

<span data-ttu-id="e7fb9-126">Hello POM dědí z hello `com.microsoft.azure.gateway.gateway-module-base` balíček, který deklaruje všechny hello závislosti, které modul projekt, která zahrnuje hello binární soubory modulu runtime, cesta ke konfiguračnímu souboru hello brány a chování při spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-126">hello POM inherits from hello `com.microsoft.azure.gateway.gateway-module-base` package, which declares all of hello dependencies needed by a module project which includes hello runtime binaries, hello gateway configuration file path, and hello execution behavior.</span></span> <span data-ttu-id="e7fb9-127">To nám uloží velké množství času a eliminovat potřeba toowrite hello a opakovaně přepisování stovky řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-127">This saves us lots of time and eliminate hello need toowrite and rewrite hundreds of lines of code over and over again.</span></span>

<span data-ttu-id="e7fb9-128">Potřebujeme tooupdate soubor pom.xml deklarováním hello požadované závislosti nebo modulů plug-in a hello název hello konfigurační soubor toobe používané naše modulu, jak je znázorněno v následujícím fragmentu kódu hello.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-128">We need tooupdate the pom.xml file by declaring hello required dependencies/plugins and hello name of hello configuration file toobe used by our module as shown in hello following code snippet.</span></span>

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

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a><span data-ttu-id="e7fb9-129">Základní znalosti o modul Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="e7fb9-129">Basic understanding of an Azure IoT Edge module</span></span>

<span data-ttu-id="e7fb9-130">Modul služby Azure IoT Edge lze považovat procesor dat, jejichž povinnostem je: přijímání vstupu, zpracovat a vytvoření výstupu.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-130">You can treat an Azure IoT Edge module as a data processor whose job is to: receive input, process it, and produce output.</span></span>

<span data-ttu-id="e7fb9-131">vstup Hello může být data z hardware (např. detektor pohybu), zprávu z ostatních modulů, nebo cokoliv jiného (např. náhodné číslo pravidelně generované časovač).</span><span class="sxs-lookup"><span data-stu-id="e7fb9-131">hello input might be data from hardware (like a motion detector), a message from other modules, or anything else (like a random number generated periodically by a timer).</span></span>

<span data-ttu-id="e7fb9-132">výstup Hello je podobné toohello vstup, se může spustit chování hardware (např. hello blikat DIODU), zpráva tooother moduly nebo cokoliv jiného (např. tisk toohello konzoly).</span><span class="sxs-lookup"><span data-stu-id="e7fb9-132">hello output is similar toohello input, it could trigger hardware behavior (like hello blinking LED), a message tooother modules, or anything else (like printing toohello console).</span></span>

<span data-ttu-id="e7fb9-133">Moduly navzájem komunikují pomocí `com.microsoft.azure.gateway.messaging.Message` třídy.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-133">Modules communicate with each other using `com.microsoft.azure.gateway.messaging.Message` class.</span></span> <span data-ttu-id="e7fb9-134">Hello **obsahu** z `Message` je bajtové pole, která umožňuje představující jakýkoli druh dat se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-134">hello **Content** of a `Message` is a byte array which is capable of representing any kind of data you like.</span></span> <span data-ttu-id="e7fb9-135">**Vlastnosti** jsou také k dispozici v hello `Message` a jsou jednoduše mapování řetězec řetězec.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-135">**Properties** are also available in hello `Message` and are simply a string-to-string mapping.</span></span> <span data-ttu-id="e7fb9-136">Může si můžete představit **vlastnosti** jako hello hlavičky v požadavku HTTP, nebo hello metadata souboru.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-136">You may think of **Properties** as hello headers in an HTTP request, or hello metadata of a file.</span></span>

<span data-ttu-id="e7fb9-137">V pořadí toodevelop modul Azure IoT Edge v jazyce Java, budete potřebovat toocreate novou třídu modul, který dědí z `com.microsoft.azure.gateway.core.GatewayModule` a implementaci abstraktní metody hello požadované `receive()` a `destroy()`.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-137">In order toodevelop an Azure IoT Edge module in Java, you need toocreate a new module class which inherits from `com.microsoft.azure.gateway.core.GatewayModule` and implement hello required abstract methods `receive()` and `destroy()`.</span></span> <span data-ttu-id="e7fb9-138">V tomto okamžiku můžete také tooimplement hello volitelné `start()` nebo `create()` také metody.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-138">At this point, you may also choose tooimplement hello optional `start()` or `create()` methods as well.</span></span> <span data-ttu-id="e7fb9-139">Hello následující fragment kódu ukazuje, jak tooget zahájeno vytváření modul Azure IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-139">hello following code snippet shows you how tooget started authoring an Azure IoT Edge module.</span></span>

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

### <a name="converter-module"></a><span data-ttu-id="e7fb9-140">Převaděč modulu</span><span class="sxs-lookup"><span data-stu-id="e7fb9-140">Converter module</span></span>

| <span data-ttu-id="e7fb9-141">Vstup</span><span class="sxs-lookup"><span data-stu-id="e7fb9-141">Input</span></span>                    | <span data-ttu-id="e7fb9-142">Procesor</span><span class="sxs-lookup"><span data-stu-id="e7fb9-142">Processor</span></span>                              | <span data-ttu-id="e7fb9-143">Výstup</span><span class="sxs-lookup"><span data-stu-id="e7fb9-143">Output</span></span>                 | <span data-ttu-id="e7fb9-144">Zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="e7fb9-144">Source File</span></span>            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| <span data-ttu-id="e7fb9-145">Teplotní datových zpráv</span><span class="sxs-lookup"><span data-stu-id="e7fb9-145">Temperature data message</span></span> | <span data-ttu-id="e7fb9-146">Analýza a vytvořit novou zprávu JSON</span><span class="sxs-lookup"><span data-stu-id="e7fb9-146">Parse and construct a new JSON message</span></span> | <span data-ttu-id="e7fb9-147">Struktura JSON zpráv</span><span class="sxs-lookup"><span data-stu-id="e7fb9-147">Structure JSON message</span></span> | `ConverterModule.java` |

<span data-ttu-id="e7fb9-148">Tento modul je typické modul Azure IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-148">This module is a typical Azure IoT Edge module.</span></span> <span data-ttu-id="e7fb9-149">Přijímá zprávy teploty z ostatních modulů (modul hardwarového nebo v tomto případě naše simulované modulu lit); a pak normalizuje uvítací zprávu teploty tooa strukturovaná JSON zprávy (včetně připojování hello ID zprávy, nastavení hello vlastnost jestli potřebujeme tootrigger hello teploty výstrahy a tak dále).</span><span class="sxs-lookup"><span data-stu-id="e7fb9-149">It accepts temperature messages from other modules (a hardware module, or in this case our simulated BLE module); and then normalizes hello temperature message in tooa structured JSON message (including appending hello message ID, setting hello property of whether we need tootrigger hello temperature alert, and so on).</span></span>

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

### <a name="printer-module"></a><span data-ttu-id="e7fb9-150">Modul tiskárny</span><span class="sxs-lookup"><span data-stu-id="e7fb9-150">Printer module</span></span>

| <span data-ttu-id="e7fb9-151">Vstup</span><span class="sxs-lookup"><span data-stu-id="e7fb9-151">Input</span></span>                          | <span data-ttu-id="e7fb9-152">Procesor</span><span class="sxs-lookup"><span data-stu-id="e7fb9-152">Processor</span></span> | <span data-ttu-id="e7fb9-153">Výstup</span><span class="sxs-lookup"><span data-stu-id="e7fb9-153">Output</span></span>                     | <span data-ttu-id="e7fb9-154">Zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="e7fb9-154">Source File</span></span>          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| <span data-ttu-id="e7fb9-155">Jakékoli zprávy z ostatních modulů</span><span class="sxs-lookup"><span data-stu-id="e7fb9-155">Any message from other modules</span></span> | <span data-ttu-id="e7fb9-156">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="e7fb9-156">N/A</span></span>       | <span data-ttu-id="e7fb9-157">Protokolování zpráv tooconsole hello</span><span class="sxs-lookup"><span data-stu-id="e7fb9-157">Log hello message tooconsole</span></span> | `PrinterModule.java` |

<span data-ttu-id="e7fb9-158">Toto je jednoduchá, není potřeba vysvětlovat, modul, který výstupy hello přijaté zprávy toohello okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-158">This is a simple, self-explanatory, module which outputs hello received messages toohello terminal window.</span></span>

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a><span data-ttu-id="e7fb9-159">Konfigurace Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="e7fb9-159">Azure IoT Edge configuration</span></span>

<span data-ttu-id="e7fb9-160">posledním krokem Hello před spuštěním moduly hello je tooconfigure hello Azure IoT okraj a tooestablish hello připojení mezi moduly.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-160">hello final step before running hello modules is tooconfigure hello Azure IoT Edge and tooestablish hello connections between modules.</span></span>

<span data-ttu-id="e7fb9-161">Nejprve je třeba toodeclare naše zavaděč Java (od Azure IoT Edge podporuje zavaděče různých jazyků), který může být odkazuje jeho `name` v částech hello i později.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-161">First we need toodeclare our Java loader (since Azure IoT Edge supports loaders of different languages) which could be referenced by its `name` in hello sections afterward.</span></span>

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

<span data-ttu-id="e7fb9-162">Jakmile jsme prohlásí, že je naše zavaděče, bude také potřebujeme toodeclare také naše moduly.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-162">Once we have declared our loaders, we will also need toodeclare our modules as well.</span></span> <span data-ttu-id="e7fb9-163">Podobné zavaděče hello toodeclaring, že lze také odkazovat podle jejich `name` atribut.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-163">Similar toodeclaring hello loaders, they can also be referenced by their `name` attribute.</span></span> <span data-ttu-id="e7fb9-164">Když modul deklarace, potřebujeme toospecify hello zavaděč měli použít (které by se měly hello jeden jsme definovali před) a hello vstupního bodu (by měl být hello normalizovaný název třídy Naše modulu) pro každý modul.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-164">When declaring a module, we need toospecify hello loader it should use (which should be hello one we defined before) and hello entry-point (should be hello normalized class name of our module) for each module.</span></span> <span data-ttu-id="e7fb9-165">Hello `simulated_device` modul je nativní modul, který je součástí hello Azure IoT Edge core runtime balíčku.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-165">hello `simulated_device` module is a native module which is included in hello Azure IoT Edge core runtime package.</span></span> <span data-ttu-id="e7fb9-166">Vždy by měla obsahovat vaše `args` v souboru JSON, i když je hello `null`.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-166">You should always include `args` in hello JSON file even if it is `null`.</span></span>

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

<span data-ttu-id="e7fb9-167">Na konci hello hello konfigurace naváže připojení hello.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-167">At hello end of hello configuration, we establish hello connections.</span></span> <span data-ttu-id="e7fb9-168">Je každé připojení vyjádřená `source` a `sink`.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-168">Each connection is expressed by `source` and `sink`.</span></span> <span data-ttu-id="e7fb9-169">Jejich by měl obě odkazovat modul předem definovaná.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-169">They should both reference a pre-defined module.</span></span> <span data-ttu-id="e7fb9-170">zprávu výstup Hello `source` modulu se předají toohello vstup z `sink` modulu.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-170">hello output message of `source` module will be forwarded toohello input of `sink` module.</span></span>

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

## <a name="running-hello-modules"></a><span data-ttu-id="e7fb9-171">Spuštěné moduly hello</span><span class="sxs-lookup"><span data-stu-id="e7fb9-171">Running hello modules</span></span>

<span data-ttu-id="e7fb9-172">Použití `mvn package` toobuild všechno, co do hello `target/` složky.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-172">Use `mvn package` toobuild everything into hello `target/` folder.</span></span> <span data-ttu-id="e7fb9-173">`mvn clean package`je také vhodné pro čistá sestavení.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-173">`mvn clean package` is also recommended for a clean build.</span></span>

<span data-ttu-id="e7fb9-174">Použití `mvn exec:exec` toorun hello Azure IoT okraj a měli zjistit, zda jsou hello teploty data a všechny vlastnosti hello konzoly na tištěných toohello s pevnou sazbou.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-174">Use `mvn exec:exec` toorun hello Azure IoT Edge and you should observe that hello temperature data and all hello properties are printed toohello console at a fixed rate.</span></span>

<span data-ttu-id="e7fb9-175">Pokud chcete aplikace hello tooterminate, stiskněte klávesu `<Enter>` klíč.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-175">If you want tooterminate hello application, press `<Enter>` key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e7fb9-176">Není doporučeno toouse Ctrl + C tooterminate hello IoT hraniční brány aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-176">It is not recommended toouse Ctrl + C tooterminate hello IoT Edge gateway application.</span></span> <span data-ttu-id="e7fb9-177">Jak to může způsobit hello proces tooterminate neobvyklým způsobem.</span><span class="sxs-lookup"><span data-stu-id="e7fb9-177">As this may cause hello process tooterminate abnormally.</span></span>

