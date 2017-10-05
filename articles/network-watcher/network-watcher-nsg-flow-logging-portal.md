---
title: "Správa sítě zabezpečení skupiny toku protokoly s sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak spravovat protokoly toku skupinu zabezpečení sítě v sledovací proces sítě Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 41cb5ffab9bd3a3bed75ffdb6a7383ca1690f810
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-network-security-group-flow-logs-in-the-azure-portal"></a><span data-ttu-id="2e1a9-103">Správa sítě zabezpečení skupiny toku protokoly na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2e1a9-103">Manage network security group flow logs in the Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="2e1a9-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2e1a9-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="2e1a9-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e1a9-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="2e1a9-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="2e1a9-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="2e1a9-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2e1a9-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="2e1a9-108">REST API</span><span class="sxs-lookup"><span data-stu-id="2e1a9-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="2e1a9-109">Protokoly toku skupiny zabezpečení sítě jsou funkce sledovací proces sítě, která umožňuje zobrazit informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-109">Network security group flow logs are a feature of Network Watcher that enables you to view information about ingress and egress IP traffic through a network security group.</span></span> <span data-ttu-id="2e1a9-110">Tyto protokoly toku jsou zapsané ve formátu JSON a obsahují důležité informace, včetně:</span><span class="sxs-lookup"><span data-stu-id="2e1a9-110">These flow logs are written in JSON format and provide important information, including:</span></span> 

- <span data-ttu-id="2e1a9-111">Odchozí a příchozí tok na základě pravidla.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-111">Outbound and inbound flows on a per-rule basis.</span></span>
- <span data-ttu-id="2e1a9-112">Síťové adaptéry, které toku platí pro.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-112">The NIC that the flow applies to.</span></span>
- <span data-ttu-id="2e1a9-113">5 řazené kolekce členů informace o toku (zdrojové nebo cílové adresy IP, zdrojový nebo cílový port, protokol).</span><span class="sxs-lookup"><span data-stu-id="2e1a9-113">5-tuple information about the flow (source/destination IP, source/destination port, protocol).</span></span>
- <span data-ttu-id="2e1a9-114">Informace o tom, jestli je povolené nebo zakázané přenosy.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-114">Information about whether traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2e1a9-115">Než začnete</span><span class="sxs-lookup"><span data-stu-id="2e1a9-115">Before you begin</span></span>

<span data-ttu-id="2e1a9-116">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit instanci sledovací proces sítě](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="2e1a9-116">This scenario assumes you have already followed the steps in [Create a Network Watcher instance](network-watcher-create.md).</span></span> <span data-ttu-id="2e1a9-117">Tento scénář také předpokládá, že máte skupinu prostředků s platným virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-117">The scenario also assumes that a you have a resource group with a valid virtual machine.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="2e1a9-118">Registrace zprostředkovatele statistiky</span><span class="sxs-lookup"><span data-stu-id="2e1a9-118">Register Insights provider</span></span>

<span data-ttu-id="2e1a9-119">Pro tok protokolování nemusí fungovat správně **Microsoft.Insights** zprostředkovatele musí být zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-119">For flow logging to work successfully, the **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="2e1a9-120">K registraci poskytovatele, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2e1a9-120">To register the provider, take the following steps:</span></span> 

1. <span data-ttu-id="2e1a9-121">Přejděte na **odběry**a potom vyberte předplatné, pro který chcete povolit protokoly toku.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-121">Go to **Subscriptions**, and then select the subscription for which you want to enable flow logs.</span></span> 
2. <span data-ttu-id="2e1a9-122">Na **předplatné** vyberte **zprostředkovatelé prostředků**.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-122">On the **Subscription** blade, select **Resource Providers**.</span></span> 
3. <span data-ttu-id="2e1a9-123">Podívejte se na seznam zprostředkovatelů a ověřte, zda **microsoft.insights** zprostředkovatel registroval.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-123">Look at the list of providers, and verify that the **microsoft.insights** provider is registered.</span></span> <span data-ttu-id="2e1a9-124">Pokud ne, pak vyberte **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-124">If not, then select **Register**.</span></span>

![Zprostředkovatelé zobrazení][providers]

## <a name="enable-flow-logs"></a><span data-ttu-id="2e1a9-126">Povolení toku protokolů</span><span class="sxs-lookup"><span data-stu-id="2e1a9-126">Enable flow logs</span></span>

<span data-ttu-id="2e1a9-127">Tyto kroky vás provede procesem povolování toku protokoly na skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-127">These steps take you through the process of enabling flow logs on a network security group.</span></span>

### <a name="step-1"></a><span data-ttu-id="2e1a9-128">Krok 1</span><span class="sxs-lookup"><span data-stu-id="2e1a9-128">Step 1</span></span>

<span data-ttu-id="2e1a9-129">Přejděte do instance sledovací proces sítě a potom vyberte **NSG toku protokoly**.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-129">Go to a Network Watcher instance, and then select **NSG Flow logs**.</span></span>

![Přehled protokoly toku][1]

### <a name="step-2"></a><span data-ttu-id="2e1a9-131">Krok 2</span><span class="sxs-lookup"><span data-stu-id="2e1a9-131">Step 2</span></span>

<span data-ttu-id="2e1a9-132">Ze seznamu vyberte skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-132">Select a network security group from the list.</span></span>

![Přehled protokoly toku][2]

### <a name="step-3"></a><span data-ttu-id="2e1a9-134">Krok 3</span><span class="sxs-lookup"><span data-stu-id="2e1a9-134">Step 3</span></span> 

<span data-ttu-id="2e1a9-135">Na **tok se nastavení protokolů** okno, nastaví stav na **na**a pak nakonfigurujte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-135">On the **Flow logs settings** blade, set the status to **On**, and then configure a storage account.</span></span>  <span data-ttu-id="2e1a9-136">Až budete hotoví, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-136">When you're done, select **OK**.</span></span> <span data-ttu-id="2e1a9-137">Potom vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-137">Then select **Save**.</span></span>

![Přehled protokoly toku][3]

## <a name="download-flow-logs"></a><span data-ttu-id="2e1a9-139">Stažení protokolů o toku</span><span class="sxs-lookup"><span data-stu-id="2e1a9-139">Download flow logs</span></span>

<span data-ttu-id="2e1a9-140">Tok protokoly se ukládají do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-140">Flow logs are saved in a storage account.</span></span> <span data-ttu-id="2e1a9-141">Stažení protokolů toku k jejich zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-141">Download your flow logs to view them.</span></span>

### <a name="step-1"></a><span data-ttu-id="2e1a9-142">Krok 1</span><span class="sxs-lookup"><span data-stu-id="2e1a9-142">Step 1</span></span>

<span data-ttu-id="2e1a9-143">Ke stažení protokolů toku, vyberte **toku protokoly si můžete stáhnout z účtů úložiště**.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-143">To download flow logs, select **You can download flow logs from configured storage accounts**.</span></span> <span data-ttu-id="2e1a9-144">Tento krok přejdete na zobrazení účtu úložiště kde si můžete vybrat, které protokoly ke stažení.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-144">This step takes you to a storage account view where you can choose which logs to download.</span></span>

![Nastavení protokolů toku][4]

### <a name="step-2"></a><span data-ttu-id="2e1a9-146">Krok 2</span><span class="sxs-lookup"><span data-stu-id="2e1a9-146">Step 2</span></span>

<span data-ttu-id="2e1a9-147">Přejděte k účtu úložiště správné.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-147">Go to the correct storage account.</span></span> <span data-ttu-id="2e1a9-148">Potom vyberte **kontejnery** > **insights-log-networksecuritygroupflowevent**.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-148">Then select **Containers** > **insights-log-networksecuritygroupflowevent**.</span></span>

![Nastavení protokolů toku][5]

### <a name="step-3"></a><span data-ttu-id="2e1a9-150">Krok 3</span><span class="sxs-lookup"><span data-stu-id="2e1a9-150">Step 3</span></span>

<span data-ttu-id="2e1a9-151">Přejděte do umístění protokolu toku, vyberte ji a potom vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="2e1a9-151">Go to the location of the flow log, select it, and then select **Download**.</span></span>

![Nastavení protokolů toku][6]

<span data-ttu-id="2e1a9-153">Informace o struktuře protokolu najdete v článku [přehled protokolu toku skupiny zabezpečení sítě](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2e1a9-153">For information about the structure of the log, visit [Network security group flow log overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e1a9-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e1a9-154">Next steps</span></span>

<span data-ttu-id="2e1a9-155">Zjistěte, jak [vizualizovat protokolů NSG toku pomocí PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="2e1a9-155">Learn how to [visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
