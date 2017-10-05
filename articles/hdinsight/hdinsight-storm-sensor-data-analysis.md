---
title: "Analýza dat snímačů pomocí Apache Storm a HBase | Microsoft Docs"
description: "Zjistěte, jak se připojit k Apache Storm s virtuální sítí. Používat Storm pro zpracování dat snímačů z centra událostí s HBase a vizualizovat s D3.js."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/09/2017
ms.author: larryfr
ms.openlocfilehash: 0d1cc959c87bd64ed728f8b56c9b9156fa492a8b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a><span data-ttu-id="d8df0-104">Analýza dat snímačů s Apache Storm, centra událostí a HBase v HDInsight (Hadoop)</span><span class="sxs-lookup"><span data-stu-id="d8df0-104">Analyze sensor data with Apache Storm, Event Hub, and HBase in HDInsight (Hadoop)</span></span>

<span data-ttu-id="d8df0-105">Další informace o použití Apache Storm v HDInsight ke zpracování dat snímačů z centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="d8df0-105">Learn how to use Apache Storm on HDInsight to process sensor data from Azure Event Hub.</span></span> <span data-ttu-id="d8df0-106">Data jsou pak uloženy do Apache HBase v HDInsight a detekují pomocí D3.js.</span><span class="sxs-lookup"><span data-stu-id="d8df0-106">The data is then stored into Apache HBase on HDInsight, and visualized using D3.js.</span></span>

<span data-ttu-id="d8df0-107">Šablony Azure Resource Manager v tomto dokumentu ukazuje, jak vytvořit několik prostředků Azure ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="d8df0-107">The Azure Resource Manager template used in this document demonstrates how to create multiple Azure resources in a resource group.</span></span> <span data-ttu-id="d8df0-108">Šablona vytvoří virtuální síť Azure, dva clustery HDInsight (Storm a HBase) a webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="d8df0-108">The template creates an Azure Virtual Network, two HDInsight clusters (Storm and HBase) and an Azure Web App.</span></span> <span data-ttu-id="d8df0-109">Implementace node.js řídicího panelu webu v reálném čase je automaticky nasadí do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8df0-109">A node.js implementation of a real-time web dashboard is automatically deployed to the web app.</span></span>

> [!NOTE]
> <span data-ttu-id="d8df0-110">Informace v tomto dokumentu a v příkladu v tomto dokumentu vyžadují HDInsight verze 3.6.</span><span class="sxs-lookup"><span data-stu-id="d8df0-110">The information in this document and example in this document require HDInsight version 3.6.</span></span>
>
> <span data-ttu-id="d8df0-111">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="d8df0-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d8df0-112">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d8df0-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8df0-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d8df0-113">Prerequisites</span></span>

* <span data-ttu-id="d8df0-114">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="d8df0-114">An Azure subscription.</span></span>
* <span data-ttu-id="d8df0-115">[Node.js](http://nodejs.org/): slouží k náhledu řídicího panelu webové místně na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="d8df0-115">[Node.js](http://nodejs.org/): Used to preview the web dashboard locally on your development environment.</span></span>
* <span data-ttu-id="d8df0-116">[Java a JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): použité k jejich vývoji topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="d8df0-116">[Java and the JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): Used to develop the Storm topology.</span></span>
* <span data-ttu-id="d8df0-117">[Maven](http://maven.apache.org/what-is-maven.html): použít k sestavení a kompilaci projektu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-117">[Maven](http://maven.apache.org/what-is-maven.html): Used to build and compile the project.</span></span>
* <span data-ttu-id="d8df0-118">[Git](http://git-scm.com/): používá ke stahování projektu z Githubu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-118">[Git](http://git-scm.com/): Used to download the project from GitHub.</span></span>
* <span data-ttu-id="d8df0-119">**SSH** klienta: používá k připojení ke clusterům HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="d8df0-119">An **SSH** client: Used to connect to the Linux-based HDInsight clusters.</span></span> <span data-ttu-id="d8df0-120">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="d8df0-120">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>


> [!IMPORTANT]
> <span data-ttu-id="d8df0-121">Není nutné stávajícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8df0-121">You do not need an existing HDInsight cluster.</span></span> <span data-ttu-id="d8df0-122">Kroky v tomto dokumentu vytvořte následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="d8df0-122">The steps in this document create the following resources:</span></span>
> 
> * <span data-ttu-id="d8df0-123">Virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="d8df0-123">An Azure Virtual Network</span></span>
> * <span data-ttu-id="d8df0-124">Storm v clusteru HDInsight (systémem Linux dva uzly pracovního procesu)</span><span class="sxs-lookup"><span data-stu-id="d8df0-124">A Storm on HDInsight cluster (Linux-based, two worker nodes)</span></span>
> * <span data-ttu-id="d8df0-125">Na clusteru HDInsight HBase (systémem Linux dva uzly pracovního procesu)</span><span class="sxs-lookup"><span data-stu-id="d8df0-125">An HBase on HDInsight cluster (Linux-based, two worker nodes)</span></span>
> * <span data-ttu-id="d8df0-126">Webové aplikace Azure, který je hostitelem webové řídicí panel</span><span class="sxs-lookup"><span data-stu-id="d8df0-126">An Azure Web App that hosts the web dashboard</span></span>

## <a name="architecture"></a><span data-ttu-id="d8df0-127">Architektura</span><span class="sxs-lookup"><span data-stu-id="d8df0-127">Architecture</span></span>

![diagram architektury](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

<span data-ttu-id="d8df0-129">V tomto příkladu se skládá z následujících součástí:</span><span class="sxs-lookup"><span data-stu-id="d8df0-129">This example consists of the following components:</span></span>

* <span data-ttu-id="d8df0-130">**Azure Event Hubs**: obsahuje data, která se shromažďují ze senzorů.</span><span class="sxs-lookup"><span data-stu-id="d8df0-130">**Azure Event Hubs**: Contains data that is collected from sensors.</span></span>
* <span data-ttu-id="d8df0-131">**Storm v HDInsight**: poskytuje v reálném čase zpracování dat z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="d8df0-131">**Storm on HDInsight**: Provides real-time processing of data from Event Hub.</span></span>
* <span data-ttu-id="d8df0-132">**HBase v HDInsight**: po zpracování pomocí Storm poskytuje trvalé úložiště dat typu NoSQL pro data.</span><span class="sxs-lookup"><span data-stu-id="d8df0-132">**HBase on HDInsight**: Provides a persistent NoSQL data store for data after it has been processed by Storm.</span></span>
* <span data-ttu-id="d8df0-133">**Služba Azure virtuální sítě**: umožňuje zabezpečené komunikaci mezi Storm v HDInsight a HBase v HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="d8df0-133">**Azure Virtual Network service**: Enables secure communications between the Storm on HDInsight and HBase on HDInsight clusters.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="d8df0-134">Virtuální sítě je potřeba při použití Java HBase klientského rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d8df0-134">A virtual network is required when using the Java HBase client API.</span></span> <span data-ttu-id="d8df0-135">Ho nebude vystavena, přes bránu veřejné pro clustery HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-135">It is not exposed over the public gateway for HBase clusters.</span></span> <span data-ttu-id="d8df0-136">Instalace clustery HBase a Storm do stejné virtuální síti umožňuje Storm cluster (nebo všechny ostatní systémy ve virtuální síti) přímý přístup k HBase pomocí rozhraní API klienta.</span><span class="sxs-lookup"><span data-stu-id="d8df0-136">Installing HBase and Storm clusters into the same virtual network allows the Storm cluster (or any other system on the virtual network) to directly access HBase using client API.</span></span>

* <span data-ttu-id="d8df0-137">**Řídicí panel webu**: příklad řídicího panelu, který grafy data v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-137">**Dashboard website**: An example dashboard that charts data in real time.</span></span>
  
  * <span data-ttu-id="d8df0-138">Web je implementované v Node.js.</span><span class="sxs-lookup"><span data-stu-id="d8df0-138">The website is implemented in Node.js.</span></span>
  * <span data-ttu-id="d8df0-139">[Socket.IO](http://socket.io/) se používá pro komunikaci v reálném čase mezi topologie Storm a webu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-139">[Socket.io](http://socket.io/) is used for real-time communication between the Storm topology and the website.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="d8df0-140">Pro komunikaci pomocí Socket.io je podrobností implementace.</span><span class="sxs-lookup"><span data-stu-id="d8df0-140">Using Socket.io for communication is an implementation detail.</span></span> <span data-ttu-id="d8df0-141">Můžete použít libovolnou architekturu komunikace, jako je například nezpracovaná Websocket nebo SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8df0-141">You can use any communications framework, such as raw WebSockets or SignalR.</span></span>

  * <span data-ttu-id="d8df0-142">[D3.js](http://d3js.org/) se používá k graf data, která se posílá webu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-142">[D3.js](http://d3js.org/) is used to graph the data that is sent to the website.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8df0-143">Dva clustery jsou povinné, protože neexistuje žádná podporovaná metoda vytvoření jednoho clusteru HDInsight Storm a HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-143">Two clusters are required, as there is no supported method to create one HDInsight cluster for both Storm and HBase.</span></span>

<span data-ttu-id="d8df0-144">Topologie čte data z centra událostí pomocí [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) třídy a zápisu dat do HBase pomocí [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) třídy.</span><span class="sxs-lookup"><span data-stu-id="d8df0-144">The topology reads data from Event Hub by using the [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) class, and writes data into HBase using the [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) class.</span></span> <span data-ttu-id="d8df0-145">Komunikace s webem se dá udělat pomocí [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).</span><span class="sxs-lookup"><span data-stu-id="d8df0-145">Communication with the website is accomplished by using [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java).</span></span>

<span data-ttu-id="d8df0-146">Následující diagram popisuje rozložení topologie:</span><span class="sxs-lookup"><span data-stu-id="d8df0-146">The following diagram explains the layout of the topology:</span></span>

![diagram topologie](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> <span data-ttu-id="d8df0-148">Tento diagram je zjednodušený přehled topologie.</span><span class="sxs-lookup"><span data-stu-id="d8df0-148">This diagram is a simplified view of the topology.</span></span> <span data-ttu-id="d8df0-149">Pro každý oddíl v Centru událostí je vytvořena instance jednotlivých součástí.</span><span class="sxs-lookup"><span data-stu-id="d8df0-149">An instance of each component is created for each partition in your Event Hub.</span></span> <span data-ttu-id="d8df0-150">Tyto instance jsou rozdělené mezi uzly v clusteru a data se směruje mezi nimi takto:</span><span class="sxs-lookup"><span data-stu-id="d8df0-150">These instances are distributed across the nodes in the cluster, and data is routed between them as follows:</span></span>
> 
> * <span data-ttu-id="d8df0-151">Data z spout pro Analyzátor je skupinu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="d8df0-151">Data from the spout to the parser is load balanced.</span></span>
> * <span data-ttu-id="d8df0-152">Data z analyzátor na řídicí panel a HBase se seskupují podle ID zařízení tak, aby zprávy ze stejného zařízení vždy tok stejné komponenty.</span><span class="sxs-lookup"><span data-stu-id="d8df0-152">Data from the parser to the Dashboard and HBase is grouped by Device ID, so that messages from the same device always flow to the same component.</span></span>

### <a name="topology-components"></a><span data-ttu-id="d8df0-153">Topologie součásti</span><span class="sxs-lookup"><span data-stu-id="d8df0-153">Topology components</span></span>

* <span data-ttu-id="d8df0-154">**Event Hub Spout**: spout je zadaný jako součást Apache Storm verze 0.10.0 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="d8df0-154">**Event Hub Spout**: The spout is provided as part of Apache Storm version 0.10.0 and higher.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="d8df0-155">Spout centra událostí použité v tomto příkladu vyžaduje Storm v HDInsight clusteru verze 3.5 nebo 3.6.</span><span class="sxs-lookup"><span data-stu-id="d8df0-155">The Event Hub spout used in this example requires a Storm on HDInsight cluster version 3.5 or 3.6.</span></span>

* <span data-ttu-id="d8df0-156">**ParserBolt.java**: data, která je vysílaných spout je nezpracovaném formátu JSON a současně je vygenerované příležitostně více než jednu událost.</span><span class="sxs-lookup"><span data-stu-id="d8df0-156">**ParserBolt.java**: The data that is emitted by the spout is raw JSON, and occasionally more than one event is emitted at a time.</span></span> <span data-ttu-id="d8df0-157">Touto funkcí bolt čte data vysílaných spout a analyzuje zpráva JSON.</span><span class="sxs-lookup"><span data-stu-id="d8df0-157">This bolt reads the data emitted by the spout and parses the JSON message.</span></span> <span data-ttu-id="d8df0-158">Bolt pak vysílá data jako řazené kolekce členů, která obsahuje více polí.</span><span class="sxs-lookup"><span data-stu-id="d8df0-158">The bolt then emits the data as a tuple that contains multiple fields.</span></span>
* <span data-ttu-id="d8df0-159">**DashboardBolt.java**: Tato součást ukazuje, jak používat k odesílání dat v reálném čase do řídicího panelu webové Socket.io klientské knihovny pro jazyk Java.</span><span class="sxs-lookup"><span data-stu-id="d8df0-159">**DashboardBolt.java**: This component demonstrates how to use the Socket.io client library for Java to send data in real time to the web dashboard.</span></span>
* <span data-ttu-id="d8df0-160">**Ne hbase.yaml**: definici topologie používá při spuštění v místním režimu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-160">**no-hbase.yaml**: The topology definition used when running in local mode.</span></span> <span data-ttu-id="d8df0-161">HBase součásti nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="d8df0-161">It does not use HBase components.</span></span>
* <span data-ttu-id="d8df0-162">**s hbase.yaml**: definici topologie používá při spuštění topologii v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8df0-162">**with-hbase.yaml**: The topology definition used when running the topology on the cluster.</span></span> <span data-ttu-id="d8df0-163">Používá HBase součásti.</span><span class="sxs-lookup"><span data-stu-id="d8df0-163">It does use HBase components.</span></span>
* <span data-ttu-id="d8df0-164">**dev.Properties**: informace o konfiguraci funkcí spout centra událostí, HBase bolt a součástí řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-164">**dev.properties**: The configuration information for the Event Hub spout, HBase bolt, and dashboard components.</span></span>

## <a name="prepare-your-environment"></a><span data-ttu-id="d8df0-165">Příprava prostředí</span><span class="sxs-lookup"><span data-stu-id="d8df0-165">Prepare your environment</span></span>

<span data-ttu-id="d8df0-166">Než použijete tento příklad, musíte vytvořit centra událostí Azure, který čte topologie Storm z.</span><span class="sxs-lookup"><span data-stu-id="d8df0-166">Before you use this example, you must create an Azure Event Hub, which the Storm topology reads from.</span></span>

### <a name="configure-event-hub"></a><span data-ttu-id="d8df0-167">Konfigurace centra událostí</span><span class="sxs-lookup"><span data-stu-id="d8df0-167">Configure Event Hub</span></span>

<span data-ttu-id="d8df0-168">Centrum událostí je zdroj dat pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="d8df0-168">Event Hub is the data source for this example.</span></span> <span data-ttu-id="d8df0-169">Použijte následující postup k vytvoření centra událostí.</span><span class="sxs-lookup"><span data-stu-id="d8df0-169">Use the following steps to create an Event Hub.</span></span>

1. <span data-ttu-id="d8df0-170">Z [portál Azure](https://portal.azure.com), vyberte **+ nový** -> **Internet věcí** -> **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="d8df0-170">From the [Azure portal](https://portal.azure.com), select **+ New** -> **Internet of Things** -> **Event Hubs**.</span></span>
2. <span data-ttu-id="d8df0-171">V **vytvořit Namespace** část, provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="d8df0-171">In the **Create Namespace** section, perform the following tasks:</span></span>
   
   1. <span data-ttu-id="d8df0-172">Zadejte **název** pro obor názvů.</span><span class="sxs-lookup"><span data-stu-id="d8df0-172">Enter a **Name** for the namespace.</span></span>
   2. <span data-ttu-id="d8df0-173">Vyberte cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="d8df0-173">Select a pricing tier.</span></span> <span data-ttu-id="d8df0-174">**Základní** stačí pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="d8df0-174">**Basic** is sufficient for this example.</span></span>
   3. <span data-ttu-id="d8df0-175">Vyberte Azure **předplatné** používat.</span><span class="sxs-lookup"><span data-stu-id="d8df0-175">Select the Azure **Subscription** to use.</span></span>
   4. <span data-ttu-id="d8df0-176">Vyberte existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="d8df0-176">Either select an existing resource group or create a new one.</span></span>
   5. <span data-ttu-id="d8df0-177">Vyberte **umístění** pro centra událostí.</span><span class="sxs-lookup"><span data-stu-id="d8df0-177">Select the **Location** for the Event Hub.</span></span>
   6. <span data-ttu-id="d8df0-178">Vyberte **připnout na řídicí panel**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d8df0-178">Select **Pin to dashboard**, and then click **Create**.</span></span>

3. <span data-ttu-id="d8df0-179">Po dokončení procesu vytváření, zobrazí se informace služby Event Hubs pro obor názvů.</span><span class="sxs-lookup"><span data-stu-id="d8df0-179">When the creation process completes, the Event Hubs information for your namespace is displayed.</span></span> <span data-ttu-id="d8df0-180">Zde vyberte **+ přidat centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="d8df0-180">From here, select **+ Add Event Hub**.</span></span> <span data-ttu-id="d8df0-181">V **vytvoření centra událostí** části, zadejte název **sensordata**a potom vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d8df0-181">In the **Create Event Hub** section, enter a name of **sensordata**, and then select **Create**.</span></span> <span data-ttu-id="d8df0-182">Nechte ostatní pole na výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d8df0-182">Leave the other fields at the default values.</span></span>
4. <span data-ttu-id="d8df0-183">Ze služby Event Hubs zobrazení pro obor názvů, vyberte **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="d8df0-183">From the Event Hubs view for your namespace, select **Event Hubs**.</span></span> <span data-ttu-id="d8df0-184">Vyberte **sensordata** položku.</span><span class="sxs-lookup"><span data-stu-id="d8df0-184">Select the **sensordata** entry.</span></span>
5. <span data-ttu-id="d8df0-185">Sensordata centra událostí, vyberte **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="d8df0-185">From the sensordata Event Hub, select **Shared access policies**.</span></span> <span data-ttu-id="d8df0-186">Použití **+ přidat** odkaz přidat následující zásady:</span><span class="sxs-lookup"><span data-stu-id="d8df0-186">Use the **+ Add** link to add the following policies:</span></span>

    | <span data-ttu-id="d8df0-187">Název zásady</span><span class="sxs-lookup"><span data-stu-id="d8df0-187">Policy name</span></span> | <span data-ttu-id="d8df0-188">Deklarace identity</span><span class="sxs-lookup"><span data-stu-id="d8df0-188">Claims</span></span> |
    | ----- | ----- |
    | <span data-ttu-id="d8df0-189">zařízení</span><span class="sxs-lookup"><span data-stu-id="d8df0-189">devices</span></span> | <span data-ttu-id="d8df0-190">Odeslat</span><span class="sxs-lookup"><span data-stu-id="d8df0-190">Send</span></span> |
    | <span data-ttu-id="d8df0-191">Storm</span><span class="sxs-lookup"><span data-stu-id="d8df0-191">storm</span></span> | <span data-ttu-id="d8df0-192">Naslouchání</span><span class="sxs-lookup"><span data-stu-id="d8df0-192">Listen</span></span> |

1. <span data-ttu-id="d8df0-193">Vyberte obě zásady a poznamenejte si **primární klíč** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-193">Select both policies and make a note of the **PRIMARY KEY** value.</span></span> <span data-ttu-id="d8df0-194">Potřebujete hodnota pro obě zásady v budoucnu krocích.</span><span class="sxs-lookup"><span data-stu-id="d8df0-194">You need the value for both policies in future steps.</span></span>

## <a name="download-and-configure-the-project"></a><span data-ttu-id="d8df0-195">Stáhněte si a konfigurace projektu</span><span class="sxs-lookup"><span data-stu-id="d8df0-195">Download and configure the project</span></span>

<span data-ttu-id="d8df0-196">Použijte následující ke stažení projektu z Githubu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-196">Use the following to download the project from GitHub.</span></span>

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

<span data-ttu-id="d8df0-197">Po dokončení příkazu, máte následující adresářovou strukturu:</span><span class="sxs-lookup"><span data-stu-id="d8df0-197">After the command completes, you have the following directory structure:</span></span>

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO to the web dashboard.
        dashboard/nodejs/ - this is the node.js web dashboard.
        SendEvents/ - utilities to send fake sensor data.

> [!NOTE]
> <span data-ttu-id="d8df0-198">Tento dokument není přejděte na podrobnosti o kód, který najdete v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-198">This document does not go in to full details of the code included in this example.</span></span> <span data-ttu-id="d8df0-199">Kód, je však plně komentář.</span><span class="sxs-lookup"><span data-stu-id="d8df0-199">However, the code is fully commented.</span></span>

<span data-ttu-id="d8df0-200">Ke konfiguraci projektu ke čtení z centra událostí, otevřete `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` souboru a přidejte informace vašeho centra událostí do následující řádky:</span><span class="sxs-lookup"><span data-stu-id="d8df0-200">To configure the project to read from Event Hub, open the `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` file and add your Event Hub information to the following lines:</span></span>

```bash
eventhub.read.policy.name: your_read_policy_name
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a><span data-ttu-id="d8df0-201">Kompilace a testování místně</span><span class="sxs-lookup"><span data-stu-id="d8df0-201">Compile and test locally</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8df0-202">Místně pomocí topologie vyžaduje funkční Storm vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="d8df0-202">Using the topology locally requires a working Storm development environment.</span></span> <span data-ttu-id="d8df0-203">Další informace najdete v tématu [nastavení vývojového prostředí Storm](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) na Apache.org.</span><span class="sxs-lookup"><span data-stu-id="d8df0-203">For more information, see [Setting up a Storm development environment](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) at Apache.org.</span></span>

> [!WARNING]
> <span data-ttu-id="d8df0-204">Pokud používáte vývojového prostředí systému Windows, může se zobrazit `java.io.IOException` při spuštění topologii místně.</span><span class="sxs-lookup"><span data-stu-id="d8df0-204">If you are using a Windows development environment, you may receive a `java.io.IOException` when running the topology locally.</span></span> <span data-ttu-id="d8df0-205">Pokud ano, přesunete k topologii systémem HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8df0-205">If so, move on to running the topology on HDInsight.</span></span>

<span data-ttu-id="d8df0-206">Před testováním, musíte spustit řídicí panel k zobrazení výstupu topologie a generovat data uložit do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="d8df0-206">Before testing, you must start the dashboard to view the output of the topology and generate data to store in Event Hub.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8df0-207">Komponenta HBase Tato topologie není aktivní, při testování místně.</span><span class="sxs-lookup"><span data-stu-id="d8df0-207">The HBase component of this topology is not active when testing locally.</span></span> <span data-ttu-id="d8df0-208">Rozhraní API Java pro cluster HBase, není přístupná z mimo virtuální síť Azure, který obsahuje clustery.</span><span class="sxs-lookup"><span data-stu-id="d8df0-208">The Java API for the HBase cluster cannot be accessed from outside the Azure Virtual Network that contains the clusters.</span></span>

### <a name="start-the-web-application"></a><span data-ttu-id="d8df0-209">Spuštění webové aplikace</span><span class="sxs-lookup"><span data-stu-id="d8df0-209">Start the web application</span></span>

1. <span data-ttu-id="d8df0-210">Otevřete příkazový řádek a přejděte do adresáře `hdinsight-eventhub-example/dashboard`.</span><span class="sxs-lookup"><span data-stu-id="d8df0-210">Open a command prompt and change directories to `hdinsight-eventhub-example/dashboard`.</span></span> <span data-ttu-id="d8df0-211">Použijte následující příkaz k instalaci závislosti nutné webovou aplikací:</span><span class="sxs-lookup"><span data-stu-id="d8df0-211">Use the following command to install the dependencies needed by the web application:</span></span>
   
    ```bash
    npm install
    ```

2. <span data-ttu-id="d8df0-212">Použijte následující příkaz pro spuštění webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="d8df0-212">Use the following command to start the web application:</span></span>
   
    ```bash
    node server.js
    ```
   
    <span data-ttu-id="d8df0-213">Zobrazí zpráva podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="d8df0-213">You see a message similar to the following text:</span></span>
   
        Server listening at port 3000

3. <span data-ttu-id="d8df0-214">Otevřete webový prohlížeč a zadejte `http://localhost:3000/` jako adresu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-214">Open a web browser and enter `http://localhost:3000/` as the address.</span></span> <span data-ttu-id="d8df0-215">Zobrazí se stránka podobná na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="d8df0-215">A page similar to the following image is displayed:</span></span>
   
    ![webové řídicí panel](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)
   
    <span data-ttu-id="d8df0-217">Nechte tato příkazového řádku otevřené.</span><span class="sxs-lookup"><span data-stu-id="d8df0-217">Leave this command prompt open.</span></span> <span data-ttu-id="d8df0-218">Po testování, pomocí kombinace kláves Ctrl-C zastavení webového serveru.</span><span class="sxs-lookup"><span data-stu-id="d8df0-218">After testing, use Ctrl-C to stop the web server.</span></span>

### <a name="generate-data"></a><span data-ttu-id="d8df0-219">Generování dat</span><span class="sxs-lookup"><span data-stu-id="d8df0-219">Generate data</span></span>

> [!NOTE]
> <span data-ttu-id="d8df0-220">Postup v této části použijte Node.js tak, aby bylo možné na jakékoli platformě.</span><span class="sxs-lookup"><span data-stu-id="d8df0-220">The steps in this section use Node.js so that they can be used on any platform.</span></span> <span data-ttu-id="d8df0-221">Další příklady jazyka najdete v článku `SendEvents` adresáře.</span><span class="sxs-lookup"><span data-stu-id="d8df0-221">For other language examples, see the `SendEvents` directory.</span></span>

1. <span data-ttu-id="d8df0-222">Otevřete nový řádek, prostředí nebo terminálu a přejděte do adresáře `hdinsight-eventhub-example/SendEvents/nodejs`.</span><span class="sxs-lookup"><span data-stu-id="d8df0-222">Open a new prompt, shell, or terminal, and change directories to `hdinsight-eventhub-example/SendEvents/nodejs`.</span></span> <span data-ttu-id="d8df0-223">Pokud chcete nainstalovat závislosti potřebné pro aplikaci, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d8df0-223">To install the dependencies needed by the application, use the following command:</span></span>

    ```bash
    npm install
    ```

2. <span data-ttu-id="d8df0-224">Otevřete `app.js` soubor v textovém editoru a přidání informací o centra událostí, které jste dříve získali:</span><span class="sxs-lookup"><span data-stu-id="d8df0-224">Open the `app.js` file in a text editor and add the Event Hub information you obtained earlier:</span></span>
   
    ```javascript
    // ServiceBus Namespace
    var namespace = 'YourNamespace';
    // Event Hub Name
    var hubname ='sensordata';
    // Shared access Policy name and key (from Event Hub configuration)
    var my_key_name = 'devices';
    var my_key = 'YourKey';
    ```
   
   > [!NOTE]
   > <span data-ttu-id="d8df0-225">Tento příklad předpokládá, že jste použili `sensordata` jako název vašeho centra událostí.</span><span class="sxs-lookup"><span data-stu-id="d8df0-225">This example assumes that you have used `sensordata` as the name of your Event Hub.</span></span> <span data-ttu-id="d8df0-226">A že `devices` jako název zásady, který má `Send` deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="d8df0-226">And that `devices` as the name of the policy that has a `Send` claim.</span></span>

3. <span data-ttu-id="d8df0-227">Chcete-li vložit nové položky v Centru událostí použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d8df0-227">Use the following command to insert new entries in Event Hub:</span></span>
   
    ```bash
    node app.js
    ```
   
    <span data-ttu-id="d8df0-228">Zobrazí několik řádků výstupu, které obsahují data odeslaná do centra událostí:</span><span class="sxs-lookup"><span data-stu-id="d8df0-228">You see several lines of output that contain the data sent to Event Hub:</span></span>
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="build-and-start-the-topology"></a><span data-ttu-id="d8df0-229">Sestavte a spusťte topologie</span><span class="sxs-lookup"><span data-stu-id="d8df0-229">Build and start the topology</span></span>

1. <span data-ttu-id="d8df0-230">Otevřete nový příkazový řádek a přejděte do adresáře `hdinsight-eventhub-example/TemperatureMonitor`.</span><span class="sxs-lookup"><span data-stu-id="d8df0-230">Open a new command prompt and change directories to `hdinsight-eventhub-example/TemperatureMonitor`.</span></span> <span data-ttu-id="d8df0-231">Pro sestavení a balíček topologii, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d8df0-231">To build and package the topology, use the following command:</span></span> 

    ```bash
    mvn clean package
    ```

2. <span data-ttu-id="d8df0-232">Pokud chcete spustit topologii v místním režimu, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d8df0-232">To start the topology in local mode, use the following command:</span></span>

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * <span data-ttu-id="d8df0-233">`--local`Spustí topologii v místním režimu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-233">`--local` starts the topology in local mode.</span></span>
    * <span data-ttu-id="d8df0-234">`--filter`používá `dev.properties` souboru k naplnění parametry v definici topologie.</span><span class="sxs-lookup"><span data-stu-id="d8df0-234">`--filter` uses the `dev.properties` file to populate parameters in the topology definition.</span></span>
    * <span data-ttu-id="d8df0-235">`resources/no-hbase.yaml`používá `no-hbase.yaml` definice topologie.</span><span class="sxs-lookup"><span data-stu-id="d8df0-235">`resources/no-hbase.yaml` uses the `no-hbase.yaml` topology definition.</span></span>
 
   <span data-ttu-id="d8df0-236">Po zahájení, topologii čte položky z centra událostí a odešle je na řídicí panel spuštěna na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="d8df0-236">Once started, the topology reads entries from Event Hub, and sends them to the dashboard running on your local machine.</span></span> <span data-ttu-id="d8df0-237">Měli byste vidět řádky na webový řídicím panelu nezobrazí, podobně jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="d8df0-237">You should see lines appear in the web dashboard, similar to the following image:</span></span>
   
    ![řídicí panel s daty](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

2. <span data-ttu-id="d8df0-239">Řídicí panel průběhu, použijte `node app.js` příkazu z předchozích kroků a odesílat nová data do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="d8df0-239">While the dashboard is running, use the `node app.js` command from the previous steps to send new data to Event Hubs.</span></span> <span data-ttu-id="d8df0-240">Protože teploty hodnoty jsou náhodně vygenerované, by měl aktualizovat graf můžete zobrazit velké změny v teploty.</span><span class="sxs-lookup"><span data-stu-id="d8df0-240">Because the temperature values are randomly generated, the graph should update to show large changes in temperature.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d8df0-241">Musí být v **hdinsight-eventhub – příklad/SendEvents/Nodejs** directory při použití `node app.js` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8df0-241">You must be in the **hdinsight-eventhub-example/SendEvents/Nodejs** directory when using the `node app.js` command.</span></span>

3. <span data-ttu-id="d8df0-242">Po ověření, že aktualizace řídicího panelu, zastavte topologie pomocí kombinace kláves Ctrl + C.</span><span class="sxs-lookup"><span data-stu-id="d8df0-242">After verifying that the dashboard updates, stop the topology using Ctrl+C.</span></span> <span data-ttu-id="d8df0-243">Ctrl + C můžete také zastavit místní webový server.</span><span class="sxs-lookup"><span data-stu-id="d8df0-243">You can use Ctrl+C to stop the local web server also.</span></span>

## <a name="create-a-storm-and-hbase-cluster"></a><span data-ttu-id="d8df0-244">Vytvoření clusteru Storm a HBase</span><span class="sxs-lookup"><span data-stu-id="d8df0-244">Create a Storm and HBase cluster</span></span>

<span data-ttu-id="d8df0-245">Kroky v tomto tématu použijte [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md) pro vytvoření clusteru služby Azure Virtual Network a Storm a HBase ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="d8df0-245">The steps in this section use an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md) to create an Azure Virtual Network and a Storm and HBase cluster on the virtual network.</span></span> <span data-ttu-id="d8df0-246">Šablony také vytvoří Azure Web App a nasadí kopie řídicího panelu do ní.</span><span class="sxs-lookup"><span data-stu-id="d8df0-246">The template also creates an Azure Web App and deploys a copy of the dashboard into it.</span></span>

> [!NOTE]
> <span data-ttu-id="d8df0-247">Virtuální síť se používá, takže topologie spuštěné v clusteru Storm může komunikovat přímo s clusteru HBase pomocí rozhraní API HBase Java.</span><span class="sxs-lookup"><span data-stu-id="d8df0-247">A virtual network is used so that the topology running on the Storm cluster can directly communicate with the HBase cluster using the HBase Java API.</span></span>

<span data-ttu-id="d8df0-248">Šablony Resource Manageru v tomto dokumentu se nachází v kontejneru veřejného objektu blob na **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.</span><span class="sxs-lookup"><span data-stu-id="d8df0-248">The Resource Manager template used in this document is located in a public blob container at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.</span></span>

1. <span data-ttu-id="d8df0-249">Klikněte na následující tlačítko a přihlaste se k Azure a otevřete šablonu Resource Manageru na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d8df0-249">Click the following button to sign in to Azure and open the Resource Manager template in the Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/hdinsight-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. <span data-ttu-id="d8df0-250">Z **vlastní nasazení** části, zadejte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d8df0-250">From the **Custom deployment** section, enter the following values:</span></span>
   
    ![Parametry HDInsight](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
   
   * <span data-ttu-id="d8df0-252">**Základní název clusteru**: Tato hodnota se používá jako základní název pro Storm a HBase clusterů.</span><span class="sxs-lookup"><span data-stu-id="d8df0-252">**Base Cluster Name**: This value is used as the base name for the Storm and HBase clusters.</span></span> <span data-ttu-id="d8df0-253">Například zadáním **abc** vytvoří cluster Storm s názvem **storm abc** a cluster HBase s názvem **hbase abc**.</span><span class="sxs-lookup"><span data-stu-id="d8df0-253">For example, entering **abc** creates a Storm cluster named **storm-abc** and an HBase cluster named **hbase-abc**.</span></span>
   * <span data-ttu-id="d8df0-254">**Uživatelské jméno přihlášení clusteru**: uživatelské jméno správce pro clustery Storm a HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-254">**Cluster Login User Name**: The admin user name for the Storm and HBase clusters.</span></span>
   * <span data-ttu-id="d8df0-255">**Heslo pro přihlášení clusteru**: uživatelské heslo správce pro clustery Storm a HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-255">**Cluster Login Password**: The admin user password for the Storm and HBase clusters.</span></span>
   * <span data-ttu-id="d8df0-256">**Uživatelské jméno SSH**: SSH, aby uživatel vytvořil pro clustery Storm a HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-256">**SSH User Name**: The SSH user to create for the Storm and HBase clusters.</span></span>
   * <span data-ttu-id="d8df0-257">**Heslo SSH**: heslo pro uživatele SSH pro clustery Storm a HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-257">**SSH Password**: The password for the SSH user for the Storm and HBase clusters.</span></span>
   * <span data-ttu-id="d8df0-258">**Umístění**: oblast, která clustery jsou vytvořeny v.</span><span class="sxs-lookup"><span data-stu-id="d8df0-258">**Location**: The region that the clusters are created in.</span></span>
     
     <span data-ttu-id="d8df0-259">Klikněte na možnost **OK** a uložte parametry.</span><span class="sxs-lookup"><span data-stu-id="d8df0-259">Click **OK** to save the parameters.</span></span>

3. <span data-ttu-id="d8df0-260">Použití **Základy** části vytvořit skupinu prostředků nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="d8df0-260">Use the **Basics** section to create a resource group or select an existing one.</span></span>
4. <span data-ttu-id="d8df0-261">V **umístění skupiny prostředků** rozevírací nabídce vyberte stejné umístění jako jste vybrali pro **umístění** parametr ve **nastavení** části.</span><span class="sxs-lookup"><span data-stu-id="d8df0-261">In the **Resource group location** dropdown menu, select the same location as you selected for the **Location** parameter in the **Settings** section.</span></span>
5. <span data-ttu-id="d8df0-262">Přečtěte si podmínky a ujednání a pak vyberte **souhlasím s podmínkami a ujednáními výše uvedených**.</span><span class="sxs-lookup"><span data-stu-id="d8df0-262">Read the terms and conditions, and then select **I agree to the terms and conditions stated above**.</span></span>
6. <span data-ttu-id="d8df0-263">Nakonec zkontrolujte **připnout na řídicí panel** a pak vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="d8df0-263">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="d8df0-264">Chcete-li vytvořit clustery trvá asi 20 minut.</span><span class="sxs-lookup"><span data-stu-id="d8df0-264">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="d8df0-265">Po vytvoření prostředky, zobrazí se informace o skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="d8df0-265">Once the resources have been created, information about the resource group is displayed.</span></span>

![Skupinu prostředků pro virtuální síť a clustery](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="d8df0-267">Všimněte si, že jsou názvy clusterů HDInsight **storm BASENAME** a **hbase BASENAME**, kde BASENAME je jméno, které jste zadali v šabloně.</span><span class="sxs-lookup"><span data-stu-id="d8df0-267">Notice that the names of the HDInsight clusters are **storm-BASENAME** and **hbase-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="d8df0-268">Použít tyto názvy v pozdější fázi při připojení k clustery.</span><span class="sxs-lookup"><span data-stu-id="d8df0-268">You use these names in a later step when connecting to the clusters.</span></span> <span data-ttu-id="d8df0-269">Také Upozorňujeme, že je název řídicího panelu webu **basename – řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="d8df0-269">Also note that the name of the dashboard site is **basename-dashboard**.</span></span> <span data-ttu-id="d8df0-270">Tato hodnota se používá později v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-270">This value is used later in this document.</span></span>

## <a name="configure-the-dashboard-bolt"></a><span data-ttu-id="d8df0-271">Konfigurace funkcí bolt řídicí panel</span><span class="sxs-lookup"><span data-stu-id="d8df0-271">Configure the Dashboard bolt</span></span>

<span data-ttu-id="d8df0-272">Pokud chcete odesílat data na řídicí panel nasadit jako webovou aplikaci, je třeba upravit následující řádek v `dev.properties`souboru:</span><span class="sxs-lookup"><span data-stu-id="d8df0-272">To send data to the dashboard deployed as a web app, you must modify the following line in the `dev.properties`file:</span></span>

```yaml
dashboard.uri: http://localhost:3000
```

<span data-ttu-id="d8df0-273">Změna `http://localhost:3000` k `http://BASENAME-dashboard.azurewebsites.net` a soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="d8df0-273">Change `http://localhost:3000` to `http://BASENAME-dashboard.azurewebsites.net` and save the file.</span></span> <span data-ttu-id="d8df0-274">Nahraďte **BASENAME** s základní jméno, které jste zadali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="d8df0-274">Replace **BASENAME** with the base name you provided in the previous step.</span></span> <span data-ttu-id="d8df0-275">Můžete taky Vybrat řídicí panel a zobrazit adresu URL předtím vytvořili skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d8df0-275">You can also use the resource group created previously to select the dashboard and view the URL.</span></span>

## <a name="create-the-hbase-table"></a><span data-ttu-id="d8df0-276">Vytvoření tabulky HBase</span><span class="sxs-lookup"><span data-stu-id="d8df0-276">Create the HBase table</span></span>

<span data-ttu-id="d8df0-277">K uložení dat v HBase, jsme musíte nejdřív vytvořit tabulku.</span><span class="sxs-lookup"><span data-stu-id="d8df0-277">To store data in HBase, we must first create a table.</span></span> <span data-ttu-id="d8df0-278">Předem vytvořit prostředky, které Storm potřebuje k zápisu, jako pokusu o vytvoření prostředky z uvnitř Storm může způsobit topologie více instancí pokusu o vytvoření stejného zdroje.</span><span class="sxs-lookup"><span data-stu-id="d8df0-278">Pre-create resources that Storm needs to write to, as trying to create resources from inside a Storm topology can result in multiple instances trying to create the same resource.</span></span> <span data-ttu-id="d8df0-279">Vytvořit prostředky mimo topologie a používat Storm pro čtení/zápis a analýzy.</span><span class="sxs-lookup"><span data-stu-id="d8df0-279">Create the resources outside the topology and use Storm for reading/writing and analytics.</span></span>

1. <span data-ttu-id="d8df0-280">Použití SSH se připojit ke clusteru HBase pomocí SSH uživatele a heslo, které jste zadali v šabloně při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8df0-280">Use SSH to connect to the HBase cluster using the SSH user and password you supplied to the template during cluster creation.</span></span> <span data-ttu-id="d8df0-281">Například, pokud připojení pomocí `ssh` příkazu, použijte následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="d8df0-281">For example, if connecting using the `ssh` command, you would use the following syntax:</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    <span data-ttu-id="d8df0-282">Nahraďte `sshuser` uživatelským jménem SSH, které jste zadali při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8df0-282">Replace `sshuser` with the SSH user name you provided when creating the cluster.</span></span> <span data-ttu-id="d8df0-283">Nahraďte `clustername` s názvem clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-283">Replace `clustername` with the HBase cluster name.</span></span>

2. <span data-ttu-id="d8df0-284">Z relace SSH spusťte prostředí HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-284">From the SSH session, start the HBase shell.</span></span>
   
    ```bash
    hbase shell
    ```
   
    <span data-ttu-id="d8df0-285">Jakmile prostředí načetl, zobrazí `hbase(main):001:0>` řádku.</span><span class="sxs-lookup"><span data-stu-id="d8df0-285">Once the shell has loaded, you see an `hbase(main):001:0>` prompt.</span></span>

3. <span data-ttu-id="d8df0-286">Z prostředí HBase zadejte následující příkaz a vytvořte tabulku pro ukládání dat snímačů:</span><span class="sxs-lookup"><span data-stu-id="d8df0-286">From the HBase shell, enter the following command to create a table to store the sensor data:</span></span>
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. <span data-ttu-id="d8df0-287">Ověřte, že tabulka byla vytvořena pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d8df0-287">Verify that the table has been created by using the following command:</span></span>
   
    ```hbase
    scan 'SensorData'
    ```
   
    <span data-ttu-id="d8df0-288">Vrátí informace podobně jako v následujícím příkladu, označující, že jsou 0 řádky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="d8df0-288">This returns information similar to the following example, indicating that there are 0 rows in the table.</span></span>
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. <span data-ttu-id="d8df0-289">Zadejte `exit` ukončíte prostředí HBase:</span><span class="sxs-lookup"><span data-stu-id="d8df0-289">Enter `exit` to exit the HBase shell:</span></span>

## <a name="configure-the-hbase-bolt"></a><span data-ttu-id="d8df0-290">Konfigurace funkcí bolt HBase</span><span class="sxs-lookup"><span data-stu-id="d8df0-290">Configure the HBase bolt</span></span>

<span data-ttu-id="d8df0-291">Zapsat do HBase z clusteru Storm, je nutné zadat HBase bolt s podrobností konfigurace clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-291">To write to HBase from the Storm cluster, you must provide the HBase bolt with the configuration details of your HBase cluster.</span></span>

1. <span data-ttu-id="d8df0-292">K načtení Zookeeper kvora pro váš cluster HBase, použijte jednu z následujících příkladech:</span><span class="sxs-lookup"><span data-stu-id="d8df0-292">Use one of the following examples to retrieve the Zookeeper quorum for your HBase cluster:</span></span>

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > <span data-ttu-id="d8df0-293">Parametr `your_HDInsight_cluster_name` nahraďte názvem vašeho clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8df0-293">Replace `your_HDInsight_cluster_name` with the name of your HDInsight cluster.</span></span> <span data-ttu-id="d8df0-294">Další informace o instalaci `jq` nástroj, najdete v části [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).</span><span class="sxs-lookup"><span data-stu-id="d8df0-294">For more information on installing the `jq` utility, see [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).</span></span>
    >
    > <span data-ttu-id="d8df0-295">Po zobrazení výzvy zadejte heslo pro přihlašovací jméno správce HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8df0-295">When prompted, enter the password for the HDInsight admin login.</span></span>

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name`
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > <span data-ttu-id="d8df0-296">Nahraďte ' your_HDInsight_cluster_name s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8df0-296">Replace \`your_HDInsight_cluster_name with the name of your HDInsight cluster.</span></span> <span data-ttu-id="d8df0-297">Po zobrazení výzvy zadejte heslo pro přihlašovací jméno správce HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d8df0-297">When prompted, enter the password for the HDInsight admin login.</span></span>
    >
    > <span data-ttu-id="d8df0-298">Tento příklad vyžaduje prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d8df0-298">This example requires Azure PowerShell.</span></span> <span data-ttu-id="d8df0-299">Další informace o použití prostředí Azure PowerShell najdete v tématu [Začínáme s Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)</span><span class="sxs-lookup"><span data-stu-id="d8df0-299">For more information on using Azure PowerShell, see [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)</span></span>

    <span data-ttu-id="d8df0-300">Informace vrácené těchto příkladech je podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="d8df0-300">The information returned by these examples is similar to the following text:</span></span>

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    <span data-ttu-id="d8df0-301">Tyto informace slouží Storm ke komunikaci s clusterem HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-301">This information is used by Storm to communicate with the HBase cluster.</span></span>

2. <span data-ttu-id="d8df0-302">Upravit `dev.properties` souboru a přidejte informace o Zookeeper kvora do následujícího řádku:</span><span class="sxs-lookup"><span data-stu-id="d8df0-302">Modify the `dev.properties` file and add the Zookeeper quorum information to the following line:</span></span>

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a><span data-ttu-id="d8df0-303">Vytvoření balíčku a nasadit řešení do HDInsight</span><span class="sxs-lookup"><span data-stu-id="d8df0-303">Build, package, and deploy the solution to HDInsight</span></span>

<span data-ttu-id="d8df0-304">Ve vašem vývojovém prostředí použijte následující kroky k nasazení topologie Storm ke clusteru storm.</span><span class="sxs-lookup"><span data-stu-id="d8df0-304">In your development environment, use the following steps to deploy the Storm topology to the storm cluster.</span></span>

1. <span data-ttu-id="d8df0-305">Z `TemperatureMonitor` adresář, použijte následující příkaz k provedení nového sestavení a vytvoření JAR balíčků z projektu:</span><span class="sxs-lookup"><span data-stu-id="d8df0-305">From the `TemperatureMonitor` directory, use the following command to perform a new build and create a JAR package from your project:</span></span>
   
        mvn clean package
   
    <span data-ttu-id="d8df0-306">Tento příkaz vytvoří soubor s názvem `TemperatureMonitor-1.0-SNAPSHOT.jar in the `cíl ' adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="d8df0-306">This command creates a file named `TemperatureMonitor-1.0-SNAPSHOT.jar in the `target\` directory of your project.</span></span>

2. <span data-ttu-id="d8df0-307">Spojovací bod služby použít k nahrání `TemperatureMonitor-1.0-SNAPSHOT.jar` a `dev.properties` soubory do clusteru Storm.</span><span class="sxs-lookup"><span data-stu-id="d8df0-307">Use scp to upload the `TemperatureMonitor-1.0-SNAPSHOT.jar` and `dev.properties` files to your Storm cluster.</span></span> <span data-ttu-id="d8df0-308">V následujícím příkladu nahraďte `sshuser` uživatele SSH, jste zadali při vytváření clusteru, a `clustername` s názvem vašeho clusteru Storm.</span><span class="sxs-lookup"><span data-stu-id="d8df0-308">In the following example, replace `sshuser` with the SSH user you provided when creating the cluster, and `clustername` with the name of your Storm cluster.</span></span> <span data-ttu-id="d8df0-309">Po zobrazení výzvy zadejte heslo pro uživatele SSH.</span><span class="sxs-lookup"><span data-stu-id="d8df0-309">When prompted, enter the password for the SSH user.</span></span>
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > <span data-ttu-id="d8df0-310">Ho může trvat několik minut odesílat soubory.</span><span class="sxs-lookup"><span data-stu-id="d8df0-310">It may take several minutes to upload the files.</span></span>

    <span data-ttu-id="d8df0-311">Další informace o používání `scp` a `ssh` příkazy s HDInsight, najdete v části [použití SSH s HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="d8df0-311">For more information on using the `scp` and `ssh` commands with HDInsight, see [Use SSH with HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

3. <span data-ttu-id="d8df0-312">Jakmile soubor byl odeslán, připojte se ke clusteru Storm pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="d8df0-312">Once the file has been uploaded, connect to the Storm cluster using SSH.</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="d8df0-313">Nahraďte `sshuser` s uživatelským jménem SSH.</span><span class="sxs-lookup"><span data-stu-id="d8df0-313">Replace `sshuser` with the SSH user name.</span></span> <span data-ttu-id="d8df0-314">Nahraďte `clustername` s názvem clusteru Storm.</span><span class="sxs-lookup"><span data-stu-id="d8df0-314">Replace `clustername` with the Storm cluster name.</span></span>

4. <span data-ttu-id="d8df0-315">Chcete-li spustit topologii, použijte následující příkaz z relace SSH:</span><span class="sxs-lookup"><span data-stu-id="d8df0-315">To start the topology, use the following command from the SSH session:</span></span>
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * <span data-ttu-id="d8df0-316">`--remote`odešle topologii pro službu Nimbus, která distribuuje jej do nadřízeného uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d8df0-316">`--remote` submits the topology to the Nimbus service, which distributes it to the supervisor nodes in the cluster.</span></span>
    * <span data-ttu-id="d8df0-317">`--filter`používá `dev.properties` souboru k naplnění parametry v definici topologie.</span><span class="sxs-lookup"><span data-stu-id="d8df0-317">`--filter` uses the `dev.properties` file to populate parameters in the topology definition.</span></span>
    * <span data-ttu-id="d8df0-318">`-R /with-hbase.yaml`používá `with-hbase.yaml` topologie, které jsou součástí balíčku.</span><span class="sxs-lookup"><span data-stu-id="d8df0-318">`-R /with-hbase.yaml` uses the `with-hbase.yaml` topology included in the package.</span></span>

5. <span data-ttu-id="d8df0-319">Po spuštění topologii, otevřete prohlížeč na web můžete publikovat na platformě Azure, potom použít `node app.js` příkaz, který odesílá data do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="d8df0-319">After the topology has started, open a browser to the website you published on Azure, then use the `node app.js` command to send data to Event Hub.</span></span> <span data-ttu-id="d8df0-320">Měli byste vidět řídicího panelu webové aktualizace pro zobrazení informací.</span><span class="sxs-lookup"><span data-stu-id="d8df0-320">You should see the web dashboard update to display the information.</span></span>
   
    ![řídicí panel](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a><span data-ttu-id="d8df0-322">Zobrazení dat HBase</span><span class="sxs-lookup"><span data-stu-id="d8df0-322">View HBase data</span></span>

<span data-ttu-id="d8df0-323">Použijte následující postup pro připojení k HBase a ověřte, že data byla zapsána na tabulky:</span><span class="sxs-lookup"><span data-stu-id="d8df0-323">Use the following steps to connect to HBase and verify that the data has been written to the table:</span></span>

1. <span data-ttu-id="d8df0-324">Použití SSH se připojit ke clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-324">Use SSH to connect to the HBase cluster.</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="d8df0-325">Nahraďte `sshuser` s uživatelským jménem SSH.</span><span class="sxs-lookup"><span data-stu-id="d8df0-325">Replace `sshuser` with the SSH user name.</span></span> <span data-ttu-id="d8df0-326">Nahraďte `clustername` s názvem clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-326">Replace `clustername` with the HBase cluster name.</span></span>

2. <span data-ttu-id="d8df0-327">Z relace SSH spusťte prostředí HBase.</span><span class="sxs-lookup"><span data-stu-id="d8df0-327">From the SSH session, start the HBase shell.</span></span>
   
    ```bash
    hbase shell
    ```
   
    <span data-ttu-id="d8df0-328">Jakmile prostředí načetl, zobrazí `hbase(main):001:0>` řádku.</span><span class="sxs-lookup"><span data-stu-id="d8df0-328">Once the shell has loaded, you see an `hbase(main):001:0>` prompt.</span></span>

3. <span data-ttu-id="d8df0-329">Zobrazení řádků z tabulky:</span><span class="sxs-lookup"><span data-stu-id="d8df0-329">View rows from the table:</span></span>
   
    ```hbase
    scan 'SensorData'
    ```
   
    <span data-ttu-id="d8df0-330">Tento příkaz vrátí informace podobná následující text, označující, že je v tabulce data.</span><span class="sxs-lookup"><span data-stu-id="d8df0-330">This command returns information similar to the following text, indicating that there is data in the table.</span></span>
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > <span data-ttu-id="d8df0-331">Tato operace kontroly vrací maximálně 10 řádků z tabulky.</span><span class="sxs-lookup"><span data-stu-id="d8df0-331">This scan operation returns a maximum of 10 rows from the table.</span></span>

## <a name="delete-your-clusters"></a><span data-ttu-id="d8df0-332">Odstranění clusterů</span><span class="sxs-lookup"><span data-stu-id="d8df0-332">Delete your clusters</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="d8df0-333">Pokud chcete odstranit clustery, úložiště a webové aplikace v jednom okamžiku, odstraňte skupinu prostředků, která je obsahuje.</span><span class="sxs-lookup"><span data-stu-id="d8df0-333">To delete the clusters, storage, and web app at one time, delete the resource group that contains them.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8df0-334">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8df0-334">Next steps</span></span>

<span data-ttu-id="d8df0-335">Další příklady topologií Storm s HDInsight naleznete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)</span><span class="sxs-lookup"><span data-stu-id="d8df0-335">For more examples of Storm topologies with HDInsight, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md)</span></span>

<span data-ttu-id="d8df0-336">Další informace o Apache Storm naleznete v tématu [Apache Storm](https://storm.incubator.apache.org/) lokality.</span><span class="sxs-lookup"><span data-stu-id="d8df0-336">For more information about Apache Storm, see the [Apache Storm](https://storm.incubator.apache.org/) site.</span></span>

<span data-ttu-id="d8df0-337">Další informace o HBase v HDInsight, najdete v článku [HBase s HDInsight přehled](hdinsight-hbase-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d8df0-337">For more information about HBase on HDInsight, see the [HBase with HDInsight Overview](hdinsight-hbase-overview.md).</span></span>

<span data-ttu-id="d8df0-338">Další informace o Socket.io najdete v tématu [socket.io](http://socket.io/) lokality.</span><span class="sxs-lookup"><span data-stu-id="d8df0-338">For more information about Socket.io, see the [socket.io](http://socket.io/) site.</span></span>

<span data-ttu-id="d8df0-339">Další informace o D3.js najdete v tématu [D3.js - dokumenty řízené daty](http://d3js.org/).</span><span class="sxs-lookup"><span data-stu-id="d8df0-339">For more information about D3.js, see [D3.js - Data Driven Documents](http://d3js.org/).</span></span>

<span data-ttu-id="d8df0-340">Informace o vytváření topologie v jazyce Java najdete v tématu [Java vývoj topologií pro Apache Storm v HDInsight](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="d8df0-340">For information about creating topologies in Java, see [Develop Java topologies for Apache Storm on HDInsight](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="d8df0-341">Informace o vytváření topologie v rozhraní .NET najdete v tématu [vývoj topologií C# pro Apache Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="d8df0-341">For information about creating topologies in .NET, see [Develop C# topologies for Apache Storm on HDInsight using Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

[azure-portal]: https://portal.azure.com
