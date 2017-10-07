---
title: aaaIntroduction tooAzure Queue storage | Microsoft Docs
description: "Úvod tooAzure Queue storage"
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
ms.openlocfilehash: 669effedff7beedde8a119c350a2a70edafedcf0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooqueues"></a><span data-ttu-id="46c21-103">TooQueues Úvod</span><span class="sxs-lookup"><span data-stu-id="46c21-103">Introduction tooQueues</span></span>

<span data-ttu-id="46c21-104">Azure Queue storage je služba pro ukládání velkého počtu zpráv, které lze přistupovat z libovolné místo v hello, world prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="46c21-104">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="46c21-105">Zpráva s jednou frontou může být až velikost too64 KB a jedna fronta můžete obsahovat miliony zpráv, až toohello limit celkové kapacity účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="46c21-105">A single queue message can be up too64 KB in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span>

## <a name="common-uses"></a><span data-ttu-id="46c21-106">Běžná použití</span><span class="sxs-lookup"><span data-stu-id="46c21-106">Common uses</span></span>

<span data-ttu-id="46c21-107">Toto jsou některá běžná použití úložiště služby Queue Storage:</span><span class="sxs-lookup"><span data-stu-id="46c21-107">Common uses of Queue storage include:</span></span>

* <span data-ttu-id="46c21-108">Vytvoření backlogu práce tooprocess asynchronně</span><span class="sxs-lookup"><span data-stu-id="46c21-108">Creating a backlog of work tooprocess asynchronously</span></span>
* <span data-ttu-id="46c21-109">Předávání zpráv z webové role tooan pracovního procesu Azure role Azure</span><span class="sxs-lookup"><span data-stu-id="46c21-109">Passing messages from an Azure web role tooan Azure worker role</span></span>

## <a name="queue-service-concepts"></a><span data-ttu-id="46c21-110">Koncepty služby front</span><span class="sxs-lookup"><span data-stu-id="46c21-110">Queue service concepts</span></span>

<span data-ttu-id="46c21-111">Hello služba front obsahuje hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="46c21-111">hello Queue service contains hello following components:</span></span>

![Koncepty fronty](./media/storage-queues-introduction/queue1.png)

* <span data-ttu-id="46c21-113">**Formát adresy URL:** fronty jsou adresovatelné hello následující formát adresy URL:</span><span class="sxs-lookup"><span data-stu-id="46c21-113">**URL format:** Queues are addressable using hello following URL format:</span></span>   
    <span data-ttu-id="46c21-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span><span class="sxs-lookup"><span data-stu-id="46c21-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span></span> 
  
    <span data-ttu-id="46c21-115">Následující adresa URL Hello řeší frontu v diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="46c21-115">hello following URL addresses a queue in hello diagram:</span></span>  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* <span data-ttu-id="46c21-116">**Účet úložiště:** všechny přístup tooAzure úložiště se provádí prostřednictvím účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="46c21-116">**Storage account:** All access tooAzure Storage is done through a storage account.</span></span> <span data-ttu-id="46c21-117">Podrobné informace o kapacitě účtu úložiště najdete v článku [Škálovatelnost a cíle výkonnosti služby Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="46c21-117">See [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) for details about storage account capacity.</span></span>

* <span data-ttu-id="46c21-118">**Fronta:** Fronta obsahuje sadu zpráv.</span><span class="sxs-lookup"><span data-stu-id="46c21-118">**Queue:** A queue contains a set of messages.</span></span> <span data-ttu-id="46c21-119">Všechny zprávy musí být ve frontě.</span><span class="sxs-lookup"><span data-stu-id="46c21-119">All messages must be in a queue.</span></span> <span data-ttu-id="46c21-120">Všimněte si, že název fronty hello musí být psaný malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="46c21-120">Note that hello queue name must be all lowercase.</span></span> <span data-ttu-id="46c21-121">Informace o pojmenování front najdete v tématu [Pojmenování front a metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).</span><span class="sxs-lookup"><span data-stu-id="46c21-121">For information on naming queues, see [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

* <span data-ttu-id="46c21-122">**Zpráva:** A zprávy, v libovolném formátu o až too64 KB.</span><span class="sxs-lookup"><span data-stu-id="46c21-122">**Message:** A message, in any format, of up too64 KB.</span></span> <span data-ttu-id="46c21-123">Hello maximální dobu, kterou může zpráva zůstat ve frontě hello je sedm dní.</span><span class="sxs-lookup"><span data-stu-id="46c21-123">hello maximum time that a message can remain in hello queue is seven days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="46c21-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="46c21-124">Next steps</span></span>

* [<span data-ttu-id="46c21-125">Vytvoření účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="46c21-125">Create a storage account</span></span>](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [<span data-ttu-id="46c21-126">Začínáme s fronty pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="46c21-126">Getting started with Queues using .NET</span></span>](storage-dotnet-how-to-use-queues.md)
