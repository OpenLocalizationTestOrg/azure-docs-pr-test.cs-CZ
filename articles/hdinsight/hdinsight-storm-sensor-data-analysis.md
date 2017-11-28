---
title: "data snímače aaaAnalyze s Apache Storm a HBase | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooApache Storm s virtuální sítí. Pomocí Storm s HBase tooprocess data snímačů z centra událostí a vizualizovat s D3.js."
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
ms.openlocfilehash: e54fe9ffc720b0089f90e302b24a9438bd43999a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a><span data-ttu-id="dde8c-104">Analýza dat snímačů s Apache Storm, centra událostí a HBase v HDInsight (Hadoop)</span><span class="sxs-lookup"><span data-stu-id="dde8c-104">Analyze sensor data with Apache Storm, Event Hub, and HBase in HDInsight (Hadoop)</span></span>

<span data-ttu-id="dde8c-105">Zjistěte, jak toouse Apache Storm v HDInsight tooprocess senzor, data z centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="dde8c-105">Learn how toouse Apache Storm on HDInsight tooprocess sensor data from Azure Event Hub.</span></span> <span data-ttu-id="dde8c-106">Hello data jsou pak uloženy do Apache HBase v HDInsight a detekují pomocí D3.js.</span><span class="sxs-lookup"><span data-stu-id="dde8c-106">hello data is then stored into Apache HBase on HDInsight, and visualized using D3.js.</span></span>

<span data-ttu-id="dde8c-107">šablony Azure Resource Manager Hello použité v tomto dokumentu ukazuje, jak toocreate několik prostředků Azure ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="dde8c-107">hello Azure Resource Manager template used in this document demonstrates how toocreate multiple Azure resources in a resource group.</span></span> <span data-ttu-id="dde8c-108">Hello šablona vytvoří virtuální síť Azure, dva clustery HDInsight (Storm a HBase) a webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="dde8c-108">hello template creates an Azure Virtual Network, two HDInsight clusters (Storm and HBase) and an Azure Web App.</span></span> <span data-ttu-id="dde8c-109">Implementace node.js řídicího panelu webu v reálném čase je toohello automaticky nasazené webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="dde8c-109">A node.js implementation of a real-time web dashboard is automatically deployed toohello web app.</span></span>

> [!NOTE]
> <span data-ttu-id="dde8c-110">Hello informace v tomto dokumentu a v příkladu v tomto dokumentu vyžadují HDInsight verze 3.6.</span><span class="sxs-lookup"><span data-stu-id="dde8c-110">hello information in this document and example in this document require HDInsight version 3.6.</span></span>
>
> <span data-ttu-id="dde8c-111">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="dde8c-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="dde8c-112">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="dde8c-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dde8c-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dde8c-113">Prerequisites</span></span>

* <span data-ttu-id="dde8c-114">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="dde8c-114">An Azure subscription.</span></span>
* <span data-ttu-id="dde8c-115">[Node.js](http://nodejs.org/): řídicího panelu webové hello použité toopreview místně na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="dde8c-115">[Node.js](http://nodejs.org/): Used toopreview hello web dashboard locally on your development environment.</span></span>
* <span data-ttu-id="dde8c-116">[Java a hello JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): používá topologie Storm toodevelop hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-116">[Java and hello JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): Used toodevelop hello Storm topology.</span></span>
* <span data-ttu-id="dde8c-117">[Maven](http://maven.apache.org/what-is-maven.html): používané toobuild a kompilace projektu hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-117">[Maven](http://maven.apache.org/what-is-maven.html): Used toobuild and compile hello project.</span></span>
* <span data-ttu-id="dde8c-118">[Git](http://git-scm.com/): používané toodownload hello projektu z Githubu.</span><span class="sxs-lookup"><span data-stu-id="dde8c-118">[Git](http://git-scm.com/): Used toodownload hello project from GitHub.</span></span>
* <span data-ttu-id="dde8c-119">**SSH** klienta: používá clustery HDInsight se systémem Linux toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="dde8c-119">An **SSH** client: Used tooconnect toohello Linux-based HDInsight clusters.</span></span> <span data-ttu-id="dde8c-120">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="dde8c-120">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>


> [!IMPORTANT]
> <span data-ttu-id="dde8c-121">Není nutné stávajícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dde8c-121">You do not need an existing HDInsight cluster.</span></span> <span data-ttu-id="dde8c-122">Hello kroky v tomto dokumentu vytvořte hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="dde8c-122">hello steps in this document create hello following resources:</span></span>
> 
> * <span data-ttu-id="dde8c-123">Virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="dde8c-123">An Azure Virtual Network</span></span>
> * <span data-ttu-id="dde8c-124">Storm v clusteru HDInsight (systémem Linux dva uzly pracovního procesu)</span><span class="sxs-lookup"><span data-stu-id="dde8c-124">A Storm on HDInsight cluster (Linux-based, two worker nodes)</span></span>
> * <span data-ttu-id="dde8c-125">Na clusteru HDInsight HBase (systémem Linux dva uzly pracovního procesu)</span><span class="sxs-lookup"><span data-stu-id="dde8c-125">An HBase on HDInsight cluster (Linux-based, two worker nodes)</span></span>
> * <span data-ttu-id="dde8c-126">Webové aplikace Azure, který je hostitelem webové hello řídicí panel</span><span class="sxs-lookup"><span data-stu-id="dde8c-126">An Azure Web App that hosts hello web dashboard</span></span>

## <a name="architecture"></a><span data-ttu-id="dde8c-127">Architektura</span><span class="sxs-lookup"><span data-stu-id="dde8c-127">Architecture</span></span>

![diagram architektury](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

<span data-ttu-id="dde8c-129">V tomto příkladu se skládá z hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="dde8c-129">This example consists of hello following components:</span></span>

* <span data-ttu-id="dde8c-130">**Azure Event Hubs**: obsahuje data, která se shromažďují ze senzorů.</span><span class="sxs-lookup"><span data-stu-id="dde8c-130">**Azure Event Hubs**: Contains data that is collected from sensors.</span></span>
* <span data-ttu-id="dde8c-131">**Storm v HDInsight**: poskytuje v reálném čase zpracování dat z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="dde8c-131">**Storm on HDInsight**: Provides real-time processing of data from Event Hub.</span></span>
* <span data-ttu-id="dde8c-132">**HBase v HDInsight**: po zpracování pomocí Storm poskytuje trvalé úložiště dat typu NoSQL pro data.</span><span class="sxs-lookup"><span data-stu-id="dde8c-132">**HBase on HDInsight**: Provides a persistent NoSQL data store for data after it has been processed by Storm.</span></span>
* <span data-ttu-id="dde8c-133">**Služba Azure virtuální sítě**: umožňuje zabezpečení komunikace mezi hello Storm v HDInsight a HBase v HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="dde8c-133">**Azure Virtual Network service**: Enables secure communications between hello Storm on HDInsight and HBase on HDInsight clusters.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="dde8c-134">Při použití rozhraní API klienta hello Java HBase je potřeba virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="dde8c-134">A virtual network is required when using hello Java HBase client API.</span></span> <span data-ttu-id="dde8c-135">Ho nebude vystavena, přes hello veřejné brány pro clustery HBase.</span><span class="sxs-lookup"><span data-stu-id="dde8c-135">It is not exposed over hello public gateway for HBase clusters.</span></span> <span data-ttu-id="dde8c-136">Instalaci HBase a Storm clusterů do stejné virtuální síti umožňuje hello hello clusteru Storm (nebo všechny ostatní systémy ve virtuální síti hello) toodirectly přístup HBase pomocí rozhraní API klienta.</span><span class="sxs-lookup"><span data-stu-id="dde8c-136">Installing HBase and Storm clusters into hello same virtual network allows hello Storm cluster (or any other system on hello virtual network) toodirectly access HBase using client API.</span></span>

* <span data-ttu-id="dde8c-137">**Řídicí panel webu**: příklad řídicího panelu, který grafy data v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="dde8c-137">**Dashboard website**: An example dashboard that charts data in real time.</span></span>
  
  * <span data-ttu-id="dde8c-138">Hello webu je implementované v Node.js.</span><span class="sxs-lookup"><span data-stu-id="dde8c-138">hello website is implemented in Node.js.</span></span>
  * <span data-ttu-id="dde8c-139">[Socket.IO](http://socket.io/) se používá pro komunikaci v reálném čase mezi topologie Storm hello a hello webu.</span><span class="sxs-lookup"><span data-stu-id="dde8c-139">[Socket.io](http://socket.io/) is used for real-time communication between hello Storm topology and hello website.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="dde8c-140">Pro komunikaci pomocí Socket.io je podrobností implementace.</span><span class="sxs-lookup"><span data-stu-id="dde8c-140">Using Socket.io for communication is an implementation detail.</span></span> <span data-ttu-id="dde8c-141">Můžete použít libovolnou architekturu komunikace, jako je například nezpracovaná Websocket nebo SignalR.</span><span class="sxs-lookup"><span data-stu-id="dde8c-141">You can use any communications framework, such as raw WebSockets or SignalR.</span></span>

  * <span data-ttu-id="dde8c-142">[D3.js](http://d3js.org/) jsou použité toograph hello data, která je odeslána toohello webu.</span><span class="sxs-lookup"><span data-stu-id="dde8c-142">[D3.js](http://d3js.org/) is used toograph hello data that is sent toohello website.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dde8c-143">Dva clustery jsou povinné, protože neexistuje žádná podporovaná metoda toocreate jeden cluster HDInsight Storm a HBase.</span><span class="sxs-lookup"><span data-stu-id="dde8c-143">Two clusters are required, as there is no supported method toocreate one HDInsight cluster for both Storm and HBase.</span></span>

<span data-ttu-id="dde8c-144">Hello topologie čte data z centra událostí pomocí hello [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) třídy a zápisu dat do HBase pomocí hello [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) Třída.</span><span class="sxs-lookup"><span data-stu-id="dde8c-144">hello topology reads data from Event Hub by using hello [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) class, and writes data into HBase using hello [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) class.</span></span> <span data-ttu-id="dde8c-145">Komunikace s webem hello se dá udělat pomocí [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).</span><span class="sxs-lookup"><span data-stu-id="dde8c-145">Communication with hello website is accomplished by using [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java).</span></span>

<span data-ttu-id="dde8c-146">Hello následující diagram popisuje rozložení hello hello topologie:</span><span class="sxs-lookup"><span data-stu-id="dde8c-146">hello following diagram explains hello layout of hello topology:</span></span>

![diagram topologie](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> <span data-ttu-id="dde8c-148">Tento diagram je zjednodušený přehled topologie hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-148">This diagram is a simplified view of hello topology.</span></span> <span data-ttu-id="dde8c-149">Pro každý oddíl v Centru událostí je vytvořena instance jednotlivých součástí.</span><span class="sxs-lookup"><span data-stu-id="dde8c-149">An instance of each component is created for each partition in your Event Hub.</span></span> <span data-ttu-id="dde8c-150">Tyto instance jsou rozmístěny v hello uzly v clusteru hello a data se směruje mezi nimi takto:</span><span class="sxs-lookup"><span data-stu-id="dde8c-150">These instances are distributed across hello nodes in hello cluster, and data is routed between them as follows:</span></span>
> 
> * <span data-ttu-id="dde8c-151">Data z hello spout toohello Analyzátor je skupinu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="dde8c-151">Data from hello spout toohello parser is load balanced.</span></span>
> * <span data-ttu-id="dde8c-152">Data z hello analyzátor toohello řídicí panel a HBase se seskupují podle ID zařízení, tak, aby zprávy z hello stejného zařízení vždy toku toohello stejné komponenty.</span><span class="sxs-lookup"><span data-stu-id="dde8c-152">Data from hello parser toohello Dashboard and HBase is grouped by Device ID, so that messages from hello same device always flow toohello same component.</span></span>

### <a name="topology-components"></a><span data-ttu-id="dde8c-153">Topologie součásti</span><span class="sxs-lookup"><span data-stu-id="dde8c-153">Topology components</span></span>

* <span data-ttu-id="dde8c-154">**Event Hub Spout**: hello spout je zadaný jako součást Apache Storm verze 0.10.0 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="dde8c-154">**Event Hub Spout**: hello spout is provided as part of Apache Storm version 0.10.0 and higher.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="dde8c-155">spout Hello centra událostí použité v tomto příkladu vyžaduje Storm v HDInsight clusteru verze 3.5 nebo 3.6.</span><span class="sxs-lookup"><span data-stu-id="dde8c-155">hello Event Hub spout used in this example requires a Storm on HDInsight cluster version 3.5 or 3.6.</span></span>

* <span data-ttu-id="dde8c-156">**ParserBolt.java**: hello data, která je vysílaných hello spout je nezpracovaném formátu JSON a současně je vygenerované příležitostně více než jednu událost.</span><span class="sxs-lookup"><span data-stu-id="dde8c-156">**ParserBolt.java**: hello data that is emitted by hello spout is raw JSON, and occasionally more than one event is emitted at a time.</span></span> <span data-ttu-id="dde8c-157">Touto funkcí bolt čte data hello vysílaných hello spout a analyzuje uvítací zprávu JSON.</span><span class="sxs-lookup"><span data-stu-id="dde8c-157">This bolt reads hello data emitted by hello spout and parses hello JSON message.</span></span> <span data-ttu-id="dde8c-158">Hello bolt pak vysílá hello data jako řazené kolekce členů, která obsahuje více polí.</span><span class="sxs-lookup"><span data-stu-id="dde8c-158">hello bolt then emits hello data as a tuple that contains multiple fields.</span></span>
* <span data-ttu-id="dde8c-159">**DashboardBolt.java**: Tato součást ukazuje, jak toouse hello Socket.io klientské knihovny pro aplikace Java toosend data v reálném čase toohello webový řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="dde8c-159">**DashboardBolt.java**: This component demonstrates how toouse hello Socket.io client library for Java toosend data in real time toohello web dashboard.</span></span>
* <span data-ttu-id="dde8c-160">**Ne hbase.yaml**: hello Definice topologie použitá při spuštění v místním režimu.</span><span class="sxs-lookup"><span data-stu-id="dde8c-160">**no-hbase.yaml**: hello topology definition used when running in local mode.</span></span> <span data-ttu-id="dde8c-161">HBase součásti nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="dde8c-161">It does not use HBase components.</span></span>
* <span data-ttu-id="dde8c-162">**s hbase.yaml**: hello Definice topologie použitá při spuštění v clusteru hello hello topologie.</span><span class="sxs-lookup"><span data-stu-id="dde8c-162">**with-hbase.yaml**: hello topology definition used when running hello topology on hello cluster.</span></span> <span data-ttu-id="dde8c-163">Používá HBase součásti.</span><span class="sxs-lookup"><span data-stu-id="dde8c-163">It does use HBase components.</span></span>
* <span data-ttu-id="dde8c-164">**dev.Properties**: hello informace o konfiguraci pro hello Event Hub spout HBase bolt a součástí řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="dde8c-164">**dev.properties**: hello configuration information for hello Event Hub spout, HBase bolt, and dashboard components.</span></span>

## <a name="prepare-your-environment"></a><span data-ttu-id="dde8c-165">Příprava prostředí</span><span class="sxs-lookup"><span data-stu-id="dde8c-165">Prepare your environment</span></span>

<span data-ttu-id="dde8c-166">Než použijete tento příklad, musíte vytvořit centra událostí Azure, kterou topologii Storm hello čte z.</span><span class="sxs-lookup"><span data-stu-id="dde8c-166">Before you use this example, you must create an Azure Event Hub, which hello Storm topology reads from.</span></span>

### <a name="configure-event-hub"></a><span data-ttu-id="dde8c-167">Konfigurace centra událostí</span><span class="sxs-lookup"><span data-stu-id="dde8c-167">Configure Event Hub</span></span>

<span data-ttu-id="dde8c-168">Centrum událostí je hello zdroj dat pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="dde8c-168">Event Hub is hello data source for this example.</span></span> <span data-ttu-id="dde8c-169">Pomocí následujících kroků toocreate centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-169">Use hello following steps toocreate an Event Hub.</span></span>

1. <span data-ttu-id="dde8c-170">Z hello [portál Azure](https://portal.azure.com), vyberte **+ nový** -> **Internet věcí** -> **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="dde8c-170">From hello [Azure portal](https://portal.azure.com), select **+ New** -> **Internet of Things** -> **Event Hubs**.</span></span>
2. <span data-ttu-id="dde8c-171">V hello **vytvořit Namespace** část, proveďte následující úlohy hello:</span><span class="sxs-lookup"><span data-stu-id="dde8c-171">In hello **Create Namespace** section, perform hello following tasks:</span></span>
   
   1. <span data-ttu-id="dde8c-172">Zadejte **název** pro obor názvů hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-172">Enter a **Name** for hello namespace.</span></span>
   2. <span data-ttu-id="dde8c-173">Vyberte cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="dde8c-173">Select a pricing tier.</span></span> <span data-ttu-id="dde8c-174">**Základní** stačí pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="dde8c-174">**Basic** is sufficient for this example.</span></span>
   3. <span data-ttu-id="dde8c-175">Vyberte hello Azure **předplatné** toouse.</span><span class="sxs-lookup"><span data-stu-id="dde8c-175">Select hello Azure **Subscription** toouse.</span></span>
   4. <span data-ttu-id="dde8c-176">Vyberte existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="dde8c-176">Either select an existing resource group or create a new one.</span></span>
   5. <span data-ttu-id="dde8c-177">Vyberte hello **umístění** pro hello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="dde8c-177">Select hello **Location** for hello Event Hub.</span></span>
   6. <span data-ttu-id="dde8c-178">Vyberte **Pin toodashboard**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dde8c-178">Select **Pin toodashboard**, and then click **Create**.</span></span>

3. <span data-ttu-id="dde8c-179">Po dokončení procesu vytváření hello hello Event Hubs informace pro vašeho oboru názvů se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="dde8c-179">When hello creation process completes, hello Event Hubs information for your namespace is displayed.</span></span> <span data-ttu-id="dde8c-180">Zde vyberte **+ přidat centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="dde8c-180">From here, select **+ Add Event Hub**.</span></span> <span data-ttu-id="dde8c-181">V hello **vytvoření centra událostí** části, zadejte název **sensordata**a potom vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dde8c-181">In hello **Create Event Hub** section, enter a name of **sensordata**, and then select **Create**.</span></span> <span data-ttu-id="dde8c-182">Ostatní pole na výchozí hodnoty hello zůstanou hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-182">Leave hello other fields at hello default values.</span></span>
4. <span data-ttu-id="dde8c-183">Ze služby Event Hubs hello zobrazení pro obor názvů, vyberte **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="dde8c-183">From hello Event Hubs view for your namespace, select **Event Hubs**.</span></span> <span data-ttu-id="dde8c-184">Vyberte hello **sensordata** položku.</span><span class="sxs-lookup"><span data-stu-id="dde8c-184">Select hello **sensordata** entry.</span></span>
5. <span data-ttu-id="dde8c-185">Hello sensordata centra událostí, vyberte **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="dde8c-185">From hello sensordata Event Hub, select **Shared access policies**.</span></span> <span data-ttu-id="dde8c-186">Použití hello **+ přidat** hello tooadd odkaz následující zásady:</span><span class="sxs-lookup"><span data-stu-id="dde8c-186">Use hello **+ Add** link tooadd hello following policies:</span></span>

    | <span data-ttu-id="dde8c-187">Název zásady</span><span class="sxs-lookup"><span data-stu-id="dde8c-187">Policy name</span></span> | <span data-ttu-id="dde8c-188">Deklarace identity</span><span class="sxs-lookup"><span data-stu-id="dde8c-188">Claims</span></span> |
    | ----- | ----- |
    | <span data-ttu-id="dde8c-189">zařízení</span><span class="sxs-lookup"><span data-stu-id="dde8c-189">devices</span></span> | <span data-ttu-id="dde8c-190">Odeslat</span><span class="sxs-lookup"><span data-stu-id="dde8c-190">Send</span></span> |
    | <span data-ttu-id="dde8c-191">Storm</span><span class="sxs-lookup"><span data-stu-id="dde8c-191">storm</span></span> | <span data-ttu-id="dde8c-192">Naslouchání</span><span class="sxs-lookup"><span data-stu-id="dde8c-192">Listen</span></span> |

1. <span data-ttu-id="dde8c-193">Vyberte obě zásady a poznamenejte si hello **primární klíč** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dde8c-193">Select both policies and make a note of hello **PRIMARY KEY** value.</span></span> <span data-ttu-id="dde8c-194">Potřebujete hello hodnota pro obě zásady v budoucnu krocích.</span><span class="sxs-lookup"><span data-stu-id="dde8c-194">You need hello value for both policies in future steps.</span></span>

## <a name="download-and-configure-hello-project"></a><span data-ttu-id="dde8c-195">Stáhnout a nakonfigurovat hello projektu</span><span class="sxs-lookup"><span data-stu-id="dde8c-195">Download and configure hello project</span></span>

<span data-ttu-id="dde8c-196">Pomocí následujících toodownload hello projektu z Githubu hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-196">Use hello following toodownload hello project from GitHub.</span></span>

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

<span data-ttu-id="dde8c-197">Po dokončení příkazu hello máte hello následující adresářovou strukturu:</span><span class="sxs-lookup"><span data-stu-id="dde8c-197">After hello command completes, you have hello following directory structure:</span></span>

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains hello topology
            resources/
                log4j2.xml - set logging toominimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO toohello web dashboard.
        dashboard/nodejs/ - this is hello node.js web dashboard.
        SendEvents/ - utilities toosend fake sensor data.

> [!NOTE]
> <span data-ttu-id="dde8c-198">Tento dokument nepřekračuje v podrobnostech toofull hello kódu zahrnuty v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="dde8c-198">This document does not go in toofull details of hello code included in this example.</span></span> <span data-ttu-id="dde8c-199">Ale hello kód plně označené jako komentář.</span><span class="sxs-lookup"><span data-stu-id="dde8c-199">However, hello code is fully commented.</span></span>

<span data-ttu-id="dde8c-200">tooconfigure hello projektu tooread z centra událostí, otevřete hello `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` souboru a přidejte toohello informace vašeho centra událostí následující řádky:</span><span class="sxs-lookup"><span data-stu-id="dde8c-200">tooconfigure hello project tooread from Event Hub, open hello `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` file and add your Event Hub information toohello following lines:</span></span>

```bash
eventhub.read.policy.name: your_read_policy_name
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a><span data-ttu-id="dde8c-201">Kompilace a testování místně</span><span class="sxs-lookup"><span data-stu-id="dde8c-201">Compile and test locally</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dde8c-202">Použití topologie hello místně vyžaduje funkční Storm vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="dde8c-202">Using hello topology locally requires a working Storm development environment.</span></span> <span data-ttu-id="dde8c-203">Další informace najdete v tématu [nastavení vývojového prostředí Storm](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) na Apache.org.</span><span class="sxs-lookup"><span data-stu-id="dde8c-203">For more information, see [Setting up a Storm development environment](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) at Apache.org.</span></span>

> [!WARNING]
> <span data-ttu-id="dde8c-204">Pokud používáte vývojového prostředí systému Windows, může se zobrazit `java.io.IOException` při spuštění hello topologie místně.</span><span class="sxs-lookup"><span data-stu-id="dde8c-204">If you are using a Windows development environment, you may receive a `java.io.IOException` when running hello topology locally.</span></span> <span data-ttu-id="dde8c-205">Pokud ano, přesuňte na topologii hello toorunning v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dde8c-205">If so, move on toorunning hello topology on HDInsight.</span></span>

<span data-ttu-id="dde8c-206">Před testováním, musí začínat hello řídicí panel tooview hello výstup hello topologie a generování dat toostore v Centru událostí.</span><span class="sxs-lookup"><span data-stu-id="dde8c-206">Before testing, you must start hello dashboard tooview hello output of hello topology and generate data toostore in Event Hub.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dde8c-207">Hello HBase součást Tato topologie není aktivní, při testování místně.</span><span class="sxs-lookup"><span data-stu-id="dde8c-207">hello HBase component of this topology is not active when testing locally.</span></span> <span data-ttu-id="dde8c-208">Hello Java API pro hello HBase cluster není přístupná z hello mimo virtuální síť Azure, která obsahuje hello cluster.</span><span class="sxs-lookup"><span data-stu-id="dde8c-208">hello Java API for hello HBase cluster cannot be accessed from outside hello Azure Virtual Network that contains hello clusters.</span></span>

### <a name="start-hello-web-application"></a><span data-ttu-id="dde8c-209">Spustit hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="dde8c-209">Start hello web application</span></span>

1. <span data-ttu-id="dde8c-210">Otevřete příkazový řádek a změňte adresáře příliš`hdinsight-eventhub-example/dashboard`.</span><span class="sxs-lookup"><span data-stu-id="dde8c-210">Open a command prompt and change directories too`hdinsight-eventhub-example/dashboard`.</span></span> <span data-ttu-id="dde8c-211">Použijte následující příkaz tooinstall hello závislosti nutné hello webové aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="dde8c-211">Use hello following command tooinstall hello dependencies needed by hello web application:</span></span>
   
    ```bash
    npm install
    ```

2. <span data-ttu-id="dde8c-212">Použijte následující příkaz toostart hello webové aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="dde8c-212">Use hello following command toostart hello web application:</span></span>
   
    ```bash
    node server.js
    ```
   
    <span data-ttu-id="dde8c-213">Zobrazí zpráva podobná toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="dde8c-213">You see a message similar toohello following text:</span></span>
   
        Server listening at port 3000

3. <span data-ttu-id="dde8c-214">Otevřete webový prohlížeč a zadejte `http://localhost:3000/` jako adresa hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-214">Open a web browser and enter `http://localhost:3000/` as hello address.</span></span> <span data-ttu-id="dde8c-215">Zobrazí se stránka podobné toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="dde8c-215">A page similar toohello following image is displayed:</span></span>
   
    ![webové řídicí panel](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)
   
    <span data-ttu-id="dde8c-217">Nechte tato příkazového řádku otevřené.</span><span class="sxs-lookup"><span data-stu-id="dde8c-217">Leave this command prompt open.</span></span> <span data-ttu-id="dde8c-218">Po testování, pomocí kombinace kláves Ctrl-C toostop hello webový server.</span><span class="sxs-lookup"><span data-stu-id="dde8c-218">After testing, use Ctrl-C toostop hello web server.</span></span>

### <a name="generate-data"></a><span data-ttu-id="dde8c-219">Generování dat</span><span class="sxs-lookup"><span data-stu-id="dde8c-219">Generate data</span></span>

> [!NOTE]
> <span data-ttu-id="dde8c-220">Hello kroky v této části použijte Node.js tak, aby bylo možné na jakékoli platformě.</span><span class="sxs-lookup"><span data-stu-id="dde8c-220">hello steps in this section use Node.js so that they can be used on any platform.</span></span> <span data-ttu-id="dde8c-221">Další příklady jazyka najdete v tématu hello `SendEvents` adresáře.</span><span class="sxs-lookup"><span data-stu-id="dde8c-221">For other language examples, see hello `SendEvents` directory.</span></span>

1. <span data-ttu-id="dde8c-222">Otevřete nový řádek, prostředí nebo terminálu a změňte adresáře příliš`hdinsight-eventhub-example/SendEvents/nodejs`.</span><span class="sxs-lookup"><span data-stu-id="dde8c-222">Open a new prompt, shell, or terminal, and change directories too`hdinsight-eventhub-example/SendEvents/nodejs`.</span></span> <span data-ttu-id="dde8c-223">tooinstall hello závislosti potřebné hello aplikaci, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="dde8c-223">tooinstall hello dependencies needed by hello application, use hello following command:</span></span>

    ```bash
    npm install
    ```

2. <span data-ttu-id="dde8c-224">Otevřete hello `app.js` soubor v textovém editoru a přidejte hello informace centra událostí, které jste dříve získali:</span><span class="sxs-lookup"><span data-stu-id="dde8c-224">Open hello `app.js` file in a text editor and add hello Event Hub information you obtained earlier:</span></span>
   
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
   > <span data-ttu-id="dde8c-225">Tento příklad předpokládá, že jste použili `sensordata` jako hello název vašeho centra událostí.</span><span class="sxs-lookup"><span data-stu-id="dde8c-225">This example assumes that you have used `sensordata` as hello name of your Event Hub.</span></span> <span data-ttu-id="dde8c-226">A že `devices` jako název hello hello zásad, který má `Send` deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="dde8c-226">And that `devices` as hello name of hello policy that has a `Send` claim.</span></span>

3. <span data-ttu-id="dde8c-227">Použijte následující příkaz tooinsert nové položky v Centru událostí hello:</span><span class="sxs-lookup"><span data-stu-id="dde8c-227">Use hello following command tooinsert new entries in Event Hub:</span></span>
   
    ```bash
    node app.js
    ```
   
    <span data-ttu-id="dde8c-228">Zobrazí několik řádky výstupu, které obsahují hello data odesílání tooEvent rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="dde8c-228">You see several lines of output that contain hello data sent tooEvent Hub:</span></span>
   
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

### <a name="build-and-start-hello-topology"></a><span data-ttu-id="dde8c-229">Sestavte a spusťte hello topologie</span><span class="sxs-lookup"><span data-stu-id="dde8c-229">Build and start hello topology</span></span>

1. <span data-ttu-id="dde8c-230">Otevřete nový příkazový řádek a změňte adresáře příliš`hdinsight-eventhub-example/TemperatureMonitor`.</span><span class="sxs-lookup"><span data-stu-id="dde8c-230">Open a new command prompt and change directories too`hdinsight-eventhub-example/TemperatureMonitor`.</span></span> <span data-ttu-id="dde8c-231">toobuild a balíček hello topologie, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="dde8c-231">toobuild and package hello topology, use hello following command:</span></span> 

    ```bash
    mvn clean package
    ```

2. <span data-ttu-id="dde8c-232">topologie hello toostart v místním režimu, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dde8c-232">toostart hello topology in local mode, use hello following command:</span></span>

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * <span data-ttu-id="dde8c-233">`--local`Spustí hello topologie v místním režimu.</span><span class="sxs-lookup"><span data-stu-id="dde8c-233">`--local` starts hello topology in local mode.</span></span>
    * <span data-ttu-id="dde8c-234">`--filter`hello používá `dev.properties` souboru toopopulate parametry v definici topologie hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-234">`--filter` uses hello `dev.properties` file toopopulate parameters in hello topology definition.</span></span>
    * <span data-ttu-id="dde8c-235">`resources/no-hbase.yaml`hello používá `no-hbase.yaml` definice topologie.</span><span class="sxs-lookup"><span data-stu-id="dde8c-235">`resources/no-hbase.yaml` uses hello `no-hbase.yaml` topology definition.</span></span>
 
   <span data-ttu-id="dde8c-236">Po zahájení, hello topologie čte položky z centra událostí a odešle je řídicí panel toohello spuštěna na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="dde8c-236">Once started, hello topology reads entries from Event Hub, and sends them toohello dashboard running on your local machine.</span></span> <span data-ttu-id="dde8c-237">Měli byste vidět řádky v řídicím panelu webové hello, podobně jako toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="dde8c-237">You should see lines appear in hello web dashboard, similar toohello following image:</span></span>
   
    ![řídicí panel s daty](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

2. <span data-ttu-id="dde8c-239">Je spuštěn hello řídicí panel, použijte hello `node app.js` příkaz z hello předchozí kroky toosend nové tooEvent datového centra.</span><span class="sxs-lookup"><span data-stu-id="dde8c-239">While hello dashboard is running, use hello `node app.js` command from hello previous steps toosend new data tooEvent Hubs.</span></span> <span data-ttu-id="dde8c-240">Protože hello teploty hodnoty jsou náhodně vygenerované, by měl aktualizovat graf hello tooshow velké změny v teploty.</span><span class="sxs-lookup"><span data-stu-id="dde8c-240">Because hello temperature values are randomly generated, hello graph should update tooshow large changes in temperature.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="dde8c-241">Musí být v hello **hdinsight-eventhub – příklad/SendEvents/Nodejs** directory při použití hello `node app.js` příkaz.</span><span class="sxs-lookup"><span data-stu-id="dde8c-241">You must be in hello **hdinsight-eventhub-example/SendEvents/Nodejs** directory when using hello `node app.js` command.</span></span>

3. <span data-ttu-id="dde8c-242">Po ověření, tento řídicí panel hello aktualizací, zastavte hello topologie pomocí kombinace kláves Ctrl + C.</span><span class="sxs-lookup"><span data-stu-id="dde8c-242">After verifying that hello dashboard updates, stop hello topology using Ctrl+C.</span></span> <span data-ttu-id="dde8c-243">Také můžete použít kombinaci kláves Ctrl + C toostop hello místní webový server.</span><span class="sxs-lookup"><span data-stu-id="dde8c-243">You can use Ctrl+C toostop hello local web server also.</span></span>

## <a name="create-a-storm-and-hbase-cluster"></a><span data-ttu-id="dde8c-244">Vytvoření clusteru Storm a HBase</span><span class="sxs-lookup"><span data-stu-id="dde8c-244">Create a Storm and HBase cluster</span></span>

<span data-ttu-id="dde8c-245">Hello kroky v této části použijte [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md) toocreate clusteru služby Azure Virtual Network a Storm a HBase ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-245">hello steps in this section use an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md) toocreate an Azure Virtual Network and a Storm and HBase cluster on hello virtual network.</span></span> <span data-ttu-id="dde8c-246">šablony Hello také vytvoří Azure Web App a nasadí kopii hello řídicího panelu do ní.</span><span class="sxs-lookup"><span data-stu-id="dde8c-246">hello template also creates an Azure Web App and deploys a copy of hello dashboard into it.</span></span>

> [!NOTE]
> <span data-ttu-id="dde8c-247">Virtuální síť se používá, aby hello topologie běží v clusteru Storm hello přímo komunikovat s hello clusteru HBase pomocí hello HBase Java API.</span><span class="sxs-lookup"><span data-stu-id="dde8c-247">A virtual network is used so that hello topology running on hello Storm cluster can directly communicate with hello HBase cluster using hello HBase Java API.</span></span>

<span data-ttu-id="dde8c-248">Šablona Resource Manager Hello v tomto dokumentu se nachází v kontejneru veřejného objektu blob na **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.</span><span class="sxs-lookup"><span data-stu-id="dde8c-248">hello Resource Manager template used in this document is located in a public blob container at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.</span></span>

1. <span data-ttu-id="dde8c-249">Klikněte na následující tlačítko toosign v tooAzure a otevřete hello šablony Resource Manageru v hello portál Azure hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-249">Click hello following button toosign in tooAzure and open hello Resource Manager template in hello Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/hdinsight-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. <span data-ttu-id="dde8c-250">Z hello **vlastní nasazení** zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="dde8c-250">From hello **Custom deployment** section, enter hello following values:</span></span>
   
    ![Parametry HDInsight](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
   
   * <span data-ttu-id="dde8c-252">**Základní název clusteru**: Tato hodnota se používá jako hello základní název pro clustery Storm a HBase hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-252">**Base Cluster Name**: This value is used as hello base name for hello Storm and HBase clusters.</span></span> <span data-ttu-id="dde8c-253">Například zadáním **abc** vytvoří cluster Storm s názvem **storm abc** a cluster HBase s názvem **hbase abc**.</span><span class="sxs-lookup"><span data-stu-id="dde8c-253">For example, entering **abc** creates a Storm cluster named **storm-abc** and an HBase cluster named **hbase-abc**.</span></span>
   * <span data-ttu-id="dde8c-254">**Uživatelské jméno přihlášení clusteru**: uživatelské jméno správce hello u clusterů Storm a HBase hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-254">**Cluster Login User Name**: hello admin user name for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="dde8c-255">**Heslo pro přihlášení clusteru**: heslo uživatele správce hello u clusterů Storm a HBase hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-255">**Cluster Login Password**: hello admin user password for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="dde8c-256">**Uživatelské jméno SSH**: hello toocreate uživatele SSH pro clustery Storm a HBase hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-256">**SSH User Name**: hello SSH user toocreate for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="dde8c-257">**Heslo SSH**: hello heslo pro uživatele SSH hello u clusterů Storm a HBase hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-257">**SSH Password**: hello password for hello SSH user for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="dde8c-258">**Umístění**: hello oblast, která hello clustery jsou vytvořeny v.</span><span class="sxs-lookup"><span data-stu-id="dde8c-258">**Location**: hello region that hello clusters are created in.</span></span>
     
     <span data-ttu-id="dde8c-259">Klikněte na tlačítko **OK** toosave hello parametry.</span><span class="sxs-lookup"><span data-stu-id="dde8c-259">Click **OK** toosave hello parameters.</span></span>

3. <span data-ttu-id="dde8c-260">Použití hello **Základy** části toocreate skupinu prostředků nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="dde8c-260">Use hello **Basics** section toocreate a resource group or select an existing one.</span></span>
4. <span data-ttu-id="dde8c-261">V hello **umístění skupiny prostředků** rozevírací nabídce vyberte hello stejné umístění, jako jste vybrali pro hello **umístění** parametr v hello **nastavení** části.</span><span class="sxs-lookup"><span data-stu-id="dde8c-261">In hello **Resource group location** dropdown menu, select hello same location as you selected for hello **Location** parameter in hello **Settings** section.</span></span>
5. <span data-ttu-id="dde8c-262">Přečtěte si hello podmínky a ujednání a pak vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**.</span><span class="sxs-lookup"><span data-stu-id="dde8c-262">Read hello terms and conditions, and then select **I agree toohello terms and conditions stated above**.</span></span>
6. <span data-ttu-id="dde8c-263">Nakonec zkontrolujte **Pin toodashboard** a pak vyberte **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="dde8c-263">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="dde8c-264">Trvá přibližně 20 minut toocreate hello clustery.</span><span class="sxs-lookup"><span data-stu-id="dde8c-264">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="dde8c-265">Po vytvoření hello prostředky, zobrazí se informace o skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-265">Once hello resources have been created, information about hello resource group is displayed.</span></span>

![Skupinu prostředků pro virtuální síť hello a clustery](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="dde8c-267">Všimněte si, že jsou názvy hello clusterů HDInsight hello **storm BASENAME** a **hbase BASENAME**, kde BASENAME je hello jméno, které jste zadali toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="dde8c-267">Notice that hello names of hello HDInsight clusters are **storm-BASENAME** and **hbase-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="dde8c-268">Můžete použít tyto názvy v pozdější fázi při připojování toohello clustery.</span><span class="sxs-lookup"><span data-stu-id="dde8c-268">You use these names in a later step when connecting toohello clusters.</span></span> <span data-ttu-id="dde8c-269">Všimněte si, že hello název hello řídicího panelu webu je také **basename – řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="dde8c-269">Also note that hello name of hello dashboard site is **basename-dashboard**.</span></span> <span data-ttu-id="dde8c-270">Tato hodnota se používá později v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="dde8c-270">This value is used later in this document.</span></span>

## <a name="configure-hello-dashboard-bolt"></a><span data-ttu-id="dde8c-271">Konfigurace funkcí bolt hello řídicí panel</span><span class="sxs-lookup"><span data-stu-id="dde8c-271">Configure hello Dashboard bolt</span></span>

<span data-ttu-id="dde8c-272">toosend data toohello řídicí panel nasadit jako webovou aplikaci, musíte upravit hello následující řádek ve hello `dev.properties`souboru:</span><span class="sxs-lookup"><span data-stu-id="dde8c-272">toosend data toohello dashboard deployed as a web app, you must modify hello following line in hello `dev.properties`file:</span></span>

```yaml
dashboard.uri: http://localhost:3000
```

<span data-ttu-id="dde8c-273">Změna `http://localhost:3000` příliš`http://BASENAME-dashboard.azurewebsites.net` a uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-273">Change `http://localhost:3000` too`http://BASENAME-dashboard.azurewebsites.net` and save hello file.</span></span> <span data-ttu-id="dde8c-274">Nahraďte **BASENAME** základní názvem hello jste zadali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-274">Replace **BASENAME** with hello base name you provided in hello previous step.</span></span> <span data-ttu-id="dde8c-275">Můžete vytvořit také skupinu prostředků hello vytvořili dříve tooselect hello řídicí panely a zobrazení hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="dde8c-275">You can also use hello resource group created previously tooselect hello dashboard and view hello URL.</span></span>

## <a name="create-hello-hbase-table"></a><span data-ttu-id="dde8c-276">Vytvoření tabulky HBase hello</span><span class="sxs-lookup"><span data-stu-id="dde8c-276">Create hello HBase table</span></span>

<span data-ttu-id="dde8c-277">toostore data v HBase, jsme musíte nejdřív vytvořit tabulku.</span><span class="sxs-lookup"><span data-stu-id="dde8c-277">toostore data in HBase, we must first create a table.</span></span> <span data-ttu-id="dde8c-278">Předem vytvořit prostředky, které potřebuje Storm toowrite k, protože snažíme toocreate prostředky z uvnitř topologie Storm může způsobit pokusu o několik instancí toocreate hello stejného zdroje.</span><span class="sxs-lookup"><span data-stu-id="dde8c-278">Pre-create resources that Storm needs toowrite to, as trying toocreate resources from inside a Storm topology can result in multiple instances trying toocreate hello same resource.</span></span> <span data-ttu-id="dde8c-279">Vytvořte hello prostředky mimo hello topologie a používat Storm pro čtení/zápis a analýzy.</span><span class="sxs-lookup"><span data-stu-id="dde8c-279">Create hello resources outside hello topology and use Storm for reading/writing and analytics.</span></span>

1. <span data-ttu-id="dde8c-280">Pomocí SSH tooconnect toohello clusteru HBase pomocí hello SSH uživatele a heslo zadané toohello šablonu při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="dde8c-280">Use SSH tooconnect toohello HBase cluster using hello SSH user and password you supplied toohello template during cluster creation.</span></span> <span data-ttu-id="dde8c-281">Například, pokud připojení pomocí hello `ssh` příkaz, využije hello následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="dde8c-281">For example, if connecting using hello `ssh` command, you would use hello following syntax:</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    <span data-ttu-id="dde8c-282">Nahraďte `sshuser` s uživatelským jménem SSH hello jste zadali při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-282">Replace `sshuser` with hello SSH user name you provided when creating hello cluster.</span></span> <span data-ttu-id="dde8c-283">Nahraďte `clustername` s názvem clusteru HBase hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-283">Replace `clustername` with hello HBase cluster name.</span></span>

2. <span data-ttu-id="dde8c-284">Z relace SSH hello spusťte prostředí HBase hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-284">From hello SSH session, start hello HBase shell.</span></span>
   
    ```bash
    hbase shell
    ```
   
    <span data-ttu-id="dde8c-285">Jakmile hello prostředí načetl, zobrazí `hbase(main):001:0>` řádku.</span><span class="sxs-lookup"><span data-stu-id="dde8c-285">Once hello shell has loaded, you see an `hbase(main):001:0>` prompt.</span></span>

3. <span data-ttu-id="dde8c-286">Z hello prostředí HBase zadejte následující příkaz toocreate dat snímačů hello v tabulce toostore hello:</span><span class="sxs-lookup"><span data-stu-id="dde8c-286">From hello HBase shell, enter hello following command toocreate a table toostore hello sensor data:</span></span>
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. <span data-ttu-id="dde8c-287">Ověřte, zda že tato hello tabulka byla vytvořena pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dde8c-287">Verify that hello table has been created by using hello following command:</span></span>
   
    ```hbase
    scan 'SensorData'
    ```
   
    <span data-ttu-id="dde8c-288">Tento příkaz vrátí informace podobné toohello následující ukázka, označující, že jsou v tabulce hello 0 řádků.</span><span class="sxs-lookup"><span data-stu-id="dde8c-288">This returns information similar toohello following example, indicating that there are 0 rows in hello table.</span></span>
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. <span data-ttu-id="dde8c-289">Zadejte `exit` tooexit hello prostředí HBase:</span><span class="sxs-lookup"><span data-stu-id="dde8c-289">Enter `exit` tooexit hello HBase shell:</span></span>

## <a name="configure-hello-hbase-bolt"></a><span data-ttu-id="dde8c-290">Konfigurace funkcí bolt HBase hello</span><span class="sxs-lookup"><span data-stu-id="dde8c-290">Configure hello HBase bolt</span></span>

<span data-ttu-id="dde8c-291">toowrite tooHBase z clusteru Storm hello, je nutné zadat hello HBase bolt s hello podrobnosti o konfiguraci vašeho clusteru HBase.</span><span class="sxs-lookup"><span data-stu-id="dde8c-291">toowrite tooHBase from hello Storm cluster, you must provide hello HBase bolt with hello configuration details of your HBase cluster.</span></span>

1. <span data-ttu-id="dde8c-292">Použijte jednu z hello následující příklady tooretrieve hello Zookeeper kvora pro váš cluster HBase:</span><span class="sxs-lookup"><span data-stu-id="dde8c-292">Use one of hello following examples tooretrieve hello Zookeeper quorum for your HBase cluster:</span></span>

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > <span data-ttu-id="dde8c-293">Nahraďte `your_HDInsight_cluster_name` s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dde8c-293">Replace `your_HDInsight_cluster_name` with hello name of your HDInsight cluster.</span></span> <span data-ttu-id="dde8c-294">Další informace o instalaci hello `jq` nástroj, najdete v části [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).</span><span class="sxs-lookup"><span data-stu-id="dde8c-294">For more information on installing hello `jq` utility, see [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).</span></span>
    >
    > <span data-ttu-id="dde8c-295">Po zobrazení výzvy zadejte přihlašovací jméno správce HDInsight hello hello heslo.</span><span class="sxs-lookup"><span data-stu-id="dde8c-295">When prompted, enter hello password for hello HDInsight admin login.</span></span>

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name`
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > <span data-ttu-id="dde8c-296">Nahraďte ' your_HDInsight_cluster_name s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dde8c-296">Replace \`your_HDInsight_cluster_name with hello name of your HDInsight cluster.</span></span> <span data-ttu-id="dde8c-297">Po zobrazení výzvy zadejte přihlašovací jméno správce HDInsight hello hello heslo.</span><span class="sxs-lookup"><span data-stu-id="dde8c-297">When prompted, enter hello password for hello HDInsight admin login.</span></span>
    >
    > <span data-ttu-id="dde8c-298">Tento příklad vyžaduje prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dde8c-298">This example requires Azure PowerShell.</span></span> <span data-ttu-id="dde8c-299">Další informace o použití prostředí Azure PowerShell najdete v tématu [Začínáme s Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)</span><span class="sxs-lookup"><span data-stu-id="dde8c-299">For more information on using Azure PowerShell, see [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)</span></span>

    <span data-ttu-id="dde8c-300">informace Hello vrácený tyto příklady jsou podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="dde8c-300">hello information returned by these examples is similar toohello following text:</span></span>

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    <span data-ttu-id="dde8c-301">Tyto informace slouží toocommunicate Storm s clusterem HBase hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-301">This information is used by Storm toocommunicate with hello HBase cluster.</span></span>

2. <span data-ttu-id="dde8c-302">Upravit hello `dev.properties` souboru a přidejte hello Zookeeper kvora informace toohello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="dde8c-302">Modify hello `dev.properties` file and add hello Zookeeper quorum information toohello following line:</span></span>

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-hello-solution-toohdinsight"></a><span data-ttu-id="dde8c-303">Vytvoření balíčku a nasadit řešení tooHDInsight hello</span><span class="sxs-lookup"><span data-stu-id="dde8c-303">Build, package, and deploy hello solution tooHDInsight</span></span>

<span data-ttu-id="dde8c-304">Ve vašem vývojovém prostředí použijte následující kroky toodeploy hello Storm topologie toohello cluster storm hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-304">In your development environment, use hello following steps toodeploy hello Storm topology toohello storm cluster.</span></span>

1. <span data-ttu-id="dde8c-305">Z hello `TemperatureMonitor` adresář, použijte hello následující příkaz tooperform nového sestavení a vytvoření balíčku JAR z projektu:</span><span class="sxs-lookup"><span data-stu-id="dde8c-305">From hello `TemperatureMonitor` directory, use hello following command tooperform a new build and create a JAR package from your project:</span></span>
   
        mvn clean package
   
    <span data-ttu-id="dde8c-306">Tento příkaz vytvoří soubor s názvem `TemperatureMonitor-1.0-SNAPSHOT.jar in hello `cíl ' adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="dde8c-306">This command creates a file named `TemperatureMonitor-1.0-SNAPSHOT.jar in hello `target\` directory of your project.</span></span>

2. <span data-ttu-id="dde8c-307">Použití spojovací bod služby tooupload hello `TemperatureMonitor-1.0-SNAPSHOT.jar` a `dev.properties` cluster Storm tooyour soubory.</span><span class="sxs-lookup"><span data-stu-id="dde8c-307">Use scp tooupload hello `TemperatureMonitor-1.0-SNAPSHOT.jar` and `dev.properties` files tooyour Storm cluster.</span></span> <span data-ttu-id="dde8c-308">Následující příklad, nahraďte v hello `sshuser` uživatele SSH hello jste zadali při vytváření clusteru hello a `clustername` s názvem hello clusteru Storm.</span><span class="sxs-lookup"><span data-stu-id="dde8c-308">In hello following example, replace `sshuser` with hello SSH user you provided when creating hello cluster, and `clustername` with hello name of your Storm cluster.</span></span> <span data-ttu-id="dde8c-309">Po zobrazení výzvy zadejte hello heslo pro uživatele SSH hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-309">When prompted, enter hello password for hello SSH user.</span></span>
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > <span data-ttu-id="dde8c-310">Soubory hello tooupload může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="dde8c-310">It may take several minutes tooupload hello files.</span></span>

    <span data-ttu-id="dde8c-311">Další informace o používání hello `scp` a `ssh` příkazy s HDInsight, najdete v části [použití SSH s HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="dde8c-311">For more information on using hello `scp` and `ssh` commands with HDInsight, see [Use SSH with HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

3. <span data-ttu-id="dde8c-312">Po odeslání hello souboru, připojte cluster Storm toohello pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="dde8c-312">Once hello file has been uploaded, connect toohello Storm cluster using SSH.</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="dde8c-313">Nahraďte `sshuser` s uživatelským jménem SSH hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-313">Replace `sshuser` with hello SSH user name.</span></span> <span data-ttu-id="dde8c-314">Nahraďte `clustername` s názvem clusteru Storm hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-314">Replace `clustername` with hello Storm cluster name.</span></span>

4. <span data-ttu-id="dde8c-315">toostart hello topologie, použijte následující příkaz z relace SSH hello hello:</span><span class="sxs-lookup"><span data-stu-id="dde8c-315">toostart hello topology, use hello following command from hello SSH session:</span></span>
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * <span data-ttu-id="dde8c-316">`--remote`odešle hello topologie toohello Nimbus služba, která distribuuje jej toohello nadřízeného uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-316">`--remote` submits hello topology toohello Nimbus service, which distributes it toohello supervisor nodes in hello cluster.</span></span>
    * <span data-ttu-id="dde8c-317">`--filter`hello používá `dev.properties` souboru toopopulate parametry v definici topologie hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-317">`--filter` uses hello `dev.properties` file toopopulate parameters in hello topology definition.</span></span>
    * <span data-ttu-id="dde8c-318">`-R /with-hbase.yaml`hello používá `with-hbase.yaml` topologie, které jsou součástí balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-318">`-R /with-hbase.yaml` uses hello `with-hbase.yaml` topology included in hello package.</span></span>

5. <span data-ttu-id="dde8c-319">Po spuštění hello topologie, otevřete web toohello prohlížeče jste publikovali v Azure, a pak použijte hello `node app.js` příkaz toosend data tooEvent rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="dde8c-319">After hello topology has started, open a browser toohello website you published on Azure, then use hello `node app.js` command toosend data tooEvent Hub.</span></span> <span data-ttu-id="dde8c-320">Měli byste vidět hello webový řídicím panelu toodisplay hello informace o aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="dde8c-320">You should see hello web dashboard update toodisplay hello information.</span></span>
   
    ![řídicí panel](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a><span data-ttu-id="dde8c-322">Zobrazení dat HBase</span><span class="sxs-lookup"><span data-stu-id="dde8c-322">View HBase data</span></span>

<span data-ttu-id="dde8c-323">Použijte následující postup tooconnect tooHBase hello a ověřte, že hello data byla zapsána toohello tabulky:</span><span class="sxs-lookup"><span data-stu-id="dde8c-323">Use hello following steps tooconnect tooHBase and verify that hello data has been written toohello table:</span></span>

1. <span data-ttu-id="dde8c-324">Použijte SSH tooconnect toohello HBase cluster.</span><span class="sxs-lookup"><span data-stu-id="dde8c-324">Use SSH tooconnect toohello HBase cluster.</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="dde8c-325">Nahraďte `sshuser` s uživatelským jménem SSH hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-325">Replace `sshuser` with hello SSH user name.</span></span> <span data-ttu-id="dde8c-326">Nahraďte `clustername` s názvem clusteru HBase hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-326">Replace `clustername` with hello HBase cluster name.</span></span>

2. <span data-ttu-id="dde8c-327">Z relace SSH hello spusťte prostředí HBase hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-327">From hello SSH session, start hello HBase shell.</span></span>
   
    ```bash
    hbase shell
    ```
   
    <span data-ttu-id="dde8c-328">Jakmile hello prostředí načetl, zobrazí `hbase(main):001:0>` řádku.</span><span class="sxs-lookup"><span data-stu-id="dde8c-328">Once hello shell has loaded, you see an `hbase(main):001:0>` prompt.</span></span>

3. <span data-ttu-id="dde8c-329">Zobrazení řádků z tabulky hello:</span><span class="sxs-lookup"><span data-stu-id="dde8c-329">View rows from hello table:</span></span>
   
    ```hbase
    scan 'SensorData'
    ```
   
    <span data-ttu-id="dde8c-330">Tento příkaz vrátí informace podobné toohello následující text, označující, že je v tabulce hello data.</span><span class="sxs-lookup"><span data-stu-id="dde8c-330">This command returns information similar toohello following text, indicating that there is data in hello table.</span></span>
   
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
   > <span data-ttu-id="dde8c-331">Tato operace kontroly vrátí maximálně 10 řádků z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="dde8c-331">This scan operation returns a maximum of 10 rows from hello table.</span></span>

## <a name="delete-your-clusters"></a><span data-ttu-id="dde8c-332">Odstranění clusterů</span><span class="sxs-lookup"><span data-stu-id="dde8c-332">Delete your clusters</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="dde8c-333">toodelete hello clustery, úložiště a webové aplikace v jednom okamžiku odstranit hello skupinu prostředků, který je obsahuje.</span><span class="sxs-lookup"><span data-stu-id="dde8c-333">toodelete hello clusters, storage, and web app at one time, delete hello resource group that contains them.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dde8c-334">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dde8c-334">Next steps</span></span>

<span data-ttu-id="dde8c-335">Další příklady topologií Storm s HDInsight naleznete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)</span><span class="sxs-lookup"><span data-stu-id="dde8c-335">For more examples of Storm topologies with HDInsight, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md)</span></span>

<span data-ttu-id="dde8c-336">Další informace o Apache Storm naleznete v tématu hello [Apache Storm](https://storm.incubator.apache.org/) lokality.</span><span class="sxs-lookup"><span data-stu-id="dde8c-336">For more information about Apache Storm, see hello [Apache Storm](https://storm.incubator.apache.org/) site.</span></span>

<span data-ttu-id="dde8c-337">Další informace o HBase v HDInsight, naleznete v části hello [HBase s HDInsight přehled](hdinsight-hbase-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dde8c-337">For more information about HBase on HDInsight, see hello [HBase with HDInsight Overview](hdinsight-hbase-overview.md).</span></span>

<span data-ttu-id="dde8c-338">Další informace o Socket.io najdete v tématu hello [socket.io](http://socket.io/) lokality.</span><span class="sxs-lookup"><span data-stu-id="dde8c-338">For more information about Socket.io, see hello [socket.io](http://socket.io/) site.</span></span>

<span data-ttu-id="dde8c-339">Další informace o D3.js najdete v tématu [D3.js - dokumenty řízené daty](http://d3js.org/).</span><span class="sxs-lookup"><span data-stu-id="dde8c-339">For more information about D3.js, see [D3.js - Data Driven Documents](http://d3js.org/).</span></span>

<span data-ttu-id="dde8c-340">Informace o vytváření topologie v jazyce Java najdete v tématu [Java vývoj topologií pro Apache Storm v HDInsight](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="dde8c-340">For information about creating topologies in Java, see [Develop Java topologies for Apache Storm on HDInsight](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="dde8c-341">Informace o vytváření topologie v rozhraní .NET najdete v tématu [vývoj topologií C# pro Apache Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="dde8c-341">For information about creating topologies in .NET, see [Develop C# topologies for Apache Storm on HDInsight using Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

[azure-portal]: https://portal.azure.com
