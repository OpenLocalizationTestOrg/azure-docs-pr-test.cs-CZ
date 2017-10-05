---
title: "Přesun dat z místního serveru SQL do SQL Azure s Azure Data Factory | Microsoft Docs"
description: "Nastavte kanál ADF, která vytvoří dvě aktivity migrace dat, které společně přesun dat na každý den mezi databází na místě a v cloudu."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 36837c83-2015-48be-b850-c4346aa5ae8a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 39fe26d3388be8b558f05063a8965889c013a41e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-from-an-on-premises-sql-server-to-sql-azure-with-azure-data-factory"></a><span data-ttu-id="3cefb-103">Přesun dat z místního serveru SQL do SQL Azure s Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="3cefb-103">Move data from an on-premises SQL server to SQL Azure with Azure Data Factory</span></span>
<span data-ttu-id="3cefb-104">Toto téma ukazuje, jak pro přesun dat z databáze serveru SQL místní databázi SQL Azure přes Azure Blob Storage pomocí Azure Data Factory (ADF).</span><span class="sxs-lookup"><span data-stu-id="3cefb-104">This topic shows how to move data from an on-premises SQL Server Database to a SQL Azure Database via Azure Blob Storage using the Azure Data Factory (ADF).</span></span>

<span data-ttu-id="3cefb-105">Tabulka, která shrnuje různé možnosti pro přesun dat do Azure SQL Database, najdete v části [přesun dat do Azure SQL Database pro Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span><span class="sxs-lookup"><span data-stu-id="3cefb-105">For a table that summarizes various options for moving data to an Azure SQL Database, see [Move data to an Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span></span>

## <span data-ttu-id="3cefb-106"><a name="intro"></a>Úvod: Co je ADF a pokud by ho použít k migraci dat?</span><span class="sxs-lookup"><span data-stu-id="3cefb-106"><a name="intro"></a>Introduction: What is ADF and when should it be used to migrate data?</span></span>
<span data-ttu-id="3cefb-107">Azure Data Factory je plně spravovaná Cloudová datová integrační služba, která orchestruje a automatizuje přesouvání a transformaci dat.</span><span class="sxs-lookup"><span data-stu-id="3cefb-107">Azure Data Factory is a fully managed cloud-based data integration service that orchestrates and automates the movement and transformation of data.</span></span> <span data-ttu-id="3cefb-108">Klíče koncept v modelu ADF je kanálu.</span><span class="sxs-lookup"><span data-stu-id="3cefb-108">The key concept in the ADF model is pipeline.</span></span> <span data-ttu-id="3cefb-109">Kanál je logické seskupení aktivit, z nichž každý definuje akce lze provádět na data obsažená v datových sadách.</span><span class="sxs-lookup"><span data-stu-id="3cefb-109">A pipeline is a logical grouping of Activities, each of which defines the actions to perform on the data contained in Datasets.</span></span> <span data-ttu-id="3cefb-110">Propojené služby se používá k definování informace potřebné pro vytváření dat pro připojení k datové prostředky.</span><span class="sxs-lookup"><span data-stu-id="3cefb-110">Linked services are used to define the information needed for Data Factory to connect to the data resources.</span></span>

<span data-ttu-id="3cefb-111">S ADF může být složené stávající služby zpracování dat do datových kanálů, které jsou vysoce dostupné a spravovaných v cloudu.</span><span class="sxs-lookup"><span data-stu-id="3cefb-111">With ADF, existing data processing services can be composed into data pipelines that are highly available and managed in the cloud.</span></span> <span data-ttu-id="3cefb-112">Tyto kanály dat může být naplánovaná ingestování, Příprava, transformace, analyzovat a publikovat data a ADF spravuje a orchestruje komplexní dat a zpracování závislosti.</span><span class="sxs-lookup"><span data-stu-id="3cefb-112">These data pipelines can be scheduled to ingest, prepare, transform, analyze, and publish data, and ADF manages and orchestrates the complex data and processing dependencies.</span></span> <span data-ttu-id="3cefb-113">Řešení můžete rychle vytvořené a nasazené v cloudu, připojení stále více využívají místní a zdroje dat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="3cefb-113">Solutions can be quickly built and deployed in the cloud, connecting a growing number of on-premises and cloud data sources.</span></span>

<span data-ttu-id="3cefb-114">Zvažte použití ADF:</span><span class="sxs-lookup"><span data-stu-id="3cefb-114">Consider using ADF:</span></span>

* <span data-ttu-id="3cefb-115">Když dat je třeba průběžně migrovat v hybridním scénáři, který přistupuje k i místní a cloudové prostředky</span><span class="sxs-lookup"><span data-stu-id="3cefb-115">when data needs to be continually migrated in a hybrid scenario that accesses both on-premises and cloud resources</span></span>
* <span data-ttu-id="3cefb-116">Pokud data je zpracován, nebo musí být změněna nebo mít obchodní logiky přidána při migraci.</span><span class="sxs-lookup"><span data-stu-id="3cefb-116">when the data is transacted or needs to be modified or have business logic added to it when being migrated.</span></span>

<span data-ttu-id="3cefb-117">ADF umožňuje na plánování a sledování úloh pomocí jednoduché skripty JSON, které spravují přesun dat v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="3cefb-117">ADF allows for the scheduling and monitoring of jobs using simple JSON scripts that manage the movement of data on a periodic basis.</span></span> <span data-ttu-id="3cefb-118">ADF má také další funkce, například podporu pro komplexní operace.</span><span class="sxs-lookup"><span data-stu-id="3cefb-118">ADF also has other capabilities such as support for complex operations.</span></span> <span data-ttu-id="3cefb-119">Další informace o ADF, naleznete v dokumentaci na [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="3cefb-119">For more information on ADF, see the documentation at [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).</span></span>

## <span data-ttu-id="3cefb-120"><a name="scenario"></a>Tento scénář</span><span class="sxs-lookup"><span data-stu-id="3cefb-120"><a name="scenario"></a>The Scenario</span></span>
<span data-ttu-id="3cefb-121">Nemůžeme nastavit kanál ADF, která vytvoří dvě aktivity migrace dat.</span><span class="sxs-lookup"><span data-stu-id="3cefb-121">We set up an ADF pipeline that composes two data migration activities.</span></span> <span data-ttu-id="3cefb-122">Společně se přesouvat data na každý den mezi místní databázi SQL a Azure SQL Database v cloudu.</span><span class="sxs-lookup"><span data-stu-id="3cefb-122">Together they move data on a daily basis between an on-premises SQL database and an Azure SQL Database in the cloud.</span></span> <span data-ttu-id="3cefb-123">Jsou dvě aktivity:</span><span class="sxs-lookup"><span data-stu-id="3cefb-123">The two activities are:</span></span>

* <span data-ttu-id="3cefb-124">kopírování dat z databáze místní SQL Server k účtu Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3cefb-124">copy data from an on-premises SQL Server database to an Azure Blob Storage account</span></span>
* <span data-ttu-id="3cefb-125">kopírování dat z účtu úložiště objektů Blob Azure do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="3cefb-125">copy data from the Azure Blob Storage account to an Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="3cefb-126">Postupy v tomto poli byly upraveny, z podrobnější kurzu poskytovaných týmem ADF: [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) jsou odkazy na příslušné části tohoto tématu k dispozici v případě nutnosti.</span><span class="sxs-lookup"><span data-stu-id="3cefb-126">The steps shown here have been adapted from the more detailed tutorial provided by the ADF team: [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) References to the relevant sections of that topic are provided when appropriate.</span></span>
>
>

## <span data-ttu-id="3cefb-127"><a name="prereqs"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="3cefb-127"><a name="prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="3cefb-128">Tento kurz předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="3cefb-128">This tutorial assumes you have:</span></span>

* <span data-ttu-id="3cefb-129">**Předplatné**.</span><span class="sxs-lookup"><span data-stu-id="3cefb-129">An **Azure subscription**.</span></span> <span data-ttu-id="3cefb-130">Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3cefb-130">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="3cefb-131">**Účtu úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="3cefb-131">An **Azure storage account**.</span></span> <span data-ttu-id="3cefb-132">Používáte účet úložiště Azure pro ukládání dat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3cefb-132">You use an Azure storage account for storing the data in this tutorial.</span></span> <span data-ttu-id="3cefb-133">Pokud nemáte účet úložiště Azure, přečtěte si článek [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="3cefb-133">If you don't have an Azure storage account, see the [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article.</span></span> <span data-ttu-id="3cefb-134">Po vytvoření účtu úložiště je třeba získat klíč účtu, který se používá pro přístup k účtu.</span><span class="sxs-lookup"><span data-stu-id="3cefb-134">After you have created the storage account, you need to obtain the account key used to access the storage.</span></span> <span data-ttu-id="3cefb-135">V tématu [Správa přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="3cefb-135">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>
* <span data-ttu-id="3cefb-136">Přístup **databáze Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="3cefb-136">Access to an **Azure SQL Database**.</span></span> <span data-ttu-id="3cefb-137">Pokud je potřeba nastavit Azure SQL Database, tpoic [Začínáme se službou Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) poskytuje informace o tom, jak zřídit novou instanci třídy Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="3cefb-137">If you must set up an Azure SQL Database, the tpoic [Getting Started with Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) provides information on how to provision a new instance of an Azure SQL Database.</span></span>
* <span data-ttu-id="3cefb-138">Nainstalovaný a nakonfigurovaný **prostředí Azure PowerShell** místně.</span><span class="sxs-lookup"><span data-stu-id="3cefb-138">Installed and configured **Azure PowerShell** locally.</span></span> <span data-ttu-id="3cefb-139">Pokyny najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3cefb-139">For instructions, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

> [!NOTE]
> <span data-ttu-id="3cefb-140">Tento postup používá [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3cefb-140">This procedure uses the [Azure portal](https://portal.azure.com/).</span></span>
>
>

## <span data-ttu-id="3cefb-141"><a name="upload-data"></a>Nahrát data do SQL serveru na místě</span><span class="sxs-lookup"><span data-stu-id="3cefb-141"><a name="upload-data"></a> Upload the data to your on-premises SQL Server</span></span>
<span data-ttu-id="3cefb-142">Používáme [datovou sadu NYC taxíkem](http://chriswhong.com/open-data/foil_nyc_taxi/) k předvedení proces migrace.</span><span class="sxs-lookup"><span data-stu-id="3cefb-142">We use the [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) to demonstrate the migration process.</span></span> <span data-ttu-id="3cefb-143">Je NYC taxíkem datová sada k dispozici, jak je uvedeno v tomto příspěvku v úložišti objektů blob v Azure [NYC taxíkem Data](http://www.andresmh.com/nyctaxitrips/).</span><span class="sxs-lookup"><span data-stu-id="3cefb-143">The NYC Taxi dataset is available, as noted in that post, on Azure blob storage [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/).</span></span> <span data-ttu-id="3cefb-144">Data má dva soubory, trip_data.csv souboru, který obsahuje podrobnosti o cestě, a trip_far.csv soubor, který obsahuje podrobnosti o tarif placené pro každou cestu.</span><span class="sxs-lookup"><span data-stu-id="3cefb-144">The data has two files, the trip_data.csv file, which contains trip details, and the  trip_far.csv file, which contains details of the fare paid for each trip.</span></span> <span data-ttu-id="3cefb-145">Ukázka a popis tyto soubory jsou uvedeny v [NYC taxíkem služebních cest datovou sadu popis](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span><span class="sxs-lookup"><span data-stu-id="3cefb-145">A sample and description of these files are provided in [NYC Taxi Trips Dataset Description](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span></span>

<span data-ttu-id="3cefb-146">Můžete přizpůsobit postup uvedený v tomto poli na sadu svoje vlastní data, nebo postupujte podle kroků, jak je popsáno pomocí NYC taxíkem datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3cefb-146">You can either adapt the procedure provided here to a set of your own data or follow the steps as described by using the NYC Taxi dataset.</span></span> <span data-ttu-id="3cefb-147">Datová sada NYC taxíkem nahrát do místní databáze systému SQL Server, postupujte podle pokynů uvedených v [hromadně importovat Data do databáze serveru SQL](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span><span class="sxs-lookup"><span data-stu-id="3cefb-147">To upload the NYC Taxi dataset into your on-premises SQL Server database, follow the procedure outlined in [Bulk Import Data into SQL Server Database](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span></span> <span data-ttu-id="3cefb-148">Tyto pokyny jsou pro systém SQL Server na virtuální počítač Azure, ale postup pro odesílání na místní SQL Server je stejný.</span><span class="sxs-lookup"><span data-stu-id="3cefb-148">These instructions are for a SQL Server on an Azure Virtual Machine, but the procedure for uploading to the on-premises SQL Server is the same.</span></span>

## <span data-ttu-id="3cefb-149"><a name="create-adf"></a>Vytvoření služby Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="3cefb-149"><a name="create-adf"></a> Create an Azure Data Factory</span></span>
<span data-ttu-id="3cefb-150">Pokyny pro vytvoření nové Azure Data Factory a skupiny prostředků v [portál Azure](https://portal.azure.com/) jsou k dispozici [vytvoření služby Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span><span class="sxs-lookup"><span data-stu-id="3cefb-150">The instructions for creating a new Azure Data Factory and a resource group in the [Azure portal](https://portal.azure.com/) are provided [Create an Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span></span> <span data-ttu-id="3cefb-151">Název nové instance ADF *adfdsp* a pojmenujte vytvoření skupiny prostředků *adfdsprg*.</span><span class="sxs-lookup"><span data-stu-id="3cefb-151">Name the new ADF instance *adfdsp* and name the resource group created *adfdsprg*.</span></span>

## <a name="install-and-configure-up-the-data-management-gateway"></a><span data-ttu-id="3cefb-152">Instalace a konfigurace se Brána pro správu dat</span><span class="sxs-lookup"><span data-stu-id="3cefb-152">Install and configure up the Data Management Gateway</span></span>
<span data-ttu-id="3cefb-153">Povolit kanály v objektu pro vytváření dat Azure pro práci s SQL serveru místní, musíte ji přidat k objektu pro vytváření dat jako propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3cefb-153">To enable your pipelines in an Azure data factory to work with an on-premises SQL Server, you need to add it as a Linked Service to the data factory.</span></span> <span data-ttu-id="3cefb-154">K vytvoření propojené služby SQL serveru místní, musíte:</span><span class="sxs-lookup"><span data-stu-id="3cefb-154">To create a Linked Service for an on-premises SQL Server, you must:</span></span>

* <span data-ttu-id="3cefb-155">Stáhněte a nainstalujte Brána pro správu dat na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="3cefb-155">download and install Microsoft Data Management Gateway onto the on-premises computer.</span></span>
* <span data-ttu-id="3cefb-156">Nakonfigurujte propojené služby pro místní zdroje dat pro použití brány.</span><span class="sxs-lookup"><span data-stu-id="3cefb-156">configure the linked service for the on-premises data source to use the gateway.</span></span>

<span data-ttu-id="3cefb-157">Brána pro správu dat serializuje a deserializuje zdroj a jímka data v počítači, který je hostitelem.</span><span class="sxs-lookup"><span data-stu-id="3cefb-157">The Data Management Gateway serializes and deserializes the source and sink data on the computer where it is hosted.</span></span>

<span data-ttu-id="3cefb-158">Pokyny k instalaci a informace o Brána pro správu dat najdete v tématu [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="3cefb-158">For set-up instructions and details on Data Management Gateway, see [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span></span>

## <span data-ttu-id="3cefb-159"><a name="adflinkedservices"></a>Vytvoření propojených služeb pro připojení ke zdrojům dat</span><span class="sxs-lookup"><span data-stu-id="3cefb-159"><a name="adflinkedservices"></a>Create linked services to connect to the data resources</span></span>
<span data-ttu-id="3cefb-160">Propojená služba definuje informace potřebné pro vytváření dat Azure pro připojení k prostředku data.</span><span class="sxs-lookup"><span data-stu-id="3cefb-160">A linked service defines the information needed for Azure Data Factory to connect to a data resource.</span></span> <span data-ttu-id="3cefb-161">Podrobný postup pro vytvoření propojené služby je k dispozici v [vytvoření propojených služeb](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span><span class="sxs-lookup"><span data-stu-id="3cefb-161">The step-by-step procedure for creating linked services is provided in [Create linked services](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span></span>

<span data-ttu-id="3cefb-162">Máme tři zdroje v tomto scénáři, pro které jsou potřeba propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3cefb-162">We have three resources in this scenario for which linked services are needed.</span></span>

1. [<span data-ttu-id="3cefb-163">Propojené služby pro místní systém SQL Server</span><span class="sxs-lookup"><span data-stu-id="3cefb-163">Linked service for on-premises SQL Server</span></span>](#adf-linked-service-onprem-sql)
2. [<span data-ttu-id="3cefb-164">Propojené služby pro Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3cefb-164">Linked service for Azure Blob Storage</span></span>](#adf-linked-service-blob-store)
3. [<span data-ttu-id="3cefb-165">Propojené služby pro databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="3cefb-165">Linked service for Azure SQL database</span></span>](#adf-linked-service-azure-sql)

### <span data-ttu-id="3cefb-166"><a name="adf-linked-service-onprem-sql"></a>Propojené služby pro místní databázi systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="3cefb-166"><a name="adf-linked-service-onprem-sql"></a>Linked service for on-premises SQL Server database</span></span>
<span data-ttu-id="3cefb-167">Vytvoření propojené služby pro místní systém SQL Server:</span><span class="sxs-lookup"><span data-stu-id="3cefb-167">To create the linked service for the on-premises SQL Server:</span></span>

* <span data-ttu-id="3cefb-168">klikněte **Data Store** v ADF cílová stránka na portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="3cefb-168">click the **Data Store** in the ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="3cefb-169">Vyberte **SQL** a zadejte *uživatelské jméno* a *heslo* přihlašovací údaje pro místní systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3cefb-169">select **SQL** and enter the *username* and *password* credentials for the on-premises SQL Server.</span></span> <span data-ttu-id="3cefb-170">Musíte zadat název serveru jako **servername plně kvalifikovaný název instance zpětné lomítko (servername\instancename)**.</span><span class="sxs-lookup"><span data-stu-id="3cefb-170">You need to enter the servername as a **fully qualified servername backslash instance name (servername\instancename)**.</span></span> <span data-ttu-id="3cefb-171">Název propojené služby *adfonpremsql*.</span><span class="sxs-lookup"><span data-stu-id="3cefb-171">Name the linked service *adfonpremsql*.</span></span>

### <span data-ttu-id="3cefb-172"><a name="adf-linked-service-blob-store"></a>Propojené služby pro objekt Blob</span><span class="sxs-lookup"><span data-stu-id="3cefb-172"><a name="adf-linked-service-blob-store"></a>Linked service for Blob</span></span>
<span data-ttu-id="3cefb-173">Vytvoření propojené služby pro účet Azure Blob Storage:</span><span class="sxs-lookup"><span data-stu-id="3cefb-173">To create the linked service for the Azure Blob Storage account:</span></span>

* <span data-ttu-id="3cefb-174">klikněte **Data Store** v ADF cílová stránka na portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="3cefb-174">click the **Data Store** in the ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="3cefb-175">Vyberte **účet úložiště Azure**</span><span class="sxs-lookup"><span data-stu-id="3cefb-175">select **Azure Storage Account**</span></span>
* <span data-ttu-id="3cefb-176">Zadejte název klíče a kontejneru účtu úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="3cefb-176">enter the Azure Blob Storage account key and container name.</span></span> <span data-ttu-id="3cefb-177">Název propojené služby *adfds*.</span><span class="sxs-lookup"><span data-stu-id="3cefb-177">Name the Linked Service *adfds*.</span></span>

### <span data-ttu-id="3cefb-178"><a name="adf-linked-service-azure-sql"></a>Propojené služby pro databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="3cefb-178"><a name="adf-linked-service-azure-sql"></a>Linked service for Azure SQL database</span></span>
<span data-ttu-id="3cefb-179">Vytvoření propojené služby pro Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="3cefb-179">To create the linked service for the Azure SQL Database:</span></span>

* <span data-ttu-id="3cefb-180">klikněte **Data Store** v ADF cílová stránka na portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="3cefb-180">click the **Data Store** in the ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="3cefb-181">Vyberte **Azure SQL** a zadejte *uživatelské jméno* a *heslo* přihlašovací údaje pro databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="3cefb-181">select **Azure SQL** and enter the *username* and *password* credentials for the Azure SQL Database.</span></span> <span data-ttu-id="3cefb-182">*Uživatelské jméno* musí být zadány jako  *user@servername* .</span><span class="sxs-lookup"><span data-stu-id="3cefb-182">The *username* must be specified as *user@servername*.</span></span>   

## <span data-ttu-id="3cefb-183"><a name="adf-tables"></a>Definovat a vytvářet tabulky určete, jak pro přístup k datové sady</span><span class="sxs-lookup"><span data-stu-id="3cefb-183"><a name="adf-tables"></a>Define and create tables to specify how to access the datasets</span></span>
<span data-ttu-id="3cefb-184">Vytváření tabulek, které určují strukturu, umístění a dostupnost datové sady pomocí následujících postupů založených na skriptech.</span><span class="sxs-lookup"><span data-stu-id="3cefb-184">Create tables that specify the structure, location, and availability of the datasets with the following script-based procedures.</span></span> <span data-ttu-id="3cefb-185">Soubory JSON se používají k definování tabulky.</span><span class="sxs-lookup"><span data-stu-id="3cefb-185">JSON files are used to define the tables.</span></span> <span data-ttu-id="3cefb-186">Další informace o struktuře těchto souborů najdete v tématu [datové sady](../data-factory/data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="3cefb-186">For more information on the structure of these files, see [Datasets](../data-factory/data-factory-create-datasets.md).</span></span>

> [!NOTE]
> <span data-ttu-id="3cefb-187">Musí provést `Add-AzureAccount` rutiny před provedením [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) rutiny a potvrďte, že je vybraný právo předplatné pro provedení příkazu.</span><span class="sxs-lookup"><span data-stu-id="3cefb-187">You should execute the `Add-AzureAccount` cmdlet before executing the [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet to confirm that the right Azure subscription is selected for the command execution.</span></span> <span data-ttu-id="3cefb-188">Dokumentaci této rutiny najdete v tématu [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="3cefb-188">For documentation of this cmdlet, see [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span></span>
>
>

<span data-ttu-id="3cefb-189">Na základě JSON definice v tabulkách použijte tyto názvy:</span><span class="sxs-lookup"><span data-stu-id="3cefb-189">The JSON-based definitions in the tables use the following names:</span></span>

* <span data-ttu-id="3cefb-190">**název tabulky** v místním systému SQL server je *nyctaxi_data*</span><span class="sxs-lookup"><span data-stu-id="3cefb-190">the **table name** in the on-premises SQL server is *nyctaxi_data*</span></span>
* <span data-ttu-id="3cefb-191">**název kontejneru** ve službě Azure Blob Storage je účet *containername*</span><span class="sxs-lookup"><span data-stu-id="3cefb-191">the **container name** in the Azure Blob Storage account is *containername*</span></span>  

<span data-ttu-id="3cefb-192">Tři definice tabulek jsou potřeba pro tento kanál ADF:</span><span class="sxs-lookup"><span data-stu-id="3cefb-192">Three table definitions are needed for this ADF pipeline:</span></span>

1. [<span data-ttu-id="3cefb-193">Místní tabulky SQL</span><span class="sxs-lookup"><span data-stu-id="3cefb-193">SQL on-premises Table</span></span>](#adf-table-onprem-sql)
2. [<span data-ttu-id="3cefb-194">Tabulka objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="3cefb-194">Blob Table </span></span>](#adf-table-blob-store)
3. [<span data-ttu-id="3cefb-195">SQL Azure Table</span><span class="sxs-lookup"><span data-stu-id="3cefb-195">SQL Azure Table</span></span>](#adf-table-azure-sql)

> [!NOTE]
> <span data-ttu-id="3cefb-196">Tyto postupy pomocí prostředí Azure PowerShell můžete definovat a vytvořit aktivity ADF.</span><span class="sxs-lookup"><span data-stu-id="3cefb-196">These procedures use Azure PowerShell to define and create the ADF activities.</span></span> <span data-ttu-id="3cefb-197">Ale tyto úkoly lze provést také pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3cefb-197">But these tasks can also be accomplished using the Azure portal.</span></span> <span data-ttu-id="3cefb-198">Podrobnosti najdete v tématu [vytvoření datových sad](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span><span class="sxs-lookup"><span data-stu-id="3cefb-198">For details, see [Create datasets](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span></span>
>
>

### <span data-ttu-id="3cefb-199"><a name="adf-table-onprem-sql"></a>Místní tabulky SQL</span><span class="sxs-lookup"><span data-stu-id="3cefb-199"><a name="adf-table-onprem-sql"></a>SQL on-premises Table</span></span>
<span data-ttu-id="3cefb-200">Definice tabulky pro místní systém SQL Server je zadán v následujícím souboru JSON:</span><span class="sxs-lookup"><span data-stu-id="3cefb-200">The table definition for the on-premises SQL Server is specified in the following JSON file:</span></span>

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

<span data-ttu-id="3cefb-201">Názvy sloupců sem nebyly zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="3cefb-201">The column names were not included here.</span></span> <span data-ttu-id="3cefb-202">Můžete se na názvy sloupců zahrnutím je zde dílčí vyberte (podrobnosti najdete [ADF dokumentaci](../data-factory/data-factory-data-movement-activities.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="3cefb-202">You can sub-select on the column names by including them here (for details check the [ADF documentation](../data-factory/data-factory-data-movement-activities.md) topic.</span></span>

<span data-ttu-id="3cefb-203">Kopírování názvem definici JSON tabulky do souboru *onpremtabledef.json* souboru a uložte ho do vhodného umístění (zde předpokládá, že *C:\temp\onpremtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="3cefb-203">Copy the JSON definition of the table into a file called *onpremtabledef.json* file and save it to a known location (here assumed to be *C:\temp\onpremtabledef.json*).</span></span> <span data-ttu-id="3cefb-204">Vytvořte v tabulce v ADF pomocí následující rutiny prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="3cefb-204">Create the table in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <span data-ttu-id="3cefb-205"><a name="adf-table-blob-store"></a>Tabulka objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="3cefb-205"><a name="adf-table-blob-store"></a>Blob Table</span></span>
<span data-ttu-id="3cefb-206">Definice tabulky pro výstupní umístění objektu blob je v následujícím (mapuje ingestovaný data z místně do objektu blob Azure):</span><span class="sxs-lookup"><span data-stu-id="3cefb-206">Definition for the table for the output blob location is in the following (this maps the ingested data from on-premises to Azure blob):</span></span>

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

<span data-ttu-id="3cefb-207">Kopírování názvem definici JSON tabulky do souboru *bloboutputtabledef.json* souboru a uložte ho do vhodného umístění (zde předpokládá, že *C:\temp\bloboutputtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="3cefb-207">Copy the JSON definition of the table into a file called *bloboutputtabledef.json* file and save it to a known location (here assumed to be *C:\temp\bloboutputtabledef.json*).</span></span> <span data-ttu-id="3cefb-208">Vytvořte v tabulce v ADF pomocí následující rutiny prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="3cefb-208">Create the table in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <span data-ttu-id="3cefb-209"><a name="adf-table-azure-sq"></a>SQL Azure Table</span><span class="sxs-lookup"><span data-stu-id="3cefb-209"><a name="adf-table-azure-sq"></a>SQL Azure Table</span></span>
<span data-ttu-id="3cefb-210">Definice tabulky SQL Azure výstupu v následujícím (toto schéma mapuje dat pocházejících z objektu blob):</span><span class="sxs-lookup"><span data-stu-id="3cefb-210">Definition for the table for the SQL Azure output is in the following (this schema maps the data coming from the blob):</span></span>

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

<span data-ttu-id="3cefb-211">Kopírování názvem definici JSON tabulky do souboru *AzureSqlTable.json* souboru a uložte ho do vhodného umístění (zde předpokládá, že *C:\temp\AzureSqlTable.json*).</span><span class="sxs-lookup"><span data-stu-id="3cefb-211">Copy the JSON definition of the table into a file called *AzureSqlTable.json* file and save it to a known location (here assumed to be *C:\temp\AzureSqlTable.json*).</span></span> <span data-ttu-id="3cefb-212">Vytvořte v tabulce v ADF pomocí následující rutiny prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="3cefb-212">Create the table in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


## <span data-ttu-id="3cefb-213"><a name="adf-pipeline"></a>Definovat a vytvořit kanál</span><span class="sxs-lookup"><span data-stu-id="3cefb-213"><a name="adf-pipeline"></a>Define and create the pipeline</span></span>
<span data-ttu-id="3cefb-214">Zadejte aktivity, které patří do tohoto kanálu a vytvoření kanálu pomocí následujících postupů založených na skriptech.</span><span class="sxs-lookup"><span data-stu-id="3cefb-214">Specify the activities that belong to the pipeline and create the pipeline with the following script-based procedures.</span></span> <span data-ttu-id="3cefb-215">Soubor JSON se používá k definování vlastností, kanálu.</span><span class="sxs-lookup"><span data-stu-id="3cefb-215">A JSON file is used to define the pipeline properties.</span></span>

* <span data-ttu-id="3cefb-216">Skript předpokládá, že **název kanálu** je *AMLDSProcessPipeline*.</span><span class="sxs-lookup"><span data-stu-id="3cefb-216">The script assumes that the **pipeline name** is *AMLDSProcessPipeline*.</span></span>
* <span data-ttu-id="3cefb-217">Všimněte si také, že nastaví se periodicity kanálu, který má být provedeny na každý den a použít výchozí doba provádění úlohy (12: 00 UTC).</span><span class="sxs-lookup"><span data-stu-id="3cefb-217">Also note that we set the periodicity of the pipeline to be executed on daily basis and use the default execution time for the job (12 am UTC).</span></span>

> [!NOTE]
> <span data-ttu-id="3cefb-218">Následující postupy použijte prostředí Azure PowerShell definovat a vytvořit kanál ADF.</span><span class="sxs-lookup"><span data-stu-id="3cefb-218">The following procedures use Azure PowerShell to define and create the ADF pipeline.</span></span> <span data-ttu-id="3cefb-219">Ale tento úkol můžete udělat taky pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3cefb-219">But this task can also be accomplished using the Azure portal.</span></span> <span data-ttu-id="3cefb-220">Podrobnosti najdete v tématu [vytvořit kanál](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span><span class="sxs-lookup"><span data-stu-id="3cefb-220">For details, see [Create pipeline](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span></span>
>
>

<span data-ttu-id="3cefb-221">Pomocí definice tabulek, které jsou uvedeny dříve, definici kanálu pro ADF určen takto:</span><span class="sxs-lookup"><span data-stu-id="3cefb-221">Using the table definitions provided previously, the pipeline definition for the ADF is specified as follows:</span></span>

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premises SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",                
                            }            
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

<span data-ttu-id="3cefb-222">Kopírování volat tuto definici JSON kanálu do souboru *pipelinedef.json* souboru a uložte ho do vhodného umístění (zde předpokládá, že *C:\temp\pipelinedef.json*).</span><span class="sxs-lookup"><span data-stu-id="3cefb-222">Copy this JSON definition of the pipeline into a file called *pipelinedef.json* file and save it to a known location (here assumed to be *C:\temp\pipelinedef.json*).</span></span> <span data-ttu-id="3cefb-223">Vytvoření kanálu v ADF pomocí následující rutiny prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="3cefb-223">Create the pipeline in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

<span data-ttu-id="3cefb-224">Zkontrolujte, jestli můžete kanálu na ADF na portálu Azure Classic zobrazí jako následující (klepnutím na diagram)</span><span class="sxs-lookup"><span data-stu-id="3cefb-224">Confirm that you can see the pipeline on the ADF in the Azure Classic Portal show up as following (when you click the diagram)</span></span>

![ADF kanálu](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)

## <span data-ttu-id="3cefb-226"><a name="adf-pipeline-start"></a>Spuštění kanálu</span><span class="sxs-lookup"><span data-stu-id="3cefb-226"><a name="adf-pipeline-start"></a>Start the Pipeline</span></span>
<span data-ttu-id="3cefb-227">Kanál můžete spustit nyní pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="3cefb-227">The pipeline can now be run using the following command:</span></span>

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

<span data-ttu-id="3cefb-228">*Počátečním* a *koncovým datem* parametr hodnoty se musí nahradit skutečnými daty, mezi kterými chcete kanál ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="3cefb-228">The *startdate* and *enddate* parameter values need to be replaced with the actual dates between which you want the pipeline to run.</span></span>

<span data-ttu-id="3cefb-229">Po kanálu provede, byste měli vidět data z kontejneru vybraného pro tento objekt blob, jeden soubor za den zobrazí.</span><span class="sxs-lookup"><span data-stu-id="3cefb-229">Once the pipeline executes, you should be able to see the data show up in the container selected for the blob, one file per day.</span></span>

<span data-ttu-id="3cefb-230">Všimněte si, že jsme nebyly využít funkce poskytované službou ADF kanálu data postupně.</span><span class="sxs-lookup"><span data-stu-id="3cefb-230">Note that we have not leveraged the functionality provided by ADF to pipe data incrementally.</span></span> <span data-ttu-id="3cefb-231">Další informace o tom, jak udělat toto a další funkce poskytované verzí ADF, najdete v článku [ADF dokumentaci](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="3cefb-231">For more information on how to do this and other capabilities provided by ADF, see the [ADF documentation](https://azure.microsoft.com/services/data-factory/).</span></span>
