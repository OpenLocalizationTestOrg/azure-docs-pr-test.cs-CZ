---
title: "aaaMove data z DB2 pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak toomove data z DB2 místní databázi pomocí aktivity kopírování objektu pro vytváření dat Azure"
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
ms.openlocfilehash: 696ac059be644cb3901c37d2fc746e0682c65a1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a><span data-ttu-id="95f68-103">Přesun dat z DB2 pomocí Azure Data Factory kopie aktivity</span><span class="sxs-lookup"><span data-stu-id="95f68-103">Move data from DB2 by using Azure Data Factory Copy Activity</span></span>
<span data-ttu-id="95f68-104">Tento článek popisuje, jak můžete použít aktivitu kopírování v Azure Data Factory toocopy dat z úložiště dat tooa databáze DB2 místně.</span><span class="sxs-lookup"><span data-stu-id="95f68-104">This article describes how you can use Copy Activity in Azure Data Factory toocopy data from an on-premises DB2 database tooa data store.</span></span> <span data-ttu-id="95f68-105">Můžete zkopírovat tooany úložiště dat, která je uvedena jako podporovaných jímku v hello [aktivity přesunu dat pro vytváření dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) článku.</span><span class="sxs-lookup"><span data-stu-id="95f68-105">You can copy data tooany store that is listed as a supported sink in hello [Data Factory data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> <span data-ttu-id="95f68-106">Toto téma je založený na článek hello Data Factory, který zobrazí přehled o přesun dat pomocí aktivity kopírování a jsou uvedeny kombinace úložiště dat hello podporována.</span><span class="sxs-lookup"><span data-stu-id="95f68-106">This topic builds on hello Data Factory article, which presents an overview of data movement by using Copy Activity and lists hello supported data store combinations.</span></span> 

<span data-ttu-id="95f68-107">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z databáze tooa DB2 [úložiště dat podporovaných podřízený](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="95f68-107">Data Factory currently supports only moving data from a DB2 database tooa [supported sink data store](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="95f68-108">Přesun dat z jiných dat ukládá tooa DB2 databáze není podporována.</span><span class="sxs-lookup"><span data-stu-id="95f68-108">Moving data from other data stores tooa DB2 database is not supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95f68-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="95f68-109">Prerequisites</span></span>
<span data-ttu-id="95f68-110">Objekt pro vytváření dat podporuje připojování tooan místní databázi DB2 pomocí hello [Brána pro správu dat](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="95f68-110">Data Factory supports connecting tooan on-premises DB2 database by using hello [data management gateway](data-factory-data-management-gateway.md).</span></span> <span data-ttu-id="95f68-111">Podrobné pokyny tooset hello brány dat kanálu toomove dat naleznete v tématu hello [přesun dat z místní toocloud](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="95f68-111">For step-by-step instructions tooset up hello gateway data pipeline toomove your data, see hello [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="95f68-112">Vyžaduje se brána, i pokud hello DB2 je hostitelem virtuálního počítače Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="95f68-112">A gateway is required even if hello DB2 is hosted on Azure IaaS VM.</span></span> <span data-ttu-id="95f68-113">Hello brány můžete nainstalovat na hello stejný virtuální počítač IaaS jako úložiště dat hello.</span><span class="sxs-lookup"><span data-stu-id="95f68-113">You can install hello gateway on hello same IaaS VM as hello data store.</span></span> <span data-ttu-id="95f68-114">Pokud brána hello se můžete připojit toohello databáze, můžete hello bránu nainstalovat na jiný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="95f68-114">If hello gateway can connect toohello database, you can install hello gateway on a different VM.</span></span>

<span data-ttu-id="95f68-115">Brána pro správu dat Hello poskytuje integrované ovladače DB2, není nutné toomanually nainstalujte dat toocopy ovladače z DB2.</span><span class="sxs-lookup"><span data-stu-id="95f68-115">hello data management gateway provides a built-in DB2 driver, so you don't need toomanually install a driver toocopy data from DB2.</span></span>

> [!NOTE]
> <span data-ttu-id="95f68-116">Tipy pro řešení problémů připojení a problémy brány najdete v tématu hello [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) článku.</span><span class="sxs-lookup"><span data-stu-id="95f68-116">For tips on troubleshooting connection and gateway issues, see hello [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) article.</span></span>


## <a name="supported-versions"></a><span data-ttu-id="95f68-117">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="95f68-117">Supported versions</span></span>
<span data-ttu-id="95f68-118">konektor Hello DB2 objekt pro vytváření dat podporuje hello IBM DB2 platformy a verze správce přístupu k SQL distribuované relační databáze architektura (DRDA) verze 9, 10 a 11:</span><span class="sxs-lookup"><span data-stu-id="95f68-118">hello Data Factory DB2 connector supports hello following IBM DB2 platforms and versions with Distributed Relational Database Architecture (DRDA) SQL Access Manager versions 9, 10, and 11:</span></span>

* <span data-ttu-id="95f68-119">IBM DB2 pro z verze 11.1 operačního systému</span><span class="sxs-lookup"><span data-stu-id="95f68-119">IBM DB2 for z/OS version 11.1</span></span>
* <span data-ttu-id="95f68-120">IBM DB2 pro z verze 10.1 operačního systému</span><span class="sxs-lookup"><span data-stu-id="95f68-120">IBM DB2 for z/OS version 10.1</span></span>
* <span data-ttu-id="95f68-121">IBM DB2 verze i (AS400) 7.2</span><span class="sxs-lookup"><span data-stu-id="95f68-121">IBM DB2 for i (AS400) version 7.2</span></span>
* <span data-ttu-id="95f68-122">IBM DB2 i (AS400) verze 7.1</span><span class="sxs-lookup"><span data-stu-id="95f68-122">IBM DB2 for i (AS400) version 7.1</span></span>
* <span data-ttu-id="95f68-123">IBM DB2 pro Windows (LUW) verze 11, Linux a UNIX</span><span class="sxs-lookup"><span data-stu-id="95f68-123">IBM DB2 for Linux, UNIX, and Windows (LUW) version 11</span></span>
* <span data-ttu-id="95f68-124">IBM DB2 pro LUW verze 10.5</span><span class="sxs-lookup"><span data-stu-id="95f68-124">IBM DB2 for LUW version 10.5</span></span>
* <span data-ttu-id="95f68-125">IBM DB2 pro LUW verze 10.1</span><span class="sxs-lookup"><span data-stu-id="95f68-125">IBM DB2 for LUW version 10.1</span></span>

> [!TIP]
> <span data-ttu-id="95f68-126">Pokud se zobrazí chybová zpráva hello "hello balíčku odpovídající tooan SQL příkaz žádost o spuštění nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="95f68-126">If you receive hello error message "hello package corresponding tooan SQL statement execution request was not found.</span></span> <span data-ttu-id="95f68-127">SQLSTATE = 51002 = SQLCODE-805, "hello důvod je potřebný balíček není vytvořena pro hello normální uživatele na hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="95f68-127">SQLSTATE=51002 SQLCODE=-805," hello reason is a necessary package is not created for hello normal user on hello OS.</span></span> <span data-ttu-id="95f68-128">tooresolve-li tento problém, postupujte podle těchto pokynů pro váš typ serveru DB2:</span><span class="sxs-lookup"><span data-stu-id="95f68-128">tooresolve this issue, follow these instructions for your DB2 server type:</span></span>
> - <span data-ttu-id="95f68-129">DB2 pro i (AS400): může uživatel power vytvoření kolekce hello hello normální uživatele před spuštěním aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="95f68-129">DB2 for i (AS400): Let a power user create hello collection for hello normal user before running Copy Activity.</span></span> <span data-ttu-id="95f68-130">toocreate hello shromažďování, použití hello příkaz:`create collection <username>`</span><span class="sxs-lookup"><span data-stu-id="95f68-130">toocreate hello collection, use hello command: `create collection <username>`</span></span>
> - <span data-ttu-id="95f68-131">DB2 pro z/OS nebo LUW: použití účtu s vysokou úrovní oprávnění – power users nebo správce, který má balíček autority a vazby, BINDADD, UDĚLTE oprávnění spouštět tooPUBLIC – toorun hello jednou kopírování.</span><span class="sxs-lookup"><span data-stu-id="95f68-131">DB2 for z/OS or LUW: Use a high privilege account--a power user or admin that has package authorities and BIND, BINDADD, GRANT EXECUTE tooPUBLIC permissions--toorun hello copy once.</span></span> <span data-ttu-id="95f68-132">Hello potřebný balíček je automaticky vytvoří během kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="95f68-132">hello necessary package is automatically created during hello copy.</span></span> <span data-ttu-id="95f68-133">Potom můžete přepnout zpět toohello normální pro vaše další kopie spustí.</span><span class="sxs-lookup"><span data-stu-id="95f68-133">Afterward, you can switch back toohello normal user for your subsequent copy runs.</span></span>

## <a name="getting-started"></a><span data-ttu-id="95f68-134">Začínáme</span><span class="sxs-lookup"><span data-stu-id="95f68-134">Getting started</span></span>
<span data-ttu-id="95f68-135">Můžete vytvoření kanálu s kopie aktivity toomove dat z úložiště dat DB2 místně pomocí různých nástrojů a rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="95f68-135">You can create a pipeline with a copy activity toomove data from an on-premises DB2 data store by using different tools and APIs:</span></span> 

- <span data-ttu-id="95f68-136">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello Průvodce kopírováním objekt pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="95f68-136">hello easiest way toocreate a pipeline is toouse hello Azure Data Factory Copy Wizard.</span></span> <span data-ttu-id="95f68-137">Rychlé návod k vytvoření kanálu pomocí Průvodce kopírováním hello najdete v tématu hello [kurz: vytvoření kanálu pomocí Průvodce kopírováním hello](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="95f68-137">For a quick walkthrough on creating a pipeline by using hello Copy Wizard, see hello [Tutorial: Create a pipeline by using hello Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span> 
- <span data-ttu-id="95f68-138">Můžete použít také nástroje toocreate kanál, včetně hello portálu Azure, Visual Studio, prostředí Azure PowerShell, Azure Resource Manager šablony, hello .NET API a hello REST API.</span><span class="sxs-lookup"><span data-stu-id="95f68-138">You can also use tools toocreate a pipeline, including hello Azure portal, Visual Studio, Azure PowerShell, an Azure Resource Manager template, hello .NET API, and hello REST API.</span></span> <span data-ttu-id="95f68-139">Podrobné pokyny toocreate kanál s aktivitou kopírování, najdete v části hello [aktivity kopírování kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="95f68-139">For step-by-step instructions toocreate a pipeline with a copy activity, see hello [Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

<span data-ttu-id="95f68-140">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="95f68-140">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="95f68-141">Vytvoření propojené služby toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="95f68-141">Create linked services toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="95f68-142">Vytvoření datových sad toorepresent vstupní a výstupní data pro operace kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="95f68-142">Create datasets toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="95f68-143">Vytvoření kanálu s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="95f68-143">Create a pipeline with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="95f68-144">Při použití hello Průvodce kopírováním, definice JSON pro hello Data Factory propojené služby, můžete vytvořit automaticky datových sad a kanálu entity.</span><span class="sxs-lookup"><span data-stu-id="95f68-144">When you use hello Copy Wizard, JSON definitions for hello Data Factory linked services, datasets, and pipeline entities are automatically created for you.</span></span> <span data-ttu-id="95f68-145">Při použití nástroje nebo rozhraní API (s výjimkou hello .NET API), můžete definovat hello entit služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="95f68-145">When you use tools or APIs (except hello .NET API), you define hello Data Factory entities by using hello JSON format.</span></span> <span data-ttu-id="95f68-146">Hello [JSON příklad: kopírování dat z DB2 tooAzure úložiště objektů Blob](#json-example-copy-data-from-db2-to-azure-blob) ukazuje hello JSON definice pro hello entit služby Data Factory, které jsou používané toocopy data z místního úložiště dat DB2.</span><span class="sxs-lookup"><span data-stu-id="95f68-146">hello [JSON example: Copy data from DB2 tooAzure Blob storage](#json-example-copy-data-from-db2-to-azure-blob) shows hello JSON definitions for hello Data Factory entities that are used toocopy data from an on-premises DB2 data store.</span></span>

<span data-ttu-id="95f68-147">Hello následující části obsahují podrobné informace o hello JSON vlastnosti, které jsou používané toodefine hello objekt pro vytváření dat entity, které jsou úložiště dat konkrétní tooa DB2.</span><span class="sxs-lookup"><span data-stu-id="95f68-147">hello following sections provide details about hello JSON properties that are used toodefine hello Data Factory entities that are specific tooa DB2 data store.</span></span>

## <a name="db2-linked-service-properties"></a><span data-ttu-id="95f68-148">Vlastnosti DB2 propojené služby</span><span class="sxs-lookup"><span data-stu-id="95f68-148">DB2 linked service properties</span></span>
<span data-ttu-id="95f68-149">Hello následující tabulka uvádí vlastnosti hello JSON, které jsou specifické tooa DB2 propojené služby.</span><span class="sxs-lookup"><span data-stu-id="95f68-149">hello following table lists hello JSON properties that are specific tooa DB2 linked service.</span></span>

| <span data-ttu-id="95f68-150">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="95f68-150">Property</span></span> | <span data-ttu-id="95f68-151">Popis</span><span class="sxs-lookup"><span data-stu-id="95f68-151">Description</span></span> | <span data-ttu-id="95f68-152">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="95f68-152">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="95f68-153">**Typ**</span><span class="sxs-lookup"><span data-stu-id="95f68-153">**type**</span></span> |<span data-ttu-id="95f68-154">Tato vlastnost musí být nastavená příliš**OnPremisesDB2**.</span><span class="sxs-lookup"><span data-stu-id="95f68-154">This property must be set too**OnPremisesDB2**.</span></span> |<span data-ttu-id="95f68-155">Ano</span><span class="sxs-lookup"><span data-stu-id="95f68-155">Yes</span></span> |
| <span data-ttu-id="95f68-156">**Server**</span><span class="sxs-lookup"><span data-stu-id="95f68-156">**server**</span></span> |<span data-ttu-id="95f68-157">Hello název serveru hello DB2.</span><span class="sxs-lookup"><span data-stu-id="95f68-157">hello name of hello DB2 server.</span></span> |<span data-ttu-id="95f68-158">Ano</span><span class="sxs-lookup"><span data-stu-id="95f68-158">Yes</span></span> |
| <span data-ttu-id="95f68-159">**databáze**</span><span class="sxs-lookup"><span data-stu-id="95f68-159">**database**</span></span> |<span data-ttu-id="95f68-160">Hello název databáze hello DB2.</span><span class="sxs-lookup"><span data-stu-id="95f68-160">hello name of hello DB2 database.</span></span> |<span data-ttu-id="95f68-161">Ano</span><span class="sxs-lookup"><span data-stu-id="95f68-161">Yes</span></span> |
| <span data-ttu-id="95f68-162">**schéma**</span><span class="sxs-lookup"><span data-stu-id="95f68-162">**schema**</span></span> |<span data-ttu-id="95f68-163">Název Hello hello schématu v databázi hello DB2.</span><span class="sxs-lookup"><span data-stu-id="95f68-163">hello name of hello schema in hello DB2 database.</span></span> <span data-ttu-id="95f68-164">Tato vlastnost je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="95f68-164">This property is case-sensitive.</span></span> |<span data-ttu-id="95f68-165">Ne</span><span class="sxs-lookup"><span data-stu-id="95f68-165">No</span></span> |
| <span data-ttu-id="95f68-166">**authenticationType.**</span><span class="sxs-lookup"><span data-stu-id="95f68-166">**authenticationType**</span></span> |<span data-ttu-id="95f68-167">Hello typ ověřování, které je použité tooconnect toohello DB2 databáze.</span><span class="sxs-lookup"><span data-stu-id="95f68-167">hello type of authentication that is used tooconnect toohello DB2 database.</span></span> <span data-ttu-id="95f68-168">Hello možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="95f68-168">hello possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="95f68-169">Ano</span><span class="sxs-lookup"><span data-stu-id="95f68-169">Yes</span></span> |
| <span data-ttu-id="95f68-170">**uživatelské jméno**</span><span class="sxs-lookup"><span data-stu-id="95f68-170">**username**</span></span> |<span data-ttu-id="95f68-171">Název Hello hello uživatelskému účtu, pokud používáte ověřování Basic nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="95f68-171">hello name for hello user account if you use Basic or Windows authentication.</span></span> |<span data-ttu-id="95f68-172">Ne</span><span class="sxs-lookup"><span data-stu-id="95f68-172">No</span></span> |
| <span data-ttu-id="95f68-173">**heslo**</span><span class="sxs-lookup"><span data-stu-id="95f68-173">**password**</span></span> |<span data-ttu-id="95f68-174">Hello heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="95f68-174">hello password for hello user account.</span></span> |<span data-ttu-id="95f68-175">Ne</span><span class="sxs-lookup"><span data-stu-id="95f68-175">No</span></span> |
| <span data-ttu-id="95f68-176">**gatewayName**</span><span class="sxs-lookup"><span data-stu-id="95f68-176">**gatewayName**</span></span> |<span data-ttu-id="95f68-177">Název Hello hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi DB2.</span><span class="sxs-lookup"><span data-stu-id="95f68-177">hello name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises DB2 database.</span></span> |<span data-ttu-id="95f68-178">Ano</span><span class="sxs-lookup"><span data-stu-id="95f68-178">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="95f68-179">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="95f68-179">Dataset properties</span></span>
<span data-ttu-id="95f68-180">Seznam hello části a vlastnosti, které jsou k dispozici pro definování datové sady, naleznete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="95f68-180">For a list of hello sections and properties that are available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="95f68-181">Částech, jako například **struktura**, **dostupnosti**a hello **zásad** pro datovou sadu JSON, jsou podobné pro všechny typy datovou sadu (SQL Azure, Azure Blob storage, Azure Table úložiště a tak dále).</span><span class="sxs-lookup"><span data-stu-id="95f68-181">Sections, such as **structure**, **availability**, and hello **policy** for a dataset JSON, are similar for all dataset types (Azure SQL, Azure Blob storage, Azure Table storage, and so on).</span></span>

<span data-ttu-id="95f68-182">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="95f68-182">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="95f68-183">Hello **rámci typeProperties** části datové sady typu **RelationalTable**, která obsahuje datovou sadu hello DB2, má hello následující vlastnost:</span><span class="sxs-lookup"><span data-stu-id="95f68-183">hello **typeProperties** section for a dataset of type **RelationalTable**, which includes hello DB2 dataset, has hello following property:</span></span>

| <span data-ttu-id="95f68-184">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="95f68-184">Property</span></span> | <span data-ttu-id="95f68-185">Popis</span><span class="sxs-lookup"><span data-stu-id="95f68-185">Description</span></span> | <span data-ttu-id="95f68-186">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="95f68-186">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="95f68-187">**Název tabulky**</span><span class="sxs-lookup"><span data-stu-id="95f68-187">**tableName**</span></span> |<span data-ttu-id="95f68-188">Hello název tabulky hello hello DB2 instance databáze, kterou hello propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="95f68-188">hello name of hello table in hello DB2 database instance that hello linked service refers to.</span></span> <span data-ttu-id="95f68-189">Tato vlastnost je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="95f68-189">This property is case-sensitive.</span></span> |<span data-ttu-id="95f68-190">Ne (Pokud hello **dotazu** vlastnost aktivity kopírování typu **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="95f68-190">No (if hello **query** property of a copy activity of type **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="95f68-191">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="95f68-191">Copy Activity properties</span></span>
<span data-ttu-id="95f68-192">Seznam oddílů hello a vlastnosti, které jsou k dispozici pro definování aktivity kopírování najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="95f68-192">For a list of hello sections and properties that are available for defining copy activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="95f68-193">Kopírovat vlastnosti aktivity, jako například **název**, **popis**, **vstupy** tabulky, **výstupy** tabulky, a **zásad**, jsou k dispozici pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="95f68-193">Copy Activity properties, such as **name**, **description**, **inputs** table, **outputs** table, and **policy**, are available for all types of activities.</span></span> <span data-ttu-id="95f68-194">Hello vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivity pro každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="95f68-194">hello properties that are available in hello **typeProperties** section of hello activity for each activity type.</span></span> <span data-ttu-id="95f68-195">Pro aktivitu kopírování vlastnosti hello lišit v závislosti na typech hello zdroje dat a jímky.</span><span class="sxs-lookup"><span data-stu-id="95f68-195">For Copy Activity, hello properties vary depending on hello types of data sources and sinks.</span></span>

<span data-ttu-id="95f68-196">Pro aktivitu kopírování, pokud je zdroj hello typu **RelationalSource** (která zahrnuje DB2), hello následující vlastnosti jsou k dispozici v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="95f68-196">For Copy Activity, when hello source is of type **RelationalSource** (which includes DB2), hello following properties are available in hello **typeProperties** section:</span></span>

| <span data-ttu-id="95f68-197">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="95f68-197">Property</span></span> | <span data-ttu-id="95f68-198">Popis</span><span class="sxs-lookup"><span data-stu-id="95f68-198">Description</span></span> | <span data-ttu-id="95f68-199">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="95f68-199">Allowed values</span></span> | <span data-ttu-id="95f68-200">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="95f68-200">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="95f68-201">**dotaz**</span><span class="sxs-lookup"><span data-stu-id="95f68-201">**query**</span></span> |<span data-ttu-id="95f68-202">Hello vlastního dotazu tooread hello data použijte.</span><span class="sxs-lookup"><span data-stu-id="95f68-202">Use hello custom query tooread hello data.</span></span> |<span data-ttu-id="95f68-203">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="95f68-203">SQL query string.</span></span> <span data-ttu-id="95f68-204">Příklad: `"query": "select * from "MySchema"."MyTable""`</span><span class="sxs-lookup"><span data-stu-id="95f68-204">For example: `"query": "select * from "MySchema"."MyTable""`</span></span> |<span data-ttu-id="95f68-205">Ne (Pokud hello **tableName** je zadána vlastnost datové sady)</span><span class="sxs-lookup"><span data-stu-id="95f68-205">No (if hello **tableName** property of a dataset is specified)</span></span> |

> [!NOTE]
> <span data-ttu-id="95f68-206">Schéma a tabulku názvy rozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="95f68-206">Schema and table names are case-sensitive.</span></span> <span data-ttu-id="95f68-207">V příkazu dotazu hello, uzavřete názvy vlastností pomocí "" (dvojité uvozovky).</span><span class="sxs-lookup"><span data-stu-id="95f68-207">In hello query statement, enclose property names by using "" (double quotes).</span></span> <span data-ttu-id="95f68-208">Například:</span><span class="sxs-lookup"><span data-stu-id="95f68-208">For example:</span></span>
>
> ```sql
> "query": "select * from "DB2ADMIN"."Customers""
> ```

## <a name="json-example-copy-data-from-db2-tooazure-blob-storage"></a><span data-ttu-id="95f68-209">Příklad JSON: kopírování dat z DB2 tooAzure úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="95f68-209">JSON example: Copy data from DB2 tooAzure Blob storage</span></span>
<span data-ttu-id="95f68-210">Tento příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí hello [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="95f68-210">This example provides sample JSON definitions that you can use toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="95f68-211">Hello příklad ukazuje, jak toocopy data z DB2 databáze tooBlob úložiště.</span><span class="sxs-lookup"><span data-stu-id="95f68-211">hello example shows you how toocopy data from a DB2 database tooBlob storage.</span></span> <span data-ttu-id="95f68-212">Ale data se dají zkopírovat příliš[všechna podporovaná data ukládat typ jímky](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="95f68-212">However, data can be copied too[any supported data store sink type](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using Azure Data Factory Copy Activity.</span></span>

<span data-ttu-id="95f68-213">Ukázka Hello má hello následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="95f68-213">hello sample has hello following Data Factory entities:</span></span>

- <span data-ttu-id="95f68-214">DB2 propojené služby typu [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="95f68-214">A DB2 linked service of type [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)</span></span>
- <span data-ttu-id="95f68-215">Azure Blob storage, propojené služby typu [azurestorage.](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="95f68-215">An Azure Blob storage linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
- <span data-ttu-id="95f68-216">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="95f68-216">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)</span></span>
- <span data-ttu-id="95f68-217">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="95f68-217">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
- <span data-ttu-id="95f68-218">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá hello [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) vlastnosti</span><span class="sxs-lookup"><span data-stu-id="95f68-218">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses hello [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) properties</span></span>

<span data-ttu-id="95f68-219">Ukázka Hello zkopíruje data z výsledku dotazu v tooan databáze DB2 objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="95f68-219">hello sample copies data from a query result in a DB2 database tooan Azure blob hourly.</span></span> <span data-ttu-id="95f68-220">Vlastnosti Hello JSON, které se používají v ukázce hello jsou popsané v hello oddíly, které následují definice entity hello.</span><span class="sxs-lookup"><span data-stu-id="95f68-220">hello JSON properties that are used in hello sample are described in hello sections that follow hello entity definitions.</span></span>

<span data-ttu-id="95f68-221">Jako první krok nainstalujte a nakonfigurujte bránu data gateway.</span><span class="sxs-lookup"><span data-stu-id="95f68-221">As a first step, install and configure a data gateway.</span></span> <span data-ttu-id="95f68-222">Pokyny naleznete v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="95f68-222">Instructions are in hello [Moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="95f68-223">**DB2 propojené služby**</span><span class="sxs-lookup"><span data-stu-id="95f68-223">**DB2 linked service**</span></span>

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

<span data-ttu-id="95f68-224">**Objekt Blob propojená služba Azure storage**</span><span class="sxs-lookup"><span data-stu-id="95f68-224">**Azure Blob storage linked service**</span></span>

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

<span data-ttu-id="95f68-225">**Vstupní datové sady DB2**</span><span class="sxs-lookup"><span data-stu-id="95f68-225">**DB2 input dataset**</span></span>

<span data-ttu-id="95f68-226">Hello příkladu se předpokládá, že jste vytvořili tabulku v s názvem "MyTable", která má sloupec s názvem "časové razítko" hello časových řad dat DB2.</span><span class="sxs-lookup"><span data-stu-id="95f68-226">hello sample assumes that you have created a table in DB2 named "MyTable" that has a column labeled "timestamp" for hello time series data.</span></span>

<span data-ttu-id="95f68-227">Hello **externí** vlastnost je nastavena příliš "PRAVDA."</span><span class="sxs-lookup"><span data-stu-id="95f68-227">hello **external** property is set too"true."</span></span> <span data-ttu-id="95f68-228">Toto nastavení informuje služba Data Factory hello, že tato datová sada je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="95f68-228">This setting informs hello Data Factory service that this dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span> <span data-ttu-id="95f68-229">Všimněte si, že hello **typ** vlastnost je nastavena příliš**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="95f68-229">Notice that hello **type** property is set too**RelationalTable**.</span></span>


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

<span data-ttu-id="95f68-230">**Výstupní datovou sadu objektů Blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="95f68-230">**Azure Blob output dataset**</span></span>

<span data-ttu-id="95f68-231">Data jsou zapsána nový objekt blob tooa každou hodinu nastavení hello **frekvence** vlastnost příliš "Hodina" a hello **interval** too1 vlastnost.</span><span class="sxs-lookup"><span data-stu-id="95f68-231">Data is written tooa new blob every hour by setting hello **frequency** property too"Hour" and hello **interval** property too1.</span></span> <span data-ttu-id="95f68-232">Hello **folderPath** vlastností pro objekt blob hello dynamicky vyhodnotí podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="95f68-232">hello **folderPath** property for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="95f68-233">Cesta ke složce Hello používá hello rok, měsíc, den a hodina části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="95f68-233">hello folder path uses hello year, month, day, and hour parts of hello start time.</span></span>

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

<span data-ttu-id="95f68-234">**Kanál pro aktivitu kopírování hello**</span><span class="sxs-lookup"><span data-stu-id="95f68-234">**Pipeline for hello copy activity**</span></span>

<span data-ttu-id="95f68-235">Hello kanál obsahuje aktivitu kopírování, který je nakonfigurovaný toouse zadaný vstupní a výstupní datové sady a který je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="95f68-235">hello pipeline contains a copy activity that is configured toouse specified input and output datasets and which is scheduled toorun every hour.</span></span> <span data-ttu-id="95f68-236">V definici JSON pro kanál hello hello, hello **zdroj** je typ nastaven příliš**RelationalSource** a hello **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="95f68-236">In hello JSON definition for hello pipeline, hello **source** type is set too**RelationalSource** and hello **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="95f68-237">Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data z tabulky "Objednávky" hello.</span><span class="sxs-lookup"><span data-stu-id="95f68-237">hello SQL query specified for hello **query** property selects hello data from hello "Orders" table.</span></span>

```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for hello copy activity",
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

## <a name="type-mapping-for-db2"></a><span data-ttu-id="95f68-238">Mapování typu pro DB2</span><span class="sxs-lookup"><span data-stu-id="95f68-238">Type mapping for DB2</span></span>
<span data-ttu-id="95f68-239">Jak je uvedeno v hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí převody automatické typ z typu toosink typ zdroje pomocí hello následující dvoustupňový přístup:</span><span class="sxs-lookup"><span data-stu-id="95f68-239">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy Activity performs automatic type conversions from source type toosink type by using hello following two-step approach:</span></span>

1. <span data-ttu-id="95f68-240">Převést z typu nativní zdroj tooa typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="95f68-240">Convert from a native source type tooa .NET type</span></span>
2. <span data-ttu-id="95f68-241">Převést z typu nativní podřízený tooa typ rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="95f68-241">Convert from a .NET type tooa native sink type</span></span>

<span data-ttu-id="95f68-242">Hello následující mapování se používají při aktivitě kopírování převádí hello data z typu DB2 tooa typ rozhraní .NET:</span><span class="sxs-lookup"><span data-stu-id="95f68-242">hello following mappings are used when Copy Activity converts hello data from a DB2 type tooa .NET type:</span></span>

| <span data-ttu-id="95f68-243">Typ databáze DB2</span><span class="sxs-lookup"><span data-stu-id="95f68-243">DB2 database type</span></span> | <span data-ttu-id="95f68-244">Typ rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="95f68-244">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="95f68-245">SmallInt</span><span class="sxs-lookup"><span data-stu-id="95f68-245">SmallInt</span></span> |<span data-ttu-id="95f68-246">Int16</span><span class="sxs-lookup"><span data-stu-id="95f68-246">Int16</span></span> |
| <span data-ttu-id="95f68-247">Integer</span><span class="sxs-lookup"><span data-stu-id="95f68-247">Integer</span></span> |<span data-ttu-id="95f68-248">Int32</span><span class="sxs-lookup"><span data-stu-id="95f68-248">Int32</span></span> |
| <span data-ttu-id="95f68-249">BigInt</span><span class="sxs-lookup"><span data-stu-id="95f68-249">BigInt</span></span> |<span data-ttu-id="95f68-250">Int64</span><span class="sxs-lookup"><span data-stu-id="95f68-250">Int64</span></span> |
| <span data-ttu-id="95f68-251">Real</span><span class="sxs-lookup"><span data-stu-id="95f68-251">Real</span></span> |<span data-ttu-id="95f68-252">Jeden</span><span class="sxs-lookup"><span data-stu-id="95f68-252">Single</span></span> |
| <span data-ttu-id="95f68-253">Double</span><span class="sxs-lookup"><span data-stu-id="95f68-253">Double</span></span> |<span data-ttu-id="95f68-254">Double</span><span class="sxs-lookup"><span data-stu-id="95f68-254">Double</span></span> |
| <span data-ttu-id="95f68-255">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="95f68-255">Float</span></span> |<span data-ttu-id="95f68-256">Double</span><span class="sxs-lookup"><span data-stu-id="95f68-256">Double</span></span> |
| <span data-ttu-id="95f68-257">Decimal</span><span class="sxs-lookup"><span data-stu-id="95f68-257">Decimal</span></span> |<span data-ttu-id="95f68-258">Decimal</span><span class="sxs-lookup"><span data-stu-id="95f68-258">Decimal</span></span> |
| <span data-ttu-id="95f68-259">DecimalFloat</span><span class="sxs-lookup"><span data-stu-id="95f68-259">DecimalFloat</span></span> |<span data-ttu-id="95f68-260">Decimal</span><span class="sxs-lookup"><span data-stu-id="95f68-260">Decimal</span></span> |
| <span data-ttu-id="95f68-261">číselné</span><span class="sxs-lookup"><span data-stu-id="95f68-261">Numeric</span></span> |<span data-ttu-id="95f68-262">Decimal</span><span class="sxs-lookup"><span data-stu-id="95f68-262">Decimal</span></span> |
| <span data-ttu-id="95f68-263">Datum</span><span class="sxs-lookup"><span data-stu-id="95f68-263">Date</span></span> |<span data-ttu-id="95f68-264">Data a času</span><span class="sxs-lookup"><span data-stu-id="95f68-264">DateTime</span></span> |
| <span data-ttu-id="95f68-265">Čas</span><span class="sxs-lookup"><span data-stu-id="95f68-265">Time</span></span> |<span data-ttu-id="95f68-266">Časový interval</span><span class="sxs-lookup"><span data-stu-id="95f68-266">TimeSpan</span></span> |
| <span data-ttu-id="95f68-267">časové razítko</span><span class="sxs-lookup"><span data-stu-id="95f68-267">Timestamp</span></span> |<span data-ttu-id="95f68-268">Data a času</span><span class="sxs-lookup"><span data-stu-id="95f68-268">DateTime</span></span> |
| <span data-ttu-id="95f68-269">XML</span><span class="sxs-lookup"><span data-stu-id="95f68-269">Xml</span></span> |<span data-ttu-id="95f68-270">Byte]</span><span class="sxs-lookup"><span data-stu-id="95f68-270">Byte[]</span></span> |
| <span data-ttu-id="95f68-271">Char</span><span class="sxs-lookup"><span data-stu-id="95f68-271">Char</span></span> |<span data-ttu-id="95f68-272">Řetězec</span><span class="sxs-lookup"><span data-stu-id="95f68-272">String</span></span> |
| <span data-ttu-id="95f68-273">VarChar</span><span class="sxs-lookup"><span data-stu-id="95f68-273">VarChar</span></span> |<span data-ttu-id="95f68-274">Řetězec</span><span class="sxs-lookup"><span data-stu-id="95f68-274">String</span></span> |
| <span data-ttu-id="95f68-275">LongVarChar</span><span class="sxs-lookup"><span data-stu-id="95f68-275">LongVarChar</span></span> |<span data-ttu-id="95f68-276">Řetězec</span><span class="sxs-lookup"><span data-stu-id="95f68-276">String</span></span> |
| <span data-ttu-id="95f68-277">DB2DynArray</span><span class="sxs-lookup"><span data-stu-id="95f68-277">DB2DynArray</span></span> |<span data-ttu-id="95f68-278">Řetězec</span><span class="sxs-lookup"><span data-stu-id="95f68-278">String</span></span> |
| <span data-ttu-id="95f68-279">Binární</span><span class="sxs-lookup"><span data-stu-id="95f68-279">Binary</span></span> |<span data-ttu-id="95f68-280">Byte]</span><span class="sxs-lookup"><span data-stu-id="95f68-280">Byte[]</span></span> |
| <span data-ttu-id="95f68-281">VarBinary</span><span class="sxs-lookup"><span data-stu-id="95f68-281">VarBinary</span></span> |<span data-ttu-id="95f68-282">Byte]</span><span class="sxs-lookup"><span data-stu-id="95f68-282">Byte[]</span></span> |
| <span data-ttu-id="95f68-283">LongVarBinary</span><span class="sxs-lookup"><span data-stu-id="95f68-283">LongVarBinary</span></span> |<span data-ttu-id="95f68-284">Byte]</span><span class="sxs-lookup"><span data-stu-id="95f68-284">Byte[]</span></span> |
| <span data-ttu-id="95f68-285">Obrázek</span><span class="sxs-lookup"><span data-stu-id="95f68-285">Graphic</span></span> |<span data-ttu-id="95f68-286">Řetězec</span><span class="sxs-lookup"><span data-stu-id="95f68-286">String</span></span> |
| <span data-ttu-id="95f68-287">VarGraphic</span><span class="sxs-lookup"><span data-stu-id="95f68-287">VarGraphic</span></span> |<span data-ttu-id="95f68-288">Řetězec</span><span class="sxs-lookup"><span data-stu-id="95f68-288">String</span></span> |
| <span data-ttu-id="95f68-289">LongVarGraphic</span><span class="sxs-lookup"><span data-stu-id="95f68-289">LongVarGraphic</span></span> |<span data-ttu-id="95f68-290">Řetězec</span><span class="sxs-lookup"><span data-stu-id="95f68-290">String</span></span> |
| <span data-ttu-id="95f68-291">Datový typ CLOB</span><span class="sxs-lookup"><span data-stu-id="95f68-291">Clob</span></span> |<span data-ttu-id="95f68-292">Řetězec</span><span class="sxs-lookup"><span data-stu-id="95f68-292">String</span></span> |
| <span data-ttu-id="95f68-293">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="95f68-293">Blob</span></span> |<span data-ttu-id="95f68-294">Byte]</span><span class="sxs-lookup"><span data-stu-id="95f68-294">Byte[]</span></span> |
| <span data-ttu-id="95f68-295">DbClob</span><span class="sxs-lookup"><span data-stu-id="95f68-295">DbClob</span></span> |<span data-ttu-id="95f68-296">Řetězec</span><span class="sxs-lookup"><span data-stu-id="95f68-296">String</span></span> |
| <span data-ttu-id="95f68-297">SmallInt</span><span class="sxs-lookup"><span data-stu-id="95f68-297">SmallInt</span></span> |<span data-ttu-id="95f68-298">Int16</span><span class="sxs-lookup"><span data-stu-id="95f68-298">Int16</span></span> |
| <span data-ttu-id="95f68-299">Integer</span><span class="sxs-lookup"><span data-stu-id="95f68-299">Integer</span></span> |<span data-ttu-id="95f68-300">Int32</span><span class="sxs-lookup"><span data-stu-id="95f68-300">Int32</span></span> |
| <span data-ttu-id="95f68-301">BigInt</span><span class="sxs-lookup"><span data-stu-id="95f68-301">BigInt</span></span> |<span data-ttu-id="95f68-302">Int64</span><span class="sxs-lookup"><span data-stu-id="95f68-302">Int64</span></span> |
| <span data-ttu-id="95f68-303">Real</span><span class="sxs-lookup"><span data-stu-id="95f68-303">Real</span></span> |<span data-ttu-id="95f68-304">Jeden</span><span class="sxs-lookup"><span data-stu-id="95f68-304">Single</span></span> |
| <span data-ttu-id="95f68-305">Double</span><span class="sxs-lookup"><span data-stu-id="95f68-305">Double</span></span> |<span data-ttu-id="95f68-306">Double</span><span class="sxs-lookup"><span data-stu-id="95f68-306">Double</span></span> |
| <span data-ttu-id="95f68-307">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="95f68-307">Float</span></span> |<span data-ttu-id="95f68-308">Double</span><span class="sxs-lookup"><span data-stu-id="95f68-308">Double</span></span> |
| <span data-ttu-id="95f68-309">Decimal</span><span class="sxs-lookup"><span data-stu-id="95f68-309">Decimal</span></span> |<span data-ttu-id="95f68-310">Decimal</span><span class="sxs-lookup"><span data-stu-id="95f68-310">Decimal</span></span> |
| <span data-ttu-id="95f68-311">DecimalFloat</span><span class="sxs-lookup"><span data-stu-id="95f68-311">DecimalFloat</span></span> |<span data-ttu-id="95f68-312">Decimal</span><span class="sxs-lookup"><span data-stu-id="95f68-312">Decimal</span></span> |
| <span data-ttu-id="95f68-313">číselné</span><span class="sxs-lookup"><span data-stu-id="95f68-313">Numeric</span></span> |<span data-ttu-id="95f68-314">Decimal</span><span class="sxs-lookup"><span data-stu-id="95f68-314">Decimal</span></span> |
| <span data-ttu-id="95f68-315">Datum</span><span class="sxs-lookup"><span data-stu-id="95f68-315">Date</span></span> |<span data-ttu-id="95f68-316">Data a času</span><span class="sxs-lookup"><span data-stu-id="95f68-316">DateTime</span></span> |
| <span data-ttu-id="95f68-317">Čas</span><span class="sxs-lookup"><span data-stu-id="95f68-317">Time</span></span> |<span data-ttu-id="95f68-318">Časový interval</span><span class="sxs-lookup"><span data-stu-id="95f68-318">TimeSpan</span></span> |
| <span data-ttu-id="95f68-319">časové razítko</span><span class="sxs-lookup"><span data-stu-id="95f68-319">Timestamp</span></span> |<span data-ttu-id="95f68-320">Data a času</span><span class="sxs-lookup"><span data-stu-id="95f68-320">DateTime</span></span> |
| <span data-ttu-id="95f68-321">XML</span><span class="sxs-lookup"><span data-stu-id="95f68-321">Xml</span></span> |<span data-ttu-id="95f68-322">Byte]</span><span class="sxs-lookup"><span data-stu-id="95f68-322">Byte[]</span></span> |
| <span data-ttu-id="95f68-323">Char</span><span class="sxs-lookup"><span data-stu-id="95f68-323">Char</span></span> |<span data-ttu-id="95f68-324">Řetězec</span><span class="sxs-lookup"><span data-stu-id="95f68-324">String</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="95f68-325">Mapování zdrojových toosink sloupců</span><span class="sxs-lookup"><span data-stu-id="95f68-325">Map source toosink columns</span></span>
<span data-ttu-id="95f68-326">toolearn jak zjistit, toomap sloupců v toocolumns datové sady zdroje hello v datové sadě podřízený hello, [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="95f68-326">toolearn how toomap columns in hello source dataset toocolumns in hello sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-reads-from-relational-sources"></a><span data-ttu-id="95f68-327">Opakovatelných čtení z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="95f68-327">Repeatable reads from relational sources</span></span>
<span data-ttu-id="95f68-328">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="95f68-328">When you copy data from a relational data store, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="95f68-329">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="95f68-329">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="95f68-330">Můžete také nakonfigurovat opakování hello **zásad** vlastnost pro datovou sadu toorerun řez, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="95f68-330">You can also configure hello retry **policy** property for a dataset toorerun a slice when a failure occurs.</span></span> <span data-ttu-id="95f68-331">Ujistěte se, který hello stejných dat je pro čtení bez ohledu na to jak řez hello tolikrát, kolikrát je spustit znovu a bez ohledu na to, jak znovu hello řez.</span><span class="sxs-lookup"><span data-stu-id="95f68-331">Make sure that hello same data is read no matter how many times hello slice is rerun, and regardless of how you rerun hello slice.</span></span> <span data-ttu-id="95f68-332">Další informace najdete v tématu [Repeatable čte z relačními zdroji](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="95f68-332">For more information, see [Repeatable reads from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="95f68-333">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="95f68-333">Performance and tuning</span></span>
<span data-ttu-id="95f68-334">Další informace o klíčových faktorů, které ovlivňují výkon hello aktivity kopírování a způsoby toooptimize výkonu v hello [výkonu kopie aktivity a ladění průvodce](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="95f68-334">Learn about key factors that affect hello performance of Copy Activity and ways toooptimize performance in hello [Copy Activity Performance and Tuning Guide](data-factory-copy-activity-performance.md).</span></span>
