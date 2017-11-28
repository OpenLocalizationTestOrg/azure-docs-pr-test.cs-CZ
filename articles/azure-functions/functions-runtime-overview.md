---
title: "aaaAzure přehled Runtime Functions | Microsoft Docs"
description: "Přehled hello Azure funkce Runtime Preview"
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
ms.openlocfilehash: 8ce3e5037731d499c330b395c89c90109d18d65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-runtime-overview"></a><span data-ttu-id="e7357-103">Přehled Azure Functions modulu Runtime</span><span class="sxs-lookup"><span data-stu-id="e7357-103">Azure Functions Runtime Overview</span></span>

<span data-ttu-id="e7357-104">Hello Azure Functions Runtime nabízí nový způsob je tootake výhod hello jednoduchost a flexibilitu hello Azure Functions programovací model na místě.</span><span class="sxs-lookup"><span data-stu-id="e7357-104">hello Azure Functions Runtime provides a new way for you tootake advantage of hello simplicity and flexibility of hello Azure Functions programming model on-premises.</span></span> <span data-ttu-id="e7357-105">Založený na hello stejné otevřete zdroj kořeny jako Azure Functions, Azure Functions Runtime je téměř shodná vývojovém prostředí jako cloudová služba hello tooprovide nasadit místně.</span><span class="sxs-lookup"><span data-stu-id="e7357-105">Built on hello same open source roots as Azure Functions, Azure Functions Runtime is deployed on-premises tooprovide a nearly identical development experience as hello cloud service.</span></span>

![Modul Runtime Azure Functions na portálu Preview][1]

<span data-ttu-id="e7357-107">Hello Azure Functions Runtime poskytuje způsob pro vás tooexperience Azure Functions před potvrzením toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="e7357-107">hello Azure Functions Runtime provides a way for you tooexperience Azure Functions before committing toohello cloud.</span></span> <span data-ttu-id="e7357-108">Tímto způsobem můžete hello kód prostředky, které vytvoříte potom provedou toohello cloudu při migraci.</span><span class="sxs-lookup"><span data-stu-id="e7357-108">In this way, hello code assets you build can then be taken with you toohello cloud when you migrate.</span></span>  <span data-ttu-id="e7357-109">modul runtime Hello také otevře nové možnosti pro vás, například pomocí hello k výměně za chodu výpočetní výkon vaší místní počítače toorun batch procesů přes noc.</span><span class="sxs-lookup"><span data-stu-id="e7357-109">hello runtime also opens up new options for you, such as using hello spare compute power of your on-premises computers toorun batch processes overnight.</span></span> <span data-ttu-id="e7357-110">Zařízení můžete použít také v rámci vaší organizace tooconditionally odesílání dat tooother systémů, jak místně a v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="e7357-110">You can also use devices within your organization tooconditionally send data tooother systems, both on-premises and in hello cloud.</span></span>

<span data-ttu-id="e7357-111">Hello Azure Functions Runtime se skládá ze dvou částí:</span><span class="sxs-lookup"><span data-stu-id="e7357-111">hello Azure Functions Runtime consists of two pieces:</span></span>
* <span data-ttu-id="e7357-112">Role správy Runtime Azure Functions</span><span class="sxs-lookup"><span data-stu-id="e7357-112">Azure Functions Runtime Management Role</span></span>
* <span data-ttu-id="e7357-113">Role pracovního procesu modulu Runtime Azure Functions</span><span class="sxs-lookup"><span data-stu-id="e7357-113">Azure Functions Runtime Worker Role</span></span>

## <a name="azure-functions-management-role"></a><span data-ttu-id="e7357-114">Role správy Azure Functions</span><span class="sxs-lookup"><span data-stu-id="e7357-114">Azure Functions Management Role</span></span>

<span data-ttu-id="e7357-115">Hello Role správy funkce Azure poskytuje pro hello správu funkce místní hostitel.</span><span class="sxs-lookup"><span data-stu-id="e7357-115">hello Azure Functions Management Role provides a host for hello management of your Functions on-premises.</span></span> <span data-ttu-id="e7357-116">Tato role provádí hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="e7357-116">This role performs hello following tasks:</span></span>

* <span data-ttu-id="e7357-117">Hostování hello portálu pro správu funkce Azure, což je hello hello stejný jako ten, můžete vidět v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e7357-117">Hosting of hello Azure Functions Management Portal, which is hello hello same one you see in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="e7357-118">Díky tomu se budete vyvíjet funkcí v hello stejný způsobem jako v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e7357-118">This lets you develop your functions in hello same way as you would in hello Azure portal.</span></span>
* <span data-ttu-id="e7357-119">Distribuci funkcí mezi více pracovníků funkce.</span><span class="sxs-lookup"><span data-stu-id="e7357-119">Distributing functions across multiple Functions workers.</span></span>
* <span data-ttu-id="e7357-120">Poskytování publikování koncový bod, aby mohly publikovat vaše funkce přímo ze sady Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e7357-120">Providing a publishing endpoint so that you can publish your functions direct from Microsoft Visual Studio.</span></span>

## <a name="azure-functions-worker-role"></a><span data-ttu-id="e7357-121">Role pracovního procesu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="e7357-121">Azure Functions Worker Role</span></span>

<span data-ttu-id="e7357-122">Role pracovního procesu funkce Azure Hello nasazených v kontejnerech Windows a toto je, kde se provádí funkce kódu.</span><span class="sxs-lookup"><span data-stu-id="e7357-122">hello Azure Functions Worker Roles are deployed in Windows Containers and this is where your function code executes.</span></span>  <span data-ttu-id="e7357-123">Můžete nasadit více rolí pracovního procesu ve vaší organizaci a je klíče způsob, ve kterém zákazníci měli používat k výměně za chodu výpočetním výkonu.</span><span class="sxs-lookup"><span data-stu-id="e7357-123">You can deploy multiple Worker Roles throughout your organization and is a key way in which customers can make use of spare compute power.</span></span>

## <a name="minimum-requirements"></a><span data-ttu-id="e7357-124">Minimální požadavky</span><span class="sxs-lookup"><span data-stu-id="e7357-124">Minimum Requirements</span></span>

<span data-ttu-id="e7357-125">tooget začít s hello Runtime Azure Functions musí mít počítač s **systému Windows Server 2016 nebo Windows 10 Creators Update** s přístup tooa **systému SQL Server** instance.</span><span class="sxs-lookup"><span data-stu-id="e7357-125">tooget started with hello Azure Functions Runtime you must have a machine with **Windows Server 2016 or Windows 10 Creators Update** with access tooa **SQL Server** instance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7357-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7357-126">Next Steps</span></span>

<span data-ttu-id="e7357-127">Nainstalujte hello [Runtime funkce Azure preview](https://aka.ms/azafr)</span><span class="sxs-lookup"><span data-stu-id="e7357-127">Install hello [Azure Functions Runtime preview](https://aka.ms/azafr)</span></span>

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
