---
title: "aaaManage toku protokolů skupiny zabezpečení sítě s sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak toomanage skupinu zabezpečení sítě toku přihlásí sledovací proces sítě Azure"
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
ms.openlocfilehash: fb250337ab9d1a0c0d0d3569c00bc221dd102a3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-group-flow-logs-in-hello-azure-portal"></a><span data-ttu-id="e5e92-103">Správa sítě zabezpečení skupiny toku protokolů hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e5e92-103">Manage network security group flow logs in hello Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="e5e92-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e5e92-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="e5e92-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e5e92-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="e5e92-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e5e92-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="e5e92-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e5e92-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="e5e92-108">REST API</span><span class="sxs-lookup"><span data-stu-id="e5e92-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="e5e92-109">Protokoly toku skupiny zabezpečení sítě jsou funkce sledovací proces sítě, která vám umožní tooview informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="e5e92-109">Network security group flow logs are a feature of Network Watcher that enables you tooview information about ingress and egress IP traffic through a network security group.</span></span> <span data-ttu-id="e5e92-110">Tyto protokoly toku jsou zapsané ve formátu JSON a obsahují důležité informace, včetně:</span><span class="sxs-lookup"><span data-stu-id="e5e92-110">These flow logs are written in JSON format and provide important information, including:</span></span> 

- <span data-ttu-id="e5e92-111">Odchozí a příchozí tok na základě pravidla.</span><span class="sxs-lookup"><span data-stu-id="e5e92-111">Outbound and inbound flows on a per-rule basis.</span></span>
- <span data-ttu-id="e5e92-112">Hello síťovou kartu, která hello postup se týká.</span><span class="sxs-lookup"><span data-stu-id="e5e92-112">hello NIC that hello flow applies to.</span></span>
- <span data-ttu-id="e5e92-113">5 řazené kolekce členů informace o toku hello (zdrojové nebo cílové adresy IP, zdrojový nebo cílový port, protokol).</span><span class="sxs-lookup"><span data-stu-id="e5e92-113">5-tuple information about hello flow (source/destination IP, source/destination port, protocol).</span></span>
- <span data-ttu-id="e5e92-114">Informace o tom, jestli je povolené nebo zakázané přenosy.</span><span class="sxs-lookup"><span data-stu-id="e5e92-114">Information about whether traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="e5e92-115">Než začnete</span><span class="sxs-lookup"><span data-stu-id="e5e92-115">Before you begin</span></span>

<span data-ttu-id="e5e92-116">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit instanci sledovací proces sítě](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="e5e92-116">This scenario assumes you have already followed hello steps in [Create a Network Watcher instance](network-watcher-create.md).</span></span> <span data-ttu-id="e5e92-117">scénář Hello také předpokládá, že máte skupinu prostředků s platným virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="e5e92-117">hello scenario also assumes that a you have a resource group with a valid virtual machine.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="e5e92-118">Registrace zprostředkovatele statistiky</span><span class="sxs-lookup"><span data-stu-id="e5e92-118">Register Insights provider</span></span>

<span data-ttu-id="e5e92-119">Pro tok protokolování toowork úspěšně, hello **Microsoft.Insights** zprostředkovatele musí být zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="e5e92-119">For flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="e5e92-120">tooregister hello poskytovatele, hello proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e5e92-120">tooregister hello provider, take hello following steps:</span></span> 

1. <span data-ttu-id="e5e92-121">Přejděte příliš**odběry**a potom vyberte hello předplatné, pro které chcete tooenable toku protokoly.</span><span class="sxs-lookup"><span data-stu-id="e5e92-121">Go too**Subscriptions**, and then select hello subscription for which you want tooenable flow logs.</span></span> 
2. <span data-ttu-id="e5e92-122">Na hello **předplatné** vyberte **zprostředkovatelé prostředků**.</span><span class="sxs-lookup"><span data-stu-id="e5e92-122">On hello **Subscription** blade, select **Resource Providers**.</span></span> 
3. <span data-ttu-id="e5e92-123">Podívejte se na hello seznamu zprostředkovatelů a ověřte, že hello **microsoft.insights** zprostředkovatel registroval.</span><span class="sxs-lookup"><span data-stu-id="e5e92-123">Look at hello list of providers, and verify that hello **microsoft.insights** provider is registered.</span></span> <span data-ttu-id="e5e92-124">Pokud ne, pak vyberte **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="e5e92-124">If not, then select **Register**.</span></span>

![Zprostředkovatelé zobrazení][providers]

## <a name="enable-flow-logs"></a><span data-ttu-id="e5e92-126">Povolení toku protokolů</span><span class="sxs-lookup"><span data-stu-id="e5e92-126">Enable flow logs</span></span>

<span data-ttu-id="e5e92-127">Tyto kroky provést uživatele hello proces povolení toku protokoly na skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="e5e92-127">These steps take you through hello process of enabling flow logs on a network security group.</span></span>

### <a name="step-1"></a><span data-ttu-id="e5e92-128">Krok 1</span><span class="sxs-lookup"><span data-stu-id="e5e92-128">Step 1</span></span>

<span data-ttu-id="e5e92-129">Přejděte tooa sledovací proces sítě instance a potom vyberte **NSG toku protokoly**.</span><span class="sxs-lookup"><span data-stu-id="e5e92-129">Go tooa Network Watcher instance, and then select **NSG Flow logs**.</span></span>

![Přehled protokoly toku][1]

### <a name="step-2"></a><span data-ttu-id="e5e92-131">Krok 2</span><span class="sxs-lookup"><span data-stu-id="e5e92-131">Step 2</span></span>

<span data-ttu-id="e5e92-132">Hello seznamu vyberte skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="e5e92-132">Select a network security group from hello list.</span></span>

![Přehled protokoly toku][2]

### <a name="step-3"></a><span data-ttu-id="e5e92-134">Krok 3</span><span class="sxs-lookup"><span data-stu-id="e5e92-134">Step 3</span></span> 

<span data-ttu-id="e5e92-135">Na hello **tok se nastavení protokolů** okně nastavte stav hello příliš**na**a pak nakonfigurujte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e5e92-135">On hello **Flow logs settings** blade, set hello status too**On**, and then configure a storage account.</span></span>  <span data-ttu-id="e5e92-136">Až budete hotoví, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5e92-136">When you're done, select **OK**.</span></span> <span data-ttu-id="e5e92-137">Potom vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e5e92-137">Then select **Save**.</span></span>

![Přehled protokoly toku][3]

## <a name="download-flow-logs"></a><span data-ttu-id="e5e92-139">Stažení protokolů o toku</span><span class="sxs-lookup"><span data-stu-id="e5e92-139">Download flow logs</span></span>

<span data-ttu-id="e5e92-140">Tok protokoly se ukládají do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e5e92-140">Flow logs are saved in a storage account.</span></span> <span data-ttu-id="e5e92-141">Stáhnout vaše protokoly tooview tok je.</span><span class="sxs-lookup"><span data-stu-id="e5e92-141">Download your flow logs tooview them.</span></span>

### <a name="step-1"></a><span data-ttu-id="e5e92-142">Krok 1</span><span class="sxs-lookup"><span data-stu-id="e5e92-142">Step 1</span></span>

<span data-ttu-id="e5e92-143">Vyberte protokoly toodownload toku, **toku protokoly si můžete stáhnout z účtů úložiště**.</span><span class="sxs-lookup"><span data-stu-id="e5e92-143">toodownload flow logs, select **You can download flow logs from configured storage accounts**.</span></span> <span data-ttu-id="e5e92-144">Tento krok přejdete zobrazení účtu úložiště tooa kde si můžete vybrat, které toodownload protokoly.</span><span class="sxs-lookup"><span data-stu-id="e5e92-144">This step takes you tooa storage account view where you can choose which logs toodownload.</span></span>

![Nastavení protokolů toku][4]

### <a name="step-2"></a><span data-ttu-id="e5e92-146">Krok 2</span><span class="sxs-lookup"><span data-stu-id="e5e92-146">Step 2</span></span>

<span data-ttu-id="e5e92-147">Přejděte účet toohello správné úložiště.</span><span class="sxs-lookup"><span data-stu-id="e5e92-147">Go toohello correct storage account.</span></span> <span data-ttu-id="e5e92-148">Potom vyberte **kontejnery** > **insights-log-networksecuritygroupflowevent**.</span><span class="sxs-lookup"><span data-stu-id="e5e92-148">Then select **Containers** > **insights-log-networksecuritygroupflowevent**.</span></span>

![Nastavení protokolů toku][5]

### <a name="step-3"></a><span data-ttu-id="e5e92-150">Krok 3</span><span class="sxs-lookup"><span data-stu-id="e5e92-150">Step 3</span></span>

<span data-ttu-id="e5e92-151">Přejděte toohello umístění protokolu hello toku, vyberte ho a potom vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="e5e92-151">Go toohello location of hello flow log, select it, and then select **Download**.</span></span>

![Nastavení protokolů toku][6]

<span data-ttu-id="e5e92-153">Informace o struktuře hello hello protokolu najdete v článku [přehled protokolu toku skupiny zabezpečení sítě](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e5e92-153">For information about hello structure of hello log, visit [Network security group flow log overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5e92-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e5e92-154">Next steps</span></span>

<span data-ttu-id="e5e92-155">Zjistěte, jak příliš[vizualizovat protokolů NSG toku pomocí PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="e5e92-155">Learn how too[visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
