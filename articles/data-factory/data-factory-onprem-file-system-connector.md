---
title: "Kopírování dat do nebo ze systému souborů pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak ke zkopírování dat do a ze v místním systému souborů pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
ms.openlocfilehash: 52305e54f539de6aba2ba9cc856a09e04d608ded
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="copy-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a><span data-ttu-id="63b0f-103">Kopírování dat do a ze v místním systému souborů pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="63b0f-103">Copy data to and from an on-premises file system by using Azure Data Factory</span></span>
<span data-ttu-id="63b0f-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z v místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="63b0f-104">This article explains how to use the Copy Activity in Azure Data Factory to copy data to/from an on-premises file system.</span></span> <span data-ttu-id="63b0f-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="63b0f-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="63b0f-106">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="63b0f-106">Supported scenarios</span></span>
<span data-ttu-id="63b0f-107">Může kopírovat data **ze systému souborů na místě** ukládá do následující data:</span><span class="sxs-lookup"><span data-stu-id="63b0f-107">You can copy data **from an on-premises file system** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="63b0f-108">Může kopírovat data z následujících datových úložišť **na systém souborů místní**:</span><span class="sxs-lookup"><span data-stu-id="63b0f-108">You can copy data from the following data stores **to an on-premises file system**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> <span data-ttu-id="63b0f-109">Aktivita kopírování nedojde k odstranění zdrojového souboru po byl úspěšně zkopírován do cílové.</span><span class="sxs-lookup"><span data-stu-id="63b0f-109">Copy Activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="63b0f-110">Pokud potřebujete odstranit zdrojový soubor po úspěšné kopie, vytvořte vlastní aktivity odstranit soubor a použijte aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-110">If you need to delete the source file after a successful copy, create a custom activity to delete the file and use the activity in the pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="63b0f-111">Povolení připojení</span><span class="sxs-lookup"><span data-stu-id="63b0f-111">Enabling connectivity</span></span>
<span data-ttu-id="63b0f-112">Objekt pro vytváření dat podporuje připojení do a ze v místním systému souborů prostřednictvím **Brána pro správu dat**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-112">Data Factory supports connecting to and from an on-premises file system via **Data Management Gateway**.</span></span> <span data-ttu-id="63b0f-113">Brána pro správu dat je nutné nainstalovat v prostředí místní služby Data Factory pro připojení k úložišti všechny podporované místní data, včetně systému souborů.</span><span class="sxs-lookup"><span data-stu-id="63b0f-113">You must install the Data Management Gateway in your on-premises environment for the Data Factory service to connect to any supported on-premises data store including file system.</span></span> <span data-ttu-id="63b0f-114">Další informace o Brána pro správu dat a podrobné pokyny k nastavení brány najdete v tématu [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="63b0f-114">To learn about Data Management Gateway and for step-by-step instructions on setting up the gateway, see [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="63b0f-115">Kromě Brána pro správu dat musíte nainstalovat komunikace do a ze v místním systému souborů žádné binární soubory.</span><span class="sxs-lookup"><span data-stu-id="63b0f-115">Apart from Data Management Gateway, no other binary files need to be installed to communicate to and from an on-premises file system.</span></span> <span data-ttu-id="63b0f-116">Musíte nainstalovat a použít Brána pro správu dat, i když je systém souborů ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="63b0f-116">You must install and use the Data Management Gateway even if the file system is in Azure IaaS VM.</span></span> <span data-ttu-id="63b0f-117">Podrobné informace o bráně najdete v tématu [Brána pro správu dat](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="63b0f-117">For detailed information about the gateway, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>

<span data-ttu-id="63b0f-118">Chcete-li použít sdílenou složku systému Linux, nainstalovat [Samba](https://www.samba.org/) na Linux server a instalaci brány pro správu dat v systému Windows server.</span><span class="sxs-lookup"><span data-stu-id="63b0f-118">To use a Linux file share, install [Samba](https://www.samba.org/) on your Linux server, and install Data Management Gateway on a Windows server.</span></span> <span data-ttu-id="63b0f-119">Instalace brány pro správu dat na Linux server není podporována.</span><span class="sxs-lookup"><span data-stu-id="63b0f-119">Installing Data Management Gateway on a Linux server is not supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="63b0f-120">Začínáme</span><span class="sxs-lookup"><span data-stu-id="63b0f-120">Getting started</span></span>
<span data-ttu-id="63b0f-121">Vytvoření kanálu s aktivitou kopírování, který přesouvá data ze systému souborů pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="63b0f-121">You can create a pipeline with a copy activity that moves data to/from a file system by using different tools/APIs.</span></span>

<span data-ttu-id="63b0f-122">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-122">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="63b0f-123">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="63b0f-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="63b0f-124">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-124">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="63b0f-125">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="63b0f-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="63b0f-126">Jestli používáte nástroje nebo rozhraní API, je třeba provést následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="63b0f-126">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="63b0f-127">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-127">Create a **data factory**.</span></span> <span data-ttu-id="63b0f-128">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="63b0f-128">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="63b0f-129">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="63b0f-129">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="63b0f-130">Pokud jsou kopírování dat z Azure blob storage na systém souborů na místě, například vytvoříte dvě propojené služby k propojení vaší místní systém souborů a účet úložiště Azure pro vytváření dat..</span><span class="sxs-lookup"><span data-stu-id="63b0f-130">For example, if you are copying data from an Azure blob storage to an on-premises file system, you create two linked services to link your on-premises file system and Azure storage account to your data factory.</span></span> <span data-ttu-id="63b0f-131">Vlastnosti propojené služby, které jsou specifické pro systém souborů na místě, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="63b0f-131">For linked service properties that are specific to an on-premises file system, see [linked service properties](#linked-service-properties) section.</span></span>
3. <span data-ttu-id="63b0f-132">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="63b0f-132">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="63b0f-133">V příkladu uvedených v posledním kroku vytvoříte datové sady a zadat kontejner objektů blob a složky, která obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="63b0f-133">In the example mentioned in the last step, you create a dataset to specify the blob container and folder that contains the input data.</span></span> <span data-ttu-id="63b0f-134">A vytvořte jinou datovou sadu, která určete složku a název souboru (volitelné) v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="63b0f-134">And, you create another dataset to specify the folder and file name (optional) in your file system.</span></span> <span data-ttu-id="63b0f-135">Vlastnosti datové sady, které jsou specifické pro systém souborů na místě, najdete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="63b0f-135">For dataset properties that are specific to on-premises file system, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="63b0f-136">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="63b0f-136">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="63b0f-137">V příkladu již bylo zmíněno dříve použijete BlobSource jako zdroj a FileSystemSink jako jímku pro aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="63b0f-137">In the example mentioned earlier, you use BlobSource as a source and FileSystemSink as a sink for the copy activity.</span></span> <span data-ttu-id="63b0f-138">Podobně pokud kopírujete z místní systém souborů do úložiště objektů Blob Azure, můžete použít FileSystemSource a BlobSink v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="63b0f-138">Similarly, if you are copying from on-premises file system to Azure Blob Storage, you use FileSystemSource and BlobSink in the copy activity.</span></span> <span data-ttu-id="63b0f-139">Kopírovat vlastnosti aktivity, které jsou specifické pro systém souborů na místě, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="63b0f-139">For copy activity properties that are specific to on-premises file system, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="63b0f-140">Podrobnosti o tom, jak používat úložiště dat jako zdroj nebo jímka klikněte na odkaz v předchozí části pro data store.</span><span class="sxs-lookup"><span data-stu-id="63b0f-140">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span>

<span data-ttu-id="63b0f-141">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="63b0f-141">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="63b0f-142">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="63b0f-142">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="63b0f-143">Ukázky s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat do nebo ze systému souborů naleznete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-file-system) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="63b0f-143">For samples with JSON definitions for Data Factory entities that are used to copy data to/from a file system, see [JSON examples](#json-examples-for-copying-data-to-and-from-file-system) section of this article.</span></span>

<span data-ttu-id="63b0f-144">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které slouží k určení entit služby Data Factory, které jsou specifické pro systém souborů:</span><span class="sxs-lookup"><span data-stu-id="63b0f-144">The following sections provide details about JSON properties that are used to define Data Factory entities specific to file system:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="63b0f-145">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="63b0f-145">Linked service properties</span></span>
<span data-ttu-id="63b0f-146">Systém souborů na místě můžete propojit s objektem pro vytváření dat Azure s **místní souborový Server** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="63b0f-146">You can link an on-premises file system to an Azure data factory with the **On-Premises File Server** linked service.</span></span> <span data-ttu-id="63b0f-147">Následující tabulka obsahuje popis elementy JSON, které jsou specifické pro službu propojené místní souborový Server.</span><span class="sxs-lookup"><span data-stu-id="63b0f-147">The following table provides descriptions for JSON elements that are specific to the On-Premises File Server linked service.</span></span>

| <span data-ttu-id="63b0f-148">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="63b0f-148">Property</span></span> | <span data-ttu-id="63b0f-149">Popis</span><span class="sxs-lookup"><span data-stu-id="63b0f-149">Description</span></span> | <span data-ttu-id="63b0f-150">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="63b0f-150">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="63b0f-151">type</span><span class="sxs-lookup"><span data-stu-id="63b0f-151">type</span></span> |<span data-ttu-id="63b0f-152">Ujistěte se, že je vlastnost Typ nastavena **OnPremisesFileServer**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-152">Ensure that the type property is set to **OnPremisesFileServer**.</span></span> |<span data-ttu-id="63b0f-153">Ano</span><span class="sxs-lookup"><span data-stu-id="63b0f-153">Yes</span></span> |
| <span data-ttu-id="63b0f-154">hostitele</span><span class="sxs-lookup"><span data-stu-id="63b0f-154">host</span></span> |<span data-ttu-id="63b0f-155">Určuje cestu kořenové složky, kterou chcete zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="63b0f-155">Specifies the root path of the folder that you want to copy.</span></span> <span data-ttu-id="63b0f-156">Použít řídicí znak ' \ ' pro speciální znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="63b0f-156">Use the escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="63b0f-157">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="63b0f-157">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span> |<span data-ttu-id="63b0f-158">Ano</span><span class="sxs-lookup"><span data-stu-id="63b0f-158">Yes</span></span> |
| <span data-ttu-id="63b0f-159">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="63b0f-159">userid</span></span> |<span data-ttu-id="63b0f-160">Zadejte ID uživatele, který má přístup k serveru.</span><span class="sxs-lookup"><span data-stu-id="63b0f-160">Specify the ID of the user who has access to the server.</span></span> |<span data-ttu-id="63b0f-161">Ne (když zvolíte encryptedCredential)</span><span class="sxs-lookup"><span data-stu-id="63b0f-161">No (if you choose encryptedCredential)</span></span> |
| <span data-ttu-id="63b0f-162">heslo</span><span class="sxs-lookup"><span data-stu-id="63b0f-162">password</span></span> |<span data-ttu-id="63b0f-163">Zadejte heslo pro uživatele (ID uživatele).</span><span class="sxs-lookup"><span data-stu-id="63b0f-163">Specify the password for the user (userid).</span></span> |<span data-ttu-id="63b0f-164">Ne (když zvolíte encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="63b0f-164">No (if you choose encryptedCredential</span></span> |
| <span data-ttu-id="63b0f-165">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="63b0f-165">encryptedCredential</span></span> |<span data-ttu-id="63b0f-166">Zadejte zašifrované přihlašovací údaje, které můžete získat spuštěním rutiny New-AzureRmDataFactoryEncryptValue.</span><span class="sxs-lookup"><span data-stu-id="63b0f-166">Specify the encrypted credentials that you can get by running the New-AzureRmDataFactoryEncryptValue cmdlet.</span></span> |<span data-ttu-id="63b0f-167">Ne (když zvolíte možnost zadat ID uživatele a heslo ve formátu prostého textu)</span><span class="sxs-lookup"><span data-stu-id="63b0f-167">No (if you choose to specify userid and password in plain text)</span></span> |
| <span data-ttu-id="63b0f-168">gatewayName</span><span class="sxs-lookup"><span data-stu-id="63b0f-168">gatewayName</span></span> |<span data-ttu-id="63b0f-169">Určuje název brány, kterou Data Factory měla použít pro připojení k souborovému serveru místně.</span><span class="sxs-lookup"><span data-stu-id="63b0f-169">Specifies the name of the gateway that Data Factory should use to connect to the on-premises file server.</span></span> |<span data-ttu-id="63b0f-170">Ano</span><span class="sxs-lookup"><span data-stu-id="63b0f-170">Yes</span></span> |


### <a name="sample-linked-service-and-dataset-definitions"></a><span data-ttu-id="63b0f-171">Ukázkové propojené služby a definice datové sady</span><span class="sxs-lookup"><span data-stu-id="63b0f-171">Sample linked service and dataset definitions</span></span>
| <span data-ttu-id="63b0f-172">Scénář</span><span class="sxs-lookup"><span data-stu-id="63b0f-172">Scenario</span></span> | <span data-ttu-id="63b0f-173">Hostování v definici propojené služby</span><span class="sxs-lookup"><span data-stu-id="63b0f-173">Host in linked service definition</span></span> | <span data-ttu-id="63b0f-174">folderPath v definici datové sady</span><span class="sxs-lookup"><span data-stu-id="63b0f-174">folderPath in dataset definition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="63b0f-175">Místní složky v počítači brány pro správu dat:</span><span class="sxs-lookup"><span data-stu-id="63b0f-175">Local folder on Data Management Gateway machine:</span></span> <br/><br/><span data-ttu-id="63b0f-176">Příklady: D:\\ \* nebo D:\folder\subfolder\\*</span><span class="sxs-lookup"><span data-stu-id="63b0f-176">Examples: D:\\\* or D:\folder\subfolder\\*</span></span> |<span data-ttu-id="63b0f-177">D:\\ \\ (pro Data Management Gateway 2.0 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="63b0f-177">D:\\\\ (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/> <span data-ttu-id="63b0f-178">localhost (pro starší verze než Data Management Gateway 2.0)</span><span class="sxs-lookup"><span data-stu-id="63b0f-178">localhost (for earlier versions than Data Management Gateway 2.0)</span></span> |<span data-ttu-id="63b0f-179">. \\ \\ nebo složky\\\\podsložky (pro Data Management Gateway 2.0 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="63b0f-179">.\\\\ or folder\\\\subfolder (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/><span data-ttu-id="63b0f-180">D:\\ \\ nebo D:\\\\složky\\\\podsložky (pro brány verzi nižší než 2.0)</span><span class="sxs-lookup"><span data-stu-id="63b0f-180">D:\\\\ or D:\\\\folder\\\\subfolder (for gateway version below 2.0)</span></span> |
| <span data-ttu-id="63b0f-181">Vzdálené sdílené složce:</span><span class="sxs-lookup"><span data-stu-id="63b0f-181">Remote shared folder:</span></span> <br/><br/><span data-ttu-id="63b0f-182">Příklady: \\ \\myserver\\sdílet\\ \* nebo \\ \\myserver\\sdílet\\složky\\podsložky\\*</span><span class="sxs-lookup"><span data-stu-id="63b0f-182">Examples: \\\\myserver\\share\\\* or \\\\myserver\\share\\folder\\subfolder\\*</span></span> |<span data-ttu-id="63b0f-183">\\\\\\\\myserver\\\\sdílet</span><span class="sxs-lookup"><span data-stu-id="63b0f-183">\\\\\\\\myserver\\\\share</span></span> |<span data-ttu-id="63b0f-184">. \\ \\ nebo složky\\\\podsložky</span><span class="sxs-lookup"><span data-stu-id="63b0f-184">.\\\\ or folder\\\\subfolder</span></span> |


### <a name="example-using-username-and-password-in-plain-text"></a><span data-ttu-id="63b0f-185">Příklad: Pomocí uživatelského jména a hesla ve formátu prostého textu</span><span class="sxs-lookup"><span data-stu-id="63b0f-185">Example: Using username and password in plain text</span></span>

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

### <a name="example-using-encryptedcredential"></a><span data-ttu-id="63b0f-186">Příklad: Pomocí encryptedcredential</span><span class="sxs-lookup"><span data-stu-id="63b0f-186">Example: Using encryptedcredential</span></span>

```JSON
{
  "Name": " OnPremisesFileServerLinkedService ",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "D:\\",
      "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
      "gatewayName": "mygateway"
    }
  }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="63b0f-187">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="63b0f-187">Dataset properties</span></span>
<span data-ttu-id="63b0f-188">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části [vytváření datových sad](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="63b0f-188">For a full list of sections and properties that are available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="63b0f-189">Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="63b0f-190">V rámci typeProperties části se liší pro jednotlivé typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-190">The typeProperties section is different for each type of dataset.</span></span> <span data-ttu-id="63b0f-191">Poskytuje informace, jako je například umístění a formát dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="63b0f-191">It provides information such as the location and format of the data in the data store.</span></span> <span data-ttu-id="63b0f-192">Rámci typeProperties část datové sady typ **sdílení souborů** má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="63b0f-192">The typeProperties section for the dataset of type **FileShare** has the following properties:</span></span>

| <span data-ttu-id="63b0f-193">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="63b0f-193">Property</span></span> | <span data-ttu-id="63b0f-194">Popis</span><span class="sxs-lookup"><span data-stu-id="63b0f-194">Description</span></span> | <span data-ttu-id="63b0f-195">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="63b0f-195">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="63b0f-196">folderPath</span><span class="sxs-lookup"><span data-stu-id="63b0f-196">folderPath</span></span> |<span data-ttu-id="63b0f-197">Určuje dílčí cestou ke složce.</span><span class="sxs-lookup"><span data-stu-id="63b0f-197">Specifies the subpath to the folder.</span></span> <span data-ttu-id="63b0f-198">Použít řídicí znak ' \' pro speciální znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="63b0f-198">Use the escape character ‘\’ for special characters in the string.</span></span> <span data-ttu-id="63b0f-199">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="63b0f-199">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="63b0f-200">Tato vlastnost se můžete kombinovat **partitionBy** tak, aby měl složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="63b0f-200">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="63b0f-201">Ano</span><span class="sxs-lookup"><span data-stu-id="63b0f-201">Yes</span></span> |
| <span data-ttu-id="63b0f-202">fileName</span><span class="sxs-lookup"><span data-stu-id="63b0f-202">fileName</span></span> |<span data-ttu-id="63b0f-203">Zadejte název souboru do **folderPath** Pokud chcete, aby v tabulce odkazovat na konkrétní soubor ve složce.</span><span class="sxs-lookup"><span data-stu-id="63b0f-203">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="63b0f-204">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, tabulka odkazuje na všechny soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="63b0f-204">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="63b0f-205">Když **fileName** pro datovou sadu výstupů není zadána a **preserveHierarchy** není zadané v aktivity podřízený název vygenerovaný soubor je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="63b0f-205">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, the name of the generated file is in the following format:</span></span> <br/><br/><span data-ttu-id="63b0f-206">`Data.<Guid>.txt`(Příklad: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="63b0f-206">`Data.<Guid>.txt` (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="63b0f-207">Ne</span><span class="sxs-lookup"><span data-stu-id="63b0f-207">No</span></span> |
| <span data-ttu-id="63b0f-208">fileFilter</span><span class="sxs-lookup"><span data-stu-id="63b0f-208">fileFilter</span></span> |<span data-ttu-id="63b0f-209">Zadejte filtr pro umožňuje vybrat podmnožinu souborů v folderPath, nikoli všech souborů.</span><span class="sxs-lookup"><span data-stu-id="63b0f-209">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span> <br/><br/><span data-ttu-id="63b0f-210">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="63b0f-210">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="63b0f-211">Příklad 1: "fileFilter": "* .log"</span><span class="sxs-lookup"><span data-stu-id="63b0f-211">Example 1: "fileFilter": "*.log"</span></span><br/><span data-ttu-id="63b0f-212">Příklad 2: "fileFilter": 2014 - 1-?. TXT"</span><span class="sxs-lookup"><span data-stu-id="63b0f-212">Example 2: "fileFilter": 2014-1-?.txt"</span></span><br/><br/><span data-ttu-id="63b0f-213">Všimněte si, že fileFilter je použít pro datové sadě služby vstupní sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="63b0f-213">Note that fileFilter is applicable for an input FileShare dataset.</span></span> |<span data-ttu-id="63b0f-214">Ne</span><span class="sxs-lookup"><span data-stu-id="63b0f-214">No</span></span> |
| <span data-ttu-id="63b0f-215">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="63b0f-215">partitionedBy</span></span> |<span data-ttu-id="63b0f-216">PartitionedBy můžete použít k určení dynamické folderPath nebo název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="63b0f-216">You can use partitionedBy to specify a dynamic folderPath/fileName for time-series data.</span></span> <span data-ttu-id="63b0f-217">Příkladem je folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="63b0f-217">An example is folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="63b0f-218">Ne</span><span class="sxs-lookup"><span data-stu-id="63b0f-218">No</span></span> |
| <span data-ttu-id="63b0f-219">Formát</span><span class="sxs-lookup"><span data-stu-id="63b0f-219">format</span></span> | <span data-ttu-id="63b0f-220">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-220">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="63b0f-221">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="63b0f-221">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="63b0f-222">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="63b0f-222">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="63b0f-223">Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="63b0f-223">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="63b0f-224">Ne</span><span class="sxs-lookup"><span data-stu-id="63b0f-224">No</span></span> |
| <span data-ttu-id="63b0f-225">Komprese</span><span class="sxs-lookup"><span data-stu-id="63b0f-225">compression</span></span> | <span data-ttu-id="63b0f-226">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="63b0f-226">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="63b0f-227">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-227">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="63b0f-228">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-228">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="63b0f-229">v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="63b0f-229">see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="63b0f-230">Ne</span><span class="sxs-lookup"><span data-stu-id="63b0f-230">No</span></span> |

> [!NOTE]
> <span data-ttu-id="63b0f-231">Název souboru a fileFilter nelze současně použít.</span><span class="sxs-lookup"><span data-stu-id="63b0f-231">You cannot use fileName and fileFilter simultaneously.</span></span>

### <a name="using-partitionedby-property"></a><span data-ttu-id="63b0f-232">Pomocí vlastnost partitionedBy</span><span class="sxs-lookup"><span data-stu-id="63b0f-232">Using partitionedBy property</span></span>
<span data-ttu-id="63b0f-233">Jak je uvedeno v předchozí části, můžete zadat dynamické folderPath a název souboru pro data časové řady s **partitionedBy** vlastnost [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="63b0f-233">As mentioned in the previous section, you can specify a dynamic folderPath and filename for time series data with the **partitionedBy** property, [Data Factory functions, and the system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="63b0f-234">Zjistit další informace o datových sad časové řady, plánování a řezů, najdete v části [vytváření datových sad](data-factory-create-datasets.md), [plánování a provádění](data-factory-scheduling-and-execution.md), a [vytváření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="63b0f-234">To understand more details on time-series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="63b0f-235">Příklad 1:</span><span class="sxs-lookup"><span data-stu-id="63b0f-235">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="63b0f-236">V tomto příkladu {řez} se nahradí hodnoty proměnné objektu pro vytváření dat systému SliceStart ve formátu (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="63b0f-236">In this example, {Slice} is replaced with the value of the Data Factory system variable SliceStart in the format (YYYYMMDDHH).</span></span> <span data-ttu-id="63b0f-237">SliceStart odkazuje na spuštění řezu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-237">SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="63b0f-238">FolderPath se liší pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="63b0f-238">The folderPath is different for each slice.</span></span> <span data-ttu-id="63b0f-239">Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="63b0f-239">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="63b0f-240">Příklad 2:</span><span class="sxs-lookup"><span data-stu-id="63b0f-240">Sample 2:</span></span>

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

<span data-ttu-id="63b0f-241">V tomto příkladu se extrahují rok, měsíc, den a čas SliceStart do samostatné proměnných, které používají vlastnosti folderPath a název souboru.</span><span class="sxs-lookup"><span data-stu-id="63b0f-241">In this example, year, month, day, and time of SliceStart are extracted into separate variables that the folderPath and fileName properties use.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="63b0f-242">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="63b0f-242">Copy activity properties</span></span>
<span data-ttu-id="63b0f-243">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="63b0f-243">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="63b0f-244">Vlastnosti, například název, popis, vstupní a výstupní datové sady a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="63b0f-244">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span> <span data-ttu-id="63b0f-245">Vzhledem k tomu, vlastnosti dostupné ve **rámci typeProperties** části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="63b0f-245">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span>

<span data-ttu-id="63b0f-246">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="63b0f-246">For Copy activity, they vary depending on the types of sources and sinks.</span></span> <span data-ttu-id="63b0f-247">Pokud přesouváte data ze v místním systému souborů, nastavíte typ zdroje v aktivitě kopírování do **FileSystemSource**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-247">If you are moving data from an on-premises file system, you set the source type in the copy activity to **FileSystemSource**.</span></span> <span data-ttu-id="63b0f-248">Podobně pokud přesouváte data na systém souborů na místě, nastavíte typ jímky v aktivitě kopírování do **FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-248">Similarly, if you are moving data to an on-premises file system, you set the sink type in the copy activity to **FileSystemSink**.</span></span> <span data-ttu-id="63b0f-249">Tato část obsahuje seznam vlastností, které jsou podporované FileSystemSource a FileSystemSink.</span><span class="sxs-lookup"><span data-stu-id="63b0f-249">This section provides a list of properties supported by FileSystemSource and FileSystemSink.</span></span>

<span data-ttu-id="63b0f-250">**FileSystemSource** podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="63b0f-250">**FileSystemSource** supports the following properties:</span></span>

| <span data-ttu-id="63b0f-251">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="63b0f-251">Property</span></span> | <span data-ttu-id="63b0f-252">Popis</span><span class="sxs-lookup"><span data-stu-id="63b0f-252">Description</span></span> | <span data-ttu-id="63b0f-253">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="63b0f-253">Allowed values</span></span> | <span data-ttu-id="63b0f-254">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="63b0f-254">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="63b0f-255">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="63b0f-255">recursive</span></span> |<span data-ttu-id="63b0f-256">Označuje, zda je data načíst rekurzivně z podsložky nebo pouze do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="63b0f-256">Indicates whether the data is read recursively from the subfolders or only from the specified folder.</span></span> |<span data-ttu-id="63b0f-257">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="63b0f-257">True, False (default)</span></span> |<span data-ttu-id="63b0f-258">Ne</span><span class="sxs-lookup"><span data-stu-id="63b0f-258">No</span></span> |

<span data-ttu-id="63b0f-259">**FileSystemSink** podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="63b0f-259">**FileSystemSink** supports the following properties:</span></span>

| <span data-ttu-id="63b0f-260">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="63b0f-260">Property</span></span> | <span data-ttu-id="63b0f-261">Popis</span><span class="sxs-lookup"><span data-stu-id="63b0f-261">Description</span></span> | <span data-ttu-id="63b0f-262">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="63b0f-262">Allowed values</span></span> | <span data-ttu-id="63b0f-263">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="63b0f-263">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="63b0f-264">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="63b0f-264">copyBehavior</span></span> |<span data-ttu-id="63b0f-265">Definuje chování kopie, pokud je zdroj BlobSource nebo systému souborů.</span><span class="sxs-lookup"><span data-stu-id="63b0f-265">Defines the copy behavior when the source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="63b0f-266">**PreserveHierarchy:** zachovává hierarchii souborů v cílové složce.</span><span class="sxs-lookup"><span data-stu-id="63b0f-266">**PreserveHierarchy:** Preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="63b0f-267">To znamená relativní cesta zdrojového souboru do zdrojové složky je stejný jako relativní cestu k souboru cíl k cílové složce.</span><span class="sxs-lookup"><span data-stu-id="63b0f-267">That is, the relative path of the source file to the source folder is the same as the relative path of the target file to the target folder.</span></span><br/><br/><span data-ttu-id="63b0f-268">**FlattenHierarchy:** všechny soubory ze zdrojové složky jsou vytvořené v první úroveň cílové složce.</span><span class="sxs-lookup"><span data-stu-id="63b0f-268">**FlattenHierarchy:** All files from the source folder are created in the first level of target folder.</span></span> <span data-ttu-id="63b0f-269">Cílové soubory jsou vytvořeny pomocí názvu objektu generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="63b0f-269">The target files are created with an autogenerated name.</span></span><br/><br/><span data-ttu-id="63b0f-270">**MergeFiles:** slučuje všechny soubory ze zdrojové složky pro jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="63b0f-270">**MergeFiles:** Merges all files from the source folder to one file.</span></span> <span data-ttu-id="63b0f-271">Pokud je zadán název nebo objekt blob název souboru, název souboru sloučené je zadaný název.</span><span class="sxs-lookup"><span data-stu-id="63b0f-271">If the file name/blob name is specified, the merged file name is the specified name.</span></span> <span data-ttu-id="63b0f-272">Jinak je název automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="63b0f-272">Otherwise, it is an auto-generated file name.</span></span> |<span data-ttu-id="63b0f-273">Ne</span><span class="sxs-lookup"><span data-stu-id="63b0f-273">No</span></span> |

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="63b0f-274">Příklady rekurzivní a copyBehavior</span><span class="sxs-lookup"><span data-stu-id="63b0f-274">recursive and copyBehavior examples</span></span>
<span data-ttu-id="63b0f-275">Tato část popisuje jejich výsledné chování pro různé kombinace hodnot vlastností rekurzivní a copyBehavior operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="63b0f-275">This section describes the resulting behavior of the Copy operation for different combinations of values for the recursive and copyBehavior properties.</span></span>

| <span data-ttu-id="63b0f-276">rekurzivní hodnota</span><span class="sxs-lookup"><span data-stu-id="63b0f-276">recursive value</span></span> | <span data-ttu-id="63b0f-277">Hodnota copyBehavior</span><span class="sxs-lookup"><span data-stu-id="63b0f-277">copyBehavior value</span></span> | <span data-ttu-id="63b0f-278">Výsledné chování</span><span class="sxs-lookup"><span data-stu-id="63b0f-278">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="63b0f-279">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="63b0f-279">true</span></span> |<span data-ttu-id="63b0f-280">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="63b0f-280">preserveHierarchy</span></span> |<span data-ttu-id="63b0f-281">Pro zdrojovou složku složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="63b0f-281">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="63b0f-282">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-282">Folder1</span></span><br/><span data-ttu-id="63b0f-283">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="63b0f-283">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="63b0f-284">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="63b0f-284">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="63b0f-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="63b0f-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="63b0f-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="63b0f-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="63b0f-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="63b0f-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="63b0f-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="63b0f-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="63b0f-289">cílové složky složku1 je vytvořen s stejná struktura jako zdroj:</span><span class="sxs-lookup"><span data-stu-id="63b0f-289">the target folder Folder1 is created with the same structure as the source:</span></span><br/><br/><span data-ttu-id="63b0f-290">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-290">Folder1</span></span><br/><span data-ttu-id="63b0f-291">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="63b0f-291">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="63b0f-292">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="63b0f-292">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="63b0f-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="63b0f-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="63b0f-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="63b0f-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="63b0f-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="63b0f-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="63b0f-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="63b0f-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span> |
| <span data-ttu-id="63b0f-297">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="63b0f-297">true</span></span> |<span data-ttu-id="63b0f-298">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="63b0f-298">flattenHierarchy</span></span> |<span data-ttu-id="63b0f-299">Pro zdrojovou složku složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="63b0f-299">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="63b0f-300">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-300">Folder1</span></span><br/><span data-ttu-id="63b0f-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="63b0f-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="63b0f-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="63b0f-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="63b0f-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="63b0f-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="63b0f-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="63b0f-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="63b0f-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="63b0f-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="63b0f-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="63b0f-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="63b0f-307">cíl složku1 je vytvořen s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="63b0f-307">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="63b0f-308">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-308">Folder1</span></span><br/><span data-ttu-id="63b0f-309">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="63b0f-309">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="63b0f-310">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="63b0f-310">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="63b0f-311">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název soubor3</span><span class="sxs-lookup"><span data-stu-id="63b0f-311">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="63b0f-312">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File4</span><span class="sxs-lookup"><span data-stu-id="63b0f-312">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="63b0f-313">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File5</span><span class="sxs-lookup"><span data-stu-id="63b0f-313">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="63b0f-314">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="63b0f-314">true</span></span> |<span data-ttu-id="63b0f-315">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="63b0f-315">mergeFiles</span></span> |<span data-ttu-id="63b0f-316">Pro zdrojovou složku složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="63b0f-316">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="63b0f-317">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-317">Folder1</span></span><br/><span data-ttu-id="63b0f-318">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="63b0f-318">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="63b0f-319">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="63b0f-319">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="63b0f-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="63b0f-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="63b0f-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="63b0f-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="63b0f-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="63b0f-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="63b0f-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="63b0f-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="63b0f-324">cíl složku1 je vytvořen s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="63b0f-324">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="63b0f-325">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-325">Folder1</span></span><br/><span data-ttu-id="63b0f-326">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + soubor3 + File4 + soubor 5 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="63b0f-326">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with an auto-generated file name.</span></span> |
| <span data-ttu-id="63b0f-327">False</span><span class="sxs-lookup"><span data-stu-id="63b0f-327">false</span></span> |<span data-ttu-id="63b0f-328">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="63b0f-328">preserveHierarchy</span></span> |<span data-ttu-id="63b0f-329">Pro zdrojovou složku složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="63b0f-329">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="63b0f-330">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-330">Folder1</span></span><br/><span data-ttu-id="63b0f-331">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="63b0f-331">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="63b0f-332">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="63b0f-332">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="63b0f-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="63b0f-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="63b0f-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="63b0f-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="63b0f-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="63b0f-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="63b0f-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="63b0f-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="63b0f-337">cílové složky složku1 je vytvořen s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="63b0f-337">the target folder Folder1 is created with the following structure:</span></span><br/><br/><span data-ttu-id="63b0f-338">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-338">Folder1</span></span><br/><span data-ttu-id="63b0f-339">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="63b0f-339">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="63b0f-340">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="63b0f-340">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><span data-ttu-id="63b0f-341">Subfolder1 s soubor3, File4 a File5 není zachyceny.</span><span class="sxs-lookup"><span data-stu-id="63b0f-341">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="63b0f-342">False</span><span class="sxs-lookup"><span data-stu-id="63b0f-342">false</span></span> |<span data-ttu-id="63b0f-343">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="63b0f-343">flattenHierarchy</span></span> |<span data-ttu-id="63b0f-344">Pro zdrojovou složku složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="63b0f-344">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="63b0f-345">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-345">Folder1</span></span><br/><span data-ttu-id="63b0f-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="63b0f-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="63b0f-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="63b0f-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="63b0f-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="63b0f-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="63b0f-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="63b0f-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="63b0f-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="63b0f-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="63b0f-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="63b0f-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="63b0f-352">cílové složky složku1 je vytvořen s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="63b0f-352">the target folder Folder1 is created with the following structure:</span></span><br/><br/><span data-ttu-id="63b0f-353">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-353">Folder1</span></span><br/><span data-ttu-id="63b0f-354">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="63b0f-354">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="63b0f-355">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="63b0f-355">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><span data-ttu-id="63b0f-356">Subfolder1 s soubor3, File4 a File5 není zachyceny.</span><span class="sxs-lookup"><span data-stu-id="63b0f-356">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="63b0f-357">False</span><span class="sxs-lookup"><span data-stu-id="63b0f-357">false</span></span> |<span data-ttu-id="63b0f-358">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="63b0f-358">mergeFiles</span></span> |<span data-ttu-id="63b0f-359">Pro zdrojovou složku složku1 s následující strukturou</span><span class="sxs-lookup"><span data-stu-id="63b0f-359">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="63b0f-360">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-360">Folder1</span></span><br/><span data-ttu-id="63b0f-361">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="63b0f-361">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="63b0f-362">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="63b0f-362">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="63b0f-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="63b0f-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="63b0f-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="63b0f-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="63b0f-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="63b0f-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="63b0f-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="63b0f-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="63b0f-367">cílové složky složku1 je vytvořen s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="63b0f-367">the target folder Folder1 is created with the following structure:</span></span><br/><br/><span data-ttu-id="63b0f-368">Složku1</span><span class="sxs-lookup"><span data-stu-id="63b0f-368">Folder1</span></span><br/><span data-ttu-id="63b0f-369">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="63b0f-369">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with an auto-generated file name.</span></span><br/><span data-ttu-id="63b0f-370">&nbsp;&nbsp;&nbsp;&nbsp;Automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="63b0f-370">&nbsp;&nbsp;&nbsp;&nbsp;Auto-generated name for File1</span></span><br/><br/><span data-ttu-id="63b0f-371">Subfolder1 s soubor3, File4 a File5 není zachyceny.</span><span class="sxs-lookup"><span data-stu-id="63b0f-371">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="63b0f-372">Podporované formáty souborů a komprese</span><span class="sxs-lookup"><span data-stu-id="63b0f-372">Supported file and compression formats</span></span>
<span data-ttu-id="63b0f-373">V tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článek na podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="63b0f-373">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples-for-copying-data-to-and-from-file-system"></a><span data-ttu-id="63b0f-374">Příklady JSON pro kopírování dat do a ze systému souborů</span><span class="sxs-lookup"><span data-stu-id="63b0f-374">JSON examples for copying data to and from file system</span></span>
<span data-ttu-id="63b0f-375">Následující příklady poskytují ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="63b0f-375">The following examples provide sample JSON definitions that you can use to create a pipeline by using the [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="63b0f-376">Se ukazují, jak ke zkopírování dat do a z v místním systému souborů a úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="63b0f-376">They show how to copy data to and from an on-premises file system and Azure Blob storage.</span></span> <span data-ttu-id="63b0f-377">Však může kopírovat data *přímo* ze všech zdrojů do jakéhokoli z jímky uvedené v [podporované zdroje a jímky](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="63b0f-377">However, you can copy data *directly* from any of the sources to any of the sinks listed in [Supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a><span data-ttu-id="63b0f-378">Příklad: Kopírování dat z v místním systému souborů do úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="63b0f-378">Example: Copy data from an on-premises file system to Azure Blob storage</span></span>
<span data-ttu-id="63b0f-379">Tento příklad ukazuje, jak ke kopírování dat ze v místním systému souborů do úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="63b0f-379">This sample shows how to copy data from an on-premises file system to Azure Blob storage.</span></span> <span data-ttu-id="63b0f-380">Ukázka má následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="63b0f-380">The sample has the following Data Factory entities:</span></span>

* <span data-ttu-id="63b0f-381">Propojené služby typu [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="63b0f-381">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="63b0f-382">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="63b0f-382">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="63b0f-383">Vstup [datovou sadu](data-factory-create-datasets.md) typu [sdílení souborů](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="63b0f-383">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="63b0f-384">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="63b0f-384">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="63b0f-385">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="63b0f-385">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="63b0f-386">Následující příklad zkopíruje data časové řady ze v místním systému souborů do úložiště objektů Blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-386">The following sample copies time-series data from an on-premises file system to Azure Blob storage every hour.</span></span> <span data-ttu-id="63b0f-387">Vlastnosti JSON, které se používají v tyto ukázky jsou popsané v částech po ukázky.</span><span class="sxs-lookup"><span data-stu-id="63b0f-387">The JSON properties that are used in these samples are described in the sections after the samples.</span></span>

<span data-ttu-id="63b0f-388">Jako první krok, nastavit Brána pro správu dat podle pokynů v [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="63b0f-388">As a first step, set up Data Management Gateway as per the instructions in [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span>

<span data-ttu-id="63b0f-389">**Souborový Server místní propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="63b0f-389">**On-Premises File Server linked service:**</span></span>

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

<span data-ttu-id="63b0f-390">Doporučujeme používat **encryptedCredential** vlastnost místo toho **userid** a **heslo** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="63b0f-390">We recommend using the **encryptedCredential** property instead the **userid** and **password** properties.</span></span> <span data-ttu-id="63b0f-391">V tématu [souborového serveru propojená služba](#linked-service-properties) podrobnosti o této propojené službě.</span><span class="sxs-lookup"><span data-stu-id="63b0f-391">See [File Server linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="63b0f-392">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="63b0f-392">**Azure Storage linked service:**</span></span>

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

<span data-ttu-id="63b0f-393">**Místní soubor vstupní datové sady systému:**</span><span class="sxs-lookup"><span data-stu-id="63b0f-393">**On-premises file system input dataset:**</span></span>

<span data-ttu-id="63b0f-394">Data je převzata z nový soubor každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-394">Data is picked up from a new file every hour.</span></span> <span data-ttu-id="63b0f-395">Vlastnosti folderPath a název souboru jsou určeny podle času zahájení řezu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-395">The folderPath and fileName properties are determined based on the start time of the slice.</span></span>  

<span data-ttu-id="63b0f-396">Nastavení `"external": "true"` informuje objekt pro vytváření dat, datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="63b0f-396">Setting `"external": "true"` informs Data Factory that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "OnpremisesFileSystemInput",
  "properties": {
    "type": " FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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

<span data-ttu-id="63b0f-397">**Azure Blob storage výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="63b0f-397">**Azure Blob storage output dataset:**</span></span>

<span data-ttu-id="63b0f-398">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="63b0f-398">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="63b0f-399">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="63b0f-399">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="63b0f-400">Cesta ke složce používá rok, měsíc, den a hodina části čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="63b0f-400">The folder path uses the year, month, day, and hour parts of the start time.</span></span>

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="63b0f-401">**Aktivita kopírování v kanálu pomocí systému souborů zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="63b0f-401">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="63b0f-402">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-402">The pipeline contains a copy activity that is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="63b0f-403">V definici JSON kanálu **zdroj** je typ nastaven na **FileSystemSource**, a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-403">In the pipeline JSON definition, the **source** type is set to **FileSystemSource**, and **sink** type is set to **BlobSink**.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T19:00:00",
    "description":"Pipeline for copy activity",
    "activities":[  
      {
        "name": "OnpremisesFileSystemtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "OnpremisesFileSystemInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "FileSystemSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```

### <a name="example-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a><span data-ttu-id="63b0f-404">Příklad: Kopírování dat z databáze Azure SQL Database na systém souborů na místě</span><span class="sxs-lookup"><span data-stu-id="63b0f-404">Example: Copy data from Azure SQL Database to an on-premises file system</span></span>
<span data-ttu-id="63b0f-405">Následující příklad ukazuje:</span><span class="sxs-lookup"><span data-stu-id="63b0f-405">The following sample shows:</span></span>

* <span data-ttu-id="63b0f-406">Propojené služby typu [azuresqldatabase..](data-factory-azure-sql-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="63b0f-406">A linked service of type [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="63b0f-407">Propojené služby typu [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="63b0f-407">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="63b0f-408">Vstupní datové sady typu [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="63b0f-408">An input dataset of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="63b0f-409">Výstupní datové typu [sdílení souborů](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="63b0f-409">An output dataset of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="63b0f-410">Kanál s aktivitou kopírování, která používá [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) a [FileSystemSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="63b0f-410">A pipeline with a copy activity that uses [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) and [FileSystemSink](#copy-activity-properties).</span></span>

<span data-ttu-id="63b0f-411">Ukázka kopíruje data časové řady z tabulky Azure SQL na systém souborů na místě každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-411">The sample copies time-series data from an Azure SQL table to an on-premises file system every hour.</span></span> <span data-ttu-id="63b0f-412">Vlastnosti JSON, které se používají v tyto ukázky jsou popsané v částech po ukázky.</span><span class="sxs-lookup"><span data-stu-id="63b0f-412">The JSON properties that are used in these samples are described in sections after the samples.</span></span>

<span data-ttu-id="63b0f-413">**Azure SQL Database propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="63b0f-413">**Azure SQL Database linked service:**</span></span>

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```

<span data-ttu-id="63b0f-414">**Souborový Server místní propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="63b0f-414">**On-Premises File Server linked service:**</span></span>

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

<span data-ttu-id="63b0f-415">Doporučujeme používat **encryptedCredential** vlastnost místo použití **userid** a **heslo** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="63b0f-415">We recommend using the **encryptedCredential** property instead of using the **userid** and **password** properties.</span></span> <span data-ttu-id="63b0f-416">V tématu [systém souborů propojená služba](#linked-service-properties) podrobnosti o této propojené službě.</span><span class="sxs-lookup"><span data-stu-id="63b0f-416">See [File System linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="63b0f-417">**Azure SQL vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="63b0f-417">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="63b0f-418">Ukázka předpokládá, že jste vytvořili tabulku "MyTable" v Azure SQL, a obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="63b0f-418">The sample assumes that you've created a table “MyTable” in Azure SQL, and it contains a column called “timestampcolumn” for time-series data.</span></span>

<span data-ttu-id="63b0f-419">Nastavení ``“external”: ”true”`` informuje objekt pro vytváření dat, datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="63b0f-419">Setting ``“external”: ”true”`` informs Data Factory that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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

<span data-ttu-id="63b0f-420">**Místní soubor výstupní datovou sadu systému:**</span><span class="sxs-lookup"><span data-stu-id="63b0f-420">**On-premises file system output dataset:**</span></span>

<span data-ttu-id="63b0f-421">Data se zkopírují do nového souboru každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-421">Data is copied to a new file every hour.</span></span> <span data-ttu-id="63b0f-422">FolderPath a název souboru pro tento objekt blob jsou určeny podle času zahájení řezu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-422">The folderPath and fileName for the blob are determined based on the start time of the slice.</span></span>

```JSON
{
  "name": "OnpremisesFileSystemOutput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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

<span data-ttu-id="63b0f-423">**Aktivita kopírování v kanálu s SQL zdroj a jímka systému souborů:**</span><span class="sxs-lookup"><span data-stu-id="63b0f-423">**A copy activity in a pipeline with SQL source and File System sink:**</span></span>

<span data-ttu-id="63b0f-424">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="63b0f-424">The pipeline contains a copy activity that is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="63b0f-425">V definici JSON kanálu **zdroj** je typ nastaven na **SqlSource**a **podřízený** je typ nastaven na **FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="63b0f-425">In the pipeline JSON definition, the **source** type is set to **SqlSource**, and the **sink** type is set to **FileSystemSink**.</span></span> <span data-ttu-id="63b0f-426">Příkaz jazyka SQL, která je určená pro **SqlReaderQuery** vlastnost vybere data za poslední hodinu pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="63b0f-426">The SQL query that is specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T20:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoOnPremisesFile",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "OnpremisesFileSystemOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "FileSystemSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 3,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```


<span data-ttu-id="63b0f-427">Můžete také mapovat sloupců z datové sady zdroje na sloupce ze sady jímku dat v definici aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="63b0f-427">You can also map columns from source dataset to columns from sink dataset in the copy activity definition.</span></span> <span data-ttu-id="63b0f-428">Podrobnosti najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="63b0f-428">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="63b0f-429">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="63b0f-429">Performance and tuning</span></span>
 <span data-ttu-id="63b0f-430">Další informace o klíčových faktorů, které mít dopad na výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho najdete v tématu [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="63b0f-430">To learn about key factors that impact the performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it, see the [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>
