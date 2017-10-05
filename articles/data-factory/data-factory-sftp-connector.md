---
title: "Přesun dat ze serveru pomocí protokolu SFTP pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom, jak přesunout data z místní nebo serveru pomocí protokolu SFTP cloudu pomocí Azure Data Factory."
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
ms.date: 06/05/2017
ms.author: jingwang
ms.openlocfilehash: 3a73311342489af031ed2ea1489e56292ebf2e09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a><span data-ttu-id="ab419-103">Přesunutí dat ze serveru pomocí protokolu SFTP pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="ab419-103">Move data from an SFTP server using Azure Data Factory</span></span>
<span data-ttu-id="ab419-104">Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory pro přesun dat ze serveru pomocí protokolu SFTP lokální/Cloudová do úložiště dat podporovaných jímky.</span><span class="sxs-lookup"><span data-stu-id="ab419-104">This article outlines how to use the Copy Activity in Azure Data Factory to move data from an on-premises/cloud SFTP server to a supported sink data store.</span></span> <span data-ttu-id="ab419-105">Tento článek vychází [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který uvádí obecný přehled přesun dat s aktivitou kopírování a seznam úložiště dat, které jsou podporované jako zdroje nebo jímky.</span><span class="sxs-lookup"><span data-stu-id="ab419-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and the list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="ab419-106">Objekt pro vytváření dat aktuálně podporuje pouze přesunutí dat ze serveru pomocí protokolu SFTP k jiným úložištím dat, ale ne pro přesun dat z jiných úložišť dat k serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="ab419-106">Data factory currently supports only moving data from an SFTP server to other data stores, but not for moving data from other data stores to an SFTP server.</span></span> <span data-ttu-id="ab419-107">Podporuje místní a cloudové servery pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="ab419-107">It supports both on-premises and cloud SFTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="ab419-108">Aktivita kopírování nedojde k odstranění zdrojového souboru po byl úspěšně zkopírován do cílové.</span><span class="sxs-lookup"><span data-stu-id="ab419-108">Copy Activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="ab419-109">Pokud potřebujete odstranit zdrojový soubor po úspěšné kopie, vytvořte vlastní aktivity odstranit soubor a použijte aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="ab419-109">If you need to delete the source file after a successful copy, create a custom activity to delete the file and use the activity in the pipeline.</span></span> 

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="ab419-110">Podporované scénáře a typy ověřování</span><span class="sxs-lookup"><span data-stu-id="ab419-110">Supported scenarios and authentication types</span></span>
<span data-ttu-id="ab419-111">Tento konektor SFTP můžete použít ke zkopírování dat z **i v cloudu pomocí protokolu SFTP servery a servery pomocí protokolu SFTP místní**.</span><span class="sxs-lookup"><span data-stu-id="ab419-111">You can use this SFTP connector to copy data from **both cloud SFTP servers and on-premises SFTP servers**.</span></span> <span data-ttu-id="ab419-112">**Základní** a **parametru SshPublicKey** typy ověřování jsou podporovány při připojování k serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="ab419-112">**Basic** and **SshPublicKey** authentication types are supported when connecting to the SFTP server.</span></span>

<span data-ttu-id="ab419-113">Při kopírování dat z místního serveru pomocí protokolu SFTP, je nutné nainstalovat brána pro správu dat v prostředí nebo Azure místní počítač.</span><span class="sxs-lookup"><span data-stu-id="ab419-113">When copying data from an on-premises SFTP server, you need install a Data Management Gateway in the on-premises environment/Azure VM.</span></span> <span data-ttu-id="ab419-114">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti na bráně.</span><span class="sxs-lookup"><span data-stu-id="ab419-114">See [Data Management Gateway](data-factory-data-management-gateway.md) for details on the gateway.</span></span> <span data-ttu-id="ab419-115">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku podrobné pokyny k nastavení brány a jeho použití.</span><span class="sxs-lookup"><span data-stu-id="ab419-115">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up the gateway and using it.</span></span>

## <a name="getting-started"></a><span data-ttu-id="ab419-116">Začínáme</span><span class="sxs-lookup"><span data-stu-id="ab419-116">Getting started</span></span>
<span data-ttu-id="ab419-117">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z protokolu SFTP zdroje pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ab419-117">You can create a pipeline with a copy activity that moves data from an SFTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="ab419-118">Nejjednodušší způsob, jak vytvořit kanál je použití **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="ab419-118">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="ab419-119">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírováním data.</span><span class="sxs-lookup"><span data-stu-id="ab419-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

- <span data-ttu-id="ab419-120">Tyto nástroje můžete také použít k vytvoření kanálu: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru**, **.NET API**, a **REST API**.</span><span class="sxs-lookup"><span data-stu-id="ab419-120">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="ab419-121">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="ab419-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> <span data-ttu-id="ab419-122">JSON ukázky ke zkopírování dat z protokolu SFTP serveru do Azure Blob Storage, najdete v části [JSON příklad: kopírování dat ze serveru pomocí protokolu SFTP do objektu blob Azure](#json-example-copy-data-from-sftp-server-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="ab419-122">For JSON samples to copy data from SFTP server to Azure Blob Storage, see [JSON Example: Copy data from SFTP server to Azure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) section of this article.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="ab419-123">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="ab419-123">Linked service properties</span></span>
<span data-ttu-id="ab419-124">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro propojenou službu FTP.</span><span class="sxs-lookup"><span data-stu-id="ab419-124">The following table provides description for JSON elements specific to FTP linked service.</span></span>

| <span data-ttu-id="ab419-125">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ab419-125">Property</span></span> | <span data-ttu-id="ab419-126">Popis</span><span class="sxs-lookup"><span data-stu-id="ab419-126">Description</span></span> | <span data-ttu-id="ab419-127">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ab419-127">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ab419-128">type</span><span class="sxs-lookup"><span data-stu-id="ab419-128">type</span></span> | <span data-ttu-id="ab419-129">Vlastnost typu musí být nastavená na `Sftp`.</span><span class="sxs-lookup"><span data-stu-id="ab419-129">The type property must be set to `Sftp`.</span></span> |<span data-ttu-id="ab419-130">Ano</span><span class="sxs-lookup"><span data-stu-id="ab419-130">Yes</span></span> |
| <span data-ttu-id="ab419-131">hostitele</span><span class="sxs-lookup"><span data-stu-id="ab419-131">host</span></span> | <span data-ttu-id="ab419-132">Název nebo IP adresa serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="ab419-132">Name or IP address of the SFTP server.</span></span> |<span data-ttu-id="ab419-133">Ano</span><span class="sxs-lookup"><span data-stu-id="ab419-133">Yes</span></span> |
| <span data-ttu-id="ab419-134">port</span><span class="sxs-lookup"><span data-stu-id="ab419-134">port</span></span> |<span data-ttu-id="ab419-135">Port, na kterém naslouchá server pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="ab419-135">Port on which the SFTP server is listening.</span></span> <span data-ttu-id="ab419-136">Výchozí hodnota je: 21</span><span class="sxs-lookup"><span data-stu-id="ab419-136">The default value is: 21</span></span> |<span data-ttu-id="ab419-137">Ne</span><span class="sxs-lookup"><span data-stu-id="ab419-137">No</span></span> |
| <span data-ttu-id="ab419-138">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="ab419-138">authenticationType</span></span> |<span data-ttu-id="ab419-139">Zadejte typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="ab419-139">Specify authentication type.</span></span> <span data-ttu-id="ab419-140">Povolené hodnoty: **základní**, **parametru SshPublicKey**.</span><span class="sxs-lookup"><span data-stu-id="ab419-140">Allowed values: **Basic**, **SshPublicKey**.</span></span> <br><br> <span data-ttu-id="ab419-141">Odkazovat na [základní ověřování pomocí](#using-basic-authentication) a [pomocí SSH ověření veřejného klíče](#using-ssh-public-key-authentication) částech na další vlastnosti a ukázky JSON v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="ab419-141">Refer to [Using basic authentication](#using-basic-authentication) and [Using SSH public key authentication](#using-ssh-public-key-authentication) sections on more properties and JSON samples respectively.</span></span> |<span data-ttu-id="ab419-142">Ano</span><span class="sxs-lookup"><span data-stu-id="ab419-142">Yes</span></span> |
| <span data-ttu-id="ab419-143">skipHostKeyValidation</span><span class="sxs-lookup"><span data-stu-id="ab419-143">skipHostKeyValidation</span></span> | <span data-ttu-id="ab419-144">Určete, zda chcete přeskočit ověření klíče hostitele.</span><span class="sxs-lookup"><span data-stu-id="ab419-144">Specify whether to skip host key validation.</span></span> | <span data-ttu-id="ab419-145">Ne.</span><span class="sxs-lookup"><span data-stu-id="ab419-145">No.</span></span> <span data-ttu-id="ab419-146">Výchozí hodnota: false</span><span class="sxs-lookup"><span data-stu-id="ab419-146">The default value: false</span></span> |
| <span data-ttu-id="ab419-147">hostKeyFingerprint</span><span class="sxs-lookup"><span data-stu-id="ab419-147">hostKeyFingerprint</span></span> | <span data-ttu-id="ab419-148">Zadejte prstu klíče hostitele.</span><span class="sxs-lookup"><span data-stu-id="ab419-148">Specify the finger print of the host key.</span></span> | <span data-ttu-id="ab419-149">Ano, pokud `skipHostKeyValidation` nastaven na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="ab419-149">Yes if the `skipHostKeyValidation` is set to false.</span></span>  |
| <span data-ttu-id="ab419-150">gatewayName</span><span class="sxs-lookup"><span data-stu-id="ab419-150">gatewayName</span></span> |<span data-ttu-id="ab419-151">Název brány pro správu dat pro připojení k serveru pomocí protokolu SFTP místně.</span><span class="sxs-lookup"><span data-stu-id="ab419-151">Name of the Data Management Gateway to connect to an on-premises SFTP server.</span></span> | <span data-ttu-id="ab419-152">Ano, pokud kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="ab419-152">Yes if copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="ab419-153">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="ab419-153">encryptedCredential</span></span> | <span data-ttu-id="ab419-154">Šifrovaný přihlašovací údaje pro přístup k serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="ab419-154">Encrypted credential to access the SFTP server.</span></span> <span data-ttu-id="ab419-155">Automaticky generovaný když zadáte v Průvodci kopírovat nebo dialogové okno místní ClickOnce základní ověřování (uživatelské jméno a heslo) nebo ověřování parametru SshPublicKey (uživatelské jméno + cesta privátního klíče nebo obsah).</span><span class="sxs-lookup"><span data-stu-id="ab419-155">Auto-generated when you specify basic authentication (username + password) or SshPublicKey authentication (username + private key path or content) in copy wizard or the ClickOnce popup dialog.</span></span> | <span data-ttu-id="ab419-156">Ne.</span><span class="sxs-lookup"><span data-stu-id="ab419-156">No.</span></span> <span data-ttu-id="ab419-157">Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="ab419-157">Apply only when copying data from an on-premises SFTP server.</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="ab419-158">Použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="ab419-158">Using basic authentication</span></span>

<span data-ttu-id="ab419-159">Chcete-li základní ověřování použijte, nastavte `authenticationType` jako `Basic`a zadejte následující vlastnosti kromě konektor SFTP obecné ty, které jsou zavedené v poslední části:</span><span class="sxs-lookup"><span data-stu-id="ab419-159">To use basic authentication, set `authenticationType` as `Basic`, and specify the following properties besides the SFTP connector generic ones introduced in the last section:</span></span>

| <span data-ttu-id="ab419-160">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ab419-160">Property</span></span> | <span data-ttu-id="ab419-161">Popis</span><span class="sxs-lookup"><span data-stu-id="ab419-161">Description</span></span> | <span data-ttu-id="ab419-162">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ab419-162">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ab419-163">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="ab419-163">username</span></span> | <span data-ttu-id="ab419-164">Uživatel, který má přístup k serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="ab419-164">User who has access to the SFTP server.</span></span> |<span data-ttu-id="ab419-165">Ano</span><span class="sxs-lookup"><span data-stu-id="ab419-165">Yes</span></span> |
| <span data-ttu-id="ab419-166">heslo</span><span class="sxs-lookup"><span data-stu-id="ab419-166">password</span></span> | <span data-ttu-id="ab419-167">Heslo pro uživatele (uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="ab419-167">Password for the user (username).</span></span> | <span data-ttu-id="ab419-168">Ano</span><span class="sxs-lookup"><span data-stu-id="ab419-168">Yes</span></span> |

#### <a name="example-basic-authentication"></a><span data-ttu-id="ab419-169">Příklad: Základní ověřování</span><span class="sxs-lookup"><span data-stu-id="ab419-169">Example: Basic authentication</span></span>
```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a><span data-ttu-id="ab419-170">Příklad: Základní ověřování s šifrovaném přihlašovacím údaji</span><span class="sxs-lookup"><span data-stu-id="ab419-170">Example: Basic authentication with encrypted credential</span></span>

```JSON
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
      }
}
```

### <a name="using-ssh-public-key-authentication"></a><span data-ttu-id="ab419-171">Pomocí ověření veřejného klíče SSH</span><span class="sxs-lookup"><span data-stu-id="ab419-171">Using SSH public key authentication</span></span>

<span data-ttu-id="ab419-172">Chcete-li použít ověření veřejného klíče SSH, nastavte `authenticationType` jako `SshPublicKey`a zadejte následující vlastnosti kromě konektor SFTP obecné ty, které jsou zavedené v poslední části:</span><span class="sxs-lookup"><span data-stu-id="ab419-172">To use SSH public key authentication, set `authenticationType` as `SshPublicKey`, and specify the following properties besides the SFTP connector generic ones introduced in the last section:</span></span>

| <span data-ttu-id="ab419-173">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ab419-173">Property</span></span> | <span data-ttu-id="ab419-174">Popis</span><span class="sxs-lookup"><span data-stu-id="ab419-174">Description</span></span> | <span data-ttu-id="ab419-175">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ab419-175">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ab419-176">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="ab419-176">username</span></span> |<span data-ttu-id="ab419-177">Uživatel, který má přístup k serveru pomocí protokolu SFTP</span><span class="sxs-lookup"><span data-stu-id="ab419-177">User who has access to the SFTP server</span></span> |<span data-ttu-id="ab419-178">Ano</span><span class="sxs-lookup"><span data-stu-id="ab419-178">Yes</span></span> |
| <span data-ttu-id="ab419-179">privateKeyPath</span><span class="sxs-lookup"><span data-stu-id="ab419-179">privateKeyPath</span></span> | <span data-ttu-id="ab419-180">Zadejte absolutní cestu k souboru privátního klíče můžete přístup k této brány.</span><span class="sxs-lookup"><span data-stu-id="ab419-180">Specify absolute path to the private key file that gateway can access.</span></span> | <span data-ttu-id="ab419-181">Zadejte buď `privateKeyPath` nebo `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="ab419-181">Specify either the `privateKeyPath` or `privateKeyContent`.</span></span> <br><br> <span data-ttu-id="ab419-182">Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="ab419-182">Apply only when copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="ab419-183">privateKeyContent</span><span class="sxs-lookup"><span data-stu-id="ab419-183">privateKeyContent</span></span> | <span data-ttu-id="ab419-184">Serializovaná řetězec privátní klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="ab419-184">A serialized string of the private key content.</span></span> <span data-ttu-id="ab419-185">Průvodce kopírováním můžete číst soubor privátního klíče a automaticky extrahování privátní klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="ab419-185">The Copy Wizard can read the private key file and extract the private key content automatically.</span></span> <span data-ttu-id="ab419-186">Pokud používáte jakékoli jiné nástroje nebo SDK, použijte vlastnost privateKeyPath.</span><span class="sxs-lookup"><span data-stu-id="ab419-186">If you are using any other tool/SDK, use the privateKeyPath property instead.</span></span> | <span data-ttu-id="ab419-187">Zadejte buď `privateKeyPath` nebo `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="ab419-187">Specify either the `privateKeyPath` or `privateKeyContent`.</span></span> |
| <span data-ttu-id="ab419-188">přístupové heslo</span><span class="sxs-lookup"><span data-stu-id="ab419-188">passPhrase</span></span> | <span data-ttu-id="ab419-189">Zadejte průchodu fráze nebo hesla k dešifrování privátního klíče, pokud soubor klíče je chráněn heslo.</span><span class="sxs-lookup"><span data-stu-id="ab419-189">Specify the pass phrase/password to decrypt the private key if the key file is protected by a pass phrase.</span></span> | <span data-ttu-id="ab419-190">Ano, pokud heslo je chráněný soubor privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="ab419-190">Yes if the private key file is protected by a pass phrase.</span></span> |

> [!NOTE]
> <span data-ttu-id="ab419-191">Konektor SFTP podporují pouze klíč OpenSSH.</span><span class="sxs-lookup"><span data-stu-id="ab419-191">SFTP connector only support OpenSSH key.</span></span> <span data-ttu-id="ab419-192">Ujistěte se, že je váš soubor klíče ve správném formátu.</span><span class="sxs-lookup"><span data-stu-id="ab419-192">Make sure your key file is in the proper format.</span></span> <span data-ttu-id="ab419-193">Nástroj pro Putty můžete převést na formát OpenSSH .ppk.</span><span class="sxs-lookup"><span data-stu-id="ab419-193">You can use Putty tool to convert from .ppk to OpenSSH format.</span></span>

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a><span data-ttu-id="ab419-194">Příklad: Parametru SshPublicKey ověřování pomocí privátního klíče filePath</span><span class="sxs-lookup"><span data-stu-id="ab419-194">Example: SshPublicKey authentication using private key filePath</span></span>

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a><span data-ttu-id="ab419-195">Příklad: Parametru SshPublicKey ověřování pomocí privátního klíče obsahu</span><span class="sxs-lookup"><span data-stu-id="ab419-195">Example: SshPublicKey authentication using private key content</span></span>

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of the private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="ab419-196">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="ab419-196">Dataset properties</span></span>
<span data-ttu-id="ab419-197">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ab419-197">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="ab419-198">Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="ab419-198">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="ab419-199">**Rámci typeProperties** části se liší pro jednotlivé typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="ab419-199">The **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="ab419-200">Poskytuje informace, které jsou specifické pro daný typ datové sady.</span><span class="sxs-lookup"><span data-stu-id="ab419-200">It provides information that is specific to the dataset type.</span></span> <span data-ttu-id="ab419-201">Rámci typeProperties část datové sady typu **sdílení souborů** datová sada má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="ab419-201">The typeProperties section for a dataset of type **FileShare** dataset has the following properties:</span></span>

| <span data-ttu-id="ab419-202">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ab419-202">Property</span></span> | <span data-ttu-id="ab419-203">Popis</span><span class="sxs-lookup"><span data-stu-id="ab419-203">Description</span></span> | <span data-ttu-id="ab419-204">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ab419-204">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ab419-205">folderPath</span><span class="sxs-lookup"><span data-stu-id="ab419-205">folderPath</span></span> |<span data-ttu-id="ab419-206">Sub – cesta ke složce.</span><span class="sxs-lookup"><span data-stu-id="ab419-206">Sub path to the folder.</span></span> <span data-ttu-id="ab419-207">Použít řídicí znak ' \ ' pro speciální znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="ab419-207">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="ab419-208">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="ab419-208">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="ab419-209">Tato vlastnost se můžete kombinovat **partitionBy** tak, aby měl složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="ab419-209">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="ab419-210">Ano</span><span class="sxs-lookup"><span data-stu-id="ab419-210">Yes</span></span> |
| <span data-ttu-id="ab419-211">fileName</span><span class="sxs-lookup"><span data-stu-id="ab419-211">fileName</span></span> |<span data-ttu-id="ab419-212">Zadejte název souboru do **folderPath** Pokud chcete, aby v tabulce odkazovat na konkrétní soubor ve složce.</span><span class="sxs-lookup"><span data-stu-id="ab419-212">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="ab419-213">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, tabulka odkazuje na všechny soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="ab419-213">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="ab419-214">Pokud není zadán název souboru pro datovou sadu výstupů, název vygenerovaný soubor bude v následujícím tento formát:</span><span class="sxs-lookup"><span data-stu-id="ab419-214">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="ab419-215">Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="ab419-215">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="ab419-216">Ne</span><span class="sxs-lookup"><span data-stu-id="ab419-216">No</span></span> |
| <span data-ttu-id="ab419-217">fileFilter</span><span class="sxs-lookup"><span data-stu-id="ab419-217">fileFilter</span></span> |<span data-ttu-id="ab419-218">Zadejte filtr pro umožňuje vybrat podmnožinu souborů v folderPath, nikoli všech souborů.</span><span class="sxs-lookup"><span data-stu-id="ab419-218">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="ab419-219">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="ab419-219">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="ab419-220">Příklady 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="ab419-220">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="ab419-221">Příklad 2:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="ab419-221">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="ab419-222">fileFilter se vztahuje vstupní datové sady sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="ab419-222">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="ab419-223">Tato vlastnost není podporována s HDFS.</span><span class="sxs-lookup"><span data-stu-id="ab419-223">This property is not supported with HDFS.</span></span> |<span data-ttu-id="ab419-224">Ne</span><span class="sxs-lookup"><span data-stu-id="ab419-224">No</span></span> |
| <span data-ttu-id="ab419-225">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="ab419-225">partitionedBy</span></span> |<span data-ttu-id="ab419-226">partitionedBy slouží k určení dynamické folderPath, název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="ab419-226">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="ab419-227">Například folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="ab419-227">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="ab419-228">Ne</span><span class="sxs-lookup"><span data-stu-id="ab419-228">No</span></span> |
| <span data-ttu-id="ab419-229">Formát</span><span class="sxs-lookup"><span data-stu-id="ab419-229">format</span></span> | <span data-ttu-id="ab419-230">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="ab419-230">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="ab419-231">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="ab419-231">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="ab419-232">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="ab419-232">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="ab419-233">Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="ab419-233">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="ab419-234">Ne</span><span class="sxs-lookup"><span data-stu-id="ab419-234">No</span></span> |
| <span data-ttu-id="ab419-235">Komprese</span><span class="sxs-lookup"><span data-stu-id="ab419-235">compression</span></span> | <span data-ttu-id="ab419-236">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="ab419-236">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="ab419-237">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="ab419-237">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="ab419-238">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="ab419-238">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="ab419-239">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="ab419-239">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="ab419-240">Ne</span><span class="sxs-lookup"><span data-stu-id="ab419-240">No</span></span> |
| <span data-ttu-id="ab419-241">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="ab419-241">useBinaryTransfer</span></span> |<span data-ttu-id="ab419-242">Určit, jestli použít režim binární přenosu.</span><span class="sxs-lookup"><span data-stu-id="ab419-242">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="ab419-243">Platí pro binárního režimu a false ASCII.</span><span class="sxs-lookup"><span data-stu-id="ab419-243">True for binary mode and false ASCII.</span></span> <span data-ttu-id="ab419-244">Výchozí hodnota: True.</span><span class="sxs-lookup"><span data-stu-id="ab419-244">Default value: True.</span></span> <span data-ttu-id="ab419-245">Tuto vlastnost lze použít pouze v případě typu přidružené propojené služby typu: Server_ftp.</span><span class="sxs-lookup"><span data-stu-id="ab419-245">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="ab419-246">Ne</span><span class="sxs-lookup"><span data-stu-id="ab419-246">No</span></span> |

> [!NOTE]
> <span data-ttu-id="ab419-247">Název souboru a fileFilter nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="ab419-247">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="ab419-248">Pomocí vlastnosti partionedBy</span><span class="sxs-lookup"><span data-stu-id="ab419-248">Using partionedBy property</span></span>
<span data-ttu-id="ab419-249">Jak je uvedeno v předchozí části, můžete zadat dynamické folderPath, název souboru pro data časové řady s partitionedBy.</span><span class="sxs-lookup"><span data-stu-id="ab419-249">As mentioned in the previous section, you can specify a dynamic folderPath, filename for time series data with partitionedBy.</span></span> <span data-ttu-id="ab419-250">Můžete tak učinit pomocí makra pro vytváření dat a systémové proměnné SliceStart, SliceEnd, který označuje logické časové období pro daný datový řez.</span><span class="sxs-lookup"><span data-stu-id="ab419-250">You can do so with the Data Factory macros and the system variable SliceStart, SliceEnd that indicate the logical time period for a given data slice.</span></span>

<span data-ttu-id="ab419-251">Další informace o datové sady času řady, plánování a řezy najdete v tématu [vytváření datových sad](data-factory-create-datasets.md), [plánování a provádění](data-factory-scheduling-and-execution.md), a [vytváření kanálů](data-factory-create-pipelines.md) články.</span><span class="sxs-lookup"><span data-stu-id="ab419-251">To learn about time series datasets, scheduling, and slices, See [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="ab419-252">Příklad 1:</span><span class="sxs-lookup"><span data-stu-id="ab419-252">Sample 1:</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="ab419-253">V tomto příkladu {řez} se nahradí hodnotu objektu pro vytváření dat systému proměnné SliceStart ve formátu (YYYYMMDDHH) zadán.</span><span class="sxs-lookup"><span data-stu-id="ab419-253">In this example {Slice} is replaced with the value of Data Factory system variable SliceStart in the format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="ab419-254">Vlastnosti SliceStart odkazuje na spuštění řezu.</span><span class="sxs-lookup"><span data-stu-id="ab419-254">The SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="ab419-255">FolderPath se liší pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="ab419-255">The folderPath is different for each slice.</span></span> <span data-ttu-id="ab419-256">Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="ab419-256">Example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="ab419-257">Příklad 2:</span><span class="sxs-lookup"><span data-stu-id="ab419-257">Sample 2:</span></span>

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
<span data-ttu-id="ab419-258">V tomto příkladu jsou extrahován rok, měsíc, den a čas SliceStart do samostatné proměnné, které jsou používány folderPath a název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ab419-258">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="ab419-259">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="ab419-259">Copy activity properties</span></span>
<span data-ttu-id="ab419-260">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ab419-260">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="ab419-261">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="ab419-261">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="ab419-262">Vzhledem k tomu, vlastnosti dostupné v rámci typeProperties části aktivity se liší podle každý typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="ab419-262">Whereas, the properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="ab419-263">Pro aktivitu kopírování vlastnosti typu lišit v závislosti na typech zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="ab419-263">For Copy activity, the type properties vary depending on the types of sources and sinks.</span></span>

[!INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="ab419-264">Podporované formáty souborů a komprese</span><span class="sxs-lookup"><span data-stu-id="ab419-264">Supported file and compression formats</span></span>
<span data-ttu-id="ab419-265">V tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článek na podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ab419-265">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-sftp-server-to-azure-blob"></a><span data-ttu-id="ab419-266">Příklad JSON: Kopírování dat ze serveru pomocí protokolu SFTP do objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="ab419-266">JSON Example: Copy data from SFTP server to Azure blob</span></span>
<span data-ttu-id="ab419-267">Následující příklad uvádí ukázka JSON definice, které můžete použít k vytvoření kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ab419-267">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="ab419-268">Se ukazují, jak zkopírovat data ze zdroje SFTP do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="ab419-268">They show how to copy data from SFTP source to Azure Blob Storage.</span></span> <span data-ttu-id="ab419-269">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů do jakéhokoli z jímky uvádí [sem](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí aktivity kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ab419-269">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ab419-270">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="ab419-270">This sample provides JSON snippets.</span></span> <span data-ttu-id="ab419-271">Podrobné pokyny pro vytvoření objektu pro vytváření dat neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="ab419-271">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="ab419-272">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="ab419-272">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="ab419-273">Ukázka má následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="ab419-273">The sample has the following data factory entities:</span></span>

* <span data-ttu-id="ab419-274">Propojené služby typu [sftp](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ab419-274">A linked service of type [sftp](#linked-service-properties).</span></span>
* <span data-ttu-id="ab419-275">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ab419-275">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="ab419-276">Vstup [datovou sadu](data-factory-create-datasets.md) typu [sdílení souborů](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ab419-276">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="ab419-277">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ab419-277">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="ab419-278">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="ab419-278">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="ab419-279">Ukázka kopíruje data ze serveru pomocí protokolu SFTP do objektu blob Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="ab419-279">The sample copies data from an SFTP server to an Azure blob every hour.</span></span> <span data-ttu-id="ab419-280">Vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky.</span><span class="sxs-lookup"><span data-stu-id="ab419-280">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="ab419-281">**SFTP propojené služby**</span><span class="sxs-lookup"><span data-stu-id="ab419-281">**SFTP linked service**</span></span>

<span data-ttu-id="ab419-282">Tento příklad používá základní ověřování s uživatelské jméno a heslo v prostém textu.</span><span class="sxs-lookup"><span data-stu-id="ab419-282">This example uses the basic authentication with user name and password in plain text.</span></span> <span data-ttu-id="ab419-283">Můžete také použít jednu z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="ab419-283">You can also use one of the following ways:</span></span>

* <span data-ttu-id="ab419-284">Základní ověřování s zašifrované přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="ab419-284">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="ab419-285">Ověření veřejného klíče SSH</span><span class="sxs-lookup"><span data-stu-id="ab419-285">SSH public key authentication</span></span>

<span data-ttu-id="ab419-286">V tématu [FTP propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="ab419-286">See [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON

{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "myuser",
            "password": "mypassword",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```
<span data-ttu-id="ab419-287">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="ab419-287">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="ab419-288">**Vstupní datové sady pomocí protokolu SFTP**</span><span class="sxs-lookup"><span data-stu-id="ab419-288">**SFTP input dataset**</span></span>

<span data-ttu-id="ab419-289">Tato datová sada odkazuje na složku SFTP `mysharedfolder` a soubor `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="ab419-289">This dataset refers to the SFTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="ab419-290">Kanál zkopíruje soubor do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="ab419-290">The pipeline copies the file to the destination.</span></span>

<span data-ttu-id="ab419-291">Nastavení "externí": "PRAVDA" informuje služba Data Factory, datová sada je externí k objektu pro vytváření dat a není vyprodukované aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="ab419-291">Setting "external": "true" informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "SFTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "SftpLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="ab419-292">**Výstupní datovou sadu objektů Blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="ab419-292">**Azure Blob output dataset**</span></span>

<span data-ttu-id="ab419-293">Data se zapisují do nového objektu blob každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="ab419-293">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="ab419-294">Cesta ke složce pro tento objekt blob je vyhodnocován dynamicky podle času zahájení řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="ab419-294">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="ab419-295">Cesta ke složce používá rok, měsíc, den a čas částí čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="ab419-295">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="ab419-296">**Kanál s aktivitou kopírování**</span><span class="sxs-lookup"><span data-stu-id="ab419-296">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="ab419-297">Kanál obsahuje aktivitu kopírování, který je nakonfigurovaný na použití vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="ab419-297">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="ab419-298">V definici JSON kanálu **zdroj** je typ nastaven na **FileSystemSource** a **podřízený** je typ nastaven na **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="ab419-298">In the pipeline JSON definition, the **source** type is set to **FileSystemSource** and **sink** type is set to **BlobSink**.</span></span>

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
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
        "start": "2017-02-20T18:00:00Z",
        "end": "2017-02-20T19:00:00Z"
    }
}
```

## <a name="performance-and-tuning"></a><span data-ttu-id="ab419-299">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="ab419-299">Performance and Tuning</span></span>
<span data-ttu-id="ab419-300">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) Další informace o klíčových faktorů, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby, jak optimalizovat ho.</span><span class="sxs-lookup"><span data-stu-id="ab419-300">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab419-301">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab419-301">Next Steps</span></span>
<span data-ttu-id="ab419-302">Viz následující články:</span><span class="sxs-lookup"><span data-stu-id="ab419-302">See the following articles:</span></span>

* <span data-ttu-id="ab419-303">[Kopie aktivity kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny pro vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="ab419-303">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
