---
title: "aaaMove dat ze serveru FTP pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom toomove dat ze serveru FTP pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jingwang
ms.openlocfilehash: c707e29532b2a8a870603948cb6150ab857bd6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a><span data-ttu-id="b442d-103">Přesun dat ze serveru FTP pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b442d-103">Move data from an FTP server by using Azure Data Factory</span></span>
<span data-ttu-id="b442d-104">Tento článek vysvětluje, jak toouse hello aktivita kopírování v Azure Data Factory toomove data ze serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="b442d-104">This article explains how toouse hello copy activity in Azure Data Factory toomove data from an FTP server.</span></span> <span data-ttu-id="b442d-105">Vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-105">It builds on hello [Data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="b442d-106">Můžete zkopírovat data z úložiště dat tooany podporované podřízený FTP serveru.</span><span class="sxs-lookup"><span data-stu-id="b442d-106">You can copy data from an FTP server tooany supported sink data store.</span></span> <span data-ttu-id="b442d-107">Seznam dat úložiště, které jsou podporované jako jímky pomocí aktivity kopírování hello, najdete v části hello [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="b442d-107">For a list of data stores supported as sinks by hello copy activity, see hello [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="b442d-108">Objekt pro vytváření dat se aktuálně podporuje pouze přesunutí dat od FTP server tooother ukládá, ale není přesouvání dat od ostatních dat ukládá tooan FTP serveru.</span><span class="sxs-lookup"><span data-stu-id="b442d-108">Data Factory currently supports only moving data from an FTP server tooother data stores, but not moving data from other data stores tooan FTP server.</span></span> <span data-ttu-id="b442d-109">Podporuje místní a cloudové servery FTP.</span><span class="sxs-lookup"><span data-stu-id="b442d-109">It supports both on-premises and cloud FTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="b442d-110">Aktivita kopírování Hello neodstraní hello zdrojový soubor po úspěšně zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="b442d-110">hello copy activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="b442d-111">Pokud potřebujete toodelete hello zdrojový soubor po úspěšné kopie, vytvořte soubor vlastní aktivity toodelete hello a použít hello aktivity v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-111">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file, and use hello activity in hello pipeline.</span></span> 

## <a name="enable-connectivity"></a><span data-ttu-id="b442d-112">Povolit připojení</span><span class="sxs-lookup"><span data-stu-id="b442d-112">Enable connectivity</span></span>
<span data-ttu-id="b442d-113">Pokud přesouváte data ze **místní** úložiště dat cloudu tooa server FTP (například tooAzure úložiště objektů Blob), nainstalovat a používat Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="b442d-113">If you are moving data from an **on-premises** FTP server tooa cloud data store (for example, tooAzure Blob storage), install and use Data Management Gateway.</span></span> <span data-ttu-id="b442d-114">Hello Brána pro správu dat je klientský agent, který je nainstalován v místním počítači a umožňuje cloudové služby tooconnect tooan místnímu prostředku.</span><span class="sxs-lookup"><span data-stu-id="b442d-114">hello Data Management Gateway is a client agent that is installed on your on-premises machine, and it allows cloud services tooconnect tooan on-premises resource.</span></span> <span data-ttu-id="b442d-115">Podrobnosti najdete v tématu [Brána pro správu dat](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="b442d-115">For details, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span> <span data-ttu-id="b442d-116">Podrobné pokyny k nastavení hello brány a jeho použití naleznete v tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="b442d-116">For step-by-step instructions on setting up hello gateway and using it, see [Moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="b442d-117">Používáte server hello brány tooconnect tooan FTP, i když je hello server na Azure infrastruktury jako služby (IaaS) virtuální počítač (VM).</span><span class="sxs-lookup"><span data-stu-id="b442d-117">You use hello gateway tooconnect tooan FTP server, even if hello server is on an Azure infrastructure as a service (IaaS) virtual machine (VM).</span></span>

<span data-ttu-id="b442d-118">Je možné tooinstall hello brány hello stejné na místní počítač nebo virtuální počítač IaaS jako hello serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="b442d-118">It is possible tooinstall hello gateway on hello same on-premises machine or IaaS VM as hello FTP server.</span></span> <span data-ttu-id="b442d-119">Doporučujeme však nainstalovat bránu hello na samostatný počítač nebo sporu prostředků tooavoid virtuálních počítačů IaaS a pro dosažení vyššího výkonu.</span><span class="sxs-lookup"><span data-stu-id="b442d-119">However, we recommend that you install hello gateway on a separate machine or IaaS VM tooavoid resource contention, and for better performance.</span></span> <span data-ttu-id="b442d-120">Když instalujete na samostatný počítač hello brány, hello počítač měl být schopný tooaccess hello FTP serveru.</span><span class="sxs-lookup"><span data-stu-id="b442d-120">When you install hello gateway on a separate machine, hello machine should be able tooaccess hello FTP server.</span></span>

## <a name="get-started"></a><span data-ttu-id="b442d-121">Začínáme</span><span class="sxs-lookup"><span data-stu-id="b442d-121">Get started</span></span>
<span data-ttu-id="b442d-122">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z zdroje FTP pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b442d-122">You can create a pipeline with a copy activity that moves data from an FTP source by using different tools or APIs.</span></span>

<span data-ttu-id="b442d-123">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním služby Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="b442d-123">hello easiest way toocreate a pipeline is toouse hello **Data Factory Copy Wizard**.</span></span> <span data-ttu-id="b442d-124">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé návod.</span><span class="sxs-lookup"><span data-stu-id="b442d-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough.</span></span>

<span data-ttu-id="b442d-125">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="b442d-125">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="b442d-126">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="b442d-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="b442d-127">Jestli používáte nástroje hello nebo rozhraní API, proveďte následující kroky toocreate kanál, který přesouvá data ze zdrojových dat úložiště tooa jímku dat hello:</span><span class="sxs-lookup"><span data-stu-id="b442d-127">Whether you use hello tools or APIs, perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="b442d-128">Vytvoření **propojené služby** toolink vstupní a výstupní data úložiště tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="b442d-128">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="b442d-129">Vytvoření **datové sady** toorepresent vstupní a výstupní data pro hello operace kopírování.</span><span class="sxs-lookup"><span data-stu-id="b442d-129">Create **datasets** toorepresent input and output data for hello copy operation.</span></span>
3. <span data-ttu-id="b442d-130">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="b442d-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="b442d-131">Když použijete Průvodce hello, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="b442d-131">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="b442d-132">Pokud používáte rozhraní API (s výjimkou .NET API) nebo nástroje, definujete tyto entity služby Data Factory pomocí formátu JSON hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-132">When you use tools or APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span> <span data-ttu-id="b442d-133">Příklad s definicemi JSON entit služby Data Factory, které jsou používané toocopy data z úložiště dat služby FTP, naleznete v tématu hello [JSON příklad: kopírování dat z objektu blob tooAzure serveru FTP](#json-example-copy-data-from-ftp-server-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="b442d-133">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an FTP data store, see hello [JSON example: Copy data from FTP server tooAzure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) section of this article.</span></span>

> [!NOTE]
> <span data-ttu-id="b442d-134">Podrobnosti o podporovaných toouse formáty souborů a komprese najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span><span class="sxs-lookup"><span data-stu-id="b442d-134">For details about supported file and compression formats toouse, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span></span>

<span data-ttu-id="b442d-135">Hello následující části obsahují podrobné informace o vlastnostech formátu JSON, které jsou používané toodefine objekt pro vytváření dat entity konkrétní tooFTP.</span><span class="sxs-lookup"><span data-stu-id="b442d-135">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooFTP.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="b442d-136">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="b442d-136">Linked service properties</span></span>
<span data-ttu-id="b442d-137">Hello následující tabulka popisuje JSON elementy konkrétní tooan FTP propojené služby.</span><span class="sxs-lookup"><span data-stu-id="b442d-137">hello following table describes JSON elements specific tooan FTP linked service.</span></span>

| <span data-ttu-id="b442d-138">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b442d-138">Property</span></span> | <span data-ttu-id="b442d-139">Popis</span><span class="sxs-lookup"><span data-stu-id="b442d-139">Description</span></span> | <span data-ttu-id="b442d-140">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b442d-140">Required</span></span> | <span data-ttu-id="b442d-141">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b442d-141">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b442d-142">type</span><span class="sxs-lookup"><span data-stu-id="b442d-142">type</span></span> |<span data-ttu-id="b442d-143">Nastavte tento tooFtpServer.</span><span class="sxs-lookup"><span data-stu-id="b442d-143">Set this tooFtpServer.</span></span> |<span data-ttu-id="b442d-144">Ano</span><span class="sxs-lookup"><span data-stu-id="b442d-144">Yes</span></span> |&nbsp; |
| <span data-ttu-id="b442d-145">hostitele</span><span class="sxs-lookup"><span data-stu-id="b442d-145">host</span></span> |<span data-ttu-id="b442d-146">Zadejte hello název nebo IP adresu serveru FTP hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-146">Specify hello name or IP address of hello FTP server.</span></span> |<span data-ttu-id="b442d-147">Ano</span><span class="sxs-lookup"><span data-stu-id="b442d-147">Yes</span></span> |&nbsp; |
| <span data-ttu-id="b442d-148">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="b442d-148">authenticationType</span></span> |<span data-ttu-id="b442d-149">Zadejte typ ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-149">Specify hello authentication type.</span></span> |<span data-ttu-id="b442d-150">Ano</span><span class="sxs-lookup"><span data-stu-id="b442d-150">Yes</span></span> |<span data-ttu-id="b442d-151">Anonymní, základní</span><span class="sxs-lookup"><span data-stu-id="b442d-151">Basic, Anonymous</span></span> |
| <span data-ttu-id="b442d-152">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="b442d-152">username</span></span> |<span data-ttu-id="b442d-153">Zadejte hello uživatel, který má přístup k serveru FTP toohello.</span><span class="sxs-lookup"><span data-stu-id="b442d-153">Specify hello user who has access toohello FTP server.</span></span> |<span data-ttu-id="b442d-154">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-154">No</span></span> |&nbsp; |
| <span data-ttu-id="b442d-155">heslo</span><span class="sxs-lookup"><span data-stu-id="b442d-155">password</span></span> |<span data-ttu-id="b442d-156">Zadejte hello heslo pro uživatele hello (uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="b442d-156">Specify hello password for hello user (username).</span></span> |<span data-ttu-id="b442d-157">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-157">No</span></span> |&nbsp; |
| <span data-ttu-id="b442d-158">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="b442d-158">encryptedCredential</span></span> |<span data-ttu-id="b442d-159">Zadejte hello šifrovat přihlašovací údaje tooaccess hello FTP serveru.</span><span class="sxs-lookup"><span data-stu-id="b442d-159">Specify hello encrypted credential tooaccess hello FTP server.</span></span> |<span data-ttu-id="b442d-160">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-160">No</span></span> |&nbsp; |
| <span data-ttu-id="b442d-161">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b442d-161">gatewayName</span></span> |<span data-ttu-id="b442d-162">Zadejte název hello hello brány Brána pro správu dat tooconnect tooan na místním serveru FTP serveru.</span><span class="sxs-lookup"><span data-stu-id="b442d-162">Specify hello name of hello gateway in Data Management Gateway tooconnect tooan on-premises FTP server.</span></span> |<span data-ttu-id="b442d-163">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-163">No</span></span> |&nbsp; |
| <span data-ttu-id="b442d-164">port</span><span class="sxs-lookup"><span data-stu-id="b442d-164">port</span></span> |<span data-ttu-id="b442d-165">Zadejte hello port, na které hello FTP server naslouchá.</span><span class="sxs-lookup"><span data-stu-id="b442d-165">Specify hello port on which hello FTP server is listening.</span></span> |<span data-ttu-id="b442d-166">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-166">No</span></span> |<span data-ttu-id="b442d-167">21</span><span class="sxs-lookup"><span data-stu-id="b442d-167">21</span></span> |
| <span data-ttu-id="b442d-168">enableSsl</span><span class="sxs-lookup"><span data-stu-id="b442d-168">enableSsl</span></span> |<span data-ttu-id="b442d-169">Zadejte, zda toouse FTP přes kanál SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="b442d-169">Specify whether toouse FTP over an SSL/TLS channel.</span></span> |<span data-ttu-id="b442d-170">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-170">No</span></span> |<span data-ttu-id="b442d-171">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="b442d-171">true</span></span> |
| <span data-ttu-id="b442d-172">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="b442d-172">enableServerCertificateValidation</span></span> |<span data-ttu-id="b442d-173">Zadejte, zda tooenable server SSL certifikát ověření, pokud používáte FTP přes kanál SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="b442d-173">Specify whether tooenable server SSL certificate validation when you are using FTP over SSL/TLS channel.</span></span> |<span data-ttu-id="b442d-174">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-174">No</span></span> |<span data-ttu-id="b442d-175">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="b442d-175">true</span></span> |

### <a name="use-anonymous-authentication"></a><span data-ttu-id="b442d-176">Anonymní ověřování použijte</span><span class="sxs-lookup"><span data-stu-id="b442d-176">Use Anonymous authentication</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {        
            "authenticationType": "Anonymous",
              "host": "myftpserver.com"
        }
    }
}
```

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a><span data-ttu-id="b442d-177">Použít uživatelské jméno a heslo v prostém textu pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="b442d-177">Use username and password in plain text for basic authentication</span></span>

```JSON
{
    "name": "FTPLinkedService",
      "properties": {
    "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
}
```

### <a name="use-port-enablessl-enableservercertificatevalidation"></a><span data-ttu-id="b442d-178">Použijte port, enableSsl, enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="b442d-178">Use port, enableSsl, enableServerCertificateValidation</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a><span data-ttu-id="b442d-179">Použití encryptedCredential pro ověřování a brány</span><span class="sxs-lookup"><span data-stu-id="b442d-179">Use encryptedCredential for authentication and gateway</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "mygateway"
        }
      }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="b442d-180">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="b442d-180">Dataset properties</span></span>
<span data-ttu-id="b442d-181">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části [vytváření datových sad](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="b442d-181">For a full list of sections and properties available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="b442d-182">Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b442d-182">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="b442d-183">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b442d-183">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="b442d-184">Poskytuje informace, které je typu konkrétní toohello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="b442d-184">It provides information that is specific toohello dataset type.</span></span> <span data-ttu-id="b442d-185">Hello **rámci typeProperties** části datové sady typu **sdílení souborů** má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="b442d-185">hello **typeProperties** section for a dataset of type **FileShare** has hello following properties:</span></span>

| <span data-ttu-id="b442d-186">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b442d-186">Property</span></span> | <span data-ttu-id="b442d-187">Popis</span><span class="sxs-lookup"><span data-stu-id="b442d-187">Description</span></span> | <span data-ttu-id="b442d-188">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b442d-188">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b442d-189">folderPath</span><span class="sxs-lookup"><span data-stu-id="b442d-189">folderPath</span></span> |<span data-ttu-id="b442d-190">Složka toohello cestou.</span><span class="sxs-lookup"><span data-stu-id="b442d-190">Subpath toohello folder.</span></span> <span data-ttu-id="b442d-191">Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-191">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="b442d-192">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="b442d-192">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="b442d-193">Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez začínat a končit hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="b442d-193">You can combine this property with **partitionBy** toohave folder paths based on slice start and end date-times.</span></span> |<span data-ttu-id="b442d-194">Ano</span><span class="sxs-lookup"><span data-stu-id="b442d-194">Yes</span></span> |
| <span data-ttu-id="b442d-195">fileName</span><span class="sxs-lookup"><span data-stu-id="b442d-195">fileName</span></span> |<span data-ttu-id="b442d-196">Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-196">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="b442d-197">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-197">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="b442d-198">Když **fileName** není zadané pro datovou sadu výstupů, hello název souboru hello generována je v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="b442d-198">When **fileName** is not specified for an output dataset, hello name of hello generated file is in hello following format:</span></span> <br/><br/><span data-ttu-id="b442d-199">Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="b442d-199">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="b442d-200">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-200">No</span></span> |
| <span data-ttu-id="b442d-201">fileFilter</span><span class="sxs-lookup"><span data-stu-id="b442d-201">fileFilter</span></span> |<span data-ttu-id="b442d-202">Zadejte filtru toobe používá tooselect podmnožinu souborů v hello **folderPath**, ne všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="b442d-202">Specify a filter toobe used tooselect a subset of files in hello **folderPath**, rather than all files.</span></span><br/><br/><span data-ttu-id="b442d-203">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="b442d-203">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="b442d-204">Příklad 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="b442d-204">Example 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="b442d-205">Příklad 2:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="b442d-205">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="b442d-206">**fileFilter** lze použít pro datové sadě služby vstupní sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="b442d-206">**fileFilter** is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="b442d-207">Tato vlastnost není podporována s Hadoop Distributed File System (HDFS).</span><span class="sxs-lookup"><span data-stu-id="b442d-207">This property is not supported with Hadoop Distributed File System (HDFS).</span></span> |<span data-ttu-id="b442d-208">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-208">No</span></span> |
| <span data-ttu-id="b442d-209">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="b442d-209">partitionedBy</span></span> |<span data-ttu-id="b442d-210">Použít toospecify dynamický **folderPath** a **fileName** pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="b442d-210">Used toospecify a dynamic **folderPath** and **fileName** for time series data.</span></span> <span data-ttu-id="b442d-211">Například můžete zadat **folderPath** , je pro každou hodinu dat parametry.</span><span class="sxs-lookup"><span data-stu-id="b442d-211">For example, you can specify a **folderPath** that is parameterized for every hour of data.</span></span> |<span data-ttu-id="b442d-212">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-212">No</span></span> |
| <span data-ttu-id="b442d-213">Formát</span><span class="sxs-lookup"><span data-stu-id="b442d-213">format</span></span> | <span data-ttu-id="b442d-214">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b442d-214">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="b442d-215">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="b442d-215">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="b442d-216">Další informace najdete v tématu hello [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formátu ](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="b442d-216">For more information, see hello [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="b442d-217">Pokud chcete soubory toocopy, protože se jedná o mezi souborové úložiště (binární kopie), část formátu hello v obou definice vstupní a výstupní datovou sadu přeskočte.</span><span class="sxs-lookup"><span data-stu-id="b442d-217">If you want toocopy files as they are between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="b442d-218">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-218">No</span></span> |
| <span data-ttu-id="b442d-219">Komprese</span><span class="sxs-lookup"><span data-stu-id="b442d-219">compression</span></span> | <span data-ttu-id="b442d-220">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-220">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="b442d-221">Podporované typy jsou **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**, a jsou podporované úrovně **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="b442d-221">Supported types are **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**, and supported levels are **Optimal** and **Fastest**.</span></span> <span data-ttu-id="b442d-222">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="b442d-222">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="b442d-223">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-223">No</span></span> |
| <span data-ttu-id="b442d-224">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="b442d-224">useBinaryTransfer</span></span> |<span data-ttu-id="b442d-225">Určete, zda toouse hello binární přenášet režimu.</span><span class="sxs-lookup"><span data-stu-id="b442d-225">Specify whether toouse hello binary transfer mode.</span></span> <span data-ttu-id="b442d-226">Hello hodnoty jsou pro binárním režimu (Toto je výchozí hodnota hello) na hodnotu true a false pro ASCII.</span><span class="sxs-lookup"><span data-stu-id="b442d-226">hello values are true for binary mode (this is hello default value), and false for ASCII.</span></span> <span data-ttu-id="b442d-227">Tuto vlastnost lze použít pouze v případě hello typu přidružené propojené služby typu: Server_ftp.</span><span class="sxs-lookup"><span data-stu-id="b442d-227">This property can only be used when hello associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="b442d-228">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-228">No</span></span> |

> [!NOTE]
> <span data-ttu-id="b442d-229">**Název souboru** a **fileFilter** nelze používat současně.</span><span class="sxs-lookup"><span data-stu-id="b442d-229">**fileName** and **fileFilter** cannot be used simultaneously.</span></span>

### <a name="use-hello-partionedby-property"></a><span data-ttu-id="b442d-230">Použijte vlastnost partionedBy hello</span><span class="sxs-lookup"><span data-stu-id="b442d-230">Use hello partionedBy property</span></span>
<span data-ttu-id="b442d-231">Jak je uvedeno v předchozí části hello, můžete zadat dynamický **folderPath** a **fileName** pro data časové řady s hello **partitionedBy** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b442d-231">As mentioned in hello previous section, you can specify a dynamic **folderPath** and **fileName** for time series data with hello **partitionedBy** property.</span></span>

<span data-ttu-id="b442d-232">toolearn o datové sady času řady, plánování a řezů, najdete v části [vytváření datových sad](data-factory-create-datasets.md), [plánování a provádění](data-factory-scheduling-and-execution.md), a [vytváření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="b442d-232">toolearn about time series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="b442d-233">Ukázka 1</span><span class="sxs-lookup"><span data-stu-id="b442d-233">Sample 1</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="b442d-234">V tomto příkladu {řez} se nahradí hello hodnoty proměnné objektu pro vytváření dat systému SliceStart, v hello formátu zadaný (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="b442d-234">In this example, {Slice} is replaced with hello value of Data Factory system variable SliceStart, in hello format specified (YYYYMMDDHH).</span></span> <span data-ttu-id="b442d-235">Hello SliceStart odkazuje toostart času řezu hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-235">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="b442d-236">Cesta ke složce Hello se liší pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="b442d-236">hello folder path is different for each slice.</span></span> <span data-ttu-id="b442d-237">(Například wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.)</span><span class="sxs-lookup"><span data-stu-id="b442d-237">(For example, wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.)</span></span>

#### <a name="sample-2"></a><span data-ttu-id="b442d-238">Ukázka 2</span><span class="sxs-lookup"><span data-stu-id="b442d-238">Sample 2</span></span>

```json
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
<span data-ttu-id="b442d-239">V tomto příkladu se extrahují hello rok, měsíc, den a čas SliceStart do samostatné proměnné, které jsou používány hello **folderPath** a **fileName** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b442d-239">In this example, hello year, month, day, and time of SliceStart are extracted into separate variables that are used by hello **folderPath** and **fileName** properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="b442d-240">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="b442d-240">Copy activity properties</span></span>
<span data-ttu-id="b442d-241">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu [vytváření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="b442d-241">For a full list of sections and properties available for defining activities, see [Creating pipelines](data-factory-create-pipelines.md).</span></span> <span data-ttu-id="b442d-242">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="b442d-242">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="b442d-243">Vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivity na hello druhé straně, se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="b442d-243">Properties available in hello **typeProperties** section of hello activity, on hello other hand, vary with each activity type.</span></span> <span data-ttu-id="b442d-244">Pro aktivitu kopírování hello vlastnosti typu hello lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="b442d-244">For hello copy activity, hello type properties vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="b442d-245">Při aktivitě kopírování, pokud je zdroj hello typu **FileSystemSource**, hello následující vlastnosti jsou k dispozici v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="b442d-245">In copy activity, when hello source is of type **FileSystemSource**, hello following property is available in **typeProperties** section:</span></span>

| <span data-ttu-id="b442d-246">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b442d-246">Property</span></span> | <span data-ttu-id="b442d-247">Popis</span><span class="sxs-lookup"><span data-stu-id="b442d-247">Description</span></span> | <span data-ttu-id="b442d-248">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="b442d-248">Allowed values</span></span> | <span data-ttu-id="b442d-249">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b442d-249">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b442d-250">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="b442d-250">recursive</span></span> |<span data-ttu-id="b442d-251">Určuje, zda text hello je číst data rekurzivně z podsložky hello nebo pouze z hello zadané složky.</span><span class="sxs-lookup"><span data-stu-id="b442d-251">Indicates whether hello data is read recursively from hello subfolders, or only from hello specified folder.</span></span> |<span data-ttu-id="b442d-252">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="b442d-252">True, False (default)</span></span> |<span data-ttu-id="b442d-253">Ne</span><span class="sxs-lookup"><span data-stu-id="b442d-253">No</span></span> |

## <a name="json-example-copy-data-from-ftp-server-tooazure-blob"></a><span data-ttu-id="b442d-254">Příklad JSON: kopírování dat z tooAzure serveru FTP objektů Blob</span><span class="sxs-lookup"><span data-stu-id="b442d-254">JSON example: Copy data from FTP server tooAzure Blob</span></span>
<span data-ttu-id="b442d-255">Tento příklad ukazuje, jak toocopy data z FTP serveru tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="b442d-255">This sample shows how toocopy data from an FTP server tooAzure Blob storage.</span></span> <span data-ttu-id="b442d-256">Ale data se dají zkopírovat přímo tooany hello jímky stanovené v hello [podporované úložiště dat a formáty](data-factory-data-movement-activities.md#supported-data-stores-and-formats), a to pomocí aktivity kopírování hello v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="b442d-256">However, data can be copied directly tooany of hello sinks stated in hello [supported data stores and formats](data-factory-data-movement-activities.md#supported-data-stores-and-formats), by using hello copy activity in Data Factory.</span></span>  

<span data-ttu-id="b442d-257">Hello následující příklady poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="b442d-257">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span></span>

* <span data-ttu-id="b442d-258">Propojené služby typu [Server_ftp](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="b442d-258">A linked service of type [FtpServer](#linked-service-properties)</span></span>
* <span data-ttu-id="b442d-259">Propojené služby typu [azurestorage.](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="b442d-259">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="b442d-260">Vstup [datovou sadu](data-factory-create-datasets.md) typu [sdílení souborů](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="b442d-260">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties)</span></span>
* <span data-ttu-id="b442d-261">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="b442d-261">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="b442d-262">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="b442d-262">A [pipeline](data-factory-create-pipelines.md) with copy activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="b442d-263">Ukázka Hello kopíruje data ze FTP server tooan objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="b442d-263">hello sample copies data from an FTP server tooan Azure blob every hour.</span></span> <span data-ttu-id="b442d-264">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-264">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

### <a name="ftp-linked-service"></a><span data-ttu-id="b442d-265">Propojená služba FTP</span><span class="sxs-lookup"><span data-stu-id="b442d-265">FTP linked service</span></span>

<span data-ttu-id="b442d-266">Tento příklad používá základní ověřování s hello uživatelské jméno a heslo v prostém textu.</span><span class="sxs-lookup"><span data-stu-id="b442d-266">This example uses basic authentication, with hello user name and password in plain text.</span></span> <span data-ttu-id="b442d-267">Můžete také použít jednu z následujících způsobů hello:</span><span class="sxs-lookup"><span data-stu-id="b442d-267">You can also use one of hello following ways:</span></span>

* <span data-ttu-id="b442d-268">Anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="b442d-268">Anonymous authentication</span></span>
* <span data-ttu-id="b442d-269">Základní ověřování s zašifrované přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="b442d-269">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="b442d-270">FTP přes SSL/TLS (FTPS)</span><span class="sxs-lookup"><span data-stu-id="b442d-270">FTP over SSL/TLS (FTPS)</span></span>

<span data-ttu-id="b442d-271">V tématu hello [FTP propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="b442d-271">See hello [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
    "type": "FtpServer",
    "typeProperties": {
        "host": "myftpserver.com",           
        "authenticationType": "Basic",
        "username": "Admin",
        "password": "123456"
    }
  }
}
```
### <a name="azure-storage-linked-service"></a><span data-ttu-id="b442d-272">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b442d-272">Azure Storage linked service</span></span>

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
### <a name="ftp-input-dataset"></a><span data-ttu-id="b442d-273">FTP vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="b442d-273">FTP input dataset</span></span>

<span data-ttu-id="b442d-274">Tato datová sada odkazuje toohello FTP složky `mysharedfolder` a soubor `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="b442d-274">This dataset refers toohello FTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="b442d-275">Hello kanál kopíruje cíl toohello souboru hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-275">hello pipeline copies hello file toohello destination.</span></span>

<span data-ttu-id="b442d-276">Nastavení **externí** příliš**true** informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="b442d-276">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory, and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "FTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "FTPLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv",
      "useBinaryTransfer": true
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="b442d-277">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="b442d-277">Azure Blob output dataset</span></span>

<span data-ttu-id="b442d-278">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="b442d-278">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="b442d-279">Cesta ke složce Hello pro objekt blob hello se dynamicky vyhodnotí, podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="b442d-279">hello folder path for hello blob is dynamically evaluated, based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="b442d-280">Cesta ke složce Hello používá hello rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="b442d-280">hello folder path uses hello year, month, day, and hours parts of hello start time.</span></span>

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a><span data-ttu-id="b442d-281">Aktivita kopírování v kanálu s podřízený zdroj a objektů blob systému souborů</span><span class="sxs-lookup"><span data-stu-id="b442d-281">A copy activity in a pipeline with file system source and blob sink</span></span>

<span data-ttu-id="b442d-282">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady, a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="b442d-282">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="b442d-283">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**FileSystemSource**a hello **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="b442d-283">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource**, and hello **sink** type is set too**BlobSink**.</span></span>

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
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
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00Z",
        "end": "2016-08-24T19:00:00Z"
    }
}
```
> [!NOTE]
> <span data-ttu-id="b442d-284">toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="b442d-284">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b442d-285">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b442d-285">Next steps</span></span>
<span data-ttu-id="b442d-286">V tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="b442d-286">See hello following articles:</span></span>

* <span data-ttu-id="b442d-287">toolearn o klíči faktory, že dopad výkonu (aktivita kopírování) přesun dat v datové továrně a různé způsoby toooptimize, najdete v části hello [zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="b442d-287">toolearn about key factors that impact performance of data movement (copy activity) in Data Factory, and various ways toooptimize it, see hello [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>

* <span data-ttu-id="b442d-288">Podrobné pokyny pro vytvoření kanálu s aktivitou kopírování najdete v tématu hello [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="b442d-288">For step-by-step instructions for creating a pipeline with a copy activity, see hello [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
