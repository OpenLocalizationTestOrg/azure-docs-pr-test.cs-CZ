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
# <a name="move-data-tooand-from-azure-blob-storage"></a>Přesun dat tooand z Azure Blob Storage
[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this tooseparate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

Jakou metodu je pro vás nejvhodnější závisí na vašem scénáři. Hello [scénáře pro pokročilou analýzu v Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) pomáhá určit hello prostředky, které potřebujete pro celou řadu pracovní postupy vědecké účely dat použít v hello pokročilé analýzy procesu.

> [!NOTE]
> Úložiště objektů blob tooAzure dokončení úvod, najdete v části příliš[základy objektu Blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) a příliš[služby objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

Jako alternativu, můžete použít [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) na: 

* Vytvoření a plánování kanálu, který stahuje dat z Azure blob storage 
* předejte ji tooa publikované webové služby Azure Machine Learning, 
* příjem výsledků hello prediktivní analýzy, a 
* Nahrajte toostorage výsledky hello. 

Další informace najdete v tématu [vytvořit prediktivní kanály pomocí Azure Data Factory a Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Požadavky
Tento dokument předpokládá, že máte předplatné Azure, účet úložiště a hello odpovídající klíč úložiště pro tento účet. Před nahrávání nebo stahování dat, musíte znát klíč účet a název účtu úložiště Azure.

* tooset si předplatné Azure, najdete v části [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).
* Pokyny pro vytvoření účtu úložiště a získávání účet a klíčové informace naleznete v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md).

