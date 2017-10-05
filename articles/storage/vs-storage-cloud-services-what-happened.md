---
title: "Co se stalo s Moje projekt cloudové služby? | Dokumentace Microsoftu"
description: "Popisuje, co se stane v projektu cloudové služby po připojení k účtu úložiště Azure pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 4e0d4864c2fad624fbde39080146dc62ebebff09
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a><span data-ttu-id="80cba-104">Co se stalo s Moje projekt cloudových služeb (Visual Studio Azure Storage připojeno service)?</span><span class="sxs-lookup"><span data-stu-id="80cba-104">What happened to my cloud services project (Visual Studio Azure Storage connected service)?</span></span>
## <a name="references-added"></a><span data-ttu-id="80cba-105">Přidanými referencemi</span><span class="sxs-lookup"><span data-stu-id="80cba-105">References added</span></span>
<span data-ttu-id="80cba-106">Balíček NuGet úložiště Azure byl přidán do projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="80cba-106">The Azure Storage NuGet package was added to your Visual Studio project.</span></span>  
<span data-ttu-id="80cba-107">Tento balíček přidá následující odkazy na rozhraní .NET:</span><span class="sxs-lookup"><span data-stu-id="80cba-107">This package adds the following .NET references:</span></span>

* <span data-ttu-id="80cba-108">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="80cba-108">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="80cba-109">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="80cba-109">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="80cba-110">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="80cba-110">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="80cba-111">**Microsoft.WindowsAzure.Configuration**</span><span class="sxs-lookup"><span data-stu-id="80cba-111">**Microsoft.WindowsAzure.Configuration**</span></span>
* <span data-ttu-id="80cba-112">**Microsoft.WindowsAzure.Storage**</span><span class="sxs-lookup"><span data-stu-id="80cba-112">**Microsoft.WindowsAzure.Storage**</span></span>
* <span data-ttu-id="80cba-113">**Newtonsoft.Json**</span><span class="sxs-lookup"><span data-stu-id="80cba-113">**Newtonsoft.Json**</span></span>
* <span data-ttu-id="80cba-114">**System.Data**</span><span class="sxs-lookup"><span data-stu-id="80cba-114">**System.Data**</span></span>
* <span data-ttu-id="80cba-115">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="80cba-115">**System.Spatial**</span></span>

## <a name="connection-string-for-azure-storage-added"></a><span data-ttu-id="80cba-116">Připojovací řetězec pro Azure Storage přidán</span><span class="sxs-lookup"><span data-stu-id="80cba-116">Connection string for Azure Storage added</span></span>
<span data-ttu-id="80cba-117">Elementy byly vytvořeny s připojovací řetězec a klíč účtu vybrané úložiště.</span><span class="sxs-lookup"><span data-stu-id="80cba-117">Elements were created with the selected storage account's connection string and key.</span></span> <span data-ttu-id="80cba-118">Změny byly provedeny následující soubory:</span><span class="sxs-lookup"><span data-stu-id="80cba-118">Modifications were made to the following files:</span></span>

* <span data-ttu-id="80cba-119">**ServiceDefinition.csdef**</span><span class="sxs-lookup"><span data-stu-id="80cba-119">**ServiceDefinition.csdef**</span></span>
* <span data-ttu-id="80cba-120">**ServiceConfiguration.Cloud.cscfg**</span><span class="sxs-lookup"><span data-stu-id="80cba-120">**ServiceConfiguration.Cloud.cscfg**</span></span>
* <span data-ttu-id="80cba-121">**ServiceConfiguration.Local.cscfg**</span><span class="sxs-lookup"><span data-stu-id="80cba-121">**ServiceConfiguration.Local.cscfg**</span></span>

