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
# <a name="introduction-tooqueues"></a>TooQueues Úvod

Azure Queue storage je služba pro ukládání velkého počtu zpráv, které lze přistupovat z libovolné místo v hello, world prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS. Zpráva s jednou frontou může být až velikost too64 KB a jedna fronta můžete obsahovat miliony zpráv, až toohello limit celkové kapacity účtu úložiště.

## <a name="common-uses"></a>Běžná použití

Toto jsou některá běžná použití úložiště služby Queue Storage:

* Vytvoření backlogu práce tooprocess asynchronně
* Předávání zpráv z webové role tooan pracovního procesu Azure role Azure

## <a name="queue-service-concepts"></a>Koncepty služby front

Hello služba front obsahuje hello následující součásti:

![Koncepty fronty](./media/storage-queues-introduction/queue1.png)

* **Formát adresy URL:** fronty jsou adresovatelné hello následující formát adresy URL:   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    Následující adresa URL Hello řeší frontu v diagramu hello:  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* **Účet úložiště:** všechny přístup tooAzure úložiště se provádí prostřednictvím účtu úložiště. Podrobné informace o kapacitě účtu úložiště najdete v článku [Škálovatelnost a cíle výkonnosti služby Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

* **Fronta:** Fronta obsahuje sadu zpráv. Všechny zprávy musí být ve frontě. Všimněte si, že název fronty hello musí být psaný malými písmeny. Informace o pojmenování front najdete v tématu [Pojmenování front a metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **Zpráva:** A zprávy, v libovolném formátu o až too64 KB. Hello maximální dobu, kterou může zpráva zůstat ve frontě hello je sedm dní.

## <a name="next-steps"></a>Další kroky

* [Vytvoření účtu úložiště](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Začínáme s fronty pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md)
