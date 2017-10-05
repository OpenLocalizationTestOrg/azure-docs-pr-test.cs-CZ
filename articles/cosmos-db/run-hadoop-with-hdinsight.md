---
title: "Spustit úlohu Hadoop pomocí Azure Cosmos DB a HDInsight | Microsoft Docs"
description: "Naučte se spouštíte jednoduchou úlohu Hive, Pig a MapReduce s Azure Cosmos DB a Azure HDInsight."
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
ms.openlocfilehash: 427864fc4e494c19fcda4cfd454a9923499f6337
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <span data-ttu-id="5e4c0-103"><a name="Azure Cosmos DB-HDInsight"></a>Spustit úlohu Apache Hive, Pig nebo Hadoop pomocí Azure Cosmos DB a HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e4c0-103"><a name="Azure Cosmos DB-HDInsight"></a>Run an Apache Hive, Pig, or Hadoop job using Azure Cosmos DB and HDInsight</span></span>
<span data-ttu-id="5e4c0-104">V tomto kurzu se dozvíte, jak spustit [Apache Hive][apache-hive], [Apache Pig][apache-pig], a [Apache Hadoop] [ apache-hadoop] úloh MapReduce v Azure HDInsight s konektorem Hadoop Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-104">This tutorial shows you how to run [Apache Hive][apache-hive], [Apache Pig][apache-pig], and [Apache Hadoop][apache-hadoop] MapReduce jobs on Azure HDInsight with Cosmos DB's Hadoop connector.</span></span> <span data-ttu-id="5e4c0-105">Cosmos DB Hadoop konektor umožňuje Cosmos DB tak, aby fungoval jako zdroj a jímka pro úlohy Hive, Pig a MapReduce.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-105">Cosmos DB's Hadoop connector allows Cosmos DB to act as both a source and sink for Hive, Pig, and MapReduce jobs.</span></span> <span data-ttu-id="5e4c0-106">V tomto kurzu použije Cosmos DB jako zdroj dat i v cílovém pro úlohy Hadoop.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-106">This tutorial will use Cosmos DB as both the data source and destination for Hadoop jobs.</span></span>

<span data-ttu-id="5e4c0-107">Po dokončení tohoto kurzu, budete moct odpovězte si na následující otázky:</span><span class="sxs-lookup"><span data-stu-id="5e4c0-107">After completing this tutorial, you'll be able to answer the following questions:</span></span>

* <span data-ttu-id="5e4c0-108">Jak načíst data z databáze Cosmos pomocí úlohy Hive, Pig nebo MapReduce?</span><span class="sxs-lookup"><span data-stu-id="5e4c0-108">How do I load data from Cosmos DB using a Hive, Pig, or MapReduce job?</span></span>
* <span data-ttu-id="5e4c0-109">Jak ukládat data do databáze Cosmos. pomocí úlohy Hive, Pig nebo MapReduce?</span><span class="sxs-lookup"><span data-stu-id="5e4c0-109">How do I store data in Cosmos DB using a Hive, Pig, or MapReduce job?</span></span>

<span data-ttu-id="5e4c0-110">Doporučujeme začít následujícím videem, kde spustíme prostřednictvím úlohy Hive pomocí Cosmos databáze a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-110">We recommend getting started by watching the following video, where we run through a Hive job using Cosmos DB and HDInsight.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

<span data-ttu-id="5e4c0-111">Pak se vraťte k tomuto článku, kterou budete dostávat všechny podrobnosti na jak spouštět úlohy analytics na vaše data Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-111">Then, return to this article, where you'll receive the full details on how you can run analytics jobs on your Cosmos DB data.</span></span>

> [!TIP]
> <span data-ttu-id="5e4c0-112">Tento kurz předpokládá, že máte zkušenosti s jazykem Apache Hadoop, Hive a Pig.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-112">This tutorial assumes that you have prior experience using Apache Hadoop, Hive, and/or Pig.</span></span> <span data-ttu-id="5e4c0-113">Pokud začínáte Apache Hadoop, Hive a Pig, doporučujeme, abyste navštívíte [dokumentaci Apache Hadoop][apache-hadoop-doc].</span><span class="sxs-lookup"><span data-stu-id="5e4c0-113">If you are new to Apache Hadoop, Hive, and Pig, we recommend visiting the [Apache Hadoop documentation][apache-hadoop-doc].</span></span> <span data-ttu-id="5e4c0-114">V tomto kurzu také předpokládá, že máte zkušenosti s Cosmos DB a máte účet Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-114">This tutorial also assumes that you have prior experience with Cosmos DB and have a Cosmos DB account.</span></span> <span data-ttu-id="5e4c0-115">Pokud jste novým uživatelem Cosmos DB nebo nemáte účet Cosmos DB, podrobnosti naleznete v našem [Začínáme] [ getting-started] stránky.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-115">If you are new to Cosmos DB or you do not have a Cosmos DB account, please check out our [Getting Started][getting-started] page.</span></span>
>
>

<span data-ttu-id="5e4c0-116">Nemáte čas k dokončení tohoto kurzu a chcete jen získat celé ukázkové skripty prostředí PowerShell pro Hive, Pig a MapReduce?</span><span class="sxs-lookup"><span data-stu-id="5e4c0-116">Don't have time to complete the tutorial and just want to get the full sample PowerShell scripts for Hive, Pig, and MapReduce?</span></span> <span data-ttu-id="5e4c0-117">Nejedná se o problém, je získání [sem][hdinsight-samples].</span><span class="sxs-lookup"><span data-stu-id="5e4c0-117">Not a problem, get them [here][hdinsight-samples].</span></span> <span data-ttu-id="5e4c0-118">Stahování také obsahuje soubory hql, pig a java pro tyto ukázky.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-118">The download also contains the hql, pig, and java files for these samples.</span></span>

## <span data-ttu-id="5e4c0-119"><a name="NewestVersion"></a>Nejnovější verze</span><span class="sxs-lookup"><span data-stu-id="5e4c0-119"><a name="NewestVersion"></a>Newest Version</span></span>
<table border='1'>
    <tr><th><span data-ttu-id="5e4c0-120">Verze konektoru Hadoop</span><span class="sxs-lookup"><span data-stu-id="5e4c0-120">Hadoop Connector Version</span></span></th>
        <td><span data-ttu-id="5e4c0-121">1.2.0</span><span class="sxs-lookup"><span data-stu-id="5e4c0-121">1.2.0</span></span></td></tr>
    <tr><th><span data-ttu-id="5e4c0-122">Identifikátor Uri skriptu</span><span class="sxs-lookup"><span data-stu-id="5e4c0-122">Script Uri</span></span></th>
        <td><span data-ttu-id="5e4c0-123">https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</span><span class="sxs-lookup"><span data-stu-id="5e4c0-123">https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</span></span></td></tr>
    <tr><th><span data-ttu-id="5e4c0-124">Datum změny</span><span class="sxs-lookup"><span data-stu-id="5e4c0-124">Date Modified</span></span></th>
        <td><span data-ttu-id="5e4c0-125">04/26/2016</span><span class="sxs-lookup"><span data-stu-id="5e4c0-125">04/26/2016</span></span></td></tr>
    <tr><th><span data-ttu-id="5e4c0-126">Podporované HDInsight verze</span><span class="sxs-lookup"><span data-stu-id="5e4c0-126">Supported HDInsight Versions</span></span></th>
        <td><span data-ttu-id="5e4c0-127">3.1, 3.2</span><span class="sxs-lookup"><span data-stu-id="5e4c0-127">3.1, 3.2</span></span></td></tr>
    <tr><th><span data-ttu-id="5e4c0-128">Protokol změn</span><span class="sxs-lookup"><span data-stu-id="5e4c0-128">Change Log</span></span></th>
        <td><span data-ttu-id="5e4c0-129">Aktualizovat Azure Cosmos DB Java SDK pro 1.6.0</span><span class="sxs-lookup"><span data-stu-id="5e4c0-129">Updated Azure Cosmos DB Java SDK to 1.6.0</span></span></br>
            <span data-ttu-id="5e4c0-130">Přidaná podpora pro dělené kolekce jako zdroj a jímka</span><span class="sxs-lookup"><span data-stu-id="5e4c0-130">Added support for partitioned collections as both a source and sink</span></span></br>
        </td></tr>
</table>

## <span data-ttu-id="5e4c0-131"><a name="Prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="5e4c0-131"><a name="Prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="5e4c0-132">Než budete postupovat podle pokynů v tomto kurzu, ujistěte se, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="5e4c0-132">Before following the instructions in this tutorial, ensure that you have the following:</span></span>

* <span data-ttu-id="5e4c0-133">Účet Cosmos DB, databázi a kolekci s dokumenty uvnitř.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-133">A Cosmos DB account, a database, and a collection with documents inside.</span></span> <span data-ttu-id="5e4c0-134">Další informace najdete v tématu [Začínáme s Cosmos DB][getting-started].</span><span class="sxs-lookup"><span data-stu-id="5e4c0-134">For more information, see [Getting Started with Cosmos DB][getting-started].</span></span> <span data-ttu-id="5e4c0-135">Import ukázkových dat do účtu Cosmos DB s [nástroj pro import Cosmos DB][import-data].</span><span class="sxs-lookup"><span data-stu-id="5e4c0-135">Import sample data into your Cosmos DB account with the [Cosmos DB import tool][import-data].</span></span>
* <span data-ttu-id="5e4c0-136">Propustnost.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-136">Throughput.</span></span> <span data-ttu-id="5e4c0-137">Čtení a zápisy z prostředí HDInsight započítají vůči vaší jednotek přiděleného žádosti pro kolekce.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-137">Reads and writes from HDInsight will be counted towards your allotted request units for your collections.</span></span>
* <span data-ttu-id="5e4c0-138">Kapacitu pro další uloženou proceduru v každém výstupní kolekce.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-138">Capacity for an additional stored procedure within each output collection.</span></span> <span data-ttu-id="5e4c0-139">Uložené procedury používají pro přenos výsledné dokumenty.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-139">The stored procedures are used for transferring resulting documents.</span></span>
* <span data-ttu-id="5e4c0-140">Kapacity pro výsledný dokumenty z úloh Hive, Pig nebo MapReduce.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-140">Capacity for the resulting documents from the Hive, Pig, or MapReduce jobs.</span></span>
* <span data-ttu-id="5e4c0-141">[*Volitelné*] kapacity pro další kolekci.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-141">[*Optional*] Capacity for an additional collection.</span></span>

> [!WARNING]
> <span data-ttu-id="5e4c0-142">Aby se zabránilo vytvoření nové kolekce během některé z úloh, můžete buď vytisknout výsledky do stdout, uložte si výstup do vašeho kontejneru WASB nebo zadejte již existující kolekci.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-142">In order to avoid the creation of a new collection during any of the jobs, you can either print the results to stdout, save the output to your WASB container, or specify an already existing collection.</span></span> <span data-ttu-id="5e4c0-143">V případě zadání existující kolekci, se vytvoří nové dokumenty v kolekci a stávající dokumenty bude mít vliv pouze pokud dojde ke konfliktu v *ID*.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-143">In the case of specifying an existing collection, new documents will be created inside the collection and already existing documents will only be affected if there is a conflict in *ids*.</span></span> <span data-ttu-id="5e4c0-144">**Konektor s docházet ke konfliktům id automaticky přepíše stávající dokumenty**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-144">**The connector will automatically overwrite existing documents with id conflicts**.</span></span> <span data-ttu-id="5e4c0-145">Tuto funkci můžete vypnout pomocí nastavení upsert možnost na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-145">You can turn off this feature by setting the upsert option to false.</span></span> <span data-ttu-id="5e4c0-146">Pokud je hodnota false upsert a dojde ke konfliktu, se nezdaří úlohy Hadoop; vytváření sestav chybu id konflikt.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-146">If upsert is false and a conflict occurs, the Hadoop job will fail; reporting an id conflict error.</span></span>
>
>

## <span data-ttu-id="5e4c0-147"><a name="ProvisionHDInsight"></a>Krok 1: Vytvoření nového clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e4c0-147"><a name="ProvisionHDInsight"></a>Step 1: Create a new HDInsight cluster</span></span>
<span data-ttu-id="5e4c0-148">Tento kurz používá akce skriptu z portálu Azure k přizpůsobení clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-148">This tutorial uses Script Action from the Azure Portal to customize your HDInsight cluster.</span></span> <span data-ttu-id="5e4c0-149">V tomto kurzu budeme používat portál Azure k vytvoření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-149">In this tutorial, we will use the Azure Portal to create your HDInsight cluster.</span></span> <span data-ttu-id="5e4c0-150">Pokyny o tom, jak pomocí rutin prostředí PowerShell nebo sady SDK rozhraní .NET HDInsight, podívejte se [HDInsight přizpůsobit clustery pomocí akce skriptu] [ hdinsight-custom-provision] článku.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-150">For instructions on how to use PowerShell cmdlets or the HDInsight .NET SDK, check out the [Customize HDInsight clusters using Script Action][hdinsight-custom-provision] article.</span></span>

1. <span data-ttu-id="5e4c0-151">Přihlaste se k [portál Azure][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="5e4c0-151">Sign in to the [Azure Portal][azure-portal].</span></span>
2. <span data-ttu-id="5e4c0-152">Klikněte na tlačítko **+ nový** horní levé navigaci, vyhledejte **HDInsight** v horním panelu vyhledávání v novém okně.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-152">Click **+ New** on the top of the left navigation, search for **HDInsight** in the top search bar on the New blade.</span></span>
3. <span data-ttu-id="5e4c0-153">**HDInsight** publikováno **Microsoft** se zobrazí v horní části výsledky.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-153">**HDInsight** published by **Microsoft** will appear at the top of the Results.</span></span> <span data-ttu-id="5e4c0-154">Klikněte na něj a potom na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-154">Click on it and then click **Create**.</span></span>
4. <span data-ttu-id="5e4c0-155">V novém clusteru HDInsight vytvořit okno, zadejte vaše **název clusteru** a vyberte **předplatné** chcete zřídit tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-155">On the New HDInsight Cluster create blade, enter your **Cluster Name** and select the **Subscription** you want to provision this resource under.</span></span>

    <table border='1'>
        <tr><td><span data-ttu-id="5e4c0-156">Název clusteru</span><span class="sxs-lookup"><span data-stu-id="5e4c0-156">Cluster name</span></span></td><td><span data-ttu-id="5e4c0-157">Název clusteru.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-157">Name the cluster.</span></span><br/>
<span data-ttu-id="5e4c0-158">Název serveru DNS musí spustit a končit znakem alpha číselné a může obsahovat pomlčky.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-158">DNS name must start and end with an alpha numeric character, and may contain dashes.</span></span><br/>
<span data-ttu-id="5e4c0-159">Pole musí být řetězec o délce 3 až 63 znaků.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-159">The field must be a string between 3 and 63 characters.</span></span></td></tr>
        <tr><td><span data-ttu-id="5e4c0-160">Název odběru</span><span class="sxs-lookup"><span data-stu-id="5e4c0-160">Subscription Name</span></span></td>
            <td><span data-ttu-id="5e4c0-161">Pokud máte více než jedno předplatné Azure, vyberte odběr, který bude hostitelem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-161">If you have more than one Azure Subscription, select the subscription that will host your HDInsight cluster.</span></span> </td></tr>
    </table><span data-ttu-id="5e4c0-162">
5.Klikněte na tlačítko **vybrat typ clusteru** a nastavte následující vlastnosti do zadaných hodnot.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-162">
5. Click **Select Cluster Type** and set the following properties to the specified values.</span></span>

    <table border='1'>
        <tr><td><span data-ttu-id="5e4c0-163">Typ clusteru</span><span class="sxs-lookup"><span data-stu-id="5e4c0-163">Cluster type</span></span></td><td><span data-ttu-id="5e4c0-164"><strong>Hadoop</strong></span><span class="sxs-lookup"><span data-stu-id="5e4c0-164"><strong>Hadoop</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="5e4c0-165">Vrstvy clusteru</span><span class="sxs-lookup"><span data-stu-id="5e4c0-165">Cluster tier</span></span></td><td><span data-ttu-id="5e4c0-166"><strong>Standard</strong></span><span class="sxs-lookup"><span data-stu-id="5e4c0-166"><strong>Standard</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="5e4c0-167">Operační systém</span><span class="sxs-lookup"><span data-stu-id="5e4c0-167">Operating System</span></span></td><td><span data-ttu-id="5e4c0-168"><strong>Windows</strong></span><span class="sxs-lookup"><span data-stu-id="5e4c0-168"><strong>Windows</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="5e4c0-169">Verze</span><span class="sxs-lookup"><span data-stu-id="5e4c0-169">Version</span></span></td><td><span data-ttu-id="5e4c0-170">nejnovější verzi</span><span class="sxs-lookup"><span data-stu-id="5e4c0-170">latest version</span></span></td></tr>
    </table>

    <span data-ttu-id="5e4c0-171">Nyní, klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-171">Now, click **SELECT**.</span></span>

    ![Zadejte podrobnosti o počáteční clusteru Hadoop HDInsight][image-customprovision-page1]
6. <span data-ttu-id="5e4c0-173">Klikněte na **pověření** nastavit přihlašovací jméno a přihlašovací údaje vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-173">Click on **Credentials** to set your login and remote access credentials.</span></span> <span data-ttu-id="5e4c0-174">Zvolte vaše **uživatelské jméno přihlášení clusteru** a **clusteru heslo pro přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-174">Choose your **Cluster Login Username** and **Cluster Login Password**.</span></span>

    <span data-ttu-id="5e4c0-175">Pokud chcete vzdálené do clusteru, vyberte *Ano* v dolní části okna a zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-175">If you want to remote into your cluster, select *yes* at the bottom of the blade and provide a username and password.</span></span>
7. <span data-ttu-id="5e4c0-176">Klikněte na **zdroj dat** nastavení vaší primární umístění pro přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-176">Click on **Data Source** to set your primary location for data access.</span></span> <span data-ttu-id="5e4c0-177">Vyberte **metodu výběru** a zadejte již existující účet úložiště nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-177">Choose the **Selection Method** and specify an already existing storage account or create a new one.</span></span>
8. <span data-ttu-id="5e4c0-178">V okně stejné zadat **výchozí kontejner** a **umístění**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-178">On the same blade, specify a **Default Container** and a **Location**.</span></span> <span data-ttu-id="5e4c0-179">A klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-179">And, click **SELECT**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5e4c0-180">Vyberte umístění blízko oblast účtu Cosmos DB pro lepší výkon</span><span class="sxs-lookup"><span data-stu-id="5e4c0-180">Select a location close to your Cosmos DB account region for better performance</span></span>
   >
   >
9. <span data-ttu-id="5e4c0-181">Klikněte na **cenová** vyberte počet a typ uzlů.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-181">Click on **Pricing** to select the number and type of nodes.</span></span> <span data-ttu-id="5e4c0-182">Můžete ponechat výchozí konfigurace a později na škálovat počet uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-182">You can keep the default configuration and scale the number of Worker nodes later on.</span></span>
10. <span data-ttu-id="5e4c0-183">Klikněte na tlačítko **volitelné konfiguraci**, pak **skript akce** v okně volitelné konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-183">Click **Optional Configuration**, then **Script Actions** in the Optional Configuration Blade.</span></span>

     <span data-ttu-id="5e4c0-184">V akcí skriptů zadejte následující informace, chcete-li přizpůsobit clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-184">In Script Actions, enter the following information to customize your HDInsight cluster.</span></span>

     <table border='1'>
         <tr><th><span data-ttu-id="5e4c0-185">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e4c0-185">Property</span></span></th><th><span data-ttu-id="5e4c0-186">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5e4c0-186">Value</span></span></th></tr>
         <tr><td><span data-ttu-id="5e4c0-187">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5e4c0-187">Name</span></span></td>
             <td><span data-ttu-id="5e4c0-188">Zadejte název akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-188">Specify a name for the script action.</span></span></td></tr>
         <tr><td><span data-ttu-id="5e4c0-189">Identifikátor URI skriptu</span><span class="sxs-lookup"><span data-stu-id="5e4c0-189">Script URI</span></span></td>
             <td><span data-ttu-id="5e4c0-190">Zadejte identifikátor URI skriptu, která je volána, chcete-li přizpůsobit clusteru.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-190">Specify the URI to the script that is invoked to customize the cluster.</span></span></br></br>
<span data-ttu-id="5e4c0-191">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="5e4c0-191">Please enter:</span></span> </br> <span data-ttu-id="5e4c0-192"><strong>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</strong>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-192"><strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="5e4c0-193">Hlavní uzel</span><span class="sxs-lookup"><span data-stu-id="5e4c0-193">Head</span></span></td>
             <td><span data-ttu-id="5e4c0-194">Klikněte na zaškrtávací políčko pro spuštění skriptu prostředí PowerShell na hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-194">Click the checkbox to run the PowerShell script onto the Head node.</span></span></br></br><span data-ttu-id="5e4c0-195">
             <strong>Zaškrtněte toto políčko</strong>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-195">
             <strong>Check this checkbox</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="5e4c0-196">Worker</span><span class="sxs-lookup"><span data-stu-id="5e4c0-196">Worker</span></span></td>
             <td><span data-ttu-id="5e4c0-197">Klikněte na zaškrtávací políčko pro spuštění skriptu prostředí PowerShell do pracovního uzlu.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-197">Click the checkbox to run the PowerShell script onto the Worker node.</span></span></br></br><span data-ttu-id="5e4c0-198">
             <strong>Zaškrtněte toto políčko</strong>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-198">
             <strong>Check this checkbox</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="5e4c0-199">Zookeeper</span><span class="sxs-lookup"><span data-stu-id="5e4c0-199">Zookeeper</span></span></td>
             <td><span data-ttu-id="5e4c0-200">Klikněte na zaškrtávací políčko pro spuštění skriptu prostředí PowerShell na Zookeeper.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-200">Click the checkbox to run the PowerShell script onto the Zookeeper.</span></span></br></br><span data-ttu-id="5e4c0-201">
             <strong>Není potřeba</strong>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-201">
             <strong>Not needed</strong>.</span></span>
             </td></tr>
         <tr><td><span data-ttu-id="5e4c0-202">Parametry</span><span class="sxs-lookup"><span data-stu-id="5e4c0-202">Parameters</span></span></td>
             <td><span data-ttu-id="5e4c0-203">Zadejte parametry, pokud se vyžadují skriptem.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-203">Specify the parameters, if required by the script.</span></span></br></br><span data-ttu-id="5e4c0-204">
             <strong>Žádné parametry potřeby</strong>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-204">
             <strong>No Parameters needed</strong>.</span></span></td></tr>
     </table><span data-ttu-id="5e4c0-205">
11.Buď vytvořit novou **skupiny prostředků** nebo použít existující skupinu prostředků v rámci svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-205">
11. Create either a new **Resource Group** or use an existing Resource Group under your Azure Subscription.</span></span>
<span data-ttu-id="5e4c0-206">12.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-206">12.</span></span> <span data-ttu-id="5e4c0-207">Teď, zkontrolujte **připnout na řídicí panel** sledování jeho nasazení a klikněte na tlačítko **vytvořit**!</span><span class="sxs-lookup"><span data-stu-id="5e4c0-207">Now, check **Pin to dashboard** to track its deployment and click **Create**!</span></span>

## <span data-ttu-id="5e4c0-208"><a name="InstallCmdlets"></a>Krok 2: Instalace a konfigurace prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e4c0-208"><a name="InstallCmdlets"></a>Step 2: Install and configure Azure PowerShell</span></span>
1. <span data-ttu-id="5e4c0-209">Nainstalujte Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-209">Install Azure PowerShell.</span></span> <span data-ttu-id="5e4c0-210">Pokyny naleznete [sem][powershell-install-configure].</span><span class="sxs-lookup"><span data-stu-id="5e4c0-210">Instructions can be found [here][powershell-install-configure].</span></span>

   > [!NOTE]
   > <span data-ttu-id="5e4c0-211">Jenom pro dotazy Hive, případně můžete použít HDInsight je online Editor Hive.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-211">Alternatively, just for Hive queries, you can use HDInsight's online Hive Editor.</span></span> <span data-ttu-id="5e4c0-212">Uděláte to tak, přihlaste se k [portálu Azure][azure-portal], klikněte na tlačítko **HDInsight** v levém podokně zobrazíte seznam clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-212">To do so, sign in to the [Azure Portal][azure-portal], click **HDInsight** on the left pane to view a list of your HDInsight clusters.</span></span> <span data-ttu-id="5e4c0-213">Klikněte na cluster, který chcete spouštět dotazy Hive a pak klikněte na **dotazu konzoly**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-213">Click the cluster you want to run Hive queries on, and then click **Query Console**.</span></span>
   >
   >
2. <span data-ttu-id="5e4c0-214">Otevřete Integrované skriptovací prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="5e4c0-214">Open the Azure PowerShell Integrated Scripting Environment:</span></span>

   * <span data-ttu-id="5e4c0-215">V počítači se systémem Windows 8 nebo Windows Server 2012 nebo vyšší můžete použít integrované hledání.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-215">On a computer running Windows 8 or Windows Server 2012 or higher, you can use the built-in Search.</span></span> <span data-ttu-id="5e4c0-216">Na obrazovce Start zadejte **prostředí powershell ise** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-216">From the Start screen, type **powershell ise** and click **Enter**.</span></span>
   * <span data-ttu-id="5e4c0-217">V počítači se systémem starším než Windows 8 nebo Windows Server 2012 pomocí nabídky Start.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-217">On a computer running a version earlier than Windows 8 or Windows Server 2012, use the Start menu.</span></span> <span data-ttu-id="5e4c0-218">Z nabídky Start, zadejte **příkazového řádku** do vyhledávacího pole a pak v seznamu výsledků, klikněte na tlačítko **příkazového řádku**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-218">From the Start menu, type **Command Prompt** in the search box, then in the list of results, click **Command Prompt**.</span></span> <span data-ttu-id="5e4c0-219">V příkazovém řádku zadejte **powershell_ise** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-219">In the Command Prompt, type **powershell_ise** and click **Enter**.</span></span>
3. <span data-ttu-id="5e4c0-220">Přidání účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-220">Add your Azure Account.</span></span>

   1. <span data-ttu-id="5e4c0-221">V podokně konzoly zadejte **Add-AzureAccount** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-221">In the Console Pane, type **Add-AzureAccount** and click **Enter**.</span></span>
   2. <span data-ttu-id="5e4c0-222">Zadejte e-mailová adresa spojená s předplatným Azure a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-222">Type in the email address associated with your Azure subscription and click **Continue**.</span></span>
   3. <span data-ttu-id="5e4c0-223">Zadejte heslo pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-223">Type in the password for your Azure subscription.</span></span>
   4. <span data-ttu-id="5e4c0-224">Klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-224">Click **Sign in**.</span></span>
4. <span data-ttu-id="5e4c0-225">Následující diagram identifikuje důležité části prostředí Azure PowerShell skriptování.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-225">The following diagram identifies the important parts of your Azure PowerShell Scripting Environment.</span></span>

    ![Diagram pro prostředí Azure PowerShell][azure-powershell-diagram]

## <span data-ttu-id="5e4c0-227"><a name="RunHive"></a>Krok 3: Spuštění úlohy Hive pomocí Cosmos databáze a HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e4c0-227"><a name="RunHive"></a>Step 3: Run a Hive job using Cosmos DB and HDInsight</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5e4c0-228">Všechny proměnné indikován < > musí být zadána pomocí nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-228">All variables indicated by < > must be filled in using your configuration settings.</span></span>
>
>

1. <span data-ttu-id="5e4c0-229">V podokně pro skript prostředí PowerShell nastavte následující proměnné.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-229">Set the following variables in your PowerShell Script pane.</span></span>

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p><span data-ttu-id="5e4c0-230">Začněme, vytváření řetězec vašeho dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-230">Let's begin constructing your query string.</span></span> <span data-ttu-id="5e4c0-231">Jsme budete napsat dotaz Hive, které trvá vygenerované systémem časová razítka (_ts) a jedinečné ID (_rid) z Azure Cosmos DB kolekce všechny dokumenty, zaznamená všechny dokumenty podle počtu minut a pak uloží výsledky zpět do nové kolekce Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-231">We'll write a Hive query that takes all documents' system generated timestamps (_ts) and unique ids (_rid) from an Azure Cosmos DB collection, tallies all documents by the minute, and then stores the results back into a new Azure Cosmos DB collection.</span></span></p>

    <p><span data-ttu-id="5e4c0-232">Nejdříve vytvoříme tabulku Hive z našich kolekce Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-232">First, let's create a Hive table from our Azure Cosmos DB collection.</span></span> <span data-ttu-id="5e4c0-233">Přidejte následující fragment kódu do podokna skript prostředí PowerShell <strong>po</strong> fragmentu kódu od #1.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-233">Add the following code snippet to the PowerShell Script pane <strong>after</strong> the code snippet from #1.</span></span> <span data-ttu-id="5e4c0-234">Zajistěte, aby naše dokumenty, které se právě _ts zahrnete volitelné DocumentDB.query parametr t uvolnění dočasné paměti a _rid.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-234">Make sure you include the optional DocumentDB.query parameter t trim our documents to just _ts and _rid.</span></span></p>

   > [!NOTE]
   > <span data-ttu-id="5e4c0-235">**Pojmenování DocumentDB.inputCollections se jedná o chybu.**</span><span class="sxs-lookup"><span data-stu-id="5e4c0-235">**Naming DocumentDB.inputCollections was not a mistake.**</span></span> <span data-ttu-id="5e4c0-236">Ano, jsme povolit přidávání více kolekcí jako vstup:</span><span class="sxs-lookup"><span data-stu-id="5e4c0-236">Yes, we allow adding multiple collections as an input:</span></span> </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. <span data-ttu-id="5e4c0-237">V dalším kroku vytvoříme pro kolekci výstupní tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-237">Next, let's create a Hive table for the output collection.</span></span> <span data-ttu-id="5e4c0-238">Vlastnosti dokumentu výstup bude měsíc, den, hodinu, minutu a celkový počet výskytů.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-238">The output document properties will be the month, day, hour, minute, and the total number of occurrences.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5e4c0-239">**Ještě znovu pojmenování DocumentDB.outputCollections se jedná o chybu.**</span><span class="sxs-lookup"><span data-stu-id="5e4c0-239">**Yet again, naming DocumentDB.outputCollections was not a mistake.**</span></span> <span data-ttu-id="5e4c0-240">Ano, jsme povolit přidávání více kolekcí jako výstup:</span><span class="sxs-lookup"><span data-stu-id="5e4c0-240">Yes, we allow adding multiple collections as an output:</span></span> </br>
   > <span data-ttu-id="5e4c0-241">'*DocumentDB.outputCollections*'='*\<název kolekce DocumentDB výstupu 1\>*,*\<název kolekce DocumentDB výstupu 2\>*.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-241">'*DocumentDB.outputCollections*' = '*\<DocumentDB Output Collection Name 1\>*,*\<DocumentDB Output Collection Name 2\>*'</span></span> </br> <span data-ttu-id="5e4c0-242">Názvy kolekce jsou oddělené bez mezer pouze jeden čárkou.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-242">The collection names are separated without spaces, using only a single comma.</span></span> </br></br>
   > <span data-ttu-id="5e4c0-243">Dokumenty bude distribuované kruhového dotazování napříč více kolekcí.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-243">Documents will be distributed round-robin across multiple collections.</span></span> <span data-ttu-id="5e4c0-244">Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v další kolekce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-244">A batch of documents will be stored in one collection, then a second batch of documents will be stored in the next collection, and so forth.</span></span>
   >
   >

       # Create a Hive table for the output data to DocumentDB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. <span data-ttu-id="5e4c0-245">Nakonec umožňuje tally dokumenty měsíc, den, hodinu a minutu a vložit výsledky zpět do výstupní tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-245">Finally, let's tally the documents by month, day, hour, and minute and insert the results back into the output Hive table.</span></span>

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. <span data-ttu-id="5e4c0-246">Přidejte následující fragment skriptu k vytvoření definice úlohy Hive z předchozího dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-246">Add the following script snippet to create a Hive job definition from the previous query.</span></span>

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    <span data-ttu-id="5e4c0-247">Můžete také použít-souboru přepínač tak, aby určete soubor skriptu HiveQL na HDFS.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-247">You can also use the -File switch to specify a HiveQL script file on HDFS.</span></span>
4. <span data-ttu-id="5e4c0-248">Přidejte následující fragment uložte čas spuštění a odeslání úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-248">Add the following snippet to save the start time and submit the Hive job.</span></span>

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. <span data-ttu-id="5e4c0-249">Přidejte následující příkaz a počkejte na dokončení úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-249">Add the following to wait for the Hive job to complete.</span></span>

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. <span data-ttu-id="5e4c0-250">Přidejte následující tisknout standardní výstupní zařízení a počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-250">Add the following to print the standard output and the start and end times.</span></span>

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. <span data-ttu-id="5e4c0-251">**Spustit** nový skript!</span><span class="sxs-lookup"><span data-stu-id="5e4c0-251">**Run** your new script!</span></span> <span data-ttu-id="5e4c0-252">**Klikněte na tlačítko** tlačítko zelená execute.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-252">**Click** the green execute button.</span></span>
8. <span data-ttu-id="5e4c0-253">Zkontrolujte výsledky.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-253">Check the results.</span></span> <span data-ttu-id="5e4c0-254">Přihlaste se k [portál Azure][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="5e4c0-254">Sign into the [Azure Portal][azure-portal].</span></span>

   1. <span data-ttu-id="5e4c0-255">Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-255">Click <strong>Browse</strong> on the left-side panel.</span></span> </br>
   2. <span data-ttu-id="5e4c0-256">Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním panelu Procházet.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-256">Click <strong>everything</strong> at the top-right of the browse panel.</span></span> </br>
   3. <span data-ttu-id="5e4c0-257">Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-257">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span> </br>
   4. <span data-ttu-id="5e4c0-258">Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené ke kolekci výstupu, zadané v dotazu Hive.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-258">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with the output collection specified in your Hive query.</span></span></br>
   5. <span data-ttu-id="5e4c0-259">Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-259">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span></br></p>

   <span data-ttu-id="5e4c0-260">Zobrazí se výsledky dotazu Hive.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-260">You will see the results of your Hive query.</span></span>

   ![Výsledky dotazu Hive][image-hive-query-results]

## <span data-ttu-id="5e4c0-262"><a name="RunPig"></a>Krok 4: Spuštění úlohy Pig pomocí Cosmos databáze a HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e4c0-262"><a name="RunPig"></a>Step 4: Run a Pig job using Cosmos DB and HDInsight</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5e4c0-263">Všechny proměnné indikován < > musí být zadána pomocí nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-263">All variables indicated by < > must be filled in using your configuration settings.</span></span>
>
>

1. <span data-ttu-id="5e4c0-264">V podokně pro skript prostředí PowerShell nastavte následující proměnné.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-264">Set the following variables in your PowerShell Script pane.</span></span>

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p><span data-ttu-id="5e4c0-265">Začněme, vytváření řetězec vašeho dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-265">Let's begin constructing your query string.</span></span> <span data-ttu-id="5e4c0-266">Jsme budete napsat dotaz Pig, které trvá vygenerované systémem časová razítka (_ts) a jedinečné ID (_rid) z Azure Cosmos DB kolekce všechny dokumenty, zaznamená všechny dokumenty podle počtu minut a pak uloží výsledky zpět do nové kolekce Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-266">We'll write a Pig query that takes all documents' system generated timestamps (_ts) and unique ids (_rid) from an Azure Cosmos DB collection, tallies all documents by the minute, and then stores the results back into a new Azure Cosmos DB collection.</span></span></p>
    <p><span data-ttu-id="5e4c0-267">Nejdřív načíst dokumenty z databáze Cosmos do HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-267">First, load documents from Cosmos DB into HDInsight.</span></span> <span data-ttu-id="5e4c0-268">Přidejte následující fragment kódu do podokna skript prostředí PowerShell <strong>po</strong> fragmentu kódu od #1.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-268">Add the following code snippet to the PowerShell Script pane <strong>after</strong> the code snippet from #1.</span></span> <span data-ttu-id="5e4c0-269">Nezapomeňte přidat DocumentDB dotaz na volitelný parametr dotazu DocumentDB oříznout naše dokumenty, které se právě _ts a _rid.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-269">Make sure to add a DocumentDB query to the optional DocumentDB query parameter to trim our documents to just _ts and _rid.</span></span></p>

   > [!NOTE]
   > <span data-ttu-id="5e4c0-270">Ano, jsme povolit přidávání více kolekcí jako vstup:</span><span class="sxs-lookup"><span data-stu-id="5e4c0-270">Yes, we allow adding multiple collections as an input:</span></span> </br>
   > <span data-ttu-id="5e4c0-271">'*\<Název kolekce DocumentDB vstup 1\>*,*\<název kolekce DocumentDB vstup 2\>*.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-271">'*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*'</span></span></br> <span data-ttu-id="5e4c0-272">Názvy kolekce jsou oddělené bez mezer pouze jeden čárkou.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-272">The collection names are separated without spaces, using only a single comma.</span></span> </b>
   >
   >

    <span data-ttu-id="5e4c0-273">Dokumenty bude distribuované kruhového dotazování napříč více kolekcí.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-273">Documents will be distributed round-robin across multiple collections.</span></span> <span data-ttu-id="5e4c0-274">Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v další kolekce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-274">A batch of documents will be stored in one collection, then a second batch of documents will be stored in the next collection, and so forth.</span></span>

        # Load data from Cosmos DB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. <span data-ttu-id="5e4c0-275">Dále umožňuje tally podle měsíc, den, hodinu, minutu a celkový počet výskytů dokumenty.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-275">Next, let's tally the documents by the month, day, hour, minute, and the total number of occurrences.</span></span>

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. <span data-ttu-id="5e4c0-276">Umožňuje uložit nakonec výsledky do naší nové kolekce výstup.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-276">Finally, let's store the results into our new output collection.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5e4c0-277">Ano, jsme povolit přidávání více kolekcí jako výstup:</span><span class="sxs-lookup"><span data-stu-id="5e4c0-277">Yes, we allow adding multiple collections as an output:</span></span> </br>
   > <span data-ttu-id="5e4c0-278">'\<Název kolekce DocumentDB výstupu 1\>,\<název kolekce DocumentDB výstupu 2\>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-278">'\<DocumentDB Output Collection Name 1\>,\<DocumentDB Output Collection Name 2\>'</span></span></br> <span data-ttu-id="5e4c0-279">Názvy kolekce jsou oddělené bez mezer pouze jeden čárkou.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-279">The collection names are separated without spaces, using only a single comma.</span></span></br>
   > <span data-ttu-id="5e4c0-280">Dokumenty bude distribuované kruhového dotazování napříč více kolekcí.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-280">Documents will be distributed round-robin across the multiple collections.</span></span> <span data-ttu-id="5e4c0-281">Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v další kolekce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-281">A batch of documents will be stored in one collection, then a second batch of documents will be stored in the next collection, and so forth.</span></span>
   >
   >

        # Store output data to Cosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. <span data-ttu-id="5e4c0-282">Přidejte následující fragment skriptu k vytvoření definice úlohy Pig z předchozího dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-282">Add the following script snippet to create a Pig job definition from the previous query.</span></span>

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    <span data-ttu-id="5e4c0-283">Můžete také použít-souboru přepínač tak, aby určete soubor skriptu Pig na HDFS.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-283">You can also use the -File switch to specify a Pig script file on HDFS.</span></span>
6. <span data-ttu-id="5e4c0-284">Přidejte následující fragment uložte čas spuštění a odeslat úlohu Pig.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-284">Add the following snippet to save the start time and submit the Pig job.</span></span>

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. <span data-ttu-id="5e4c0-285">Přidejte následující příkaz a počkejte na dokončení úlohy Pig.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-285">Add the following to wait for the Pig job to complete.</span></span>

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. <span data-ttu-id="5e4c0-286">Přidejte následující tisknout standardní výstupní zařízení a počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-286">Add the following to print the standard output and the start and end times.</span></span>

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. <span data-ttu-id="5e4c0-287">**Spustit** nový skript!</span><span class="sxs-lookup"><span data-stu-id="5e4c0-287">**Run** your new script!</span></span> <span data-ttu-id="5e4c0-288">**Klikněte na tlačítko** tlačítko zelená execute.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-288">**Click** the green execute button.</span></span>
10. <span data-ttu-id="5e4c0-289">Zkontrolujte výsledky.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-289">Check the results.</span></span> <span data-ttu-id="5e4c0-290">Přihlaste se k [portál Azure][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="5e4c0-290">Sign into the [Azure Portal][azure-portal].</span></span>

    1. <span data-ttu-id="5e4c0-291">Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-291">Click <strong>Browse</strong> on the left-side panel.</span></span> </br>
    2. <span data-ttu-id="5e4c0-292">Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním panelu Procházet.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-292">Click <strong>everything</strong> at the top-right of the browse panel.</span></span> </br>
    3. <span data-ttu-id="5e4c0-293">Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-293">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span> </br>
    4. <span data-ttu-id="5e4c0-294">Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené ke kolekci výstupu, zadané v dotazu Pig.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-294">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with the output collection specified in your Pig query.</span></span></br>
    5. <span data-ttu-id="5e4c0-295">Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-295">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span></br></p>

    <span data-ttu-id="5e4c0-296">Zobrazí se výsledky dotazu Pig.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-296">You will see the results of your Pig query.</span></span>

    ![Výsledky dotazu pig][image-pig-query-results]

## <span data-ttu-id="5e4c0-298"><a name="RunMapReduce"></a>Krok 5: Spustit úlohu MapReduce pomocí Azure Cosmos DB a HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e4c0-298"><a name="RunMapReduce"></a>Step 5: Run a MapReduce job using Azure Cosmos DB and HDInsight</span></span>
1. <span data-ttu-id="5e4c0-299">V podokně pro skript prostředí PowerShell nastavte následující proměnné.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-299">Set the following variables in your PowerShell Script pane.</span></span>

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. <span data-ttu-id="5e4c0-300">Jsme budete spustit úlohu MapReduce, která sečte počet výskytů pro každou vlastnost dokumentu z kolekce Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-300">We'll execute a MapReduce job that tallies the number of occurrences for each Document property from your Azure Cosmos DB collection.</span></span> <span data-ttu-id="5e4c0-301">Přidejte tento fragment skriptu **po** výše uvedeném fragmentu.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-301">Add this script snippet **after** the snippet above.</span></span>

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > <span data-ttu-id="5e4c0-302">TallyProperties v01.jar se dodává s vlastní instalace služby konektoru Hadoop DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-302">TallyProperties-v01.jar comes with the custom installation of the Cosmos DB Hadoop Connector.</span></span>
   >
   >
3. <span data-ttu-id="5e4c0-303">Přidáním následujícího příkazu se odeslat úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-303">Add the following command to submit the MapReduce job.</span></span>

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    <span data-ttu-id="5e4c0-304">Kromě definici úlohy MapReduce je také zadat název clusteru HDInsight, kde chcete spustit úlohu MapReduce a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-304">In addition to the MapReduce job definition, you also provide the HDInsight cluster name where you want to run the MapReduce job, and the credentials.</span></span> <span data-ttu-id="5e4c0-305">Start-AzureHDInsightJob je synchronního volání.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-305">The Start-AzureHDInsightJob is an asynchronized call.</span></span> <span data-ttu-id="5e4c0-306">Chcete-li zkontrolovat na dokončení úlohy, použijte *čekání AzureHDInsightJob* rutiny.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-306">To check the completion of the job, use the *Wait-AzureHDInsightJob* cmdlet.</span></span>
4. <span data-ttu-id="5e4c0-307">Přidejte následující příkaz a zkontrolujte všechny chyby se spustit úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-307">Add the following command to check any errors with running the MapReduce job.</span></span>

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. <span data-ttu-id="5e4c0-308">**Spustit** nový skript!</span><span class="sxs-lookup"><span data-stu-id="5e4c0-308">**Run** your new script!</span></span> <span data-ttu-id="5e4c0-309">**Klikněte na tlačítko** tlačítko zelená execute.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-309">**Click** the green execute button.</span></span>
6. <span data-ttu-id="5e4c0-310">Zkontrolujte výsledky.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-310">Check the results.</span></span> <span data-ttu-id="5e4c0-311">Přihlaste se k [portál Azure][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="5e4c0-311">Sign into the [Azure Portal][azure-portal].</span></span>

   1. <span data-ttu-id="5e4c0-312">Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-312">Click <strong>Browse</strong> on the left-side panel.</span></span>
   2. <span data-ttu-id="5e4c0-313">Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním panelu Procházet.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-313">Click <strong>everything</strong> at the top-right of the browse panel.</span></span>
   3. <span data-ttu-id="5e4c0-314">Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-314">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span>
   4. <span data-ttu-id="5e4c0-315">Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené ke kolekci výstupu, zadané v úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-315">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with the output collection specified in your MapReduce job.</span></span>
   5. <span data-ttu-id="5e4c0-316">Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-316">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span>

      <span data-ttu-id="5e4c0-317">Zobrazí se výsledky úlohy MapReduce.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-317">You will see the results of your MapReduce job.</span></span>

      ![MapReduce výsledky dotazu][image-mapreduce-query-results]

## <span data-ttu-id="5e4c0-319"><a name="NextSteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e4c0-319"><a name="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="5e4c0-320">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="5e4c0-320">Congratulations!</span></span> <span data-ttu-id="5e4c0-321">Právě jste spustili vaše první úlohy Hive, Pig a MapReduce pomocí Azure Cosmos DB a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-321">You just ran your first Hive, Pig, and MapReduce jobs using Azure Cosmos DB and HDInsight.</span></span>

<span data-ttu-id="5e4c0-322">Máme open source naše konektor Hadoop.</span><span class="sxs-lookup"><span data-stu-id="5e4c0-322">We have open sourced our Hadoop Connector.</span></span> <span data-ttu-id="5e4c0-323">Pokud byste chtěli, můžete přispívat na [Githubu][github].</span><span class="sxs-lookup"><span data-stu-id="5e4c0-323">If you're interested, you can contribute on [GitHub][github].</span></span>

<span data-ttu-id="5e4c0-324">Další informace naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="5e4c0-324">To learn more, see the following articles:</span></span>

* <span data-ttu-id="5e4c0-325">[Vývoj aplikace Java pomocí Documentdb][documentdb-java-application]</span><span class="sxs-lookup"><span data-stu-id="5e4c0-325">[Develop a Java application with Documentdb][documentdb-java-application]</span></span>
* <span data-ttu-id="5e4c0-326">[Vývoj aplikací Java MapReduce pro Hadoop v HDInsight][hdinsight-develop-deploy-java-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="5e4c0-326">[Develop Java MapReduce programs for Hadoop in HDInsight][hdinsight-develop-deploy-java-mapreduce]</span></span>
* <span data-ttu-id="5e4c0-327">[Začínáme používat Hadoop s Hive v HDInsight k analýze používání mobilního telefonu][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="5e4c0-327">[Get started using Hadoop with Hive in HDInsight to analyze mobile handset use][hdinsight-get-started]</span></span>
* <span data-ttu-id="5e4c0-328">[Používání nástroje MapReduce s HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="5e4c0-328">[Use MapReduce with HDInsight][hdinsight-use-mapreduce]</span></span>
* <span data-ttu-id="5e4c0-329">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="5e4c0-329">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="5e4c0-330">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="5e4c0-330">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="5e4c0-331">[Přizpůsobení clusterů HDInsight pomocí akce skriptu][hdinsight-hadoop-customize-cluster]</span><span class="sxs-lookup"><span data-stu-id="5e4c0-331">[Customize HDInsight clusters using Script Action][hdinsight-hadoop-customize-cluster]</span></span>

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
