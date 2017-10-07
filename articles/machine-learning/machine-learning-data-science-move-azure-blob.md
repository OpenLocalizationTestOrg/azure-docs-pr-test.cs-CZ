---
title: aaaMove tooand dat z Azure Blob Storage | Microsoft Docs
description: "Přesun dat tooand z Azure Blob Storage"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d6681e30-ab45-45ea-a9fb-ac8acefe544d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;sachouks
ms.openlocfilehash: e12b8c157955195e826f8b108075afaf25ca7bd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage"></a><span data-ttu-id="30b84-103">Přesun dat tooand z Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="30b84-103">Move data tooand from Azure Blob Storage</span></span>
[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this tooseparate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

<span data-ttu-id="30b84-104">Jakou metodu je pro vás nejvhodnější závisí na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="30b84-104">Which method is best for you depends on your scenario.</span></span> <span data-ttu-id="30b84-105">Hello [scénáře pro pokročilou analýzu v Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) pomáhá určit hello prostředky, které potřebujete pro celou řadu pracovní postupy vědecké účely dat použít v hello pokročilé analýzy procesu.</span><span class="sxs-lookup"><span data-stu-id="30b84-105">hello [Scenarios for advanced analytics in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) article helps you determine hello resources you need for a variety of data science workflows used in hello advanced analytics process.</span></span>

> [!NOTE]
> <span data-ttu-id="30b84-106">Úložiště objektů blob tooAzure dokončení úvod, najdete v části příliš[základy objektu Blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) a příliš[služby objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="30b84-106">For a complete introduction tooAzure blob storage, refer too[Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and too[Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

<span data-ttu-id="30b84-107">Jako alternativu, můžete použít [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) na:</span><span class="sxs-lookup"><span data-stu-id="30b84-107">As an alternative, you can use [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) to:</span></span> 

* <span data-ttu-id="30b84-108">Vytvoření a plánování kanálu, který stahuje dat z Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="30b84-108">create and schedule a pipeline that downloads data from Azure blob storage,</span></span> 
* <span data-ttu-id="30b84-109">předejte ji tooa publikované webové služby Azure Machine Learning,</span><span class="sxs-lookup"><span data-stu-id="30b84-109">pass it tooa published Azure Machine Learning web service,</span></span> 
* <span data-ttu-id="30b84-110">příjem výsledků hello prediktivní analýzy, a</span><span class="sxs-lookup"><span data-stu-id="30b84-110">receive hello predictive analytics results, and</span></span> 
* <span data-ttu-id="30b84-111">Nahrajte toostorage výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="30b84-111">upload hello results toostorage.</span></span> 

<span data-ttu-id="30b84-112">Další informace najdete v tématu [vytvořit prediktivní kanály pomocí Azure Data Factory a Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).</span><span class="sxs-lookup"><span data-stu-id="30b84-112">For more information, see [Create predictive pipelines using Azure Data Factory and Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30b84-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="30b84-113">Prerequisites</span></span>
<span data-ttu-id="30b84-114">Tento dokument předpokládá, že máte předplatné Azure, účet úložiště a hello odpovídající klíč úložiště pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="30b84-114">This document assumes that you have an Azure subscription, a storage account, and hello corresponding storage key for that account.</span></span> <span data-ttu-id="30b84-115">Před nahrávání nebo stahování dat, musíte znát klíč účet a název účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="30b84-115">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="30b84-116">tooset si předplatné Azure, najdete v části [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="30b84-116">tooset up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="30b84-117">Pokyny pro vytvoření účtu úložiště a získávání účet a klíčové informace naleznete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="30b84-117">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

