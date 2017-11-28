---
title: aaaMove data ze zdroje HTTP - Azure | Microsoft Docs
description: "Informace o tom, jak zdroje dat toomove z místní nebo cloudové HTTP pomocí Azure Data Factory."
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
ms.openlocfilehash: e39b9cbff870aef4be91938cacff39a2fd12d64a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a><span data-ttu-id="6c927-103">Přesun dat z HTTP zdroje pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="6c927-103">Move data from an HTTP source using Azure Data Factory</span></span>
<span data-ttu-id="6c927-104">Tento článek popisuje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z tooa koncový bod HTTP lokální/Cloudová podporované úložiště dat jímky.</span><span class="sxs-lookup"><span data-stu-id="6c927-104">This article outlines how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises/cloud HTTP endpoint tooa supported sink data store.</span></span> <span data-ttu-id="6c927-105">Tento článek vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s kopie aktivity a hello seznamu úložiště dat, které jsou podporované jako zdroje nebo jímky.</span><span class="sxs-lookup"><span data-stu-id="6c927-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and hello list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="6c927-106">Objekt pro vytváření dat aktuálně podporuje pouze přesun dat z protokolu HTTP zdroje tooother datová úložiště, ale není přesouvání dat od ostatních dat ukládá cílové tooan HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c927-106">Data factory currently supports only moving data from an HTTP source tooother data stores, but not moving data from other data stores tooan HTTP destination.</span></span>

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="6c927-107">Podporované scénáře a typy ověřování</span><span class="sxs-lookup"><span data-stu-id="6c927-107">Supported scenarios and authentication types</span></span>
<span data-ttu-id="6c927-108">Můžete použít tento HTTP konektor tooretrieve data z **cloudové i místní koncový bod HTTP/s** pomocí protokolu HTTP **získat** nebo **POST** metoda.</span><span class="sxs-lookup"><span data-stu-id="6c927-108">You can use this HTTP connector tooretrieve data from **both cloud and on-premises HTTP/s endpoint** by using HTTP **GET** or **POST** method.</span></span> <span data-ttu-id="6c927-109">jsou podporovány následující typy ověřování Hello: **anonymní**, **základní**, **Digest**, **Windows**, a  **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="6c927-109">hello following authentication types are supported: **Anonymous**, **Basic**, **Digest**, **Windows**, and **ClientCertificate**.</span></span> <span data-ttu-id="6c927-110">Všimněte si hello rozdíl mezi tento konektor a hello [konektor tabulky webových](data-factory-web-table-connector.md) je: hello druhé je použité tooextract tabulku obsahu z webové stránky HTML.</span><span class="sxs-lookup"><span data-stu-id="6c927-110">Note hello difference between this connector and hello [Web table connector](data-factory-web-table-connector.md) is: hello latter is used tooextract table content from web HTML page.</span></span>

<span data-ttu-id="6c927-111">Při kopírování dat z místní koncový bod protokolu HTTP, je nutné nainstalovat brána pro správu dat v hello místní prostředí nebo Azure virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6c927-111">When copying data from an on-premises HTTP endpoint, you need install a Data Management Gateway in hello on-premises environment/Azure VM.</span></span> <span data-ttu-id="6c927-112">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) toolearn článek o Brána pro správu dat a podrobné pokyny k nastavení hello brány.</span><span class="sxs-lookup"><span data-stu-id="6c927-112">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

## <a name="getting-started"></a><span data-ttu-id="6c927-113">Začínáme</span><span class="sxs-lookup"><span data-stu-id="6c927-113">Getting started</span></span>
<span data-ttu-id="6c927-114">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z zdroje HTTP pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6c927-114">You can create a pipeline with a copy activity that moves data from an HTTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="6c927-115">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="6c927-115">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="6c927-116">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="6c927-116">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

- <span data-ttu-id="6c927-117">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="6c927-117">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="6c927-118">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="6c927-118">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> <span data-ttu-id="6c927-119">JSON – ukázky toocopy data z tooAzure HTTP zdroj úložiště objektů Blob, najdete v části [JSON příklady](#json-examples) části Tento článek.</span><span class="sxs-lookup"><span data-stu-id="6c927-119">For JSON samples toocopy data from HTTP source tooAzure Blob Storage, see [JSON examples](#json-examples) section of this articles.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="6c927-120">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="6c927-120">Linked service properties</span></span>
<span data-ttu-id="6c927-121">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooHTTP propojené služby.</span><span class="sxs-lookup"><span data-stu-id="6c927-121">hello following table provides description for JSON elements specific tooHTTP linked service.</span></span>

| <span data-ttu-id="6c927-122">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6c927-122">Property</span></span> | <span data-ttu-id="6c927-123">Popis</span><span class="sxs-lookup"><span data-stu-id="6c927-123">Description</span></span> | <span data-ttu-id="6c927-124">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6c927-124">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6c927-125">type</span><span class="sxs-lookup"><span data-stu-id="6c927-125">type</span></span> | <span data-ttu-id="6c927-126">vlastnost typu Hello musí být nastavena na: `Http`.</span><span class="sxs-lookup"><span data-stu-id="6c927-126">hello type property must be set to: `Http`.</span></span> | <span data-ttu-id="6c927-127">Ano</span><span class="sxs-lookup"><span data-stu-id="6c927-127">Yes</span></span> |
| <span data-ttu-id="6c927-128">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="6c927-128">url</span></span> | <span data-ttu-id="6c927-129">Základní adresa URL toohello webového serveru</span><span class="sxs-lookup"><span data-stu-id="6c927-129">Base URL toohello Web Server</span></span> | <span data-ttu-id="6c927-130">Ano</span><span class="sxs-lookup"><span data-stu-id="6c927-130">Yes</span></span> |
| <span data-ttu-id="6c927-131">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="6c927-131">authenticationType</span></span> | <span data-ttu-id="6c927-132">Určuje typ ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="6c927-132">Specifies hello authentication type.</span></span> <span data-ttu-id="6c927-133">Povolené hodnoty jsou: **anonymní**, **základní**, **Digest**, **Windows**, **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="6c927-133">Allowed values are: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**.</span></span> <br><br> <span data-ttu-id="6c927-134">V uvedeném pořadí odkazovat toosections dál v této tabulce na další vlastnosti a ukázky JSON pro tyto typy ověřování.</span><span class="sxs-lookup"><span data-stu-id="6c927-134">Refer toosections below this table on more properties and JSON samples for those authentication types respectively.</span></span> | <span data-ttu-id="6c927-135">Ano</span><span class="sxs-lookup"><span data-stu-id="6c927-135">Yes</span></span> |
| <span data-ttu-id="6c927-136">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="6c927-136">enableServerCertificateValidation</span></span> | <span data-ttu-id="6c927-137">Zadejte, zda, tooenable serveru SSL ověření certifikátu, pokud je zdroj HTTPS webového serveru</span><span class="sxs-lookup"><span data-stu-id="6c927-137">Specify whether tooenable server SSL certificate validation if source is HTTPS Web Server</span></span> | <span data-ttu-id="6c927-138">Ne, výchozí hodnota je true</span><span class="sxs-lookup"><span data-stu-id="6c927-138">No, default is true</span></span> |
| <span data-ttu-id="6c927-139">gatewayName</span><span class="sxs-lookup"><span data-stu-id="6c927-139">gatewayName</span></span> | <span data-ttu-id="6c927-140">Název hello Brána pro správu dat tooconnect tooan místní zdroje pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c927-140">Name of hello Data Management Gateway tooconnect tooan on-premises HTTP source.</span></span> | <span data-ttu-id="6c927-141">Ano, pokud kopírování dat z místního zdroje HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c927-141">Yes if copying data from an on-premises HTTP source.</span></span> |
| <span data-ttu-id="6c927-142">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="6c927-142">encryptedCredential</span></span> | <span data-ttu-id="6c927-143">Šifrovaný přihlašovací údaj tooaccess hello koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c927-143">Encrypted credential tooaccess hello HTTP endpoint.</span></span> <span data-ttu-id="6c927-144">Automaticky vygenerované při konfiguraci hello ověřovací informace v kopie Průvodce nebo hello ClickOnce automaticky otevřeném okně. dialog.</span><span class="sxs-lookup"><span data-stu-id="6c927-144">Auto-generated when you configure hello authentication information in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="6c927-145">Ne.</span><span class="sxs-lookup"><span data-stu-id="6c927-145">No.</span></span> <span data-ttu-id="6c927-146">Platí jenom v případě, že kopírování dat z místního serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c927-146">Apply only when copying data from an on-premises HTTP server.</span></span> |

<span data-ttu-id="6c927-147">V tématu [přesun dat mezi místní zdroje a hello cloudu s Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md) podrobnosti o nastavení přihlašovacích údajů pro zdroj dat konektor místní HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c927-147">See [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for details about setting credentials for on-premises HTTP connector data source.</span></span>

### <a name="using-basic-digest-or-windows-authentication"></a><span data-ttu-id="6c927-148">Pomocí ověřování Basic, ověřování algoritmem Digest nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="6c927-148">Using Basic, Digest, or Windows authentication</span></span>

<span data-ttu-id="6c927-149">Nastavit `authenticationType` jako `Basic`, `Digest`, nebo `Windows`a zadejte následující vlastnosti kromě hello HTTP konektor obecné těch, které jsou zavedené výše hello:</span><span class="sxs-lookup"><span data-stu-id="6c927-149">Set `authenticationType` as `Basic`, `Digest`, or `Windows`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="6c927-150">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6c927-150">Property</span></span> | <span data-ttu-id="6c927-151">Popis</span><span class="sxs-lookup"><span data-stu-id="6c927-151">Description</span></span> | <span data-ttu-id="6c927-152">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6c927-152">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6c927-153">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="6c927-153">username</span></span> | <span data-ttu-id="6c927-154">Uživatelské jméno tooaccess hello koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c927-154">Username tooaccess hello HTTP endpoint.</span></span> | <span data-ttu-id="6c927-155">Ano</span><span class="sxs-lookup"><span data-stu-id="6c927-155">Yes</span></span> |
| <span data-ttu-id="6c927-156">heslo</span><span class="sxs-lookup"><span data-stu-id="6c927-156">password</span></span> | <span data-ttu-id="6c927-157">Heslo pro uživatele hello (uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="6c927-157">Password for hello user (username).</span></span> | <span data-ttu-id="6c927-158">Ano</span><span class="sxs-lookup"><span data-stu-id="6c927-158">Yes</span></span> |

#### <a name="example-using-basic-digest-or-windows-authentication"></a><span data-ttu-id="6c927-159">Příklad: použití ověřování Basic, ověřování algoritmem Digest nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="6c927-159">Example: using Basic, Digest, or Windows authentication</span></span>

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

### <a name="using-clientcertificate-authentication"></a><span data-ttu-id="6c927-160">Pomocí ClientCertificate ověřování</span><span class="sxs-lookup"><span data-stu-id="6c927-160">Using ClientCertificate authentication</span></span>

<span data-ttu-id="6c927-161">toouse základní ověřování, nastavit `authenticationType` jako `ClientCertificate`a zadejte následující vlastnosti kromě hello HTTP konektor obecné těch, které jsou zavedené výše hello:</span><span class="sxs-lookup"><span data-stu-id="6c927-161">toouse basic authentication, set `authenticationType` as `ClientCertificate`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="6c927-162">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6c927-162">Property</span></span> | <span data-ttu-id="6c927-163">Popis</span><span class="sxs-lookup"><span data-stu-id="6c927-163">Description</span></span> | <span data-ttu-id="6c927-164">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6c927-164">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6c927-165">embeddedCertData</span><span class="sxs-lookup"><span data-stu-id="6c927-165">embeddedCertData</span></span> | <span data-ttu-id="6c927-166">Hello obsah kódováním Base64 binárních dat souboru hello Personal Information Exchange (PFX).</span><span class="sxs-lookup"><span data-stu-id="6c927-166">hello Base64-encoded contents of binary data of hello Personal Information Exchange (PFX) file.</span></span> | <span data-ttu-id="6c927-167">Zadejte buď hello `embeddedCertData` nebo `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="6c927-167">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="6c927-168">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="6c927-168">certThumbprint</span></span> | <span data-ttu-id="6c927-169">Hello kryptografický otisk certifikátu hello, který byl nainstalován v úložišti certifikátů počítače brány.</span><span class="sxs-lookup"><span data-stu-id="6c927-169">hello thumbprint of hello certificate that was installed on your gateway machine’s cert store.</span></span> <span data-ttu-id="6c927-170">Platí jenom v případě, že kopírování dat z místního zdroje HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c927-170">Apply only when copying data from an on-premises HTTP source.</span></span> | <span data-ttu-id="6c927-171">Zadejte buď hello `embeddedCertData` nebo `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="6c927-171">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="6c927-172">heslo</span><span class="sxs-lookup"><span data-stu-id="6c927-172">password</span></span> | <span data-ttu-id="6c927-173">Heslo přidružené k hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6c927-173">Password associated with hello certificate.</span></span> | <span data-ttu-id="6c927-174">Ne</span><span class="sxs-lookup"><span data-stu-id="6c927-174">No</span></span> |

<span data-ttu-id="6c927-175">Pokud používáte `certThumbprint` pro ověřování a hello certifikát nainstalován v osobním úložišti hello hello místního počítače, je třeba služba brány pro toohello toogrant hello oprávnění ke čtení:</span><span class="sxs-lookup"><span data-stu-id="6c927-175">If you use `certThumbprint` for authentication and hello certificate is installed in hello personal store of hello local computer, you need toogrant hello read permission toohello gateway service:</span></span>

1. <span data-ttu-id="6c927-176">Spusťte konzolu Microsoft Management Console (MMC).</span><span class="sxs-lookup"><span data-stu-id="6c927-176">Launch Microsoft Management Console (MMC).</span></span> <span data-ttu-id="6c927-177">Přidat hello **certifikáty** modul snap-in tohoto cíle hello **místního počítače**.</span><span class="sxs-lookup"><span data-stu-id="6c927-177">Add hello **Certificates** snap-in that targets hello **Local Computer**.</span></span>
2. <span data-ttu-id="6c927-178">Rozbalte položku **certifikáty**, **osobní**a klikněte na tlačítko **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="6c927-178">Expand **Certificates**, **Personal**, and click **Certificates**.</span></span>
3. <span data-ttu-id="6c927-179">Klikněte pravým tlačítkem na certifikát hello z osobního úložiště hello a vyberte **všechny úlohy**->**spravovat privátní klíče...**</span><span class="sxs-lookup"><span data-stu-id="6c927-179">Right-click hello certificate from hello personal store, and select **All Tasks**->**Manage Private Keys...**</span></span>
3. <span data-ttu-id="6c927-180">Na hello **zabezpečení** přidejte hello uživatelský účet, pod kterým běží hostitelská služba brány správy dat s certifikátem toohello hello přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="6c927-180">On hello **Security** tab, add hello user account under which Data Management Gateway Host Service is running with hello read access toohello certificate.</span></span>  

#### <a name="example-using-client-certificate"></a><span data-ttu-id="6c927-181">Příklad: pomocí klientského certifikátu</span><span class="sxs-lookup"><span data-stu-id="6c927-181">Example: using client certificate</span></span>
<span data-ttu-id="6c927-182">Tato propojená služba propojuje vaše data factory tooan místní HTTP webový server.</span><span class="sxs-lookup"><span data-stu-id="6c927-182">This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="6c927-183">Používá klientský certifikát, který je nainstalován na počítači hello s brána správy dat nainstalována.</span><span class="sxs-lookup"><span data-stu-id="6c927-183">It uses a client certificate that is installed on hello machine with Data Management Gateway installed.</span></span>

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

#### <a name="example-using-client-certificate-in-a-file"></a><span data-ttu-id="6c927-184">Příklad: pomocí klientského certifikátu do souboru</span><span class="sxs-lookup"><span data-stu-id="6c927-184">Example: using client certificate in a file</span></span>
<span data-ttu-id="6c927-185">Tato propojená služba propojuje vaše data factory tooan místní HTTP webový server.</span><span class="sxs-lookup"><span data-stu-id="6c927-185">This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="6c927-186">Soubor certifikátu klienta na počítači hello používá s brána správy dat nainstalována.</span><span class="sxs-lookup"><span data-stu-id="6c927-186">It uses a client certificate file on hello machine with Data Management Gateway installed.</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="6c927-187">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="6c927-187">Dataset properties</span></span>
<span data-ttu-id="6c927-188">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6c927-188">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="6c927-189">Oddíly, jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu (Azure SQL Azure blob, tabulky Azure, atd.).</span><span class="sxs-lookup"><span data-stu-id="6c927-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="6c927-190">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu a poskytuje informace o umístění hello hello dat v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="6c927-190">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="6c927-191">rámci typeProperties Hello část datové sady typ **Http** má následující vlastnosti hello</span><span class="sxs-lookup"><span data-stu-id="6c927-191">hello typeProperties section for dataset of type **Http** has hello following properties</span></span>

| <span data-ttu-id="6c927-192">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6c927-192">Property</span></span> | <span data-ttu-id="6c927-193">Popis</span><span class="sxs-lookup"><span data-stu-id="6c927-193">Description</span></span> | <span data-ttu-id="6c927-194">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6c927-194">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="6c927-195">type</span><span class="sxs-lookup"><span data-stu-id="6c927-195">type</span></span> | <span data-ttu-id="6c927-196">Zadaný typ hello hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6c927-196">Specified hello type of hello dataset.</span></span> <span data-ttu-id="6c927-197">musí být nastaven příliš`Http`.</span><span class="sxs-lookup"><span data-stu-id="6c927-197">must be set too`Http`.</span></span> | <span data-ttu-id="6c927-198">Ano</span><span class="sxs-lookup"><span data-stu-id="6c927-198">Yes</span></span> |
| <span data-ttu-id="6c927-199">relativeUrl</span><span class="sxs-lookup"><span data-stu-id="6c927-199">relativeUrl</span></span> | <span data-ttu-id="6c927-200">Relativní adresa URL toohello prostředek obsahující hello data.</span><span class="sxs-lookup"><span data-stu-id="6c927-200">A relative URL toohello resource that contains hello data.</span></span> <span data-ttu-id="6c927-201">Pokud cesta není zadána, je použít jenom hello adrese URL zadané v definici hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="6c927-201">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> <br><br> <span data-ttu-id="6c927-202">tooconstruct dynamické adresy URL, můžete použít [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md), například "relativeUrl": "$$Text.Format ('/ my/sestavy? měsíc = {0:yyyy}-{0:MM} & fmt = csv', SliceStart)".</span><span class="sxs-lookup"><span data-stu-id="6c927-202">tooconstruct dynamic URL, you can use [Data Factory functions and system variables](data-factory-functions-variables.md), e.g. "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)".</span></span> | <span data-ttu-id="6c927-203">Ne</span><span class="sxs-lookup"><span data-stu-id="6c927-203">No</span></span> |
| <span data-ttu-id="6c927-204">requestMethod</span><span class="sxs-lookup"><span data-stu-id="6c927-204">requestMethod</span></span> | <span data-ttu-id="6c927-205">Metoda HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c927-205">Http method.</span></span> <span data-ttu-id="6c927-206">Povolené hodnoty jsou **získat** nebo **POST**.</span><span class="sxs-lookup"><span data-stu-id="6c927-206">Allowed values are **GET** or **POST**.</span></span> | <span data-ttu-id="6c927-207">Ne.</span><span class="sxs-lookup"><span data-stu-id="6c927-207">No.</span></span> <span data-ttu-id="6c927-208">Výchozí hodnota je `GET`.</span><span class="sxs-lookup"><span data-stu-id="6c927-208">Default is `GET`.</span></span> |
| <span data-ttu-id="6c927-209">additionalHeaders</span><span class="sxs-lookup"><span data-stu-id="6c927-209">additionalHeaders</span></span> | <span data-ttu-id="6c927-210">Další hlavičky žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c927-210">Additional HTTP request headers.</span></span> | <span data-ttu-id="6c927-211">Ne</span><span class="sxs-lookup"><span data-stu-id="6c927-211">No</span></span> |
| <span data-ttu-id="6c927-212">RequestBody</span><span class="sxs-lookup"><span data-stu-id="6c927-212">requestBody</span></span> | <span data-ttu-id="6c927-213">Text pro požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c927-213">Body for HTTP request.</span></span> | <span data-ttu-id="6c927-214">Ne</span><span class="sxs-lookup"><span data-stu-id="6c927-214">No</span></span> |
| <span data-ttu-id="6c927-215">Formát</span><span class="sxs-lookup"><span data-stu-id="6c927-215">format</span></span> | <span data-ttu-id="6c927-216">Pokud chcete, aby toosimply **načtení dat hello z koncový bod HTTP jako-je** bez analýza ho, přeskočte tento formát nastavení.</span><span class="sxs-lookup"><span data-stu-id="6c927-216">If you want toosimply **retrieve hello data from HTTP endpoint as-is** without parsing it, skip this format settings.</span></span> <br><br> <span data-ttu-id="6c927-217">Pokud chcete tooparse hello HTTP odpovědi obsahu během kopírování, jsou podporovány následující typy formátu hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="6c927-217">If you want tooparse hello HTTP response content during copy, hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="6c927-218">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="6c927-218">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> |<span data-ttu-id="6c927-219">Ne</span><span class="sxs-lookup"><span data-stu-id="6c927-219">No</span></span> |
| <span data-ttu-id="6c927-220">Komprese</span><span class="sxs-lookup"><span data-stu-id="6c927-220">compression</span></span> | <span data-ttu-id="6c927-221">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="6c927-221">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="6c927-222">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="6c927-222">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="6c927-223">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="6c927-223">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="6c927-224">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="6c927-224">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="6c927-225">Ne</span><span class="sxs-lookup"><span data-stu-id="6c927-225">No</span></span> |

### <a name="example-using-hello-get-default-method"></a><span data-ttu-id="6c927-226">Příklad: pomocí hello metodu GET (výchozí)</span><span class="sxs-lookup"><span data-stu-id="6c927-226">Example: using hello GET (default) method</span></span>

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

### <a name="example-using-hello-post-method"></a><span data-ttu-id="6c927-227">Příklad: pomocí metody POST hello</span><span class="sxs-lookup"><span data-stu-id="6c927-227">Example: using hello POST method</span></span>

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

## <a name="copy-activity-properties"></a><span data-ttu-id="6c927-228">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="6c927-228">Copy activity properties</span></span>
<span data-ttu-id="6c927-229">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6c927-229">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="6c927-230">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="6c927-230">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="6c927-231">Vlastnosti, které jsou k dispozici v hello **rámci typeProperties** části hello aktivit na hello se každý typ aktivity lišit podle druhé straně.</span><span class="sxs-lookup"><span data-stu-id="6c927-231">Properties available in hello **typeProperties** section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="6c927-232">Pro aktivitu kopírování budou lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="6c927-232">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="6c927-233">V současné době po hello zdroj v aktivitě kopírování typu **HttpSource**, jsou podporovány následující vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="6c927-233">Currently, when hello source in copy activity is of type **HttpSource**, hello following properties are supported.</span></span>

| <span data-ttu-id="6c927-234">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6c927-234">Property</span></span> | <span data-ttu-id="6c927-235">Popis</span><span class="sxs-lookup"><span data-stu-id="6c927-235">Description</span></span> | <span data-ttu-id="6c927-236">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6c927-236">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="6c927-237">httpRequestTimeout</span><span class="sxs-lookup"><span data-stu-id="6c927-237">httpRequestTimeout</span></span> | <span data-ttu-id="6c927-238">Hello vypršení časového limitu (časový interval) pro tooget požadavku HTTP hello odpověď.</span><span class="sxs-lookup"><span data-stu-id="6c927-238">hello timeout (TimeSpan) for hello HTTP request tooget a response.</span></span> <span data-ttu-id="6c927-239">Je hello časový limit tooget odpověď, nebyla hello časový limit tooread data odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6c927-239">It is hello timeout tooget a response, not hello timeout tooread response data.</span></span> | <span data-ttu-id="6c927-240">Ne.</span><span class="sxs-lookup"><span data-stu-id="6c927-240">No.</span></span> <span data-ttu-id="6c927-241">Výchozí hodnota: 00:01:40</span><span class="sxs-lookup"><span data-stu-id="6c927-241">Default value: 00:01:40</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="6c927-242">Podporované formáty souborů a komprese</span><span class="sxs-lookup"><span data-stu-id="6c927-242">Supported file and compression formats</span></span>
<span data-ttu-id="6c927-243">V tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článek na podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6c927-243">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="6c927-244">Příklady JSON</span><span class="sxs-lookup"><span data-stu-id="6c927-244">JSON examples</span></span>
<span data-ttu-id="6c927-245">Následující ukázka Hello poskytují definice JSON ukázka používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="6c927-245">hello following example provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="6c927-246">Ukazují jak zdroje dat toocopy z HTTP tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="6c927-246">They show how toocopy data from HTTP source tooAzure Blob Storage.</span></span> <span data-ttu-id="6c927-247">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů tooany z hello jímky uvádí [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="6c927-247">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-http-source-tooazure-blob-storage"></a><span data-ttu-id="6c927-248">Příklad: Kopírování dat z tooAzure HTTP zdroj úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="6c927-248">Example: Copy data from HTTP source tooAzure Blob Storage</span></span>
<span data-ttu-id="6c927-249">Hello řešení Data Factory pro tato ukázka obsahuje hello následující entity služby Data Factory:</span><span class="sxs-lookup"><span data-stu-id="6c927-249">hello Data Factory solution for this sample contains hello following Data Factory entities:</span></span>

1. <span data-ttu-id="6c927-250">Propojené služby typu [HTTP](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="6c927-250">A linked service of type [HTTP](#linked-service-properties).</span></span>
2. <span data-ttu-id="6c927-251">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="6c927-251">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="6c927-252">Vstup [datovou sadu](data-factory-create-datasets.md) typu [Http](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6c927-252">An input [dataset](data-factory-create-datasets.md) of type [Http](#dataset-properties).</span></span>
4. <span data-ttu-id="6c927-253">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6c927-253">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="6c927-254">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [HttpSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="6c927-254">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [HttpSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="6c927-255">Ukázka Hello zkopíruje data z tooan zdroje HTTP objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="6c927-255">hello sample copies data from an HTTP source tooan Azure blob every hour.</span></span> <span data-ttu-id="6c927-256">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="6c927-256">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

### <a name="http-linked-service"></a><span data-ttu-id="6c927-257">Služba HTTP propojené</span><span class="sxs-lookup"><span data-stu-id="6c927-257">HTTP linked service</span></span>
<span data-ttu-id="6c927-258">Tento příklad, že používá hello HTTP propojená služba s anonymní ověřování.</span><span class="sxs-lookup"><span data-stu-id="6c927-258">This example uses hello HTTP linked service with anonymous authentication.</span></span> <span data-ttu-id="6c927-259">V tématu [HTTP propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="6c927-259">See [HTTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

### <a name="azure-storage-linked-service"></a><span data-ttu-id="6c927-260">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6c927-260">Azure Storage linked service</span></span>

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

### <a name="http-input-dataset"></a><span data-ttu-id="6c927-261">Vstupní datové sady HTTP</span><span class="sxs-lookup"><span data-stu-id="6c927-261">HTTP input dataset</span></span>
<span data-ttu-id="6c927-262">Nastavení **externí** příliš**true** informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="6c927-262">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="6c927-263">Výstupní datová sada Azure Blob</span><span class="sxs-lookup"><span data-stu-id="6c927-263">Azure Blob output dataset</span></span>

<span data-ttu-id="6c927-264">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="6c927-264">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

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

### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="6c927-265">Kanál s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="6c927-265">Pipeline with Copy activity</span></span>

<span data-ttu-id="6c927-266">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="6c927-266">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="6c927-267">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**HttpSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="6c927-267">In hello pipeline JSON definition, hello **source** type is set too**HttpSource** and **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="6c927-268">V tématu [HttpSource](#copy-activity-properties) hello seznam vlastnostech podporovaných zprostředkovatelem hello HttpSource.</span><span class="sxs-lookup"><span data-stu-id="6c927-268">See [HttpSource](#copy-activity-properties) for hello list of properties supported by hello HttpSource.</span></span>

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
        "description": "Copy from an HTTP source tooan Azure blob",
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
> <span data-ttu-id="6c927-269">toomap sloupce z toocolumns datové sady zdroje z podřízený datové sady, najdete v části [mapování sloupců datovou sadu v Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="6c927-269">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="6c927-270">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="6c927-270">Performance and Tuning</span></span>
<span data-ttu-id="6c927-271">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="6c927-271">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
