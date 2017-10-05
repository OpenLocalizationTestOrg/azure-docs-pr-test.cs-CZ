---
title: "Referenční informace pro vývojáře řešení založených na Azure Data Factory"
description: "Další informace o různé způsoby, jak vytvořit, sledovat a spravovat Azure data Factory"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: dc752fa1-a6c3-4753-904e-9f32d0a940b7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2017
ms.author: spelluru
redirect_url: https://azure.microsoft.com/services/data-factory/
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: e0f6c078b60df6bb7381d066bebb3e400b3b97ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-data-factory-developer-reference"></a><span data-ttu-id="233d9-103">Referenční informace pro vývojáře řešení založených na Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="233d9-103">Azure Data Factory Developer Reference</span></span>
<span data-ttu-id="233d9-104">Můžete vytvořit, sledovat a spravovat objekty Factory pomocí portálu Azure, Azure PowerShell, knihovna tříd rozhraní .NET nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="233d9-104">You can create, monitor, and manage the factories using either Azure portal, Azure PowerShell, .NET Class Library, or REST API.</span></span>

| <span data-ttu-id="233d9-105">Metoda</span><span class="sxs-lookup"><span data-stu-id="233d9-105">Method</span></span> | <span data-ttu-id="233d9-106">Umístění prostředku</span><span class="sxs-lookup"><span data-stu-id="233d9-106">Resource Location</span></span> | <span data-ttu-id="233d9-107">Odkazy na vývojáře</span><span class="sxs-lookup"><span data-stu-id="233d9-107">Developer References</span></span> |
| --- | --- | --- |
| <span data-ttu-id="233d9-108">portál Azure</span><span class="sxs-lookup"><span data-stu-id="233d9-108">Azure portal</span></span> |[<span data-ttu-id="233d9-109">https://Portal.Azure.com/</span><span class="sxs-lookup"><span data-stu-id="233d9-109">https://portal.azure.com/</span></span>](https://portal.azure.com) |[<span data-ttu-id="233d9-110">Začínáme s Azure Data Factory (portál Azure)</span><span class="sxs-lookup"><span data-stu-id="233d9-110">Get started with Azure Data Factory (Azure portal)</span></span>](data-factory-build-your-first-pipeline-using-editor.md) |
| <span data-ttu-id="233d9-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="233d9-111">Azure PowerShell</span></span> |<span data-ttu-id="233d9-112">Stáhněte si nejnovější [prostředí Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="233d9-112">Download the latest [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)</span></span> |[<span data-ttu-id="233d9-113">Reference k rutině</span><span class="sxs-lookup"><span data-stu-id="233d9-113">Cmdlet reference</span></span>](https://msdn.microsoft.com/library/dn820234.aspx) |
| <span data-ttu-id="233d9-114">Knihovna tříd rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="233d9-114">.NET Class Library</span></span> |<span data-ttu-id="233d9-115">.NET SDK služby Azure Data Factory můžete vytvářet, monitorovat a spravovat Azure data Factory a rozšířit služby Data Factory pomocí aktivity .NET.</span><span class="sxs-lookup"><span data-stu-id="233d9-115">The Azure Data Factory .NET SDK enables you to create, monitor, and manage Azure data factories and extend Data Factory using a .NET activity.</span></span> <span data-ttu-id="233d9-116">V tématu [použít vlastní aktivity v kanálu Azure Data Factory](data-factory-use-custom-activities.md) a [vytvořit, sledovat a spravovat objekty pro vytváření dat Azure pomocí sady .NET SDK Data Factory](data-factory-create-data-factories-programmatically.md) články, které vám pomohou začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="233d9-116">See [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) and [Create, monitor, and manage Azure data factories using Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) articles to help you get started.</span></span><br/><br/><span data-ttu-id="233d9-117"><b>Stažení nejnovější Nuget</b></span><span class="sxs-lookup"><span data-stu-id="233d9-117"><b>Downloading the latest Nuget</b></span></span><br/><span data-ttu-id="233d9-118">Můžete si stáhnout nejnovější balíček Nuget knihovny Azure Data Factory správy z: [https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/)</span><span class="sxs-lookup"><span data-stu-id="233d9-118">You can download the latest Azure Data Factory Management Library Nuget package from: [https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/)</span></span><br/><br/><span data-ttu-id="233d9-119">**Pomocí konzoly Správce balíčků v sadě Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="233d9-119">**Using Package Manager Console in Visual Studio**</span></span><br/><span data-ttu-id="233d9-120">Spuštěním následujícího příkazu v sadě Visual Studio Konzola správce balíčků získat nejnovější Knihovna správy Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="233d9-120">You can run the following command in Visual Studio’s Package Manager Console to get the latest Azure Data Factory Management Library</span></span><br/><br/><span data-ttu-id="233d9-121">Install-Package Microsoft.Azure.Management.DataFactories</span><span class="sxs-lookup"><span data-stu-id="233d9-121">Install-Package Microsoft.Azure.Management.DataFactories</span></span> |[<span data-ttu-id="233d9-122">.NET SDK – referenční informace</span><span class="sxs-lookup"><span data-stu-id="233d9-122">.NET SDK Reference</span></span>](https://msdn.microsoft.com/library/mt415893.aspx) |
| <span data-ttu-id="233d9-123">REST API</span><span class="sxs-lookup"><span data-stu-id="233d9-123">REST API</span></span> |<span data-ttu-id="233d9-124">REST API služby Data Factory můžete vytvářet, sledovat a spravovat Azure data Factory.</span><span class="sxs-lookup"><span data-stu-id="233d9-124">You can use the Data Factory REST API to create, monitor, and manage Azure data factories.</span></span> |[<span data-ttu-id="233d9-125">Reference k rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="233d9-125">REST API Reference</span></span>](https://msdn.microsoft.com/library/dn906738.aspx) |

