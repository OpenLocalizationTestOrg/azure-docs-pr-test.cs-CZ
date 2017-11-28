---
title: "aaaMove data z tooSQL místní SQL Server Azure s Azure Data Factory | Microsoft Docs"
description: "Nastavte kanál ADF, která vytvoří dvě aktivity migrace dat, které společně přesun dat na každý den mezi databází na místě a v cloudu hello."
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
ms.openlocfilehash: 7f7e78c7a84a259539221d3235b76bb5a3cf9866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-sql-server-toosql-azure-with-azure-data-factory"></a><span data-ttu-id="a8e86-103">Přesun dat z tooSQL serveru SQL místní Azure s Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="a8e86-103">Move data from an on-premises SQL server tooSQL Azure with Azure Data Factory</span></span>
<span data-ttu-id="a8e86-104">Toto téma ukazuje, jak hello toomove data ze tooa databáze systému SQL Server místní databáze SQL Azure přes Azure Blob Storage pomocí Azure Data Factory (ADF).</span><span class="sxs-lookup"><span data-stu-id="a8e86-104">This topic shows how toomove data from an on-premises SQL Server Database tooa SQL Azure Database via Azure Blob Storage using hello Azure Data Factory (ADF).</span></span>

<span data-ttu-id="a8e86-105">Tabulka, která shrnuje různé možnosti pro přesunutí dat tooan Azure SQL Database, najdete v části [přesunout data tooan Azure SQL Database pro Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span><span class="sxs-lookup"><span data-stu-id="a8e86-105">For a table that summarizes various options for moving data tooan Azure SQL Database, see [Move data tooan Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span></span>

## <span data-ttu-id="a8e86-106"><a name="intro"></a>Úvod: Co je ADF a když měli použít toomigrate dat?</span><span class="sxs-lookup"><span data-stu-id="a8e86-106"><a name="intro"></a>Introduction: What is ADF and when should it be used toomigrate data?</span></span>
<span data-ttu-id="a8e86-107">Azure Data Factory je plně spravovaná Cloudová datová integrační služba, která orchestruje a automatizuje hello přesouvání a transformaci dat.</span><span class="sxs-lookup"><span data-stu-id="a8e86-107">Azure Data Factory is a fully managed cloud-based data integration service that orchestrates and automates hello movement and transformation of data.</span></span> <span data-ttu-id="a8e86-108">Hello klíče koncept v modelu ADF hello je kanálu.</span><span class="sxs-lookup"><span data-stu-id="a8e86-108">hello key concept in hello ADF model is pipeline.</span></span> <span data-ttu-id="a8e86-109">Kanál je logické seskupení aktivit, z nichž každý definuje hello akce tooperform na hello data obsažená v datových sadách.</span><span class="sxs-lookup"><span data-stu-id="a8e86-109">A pipeline is a logical grouping of Activities, each of which defines hello actions tooperform on hello data contained in Datasets.</span></span> <span data-ttu-id="a8e86-110">Propojené služby jsou použité toodefine hello informace potřebné pro vytváření dat tooconnect toohello datové prostředky.</span><span class="sxs-lookup"><span data-stu-id="a8e86-110">Linked services are used toodefine hello information needed for Data Factory tooconnect toohello data resources.</span></span>

<span data-ttu-id="a8e86-111">S ADF se může skládat stávající služby zpracování dat do datových kanálů, které jsou vysoce dostupné a spravovaných v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="a8e86-111">With ADF, existing data processing services can be composed into data pipelines that are highly available and managed in hello cloud.</span></span> <span data-ttu-id="a8e86-112">Tyto kanály dat může být naplánované tooingest, Příprava, transformace, analyzovat a publikovat data a ADF spravuje a orchestruje hello komplexní dat a zpracování závislosti.</span><span class="sxs-lookup"><span data-stu-id="a8e86-112">These data pipelines can be scheduled tooingest, prepare, transform, analyze, and publish data, and ADF manages and orchestrates hello complex data and processing dependencies.</span></span> <span data-ttu-id="a8e86-113">Řešení může být hello rychle vytvořené a nasazené v cloudu, připojení stále více využívají místní a zdroje dat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="a8e86-113">Solutions can be quickly built and deployed in hello cloud, connecting a growing number of on-premises and cloud data sources.</span></span>

<span data-ttu-id="a8e86-114">Zvažte použití ADF:</span><span class="sxs-lookup"><span data-stu-id="a8e86-114">Consider using ADF:</span></span>

* <span data-ttu-id="a8e86-115">Pokud data potřebám toobe průběžně migrovat v hybridním scénáři, který přistupuje k i místní a cloudové prostředky</span><span class="sxs-lookup"><span data-stu-id="a8e86-115">when data needs toobe continually migrated in a hybrid scenario that accesses both on-premises and cloud resources</span></span>
* <span data-ttu-id="a8e86-116">Když hello dat je zpracován, nebo musí toobe upravené nebo mít obchodní logiku přidat tooit při migraci.</span><span class="sxs-lookup"><span data-stu-id="a8e86-116">when hello data is transacted or needs toobe modified or have business logic added tooit when being migrated.</span></span>

<span data-ttu-id="a8e86-117">ADF umožňuje hello plánování a sledování úloh pomocí jednoduché skripty JSON, které spravují hello přesouvání dat v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="a8e86-117">ADF allows for hello scheduling and monitoring of jobs using simple JSON scripts that manage hello movement of data on a periodic basis.</span></span> <span data-ttu-id="a8e86-118">ADF má také další funkce, například podporu pro komplexní operace.</span><span class="sxs-lookup"><span data-stu-id="a8e86-118">ADF also has other capabilities such as support for complex operations.</span></span> <span data-ttu-id="a8e86-119">Další informace o ADF, naleznete v dokumentaci k hello v [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="a8e86-119">For more information on ADF, see hello documentation at [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).</span></span>

## <span data-ttu-id="a8e86-120"><a name="scenario"></a>Hello scénář</span><span class="sxs-lookup"><span data-stu-id="a8e86-120"><a name="scenario"></a>hello Scenario</span></span>
<span data-ttu-id="a8e86-121">Nemůžeme nastavit kanál ADF, která vytvoří dvě aktivity migrace dat.</span><span class="sxs-lookup"><span data-stu-id="a8e86-121">We set up an ADF pipeline that composes two data migration activities.</span></span> <span data-ttu-id="a8e86-122">Společně se přesouvat data na každý den mezi místní databázi SQL a Azure SQL Database v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="a8e86-122">Together they move data on a daily basis between an on-premises SQL database and an Azure SQL Database in hello cloud.</span></span> <span data-ttu-id="a8e86-123">jsou dvě aktivity Hello:</span><span class="sxs-lookup"><span data-stu-id="a8e86-123">hello two activities are:</span></span>

* <span data-ttu-id="a8e86-124">kopírování dat z databáze tooan systému SQL Server na místní účet Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="a8e86-124">copy data from an on-premises SQL Server database tooan Azure Blob Storage account</span></span>
* <span data-ttu-id="a8e86-125">kopírování dat z tooan účet Azure Blob Storage hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a8e86-125">copy data from hello Azure Blob Storage account tooan Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="a8e86-126">Hello postupy zde byly upraveny, z hello podrobnější kurzu poskytované hello ADF team: [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) odkazuje toohello příslušných částech tohoto tématu jsou k dispozici v případě nutnosti.</span><span class="sxs-lookup"><span data-stu-id="a8e86-126">hello steps shown here have been adapted from hello more detailed tutorial provided by hello ADF team: [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) References toohello relevant sections of that topic are provided when appropriate.</span></span>
>
>

## <span data-ttu-id="a8e86-127"><a name="prereqs"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="a8e86-127"><a name="prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="a8e86-128">Tento kurz předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="a8e86-128">This tutorial assumes you have:</span></span>

* <span data-ttu-id="a8e86-129">**Předplatné**.</span><span class="sxs-lookup"><span data-stu-id="a8e86-129">An **Azure subscription**.</span></span> <span data-ttu-id="a8e86-130">Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8e86-130">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="a8e86-131">**Účtu úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="a8e86-131">An **Azure storage account**.</span></span> <span data-ttu-id="a8e86-132">Používáte účet úložiště Azure pro ukládání dat hello v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a8e86-132">You use an Azure storage account for storing hello data in this tutorial.</span></span> <span data-ttu-id="a8e86-133">Pokud nemáte účet úložiště Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) článku.</span><span class="sxs-lookup"><span data-stu-id="a8e86-133">If you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article.</span></span> <span data-ttu-id="a8e86-134">Po vytvoření účtu úložiště hello musíte tooobtain hello účet, který klíč používá tooaccess hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="a8e86-134">After you have created hello storage account, you need tooobtain hello account key used tooaccess hello storage.</span></span> <span data-ttu-id="a8e86-135">V tématu [Správa přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="a8e86-135">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>
* <span data-ttu-id="a8e86-136">Přístup k tooan **Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="a8e86-136">Access tooan **Azure SQL Database**.</span></span> <span data-ttu-id="a8e86-137">Pokud je potřeba nastavit Azure SQL Database, hello tpoic [Začínáme se službou Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) obsahuje informace o tooprovision novou instanci třídy Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a8e86-137">If you must set up an Azure SQL Database, hello tpoic [Getting Started with Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) provides information on how tooprovision a new instance of an Azure SQL Database.</span></span>
* <span data-ttu-id="a8e86-138">Nainstalovaný a nakonfigurovaný **prostředí Azure PowerShell** místně.</span><span class="sxs-lookup"><span data-stu-id="a8e86-138">Installed and configured **Azure PowerShell** locally.</span></span> <span data-ttu-id="a8e86-139">Pokyny najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a8e86-139">For instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

> [!NOTE]
> <span data-ttu-id="a8e86-140">Tento postup používá hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a8e86-140">This procedure uses hello [Azure portal](https://portal.azure.com/).</span></span>
>
>

## <span data-ttu-id="a8e86-141"><a name="upload-data"></a>Nahrávání hello data tooyour místní systém SQL Server</span><span class="sxs-lookup"><span data-stu-id="a8e86-141"><a name="upload-data"></a> Upload hello data tooyour on-premises SQL Server</span></span>
<span data-ttu-id="a8e86-142">Používáme hello [datovou sadu NYC taxíkem](http://chriswhong.com/open-data/foil_nyc_taxi/) proces migrace toodemonstrate hello.</span><span class="sxs-lookup"><span data-stu-id="a8e86-142">We use hello [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) toodemonstrate hello migration process.</span></span> <span data-ttu-id="a8e86-143">Hello NYC taxíkem datová sada k dispozici, jak je uvedeno v tomto příspěvku v úložišti objektů blob v Azure [NYC taxíkem Data](http://www.andresmh.com/nyctaxitrips/).</span><span class="sxs-lookup"><span data-stu-id="a8e86-143">hello NYC Taxi dataset is available, as noted in that post, on Azure blob storage [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/).</span></span> <span data-ttu-id="a8e86-144">Hello dat má dva soubory, hello trip_data.csv souboru, který obsahuje podrobnosti o cestě, a hello trip_far.csv soubor, který obsahuje podrobnosti o tarif hello placené pro každou cestu.</span><span class="sxs-lookup"><span data-stu-id="a8e86-144">hello data has two files, hello trip_data.csv file, which contains trip details, and hello  trip_far.csv file, which contains details of hello fare paid for each trip.</span></span> <span data-ttu-id="a8e86-145">Ukázka a popis tyto soubory jsou uvedeny v [NYC taxíkem služebních cest datovou sadu popis](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span><span class="sxs-lookup"><span data-stu-id="a8e86-145">A sample and description of these files are provided in [NYC Taxi Trips Dataset Description](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span></span>

<span data-ttu-id="a8e86-146">Můžete přizpůsobit hello postup uvedený v tomto poli tooa sadu svoje vlastní data, nebo postupujte podle kroků hello, jak je popsáno pomocí hello NYC taxíkem datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="a8e86-146">You can either adapt hello procedure provided here tooa set of your own data or follow hello steps as described by using hello NYC Taxi dataset.</span></span> <span data-ttu-id="a8e86-147">tooupload hello NYC taxíkem datové sady do místní databáze systému SQL Server, postupujte podle uvedených v postupu hello [hromadně importovat Data do databáze serveru SQL](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span><span class="sxs-lookup"><span data-stu-id="a8e86-147">tooupload hello NYC Taxi dataset into your on-premises SQL Server database, follow hello procedure outlined in [Bulk Import Data into SQL Server Database](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span></span> <span data-ttu-id="a8e86-148">Tyto pokyny jsou pro systém SQL Server na virtuální počítač Azure, ale hello hello postup pro odesílání toohello místní SQL Server je stejný.</span><span class="sxs-lookup"><span data-stu-id="a8e86-148">These instructions are for a SQL Server on an Azure Virtual Machine, but hello procedure for uploading toohello on-premises SQL Server is hello same.</span></span>

## <span data-ttu-id="a8e86-149"><a name="create-adf"></a>Vytvoření služby Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="a8e86-149"><a name="create-adf"></a> Create an Azure Data Factory</span></span>
<span data-ttu-id="a8e86-150">Pokyny pro vytvoření nové Azure Data Factory a skupině prostředků v hello Hello [portál Azure](https://portal.azure.com/) jsou k dispozici [vytvoření služby Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span><span class="sxs-lookup"><span data-stu-id="a8e86-150">hello instructions for creating a new Azure Data Factory and a resource group in hello [Azure portal](https://portal.azure.com/) are provided [Create an Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span></span> <span data-ttu-id="a8e86-151">Název hello novou instanci ADF *adfdsp* a skupinu prostředků, název hello vytvořili *adfdsprg*.</span><span class="sxs-lookup"><span data-stu-id="a8e86-151">Name hello new ADF instance *adfdsp* and name hello resource group created *adfdsprg*.</span></span>

## <a name="install-and-configure-up-hello-data-management-gateway"></a><span data-ttu-id="a8e86-152">Instalace a konfigurace až hello Brána pro správu dat</span><span class="sxs-lookup"><span data-stu-id="a8e86-152">Install and configure up hello Data Management Gateway</span></span>
<span data-ttu-id="a8e86-153">tooenable kanály toowork objekt pro vytváření dat Azure s SQL serverem místní, je nutné tooadd jej jako objekt pro vytváření toohello dat propojené služby.</span><span class="sxs-lookup"><span data-stu-id="a8e86-153">tooenable your pipelines in an Azure data factory toowork with an on-premises SQL Server, you need tooadd it as a Linked Service toohello data factory.</span></span> <span data-ttu-id="a8e86-154">toocreate a propojené služby SQL serveru místní, musíte:</span><span class="sxs-lookup"><span data-stu-id="a8e86-154">toocreate a Linked Service for an on-premises SQL Server, you must:</span></span>

* <span data-ttu-id="a8e86-155">Stáhněte a nainstalujte Brána pro správu dat na místním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="a8e86-155">download and install Microsoft Data Management Gateway onto hello on-premises computer.</span></span>
* <span data-ttu-id="a8e86-156">konfiguraci hello propojené služby pro brány hello toouse zdroje dat pro místní hello.</span><span class="sxs-lookup"><span data-stu-id="a8e86-156">configure hello linked service for hello on-premises data source toouse hello gateway.</span></span>

<span data-ttu-id="a8e86-157">Hello Brána pro správu dat serializuje a deserializuje hello zdroj a jímka data na hello počítači, který je hostitelem.</span><span class="sxs-lookup"><span data-stu-id="a8e86-157">hello Data Management Gateway serializes and deserializes hello source and sink data on hello computer where it is hosted.</span></span>

<span data-ttu-id="a8e86-158">Pokyny k instalaci a informace o Brána pro správu dat najdete v tématu [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="a8e86-158">For set-up instructions and details on Data Management Gateway, see [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span></span>

## <span data-ttu-id="a8e86-159"><a name="adflinkedservices"></a>Vytvoření propojené služby tooconnect toohello datových prostředků</span><span class="sxs-lookup"><span data-stu-id="a8e86-159"><a name="adflinkedservices"></a>Create linked services tooconnect toohello data resources</span></span>
<span data-ttu-id="a8e86-160">Propojená služba definuje hello informace potřebné pro Azure Data Factory tooconnect tooa datový prostředek.</span><span class="sxs-lookup"><span data-stu-id="a8e86-160">A linked service defines hello information needed for Azure Data Factory tooconnect tooa data resource.</span></span> <span data-ttu-id="a8e86-161">Hello podrobný postup pro vytvoření propojené služby je k dispozici v [vytvoření propojených služeb](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span><span class="sxs-lookup"><span data-stu-id="a8e86-161">hello step-by-step procedure for creating linked services is provided in [Create linked services](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span></span>

<span data-ttu-id="a8e86-162">Máme tři zdroje v tomto scénáři, pro které jsou potřeba propojené služby.</span><span class="sxs-lookup"><span data-stu-id="a8e86-162">We have three resources in this scenario for which linked services are needed.</span></span>

1. [<span data-ttu-id="a8e86-163">Propojené služby pro místní systém SQL Server</span><span class="sxs-lookup"><span data-stu-id="a8e86-163">Linked service for on-premises SQL Server</span></span>](#adf-linked-service-onprem-sql)
2. [<span data-ttu-id="a8e86-164">Propojené služby pro Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="a8e86-164">Linked service for Azure Blob Storage</span></span>](#adf-linked-service-blob-store)
3. [<span data-ttu-id="a8e86-165">Propojené služby pro databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="a8e86-165">Linked service for Azure SQL database</span></span>](#adf-linked-service-azure-sql)

### <span data-ttu-id="a8e86-166"><a name="adf-linked-service-onprem-sql"></a>Propojené služby pro místní databázi systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="a8e86-166"><a name="adf-linked-service-onprem-sql"></a>Linked service for on-premises SQL Server database</span></span>
<span data-ttu-id="a8e86-167">toocreate hello propojené služby pro hello místní systém SQL Server:</span><span class="sxs-lookup"><span data-stu-id="a8e86-167">toocreate hello linked service for hello on-premises SQL Server:</span></span>

* <span data-ttu-id="a8e86-168">Klikněte na tlačítko hello **Data Store** v hello ADF cílová stránka na portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="a8e86-168">click hello **Data Store** in hello ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="a8e86-169">Vyberte **SQL** a zadejte hello *uživatelské jméno* a *heslo* přihlašovací údaje pro hello místní systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a8e86-169">select **SQL** and enter hello *username* and *password* credentials for hello on-premises SQL Server.</span></span> <span data-ttu-id="a8e86-170">Je třeba tooenter hello servername jako **servername plně kvalifikovaný název instance zpětné lomítko (servername\instancename)**.</span><span class="sxs-lookup"><span data-stu-id="a8e86-170">You need tooenter hello servername as a **fully qualified servername backslash instance name (servername\instancename)**.</span></span> <span data-ttu-id="a8e86-171">Název hello propojená služba *adfonpremsql*.</span><span class="sxs-lookup"><span data-stu-id="a8e86-171">Name hello linked service *adfonpremsql*.</span></span>

### <span data-ttu-id="a8e86-172"><a name="adf-linked-service-blob-store"></a>Propojené služby pro objekt Blob</span><span class="sxs-lookup"><span data-stu-id="a8e86-172"><a name="adf-linked-service-blob-store"></a>Linked service for Blob</span></span>
<span data-ttu-id="a8e86-173">toocreate hello propojené služby pro účet Azure Blob Storage hello:</span><span class="sxs-lookup"><span data-stu-id="a8e86-173">toocreate hello linked service for hello Azure Blob Storage account:</span></span>

* <span data-ttu-id="a8e86-174">Klikněte na tlačítko hello **Data Store** v hello ADF cílová stránka na portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="a8e86-174">click hello **Data Store** in hello ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="a8e86-175">Vyberte **účet úložiště Azure**</span><span class="sxs-lookup"><span data-stu-id="a8e86-175">select **Azure Storage Account**</span></span>
* <span data-ttu-id="a8e86-176">Zadejte název klíče a kontejneru Azure Blob Storage účtu hello.</span><span class="sxs-lookup"><span data-stu-id="a8e86-176">enter hello Azure Blob Storage account key and container name.</span></span> <span data-ttu-id="a8e86-177">Název hello propojenou službu *adfds*.</span><span class="sxs-lookup"><span data-stu-id="a8e86-177">Name hello Linked Service *adfds*.</span></span>

### <span data-ttu-id="a8e86-178"><a name="adf-linked-service-azure-sql"></a>Propojené služby pro databázi Azure SQL</span><span class="sxs-lookup"><span data-stu-id="a8e86-178"><a name="adf-linked-service-azure-sql"></a>Linked service for Azure SQL database</span></span>
<span data-ttu-id="a8e86-179">toocreate hello propojené služby pro hello Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="a8e86-179">toocreate hello linked service for hello Azure SQL Database:</span></span>

* <span data-ttu-id="a8e86-180">Klikněte na tlačítko hello **Data Store** v hello ADF cílová stránka na portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="a8e86-180">click hello **Data Store** in hello ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="a8e86-181">Vyberte **Azure SQL** a zadejte hello *uživatelské jméno* a *heslo* přihlašovací údaje pro hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a8e86-181">select **Azure SQL** and enter hello *username* and *password* credentials for hello Azure SQL Database.</span></span> <span data-ttu-id="a8e86-182">Hello *uživatelské jméno* musí být zadány jako  *user@servername* .</span><span class="sxs-lookup"><span data-stu-id="a8e86-182">hello *username* must be specified as *user@servername*.</span></span>   

## <span data-ttu-id="a8e86-183"><a name="adf-tables"></a>Definovat a vytvářet tabulky toospecify jak tooaccess hello datové sady</span><span class="sxs-lookup"><span data-stu-id="a8e86-183"><a name="adf-tables"></a>Define and create tables toospecify how tooaccess hello datasets</span></span>
<span data-ttu-id="a8e86-184">Vytváření tabulek, která ke specifikaci hello struktura, umístění a dostupnost datové sady hello hello následující postupy založené na skriptech.</span><span class="sxs-lookup"><span data-stu-id="a8e86-184">Create tables that specify hello structure, location, and availability of hello datasets with hello following script-based procedures.</span></span> <span data-ttu-id="a8e86-185">Soubory JSON jsou použité toodefine hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="a8e86-185">JSON files are used toodefine hello tables.</span></span> <span data-ttu-id="a8e86-186">Další informace o struktuře hello těchto souborů naleznete v tématu [datové sady](../data-factory/data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="a8e86-186">For more information on hello structure of these files, see [Datasets](../data-factory/data-factory-create-datasets.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a8e86-187">Musí provést hello `Add-AzureAccount` rutiny před provedením hello [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) pro provedení příkazu hello je vybrán tooconfirm rutiny, která hello právo předplatné.</span><span class="sxs-lookup"><span data-stu-id="a8e86-187">You should execute hello `Add-AzureAccount` cmdlet before executing hello [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet tooconfirm that hello right Azure subscription is selected for hello command execution.</span></span> <span data-ttu-id="a8e86-188">Dokumentaci této rutiny najdete v tématu [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="a8e86-188">For documentation of this cmdlet, see [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span></span>
>
>

<span data-ttu-id="a8e86-189">Hello na základě JSON definice v tabulkách hello použijte hello následující názvy:</span><span class="sxs-lookup"><span data-stu-id="a8e86-189">hello JSON-based definitions in hello tables use hello following names:</span></span>

* <span data-ttu-id="a8e86-190">Hello **název tabulky** v hello místní SQL server je *nyctaxi_data*</span><span class="sxs-lookup"><span data-stu-id="a8e86-190">hello **table name** in hello on-premises SQL server is *nyctaxi_data*</span></span>
* <span data-ttu-id="a8e86-191">Hello **název kontejneru** v hello Azure Blob Storage je účet *containername*</span><span class="sxs-lookup"><span data-stu-id="a8e86-191">hello **container name** in hello Azure Blob Storage account is *containername*</span></span>  

<span data-ttu-id="a8e86-192">Tři definice tabulek jsou potřeba pro tento kanál ADF:</span><span class="sxs-lookup"><span data-stu-id="a8e86-192">Three table definitions are needed for this ADF pipeline:</span></span>

1. [<span data-ttu-id="a8e86-193">Místní tabulky SQL</span><span class="sxs-lookup"><span data-stu-id="a8e86-193">SQL on-premises Table</span></span>](#adf-table-onprem-sql)
2. [<span data-ttu-id="a8e86-194">Tabulka objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="a8e86-194">Blob Table </span></span>](#adf-table-blob-store)
3. [<span data-ttu-id="a8e86-195">SQL Azure Table</span><span class="sxs-lookup"><span data-stu-id="a8e86-195">SQL Azure Table</span></span>](#adf-table-azure-sql)

> [!NOTE]
> <span data-ttu-id="a8e86-196">Tyto postupy použijte toodefine prostředí Azure PowerShell a vytvořit hello ADF aktivity.</span><span class="sxs-lookup"><span data-stu-id="a8e86-196">These procedures use Azure PowerShell toodefine and create hello ADF activities.</span></span> <span data-ttu-id="a8e86-197">Ale tyto úkoly lze provést také pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a8e86-197">But these tasks can also be accomplished using hello Azure portal.</span></span> <span data-ttu-id="a8e86-198">Podrobnosti najdete v tématu [vytvoření datových sad](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span><span class="sxs-lookup"><span data-stu-id="a8e86-198">For details, see [Create datasets](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span></span>
>
>

### <span data-ttu-id="a8e86-199"><a name="adf-table-onprem-sql"></a>Místní tabulky SQL</span><span class="sxs-lookup"><span data-stu-id="a8e86-199"><a name="adf-table-onprem-sql"></a>SQL on-premises Table</span></span>
<span data-ttu-id="a8e86-200">definice tabulky Hello hello místního systému SQL Server je uveden v následující soubor JSON hello:</span><span class="sxs-lookup"><span data-stu-id="a8e86-200">hello table definition for hello on-premises SQL Server is specified in hello following JSON file:</span></span>

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

<span data-ttu-id="a8e86-201">názvy sloupců Hello nebyly zde zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="a8e86-201">hello column names were not included here.</span></span> <span data-ttu-id="a8e86-202">Můžete se na názvy sloupců hello zahrnutím je zde dílčí vyberte (pro podrobnosti zkontrolujte hello [ADF dokumentaci](../data-factory/data-factory-data-movement-activities.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="a8e86-202">You can sub-select on hello column names by including them here (for details check hello [ADF documentation](../data-factory/data-factory-data-movement-activities.md) topic.</span></span>

<span data-ttu-id="a8e86-203">Zkopírujte definici JSON hello hello tabulky do souboru s názvem *onpremtabledef.json* souboru a uložte ho tooa označuje umístění (zde předpokládá, že toobe *C:\temp\onpremtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="a8e86-203">Copy hello JSON definition of hello table into a file called *onpremtabledef.json* file and save it tooa known location (here assumed toobe *C:\temp\onpremtabledef.json*).</span></span> <span data-ttu-id="a8e86-204">Vytváření tabulek hello v ADF s hello následující rutiny Azure Powershellu:</span><span class="sxs-lookup"><span data-stu-id="a8e86-204">Create hello table in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <span data-ttu-id="a8e86-205"><a name="adf-table-blob-store"></a>Tabulka objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="a8e86-205"><a name="adf-table-blob-store"></a>Blob Table</span></span>
<span data-ttu-id="a8e86-206">Definice tabulky hello pro hello výstupní umístění objektu blob je v následující hello (mapuje hello požity dat z objektu blob tooAzure místní):</span><span class="sxs-lookup"><span data-stu-id="a8e86-206">Definition for hello table for hello output blob location is in hello following (this maps hello ingested data from on-premises tooAzure blob):</span></span>

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

<span data-ttu-id="a8e86-207">Zkopírujte definici JSON hello hello tabulky do souboru s názvem *bloboutputtabledef.json* souboru a uložte ho tooa označuje umístění (zde předpokládá, že toobe *C:\temp\bloboutputtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="a8e86-207">Copy hello JSON definition of hello table into a file called *bloboutputtabledef.json* file and save it tooa known location (here assumed toobe *C:\temp\bloboutputtabledef.json*).</span></span> <span data-ttu-id="a8e86-208">Vytváření tabulek hello v ADF s hello následující rutiny Azure Powershellu:</span><span class="sxs-lookup"><span data-stu-id="a8e86-208">Create hello table in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <span data-ttu-id="a8e86-209"><a name="adf-table-azure-sq"></a>SQL Azure Table</span><span class="sxs-lookup"><span data-stu-id="a8e86-209"><a name="adf-table-azure-sq"></a>SQL Azure Table</span></span>
<span data-ttu-id="a8e86-210">Definice pro hello tabulku pro výstup SQL Azure hello se následující hello (toto schéma mapuje hello dat pocházejících z objektu blob hello):</span><span class="sxs-lookup"><span data-stu-id="a8e86-210">Definition for hello table for hello SQL Azure output is in hello following (this schema maps hello data coming from hello blob):</span></span>

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

<span data-ttu-id="a8e86-211">Zkopírujte definici JSON hello hello tabulky do souboru s názvem *AzureSqlTable.json* souboru a uložte ho tooa označuje umístění (zde předpokládá, že toobe *C:\temp\AzureSqlTable.json*).</span><span class="sxs-lookup"><span data-stu-id="a8e86-211">Copy hello JSON definition of hello table into a file called *AzureSqlTable.json* file and save it tooa known location (here assumed toobe *C:\temp\AzureSqlTable.json*).</span></span> <span data-ttu-id="a8e86-212">Vytváření tabulek hello v ADF s hello následující rutiny Azure Powershellu:</span><span class="sxs-lookup"><span data-stu-id="a8e86-212">Create hello table in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


## <span data-ttu-id="a8e86-213"><a name="adf-pipeline"></a>Definovat a vytvořit kanál hello</span><span class="sxs-lookup"><span data-stu-id="a8e86-213"><a name="adf-pipeline"></a>Define and create hello pipeline</span></span>
<span data-ttu-id="a8e86-214">Zadejte hello aktivity, které patří toohello kanálu a vytvoření kanálu hello s hello následující postupy založené na skriptech.</span><span class="sxs-lookup"><span data-stu-id="a8e86-214">Specify hello activities that belong toohello pipeline and create hello pipeline with hello following script-based procedures.</span></span> <span data-ttu-id="a8e86-215">Soubor JSON je použité toodefine hello kanálu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a8e86-215">A JSON file is used toodefine hello pipeline properties.</span></span>

* <span data-ttu-id="a8e86-216">Hello skript předpokládá, že hello **název kanálu** je *AMLDSProcessPipeline*.</span><span class="sxs-lookup"><span data-stu-id="a8e86-216">hello script assumes that hello **pipeline name** is *AMLDSProcessPipeline*.</span></span>
* <span data-ttu-id="a8e86-217">Všimněte si také, že nastaví hello periodicity toobe kanálu hello provést na každý den základ a použití hello výchozí doba provádění úlohy hello (12: 00 UTC).</span><span class="sxs-lookup"><span data-stu-id="a8e86-217">Also note that we set hello periodicity of hello pipeline toobe executed on daily basis and use hello default execution time for hello job (12 am UTC).</span></span>

> [!NOTE]
> <span data-ttu-id="a8e86-218">Hello následující postupy použijte toodefine prostředí Azure PowerShell a vytvoření kanálu hello ADF.</span><span class="sxs-lookup"><span data-stu-id="a8e86-218">hello following procedures use Azure PowerShell toodefine and create hello ADF pipeline.</span></span> <span data-ttu-id="a8e86-219">Ale tento úkol můžete udělat taky pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a8e86-219">But this task can also be accomplished using the Azure portal.</span></span> <span data-ttu-id="a8e86-220">Podrobnosti najdete v tématu [vytvořit kanál](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span><span class="sxs-lookup"><span data-stu-id="a8e86-220">For details, see [Create pipeline](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span></span>
>
>

<span data-ttu-id="a8e86-221">Pomocí definice tabulky hello uvedeny dříve, definice kanálu hello hello ADF, je zadána následovně:</span><span class="sxs-lookup"><span data-stu-id="a8e86-221">Using hello table definitions provided previously, hello pipeline definition for hello ADF is specified as follows:</span></span>

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL tooAzure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premises SQL server tooblob",     
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
                        "description": "Push data tooSql Azure",        
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

<span data-ttu-id="a8e86-222">Zkopírujte tuto definici JSON kanálu hello do souboru s názvem *pipelinedef.json* souboru a uložte ho tooa označuje umístění (zde předpokládá, že toobe *C:\temp\pipelinedef.json*).</span><span class="sxs-lookup"><span data-stu-id="a8e86-222">Copy this JSON definition of hello pipeline into a file called *pipelinedef.json* file and save it tooa known location (here assumed toobe *C:\temp\pipelinedef.json*).</span></span> <span data-ttu-id="a8e86-223">Vytvoření kanálu hello v ADF s hello následující rutiny Azure Powershellu:</span><span class="sxs-lookup"><span data-stu-id="a8e86-223">Create hello pipeline in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

<span data-ttu-id="a8e86-224">Zkontrolujte, jestli můžete hello kanálu na hello ADF v hello portálu Azure Classic zobrazí jako následující (při kliknutí na tlačítko hello diagram)</span><span class="sxs-lookup"><span data-stu-id="a8e86-224">Confirm that you can see hello pipeline on hello ADF in hello Azure Classic Portal show up as following (when you click hello diagram)</span></span>

![ADF kanálu](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)

## <span data-ttu-id="a8e86-226"><a name="adf-pipeline-start"></a>Hello spuštění kanálu</span><span class="sxs-lookup"><span data-stu-id="a8e86-226"><a name="adf-pipeline-start"></a>Start hello Pipeline</span></span>
<span data-ttu-id="a8e86-227">Hello kanálu můžete spustit nyní pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a8e86-227">hello pipeline can now be run using hello following command:</span></span>

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

<span data-ttu-id="a8e86-228">Hello *počátečním* a *koncovým datem* hodnoty parametrů potřebovat toobe nahradí skutečnými daty hello, mezi kterými chcete toorun hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="a8e86-228">hello *startdate* and *enddate* parameter values need toobe replaced with hello actual dates between which you want hello pipeline toorun.</span></span>

<span data-ttu-id="a8e86-229">Jakmile hello kanálu provede, musí být schopný toosee hello data se zobrazí v kontejneru hello vybraného pro objekt blob hello, jeden soubor za den.</span><span class="sxs-lookup"><span data-stu-id="a8e86-229">Once hello pipeline executes, you should be able toosee hello data show up in hello container selected for hello blob, one file per day.</span></span>

<span data-ttu-id="a8e86-230">Všimněte si, že jsme nebyly využít hello funkce poskytované službou ADF toopipe data postupně.</span><span class="sxs-lookup"><span data-stu-id="a8e86-230">Note that we have not leveraged hello functionality provided by ADF toopipe data incrementally.</span></span> <span data-ttu-id="a8e86-231">Další informace o tom, jak toodo tato a další funkce poskytované verzí ADF, najdete v části hello [ADF dokumentaci](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="a8e86-231">For more information on how toodo this and other capabilities provided by ADF, see hello [ADF documentation](https://azure.microsoft.com/services/data-factory/).</span></span>
