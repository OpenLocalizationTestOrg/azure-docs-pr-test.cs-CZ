---
title: "Přesun dat ze zdroje HTTP - Azure | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z místní nebo zdroji HTTP cloudu pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 3cc1bd293868b0bb093f617ac12e16c26780fc89
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a><span data-ttu-id="db45e-103">Přesun dat z HTTP zdroje pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="db45e-103">Move data from an HTTP source using Azure Data Factory</span></span>
<span data-ttu-id="db45e-104">Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat z koncový bod HTTP lokální/Cloudová k úložišti dat podporovaných jímky.</span><span class="sxs-lookup"><span data-stu-id="db45e-104">This article outlines how to use the Copy Activity in Azure Data Factory to move data from an on-premises/cloud HTTP endpoint to a supported sink data store.</span></span> <span data-ttu-id="db45e-105">Tento článek vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který uvádí obecný přehled přesun dat s aktivitou kopírování a seznam úložiště dat, které jsou podporované jako zdroje nebo jímky.</span><span class="sxs-lookup"><span data-stu-id="db45e-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and the list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="db45e-106">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat z HTTP zdroje k jiným úložištím dat, ale není přesouvání dat od ostatních dat ukládá na umístění protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-106">Data factory currently supports only moving data from an HTTP source to other data stores, but not moving data from other data stores to an HTTP destination.</span></span>

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="db45e-107">Podporované scénáře a typy ověřování</span><span class="sxs-lookup"><span data-stu-id="db45e-107">Supported scenarios and authentication types</span></span>
<span data-ttu-id="db45e-108">Tento konektor HTTP můžete použít k načtení dat z **cloudové i místní koncový bod HTTP/s** pomocí protokolu HTTP **získat** nebo **POST** metoda.</span><span class="sxs-lookup"><span data-stu-id="db45e-108">You can use this HTTP connector to retrieve data from **both cloud and on-premises HTTP/s endpoint** by using HTTP **GET** or **POST** method.</span></span> <span data-ttu-id="db45e-109">Jsou podporovány následující typy ověřování: **anonymní**, **základní**, **Digest**, **Windows**, a  **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="db45e-109">The following authentication types are supported: **Anonymous**, **Basic**, **Digest**, **Windows**, and **ClientCertificate**.</span></span> <span data-ttu-id="db45e-110">Všimněte si rozdíl mezi tohoto konektoru a [konektor tabulky webových](data-factory-web-table-connector.md) je: se používá k extrahování obsahu tabulky z HTML webové stránky.</span><span class="sxs-lookup"><span data-stu-id="db45e-110">Note the difference between this connector and the [Web table connector](data-factory-web-table-connector.md) is: the latter is used to extract table content from web HTML page.</span></span>

<span data-ttu-id="db45e-111">Při kopírování dat z místní koncový bod protokolu HTTP, je nutné nainstalovat brána pro správu dat v prostředí nebo Azure místní počítač.</span><span class="sxs-lookup"><span data-stu-id="db45e-111">When copying data from an on-premises HTTP endpoint, you need install a Data Management Gateway in the on-premises environment/Azure VM.</span></span> <span data-ttu-id="db45e-112">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku se dozvíte o Brána pro správu dat a podrobné pokyny o nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="db45e-112">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

## <a name="getting-started"></a><span data-ttu-id="db45e-113">Začínáme</span><span class="sxs-lookup"><span data-stu-id="db45e-113">Getting started</span></span>
<span data-ttu-id="db45e-114">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z zdroje HTTP pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="db45e-114">You can create a pipeline with a copy activity that moves data from an HTTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="db45e-115">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="db45e-115">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="db45e-116">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="db45e-116">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

- <span data-ttu-id="db45e-117">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="db45e-117">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="db45e-118">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="db45e-118">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> <span data-ttu-id="db45e-119">JSON ukázky ke zkopírování dat z HTTP zdroje do Azure Blob Storage, najdete v části [JSON příklady](#json-examples) části Tento článek.</span><span class="sxs-lookup"><span data-stu-id="db45e-119">For JSON samples to copy data from HTTP source to Azure Blob Storage, see [JSON examples](#json-examples) section of this articles.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="db45e-120">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="db45e-120">Linked service properties</span></span>
<span data-ttu-id="db45e-121">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro protokol HTTP propojené služby.</span><span class="sxs-lookup"><span data-stu-id="db45e-121">The following table provides description for JSON elements specific to HTTP linked service.</span></span>

| <span data-ttu-id="db45e-122">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="db45e-122">Property</span></span> | <span data-ttu-id="db45e-123">Popis</span><span class="sxs-lookup"><span data-stu-id="db45e-123">Description</span></span> | <span data-ttu-id="db45e-124">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="db45e-124">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="db45e-125">type</span><span class="sxs-lookup"><span data-stu-id="db45e-125">type</span></span> | <span data-ttu-id="db45e-126">Vlastnost typu musí být nastavena na: `Http`.</span><span class="sxs-lookup"><span data-stu-id="db45e-126">The type property must be set to: `Http`.</span></span> | <span data-ttu-id="db45e-127">Ano</span><span class="sxs-lookup"><span data-stu-id="db45e-127">Yes</span></span> |
| <span data-ttu-id="db45e-128">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="db45e-128">url</span></span> | <span data-ttu-id="db45e-129">Základní adresu URL na webový server</span><span class="sxs-lookup"><span data-stu-id="db45e-129">Base URL to the Web Server</span></span> | <span data-ttu-id="db45e-130">Ano</span><span class="sxs-lookup"><span data-stu-id="db45e-130">Yes</span></span> |
| <span data-ttu-id="db45e-131">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="db45e-131">authenticationType</span></span> | <span data-ttu-id="db45e-132">Určuje typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="db45e-132">Specifies the authentication type.</span></span> <span data-ttu-id="db45e-133">Povolené hodnoty jsou: **anonymní**, **základní**, **Digest**, **Windows**, **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="db45e-133">Allowed values are: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**.</span></span> <br><br> <span data-ttu-id="db45e-134">Naleznete v části dál v této tabulce na další vlastnosti a ukázky JSON pro tyto typy ověřování v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="db45e-134">Refer to sections below this table on more properties and JSON samples for those authentication types respectively.</span></span> | <span data-ttu-id="db45e-135">Ano</span><span class="sxs-lookup"><span data-stu-id="db45e-135">Yes</span></span> |
| <span data-ttu-id="db45e-136">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="db45e-136">enableServerCertificateValidation</span></span> | <span data-ttu-id="db45e-137">Určete, zda chcete povolit ověřování certifikátu protokolu SSL serveru, pokud je zdroj HTTPS webového serveru</span><span class="sxs-lookup"><span data-stu-id="db45e-137">Specify whether to enable server SSL certificate validation if source is HTTPS Web Server</span></span> | <span data-ttu-id="db45e-138">Ne, výchozí hodnota je true</span><span class="sxs-lookup"><span data-stu-id="db45e-138">No, default is true</span></span> |
| <span data-ttu-id="db45e-139">gatewayName</span><span class="sxs-lookup"><span data-stu-id="db45e-139">gatewayName</span></span> | <span data-ttu-id="db45e-140">Název brány pro správu dat pro připojení k místnímu zdroji HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-140">Name of the Data Management Gateway to connect to an on-premises HTTP source.</span></span> | <span data-ttu-id="db45e-141">Ano, pokud kopírování dat z místního zdroje HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-141">Yes if copying data from an on-premises HTTP source.</span></span> |
| <span data-ttu-id="db45e-142">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="db45e-142">encryptedCredential</span></span> | <span data-ttu-id="db45e-143">Šifrovaný přihlašovací údaje pro přístup k koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-143">Encrypted credential to access the HTTP endpoint.</span></span> <span data-ttu-id="db45e-144">Automaticky vygenerované při konfiguraci informace o ověřování v Průvodce kopírováním nebo dialogové okno místní ClickOnce.</span><span class="sxs-lookup"><span data-stu-id="db45e-144">Auto-generated when you configure the authentication information in copy wizard or the ClickOnce popup dialog.</span></span> | <span data-ttu-id="db45e-145">Ne.</span><span class="sxs-lookup"><span data-stu-id="db45e-145">No.</span></span> <span data-ttu-id="db45e-146">Platí jenom v případě, že kopírování dat z místního serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-146">Apply only when copying data from an on-premises HTTP server.</span></span> |

<span data-ttu-id="db45e-147">V tématu [přesun dat mezi místní zdroje a cloudu s Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md) podrobnosti o nastavení přihlašovacích údajů pro zdroj dat konektor místní HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-147">See [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for details about setting credentials for on-premises HTTP connector data source.</span></span>

### <a name="using-basic-digest-or-windows-authentication"></a><span data-ttu-id="db45e-148">Pomocí ověřování Basic, ověřování algoritmem Digest nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="db45e-148">Using Basic, Digest, or Windows authentication</span></span>

<span data-ttu-id="db45e-149">Nastavit `authenticationType` jako `Basic`, `Digest`, nebo `Windows`a zadejte následující vlastnosti kromě konektor HTTP obecné ty, které jsou zavedené výše:</span><span class="sxs-lookup"><span data-stu-id="db45e-149">Set `authenticationType` as `Basic`, `Digest`, or `Windows`, and specify the following properties besides the HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="db45e-150">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="db45e-150">Property</span></span> | <span data-ttu-id="db45e-151">Popis</span><span class="sxs-lookup"><span data-stu-id="db45e-151">Description</span></span> | <span data-ttu-id="db45e-152">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="db45e-152">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="db45e-153">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="db45e-153">username</span></span> | <span data-ttu-id="db45e-154">Uživatelské jméno pro přístup k koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-154">Username to access the HTTP endpoint.</span></span> | <span data-ttu-id="db45e-155">Ano</span><span class="sxs-lookup"><span data-stu-id="db45e-155">Yes</span></span> |
| <span data-ttu-id="db45e-156">heslo</span><span class="sxs-lookup"><span data-stu-id="db45e-156">password</span></span> | <span data-ttu-id="db45e-157">Heslo pro uživatele (uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="db45e-157">Password for the user (username).</span></span> | <span data-ttu-id="db45e-158">Ano</span><span class="sxs-lookup"><span data-stu-id="db45e-158">Yes</span></span> |

#### <a name="example-using-basic-digest-or-windows-authentication"></a><span data-ttu-id="db45e-159">Příklad: použití ověřování Basic, ověřování algoritmem Digest nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="db45e-159">Example: using Basic, Digest, or Windows authentication</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a><span data-ttu-id="db45e-160">Pomocí ClientCertificate ověřování</span><span class="sxs-lookup"><span data-stu-id="db45e-160">Using ClientCertificate authentication</span></span>

<span data-ttu-id="db45e-161">Chcete-li základní ověřování použijte, nastavte `authenticationType` jako `ClientCertificate`a zadejte následující vlastnosti kromě konektor HTTP obecné ty, které jsou zavedené výše:</span><span class="sxs-lookup"><span data-stu-id="db45e-161">To use basic authentication, set `authenticationType` as `ClientCertificate`, and specify the following properties besides the HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="db45e-162">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="db45e-162">Property</span></span> | <span data-ttu-id="db45e-163">Popis</span><span class="sxs-lookup"><span data-stu-id="db45e-163">Description</span></span> | <span data-ttu-id="db45e-164">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="db45e-164">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="db45e-165">embeddedCertData</span><span class="sxs-lookup"><span data-stu-id="db45e-165">embeddedCertData</span></span> | <span data-ttu-id="db45e-166">Obsah s kódováním base64, pomocí binárních dat soubor Personal Information Exchange (PFX).</span><span class="sxs-lookup"><span data-stu-id="db45e-166">The Base64-encoded contents of binary data of the Personal Information Exchange (PFX) file.</span></span> | <span data-ttu-id="db45e-167">Zadejte buď `embeddedCertData` nebo `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="db45e-167">Specify either the `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="db45e-168">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="db45e-168">certThumbprint</span></span> | <span data-ttu-id="db45e-169">Kryptografický otisk certifikátu, který byl nainstalován v úložišti certifikátů počítače brány.</span><span class="sxs-lookup"><span data-stu-id="db45e-169">The thumbprint of the certificate that was installed on your gateway machine’s cert store.</span></span> <span data-ttu-id="db45e-170">Platí jenom v případě, že kopírování dat z místního zdroje HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-170">Apply only when copying data from an on-premises HTTP source.</span></span> | <span data-ttu-id="db45e-171">Zadejte buď `embeddedCertData` nebo `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="db45e-171">Specify either the `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="db45e-172">heslo</span><span class="sxs-lookup"><span data-stu-id="db45e-172">password</span></span> | <span data-ttu-id="db45e-173">Heslo přidružené k certifikátu.</span><span class="sxs-lookup"><span data-stu-id="db45e-173">Password associated with the certificate.</span></span> | <span data-ttu-id="db45e-174">Ne</span><span class="sxs-lookup"><span data-stu-id="db45e-174">No</span></span> |

<span data-ttu-id="db45e-175">Pokud používáte `certThumbprint` pro ověřování a certifikát nainstalovaný v osobním úložišti místního počítače, musí udělit oprávnění ke čtení ke službě brány:</span><span class="sxs-lookup"><span data-stu-id="db45e-175">If you use `certThumbprint` for authentication and the certificate is installed in the personal store of the local computer, you need to grant the read permission to the gateway service:</span></span>

1. <span data-ttu-id="db45e-176">Spusťte konzolu Microsoft Management Console (MMC).</span><span class="sxs-lookup"><span data-stu-id="db45e-176">Launch Microsoft Management Console (MMC).</span></span> <span data-ttu-id="db45e-177">Přidat **certifikáty** modul snap-in zacílený **místního počítače**.</span><span class="sxs-lookup"><span data-stu-id="db45e-177">Add the **Certificates** snap-in that targets the **Local Computer**.</span></span>
2. <span data-ttu-id="db45e-178">Rozbalte položku **certifikáty**, **osobní**a klikněte na tlačítko **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="db45e-178">Expand **Certificates**, **Personal**, and click **Certificates**.</span></span>
3. <span data-ttu-id="db45e-179">Klikněte pravým tlačítkem na certifikát z osobního úložiště a vyberte **všechny úlohy**->**spravovat privátní klíče...**</span><span class="sxs-lookup"><span data-stu-id="db45e-179">Right-click the certificate from the personal store, and select **All Tasks**->**Manage Private Keys...**</span></span>
3. <span data-ttu-id="db45e-180">Na **zabezpečení** přidejte uživatelský účet, pod kterou je spuštěna hostitelská služba brány správy dat s přístupem pro čtení k certifikátu.</span><span class="sxs-lookup"><span data-stu-id="db45e-180">On the **Security** tab, add the user account under which Data Management Gateway Host Service is running with the read access to the certificate.</span></span>  

#### <a name="example-using-client-certificate"></a><span data-ttu-id="db45e-181">Příklad: pomocí klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="db45e-181">Example: using client certificate</span></span>
<span data-ttu-id="db45e-182">Tato propojená služba propojuje datovou továrnu místní webový server HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-182">This linked service links your data factory to an on-premises HTTP web server.</span></span> <span data-ttu-id="db45e-183">Používá klientský certifikát, který je nainstalován na počítači s brána správy dat nainstalována.</span><span class="sxs-lookup"><span data-stu-id="db45e-183">It uses a client certificate that is installed on the machine with Data Management Gateway installed.</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"

        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a><span data-ttu-id="db45e-184">Příklad: pomocí klientského certifikátu do souboru</span><span class="sxs-lookup"><span data-stu-id="db45e-184">Example: using client certificate in a file</span></span>
<span data-ttu-id="db45e-185">Tato propojená služba propojuje datovou továrnu místní webový server HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-185">This linked service links your data factory to an on-premises HTTP web server.</span></span> <span data-ttu-id="db45e-186">Používá soubor certifikátu klienta na počítači s brána správy dat nainstalována.</span><span class="sxs-lookup"><span data-stu-id="db45e-186">It uses a client certificate file on the machine with Data Management Gateway installed.</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="db45e-187">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="db45e-187">Dataset properties</span></span>
<span data-ttu-id="db45e-188">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="db45e-188">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="db45e-189">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="db45e-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="db45e-190">**Rámci typeProperties** oddílu se liší pro jednotlivé typy datovou sadu a informace o umístění dat v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="db45e-190">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="db45e-191">Rámci typeProperties část datové sady typ **Http** má následující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="db45e-191">The typeProperties section for dataset of type **Http** has the following properties</span></span>

| <span data-ttu-id="db45e-192">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="db45e-192">Property</span></span> | <span data-ttu-id="db45e-193">Popis</span><span class="sxs-lookup"><span data-stu-id="db45e-193">Description</span></span> | <span data-ttu-id="db45e-194">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="db45e-194">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="db45e-195">type</span><span class="sxs-lookup"><span data-stu-id="db45e-195">type</span></span> | <span data-ttu-id="db45e-196">Zadaný typ datové sady.</span><span class="sxs-lookup"><span data-stu-id="db45e-196">Specified the type of the dataset.</span></span> <span data-ttu-id="db45e-197">musí být nastavena na `Http`.</span><span class="sxs-lookup"><span data-stu-id="db45e-197">must be set to `Http`.</span></span> | <span data-ttu-id="db45e-198">Ano</span><span class="sxs-lookup"><span data-stu-id="db45e-198">Yes</span></span> |
| <span data-ttu-id="db45e-199">relativeUrl</span><span class="sxs-lookup"><span data-stu-id="db45e-199">relativeUrl</span></span> | <span data-ttu-id="db45e-200">Relativní adresa URL k prostředku, který obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="db45e-200">A relative URL to the resource that contains the data.</span></span> <span data-ttu-id="db45e-201">Pokud cesta není zadána, je použít jenom adresu URL, zadaný v definici propojené služby.</span><span class="sxs-lookup"><span data-stu-id="db45e-201">When path is not specified, only the URL specified in the linked service definition is used.</span></span> <br><br> <span data-ttu-id="db45e-202">Vytvořit dynamické adresy URL, můžete použít [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md), například "relativeUrl": "$$Text.Format ('/ my/sestavy? měsíc = {0:yyyy}-{0:MM} & fmt = csv', SliceStart)".</span><span class="sxs-lookup"><span data-stu-id="db45e-202">To construct dynamic URL, you can use [Data Factory functions and system variables](data-factory-functions-variables.md), e.g. "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)".</span></span> | <span data-ttu-id="db45e-203">Ne</span><span class="sxs-lookup"><span data-stu-id="db45e-203">No</span></span> |
| <span data-ttu-id="db45e-204">requestMethod</span><span class="sxs-lookup"><span data-stu-id="db45e-204">requestMethod</span></span> | <span data-ttu-id="db45e-205">Metoda HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-205">Http method.</span></span> <span data-ttu-id="db45e-206">Povolené hodnoty jsou **získat** nebo **POST**.</span><span class="sxs-lookup"><span data-stu-id="db45e-206">Allowed values are **GET** or **POST**.</span></span> | <span data-ttu-id="db45e-207">Ne.</span><span class="sxs-lookup"><span data-stu-id="db45e-207">No.</span></span> <span data-ttu-id="db45e-208">Výchozí hodnota je `GET`.</span><span class="sxs-lookup"><span data-stu-id="db45e-208">Default is `GET`.</span></span> |
| <span data-ttu-id="db45e-209">additionalHeaders</span><span class="sxs-lookup"><span data-stu-id="db45e-209">additionalHeaders</span></span> | <span data-ttu-id="db45e-210">Další hlavičky žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-210">Additional HTTP request headers.</span></span> | <span data-ttu-id="db45e-211">Ne</span><span class="sxs-lookup"><span data-stu-id="db45e-211">No</span></span> |
| <span data-ttu-id="db45e-212">RequestBody</span><span class="sxs-lookup"><span data-stu-id="db45e-212">requestBody</span></span> | <span data-ttu-id="db45e-213">Text pro požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-213">Body for HTTP request.</span></span> | <span data-ttu-id="db45e-214">Ne</span><span class="sxs-lookup"><span data-stu-id="db45e-214">No</span></span> |
| <span data-ttu-id="db45e-215">Formát</span><span class="sxs-lookup"><span data-stu-id="db45e-215">format</span></span> | <span data-ttu-id="db45e-216">Pokud chcete jednoduše **načtou data z koncový bod HTTP jako-je** bez analýza ho, přeskočte tento formát nastavení.</span><span class="sxs-lookup"><span data-stu-id="db45e-216">If you want to simply **retrieve the data from HTTP endpoint as-is** without parsing it, skip this format settings.</span></span> <br><br> <span data-ttu-id="db45e-217">Pokud chcete analyzovat během kopírování obsahu odpovědi HTTP, jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="db45e-217">If you want to parse the HTTP response content during copy, the following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="db45e-218">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="db45e-218">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> |<span data-ttu-id="db45e-219">Ne</span><span class="sxs-lookup"><span data-stu-id="db45e-219">No</span></span> |
| <span data-ttu-id="db45e-220">Komprese</span><span class="sxs-lookup"><span data-stu-id="db45e-220">compression</span></span> | <span data-ttu-id="db45e-221">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="db45e-221">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="db45e-222">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="db45e-222">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="db45e-223">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="db45e-223">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="db45e-224">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="db45e-224">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="db45e-225">Ne</span><span class="sxs-lookup"><span data-stu-id="db45e-225">No</span></span> |

### <a name="example-using-the-get-default-method"></a><span data-ttu-id="db45e-226">Příklad: použití metody GET (výchozí)</span><span class="sxs-lookup"><span data-stu-id="db45e-226">Example: using the GET (default) method</span></span>

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

### <a name="example-using-the-post-method"></a><span data-ttu-id="db45e-227">Příklad: pomocí metody POST</span><span class="sxs-lookup"><span data-stu-id="db45e-227">Example: using the POST method</span></span>

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
           "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a><span data-ttu-id="db45e-228">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="db45e-228">Copy activity properties</span></span>
<span data-ttu-id="db45e-229">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="db45e-229">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="db45e-230">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="db45e-230">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="db45e-231">Vlastnosti, které jsou k dispozici v **rámci typeProperties** části aktivity na druhé straně lišit každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="db45e-231">Properties available in the **typeProperties** section of the activity on the other hand vary with each activity type.</span></span> <span data-ttu-id="db45e-232">Pro aktivitu kopírování budou lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="db45e-232">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="db45e-233">V současné době po zdroji v aktivitě kopírování typu **HttpSource**, jsou podporovány následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="db45e-233">Currently, when the source in copy activity is of type **HttpSource**, the following properties are supported.</span></span>

| <span data-ttu-id="db45e-234">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="db45e-234">Property</span></span> | <span data-ttu-id="db45e-235">Popis</span><span class="sxs-lookup"><span data-stu-id="db45e-235">Description</span></span> | <span data-ttu-id="db45e-236">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="db45e-236">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="db45e-237">httpRequestTimeout</span><span class="sxs-lookup"><span data-stu-id="db45e-237">httpRequestTimeout</span></span> | <span data-ttu-id="db45e-238">Časový limit (TimeSpan) pro získání odezvy požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="db45e-238">The timeout (TimeSpan) for the HTTP request to get a response.</span></span> <span data-ttu-id="db45e-239">Získání odezvy, není časový limit číst data odpovědi je časový limit.</span><span class="sxs-lookup"><span data-stu-id="db45e-239">It is the timeout to get a response, not the timeout to read response data.</span></span> | <span data-ttu-id="db45e-240">Ne.</span><span class="sxs-lookup"><span data-stu-id="db45e-240">No.</span></span> <span data-ttu-id="db45e-241">Výchozí hodnota: 00:01:40</span><span class="sxs-lookup"><span data-stu-id="db45e-241">Default value: 00:01:40</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="db45e-242">Podporované formáty souborů a komprese</span><span class="sxs-lookup"><span data-stu-id="db45e-242">Supported file and compression formats</span></span>
<span data-ttu-id="db45e-243">V tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článek na podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="db45e-243">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="db45e-244">Příklady JSON</span><span class="sxs-lookup"><span data-stu-id="db45e-244">JSON examples</span></span>
<span data-ttu-id="db45e-245">Následující příklad zadejte ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="db45e-245">The following example provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="db45e-246">Se ukazují, jak zkopírovat data ze zdroje HTTP do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="db45e-246">They show how to copy data from HTTP source to Azure Blob Storage.</span></span> <span data-ttu-id="db45e-247">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="db45e-247">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-http-source-to-azure-blob-storage"></a><span data-ttu-id="db45e-248">Příklad: Kopírování dat ze zdroje HTTP do Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="db45e-248">Example: Copy data from HTTP source to Azure Blob Storage</span></span>
<span data-ttu-id="db45e-249">Řešení Data Factory pro tato ukázka obsahuje následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="db45e-249">The Data Factory solution for this sample contains the following Data Factory entities:</span></span>

1. <span data-ttu-id="db45e-250">Propojené služby typu [HTTP](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="db45e-250">A linked service of type [HTTP](#linked-service-properties).</span></span>
2. <span data-ttu-id="db45e-251">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="db45e-251">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="db45e-252">Vstup [datovou sadu](data-factory-create-datasets.md) typu [Http](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="db45e-252">An input [dataset](data-factory-create-datasets.md) of type [Http](#dataset-properties).</span></span>
4. <span data-ttu-id="db45e-253">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="db45e-253">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="db45e-254">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [HttpSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="db45e-254">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [HttpSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="db45e-255">Ukázka zkopíruje data z HTTP zdroje do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="db45e-255">The sample copies data from an HTTP source to an Azure blob every hour.</span></span> <span data-ttu-id="db45e-256">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="db45e-256">The JSON properties used in these samples are described in sections following the samples.</span></span>

### <a name="http-linked-service"></a><span data-ttu-id="db45e-257">Služba HTTP propojené</span><span class="sxs-lookup"><span data-stu-id="db45e-257">HTTP linked service</span></span>
<span data-ttu-id="db45e-258">Tento příklad používá služba HTTP propojené s anonymní ověřování.</span><span class="sxs-lookup"><span data-stu-id="db45e-258">This example uses the HTTP linked service with anonymous authentication.</span></span> <span data-ttu-id="db45e-259">V tématu [HTTP propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="db45e-259">See [HTTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="db45e-260">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="db45e-260">Azure Storage linked service</span></span>

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

### <a name="http-input-dataset"></a><span data-ttu-id="db45e-261">Vstupní datové sady HTTP</span><span class="sxs-lookup"><span data-stu-id="db45e-261">HTTP input dataset</span></span>
<span data-ttu-id="db45e-262">Nastavení **externí** k **true** služba Data Factory informuje, že datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="db45e-262">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}

```

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="db45e-263">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="db45e-263">Azure Blob output dataset</span></span>

<span data-ttu-id="db45e-264">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="db45e-264">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

```JSON
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="db45e-265">Kanál s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="db45e-265">Pipeline with Copy activity</span></span>

<span data-ttu-id="db45e-266">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="db45e-266">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="db45e-267">V definici JSON kanálu **zdroj** je typ nastaven na **HttpSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="db45e-267">In the pipeline JSON definition, the **source** type is set to **HttpSource** and **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="db45e-268">V tématu [HttpSource](#copy-activity-properties) pro seznam vlastností HttpSource nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="db45e-268">See [HttpSource](#copy-activity-properties) for the list of properties supported by the HttpSource.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "HttpSourceToAzureBlob",
        "description": "Copy from an HTTP source to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "HttpSourceDataInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "HttpSource"
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

> [!NOTE]
> <span data-ttu-id="db45e-269">Mapování sloupců z datové sady zdroje na sloupce ze sady jímku dat naleznete v tématu [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="db45e-269">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="db45e-270">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="db45e-270">Performance and Tuning</span></span>
<span data-ttu-id="db45e-271">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="db45e-271">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
