---
title: "Přesun dat z SAP HANA pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z SAP HANA pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 2ab488d82d24999a6231e40cb719715463c51d64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-sap-hana-using-azure-data-factory"></a><span data-ttu-id="51ecb-103">Přesun dat z SAP HANA pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="51ecb-103">Move data From SAP HANA using Azure Data Factory</span></span>
<span data-ttu-id="51ecb-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z místní SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="51ecb-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises SAP HANA.</span></span> <span data-ttu-id="51ecb-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="51ecb-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="51ecb-106">Z úložiště dat SAP HANA místní může kopírovat data do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="51ecb-106">You can copy data from an on-premises SAP HANA data store to any supported sink data store.</span></span> <span data-ttu-id="51ecb-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="51ecb-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="51ecb-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z SAP HANA k jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat na SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="51ecb-108">Data factory currently supports only moving data from an SAP HANA to other data stores, but not for moving data from other data stores to an SAP HANA.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="51ecb-109">Podporované verze a instalaci</span><span class="sxs-lookup"><span data-stu-id="51ecb-109">Supported versions and installation</span></span>
<span data-ttu-id="51ecb-110">Tento konektor podporuje všechny verze databáze SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="51ecb-110">This connector supports any version of SAP HANA database.</span></span> <span data-ttu-id="51ecb-111">Podporuje kopírování dat z modelů HANA informace (například analýzy a výpočet zobrazení) a řádek nebo sloupce tabulky pomocí dotazů SQL.</span><span class="sxs-lookup"><span data-stu-id="51ecb-111">It supports copying data from HANA information models (such as Analytic and Calculation views) and Row/Column tables using SQL queries.</span></span>

<span data-ttu-id="51ecb-112">Pokud chcete povolit připojení k instanci SAP HANA, nainstalujte následující součásti:</span><span class="sxs-lookup"><span data-stu-id="51ecb-112">To enable the connectivity to the SAP HANA instance, install the following components:</span></span>
- <span data-ttu-id="51ecb-113">**Brána pro správu dat**: podporuje služby Data Factory připojení k datům místní úložiště (včetně SAP HANA) pomocí součásti názvem Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="51ecb-113">**Data Management Gateway**: Data Factory service supports connecting to on-premises data stores (including SAP HANA) using a component called Data Management Gateway.</span></span> <span data-ttu-id="51ecb-114">Další informace o Brána pro správu dat a podrobné pokyny pro nastavení brány najdete v tématu [přesouvání dat mezi místní data uložit do cloudu úložiště dat](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="51ecb-114">To learn about Data Management Gateway and step-by-step instructions for setting up the gateway, see [Moving data between on-premises data store to cloud data store](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="51ecb-115">Vyžaduje se brána, i když SAP HANA je hostovaná ve virtuálním počítači Azure IaaS (VM).</span><span class="sxs-lookup"><span data-stu-id="51ecb-115">Gateway is required even if the SAP HANA is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="51ecb-116">Bránu můžete nainstalovat na stejný virtuální počítač jako úložiště dat nebo na jiný virtuální počítač, dokud brána se může připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="51ecb-116">You can install the gateway on the same VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>
- <span data-ttu-id="51ecb-117">**Ovladač SAP HANA ODBC** na počítači s bránou.</span><span class="sxs-lookup"><span data-stu-id="51ecb-117">**SAP HANA ODBC driver** on the gateway machine.</span></span> <span data-ttu-id="51ecb-118">Ovladač SAP HANA ODBC z si můžete stáhnout [SAP služby Stažení softwaru](https://support.sap.com/swdc).</span><span class="sxs-lookup"><span data-stu-id="51ecb-118">You can download the SAP HANA ODBC driver from the [SAP Software Download Center](https://support.sap.com/swdc).</span></span> <span data-ttu-id="51ecb-119">Vyhledávání pomocí klíčového slova **SAP HANA klienta pro systém Windows**.</span><span class="sxs-lookup"><span data-stu-id="51ecb-119">Search with the keyword **SAP HANA CLIENT for Windows**.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="51ecb-120">Začínáme</span><span class="sxs-lookup"><span data-stu-id="51ecb-120">Getting started</span></span>
<span data-ttu-id="51ecb-121">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z úložiště místní Cassandra data pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="51ecb-121">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="51ecb-122">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="51ecb-122">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="51ecb-123">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="51ecb-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="51ecb-124">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="51ecb-124">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="51ecb-125">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="51ecb-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="51ecb-126">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="51ecb-126">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="51ecb-127">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="51ecb-127">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="51ecb-128">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="51ecb-128">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="51ecb-129">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="51ecb-129">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="51ecb-130">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="51ecb-130">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="51ecb-131">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="51ecb-131">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="51ecb-132">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z SAP HANA místní, naleznete v tématu [JSON příklad: kopírování dat z SAP HANA do objektu Blob Azure](#json-example-copy-data-from-sap-hana-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="51ecb-132">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises SAP HANA, see [JSON example: Copy data from SAP HANA to Azure Blob](#json-example-copy-data-from-sap-hana-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="51ecb-133">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení konkrétní entity služby Data Factory k úložišti dat SAP HANA:</span><span class="sxs-lookup"><span data-stu-id="51ecb-133">The following sections provide details about JSON properties that are used to define Data Factory entities specific to an SAP HANA data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="51ecb-134">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="51ecb-134">Linked service properties</span></span>
<span data-ttu-id="51ecb-135">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro SAP HANA propojené služby.</span><span class="sxs-lookup"><span data-stu-id="51ecb-135">The following table provides description for JSON elements specific to SAP HANA linked service.</span></span>

<span data-ttu-id="51ecb-136">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="51ecb-136">Property</span></span> | <span data-ttu-id="51ecb-137">Popis</span><span class="sxs-lookup"><span data-stu-id="51ecb-137">Description</span></span> | <span data-ttu-id="51ecb-138">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="51ecb-138">Allowed values</span></span> | <span data-ttu-id="51ecb-139">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="51ecb-139">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="51ecb-140">server</span><span class="sxs-lookup"><span data-stu-id="51ecb-140">server</span></span> | <span data-ttu-id="51ecb-141">Název serveru, na kterém se nachází instance SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="51ecb-141">Name of the server on which the SAP HANA instance resides.</span></span> <span data-ttu-id="51ecb-142">Pokud váš server používá vlastní port, zadejte `server:port`.</span><span class="sxs-lookup"><span data-stu-id="51ecb-142">If your server is using a customized port, specify `server:port`.</span></span> | <span data-ttu-id="51ecb-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="51ecb-143">string</span></span> | <span data-ttu-id="51ecb-144">Ano</span><span class="sxs-lookup"><span data-stu-id="51ecb-144">Yes</span></span>
<span data-ttu-id="51ecb-145">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="51ecb-145">authenticationType</span></span> | <span data-ttu-id="51ecb-146">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="51ecb-146">Type of authentication.</span></span> | <span data-ttu-id="51ecb-147">Řetězec.</span><span class="sxs-lookup"><span data-stu-id="51ecb-147">string.</span></span> <span data-ttu-id="51ecb-148">"Základní" nebo "Systém Windows"</span><span class="sxs-lookup"><span data-stu-id="51ecb-148">"Basic" or "Windows"</span></span> | <span data-ttu-id="51ecb-149">Ano</span><span class="sxs-lookup"><span data-stu-id="51ecb-149">Yes</span></span> 
<span data-ttu-id="51ecb-150">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="51ecb-150">username</span></span> | <span data-ttu-id="51ecb-151">Jméno uživatele, který má přístup k serveru SAP</span><span class="sxs-lookup"><span data-stu-id="51ecb-151">Name of the user who has access to the SAP server</span></span> | <span data-ttu-id="51ecb-152">Řetězec</span><span class="sxs-lookup"><span data-stu-id="51ecb-152">string</span></span> | <span data-ttu-id="51ecb-153">Ano</span><span class="sxs-lookup"><span data-stu-id="51ecb-153">Yes</span></span>
<span data-ttu-id="51ecb-154">heslo</span><span class="sxs-lookup"><span data-stu-id="51ecb-154">password</span></span> | <span data-ttu-id="51ecb-155">Heslo pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="51ecb-155">Password for the user.</span></span> | <span data-ttu-id="51ecb-156">Řetězec</span><span class="sxs-lookup"><span data-stu-id="51ecb-156">string</span></span> | <span data-ttu-id="51ecb-157">Ano</span><span class="sxs-lookup"><span data-stu-id="51ecb-157">Yes</span></span>
<span data-ttu-id="51ecb-158">gatewayName</span><span class="sxs-lookup"><span data-stu-id="51ecb-158">gatewayName</span></span> | <span data-ttu-id="51ecb-159">Název brány, kterou služba Data Factory měla použít pro připojení k místní instanci SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="51ecb-159">Name of the gateway that the Data Factory service should use to connect to the on-premises SAP HANA instance.</span></span> | <span data-ttu-id="51ecb-160">Řetězec</span><span class="sxs-lookup"><span data-stu-id="51ecb-160">string</span></span> | <span data-ttu-id="51ecb-161">Ano</span><span class="sxs-lookup"><span data-stu-id="51ecb-161">Yes</span></span>
<span data-ttu-id="51ecb-162">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="51ecb-162">encryptedCredential</span></span> | <span data-ttu-id="51ecb-163">Řetězec šifrovaný přihlašovací údaj.</span><span class="sxs-lookup"><span data-stu-id="51ecb-163">The encrypted credential string.</span></span> | <span data-ttu-id="51ecb-164">Řetězec</span><span class="sxs-lookup"><span data-stu-id="51ecb-164">string</span></span> | <span data-ttu-id="51ecb-165">Ne</span><span class="sxs-lookup"><span data-stu-id="51ecb-165">No</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="51ecb-166">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="51ecb-166">Dataset properties</span></span>
<span data-ttu-id="51ecb-167">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="51ecb-167">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="51ecb-168">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="51ecb-168">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="51ecb-169">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="51ecb-169">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="51ecb-170">Nejsou k dispozici žádné vlastnosti specifické pro typ podporované pro datovou sadu SAP HANA typu **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="51ecb-170">There are no type-specific properties supported for the SAP HANA dataset of type **RelationalTable**.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="51ecb-171">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="51ecb-171">Copy activity properties</span></span>
<span data-ttu-id="51ecb-172">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="51ecb-172">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="51ecb-173">Vlastnosti, například název, popis, vstupní a výstupní tabulky, jsou zásady jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="51ecb-173">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="51ecb-174">Vzhledem k tomu, vlastnosti dostupné ve **rámci typeProperties** části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="51ecb-174">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="51ecb-175">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="51ecb-175">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="51ecb-176">Pokud je zdroj v aktivitě kopírování typu **RelationalSource** (která zahrnuje SAP HANA), následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="51ecb-176">When source in copy activity is of type **RelationalSource** (which includes SAP HANA), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="51ecb-177">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="51ecb-177">Property</span></span> | <span data-ttu-id="51ecb-178">Popis</span><span class="sxs-lookup"><span data-stu-id="51ecb-178">Description</span></span> | <span data-ttu-id="51ecb-179">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="51ecb-179">Allowed values</span></span> | <span data-ttu-id="51ecb-180">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="51ecb-180">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="51ecb-181">query</span><span class="sxs-lookup"><span data-stu-id="51ecb-181">query</span></span> | <span data-ttu-id="51ecb-182">Určuje příkaz jazyka SQL pro čtení dat z instance SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="51ecb-182">Specifies the SQL query to read data from the SAP HANA instance.</span></span> | <span data-ttu-id="51ecb-183">Dotaz SQL.</span><span class="sxs-lookup"><span data-stu-id="51ecb-183">SQL query.</span></span> | <span data-ttu-id="51ecb-184">Ano</span><span class="sxs-lookup"><span data-stu-id="51ecb-184">Yes</span></span> |

## <a name="json-example-copy-data-from-sap-hana-to-azure-blob"></a><span data-ttu-id="51ecb-185">Příklad JSON: kopírování dat z SAP HANA do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="51ecb-185">JSON example: Copy data from SAP HANA to Azure Blob</span></span>
<span data-ttu-id="51ecb-186">Následující příklad obsahuje ukázkové JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="51ecb-186">The following sample provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="51ecb-187">Tento příklad ukazuje postup kopírování dat z SAP HANA místně do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="51ecb-187">This sample shows how to copy data from an on-premises SAP HANA to an Azure Blob Storage.</span></span> <span data-ttu-id="51ecb-188">Však můžete zkopírovat data **přímo** žádnému jímky uvedené [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="51ecb-188">However, data can be copied **directly** to any of the sinks listed [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="51ecb-189">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="51ecb-189">This sample provides JSON snippets.</span></span> <span data-ttu-id="51ecb-190">Podrobné pokyny pro vytvoření objektu pro vytváření dat neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="51ecb-190">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="51ecb-191">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="51ecb-191">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="51ecb-192">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="51ecb-192">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="51ecb-193">Propojené služby typu [SapHana](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="51ecb-193">A linked service of type [SapHana](#linked-service-properties).</span></span>
2. <span data-ttu-id="51ecb-194">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="51ecb-194">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="51ecb-195">Vstup [datovou sadu](data-factory-create-datasets.md) typu [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="51ecb-195">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="51ecb-196">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="51ecb-196">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="51ecb-197">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [RelationalSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="51ecb-197">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="51ecb-198">Ukázka zkopíruje data z instance SAP HANA do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="51ecb-198">The sample copies data from an SAP HANA instance to an Azure blob hourly.</span></span> <span data-ttu-id="51ecb-199">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="51ecb-199">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="51ecb-200">Jako první krok nastavte Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="51ecb-200">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="51ecb-201">Tyto pokyny jsou v [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="51ecb-201">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

### <a name="sap-hana-linked-service"></a><span data-ttu-id="51ecb-202">SAP HANA propojené služby</span><span class="sxs-lookup"><span data-stu-id="51ecb-202">SAP HANA linked service</span></span>
<span data-ttu-id="51ecb-203">Tato propojená služba propojuje SAP HANA instanci objektu pro vytváření dat..</span><span class="sxs-lookup"><span data-stu-id="51ecb-203">This linked service links your SAP HANA instance to the data factory.</span></span> <span data-ttu-id="51ecb-204">Vlastnost typ nastavena na **SapHana**.</span><span class="sxs-lookup"><span data-stu-id="51ecb-204">The type property is set to **SapHana**.</span></span> <span data-ttu-id="51ecb-205">Rámci typeProperties část obsahuje informace o připojení pro instance SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="51ecb-205">The typeProperties section provides connection information for the SAP HANA instance.</span></span>

```json
{
    "name": "SapHanaLinkedService",
    "properties":
    {
        "type": "SapHana",
        "typeProperties":
        {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="51ecb-206">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="51ecb-206">Azure Storage linked service</span></span>
<span data-ttu-id="51ecb-207">Tato propojená služba propojuje účet úložiště Azure pro vytváření dat..</span><span class="sxs-lookup"><span data-stu-id="51ecb-207">This linked service links your Azure Storage account to the data factory.</span></span> <span data-ttu-id="51ecb-208">Vlastnost typ nastavena na **azurestorage**.</span><span class="sxs-lookup"><span data-stu-id="51ecb-208">The type property is set to **AzureStorage**.</span></span> <span data-ttu-id="51ecb-209">Rámci typeProperties část obsahuje informace o připojení pro účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="51ecb-209">The typeProperties section provides connection information for the Azure Storage account.</span></span>

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

### <a name="sap-hana-input-dataset"></a><span data-ttu-id="51ecb-210">Vstupní datové sady SAP HANA</span><span class="sxs-lookup"><span data-stu-id="51ecb-210">SAP HANA input dataset</span></span>

<span data-ttu-id="51ecb-211">Tato datová sada definuje datovou sadu SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="51ecb-211">This dataset defines the SAP HANA dataset.</span></span> <span data-ttu-id="51ecb-212">Nastavte typ objektu pro vytváření dat datové sady, která **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="51ecb-212">You set the type of the Data Factory dataset to **RelationalTable**.</span></span> <span data-ttu-id="51ecb-213">V současné době nezadáte jakékoli vlastnosti specifické pro typ pro datové sadě služby SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="51ecb-213">Currently, you do not specify any type-specific properties for an SAP HANA dataset.</span></span> <span data-ttu-id="51ecb-214">Dotaz v definici aktivity kopírování Určuje, jaká data načíst z instance SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="51ecb-214">The query in the Copy Activity definition specifies what data to read from the SAP HANA instance.</span></span> 

<span data-ttu-id="51ecb-215">Služba Data Factory nastavení externí vlastnost na hodnotu true informuje, že v tabulce je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="51ecb-215">Setting external property to true informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

<span data-ttu-id="51ecb-216">Frekvence a intervalu vlastnosti definuje plán.</span><span class="sxs-lookup"><span data-stu-id="51ecb-216">Frequency and interval properties defines the schedule.</span></span> <span data-ttu-id="51ecb-217">V takovém případě dat je pro čtení z instance SAP HANA každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="51ecb-217">In this case, the data is read from the SAP HANA instance hourly.</span></span> 

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="51ecb-218">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="51ecb-218">Azure Blob output dataset</span></span>
<span data-ttu-id="51ecb-219">Tato datová sada definuje výstupní datovou sadu objektu Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="51ecb-219">This dataset defines the output Azure Blob dataset.</span></span> <span data-ttu-id="51ecb-220">Vlastnost typ nastavena na hodnotu AzureBlob.</span><span class="sxs-lookup"><span data-stu-id="51ecb-220">The type property is set to AzureBlob.</span></span> <span data-ttu-id="51ecb-221">V rámci typeProperties části poskytuje, které jsou uložená data zkopírovány z instance SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="51ecb-221">The typeProperties section provides where the data copied from the SAP HANA instance is stored.</span></span> <span data-ttu-id="51ecb-222">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="51ecb-222">The data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="51ecb-223">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="51ecb-223">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="51ecb-224">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="51ecb-224">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/saphana/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="51ecb-225">Kanál s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="51ecb-225">Pipeline with Copy activity</span></span>

<span data-ttu-id="51ecb-226">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="51ecb-226">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="51ecb-227">V definici JSON kanálu **zdroj** je typ nastaven na **RelationalSource** (pro SAP HANA zdroj) a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="51ecb-227">In the pipeline JSON definition, the **source** type is set to **RelationalSource** (for SAP HANA source) and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="51ecb-228">Zadané pro dotaz SQL **dotazu** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="51ecb-228">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<SQL Query for HANA>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapHanaDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDataSet"
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
                "name": "SapHanaToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```


### <a name="type-mapping-for-sap-hana"></a><span data-ttu-id="51ecb-229">Mapování typu pro SAP HANA</span><span class="sxs-lookup"><span data-stu-id="51ecb-229">Type mapping for SAP HANA</span></span>
<span data-ttu-id="51ecb-230">Jak je uvedeno v [aktivity přesunu dat](data-factory-data-movement-activities.md) článku aktivita kopírování provádí automatické typ převody z typů zdroje do jímky typů s následující postup ve dvou krocích:</span><span class="sxs-lookup"><span data-stu-id="51ecb-230">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="51ecb-231">Převést na typ .NET typy nativní zdrojů</span><span class="sxs-lookup"><span data-stu-id="51ecb-231">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="51ecb-232">Převést na typ jímky nativní typ formátu .NET</span><span class="sxs-lookup"><span data-stu-id="51ecb-232">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="51ecb-233">Při přesouvání dat od SAP HANA, se používají následující mapování z typů SAP HANA na typy .NET.</span><span class="sxs-lookup"><span data-stu-id="51ecb-233">When moving data from SAP HANA, the following mappings are used from SAP HANA types to .NET types.</span></span>

<span data-ttu-id="51ecb-234">Typ SAP HANA</span><span class="sxs-lookup"><span data-stu-id="51ecb-234">SAP HANA Type</span></span> | <span data-ttu-id="51ecb-235">.NET na základě typu</span><span class="sxs-lookup"><span data-stu-id="51ecb-235">.NET Based Type</span></span>
------------- | ---------------
<span data-ttu-id="51ecb-236">TINYINT</span><span class="sxs-lookup"><span data-stu-id="51ecb-236">TINYINT</span></span> | <span data-ttu-id="51ecb-237">Bajtů</span><span class="sxs-lookup"><span data-stu-id="51ecb-237">Byte</span></span>
<span data-ttu-id="51ecb-238">SMALLINT</span><span class="sxs-lookup"><span data-stu-id="51ecb-238">SMALLINT</span></span> | <span data-ttu-id="51ecb-239">Int16</span><span class="sxs-lookup"><span data-stu-id="51ecb-239">Int16</span></span>
<span data-ttu-id="51ecb-240">INT</span><span class="sxs-lookup"><span data-stu-id="51ecb-240">INT</span></span> | <span data-ttu-id="51ecb-241">Int32</span><span class="sxs-lookup"><span data-stu-id="51ecb-241">Int32</span></span>
<span data-ttu-id="51ecb-242">BIGINT</span><span class="sxs-lookup"><span data-stu-id="51ecb-242">BIGINT</span></span> | <span data-ttu-id="51ecb-243">Int64</span><span class="sxs-lookup"><span data-stu-id="51ecb-243">Int64</span></span>
<span data-ttu-id="51ecb-244">SKUTEČNÉ</span><span class="sxs-lookup"><span data-stu-id="51ecb-244">REAL</span></span> | <span data-ttu-id="51ecb-245">Jeden</span><span class="sxs-lookup"><span data-stu-id="51ecb-245">Single</span></span>
<span data-ttu-id="51ecb-246">DOUBLE</span><span class="sxs-lookup"><span data-stu-id="51ecb-246">DOUBLE</span></span> | <span data-ttu-id="51ecb-247">Jeden</span><span class="sxs-lookup"><span data-stu-id="51ecb-247">Single</span></span>
<span data-ttu-id="51ecb-248">DECIMAL</span><span class="sxs-lookup"><span data-stu-id="51ecb-248">DECIMAL</span></span> | <span data-ttu-id="51ecb-249">Decimal</span><span class="sxs-lookup"><span data-stu-id="51ecb-249">Decimal</span></span>
<span data-ttu-id="51ecb-250">LOGICKÁ HODNOTA</span><span class="sxs-lookup"><span data-stu-id="51ecb-250">BOOLEAN</span></span> | <span data-ttu-id="51ecb-251">Bajtů</span><span class="sxs-lookup"><span data-stu-id="51ecb-251">Byte</span></span>
<span data-ttu-id="51ecb-252">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="51ecb-252">VARCHAR</span></span> | <span data-ttu-id="51ecb-253">Řetězec</span><span class="sxs-lookup"><span data-stu-id="51ecb-253">String</span></span>
<span data-ttu-id="51ecb-254">NVARCHAR</span><span class="sxs-lookup"><span data-stu-id="51ecb-254">NVARCHAR</span></span> | <span data-ttu-id="51ecb-255">Řetězec</span><span class="sxs-lookup"><span data-stu-id="51ecb-255">String</span></span>
<span data-ttu-id="51ecb-256">DATOVÝ TYP CLOB</span><span class="sxs-lookup"><span data-stu-id="51ecb-256">CLOB</span></span> | <span data-ttu-id="51ecb-257">Byte]</span><span class="sxs-lookup"><span data-stu-id="51ecb-257">Byte[]</span></span>
<span data-ttu-id="51ecb-258">ALPHANUM</span><span class="sxs-lookup"><span data-stu-id="51ecb-258">ALPHANUM</span></span> | <span data-ttu-id="51ecb-259">Řetězec</span><span class="sxs-lookup"><span data-stu-id="51ecb-259">String</span></span>
<span data-ttu-id="51ecb-260">OBJEKT BLOB</span><span class="sxs-lookup"><span data-stu-id="51ecb-260">BLOB</span></span> | <span data-ttu-id="51ecb-261">Byte]</span><span class="sxs-lookup"><span data-stu-id="51ecb-261">Byte[]</span></span>
<span data-ttu-id="51ecb-262">DATUM</span><span class="sxs-lookup"><span data-stu-id="51ecb-262">DATE</span></span> | <span data-ttu-id="51ecb-263">Data a času</span><span class="sxs-lookup"><span data-stu-id="51ecb-263">DateTime</span></span>
<span data-ttu-id="51ecb-264">ČAS</span><span class="sxs-lookup"><span data-stu-id="51ecb-264">TIME</span></span> | <span data-ttu-id="51ecb-265">Časový interval</span><span class="sxs-lookup"><span data-stu-id="51ecb-265">TimeSpan</span></span>
<span data-ttu-id="51ecb-266">ČASOVÉ RAZÍTKO</span><span class="sxs-lookup"><span data-stu-id="51ecb-266">TIMESTAMP</span></span> | <span data-ttu-id="51ecb-267">Data a času</span><span class="sxs-lookup"><span data-stu-id="51ecb-267">DateTime</span></span>
<span data-ttu-id="51ecb-268">SECONDDATE</span><span class="sxs-lookup"><span data-stu-id="51ecb-268">SECONDDATE</span></span> | <span data-ttu-id="51ecb-269">Data a času</span><span class="sxs-lookup"><span data-stu-id="51ecb-269">DateTime</span></span>

## <a name="known-limitations"></a><span data-ttu-id="51ecb-270">Známá omezení</span><span class="sxs-lookup"><span data-stu-id="51ecb-270">Known limitations</span></span>
<span data-ttu-id="51ecb-271">Při kopírování dat z SAP HANA existuje několik známá omezení:</span><span class="sxs-lookup"><span data-stu-id="51ecb-271">There are a few known limitations when copying data from SAP HANA:</span></span>

- <span data-ttu-id="51ecb-272">NVARCHAR řetězce byl zkrácen na maximální délce 4000 znaků Unicode</span><span class="sxs-lookup"><span data-stu-id="51ecb-272">NVARCHAR strings are truncated to maximum length of 4000 Unicode characters</span></span>
- <span data-ttu-id="51ecb-273">SMALLDECIMAL není podporován.</span><span class="sxs-lookup"><span data-stu-id="51ecb-273">SMALLDECIMAL is not supported</span></span>
- <span data-ttu-id="51ecb-274">VARBINARY není podporován.</span><span class="sxs-lookup"><span data-stu-id="51ecb-274">VARBINARY is not supported</span></span>
- <span data-ttu-id="51ecb-275">Platná data jsou mezi 1899/12/30 a 9999/12/31</span><span class="sxs-lookup"><span data-stu-id="51ecb-275">Valid Dates are between 1899/12/30 and 9999/12/31</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="51ecb-276">Mapování zdroje jímky sloupců</span><span class="sxs-lookup"><span data-stu-id="51ecb-276">Map source to sink columns</span></span>
<span data-ttu-id="51ecb-277">Další informace o mapování sloupců v datové sadě zdrojového sloupce v datové sadě podřízený najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="51ecb-277">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="51ecb-278">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="51ecb-278">Repeatable read from relational sources</span></span>
<span data-ttu-id="51ecb-279">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="51ecb-279">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="51ecb-280">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="51ecb-280">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="51ecb-281">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="51ecb-281">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="51ecb-282">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="51ecb-282">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="51ecb-283">V tématu [Repeatable číst z relační zdrojů](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="51ecb-283">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="51ecb-284">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="51ecb-284">Performance and Tuning</span></span>
<span data-ttu-id="51ecb-285">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="51ecb-285">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
