---
title: "aaaProcess události ze služby Event Hubs se Storm - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooprocess data z Azure Event Hubs s topologií C# Storm vytvořena v sadě Visual Studio pomocí hello nástroje HDInsight pro Visual Studio."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 67f9d08c-eea0-401b-952b-db765655dad0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 30cd910d80eba066f283197bcbbaf11145bc5524
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a><span data-ttu-id="bda12-103">Zpracování událostí z Azure Event Hubs se Storm v HDInsight (C#)</span><span class="sxs-lookup"><span data-stu-id="bda12-103">Process events from Azure Event Hubs with Storm on HDInsight (C#)</span></span>

<span data-ttu-id="bda12-104">Zjistěte, jak toowork s Azure Event Hubs z Apache Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bda12-104">Learn how toowork with Azure Event Hubs from Apache Storm on HDInsight.</span></span> <span data-ttu-id="bda12-105">Tento dokument používá C# Storm topologie tooread a zápisu dat z centra Evbent</span><span class="sxs-lookup"><span data-stu-id="bda12-105">This document uses a C# Storm topology tooread and write data from Evbent Hubs</span></span>

> [!NOTE]
> <span data-ttu-id="bda12-106">Java verzi tohoto projektu, naleznete v části [zpracovat události z Azure Event Hubs se Storm v HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="bda12-106">For a Java version of this project, see [Process events from Azure Event Hubs with Storm on HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span></span>

## <a name="scpnet"></a><span data-ttu-id="bda12-107">SCP.NET</span><span class="sxs-lookup"><span data-stu-id="bda12-107">SCP.NET</span></span>

<span data-ttu-id="bda12-108">Hello kroky v tomto dokumentu používají SCP.NET balíčku NuGet, který umožňuje snadno toocreate C# topologií a součásti pro použití s nástrojem Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bda12-108">hello steps in this document use SCP.NET, a NuGet package that makes it easy toocreate C# topologies and components for use with Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bda12-109">Při hello kroky v tomto dokumentu spoléhají na vývojového prostředí systému Windows pomocí sady Visual Studio, zkompilovaný projekt hello může být odeslaná tooa Storm v clusteru HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="bda12-109">While hello steps in this document rely on a Windows development environment with Visual Studio, hello compiled project can be submitted tooa Storm on HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="bda12-110">Clustery se systémem Linux vytvořené po 28 říjnu 2016 se podporují jenom SCP.NET topologie.</span><span class="sxs-lookup"><span data-stu-id="bda12-110">Only Linux-based clusters created after October 28, 2016, support SCP.NET topologies.</span></span>

<span data-ttu-id="bda12-111">HDInsight 3.4 a větší použití Mono toorun C# topologií.</span><span class="sxs-lookup"><span data-stu-id="bda12-111">HDInsight 3.4 and greater use Mono toorun C# topologies.</span></span> <span data-ttu-id="bda12-112">spolupracuje se službou HDInsight 3.6 Hello příkladu v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="bda12-112">hello example used in this document works with HDInsight 3.6.</span></span> <span data-ttu-id="bda12-113">Pokud máte v plánu na vytváření řešení .NET pro HDInsight, zkontrolujte hello [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu potenciální nekompatibility.</span><span class="sxs-lookup"><span data-stu-id="bda12-113">If you plan on creating your own .NET solutions for HDInsight, check hello [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document for potential incompatibilities.</span></span>

### <a name="cluster-versioning"></a><span data-ttu-id="bda12-114">Správa verzí clusteru</span><span class="sxs-lookup"><span data-stu-id="bda12-114">Cluster versioning</span></span>

<span data-ttu-id="bda12-115">Hello balíček Microsoft.SCP.Net.SDK NuGet, které používáte pro svůj projekt musí odpovídat hello hlavní verzi Storm v HDInsight nainstalována.</span><span class="sxs-lookup"><span data-stu-id="bda12-115">hello Microsoft.SCP.Net.SDK NuGet package you use for your project must match hello major version of Storm installed on HDInsight.</span></span> <span data-ttu-id="bda12-116">HDInsight verze 3.5 a 3.6 používat Storm 1.x, musíte použít verzi 1.0.x.x SCP.NET s těchto clusterů.</span><span class="sxs-lookup"><span data-stu-id="bda12-116">HDInsight versions 3.5 and 3.6 use Storm 1.x, so you must use SCP.NET version 1.0.x.x with these clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bda12-117">Příklad Hello v tomto dokumentu očekává HDInsight 3.5 nebo 3,6 clusteru.</span><span class="sxs-lookup"><span data-stu-id="bda12-117">hello example in this document expects an HDInsight 3.5 or 3.6 cluster.</span></span>
>
> <span data-ttu-id="bda12-118">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="bda12-118">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="bda12-119">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="bda12-119">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="bda12-120">Topologie C# musí být také .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="bda12-120">C# topologies must also target .NET 4.5.</span></span>

## <a name="how-toowork-with-event-hubs"></a><span data-ttu-id="bda12-121">Jak toowork službou Event Hubs</span><span class="sxs-lookup"><span data-stu-id="bda12-121">How toowork with Event Hubs</span></span>

<span data-ttu-id="bda12-122">Společnost Microsoft poskytuje sadu komponent v jazyce Java, které se dají použít toocommunicate službou Event Hubs od topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="bda12-122">Microsoft provides a set of Java components that can be used toocommunicate with Event Hubs from a Storm topology.</span></span> <span data-ttu-id="bda12-123">Můžete najít hello Java archivu (JAR) soubor, který obsahuje HDInsight 3.6 kompatibilní verze těchto součástí v [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="bda12-123">You can find hello Java archive (JAR) file that contains an HDInsight 3.6 compatible version of these components at [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bda12-124">Při hello součásti jsou napsané v jazyce Java, je můžete snadno použít z topologie C#.</span><span class="sxs-lookup"><span data-stu-id="bda12-124">While hello components are written in Java, you can easily use them from a C# topology.</span></span>

<span data-ttu-id="bda12-125">v tomto příkladu se používají Hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="bda12-125">hello following components are used in this example:</span></span>

* <span data-ttu-id="bda12-126">__EventHubSpout__: čte data ze služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="bda12-126">__EventHubSpout__: Reads data from Event Hubs.</span></span>
* <span data-ttu-id="bda12-127">__EventHubBolt__: zapíše tooEvent datového centra.</span><span class="sxs-lookup"><span data-stu-id="bda12-127">__EventHubBolt__: Writes data tooEvent Hubs.</span></span>
* <span data-ttu-id="bda12-128">__EventHubSpoutConfig__: používá tooconfigure EventHubSpout.</span><span class="sxs-lookup"><span data-stu-id="bda12-128">__EventHubSpoutConfig__: Used tooconfigure EventHubSpout.</span></span>
* <span data-ttu-id="bda12-129">__EventHubBoltConfig__: používá tooconfigure EventHubBolt.</span><span class="sxs-lookup"><span data-stu-id="bda12-129">__EventHubBoltConfig__: Used tooconfigure EventHubBolt.</span></span>

### <a name="example-spout-usage"></a><span data-ttu-id="bda12-130">Příklad použití funkcí spout</span><span class="sxs-lookup"><span data-stu-id="bda12-130">Example spout usage</span></span>

<span data-ttu-id="bda12-131">SCP.NET poskytuje metody pro přidání EventHubSpout tooyour topologie.</span><span class="sxs-lookup"><span data-stu-id="bda12-131">SCP.NET provides methods for adding an EventHubSpout tooyour topology.</span></span> <span data-ttu-id="bda12-132">Tyto metody umožňují snadnější tooadd spout než použití hello obecné metody pro přidání součásti Java.</span><span class="sxs-lookup"><span data-stu-id="bda12-132">These methods make it easier tooadd a spout than using hello generic methods for adding a Java component.</span></span> <span data-ttu-id="bda12-133">Hello následující příklad ukazuje, jak toocreate spout pomocí hello __SetEventHubSpout__ a **EventHubSpoutConfig** metody poskytované SCP.NET:</span><span class="sxs-lookup"><span data-stu-id="bda12-133">hello following example demonstrates how toocreate a spout by using hello __SetEventHubSpout__ and **EventHubSpoutConfig** methods provided by SCP.NET:</span></span>

```csharp
 topologyBuilder.SetEventHubSpout(
    "EventHubSpout",
    new EventHubSpoutConfig(
        ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"],
        ConfigurationManager.AppSettings["EventHubSharedAccessKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        ConfigurationManager.AppSettings["EventHubEntityPath"],
        eventHubPartitions),
    eventHubPartitions);
```

<span data-ttu-id="bda12-134">Hello předchozí příklad vytvoří novou funkcí spout součást s názvem __EventHubSpout__a nakonfiguruje ho toocommunicate pomocí centra událostí.</span><span class="sxs-lookup"><span data-stu-id="bda12-134">hello previous example creates a new spout component named __EventHubSpout__, and configures it toocommunicate with an event hub.</span></span> <span data-ttu-id="bda12-135">Hello paralelismus nápovědu pro součást hello se nastavuje toohello počet oddílů v Centru událostí hello.</span><span class="sxs-lookup"><span data-stu-id="bda12-135">hello parallelism hint for hello component is set toohello number of partitions in hello event hub.</span></span> <span data-ttu-id="bda12-136">Toto nastavení umožňuje Storm toocreate instance hello součásti pro každý oddíl.</span><span class="sxs-lookup"><span data-stu-id="bda12-136">This setting allows Storm toocreate an instance of hello component for each partition.</span></span>

### <a name="example-bolt-usage"></a><span data-ttu-id="bda12-137">Příklad použití bolt</span><span class="sxs-lookup"><span data-stu-id="bda12-137">Example bolt usage</span></span>

<span data-ttu-id="bda12-138">Použití hello **JavaComponmentConstructor** toocreate metoda instanci hello bolt.</span><span class="sxs-lookup"><span data-stu-id="bda12-138">Use hello **JavaComponmentConstructor** method toocreate an instance of hello bolt.</span></span> <span data-ttu-id="bda12-139">Hello následující příklad ukazuje, jak toocreate a nakonfigurujte novou instanci třídy hello **EventHubBolt**:</span><span class="sxs-lookup"><span data-stu-id="bda12-139">hello following example demonstrates how toocreate and configure a new instance of hello **EventHubBolt**:</span></span>

```csharp
// Java construcvtor for hello Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set hello bolt toosubscribe toodata from hello spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]
> <span data-ttu-id="bda12-140">Tento příklad používá Clojure výrazu předaného jako řetězec, místo použití **JavaComponentConstructor** toocreate **EventHubBoltConfig**, jako příklad spout hello.</span><span class="sxs-lookup"><span data-stu-id="bda12-140">This example uses a Clojure expression passed as a string, instead of using **JavaComponentConstructor** toocreate an **EventHubBoltConfig**, as hello spout example did.</span></span> <span data-ttu-id="bda12-141">Buď metoda funguje.</span><span class="sxs-lookup"><span data-stu-id="bda12-141">Either method works.</span></span> <span data-ttu-id="bda12-142">Použijte hello metodu, která funguje nejlepší tooyou.</span><span class="sxs-lookup"><span data-stu-id="bda12-142">Use hello method that feels best tooyou.</span></span>

## <a name="download-hello-completed-project"></a><span data-ttu-id="bda12-143">Stáhněte si projekt hello byla dokončena</span><span class="sxs-lookup"><span data-stu-id="bda12-143">Download hello completed project</span></span>

<span data-ttu-id="bda12-144">Můžete stáhnout úplnou verzi vytvořených v tomto kurzu z projektů hello [Githubu](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="bda12-144">You can download a complete version of hello project created in this tutorial from [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span> <span data-ttu-id="bda12-145">Však stále potřebujete tooprovide konfigurační nastavení pomocí hello postupu v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bda12-145">However, you still need tooprovide configuration settings by following hello steps in this tutorial.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="bda12-146">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bda12-146">Prerequisites</span></span>

* <span data-ttu-id="bda12-147">[Apache Storm v HDInsight clusteru verze 3.5 nebo 3.6](hdinsight-apache-storm-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bda12-147">An [Apache Storm on HDInsight cluster version 3.5 or 3.6](hdinsight-apache-storm-tutorial-get-started.md).</span></span>

    > [!WARNING]
    > <span data-ttu-id="bda12-148">Příklad Hello v tomto dokumentu vyžaduje Storm v HDInsight verze 3.5 nebo 3.6.</span><span class="sxs-lookup"><span data-stu-id="bda12-148">hello example used in this document requires Storm on HDInsight version 3.5 or 3.6.</span></span> <span data-ttu-id="bda12-149">To nefunguje se staršími verzemi HDInsight, z důvodu změny názvu třídy toobreaking.</span><span class="sxs-lookup"><span data-stu-id="bda12-149">This does not work with older versions of HDInsight, due toobreaking class name changes.</span></span> <span data-ttu-id="bda12-150">Verzi tohoto příkladu, který funguje s starší clustery, naleznete v části [Githubu](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span><span class="sxs-lookup"><span data-stu-id="bda12-150">For a version of this example that works with older clusters, see [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span></span>

* <span data-ttu-id="bda12-151">[Centra událostí Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="bda12-151">An [Azure event hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

* <span data-ttu-id="bda12-152">Hello [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="bda12-152">hello [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>

* <span data-ttu-id="bda12-153">Hello [nástroje HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bda12-153">hello [HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

* <span data-ttu-id="bda12-154">Java JDK 1.8 nebo později na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="bda12-154">Java JDK 1.8 or later on your development environment.</span></span> <span data-ttu-id="bda12-155">Stahování JDK jsou k dispozici na [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="bda12-155">JDK downloads are available from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>

  * <span data-ttu-id="bda12-156">Hello **JAVA_HOME** prostředí proměnné musí bodu toohello adresář, který obsahuje Java.</span><span class="sxs-lookup"><span data-stu-id="bda12-156">hello **JAVA_HOME** environment variable must point toohello directory that contains Java.</span></span>
  * <span data-ttu-id="bda12-157">Hello **%JAVA_HOME%/bin** adresář se musí nacházet v cestě hello.</span><span class="sxs-lookup"><span data-stu-id="bda12-157">hello **%JAVA_HOME%/bin** directory must be in hello path.</span></span>

## <a name="download-hello-event-hubs-components"></a><span data-ttu-id="bda12-158">Stažení komponent hello Event Hubs</span><span class="sxs-lookup"><span data-stu-id="bda12-158">Download hello Event Hubs components</span></span>

<span data-ttu-id="bda12-159">Stažení hello Event Hubs spout a funkce bolt z komponenty [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="bda12-159">Download hello Event Hubs spout and bolt component from [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

<span data-ttu-id="bda12-160">Vytvořte adresář s názvem `eventhubspout`a uložte soubor hello do adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="bda12-160">Create a directory named `eventhubspout`, and save hello file into hello directory.</span></span>

## <a name="configure-event-hubs"></a><span data-ttu-id="bda12-161">Konfigurace služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="bda12-161">Configure Event Hubs</span></span>

<span data-ttu-id="bda12-162">Event Hubs je hello zdroj dat pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="bda12-162">Event Hubs is hello data source for this example.</span></span> <span data-ttu-id="bda12-163">Použijte hello informace v části "Vytvoření centra událostí" hello z [Začínáme se službou Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="bda12-163">Use hello information in hello "Create an event hub" section of [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

1. <span data-ttu-id="bda12-164">Po vytvoření centra událostí hello zobrazit hello **EventHub** okno v hello Azure portálu a vyberte možnost **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="bda12-164">After hello event hub has been created, view hello **EventHub** blade in hello Azure portal, and select **Shared access policies**.</span></span> <span data-ttu-id="bda12-165">Vyberte **+ přidat** tooadd hello následující zásady:</span><span class="sxs-lookup"><span data-stu-id="bda12-165">Select **+ Add** tooadd hello following policies:</span></span>

   | <span data-ttu-id="bda12-166">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="bda12-166">Name</span></span> | <span data-ttu-id="bda12-167">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="bda12-167">Permissions</span></span> |
   | --- | --- |
   | <span data-ttu-id="bda12-168">Modul pro zápis</span><span class="sxs-lookup"><span data-stu-id="bda12-168">writer</span></span> |<span data-ttu-id="bda12-169">Odeslat</span><span class="sxs-lookup"><span data-stu-id="bda12-169">Send</span></span> |
   | <span data-ttu-id="bda12-170">Čtecí modul</span><span class="sxs-lookup"><span data-stu-id="bda12-170">reader</span></span> |<span data-ttu-id="bda12-171">Naslouchání</span><span class="sxs-lookup"><span data-stu-id="bda12-171">Listen</span></span> |

    ![Okno zásady přístupu – snímek obrazovky sdílené složky](./media/hdinsight-storm-develop-csharp-event-hub-topology/sas.png)

2. <span data-ttu-id="bda12-173">Vyberte hello **čtečky** a **zapisovače** zásady.</span><span class="sxs-lookup"><span data-stu-id="bda12-173">Select hello **reader** and **writer** policies.</span></span> <span data-ttu-id="bda12-174">Zkopírujte a uložte hodnotu primárního klíče hello pro obě zásady, jako jsou tyto hodnoty použít později.</span><span class="sxs-lookup"><span data-stu-id="bda12-174">Copy and save hello primary key value for both policies, as these values are used later.</span></span>

## <a name="configure-hello-eventhubwriter"></a><span data-ttu-id="bda12-175">Konfigurace hello EventHubWriter</span><span class="sxs-lookup"><span data-stu-id="bda12-175">Configure hello EventHubWriter</span></span>

1. <span data-ttu-id="bda12-176">Pokud jste ještě nenainstalovali hello nejnovější verzi hello nástroje HDInsight pro Visual Studio, najdete v části [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bda12-176">If you have not already installed hello latest version of hello HDInsight tools for Visual Studio, see [Get started using HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="bda12-177">Stáhněte si řešení hello z [eventhub. storm hybridní](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="bda12-177">Download hello solution from [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span>

3. <span data-ttu-id="bda12-178">V hello **EventHubWriter** projekt, otevřete hello **App.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="bda12-178">In hello **EventHubWriter** project, open hello **App.config** file.</span></span> <span data-ttu-id="bda12-179">Pomocí informací hello z centra událostí hello nakonfigurované starší toofill v hello hodnotu hello následující klíče:</span><span class="sxs-lookup"><span data-stu-id="bda12-179">Use hello information from hello event hub that you configured earlier toofill in hello value for hello following keys:</span></span>

   | <span data-ttu-id="bda12-180">Klíč</span><span class="sxs-lookup"><span data-stu-id="bda12-180">Key</span></span> | <span data-ttu-id="bda12-181">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bda12-181">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="bda12-182">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="bda12-182">EventHubPolicyName</span></span> |<span data-ttu-id="bda12-183">Zapisovač (Pokud jste použili jiný název pro zásady hello s *odeslat* oprávnění, použijte místo toho.)</span><span class="sxs-lookup"><span data-stu-id="bda12-183">writer (If you used a different name for hello policy with *Send* permission, use it instead.)</span></span> |
   | <span data-ttu-id="bda12-184">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="bda12-184">EventHubPolicyKey</span></span> |<span data-ttu-id="bda12-185">Hello klíč pro zásady zapisovače hello.</span><span class="sxs-lookup"><span data-stu-id="bda12-185">hello key for hello writer policy.</span></span> |
   | <span data-ttu-id="bda12-186">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="bda12-186">EventHubNamespace</span></span> |<span data-ttu-id="bda12-187">Hello obor názvů, který obsahuje Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="bda12-187">hello namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="bda12-188">EventHubName</span><span class="sxs-lookup"><span data-stu-id="bda12-188">EventHubName</span></span> |<span data-ttu-id="bda12-189">Název centra událostí.</span><span class="sxs-lookup"><span data-stu-id="bda12-189">Your event hub name.</span></span> |
   | <span data-ttu-id="bda12-190">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="bda12-190">EventHubPartitionCount</span></span> |<span data-ttu-id="bda12-191">Hello počet oddílů v Centru událostí.</span><span class="sxs-lookup"><span data-stu-id="bda12-191">hello number of partitions in your event hub.</span></span> |

4. <span data-ttu-id="bda12-192">Uložte a zavřete hello **App.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="bda12-192">Save and close hello **App.config** file.</span></span>

## <a name="configure-hello-eventhubreader"></a><span data-ttu-id="bda12-193">Konfigurace hello EventHubReader</span><span class="sxs-lookup"><span data-stu-id="bda12-193">Configure hello EventHubReader</span></span>

1. <span data-ttu-id="bda12-194">Otevřete hello **EventHubReader** projektu.</span><span class="sxs-lookup"><span data-stu-id="bda12-194">Open hello **EventHubReader** project.</span></span>

2. <span data-ttu-id="bda12-195">Otevřete hello **App.config** souboru hello **EventHubReader**.</span><span class="sxs-lookup"><span data-stu-id="bda12-195">Open hello **App.config** file for hello **EventHubReader**.</span></span> <span data-ttu-id="bda12-196">Pomocí informací hello z centra událostí hello nakonfigurované starší toofill v hello hodnotu hello následující klíče:</span><span class="sxs-lookup"><span data-stu-id="bda12-196">Use hello information from hello event hub that you configured earlier toofill in hello value for hello following keys:</span></span>

   | <span data-ttu-id="bda12-197">Klíč</span><span class="sxs-lookup"><span data-stu-id="bda12-197">Key</span></span> | <span data-ttu-id="bda12-198">Hodnota</span><span class="sxs-lookup"><span data-stu-id="bda12-198">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="bda12-199">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="bda12-199">EventHubPolicyName</span></span> |<span data-ttu-id="bda12-200">Čtečka (Pokud jste použili jiný název pro zásady hello s *naslouchání* oprávnění, použijte místo toho.)</span><span class="sxs-lookup"><span data-stu-id="bda12-200">reader (If you used a different name for hello policy with *listen* permission, use it instead.)</span></span> |
   | <span data-ttu-id="bda12-201">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="bda12-201">EventHubPolicyKey</span></span> |<span data-ttu-id="bda12-202">Hello klíč pro zásady čtečky hello.</span><span class="sxs-lookup"><span data-stu-id="bda12-202">hello key for hello reader policy.</span></span> |
   | <span data-ttu-id="bda12-203">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="bda12-203">EventHubNamespace</span></span> |<span data-ttu-id="bda12-204">Hello obor názvů, který obsahuje Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="bda12-204">hello namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="bda12-205">EventHubName</span><span class="sxs-lookup"><span data-stu-id="bda12-205">EventHubName</span></span> |<span data-ttu-id="bda12-206">Název centra událostí.</span><span class="sxs-lookup"><span data-stu-id="bda12-206">Your event hub name.</span></span> |
   | <span data-ttu-id="bda12-207">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="bda12-207">EventHubPartitionCount</span></span> |<span data-ttu-id="bda12-208">Hello počet oddílů v Centru událostí.</span><span class="sxs-lookup"><span data-stu-id="bda12-208">hello number of partitions in your event hub.</span></span> |

3. <span data-ttu-id="bda12-209">Uložte a zavřete hello **App.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="bda12-209">Save and close hello **App.config** file.</span></span>

## <a name="deploy-hello-topologies"></a><span data-ttu-id="bda12-210">Hello topologie nasazení</span><span class="sxs-lookup"><span data-stu-id="bda12-210">Deploy hello topologies</span></span>

1. <span data-ttu-id="bda12-211">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **EventHubReader** projektu a vyberte **odeslání tooStorm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="bda12-211">From **Solution Explorer**, right-click hello **EventHubReader** project, and select **Submit tooStorm on HDInsight**.</span></span>

    ![Snímek obrazovky řešení Explorer, se tooStorm odeslání v HDInsight zvýrazněná](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. <span data-ttu-id="bda12-213">Na hello **odeslání topologie** dialogové okno, vyberte vaše **Storm Cluster**.</span><span class="sxs-lookup"><span data-stu-id="bda12-213">On hello **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="bda12-214">Rozbalte položku **další konfigurace**, vyberte **cesty k souborům Java**, vyberte **...** a vyberte hello adresář, který obsahuje soubor JAR hello, který jste předtím stáhli.</span><span class="sxs-lookup"><span data-stu-id="bda12-214">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select hello directory that contains hello JAR file that you downloaded earlier.</span></span> <span data-ttu-id="bda12-215">Nakonec klikněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="bda12-215">Finally, click **Submit**.</span></span>

    ![Dialogové okno snímek obrazovky odeslání topologie](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. <span data-ttu-id="bda12-217">Při odeslání hello topologie hello **prohlížeč topologie Storm** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="bda12-217">When hello topology has been submitted, hello **Storm Topologies Viewer** appears.</span></span> <span data-ttu-id="bda12-218">tooview informací o topologii hello, vyberte hello **EventHubReader** topologie v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="bda12-218">tooview information about hello topology, select hello **EventHubReader** topology in hello left pane.</span></span>

    ![Snímek obrazovky prohlížeč topologie Storm](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. <span data-ttu-id="bda12-220">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **EventHubWriter** projektu a vyberte **odeslání tooStorm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="bda12-220">From **Solution Explorer**, right-click hello **EventHubWriter** project, and select **Submit tooStorm on HDInsight**.</span></span>

5. <span data-ttu-id="bda12-221">Na hello **odeslání topologie** dialogové okno, vyberte vaše **Storm Cluster**.</span><span class="sxs-lookup"><span data-stu-id="bda12-221">On hello **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="bda12-222">Rozbalte položku **další konfigurace**, vyberte **cesty k souborům Java**, vyberte **...** , a vyberte hello adresář, který obsahuje soubor JAR hello jste předtím stáhli.</span><span class="sxs-lookup"><span data-stu-id="bda12-222">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select hello directory that contains hello JAR file you downloaded earlier.</span></span> <span data-ttu-id="bda12-223">Nakonec klikněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="bda12-223">Finally, click **Submit**.</span></span>

6. <span data-ttu-id="bda12-224">Pokud byla odeslána hello topologie, aktualizujte seznam topologie hello v hello **prohlížeč topologie Storm** tooverify obou topologií spuštěných v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="bda12-224">When hello topology has been submitted, refresh hello topology list in hello **Storm Topologies Viewer** tooverify that both topologies are running on hello cluster.</span></span>

7. <span data-ttu-id="bda12-225">V **prohlížeč topologie Storm**, vyberte hello **EventHubReader** topologie.</span><span class="sxs-lookup"><span data-stu-id="bda12-225">In **Storm Topologies Viewer**, select hello **EventHubReader** topology.</span></span>

8. <span data-ttu-id="bda12-226">součást hello tooopen shrnutí hello bolt, dvakrát klikněte na hello **LogBolt** součásti v diagramu hello.</span><span class="sxs-lookup"><span data-stu-id="bda12-226">tooopen hello component summary for hello bolt, double-click hello **LogBolt** component in hello diagram.</span></span>

9. <span data-ttu-id="bda12-227">V hello **vykonavatelů** vyberte jeden z odkazů hello v hello **Port** sloupce.</span><span class="sxs-lookup"><span data-stu-id="bda12-227">In hello **Executors** section, select one of hello links in hello **Port** column.</span></span> <span data-ttu-id="bda12-228">Zobrazí se informace protokolována komponentou hello.</span><span class="sxs-lookup"><span data-stu-id="bda12-228">This displays information logged by hello component.</span></span> <span data-ttu-id="bda12-229">Hello protokolovat informace jsou podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="bda12-229">hello logged information is similar toohello following text:</span></span>

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-hello-topologies"></a><span data-ttu-id="bda12-230">Zastavení topologie hello</span><span class="sxs-lookup"><span data-stu-id="bda12-230">Stop hello topologies</span></span>

<span data-ttu-id="bda12-231">topologie hello toostop, vyberte každý topologii v hello **prohlížeč topologie Storm**, pak klikněte na tlačítko **Kill**.</span><span class="sxs-lookup"><span data-stu-id="bda12-231">toostop hello topologies, select each topology in hello **Storm Topology Viewer**, then click **Kill**.</span></span>

![Snímek obrazovky systému Storm topologie prohlížeč, s tlačítkem Kill](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a><span data-ttu-id="bda12-233">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="bda12-233">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="bda12-234">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bda12-234">Next steps</span></span>

<span data-ttu-id="bda12-235">V tomto dokumentu jste zjistili, jak toouse hello Java Event Hubs spout a funkce bolt z C# topologie toowork s daty v Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="bda12-235">In this document, you have learned how toouse hello Java Event Hubs spout and bolt from a C# topology toowork with data in Azure Event Hubs.</span></span> <span data-ttu-id="bda12-236">toolearn Další informace o vytváření topologie C#, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="bda12-236">toolearn more about creating C# topologies, see hello following:</span></span>

* [<span data-ttu-id="bda12-237">Vývoj topologie C# pro Apache Storm v HDInsight pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bda12-237">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="bda12-238">Průvodce programováním spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="bda12-238">SCP programming guide</span></span>](hdinsight-storm-scp-programming-guide.md)
* [<span data-ttu-id="bda12-239">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="bda12-239">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
