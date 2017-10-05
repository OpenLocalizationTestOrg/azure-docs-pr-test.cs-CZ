---
title: "Řešení potíží s brány virtuální sítě Azure a připojení – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka vysvětluje, jak používat sledovací proces sítě Azure Poradce při potížích s rutiny prostředí PowerShell"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: e135e719d8f267f6a189d0f8a903a49980a5a035
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a><span data-ttu-id="21d34-103">Řešení potíží s brány virtuální sítě a připojení pomocí prostředí PowerShell sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="21d34-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="21d34-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="21d34-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="21d34-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="21d34-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="21d34-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="21d34-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="21d34-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="21d34-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="21d34-108">REST API</span><span class="sxs-lookup"><span data-stu-id="21d34-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="21d34-109">Sledovací proces sítě obsahuje řadu funkcí ve vztahu k pochopení síťovým prostředkům v Azure.</span><span class="sxs-lookup"><span data-stu-id="21d34-109">Network Watcher provides many capabilities as it relates to understanding your network resources in Azure.</span></span> <span data-ttu-id="21d34-110">Jeden z těchto funkcí je prostředek řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="21d34-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="21d34-111">Řešení potíží s prostředků je možné volat prostřednictvím portálu, prostředí PowerShell, rozhraní příkazového řádku nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="21d34-111">Resource troubleshooting can be called through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="21d34-112">Při volání, sledovací proces sítě kontroluje stav bránu virtuální sítě nebo připojení a vrátí nalezených výsledcích.</span><span class="sxs-lookup"><span data-stu-id="21d34-112">When called, Network Watcher inspects the health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="21d34-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="21d34-113">Before you begin</span></span>

<span data-ttu-id="21d34-114">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="21d34-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

<span data-ttu-id="21d34-115">Seznam podporovaných brány typy návštěvě [typy podporované brány](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="21d34-115">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="21d34-116">Přehled</span><span class="sxs-lookup"><span data-stu-id="21d34-116">Overview</span></span>

<span data-ttu-id="21d34-117">Řešení potíží s prostředků nabízí možnost odstraňování problémů, které nastat u brány virtuální sítě a připojení.</span><span class="sxs-lookup"><span data-stu-id="21d34-117">Resource troubleshooting provides the ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="21d34-118">Po odeslání žádosti k prostředku, řešení potíží s protokoly Probíhá dotaz a prověřovány.</span><span class="sxs-lookup"><span data-stu-id="21d34-118">When a request is made to resource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="21d34-119">Po dokončení kontroly budou vráceny výsledky.</span><span class="sxs-lookup"><span data-stu-id="21d34-119">When inspection is complete, the results are returned.</span></span> <span data-ttu-id="21d34-120">Prostředek, který řešení problémů s požadavky jsou dlouho spuštěný žádosti, což může trvat několik minut vrácení výsledku.</span><span class="sxs-lookup"><span data-stu-id="21d34-120">Resource troubleshooting requests are long running requests, which could take multiple minutes to return a result.</span></span> <span data-ttu-id="21d34-121">Protokoly z řešení potíží se ukládají v kontejneru v účtu úložiště, který je zadán.</span><span class="sxs-lookup"><span data-stu-id="21d34-121">The logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="troubleshoot-a-gateway-or-connection"></a><span data-ttu-id="21d34-122">Řešení potíží s bránu nebo připojení</span><span class="sxs-lookup"><span data-stu-id="21d34-122">Troubleshoot a gateway or connection</span></span>

1. <span data-ttu-id="21d34-123">Přejděte na [portál Azure](https://portal.azure.com) a klikněte na tlačítko **sítě** > **sledovací proces sítě** > **Diagnostika sítě VPN**</span><span class="sxs-lookup"><span data-stu-id="21d34-123">Navigate to the [Azure portal](https://portal.azure.com) and click **Networking** > **Network Watcher** > **VPN Diagnostics**</span></span>
2. <span data-ttu-id="21d34-124">Vyberte **předplatné**, **skupiny prostředků**, a **umístění**.</span><span class="sxs-lookup"><span data-stu-id="21d34-124">Select a **Subscription**, **Resource Group**, and **Location**.</span></span>
3. <span data-ttu-id="21d34-125">Řešení potíží s prostředků vrátí data o stavu prostředku.</span><span class="sxs-lookup"><span data-stu-id="21d34-125">Resource troubleshooting returns data about the health of the resource.</span></span> <span data-ttu-id="21d34-126">Navíc šetří relevantní protokoly na účet úložiště mají být zkontrolovány.</span><span class="sxs-lookup"><span data-stu-id="21d34-126">It also saves relevant logs to a storage account to be reviewed.</span></span> <span data-ttu-id="21d34-127">Klikněte na tlačítko **účet úložiště** a vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="21d34-127">Click **Storage Account** to select a storage account.</span></span>
4. <span data-ttu-id="21d34-128">Na **účty úložiště** okně vyberte stávající účet úložiště, nebo klikněte na **+ účet úložiště** vytvoříte nový.</span><span class="sxs-lookup"><span data-stu-id="21d34-128">On the **Storage accounts** blade, select an existing storage account or click **+ Storage account** to create a new one.</span></span>
5. <span data-ttu-id="21d34-129">Na **kontejnery** okně zvolte existující kontejner, nebo klikněte na tlačítko **+ kontejner** vytvořit nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="21d34-129">On the **Containers** blade, choose an existing container or click **+ Container** to create a new container.</span></span> <span data-ttu-id="21d34-130">Po dokončení klikněte na tlačítko **vyberte**</span><span class="sxs-lookup"><span data-stu-id="21d34-130">When complete click **Select**</span></span>
6. <span data-ttu-id="21d34-131">Vyberte prostředky, brány a připojení k řešení potíží a klikněte na tlačítko **spustit Poradce při potížích**</span><span class="sxs-lookup"><span data-stu-id="21d34-131">Select the Gateway and Connection resources to troubleshoot and click **Start Troubleshooting**</span></span>

<span data-ttu-id="21d34-132">Pokud je vybraných víc zdrojů, řešení potíží s běží souběžně na prostředky brány.</span><span class="sxs-lookup"><span data-stu-id="21d34-132">If multiple resources are selected, troubleshooting is run concurrently on gateway resources.</span></span> <span data-ttu-id="21d34-133">Nelze spustit na připojení a je přiřazen brány ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="21d34-133">It can not run on a connection and it's associated gateway at the same time.</span></span> <span data-ttu-id="21d34-134">Doporučuje se odstraňování potíží s bránami nejprve odstraňování problémů s připojením je delší proces.</span><span class="sxs-lookup"><span data-stu-id="21d34-134">It is recommended to troubleshoot gateways first as connection troubleshooting is a longer process.</span></span> <span data-ttu-id="21d34-135">Diagnostika sítě VPN spuštěného na prostředku **stav řešení potíží s** sloupec bude zobrazovat stav **systémem**</span><span class="sxs-lookup"><span data-stu-id="21d34-135">While VPN Diagnostics is running on a resource the **TROUBLESHOOTING STATUS** column will show a status of **Running**</span></span>

<span data-ttu-id="21d34-136">Po dokončení ve sloupci stav změní na **stavu v pořádku** nebo **není v pořádku**.</span><span class="sxs-lookup"><span data-stu-id="21d34-136">When complete, the status column changes to **Healthy** or **Unhealthy**.</span></span>

![řešení potíží s dokončení][2]

## <a name="understanding-the-results"></a><span data-ttu-id="21d34-138">Seznámení s výsledky</span><span class="sxs-lookup"><span data-stu-id="21d34-138">Understanding the results</span></span>

<span data-ttu-id="21d34-139">V **podrobnosti** části okna, **stav** kartě se zobrazují stav poslední Poradce při potížích spuštění na vybrané prostředky.</span><span class="sxs-lookup"><span data-stu-id="21d34-139">In the **Details** section of the window, the **Status** tab shows the status of the last troubleshooting run on the selected resource.</span></span> <span data-ttu-id="21d34-140">Výsledky nejnovější diagnostiky jsou uvedené xx minut po posledním spuštění.</span><span class="sxs-lookup"><span data-stu-id="21d34-140">Results of the latest diagnostic are shown xx minutes after the last run.</span></span>

|<span data-ttu-id="21d34-141">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="21d34-141">Property</span></span>  |<span data-ttu-id="21d34-142">Popis</span><span class="sxs-lookup"><span data-stu-id="21d34-142">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="21d34-143">Prostředek</span><span class="sxs-lookup"><span data-stu-id="21d34-143">Resource</span></span>     | <span data-ttu-id="21d34-144">Odkaz na prostředek.</span><span class="sxs-lookup"><span data-stu-id="21d34-144">A link to the resource.</span></span>        |
|<span data-ttu-id="21d34-145">Cestu k úložišti</span><span class="sxs-lookup"><span data-stu-id="21d34-145">Storage path</span></span>     |  <span data-ttu-id="21d34-146">Cesta k účtu úložiště a kontejneru, které obsahují protokolů (Pokud žádné byly vytvořeny během spuštění).</span><span class="sxs-lookup"><span data-stu-id="21d34-146">Path to the storage account and container that contain the logs (if any were produced during the run).</span></span> <span data-ttu-id="21d34-147">Toto nastavení není zachována po opuštění portálu.</span><span class="sxs-lookup"><span data-stu-id="21d34-147">This setting does not persist after you leave the portal.</span></span>        |
|<span data-ttu-id="21d34-148">Souhrn</span><span class="sxs-lookup"><span data-stu-id="21d34-148">Summary</span></span>     | <span data-ttu-id="21d34-149">Shrnutí stavu prostředků.</span><span class="sxs-lookup"><span data-stu-id="21d34-149">Summary of the resource health.</span></span>        |
|<span data-ttu-id="21d34-150">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="21d34-150">Detail</span></span>     | <span data-ttu-id="21d34-151">Podrobné informace o stavu prostředků.</span><span class="sxs-lookup"><span data-stu-id="21d34-151">Detailed information on the resource health.</span></span>        |
|<span data-ttu-id="21d34-152">Poslední spuštění</span><span class="sxs-lookup"><span data-stu-id="21d34-152">Last run</span></span>     | <span data-ttu-id="21d34-153">Čas poslední čas Poradce při potížích se spustil.</span><span class="sxs-lookup"><span data-stu-id="21d34-153">The time the last time troubleshooting was ran.</span></span>        |


<span data-ttu-id="21d34-154">**Akce** karta obsahuje obecné pokyny o tom, jak problém vyřešit.</span><span class="sxs-lookup"><span data-stu-id="21d34-154">The **Action** tab provides general guidance on how to resolve the issue.</span></span> <span data-ttu-id="21d34-155">Pokud může být akce pro tento problém, je k dispozici odkaz s další pokyny.</span><span class="sxs-lookup"><span data-stu-id="21d34-155">If an action can be taken for the issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="21d34-156">V případě, že tam, kde není žádná další pokyny, odpověď obsahuje adresu url pro otevření případu podpory.</span><span class="sxs-lookup"><span data-stu-id="21d34-156">In the case where there is no additional guidance, the response provides the url to open a support case.</span></span>  <span data-ttu-id="21d34-157">Další informace o vlastnostech odpovědi a co je součástí najdete v článku [řešení sledovací proces sítě – přehled](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="21d34-157">For more information about the properties of the response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>


## <a name="next-steps"></a><span data-ttu-id="21d34-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="21d34-158">Next steps</span></span>

<span data-ttu-id="21d34-159">Pokud nastavení bylo změněno tohoto připojení VPN zastavit, přečtěte si téma [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) sledovat pravidla zabezpečení sítě skupiny a zabezpečení, které může být nejistá.</span><span class="sxs-lookup"><span data-stu-id="21d34-159">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that may be in question.</span></span>


[2]: ./media/network-watcher-troubleshoot-manage-portal/2.png
