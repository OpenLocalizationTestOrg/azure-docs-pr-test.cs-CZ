---
title: "aaaAzure referenční informace pro vývojáře objekt pro vytváření dat"
description: "Další informace o toocreate různé způsoby, sledovat a spravovat Azure data Factory"
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
ms.openlocfilehash: 5384f2209ff0f788527542ddd400c1d163d587c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory-developer-reference"></a><span data-ttu-id="6a7a1-103">Referenční informace pro vývojáře řešení založených na Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="6a7a1-103">Azure Data Factory Developer Reference</span></span>
<span data-ttu-id="6a7a1-104">Můžete vytvořit, sledovat a spravovat objekty Factory hello pomocí portálu Azure, Azure PowerShell, knihovna tříd rozhraní .NET nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="6a7a1-104">You can create, monitor, and manage hello factories using either Azure portal, Azure PowerShell, .NET Class Library, or REST API.</span></span>

| <span data-ttu-id="6a7a1-105">Metoda</span><span class="sxs-lookup"><span data-stu-id="6a7a1-105">Method</span></span> | <span data-ttu-id="6a7a1-106">Umístění prostředku</span><span class="sxs-lookup"><span data-stu-id="6a7a1-106">Resource Location</span></span> | <span data-ttu-id="6a7a1-107">Odkazy na vývojáře</span><span class="sxs-lookup"><span data-stu-id="6a7a1-107">Developer References</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6a7a1-108">portál Azure</span><span class="sxs-lookup"><span data-stu-id="6a7a1-108">Azure portal</span></span> |[<span data-ttu-id="6a7a1-109">https://Portal.Azure.com/</span><span class="sxs-lookup"><span data-stu-id="6a7a1-109">https://portal.azure.com/</span></span>](https://portal.azure.com) |[<span data-ttu-id="6a7a1-110">Začínáme s Azure Data Factory (portál Azure)</span><span class="sxs-lookup"><span data-stu-id="6a7a1-110">Get started with Azure Data Factory (Azure portal)</span></span>](data-factory-build-your-first-pipeline-using-editor.md) |
| <span data-ttu-id="6a7a1-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a7a1-111">Azure PowerShell</span></span> |<span data-ttu-id="6a7a1-112">Stáhněte si nejnovější hello [prostředí Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="6a7a1-112">Download hello latest [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)</span></span> |[<span data-ttu-id="6a7a1-113">Reference k rutině</span><span class="sxs-lookup"><span data-stu-id="6a7a1-113">Cmdlet reference</span></span>](https://msdn.microsoft.com/library/dn820234.aspx) |
| <span data-ttu-id="6a7a1-114">Knihovna tříd rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="6a7a1-114">.NET Class Library</span></span> |<span data-ttu-id="6a7a1-115">umožňuje .NET SDK služby Azure Data Factory Hello toocreate můžete sledovat a správa Azure data Factory a rozšířit služby Data Factory pomocí aktivity .NET.</span><span class="sxs-lookup"><span data-stu-id="6a7a1-115">hello Azure Data Factory .NET SDK enables you toocreate, monitor, and manage Azure data factories and extend Data Factory using a .NET activity.</span></span> <span data-ttu-id="6a7a1-116">V tématu [použít vlastní aktivity v kanálu Azure Data Factory](data-factory-use-custom-activities.md) a [vytvořit, sledovat a spravovat objekty pro vytváření dat Azure pomocí sady .NET SDK Data Factory](data-factory-create-data-factories-programmatically.md) články toohelp začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="6a7a1-116">See [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) and [Create, monitor, and manage Azure data factories using Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) articles toohelp you get started.</span></span><br/><br/><span data-ttu-id="6a7a1-117"><b>Stahování hello nejnovější Nuget</b></span><span class="sxs-lookup"><span data-stu-id="6a7a1-117"><b>Downloading hello latest Nuget</b></span></span><br/><span data-ttu-id="6a7a1-118">Si můžete stáhnout nejnovější balíček Nuget knihovny Azure Data Factory správu hello z: [https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/)</span><span class="sxs-lookup"><span data-stu-id="6a7a1-118">You can download hello latest Azure Data Factory Management Library Nuget package from: [https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/)</span></span><br/><br/><span data-ttu-id="6a7a1-119">**Pomocí konzoly Správce balíčků v sadě Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="6a7a1-119">**Using Package Manager Console in Visual Studio**</span></span><br/><span data-ttu-id="6a7a1-120">Můžete spustit hello následující příkaz v sadě Visual Studio Konzola správce balíčků tooget hello nejnovější Azure Data Factory správy knihovny</span><span class="sxs-lookup"><span data-stu-id="6a7a1-120">You can run hello following command in Visual Studio’s Package Manager Console tooget hello latest Azure Data Factory Management Library</span></span><br/><br/><span data-ttu-id="6a7a1-121">Install-Package Microsoft.Azure.Management.DataFactories</span><span class="sxs-lookup"><span data-stu-id="6a7a1-121">Install-Package Microsoft.Azure.Management.DataFactories</span></span> |[<span data-ttu-id="6a7a1-122">.NET SDK – referenční informace</span><span class="sxs-lookup"><span data-stu-id="6a7a1-122">.NET SDK Reference</span></span>](https://msdn.microsoft.com/library/mt415893.aspx) |
| <span data-ttu-id="6a7a1-123">REST API</span><span class="sxs-lookup"><span data-stu-id="6a7a1-123">REST API</span></span> |<span data-ttu-id="6a7a1-124">Můžete použít hello toocreate REST API služby Data Factory, sledovat a spravovat Azure data Factory.</span><span class="sxs-lookup"><span data-stu-id="6a7a1-124">You can use hello Data Factory REST API toocreate, monitor, and manage Azure data factories.</span></span> |[<span data-ttu-id="6a7a1-125">Reference k rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="6a7a1-125">REST API Reference</span></span>](https://msdn.microsoft.com/library/dn906738.aspx) |

