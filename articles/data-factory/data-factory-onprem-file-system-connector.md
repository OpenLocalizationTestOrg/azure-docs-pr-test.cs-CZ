---
title: "aaaCopy data do nebo ze systému souborů pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak toocopy tooand data ze v místním systému souborů pomocí Azure Data Factory."
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
ms.openlocfilehash: 201b8bc3ffa639df781443aa0c3f95c975d280be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-an-on-premises-file-system-by-using-azure-data-factory"></a><span data-ttu-id="43845-103">Kopírovat data tooand ze v místním systému souborů pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="43845-103">Copy data tooand from an on-premises file system by using Azure Data Factory</span></span>
<span data-ttu-id="43845-104">Tento článek vysvětluje, jak toouse hello aktivitu kopírování v Azure Data Factory toocopy data ze v místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="43845-104">This article explains how toouse hello Copy Activity in Azure Data Factory toocopy data to/from an on-premises file system.</span></span> <span data-ttu-id="43845-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="43845-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="43845-106">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="43845-106">Supported scenarios</span></span>
<span data-ttu-id="43845-107">Může kopírovat data **ze systému souborů na místě** toohello následující úložišť dat:</span><span class="sxs-lookup"><span data-stu-id="43845-107">You can copy data **from an on-premises file system** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="43845-108">Data můžete zkopírovat z hello následující úložišť dat **tooan místní systém souborů**:</span><span class="sxs-lookup"><span data-stu-id="43845-108">You can copy data from hello following data stores **tooan on-premises file system**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> <span data-ttu-id="43845-109">Aktivita kopírování neodstraní hello zdrojový soubor po úspěšně zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="43845-109">Copy Activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="43845-110">Pokud potřebujete toodelete hello zdrojový soubor po úspěšné kopie, vytvořte soubor vlastní aktivity toodelete hello a používat hello aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="43845-110">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file and use hello activity in hello pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="43845-111">Povolení připojení</span><span class="sxs-lookup"><span data-stu-id="43845-111">Enabling connectivity</span></span>
<span data-ttu-id="43845-112">Objekt pro vytváření dat podporuje připojování tooand ze v místním systému souborů prostřednictvím **Brána pro správu dat**.</span><span class="sxs-lookup"><span data-stu-id="43845-112">Data Factory supports connecting tooand from an on-premises file system via **Data Management Gateway**.</span></span> <span data-ttu-id="43845-113">Je nutné nainstalovat hello Brána pro správu dat ve vašem prostředí místní pro hello Data Factory služby tooconnect tooany podporované místní úložiště dat včetně systému souborů.</span><span class="sxs-lookup"><span data-stu-id="43845-113">You must install hello Data Management Gateway in your on-premises environment for hello Data Factory service tooconnect tooany supported on-premises data store including file system.</span></span> <span data-ttu-id="43845-114">toolearn o Brána pro správu dat a podrobné pokyny k nastavení hello brány, najdete v části [přesun dat mezi místní zdroje a hello cloudu s Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="43845-114">toolearn about Data Management Gateway and for step-by-step instructions on setting up hello gateway, see [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="43845-115">Kromě Brána pro správu dat je třeba tooand toocommunicate toobe nainstalovat ze v místním systému souborů žádné binární soubory.</span><span class="sxs-lookup"><span data-stu-id="43845-115">Apart from Data Management Gateway, no other binary files need toobe installed toocommunicate tooand from an on-premises file system.</span></span> <span data-ttu-id="43845-116">Musíte nainstalovat a použít hello Brána pro správu dat, i když je systém souborů hello ve virtuálním počítači Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="43845-116">You must install and use hello Data Management Gateway even if hello file system is in Azure IaaS VM.</span></span> <span data-ttu-id="43845-117">Podrobné informace o bráně hello najdete v tématu [Brána pro správu dat](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="43845-117">For detailed information about hello gateway, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>

<span data-ttu-id="43845-118">Nainstalujte toouse sdílenou složku systému Linux, [Samba](https://www.samba.org/) na Linux server a instalaci brány pro správu dat v systému Windows server.</span><span class="sxs-lookup"><span data-stu-id="43845-118">toouse a Linux file share, install [Samba](https://www.samba.org/) on your Linux server, and install Data Management Gateway on a Windows server.</span></span> <span data-ttu-id="43845-119">Instalace brány pro správu dat na Linux server není podporována.</span><span class="sxs-lookup"><span data-stu-id="43845-119">Installing Data Management Gateway on a Linux server is not supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="43845-120">Začínáme</span><span class="sxs-lookup"><span data-stu-id="43845-120">Getting started</span></span>
<span data-ttu-id="43845-121">Vytvoření kanálu s aktivitou kopírování, který přesouvá data ze systému souborů pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="43845-121">You can create a pipeline with a copy activity that moves data to/from a file system by using different tools/APIs.</span></span>

<span data-ttu-id="43845-122">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="43845-122">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="43845-123">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="43845-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="43845-124">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="43845-124">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="43845-125">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="43845-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="43845-126">Jestli používáte nástroje hello nebo rozhraní API, je třeba provést následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="43845-126">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="43845-127">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="43845-127">Create a **data factory**.</span></span> <span data-ttu-id="43845-128">Objekt pro vytváření dat může obsahovat jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="43845-128">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="43845-129">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="43845-129">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="43845-130">Například pokud data kopírujete ze systému souborů místní tooan úložiště objektů blob v Azure, vytvoříte dvě propojené služby toolink v místním systému souborů a účet úložiště Azure tooyour služby data factory.</span><span class="sxs-lookup"><span data-stu-id="43845-130">For example, if you are copying data from an Azure blob storage tooan on-premises file system, you create two linked services toolink your on-premises file system and Azure storage account tooyour data factory.</span></span> <span data-ttu-id="43845-131">Vlastnosti propojené služby, které jsou specifické tooan v místním systému souborů, najdete v části [propojené vlastnosti služby](#linked-service-properties) části.</span><span class="sxs-lookup"><span data-stu-id="43845-131">For linked service properties that are specific tooan on-premises file system, see [linked service properties](#linked-service-properties) section.</span></span>
3. <span data-ttu-id="43845-132">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="43845-132">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="43845-133">V příkladu hello uvedených v posledním kroku hello vytvořte kontejner objektů blob hello toospecify datovou sadu a složky, která obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="43845-133">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="43845-134">A vytvoříte jinou datovou sadu toospecify hello složku a název souboru (volitelné) v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="43845-134">And, you create another dataset toospecify hello folder and file name (optional) in your file system.</span></span> <span data-ttu-id="43845-135">Vlastnosti datové sady, které jsou specifické tooon místní systém souborů, najdete v části [vlastnosti datové sady](#dataset-properties) části.</span><span class="sxs-lookup"><span data-stu-id="43845-135">For dataset properties that are specific tooon-premises file system, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="43845-136">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="43845-136">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="43845-137">V příkladu hello již bylo zmíněno dříve použijete BlobSource jako zdroj a FileSystemSink jako jímku pro aktivitu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="43845-137">In hello example mentioned earlier, you use BlobSource as a source and FileSystemSink as a sink for hello copy activity.</span></span> <span data-ttu-id="43845-138">Podobně pokud zkopírujete z místního souboru systému tooAzure úložiště objektů Blob, použijte FileSystemSource a BlobSink v aktivitě kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="43845-138">Similarly, if you are copying from on-premises file system tooAzure Blob Storage, you use FileSystemSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="43845-139">Kopie aktivity vlastnosti, které jsou specifické tooon místní systém souborů, najdete v části [zkopírovat vlastnosti aktivity](#copy-activity-properties) části.</span><span class="sxs-lookup"><span data-stu-id="43845-139">For copy activity properties that are specific tooon-premises file system, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="43845-140">Podrobnosti o způsobu toouse úložiště dat jako zdroj nebo jímka klikněte na tlačítko hello odkaz v předchozí části hello pro data store.</span><span class="sxs-lookup"><span data-stu-id="43845-140">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>

<span data-ttu-id="43845-141">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="43845-141">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="43845-142">Při použití nástroje nebo rozhraní API (s výjimkou .NET API), můžete definovat tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="43845-142">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="43845-143">Ukázky s definicemi JSON entit služby Data Factory, které jsou používané toocopy data do nebo ze systému souborů naleznete v části [JSON příklady](#json-examples-for-copying-data-to-and-from-file-system) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="43845-143">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from a file system, see [JSON examples](#json-examples-for-copying-data-to-and-from-file-system) section of this article.</span></span>

<span data-ttu-id="43845-144">Hello následující části obsahují podrobnosti o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní toofile systému:</span><span class="sxs-lookup"><span data-stu-id="43845-144">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific toofile system:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="43845-145">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="43845-145">Linked service properties</span></span>
<span data-ttu-id="43845-146">Můžete se propojit místní soubor systému tooan pro vytváření dat Azure s hello **místní souborový Server** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="43845-146">You can link an on-premises file system tooan Azure data factory with hello **On-Premises File Server** linked service.</span></span> <span data-ttu-id="43845-147">Hello následující tabulka obsahuje popis JSON prvky, které jsou specifické toohello místní souborový Server propojené služby.</span><span class="sxs-lookup"><span data-stu-id="43845-147">hello following table provides descriptions for JSON elements that are specific toohello On-Premises File Server linked service.</span></span>

| <span data-ttu-id="43845-148">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="43845-148">Property</span></span> | <span data-ttu-id="43845-149">Popis</span><span class="sxs-lookup"><span data-stu-id="43845-149">Description</span></span> | <span data-ttu-id="43845-150">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="43845-150">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="43845-151">type</span><span class="sxs-lookup"><span data-stu-id="43845-151">type</span></span> |<span data-ttu-id="43845-152">Ujistěte se, že hello vlastnost Typ nastavena příliš**OnPremisesFileServer**.</span><span class="sxs-lookup"><span data-stu-id="43845-152">Ensure that hello type property is set too**OnPremisesFileServer**.</span></span> |<span data-ttu-id="43845-153">Ano</span><span class="sxs-lookup"><span data-stu-id="43845-153">Yes</span></span> |
| <span data-ttu-id="43845-154">hostitele</span><span class="sxs-lookup"><span data-stu-id="43845-154">host</span></span> |<span data-ttu-id="43845-155">Určuje hello kořenovou cestu hello složky, které chcete toocopy.</span><span class="sxs-lookup"><span data-stu-id="43845-155">Specifies hello root path of hello folder that you want toocopy.</span></span> <span data-ttu-id="43845-156">Použít hello řídicí znak ' \ ' pro speciální znaky v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="43845-156">Use hello escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="43845-157">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="43845-157">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span> |<span data-ttu-id="43845-158">Ano</span><span class="sxs-lookup"><span data-stu-id="43845-158">Yes</span></span> |
| <span data-ttu-id="43845-159">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="43845-159">userid</span></span> |<span data-ttu-id="43845-160">Zadejte ID hello hello uživatele, který má přístup toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="43845-160">Specify hello ID of hello user who has access toohello server.</span></span> |<span data-ttu-id="43845-161">Ne (když zvolíte encryptedCredential)</span><span class="sxs-lookup"><span data-stu-id="43845-161">No (if you choose encryptedCredential)</span></span> |
| <span data-ttu-id="43845-162">heslo</span><span class="sxs-lookup"><span data-stu-id="43845-162">password</span></span> |<span data-ttu-id="43845-163">Zadejte hello heslo pro uživatele hello (ID uživatele).</span><span class="sxs-lookup"><span data-stu-id="43845-163">Specify hello password for hello user (userid).</span></span> |<span data-ttu-id="43845-164">Ne (když zvolíte encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="43845-164">No (if you choose encryptedCredential</span></span> |
| <span data-ttu-id="43845-165">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="43845-165">encryptedCredential</span></span> |<span data-ttu-id="43845-166">Zadejte hello šifrovat přihlašovací údaje, které můžete získat spuštěním rutiny New-AzureRmDataFactoryEncryptValue hello.</span><span class="sxs-lookup"><span data-stu-id="43845-166">Specify hello encrypted credentials that you can get by running hello New-AzureRmDataFactoryEncryptValue cmdlet.</span></span> |<span data-ttu-id="43845-167">Ne (když zvolíte toospecify ID uživatele a heslo ve formátu prostého textu)</span><span class="sxs-lookup"><span data-stu-id="43845-167">No (if you choose toospecify userid and password in plain text)</span></span> |
| <span data-ttu-id="43845-168">gatewayName</span><span class="sxs-lookup"><span data-stu-id="43845-168">gatewayName</span></span> |<span data-ttu-id="43845-169">Určuje název hello hello brány, že objekt pro vytváření dat mají používat tooconnect toohello místní souborový server.</span><span class="sxs-lookup"><span data-stu-id="43845-169">Specifies hello name of hello gateway that Data Factory should use tooconnect toohello on-premises file server.</span></span> |<span data-ttu-id="43845-170">Ano</span><span class="sxs-lookup"><span data-stu-id="43845-170">Yes</span></span> |


### <a name="sample-linked-service-and-dataset-definitions"></a><span data-ttu-id="43845-171">Ukázkové propojené služby a definice datové sady</span><span class="sxs-lookup"><span data-stu-id="43845-171">Sample linked service and dataset definitions</span></span>
| <span data-ttu-id="43845-172">Scénář</span><span class="sxs-lookup"><span data-stu-id="43845-172">Scenario</span></span> | <span data-ttu-id="43845-173">Hostování v definici propojené služby</span><span class="sxs-lookup"><span data-stu-id="43845-173">Host in linked service definition</span></span> | <span data-ttu-id="43845-174">folderPath v definici datové sady</span><span class="sxs-lookup"><span data-stu-id="43845-174">folderPath in dataset definition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="43845-175">Místní složky v počítači brány pro správu dat:</span><span class="sxs-lookup"><span data-stu-id="43845-175">Local folder on Data Management Gateway machine:</span></span> <br/><br/><span data-ttu-id="43845-176">Příklady: D:\\ \* nebo D:\folder\subfolder\\*</span><span class="sxs-lookup"><span data-stu-id="43845-176">Examples: D:\\\* or D:\folder\subfolder\\*</span></span> |<span data-ttu-id="43845-177">D:\\ \\ (pro Data Management Gateway 2.0 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="43845-177">D:\\\\ (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/> <span data-ttu-id="43845-178">localhost (pro starší verze než Data Management Gateway 2.0)</span><span class="sxs-lookup"><span data-stu-id="43845-178">localhost (for earlier versions than Data Management Gateway 2.0)</span></span> |<span data-ttu-id="43845-179">. \\ \\ nebo složky\\\\podsložky (pro Data Management Gateway 2.0 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="43845-179">.\\\\ or folder\\\\subfolder (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/><span data-ttu-id="43845-180">D:\\ \\ nebo D:\\\\složky\\\\podsložky (pro brány verzi nižší než 2.0)</span><span class="sxs-lookup"><span data-stu-id="43845-180">D:\\\\ or D:\\\\folder\\\\subfolder (for gateway version below 2.0)</span></span> |
| <span data-ttu-id="43845-181">Vzdálené sdílené složce:</span><span class="sxs-lookup"><span data-stu-id="43845-181">Remote shared folder:</span></span> <br/><br/><span data-ttu-id="43845-182">Příklady: \\ \\myserver\\sdílet\\ \* nebo \\ \\myserver\\sdílet\\složky\\podsložky\\*</span><span class="sxs-lookup"><span data-stu-id="43845-182">Examples: \\\\myserver\\share\\\* or \\\\myserver\\share\\folder\\subfolder\\*</span></span> |<span data-ttu-id="43845-183">\\\\\\\\myserver\\\\sdílet</span><span class="sxs-lookup"><span data-stu-id="43845-183">\\\\\\\\myserver\\\\share</span></span> |<span data-ttu-id="43845-184">. \\ \\ nebo složky\\\\podsložky</span><span class="sxs-lookup"><span data-stu-id="43845-184">.\\\\ or folder\\\\subfolder</span></span> |


### <a name="example-using-username-and-password-in-plain-text"></a><span data-ttu-id="43845-185">Příklad: Pomocí uživatelského jména a hesla ve formátu prostého textu</span><span class="sxs-lookup"><span data-stu-id="43845-185">Example: Using username and password in plain text</span></span>

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

### <a name="example-using-encryptedcredential"></a><span data-ttu-id="43845-186">Příklad: Pomocí encryptedcredential</span><span class="sxs-lookup"><span data-stu-id="43845-186">Example: Using encryptedcredential</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="43845-187">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="43845-187">Dataset properties</span></span>
<span data-ttu-id="43845-188">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části [vytváření datových sad](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="43845-188">For a full list of sections and properties that are available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="43845-189">Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="43845-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="43845-190">část rámci typeProperties Hello se liší pro jednotlivé typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="43845-190">hello typeProperties section is different for each type of dataset.</span></span> <span data-ttu-id="43845-191">Poskytuje informace, jako je například umístění hello a formát hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="43845-191">It provides information such as hello location and format of hello data in hello data store.</span></span> <span data-ttu-id="43845-192">rámci typeProperties Hello části pro datovou sadu hello typu **sdílení souborů** má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="43845-192">hello typeProperties section for hello dataset of type **FileShare** has hello following properties:</span></span>

| <span data-ttu-id="43845-193">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="43845-193">Property</span></span> | <span data-ttu-id="43845-194">Popis</span><span class="sxs-lookup"><span data-stu-id="43845-194">Description</span></span> | <span data-ttu-id="43845-195">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="43845-195">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="43845-196">folderPath</span><span class="sxs-lookup"><span data-stu-id="43845-196">folderPath</span></span> |<span data-ttu-id="43845-197">Určuje složku toohello cestou hello.</span><span class="sxs-lookup"><span data-stu-id="43845-197">Specifies hello subpath toohello folder.</span></span> <span data-ttu-id="43845-198">Použít hello řídicí znak ' \' pro speciální znaky v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="43845-198">Use hello escape character ‘\’ for special characters in hello string.</span></span> <span data-ttu-id="43845-199">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="43845-199">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="43845-200">Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="43845-200">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="43845-201">Ano</span><span class="sxs-lookup"><span data-stu-id="43845-201">Yes</span></span> |
| <span data-ttu-id="43845-202">fileName</span><span class="sxs-lookup"><span data-stu-id="43845-202">fileName</span></span> |<span data-ttu-id="43845-203">Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="43845-203">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="43845-204">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="43845-204">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="43845-205">Když **fileName** pro datovou sadu výstupů není zadána a **preserveHierarchy** není zadané v aktivity podřízený hello název souboru hello generované se hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="43845-205">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, hello name of hello generated file is in hello following format:</span></span> <br/><br/><span data-ttu-id="43845-206">`Data.<Guid>.txt`(Příklad: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="43845-206">`Data.<Guid>.txt` (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="43845-207">Ne</span><span class="sxs-lookup"><span data-stu-id="43845-207">No</span></span> |
| <span data-ttu-id="43845-208">fileFilter</span><span class="sxs-lookup"><span data-stu-id="43845-208">fileFilter</span></span> |<span data-ttu-id="43845-209">Zadejte že filtr toobe používá tooselect podmnožinu souborů v hello folderPath, nikoli všech souborů.</span><span class="sxs-lookup"><span data-stu-id="43845-209">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span> <br/><br/><span data-ttu-id="43845-210">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="43845-210">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="43845-211">Příklad 1: "fileFilter": "* .log"</span><span class="sxs-lookup"><span data-stu-id="43845-211">Example 1: "fileFilter": "*.log"</span></span><br/><span data-ttu-id="43845-212">Příklad 2: "fileFilter": 2014 - 1-?. TXT"</span><span class="sxs-lookup"><span data-stu-id="43845-212">Example 2: "fileFilter": 2014-1-?.txt"</span></span><br/><br/><span data-ttu-id="43845-213">Všimněte si, že fileFilter je použít pro datové sadě služby vstupní sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="43845-213">Note that fileFilter is applicable for an input FileShare dataset.</span></span> |<span data-ttu-id="43845-214">Ne</span><span class="sxs-lookup"><span data-stu-id="43845-214">No</span></span> |
| <span data-ttu-id="43845-215">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="43845-215">partitionedBy</span></span> |<span data-ttu-id="43845-216">Můžete vytvořit partitionedBy toospecify dynamické folderPath nebo název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="43845-216">You can use partitionedBy toospecify a dynamic folderPath/fileName for time-series data.</span></span> <span data-ttu-id="43845-217">Příkladem je folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="43845-217">An example is folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="43845-218">Ne</span><span class="sxs-lookup"><span data-stu-id="43845-218">No</span></span> |
| <span data-ttu-id="43845-219">Formát</span><span class="sxs-lookup"><span data-stu-id="43845-219">format</span></span> | <span data-ttu-id="43845-220">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="43845-220">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="43845-221">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="43845-221">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="43845-222">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="43845-222">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="43845-223">Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="43845-223">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="43845-224">Ne</span><span class="sxs-lookup"><span data-stu-id="43845-224">No</span></span> |
| <span data-ttu-id="43845-225">Komprese</span><span class="sxs-lookup"><span data-stu-id="43845-225">compression</span></span> | <span data-ttu-id="43845-226">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="43845-226">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="43845-227">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="43845-227">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="43845-228">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="43845-228">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="43845-229">v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="43845-229">see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="43845-230">Ne</span><span class="sxs-lookup"><span data-stu-id="43845-230">No</span></span> |

> [!NOTE]
> <span data-ttu-id="43845-231">Název souboru a fileFilter nelze současně použít.</span><span class="sxs-lookup"><span data-stu-id="43845-231">You cannot use fileName and fileFilter simultaneously.</span></span>

### <a name="using-partitionedby-property"></a><span data-ttu-id="43845-232">Pomocí vlastnost partitionedBy</span><span class="sxs-lookup"><span data-stu-id="43845-232">Using partitionedBy property</span></span>
<span data-ttu-id="43845-233">Jak je uvedeno v předchozí části hello, můžete zadat dynamické folderPath a název souboru pro data časové řady s hello **partitionedBy** vlastnost [funkce pro vytváření dat a systémové proměnné hello](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="43845-233">As mentioned in hello previous section, you can specify a dynamic folderPath and filename for time series data with hello **partitionedBy** property, [Data Factory functions, and hello system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="43845-234">Další informace o datových sad časové řady, plánování a řezů, toounderstand v tématu [vytváření datových sad](data-factory-create-datasets.md), [plánování a provádění](data-factory-scheduling-and-execution.md), a [vytváření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="43845-234">toounderstand more details on time-series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="43845-235">Příklad 1:</span><span class="sxs-lookup"><span data-stu-id="43845-235">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="43845-236">V tomto příkladu {řez} se nahradí hodnotu hello hello objekt pro vytváření dat systému proměnné SliceStart ve formátu hello (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="43845-236">In this example, {Slice} is replaced with hello value of hello Data Factory system variable SliceStart in hello format (YYYYMMDDHH).</span></span> <span data-ttu-id="43845-237">SliceStart odkazuje toostart času řezu hello.</span><span class="sxs-lookup"><span data-stu-id="43845-237">SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="43845-238">Hello folderPath se liší pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="43845-238">hello folderPath is different for each slice.</span></span> <span data-ttu-id="43845-239">Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="43845-239">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="43845-240">Příklad 2:</span><span class="sxs-lookup"><span data-stu-id="43845-240">Sample 2:</span></span>

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

<span data-ttu-id="43845-241">V tomto příkladu se extrahují rok, měsíc, den a čas SliceStart do samostatné proměnných, které používají hello folderPath a název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="43845-241">In this example, year, month, day, and time of SliceStart are extracted into separate variables that hello folderPath and fileName properties use.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="43845-242">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="43845-242">Copy activity properties</span></span>
<span data-ttu-id="43845-243">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="43845-243">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="43845-244">Vlastnosti, například název, popis, vstupní a výstupní datové sady a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="43845-244">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span> <span data-ttu-id="43845-245">Vzhledem k tomu, vlastnosti dostupné ve hello **rámci typeProperties** části hello aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="43845-245">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span>

<span data-ttu-id="43845-246">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="43845-246">For Copy activity, they vary depending on hello types of sources and sinks.</span></span> <span data-ttu-id="43845-247">Pokud přesouváte data ze v místním systému souborů, nastavíte typ zdroje hello v aktivitě kopírování hello příliš**FileSystemSource**.</span><span class="sxs-lookup"><span data-stu-id="43845-247">If you are moving data from an on-premises file system, you set hello source type in hello copy activity too**FileSystemSource**.</span></span> <span data-ttu-id="43845-248">Podobně pokud přesouváte data tooan místní systém souborů, příliš nastavte typ jímky hello v aktivitě kopírování hello**FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="43845-248">Similarly, if you are moving data tooan on-premises file system, you set hello sink type in hello copy activity too**FileSystemSink**.</span></span> <span data-ttu-id="43845-249">Tato část obsahuje seznam vlastností, které jsou podporované FileSystemSource a FileSystemSink.</span><span class="sxs-lookup"><span data-stu-id="43845-249">This section provides a list of properties supported by FileSystemSource and FileSystemSink.</span></span>

<span data-ttu-id="43845-250">**FileSystemSource** podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="43845-250">**FileSystemSource** supports hello following properties:</span></span>

| <span data-ttu-id="43845-251">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="43845-251">Property</span></span> | <span data-ttu-id="43845-252">Popis</span><span class="sxs-lookup"><span data-stu-id="43845-252">Description</span></span> | <span data-ttu-id="43845-253">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="43845-253">Allowed values</span></span> | <span data-ttu-id="43845-254">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="43845-254">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="43845-255">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="43845-255">recursive</span></span> |<span data-ttu-id="43845-256">Určuje, zda text hello je číst data rekurzivně z podsložky hello nebo pouze z hello zadané složky.</span><span class="sxs-lookup"><span data-stu-id="43845-256">Indicates whether hello data is read recursively from hello subfolders or only from hello specified folder.</span></span> |<span data-ttu-id="43845-257">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="43845-257">True, False (default)</span></span> |<span data-ttu-id="43845-258">Ne</span><span class="sxs-lookup"><span data-stu-id="43845-258">No</span></span> |

<span data-ttu-id="43845-259">**FileSystemSink** podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="43845-259">**FileSystemSink** supports hello following properties:</span></span>

| <span data-ttu-id="43845-260">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="43845-260">Property</span></span> | <span data-ttu-id="43845-261">Popis</span><span class="sxs-lookup"><span data-stu-id="43845-261">Description</span></span> | <span data-ttu-id="43845-262">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="43845-262">Allowed values</span></span> | <span data-ttu-id="43845-263">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="43845-263">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="43845-264">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="43845-264">copyBehavior</span></span> |<span data-ttu-id="43845-265">Definuje chování kopie hello, pokud je zdroj hello BlobSource nebo systému souborů.</span><span class="sxs-lookup"><span data-stu-id="43845-265">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="43845-266">**PreserveHierarchy:** zachovává hello hierarchií souborů v cílové složce hello.</span><span class="sxs-lookup"><span data-stu-id="43845-266">**PreserveHierarchy:** Preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="43845-267">Relativní cesta hello hello zdrojového souboru toohello zdrojové složky tedy je hello stejná jako relativní cesta hello hello cílový soubor toohello cílové složky.</span><span class="sxs-lookup"><span data-stu-id="43845-267">That is, hello relative path of hello source file toohello source folder is hello same as hello relative path of hello target file toohello target folder.</span></span><br/><br/><span data-ttu-id="43845-268">**FlattenHierarchy:** všechny soubory ze zdrojové složky hello se vytvoří v hello první úroveň cílové složce.</span><span class="sxs-lookup"><span data-stu-id="43845-268">**FlattenHierarchy:** All files from hello source folder are created in hello first level of target folder.</span></span> <span data-ttu-id="43845-269">Hello zaměřením jsou vytvořen s názvem generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="43845-269">hello target files are created with an autogenerated name.</span></span><br/><br/><span data-ttu-id="43845-270">**MergeFiles:** slučuje všechny soubory ze hello zdrojové složky tooone souboru.</span><span class="sxs-lookup"><span data-stu-id="43845-270">**MergeFiles:** Merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="43845-271">Pokud je zadán název název nebo objekt blob souboru hello, hello sloučené soubor je zadaný název hello.</span><span class="sxs-lookup"><span data-stu-id="43845-271">If hello file name/blob name is specified, hello merged file name is hello specified name.</span></span> <span data-ttu-id="43845-272">Jinak je název automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="43845-272">Otherwise, it is an auto-generated file name.</span></span> |<span data-ttu-id="43845-273">Ne</span><span class="sxs-lookup"><span data-stu-id="43845-273">No</span></span> |

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="43845-274">Příklady rekurzivní a copyBehavior</span><span class="sxs-lookup"><span data-stu-id="43845-274">recursive and copyBehavior examples</span></span>
<span data-ttu-id="43845-275">Tato část popisuje hello výsledné chování hello kopírování pro různé kombinace hodnot vlastností rekurzivní a copyBehavior hello.</span><span class="sxs-lookup"><span data-stu-id="43845-275">This section describes hello resulting behavior of hello Copy operation for different combinations of values for hello recursive and copyBehavior properties.</span></span>

| <span data-ttu-id="43845-276">rekurzivní hodnota</span><span class="sxs-lookup"><span data-stu-id="43845-276">recursive value</span></span> | <span data-ttu-id="43845-277">Hodnota copyBehavior</span><span class="sxs-lookup"><span data-stu-id="43845-277">copyBehavior value</span></span> | <span data-ttu-id="43845-278">Výsledné chování</span><span class="sxs-lookup"><span data-stu-id="43845-278">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="43845-279">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="43845-279">true</span></span> |<span data-ttu-id="43845-280">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="43845-280">preserveHierarchy</span></span> |<span data-ttu-id="43845-281">Pro zdrojovou složku složku1 s hello strukturu,</span><span class="sxs-lookup"><span data-stu-id="43845-281">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="43845-282">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-282">Folder1</span></span><br/><span data-ttu-id="43845-283">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="43845-283">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="43845-284">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="43845-284">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="43845-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="43845-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="43845-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="43845-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="43845-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="43845-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="43845-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="43845-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="43845-289">Hello cílové složce složku1 je vytvořen s hello stejné struktury jako zdroj hello:</span><span class="sxs-lookup"><span data-stu-id="43845-289">hello target folder Folder1 is created with hello same structure as hello source:</span></span><br/><br/><span data-ttu-id="43845-290">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-290">Folder1</span></span><br/><span data-ttu-id="43845-291">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="43845-291">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="43845-292">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="43845-292">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="43845-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="43845-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="43845-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="43845-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="43845-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="43845-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="43845-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="43845-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span> |
| <span data-ttu-id="43845-297">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="43845-297">true</span></span> |<span data-ttu-id="43845-298">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="43845-298">flattenHierarchy</span></span> |<span data-ttu-id="43845-299">Pro zdrojovou složku složku1 s hello strukturu,</span><span class="sxs-lookup"><span data-stu-id="43845-299">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="43845-300">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-300">Folder1</span></span><br/><span data-ttu-id="43845-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="43845-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="43845-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="43845-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="43845-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="43845-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="43845-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="43845-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="43845-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="43845-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="43845-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="43845-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="43845-307">Vytvoření cíle Hello složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="43845-307">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="43845-308">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-308">Folder1</span></span><br/><span data-ttu-id="43845-309">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="43845-309">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="43845-310">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="43845-310">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="43845-311">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název soubor3</span><span class="sxs-lookup"><span data-stu-id="43845-311">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="43845-312">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File4</span><span class="sxs-lookup"><span data-stu-id="43845-312">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="43845-313">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File5</span><span class="sxs-lookup"><span data-stu-id="43845-313">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="43845-314">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="43845-314">true</span></span> |<span data-ttu-id="43845-315">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="43845-315">mergeFiles</span></span> |<span data-ttu-id="43845-316">Pro zdrojovou složku složku1 s hello strukturu,</span><span class="sxs-lookup"><span data-stu-id="43845-316">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="43845-317">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-317">Folder1</span></span><br/><span data-ttu-id="43845-318">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="43845-318">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="43845-319">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="43845-319">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="43845-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="43845-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="43845-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="43845-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="43845-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="43845-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="43845-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="43845-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="43845-324">Vytvoření cíle Hello složku1 s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="43845-324">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="43845-325">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-325">Folder1</span></span><br/><span data-ttu-id="43845-326">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + soubor3 + File4 + soubor 5 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="43845-326">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with an auto-generated file name.</span></span> |
| <span data-ttu-id="43845-327">False</span><span class="sxs-lookup"><span data-stu-id="43845-327">false</span></span> |<span data-ttu-id="43845-328">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="43845-328">preserveHierarchy</span></span> |<span data-ttu-id="43845-329">Pro zdrojovou složku složku1 s hello strukturu,</span><span class="sxs-lookup"><span data-stu-id="43845-329">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="43845-330">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-330">Folder1</span></span><br/><span data-ttu-id="43845-331">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="43845-331">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="43845-332">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="43845-332">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="43845-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="43845-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="43845-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="43845-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="43845-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="43845-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="43845-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="43845-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="43845-337">Hello cílové složce složku1 je vytvořen s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="43845-337">hello target folder Folder1 is created with hello following structure:</span></span><br/><br/><span data-ttu-id="43845-338">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-338">Folder1</span></span><br/><span data-ttu-id="43845-339">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="43845-339">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="43845-340">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="43845-340">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><span data-ttu-id="43845-341">Subfolder1 s soubor3, File4 a File5 není zachyceny.</span><span class="sxs-lookup"><span data-stu-id="43845-341">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="43845-342">False</span><span class="sxs-lookup"><span data-stu-id="43845-342">false</span></span> |<span data-ttu-id="43845-343">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="43845-343">flattenHierarchy</span></span> |<span data-ttu-id="43845-344">Pro zdrojovou složku složku1 s hello strukturu,</span><span class="sxs-lookup"><span data-stu-id="43845-344">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="43845-345">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-345">Folder1</span></span><br/><span data-ttu-id="43845-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="43845-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="43845-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="43845-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="43845-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="43845-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="43845-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="43845-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="43845-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="43845-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="43845-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="43845-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="43845-352">Hello cílové složce složku1 je vytvořen s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="43845-352">hello target folder Folder1 is created with hello following structure:</span></span><br/><br/><span data-ttu-id="43845-353">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-353">Folder1</span></span><br/><span data-ttu-id="43845-354">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="43845-354">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="43845-355">&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2</span><span class="sxs-lookup"><span data-stu-id="43845-355">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><span data-ttu-id="43845-356">Subfolder1 s soubor3, File4 a File5 není zachyceny.</span><span class="sxs-lookup"><span data-stu-id="43845-356">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="43845-357">False</span><span class="sxs-lookup"><span data-stu-id="43845-357">false</span></span> |<span data-ttu-id="43845-358">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="43845-358">mergeFiles</span></span> |<span data-ttu-id="43845-359">Pro zdrojovou složku složku1 s hello strukturu,</span><span class="sxs-lookup"><span data-stu-id="43845-359">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="43845-360">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-360">Folder1</span></span><br/><span data-ttu-id="43845-361">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="43845-361">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="43845-362">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="43845-362">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="43845-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="43845-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="43845-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3</span><span class="sxs-lookup"><span data-stu-id="43845-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="43845-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="43845-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="43845-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="43845-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="43845-367">Hello cílové složce složku1 je vytvořen s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="43845-367">hello target folder Folder1 is created with hello following structure:</span></span><br/><br/><span data-ttu-id="43845-368">Složku1</span><span class="sxs-lookup"><span data-stu-id="43845-368">Folder1</span></span><br/><span data-ttu-id="43845-369">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="43845-369">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with an auto-generated file name.</span></span><br/><span data-ttu-id="43845-370">&nbsp;&nbsp;&nbsp;&nbsp;Automaticky generovaný název File1</span><span class="sxs-lookup"><span data-stu-id="43845-370">&nbsp;&nbsp;&nbsp;&nbsp;Auto-generated name for File1</span></span><br/><br/><span data-ttu-id="43845-371">Subfolder1 s soubor3, File4 a File5 není zachyceny.</span><span class="sxs-lookup"><span data-stu-id="43845-371">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="43845-372">Podporované formáty souborů a komprese</span><span class="sxs-lookup"><span data-stu-id="43845-372">Supported file and compression formats</span></span>
<span data-ttu-id="43845-373">V tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článek na podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="43845-373">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples-for-copying-data-tooand-from-file-system"></a><span data-ttu-id="43845-374">Příklady JSON pro kopírování dat tooand ze systému souborů</span><span class="sxs-lookup"><span data-stu-id="43845-374">JSON examples for copying data tooand from file system</span></span>
<span data-ttu-id="43845-375">Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí hello [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="43845-375">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="43845-376">Ukazují jak toocopy tooand data ze služby v místním systému souborů a úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="43845-376">They show how toocopy data tooand from an on-premises file system and Azure Blob storage.</span></span> <span data-ttu-id="43845-377">Však může kopírovat data *přímo* ze všech zdrojů tooany hello z hello jímky uvedené v [podporované zdroje a jímky](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="43845-377">However, you can copy data *directly* from any of hello sources tooany of hello sinks listed in [Supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-an-on-premises-file-system-tooazure-blob-storage"></a><span data-ttu-id="43845-378">Příklad: Kopírování dat z tooAzure systému souboru místní úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="43845-378">Example: Copy data from an on-premises file system tooAzure Blob storage</span></span>
<span data-ttu-id="43845-379">Tento příklad ukazuje, jak toocopy data ze systému tooAzure soubor místní úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="43845-379">This sample shows how toocopy data from an on-premises file system tooAzure Blob storage.</span></span> <span data-ttu-id="43845-380">Ukázka Hello má hello následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="43845-380">hello sample has hello following Data Factory entities:</span></span>

* <span data-ttu-id="43845-381">Propojené služby typu [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="43845-381">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="43845-382">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="43845-382">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="43845-383">Vstup [datovou sadu](data-factory-create-datasets.md) typu [sdílení souborů](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="43845-383">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="43845-384">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="43845-384">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="43845-385">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="43845-385">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="43845-386">Následující ukázka Hello kopíruje data časové řady z tooAzure systému souboru místní úložiště objektů Blob každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="43845-386">hello following sample copies time-series data from an on-premises file system tooAzure Blob storage every hour.</span></span> <span data-ttu-id="43845-387">Vlastnosti Hello JSON, které se používají v tyto ukázky jsou popsané v části hello po hello ukázky.</span><span class="sxs-lookup"><span data-stu-id="43845-387">hello JSON properties that are used in these samples are described in hello sections after hello samples.</span></span>

<span data-ttu-id="43845-388">Jako první krok, nastavit Brána pro správu dat podle pokynů hello v [přesun dat mezi místní zdroje a hello cloudu s Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="43845-388">As a first step, set up Data Management Gateway as per hello instructions in [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span>

<span data-ttu-id="43845-389">**Souborový Server místní propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="43845-389">**On-Premises File Server linked service:**</span></span>

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

<span data-ttu-id="43845-390">Doporučujeme používat hello **encryptedCredential** vlastnost místo hello **userid** a **heslo** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="43845-390">We recommend using hello **encryptedCredential** property instead hello **userid** and **password** properties.</span></span> <span data-ttu-id="43845-391">V tématu [souborového serveru propojená služba](#linked-service-properties) podrobnosti o této propojené službě.</span><span class="sxs-lookup"><span data-stu-id="43845-391">See [File Server linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="43845-392">**Propojená služba Azure Storage:**</span><span class="sxs-lookup"><span data-stu-id="43845-392">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="43845-393">**Místní soubor vstupní datové sady systému:**</span><span class="sxs-lookup"><span data-stu-id="43845-393">**On-premises file system input dataset:**</span></span>

<span data-ttu-id="43845-394">Data je převzata z nový soubor každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="43845-394">Data is picked up from a new file every hour.</span></span> <span data-ttu-id="43845-395">Hello folderPath a určuje název souboru vlastnosti podle času zahájení hello hello řezu.</span><span class="sxs-lookup"><span data-stu-id="43845-395">hello folderPath and fileName properties are determined based on hello start time of hello slice.</span></span>  

<span data-ttu-id="43845-396">Nastavení `"external": "true"` tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello informuje Data Factory.</span><span class="sxs-lookup"><span data-stu-id="43845-396">Setting `"external": "true"` informs Data Factory that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="43845-397">**Azure Blob storage výstupní datovou sadu:**</span><span class="sxs-lookup"><span data-stu-id="43845-397">**Azure Blob storage output dataset:**</span></span>

<span data-ttu-id="43845-398">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="43845-398">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="43845-399">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="43845-399">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="43845-400">Cesta ke složce Hello používá hello rok, měsíc, den a hodina části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="43845-400">hello folder path uses hello year, month, day, and hour parts of hello start time.</span></span>

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

<span data-ttu-id="43845-401">**Aktivita kopírování v kanálu pomocí systému souborů zdrojový a podřízený objekt Blob:**</span><span class="sxs-lookup"><span data-stu-id="43845-401">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="43845-402">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady, a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="43845-402">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="43845-403">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**FileSystemSource**, a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="43845-403">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource**, and **sink** type is set too**BlobSink**.</span></span>

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

### <a name="example-copy-data-from-azure-sql-database-tooan-on-premises-file-system"></a><span data-ttu-id="43845-404">Příklad: Kopírování dat z Azure SQL Database tooan v místním systému souborů</span><span class="sxs-lookup"><span data-stu-id="43845-404">Example: Copy data from Azure SQL Database tooan on-premises file system</span></span>
<span data-ttu-id="43845-405">Následující ukázka ukazuje Hello:</span><span class="sxs-lookup"><span data-stu-id="43845-405">hello following sample shows:</span></span>

* <span data-ttu-id="43845-406">Propojené služby typu [azuresqldatabase..](data-factory-azure-sql-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="43845-406">A linked service of type [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="43845-407">Propojené služby typu [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="43845-407">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="43845-408">Vstupní datové sady typu [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="43845-408">An input dataset of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="43845-409">Výstupní datové typu [sdílení souborů](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="43845-409">An output dataset of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="43845-410">Kanál s aktivitou kopírování, která používá [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) a [FileSystemSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="43845-410">A pipeline with a copy activity that uses [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) and [FileSystemSink](#copy-activity-properties).</span></span>

<span data-ttu-id="43845-411">Ukázka Hello zkopíruje data časové řady ze systému souborů místní tooan tabulky Azure SQL každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="43845-411">hello sample copies time-series data from an Azure SQL table tooan on-premises file system every hour.</span></span> <span data-ttu-id="43845-412">Vlastnosti Hello JSON, které se používají v tyto ukázky jsou popsané v částech po hello ukázky.</span><span class="sxs-lookup"><span data-stu-id="43845-412">hello JSON properties that are used in these samples are described in sections after hello samples.</span></span>

<span data-ttu-id="43845-413">**Azure SQL Database propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="43845-413">**Azure SQL Database linked service:**</span></span>

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

<span data-ttu-id="43845-414">**Souborový Server místní propojené služby:**</span><span class="sxs-lookup"><span data-stu-id="43845-414">**On-Premises File Server linked service:**</span></span>

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

<span data-ttu-id="43845-415">Doporučujeme používat hello **encryptedCredential** vlastnost místo použití hello **userid** a **heslo** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="43845-415">We recommend using hello **encryptedCredential** property instead of using hello **userid** and **password** properties.</span></span> <span data-ttu-id="43845-416">V tématu [systém souborů propojená služba](#linked-service-properties) podrobnosti o této propojené službě.</span><span class="sxs-lookup"><span data-stu-id="43845-416">See [File System linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="43845-417">**Azure SQL vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="43845-417">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="43845-418">Hello příkladu se předpokládá, že jste vytvořili tabulku "MyTable" v Azure SQL, která obsahuje sloupec s názvem "timestampcolumn" pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="43845-418">hello sample assumes that you've created a table “MyTable” in Azure SQL, and it contains a column called “timestampcolumn” for time-series data.</span></span>

<span data-ttu-id="43845-419">Nastavení ``“external”: ”true”`` tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello informuje Data Factory.</span><span class="sxs-lookup"><span data-stu-id="43845-419">Setting ``“external”: ”true”`` informs Data Factory that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="43845-420">**Místní soubor výstupní datovou sadu systému:**</span><span class="sxs-lookup"><span data-stu-id="43845-420">**On-premises file system output dataset:**</span></span>

<span data-ttu-id="43845-421">Data jsou nový soubor zkopírovaný tooa každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="43845-421">Data is copied tooa new file every hour.</span></span> <span data-ttu-id="43845-422">Hello folderPath a název souboru pro objekt blob hello určuje podle času zahájení hello hello řezu.</span><span class="sxs-lookup"><span data-stu-id="43845-422">hello folderPath and fileName for hello blob are determined based on hello start time of hello slice.</span></span>

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

<span data-ttu-id="43845-423">**Aktivita kopírování v kanálu s SQL zdroj a jímka systému souborů:**</span><span class="sxs-lookup"><span data-stu-id="43845-423">**A copy activity in a pipeline with SQL source and File System sink:**</span></span>

<span data-ttu-id="43845-424">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady, a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="43845-424">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="43845-425">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**SqlSource**a hello **podřízený** je typ nastaven příliš**FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="43845-425">In hello pipeline JSON definition, hello **source** type is set too**SqlSource**, and hello **sink** type is set too**FileSystemSink**.</span></span> <span data-ttu-id="43845-426">Hello dotaz SQL, který je zadán pro hello **SqlReaderQuery** vlastnost vybere hello data v hello za hodinu toocopy.</span><span class="sxs-lookup"><span data-stu-id="43845-426">hello SQL query that is specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

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


<span data-ttu-id="43845-427">Můžete také mapovat sloupců z toocolumns datové sady zdroje z podřízený datovou sadu v definici aktivity kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="43845-427">You can also map columns from source dataset toocolumns from sink dataset in hello copy activity definition.</span></span> <span data-ttu-id="43845-428">Podrobnosti najdete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="43845-428">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="43845-429">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="43845-429">Performance and tuning</span></span>
 <span data-ttu-id="43845-430">výkon této dopad hello přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize faktory toolearn o klíči, najdete v části hello [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="43845-430">toolearn about key factors that impact hello performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it, see hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>
