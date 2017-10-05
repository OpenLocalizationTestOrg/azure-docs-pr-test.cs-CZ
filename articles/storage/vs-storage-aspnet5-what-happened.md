---
title: "Co se stalo s projektu ASP.NET 5 (Visual Studio připojené služby) | Microsoft Docs"
description: "Popisuje, co se stane po připojení k účtu úložiště Azure v projektu Visual Studio ASP.NET 5 pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 4390993772eaf35516e48ad7adcdcec5f1df8d71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a><span data-ttu-id="37d9a-103">Co se stalo s projektu ASP.NET 5 (Visual Studio Azure Storage připojeno services)?</span><span class="sxs-lookup"><span data-stu-id="37d9a-103">What happened to my ASP.NET 5 project (Visual Studio Azure Storage connected services)?</span></span>
## <a name="references-added"></a><span data-ttu-id="37d9a-104">Přidanými referencemi</span><span class="sxs-lookup"><span data-stu-id="37d9a-104">References added</span></span>
<span data-ttu-id="37d9a-105">Balíček NuGet úložiště Azure byl přidán do projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="37d9a-105">The Azure Storage NuGet package was added to your Visual Studio project.</span></span>  
<span data-ttu-id="37d9a-106">Tento balíček přidá následující odkazy na rozhraní .NET:</span><span class="sxs-lookup"><span data-stu-id="37d9a-106">This package adds the following .NET references:</span></span>

* <span data-ttu-id="37d9a-107">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="37d9a-107">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="37d9a-108">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="37d9a-108">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="37d9a-109">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="37d9a-109">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="37d9a-110">**Microsoft.WindowsAzure.Configuration**</span><span class="sxs-lookup"><span data-stu-id="37d9a-110">**Microsoft.WindowsAzure.Configuration**</span></span>
* <span data-ttu-id="37d9a-111">**Microsoft.WindowsAzure.Storage**</span><span class="sxs-lookup"><span data-stu-id="37d9a-111">**Microsoft.WindowsAzure.Storage**</span></span>
* <span data-ttu-id="37d9a-112">**Newtonsoft.Json**</span><span class="sxs-lookup"><span data-stu-id="37d9a-112">**Newtonsoft.Json**</span></span>
* <span data-ttu-id="37d9a-113">**System.Data**</span><span class="sxs-lookup"><span data-stu-id="37d9a-113">**System.Data**</span></span>
* <span data-ttu-id="37d9a-114">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="37d9a-114">**System.Spatial**</span></span>

<span data-ttu-id="37d9a-115">Navíc balíček NuGet **Microsoft.Framework.Configuration.Json** byla přidána.</span><span class="sxs-lookup"><span data-stu-id="37d9a-115">Also, the NuGet package **Microsoft.Framework.Configuration.Json** was added.</span></span>

## <a name="connection-string-for-azure-storage-added"></a><span data-ttu-id="37d9a-116">Připojovací řetězec pro Azure Storage přidán</span><span class="sxs-lookup"><span data-stu-id="37d9a-116">Connection string for Azure Storage added</span></span>
<span data-ttu-id="37d9a-117">V souboru config.json projektu byl vytvořen element připojovací řetězec a klíč účtu vybrané úložiště.</span><span class="sxs-lookup"><span data-stu-id="37d9a-117">In the config.json file of your project, an element was created with the selected storage account's connection string and key.</span></span>

<span data-ttu-id="37d9a-118">Další informace najdete v tématu [ASP.NET 5](http://www.asp.net/vnext).</span><span class="sxs-lookup"><span data-stu-id="37d9a-118">For more information, see [ASP.NET 5](http://www.asp.net/vnext).</span></span>

