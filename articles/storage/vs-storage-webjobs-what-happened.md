---
title: "došlo k aaaWhat projektu úlohy WebJob toomy (Visual Studio Azure Storage připojeno service)? | Dokumentace Microsoftu"
description: "Popisuje, co se stalo v projektu webové úlohy Azure po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 7735f49b1e7ec8dda30d1262d7ce65454604b610
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-happened-toomy-webjob-project-visual-studio-azure-storage-connected-service"></a>Co se stalo toomy webové úlohy projektu (Visual Studio Azure Storage připojeno service)?
## <a name="references-added"></a>Přidanými referencemi
balíček NuGet úložiště Azure Hello přidala tooor aktualizována v projektu sady Visual Studio.  
Tento balíček přidá hello následující odkazy na rozhraní .NET:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Připojovací řetězec pro Azure Storage přidán
V souboru App.config hello projektu hello **AzureWebJobsStorage** a **AzureWebJobsDashboard** položky, byly aktualizovány připojovací řetězec a klíč účtu úložiště hello vybrané.

Další informace najdete v tématu [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

