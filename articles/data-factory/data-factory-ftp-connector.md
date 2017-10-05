---
title: "Přesun dat ze serveru FTP pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data ze serveru FTP pomocí Azure Data Factory."
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
ms.openlocfilehash: f8f31f3a2ee02c964737dd32145499f3dcfd0624
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a><span data-ttu-id="5295d-103">Přesun dat ze serveru FTP pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="5295d-103">Move data from an FTP server by using Azure Data Factory</span></span>
<span data-ttu-id="5295d-104">Tento článek vysvětluje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat ze serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="5295d-104">This article explains how to use the copy activity in Azure Data Factory to move data from an FTP server.</span></span> <span data-ttu-id="5295d-105">Vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="5295d-105">It builds on the [Data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="5295d-106">Ze serveru FTP můžete zkopírovat data do úložiště dat žádné podporované jímky.</span><span class="sxs-lookup"><span data-stu-id="5295d-106">You can copy data from an FTP server to any supported sink data store.</span></span> <span data-ttu-id="5295d-107">Seznam úložišť dat jako jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabulky.</span><span class="sxs-lookup"><span data-stu-id="5295d-107">For a list of data stores supported as sinks by the copy activity, see the [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="5295d-108">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat ze serveru FTP na jiným úložištím dat, ale není přesouvání dat od ostatních dat ukládá na FTP server.</span><span class="sxs-lookup"><span data-stu-id="5295d-108">Data Factory currently supports only moving data from an FTP server to other data stores, but not moving data from other data stores to an FTP server.</span></span> <span data-ttu-id="5295d-109">Podporuje místní a cloudové servery FTP.</span><span class="sxs-lookup"><span data-stu-id="5295d-109">It supports both on-premises and cloud FTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="5295d-110">Aktivita kopírování nedojde k odstranění zdrojového souboru po byl úspěšně zkopírován do cílové.</span><span class="sxs-lookup"><span data-stu-id="5295d-110">The copy activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="5295d-111">Pokud potřebujete odstranit zdrojový soubor po úspěšné kopie, vytvořte vlastní aktivity odstranit soubor a použijte aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="5295d-111">If you need to delete the source file after a successful copy, create a custom activity to delete the file, and use the activity in the pipeline.</span></span> 

## <a name="enable-connectivity"></a><span data-ttu-id="5295d-112">Povolit připojení</span><span class="sxs-lookup"><span data-stu-id="5295d-112">Enable connectivity</span></span>
<span data-ttu-id="5295d-113">Pokud přesouváte data ze **místní** serveru FTP do cloudu dat uložit (například do Azure Blob storage), instalaci a používání brány pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="5295d-113">If you are moving data from an **on-premises** FTP server to a cloud data store (for example, to Azure Blob storage), install and use Data Management Gateway.</span></span> <span data-ttu-id="5295d-114">Brána pro správu dat je klientský agent, který je nainstalován v místním počítači a umožňuje cloudové služby pro připojení k místnímu prostředku.</span><span class="sxs-lookup"><span data-stu-id="5295d-114">The Data Management Gateway is a client agent that is installed on your on-premises machine, and it allows cloud services to connect to an on-premises resource.</span></span> <span data-ttu-id="5295d-115">Podrobnosti najdete v tématu [Brána pro správu dat](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="5295d-115">For details, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span> <span data-ttu-id="5295d-116">Podrobné pokyny k nastavení registrace brány a pomocí, najdete v tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="5295d-116">For step-by-step instructions on setting up the gateway and using it, see [Moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="5295d-117">Používáte bránu pro připojení k serveru FTP, i, pokud je server v Azure infrastruktury jako služby (IaaS) virtuální počítač (VM).</span><span class="sxs-lookup"><span data-stu-id="5295d-117">You use the gateway to connect to an FTP server, even if the server is on an Azure infrastructure as a service (IaaS) virtual machine (VM).</span></span>

<span data-ttu-id="5295d-118">Je možné nainstalovat bránu na stejný místní počítač nebo virtuální počítač IaaS jako FTP server.</span><span class="sxs-lookup"><span data-stu-id="5295d-118">It is possible to install the gateway on the same on-premises machine or IaaS VM as the FTP server.</span></span> <span data-ttu-id="5295d-119">Nicméně doporučujeme nainstalovat bránu na samostatný počítač nebo virtuální počítač IaaS předejít sporu prostředků a pro dosažení vyššího výkonu.</span><span class="sxs-lookup"><span data-stu-id="5295d-119">However, we recommend that you install the gateway on a separate machine or IaaS VM to avoid resource contention, and for better performance.</span></span> <span data-ttu-id="5295d-120">Při instalaci brány na samostatný počítač, na počítač byste měli mít přístup k serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="5295d-120">When you install the gateway on a separate machine, the machine should be able to access the FTP server.</span></span>

## <a name="get-started"></a><span data-ttu-id="5295d-121">Začínáme</span><span class="sxs-lookup"><span data-stu-id="5295d-121">Get started</span></span>
<span data-ttu-id="5295d-122">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z zdroje FTP pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5295d-122">You can create a pipeline with a copy activity that moves data from an FTP source by using different tools or APIs.</span></span>

<span data-ttu-id="5295d-123">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním služby Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="5295d-123">The easiest way to create a pipeline is to use the **Data Factory Copy Wizard**.</span></span> <span data-ttu-id="5295d-124">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé návod.</span><span class="sxs-lookup"><span data-stu-id="5295d-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough.</span></span>

<span data-ttu-id="5295d-125">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="5295d-125">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="5295d-126">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="5295d-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="5295d-127">Jestli používáte nástroje nebo rozhraní API, proveďte následující kroky k vytvoření kanálu, který přesouvá data ze zdrojového úložiště dat do úložiště dat podřízený:</span><span class="sxs-lookup"><span data-stu-id="5295d-127">Whether you use the tools or APIs, perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="5295d-128">Vytvoření **propojené služby** propojení vstupní a výstupní data ukládá do data factory.</span><span class="sxs-lookup"><span data-stu-id="5295d-128">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="5295d-129">Vytvoření **datové sady** představují vstupní a výstupní data pro kopírování.</span><span class="sxs-lookup"><span data-stu-id="5295d-129">Create **datasets** to represent input and output data for the copy operation.</span></span>
3. <span data-ttu-id="5295d-130">Vytvoření **kanálu** s aktivitou kopírování, která přebírá datovou sadu jako vstup a datovou sadu jako výstup.</span><span class="sxs-lookup"><span data-stu-id="5295d-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="5295d-131">Když použijete průvodce, jsou automaticky vytvoří definice JSON pro tyto entity služby Data Factory (propojené služby, datové sady a kanál).</span><span class="sxs-lookup"><span data-stu-id="5295d-131">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="5295d-132">Pokud používáte rozhraní API (s výjimkou .NET API) nebo nástroje, definujete tyto entity služby Data Factory pomocí formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="5295d-132">When you use tools or APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span> <span data-ttu-id="5295d-133">Ukázku s definicemi JSON entit služby Data Factory, které se používají ke zkopírování dat z úložiště dat služby FTP, najdete [JSON příklad: kopírování dat ze serveru FTP do objektu blob Azure](#json-example-copy-data-from-ftp-server-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="5295d-133">For a sample with JSON definitions for Data Factory entities that are used to copy data from an FTP data store, see the [JSON example: Copy data from FTP server to Azure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) section of this article.</span></span>

> [!NOTE]
> <span data-ttu-id="5295d-134">Podrobnosti o podporovaných formátech souborů a komprese používat najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span><span class="sxs-lookup"><span data-stu-id="5295d-134">For details about supported file and compression formats to use, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span></span>

<span data-ttu-id="5295d-135">Následující části obsahují podrobnosti o vlastnostech formátu JSON, které se používají pro definování konkrétní entity služby Data Factory k serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="5295d-135">The following sections provide details about JSON properties that are used to define Data Factory entities specific to FTP.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="5295d-136">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="5295d-136">Linked service properties</span></span>
<span data-ttu-id="5295d-137">Následující tabulka popisuje elementy JSON, které jsou specifické pro služby FTP propojený.</span><span class="sxs-lookup"><span data-stu-id="5295d-137">The following table describes JSON elements specific to an FTP linked service.</span></span>

| <span data-ttu-id="5295d-138">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5295d-138">Property</span></span> | <span data-ttu-id="5295d-139">Popis</span><span class="sxs-lookup"><span data-stu-id="5295d-139">Description</span></span> | <span data-ttu-id="5295d-140">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5295d-140">Required</span></span> | <span data-ttu-id="5295d-141">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5295d-141">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5295d-142">type</span><span class="sxs-lookup"><span data-stu-id="5295d-142">type</span></span> |<span data-ttu-id="5295d-143">Tuto možnost nastavíte na Server_ftp.</span><span class="sxs-lookup"><span data-stu-id="5295d-143">Set this to FtpServer.</span></span> |<span data-ttu-id="5295d-144">Ano</span><span class="sxs-lookup"><span data-stu-id="5295d-144">Yes</span></span> |&nbsp; |
| <span data-ttu-id="5295d-145">hostitele</span><span class="sxs-lookup"><span data-stu-id="5295d-145">host</span></span> |<span data-ttu-id="5295d-146">Zadejte název nebo IP adresu serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="5295d-146">Specify the name or IP address of the FTP server.</span></span> |<span data-ttu-id="5295d-147">Ano</span><span class="sxs-lookup"><span data-stu-id="5295d-147">Yes</span></span> |&nbsp; |
| <span data-ttu-id="5295d-148">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5295d-148">authenticationType</span></span> |<span data-ttu-id="5295d-149">Zadejte typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="5295d-149">Specify the authentication type.</span></span> |<span data-ttu-id="5295d-150">Ano</span><span class="sxs-lookup"><span data-stu-id="5295d-150">Yes</span></span> |<span data-ttu-id="5295d-151">Anonymní, základní</span><span class="sxs-lookup"><span data-stu-id="5295d-151">Basic, Anonymous</span></span> |
| <span data-ttu-id="5295d-152">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5295d-152">username</span></span> |<span data-ttu-id="5295d-153">Zadejte uživatele, který má přístup k serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="5295d-153">Specify the user who has access to the FTP server.</span></span> |<span data-ttu-id="5295d-154">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-154">No</span></span> |&nbsp; |
| <span data-ttu-id="5295d-155">heslo</span><span class="sxs-lookup"><span data-stu-id="5295d-155">password</span></span> |<span data-ttu-id="5295d-156">Zadejte heslo pro uživatele (uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="5295d-156">Specify the password for the user (username).</span></span> |<span data-ttu-id="5295d-157">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-157">No</span></span> |&nbsp; |
| <span data-ttu-id="5295d-158">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5295d-158">encryptedCredential</span></span> |<span data-ttu-id="5295d-159">Zadejte šifrované pověření pro přístup k serveru FTP.</span><span class="sxs-lookup"><span data-stu-id="5295d-159">Specify the encrypted credential to access the FTP server.</span></span> |<span data-ttu-id="5295d-160">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-160">No</span></span> |&nbsp; |
| <span data-ttu-id="5295d-161">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5295d-161">gatewayName</span></span> |<span data-ttu-id="5295d-162">Zadejte název brány v Brána pro správu dat pro připojení k serveru FTP na místě.</span><span class="sxs-lookup"><span data-stu-id="5295d-162">Specify the name of the gateway in Data Management Gateway to connect to an on-premises FTP server.</span></span> |<span data-ttu-id="5295d-163">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-163">No</span></span> |&nbsp; |
| <span data-ttu-id="5295d-164">port</span><span class="sxs-lookup"><span data-stu-id="5295d-164">port</span></span> |<span data-ttu-id="5295d-165">Zadejte port, na kterém naslouchá FTP server.</span><span class="sxs-lookup"><span data-stu-id="5295d-165">Specify the port on which the FTP server is listening.</span></span> |<span data-ttu-id="5295d-166">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-166">No</span></span> |<span data-ttu-id="5295d-167">21</span><span class="sxs-lookup"><span data-stu-id="5295d-167">21</span></span> |
| <span data-ttu-id="5295d-168">enableSsl</span><span class="sxs-lookup"><span data-stu-id="5295d-168">enableSsl</span></span> |<span data-ttu-id="5295d-169">Určete, zda chcete pomocí funkce FTP přes kanál SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="5295d-169">Specify whether to use FTP over an SSL/TLS channel.</span></span> |<span data-ttu-id="5295d-170">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-170">No</span></span> |<span data-ttu-id="5295d-171">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="5295d-171">true</span></span> |
| <span data-ttu-id="5295d-172">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="5295d-172">enableServerCertificateValidation</span></span> |<span data-ttu-id="5295d-173">Určete, zda chcete povolit ověřování certifikátu serveru SSL při použití FTP přes kanál SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="5295d-173">Specify whether to enable server SSL certificate validation when you are using FTP over SSL/TLS channel.</span></span> |<span data-ttu-id="5295d-174">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-174">No</span></span> |<span data-ttu-id="5295d-175">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="5295d-175">true</span></span> |

### <a name="use-anonymous-authentication"></a><span data-ttu-id="5295d-176">Anonymní ověřování použijte</span><span class="sxs-lookup"><span data-stu-id="5295d-176">Use Anonymous authentication</span></span>

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

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a><span data-ttu-id="5295d-177">Použít uživatelské jméno a heslo v prostém textu pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="5295d-177">Use username and password in plain text for basic authentication</span></span>

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

### <a name="use-port-enablessl-enableservercertificatevalidation"></a><span data-ttu-id="5295d-178">Použijte port, enableSsl, enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="5295d-178">Use port, enableSsl, enableServerCertificateValidation</span></span>

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

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a><span data-ttu-id="5295d-179">Použití encryptedCredential pro ověřování a brány</span><span class="sxs-lookup"><span data-stu-id="5295d-179">Use encryptedCredential for authentication and gateway</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="5295d-180">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="5295d-180">Dataset properties</span></span>
<span data-ttu-id="5295d-181">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části [vytváření datových sad](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="5295d-181">For a full list of sections and properties available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="5295d-182">Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5295d-182">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="5295d-183">**Rámci typeProperties** části se liší pro jednotlivé typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5295d-183">The **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="5295d-184">Poskytuje informace, které jsou specifické pro daný typ datové sady.</span><span class="sxs-lookup"><span data-stu-id="5295d-184">It provides information that is specific to the dataset type.</span></span> <span data-ttu-id="5295d-185">**Rámci typeProperties** části datové sady typu **sdílení souborů** má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="5295d-185">The **typeProperties** section for a dataset of type **FileShare** has the following properties:</span></span>

| <span data-ttu-id="5295d-186">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5295d-186">Property</span></span> | <span data-ttu-id="5295d-187">Popis</span><span class="sxs-lookup"><span data-stu-id="5295d-187">Description</span></span> | <span data-ttu-id="5295d-188">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5295d-188">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5295d-189">folderPath</span><span class="sxs-lookup"><span data-stu-id="5295d-189">folderPath</span></span> |<span data-ttu-id="5295d-190">Dílčí cestou ke složce.</span><span class="sxs-lookup"><span data-stu-id="5295d-190">Subpath to the folder.</span></span> <span data-ttu-id="5295d-191">Použít řídicí znak ' \ ' pro speciální znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="5295d-191">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="5295d-192">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="5295d-192">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="5295d-193">Tato vlastnost se můžete kombinovat **partitionBy** cesty ke složce zadat podle řez spuštění a ukončení hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="5295d-193">You can combine this property with **partitionBy** to have folder paths based on slice start and end date-times.</span></span> |<span data-ttu-id="5295d-194">Ano</span><span class="sxs-lookup"><span data-stu-id="5295d-194">Yes</span></span> |
| <span data-ttu-id="5295d-195">fileName</span><span class="sxs-lookup"><span data-stu-id="5295d-195">fileName</span></span> |<span data-ttu-id="5295d-196">Zadejte název souboru do **folderPath** Pokud chcete, aby v tabulce odkazovat na konkrétní soubor ve složce.</span><span class="sxs-lookup"><span data-stu-id="5295d-196">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="5295d-197">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, tabulka odkazuje na všechny soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="5295d-197">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="5295d-198">Když **fileName** není zadané pro datovou sadu výstupů, je název vygenerovaný soubor v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="5295d-198">When **fileName** is not specified for an output dataset, the name of the generated file is in the following format:</span></span> <br/><br/><span data-ttu-id="5295d-199">Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="5295d-199">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="5295d-200">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-200">No</span></span> |
| <span data-ttu-id="5295d-201">fileFilter</span><span class="sxs-lookup"><span data-stu-id="5295d-201">fileFilter</span></span> |<span data-ttu-id="5295d-202">Zadejte filtr pro umožňuje vybrat podmnožinu souborů v **folderPath**, ne všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="5295d-202">Specify a filter to be used to select a subset of files in the **folderPath**, rather than all files.</span></span><br/><br/><span data-ttu-id="5295d-203">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="5295d-203">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="5295d-204">Příklad 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="5295d-204">Example 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="5295d-205">Příklad 2:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="5295d-205">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="5295d-206">**fileFilter** lze použít pro datové sadě služby vstupní sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="5295d-206">**fileFilter** is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="5295d-207">Tato vlastnost není podporována s Hadoop Distributed File System (HDFS).</span><span class="sxs-lookup"><span data-stu-id="5295d-207">This property is not supported with Hadoop Distributed File System (HDFS).</span></span> |<span data-ttu-id="5295d-208">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-208">No</span></span> |
| <span data-ttu-id="5295d-209">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="5295d-209">partitionedBy</span></span> |<span data-ttu-id="5295d-210">Slouží k zadání dynamický **folderPath** a **fileName** pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="5295d-210">Used to specify a dynamic **folderPath** and **fileName** for time series data.</span></span> <span data-ttu-id="5295d-211">Například můžete zadat **folderPath** , je pro každou hodinu dat parametry.</span><span class="sxs-lookup"><span data-stu-id="5295d-211">For example, you can specify a **folderPath** that is parameterized for every hour of data.</span></span> |<span data-ttu-id="5295d-212">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-212">No</span></span> |
| <span data-ttu-id="5295d-213">Formát</span><span class="sxs-lookup"><span data-stu-id="5295d-213">format</span></span> | <span data-ttu-id="5295d-214">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5295d-214">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="5295d-215">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5295d-215">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="5295d-216">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="5295d-216">For more information, see the [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="5295d-217">Pokud chcete zkopírovat soubory, jako jsou mezi souborové úložiště (binární kopie), přejděte v části formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5295d-217">If you want to copy files as they are between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="5295d-218">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-218">No</span></span> |
| <span data-ttu-id="5295d-219">Komprese</span><span class="sxs-lookup"><span data-stu-id="5295d-219">compression</span></span> | <span data-ttu-id="5295d-220">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="5295d-220">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="5295d-221">Podporované typy jsou **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**, a jsou podporované úrovně **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="5295d-221">Supported types are **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**, and supported levels are **Optimal** and **Fastest**.</span></span> <span data-ttu-id="5295d-222">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="5295d-222">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="5295d-223">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-223">No</span></span> |
| <span data-ttu-id="5295d-224">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="5295d-224">useBinaryTransfer</span></span> |<span data-ttu-id="5295d-225">Určete, zda chcete použít režim binární přenosu.</span><span class="sxs-lookup"><span data-stu-id="5295d-225">Specify whether to use the binary transfer mode.</span></span> <span data-ttu-id="5295d-226">Hodnoty jsou pro binárního režimu (to je výchozí hodnota), na hodnotu true a false pro ASCII.</span><span class="sxs-lookup"><span data-stu-id="5295d-226">The values are true for binary mode (this is the default value), and false for ASCII.</span></span> <span data-ttu-id="5295d-227">Tuto vlastnost lze použít pouze v případě typu přidružené propojené služby typu: Server_ftp.</span><span class="sxs-lookup"><span data-stu-id="5295d-227">This property can only be used when the associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="5295d-228">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-228">No</span></span> |

> [!NOTE]
> <span data-ttu-id="5295d-229">**Název souboru** a **fileFilter** nelze používat současně.</span><span class="sxs-lookup"><span data-stu-id="5295d-229">**fileName** and **fileFilter** cannot be used simultaneously.</span></span>

### <a name="use-the-partionedby-property"></a><span data-ttu-id="5295d-230">Použijte vlastnost partionedBy</span><span class="sxs-lookup"><span data-stu-id="5295d-230">Use the partionedBy property</span></span>
<span data-ttu-id="5295d-231">Jak je uvedeno v předchozí části, můžete zadat dynamický **folderPath** a **fileName** pro data časové řady s **partitionedBy** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5295d-231">As mentioned in the previous section, you can specify a dynamic **folderPath** and **fileName** for time series data with the **partitionedBy** property.</span></span>

<span data-ttu-id="5295d-232">Další informace o datové sady času řady, plánování a řezy najdete v tématu [vytváření datových sad](data-factory-create-datasets.md), [plánování a provádění](data-factory-scheduling-and-execution.md), a [vytváření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="5295d-232">To learn about time series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="5295d-233">Ukázka 1</span><span class="sxs-lookup"><span data-stu-id="5295d-233">Sample 1</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="5295d-234">V tomto příkladu {řez} se nahradí hodnotu objektu pro vytváření dat systému proměnné SliceStart, ve formátu určeném (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="5295d-234">In this example, {Slice} is replaced with the value of Data Factory system variable SliceStart, in the format specified (YYYYMMDDHH).</span></span> <span data-ttu-id="5295d-235">Vlastnosti SliceStart odkazuje na spuštění řezu.</span><span class="sxs-lookup"><span data-stu-id="5295d-235">The SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="5295d-236">Cesta ke složce se liší pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="5295d-236">The folder path is different for each slice.</span></span> <span data-ttu-id="5295d-237">(Například wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.)</span><span class="sxs-lookup"><span data-stu-id="5295d-237">(For example, wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.)</span></span>

#### <a name="sample-2"></a><span data-ttu-id="5295d-238">Ukázka 2</span><span class="sxs-lookup"><span data-stu-id="5295d-238">Sample 2</span></span>

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
<span data-ttu-id="5295d-239">V tomto příkladu jsou extrahován rok, měsíc, den a čas SliceStart do samostatné proměnné, které jsou používány **folderPath** a **fileName** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5295d-239">In this example, the year, month, day, and time of SliceStart are extracted into separate variables that are used by the **folderPath** and **fileName** properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="5295d-240">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="5295d-240">Copy activity properties</span></span>
<span data-ttu-id="5295d-241">Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu [vytváření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="5295d-241">For a full list of sections and properties available for defining activities, see [Creating pipelines](data-factory-create-pipelines.md).</span></span> <span data-ttu-id="5295d-242">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="5295d-242">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="5295d-243">Vlastnosti, které jsou k dispozici v **rámci typeProperties** části aktivity, na druhé straně lišit každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="5295d-243">Properties available in the **typeProperties** section of the activity, on the other hand, vary with each activity type.</span></span> <span data-ttu-id="5295d-244">Pro aktivitu kopírování vlastnosti typu lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="5295d-244">For the copy activity, the type properties vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="5295d-245">Při aktivitě kopírování, pokud je zdroj typu **FileSystemSource**, je k dispozici v této vlastnosti **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5295d-245">In copy activity, when the source is of type **FileSystemSource**, the following property is available in **typeProperties** section:</span></span>

| <span data-ttu-id="5295d-246">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5295d-246">Property</span></span> | <span data-ttu-id="5295d-247">Popis</span><span class="sxs-lookup"><span data-stu-id="5295d-247">Description</span></span> | <span data-ttu-id="5295d-248">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5295d-248">Allowed values</span></span> | <span data-ttu-id="5295d-249">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5295d-249">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5295d-250">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="5295d-250">recursive</span></span> |<span data-ttu-id="5295d-251">Označuje, zda je data načíst rekurzivně z podsložky nebo pouze do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="5295d-251">Indicates whether the data is read recursively from the subfolders, or only from the specified folder.</span></span> |<span data-ttu-id="5295d-252">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5295d-252">True, False (default)</span></span> |<span data-ttu-id="5295d-253">Ne</span><span class="sxs-lookup"><span data-stu-id="5295d-253">No</span></span> |

## <a name="json-example-copy-data-from-ftp-server-to-azure-blob"></a><span data-ttu-id="5295d-254">Příklad JSON: kopírování dat ze serveru FTP do objektu Blob Azure</span><span class="sxs-lookup"><span data-stu-id="5295d-254">JSON example: Copy data from FTP server to Azure Blob</span></span>
<span data-ttu-id="5295d-255">Tento příklad ukazuje postup kopírování dat ze serveru FTP do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="5295d-255">This sample shows how to copy data from an FTP server to Azure Blob storage.</span></span> <span data-ttu-id="5295d-256">Ale data se dají zkopírovat přímo do jakéhokoli z jímky uvádí [podporované úložiště dat a formáty](data-factory-data-movement-activities.md#supported-data-stores-and-formats), pomocí aktivity kopírování v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="5295d-256">However, data can be copied directly to any of the sinks stated in the [supported data stores and formats](data-factory-data-movement-activities.md#supported-data-stores-and-formats), by using the copy activity in Data Factory.</span></span>  

<span data-ttu-id="5295d-257">Následující příklady poskytují ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), nebo [prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="5295d-257">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span></span>

* <span data-ttu-id="5295d-258">Propojené služby typu [Server_ftp](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="5295d-258">A linked service of type [FtpServer](#linked-service-properties)</span></span>
* <span data-ttu-id="5295d-259">Propojené služby typu [azurestorage.](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="5295d-259">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="5295d-260">Vstup [datovou sadu](data-factory-create-datasets.md) typu [sdílení souborů](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="5295d-260">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties)</span></span>
* <span data-ttu-id="5295d-261">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="5295d-261">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="5295d-262">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="5295d-262">A [pipeline](data-factory-create-pipelines.md) with copy activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="5295d-263">Ukázka kopíruje data ze serveru FTP do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="5295d-263">The sample copies data from an FTP server to an Azure blob every hour.</span></span> <span data-ttu-id="5295d-264">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="5295d-264">The JSON properties used in these samples are described in sections following the samples.</span></span>

### <a name="ftp-linked-service"></a><span data-ttu-id="5295d-265">Propojená služba FTP</span><span class="sxs-lookup"><span data-stu-id="5295d-265">FTP linked service</span></span>

<span data-ttu-id="5295d-266">Tento příklad používá základní ověřování, uživatelské jméno a heslo v prostém textu.</span><span class="sxs-lookup"><span data-stu-id="5295d-266">This example uses basic authentication, with the user name and password in plain text.</span></span> <span data-ttu-id="5295d-267">Můžete také použít jednu z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="5295d-267">You can also use one of the following ways:</span></span>

* <span data-ttu-id="5295d-268">Anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="5295d-268">Anonymous authentication</span></span>
* <span data-ttu-id="5295d-269">Základní ověřování s zašifrované přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="5295d-269">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="5295d-270">FTP přes SSL/TLS (FTPS)</span><span class="sxs-lookup"><span data-stu-id="5295d-270">FTP over SSL/TLS (FTPS)</span></span>

<span data-ttu-id="5295d-271">Najdete v článku [FTP propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="5295d-271">See the [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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
### <a name="azure-storage-linked-service"></a><span data-ttu-id="5295d-272">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5295d-272">Azure Storage linked service</span></span>

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
### <a name="ftp-input-dataset"></a><span data-ttu-id="5295d-273">FTP vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="5295d-273">FTP input dataset</span></span>

<span data-ttu-id="5295d-274">Tato datová sada odkazuje na složku FTP `mysharedfolder` a soubor `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="5295d-274">This dataset refers to the FTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="5295d-275">Kanál zkopíruje soubor do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="5295d-275">The pipeline copies the file to the destination.</span></span>

<span data-ttu-id="5295d-276">Nastavení **externí** k **true** informuje služba Data Factory, že je externí k objektu pro vytváření dat datové sady a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="5295d-276">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory, and is not produced by an activity in the data factory.</span></span>

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

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="5295d-277">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="5295d-277">Azure Blob output dataset</span></span>

<span data-ttu-id="5295d-278">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="5295d-278">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="5295d-279">Cesta ke složce pro tento objekt blob se dynamicky vyhodnotí, podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="5295d-279">The folder path for the blob is dynamically evaluated, based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="5295d-280">Cesta ke složce používá rok, měsíc, den a čas části čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="5295d-280">The folder path uses the year, month, day, and hours parts of the start time.</span></span>

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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a><span data-ttu-id="5295d-281">Aktivita kopírování v kanálu s podřízený zdroj a objektů blob systému souborů</span><span class="sxs-lookup"><span data-stu-id="5295d-281">A copy activity in a pipeline with file system source and blob sink</span></span>

<span data-ttu-id="5295d-282">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="5295d-282">The pipeline contains a copy activity that is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="5295d-283">V definici JSON kanálu **zdroj** je typ nastaven na **FileSystemSource**a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="5295d-283">In the pipeline JSON definition, the **source** type is set to **FileSystemSource**, and the **sink** type is set to **BlobSink**.</span></span>

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
> <span data-ttu-id="5295d-284">Mapování sloupců z datové sady zdroje na sloupce ze sady jímku dat naleznete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="5295d-284">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5295d-285">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5295d-285">Next steps</span></span>
<span data-ttu-id="5295d-286">Viz následující články:</span><span class="sxs-lookup"><span data-stu-id="5295d-286">See the following articles:</span></span>

* <span data-ttu-id="5295d-287">Další informace o klíčových faktorů této dopad výkon přesun dat (aktivita kopírování) v objektu pro vytváření dat a různé způsoby, jak ji optimalizovat, najdete v článku [zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="5295d-287">To learn about key factors that impact performance of data movement (copy activity) in Data Factory, and various ways to optimize it, see the [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>

* <span data-ttu-id="5295d-288">Podrobné pokyny pro vytvoření kanálu s aktivitou kopírování najdete v tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="5295d-288">For step-by-step instructions for creating a pipeline with a copy activity, see the [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
