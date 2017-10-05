---
title: "Přesun dat z místní HDFS | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z HDFS místně pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 9a8f3156a62a1a7aa49377349e8a85454efeda50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a><span data-ttu-id="7eb1a-103">Přesun dat z místní HDFS pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="7eb1a-103">Move data from on-premises HDFS using Azure Data Factory</span></span>
<span data-ttu-id="7eb1a-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z místní službě HDFS.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises HDFS.</span></span> <span data-ttu-id="7eb1a-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="7eb1a-106">Můžete zkopírovat data z HDFS do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-106">You can copy data from HDFS to any supported sink data store.</span></span> <span data-ttu-id="7eb1a-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="7eb1a-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z místní službě HDFS k jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat na místní službě HDFS.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-108">Data factory currently supports only moving data from an on-premises HDFS to other data stores, but not for moving data from other data stores to an on-premises HDFS.</span></span>

> [!NOTE]
> <span data-ttu-id="7eb1a-109">Aktivita kopírování nedojde k odstranění zdrojového souboru po byl úspěšně zkopírován do cílové.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-109">Copy Activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="7eb1a-110">Pokud potřebujete odstranit zdrojový soubor po úspěšné kopie, vytvořte vlastní aktivity odstranit soubor a použijte aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-110">If you need to delete the source file after a successful copy, create a custom activity to delete the file and use the activity in the pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="7eb1a-111">Povolení připojení</span><span class="sxs-lookup"><span data-stu-id="7eb1a-111">Enabling connectivity</span></span>
<span data-ttu-id="7eb1a-112">Služba data Factory podporuje připojení k místní službě HDFS pomocí Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-112">Data Factory service supports connecting to on-premises HDFS using the Data Management Gateway.</span></span> <span data-ttu-id="7eb1a-113">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku se dozvíte o Brána pro správu dat a podrobné pokyny o nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-113">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span> <span data-ttu-id="7eb1a-114">Použijte bránu pro připojení k HDFS i v případě, že je hostovaná ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-114">Use the gateway to connect to HDFS even if it is hosted in an Azure IaaS VM.</span></span>

> [!NOTE]
> <span data-ttu-id="7eb1a-115">Ujistěte se, že brána pro správu dat přístup k **všechny** [název uzlu serveru]: [name portu uzlu] a [data uzlu servery]: [datový uzel port] clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-115">Make sure the Data Management Gateway can access to **ALL** the [name node server]:[name node port] and [data node servers]:[data node port] of the Hadoop cluster.</span></span> <span data-ttu-id="7eb1a-116">Výchozí [název uzlu port] je 50070 a výchozí hodnota [datový uzel port] je 50075.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-116">Default [name node port] is 50070, and default [data node port] is 50075.</span></span>

<span data-ttu-id="7eb1a-117">Zatímco můžete bránu nainstalovat na stejný místní počítač nebo virtuální počítač Azure, jako HDFS, doporučujeme nainstalovat bránu na samostatný počítač nebo Azure IaaS virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-117">While you can install gateway on the same on-premises machine or the Azure VM as the HDFS, we recommend that you install the gateway on a separate machine/Azure IaaS VM.</span></span> <span data-ttu-id="7eb1a-118">S brány na samostatný počítač snižuje kolize prostředků a zvyšuje výkon.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-118">Having gateway on a separate machine reduces resource contention and improves performance.</span></span> <span data-ttu-id="7eb1a-119">Při instalaci brány na samostatný počítač, na počítač byste měli mít přístup k počítači s HDFS.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-119">When you install the gateway on a separate machine, the machine should be able to access the machine with the HDFS.</span></span>

## <a name="getting-started"></a><span data-ttu-id="7eb1a-120">Začínáme</span><span class="sxs-lookup"><span data-stu-id="7eb1a-120">Getting started</span></span>
<span data-ttu-id="7eb1a-121">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z HDFS zdroje pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-121">You can create a pipeline with a copy activity that moves data from a HDFS source by using different tools/APIs.</span></span>

<span data-ttu-id="7eb1a-122">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-122">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="7eb1a-123">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="7eb1a-124">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-124">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="7eb1a-125">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="7eb1a-126">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-126">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="7eb1a-127">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-127">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="7eb1a-128">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-128">Create **datasets** to represent input and output data for the copy operation.</span></span>
3. <span data-ttu-id="7eb1a-129">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-129">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="7eb1a-130">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-130">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="7eb1a-131">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-131">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="7eb1a-132">Příklad s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z úložiště dat HDFS, naleznete v tématu [JSON příklad: kopírování dat z místní HDFS do objektu Blob Azure](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-132">For a sample with JSON definitions for Data Factory entities that are used to copy data from a HDFS data store, see [JSON example: Copy data from on-premises HDFS to Azure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) section of this article.</span></span>

<span data-ttu-id="7eb1a-133">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k definování entit služby Data Factory, které jsou specifické pro HDFS:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-133">The following sections provide details about JSON properties that are used to define Data Factory entities specific to HDFS:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="7eb1a-134">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="7eb1a-134">Linked service properties</span></span>
<span data-ttu-id="7eb1a-135">Propojená služba odkazuje na objekt pro vytváření dat úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-135">A linked service links a data store to a data factory.</span></span> <span data-ttu-id="7eb1a-136">Vytvoření propojené služby typu **Hdfs** propojit místní službě HDFS do data factory.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-136">You create a linked service of type **Hdfs** to link an on-premises HDFS to your data factory.</span></span> <span data-ttu-id="7eb1a-137">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro HDFS propojené služby.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-137">The following table provides description for JSON elements specific to HDFS linked service.</span></span>

| <span data-ttu-id="7eb1a-138">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7eb1a-138">Property</span></span> | <span data-ttu-id="7eb1a-139">Popis</span><span class="sxs-lookup"><span data-stu-id="7eb1a-139">Description</span></span> | <span data-ttu-id="7eb1a-140">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7eb1a-140">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7eb1a-141">type</span><span class="sxs-lookup"><span data-stu-id="7eb1a-141">type</span></span> |<span data-ttu-id="7eb1a-142">Vlastnost typu musí být nastavena na: **Hdfs**</span><span class="sxs-lookup"><span data-stu-id="7eb1a-142">The type property must be set to: **Hdfs**</span></span> |<span data-ttu-id="7eb1a-143">Ano</span><span class="sxs-lookup"><span data-stu-id="7eb1a-143">Yes</span></span> |
| <span data-ttu-id="7eb1a-144">URL</span><span class="sxs-lookup"><span data-stu-id="7eb1a-144">Url</span></span> |<span data-ttu-id="7eb1a-145">Adresa URL HDFS</span><span class="sxs-lookup"><span data-stu-id="7eb1a-145">URL to the HDFS</span></span> |<span data-ttu-id="7eb1a-146">Ano</span><span class="sxs-lookup"><span data-stu-id="7eb1a-146">Yes</span></span> |
| <span data-ttu-id="7eb1a-147">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-147">authenticationType</span></span> |<span data-ttu-id="7eb1a-148">Anonymní, nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-148">Anonymous, or Windows.</span></span> <br><br> <span data-ttu-id="7eb1a-149">Použít **ověřování protokolem Kerberos** HDFS konektor, najdete v části [v této části](#use-kerberos-authentication-for-hdfs-connector) odpovídajícím způsobem nastavit v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-149">To use **Kerberos authentication** for HDFS connector, refer to [this section](#use-kerberos-authentication-for-hdfs-connector) to set up your on-premises environment accordingly.</span></span> |<span data-ttu-id="7eb1a-150">Ano</span><span class="sxs-lookup"><span data-stu-id="7eb1a-150">Yes</span></span> |
| <span data-ttu-id="7eb1a-151">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="7eb1a-151">userName</span></span> |<span data-ttu-id="7eb1a-152">Ověřování uživatelského jména pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-152">Username for Windows authentication.</span></span> |<span data-ttu-id="7eb1a-153">Ano (pro ověřování systému Windows)</span><span class="sxs-lookup"><span data-stu-id="7eb1a-153">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="7eb1a-154">heslo</span><span class="sxs-lookup"><span data-stu-id="7eb1a-154">password</span></span> |<span data-ttu-id="7eb1a-155">Heslo pro ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-155">Password for Windows authentication.</span></span> |<span data-ttu-id="7eb1a-156">Ano (pro ověřování systému Windows)</span><span class="sxs-lookup"><span data-stu-id="7eb1a-156">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="7eb1a-157">gatewayName</span><span class="sxs-lookup"><span data-stu-id="7eb1a-157">gatewayName</span></span> |<span data-ttu-id="7eb1a-158">Název brány, kterou služba Data Factory měla použít pro připojení k HDFS.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-158">Name of the gateway that the Data Factory service should use to connect to the HDFS.</span></span> |<span data-ttu-id="7eb1a-159">Ano</span><span class="sxs-lookup"><span data-stu-id="7eb1a-159">Yes</span></span> |
| <span data-ttu-id="7eb1a-160">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="7eb1a-160">encryptedCredential</span></span> |<span data-ttu-id="7eb1a-161">[Nové AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) výstup pověření přístup.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-161">[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of the access credential.</span></span> |<span data-ttu-id="7eb1a-162">Ne</span><span class="sxs-lookup"><span data-stu-id="7eb1a-162">No</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="7eb1a-163">Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="7eb1a-163">Using Anonymous authentication</span></span>

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-windows-authentication"></a><span data-ttu-id="7eb1a-164">Pomocí ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="7eb1a-164">Using Windows authentication</span></span>

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```
## <a name="dataset-properties"></a><span data-ttu-id="7eb1a-165">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="7eb1a-165">Dataset properties</span></span>
<span data-ttu-id="7eb1a-166">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-166">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="7eb1a-167">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="7eb1a-168">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-168">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="7eb1a-169">Rámci typeProperties část datové sady typ **sdílení souborů** (což zahrnuje datová sada HDFS) má následující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7eb1a-169">The typeProperties section for dataset of type **FileShare** (which includes HDFS dataset) has the following properties</span></span>

| <span data-ttu-id="7eb1a-170">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7eb1a-170">Property</span></span> | <span data-ttu-id="7eb1a-171">Popis</span><span class="sxs-lookup"><span data-stu-id="7eb1a-171">Description</span></span> | <span data-ttu-id="7eb1a-172">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7eb1a-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7eb1a-173">folderPath</span><span class="sxs-lookup"><span data-stu-id="7eb1a-173">folderPath</span></span> |<span data-ttu-id="7eb1a-174">Cesta ke složce.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-174">Path to the folder.</span></span> <span data-ttu-id="7eb1a-175">Příklad:`myfolder`</span><span class="sxs-lookup"><span data-stu-id="7eb1a-175">Example: `myfolder`</span></span><br/><br/><span data-ttu-id="7eb1a-176">Použít řídicí znak ' \ ' pro speciální znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-176">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="7eb1a-177">Příklad: pro folder\subfolder, určete složku\\\\podsložky a pro d:\samplefolder, zadejte d:\\\\ukázková_složka.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-177">For example: for folder\subfolder, specify folder\\\\subfolder and for d:\samplefolder, specify d:\\\\samplefolder.</span></span><br/><br/><span data-ttu-id="7eb1a-178">Tato vlastnost se můžete kombinovat **partitionBy** tak, aby měl složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-178">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="7eb1a-179">Ano</span><span class="sxs-lookup"><span data-stu-id="7eb1a-179">Yes</span></span> |
| <span data-ttu-id="7eb1a-180">fileName</span><span class="sxs-lookup"><span data-stu-id="7eb1a-180">fileName</span></span> |<span data-ttu-id="7eb1a-181">Zadejte název souboru do **folderPath** Pokud chcete, aby v tabulce odkazovat na konkrétní soubor ve složce.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-181">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="7eb1a-182">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, tabulka odkazuje na všechny soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-182">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="7eb1a-183">Pokud není zadán název souboru pro datovou sadu výstupů, název vygenerovaný soubor bude v následujícím tento formát:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-183">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="7eb1a-184">Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="7eb1a-184">Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="7eb1a-185">Ne</span><span class="sxs-lookup"><span data-stu-id="7eb1a-185">No</span></span> |
| <span data-ttu-id="7eb1a-186">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="7eb1a-186">partitionedBy</span></span> |<span data-ttu-id="7eb1a-187">partitionedBy slouží k určení dynamické folderPath, název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-187">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="7eb1a-188">Příklad: folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-188">Example: folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="7eb1a-189">Ne</span><span class="sxs-lookup"><span data-stu-id="7eb1a-189">No</span></span> |
| <span data-ttu-id="7eb1a-190">Formát</span><span class="sxs-lookup"><span data-stu-id="7eb1a-190">format</span></span> | <span data-ttu-id="7eb1a-191">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-191">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="7eb1a-192">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-192">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="7eb1a-193">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-193">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="7eb1a-194">Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-194">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="7eb1a-195">Ne</span><span class="sxs-lookup"><span data-stu-id="7eb1a-195">No</span></span> |
| <span data-ttu-id="7eb1a-196">Komprese</span><span class="sxs-lookup"><span data-stu-id="7eb1a-196">compression</span></span> | <span data-ttu-id="7eb1a-197">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-197">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="7eb1a-198">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-198">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="7eb1a-199">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-199">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="7eb1a-200">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-200">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="7eb1a-201">Ne</span><span class="sxs-lookup"><span data-stu-id="7eb1a-201">No</span></span> |

> [!NOTE]
> <span data-ttu-id="7eb1a-202">Název souboru a fileFilter nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-202">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="7eb1a-203">Pomocí vlastnosti partionedBy</span><span class="sxs-lookup"><span data-stu-id="7eb1a-203">Using partionedBy property</span></span>
<span data-ttu-id="7eb1a-204">Jak je uvedeno v předchozí části, můžete zadat dynamické folderPath a název souboru pro data časové řady s **partitionedBy** vlastnost [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-204">As mentioned in the previous section, you can specify a dynamic folderPath and filename for time series data with the **partitionedBy** property, [Data Factory functions, and the system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="7eb1a-205">Další informace o datové sady času řady, plánování a řezů, najdete v části [vytváření datových sad](data-factory-create-datasets.md), [plánování a provádění](data-factory-scheduling-and-execution.md), a [vytváření kanálů](data-factory-create-pipelines.md) články.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-205">To learn more about time series datasets, scheduling, and slices, see [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="7eb1a-206">Příklad 1:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-206">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="7eb1a-207">V tomto příkladu {řez} se nahradí hodnotu objektu pro vytváření dat systému proměnné SliceStart ve formátu (YYYYMMDDHH) zadán.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-207">In this example {Slice} is replaced with the value of Data Factory system variable SliceStart in the format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="7eb1a-208">Vlastnosti SliceStart odkazuje na spuštění řezu.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-208">The SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="7eb1a-209">FolderPath se liší pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-209">The folderPath is different for each slice.</span></span> <span data-ttu-id="7eb1a-210">Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-210">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="7eb1a-211">Příklad 2:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-211">Sample 2:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
<span data-ttu-id="7eb1a-212">V tomto příkladu jsou extrahován rok, měsíc, den a čas SliceStart do samostatné proměnné, které jsou používány folderPath a název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-212">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="7eb1a-213">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="7eb1a-213">Copy activity properties</span></span>
<span data-ttu-id="7eb1a-214">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-214">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="7eb1a-215">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-215">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="7eb1a-216">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-216">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="7eb1a-217">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-217">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="7eb1a-218">Pro aktivitu kopírování, pokud je zdroj typu **FileSystemSource** následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-218">For Copy Activity, when source is of type **FileSystemSource** the following properties are available in typeProperties section:</span></span>

<span data-ttu-id="7eb1a-219">**FileSystemSource** podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-219">**FileSystemSource** supports the following properties:</span></span>

| <span data-ttu-id="7eb1a-220">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7eb1a-220">Property</span></span> | <span data-ttu-id="7eb1a-221">Popis</span><span class="sxs-lookup"><span data-stu-id="7eb1a-221">Description</span></span> | <span data-ttu-id="7eb1a-222">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="7eb1a-222">Allowed values</span></span> | <span data-ttu-id="7eb1a-223">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7eb1a-223">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7eb1a-224">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="7eb1a-224">recursive</span></span> |<span data-ttu-id="7eb1a-225">Označuje, zda je data načíst rekurzivně z dílčí složky nebo pouze do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-225">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="7eb1a-226">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="7eb1a-226">True, False (default)</span></span> |<span data-ttu-id="7eb1a-227">Ne</span><span class="sxs-lookup"><span data-stu-id="7eb1a-227">No</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="7eb1a-228">Podporované formáty souborů a komprese</span><span class="sxs-lookup"><span data-stu-id="7eb1a-228">Supported file and compression formats</span></span>
<span data-ttu-id="7eb1a-229">V tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článek na podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-229">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-on-premises-hdfs-to-azure-blob"></a><span data-ttu-id="7eb1a-230">Příklad JSON: kopírování dat z místní HDFS do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="7eb1a-230">JSON example: Copy data from on-premises HDFS to Azure Blob</span></span>
<span data-ttu-id="7eb1a-231">Tento příklad ukazuje, jak ke zkopírování dat z místní službě HDFS do úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-231">This sample shows how to copy data from an on-premises HDFS to Azure Blob Storage.</span></span> <span data-ttu-id="7eb1a-232">Však můžete zkopírovat data **přímo** žádnému jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-232">However, data can be copied **directly** to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="7eb1a-233">Ukázka poskytuje JSON definice pro následující entity služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-233">The sample provides JSON definitions for the following Data Factory entities.</span></span> <span data-ttu-id="7eb1a-234">Tyto definice můžete použít k vytvoření kanálu pro zkopírování dat z HDFS do Azure Blob Storage pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-234">You can use these definitions to create a pipeline to copy data from HDFS to Azure Blob Storage by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span>

1. <span data-ttu-id="7eb1a-235">Propojené služby typu [OnPremisesHdfs](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-235">A linked service of type [OnPremisesHdfs](#linked-service-properties).</span></span>
2. <span data-ttu-id="7eb1a-236">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-236">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="7eb1a-237">Vstup [datovou sadu](data-factory-create-datasets.md) typu [sdílení souborů](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-237">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
4. <span data-ttu-id="7eb1a-238">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-238">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="7eb1a-239">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-239">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="7eb1a-240">Ukázka zkopíruje data z místní službě HDFS do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-240">The sample copies data from an on-premises HDFS to an Azure blob every hour.</span></span> <span data-ttu-id="7eb1a-241">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-241">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="7eb1a-242">Jako první krok nastavte Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-242">As a first step, set up the data management gateway.</span></span> <span data-ttu-id="7eb1a-243">Podle pokynů [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-243">The instructions in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="7eb1a-244">**Propojená služba HDFS:** tento příklad používá ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-244">**HDFS linked service:** This example uses the Windows authentication.</span></span> <span data-ttu-id="7eb1a-245">V tématu [HDFS propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-245">See [HDFS linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

<span data-ttu-id="7eb1a-246">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="7eb1a-246">**Azure Storage linked service:**</span></span>

```JSON
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

<span data-ttu-id="7eb1a-247">**HDFS vstupní datovou sadu:** tato datová sada odkazuje na složku HDFS DataTransfer/UnitTest /.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-247">**HDFS input dataset:** This dataset refers to the HDFS folder DataTransfer/UnitTest/.</span></span> <span data-ttu-id="7eb1a-248">Kanál zkopíruje všechny soubory v této složce do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-248">The pipeline copies all the files in this folder to the destination.</span></span>

<span data-ttu-id="7eb1a-249">Nastavení "externí": "PRAVDA" informuje služba Data Factory, datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-249">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

<span data-ttu-id="7eb1a-250">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="7eb1a-250">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="7eb1a-251">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-251">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="7eb1a-252">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-252">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="7eb1a-253">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-253">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```JSON
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="7eb1a-254">**Aktivita kopírování v kanálu pomocí systému souborů zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="7eb1a-254">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="7eb1a-255">Kanál obsahuje aktivitu kopírování, která je konfigurovaná pro používání těchto vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-255">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="7eb1a-256">V definici JSON kanálu **zdroj** je typ nastaven na **FileSystemSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-256">In the pipeline JSON definition, the **source** type is set to **FileSystemSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="7eb1a-257">Zadané pro dotaz SQL **dotazu** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-257">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

```JSON
{
    "name": "pipeline",
    "properties":
    {
        "activities":
        [
            {
                "name": "HdfsToBlobCopy",
                "inputs": [ {"name": "InputDataset"} ],
                "outputs": [ {"name": "OutputDataset"} ],
                "type": "Copy",
                "typeProperties":
                {
                    "source":
                    {
                        "type": "FileSystemSource"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "policy":
                {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a><span data-ttu-id="7eb1a-258">Ověřování pomocí protokolu Kerberos pro HDFS konektor</span><span class="sxs-lookup"><span data-stu-id="7eb1a-258">Use Kerberos authentication for HDFS connector</span></span>
<span data-ttu-id="7eb1a-259">Existují dvě možnosti nastavit v místním prostředí tak, aby používala ověřování protokolu Kerberos v konektoru HDFS.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-259">There are two options to set up the on-premises environment so as to use Kerberos Authentication in HDFS connector.</span></span> <span data-ttu-id="7eb1a-260">Je možné, že byl lépe vyhovuje váš případ.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-260">You can choose the one better fits your case.</span></span>
* <span data-ttu-id="7eb1a-261">Možnost 1: [připojení počítače brány ve sféře Kerberos](#kerberos-join-realm)</span><span class="sxs-lookup"><span data-stu-id="7eb1a-261">Option 1: [Join gateway machine in Kerberos realm](#kerberos-join-realm)</span></span>
* <span data-ttu-id="7eb1a-262">Možnost 2: [povolit vzájemné vztah důvěryhodnosti mezi doménou systému Windows a Sféra Kerberos](#kerberos-mutual-trust)</span><span class="sxs-lookup"><span data-stu-id="7eb1a-262">Option 2: [Enable mutual trust between Windows domain and Kerberos realm](#kerberos-mutual-trust)</span></span>

### <span data-ttu-id="7eb1a-263"><a name="kerberos-join-realm"></a>Možnost 1: Připojení počítače brány ve sféře Kerberos</span><span class="sxs-lookup"><span data-stu-id="7eb1a-263"><a name="kerberos-join-realm"></a>Option 1: Join gateway machine in Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="7eb1a-264">Požadavek:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-264">Requirement:</span></span>

* <span data-ttu-id="7eb1a-265">Počítač brány je potřeba připojit Sféra Kerberos a nemůže připojit k žádné doméně systému Windows.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-265">The gateway machine needs to join the Kerberos realm and can’t join any Windows domain.</span></span>

#### <a name="how-to-configure"></a><span data-ttu-id="7eb1a-266">Postup konfigurace:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-266">How to configure:</span></span>

<span data-ttu-id="7eb1a-267">**Na počítači brány:**</span><span class="sxs-lookup"><span data-stu-id="7eb1a-267">**On gateway machine:**</span></span>

1.  <span data-ttu-id="7eb1a-268">Spustit **Ksetup** nástroj Konfigurace serveru protokolu Kerberos služby KDC a sféru.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-268">Run the **Ksetup** utility to configure the Kerberos KDC server and realm.</span></span>

    <span data-ttu-id="7eb1a-269">Počítač musí být nakonfigurován jako člena pracovní skupiny, protože Sféra Kerberos se liší od domény systému Windows.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-269">The machine must be configured as a member of a workgroup since a Kerberos realm is different from a Windows domain.</span></span> <span data-ttu-id="7eb1a-270">Toho lze dosáhnout nastavením Sféra Kerberos a takto přidejte server služby KDC.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-270">This can be achieved by setting the Kerberos realm and adding a KDC server as follows.</span></span> <span data-ttu-id="7eb1a-271">Nahraďte *REALM.COM* s vlastními příslušných sféry podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-271">Replace *REALM.COM* with your own respective realm as needed.</span></span>

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    <span data-ttu-id="7eb1a-272">**Restartujte** počítače po provedení těchto příkazů 2.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-272">**Restart** the machine after executing these 2 commands.</span></span>

2.  <span data-ttu-id="7eb1a-273">Ověření konfigurace s **Ksetup** příkaz.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-273">Verify the configuration with **Ksetup** command.</span></span> <span data-ttu-id="7eb1a-274">Výstup by měl vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-274">The output should be like:</span></span>

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

<span data-ttu-id="7eb1a-275">**V Azure Data Factory:**</span><span class="sxs-lookup"><span data-stu-id="7eb1a-275">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="7eb1a-276">Konfigurovat pomocí konektoru HDFS **ověřování systému Windows** společně s hlavní název protokolu Kerberos a heslo pro připojení ke zdroji dat HDFS.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-276">Configure the HDFS connector using **Windows authentication** together with your Kerberos principal name and password to connect to the HDFS data source.</span></span> <span data-ttu-id="7eb1a-277">Zkontrolujte [vlastnosti propojené služby HDFS](#linked-service-properties) části na podrobnosti o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-277">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

### <span data-ttu-id="7eb1a-278"><a name="kerberos-mutual-trust"></a>Možnost 2: Povolení vzájemné vztah důvěryhodnosti mezi doménou systému Windows a Sféra Kerberos</span><span class="sxs-lookup"><span data-stu-id="7eb1a-278"><a name="kerberos-mutual-trust"></a>Option 2: Enable mutual trust between Windows domain and Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="7eb1a-279">Požadavek:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-279">Requirement:</span></span>
*   <span data-ttu-id="7eb1a-280">Počítač brány musí připojit k doméně systému Windows.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-280">The gateway machine must join a Windows domain.</span></span>
*   <span data-ttu-id="7eb1a-281">Budete potřebovat oprávnění k aktualizaci nastavení řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-281">You need permission to update the domain controller's settings.</span></span>

#### <a name="how-to-configure"></a><span data-ttu-id="7eb1a-282">Postup konfigurace:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-282">How to configure:</span></span>

> [!NOTE]
> <span data-ttu-id="7eb1a-283">Nahraďte REALM.COM a AD.COM v následujícím kurzu vlastní příslušných sféry a řadič domény, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-283">Replace REALM.COM and AD.COM in the following tutorial with your own respective realm and domain controller as needed.</span></span>

<span data-ttu-id="7eb1a-284">**Na serveru služby KDC:**</span><span class="sxs-lookup"><span data-stu-id="7eb1a-284">**On KDC server:**</span></span>

1.  <span data-ttu-id="7eb1a-285">Upravit konfiguraci služby KDC v **krb5.conf** souboru chcete, aby služba KDC vztahu důvěryhodnosti domény systému Windows na následující konfigurace šablony.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-285">Edit the KDC configuration in **krb5.conf** file to let KDC trust Windows Domain referring to the following configuration template.</span></span> <span data-ttu-id="7eb1a-286">Ve výchozím nastavení nachází ve **/etc/krb5.conf**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-286">By default, the configuration is located at **/etc/krb5.conf**.</span></span>

            [logging]
             default = FILE:/var/log/krb5libs.log
             kdc = FILE:/var/log/krb5kdc.log
             admin_server = FILE:/var/log/kadmind.log

            [libdefaults]
             default_realm = REALM.COM
             dns_lookup_realm = false
             dns_lookup_kdc = false
             ticket_lifetime = 24h
             renew_lifetime = 7d
             forwardable = true

            [realms]
             REALM.COM = {
              kdc = node.REALM.COM
              admin_server = node.REALM.COM
             }
            AD.COM = {
             kdc = windc.ad.com
             admin_server = windc.ad.com
            }

            [domain_realm]
             .REALM.COM = REALM.COM
             REALM.COM = REALM.COM
             .ad.com = AD.COM
             ad.com = AD.COM

            [capaths]
             AD.COM = {
              REALM.COM = .
             }

  <span data-ttu-id="7eb1a-287">**Restartujte** služba KDC po konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-287">**Restart** the KDC service after configuration.</span></span>

2.  <span data-ttu-id="7eb1a-288">Příprava objekt zabezpečení s názvem  **krbtgt/REALM.COM@AD.COM**  na serveru služby KDC pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-288">Prepare a principal named **krbtgt/REALM.COM@AD.COM** in KDC server with the following command:</span></span>

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  <span data-ttu-id="7eb1a-289">V **hadoop.security.auth_to_local** konfigurace služby HDFS souboru, přidejte `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-289">In **hadoop.security.auth_to_local** HDFS service configuration file, add `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span></span>

<span data-ttu-id="7eb1a-290">**Na řadiči domény:**</span><span class="sxs-lookup"><span data-stu-id="7eb1a-290">**On domain controller:**</span></span>

1.  <span data-ttu-id="7eb1a-291">Spusťte následující **Ksetup** přidejte položku sféry:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-291">Run the following **Ksetup** commands to add a realm entry:</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  <span data-ttu-id="7eb1a-292">Navázání vztahu důvěryhodnosti z domény systému Windows k Sféra Kerberos.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-292">Establish trust from Windows Domain to Kerberos Realm.</span></span> <span data-ttu-id="7eb1a-293">[heslo] je heslo pro objekt  **krbtgt/REALM.COM@AD.COM** .</span><span class="sxs-lookup"><span data-stu-id="7eb1a-293">[password] is the password for the principal **krbtgt/REALM.COM@AD.COM**.</span></span>

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  <span data-ttu-id="7eb1a-294">Vyberte šifrovací algoritmus použitý v protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-294">Select encryption algorithm used in Kerberos.</span></span>

    1. <span data-ttu-id="7eb1a-295">Přejděte do správce serveru > Správa zásad skupiny > domény > objekty zásad skupiny > výchozí nebo zásady služby Active Domain a upravit.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-295">Go to Server Manager > Group Policy Management > Domain > Group Policy Objects > Default or Active Domain Policy, and Edit.</span></span>

    2. <span data-ttu-id="7eb1a-296">V **Editor správy zásad skupiny** automaticky otevíraném okně, přejděte na konfigurace počítače > zásady > Nastavení systému Windows > Nastavení zabezpečení > Místní zásady > Možnosti zabezpečení a nakonfigurujte **zabezpečení sítě: typy konfigurace šifrování povolené pro protokol Kerberos**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-296">In the **Group Policy Management Editor** popup window, go to Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options, and configure **Network security: Configure Encryption types allowed for Kerberos**.</span></span>

    3. <span data-ttu-id="7eb1a-297">Vyberte algoritmus šifrování, kterou chcete použít při připojení k služby KDC.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-297">Select the encryption algorithm you want to use when connect to KDC.</span></span> <span data-ttu-id="7eb1a-298">Běžně můžete jednoduše vybrat všechny možnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-298">Commonly, you can simply select all the options.</span></span>

        ![Typy šifrování konfigurace pro protokol Kerberos](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. <span data-ttu-id="7eb1a-300">Použití **Ksetup** příkazu zadejte šifrovací algoritmus, který se má použít na konkrétní SFÉRY.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-300">Use **Ksetup** command to specify the encryption algorithm to be used on the specific REALM.</span></span>

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  <span data-ttu-id="7eb1a-301">Vytvoření mapování mezi účet domény a objektu zabezpečení pomocí protokolu Kerberos, aby bylo možné používat objekt zabezpečení protokolu Kerberos v doméně systému Windows.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-301">Create the mapping between the domain account and Kerberos principal, in order to use Kerberos principal in Windows Domain.</span></span>

    1. <span data-ttu-id="7eb1a-302">Spuštění nástroje pro správu > **Active Directory Users and Computers**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-302">Start the Administrative tools > **Active Directory Users and Computers**.</span></span>

    2. <span data-ttu-id="7eb1a-303">Konfigurace rozšířených funkcí kliknutím **zobrazení** > **pokročilé funkce**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-303">Configure advanced features by clicking **View** > **Advanced Features**.</span></span>

    3. <span data-ttu-id="7eb1a-304">Najděte účet, ke které chcete vytvořit mapování a klikněte pravým tlačítkem myši zobrazíte **mapování názvů** > klikněte na tlačítko **Kerberos názvy** kartě.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-304">Locate the account to which you want to create mappings, and right-click to view **Name Mappings** > click **Kerberos Names** tab.</span></span>

    4. <span data-ttu-id="7eb1a-305">Přidáte objekt z sféru.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-305">Add a principal from the realm.</span></span>

        ![Mapování Identity zabezpečení](media/data-factory-hdfs-connector/map-security-identity.png)

<span data-ttu-id="7eb1a-307">**Na počítači brány:**</span><span class="sxs-lookup"><span data-stu-id="7eb1a-307">**On gateway machine:**</span></span>

* <span data-ttu-id="7eb1a-308">Spusťte následující **Ksetup** přidejte položku sféry.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-308">Run the following **Ksetup** commands to add a realm entry.</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

<span data-ttu-id="7eb1a-309">**V Azure Data Factory:**</span><span class="sxs-lookup"><span data-stu-id="7eb1a-309">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="7eb1a-310">Konfigurovat pomocí konektoru HDFS **ověřování systému Windows** společně s buď váš účet domény nebo objekt zabezpečení protokolu Kerberos pro připojení ke zdroji dat HDFS.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-310">Configure the HDFS connector using **Windows authentication** together with either your Domain Account or Kerberos Principal to connect to the HDFS data source.</span></span> <span data-ttu-id="7eb1a-311">Zkontrolujte [vlastnosti propojené služby HDFS](#linked-service-properties) části na podrobnosti o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-311">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

> [!NOTE]
> <span data-ttu-id="7eb1a-312">Mapování sloupců z datové sady zdroje na sloupce ze sady jímku dat naleznete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-312">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="performance-and-tuning"></a><span data-ttu-id="7eb1a-313">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="7eb1a-313">Performance and Tuning</span></span>
<span data-ttu-id="7eb1a-314">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-314">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
