---
title: "aaaTroubleshoot brány virtuální sítě Azure a připojení – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka vysvětluje, jak řešit toouse hello sledovací proces sítě Azure rutiny prostředí PowerShell"
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
ms.openlocfilehash: bd568d34091209390c57d22475559bdb99ad2c59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a><span data-ttu-id="8c63e-103">Řešení potíží s brány virtuální sítě a připojení pomocí prostředí PowerShell sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="8c63e-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="8c63e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8c63e-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="8c63e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8c63e-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="8c63e-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8c63e-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="8c63e-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8c63e-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="8c63e-108">REST API</span><span class="sxs-lookup"><span data-stu-id="8c63e-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="8c63e-109">Sledovací proces sítě obsahuje řadu funkcí, protože se týká toounderstanding síťovým prostředkům v Azure.</span><span class="sxs-lookup"><span data-stu-id="8c63e-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="8c63e-110">Jeden z těchto funkcí je prostředek řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="8c63e-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="8c63e-111">Řešení potíží s prostředků je možné volat prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API.</span><span class="sxs-lookup"><span data-stu-id="8c63e-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="8c63e-112">Při volání, sledovací proces sítě kontroluje stav hello bránu virtuální sítě nebo připojení a vrátí nalezených výsledcích.</span><span class="sxs-lookup"><span data-stu-id="8c63e-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8c63e-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="8c63e-113">Before you begin</span></span>

<span data-ttu-id="8c63e-114">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="8c63e-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="8c63e-115">Seznam podporovaných brány typy návštěvě [typy podporované brány](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="8c63e-115">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="8c63e-116">Přehled</span><span class="sxs-lookup"><span data-stu-id="8c63e-116">Overview</span></span>

<span data-ttu-id="8c63e-117">Řešení potíží s prostředků poskytuje hello možnosti řešení potíží, které nastat u brány virtuální sítě a připojení.</span><span class="sxs-lookup"><span data-stu-id="8c63e-117">Resource troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="8c63e-118">Když je žádosti tooresource řešení potíží s protokoly Probíhá dotaz a prověřovány.</span><span class="sxs-lookup"><span data-stu-id="8c63e-118">When a request is made tooresource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="8c63e-119">Po dokončení kontroly hello výsledky se vrátí.</span><span class="sxs-lookup"><span data-stu-id="8c63e-119">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="8c63e-120">Požadavky na zdroje, řešení problémů s požadavky jsou dlouho spuštěný, což může trvat několik minut tooreturn výsledku.</span><span class="sxs-lookup"><span data-stu-id="8c63e-120">Resource troubleshooting requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="8c63e-121">Hello protokoly z řešení potíží se ukládají v kontejneru v účtu úložiště, který je zadán.</span><span class="sxs-lookup"><span data-stu-id="8c63e-121">hello logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="troubleshoot-a-gateway-or-connection"></a><span data-ttu-id="8c63e-122">Řešení potíží s bránu nebo připojení</span><span class="sxs-lookup"><span data-stu-id="8c63e-122">Troubleshoot a gateway or connection</span></span>

1. <span data-ttu-id="8c63e-123">Přejděte toohello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **sítě** > **sledovací proces sítě** > **Diagnostika sítě VPN**</span><span class="sxs-lookup"><span data-stu-id="8c63e-123">Navigate toohello [Azure portal](https://portal.azure.com) and click **Networking** > **Network Watcher** > **VPN Diagnostics**</span></span>
2. <span data-ttu-id="8c63e-124">Vyberte **předplatné**, **skupiny prostředků**, a **umístění**.</span><span class="sxs-lookup"><span data-stu-id="8c63e-124">Select a **Subscription**, **Resource Group**, and **Location**.</span></span>
3. <span data-ttu-id="8c63e-125">Řešení potíží s prostředků vrátí data o stavu hello hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="8c63e-125">Resource troubleshooting returns data about hello health of hello resource.</span></span> <span data-ttu-id="8c63e-126">Ukládá také účet úložiště tooa relevantní protokoly toobe zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="8c63e-126">It also saves relevant logs tooa storage account toobe reviewed.</span></span> <span data-ttu-id="8c63e-127">Klikněte na tlačítko **účet úložiště** tooselect účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="8c63e-127">Click **Storage Account** tooselect a storage account.</span></span>
4. <span data-ttu-id="8c63e-128">Na hello **účty úložiště** okně vyberte stávající účet úložiště, nebo klikněte na **+ účet úložiště** toocreate novou.</span><span class="sxs-lookup"><span data-stu-id="8c63e-128">On hello **Storage accounts** blade, select an existing storage account or click **+ Storage account** toocreate a new one.</span></span>
5. <span data-ttu-id="8c63e-129">Na hello **kontejnery** okně zvolte existující kontejner, nebo klikněte na tlačítko **+ kontejner** toocreate nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="8c63e-129">On hello **Containers** blade, choose an existing container or click **+ Container** toocreate a new container.</span></span> <span data-ttu-id="8c63e-130">Po dokončení klikněte na tlačítko **vyberte**</span><span class="sxs-lookup"><span data-stu-id="8c63e-130">When complete click **Select**</span></span>
6. <span data-ttu-id="8c63e-131">Vyberte prostředky tootroubleshoot hello brány a připojení a klikněte na **spustit Poradce při potížích**</span><span class="sxs-lookup"><span data-stu-id="8c63e-131">Select hello Gateway and Connection resources tootroubleshoot and click **Start Troubleshooting**</span></span>

<span data-ttu-id="8c63e-132">Pokud je vybraných víc zdrojů, řešení potíží s běží souběžně na prostředky brány.</span><span class="sxs-lookup"><span data-stu-id="8c63e-132">If multiple resources are selected, troubleshooting is run concurrently on gateway resources.</span></span> <span data-ttu-id="8c63e-133">Nelze spustit na připojení a je přiřazen brány v uzlu hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="8c63e-133">It can not run on a connection and it's associated gateway at hello same time.</span></span> <span data-ttu-id="8c63e-134">Je doporučeno tootroubleshoot brány nejprve odstraňování problémů s připojením je delší proces.</span><span class="sxs-lookup"><span data-stu-id="8c63e-134">It is recommended tootroubleshoot gateways first as connection troubleshooting is a longer process.</span></span> <span data-ttu-id="8c63e-135">Diagnostika sítě VPN spuštěného na prostředek hello **stav řešení potíží s** sloupec bude zobrazovat stav **systémem**</span><span class="sxs-lookup"><span data-stu-id="8c63e-135">While VPN Diagnostics is running on a resource hello **TROUBLESHOOTING STATUS** column will show a status of **Running**</span></span>

<span data-ttu-id="8c63e-136">Po dokončení hello změny stavu sloupec příliš**stavu v pořádku** nebo **není v pořádku**.</span><span class="sxs-lookup"><span data-stu-id="8c63e-136">When complete, hello status column changes too**Healthy** or **Unhealthy**.</span></span>

![řešení potíží s dokončení][2]

## <a name="understanding-hello-results"></a><span data-ttu-id="8c63e-138">Seznámení s výsledky hello</span><span class="sxs-lookup"><span data-stu-id="8c63e-138">Understanding hello results</span></span>

<span data-ttu-id="8c63e-139">V hello **podrobnosti** části okna hello hello **stav** kartě se zobrazují stav hello hello poslední řešení potíží spuštění na hello vybrané prostředku.</span><span class="sxs-lookup"><span data-stu-id="8c63e-139">In hello **Details** section of hello window, hello **Status** tab shows hello status of hello last troubleshooting run on hello selected resource.</span></span> <span data-ttu-id="8c63e-140">Výsledky nejnovější diagnostiky hello jsou uvedené xx minut po hello posledního spuštění.</span><span class="sxs-lookup"><span data-stu-id="8c63e-140">Results of hello latest diagnostic are shown xx minutes after hello last run.</span></span>

|<span data-ttu-id="8c63e-141">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="8c63e-141">Property</span></span>  |<span data-ttu-id="8c63e-142">Popis</span><span class="sxs-lookup"><span data-stu-id="8c63e-142">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="8c63e-143">Prostředek</span><span class="sxs-lookup"><span data-stu-id="8c63e-143">Resource</span></span>     | <span data-ttu-id="8c63e-144">Prostředek toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="8c63e-144">A link toohello resource.</span></span>        |
|<span data-ttu-id="8c63e-145">Cestu k úložišti</span><span class="sxs-lookup"><span data-stu-id="8c63e-145">Storage path</span></span>     |  <span data-ttu-id="8c63e-146">Cesta toohello účtu úložiště a kontejneru obsahující hello protokolů (Pokud žádné byly vytvořeny během hello spuštění).</span><span class="sxs-lookup"><span data-stu-id="8c63e-146">Path toohello storage account and container that contain hello logs (if any were produced during hello run).</span></span> <span data-ttu-id="8c63e-147">Toto nastavení není zachována po opuštění hello portálu.</span><span class="sxs-lookup"><span data-stu-id="8c63e-147">This setting does not persist after you leave hello portal.</span></span>        |
|<span data-ttu-id="8c63e-148">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8c63e-148">Summary</span></span>     | <span data-ttu-id="8c63e-149">Shrnutí stavu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="8c63e-149">Summary of hello resource health.</span></span>        |
|<span data-ttu-id="8c63e-150">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="8c63e-150">Detail</span></span>     | <span data-ttu-id="8c63e-151">Podrobné informace o stavu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="8c63e-151">Detailed information on hello resource health.</span></span>        |
|<span data-ttu-id="8c63e-152">Poslední spuštění</span><span class="sxs-lookup"><span data-stu-id="8c63e-152">Last run</span></span>     | <span data-ttu-id="8c63e-153">Hello hello čas poslední čas řešení potíží se spustil.</span><span class="sxs-lookup"><span data-stu-id="8c63e-153">hello time hello last time troubleshooting was ran.</span></span>        |


<span data-ttu-id="8c63e-154">Hello **akce** karta obsahuje obecné pokyny k jak tooresolve hello problém.</span><span class="sxs-lookup"><span data-stu-id="8c63e-154">hello **Action** tab provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="8c63e-155">Pokud může být akce hello problému, je k dispozici odkaz s další pokyny.</span><span class="sxs-lookup"><span data-stu-id="8c63e-155">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="8c63e-156">V případě hello tam, kde není žádná další pokyny, hello odpovědi poskytuje hello url tooopen případu podpory.</span><span class="sxs-lookup"><span data-stu-id="8c63e-156">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="8c63e-157">Další informace o vlastnostech hello hello odpovědi a co je součástí najdete v článku [řešení sledovací proces sítě – přehled](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="8c63e-157">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>


## <a name="next-steps"></a><span data-ttu-id="8c63e-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c63e-158">Next steps</span></span>

<span data-ttu-id="8c63e-159">Pokud nastavení bylo změněno tohoto připojení VPN zastavit, přečtěte si téma [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které může být nejistá.</span><span class="sxs-lookup"><span data-stu-id="8c63e-159">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>


[2]: ./media/network-watcher-troubleshoot-manage-portal/2.png
