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
ms.openlocfilehash: fd27e1ac9a773e7b0f5245aa74acdb0521cd098c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-samples-using-java"></a><span data-ttu-id="e30a5-104">Ukázek Azure Storage používá Java</span><span class="sxs-lookup"><span data-stu-id="e30a5-104">Azure Storage samples using Java</span></span>

## <a name="java-sample-index"></a><span data-ttu-id="e30a5-105">Ukázka index Java</span><span class="sxs-lookup"><span data-stu-id="e30a5-105">Java sample index</span></span>

<span data-ttu-id="e30a5-106">Následující tabulka obsahuje přehled o našem úložišti ukázky a pokryté v každé ukázkové scénáře.</span><span class="sxs-lookup"><span data-stu-id="e30a5-106">The following table provides an overview of our samples repository and the scenarios covered in each sample.</span></span> <span data-ttu-id="e30a5-107">Kliknutím na odkazy k zobrazení odpovídající ukázkový kód v Githubu.</span><span class="sxs-lookup"><span data-stu-id="e30a5-107">Click on the links to view the corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="e30a5-108">Koncový bod</span><span class="sxs-lookup"><span data-stu-id="e30a5-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="e30a5-109">Scénář</span><span class="sxs-lookup"><span data-stu-id="e30a5-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="e30a5-110">Ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="e30a5-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="e30a5-111"><b>Objekt BLOB</b></span><span class="sxs-lookup"><span data-stu-id="e30a5-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="e30a5-112">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="e30a5-112">Append Blob</span></span></td> 
<td><span data-ttu-id="e30a5-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-114">Objekt Blob bloku</span><span class="sxs-lookup"><span data-stu-id="e30a5-114">Block Blob</span></span></td>
<td><span data-ttu-id="e30a5-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-116">Šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="e30a5-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="e30a5-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Začínáme s šifrování na straně klienta Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Getting Started with Azure Client Side Encryption in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-118">Zkopírování objektu Blob</span><span class="sxs-lookup"><span data-stu-id="e30a5-118">Copy Blob</span></span></td>
<td><span data-ttu-id="e30a5-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-120">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="e30a5-120">Create Container</span></span></td>
<td><span data-ttu-id="e30a5-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-122">Odstranit objekt Blob</span><span class="sxs-lookup"><span data-stu-id="e30a5-122">Delete Blob</span></span></td>
<td><span data-ttu-id="e30a5-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-124">Odstranit kontejner</span><span class="sxs-lookup"><span data-stu-id="e30a5-124">Delete Container</span></span></td>
<td><span data-ttu-id="e30a5-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-126">Objekt BLOB Metadata nebo vlastnosti nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="e30a5-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="e30a5-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-128">Seznam ACL nebo Metadata nebo vlastnosti kontejneru</span><span class="sxs-lookup"><span data-stu-id="e30a5-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="e30a5-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-130">Get rozsahů stránek</span><span class="sxs-lookup"><span data-stu-id="e30a5-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="e30a5-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Ukázka testů objekt Blob stránky</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Page Blob Tests Sample</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-132">Zapůjčení nebo kontejner objektů Blob</span><span class="sxs-lookup"><span data-stu-id="e30a5-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="e30a5-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-134">Seznam objektů Blob nebo kontejneru</span><span class="sxs-lookup"><span data-stu-id="e30a5-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="e30a5-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-136">Objekt Blob stránky</span><span class="sxs-lookup"><span data-stu-id="e30a5-136">Page Blob</span></span></td>
<td><span data-ttu-id="e30a5-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="e30a5-138">SAS</span><span class="sxs-lookup"><span data-stu-id="e30a5-138">SAS</span></span></td>
<td><span data-ttu-id="e30a5-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">Ukázka testů SAS</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS Tests Sample</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="e30a5-140">Vlastnosti služby</span><span class="sxs-lookup"><span data-stu-id="e30a5-140">Service Properties</span></span></td>
<td><span data-ttu-id="e30a5-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="e30a5-142">Objekt Blob snímku</span><span class="sxs-lookup"><span data-stu-id="e30a5-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="e30a5-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="e30a5-144"><b>File</b></span><span class="sxs-lookup"><span data-stu-id="e30a5-144"><b>File</b></span></span></td>
<td><span data-ttu-id="e30a5-145">Vytvoření sdílené složky, adresáře nebo soubory</span><span class="sxs-lookup"><span data-stu-id="e30a5-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="e30a5-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="e30a5-147">Odstranění sdílené složky, adresáře nebo soubory</span><span class="sxs-lookup"><span data-stu-id="e30a5-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="e30a5-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-149">Directory vlastnosti nebo Metadata</span><span class="sxs-lookup"><span data-stu-id="e30a5-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="e30a5-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-151">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="e30a5-151">Download Files</span></span></td> 
<td><span data-ttu-id="e30a5-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-153">Soubor vlastnosti nebo Metadata nebo metriky</span><span class="sxs-lookup"><span data-stu-id="e30a5-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="e30a5-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-155">Vlastnosti souboru služby</span><span class="sxs-lookup"><span data-stu-id="e30a5-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="e30a5-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-157">Seznam adresářů a souborů</span><span class="sxs-lookup"><span data-stu-id="e30a5-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="e30a5-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="e30a5-159">Zobrazit seznam sdílených složek</span><span class="sxs-lookup"><span data-stu-id="e30a5-159">List Shares</span></span></td> 
<td><span data-ttu-id="e30a5-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="e30a5-161">Sdílené složky vlastnosti nebo Metadata nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="e30a5-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="e30a5-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="e30a5-163"><b>Fronty</b></span><span class="sxs-lookup"><span data-stu-id="e30a5-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="e30a5-164">Přidat zprávu</span><span class="sxs-lookup"><span data-stu-id="e30a5-164">Add Message</span></span></td> 
<td><span data-ttu-id="e30a5-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-166">Šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="e30a5-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="e30a5-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-168">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="e30a5-168">Create Queues</span></span></td> 
<td><span data-ttu-id="e30a5-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-170">Odstranit nebo fronta zpráv</span><span class="sxs-lookup"><span data-stu-id="e30a5-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="e30a5-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-172">Prohlížení zpráv</span><span class="sxs-lookup"><span data-stu-id="e30a5-172">Peek Message</span></span></td> 
<td><span data-ttu-id="e30a5-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-174">Fronty seznamu ACL nebo Metadata nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="e30a5-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="e30a5-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-176">Vlastnosti fronty služby</span><span class="sxs-lookup"><span data-stu-id="e30a5-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="e30a5-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-178">Zpráva aktualizace</span><span class="sxs-lookup"><span data-stu-id="e30a5-178">Update Message</span></span></td> 
<td><span data-ttu-id="e30a5-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="e30a5-180"><b>Tabulka</b></span><span class="sxs-lookup"><span data-stu-id="e30a5-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="e30a5-181">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="e30a5-181">Create Table</span></span></td> 
<td><span data-ttu-id="e30a5-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-183">Odstranit Entity nebo tabulku</span><span class="sxs-lookup"><span data-stu-id="e30a5-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="e30a5-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-185">Vložení a sloučení nebo nahrazení Entity</span><span class="sxs-lookup"><span data-stu-id="e30a5-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="e30a5-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-187">Dotaz entity</span><span class="sxs-lookup"><span data-stu-id="e30a5-187">Query Entities</span></span></td> 
<td><span data-ttu-id="e30a5-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-189">Dotazu na tabulky</span><span class="sxs-lookup"><span data-stu-id="e30a5-189">Query Tables</span></span></td> 
<td><span data-ttu-id="e30a5-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-191">Tabulka seznamu ACL nebo vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e30a5-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="e30a5-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Začínáme se službou Azure tabulky v jazyce Java</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="e30a5-193">Aktualizace Entity</span><span class="sxs-lookup"><span data-stu-id="e30a5-193">Update Entity</span></span></td> 
<td><span data-ttu-id="e30a5-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Ukázky knihovny klienta Java úložiště</a></span><span class="sxs-lookup"><span data-stu-id="e30a5-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="e30a5-195">Knihovna Azure ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="e30a5-195">Azure Code Samples library</span></span>

<span data-ttu-id="e30a5-196">Chcete-li zobrazit knihovně ucelenou ukázku, přejděte na [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=storage) knihovny, která obsahuje ukázky pro úložiště Azure, které můžete stáhnout a spustit místně.</span><span class="sxs-lookup"><span data-stu-id="e30a5-196">To view the complete sample library, go to the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="e30a5-197">Knihovna ukázka kódu obsahuje ukázkový kód ve formátu ZIP.</span><span class="sxs-lookup"><span data-stu-id="e30a5-197">The Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="e30a5-198">Alternativně můžete procházet a naklonujte úložiště GitHub pro jednotlivé vzorky.</span><span class="sxs-lookup"><span data-stu-id="e30a5-198">Alternatively, you can browse and clone the GitHub repository for each sample.</span></span>

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="e30a5-199">Získávání příručky Začínáme</span><span class="sxs-lookup"><span data-stu-id="e30a5-199">Getting started guides</span></span>

<span data-ttu-id="e30a5-200">Pokud hledáte pokyny, jak nainstalovat a začít pracovat s knihovny klienta Azure Storage, projděte si následující příručky.</span><span class="sxs-lookup"><span data-stu-id="e30a5-200">Check out the following guides if you are looking for instructions on how to install and get started with the Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="e30a5-201">Začínáme se službou objektů Blob v Azure v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="e30a5-201">Getting Started with Azure Blob Service in Java</span></span>](../blobs/storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="e30a5-202">Začínáme se službou Azure fronty v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="e30a5-202">Getting Started with Azure Queue Service in Java</span></span>](../storage-java-how-to-use-queue-storage.md)
* [<span data-ttu-id="e30a5-203">Začínáme se službou Azure tabulky v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="e30a5-203">Getting Started with Azure Table Service in Java</span></span>](../../cosmos-db/table-storage-how-to-use-java.md)
* [<span data-ttu-id="e30a5-204">Začínáme se službou Azure souboru v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="e30a5-204">Getting Started with Azure File Service in Java</span></span>](../storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a><span data-ttu-id="e30a5-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e30a5-205">Next steps</span></span>

<span data-ttu-id="e30a5-206">Informace o ukázky pro jiné jazyky:</span><span class="sxs-lookup"><span data-stu-id="e30a5-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="e30a5-207">Rozhraní .NET: [ukázky azure Storage pomocí rozhraní .NET](../storage-samples-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="e30a5-207">.NET: [Azure Storage samples using .NET](../storage-samples-dotnet.md)</span></span>
* <span data-ttu-id="e30a5-208">Všechny ostatní jazyky: [ukázky Azure Storage](../storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="e30a5-208">All other languages: [Azure Storage samples](../storage-samples.md)</span></span>
