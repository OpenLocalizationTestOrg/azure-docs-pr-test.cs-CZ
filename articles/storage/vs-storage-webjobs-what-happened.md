---
title: "Co se stalo s Moje webová úloha projektu (Visual Studio Azure Storage připojeno service)? | Dokumentace Microsoftu"
description: "Popisuje, co se stalo v projektu webové úlohy Azure po připojení k účtu úložiště pomocí sady Visual Studio připojené služby"
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
ms.openlocfilehash: 3b28ddeadc87937941d60b16fae817e59a220b22
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a><span data-ttu-id="f4b04-104">Co se stalo s Moje webová úloha projektu (Visual Studio Azure Storage připojeno service)?</span><span class="sxs-lookup"><span data-stu-id="f4b04-104">What happened to my WebJob project (Visual Studio Azure Storage connected service)?</span></span>
## <a name="references-added"></a><span data-ttu-id="f4b04-105">Přidanými referencemi</span><span class="sxs-lookup"><span data-stu-id="f4b04-105">References Added</span></span>
<span data-ttu-id="f4b04-106">Balíček NuGet úložiště Azure se přidání nebo aktualizaci v projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4b04-106">The Azure Storage NuGet package was added to or updated in your Visual Studio project.</span></span>  
<span data-ttu-id="f4b04-107">Tento balíček přidá následující odkazy na rozhraní .NET:</span><span class="sxs-lookup"><span data-stu-id="f4b04-107">This package adds the following .NET references:</span></span>

* <span data-ttu-id="f4b04-108">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="f4b04-108">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="f4b04-109">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="f4b04-109">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="f4b04-110">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="f4b04-110">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="f4b04-111">**Microsoft.WindowsAzure.ConfigurationManager**</span><span class="sxs-lookup"><span data-stu-id="f4b04-111">**Microsoft.WindowsAzure.ConfigurationManager**</span></span>
* <span data-ttu-id="f4b04-112">**Microsoft.WindowsAzure.Storage**</span><span class="sxs-lookup"><span data-stu-id="f4b04-112">**Microsoft.WindowsAzure.Storage**</span></span>
* <span data-ttu-id="f4b04-113">**Newtonsoft.Json**</span><span class="sxs-lookup"><span data-stu-id="f4b04-113">**Newtonsoft.Json**</span></span>
* <span data-ttu-id="f4b04-114">**System.Data**</span><span class="sxs-lookup"><span data-stu-id="f4b04-114">**System.Data**</span></span>
* <span data-ttu-id="f4b04-115">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="f4b04-115">**System.Spatial**</span></span>

## <a name="connection-string-for-azure-storage-added"></a><span data-ttu-id="f4b04-116">Připojovací řetězec pro Azure Storage přidán</span><span class="sxs-lookup"><span data-stu-id="f4b04-116">Connection string for Azure Storage added</span></span>
<span data-ttu-id="f4b04-117">V souboru App.config vašeho projektu **AzureWebJobsStorage** a **AzureWebJobsDashboard** položky, byly aktualizovány s vybraným účtem úložiště pro připojovací řetězec a klíč.</span><span class="sxs-lookup"><span data-stu-id="f4b04-117">In the App.config file of your project, the **AzureWebJobsStorage** and **AzureWebJobsDashboard** entries were updated with the selected storage account's connection string and key.</span></span>

<span data-ttu-id="f4b04-118">Další informace najdete v tématu [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="f4b04-118">For more information, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>
