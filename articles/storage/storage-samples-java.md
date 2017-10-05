---
title: "Ukázek Azure Storage pomocí Java | Microsoft Docs"
description: "Zobrazit, stáhnout a spustit ukázkový kód a aplikace pro Azure Storage. Umožňuje zjistit Začínáme ukázky pro objekty BLOB, fronty, tabulky a soubory, pomocí knihovny klienta úložiště Java."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: 98e6022062b4ef5b5c71b54a0e94775b925d216b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-storage-samples-using-java"></a><span data-ttu-id="f74e9-104">Ukázek Azure Storage používá Java</span><span class="sxs-lookup"><span data-stu-id="f74e9-104">Azure Storage samples using Java</span></span>

## <a name="java-sample-index"></a><span data-ttu-id="f74e9-105">Ukázka index Java</span><span class="sxs-lookup"><span data-stu-id="f74e9-105">Java sample index</span></span>

<span data-ttu-id="f74e9-106">Následující tabulka obsahuje přehled o našem úložišti ukázky a pokryté v každé ukázkové scénáře.</span><span class="sxs-lookup"><span data-stu-id="f74e9-106">The following table provides an overview of our samples repository and the scenarios covered in each sample.</span></span> <span data-ttu-id="f74e9-107">Kliknutím na odkazy k zobrazení odpovídající ukázkový kód v Githubu.</span><span class="sxs-lookup"><span data-stu-id="f74e9-107">Click on the links to view the corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="f74e9-108">Koncový bod</span><span class="sxs-lookup"><span data-stu-id="f74e9-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="f74e9-109">Scénář</span><span class="sxs-lookup"><span data-stu-id="f74e9-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="f74e9-110">Ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="f74e9-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="f74e9-111"><b>Objekt BLOB</b></span><span class="sxs-lookup"><span data-stu-id="f74e9-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="f74e9-112">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="f74e9-112">Append Blob</span></span></td> 
<td><span data-ttu-id="f74e9-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-114">Objekt Blob bloku</span><span class="sxs-lookup"><span data-stu-id="f74e9-114">Block Blob</span></span></td>
<td><span data-ttu-id="f74e9-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-116">Šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="f74e9-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="f74e9-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Začínáme s šifrování na straně klienta Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Getting Started with Azure Client Side Encryption in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-118">Zkopírování objektu Blob</span><span class="sxs-lookup"><span data-stu-id="f74e9-118">Copy Blob</span></span></td>
<td><span data-ttu-id="f74e9-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-120">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="f74e9-120">Create Container</span></span></td>
<td><span data-ttu-id="f74e9-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-122">Odstranit objekt Blob</span><span class="sxs-lookup"><span data-stu-id="f74e9-122">Delete Blob</span></span></td>
<td><span data-ttu-id="f74e9-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-124">Odstranit kontejner</span><span class="sxs-lookup"><span data-stu-id="f74e9-124">Delete Container</span></span></td>
<td><span data-ttu-id="f74e9-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-126">Objekt BLOB Metadata nebo vlastnosti nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="f74e9-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="f74e9-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-128">Seznam ACL nebo Metadata nebo vlastnosti kontejneru</span><span class="sxs-lookup"><span data-stu-id="f74e9-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="f74e9-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-130">Get rozsahů stránek</span><span class="sxs-lookup"><span data-stu-id="f74e9-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="f74e9-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Ukázka testů objekt Blob stránky</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Page Blob Tests Sample</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-132">Zapůjčení nebo kontejner objektů Blob</span><span class="sxs-lookup"><span data-stu-id="f74e9-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="f74e9-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-134">Seznam objektů Blob nebo kontejneru</span><span class="sxs-lookup"><span data-stu-id="f74e9-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="f74e9-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-136">Objekt Blob stránky</span><span class="sxs-lookup"><span data-stu-id="f74e9-136">Page Blob</span></span></td>
<td><span data-ttu-id="f74e9-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="f74e9-138">SAS</span><span class="sxs-lookup"><span data-stu-id="f74e9-138">SAS</span></span></td>
<td><span data-ttu-id="f74e9-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">Ukázka testů SAS</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS Tests Sample</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="f74e9-140">Vlastnosti služby</span><span class="sxs-lookup"><span data-stu-id="f74e9-140">Service Properties</span></span></td>
<td><span data-ttu-id="f74e9-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="f74e9-142">Objekt Blob snímku</span><span class="sxs-lookup"><span data-stu-id="f74e9-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="f74e9-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="f74e9-144"><b>File</b></span><span class="sxs-lookup"><span data-stu-id="f74e9-144"><b>File</b></span></span></td>
<td><span data-ttu-id="f74e9-145">Vytvoření sdílené složky, adresáře nebo soubory</span><span class="sxs-lookup"><span data-stu-id="f74e9-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="f74e9-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="f74e9-147">Odstranění sdílené složky, adresáře nebo soubory</span><span class="sxs-lookup"><span data-stu-id="f74e9-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="f74e9-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-149">Directory vlastnosti nebo Metadata</span><span class="sxs-lookup"><span data-stu-id="f74e9-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="f74e9-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-151">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="f74e9-151">Download Files</span></span></td> 
<td><span data-ttu-id="f74e9-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-153">Soubor vlastnosti nebo Metadata nebo metriky</span><span class="sxs-lookup"><span data-stu-id="f74e9-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="f74e9-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-155">Vlastnosti souboru služby</span><span class="sxs-lookup"><span data-stu-id="f74e9-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="f74e9-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-157">Seznam adresářů a souborů</span><span class="sxs-lookup"><span data-stu-id="f74e9-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="f74e9-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="f74e9-159">Zobrazit seznam sdílených složek</span><span class="sxs-lookup"><span data-stu-id="f74e9-159">List Shares</span></span></td> 
<td><span data-ttu-id="f74e9-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="f74e9-161">Sdílené složky vlastnosti nebo Metadata nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="f74e9-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="f74e9-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="f74e9-163"><b>Fronty</b></span><span class="sxs-lookup"><span data-stu-id="f74e9-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="f74e9-164">Přidat zprávu</span><span class="sxs-lookup"><span data-stu-id="f74e9-164">Add Message</span></span></td> 
<td><span data-ttu-id="f74e9-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-166">Šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="f74e9-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="f74e9-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-168">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="f74e9-168">Create Queues</span></span></td> 
<td><span data-ttu-id="f74e9-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-170">Odstranit nebo fronta zpráv</span><span class="sxs-lookup"><span data-stu-id="f74e9-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="f74e9-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-172">Prohlížení zpráv</span><span class="sxs-lookup"><span data-stu-id="f74e9-172">Peek Message</span></span></td> 
<td><span data-ttu-id="f74e9-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-174">Fronty seznamu ACL nebo Metadata nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="f74e9-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="f74e9-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-176">Vlastnosti fronty služby</span><span class="sxs-lookup"><span data-stu-id="f74e9-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="f74e9-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-178">Zpráva aktualizace</span><span class="sxs-lookup"><span data-stu-id="f74e9-178">Update Message</span></span></td> 
<td><span data-ttu-id="f74e9-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="f74e9-180"><b>Tabulka</b></span><span class="sxs-lookup"><span data-stu-id="f74e9-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="f74e9-181">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="f74e9-181">Create Table</span></span></td> 
<td><span data-ttu-id="f74e9-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-183">Odstranit Entity nebo tabulku</span><span class="sxs-lookup"><span data-stu-id="f74e9-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="f74e9-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-185">Vložení a sloučení nebo nahrazení Entity</span><span class="sxs-lookup"><span data-stu-id="f74e9-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="f74e9-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-187">Dotaz entity</span><span class="sxs-lookup"><span data-stu-id="f74e9-187">Query Entities</span></span></td> 
<td><span data-ttu-id="f74e9-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-189">Dotazu na tabulky</span><span class="sxs-lookup"><span data-stu-id="f74e9-189">Query Tables</span></span></td> 
<td><span data-ttu-id="f74e9-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-191">Tabulka seznamu ACL nebo vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f74e9-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="f74e9-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="f74e9-193">Aktualizace Entity</span><span class="sxs-lookup"><span data-stu-id="f74e9-193">Update Entity</span></span></td> 
<td><span data-ttu-id="f74e9-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="f74e9-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="f74e9-195">Knihovna Azure ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="f74e9-195">Azure Code Samples library</span></span>

<span data-ttu-id="f74e9-196">Chcete-li zobrazit knihovně ucelenou ukázku, přejděte na [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=storage) knihovny, která obsahuje ukázky pro úložiště Azure, které můžete stáhnout a spustit místně.</span><span class="sxs-lookup"><span data-stu-id="f74e9-196">To view the complete sample library, go to the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="f74e9-197">Knihovna ukázka kódu obsahuje ukázkový kód ve formátu ZIP.</span><span class="sxs-lookup"><span data-stu-id="f74e9-197">The Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="f74e9-198">Alternativně můžete procházet a naklonujte úložiště GitHub pro jednotlivé vzorky.</span><span class="sxs-lookup"><span data-stu-id="f74e9-198">Alternatively, you can browse and clone the GitHub repository for each sample.</span></span>

[!INCLUDE [storage-java-samples-include](../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="f74e9-199">Získávání příručky Začínáme</span><span class="sxs-lookup"><span data-stu-id="f74e9-199">Getting started guides</span></span>

<span data-ttu-id="f74e9-200">Pokud hledáte pokyny, jak nainstalovat a začít pracovat s knihovny klienta Azure Storage, projděte si následující příručky.</span><span class="sxs-lookup"><span data-stu-id="f74e9-200">Check out the following guides if you are looking for instructions on how to install and get started with the Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="f74e9-201">Začínáme se službou objektů Blob v Azure v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="f74e9-201">Getting Started with Azure Blob Service in Java</span></span>](storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="f74e9-202">Začínáme se službou Azure fronty v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="f74e9-202">Getting Started with Azure Queue Service in Java</span></span>](storage-java-how-to-use-queue-storage.md)
* [<span data-ttu-id="f74e9-203">Začínáme se službou Azure tabulky v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="f74e9-203">Getting Started with Azure Table Service in Java</span></span>](storage-java-how-to-use-table-storage.md)
* [<span data-ttu-id="f74e9-204">Začínáme se službou Azure souboru v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="f74e9-204">Getting Started with Azure File Service in Java</span></span>](storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a><span data-ttu-id="f74e9-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f74e9-205">Next steps</span></span>

<span data-ttu-id="f74e9-206">Informace o ukázky pro jiné jazyky:</span><span class="sxs-lookup"><span data-stu-id="f74e9-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="f74e9-207">Rozhraní .NET: [ukázky azure Storage pomocí rozhraní .NET](storage-samples-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="f74e9-207">.NET: [Azure Storage samples using .NET](storage-samples-dotnet.md)</span></span>
* <span data-ttu-id="f74e9-208">Všechny ostatní jazyky: [ukázky Azure Storage](storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="f74e9-208">All other languages: [Azure Storage samples](storage-samples.md)</span></span>
