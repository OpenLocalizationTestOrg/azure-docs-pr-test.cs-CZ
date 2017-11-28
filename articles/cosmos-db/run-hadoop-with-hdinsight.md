---
title: "aaaRun Hadoop úlohy pomocí Azure Cosmos DB a HDInsight | Microsoft Docs"
description: "Zjistěte, jak toorun jednoduché Hive, Pig a MapReduce úlohy s Azure Cosmos DB a Azure HDInsight."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 06f0ea9d-07cb-4593-a9c5-ab912b62ac42
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/08/2017
ms.author: denlee
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2e27499f2c4ba951af9a1ade1bcc9c1b6d298fcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="474d6-103"><a name="Azure Cosmos DB-HDInsight"></a>Spustit úlohu Apache Hive, Pig nebo Hadoop pomocí Azure Cosmos DB a HDInsight</span><span class="sxs-lookup"><span data-stu-id="474d6-103"><a name="Azure Cosmos DB-HDInsight"></a>Run an Apache Hive, Pig, or Hadoop job using Azure Cosmos DB and HDInsight</span></span>
<span data-ttu-id="474d6-104">Tento kurz ukazuje, jak toorun [Apache Hive][apache-hive], [Apache Pig][apache-pig], a [Apache Hadoop] [ apache-hadoop] Úloh MapReduce v Azure HDInsight s konektorem Hadoop Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="474d6-104">This tutorial shows you how toorun [Apache Hive][apache-hive], [Apache Pig][apache-pig], and [Apache Hadoop][apache-hadoop] MapReduce jobs on Azure HDInsight with Cosmos DB's Hadoop connector.</span></span> <span data-ttu-id="474d6-105">Cosmos DB na Hadoop konektor umožňuje tooact Cosmos DB jako zdroj a jímka pro úlohy Hive, Pig a MapReduce.</span><span class="sxs-lookup"><span data-stu-id="474d6-105">Cosmos DB's Hadoop connector allows Cosmos DB tooact as both a source and sink for Hive, Pig, and MapReduce jobs.</span></span> <span data-ttu-id="474d6-106">V tomto kurzu použije Cosmos DB jako hello data zdrojového a cílového pro úlohy Hadoop.</span><span class="sxs-lookup"><span data-stu-id="474d6-106">This tutorial will use Cosmos DB as both hello data source and destination for Hadoop jobs.</span></span>

<span data-ttu-id="474d6-107">Po dokončení tohoto kurzu, budete moct tooanswer hello následující otázky:</span><span class="sxs-lookup"><span data-stu-id="474d6-107">After completing this tutorial, you'll be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="474d6-108">Jak načíst data z databáze Cosmos pomocí úlohy Hive, Pig nebo MapReduce?</span><span class="sxs-lookup"><span data-stu-id="474d6-108">How do I load data from Cosmos DB using a Hive, Pig, or MapReduce job?</span></span>
* <span data-ttu-id="474d6-109">Jak ukládat data do databáze Cosmos. pomocí úlohy Hive, Pig nebo MapReduce?</span><span class="sxs-lookup"><span data-stu-id="474d6-109">How do I store data in Cosmos DB using a Hive, Pig, or MapReduce job?</span></span>

<span data-ttu-id="474d6-110">Doporučujeme začít sledování hello následující video, které jsme spuštění prostřednictvím úlohy Hive pomocí Cosmos databáze a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="474d6-110">We recommend getting started by watching hello following video, where we run through a Hive job using Cosmos DB and HDInsight.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

<span data-ttu-id="474d6-111">Pak se vraťte toothis článek, kterou budete dostávat hello úplné podrobnosti o jak spouštět úlohy analytics na vaše data Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="474d6-111">Then, return toothis article, where you'll receive hello full details on how you can run analytics jobs on your Cosmos DB data.</span></span>

> [!TIP]
> <span data-ttu-id="474d6-112">Tento kurz předpokládá, že máte zkušenosti s jazykem Apache Hadoop, Hive a Pig.</span><span class="sxs-lookup"><span data-stu-id="474d6-112">This tutorial assumes that you have prior experience using Apache Hadoop, Hive, and/or Pig.</span></span> <span data-ttu-id="474d6-113">Pokud jste nový tooApache Hadoop, Hive a Pig, doporučujeme navštívit hello [dokumentaci Apache Hadoop][apache-hadoop-doc].</span><span class="sxs-lookup"><span data-stu-id="474d6-113">If you are new tooApache Hadoop, Hive, and Pig, we recommend visiting hello [Apache Hadoop documentation][apache-hadoop-doc].</span></span> <span data-ttu-id="474d6-114">V tomto kurzu také předpokládá, že máte zkušenosti s Cosmos DB a máte účet Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="474d6-114">This tutorial also assumes that you have prior experience with Cosmos DB and have a Cosmos DB account.</span></span> <span data-ttu-id="474d6-115">Pokud jste nový tooCosmos DB nebo nemáte účet Cosmos DB, podrobnosti naleznete v našem [Začínáme] [ getting-started] stránky.</span><span class="sxs-lookup"><span data-stu-id="474d6-115">If you are new tooCosmos DB or you do not have a Cosmos DB account, please check out our [Getting Started][getting-started] page.</span></span>
>
>

<span data-ttu-id="474d6-116">Nemáte čas toocomplete hello kurzu a právě chcete tooget hello úplnou ukázku najdete na PowerShell skripty pro Hive, Pig a MapReduce?</span><span class="sxs-lookup"><span data-stu-id="474d6-116">Don't have time toocomplete hello tutorial and just want tooget hello full sample PowerShell scripts for Hive, Pig, and MapReduce?</span></span> <span data-ttu-id="474d6-117">Nejedná se o problém, je získání [sem][hdinsight-samples].</span><span class="sxs-lookup"><span data-stu-id="474d6-117">Not a problem, get them [here][hdinsight-samples].</span></span> <span data-ttu-id="474d6-118">stažení Hello také obsahuje hello hql, pig a java soubory pro tyto ukázky.</span><span class="sxs-lookup"><span data-stu-id="474d6-118">hello download also contains hello hql, pig, and java files for these samples.</span></span>

## <span data-ttu-id="474d6-119"><a name="NewestVersion"></a>Nejnovější verze</span><span class="sxs-lookup"><span data-stu-id="474d6-119"><a name="NewestVersion"></a>Newest Version</span></span>
<table border='1'>
    <tr><th><span data-ttu-id="474d6-120">Verze konektoru Hadoop</span><span class="sxs-lookup"><span data-stu-id="474d6-120">Hadoop Connector Version</span></span></th>
        <td><span data-ttu-id="474d6-121">1.2.0</span><span class="sxs-lookup"><span data-stu-id="474d6-121">1.2.0</span></span></td></tr>
    <tr><th><span data-ttu-id="474d6-122">Identifikátor Uri skriptu</span><span class="sxs-lookup"><span data-stu-id="474d6-122">Script Uri</span></span></th>
        <td><span data-ttu-id="474d6-123">https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</span><span class="sxs-lookup"><span data-stu-id="474d6-123">https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</span></span></td></tr>
    <tr><th><span data-ttu-id="474d6-124">Datum změny</span><span class="sxs-lookup"><span data-stu-id="474d6-124">Date Modified</span></span></th>
        <td><span data-ttu-id="474d6-125">04/26/2016</span><span class="sxs-lookup"><span data-stu-id="474d6-125">04/26/2016</span></span></td></tr>
    <tr><th><span data-ttu-id="474d6-126">Podporované HDInsight verze</span><span class="sxs-lookup"><span data-stu-id="474d6-126">Supported HDInsight Versions</span></span></th>
        <td><span data-ttu-id="474d6-127">3.1, 3.2</span><span class="sxs-lookup"><span data-stu-id="474d6-127">3.1, 3.2</span></span></td></tr>
    <tr><th><span data-ttu-id="474d6-128">Protokol změn</span><span class="sxs-lookup"><span data-stu-id="474d6-128">Change Log</span></span></th>
        <td><span data-ttu-id="474d6-129">Aktualizované Cosmos Azure DB Java SDK too1.6.0</span><span class="sxs-lookup"><span data-stu-id="474d6-129">Updated Azure Cosmos DB Java SDK too1.6.0</span></span></br>
            <span data-ttu-id="474d6-130">Přidaná podpora pro dělené kolekce jako zdroj a jímka</span><span class="sxs-lookup"><span data-stu-id="474d6-130">Added support for partitioned collections as both a source and sink</span></span></br>
        </td></tr>
</table>

## <span data-ttu-id="474d6-131"><a name="Prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="474d6-131"><a name="Prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="474d6-132">Než budete postupovat hello pokyny v tomto kurzu, zajistěte, abyste měli hello následující:</span><span class="sxs-lookup"><span data-stu-id="474d6-132">Before following hello instructions in this tutorial, ensure that you have hello following:</span></span>

* <span data-ttu-id="474d6-133">Účet Cosmos DB, databázi a kolekci s dokumenty uvnitř.</span><span class="sxs-lookup"><span data-stu-id="474d6-133">A Cosmos DB account, a database, and a collection with documents inside.</span></span> <span data-ttu-id="474d6-134">Další informace najdete v tématu [Začínáme s Cosmos DB][getting-started].</span><span class="sxs-lookup"><span data-stu-id="474d6-134">For more information, see [Getting Started with Cosmos DB][getting-started].</span></span> <span data-ttu-id="474d6-135">Import ukázkových dat do účtu Cosmos DB s hello [nástroj pro import Cosmos DB][import-data].</span><span class="sxs-lookup"><span data-stu-id="474d6-135">Import sample data into your Cosmos DB account with hello [Cosmos DB import tool][import-data].</span></span>
* <span data-ttu-id="474d6-136">Propustnost.</span><span class="sxs-lookup"><span data-stu-id="474d6-136">Throughput.</span></span> <span data-ttu-id="474d6-137">Čtení a zápisy z prostředí HDInsight započítají vůči vaší jednotek přiděleného žádosti pro kolekce.</span><span class="sxs-lookup"><span data-stu-id="474d6-137">Reads and writes from HDInsight will be counted towards your allotted request units for your collections.</span></span>
* <span data-ttu-id="474d6-138">Kapacitu pro další uloženou proceduru v každém výstupní kolekce.</span><span class="sxs-lookup"><span data-stu-id="474d6-138">Capacity for an additional stored procedure within each output collection.</span></span> <span data-ttu-id="474d6-139">Hello uložené procedury používají přenosu výsledné dokumentů.</span><span class="sxs-lookup"><span data-stu-id="474d6-139">hello stored procedures are used for transferring resulting documents.</span></span>
* <span data-ttu-id="474d6-140">Kapacita hello výsledné dokumenty z úlohy Hive, Pig nebo MapReduce hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-140">Capacity for hello resulting documents from hello Hive, Pig, or MapReduce jobs.</span></span>
* <span data-ttu-id="474d6-141">[*Volitelné*] kapacity pro další kolekci.</span><span class="sxs-lookup"><span data-stu-id="474d6-141">[*Optional*] Capacity for an additional collection.</span></span>

> [!WARNING]
> <span data-ttu-id="474d6-142">V pořadí tooavoid hello vytvoření nové kolekce v žádné z hello úlohy můžete buď tisku hello výsledky toostdout, uložit kontejneru WASB tooyour výstup hello nebo zadat již existující kolekci.</span><span class="sxs-lookup"><span data-stu-id="474d6-142">In order tooavoid hello creation of a new collection during any of hello jobs, you can either print hello results toostdout, save hello output tooyour WASB container, or specify an already existing collection.</span></span> <span data-ttu-id="474d6-143">V případě hello zadávání existující kolekci se vytvoří nové dokumenty uvnitř hello kolekce a stávající dokumenty bude mít vliv pouze pokud dojde ke konfliktu v *ID*.</span><span class="sxs-lookup"><span data-stu-id="474d6-143">In hello case of specifying an existing collection, new documents will be created inside hello collection and already existing documents will only be affected if there is a conflict in *ids*.</span></span> <span data-ttu-id="474d6-144">**konektor Hello automaticky přepíše stávající dokumenty s docházet ke konfliktům id**.</span><span class="sxs-lookup"><span data-stu-id="474d6-144">**hello connector will automatically overwrite existing documents with id conflicts**.</span></span> <span data-ttu-id="474d6-145">Tuto funkci můžete vypnout pomocí nastavení toofalse možnost upsert hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-145">You can turn off this feature by setting hello upsert option toofalse.</span></span> <span data-ttu-id="474d6-146">Pokud je hodnota false upsert a dojde ke konfliktu, se nezdaří úlohy Hadoop hello; vytváření sestav chybu id konflikt.</span><span class="sxs-lookup"><span data-stu-id="474d6-146">If upsert is false and a conflict occurs, hello Hadoop job will fail; reporting an id conflict error.</span></span>
>
>

## <span data-ttu-id="474d6-147"><a name="ProvisionHDInsight"></a>Krok 1: Vytvoření nového clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="474d6-147"><a name="ProvisionHDInsight"></a>Step 1: Create a new HDInsight cluster</span></span>
<span data-ttu-id="474d6-148">Tento kurz používá akce skriptu z portálu Azure toocustomize hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="474d6-148">This tutorial uses Script Action from hello Azure Portal toocustomize your HDInsight cluster.</span></span> <span data-ttu-id="474d6-149">V tomto kurzu budeme používat portál Azure toocreate hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="474d6-149">In this tutorial, we will use hello Azure Portal toocreate your HDInsight cluster.</span></span> <span data-ttu-id="474d6-150">Pokyny, jak toouse rutiny prostředí PowerShell nebo hello SDK rozhraní .NET HDInsight, podívejte se [HDInsight přizpůsobit clustery pomocí akce skriptu] [ hdinsight-custom-provision] článku.</span><span class="sxs-lookup"><span data-stu-id="474d6-150">For instructions on how toouse PowerShell cmdlets or hello HDInsight .NET SDK, check out the [Customize HDInsight clusters using Script Action][hdinsight-custom-provision] article.</span></span>

1. <span data-ttu-id="474d6-151">Přihlaste se toohello [portálu Azure][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="474d6-151">Sign in toohello [Azure Portal][azure-portal].</span></span>
2. <span data-ttu-id="474d6-152">Klikněte na tlačítko **+ nový** na hello horní části hello levé navigační, vyhledejte **HDInsight** v hello horním panelu vyhledávání v novém okně hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-152">Click **+ New** on hello top of hello left navigation, search for **HDInsight** in hello top search bar on hello New blade.</span></span>
3. <span data-ttu-id="474d6-153">**HDInsight** publikováno **Microsoft** se zobrazí v horní části hello hello výsledků.</span><span class="sxs-lookup"><span data-stu-id="474d6-153">**HDInsight** published by **Microsoft** will appear at hello top of hello Results.</span></span> <span data-ttu-id="474d6-154">Klikněte na něj a potom na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="474d6-154">Click on it and then click **Create**.</span></span>
4. <span data-ttu-id="474d6-155">Na nový HDInsight Cluster hello vytvořit okno, zadejte vaše **název clusteru** a vyberte hello **předplatné** chcete tooprovision tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="474d6-155">On hello New HDInsight Cluster create blade, enter your **Cluster Name** and select hello **Subscription** you want tooprovision this resource under.</span></span>

    <table border='1'>
        <tr><td><span data-ttu-id="474d6-156">Název clusteru</span><span class="sxs-lookup"><span data-stu-id="474d6-156">Cluster name</span></span></td><td><span data-ttu-id="474d6-157">Název clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-157">Name hello cluster.</span></span><br/>
<span data-ttu-id="474d6-158">Název serveru DNS musí spustit a končit znakem alpha číselné a může obsahovat pomlčky.</span><span class="sxs-lookup"><span data-stu-id="474d6-158">DNS name must start and end with an alpha numeric character, and may contain dashes.</span></span><br/>
<span data-ttu-id="474d6-159">Hello pole musí být řetězec o délce 3 až 63 znaků.</span><span class="sxs-lookup"><span data-stu-id="474d6-159">hello field must be a string between 3 and 63 characters.</span></span></td></tr>
        <tr><td><span data-ttu-id="474d6-160">Název odběru</span><span class="sxs-lookup"><span data-stu-id="474d6-160">Subscription Name</span></span></td>
            <td><span data-ttu-id="474d6-161">Pokud máte více než jedno předplatné Azure, vyberte předplatné hello, který bude hostitelem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="474d6-161">If you have more than one Azure Subscription, select hello subscription that will host your HDInsight cluster.</span></span> </td></tr>
    </table><span data-ttu-id="474d6-162">
5.Klikněte na tlačítko **vybrat typ clusteru** a následující vlastnosti toohello hello sadu zadané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="474d6-162">
5. Click **Select Cluster Type** and set hello following properties toohello specified values.</span></span>

    <table border='1'>
        <tr><td><span data-ttu-id="474d6-163">Typ clusteru</span><span class="sxs-lookup"><span data-stu-id="474d6-163">Cluster type</span></span></td><td><span data-ttu-id="474d6-164"><strong>Hadoop</strong></span><span class="sxs-lookup"><span data-stu-id="474d6-164"><strong>Hadoop</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="474d6-165">Vrstvy clusteru</span><span class="sxs-lookup"><span data-stu-id="474d6-165">Cluster tier</span></span></td><td><span data-ttu-id="474d6-166"><strong>Standard</strong></span><span class="sxs-lookup"><span data-stu-id="474d6-166"><strong>Standard</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="474d6-167">Operační systém</span><span class="sxs-lookup"><span data-stu-id="474d6-167">Operating System</span></span></td><td><span data-ttu-id="474d6-168"><strong>Windows</strong></span><span class="sxs-lookup"><span data-stu-id="474d6-168"><strong>Windows</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="474d6-169">Verze</span><span class="sxs-lookup"><span data-stu-id="474d6-169">Version</span></span></td><td><span data-ttu-id="474d6-170">nejnovější verzi</span><span class="sxs-lookup"><span data-stu-id="474d6-170">latest version</span></span></td></tr>
    </table>

    <span data-ttu-id="474d6-171">Nyní, klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="474d6-171">Now, click **SELECT**.</span></span>

    ![Zadejte podrobnosti o počáteční clusteru Hadoop HDInsight][image-customprovision-page1]
6. <span data-ttu-id="474d6-173">Klikněte na **pověření** tooset přihlašovací jméno a přihlašovací údaje vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="474d6-173">Click on **Credentials** tooset your login and remote access credentials.</span></span> <span data-ttu-id="474d6-174">Zvolte vaše **uživatelské jméno přihlášení clusteru** a **clusteru heslo pro přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="474d6-174">Choose your **Cluster Login Username** and **Cluster Login Password**.</span></span>

    <span data-ttu-id="474d6-175">Pokud chcete tooremote do clusteru, vyberte *Ano* v hello dolní části okna hello a zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="474d6-175">If you want tooremote into your cluster, select *yes* at hello bottom of hello blade and provide a username and password.</span></span>
7. <span data-ttu-id="474d6-176">Klikněte na **zdroj dat** přístup k vaší primární umístění pro data tooset.</span><span class="sxs-lookup"><span data-stu-id="474d6-176">Click on **Data Source** tooset your primary location for data access.</span></span> <span data-ttu-id="474d6-177">Zvolte hello **metodu výběru** a zadejte již existující účet úložiště nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="474d6-177">Choose hello **Selection Method** and specify an already existing storage account or create a new one.</span></span>
8. <span data-ttu-id="474d6-178">Na hello stejné okno, zadejte **výchozí kontejner** a **umístění**.</span><span class="sxs-lookup"><span data-stu-id="474d6-178">On hello same blade, specify a **Default Container** and a **Location**.</span></span> <span data-ttu-id="474d6-179">A klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="474d6-179">And, click **SELECT**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="474d6-180">Vyberte zavřít tooyour umístění Cosmos DB oblast účtu pro lepší výkon</span><span class="sxs-lookup"><span data-stu-id="474d6-180">Select a location close tooyour Cosmos DB account region for better performance</span></span>
   >
   >
9. <span data-ttu-id="474d6-181">Klikněte na **cenová** tooselect hello počet a typ uzlů.</span><span class="sxs-lookup"><span data-stu-id="474d6-181">Click on **Pricing** tooselect hello number and type of nodes.</span></span> <span data-ttu-id="474d6-182">Můžete později na zachovat hello výchozí konfigurace a škálování hello počet uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="474d6-182">You can keep hello default configuration and scale hello number of Worker nodes later on.</span></span>
10. <span data-ttu-id="474d6-183">Klikněte na tlačítko **volitelné konfiguraci**, pak **akcí skriptů** v hello volitelné konfigurace okna.</span><span class="sxs-lookup"><span data-stu-id="474d6-183">Click **Optional Configuration**, then **Script Actions** in hello Optional Configuration Blade.</span></span>

     <span data-ttu-id="474d6-184">Akce skriptu zadejte následující informace toocustomize hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="474d6-184">In Script Actions, enter hello following information toocustomize your HDInsight cluster.</span></span>

     <table border='1'>
         <tr><th><span data-ttu-id="474d6-185">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="474d6-185">Property</span></span></th><th><span data-ttu-id="474d6-186">Hodnota</span><span class="sxs-lookup"><span data-stu-id="474d6-186">Value</span></span></th></tr>
         <tr><td><span data-ttu-id="474d6-187">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="474d6-187">Name</span></span></td>
             <td><span data-ttu-id="474d6-188">Zadejte název akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-188">Specify a name for hello script action.</span></span></td></tr>
         <tr><td><span data-ttu-id="474d6-189">Identifikátor URI skriptu</span><span class="sxs-lookup"><span data-stu-id="474d6-189">Script URI</span></span></td>
             <td><span data-ttu-id="474d6-190">Zadejte hello URI toohello skript, který je vyvolaná toocustomize hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="474d6-190">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span></br></br>
<span data-ttu-id="474d6-191">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="474d6-191">Please enter:</span></span> </br> <span data-ttu-id="474d6-192"><strong>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</strong>.</span><span class="sxs-lookup"><span data-stu-id="474d6-192"><strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="474d6-193">Hlavní uzel</span><span class="sxs-lookup"><span data-stu-id="474d6-193">Head</span></span></td>
             <td><span data-ttu-id="474d6-194">Klikněte na tlačítko hello políčko toorun hello skript prostředí PowerShell na hello hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="474d6-194">Click hello checkbox toorun hello PowerShell script onto hello Head node.</span></span></br></br><span data-ttu-id="474d6-195">
             <strong>Zaškrtněte toto políčko</strong>.</span><span class="sxs-lookup"><span data-stu-id="474d6-195">
             <strong>Check this checkbox</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="474d6-196">Worker</span><span class="sxs-lookup"><span data-stu-id="474d6-196">Worker</span></span></td>
             <td><span data-ttu-id="474d6-197">Klikněte na tlačítko skript prostředí PowerShell hello políčko toorun hello na hello pracovního uzlu.</span><span class="sxs-lookup"><span data-stu-id="474d6-197">Click hello checkbox toorun hello PowerShell script onto hello Worker node.</span></span></br></br><span data-ttu-id="474d6-198">
             <strong>Zaškrtněte toto políčko</strong>.</span><span class="sxs-lookup"><span data-stu-id="474d6-198">
             <strong>Check this checkbox</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="474d6-199">Zookeeper</span><span class="sxs-lookup"><span data-stu-id="474d6-199">Zookeeper</span></span></td>
             <td><span data-ttu-id="474d6-200">Klikněte na tlačítko skript prostředí PowerShell hello políčko toorun hello na hello Zookeeper.</span><span class="sxs-lookup"><span data-stu-id="474d6-200">Click hello checkbox toorun hello PowerShell script onto hello Zookeeper.</span></span></br></br><span data-ttu-id="474d6-201">
             <strong>Není potřeba</strong>.</span><span class="sxs-lookup"><span data-stu-id="474d6-201">
             <strong>Not needed</strong>.</span></span>
             </td></tr>
         <tr><td><span data-ttu-id="474d6-202">Parametry</span><span class="sxs-lookup"><span data-stu-id="474d6-202">Parameters</span></span></td>
             <td><span data-ttu-id="474d6-203">Zadejte parametry hello, pokud to vyžaduje hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="474d6-203">Specify hello parameters, if required by hello script.</span></span></br></br><span data-ttu-id="474d6-204">
             <strong>Žádné parametry potřeby</strong>.</span><span class="sxs-lookup"><span data-stu-id="474d6-204">
             <strong>No Parameters needed</strong>.</span></span></td></tr>
     </table><span data-ttu-id="474d6-205">
11.Buď vytvořit novou **skupiny prostředků** nebo použít existující skupinu prostředků v rámci svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="474d6-205">
11. Create either a new **Resource Group** or use an existing Resource Group under your Azure Subscription.</span></span>
<span data-ttu-id="474d6-206">12.</span><span class="sxs-lookup"><span data-stu-id="474d6-206">12.</span></span> <span data-ttu-id="474d6-207">Teď, zkontrolujte **Pin toodashboard** tootrack jeho nasazení a klikněte na tlačítko **vytvořit**!</span><span class="sxs-lookup"><span data-stu-id="474d6-207">Now, check **Pin toodashboard** tootrack its deployment and click **Create**!</span></span>

## <span data-ttu-id="474d6-208"><a name="InstallCmdlets"></a>Krok 2: Instalace a konfigurace prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="474d6-208"><a name="InstallCmdlets"></a>Step 2: Install and configure Azure PowerShell</span></span>
1. <span data-ttu-id="474d6-209">Nainstalujte Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="474d6-209">Install Azure PowerShell.</span></span> <span data-ttu-id="474d6-210">Pokyny naleznete [sem][powershell-install-configure].</span><span class="sxs-lookup"><span data-stu-id="474d6-210">Instructions can be found [here][powershell-install-configure].</span></span>

   > [!NOTE]
   > <span data-ttu-id="474d6-211">Jenom pro dotazy Hive, případně můžete použít HDInsight je online Editor Hive.</span><span class="sxs-lookup"><span data-stu-id="474d6-211">Alternatively, just for Hive queries, you can use HDInsight's online Hive Editor.</span></span> <span data-ttu-id="474d6-212">toodo tedy přihlásit toohello [portálu Azure][azure-portal], klikněte na tlačítko **HDInsight** tooview levém podokně na hello seznam clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="474d6-212">toodo so, sign in toohello [Azure Portal][azure-portal], click **HDInsight** on hello left pane tooview a list of your HDInsight clusters.</span></span> <span data-ttu-id="474d6-213">Klikněte na cluster hello dotazů Hive toorun na a pak klikněte na tlačítko **dotazu konzoly**.</span><span class="sxs-lookup"><span data-stu-id="474d6-213">Click hello cluster you want toorun Hive queries on, and then click **Query Console**.</span></span>
   >
   >
2. <span data-ttu-id="474d6-214">Otevřete hello Azure PowerShell Integrované skriptovací prostředí:</span><span class="sxs-lookup"><span data-stu-id="474d6-214">Open hello Azure PowerShell Integrated Scripting Environment:</span></span>

   * <span data-ttu-id="474d6-215">V počítači se systémem Windows 8 nebo Windows Server 2012 nebo vyšší, můžete použít předdefinované hello vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="474d6-215">On a computer running Windows 8 or Windows Server 2012 or higher, you can use hello built-in Search.</span></span> <span data-ttu-id="474d6-216">Na úvodní obrazovce hello zadejte **prostředí powershell ise** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="474d6-216">From hello Start screen, type **powershell ise** and click **Enter**.</span></span>
   * <span data-ttu-id="474d6-217">V počítači se systémem starším než Windows 8 nebo Windows Server 2012 na verzi použijte nabídku Start hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-217">On a computer running a version earlier than Windows 8 or Windows Server 2012, use hello Start menu.</span></span> <span data-ttu-id="474d6-218">Z nabídky Start hello, zadejte **příkazového řádku** hello vyhledávacího pole a pak v seznamu hello výsledků, klikněte na tlačítko **příkazového řádku**.</span><span class="sxs-lookup"><span data-stu-id="474d6-218">From hello Start menu, type **Command Prompt** in hello search box, then in hello list of results, click **Command Prompt**.</span></span> <span data-ttu-id="474d6-219">Hello příkazového řádku, zadejte **powershell_ise** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="474d6-219">In hello Command Prompt, type **powershell_ise** and click **Enter**.</span></span>
3. <span data-ttu-id="474d6-220">Přidání účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="474d6-220">Add your Azure Account.</span></span>

   1. <span data-ttu-id="474d6-221">V podokně konzoly hello, zadejte **Add-AzureAccount** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="474d6-221">In hello Console Pane, type **Add-AzureAccount** and click **Enter**.</span></span>
   2. <span data-ttu-id="474d6-222">Zadejte e-mailovou adresu hello spojené s předplatným Azure a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="474d6-222">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span>
   3. <span data-ttu-id="474d6-223">Zadejte heslo hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="474d6-223">Type in hello password for your Azure subscription.</span></span>
   4. <span data-ttu-id="474d6-224">Klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="474d6-224">Click **Sign in**.</span></span>
4. <span data-ttu-id="474d6-225">Následující diagram Hello identifikuje hello důležitou součástí prostředí Azure PowerShell skriptování.</span><span class="sxs-lookup"><span data-stu-id="474d6-225">hello following diagram identifies hello important parts of your Azure PowerShell Scripting Environment.</span></span>

    ![Diagram pro prostředí Azure PowerShell][azure-powershell-diagram]

## <span data-ttu-id="474d6-227"><a name="RunHive"></a>Krok 3: Spuštění úlohy Hive pomocí Cosmos databáze a HDInsight</span><span class="sxs-lookup"><span data-stu-id="474d6-227"><a name="RunHive"></a>Step 3: Run a Hive job using Cosmos DB and HDInsight</span></span>
> [!IMPORTANT]
> <span data-ttu-id="474d6-228">Všechny proměnné indikován < > musí být zadána pomocí nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="474d6-228">All variables indicated by < > must be filled in using your configuration settings.</span></span>
>
>

1. <span data-ttu-id="474d6-229">Nastavit hello následující proměnné v váš skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="474d6-229">Set hello following variables in your PowerShell Script pane.</span></span>

        # Provide Azure subscription name, hello Azure Storage account and container that is used for hello default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide hello HDInsight cluster name where you want toorun hello Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p><span data-ttu-id="474d6-230">Začněme, vytváření řetězec vašeho dotazu.</span><span class="sxs-lookup"><span data-stu-id="474d6-230">Let's begin constructing your query string.</span></span> <span data-ttu-id="474d6-231">Jsme budete napsat dotaz Hive, které trvá vygenerované systémem časová razítka (_ts) a jedinečné ID (_rid) z Azure Cosmos DB kolekce všechny dokumenty, zaznamená všechny dokumenty o hello minutu a pak uloží výsledky hello zpět do nové kolekce Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="474d6-231">We'll write a Hive query that takes all documents' system generated timestamps (_ts) and unique ids (_rid) from an Azure Cosmos DB collection, tallies all documents by hello minute, and then stores hello results back into a new Azure Cosmos DB collection.</span></span></p>

    <p><span data-ttu-id="474d6-232">Nejdříve vytvoříme tabulku Hive z našich kolekce Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="474d6-232">First, let's create a Hive table from our Azure Cosmos DB collection.</span></span> <span data-ttu-id="474d6-233">Přidejte následující kód fragment kódu toohello podokno skriptu prostředí PowerShell hello <strong>po</strong> fragment kódu hello od #1.</span><span class="sxs-lookup"><span data-stu-id="474d6-233">Add hello following code snippet toohello PowerShell Script pane <strong>after</strong> hello code snippet from #1.</span></span> <span data-ttu-id="474d6-234">Ujistěte se, že naše _ts toojust dokumenty a _rid obsahuje hello volitelné DocumentDB.query parametr t uvolnění dočasné paměti.</span><span class="sxs-lookup"><span data-stu-id="474d6-234">Make sure you include hello optional DocumentDB.query parameter t trim our documents toojust _ts and _rid.</span></span></p>

   > [!NOTE]
   > <span data-ttu-id="474d6-235">**Pojmenování DocumentDB.inputCollections se jedná o chybu.**</span><span class="sxs-lookup"><span data-stu-id="474d6-235">**Naming DocumentDB.inputCollections was not a mistake.**</span></span> <span data-ttu-id="474d6-236">Ano, jsme povolit přidávání více kolekcí jako vstup:</span><span class="sxs-lookup"><span data-stu-id="474d6-236">Yes, we allow adding multiple collections as an input:</span></span> </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> hello collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from DocumentDB. Pass DocumentDB hello query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. <span data-ttu-id="474d6-237">V dalším kroku vytvoříme tabulku Hive pro kolekci výstup hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-237">Next, let's create a Hive table for hello output collection.</span></span> <span data-ttu-id="474d6-238">Vlastnosti dokumentu výstup Hello bude hello měsíc, den, hodinu, minutu a celkový počet výskytů hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-238">hello output document properties will be hello month, day, hour, minute, and hello total number of occurrences.</span></span>

   > [!NOTE]
   > <span data-ttu-id="474d6-239">**Ještě znovu pojmenování DocumentDB.outputCollections se jedná o chybu.**</span><span class="sxs-lookup"><span data-stu-id="474d6-239">**Yet again, naming DocumentDB.outputCollections was not a mistake.**</span></span> <span data-ttu-id="474d6-240">Ano, jsme povolit přidávání více kolekcí jako výstup:</span><span class="sxs-lookup"><span data-stu-id="474d6-240">Yes, we allow adding multiple collections as an output:</span></span> </br>
   > <span data-ttu-id="474d6-241">'*DocumentDB.outputCollections*'='*\<název kolekce DocumentDB výstupu 1\>*,*\<název kolekce DocumentDB výstupu 2\>*.</span><span class="sxs-lookup"><span data-stu-id="474d6-241">'*DocumentDB.outputCollections*' = '*\<DocumentDB Output Collection Name 1\>*,*\<DocumentDB Output Collection Name 2\>*'</span></span> </br> <span data-ttu-id="474d6-242">názvy Hello kolekce jsou oddělené bez mezer pouze jeden čárkou.</span><span class="sxs-lookup"><span data-stu-id="474d6-242">hello collection names are separated without spaces, using only a single comma.</span></span> </br></br>
   > <span data-ttu-id="474d6-243">Dokumenty bude distribuované kruhového dotazování napříč více kolekcí.</span><span class="sxs-lookup"><span data-stu-id="474d6-243">Documents will be distributed round-robin across multiple collections.</span></span> <span data-ttu-id="474d6-244">Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v hello další kolekce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="474d6-244">A batch of documents will be stored in one collection, then a second batch of documents will be stored in hello next collection, and so forth.</span></span>
   >
   >

       # Create a Hive table for hello output data tooDocumentDB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. <span data-ttu-id="474d6-245">Nakonec umožňuje tally hello dokumentů měsíc, den, hodinu a minutu a vložení hello výsledky zpět do hello výstupní tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="474d6-245">Finally, let's tally hello documents by month, day, hour, and minute and insert hello results back into hello output Hive table.</span></span>

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. <span data-ttu-id="474d6-246">Přidejte následující fragment kódu toocreate skriptu definice úlohy Hive z předchozího dotazu hello hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-246">Add hello following script snippet toocreate a Hive job definition from hello previous query.</span></span>

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    <span data-ttu-id="474d6-247">Můžete také použít hello – soubor přepínač toospecify soubor skriptu HiveQL na HDFS.</span><span class="sxs-lookup"><span data-stu-id="474d6-247">You can also use hello -File switch toospecify a HiveQL script file on HDFS.</span></span>
4. <span data-ttu-id="474d6-248">Přidejte následující fragment kódu toosave hello počáteční čas hello a odeslat úlohu Hive hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-248">Add hello following snippet toosave hello start time and submit hello Hive job.</span></span>

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. <span data-ttu-id="474d6-249">Přidejte následující toowait pro toocomplete úlohy Hive hello hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-249">Add hello following toowait for hello Hive job toocomplete.</span></span>

        # Wait for hello Hive job toocomplete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. <span data-ttu-id="474d6-250">Přidejte následující počáteční hello tooprint standardní výstupní zařízení a hello hello a ukončení.</span><span class="sxs-lookup"><span data-stu-id="474d6-250">Add hello following tooprint hello standard output and hello start and end times.</span></span>

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. <span data-ttu-id="474d6-251">**Spustit** nový skript!</span><span class="sxs-lookup"><span data-stu-id="474d6-251">**Run** your new script!</span></span> <span data-ttu-id="474d6-252">**Klikněte na tlačítko** hello zelená provést tlačítko.</span><span class="sxs-lookup"><span data-stu-id="474d6-252">**Click** hello green execute button.</span></span>
8. <span data-ttu-id="474d6-253">Zkontrolujte výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-253">Check hello results.</span></span> <span data-ttu-id="474d6-254">Přihlaste se k hello [portálu Azure][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="474d6-254">Sign into hello [Azure Portal][azure-portal].</span></span>

   1. <span data-ttu-id="474d6-255">Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-255">Click <strong>Browse</strong> on hello left-side panel.</span></span> </br>
   2. <span data-ttu-id="474d6-256">Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním hello hello procházet panelu.</span><span class="sxs-lookup"><span data-stu-id="474d6-256">Click <strong>everything</strong> at hello top-right of hello browse panel.</span></span> </br>
   3. <span data-ttu-id="474d6-257">Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>.</span><span class="sxs-lookup"><span data-stu-id="474d6-257">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span> </br>
   4. <span data-ttu-id="474d6-258">Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené zadaná v kolekce výstup hello dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="474d6-258">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with hello output collection specified in your Hive query.</span></span></br>
   5. <span data-ttu-id="474d6-259">Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.</span><span class="sxs-lookup"><span data-stu-id="474d6-259">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span></br></p>

   <span data-ttu-id="474d6-260">Zobrazí se hello výsledky dotazu Hive.</span><span class="sxs-lookup"><span data-stu-id="474d6-260">You will see hello results of your Hive query.</span></span>

   ![Výsledky dotazu Hive][image-hive-query-results]

## <span data-ttu-id="474d6-262"><a name="RunPig"></a>Krok 4: Spuštění úlohy Pig pomocí Cosmos databáze a HDInsight</span><span class="sxs-lookup"><span data-stu-id="474d6-262"><a name="RunPig"></a>Step 4: Run a Pig job using Cosmos DB and HDInsight</span></span>
> [!IMPORTANT]
> <span data-ttu-id="474d6-263">Všechny proměnné indikován < > musí být zadána pomocí nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="474d6-263">All variables indicated by < > must be filled in using your configuration settings.</span></span>
>
>

1. <span data-ttu-id="474d6-264">Nastavit hello následující proměnné v váš skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="474d6-264">Set hello following variables in your PowerShell Script pane.</span></span>

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want toorun hello Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p><span data-ttu-id="474d6-265">Začněme, vytváření řetězec vašeho dotazu.</span><span class="sxs-lookup"><span data-stu-id="474d6-265">Let's begin constructing your query string.</span></span> <span data-ttu-id="474d6-266">Jsme budete napsat dotaz Pig, které trvá vygenerované systémem časová razítka (_ts) a jedinečné ID (_rid) z Azure Cosmos DB kolekce všechny dokumenty, zaznamená všechny dokumenty o hello minutu a pak uloží výsledky hello zpět do nové kolekce Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="474d6-266">We'll write a Pig query that takes all documents' system generated timestamps (_ts) and unique ids (_rid) from an Azure Cosmos DB collection, tallies all documents by hello minute, and then stores hello results back into a new Azure Cosmos DB collection.</span></span></p>
    <p><span data-ttu-id="474d6-267">Nejdřív načíst dokumenty z databáze Cosmos do HDInsight.</span><span class="sxs-lookup"><span data-stu-id="474d6-267">First, load documents from Cosmos DB into HDInsight.</span></span> <span data-ttu-id="474d6-268">Přidejte následující kód fragment kódu toohello podokno skriptu prostředí PowerShell hello <strong>po</strong> fragment kódu hello od #1.</span><span class="sxs-lookup"><span data-stu-id="474d6-268">Add hello following code snippet toohello PowerShell Script pane <strong>after</strong> hello code snippet from #1.</span></span> <span data-ttu-id="474d6-269">Zajistěte, aby naše _ts toojust dokumenty a _rid dotazů tooadd DocumentDB tootrim parametr dotazu volitelné DocumentDB toohello.</span><span class="sxs-lookup"><span data-stu-id="474d6-269">Make sure tooadd a DocumentDB query toohello optional DocumentDB query parameter tootrim our documents toojust _ts and _rid.</span></span></p>

   > [!NOTE]
   > <span data-ttu-id="474d6-270">Ano, jsme povolit přidávání více kolekcí jako vstup:</span><span class="sxs-lookup"><span data-stu-id="474d6-270">Yes, we allow adding multiple collections as an input:</span></span> </br>
   > <span data-ttu-id="474d6-271">'*\<Název kolekce DocumentDB vstup 1\>*,*\<název kolekce DocumentDB vstup 2\>*.</span><span class="sxs-lookup"><span data-stu-id="474d6-271">'*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*'</span></span></br> <span data-ttu-id="474d6-272">názvy Hello kolekce jsou oddělené bez mezer pouze jeden čárkou.</span><span class="sxs-lookup"><span data-stu-id="474d6-272">hello collection names are separated without spaces, using only a single comma.</span></span> </b>
   >
   >

    <span data-ttu-id="474d6-273">Dokumenty bude distribuované kruhového dotazování napříč více kolekcí.</span><span class="sxs-lookup"><span data-stu-id="474d6-273">Documents will be distributed round-robin across multiple collections.</span></span> <span data-ttu-id="474d6-274">Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v hello další kolekce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="474d6-274">A batch of documents will be stored in one collection, then a second batch of documents will be stored in hello next collection, and so forth.</span></span>

        # Load data from Cosmos DB. Pass DocumentDB query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. <span data-ttu-id="474d6-275">Dále umožňuje tally hello dokumentů podle hello měsíc, den, hodinu, minutu a celkový počet výskytů hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-275">Next, let's tally hello documents by hello month, day, hour, minute, and hello total number of occurrences.</span></span>

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. <span data-ttu-id="474d6-276">Umožňuje uložit nakonec hello výsledky do naší nové kolekce výstup.</span><span class="sxs-lookup"><span data-stu-id="474d6-276">Finally, let's store hello results into our new output collection.</span></span>

   > [!NOTE]
   > <span data-ttu-id="474d6-277">Ano, jsme povolit přidávání více kolekcí jako výstup:</span><span class="sxs-lookup"><span data-stu-id="474d6-277">Yes, we allow adding multiple collections as an output:</span></span> </br>
   > <span data-ttu-id="474d6-278">'\<Název kolekce DocumentDB výstupu 1\>,\<název kolekce DocumentDB výstupu 2\>.</span><span class="sxs-lookup"><span data-stu-id="474d6-278">'\<DocumentDB Output Collection Name 1\>,\<DocumentDB Output Collection Name 2\>'</span></span></br> <span data-ttu-id="474d6-279">názvy Hello kolekce jsou oddělené bez mezer pouze jeden čárkou.</span><span class="sxs-lookup"><span data-stu-id="474d6-279">hello collection names are separated without spaces, using only a single comma.</span></span></br>
   > <span data-ttu-id="474d6-280">Dokumenty budou být distribuované kruhového dotazování v rámci hello více kolekcí.</span><span class="sxs-lookup"><span data-stu-id="474d6-280">Documents will be distributed round-robin across hello multiple collections.</span></span> <span data-ttu-id="474d6-281">Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v hello další kolekce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="474d6-281">A batch of documents will be stored in one collection, then a second batch of documents will be stored in hello next collection, and so forth.</span></span>
   >
   >

        # Store output data tooCosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. <span data-ttu-id="474d6-282">Přidejte následující fragment kódu toocreate skriptu definice úlohy Pig z předchozího dotazu hello hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-282">Add hello following script snippet toocreate a Pig job definition from hello previous query.</span></span>

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    <span data-ttu-id="474d6-283">Můžete také použít hello – soubor přepínač toospecify soubor skriptu Pig na HDFS.</span><span class="sxs-lookup"><span data-stu-id="474d6-283">You can also use hello -File switch toospecify a Pig script file on HDFS.</span></span>
6. <span data-ttu-id="474d6-284">Přidejte následující fragment kódu toosave hello počáteční čas hello a odeslat úlohu Pig hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-284">Add hello following snippet toosave hello start time and submit hello Pig job.</span></span>

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. <span data-ttu-id="474d6-285">Přidejte následující toowait pro toocomplete úlohy Pig hello hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-285">Add hello following toowait for hello Pig job toocomplete.</span></span>

        # Wait for hello Pig job toocomplete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. <span data-ttu-id="474d6-286">Přidejte následující počáteční hello tooprint standardní výstupní zařízení a hello hello a ukončení.</span><span class="sxs-lookup"><span data-stu-id="474d6-286">Add hello following tooprint hello standard output and hello start and end times.</span></span>

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. <span data-ttu-id="474d6-287">**Spustit** nový skript!</span><span class="sxs-lookup"><span data-stu-id="474d6-287">**Run** your new script!</span></span> <span data-ttu-id="474d6-288">**Klikněte na tlačítko** hello zelená provést tlačítko.</span><span class="sxs-lookup"><span data-stu-id="474d6-288">**Click** hello green execute button.</span></span>
10. <span data-ttu-id="474d6-289">Zkontrolujte výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-289">Check hello results.</span></span> <span data-ttu-id="474d6-290">Přihlaste se k hello [portálu Azure][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="474d6-290">Sign into hello [Azure Portal][azure-portal].</span></span>

    1. <span data-ttu-id="474d6-291">Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-291">Click <strong>Browse</strong> on hello left-side panel.</span></span> </br>
    2. <span data-ttu-id="474d6-292">Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním hello hello procházet panelu.</span><span class="sxs-lookup"><span data-stu-id="474d6-292">Click <strong>everything</strong> at hello top-right of hello browse panel.</span></span> </br>
    3. <span data-ttu-id="474d6-293">Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>.</span><span class="sxs-lookup"><span data-stu-id="474d6-293">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span> </br>
    4. <span data-ttu-id="474d6-294">Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené zadaná v kolekce výstup hello Pig dotazu.</span><span class="sxs-lookup"><span data-stu-id="474d6-294">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with hello output collection specified in your Pig query.</span></span></br>
    5. <span data-ttu-id="474d6-295">Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.</span><span class="sxs-lookup"><span data-stu-id="474d6-295">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span></br></p>

    <span data-ttu-id="474d6-296">Zobrazí se výsledky dotazu Pig hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-296">You will see hello results of your Pig query.</span></span>

    ![Výsledky dotazu pig][image-pig-query-results]

## <span data-ttu-id="474d6-298"><a name="RunMapReduce"></a>Krok 5: Spustit úlohu MapReduce pomocí Azure Cosmos DB a HDInsight</span><span class="sxs-lookup"><span data-stu-id="474d6-298"><a name="RunMapReduce"></a>Step 5: Run a MapReduce job using Azure Cosmos DB and HDInsight</span></span>
1. <span data-ttu-id="474d6-299">Nastavit hello následující proměnné v váš skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="474d6-299">Set hello following variables in your PowerShell Script pane.</span></span>

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. <span data-ttu-id="474d6-300">Jsme budete spustit úlohu MapReduce, která sečte hello počet výskytů pro každou vlastnost dokumentu z kolekce Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="474d6-300">We'll execute a MapReduce job that tallies hello number of occurrences for each Document property from your Azure Cosmos DB collection.</span></span> <span data-ttu-id="474d6-301">Přidejte tento fragment skriptu **po** výše fragmentu hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-301">Add this script snippet **after** hello snippet above.</span></span>

        # Define hello MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > <span data-ttu-id="474d6-302">TallyProperties v01.jar se dodává s hello vlastní instalace hello Cosmos DB Hadoop konektor.</span><span class="sxs-lookup"><span data-stu-id="474d6-302">TallyProperties-v01.jar comes with hello custom installation of hello Cosmos DB Hadoop Connector.</span></span>
   >
   >
3. <span data-ttu-id="474d6-303">Přidejte následující úlohu MapReduce hello toosubmit příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-303">Add hello following command toosubmit hello MapReduce job.</span></span>

        # Save hello start time and submit hello job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    <span data-ttu-id="474d6-304">Kromě toho toohello definice úlohy MapReduce, je také zadat název clusteru HDInsight hello, kam chcete úlohu MapReduce hello toorun a přihlašovací údaje hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-304">In addition toohello MapReduce job definition, you also provide hello HDInsight cluster name where you want toorun hello MapReduce job, and hello credentials.</span></span> <span data-ttu-id="474d6-305">Hello počáteční AzureHDInsightJob je synchronního volání.</span><span class="sxs-lookup"><span data-stu-id="474d6-305">hello Start-AzureHDInsightJob is an asynchronized call.</span></span> <span data-ttu-id="474d6-306">dokončení hello toocheck hello úlohy, použijte hello *čekání AzureHDInsightJob* rutiny.</span><span class="sxs-lookup"><span data-stu-id="474d6-306">toocheck hello completion of hello job, use hello *Wait-AzureHDInsightJob* cmdlet.</span></span>
4. <span data-ttu-id="474d6-307">Přidejte následující příkaz toocheck hello žádné chyby u spuštěná úloha MapReduce hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-307">Add hello following command toocheck any errors with running hello MapReduce job.</span></span>

        # Get hello job output and print hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. <span data-ttu-id="474d6-308">**Spustit** nový skript!</span><span class="sxs-lookup"><span data-stu-id="474d6-308">**Run** your new script!</span></span> <span data-ttu-id="474d6-309">**Klikněte na tlačítko** hello zelená provést tlačítko.</span><span class="sxs-lookup"><span data-stu-id="474d6-309">**Click** hello green execute button.</span></span>
6. <span data-ttu-id="474d6-310">Zkontrolujte výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-310">Check hello results.</span></span> <span data-ttu-id="474d6-311">Přihlaste se k hello [portálu Azure][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="474d6-311">Sign into hello [Azure Portal][azure-portal].</span></span>

   1. <span data-ttu-id="474d6-312">Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu hello.</span><span class="sxs-lookup"><span data-stu-id="474d6-312">Click <strong>Browse</strong> on hello left-side panel.</span></span>
   2. <span data-ttu-id="474d6-313">Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním hello hello procházet panelu.</span><span class="sxs-lookup"><span data-stu-id="474d6-313">Click <strong>everything</strong> at hello top-right of hello browse panel.</span></span>
   3. <span data-ttu-id="474d6-314">Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>.</span><span class="sxs-lookup"><span data-stu-id="474d6-314">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span>
   4. <span data-ttu-id="474d6-315">Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené zadaná v kolekce výstup hello úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="474d6-315">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with hello output collection specified in your MapReduce job.</span></span>
   5. <span data-ttu-id="474d6-316">Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.</span><span class="sxs-lookup"><span data-stu-id="474d6-316">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span>

      <span data-ttu-id="474d6-317">Zobrazí se hello výsledků úlohy MapReduce.</span><span class="sxs-lookup"><span data-stu-id="474d6-317">You will see hello results of your MapReduce job.</span></span>

      ![MapReduce výsledky dotazu][image-mapreduce-query-results]

## <span data-ttu-id="474d6-319"><a name="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="474d6-319"><a name="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="474d6-320">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="474d6-320">Congratulations!</span></span> <span data-ttu-id="474d6-321">Právě jste spustili vaše první úlohy Hive, Pig a MapReduce pomocí Azure Cosmos DB a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="474d6-321">You just ran your first Hive, Pig, and MapReduce jobs using Azure Cosmos DB and HDInsight.</span></span>

<span data-ttu-id="474d6-322">Máme open source naše konektor Hadoop.</span><span class="sxs-lookup"><span data-stu-id="474d6-322">We have open sourced our Hadoop Connector.</span></span> <span data-ttu-id="474d6-323">Pokud byste chtěli, můžete přispívat na [Githubu][github].</span><span class="sxs-lookup"><span data-stu-id="474d6-323">If you're interested, you can contribute on [GitHub][github].</span></span>

<span data-ttu-id="474d6-324">toolearn více, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="474d6-324">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="474d6-325">[Vývoj aplikace Java pomocí Documentdb][documentdb-java-application]</span><span class="sxs-lookup"><span data-stu-id="474d6-325">[Develop a Java application with Documentdb][documentdb-java-application]</span></span>
* <span data-ttu-id="474d6-326">[Vývoj aplikací Java MapReduce pro Hadoop v HDInsight][hdinsight-develop-deploy-java-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="474d6-326">[Develop Java MapReduce programs for Hadoop in HDInsight][hdinsight-develop-deploy-java-mapreduce]</span></span>
* <span data-ttu-id="474d6-327">[Začínáme používat Hadoop s Hive v HDInsight tooanalyze mobilního telefonu použití][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="474d6-327">[Get started using Hadoop with Hive in HDInsight tooanalyze mobile handset use][hdinsight-get-started]</span></span>
* <span data-ttu-id="474d6-328">[Používání nástroje MapReduce s HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="474d6-328">[Use MapReduce with HDInsight][hdinsight-use-mapreduce]</span></span>
* <span data-ttu-id="474d6-329">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="474d6-329">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="474d6-330">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="474d6-330">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="474d6-331">[Přizpůsobení clusterů HDInsight pomocí akce skriptu][hdinsight-hadoop-customize-cluster]</span><span class="sxs-lookup"><span data-stu-id="474d6-331">[Customize HDInsight clusters using Script Action][hdinsight-hadoop-customize-cluster]</span></span>

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[import-data]: import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0
