---
title: "Přesun dat do a z Azure Blob Storage | Microsoft Docs"
description: "Přesun dat z a do služby Azure Blob Storage"
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
ms.openlocfilehash: d9a626cccd3cdfcdc85f000bd3192aef2881e9a6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-to-and-from-azure-blob-storage"></a><span data-ttu-id="9b51e-103">Přesun dat do a z Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="9b51e-103">Move data to and from Azure Blob Storage</span></span>
[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this to separate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

<span data-ttu-id="9b51e-104">Jakou metodu je pro vás nejvhodnější závisí na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="9b51e-104">Which method is best for you depends on your scenario.</span></span> <span data-ttu-id="9b51e-105">[Scénáře pro pokročilou analýzu v Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) pomáhá určit prostředky, které potřebujete pro různé pracovní úlohy vědecké účely data, která je použita v procesu pokročilou analýzu.</span><span class="sxs-lookup"><span data-stu-id="9b51e-105">The [Scenarios for advanced analytics in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) article helps you determine the resources you need for a variety of data science workflows used in the advanced analytics process.</span></span>

> [!NOTE]
> <span data-ttu-id="9b51e-106">Dokončení Úvod do Azure blob storage, najdete v části [základy objektu Blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) a [služby objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="9b51e-106">For a complete introduction to Azure blob storage, refer to [Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and to [Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

<span data-ttu-id="9b51e-107">Jako alternativu, můžete použít [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) na:</span><span class="sxs-lookup"><span data-stu-id="9b51e-107">As an alternative, you can use [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) to:</span></span> 

* <span data-ttu-id="9b51e-108">Vytvoření a plánování kanálu, který stahuje dat z Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="9b51e-108">create and schedule a pipeline that downloads data from Azure blob storage,</span></span> 
* <span data-ttu-id="9b51e-109">předejte ji do publikované webové služby Azure Machine Learning,</span><span class="sxs-lookup"><span data-stu-id="9b51e-109">pass it to a published Azure Machine Learning web service,</span></span> 
* <span data-ttu-id="9b51e-110">Zobrazí výsledky prediktivní analýzy a</span><span class="sxs-lookup"><span data-stu-id="9b51e-110">receive the predictive analytics results, and</span></span> 
* <span data-ttu-id="9b51e-111">Nahrání výsledky do úložiště.</span><span class="sxs-lookup"><span data-stu-id="9b51e-111">upload the results to storage.</span></span> 

<span data-ttu-id="9b51e-112">Další informace najdete v tématu [vytvořit prediktivní kanály pomocí Azure Data Factory a Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).</span><span class="sxs-lookup"><span data-stu-id="9b51e-112">For more information, see [Create predictive pipelines using Azure Data Factory and Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b51e-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9b51e-113">Prerequisites</span></span>
<span data-ttu-id="9b51e-114">Tento dokument předpokládá, že máte předplatné Azure, účet úložiště a odpovídající klíč úložiště pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="9b51e-114">This document assumes that you have an Azure subscription, a storage account, and the corresponding storage key for that account.</span></span> <span data-ttu-id="9b51e-115">Před nahrávání nebo stahování dat, musíte znát klíč účet a název účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="9b51e-115">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="9b51e-116">Předplatné Azure, naleznete v tématu [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9b51e-116">To set up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="9b51e-117">Pokyny pro vytvoření účtu úložiště a získávání účet a klíčové informace naleznete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="9b51e-117">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

