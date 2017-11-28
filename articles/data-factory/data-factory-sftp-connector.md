---
title: "aaaMove data ze serveru pomocí protokolu SFTP pomocí Azure Data Factory | Microsoft Docs"
description: "Další informace o tom toomove data z místní nebo serveru pomocí protokolu SFTP cloudu pomocí Azure Data Factory."
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
ms.openlocfilehash: b976289e2c1b1899634bb5565b375499077fa554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a><span data-ttu-id="12d94-103">Přesunutí dat ze serveru pomocí protokolu SFTP pomocí Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="12d94-103">Move data from an SFTP server using Azure Data Factory</span></span>
<span data-ttu-id="12d94-104">Tento článek popisuje, jak toouse hello aktivitu kopírování v Azure Data Factory toomove data z tooa serveru pomocí protokolu SFTP lokální/Cloudová podporované úložiště dat jímky.</span><span class="sxs-lookup"><span data-stu-id="12d94-104">This article outlines how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises/cloud SFTP server tooa supported sink data store.</span></span> <span data-ttu-id="12d94-105">Tento článek vychází hello [aktivity přesunu dat](data-factory-data-movement-activities.md) článek, který představuje obecný přehled přesun dat s kopie aktivity a hello seznamu úložiště dat, které jsou podporované jako zdroje nebo jímky.</span><span class="sxs-lookup"><span data-stu-id="12d94-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and hello list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="12d94-106">Objekt pro vytváření dat v současné době podporuje pouze přesouvání dat od protokolu SFTP serveru tooother uloží, ale ne pro přesun dat z jiných dat ukládá tooan SFTP serveru.</span><span class="sxs-lookup"><span data-stu-id="12d94-106">Data factory currently supports only moving data from an SFTP server tooother data stores, but not for moving data from other data stores tooan SFTP server.</span></span> <span data-ttu-id="12d94-107">Podporuje místní a cloudové servery pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="12d94-107">It supports both on-premises and cloud SFTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="12d94-108">Aktivita kopírování neodstraní hello zdrojový soubor po úspěšně zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="12d94-108">Copy Activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="12d94-109">Pokud potřebujete toodelete hello zdrojový soubor po úspěšné kopie, vytvořte soubor vlastní aktivity toodelete hello a používat hello aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-109">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file and use hello activity in hello pipeline.</span></span> 

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="12d94-110">Podporované scénáře a typy ověřování</span><span class="sxs-lookup"><span data-stu-id="12d94-110">Supported scenarios and authentication types</span></span>
<span data-ttu-id="12d94-111">Můžete použít tento SFTP konektor toocopy data z **i v cloudu pomocí protokolu SFTP servery a servery pomocí protokolu SFTP místní**.</span><span class="sxs-lookup"><span data-stu-id="12d94-111">You can use this SFTP connector toocopy data from **both cloud SFTP servers and on-premises SFTP servers**.</span></span> <span data-ttu-id="12d94-112">**Základní** a **parametru SshPublicKey** typy ověřování jsou podporovány při připojování serveru pomocí protokolu SFTP toohello.</span><span class="sxs-lookup"><span data-stu-id="12d94-112">**Basic** and **SshPublicKey** authentication types are supported when connecting toohello SFTP server.</span></span>

<span data-ttu-id="12d94-113">Při kopírování dat z místního serveru pomocí protokolu SFTP, je nutné nainstalovat brána pro správu dat v hello místní prostředí nebo Azure virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="12d94-113">When copying data from an on-premises SFTP server, you need install a Data Management Gateway in hello on-premises environment/Azure VM.</span></span> <span data-ttu-id="12d94-114">V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o hello brány.</span><span class="sxs-lookup"><span data-stu-id="12d94-114">See [Data Management Gateway](data-factory-data-management-gateway.md) for details on hello gateway.</span></span> <span data-ttu-id="12d94-115">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) článku podrobné pokyny k nastavení hello brány a jeho použití.</span><span class="sxs-lookup"><span data-stu-id="12d94-115">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway and using it.</span></span>

## <a name="getting-started"></a><span data-ttu-id="12d94-116">Začínáme</span><span class="sxs-lookup"><span data-stu-id="12d94-116">Getting started</span></span>
<span data-ttu-id="12d94-117">Vytvoření kanálu s aktivitou kopírování, který přesouvá data z protokolu SFTP zdroje pomocí různých nástrojů nebo rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="12d94-117">You can create a pipeline with a copy activity that moves data from an SFTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="12d94-118">Nejjednodušší způsob, jak toocreate Hello kanálu je toouse hello **Průvodce kopírováním**.</span><span class="sxs-lookup"><span data-stu-id="12d94-118">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="12d94-119">V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) podrobný rychlé vytvoření kanálu pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

- <span data-ttu-id="12d94-120">Můžete také použít následující nástroje toocreate kanálu hello: **portál Azure**, **Visual Studio**, **prostředí Azure PowerShell**, **šablony Azure Resource Manageru** , **.NET API**, a **rozhraní REST API**.</span><span class="sxs-lookup"><span data-stu-id="12d94-120">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="12d94-121">V tématu [kurzu aktivity kopírování](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro podrobné pokyny toocreate kanál s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="12d94-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> <span data-ttu-id="12d94-122">JSON – ukázky toocopy data z tooAzure serveru pomocí protokolu SFTP úložiště objektů Blob, najdete v části [JSON příklad: kopírování dat z objektu blob tooAzure serveru pomocí protokolu SFTP](#json-example-copy-data-from-sftp-server-to-azure-blob) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="12d94-122">For JSON samples toocopy data from SFTP server tooAzure Blob Storage, see [JSON Example: Copy data from SFTP server tooAzure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) section of this article.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="12d94-123">Vlastnosti propojené služby</span><span class="sxs-lookup"><span data-stu-id="12d94-123">Linked service properties</span></span>
<span data-ttu-id="12d94-124">Hello následující tabulka obsahuje popis JSON elementy konkrétní tooFTP propojené služby.</span><span class="sxs-lookup"><span data-stu-id="12d94-124">hello following table provides description for JSON elements specific tooFTP linked service.</span></span>

| <span data-ttu-id="12d94-125">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="12d94-125">Property</span></span> | <span data-ttu-id="12d94-126">Popis</span><span class="sxs-lookup"><span data-stu-id="12d94-126">Description</span></span> | <span data-ttu-id="12d94-127">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="12d94-127">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12d94-128">type</span><span class="sxs-lookup"><span data-stu-id="12d94-128">type</span></span> | <span data-ttu-id="12d94-129">musí být nastavena vlastnost typu Hello příliš`Sftp`.</span><span class="sxs-lookup"><span data-stu-id="12d94-129">hello type property must be set too`Sftp`.</span></span> |<span data-ttu-id="12d94-130">Ano</span><span class="sxs-lookup"><span data-stu-id="12d94-130">Yes</span></span> |
| <span data-ttu-id="12d94-131">hostitele</span><span class="sxs-lookup"><span data-stu-id="12d94-131">host</span></span> | <span data-ttu-id="12d94-132">Název nebo IP adresa serveru pomocí protokolu SFTP hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-132">Name or IP address of hello SFTP server.</span></span> |<span data-ttu-id="12d94-133">Ano</span><span class="sxs-lookup"><span data-stu-id="12d94-133">Yes</span></span> |
| <span data-ttu-id="12d94-134">port</span><span class="sxs-lookup"><span data-stu-id="12d94-134">port</span></span> |<span data-ttu-id="12d94-135">Port, na které hello SFTP server naslouchá.</span><span class="sxs-lookup"><span data-stu-id="12d94-135">Port on which hello SFTP server is listening.</span></span> <span data-ttu-id="12d94-136">Hello výchozí hodnota je: 21</span><span class="sxs-lookup"><span data-stu-id="12d94-136">hello default value is: 21</span></span> |<span data-ttu-id="12d94-137">Ne</span><span class="sxs-lookup"><span data-stu-id="12d94-137">No</span></span> |
| <span data-ttu-id="12d94-138">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="12d94-138">authenticationType</span></span> |<span data-ttu-id="12d94-139">Zadejte typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="12d94-139">Specify authentication type.</span></span> <span data-ttu-id="12d94-140">Povolené hodnoty: **základní**, **parametru SshPublicKey**.</span><span class="sxs-lookup"><span data-stu-id="12d94-140">Allowed values: **Basic**, **SshPublicKey**.</span></span> <br><br> <span data-ttu-id="12d94-141">Odkazovat příliš[základní ověřování pomocí](#using-basic-authentication) a [pomocí SSH ověření veřejného klíče](#using-ssh-public-key-authentication) částech na další vlastnosti a ukázky JSON v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="12d94-141">Refer too[Using basic authentication](#using-basic-authentication) and [Using SSH public key authentication](#using-ssh-public-key-authentication) sections on more properties and JSON samples respectively.</span></span> |<span data-ttu-id="12d94-142">Ano</span><span class="sxs-lookup"><span data-stu-id="12d94-142">Yes</span></span> |
| <span data-ttu-id="12d94-143">skipHostKeyValidation</span><span class="sxs-lookup"><span data-stu-id="12d94-143">skipHostKeyValidation</span></span> | <span data-ttu-id="12d94-144">Zadejte, zda tooskip hostitele klíče ověřování.</span><span class="sxs-lookup"><span data-stu-id="12d94-144">Specify whether tooskip host key validation.</span></span> | <span data-ttu-id="12d94-145">Ne.</span><span class="sxs-lookup"><span data-stu-id="12d94-145">No.</span></span> <span data-ttu-id="12d94-146">Hello výchozí hodnota: false</span><span class="sxs-lookup"><span data-stu-id="12d94-146">hello default value: false</span></span> |
| <span data-ttu-id="12d94-147">hostKeyFingerprint</span><span class="sxs-lookup"><span data-stu-id="12d94-147">hostKeyFingerprint</span></span> | <span data-ttu-id="12d94-148">Zadejte hello prstu hello hostitele klíče.</span><span class="sxs-lookup"><span data-stu-id="12d94-148">Specify hello finger print of hello host key.</span></span> | <span data-ttu-id="12d94-149">Ano, pokud hello `skipHostKeyValidation` nastavena toofalse.</span><span class="sxs-lookup"><span data-stu-id="12d94-149">Yes if hello `skipHostKeyValidation` is set toofalse.</span></span>  |
| <span data-ttu-id="12d94-150">gatewayName</span><span class="sxs-lookup"><span data-stu-id="12d94-150">gatewayName</span></span> |<span data-ttu-id="12d94-151">Název hello Brána pro správu dat tooconnect tooan místní server pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="12d94-151">Name of hello Data Management Gateway tooconnect tooan on-premises SFTP server.</span></span> | <span data-ttu-id="12d94-152">Ano, pokud kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="12d94-152">Yes if copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="12d94-153">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="12d94-153">encryptedCredential</span></span> | <span data-ttu-id="12d94-154">Šifrovaný přihlašovací údaj tooaccess hello SFTP server.</span><span class="sxs-lookup"><span data-stu-id="12d94-154">Encrypted credential tooaccess hello SFTP server.</span></span> <span data-ttu-id="12d94-155">Automaticky generovaný když zadáte základní ověřování (uživatelské jméno a heslo) nebo ověřování parametru SshPublicKey (uživatelské jméno + cesta privátního klíče nebo obsahu) v kopie Průvodce nebo hello ClickOnce automaticky otevřeném okně. dialog.</span><span class="sxs-lookup"><span data-stu-id="12d94-155">Auto-generated when you specify basic authentication (username + password) or SshPublicKey authentication (username + private key path or content) in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="12d94-156">Ne.</span><span class="sxs-lookup"><span data-stu-id="12d94-156">No.</span></span> <span data-ttu-id="12d94-157">Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="12d94-157">Apply only when copying data from an on-premises SFTP server.</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="12d94-158">Použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="12d94-158">Using basic authentication</span></span>

<span data-ttu-id="12d94-159">toouse základní ověřování, nastavit `authenticationType` jako `Basic`a zadejte následující vlastnosti kromě hello SFTP konektor obecné ty, které jsou zavedené v poslední části hello hello:</span><span class="sxs-lookup"><span data-stu-id="12d94-159">toouse basic authentication, set `authenticationType` as `Basic`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="12d94-160">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="12d94-160">Property</span></span> | <span data-ttu-id="12d94-161">Popis</span><span class="sxs-lookup"><span data-stu-id="12d94-161">Description</span></span> | <span data-ttu-id="12d94-162">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="12d94-162">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12d94-163">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="12d94-163">username</span></span> | <span data-ttu-id="12d94-164">Uživatel, který má přístup toohello SFTP server.</span><span class="sxs-lookup"><span data-stu-id="12d94-164">User who has access toohello SFTP server.</span></span> |<span data-ttu-id="12d94-165">Ano</span><span class="sxs-lookup"><span data-stu-id="12d94-165">Yes</span></span> |
| <span data-ttu-id="12d94-166">heslo</span><span class="sxs-lookup"><span data-stu-id="12d94-166">password</span></span> | <span data-ttu-id="12d94-167">Heslo pro uživatele hello (uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="12d94-167">Password for hello user (username).</span></span> | <span data-ttu-id="12d94-168">Ano</span><span class="sxs-lookup"><span data-stu-id="12d94-168">Yes</span></span> |

#### <a name="example-basic-authentication"></a><span data-ttu-id="12d94-169">Příklad: Základní ověřování</span><span class="sxs-lookup"><span data-stu-id="12d94-169">Example: Basic authentication</span></span>
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

#### <a name="example-basic-authentication-with-encrypted-credential"></a><span data-ttu-id="12d94-170">Příklad: Základní ověřování s šifrovaném přihlašovacím údaji</span><span class="sxs-lookup"><span data-stu-id="12d94-170">Example: Basic authentication with encrypted credential</span></span>

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

### <a name="using-ssh-public-key-authentication"></a><span data-ttu-id="12d94-171">Pomocí ověření veřejného klíče SSH</span><span class="sxs-lookup"><span data-stu-id="12d94-171">Using SSH public key authentication</span></span>

<span data-ttu-id="12d94-172">nastavit toouse SSH ověření veřejného klíče, `authenticationType` jako `SshPublicKey`a zadejte následující vlastnosti kromě hello SFTP konektor obecné ty, které jsou zavedené v poslední části hello hello:</span><span class="sxs-lookup"><span data-stu-id="12d94-172">toouse SSH public key authentication, set `authenticationType` as `SshPublicKey`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="12d94-173">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="12d94-173">Property</span></span> | <span data-ttu-id="12d94-174">Popis</span><span class="sxs-lookup"><span data-stu-id="12d94-174">Description</span></span> | <span data-ttu-id="12d94-175">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="12d94-175">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12d94-176">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="12d94-176">username</span></span> |<span data-ttu-id="12d94-177">Uživatel, který má přístup toohello SFTP serveru</span><span class="sxs-lookup"><span data-stu-id="12d94-177">User who has access toohello SFTP server</span></span> |<span data-ttu-id="12d94-178">Ano</span><span class="sxs-lookup"><span data-stu-id="12d94-178">Yes</span></span> |
| <span data-ttu-id="12d94-179">privateKeyPath</span><span class="sxs-lookup"><span data-stu-id="12d94-179">privateKeyPath</span></span> | <span data-ttu-id="12d94-180">Zadejte absolutní cestu toohello soubor privátního klíče můžete přístup k této brány.</span><span class="sxs-lookup"><span data-stu-id="12d94-180">Specify absolute path toohello private key file that gateway can access.</span></span> | <span data-ttu-id="12d94-181">Zadejte buď hello `privateKeyPath` nebo `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="12d94-181">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> <br><br> <span data-ttu-id="12d94-182">Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="12d94-182">Apply only when copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="12d94-183">privateKeyContent</span><span class="sxs-lookup"><span data-stu-id="12d94-183">privateKeyContent</span></span> | <span data-ttu-id="12d94-184">Serializovaná řetězec hello privátní klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="12d94-184">A serialized string of hello private key content.</span></span> <span data-ttu-id="12d94-185">Průvodce kopírováním Hello můžete číst soubor privátního klíče hello a extrahování hello privátní klíče obsahu automaticky.</span><span class="sxs-lookup"><span data-stu-id="12d94-185">hello Copy Wizard can read hello private key file and extract hello private key content automatically.</span></span> <span data-ttu-id="12d94-186">Pokud používáte jakékoli jiné nástroje nebo SDK, použijte vlastnost privateKeyPath hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-186">If you are using any other tool/SDK, use hello privateKeyPath property instead.</span></span> | <span data-ttu-id="12d94-187">Zadejte buď hello `privateKeyPath` nebo `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="12d94-187">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> |
| <span data-ttu-id="12d94-188">přístupové heslo</span><span class="sxs-lookup"><span data-stu-id="12d94-188">passPhrase</span></span> | <span data-ttu-id="12d94-189">Pokud soubor klíče hello je chráněn heslo, zadejte hello průchodu fráze nebo hesla toodecrypt hello privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="12d94-189">Specify hello pass phrase/password toodecrypt hello private key if hello key file is protected by a pass phrase.</span></span> | <span data-ttu-id="12d94-190">Ano, pokud je chráněný soubor privátního klíče hello heslo.</span><span class="sxs-lookup"><span data-stu-id="12d94-190">Yes if hello private key file is protected by a pass phrase.</span></span> |

> [!NOTE]
> <span data-ttu-id="12d94-191">Konektor SFTP podporují pouze klíč OpenSSH.</span><span class="sxs-lookup"><span data-stu-id="12d94-191">SFTP connector only support OpenSSH key.</span></span> <span data-ttu-id="12d94-192">Ujistěte se, že je váš soubor klíče ve správném formátu hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-192">Make sure your key file is in hello proper format.</span></span> <span data-ttu-id="12d94-193">Můžete použít nástroj pro Putty tooconvert z .ppk tooOpenSSH formátu.</span><span class="sxs-lookup"><span data-stu-id="12d94-193">You can use Putty tool tooconvert from .ppk tooOpenSSH format.</span></span>

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a><span data-ttu-id="12d94-194">Příklad: Parametru SshPublicKey ověřování pomocí privátního klíče filePath</span><span class="sxs-lookup"><span data-stu-id="12d94-194">Example: SshPublicKey authentication using private key filePath</span></span>

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

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a><span data-ttu-id="12d94-195">Příklad: Parametru SshPublicKey ověřování pomocí privátního klíče obsahu</span><span class="sxs-lookup"><span data-stu-id="12d94-195">Example: SshPublicKey authentication using private key content</span></span>

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
            "privateKeyContent": "<base64 string of hello private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="12d94-196">Vlastnosti datové sady</span><span class="sxs-lookup"><span data-stu-id="12d94-196">Dataset properties</span></span>
<span data-ttu-id="12d94-197">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování datové sady, najdete v části hello [vytváření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="12d94-197">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="12d94-198">Oddíly jako je například struktura, dostupnost a zásad JSON datové sady jsou podobné pro všechny typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="12d94-198">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="12d94-199">Hello **rámci typeProperties** části se liší pro jednotlivé typy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="12d94-199">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="12d94-200">Poskytuje informace, které je typu konkrétní toohello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="12d94-200">It provides information that is specific toohello dataset type.</span></span> <span data-ttu-id="12d94-201">rámci typeProperties Hello část datové sady typu **sdílení souborů** datová sada má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="12d94-201">hello typeProperties section for a dataset of type **FileShare** dataset has hello following properties:</span></span>

| <span data-ttu-id="12d94-202">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="12d94-202">Property</span></span> | <span data-ttu-id="12d94-203">Popis</span><span class="sxs-lookup"><span data-stu-id="12d94-203">Description</span></span> | <span data-ttu-id="12d94-204">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="12d94-204">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="12d94-205">folderPath</span><span class="sxs-lookup"><span data-stu-id="12d94-205">folderPath</span></span> |<span data-ttu-id="12d94-206">Dílčí cesta toohello složka.</span><span class="sxs-lookup"><span data-stu-id="12d94-206">Sub path toohello folder.</span></span> <span data-ttu-id="12d94-207">Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-207">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="12d94-208">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="12d94-208">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="12d94-209">Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="12d94-209">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="12d94-210">Ano</span><span class="sxs-lookup"><span data-stu-id="12d94-210">Yes</span></span> |
| <span data-ttu-id="12d94-211">fileName</span><span class="sxs-lookup"><span data-stu-id="12d94-211">fileName</span></span> |<span data-ttu-id="12d94-212">Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-212">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="12d94-213">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-213">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="12d94-214">Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát:</span><span class="sxs-lookup"><span data-stu-id="12d94-214">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="12d94-215">Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="12d94-215">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="12d94-216">Ne</span><span class="sxs-lookup"><span data-stu-id="12d94-216">No</span></span> |
| <span data-ttu-id="12d94-217">fileFilter</span><span class="sxs-lookup"><span data-stu-id="12d94-217">fileFilter</span></span> |<span data-ttu-id="12d94-218">Zadejte že filtr toobe používá tooselect podmnožinu souborů v hello folderPath, nikoli všech souborů.</span><span class="sxs-lookup"><span data-stu-id="12d94-218">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="12d94-219">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="12d94-219">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="12d94-220">Příklady 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="12d94-220">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="12d94-221">Příklad 2:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="12d94-221">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="12d94-222">fileFilter se vztahuje vstupní datové sady sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="12d94-222">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="12d94-223">Tato vlastnost není podporována s HDFS.</span><span class="sxs-lookup"><span data-stu-id="12d94-223">This property is not supported with HDFS.</span></span> |<span data-ttu-id="12d94-224">Ne</span><span class="sxs-lookup"><span data-stu-id="12d94-224">No</span></span> |
| <span data-ttu-id="12d94-225">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="12d94-225">partitionedBy</span></span> |<span data-ttu-id="12d94-226">partitionedBy se dá použít toospecify dynamické folderPath, název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="12d94-226">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="12d94-227">Například folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="12d94-227">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="12d94-228">Ne</span><span class="sxs-lookup"><span data-stu-id="12d94-228">No</span></span> |
| <span data-ttu-id="12d94-229">Formát</span><span class="sxs-lookup"><span data-stu-id="12d94-229">format</span></span> | <span data-ttu-id="12d94-230">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="12d94-230">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="12d94-231">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="12d94-231">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="12d94-232">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="12d94-232">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="12d94-233">Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="12d94-233">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="12d94-234">Ne</span><span class="sxs-lookup"><span data-stu-id="12d94-234">No</span></span> |
| <span data-ttu-id="12d94-235">Komprese</span><span class="sxs-lookup"><span data-stu-id="12d94-235">compression</span></span> | <span data-ttu-id="12d94-236">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-236">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="12d94-237">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="12d94-237">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="12d94-238">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="12d94-238">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="12d94-239">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="12d94-239">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="12d94-240">Ne</span><span class="sxs-lookup"><span data-stu-id="12d94-240">No</span></span> |
| <span data-ttu-id="12d94-241">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="12d94-241">useBinaryTransfer</span></span> |<span data-ttu-id="12d94-242">Určit, jestli použít režim binární přenosu.</span><span class="sxs-lookup"><span data-stu-id="12d94-242">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="12d94-243">Platí pro binárního režimu a false ASCII.</span><span class="sxs-lookup"><span data-stu-id="12d94-243">True for binary mode and false ASCII.</span></span> <span data-ttu-id="12d94-244">Výchozí hodnota: True.</span><span class="sxs-lookup"><span data-stu-id="12d94-244">Default value: True.</span></span> <span data-ttu-id="12d94-245">Tuto vlastnost lze použít pouze v případě typu přidružené propojené služby typu: Server_ftp.</span><span class="sxs-lookup"><span data-stu-id="12d94-245">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="12d94-246">Ne</span><span class="sxs-lookup"><span data-stu-id="12d94-246">No</span></span> |

> [!NOTE]
> <span data-ttu-id="12d94-247">Název souboru a fileFilter nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="12d94-247">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="12d94-248">Pomocí vlastnosti partionedBy</span><span class="sxs-lookup"><span data-stu-id="12d94-248">Using partionedBy property</span></span>
<span data-ttu-id="12d94-249">Jak je uvedeno v předchozí části hello, můžete zadat dynamické folderPath, název souboru pro data časové řady s partitionedBy.</span><span class="sxs-lookup"><span data-stu-id="12d94-249">As mentioned in hello previous section, you can specify a dynamic folderPath, filename for time series data with partitionedBy.</span></span> <span data-ttu-id="12d94-250">Můžete tak učinit pomocí hello makra pro vytváření dat a hello systémové proměnné SliceStart, SliceEnd, který označuje hello logické časové období pro daný datový řez.</span><span class="sxs-lookup"><span data-stu-id="12d94-250">You can do so with hello Data Factory macros and hello system variable SliceStart, SliceEnd that indicate hello logical time period for a given data slice.</span></span>

<span data-ttu-id="12d94-251">toolearn o datové sady času řady, plánování a řezů, najdete v části [vytváření datových sad](data-factory-create-datasets.md), [plánování a provádění](data-factory-scheduling-and-execution.md), a [vytváření kanálů](data-factory-create-pipelines.md) články.</span><span class="sxs-lookup"><span data-stu-id="12d94-251">toolearn about time series datasets, scheduling, and slices, See [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="12d94-252">Příklad 1:</span><span class="sxs-lookup"><span data-stu-id="12d94-252">Sample 1:</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="12d94-253">V tomto příkladu {řez} se nahradí hello hodnotu objektu pro vytváření dat systému proměnné SliceStart ve formátu hello (YYYYMMDDHH) zadán.</span><span class="sxs-lookup"><span data-stu-id="12d94-253">In this example {Slice} is replaced with hello value of Data Factory system variable SliceStart in hello format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="12d94-254">Hello SliceStart odkazuje toostart času řezu hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-254">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="12d94-255">Hello folderPath se liší pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="12d94-255">hello folderPath is different for each slice.</span></span> <span data-ttu-id="12d94-256">Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="12d94-256">Example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="12d94-257">Příklad 2:</span><span class="sxs-lookup"><span data-stu-id="12d94-257">Sample 2:</span></span>

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
<span data-ttu-id="12d94-258">V tomto příkladu jsou extrahován rok, měsíc, den a čas SliceStart do samostatné proměnné, které jsou používány folderPath a název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="12d94-258">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="12d94-259">Zkopírovat vlastnosti aktivit</span><span class="sxs-lookup"><span data-stu-id="12d94-259">Copy activity properties</span></span>
<span data-ttu-id="12d94-260">Úplný seznam oddílů & vlastnosti, které jsou k dispozici pro definování aktivit najdete v tématu hello [vytváření kanálů](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="12d94-260">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="12d94-261">Vlastnosti, například název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="12d94-261">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="12d94-262">Zatímco se každý typ aktivity lišit podle vlastnosti hello k dispozici v rámci typeProperties části hello hello aktivity.</span><span class="sxs-lookup"><span data-stu-id="12d94-262">Whereas, hello properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="12d94-263">Pro aktivitu kopírování vlastnosti typu hello lišit v závislosti na typech hello zdrojů a jímky.</span><span class="sxs-lookup"><span data-stu-id="12d94-263">For Copy activity, hello type properties vary depending on hello types of sources and sinks.</span></span>

[!INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="12d94-264">Podporované formáty souborů a komprese</span><span class="sxs-lookup"><span data-stu-id="12d94-264">Supported file and compression formats</span></span>
<span data-ttu-id="12d94-265">V tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md) článek na podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="12d94-265">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-sftp-server-tooazure-blob"></a><span data-ttu-id="12d94-266">Příklad JSON: Kopírování dat z objektu blob tooAzure serveru pomocí protokolu SFTP</span><span class="sxs-lookup"><span data-stu-id="12d94-266">JSON Example: Copy data from SFTP server tooAzure blob</span></span>
<span data-ttu-id="12d94-267">Hello následující příklad obsahuje ukázkové JSON definice používané toocreate kanálu pomocí [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [prostředí Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="12d94-267">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="12d94-268">Ukazují jak zdroje dat toocopy z protokolu SFTP tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="12d94-268">They show how toocopy data from SFTP source tooAzure Blob Storage.</span></span> <span data-ttu-id="12d94-269">Nicméně je možné zkopírovat data **přímo** ze všech zdrojů tooany z hello jímky uvádí [zde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pomocí hello aktivitu kopírování v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="12d94-269">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="12d94-270">Tato ukázka obsahuje fragmenty kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="12d94-270">This sample provides JSON snippets.</span></span> <span data-ttu-id="12d94-271">Neobsahuje podrobné pokyny pro vytvoření objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-271">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="12d94-272">V tématu [přesouvání dat mezi místní umístění a cloudem](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="12d94-272">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="12d94-273">Ukázka Hello má hello následující entity objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="12d94-273">hello sample has hello following data factory entities:</span></span>

* <span data-ttu-id="12d94-274">Propojené služby typu [sftp](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="12d94-274">A linked service of type [sftp](#linked-service-properties).</span></span>
* <span data-ttu-id="12d94-275">Propojené služby typu [azurestorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="12d94-275">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="12d94-276">Vstup [datovou sadu](data-factory-create-datasets.md) typu [sdílení souborů](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="12d94-276">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="12d94-277">Výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="12d94-277">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="12d94-278">A [kanálu](data-factory-create-pipelines.md) s aktivitou kopírování, která používá [FileSystemSource](#copy-activity-properties) a [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="12d94-278">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="12d94-279">Ukázka Hello kopíruje data ze serveru tooan protokolu SFTP objektů blob v Azure každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="12d94-279">hello sample copies data from an SFTP server tooan Azure blob every hour.</span></span> <span data-ttu-id="12d94-280">Hello vlastnostech JSON použitých ve tyto ukázky jsou popsané v části následující ukázky hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-280">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="12d94-281">**SFTP propojené služby**</span><span class="sxs-lookup"><span data-stu-id="12d94-281">**SFTP linked service**</span></span>

<span data-ttu-id="12d94-282">Tento příklad používá hello základní ověřování s uživatelské jméno a heslo v prostém textu.</span><span class="sxs-lookup"><span data-stu-id="12d94-282">This example uses hello basic authentication with user name and password in plain text.</span></span> <span data-ttu-id="12d94-283">Můžete také použít jednu z následujících způsobů hello:</span><span class="sxs-lookup"><span data-stu-id="12d94-283">You can also use one of hello following ways:</span></span>

* <span data-ttu-id="12d94-284">Základní ověřování s zašifrované přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="12d94-284">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="12d94-285">Ověření veřejného klíče SSH</span><span class="sxs-lookup"><span data-stu-id="12d94-285">SSH public key authentication</span></span>

<span data-ttu-id="12d94-286">V tématu [FTP propojená služba](#linked-service-properties) části pro různé typy ověřování můžete použít.</span><span class="sxs-lookup"><span data-stu-id="12d94-286">See [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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
<span data-ttu-id="12d94-287">**Propojená služba Azure Storage**</span><span class="sxs-lookup"><span data-stu-id="12d94-287">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="12d94-288">**Vstupní datové sady pomocí protokolu SFTP**</span><span class="sxs-lookup"><span data-stu-id="12d94-288">**SFTP input dataset**</span></span>

<span data-ttu-id="12d94-289">Tato datová sada odkazuje složky pomocí protokolu SFTP toohello `mysharedfolder` a soubor `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="12d94-289">This dataset refers toohello SFTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="12d94-290">Hello kanál kopíruje cíl toohello souboru hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-290">hello pipeline copies hello file toohello destination.</span></span>

<span data-ttu-id="12d94-291">Nastavení "externí": "PRAVDA" informuje služba Data Factory hello tuto datovou sadu hello je externí toohello pro vytváření dat a není vyprodukované aktivitu v objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="12d94-291">Setting "external": "true" informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="12d94-292">**Výstupní datovou sadu objektů Blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="12d94-292">**Azure Blob output dataset**</span></span>

<span data-ttu-id="12d94-293">Data se zapisují nový objekt blob tooa každou hodinu (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="12d94-293">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="12d94-294">Cesta ke složce Hello pro objekt blob hello je vyhodnocován dynamicky podle času zahájení hello hello řezu, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="12d94-294">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="12d94-295">Cesta ke složce Hello používá rok, měsíc, den a čas části hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="12d94-295">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="12d94-296">**Kanál s aktivitou kopírování**</span><span class="sxs-lookup"><span data-stu-id="12d94-296">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="12d94-297">Hello kanál obsahuje aktivitu kopírování, která je nakonfigurovaná toouse hello vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="12d94-297">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="12d94-298">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**FileSystemSource** a **podřízený** je typ nastaven příliš**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="12d94-298">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource** and **sink** type is set too**BlobSink**.</span></span>

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

## <a name="performance-and-tuning"></a><span data-ttu-id="12d94-299">Výkon a ladění</span><span class="sxs-lookup"><span data-stu-id="12d94-299">Performance and Tuning</span></span>
<span data-ttu-id="12d94-300">V tématu [výkonu kopie aktivity & ladění průvodce](data-factory-copy-activity-performance.md) toolearn o klíči faktory, že dopad výkon přesun dat (aktivita kopírování) v Azure Data Factory a různé způsoby toooptimize ho.</span><span class="sxs-lookup"><span data-stu-id="12d94-300">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12d94-301">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12d94-301">Next Steps</span></span>
<span data-ttu-id="12d94-302">V tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="12d94-302">See hello following articles:</span></span>

* <span data-ttu-id="12d94-303">[Kopie aktivity kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny pro vytvoření kanálu s aktivitou kopírování.</span><span class="sxs-lookup"><span data-stu-id="12d94-303">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
