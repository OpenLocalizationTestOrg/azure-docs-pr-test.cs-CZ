---
title: "Ukázky aaaAzure úložiště pomocí rozhraní .NET | Microsoft Docs"
description: "Zobrazit, stáhnout a spustit ukázkový kód a aplikace pro Azure Storage. Umožňuje zjistit Začínáme ukázky pro objekty BLOB, fronty, tabulky a soubory, pomocí knihovny klienta úložiště hello .NET."
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
ms.openlocfilehash: 9a7055645b0f0658b850f024b8b19ab19840330e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-samples-using-net"></a><span data-ttu-id="1cea6-104">Ukázek Azure Storage pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1cea6-104">Azure Storage samples using .NET</span></span>

## <a name="net-sample-index"></a><span data-ttu-id="1cea6-105">Ukázka index rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1cea6-105">.NET sample index</span></span>

<span data-ttu-id="1cea6-106">Hello následující tabulka obsahuje přehled ukázek scénáře úložiště a hello zahrnutých v každém vzorku.</span><span class="sxs-lookup"><span data-stu-id="1cea6-106">hello following table provides an overview of our samples repository and hello scenarios covered in each sample.</span></span> <span data-ttu-id="1cea6-107">Klikněte na hello odkazy tooview hello odpovídající ukázkový kód v Githubu.</span><span class="sxs-lookup"><span data-stu-id="1cea6-107">Click on hello links tooview hello corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="1cea6-108">Koncový bod</span><span class="sxs-lookup"><span data-stu-id="1cea6-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="1cea6-109">Scénář</span><span class="sxs-lookup"><span data-stu-id="1cea6-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="1cea6-110">Ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="1cea6-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="1cea6-111"><b>Objekt BLOB</b></span><span class="sxs-lookup"><span data-stu-id="1cea6-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="1cea6-112">Append – objekt Blob</span><span class="sxs-lookup"><span data-stu-id="1cea6-112">Append Blob</span></span></td> 
<td><span data-ttu-id="1cea6-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">Příklad CloudBlobContainer.GetAppendBlobReference – metoda</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">CloudBlobContainer.GetAppendBlobReference Method Example</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-114">Objekt Blob bloku</span><span class="sxs-lookup"><span data-stu-id="1cea6-114">Block Blob</span></span></td>
<td><span data-ttu-id="1cea6-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie webové aplikace</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-116">Šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="1cea6-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="1cea6-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Ukázky šifrování objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Blob Encryption Samples</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-118">Zkopírování objektu Blob</span><span class="sxs-lookup"><span data-stu-id="1cea6-118">Copy Blob</span></span></td>
<td><span data-ttu-id="1cea6-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-120">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="1cea6-120">Create Container</span></span></td>
<td><span data-ttu-id="1cea6-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie webové aplikace</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-122">Odstranit objekt Blob</span><span class="sxs-lookup"><span data-stu-id="1cea6-122">Delete Blob</span></span></td>
<td><span data-ttu-id="1cea6-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie webové aplikace</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-124">Odstranit kontejner</span><span class="sxs-lookup"><span data-stu-id="1cea6-124">Delete Container</span></span></td>
<td><span data-ttu-id="1cea6-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-126">Objekt BLOB Metadata nebo vlastnosti nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="1cea6-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="1cea6-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-128">Seznam ACL nebo Metadata nebo vlastnosti kontejneru</span><span class="sxs-lookup"><span data-stu-id="1cea6-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="1cea6-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie webové aplikace</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-130">Get rozsahů stránek</span><span class="sxs-lookup"><span data-stu-id="1cea6-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="1cea6-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-132">Zapůjčení nebo kontejner objektů Blob</span><span class="sxs-lookup"><span data-stu-id="1cea6-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="1cea6-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-134">Seznam objektů Blob nebo kontejneru</span><span class="sxs-lookup"><span data-stu-id="1cea6-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="1cea6-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-136">Objekt Blob stránky</span><span class="sxs-lookup"><span data-stu-id="1cea6-136">Page Blob</span></span></td>
<td><span data-ttu-id="1cea6-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="1cea6-138">SAS</span><span class="sxs-lookup"><span data-stu-id="1cea6-138">SAS</span></span></td>
<td><span data-ttu-id="1cea6-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="1cea6-140">Vlastnosti služby</span><span class="sxs-lookup"><span data-stu-id="1cea6-140">Service Properties</span></span></td>
<td><span data-ttu-id="1cea6-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Začínáme se službou objektů BLOB</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="1cea6-142">Objekt Blob snímku</span><span class="sxs-lookup"><span data-stu-id="1cea6-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="1cea6-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Disky zálohování virtuálních počítačů Azure s přírůstkové snímky</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Backup Azure Virtual Machine Disks with Incremental Snapshots</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="1cea6-144"><b>File</b></span><span class="sxs-lookup"><span data-stu-id="1cea6-144"><b>File</b></span></span></td>
<td><span data-ttu-id="1cea6-145">Vytvoření sdílené složky, adresáře nebo soubory</span><span class="sxs-lookup"><span data-stu-id="1cea6-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="1cea6-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="1cea6-147">Odstranění sdílené složky, adresáře nebo soubory</span><span class="sxs-lookup"><span data-stu-id="1cea6-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="1cea6-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Začínáme se službou Azure souborů v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Getting Started with Azure File Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-149">Directory vlastnosti nebo Metadata</span><span class="sxs-lookup"><span data-stu-id="1cea6-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="1cea6-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-151">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="1cea6-151">Download Files</span></span></td> 
<td><span data-ttu-id="1cea6-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-153">Soubor vlastnosti nebo Metadata nebo metriky</span><span class="sxs-lookup"><span data-stu-id="1cea6-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="1cea6-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-155">Vlastnosti souboru služby</span><span class="sxs-lookup"><span data-stu-id="1cea6-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="1cea6-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-157">Seznam adresářů a souborů</span><span class="sxs-lookup"><span data-stu-id="1cea6-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="1cea6-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="1cea6-159">Zobrazit seznam sdílených složek</span><span class="sxs-lookup"><span data-stu-id="1cea6-159">List Shares</span></span></td> 
<td><span data-ttu-id="1cea6-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="1cea6-161">Sdílené složky vlastnosti nebo Metadata nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="1cea6-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="1cea6-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Ukázka úložiště souboru .NET úložiště Azure</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="1cea6-163"><b>Fronty</b></span><span class="sxs-lookup"><span data-stu-id="1cea6-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="1cea6-164">Přidat zprávu</span><span class="sxs-lookup"><span data-stu-id="1cea6-164">Add Message</span></span></td> 
<td><span data-ttu-id="1cea6-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-166">Šifrování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="1cea6-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="1cea6-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Šifrování na straně klienta úložiště Azure .NET fronty</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure Storage .NET Queue Client-Side Encryption</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-168">Vytvoření fronty</span><span class="sxs-lookup"><span data-stu-id="1cea6-168">Create Queues</span></span></td> 
<td><span data-ttu-id="1cea6-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-170">Odstranit nebo fronta zpráv</span><span class="sxs-lookup"><span data-stu-id="1cea6-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="1cea6-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-172">Prohlížení zpráv</span><span class="sxs-lookup"><span data-stu-id="1cea6-172">Peek Message</span></span></td> 
<td><span data-ttu-id="1cea6-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-174">Fronty seznamu ACL nebo Metadata nebo statistiky</span><span class="sxs-lookup"><span data-stu-id="1cea6-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="1cea6-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-176">Vlastnosti fronty služby</span><span class="sxs-lookup"><span data-stu-id="1cea6-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="1cea6-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-178">Zpráva aktualizace</span><span class="sxs-lookup"><span data-stu-id="1cea6-178">Update Message</span></span></td> 
<td><span data-ttu-id="1cea6-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Začínáme se službou Azure fronty v rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="1cea6-180"><b>Tabulka</b></span><span class="sxs-lookup"><span data-stu-id="1cea6-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="1cea6-181">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="1cea6-181">Create Table</span></span></td> 
<td><span data-ttu-id="1cea6-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Správa souběžnosti použití služby Azure Storage – ukázková aplikace</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-183">Odstranit Entity nebo tabulku</span><span class="sxs-lookup"><span data-stu-id="1cea6-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="1cea6-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Začínáme se službou Azure Table Storage pomocí rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-185">Vložení a sloučení nebo nahrazení Entity</span><span class="sxs-lookup"><span data-stu-id="1cea6-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="1cea6-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Správa souběžnosti použití služby Azure Storage – ukázková aplikace</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-187">Dotaz entity</span><span class="sxs-lookup"><span data-stu-id="1cea6-187">Query Entities</span></span></td> 
<td><span data-ttu-id="1cea6-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Začínáme se službou Azure Table Storage pomocí rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-189">Dotazu na tabulky</span><span class="sxs-lookup"><span data-stu-id="1cea6-189">Query Tables</span></span></td> 
<td><span data-ttu-id="1cea6-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Začínáme se službou Azure Table Storage pomocí rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-191">Tabulka seznamu ACL nebo vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1cea6-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="1cea6-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Začínáme se službou Azure Table Storage pomocí rozhraní .NET</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1cea6-193">Aktualizace Entity</span><span class="sxs-lookup"><span data-stu-id="1cea6-193">Update Entity</span></span></td> 
<td><span data-ttu-id="1cea6-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Správa souběžnosti použití služby Azure Storage – ukázková aplikace</a></span><span class="sxs-lookup"><span data-stu-id="1cea6-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="1cea6-195">Knihovna Azure ukázky kódu</span><span class="sxs-lookup"><span data-stu-id="1cea6-195">Azure Code Samples library</span></span>

<span data-ttu-id="1cea6-196">tooview hello ucelenou ukázku knihovny, přejděte toohello [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=storage) knihovny, která obsahuje ukázky pro úložiště Azure, které můžete stáhnout a spustit místně.</span><span class="sxs-lookup"><span data-stu-id="1cea6-196">tooview hello complete sample library, go toohello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="1cea6-197">Hello knihovně ukázka kódu obsahuje ukázkový kód ve formátu ZIP.</span><span class="sxs-lookup"><span data-stu-id="1cea6-197">hello Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="1cea6-198">Alternativně můžete procházet a klonovat úložiště GitHub hello pro jednotlivé vzorky.</span><span class="sxs-lookup"><span data-stu-id="1cea6-198">Alternatively, you can browse and clone hello GitHub repository for each sample.</span></span>

[!INCLUDE [storage-dotnet-samples-include](../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="1cea6-199">Získávání příručky Začínáme</span><span class="sxs-lookup"><span data-stu-id="1cea6-199">Getting started guides</span></span>

<span data-ttu-id="1cea6-200">Podívejte se na následující příručky, pokud hledáte pokyny, jak hello tooinstall a začátek práce s hello knihovny klienta Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="1cea6-200">Check out hello following guides if you are looking for instructions on how tooinstall and get started with hello Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="1cea6-201">Začínáme se službou objektů Blob v Azure v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1cea6-201">Getting Started with Azure Blob Service in .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="1cea6-202">Začínáme se službou Azure fronty v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1cea6-202">Getting Started with Azure Queue Service in .NET</span></span>](storage-dotnet-how-to-use-queues.md)
* [<span data-ttu-id="1cea6-203">Začínáme se službou Azure tabulky v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1cea6-203">Getting Started with Azure Table Service in .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="1cea6-204">Začínáme se službou Azure souborů v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1cea6-204">Getting Started with Azure File Service in .NET</span></span>](storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a><span data-ttu-id="1cea6-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1cea6-205">Next steps</span></span>

<span data-ttu-id="1cea6-206">Informace o ukázky pro jiné jazyky:</span><span class="sxs-lookup"><span data-stu-id="1cea6-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="1cea6-207">Java: [ukázky úložiště Azure se používá Java](storage-samples-java.md)</span><span class="sxs-lookup"><span data-stu-id="1cea6-207">Java: [Azure Storage samples using Java](storage-samples-java.md)</span></span>
* <span data-ttu-id="1cea6-208">Všechny ostatní jazyky: [ukázky Azure Storage](storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="1cea6-208">All other languages: [Azure Storage samples](storage-samples.md)</span></span>
