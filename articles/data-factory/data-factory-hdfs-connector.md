---
title: "aaaMove data z HDFS místní | Microsoft Docs"
description: "Další informace o tom, jak toomove data z místní HDFS pomocí Azure Data Factory."
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
ms.openlocfilehash: 96387e5dd089099fc2e983ab26d67c2044b973b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a><span data-ttu-id="44d8a-103">Přesun dat z místní HDFS pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="44d8a-103">Move data from on-premises HDFS using Azure Data Factory</span></span>
<span data-ttu-id="44d8a-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z místní službě HDFS.</span><span class="sxs-lookup"><span data-stu-id="44d8a-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises HDFS.</span></span> <span data-ttu-id="44d8a-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="44d8a-106">Může kopírovat data z úložiště dat podřízený HDFS tooany podporována.</span><span class="sxs-lookup"><span data-stu-id="44d8a-106">You can copy data from HDFS tooany supported sink data store.</span></span> <span data-ttu-id="44d8a-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="44d8a-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="44d8a-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z úložiště dat tooother místní HDFS, ale není pro přesun dat z jiných dat úložiště tooan místní HDFS.</span><span class="sxs-lookup"><span data-stu-id="44d8a-108">Data factory currently supports only moving data from an on-premises HDFS tooother data stores, but not for moving data from other data stores tooan on-premises HDFS.</span></span>

> [!NOTE]
> <span data-ttu-id="44d8a-109">Aktivita kopírování neodstraní hello zdrojový soubor po úspěšně zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="44d8a-109">Copy Activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="44d8a-110">Pokud potřebujete toodelete hello zdrojový soubor po úspěšné kopie, vytvořte soubor vlastní aktivity toodelete hello a používat hello aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-110">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file and use hello activity in hello pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="44d8a-111">Povolení připojení</span><span class="sxs-lookup"><span data-stu-id="44d8a-111">Enabling connectivity</span></span>
<span data-ttu-id="44d8a-112">Služba data Factory podporuje pomocí hello Brána pro správu dat připojování tooon místní HDFS.</span><span class="sxs-lookup"><span data-stu-id="44d8a-112">Data Factory service supports connecting tooon-premises HDFS using hello Data Management Gateway.</span></span> <span data-ttu-id="44d8a-113">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány.</span><span class="sxs-lookup"><span data-stu-id="44d8a-113">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span> <span data-ttu-id="44d8a-114">Použijte hello brány tooconnect tooHDFS i v případě, že je hostovaná ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="44d8a-114">Use hello gateway tooconnect tooHDFS even if it is hosted in an Azure IaaS VM.</span></span>

> [!NOTE]
> <span data-ttu-id="44d8a-115">Ujistěte se, hello Brána pro správu dat přístupný příliš**všechny** hello [název uzlu serveru]: [name portu uzlu] a [data uzlu servery]: [datový uzel port] clusteru Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-115">Make sure hello Data Management Gateway can access too**ALL** hello [name node server]:[name node port] and [data node servers]:[data node port] of hello Hadoop cluster.</span></span> <span data-ttu-id="44d8a-116">Výchozí [název uzlu port] je 50070 a výchozí hodnota [datový uzel port] je 50075.</span><span class="sxs-lookup"><span data-stu-id="44d8a-116">Default [name node port] is 50070, and default [data node port] is 50075.</span></span>

<span data-ttu-id="44d8a-117">Když jste bránu nainstalovat na hello stejné místní počítač nebo virtuální počítač Azure hello jako hello HDFS, doporučujeme nainstalovat hello brány na samostatný počítač nebo Azure IaaS virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="44d8a-117">While you can install gateway on hello same on-premises machine or hello Azure VM as hello HDFS, we recommend that you install hello gateway on a separate machine/Azure IaaS VM.</span></span> <span data-ttu-id="44d8a-118">S brány na samostatný počítač snižuje kolize prostředků a zvyšuje výkon.</span><span class="sxs-lookup"><span data-stu-id="44d8a-118">Having gateway on a separate machine reduces resource contention and improves performance.</span></span> <span data-ttu-id="44d8a-119">Když instalujete na samostatný počítač hello brány, hello počítač měl být schopný tooaccess hello počítač s hello HDFS.</span><span class="sxs-lookup"><span data-stu-id="44d8a-119">When you install hello gateway on a separate machine, hello machine should be able tooaccess hello machine with hello HDFS.</span></span>

## <a name="getting-started"></a><span data-ttu-id="44d8a-120">Začínáme</span><span class="sxs-lookup"><span data-stu-id="44d8a-120">Getting started</span></span>
<span data-ttu-id="44d8a-121">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z HDFS zdroje pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="44d8a-121">You can create a pipeline with a copy activity that moves data from a HDFS source by using different tools/APIs.</span></span>

<span data-ttu-id="44d8a-122">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="44d8a-122">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="44d8a-123">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="44d8a-124">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="44d8a-124">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="44d8a-125">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="44d8a-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="44d8a-126">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="44d8a-126">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="44d8a-127">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="44d8a-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="44d8a-128">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="44d8a-128">Create **datasets** toorepresent input and output data for hello copy operation.</span></span>
3. <span data-ttu-id="44d8a-129">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="44d8a-129">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="44d8a-130">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="44d8a-130">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="44d8a-131">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-131">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="44d8a-132">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data z jiného úložiště dat HDFS, naleznete v tématu [JSON příklad: kopírování dat z místní HDFS tooAzure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="44d8a-132">For a sample with JSON definitions for Data Factory entities that are used toocopy data from a HDFS data store, see [JSON example: Copy data from on-premises HDFS tooAzure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) section of this article.</span></span>

<span data-ttu-id="44d8a-133">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooHDFS:</span><span class="sxs-lookup"><span data-stu-id="44d8a-133">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooHDFS:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="44d8a-134">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="44d8a-134">Linked service properties</span></span>
<span data-ttu-id="44d8a-135">Propojená služba odkazuje data store tooa objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="44d8a-135">A linked service links a data store tooa data factory.</span></span> <span data-ttu-id="44d8a-136">Vytvoření propojené služby typu **Hdfs** toolink služby místní HDFS tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="44d8a-136">You create a linked service of type **Hdfs** toolink an on-premises HDFS tooyour data factory.</span></span> <span data-ttu-id="44d8a-137">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooHDFS propojené služby.</span><span class="sxs-lookup"><span data-stu-id="44d8a-137">hello following table provides description for JSON elements specific tooHDFS linked service.</span></span>

| <span data-ttu-id="44d8a-138">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="44d8a-138">Property</span></span> | <span data-ttu-id="44d8a-139">Popis</span><span class="sxs-lookup"><span data-stu-id="44d8a-139">Description</span></span> | <span data-ttu-id="44d8a-140">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="44d8a-140">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="44d8a-141">type</span><span class="sxs-lookup"><span data-stu-id="44d8a-141">type</span></span> |<span data-ttu-id="44d8a-142">vlastnost typu Hello musí být nastavena na: **Hdfs**</span><span class="sxs-lookup"><span data-stu-id="44d8a-142">hello type property must be set to: **Hdfs**</span></span> |<span data-ttu-id="44d8a-143">Ano</span><span class="sxs-lookup"><span data-stu-id="44d8a-143">Yes</span></span> |
| <span data-ttu-id="44d8a-144">URL</span><span class="sxs-lookup"><span data-stu-id="44d8a-144">Url</span></span> |<span data-ttu-id="44d8a-145">Adresa URL toohello HDFS</span><span class="sxs-lookup"><span data-stu-id="44d8a-145">URL toohello HDFS</span></span> |<span data-ttu-id="44d8a-146">Ano</span><span class="sxs-lookup"><span data-stu-id="44d8a-146">Yes</span></span> |
| <span data-ttu-id="44d8a-147">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="44d8a-147">authenticationType</span></span> |<span data-ttu-id="44d8a-148">Anonymní, nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="44d8a-148">Anonymous, or Windows.</span></span> <br><br> <span data-ttu-id="44d8a-149">toouse **ověřování protokolem Kerberos** HDFS konektor, najdete v části příliš[v této části](#use-kerberos-authentication-for-hdfs-connector) tooset prostředí místní odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="44d8a-149">toouse **Kerberos authentication** for HDFS connector, refer too[this section](#use-kerberos-authentication-for-hdfs-connector) tooset up your on-premises environment accordingly.</span></span> |<span data-ttu-id="44d8a-150">Ano</span><span class="sxs-lookup"><span data-stu-id="44d8a-150">Yes</span></span> |
| <span data-ttu-id="44d8a-151">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="44d8a-151">userName</span></span> |<span data-ttu-id="44d8a-152">Ověřování uživatelského jména pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="44d8a-152">Username for Windows authentication.</span></span> |<span data-ttu-id="44d8a-153">Ano (pro ověřování systému Windows)</span><span class="sxs-lookup"><span data-stu-id="44d8a-153">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="44d8a-154">heslo</span><span class="sxs-lookup"><span data-stu-id="44d8a-154">password</span></span> |<span data-ttu-id="44d8a-155">Heslo pro ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="44d8a-155">Password for Windows authentication.</span></span> |<span data-ttu-id="44d8a-156">Ano (pro ověřování systému Windows)</span><span class="sxs-lookup"><span data-stu-id="44d8a-156">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="44d8a-157">gatewayName</span><span class="sxs-lookup"><span data-stu-id="44d8a-157">gatewayName</span></span> |<span data-ttu-id="44d8a-158">Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello HDFS.</span><span class="sxs-lookup"><span data-stu-id="44d8a-158">Name of hello gateway that hello Data Factory service should use tooconnect toohello HDFS.</span></span> |<span data-ttu-id="44d8a-159">Ano</span><span class="sxs-lookup"><span data-stu-id="44d8a-159">Yes</span></span> |
| <span data-ttu-id="44d8a-160">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="44d8a-160">encryptedCredential</span></span> |<span data-ttu-id="44d8a-161">[Nové AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) výstup hello přístup pověření.</span><span class="sxs-lookup"><span data-stu-id="44d8a-161">[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of hello access credential.</span></span> |<span data-ttu-id="44d8a-162">Ne</span><span class="sxs-lookup"><span data-stu-id="44d8a-162">No</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="44d8a-163">Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="44d8a-163">Using Anonymous authentication</span></span>

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

### <a name="using-windows-authentication"></a><span data-ttu-id="44d8a-164">Pomocí ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="44d8a-164">Using Windows authentication</span></span>

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
## <a name="dataset-properties"></a><span data-ttu-id="44d8a-165">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="44d8a-165">Dataset properties</span></span>
<span data-ttu-id="44d8a-166">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="44d8a-166">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="44d8a-167">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="44d8a-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="44d8a-168">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-168">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="44d8a-169">rámci typeProperties Hello část datové sady typ **sdílení souborů** (což zahrnuje datová sada HDFS) má následující vlastnosti hello</span><span class="sxs-lookup"><span data-stu-id="44d8a-169">hello typeProperties section for dataset of type **FileShare** (which includes HDFS dataset) has hello following properties</span></span>

| <span data-ttu-id="44d8a-170">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="44d8a-170">Property</span></span> | <span data-ttu-id="44d8a-171">Popis</span><span class="sxs-lookup"><span data-stu-id="44d8a-171">Description</span></span> | <span data-ttu-id="44d8a-172">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="44d8a-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="44d8a-173">folderPath</span><span class="sxs-lookup"><span data-stu-id="44d8a-173">folderPath</span></span> |<span data-ttu-id="44d8a-174">Složka toohello cesta.</span><span class="sxs-lookup"><span data-stu-id="44d8a-174">Path toohello folder.</span></span> <span data-ttu-id="44d8a-175">Příklad:`myfolder`</span><span class="sxs-lookup"><span data-stu-id="44d8a-175">Example: `myfolder`</span></span><br/><br/><span data-ttu-id="44d8a-176">Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-176">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="44d8a-177">Příklad: pro folder\subfolder, určete složku\\\\podsložky a pro d:\samplefolder, zadejte d:\\\\ukázková_složka.</span><span class="sxs-lookup"><span data-stu-id="44d8a-177">For example: for folder\subfolder, specify folder\\\\subfolder and for d:\samplefolder, specify d:\\\\samplefolder.</span></span><br/><br/><span data-ttu-id="44d8a-178">Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="44d8a-178">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="44d8a-179">Ano</span><span class="sxs-lookup"><span data-stu-id="44d8a-179">Yes</span></span> |
| <span data-ttu-id="44d8a-180">fileName</span><span class="sxs-lookup"><span data-stu-id="44d8a-180">fileName</span></span> |<span data-ttu-id="44d8a-181">Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-181">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="44d8a-182">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-182">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="44d8a-183">Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát:</span><span class="sxs-lookup"><span data-stu-id="44d8a-183">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="44d8a-184">Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="44d8a-184">Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="44d8a-185">Ne</span><span class="sxs-lookup"><span data-stu-id="44d8a-185">No</span></span> |
| <span data-ttu-id="44d8a-186">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="44d8a-186">partitionedBy</span></span> |<span data-ttu-id="44d8a-187">partitionedBy se dá použít toospecify dynamické folderPath, název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="44d8a-187">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="44d8a-188">Příklad: folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="44d8a-188">Example: folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="44d8a-189">Ne</span><span class="sxs-lookup"><span data-stu-id="44d8a-189">No</span></span> |
| <span data-ttu-id="44d8a-190">Formát</span><span class="sxs-lookup"><span data-stu-id="44d8a-190">format</span></span> | <span data-ttu-id="44d8a-191">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="44d8a-191">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="44d8a-192">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="44d8a-192">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="44d8a-193">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="44d8a-193">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="44d8a-194">Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="44d8a-194">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="44d8a-195">Ne</span><span class="sxs-lookup"><span data-stu-id="44d8a-195">No</span></span> |
| <span data-ttu-id="44d8a-196">Komprese</span><span class="sxs-lookup"><span data-stu-id="44d8a-196">compression</span></span> | <span data-ttu-id="44d8a-197">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-197">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="44d8a-198">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="44d8a-198">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="44d8a-199">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="44d8a-199">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="44d8a-200">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="44d8a-200">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="44d8a-201">Ne</span><span class="sxs-lookup"><span data-stu-id="44d8a-201">No</span></span> |

> [!NOTE]
> <span data-ttu-id="44d8a-202">Název souboru a fileFilter nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="44d8a-202">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="44d8a-203">Pomocí vlastnosti partionedBy</span><span class="sxs-lookup"><span data-stu-id="44d8a-203">Using partionedBy property</span></span>
<span data-ttu-id="44d8a-204">Jak je uvedeno v předchozí části hello, můžete zadat dynamické folderPath a název souboru pro data časové řady s hello **partitionedBy** vlastnost [funkce pro vytváření dat a systémové proměnné hello](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="44d8a-204">As mentioned in hello previous section, you can specify a dynamic folderPath and filename for time series data with hello **partitionedBy** property, [Data Factory functions, and hello system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="44d8a-205">toolearn Další informace o datové sady času řady, plánování a řezů, najdete v části [vytváření datových sad](data-factory-create-datasets.md), [plánování a provádění](data-factory-scheduling-and-execution.md), a [vytváření kanálů](data-factory-create-pipelines.md) články.</span><span class="sxs-lookup"><span data-stu-id="44d8a-205">toolearn more about time series datasets, scheduling, and slices, see [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="44d8a-206">Příklad 1:</span><span class="sxs-lookup"><span data-stu-id="44d8a-206">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="44d8a-207">V tomto příkladu {řez} se nahradí hello hodnotu objektu pro vytváření dat systému proměnné SliceStart ve formátu hello (YYYYMMDDHH) zadán.</span><span class="sxs-lookup"><span data-stu-id="44d8a-207">In this example {Slice} is replaced with hello value of Data Factory system variable SliceStart in hello format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="44d8a-208">Hello SliceStart odkazuje toostart času řezu hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-208">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="44d8a-209">Hello folderPath se liší pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="44d8a-209">hello folderPath is different for each slice.</span></span> <span data-ttu-id="44d8a-210">Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="44d8a-210">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="44d8a-211">Příklad 2:</span><span class="sxs-lookup"><span data-stu-id="44d8a-211">Sample 2:</span></span>

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
<span data-ttu-id="44d8a-212">V tomto příkladu jsou extrahován rok, měsíc, den a čas SliceStart do samostatné proměnné, které jsou používány folderPath a název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="44d8a-212">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="44d8a-213">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="44d8a-213">Copy activity properties</span></span>
<span data-ttu-id="44d8a-214">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="44d8a-214">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="44d8a-215">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="44d8a-215">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="44d8a-216">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části hello hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="44d8a-216">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="44d8a-217">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="44d8a-217">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="44d8a-218">Pro aktivitu kopírování, pokud je zdroj typu **FileSystemSource** hello následující vlastnosti jsou k dispozici v rámci typeProperties části:</span><span class="sxs-lookup"><span data-stu-id="44d8a-218">For Copy Activity, when source is of type **FileSystemSource** hello following properties are available in typeProperties section:</span></span>

<span data-ttu-id="44d8a-219">**FileSystemSource** podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="44d8a-219">**FileSystemSource** supports hello following properties:</span></span>

| <span data-ttu-id="44d8a-220">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="44d8a-220">Property</span></span> | <span data-ttu-id="44d8a-221">Popis</span><span class="sxs-lookup"><span data-stu-id="44d8a-221">Description</span></span> | <span data-ttu-id="44d8a-222">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="44d8a-222">Allowed values</span></span> | <span data-ttu-id="44d8a-223">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="44d8a-223">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="44d8a-224">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="44d8a-224">recursive</span></span> |<span data-ttu-id="44d8a-225">Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky.</span><span class="sxs-lookup"><span data-stu-id="44d8a-225">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="44d8a-226">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="44d8a-226">True, False (default)</span></span> |<span data-ttu-id="44d8a-227">Ne</span><span class="sxs-lookup"><span data-stu-id="44d8a-227">No</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="44d8a-228">Podporované formáty souborů a komprese</span><span class="sxs-lookup"><span data-stu-id="44d8a-228">Supported file and compression formats</span></span>
<span data-ttu-id="44d8a-229">V tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článek na podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="44d8a-229">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-on-premises-hdfs-tooazure-blob"></a><span data-ttu-id="44d8a-230">Příklad JSON: kopírování dat z místní HDFS tooAzure objektů Blob</span><span class="sxs-lookup"><span data-stu-id="44d8a-230">JSON example: Copy data from on-premises HDFS tooAzure Blob</span></span>
<span data-ttu-id="44d8a-231">Tento příklad ukazuje, jak toocopy data z HDFS tooAzure místní úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="44d8a-231">This sample shows how toocopy data from an on-premises HDFS tooAzure Blob Storage.</span></span> <span data-ttu-id="44d8a-232">Však můžete zkopírovat data **přímo** tooany hello jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="44d8a-232">However, data can be copied **directly** tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="44d8a-233">Ukázka Hello poskytuje JSON definice hello následující entity služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="44d8a-233">hello sample provides JSON definitions for hello following Data Factory entities.</span></span> <span data-ttu-id="44d8a-234">Můžete použít tyto definice toocreate kanálu toocopy data z HDFS tooAzure úložiště objektů Blob pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="44d8a-234">You can use these definitions toocreate a pipeline toocopy data from HDFS tooAzure Blob Storage by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span>

1. <span data-ttu-id="44d8a-235">Propojené služby typu [OnPremisesHdfs](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="44d8a-235">A linked service of type [OnPremisesHdfs](#linked-service-properties).</span></span>
2. <span data-ttu-id="44d8a-236">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="44d8a-236">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="44d8a-237">Vstup [datovou sadu](data-factory-create-datasets.md) typu [sdílení souborů](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="44d8a-237">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
4. <span data-ttu-id="44d8a-238">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="44d8a-238">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="44d8a-239">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="44d8a-239">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="44d8a-240">Ukázka Hello zkopíruje data z HDFS tooan místní objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="44d8a-240">hello sample copies data from an on-premises HDFS tooan Azure blob every hour.</span></span> <span data-ttu-id="44d8a-241">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-241">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="44d8a-242">Jako první krok nastavte Brána pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-242">As a first step, set up hello data management gateway.</span></span> <span data-ttu-id="44d8a-243">Hello pokyny v hello [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku.</span><span class="sxs-lookup"><span data-stu-id="44d8a-243">hello instructions in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="44d8a-244">**Propojená služba HDFS:** tento příklad používá hello ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="44d8a-244">**HDFS linked service:** This example uses hello Windows authentication.</span></span> <span data-ttu-id="44d8a-245">V tématu [HDFS propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="44d8a-245">See [HDFS linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

<span data-ttu-id="44d8a-246">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="44d8a-246">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="44d8a-247">**HDFS vstupní datovou sadu:** tato datová sada odkazuje toohello HDFS složky DataTransfer/UnitTest /.</span><span class="sxs-lookup"><span data-stu-id="44d8a-247">**HDFS input dataset:** This dataset refers toohello HDFS folder DataTransfer/UnitTest/.</span></span> <span data-ttu-id="44d8a-248">kanál Hello zkopíruje všechny soubory hello v této složce toohello cíli.</span><span class="sxs-lookup"><span data-stu-id="44d8a-248">hello pipeline copies all hello files in this folder toohello destination.</span></span>

<span data-ttu-id="44d8a-249">Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-249">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="44d8a-250">**Azure Blob výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="44d8a-250">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="44d8a-251">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="44d8a-251">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="44d8a-252">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="44d8a-252">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="44d8a-253">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="44d8a-253">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="44d8a-254">**Aktivita kopírování v kanálu pomocí systému souborů zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="44d8a-254">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="44d8a-255">Hello kanál obsahuje aktivitu kopírování, je nakonfigurovaná toouse tyto vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="44d8a-255">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="44d8a-256">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**FileSystemSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="44d8a-256">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="44d8a-257">Dotaz SQL Hello zadaný pro hello **dotazu** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="44d8a-257">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

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

## <a name="use-kerberos-authentication-for-hdfs-connector"></a><span data-ttu-id="44d8a-258">Ověřování pomocí protokolu Kerberos pro HDFS konektor</span><span class="sxs-lookup"><span data-stu-id="44d8a-258">Use Kerberos authentication for HDFS connector</span></span>
<span data-ttu-id="44d8a-259">Existují dvě možnosti tooset hello místní prostředí, jako toouse ověřování protokolem Kerberos v HDFS konektoru.</span><span class="sxs-lookup"><span data-stu-id="44d8a-259">There are two options tooset up hello on-premises environment so as toouse Kerberos Authentication in HDFS connector.</span></span> <span data-ttu-id="44d8a-260">Můžete vybrat, zda text hello, jeden lépe vyhovuje váš případ.</span><span class="sxs-lookup"><span data-stu-id="44d8a-260">You can choose hello one better fits your case.</span></span>
* <span data-ttu-id="44d8a-261">Možnost 1: [připojení počítače brány ve sféře Kerberos](#kerberos-join-realm)</span><span class="sxs-lookup"><span data-stu-id="44d8a-261">Option 1: [Join gateway machine in Kerberos realm](#kerberos-join-realm)</span></span>
* <span data-ttu-id="44d8a-262">Možnost 2: [povolit vzájemné vztah důvěryhodnosti mezi doménou systému Windows a Sféra Kerberos](#kerberos-mutual-trust)</span><span class="sxs-lookup"><span data-stu-id="44d8a-262">Option 2: [Enable mutual trust between Windows domain and Kerberos realm](#kerberos-mutual-trust)</span></span>

### <span data-ttu-id="44d8a-263"><a name="kerberos-join-realm"></a>Možnost 1: Připojení počítače brány ve sféře Kerberos</span><span class="sxs-lookup"><span data-stu-id="44d8a-263"><a name="kerberos-join-realm"></a>Option 1: Join gateway machine in Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="44d8a-264">Požadavek:</span><span class="sxs-lookup"><span data-stu-id="44d8a-264">Requirement:</span></span>

* <span data-ttu-id="44d8a-265">počítač brány Hello potřebuje Sféra Kerberos hello toojoin a nemůže připojit k žádné doméně systému Windows.</span><span class="sxs-lookup"><span data-stu-id="44d8a-265">hello gateway machine needs toojoin hello Kerberos realm and can’t join any Windows domain.</span></span>

#### <a name="how-tooconfigure"></a><span data-ttu-id="44d8a-266">Jak tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="44d8a-266">How tooconfigure:</span></span>

<span data-ttu-id="44d8a-267">**Na počítači brány:**</span><span class="sxs-lookup"><span data-stu-id="44d8a-267">**On gateway machine:**</span></span>

1.  <span data-ttu-id="44d8a-268">Spustit hello **Ksetup** tooconfigure nástroj hello serveru pomocí protokolu Kerberos služby KDC a sféru.</span><span class="sxs-lookup"><span data-stu-id="44d8a-268">Run hello **Ksetup** utility tooconfigure hello Kerberos KDC server and realm.</span></span>

    <span data-ttu-id="44d8a-269">Hello počítače musí být nakonfigurován jako člena pracovní skupiny, protože Sféra Kerberos se liší od domény systému Windows.</span><span class="sxs-lookup"><span data-stu-id="44d8a-269">hello machine must be configured as a member of a workgroup since a Kerberos realm is different from a Windows domain.</span></span> <span data-ttu-id="44d8a-270">Toho lze dosáhnout nastavením Sféra Kerberos hello a takto přidejte server služby KDC.</span><span class="sxs-lookup"><span data-stu-id="44d8a-270">This can be achieved by setting hello Kerberos realm and adding a KDC server as follows.</span></span> <span data-ttu-id="44d8a-271">Nahraďte *REALM.COM* s vlastními příslušných sféry podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="44d8a-271">Replace *REALM.COM* with your own respective realm as needed.</span></span>

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    <span data-ttu-id="44d8a-272">**Restartujte** hello počítač po spuštění těchto příkazů 2.</span><span class="sxs-lookup"><span data-stu-id="44d8a-272">**Restart** hello machine after executing these 2 commands.</span></span>

2.  <span data-ttu-id="44d8a-273">Ověření konfigurace hello s **Ksetup** příkaz.</span><span class="sxs-lookup"><span data-stu-id="44d8a-273">Verify hello configuration with **Ksetup** command.</span></span> <span data-ttu-id="44d8a-274">výstup Hello by měl vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="44d8a-274">hello output should be like:</span></span>

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

<span data-ttu-id="44d8a-275">**V Azure Data Factory:**</span><span class="sxs-lookup"><span data-stu-id="44d8a-275">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="44d8a-276">Konfigurovat pomocí konektoru hello HDFS **ověřování systému Windows** společně s protokolem Kerberos hlavní název a heslo tooconnect toohello HDFS zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="44d8a-276">Configure hello HDFS connector using **Windows authentication** together with your Kerberos principal name and password tooconnect toohello HDFS data source.</span></span> <span data-ttu-id="44d8a-277">Zkontrolujte [vlastnosti propojené služby HDFS](#linked-service-properties) části na podrobnosti o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="44d8a-277">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

### <span data-ttu-id="44d8a-278"><a name="kerberos-mutual-trust"></a>Možnost 2: Povolení vzájemné vztah důvěryhodnosti mezi doménou systému Windows a Sféra Kerberos</span><span class="sxs-lookup"><span data-stu-id="44d8a-278"><a name="kerberos-mutual-trust"></a>Option 2: Enable mutual trust between Windows domain and Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="44d8a-279">Požadavek:</span><span class="sxs-lookup"><span data-stu-id="44d8a-279">Requirement:</span></span>
*   <span data-ttu-id="44d8a-280">počítač brány Hello musí připojit k doméně systému Windows.</span><span class="sxs-lookup"><span data-stu-id="44d8a-280">hello gateway machine must join a Windows domain.</span></span>
*   <span data-ttu-id="44d8a-281">Je nutné nastavení oprávnění tooupdate hello řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="44d8a-281">You need permission tooupdate hello domain controller's settings.</span></span>

#### <a name="how-tooconfigure"></a><span data-ttu-id="44d8a-282">Jak tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="44d8a-282">How tooconfigure:</span></span>

> [!NOTE]
> <span data-ttu-id="44d8a-283">V následujícím kurzu s vlastními příslušných sféry a řadič domény, podle potřeby hello nahraďte REALM.COM a AD.COM.</span><span class="sxs-lookup"><span data-stu-id="44d8a-283">Replace REALM.COM and AD.COM in hello following tutorial with your own respective realm and domain controller as needed.</span></span>

<span data-ttu-id="44d8a-284">**Na serveru služby KDC:**</span><span class="sxs-lookup"><span data-stu-id="44d8a-284">**On KDC server:**</span></span>

1.  <span data-ttu-id="44d8a-285">Upraví konfiguraci služby KDC hello v **krb5.conf** toolet souboru služby KDC vztahu důvěryhodnosti domény systému Windows odkazující toohello následující konfigurace šablony.</span><span class="sxs-lookup"><span data-stu-id="44d8a-285">Edit hello KDC configuration in **krb5.conf** file toolet KDC trust Windows Domain referring toohello following configuration template.</span></span> <span data-ttu-id="44d8a-286">Ve výchozím nastavení, konfiguraci hello nachází ve **/etc/krb5.conf**.</span><span class="sxs-lookup"><span data-stu-id="44d8a-286">By default, hello configuration is located at **/etc/krb5.conf**.</span></span>

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

  <span data-ttu-id="44d8a-287">**Restartujte** hello služba KDC po konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="44d8a-287">**Restart** hello KDC service after configuration.</span></span>

2.  <span data-ttu-id="44d8a-288">Příprava objekt zabezpečení s názvem  **krbtgt/REALM.COM@AD.COM**  na serveru služby KDC s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="44d8a-288">Prepare a principal named **krbtgt/REALM.COM@AD.COM** in KDC server with hello following command:</span></span>

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  <span data-ttu-id="44d8a-289">V **hadoop.security.auth_to_local** konfigurace služby HDFS souboru, přidejte `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span><span class="sxs-lookup"><span data-stu-id="44d8a-289">In **hadoop.security.auth_to_local** HDFS service configuration file, add `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span></span>

<span data-ttu-id="44d8a-290">**Na řadiči domény:**</span><span class="sxs-lookup"><span data-stu-id="44d8a-290">**On domain controller:**</span></span>

1.  <span data-ttu-id="44d8a-291">Spusťte následující hello **Ksetup** příkazy tooadd položku sféry:</span><span class="sxs-lookup"><span data-stu-id="44d8a-291">Run hello following **Ksetup** commands tooadd a realm entry:</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  <span data-ttu-id="44d8a-292">Navázání vztahu důvěryhodnosti z domény systému Windows tooKerberos sféry.</span><span class="sxs-lookup"><span data-stu-id="44d8a-292">Establish trust from Windows Domain tooKerberos Realm.</span></span> <span data-ttu-id="44d8a-293">[heslo] je hello heslo pro objekt zabezpečení hello  **krbtgt/REALM.COM@AD.COM** .</span><span class="sxs-lookup"><span data-stu-id="44d8a-293">[password] is hello password for hello principal **krbtgt/REALM.COM@AD.COM**.</span></span>

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  <span data-ttu-id="44d8a-294">Vyberte šifrovací algoritmus použitý v protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="44d8a-294">Select encryption algorithm used in Kerberos.</span></span>

    1. <span data-ttu-id="44d8a-295">Přejděte tooServer Manager > Správa zásad skupiny > domény > objekty zásad skupiny > výchozí nebo aktivní zásady domény a úpravy.</span><span class="sxs-lookup"><span data-stu-id="44d8a-295">Go tooServer Manager > Group Policy Management > Domain > Group Policy Objects > Default or Active Domain Policy, and Edit.</span></span>

    2. <span data-ttu-id="44d8a-296">V hello **Editor správy zásad skupiny** automaticky otevíraném okně, přejděte tooComputer konfigurace > zásady > Nastavení systému Windows > Nastavení zabezpečení > Místní zásady > Možnosti zabezpečení a nakonfigurujte **sítě zabezpečení: Konfigurace typy šifrování, které jsou povoleny pro protokol Kerberos**.</span><span class="sxs-lookup"><span data-stu-id="44d8a-296">In hello **Group Policy Management Editor** popup window, go tooComputer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options, and configure **Network security: Configure Encryption types allowed for Kerberos**.</span></span>

    3. <span data-ttu-id="44d8a-297">Vyberte hello šifrovací algoritmus toouse chcete při připojení tooKDC.</span><span class="sxs-lookup"><span data-stu-id="44d8a-297">Select hello encryption algorithm you want toouse when connect tooKDC.</span></span> <span data-ttu-id="44d8a-298">Běžně můžete jednoduše vybrat všechny možnosti hello.</span><span class="sxs-lookup"><span data-stu-id="44d8a-298">Commonly, you can simply select all hello options.</span></span>

        ![Typy šifrování konfigurace pro protokol Kerberos](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. <span data-ttu-id="44d8a-300">Použití **Ksetup** příkaz toospecify hello šifrovací algoritmus toobe použít na hello konkrétní SFÉRY.</span><span class="sxs-lookup"><span data-stu-id="44d8a-300">Use **Ksetup** command toospecify hello encryption algorithm toobe used on hello specific REALM.</span></span>

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  <span data-ttu-id="44d8a-301">Vytvořte objekt, v pořadí toouse Kerberos hlavní v doméně systému Windows hello mapování mezi hello doménový účet a protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="44d8a-301">Create hello mapping between hello domain account and Kerberos principal, in order toouse Kerberos principal in Windows Domain.</span></span>

    1. <span data-ttu-id="44d8a-302">Spuštění nástroje pro správu hello > **Active Directory Users and Computers**.</span><span class="sxs-lookup"><span data-stu-id="44d8a-302">Start hello Administrative tools > **Active Directory Users and Computers**.</span></span>

    2. <span data-ttu-id="44d8a-303">Konfigurace rozšířených funkcí kliknutím **zobrazení** > **pokročilé funkce**.</span><span class="sxs-lookup"><span data-stu-id="44d8a-303">Configure advanced features by clicking **View** > **Advanced Features**.</span></span>

    3. <span data-ttu-id="44d8a-304">Vyhledejte hello účet toowhich chcete toocreate mapování a klikněte pravým tlačítkem na tooview **mapování názvů** > klikněte na tlačítko **Kerberos názvy** kartě.</span><span class="sxs-lookup"><span data-stu-id="44d8a-304">Locate hello account toowhich you want toocreate mappings, and right-click tooview **Name Mappings** > click **Kerberos Names** tab.</span></span>

    4. <span data-ttu-id="44d8a-305">Přidáte objekt z hello sféry.</span><span class="sxs-lookup"><span data-stu-id="44d8a-305">Add a principal from hello realm.</span></span>

        ![Mapování Identity zabezpečení](media/data-factory-hdfs-connector/map-security-identity.png)

<span data-ttu-id="44d8a-307">**Na počítači brány:**</span><span class="sxs-lookup"><span data-stu-id="44d8a-307">**On gateway machine:**</span></span>

* <span data-ttu-id="44d8a-308">Spusťte následující hello **Ksetup** příkazy tooadd položku sféry.</span><span class="sxs-lookup"><span data-stu-id="44d8a-308">Run hello following **Ksetup** commands tooadd a realm entry.</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

<span data-ttu-id="44d8a-309">**V Azure Data Factory:**</span><span class="sxs-lookup"><span data-stu-id="44d8a-309">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="44d8a-310">Konfigurovat pomocí konektoru hello HDFS **ověřování systému Windows** společně s účet domény nebo zdroj dat HDFS toohello tooconnect objekt zabezpečení protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="44d8a-310">Configure hello HDFS connector using **Windows authentication** together with either your Domain Account or Kerberos Principal tooconnect toohello HDFS data source.</span></span> <span data-ttu-id="44d8a-311">Zkontrolujte [vlastnosti propojené služby HDFS](#linked-service-properties) části na podrobnosti o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="44d8a-311">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

> [!NOTE]
> <span data-ttu-id="44d8a-312">toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="44d8a-312">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="performance-and-tuning"></a><span data-ttu-id="44d8a-313">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="44d8a-313">Performance and Tuning</span></span>
<span data-ttu-id="44d8a-314">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="44d8a-314">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
