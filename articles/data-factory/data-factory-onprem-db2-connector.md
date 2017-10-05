---
title: "Přesun dat z DB2 pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o přesunutí dat z databáze DB2 místně pomocí Azure Data Factory kopie aktivity"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c1644e17-4560-46bb-bf3c-b923126671f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: 6a89cc44724dbb5b46a9e89d6da24d9b35ddbbef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a><span data-ttu-id="f875b-103">Přesun dat z DB2 pomocí Azure Data Factory kopie aktivity</span><span class="sxs-lookup"><span data-stu-id="f875b-103">Move data from DB2 by using Azure Data Factory Copy Activity</span></span>
<span data-ttu-id="f875b-104">Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z databáze DB2 místně do úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="f875b-104">This article describes how you can use Copy Activity in Azure Data Factory to copy data from an on-premises DB2 database to a data store.</span></span> <span data-ttu-id="f875b-105">Můžete zkopírovat data do jakékoli úložiště, která je uvedena jako podporovaných jímku v [aktivity přesunu dat pro vytváření dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) článku.</span><span class="sxs-lookup"><span data-stu-id="f875b-105">You can copy data to any store that is listed as a supported sink in the [Data Factory data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> <span data-ttu-id="f875b-106">Toto téma je založený na článku objekt pro vytváření dat, která zobrazí přehled o přesun dat pomocí aktivity kopírování a jsou uvedeny kombinace podporované datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="f875b-106">This topic builds on the Data Factory article, which presents an overview of data movement by using Copy Activity and lists the supported data store combinations.</span></span> 

<span data-ttu-id="f875b-107">Aktuálně podporuje pouze přesunutí dat z databáze DB2 k objektu pro vytváření dat [úložiště dat podporovaných podřízený](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="f875b-107">Data Factory currently supports only moving data from a DB2 database to a [supported sink data store](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="f875b-108">Přesun dat z jiných dat ukládá do DB2 databáze není podporována.</span><span class="sxs-lookup"><span data-stu-id="f875b-108">Moving data from other data stores to a DB2 database is not supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f875b-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f875b-109">Prerequisites</span></span>
<span data-ttu-id="f875b-110">Objekt pro vytváření dat podporuje připojení k místní databázi DB2 pomocí [Brána pro správu dat](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="f875b-110">Data Factory supports connecting to an on-premises DB2 database by using the [data management gateway](data-factory-data-management-gateway.md).</span></span> <span data-ttu-id="f875b-111">Podrobné pokyny k nastavení brány datovém kanálu pro přesun dat najdete v tématu [přesun dat z lokálního prostředí do cloudu](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f875b-111">For step-by-step instructions to set up the gateway data pipeline to move your data, see the [Move data from on-premises to cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="f875b-112">Vyžaduje se brána, i když DB2 je hostitelem virtuálního počítače Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="f875b-112">A gateway is required even if the DB2 is hosted on Azure IaaS VM.</span></span> <span data-ttu-id="f875b-113">Bránu můžete nainstalovat na stejný virtuální počítač IaaS jako úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="f875b-113">You can install the gateway on the same IaaS VM as the data store.</span></span> <span data-ttu-id="f875b-114">Pokud brána se může připojit k databázi, můžete bránu nainstalovat na jiný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f875b-114">If the gateway can connect to the database, you can install the gateway on a different VM.</span></span>

<span data-ttu-id="f875b-115">Brána pro správu dat poskytuje integrované DB2 ovladač, takže není nutné ručně nainstalovat ovladač ke zkopírování dat z databáze DB2.</span><span class="sxs-lookup"><span data-stu-id="f875b-115">The data management gateway provides a built-in DB2 driver, so you don't need to manually install a driver to copy data from DB2.</span></span>

> [!NOTE]
> <span data-ttu-id="f875b-116">Tipy pro řešení problémů připojení a problémy brány najdete v tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) článku.</span><span class="sxs-lookup"><span data-stu-id="f875b-116">For tips on troubleshooting connection and gateway issues, see the [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) article.</span></span>


## <a name="supported-versions"></a><span data-ttu-id="f875b-117">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="f875b-117">Supported versions</span></span>
<span data-ttu-id="f875b-118">Konektor služby Data Factory DB2 podporuje tyto platformy IBM DB2 a verze správce přístupu k SQL distribuované relační databáze architektura (DRDA) verze 9, 10 a 11:</span><span class="sxs-lookup"><span data-stu-id="f875b-118">The Data Factory DB2 connector supports the following IBM DB2 platforms and versions with Distributed Relational Database Architecture (DRDA) SQL Access Manager versions 9, 10, and 11:</span></span>

* <span data-ttu-id="f875b-119">IBM DB2 pro z verze 11.1 operačního systému</span><span class="sxs-lookup"><span data-stu-id="f875b-119">IBM DB2 for z/OS version 11.1</span></span>
* <span data-ttu-id="f875b-120">IBM DB2 pro z verze 10.1 operačního systému</span><span class="sxs-lookup"><span data-stu-id="f875b-120">IBM DB2 for z/OS version 10.1</span></span>
* <span data-ttu-id="f875b-121">IBM DB2 verze i (AS400) 7.2</span><span class="sxs-lookup"><span data-stu-id="f875b-121">IBM DB2 for i (AS400) version 7.2</span></span>
* <span data-ttu-id="f875b-122">IBM DB2 i (AS400) verze 7.1</span><span class="sxs-lookup"><span data-stu-id="f875b-122">IBM DB2 for i (AS400) version 7.1</span></span>
* <span data-ttu-id="f875b-123">IBM DB2 pro Windows (LUW) verze 11, Linux a UNIX</span><span class="sxs-lookup"><span data-stu-id="f875b-123">IBM DB2 for Linux, UNIX, and Windows (LUW) version 11</span></span>
* <span data-ttu-id="f875b-124">IBM DB2 pro LUW verze 10.5</span><span class="sxs-lookup"><span data-stu-id="f875b-124">IBM DB2 for LUW version 10.5</span></span>
* <span data-ttu-id="f875b-125">IBM DB2 pro LUW verze 10.1</span><span class="sxs-lookup"><span data-stu-id="f875b-125">IBM DB2 for LUW version 10.1</span></span>

> [!TIP]
> <span data-ttu-id="f875b-126">Pokud se zobrazí chybová zpráva "nebyl nalezen balíček odpovídající žádost o provedení příkazu SQL.</span><span class="sxs-lookup"><span data-stu-id="f875b-126">If you receive the error message "The package corresponding to an SQL statement execution request was not found.</span></span> <span data-ttu-id="f875b-127">SQLSTATE = 51002 = SQLCODE-805, "z důvodu je nutné balíček není vytvořen normální uživatele na operačního systému.</span><span class="sxs-lookup"><span data-stu-id="f875b-127">SQLSTATE=51002 SQLCODE=-805," the reason is a necessary package is not created for the normal user on the OS.</span></span> <span data-ttu-id="f875b-128">Chcete-li vyřešit tento problém, postupujte podle těchto pokynů pro váš typ serveru DB2:</span><span class="sxs-lookup"><span data-stu-id="f875b-128">To resolve this issue, follow these instructions for your DB2 server type:</span></span>
> - <span data-ttu-id="f875b-129">DB2 pro i (AS400): může uživatel power vytvoření kolekce pro běžné uživatele před spuštěním aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="f875b-129">DB2 for i (AS400): Let a power user create the collection for the normal user before running Copy Activity.</span></span> <span data-ttu-id="f875b-130">Chcete-li vytvořit kolekci, použijte příkaz:`create collection <username>`</span><span class="sxs-lookup"><span data-stu-id="f875b-130">To create the collection, use the command: `create collection <username>`</span></span>
> - <span data-ttu-id="f875b-131">DB2 pro z/OS nebo LUW: použití účtu s vysokou úrovní oprávnění – power users nebo správce, který má balíček autority a vazby, BINDADD, UDĚLTE EXECUTE veřejná oprávnění – pro kopírování jednou.</span><span class="sxs-lookup"><span data-stu-id="f875b-131">DB2 for z/OS or LUW: Use a high privilege account--a power user or admin that has package authorities and BIND, BINDADD, GRANT EXECUTE TO PUBLIC permissions--to run the copy once.</span></span> <span data-ttu-id="f875b-132">Potřebný balíček je vytvořeno automaticky při vytvoření kopie.</span><span class="sxs-lookup"><span data-stu-id="f875b-132">The necessary package is automatically created during the copy.</span></span> <span data-ttu-id="f875b-133">Potom můžete přepnout zpět na normální uživatelské pro vaše další kopie spustí.</span><span class="sxs-lookup"><span data-stu-id="f875b-133">Afterward, you can switch back to the normal user for your subsequent copy runs.</span></span>

## <a name="getting-started"></a><span data-ttu-id="f875b-134">Začínáme</span><span class="sxs-lookup"><span data-stu-id="f875b-134">Getting started</span></span>
<span data-ttu-id="f875b-135">Vytvoření kanálu s aktivitou kopírování pro přesun dat z úložiště dat DB2 místně pomocí různých nástrojů a rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="f875b-135">You can create a pipeline with a copy activity to move data from an on-premises DB2 data store by using different tools and APIs:</span></span> 

- <span data-ttu-id="f875b-136">Nejjednodušší způsob, jak vytvořit kanál je pomocí Průvodce kopírováním Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f875b-136">The easiest way to create a pipeline is to use the Azure Data Factory Copy Wizard.</span></span> <span data-ttu-id="f875b-137">Rychlé návod k vytvoření kanálu pomocí Průvodce kopírováním najdete v tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="f875b-137">For a quick walkthrough on creating a pipeline by using the Copy Wizard, see the [Tutorial: Create a pipeline by using the Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span> 
- <span data-ttu-id="f875b-138">Nástroje můžete taky vytvořit kanál, včetně portálu Azure, Visual Studio, prostředí Azure PowerShell, šablonu Azure Resource Manager, .NET API a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="f875b-138">You can also use tools to create a pipeline, including the Azure portal, Visual Studio, Azure PowerShell, an Azure Resource Manager template, the .NET API, and the REST API.</span></span> <span data-ttu-id="f875b-139">Podrobné pokyny k vytvoření kanálu s aktivitou kopírování najdete v tématu [aktivity kopírování kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="f875b-139">For step-by-step instructions to create a pipeline with a copy activity, see the [Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

<span data-ttu-id="f875b-140">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="f875b-140">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="f875b-141">Vytvoření propojených služeb propojíte vstup a výstup úložiště dat do data factory.</span><span class="sxs-lookup"><span data-stu-id="f875b-141">Create linked services to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="f875b-142">Vytvořte datové sady, které představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="f875b-142">Create datasets to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="f875b-143">Vytvoření kanálu s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="f875b-143">Create a pipeline with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="f875b-144">Pokud použijete Průvodce kopírováním, JSON definice služby Data Factory propojené služeb, datových sad a kanálu entity jsou automaticky vytvoří za vás.</span><span class="sxs-lookup"><span data-stu-id="f875b-144">When you use the Copy Wizard, JSON definitions for the Data Factory linked services, datasets, and pipeline entities are automatically created for you.</span></span> <span data-ttu-id="f875b-145">Pokud používáte rozhraní API (s výjimkou .NET API) nebo nástroje, definujete ve formátu JSON entit služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f875b-145">When you use tools or APIs (except the .NET API), you define the Data Factory entities by using the JSON format.</span></span> <span data-ttu-id="f875b-146">[JSON příklad: kopírování dat z DB2 do úložiště objektů Azure Blob](#json-example-copy-data-from-db2-to-azure-blob) ukazuje definice JSON entit služby Data Factory, které se používají ke zkopírování dat z místního úložiště dat DB2.</span><span class="sxs-lookup"><span data-stu-id="f875b-146">The [JSON example: Copy data from DB2 to Azure Blob storage](#json-example-copy-data-from-db2-to-azure-blob) shows the JSON definitions for the Data Factory entities that are used to copy data from an on-premises DB2 data store.</span></span>

<span data-ttu-id="f875b-147">Následující části obsahují podrobnosti o vlastnostech JSON, které slouží k určení entit služby Data Factory, které jsou specifické pro úložiště dat DB2.</span><span class="sxs-lookup"><span data-stu-id="f875b-147">The following sections provide details about the JSON properties that are used to define the Data Factory entities that are specific to a DB2 data store.</span></span>

## <a name="db2-linked-service-properties"></a><span data-ttu-id="f875b-148">Vlastnosti DB2 propojené služby</span><span class="sxs-lookup"><span data-stu-id="f875b-148">DB2 linked service properties</span></span>
<span data-ttu-id="f875b-149">Následující tabulka uvádí vlastnosti JSON, které jsou specifické pro DB2 propojené služby.</span><span class="sxs-lookup"><span data-stu-id="f875b-149">The following table lists the JSON properties that are specific to a DB2 linked service.</span></span>

| <span data-ttu-id="f875b-150">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f875b-150">Property</span></span> | <span data-ttu-id="f875b-151">Popis</span><span class="sxs-lookup"><span data-stu-id="f875b-151">Description</span></span> | <span data-ttu-id="f875b-152">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f875b-152">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f875b-153">**Typ**</span><span class="sxs-lookup"><span data-stu-id="f875b-153">**type**</span></span> |<span data-ttu-id="f875b-154">Tato vlastnost musí být nastavená na **OnPremisesDB2**.</span><span class="sxs-lookup"><span data-stu-id="f875b-154">This property must be set to **OnPremisesDB2**.</span></span> |<span data-ttu-id="f875b-155">Ano</span><span class="sxs-lookup"><span data-stu-id="f875b-155">Yes</span></span> |
| <span data-ttu-id="f875b-156">**Server**</span><span class="sxs-lookup"><span data-stu-id="f875b-156">**server**</span></span> |<span data-ttu-id="f875b-157">Název serveru DB2.</span><span class="sxs-lookup"><span data-stu-id="f875b-157">The name of the DB2 server.</span></span> |<span data-ttu-id="f875b-158">Ano</span><span class="sxs-lookup"><span data-stu-id="f875b-158">Yes</span></span> |
| <span data-ttu-id="f875b-159">**databáze**</span><span class="sxs-lookup"><span data-stu-id="f875b-159">**database**</span></span> |<span data-ttu-id="f875b-160">Název databáze DB2.</span><span class="sxs-lookup"><span data-stu-id="f875b-160">The name of the DB2 database.</span></span> |<span data-ttu-id="f875b-161">Ano</span><span class="sxs-lookup"><span data-stu-id="f875b-161">Yes</span></span> |
| <span data-ttu-id="f875b-162">**schéma**</span><span class="sxs-lookup"><span data-stu-id="f875b-162">**schema**</span></span> |<span data-ttu-id="f875b-163">Název schématu v databázi DB2.</span><span class="sxs-lookup"><span data-stu-id="f875b-163">The name of the schema in the DB2 database.</span></span> <span data-ttu-id="f875b-164">Tato vlastnost je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="f875b-164">This property is case-sensitive.</span></span> |<span data-ttu-id="f875b-165">Ne</span><span class="sxs-lookup"><span data-stu-id="f875b-165">No</span></span> |
| <span data-ttu-id="f875b-166">**authenticationType.**</span><span class="sxs-lookup"><span data-stu-id="f875b-166">**authenticationType**</span></span> |<span data-ttu-id="f875b-167">Typ ověřování, který se používá k připojení k databázi DB2.</span><span class="sxs-lookup"><span data-stu-id="f875b-167">The type of authentication that is used to connect to the DB2 database.</span></span> <span data-ttu-id="f875b-168">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f875b-168">The possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="f875b-169">Ano</span><span class="sxs-lookup"><span data-stu-id="f875b-169">Yes</span></span> |
| <span data-ttu-id="f875b-170">**uživatelské jméno**</span><span class="sxs-lookup"><span data-stu-id="f875b-170">**username**</span></span> |<span data-ttu-id="f875b-171">Název pro uživatelský účet, pokud používáte ověřování Basic nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="f875b-171">The name for the user account if you use Basic or Windows authentication.</span></span> |<span data-ttu-id="f875b-172">Ne</span><span class="sxs-lookup"><span data-stu-id="f875b-172">No</span></span> |
| <span data-ttu-id="f875b-173">**heslo**</span><span class="sxs-lookup"><span data-stu-id="f875b-173">**password**</span></span> |<span data-ttu-id="f875b-174">Heslo pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="f875b-174">The password for the user account.</span></span> |<span data-ttu-id="f875b-175">Ne</span><span class="sxs-lookup"><span data-stu-id="f875b-175">No</span></span> |
| <span data-ttu-id="f875b-176">**gatewayName**</span><span class="sxs-lookup"><span data-stu-id="f875b-176">**gatewayName**</span></span> |<span data-ttu-id="f875b-177">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi DB2.</span><span class="sxs-lookup"><span data-stu-id="f875b-177">The name of the gateway that the Data Factory service should use to connect to the on-premises DB2 database.</span></span> |<span data-ttu-id="f875b-178">Ano</span><span class="sxs-lookup"><span data-stu-id="f875b-178">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="f875b-179">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="f875b-179">Dataset properties</span></span>
<span data-ttu-id="f875b-180">Seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f875b-180">For a list of the sections and properties that are available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="f875b-181">Částech, jako například **struktura**, **dostupnosti**a **zásad** pro datovou sadu JSON, jsou podobné pro všechny typy datovou sadu (Azure SQL, úložiště objektů Blob v Azure, Azure Table storage a tak dále).</span><span class="sxs-lookup"><span data-stu-id="f875b-181">Sections, such as **structure**, **availability**, and the **policy** for a dataset JSON, are similar for all dataset types (Azure SQL, Azure Blob storage, Azure Table storage, and so on).</span></span>

<span data-ttu-id="f875b-182">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="f875b-182">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="f875b-183">**Rámci typeProperties** části datové sady typu **RelationalTable**, což zahrnuje datová sada DB2, má následující vlastnost:</span><span class="sxs-lookup"><span data-stu-id="f875b-183">The **typeProperties** section for a dataset of type **RelationalTable**, which includes the DB2 dataset, has the following property:</span></span>

| <span data-ttu-id="f875b-184">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f875b-184">Property</span></span> | <span data-ttu-id="f875b-185">Popis</span><span class="sxs-lookup"><span data-stu-id="f875b-185">Description</span></span> | <span data-ttu-id="f875b-186">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f875b-186">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f875b-187">**Název tabulky**</span><span class="sxs-lookup"><span data-stu-id="f875b-187">**tableName**</span></span> |<span data-ttu-id="f875b-188">Název tabulky instance databáze DB2, na kterou odkazuje propojená služba.</span><span class="sxs-lookup"><span data-stu-id="f875b-188">The name of the table in the DB2 database instance that the linked service refers to.</span></span> <span data-ttu-id="f875b-189">Tato vlastnost je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="f875b-189">This property is case-sensitive.</span></span> |<span data-ttu-id="f875b-190">Ne (Pokud **dotazu** vlastnost aktivity kopírování typu **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="f875b-190">No (if the **query** property of a copy activity of type **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="f875b-191">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="f875b-191">Copy Activity properties</span></span>
<span data-ttu-id="f875b-192">Seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity kopírování najdete v tématu [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f875b-192">For a list of the sections and properties that are available for defining copy activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="f875b-193">Kopírovat vlastnosti aktivity, jako například **název**, **popis**, **vstupy** tabulky, **výstupy** tabulky, a **zásad**, jsou k dispozici pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="f875b-193">Copy Activity properties, such as **name**, **description**, **inputs** table, **outputs** table, and **policy**, are available for all types of activities.</span></span> <span data-ttu-id="f875b-194">Vlastnosti, které jsou k dispozici v **rámci typeProperties** části aktivity pro každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="f875b-194">The properties that are available in the **typeProperties** section of the activity for each activity type.</span></span> <span data-ttu-id="f875b-195">Pro aktivitu kopírování vlastnosti lišit v závislosti na typech zdrojů dat a jímky.</span><span class="sxs-lookup"><span data-stu-id="f875b-195">For Copy Activity, the properties vary depending on the types of data sources and sinks.</span></span>

<span data-ttu-id="f875b-196">Pro aktivitu kopírování, pokud je zdroj typu **RelationalSource** (která zahrnuje DB2), jsou k dispozici v následujících vlastností **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="f875b-196">For Copy Activity, when the source is of type **RelationalSource** (which includes DB2), the following properties are available in the **typeProperties** section:</span></span>

| <span data-ttu-id="f875b-197">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f875b-197">Property</span></span> | <span data-ttu-id="f875b-198">Popis</span><span class="sxs-lookup"><span data-stu-id="f875b-198">Description</span></span> | <span data-ttu-id="f875b-199">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="f875b-199">Allowed values</span></span> | <span data-ttu-id="f875b-200">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f875b-200">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f875b-201">**dotaz**</span><span class="sxs-lookup"><span data-stu-id="f875b-201">**query**</span></span> |<span data-ttu-id="f875b-202">Pomocí vlastního dotazu přečíst data.</span><span class="sxs-lookup"><span data-stu-id="f875b-202">Use the custom query to read the data.</span></span> |<span data-ttu-id="f875b-203">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="f875b-203">SQL query string.</span></span> <span data-ttu-id="f875b-204">Příklad: `"query": "select * from "MySchema"."MyTable""`</span><span class="sxs-lookup"><span data-stu-id="f875b-204">For example: `"query": "select * from "MySchema"."MyTable""`</span></span> |<span data-ttu-id="f875b-205">Ne (Pokud **tableName** je zadána vlastnost datové sady)</span><span class="sxs-lookup"><span data-stu-id="f875b-205">No (if the **tableName** property of a dataset is specified)</span></span> |

> [!NOTE]
> <span data-ttu-id="f875b-206">Schéma a tabulku názvy rozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="f875b-206">Schema and table names are case-sensitive.</span></span> <span data-ttu-id="f875b-207">V příkazu dotazu, uzavřete názvy vlastností pomocí "" (dvojité uvozovky).</span><span class="sxs-lookup"><span data-stu-id="f875b-207">In the query statement, enclose property names by using "" (double quotes).</span></span> <span data-ttu-id="f875b-208">Například:</span><span class="sxs-lookup"><span data-stu-id="f875b-208">For example:</span></span>
>
> ```sql
> "query": "select * from "DB2ADMIN"."Customers""
> ```

## <a name="json-example-copy-data-from-db2-to-azure-blob-storage"></a><span data-ttu-id="f875b-209">Příklad JSON: kopírování dat z DB2 do úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="f875b-209">JSON example: Copy data from DB2 to Azure Blob storage</span></span>
<span data-ttu-id="f875b-210">Tento příklad obsahuje ukázkové JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f875b-210">This example provides sample JSON definitions that you can use to create a pipeline by using the [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="f875b-211">Tento příklad ukazuje, jak zkopírovat data z databáze DB2 do úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="f875b-211">The example shows you how to copy data from a DB2 database to Blob storage.</span></span> <span data-ttu-id="f875b-212">Data však můžete zkopírovat do [všechna podporovaná data ukládat typ jímky](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="f875b-212">However, data can be copied to [any supported data store sink type](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using Azure Data Factory Copy Activity.</span></span>

<span data-ttu-id="f875b-213">Ukázka má následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="f875b-213">The sample has the following Data Factory entities:</span></span>

- <span data-ttu-id="f875b-214">DB2 propojené služby typu [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="f875b-214">A DB2 linked service of type [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)</span></span>
- <span data-ttu-id="f875b-215">Azure Blob storage, propojené služby typu [azurestorage.](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="f875b-215">An Azure Blob storage linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
- <span data-ttu-id="f875b-216">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="f875b-216">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)</span></span>
- <span data-ttu-id="f875b-217">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="f875b-217">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
- <span data-ttu-id="f875b-218">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f875b-218">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses the [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) properties</span></span>

<span data-ttu-id="f875b-219">Ukázka zkopíruje data z výsledku dotazu v databázi DB2 do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="f875b-219">The sample copies data from a query result in a DB2 database to an Azure blob hourly.</span></span> <span data-ttu-id="f875b-220">Vlastnosti JSON, které se používají v ukázce jsou popsané v následujících definice entity.</span><span class="sxs-lookup"><span data-stu-id="f875b-220">The JSON properties that are used in the sample are described in the sections that follow the entity definitions.</span></span>

<span data-ttu-id="f875b-221">Jako první krok nainstalujte a nakonfigurujte bránu data gateway.</span><span class="sxs-lookup"><span data-stu-id="f875b-221">As a first step, install and configure a data gateway.</span></span> <span data-ttu-id="f875b-222">Pokyny naleznete v [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f875b-222">Instructions are in the [Moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="f875b-223">**DB2 propojené služby**</span><span class="sxs-lookup"><span data-stu-id="f875b-223">**DB2 linked service**</span></span>

```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

<span data-ttu-id="f875b-224">**Objekt Blob propojená služba Azure storage**</span><span class="sxs-lookup"><span data-stu-id="f875b-224">**Azure Blob storage linked service**</span></span>

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

<span data-ttu-id="f875b-225">**Vstupní datové sady DB2**</span><span class="sxs-lookup"><span data-stu-id="f875b-225">**DB2 input dataset**</span></span>

<span data-ttu-id="f875b-226">Příkladu se předpokládá, že jste vytvořili tabulku v databázích DB2 s názvem "MyTable", která má sloupec s názvem "časové razítko" data časové řady.</span><span class="sxs-lookup"><span data-stu-id="f875b-226">The sample assumes that you have created a table in DB2 named "MyTable" that has a column labeled "timestamp" for the time series data.</span></span>

<span data-ttu-id="f875b-227">**Externí** je nastavena na hodnotu "true".</span><span class="sxs-lookup"><span data-stu-id="f875b-227">The **external** property is set to "true."</span></span> <span data-ttu-id="f875b-228">Toto nastavení služba Data Factory informuje, že tato datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="f875b-228">This setting informs the Data Factory service that this dataset is external to the data factory and is not produced by an activity in the data factory.</span></span> <span data-ttu-id="f875b-229">Všimněte si, že **typ** je nastavena na **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="f875b-229">Notice that the **type** property is set to **RelationalTable**.</span></span>


```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="f875b-230">**Výstupní datovou sadu objektů Blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="f875b-230">**Azure Blob output dataset**</span></span>

<span data-ttu-id="f875b-231">Data se zapisují do nového objektu blob každou hodinu nastavením **frekvence** vlastnost "Hodinu" a **interval** vlastnost na hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="f875b-231">Data is written to a new blob every hour by setting the **frequency** property to "Hour" and the **interval** property to 1.</span></span> <span data-ttu-id="f875b-232">**FolderPath** vlastností pro objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="f875b-232">The **folderPath** property for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="f875b-233">Cesta ke složce používá rok, měsíc, den a hodina části čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="f875b-233">The folder path uses the year, month, day, and hour parts of the start time.</span></span>

```json
{
    "name": "AzureBlobDb2DataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="f875b-234">**Kanál pro aktivitu kopírování**</span><span class="sxs-lookup"><span data-stu-id="f875b-234">**Pipeline for the copy activity**</span></span>

<span data-ttu-id="f875b-235">Kanál obsahuje aktivitu kopírování, která je nakonfigurována pro používání zadaný vstup a výstupní datové sady a který je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="f875b-235">The pipeline contains a copy activity that is configured to use specified input and output datasets and which is scheduled to run every hour.</span></span> <span data-ttu-id="f875b-236">V definici JSON pro kanál **zdroj** je typ nastaven na **RelationalSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="f875b-236">In the JSON definition for the pipeline, the **source** type is set to **RelationalSource** and the **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="f875b-237">Zadané pro dotaz SQL **dotazu** vlastnost vybere data z tabulky "Objednávky".</span><span class="sxs-lookup"><span data-stu-id="f875b-237">The SQL query specified for the **query** property selects the data from the "Orders" table.</span></span>

```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for the copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"Orders\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "Db2DataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDb2DataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "Db2ToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-db2"></a><span data-ttu-id="f875b-238">Mapování typu pro DB2</span><span class="sxs-lookup"><span data-stu-id="f875b-238">Type mapping for DB2</span></span>
<span data-ttu-id="f875b-239">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí převody typů automatické z typ zdroje pro typ jímky pomocí následující postup ve dvou krocích:</span><span class="sxs-lookup"><span data-stu-id="f875b-239">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy Activity performs automatic type conversions from source type to sink type by using the following two-step approach:</span></span>

1. <span data-ttu-id="f875b-240">Převést z typu nativní zdroj na typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="f875b-240">Convert from a native source type to a .NET type</span></span>
2. <span data-ttu-id="f875b-241">Převést na typ nativní jímky typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="f875b-241">Convert from a .NET type to a native sink type</span></span>

<span data-ttu-id="f875b-242">Aktivita kopírování převádí data z typu DB2 na typ .NET jsou použity následující mapování:</span><span class="sxs-lookup"><span data-stu-id="f875b-242">The following mappings are used when Copy Activity converts the data from a DB2 type to a .NET type:</span></span>

| <span data-ttu-id="f875b-243">Typ databáze DB2</span><span class="sxs-lookup"><span data-stu-id="f875b-243">DB2 database type</span></span> | <span data-ttu-id="f875b-244">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="f875b-244">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="f875b-245">SmallInt</span><span class="sxs-lookup"><span data-stu-id="f875b-245">SmallInt</span></span> |<span data-ttu-id="f875b-246">Int16</span><span class="sxs-lookup"><span data-stu-id="f875b-246">Int16</span></span> |
| <span data-ttu-id="f875b-247">Integer</span><span class="sxs-lookup"><span data-stu-id="f875b-247">Integer</span></span> |<span data-ttu-id="f875b-248">Int32</span><span class="sxs-lookup"><span data-stu-id="f875b-248">Int32</span></span> |
| <span data-ttu-id="f875b-249">BigInt</span><span class="sxs-lookup"><span data-stu-id="f875b-249">BigInt</span></span> |<span data-ttu-id="f875b-250">Int64</span><span class="sxs-lookup"><span data-stu-id="f875b-250">Int64</span></span> |
| <span data-ttu-id="f875b-251">Real</span><span class="sxs-lookup"><span data-stu-id="f875b-251">Real</span></span> |<span data-ttu-id="f875b-252">Jeden</span><span class="sxs-lookup"><span data-stu-id="f875b-252">Single</span></span> |
| <span data-ttu-id="f875b-253">Double</span><span class="sxs-lookup"><span data-stu-id="f875b-253">Double</span></span> |<span data-ttu-id="f875b-254">Double</span><span class="sxs-lookup"><span data-stu-id="f875b-254">Double</span></span> |
| <span data-ttu-id="f875b-255">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="f875b-255">Float</span></span> |<span data-ttu-id="f875b-256">Double</span><span class="sxs-lookup"><span data-stu-id="f875b-256">Double</span></span> |
| <span data-ttu-id="f875b-257">Decimal</span><span class="sxs-lookup"><span data-stu-id="f875b-257">Decimal</span></span> |<span data-ttu-id="f875b-258">Decimal</span><span class="sxs-lookup"><span data-stu-id="f875b-258">Decimal</span></span> |
| <span data-ttu-id="f875b-259">DecimalFloat</span><span class="sxs-lookup"><span data-stu-id="f875b-259">DecimalFloat</span></span> |<span data-ttu-id="f875b-260">Decimal</span><span class="sxs-lookup"><span data-stu-id="f875b-260">Decimal</span></span> |
| <span data-ttu-id="f875b-261">číselné</span><span class="sxs-lookup"><span data-stu-id="f875b-261">Numeric</span></span> |<span data-ttu-id="f875b-262">Decimal</span><span class="sxs-lookup"><span data-stu-id="f875b-262">Decimal</span></span> |
| <span data-ttu-id="f875b-263">Datum</span><span class="sxs-lookup"><span data-stu-id="f875b-263">Date</span></span> |<span data-ttu-id="f875b-264">Data a času</span><span class="sxs-lookup"><span data-stu-id="f875b-264">DateTime</span></span> |
| <span data-ttu-id="f875b-265">Čas</span><span class="sxs-lookup"><span data-stu-id="f875b-265">Time</span></span> |<span data-ttu-id="f875b-266">Časový interval</span><span class="sxs-lookup"><span data-stu-id="f875b-266">TimeSpan</span></span> |
| <span data-ttu-id="f875b-267">časové razítko</span><span class="sxs-lookup"><span data-stu-id="f875b-267">Timestamp</span></span> |<span data-ttu-id="f875b-268">Data a času</span><span class="sxs-lookup"><span data-stu-id="f875b-268">DateTime</span></span> |
| <span data-ttu-id="f875b-269">XML</span><span class="sxs-lookup"><span data-stu-id="f875b-269">Xml</span></span> |<span data-ttu-id="f875b-270">Byte]</span><span class="sxs-lookup"><span data-stu-id="f875b-270">Byte[]</span></span> |
| <span data-ttu-id="f875b-271">Char</span><span class="sxs-lookup"><span data-stu-id="f875b-271">Char</span></span> |<span data-ttu-id="f875b-272">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f875b-272">String</span></span> |
| <span data-ttu-id="f875b-273">VarChar</span><span class="sxs-lookup"><span data-stu-id="f875b-273">VarChar</span></span> |<span data-ttu-id="f875b-274">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f875b-274">String</span></span> |
| <span data-ttu-id="f875b-275">LongVarChar</span><span class="sxs-lookup"><span data-stu-id="f875b-275">LongVarChar</span></span> |<span data-ttu-id="f875b-276">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f875b-276">String</span></span> |
| <span data-ttu-id="f875b-277">DB2DynArray</span><span class="sxs-lookup"><span data-stu-id="f875b-277">DB2DynArray</span></span> |<span data-ttu-id="f875b-278">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f875b-278">String</span></span> |
| <span data-ttu-id="f875b-279">Binární</span><span class="sxs-lookup"><span data-stu-id="f875b-279">Binary</span></span> |<span data-ttu-id="f875b-280">Byte]</span><span class="sxs-lookup"><span data-stu-id="f875b-280">Byte[]</span></span> |
| <span data-ttu-id="f875b-281">VarBinary</span><span class="sxs-lookup"><span data-stu-id="f875b-281">VarBinary</span></span> |<span data-ttu-id="f875b-282">Byte]</span><span class="sxs-lookup"><span data-stu-id="f875b-282">Byte[]</span></span> |
| <span data-ttu-id="f875b-283">LongVarBinary</span><span class="sxs-lookup"><span data-stu-id="f875b-283">LongVarBinary</span></span> |<span data-ttu-id="f875b-284">Byte]</span><span class="sxs-lookup"><span data-stu-id="f875b-284">Byte[]</span></span> |
| <span data-ttu-id="f875b-285">Obrázek</span><span class="sxs-lookup"><span data-stu-id="f875b-285">Graphic</span></span> |<span data-ttu-id="f875b-286">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f875b-286">String</span></span> |
| <span data-ttu-id="f875b-287">VarGraphic</span><span class="sxs-lookup"><span data-stu-id="f875b-287">VarGraphic</span></span> |<span data-ttu-id="f875b-288">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f875b-288">String</span></span> |
| <span data-ttu-id="f875b-289">LongVarGraphic</span><span class="sxs-lookup"><span data-stu-id="f875b-289">LongVarGraphic</span></span> |<span data-ttu-id="f875b-290">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f875b-290">String</span></span> |
| <span data-ttu-id="f875b-291">Datový typ CLOB</span><span class="sxs-lookup"><span data-stu-id="f875b-291">Clob</span></span> |<span data-ttu-id="f875b-292">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f875b-292">String</span></span> |
| <span data-ttu-id="f875b-293">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="f875b-293">Blob</span></span> |<span data-ttu-id="f875b-294">Byte]</span><span class="sxs-lookup"><span data-stu-id="f875b-294">Byte[]</span></span> |
| <span data-ttu-id="f875b-295">DbClob</span><span class="sxs-lookup"><span data-stu-id="f875b-295">DbClob</span></span> |<span data-ttu-id="f875b-296">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f875b-296">String</span></span> |
| <span data-ttu-id="f875b-297">SmallInt</span><span class="sxs-lookup"><span data-stu-id="f875b-297">SmallInt</span></span> |<span data-ttu-id="f875b-298">Int16</span><span class="sxs-lookup"><span data-stu-id="f875b-298">Int16</span></span> |
| <span data-ttu-id="f875b-299">Integer</span><span class="sxs-lookup"><span data-stu-id="f875b-299">Integer</span></span> |<span data-ttu-id="f875b-300">Int32</span><span class="sxs-lookup"><span data-stu-id="f875b-300">Int32</span></span> |
| <span data-ttu-id="f875b-301">BigInt</span><span class="sxs-lookup"><span data-stu-id="f875b-301">BigInt</span></span> |<span data-ttu-id="f875b-302">Int64</span><span class="sxs-lookup"><span data-stu-id="f875b-302">Int64</span></span> |
| <span data-ttu-id="f875b-303">Real</span><span class="sxs-lookup"><span data-stu-id="f875b-303">Real</span></span> |<span data-ttu-id="f875b-304">Jeden</span><span class="sxs-lookup"><span data-stu-id="f875b-304">Single</span></span> |
| <span data-ttu-id="f875b-305">Double</span><span class="sxs-lookup"><span data-stu-id="f875b-305">Double</span></span> |<span data-ttu-id="f875b-306">Double</span><span class="sxs-lookup"><span data-stu-id="f875b-306">Double</span></span> |
| <span data-ttu-id="f875b-307">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="f875b-307">Float</span></span> |<span data-ttu-id="f875b-308">Double</span><span class="sxs-lookup"><span data-stu-id="f875b-308">Double</span></span> |
| <span data-ttu-id="f875b-309">Decimal</span><span class="sxs-lookup"><span data-stu-id="f875b-309">Decimal</span></span> |<span data-ttu-id="f875b-310">Decimal</span><span class="sxs-lookup"><span data-stu-id="f875b-310">Decimal</span></span> |
| <span data-ttu-id="f875b-311">DecimalFloat</span><span class="sxs-lookup"><span data-stu-id="f875b-311">DecimalFloat</span></span> |<span data-ttu-id="f875b-312">Decimal</span><span class="sxs-lookup"><span data-stu-id="f875b-312">Decimal</span></span> |
| <span data-ttu-id="f875b-313">číselné</span><span class="sxs-lookup"><span data-stu-id="f875b-313">Numeric</span></span> |<span data-ttu-id="f875b-314">Decimal</span><span class="sxs-lookup"><span data-stu-id="f875b-314">Decimal</span></span> |
| <span data-ttu-id="f875b-315">Datum</span><span class="sxs-lookup"><span data-stu-id="f875b-315">Date</span></span> |<span data-ttu-id="f875b-316">Data a času</span><span class="sxs-lookup"><span data-stu-id="f875b-316">DateTime</span></span> |
| <span data-ttu-id="f875b-317">Čas</span><span class="sxs-lookup"><span data-stu-id="f875b-317">Time</span></span> |<span data-ttu-id="f875b-318">Časový interval</span><span class="sxs-lookup"><span data-stu-id="f875b-318">TimeSpan</span></span> |
| <span data-ttu-id="f875b-319">časové razítko</span><span class="sxs-lookup"><span data-stu-id="f875b-319">Timestamp</span></span> |<span data-ttu-id="f875b-320">Data a času</span><span class="sxs-lookup"><span data-stu-id="f875b-320">DateTime</span></span> |
| <span data-ttu-id="f875b-321">XML</span><span class="sxs-lookup"><span data-stu-id="f875b-321">Xml</span></span> |<span data-ttu-id="f875b-322">Byte]</span><span class="sxs-lookup"><span data-stu-id="f875b-322">Byte[]</span></span> |
| <span data-ttu-id="f875b-323">Char</span><span class="sxs-lookup"><span data-stu-id="f875b-323">Char</span></span> |<span data-ttu-id="f875b-324">Řetězec</span><span class="sxs-lookup"><span data-stu-id="f875b-324">String</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="f875b-325">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="f875b-325">Map source to sink columns</span></span>
<span data-ttu-id="f875b-326">Další postupy k mapování sloupců v datové sadě zdroje na sloupců v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="f875b-326">To learn how to map columns in the source dataset to columns in the sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-reads-from-relational-sources"></a><span data-ttu-id="f875b-327">Opakovatelných čtení z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="f875b-327">Repeatable reads from relational sources</span></span>
<span data-ttu-id="f875b-328">Při kopírování dat z relační datové úložiště, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="f875b-328">When you copy data from a relational data store, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="f875b-329">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="f875b-329">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="f875b-330">Můžete také nakonfigurovat opakovaném **zásad** vlastnost pro datovou sadu řez znovu spustit, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="f875b-330">You can also configure the retry **policy** property for a dataset to rerun a slice when a failure occurs.</span></span> <span data-ttu-id="f875b-331">Ujistěte se, že stejná data je pro čtení bez ohledu na to, kolikrát se znovu spustí řez, a to bez ohledu na to, jak spustit řez znovu.</span><span class="sxs-lookup"><span data-stu-id="f875b-331">Make sure that the same data is read no matter how many times the slice is rerun, and regardless of how you rerun the slice.</span></span> <span data-ttu-id="f875b-332">Další informace najdete v tématu [Repeatable čte z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="f875b-332">For more information, see [Repeatable reads from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="f875b-333">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="f875b-333">Performance and tuning</span></span>
<span data-ttu-id="f875b-334">Další informace o klíčových faktorů, které ovlivňují výkon aktivity kopírování a způsoby, jak optimalizovat výkon v [výkonu kopie aktivity a ladění průvodce](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="f875b-334">Learn about key factors that affect the performance of Copy Activity and ways to optimize performance in the [Copy Activity Performance and Tuning Guide](data-factory-copy-activity-performance.md).</span></span>