---
title: "Přehled modulu Runtime Azure Functions | Microsoft Docs"
description: "Přehled Azure Functions Runtime Preview"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: cb98d5f2aaa526555820c15ba5a32fb7e78ffc5a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-runtime-overview"></a><span data-ttu-id="3f165-103">Přehled Azure Functions modulu Runtime</span><span class="sxs-lookup"><span data-stu-id="3f165-103">Azure Functions Runtime Overview</span></span>

<span data-ttu-id="3f165-104">Azure Functions Runtime nabízí nový způsob můžete využívat jednoduchost a flexibilitu Azure Functions programovací model na místě.</span><span class="sxs-lookup"><span data-stu-id="3f165-104">The Azure Functions Runtime provides a new way for you to take advantage of the simplicity and flexibility of the Azure Functions programming model on-premises.</span></span> <span data-ttu-id="3f165-105">Postavené na stejném kořeny s otevřeným zdrojem jako funkce Azure, Azure Functions Runtime je nasadit místně a poskytuje prostředí téměř shodná vývoj jako cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="3f165-105">Built on the same open source roots as Azure Functions, Azure Functions Runtime is deployed on-premises to provide a nearly identical development experience as the cloud service.</span></span>

![Modul Runtime Azure Functions na portálu Preview][1]

<span data-ttu-id="3f165-107">Azure Functions Runtime poskytuje způsob, jak jste prostředí Azure Functions před potvrzením do cloudu.</span><span class="sxs-lookup"><span data-stu-id="3f165-107">The Azure Functions Runtime provides a way for you to experience Azure Functions before committing to the cloud.</span></span> <span data-ttu-id="3f165-108">Tímto způsobem kód prostředků, které vytvoříte pak děláte s vámi do cloudu při migraci.</span><span class="sxs-lookup"><span data-stu-id="3f165-108">In this way, the code assets you build can then be taken with you to the cloud when you migrate.</span></span>  <span data-ttu-id="3f165-109">Modul runtime také otevře nové možnosti pro vás, například pomocí k výměně za chodu výpočetní výkon vašich počítačů místní spuštění dávky procesy přes noc.</span><span class="sxs-lookup"><span data-stu-id="3f165-109">The runtime also opens up new options for you, such as using the spare compute power of your on-premises computers to run batch processes overnight.</span></span> <span data-ttu-id="3f165-110">Zařízení můžete také použít v rámci vaší organizace podmíněně odesílání dat do jiných systémů, jak místně a v cloudu.</span><span class="sxs-lookup"><span data-stu-id="3f165-110">You can also use devices within your organization to conditionally send data to other systems, both on-premises and in the cloud.</span></span>

<span data-ttu-id="3f165-111">Azure Functions Runtime se skládá ze dvou částí:</span><span class="sxs-lookup"><span data-stu-id="3f165-111">The Azure Functions Runtime consists of two pieces:</span></span>
* <span data-ttu-id="3f165-112">Role správy Runtime Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3f165-112">Azure Functions Runtime Management Role</span></span>
* <span data-ttu-id="3f165-113">Role pracovního procesu modulu Runtime Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3f165-113">Azure Functions Runtime Worker Role</span></span>

## <a name="azure-functions-management-role"></a><span data-ttu-id="3f165-114">Role správy Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3f165-114">Azure Functions Management Role</span></span>

<span data-ttu-id="3f165-115">Role Azure funkce správy poskytuje pro správu funkce místní hostitel.</span><span class="sxs-lookup"><span data-stu-id="3f165-115">The Azure Functions Management Role provides a host for the management of your Functions on-premises.</span></span> <span data-ttu-id="3f165-116">Tato role se provádí tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="3f165-116">This role performs the following tasks:</span></span>

* <span data-ttu-id="3f165-117">Z portálu Azure funkce správy, který je hostitelem stejný jako ten, můžete vidět v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3f165-117">Hosting of the Azure Functions Management Portal, which is the the same one you see in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="3f165-118">Díky tomu můžete vyvíjet funkcí stejným způsobem, jako byste na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3f165-118">This lets you develop your functions in the same way as you would in the Azure portal.</span></span>
* <span data-ttu-id="3f165-119">Distribuci funkcí mezi více pracovníků funkce.</span><span class="sxs-lookup"><span data-stu-id="3f165-119">Distributing functions across multiple Functions workers.</span></span>
* <span data-ttu-id="3f165-120">Poskytování publikování koncový bod, aby mohly publikovat vaše funkce přímo ze sady Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f165-120">Providing a publishing endpoint so that you can publish your functions direct from Microsoft Visual Studio.</span></span>

## <a name="azure-functions-worker-role"></a><span data-ttu-id="3f165-121">Role pracovního procesu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3f165-121">Azure Functions Worker Role</span></span>

<span data-ttu-id="3f165-122">Role pracovního procesu Azure funkce nasazených v kontejnerech Windows a toto je, kde se provádí funkce kódu.</span><span class="sxs-lookup"><span data-stu-id="3f165-122">The Azure Functions Worker Roles are deployed in Windows Containers and this is where your function code executes.</span></span>  <span data-ttu-id="3f165-123">Můžete nasadit více rolí pracovního procesu ve vaší organizaci a je klíče způsob, ve kterém zákazníci měli používat k výměně za chodu výpočetním výkonu.</span><span class="sxs-lookup"><span data-stu-id="3f165-123">You can deploy multiple Worker Roles throughout your organization and is a key way in which customers can make use of spare compute power.</span></span>

## <a name="minimum-requirements"></a><span data-ttu-id="3f165-124">Minimální požadavky</span><span class="sxs-lookup"><span data-stu-id="3f165-124">Minimum Requirements</span></span>

<span data-ttu-id="3f165-125">Začít s Azure Functions Runtime musí mít počítač s **systému Windows Server 2016 nebo Windows 10 Creators Update** s přístupem k **systému SQL Server** instance.</span><span class="sxs-lookup"><span data-stu-id="3f165-125">To get started with the Azure Functions Runtime you must have a machine with **Windows Server 2016 or Windows 10 Creators Update** with access to a **SQL Server** instance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f165-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f165-126">Next Steps</span></span>

<span data-ttu-id="3f165-127">Nainstalujte [Runtime funkce Azure preview](https://aka.ms/azafr)</span><span class="sxs-lookup"><span data-stu-id="3f165-127">Install the [Azure Functions Runtime preview](https://aka.ms/azafr)</span></span>

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
