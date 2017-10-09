---
title: "Ukázky aaaAzure úložiště pomocí Java | Microsoft Docs"
description: "Zobrazit, stáhnout a spustit ukázkový kód a aplikace pro Azure Storage. Umožňuje zjistit Začínáme ukázky pro objekty BLOB, fronty, tabulky a soubory, pomocí knihovny klienta úložiště hello Java."
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
ms.openlocfilehash: e3b8fbe86e82dd58c2a13a3c68760cbf6e9a6e4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-samples-using-java"></a><span data-ttu-id="c41bd-104">Ukázek Azure Storage používá Java</span><span class="sxs-lookup"><span data-stu-id="c41bd-104">Azure Storage samples using Java</span></span>

## <a name="java-sample-index"></a><span data-ttu-id="c41bd-105">Ukázka index Java</span><span class="sxs-lookup"><span data-stu-id="c41bd-105">Java sample index</span></span>

<span data-ttu-id="c41bd-106">Hello následující tabulka obsahuje přehled ukázek scénáře úložiště a hello zahrnutých v každém vzorku.</span><span class="sxs-lookup"><span data-stu-id="c41bd-106">hello following table provides an overview of our samples repository and hello scenarios covered in each sample.</span></span> <span data-ttu-id="c41bd-107">Klikněte na hello odkazy tooview hello odpovídající ukázkový kód v Githubu.</span><span class="sxs-lookup"><span data-stu-id="c41bd-107">Click on hello links tooview hello corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="c41bd-108">Koncový bod</span><span class="sxs-lookup"><span data-stu-id="c41bd-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="c41bd-109">Scénář</span><span class="sxs-lookup"><span data-stu-id="c41bd-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="c41bd-110">Ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="c41bd-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="c41bd-111"><b>Objekt BLOB</b></span><span class="sxs-lookup"><span data-stu-id="c41bd-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="c41bd-112">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="c41bd-112">Append Blob</span></span></td> 
<td><span data-ttu-id="c41bd-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-114">Objekt Blob bloku</span><span class="sxs-lookup"><span data-stu-id="c41bd-114">Block Blob</span></span></td>
<td><span data-ttu-id="c41bd-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-116">Šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="c41bd-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="c41bd-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Začínáme s šifrování na straně klienta Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Getting Started with Azure Client Side Encryption in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-118">Zkopírování objektu Blob</span><span class="sxs-lookup"><span data-stu-id="c41bd-118">Copy Blob</span></span></td>
<td><span data-ttu-id="c41bd-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-120">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="c41bd-120">Create Container</span></span></td>
<td><span data-ttu-id="c41bd-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-122">Odstranit objekt Blob</span><span class="sxs-lookup"><span data-stu-id="c41bd-122">Delete Blob</span></span></td>
<td><span data-ttu-id="c41bd-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-124">Odstranit kontejner</span><span class="sxs-lookup"><span data-stu-id="c41bd-124">Delete Container</span></span></td>
<td><span data-ttu-id="c41bd-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-126">Objekt BLOB Metadata nebo vlastnosti nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="c41bd-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="c41bd-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-128">Seznam ACL nebo Metadata nebo vlastnosti kontejneru</span><span class="sxs-lookup"><span data-stu-id="c41bd-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="c41bd-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-130">Get rozsahů stránek</span><span class="sxs-lookup"><span data-stu-id="c41bd-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="c41bd-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Ukázka testů objekt Blob stránky</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Page Blob Tests Sample</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-132">Zapůjčení nebo kontejner objektů Blob</span><span class="sxs-lookup"><span data-stu-id="c41bd-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="c41bd-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-134">Seznam objektů Blob nebo kontejneru</span><span class="sxs-lookup"><span data-stu-id="c41bd-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="c41bd-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-136">Objekt Blob stránky</span><span class="sxs-lookup"><span data-stu-id="c41bd-136">Page Blob</span></span></td>
<td><span data-ttu-id="c41bd-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="c41bd-138">SAS</span><span class="sxs-lookup"><span data-stu-id="c41bd-138">SAS</span></span></td>
<td><span data-ttu-id="c41bd-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">Ukázka testů SAS</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS Tests Sample</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="c41bd-140">Vlastnosti služby</span><span class="sxs-lookup"><span data-stu-id="c41bd-140">Service Properties</span></span></td>
<td><span data-ttu-id="c41bd-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="c41bd-142">Objekt Blob snímku</span><span class="sxs-lookup"><span data-stu-id="c41bd-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="c41bd-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="c41bd-144"><b>File</b></span><span class="sxs-lookup"><span data-stu-id="c41bd-144"><b>File</b></span></span></td>
<td><span data-ttu-id="c41bd-145">Vytvoření sdílené složky, adresáře nebo soubory</span><span class="sxs-lookup"><span data-stu-id="c41bd-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="c41bd-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="c41bd-147">Odstranění sdílené složky, adresáře nebo soubory</span><span class="sxs-lookup"><span data-stu-id="c41bd-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="c41bd-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-149">Directory vlastnosti nebo Metadata</span><span class="sxs-lookup"><span data-stu-id="c41bd-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="c41bd-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-151">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="c41bd-151">Download Files</span></span></td> 
<td><span data-ttu-id="c41bd-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-153">Soubor vlastnosti nebo Metadata nebo metriky</span><span class="sxs-lookup"><span data-stu-id="c41bd-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="c41bd-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-155">Vlastnosti souboru služby</span><span class="sxs-lookup"><span data-stu-id="c41bd-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="c41bd-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-157">Seznam adresářů a souborů</span><span class="sxs-lookup"><span data-stu-id="c41bd-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="c41bd-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="c41bd-159">Zobrazit seznam sdílených složek</span><span class="sxs-lookup"><span data-stu-id="c41bd-159">List Shares</span></span></td> 
<td><span data-ttu-id="c41bd-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="c41bd-161">Sdílené složky vlastnosti nebo Metadata nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="c41bd-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="c41bd-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="c41bd-163"><b>Fronty</b></span><span class="sxs-lookup"><span data-stu-id="c41bd-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="c41bd-164">Přidat zprávu</span><span class="sxs-lookup"><span data-stu-id="c41bd-164">Add Message</span></span></td> 
<td><span data-ttu-id="c41bd-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-166">Šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="c41bd-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="c41bd-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-168">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="c41bd-168">Create Queues</span></span></td> 
<td><span data-ttu-id="c41bd-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-170">Odstranit nebo fronta zpráv</span><span class="sxs-lookup"><span data-stu-id="c41bd-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="c41bd-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-172">Prohlížení zpráv</span><span class="sxs-lookup"><span data-stu-id="c41bd-172">Peek Message</span></span></td> 
<td><span data-ttu-id="c41bd-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-174">Fronty seznamu ACL nebo Metadata nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="c41bd-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="c41bd-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-176">Vlastnosti fronty služby</span><span class="sxs-lookup"><span data-stu-id="c41bd-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="c41bd-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-178">Zpráva aktualizace</span><span class="sxs-lookup"><span data-stu-id="c41bd-178">Update Message</span></span></td> 
<td><span data-ttu-id="c41bd-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="c41bd-180"><b>Tabulka</b></span><span class="sxs-lookup"><span data-stu-id="c41bd-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="c41bd-181">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="c41bd-181">Create Table</span></span></td> 
<td><span data-ttu-id="c41bd-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-183">Odstranit Entity nebo tabulku</span><span class="sxs-lookup"><span data-stu-id="c41bd-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="c41bd-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-185">Vložení a sloučení nebo nahrazení Entity</span><span class="sxs-lookup"><span data-stu-id="c41bd-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="c41bd-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-187">Dotaz entity</span><span class="sxs-lookup"><span data-stu-id="c41bd-187">Query Entities</span></span></td> 
<td><span data-ttu-id="c41bd-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-189">Dotazu na tabulky</span><span class="sxs-lookup"><span data-stu-id="c41bd-189">Query Tables</span></span></td> 
<td><span data-ttu-id="c41bd-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-191">Tabulka seznamu ACL nebo vlastnosti</span><span class="sxs-lookup"><span data-stu-id="c41bd-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="c41bd-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="c41bd-193">Aktualizace Entity</span><span class="sxs-lookup"><span data-stu-id="c41bd-193">Update Entity</span></span></td> 
<td><span data-ttu-id="c41bd-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="c41bd-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="c41bd-195">Knihovna Azure ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="c41bd-195">Azure Code Samples library</span></span>

<span data-ttu-id="c41bd-196">tooview hello ucelenou ukázku knihovny, přejděte toohello [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=storage) knihovny, která obsahuje ukázky pro úložiště Azure, které můžete stáhnout a spustit místně.</span><span class="sxs-lookup"><span data-stu-id="c41bd-196">tooview hello complete sample library, go toohello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="c41bd-197">Hello knihovně ukázka kódu obsahuje ukázkový kód ve formátu ZIP.</span><span class="sxs-lookup"><span data-stu-id="c41bd-197">hello Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="c41bd-198">Alternativně můžete procházet a klonovat úložiště GitHub hello pro jednotlivé vzorky.</span><span class="sxs-lookup"><span data-stu-id="c41bd-198">Alternatively, you can browse and clone hello GitHub repository for each sample.</span></span>

[!INCLUDE [storage-java-samples-include](../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="c41bd-199">Získávání příručky Začínáme</span><span class="sxs-lookup"><span data-stu-id="c41bd-199">Getting started guides</span></span>

<span data-ttu-id="c41bd-200">Podívejte se na následující příručky, pokud hledáte pokyny, jak hello tooinstall a začátek práce s hello knihovny klienta Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c41bd-200">Check out hello following guides if you are looking for instructions on how tooinstall and get started with hello Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="c41bd-201">Začínáme se službou objektů Blob v Azure v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="c41bd-201">Getting Started with Azure Blob Service in Java</span></span>](storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="c41bd-202">Začínáme se službou Azure fronty v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="c41bd-202">Getting Started with Azure Queue Service in Java</span></span>](storage-java-how-to-use-queue-storage.md)
* [<span data-ttu-id="c41bd-203">Začínáme se službou Azure tabulky v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="c41bd-203">Getting Started with Azure Table Service in Java</span></span>](storage-java-how-to-use-table-storage.md)
* [<span data-ttu-id="c41bd-204">Začínáme se službou Azure souboru v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="c41bd-204">Getting Started with Azure File Service in Java</span></span>](storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a><span data-ttu-id="c41bd-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c41bd-205">Next steps</span></span>

<span data-ttu-id="c41bd-206">Informace o ukázky pro jiné jazyky:</span><span class="sxs-lookup"><span data-stu-id="c41bd-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="c41bd-207">Rozhraní .NET: [ukázky azure Storage pomocí rozhraní .NET](storage-samples-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="c41bd-207">.NET: [Azure Storage samples using .NET](storage-samples-dotnet.md)</span></span>
* <span data-ttu-id="c41bd-208">Všechny ostatní jazyky: [ukázky Azure Storage](storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="c41bd-208">All other languages: [Azure Storage samples](storage-samples.md)</span></span>
