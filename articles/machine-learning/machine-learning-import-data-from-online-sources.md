---
title: "Umožňuje importovat data do nástroje Machine Learning Studio ze zdroje dat online | Microsoft Docs"
description: "Jak importovat data školení Azure Machine Learning Studio z různých zdrojů online."
keywords: "Importujte dat, formát dat, datové typy, zdroje dat, Cvičná data"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 701b93fe-765b-4d15-a1cf-9b607f17add6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;garye
ms.openlocfilehash: 16d4586d82ed256a90d8eb6be4aab927aed1200a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a><span data-ttu-id="3ab08-104">Import dat do nástroje Azure Machine Learning Studio z různých online zdrojů dat pomocí modulu Import Dat</span><span class="sxs-lookup"><span data-stu-id="3ab08-104">Import data into Azure Machine Learning Studio from various online data sources with the Import Data module</span></span>
<span data-ttu-id="3ab08-105">Tento článek popisuje podporu pro import dat online z různých zdrojů a informace potřebné pro přesun dat z těchto zdrojů do experimentu Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3ab08-105">This article describes the support for importing online data from various sources and the information needed to move data from these sources into an Azure Machine Learning experiment.</span></span>

> [!NOTE]
> <span data-ttu-id="3ab08-106">Tento článek poskytuje obecné informace o [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="3ab08-106">This article provides general information about the [Import Data][import-data] module.</span></span> <span data-ttu-id="3ab08-107">Podrobné informace o typech dat dostanete, formátů, parametry a odpovědi na časté otázky, naleznete v tématu referenční modulu pro [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="3ab08-107">For more detailed information about the types of data you can access, formats, parameters, and answers to common questions, see the module reference topic for the [Import Data][import-data] module.</span></span>
> 
> 

<!-- -->

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a><span data-ttu-id="3ab08-108">Úvod</span><span class="sxs-lookup"><span data-stu-id="3ab08-108">Introduction</span></span>
<span data-ttu-id="3ab08-109">Pomocí [importovat Data] [ import-data] modul, můžete přistupujete k datům z několika zdrojů dat online spuštěného experimentu [Azure Machine Learning Studio](https://studio.azureml.net/Home):</span><span class="sxs-lookup"><span data-stu-id="3ab08-109">By using the [Import Data][import-data] module, you can access data from one of several online data sources while your experiment is running in [Azure Machine Learning Studio](https://studio.azureml.net/Home):</span></span>

* <span data-ttu-id="3ab08-110">Adresa URL webového pomocí protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="3ab08-110">A Web URL using HTTP</span></span>
* <span data-ttu-id="3ab08-111">Hadoop pomocí HiveQL</span><span class="sxs-lookup"><span data-stu-id="3ab08-111">Hadoop using HiveQL</span></span>
* <span data-ttu-id="3ab08-112">Úložiště objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="3ab08-112">Azure blob storage</span></span>
* <span data-ttu-id="3ab08-113">Tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="3ab08-113">Azure table</span></span>
* <span data-ttu-id="3ab08-114">Azure SQL database nebo SQL Server na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="3ab08-114">Azure SQL database or SQL Server on Azure VM</span></span>
* <span data-ttu-id="3ab08-115">Místní databáze systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="3ab08-115">On-premises SQL Server database</span></span>
* <span data-ttu-id="3ab08-116">Datový kanál poskytovatele, aktuálně OData</span><span class="sxs-lookup"><span data-stu-id="3ab08-116">A data feed provider, OData currently</span></span>
* <span data-ttu-id="3ab08-117">Azure CosmosDB (dříve nazývané DocumentDB)</span><span class="sxs-lookup"><span data-stu-id="3ab08-117">Azure CosmosDB (previously called DocumentDB)</span></span>

<span data-ttu-id="3ab08-118">Chcete-li přistupovat ke zdrojům dat online v experimentu Studio, přidejte [importovat Data] [ import-data] modulu vaše, vyberte **zdroj dat**a pak zadejte parametry, které jsou potřebné pro přístup k data.</span><span class="sxs-lookup"><span data-stu-id="3ab08-118">To access online data sources in your Studio experiment, add the [Import Data][import-data] module to your, select the **Data source**, and then provide the parameters needed to access the data.</span></span> <span data-ttu-id="3ab08-119">Zdroje dat online, které jsou podporovány je uvedeno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="3ab08-119">The online data sources that are supported are itemized in the table below.</span></span> <span data-ttu-id="3ab08-120">Tato tabulka shrnuje formáty souborů, které jsou podporovány a parametry, které se používají pro přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="3ab08-120">This table also summarizes the file formats that are supported and parameters that are used to access the data.</span></span>

<span data-ttu-id="3ab08-121">Všimněte si, že vzhledem k tomu, že tato data školení přistupuje spuštěného experimentu, je k dispozici pouze v tomto experimentu.</span><span class="sxs-lookup"><span data-stu-id="3ab08-121">Note that because this training data is accessed while your experiment is running, it's only available in that experiment.</span></span> <span data-ttu-id="3ab08-122">Pro srovnání data, která byla uložena v modulu datové sady jsou k dispozici žádné experimentu v pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="3ab08-122">By comparison, data that has been stored in a dataset module are available to any experiment in your workspace.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3ab08-123">V současné době [importovat Data] [ import-data] a [Export dat] [ export-data] moduly lze číst a zapisovat data pouze z úložiště Azure, které jsou vytvořené pomocí Classic model nasazení.</span><span class="sxs-lookup"><span data-stu-id="3ab08-123">Currently, the [Import Data][import-data] and [Export Data][export-data] modules can read and write data only from Azure storage created using the Classic deployment model.</span></span> <span data-ttu-id="3ab08-124">Jinými slovy nový typ účtu úložiště objektů Blob Azure, která nabízí úroveň přístupu horkého úložiště nebo úroveň přístupu studeného úložiště ještě nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="3ab08-124">In other words, the new Azure Blob Storage account type that offers a hot storage access tier or cool storage access tier is not yet supported.</span></span> 
> 
> <span data-ttu-id="3ab08-125">Obecně platí, všechny účty úložiště Azure, že jste mohli vytvořit předtím, než jsou k dispozici možnost této služby by neměl mít vliv.</span><span class="sxs-lookup"><span data-stu-id="3ab08-125">Generally, any Azure storage accounts that you might have created before this service option became available should not be affected.</span></span> 
> <span data-ttu-id="3ab08-126">Pokud potřebujete vytvořit nový účet, vyberte **Classic** pro nasazení modelu, nebo pomocí Správce prostředků a vyberte **obecné účely** místo **úložiště objektů Blob** pro  **Účet typu**.</span><span class="sxs-lookup"><span data-stu-id="3ab08-126">If you need to create a new account, select **Classic** for the Deployment model, or use Resource manager and select **General purpose** rather than **Blob storage** for **Account kind**.</span></span> 
> 
> <span data-ttu-id="3ab08-127">Další informace najdete v tématu [Azure Blob Storage: horká a studená vrstvy úložiště](../storage/blobs/storage-blob-storage-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="3ab08-127">For more information, see [Azure Blob Storage: Hot and Cool Storage Tiers](../storage/blobs/storage-blob-storage-tiers.md).</span></span>
> 
> 

## <a name="supported-online-data-sources"></a><span data-ttu-id="3ab08-128">Podporované zdroje dat online</span><span class="sxs-lookup"><span data-stu-id="3ab08-128">Supported online data sources</span></span>
<span data-ttu-id="3ab08-129">Azure Machine Learning **importovat Data** modul podporuje následující zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="3ab08-129">Azure Machine Learning **Import Data** module supports the following data sources:</span></span>

| <span data-ttu-id="3ab08-130">Zdroj dat</span><span class="sxs-lookup"><span data-stu-id="3ab08-130">Data Source</span></span> | <span data-ttu-id="3ab08-131">Popis</span><span class="sxs-lookup"><span data-stu-id="3ab08-131">Description</span></span> | <span data-ttu-id="3ab08-132">Parametry</span><span class="sxs-lookup"><span data-stu-id="3ab08-132">Parameters</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3ab08-133">Adresa URL webové prostřednictvím protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="3ab08-133">Web URL via HTTP</span></span> |<span data-ttu-id="3ab08-134">Čte data hodnot oddělených čárkami (CSV), hodnoty oddělené tabulátory (TSV), formát souboru atribut vztah (ARFF) a formáty Support Vector počítače (SVM-light) z jakékoli webové adresy URL, která používá protokol HTTP</span><span class="sxs-lookup"><span data-stu-id="3ab08-134">Reads data in comma-separated values (CSV), tab-separated values (TSV), attribute-relation file format (ARFF), and Support Vector Machines (SVM-light) formats, from any web URL that uses HTTP</span></span> |<span data-ttu-id="3ab08-135"><b>Adresa URL</b>: Určuje úplný název souboru, včetně názvu souboru s jakoukoli příponou a adresu URL webu.</span><span class="sxs-lookup"><span data-stu-id="3ab08-135"><b>URL</b>: Specifies the full name of the file, including the site URL and the file name, with any extension.</span></span> <br/><br/><span data-ttu-id="3ab08-136"><b>Formát dat</b>: Určuje jeden z podporovaných datových formátů: CSV, TSV, ARFF nebo SVM-light.</span><span class="sxs-lookup"><span data-stu-id="3ab08-136"><b>Data format</b>: Specifies one of the supported data formats: CSV, TSV, ARFF, or SVM-light.</span></span> <span data-ttu-id="3ab08-137">Pokud data má řádek záhlaví, je použít k přiřazování názvy sloupců.</span><span class="sxs-lookup"><span data-stu-id="3ab08-137">If the data has a header row, it is used to assign column names.</span></span> |
| <span data-ttu-id="3ab08-138">Hadoop/HDFS</span><span class="sxs-lookup"><span data-stu-id="3ab08-138">Hadoop/HDFS</span></span> |<span data-ttu-id="3ab08-139">Čte data z distribuovaného úložiště v Hadoop.</span><span class="sxs-lookup"><span data-stu-id="3ab08-139">Reads data from distributed storage in Hadoop.</span></span> <span data-ttu-id="3ab08-140">Můžete zadat data, která chcete pomocí HiveQL, jako SQL dotazovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="3ab08-140">You specify the data you want by using HiveQL, a SQL-like query language.</span></span> <span data-ttu-id="3ab08-141">HiveQL lze také provést filtrování předtím, než přidáte data do nástroje Machine Learning Studio dat a agregovat data.</span><span class="sxs-lookup"><span data-stu-id="3ab08-141">HiveQL can also be used to aggregate data and perform data filtering before you add the data to Machine Learning Studio.</span></span> |<span data-ttu-id="3ab08-142"><b>Databázový dotaz Hive</b>: Určuje dotaz Hive sloužící ke generování data.</span><span class="sxs-lookup"><span data-stu-id="3ab08-142"><b>Hive database query</b>: Specifies the Hive query used to generate the data.</span></span><br/><br/><span data-ttu-id="3ab08-143"><b>Identifikátor URI serveru HCatalog </b> : Zadaný název clusteru formátu  *&lt;název clusteru&gt;. azurehdinsight.net.*</span><span class="sxs-lookup"><span data-stu-id="3ab08-143"><b>HCatalog server URI </b> : Specified the name of your cluster using the format *&lt;your cluster name&gt;.azurehdinsight.net.*</span></span><br/><br/><span data-ttu-id="3ab08-144"><b>Název uživatelského účtu Hadoop</b>: Určuje název účtu uživatele Hadoop používají ke zřízení clusteru.</span><span class="sxs-lookup"><span data-stu-id="3ab08-144"><b>Hadoop user account name</b>: Specifies the Hadoop user account name used to provision the cluster.</span></span><br/><br/><span data-ttu-id="3ab08-145"><b>Heslo uživatelského účtu Hadoop</b> : Určuje, že pověření použitá při zřizování clusteru.</span><span class="sxs-lookup"><span data-stu-id="3ab08-145"><b>Hadoop user account password</b> : Specifies the credentials used when provisioning the cluster.</span></span> <span data-ttu-id="3ab08-146">Další informace najdete v tématu [vytvoření Hadoop clusterů v HDInsight](../hdinsight/hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="3ab08-146">For more information, see [Create Hadoop clusters in HDInsight](../hdinsight/hdinsight-provision-clusters.md).</span></span><br/><br/><span data-ttu-id="3ab08-147"><b>Umístění výstupu dat</b>: Určuje, zda jsou data uložená v Hadoop distributed file system (HDFS) nebo v Azure.</span><span class="sxs-lookup"><span data-stu-id="3ab08-147"><b>Location of output data</b>: Specifies whether the data is stored in a Hadoop distributed file system (HDFS) or in Azure.</span></span> <br/><ul><span data-ttu-id="3ab08-148">Pokud ukládáte výstupní data v HDFS, zadejte identifikátor URI serveru HDFS.</span><span class="sxs-lookup"><span data-stu-id="3ab08-148">If you store output data in HDFS, specify the HDFS server URI.</span></span> <span data-ttu-id="3ab08-149">(Nezapomeňte použít název clusteru HDInsight bez předponu HTTPS://).</span><span class="sxs-lookup"><span data-stu-id="3ab08-149">(Be sure to use the HDInsight cluster name without the HTTPS:// prefix).</span></span> <br/><br/><span data-ttu-id="3ab08-150">Pokud výstupní data ukládáte v Azure, musíte zadat název účtu úložiště Azure, přístupový klíč k úložišti a název kontejneru úložiště.</span><span class="sxs-lookup"><span data-stu-id="3ab08-150">If you store your output data in Azure, you must specify the Azure storage account name, Storage access key and Storage container name.</span></span></ul> |
| <span data-ttu-id="3ab08-151">Databáze SQL</span><span class="sxs-lookup"><span data-stu-id="3ab08-151">SQL database</span></span> |<span data-ttu-id="3ab08-152">Čte data, která je uložená v databázi Azure SQL nebo v databázi systému SQL Server spuštěna na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="3ab08-152">Reads data that is stored in an Azure SQL database or in a SQL Server database running on an Azure virtual machine.</span></span> |<span data-ttu-id="3ab08-153"><b>Název databázového serveru</b>: Určuje název serveru, na kterém běží databáze.</span><span class="sxs-lookup"><span data-stu-id="3ab08-153"><b>Database server name</b>: Specifies the name of the server on which the database is running.</span></span><br/><ul><span data-ttu-id="3ab08-154">V případě Azure SQL Database zadejte název serveru, aby se vygenerovala.</span><span class="sxs-lookup"><span data-stu-id="3ab08-154">In case of Azure SQL Database enter the server name that is generated.</span></span> <span data-ttu-id="3ab08-155">Obvykle je formulář  *&lt;generated_identifier&gt;. database.windows.net.*</span><span class="sxs-lookup"><span data-stu-id="3ab08-155">Typically it has the form *&lt;generated_identifier&gt;.database.windows.net.*</span></span> <br/><br/><span data-ttu-id="3ab08-156">V případě systému SQL server, který je hostitelem je Azure virtuálního počítače zadejte *tcp:&lt;název DNS virtuálního počítače&gt;, 1433*</span><span class="sxs-lookup"><span data-stu-id="3ab08-156">In case of a SQL server hosted on a Azure Virtual machine enter *tcp:&lt;Virtual Machine DNS Name&gt;, 1433*</span></span></ul><br/><span data-ttu-id="3ab08-157"><b>Název databáze </b>: Určuje název databáze na serveru.</span><span class="sxs-lookup"><span data-stu-id="3ab08-157"><b>Database name </b>: Specifies the name of the database on the server.</span></span> <br/><br/><span data-ttu-id="3ab08-158"><b>Název uživatelského účtu serveru</b>: Určuje uživatelské jméno pro účet, který má přístupová oprávnění k databázi.</span><span class="sxs-lookup"><span data-stu-id="3ab08-158"><b>Server user account name</b>: Specifies a user name for an account that has access permissions for the database.</span></span> <br/><br/><span data-ttu-id="3ab08-159"><b>Heslo uživatelského účtu serveru</b>: Určuje heslo pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3ab08-159"><b>Server user account password</b>: Specifies the password for the user account.</span></span><br/><br/><span data-ttu-id="3ab08-160"><b>Přijměte všechny certifikát serveru</b>: pomocí této možnosti (méně bezpečné), pokud chcete nechat přeskočit kontrola certifikát lokality před číst vaše data.</span><span class="sxs-lookup"><span data-stu-id="3ab08-160"><b>Accept any server certificate</b>: Use this option (less secure) if you want to skip reviewing the site certificate before you read your data.</span></span><br/><br/><span data-ttu-id="3ab08-161"><b>Databázový dotaz</b>: Zadejte příkaz SQL, který popisuje data chcete číst.</span><span class="sxs-lookup"><span data-stu-id="3ab08-161"><b>Database query</b>:Enter a SQL statement that describes the data you want to read.</span></span> |
| <span data-ttu-id="3ab08-162">Místní databáze SQL</span><span class="sxs-lookup"><span data-stu-id="3ab08-162">On-premises SQL database</span></span> |<span data-ttu-id="3ab08-163">Čte data, která je uložená v místní databázi.</span><span class="sxs-lookup"><span data-stu-id="3ab08-163">Reads data that is stored in an on-premises SQL database.</span></span> |<span data-ttu-id="3ab08-164"><b>Brána data gateway</b>: Určuje název brány pro správu dat nainstalovat do počítače, kde má přístup k databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="3ab08-164"><b>Data gateway</b>: Specifies the name of the Data Management Gateway installed on a computer where it can access your SQL Server database.</span></span> <span data-ttu-id="3ab08-165">Informace o nastavení brány najdete v tématu [provést pokročilou analýzu pomocí Azure Machine Learning pomocí dat z SQL serveru místní](machine-learning-use-data-from-an-on-premises-sql-server.md).</span><span class="sxs-lookup"><span data-stu-id="3ab08-165">For information about setting up the gateway, see [Perform advanced analytics with Azure Machine Learning using data from an on-premises SQL server](machine-learning-use-data-from-an-on-premises-sql-server.md).</span></span><br/><br/><span data-ttu-id="3ab08-166"><b>Název databázového serveru</b>: Určuje název serveru, na kterém běží databáze.</span><span class="sxs-lookup"><span data-stu-id="3ab08-166"><b>Database server name</b>: Specifies the name of the server on which the database is running.</span></span><br/><br/><span data-ttu-id="3ab08-167"><b>Název databáze </b>: Určuje název databáze na serveru.</span><span class="sxs-lookup"><span data-stu-id="3ab08-167"><b>Database name </b>: Specifies the name of the database on the server.</span></span> <br/><br/><span data-ttu-id="3ab08-168"><b>Název uživatelského účtu serveru</b>: Určuje uživatelské jméno pro účet, který má přístupová oprávnění k databázi.</span><span class="sxs-lookup"><span data-stu-id="3ab08-168"><b>Server user account name</b>: Specifies a user name for an account that has access permissions for the database.</span></span> <br/><br/><span data-ttu-id="3ab08-169"><b>Uživatelské jméno a heslo</b>: klikněte na tlačítko <b>zadejte hodnoty</b> zadat přihlašovací údaje databáze.</span><span class="sxs-lookup"><span data-stu-id="3ab08-169"><b>User name and password</b>: Click <b>Enter values</b> to enter your database credentials.</span></span> <span data-ttu-id="3ab08-170">Můžete použít integrované ověřování systému Windows nebo ověřování systému SQL Server v závislosti na konfiguraci vaší místní SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3ab08-170">You can use Windows Integrated Authentication or SQL Server Authentication depending upon how your on-premises SQL Server is configured.</span></span><br/><br/><span data-ttu-id="3ab08-171"><b>Databázový dotaz</b>: Zadejte příkaz SQL, který popisuje data chcete číst.</span><span class="sxs-lookup"><span data-stu-id="3ab08-171"><b>Database query</b>:Enter a SQL statement that describes the data you want to read.</span></span> |
| <span data-ttu-id="3ab08-172">Tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="3ab08-172">Azure Table</span></span> |<span data-ttu-id="3ab08-173">Čte data ze služby Table v Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3ab08-173">Reads data from the Table service in Azure Storage.</span></span><br/><br/><span data-ttu-id="3ab08-174">Pokud si přečíst velké objemy dat zřídka, použijte služby Azure Table.</span><span class="sxs-lookup"><span data-stu-id="3ab08-174">If you read large amounts of data infrequently, use the Azure Table Service.</span></span> <span data-ttu-id="3ab08-175">Poskytuje flexibilní, nerelačních (NoSQL), řešení nesmírně škálovatelná služba, nenákladné a vysoce dostupné úložiště.</span><span class="sxs-lookup"><span data-stu-id="3ab08-175">It provides a flexible, non-relational (NoSQL), massively scalable, inexpensive, and highly available storage solution.</span></span> |<span data-ttu-id="3ab08-176">Možnosti v **importovat Data** změnit v závislosti na tom, zda přistupujete k informací veřejného nebo soukromého skladování účet, který vyžaduje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="3ab08-176">The options in the **Import Data** change depending on whether you are accessing public information or a private storage account that requires login credentials.</span></span> <span data-ttu-id="3ab08-177">To je dáno <b>typ ověřování</b> která může mít hodnotu "PublicOrSAS" nebo "Account", z nichž každá má svou vlastní sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="3ab08-177">This is determined by the <b>Authentication Type</b> which can have value of "PublicOrSAS" or "Account", each of which has its own set of parameters.</span></span> <br/><br/><span data-ttu-id="3ab08-178"><b>Veřejné nebo sdíleného přístupového podpisu (SAS) URI</b>: parametry jsou:</span><span class="sxs-lookup"><span data-stu-id="3ab08-178"><b>Public or Shared Access Signature (SAS) URI</b>: The parameters are:</span></span><br/><br/><ul><span data-ttu-id="3ab08-179"><b>Tabulka URI</b>: Určuje veřejné nebo SAS adresa URL pro tuto tabulku.</span><span class="sxs-lookup"><span data-stu-id="3ab08-179"><b>Table URI</b>: Specifies the Public or SAS URL for the table.</span></span><br/><br/><span data-ttu-id="3ab08-180"><b>Určuje řádky, které vyhledat názvy vlastností</b>: hodnoty jsou <i>TopN</i> ke kontrole zadaný počet řádků, nebo <i>ScanAll</i> získat všechny řádky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="3ab08-180"><b>Specifies the rows to scan for property names</b>: The values are <i>TopN</i> to scan the specified number of rows, or <i>ScanAll</i> to get all rows in the table.</span></span> <br/><br/><span data-ttu-id="3ab08-181">Pokud jsou data homogenní a předvídatelné, je vhodné vybrat *TopN* a zadejte číslo pro N. Pro rozsáhlé tabulky to může způsobit rychlejší doby čtení.</span><span class="sxs-lookup"><span data-stu-id="3ab08-181">If the data is homogeneous and predictable, it is recommended that you select *TopN* and enter a number for N. For large tables, this can result in quicker reading times.</span></span><br/><br/><span data-ttu-id="3ab08-182">Pokud se nastaví vlastnosti, které se liší v závislosti na hloubka a umístění tabulky strukturovaná data, vyberte *ScanAll* možnosti prohledávání všechny řádky.</span><span class="sxs-lookup"><span data-stu-id="3ab08-182">If the data is structured with sets of properties that vary based on the depth and position of the table, choose the *ScanAll* option to scan all rows.</span></span> <span data-ttu-id="3ab08-183">Tím se zajistí integritu výsledné vlastnost a převod metadat.</span><span class="sxs-lookup"><span data-stu-id="3ab08-183">This ensures the integrity of your resulting property and metadata conversion.</span></span><br/><br/></ul><span data-ttu-id="3ab08-184"><b>Účet úložiště privátního</b>: parametry jsou:</span><span class="sxs-lookup"><span data-stu-id="3ab08-184"><b>Private Storage Account</b>: The parameters are:</span></span> <br/><br/><ul><span data-ttu-id="3ab08-185"><b>Název účtu</b>: Určuje název účtu, který obsahuje tabulku, kterou chcete číst.</span><span class="sxs-lookup"><span data-stu-id="3ab08-185"><b>Account name</b>: Specifies the name of the account that contains the table to read.</span></span><br/><br/><span data-ttu-id="3ab08-186"><b>Klíč účtu</b>: Určuje klíč úložiště přidružené k účtu.</span><span class="sxs-lookup"><span data-stu-id="3ab08-186"><b>Account key</b>: Specifies the storage key associated with the account.</span></span><br/><br/><span data-ttu-id="3ab08-187"><b>Název tabulky</b> : Určuje název tabulky, která obsahuje data ke čtení.</span><span class="sxs-lookup"><span data-stu-id="3ab08-187"><b>Table name</b> : Specifies the name of the table that contains the data to read.</span></span><br/><br/><span data-ttu-id="3ab08-188"><b>Řádky, které chcete vyhledat názvy vlastností</b>: hodnoty jsou <i>TopN</i> ke kontrole zadaný počet řádků, nebo <i>ScanAll</i> získat všechny řádky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="3ab08-188"><b>Rows to scan for property names</b>: The values are <i>TopN</i> to scan the specified number of rows, or <i>ScanAll</i> to get all rows in the table.</span></span><br/><br/><span data-ttu-id="3ab08-189">Pokud se data homogenní a předvídatelné, doporučujeme vybrat *TopN* a zadejte číslo pro N. Pro rozsáhlé tabulky to může způsobit rychlejší doby čtení.</span><span class="sxs-lookup"><span data-stu-id="3ab08-189">If the data is homogeneous and predictable, we recommend that you select *TopN* and enter a number for N. For large tables, this can result in quicker reading times.</span></span><br/><br/><span data-ttu-id="3ab08-190">Pokud se nastaví vlastnosti, které se liší v závislosti na hloubka a umístění tabulky strukturovaná data, vyberte *ScanAll* možnosti prohledávání všechny řádky.</span><span class="sxs-lookup"><span data-stu-id="3ab08-190">If the data is structured with sets of properties that vary based on the depth and position of the table, choose the *ScanAll* option to scan all rows.</span></span> <span data-ttu-id="3ab08-191">Tím se zajistí integritu výsledné vlastnost a převod metadat.</span><span class="sxs-lookup"><span data-stu-id="3ab08-191">This ensures the integrity of your resulting property and metadata conversion.</span></span><br/><br/> |
| <span data-ttu-id="3ab08-192">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3ab08-192">Azure Blob Storage</span></span> |<span data-ttu-id="3ab08-193">Čte data uložená ve službě Blob v Azure Storage, včetně obrázků, nestrukturovaných textu nebo binárních dat.</span><span class="sxs-lookup"><span data-stu-id="3ab08-193">Reads data stored in the Blob service in Azure Storage, including images, unstructured text, or binary data.</span></span><br/><br/><span data-ttu-id="3ab08-194">Služba objektů Blob můžete použít veřejně vystavit data a soukromě ukládání dat aplikací.</span><span class="sxs-lookup"><span data-stu-id="3ab08-194">You can use the Blob service to publicly expose data, or to privately store application data.</span></span> <span data-ttu-id="3ab08-195">Data můžete přístup odkudkoli pomocí připojení HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3ab08-195">You can access your data from anywhere by using HTTP or HTTPS connections.</span></span> |<span data-ttu-id="3ab08-196">Možnosti v **importovat Data** modulu změnit v závislosti na tom, zda přistupujete k informací veřejného nebo soukromého skladování účet, který vyžaduje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="3ab08-196">The options in the **Import Data** module change depending on whether you are accessing public information or a private storage account that requires login credentials.</span></span> <span data-ttu-id="3ab08-197">To je dáno <b>typ ověřování</b> která může mít hodnotu "PublicOrSAS" nebo "Účet".</span><span class="sxs-lookup"><span data-stu-id="3ab08-197">This is determined by the <b>Authentication Type</b> which can have a value either of "PublicOrSAS" or of "Account".</span></span><br/><br/><span data-ttu-id="3ab08-198"><b>Veřejné nebo sdíleného přístupového podpisu (SAS) URI</b>: parametry jsou:</span><span class="sxs-lookup"><span data-stu-id="3ab08-198"><b>Public or Shared Access Signature (SAS) URI</b>: The parameters are:</span></span><br/><br/><ul><span data-ttu-id="3ab08-199"><b>Identifikátor URI</b>: Určuje veřejné nebo SAS adresa URL pro tento objekt blob úložiště.</span><span class="sxs-lookup"><span data-stu-id="3ab08-199"><b>URI</b>: Specifies the Public or SAS URL for the storage blob.</span></span><br/><br/><span data-ttu-id="3ab08-200"><b>Formát souboru</b>: Určuje formát dat ve službě Blob.</span><span class="sxs-lookup"><span data-stu-id="3ab08-200"><b>File Format</b>: Specifies the format of the data in the Blob service.</span></span> <span data-ttu-id="3ab08-201">Podporované formáty jsou sdíleného svazku clusteru, TSV a ARFF.</span><span class="sxs-lookup"><span data-stu-id="3ab08-201">The supported formats are CSV, TSV, and ARFF.</span></span><br/><br/></ul><span data-ttu-id="3ab08-202"><b>Účet úložiště privátního</b>: parametry jsou:</span><span class="sxs-lookup"><span data-stu-id="3ab08-202"><b>Private Storage Account</b>: The parameters are:</span></span> <br/><br/><ul><span data-ttu-id="3ab08-203"><b>Název účtu</b>: Určuje název účtu, který obsahuje objekt blob, který chcete číst.</span><span class="sxs-lookup"><span data-stu-id="3ab08-203"><b>Account name</b>: Specifies the name of the account that contains the blob you want to read.</span></span><br/><br/><span data-ttu-id="3ab08-204"><b>Klíč účtu</b>: Určuje klíč úložiště přidružené k účtu.</span><span class="sxs-lookup"><span data-stu-id="3ab08-204"><b>Account key</b>: Specifies the storage key associated with the account.</span></span><br/><br/><span data-ttu-id="3ab08-205"><b>Cesta ke kontejneru, adresáře nebo objekt blob </b> : Určuje název objektu blob, který obsahuje data ke čtení.</span><span class="sxs-lookup"><span data-stu-id="3ab08-205"><b>Path to container, directory, or blob </b> : Specifies the name of the blob that contains the data to read.</span></span><br/><br/><span data-ttu-id="3ab08-206"><b>Formát souboru BLOB</b>: Určuje formát dat ve službě blob.</span><span class="sxs-lookup"><span data-stu-id="3ab08-206"><b>Blob file format</b>: Specifies the format of the data in the blob service.</span></span> <span data-ttu-id="3ab08-207">Podporované datové formáty jsou sdíleného svazku clusteru, TSV, ARFF, CSV s zadané kódování a Excel.</span><span class="sxs-lookup"><span data-stu-id="3ab08-207">The supported data formats are CSV, TSV, ARFF, CSV with a specified encoding, and Excel.</span></span> <br/><br/><ul><span data-ttu-id="3ab08-208">Pokud je ve formátu CSV nebo TSV, ujistěte se, že jste označuje, zda tento soubor obsahuje řádek záhlaví.</span><span class="sxs-lookup"><span data-stu-id="3ab08-208">If the format is CSV or TSV, be sure to indicate whether the file contains a header row.</span></span><br/><br/><span data-ttu-id="3ab08-209">Možnost aplikace Excel můžete číst data ze sešitů aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="3ab08-209">You can use the Excel option to read data from Excel workbooks.</span></span> <span data-ttu-id="3ab08-210">V <i>formát dat aplikace Excel</i> možnost, znamenat, zda jsou data uložena v oblasti sešitu aplikace Excel nebo v tabulce aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="3ab08-210">In the <i>Excel data format</i> option, indicate whether the data is in an Excel worksheet range, or in an Excel table.</span></span> <span data-ttu-id="3ab08-211">V <i>listu aplikace Excel nebo vložené tabulky </i>možnost, zadejte název listu nebo tabulky, který chcete číst z.</span><span class="sxs-lookup"><span data-stu-id="3ab08-211">In the <i>Excel sheet or embedded table </i>option, specify the name of the sheet or table that you want to read from.</span></span></ul><br/> |
| <span data-ttu-id="3ab08-212">Zprostředkovatel dat informačního kanálu</span><span class="sxs-lookup"><span data-stu-id="3ab08-212">Data Feed Provider</span></span> |<span data-ttu-id="3ab08-213">Čte data z podporovaných informačního kanálu zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="3ab08-213">Reads data from a supported feed provider.</span></span> <span data-ttu-id="3ab08-214">Aktuálně se podporuje jenom formát Open Data Protocol (OData).</span><span class="sxs-lookup"><span data-stu-id="3ab08-214">Currently only the Open Data Protocol (OData) format is supported.</span></span> |<span data-ttu-id="3ab08-215"><b>Data obsahu typu</b>: Určuje formát OData.</span><span class="sxs-lookup"><span data-stu-id="3ab08-215"><b>Data content type</b>: Specifies the OData format.</span></span><br/><br/><span data-ttu-id="3ab08-216"><b>Adresa URL zdroje</b>: Určuje úplnou adresu URL pro datový kanál.</span><span class="sxs-lookup"><span data-stu-id="3ab08-216"><b>Source URL</b>: Specifies the full URL for the data feed.</span></span> <br/><span data-ttu-id="3ab08-217">Například následující adresu URL čte z ukázková databáze Northwind: http://services.odata.org/northwind/northwind.svc/</span><span class="sxs-lookup"><span data-stu-id="3ab08-217">For example, the following URL reads from the Northwind sample database: http://services.odata.org/northwind/northwind.svc/</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3ab08-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3ab08-218">Next steps</span></span>

[<span data-ttu-id="3ab08-219">Nasazení webové služby Azure ML, které používají Data importu a exportu dat moduly</span><span class="sxs-lookup"><span data-stu-id="3ab08-219">Deploying Azure ML web services that use Data Import and Data Export modules</span></span>](machine-learning-web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/