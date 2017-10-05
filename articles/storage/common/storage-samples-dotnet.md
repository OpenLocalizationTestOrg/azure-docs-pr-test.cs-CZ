---
title: "Ukázek Azure Storage pomocí rozhraní .NET | Microsoft Docs"
description: "Zobrazit, stáhnout a spustit ukázkový kód a aplikace pro Azure Storage. Umožňuje zjistit Začínáme ukázky pro objekty BLOB, fronty, tabulky a soubory, pomocí knihovny klienta úložiště .NET."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: 74777ed14ebb41ad31657f814e86724ff1e5e62e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-samples-using-net"></a><span data-ttu-id="7c02f-104">Ukázek Azure Storage pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7c02f-104">Azure Storage samples using .NET</span></span>

## <a name="net-sample-index"></a><span data-ttu-id="7c02f-105">Ukázka index rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7c02f-105">.NET sample index</span></span>

<span data-ttu-id="7c02f-106">Následující tabulka obsahuje přehled o našem úložišti ukázky a pokryté v každé ukázkové scénáře.</span><span class="sxs-lookup"><span data-stu-id="7c02f-106">The following table provides an overview of our samples repository and the scenarios covered in each sample.</span></span> <span data-ttu-id="7c02f-107">Kliknutím na odkazy k zobrazení odpovídající ukázkový kód v Githubu.</span><span class="sxs-lookup"><span data-stu-id="7c02f-107">Click on the links to view the corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="7c02f-108">Koncový bod</span><span class="sxs-lookup"><span data-stu-id="7c02f-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="7c02f-109">Scénář</span><span class="sxs-lookup"><span data-stu-id="7c02f-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="7c02f-110">Ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="7c02f-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="7c02f-111"><b>Objekt BLOB</b></span><span class="sxs-lookup"><span data-stu-id="7c02f-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="7c02f-112">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="7c02f-112">Append Blob</span></span></td> 
<td><span data-ttu-id="7c02f-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">Příklad CloudBlobContainer.GetAppendBlobReference – metoda</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">CloudBlobContainer.GetAppendBlobReference Method Example</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-114">Objekt Blob bloku</span><span class="sxs-lookup"><span data-stu-id="7c02f-114">Block Blob</span></span></td>
<td><span data-ttu-id="7c02f-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie webové aplikace</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-116">Šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="7c02f-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="7c02f-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Ukázky šifrování objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Blob Encryption Samples</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-118">Zkopírování objektu Blob</span><span class="sxs-lookup"><span data-stu-id="7c02f-118">Copy Blob</span></span></td>
<td><span data-ttu-id="7c02f-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-120">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="7c02f-120">Create Container</span></span></td>
<td><span data-ttu-id="7c02f-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie webové aplikace</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-122">Odstranit objekt Blob</span><span class="sxs-lookup"><span data-stu-id="7c02f-122">Delete Blob</span></span></td>
<td><span data-ttu-id="7c02f-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie webové aplikace</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-124">Odstranit kontejner</span><span class="sxs-lookup"><span data-stu-id="7c02f-124">Delete Container</span></span></td>
<td><span data-ttu-id="7c02f-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-126">Objekt BLOB Metadata nebo vlastnosti nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="7c02f-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="7c02f-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-128">Seznam ACL nebo Metadata nebo vlastnosti kontejneru</span><span class="sxs-lookup"><span data-stu-id="7c02f-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="7c02f-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie webové aplikace</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-130">Get rozsahů stránek</span><span class="sxs-lookup"><span data-stu-id="7c02f-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="7c02f-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-132">Zapůjčení nebo kontejner objektů Blob</span><span class="sxs-lookup"><span data-stu-id="7c02f-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="7c02f-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-134">Seznam objektů Blob nebo kontejneru</span><span class="sxs-lookup"><span data-stu-id="7c02f-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="7c02f-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-136">Objekt Blob stránky</span><span class="sxs-lookup"><span data-stu-id="7c02f-136">Page Blob</span></span></td>
<td><span data-ttu-id="7c02f-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="7c02f-138">SAS</span><span class="sxs-lookup"><span data-stu-id="7c02f-138">SAS</span></span></td>
<td><span data-ttu-id="7c02f-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="7c02f-140">Vlastnosti služby</span><span class="sxs-lookup"><span data-stu-id="7c02f-140">Service Properties</span></span></td>
<td><span data-ttu-id="7c02f-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="7c02f-142">Objekt Blob snímku</span><span class="sxs-lookup"><span data-stu-id="7c02f-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="7c02f-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Disky zálohování virtuálních počítačů Azure s přírůstkové snímky</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Backup Azure Virtual Machine Disks with Incremental Snapshots</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="7c02f-144"><b>File</b></span><span class="sxs-lookup"><span data-stu-id="7c02f-144"><b>File</b></span></span></td>
<td><span data-ttu-id="7c02f-145">Vytvoření sdílené složky, adresáře nebo soubory</span><span class="sxs-lookup"><span data-stu-id="7c02f-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="7c02f-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="7c02f-147">Odstranění sdílené složky, adresáře nebo soubory</span><span class="sxs-lookup"><span data-stu-id="7c02f-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="7c02f-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Začínáme se službou Azure souborů v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Getting Started with Azure File Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-149">Directory vlastnosti nebo Metadata</span><span class="sxs-lookup"><span data-stu-id="7c02f-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="7c02f-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-151">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="7c02f-151">Download Files</span></span></td> 
<td><span data-ttu-id="7c02f-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-153">Soubor vlastnosti nebo Metadata nebo metriky</span><span class="sxs-lookup"><span data-stu-id="7c02f-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="7c02f-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-155">Vlastnosti souboru služby</span><span class="sxs-lookup"><span data-stu-id="7c02f-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="7c02f-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-157">Seznam adresářů a souborů</span><span class="sxs-lookup"><span data-stu-id="7c02f-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="7c02f-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="7c02f-159">Zobrazit seznam sdílených složek</span><span class="sxs-lookup"><span data-stu-id="7c02f-159">List Shares</span></span></td> 
<td><span data-ttu-id="7c02f-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="7c02f-161">Sdílené složky vlastnosti nebo Metadata nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="7c02f-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="7c02f-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="7c02f-163"><b>Fronty</b></span><span class="sxs-lookup"><span data-stu-id="7c02f-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="7c02f-164">Přidat zprávu</span><span class="sxs-lookup"><span data-stu-id="7c02f-164">Add Message</span></span></td> 
<td><span data-ttu-id="7c02f-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-166">Šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="7c02f-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="7c02f-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Šifrování na straně klienta úložiště Azure .NET fronty</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure Storage .NET Queue Client-Side Encryption</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-168">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="7c02f-168">Create Queues</span></span></td> 
<td><span data-ttu-id="7c02f-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-170">Odstranit nebo fronta zpráv</span><span class="sxs-lookup"><span data-stu-id="7c02f-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="7c02f-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-172">Prohlížení zpráv</span><span class="sxs-lookup"><span data-stu-id="7c02f-172">Peek Message</span></span></td> 
<td><span data-ttu-id="7c02f-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-174">Fronty seznamu ACL nebo Metadata nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="7c02f-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="7c02f-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-176">Vlastnosti fronty služby</span><span class="sxs-lookup"><span data-stu-id="7c02f-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="7c02f-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-178">Zpráva aktualizace</span><span class="sxs-lookup"><span data-stu-id="7c02f-178">Update Message</span></span></td> 
<td><span data-ttu-id="7c02f-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="7c02f-180"><b>Tabulka</b></span><span class="sxs-lookup"><span data-stu-id="7c02f-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="7c02f-181">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="7c02f-181">Create Table</span></span></td> 
<td><span data-ttu-id="7c02f-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Správa souběžnosti použití služby Azure Storage – ukázková aplikace</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-183">Odstranit Entity nebo tabulku</span><span class="sxs-lookup"><span data-stu-id="7c02f-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="7c02f-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Začínáme se službou Azure Table Storage pomocí rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-185">Vložení a sloučení nebo nahrazení Entity</span><span class="sxs-lookup"><span data-stu-id="7c02f-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="7c02f-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Správa souběžnosti použití služby Azure Storage – ukázková aplikace</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-187">Dotaz entity</span><span class="sxs-lookup"><span data-stu-id="7c02f-187">Query Entities</span></span></td> 
<td><span data-ttu-id="7c02f-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Začínáme se službou Azure Table Storage pomocí rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-189">Dotazu na tabulky</span><span class="sxs-lookup"><span data-stu-id="7c02f-189">Query Tables</span></span></td> 
<td><span data-ttu-id="7c02f-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Začínáme se službou Azure Table Storage pomocí rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-191">Tabulka seznamu ACL nebo vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7c02f-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="7c02f-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Začínáme se službou Azure Table Storage pomocí rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="7c02f-193">Aktualizace Entity</span><span class="sxs-lookup"><span data-stu-id="7c02f-193">Update Entity</span></span></td> 
<td><span data-ttu-id="7c02f-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Správa souběžnosti použití služby Azure Storage – ukázková aplikace</a></span><span class="sxs-lookup"><span data-stu-id="7c02f-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="7c02f-195">Knihovna Azure ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="7c02f-195">Azure Code Samples library</span></span>

<span data-ttu-id="7c02f-196">Chcete-li zobrazit knihovně ucelenou ukázku, přejděte na [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=storage) knihovny, která obsahuje ukázky pro úložiště Azure, které můžete stáhnout a spustit místně.</span><span class="sxs-lookup"><span data-stu-id="7c02f-196">To view the complete sample library, go to the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="7c02f-197">Knihovna ukázka kódu obsahuje ukázkový kód ve formátu ZIP.</span><span class="sxs-lookup"><span data-stu-id="7c02f-197">The Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="7c02f-198">Alternativně můžete procházet a naklonujte úložiště GitHub pro jednotlivé vzorky.</span><span class="sxs-lookup"><span data-stu-id="7c02f-198">Alternatively, you can browse and clone the GitHub repository for each sample.</span></span>

[!INCLUDE [storage-dotnet-samples-include](../../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="7c02f-199">Získávání příručky Začínáme</span><span class="sxs-lookup"><span data-stu-id="7c02f-199">Getting started guides</span></span>

<span data-ttu-id="7c02f-200">Pokud hledáte pokyny, jak nainstalovat a začít pracovat s knihovny klienta Azure Storage, projděte si následující příručky.</span><span class="sxs-lookup"><span data-stu-id="7c02f-200">Check out the following guides if you are looking for instructions on how to install and get started with the Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="7c02f-201">Začínáme se službou objektů Blob v Azure v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7c02f-201">Getting Started with Azure Blob Service in .NET</span></span>](../blobs/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="7c02f-202">Začínáme se službou Azure fronty v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7c02f-202">Getting Started with Azure Queue Service in .NET</span></span>](../storage-dotnet-how-to-use-queues.md)
* [<span data-ttu-id="7c02f-203">Začínáme se službou Azure tabulky v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7c02f-203">Getting Started with Azure Table Service in .NET</span></span>](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [<span data-ttu-id="7c02f-204">Začínáme se službou Azure souborů v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7c02f-204">Getting Started with Azure File Service in .NET</span></span>](../storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a><span data-ttu-id="7c02f-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7c02f-205">Next steps</span></span>

<span data-ttu-id="7c02f-206">Informace o ukázky pro jiné jazyky:</span><span class="sxs-lookup"><span data-stu-id="7c02f-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="7c02f-207">Java: [ukázky úložiště Azure se používá Java](storage-samples-java.md)</span><span class="sxs-lookup"><span data-stu-id="7c02f-207">Java: [Azure Storage samples using Java](storage-samples-java.md)</span></span>
* <span data-ttu-id="7c02f-208">Všechny ostatní jazyky: [ukázky Azure Storage](../storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="7c02f-208">All other languages: [Azure Storage samples](../storage-samples.md)</span></span>