---
title: "Úvod do Azure Queue storage | Microsoft Docs"
description: "Úvod do Azure Queue storage"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: robinsh
ms.openlocfilehash: 4db7552a1b76c89151405c55c8682abbb5326bb6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-queues"></a><span data-ttu-id="2844c-103">Seznámení s frontami</span><span class="sxs-lookup"><span data-stu-id="2844c-103">Introduction to Queues</span></span>

<span data-ttu-id="2844c-104">Azure Queue Storage je služba pro ukládání velkého počtu zpráv, ke které můžete získat přístup z jakéhokoli místa na světě prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2844c-104">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="2844c-105">Zpráva s jednou frontou může mít velikost až 64 kB a jedna fronta můžete obsahovat miliony zpráv, až do dosažení celkové kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2844c-105">A single queue message can be up to 64 KB in size, and a queue can contain millions of messages, up to the total capacity limit of a storage account.</span></span>

## <a name="common-uses"></a><span data-ttu-id="2844c-106">Běžná použití</span><span class="sxs-lookup"><span data-stu-id="2844c-106">Common uses</span></span>

<span data-ttu-id="2844c-107">Toto jsou některá běžná použití úložiště služby Queue Storage:</span><span class="sxs-lookup"><span data-stu-id="2844c-107">Common uses of Queue storage include:</span></span>

* <span data-ttu-id="2844c-108">Vytvoření backlogu práce k asynchronnímu zpracování</span><span class="sxs-lookup"><span data-stu-id="2844c-108">Creating a backlog of work to process asynchronously</span></span>
* <span data-ttu-id="2844c-109">Předávání zpráv z webové role Azure do role pracovního procesu Azure</span><span class="sxs-lookup"><span data-stu-id="2844c-109">Passing messages from an Azure web role to an Azure worker role</span></span>

## <a name="queue-service-concepts"></a><span data-ttu-id="2844c-110">Koncepty služby front</span><span class="sxs-lookup"><span data-stu-id="2844c-110">Queue service concepts</span></span>

<span data-ttu-id="2844c-111">Služba front obsahuje následující součásti:</span><span class="sxs-lookup"><span data-stu-id="2844c-111">The Queue service contains the following components:</span></span>

![Koncepty fronty](./media/storage-queues-introduction/queue1.png)

* <span data-ttu-id="2844c-113">**Formát adresy URL:** Fronty jsou adresovatelné v následujícím formátu adresy URL:</span><span class="sxs-lookup"><span data-stu-id="2844c-113">**URL format:** Queues are addressable using the following URL format:</span></span>   
    <span data-ttu-id="2844c-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span><span class="sxs-lookup"><span data-stu-id="2844c-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span></span> 
  
    <span data-ttu-id="2844c-115">Následující adresa URL odkazuje na frontu v diagramu:</span><span class="sxs-lookup"><span data-stu-id="2844c-115">The following URL addresses a queue in the diagram:</span></span>  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* <span data-ttu-id="2844c-116">**Účet úložiště:** veškerý přístup do služby Azure Storage se provádí prostřednictvím účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2844c-116">**Storage account:** All access to Azure Storage is done through a storage account.</span></span> <span data-ttu-id="2844c-117">Podrobné informace o kapacitě účtu úložiště najdete v článku [Škálovatelnost a cíle výkonnosti služby Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2844c-117">See [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) for details about storage account capacity.</span></span>

* <span data-ttu-id="2844c-118">**Fronta:** Fronta obsahuje sadu zpráv.</span><span class="sxs-lookup"><span data-stu-id="2844c-118">**Queue:** A queue contains a set of messages.</span></span> <span data-ttu-id="2844c-119">Všechny zprávy musí být ve frontě.</span><span class="sxs-lookup"><span data-stu-id="2844c-119">All messages must be in a queue.</span></span> <span data-ttu-id="2844c-120">Upozorňujeme, že název fronty musí být psaný malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="2844c-120">Note that the queue name must be all lowercase.</span></span> <span data-ttu-id="2844c-121">Informace o pojmenování front najdete v tématu [Pojmenování front a metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).</span><span class="sxs-lookup"><span data-stu-id="2844c-121">For information on naming queues, see [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

* <span data-ttu-id="2844c-122">**Zpráva:** Zprávu v libovolném formátu o velikosti až 64 kB.</span><span class="sxs-lookup"><span data-stu-id="2844c-122">**Message:** A message, in any format, of up to 64 KB.</span></span> <span data-ttu-id="2844c-123">Maximální doba, kterou může zpráva zůstat ve frontě je sedm dní.</span><span class="sxs-lookup"><span data-stu-id="2844c-123">The maximum time that a message can remain in the queue is seven days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2844c-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2844c-124">Next steps</span></span>

* [<span data-ttu-id="2844c-125">Vytvoření účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="2844c-125">Create a storage account</span></span>](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [<span data-ttu-id="2844c-126">Začínáme s fronty pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="2844c-126">Getting started with Queues using .NET</span></span>](storage-dotnet-how-to-use-queues.md)
