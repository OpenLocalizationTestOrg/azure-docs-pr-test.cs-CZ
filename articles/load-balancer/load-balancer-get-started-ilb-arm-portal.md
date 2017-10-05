---
title: "Vytvoření interního nástroje pro vyrovnávání zatížení – Azure Portal | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit interní nástroj pro vyrovnávání zatížení v Resource Manageru pomocí webu Azure Portal"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ac14fb9-8d14-4892-bfe6-8bc74c48ae2c
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 8fbe9d5d04d745de51e0e41516d6c12683c98637
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a><span data-ttu-id="af1b0-103">Vytvoření interního nástroje pro vyrovnávání zatížení na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="af1b0-103">Create an Internal load balancer in the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="af1b0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="af1b0-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="af1b0-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="af1b0-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="af1b0-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="af1b0-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="af1b0-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="af1b0-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="af1b0-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="af1b0-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="af1b0-109">Tento článek se věnuje modelu nasazení Resource Manager, který Microsoft doporučuje pro většinu nových nasazení namísto [klasického modelu nasazení](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="af1b0-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a><span data-ttu-id="af1b0-110">Začínáme vytvářet interní nástroj pro vyrovnávání zatížení pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="af1b0-110">Get started creating an Internal load balancer using Azure portal</span></span>

<span data-ttu-id="af1b0-111">Pomocí následujícího postupu vytvořte interní nástroj pro vyrovnávání zatížení z webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="af1b0-111">Use the following steps to create an internal load balancer from the Azure Portal.</span></span>

1. <span data-ttu-id="af1b0-112">Otevřete prohlížeč, přejděte na web [Azure Portal](http://portal.azure.com) a přihlaste se pomocí svého účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="af1b0-112">Open a browser, navigate to the [Azure portal](http://portal.azure.com), and sign in with your Azure account.</span></span>
2. <span data-ttu-id="af1b0-113">V levém horním rohu obrazovky klikněte na **Nový** > **Sítě** > **Nástroj pro vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-113">In the upper left hand side of the screen, click **New** > **Networking** > **Load balancer**.</span></span>
3. <span data-ttu-id="af1b0-114">V okně **Vytvořit nástroj pro vyrovnávání zatížení** zadejte **Název** svého nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="af1b0-114">In the **Create load balancer** blade, enter a **Name** for your load balancer.</span></span>
4. <span data-ttu-id="af1b0-115">V části **Schéma** klikněte na **Interní**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-115">Under **Scheme**, click **Internal**.</span></span>
5. <span data-ttu-id="af1b0-116">Klikněte na **Virtuální síť** a potom vyberte virtuální síť, ve které chcete vytvořit nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="af1b0-116">Click **Virtual network**, and then select the virtual network where you want to create the load balancer.</span></span>

   > [!NOTE]
   > <span data-ttu-id="af1b0-117">Pokud nevidíte virtuální síť, kterou chcete použít, zkontrolujte **Umístění**, které používáte pro nástroj pro vyrovnávání zatížení, a odpovídajícím způsobem jej změňte.</span><span class="sxs-lookup"><span data-stu-id="af1b0-117">If you do not see the virtual network you want to use, check the **Location** you are using for the load balancer, and change it accordingly.</span></span>

6. <span data-ttu-id="af1b0-118">Klikněte na **Podsíť** a potom vyberte podsíť, ve které chcete vytvořit nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="af1b0-118">Click **Subnet**, and then select the subnet where you want to create the load balancer.</span></span>
7. <span data-ttu-id="af1b0-119">V části **Přiřazení IP adresy** klikněte buď na **Dynamické**, nebo **Statické** v závislosti na tom, jestli chcete, aby IP adresa nástroje pro vyrovnávání zatížení byla pevná (statická) nebo ne.</span><span class="sxs-lookup"><span data-stu-id="af1b0-119">Under **IP address assignment**, click either **Dynamic** or **Static**, depending on whether you want the IP address for the load balancer to be fixed (static) or not.</span></span>

   > [!NOTE]
   > <span data-ttu-id="af1b0-120">Pokud vyberete použití statické IP adresy, budete muset zadat adresu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="af1b0-120">If you select to use a static IP address, you will have to provide an address for the load balancer.</span></span>

8. <span data-ttu-id="af1b0-121">V části **Skupina prostředků** buď zadejte název nové skupiny prostředků pro nástroj pro vyrovnávání zatížení, nebo klikněte na **vybrat existující** a vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="af1b0-121">Under **Resource group** either specify the name of a new resource group for the load balancer, or click **select existing** and select an existing resource group.</span></span>
9. <span data-ttu-id="af1b0-122">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-122">Click **Create**.</span></span>

## <a name="configure-load-balancing-rules"></a><span data-ttu-id="af1b0-123">Konfigurace pravidel vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="af1b0-123">Configure load balancing rules</span></span>

<span data-ttu-id="af1b0-124">Po vytvoření nástroje pro vyrovnávání zatížení přejděte do prostředku nástroje pro vyrovnávání zatížení, abyste jej nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="af1b0-124">After the load balancer creation, navigate to the load balancer resource to configure it.</span></span>
<span data-ttu-id="af1b0-125">Nejdříve je nutné nakonfigurovat back-endový fond adres a test, teprve potom je možné konfigurovat pravidlo vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="af1b0-125">You need to configure first a back-end address pool and a probe before configuring a load balancing rule.</span></span>

### <a name="step-1-configure-a-back-end-pool"></a><span data-ttu-id="af1b0-126">Krok 1: Konfigurace back-endového fondu</span><span class="sxs-lookup"><span data-stu-id="af1b0-126">Step 1: Configure a back-end pool</span></span>

1. <span data-ttu-id="af1b0-127">Na webu Azure Portal klikněte na **Procházet** > **Nástroje pro vyrovnávání zatížení** a potom klikněte na nástroj pro vyrovnávání zatížení, který jste vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="af1b0-127">In the Azure portal, click **Browse** > **Load balancers**, and then click the load balancer you created above.</span></span>
2. <span data-ttu-id="af1b0-128">V okně **Nastavení** klikněte na **Back-endové fondy**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-128">In the **Settings** blade, click **Backend pools**.</span></span>
3. <span data-ttu-id="af1b0-129">V okně **Back-endové fondy adres** klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-129">In the **Backend address pools** blade, click **Add**.</span></span>
4. <span data-ttu-id="af1b0-130">V okně **Přidat back-endový fond** zadejte **Název** back-endového fondu a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-130">In the **Add backend pool** blade, enter a **Name** for the backend pool, and then click **OK**.</span></span>

### <a name="step-2-configure-a-probe"></a><span data-ttu-id="af1b0-131">Krok 2: Konfigurace testu</span><span class="sxs-lookup"><span data-stu-id="af1b0-131">Step 2: Configure a probe</span></span>

1. <span data-ttu-id="af1b0-132">Na webu Azure Portal klikněte na **Procházet** > **Nástroje pro vyrovnávání zatížení** a potom klikněte na nástroj pro vyrovnávání zatížení, který jste vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="af1b0-132">In the Azure portal, click **Browse** > **Load balancers**, and then click the load balancer you created above.</span></span>
2. <span data-ttu-id="af1b0-133">V okně **Nastavení** klikněte na **Testy**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-133">In the **Settings** blade, click **Probes**.</span></span>
3. <span data-ttu-id="af1b0-134">V okně **Testy** klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-134">In the **Probes**  blade, click **Add**.</span></span>
4. <span data-ttu-id="af1b0-135">V okně **Přidat test** zadejte **Název** testu.</span><span class="sxs-lookup"><span data-stu-id="af1b0-135">In the **Add probe** blade, enter a **Name** for the probe.</span></span>
5. <span data-ttu-id="af1b0-136">V části **Protokol** vyberte **HTTP** (pro weby) nebo **TCP** (pro ostatní aplikace založené na protokolu TCP).</span><span class="sxs-lookup"><span data-stu-id="af1b0-136">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="af1b0-137">V části **Port** zadejte port, který se má použít při přístupu k testu.</span><span class="sxs-lookup"><span data-stu-id="af1b0-137">Under **Port**, specify the port to use when accessing the probe.</span></span>
7. <span data-ttu-id="af1b0-138">V části **Cesta** (pouze pro testy HTTP) zadejte cestu, která se má použít jako test.</span><span class="sxs-lookup"><span data-stu-id="af1b0-138">Under **Path** (for HTTP probes only), specify the path to use as a probe.</span></span>
8. <span data-ttu-id="af1b0-139">V části **Interval** zadejte, jak často se má aplikace testovat.</span><span class="sxs-lookup"><span data-stu-id="af1b0-139">Under **Interval** specify how frequently to probe the application.</span></span>
9. <span data-ttu-id="af1b0-140">V části **Prahová hodnota špatného stavu** zadejte, kolik pokusů musí selhat, než je back-endový virtuální počítač označen jako Není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="af1b0-140">Under **Unhealthy threshold**, specify how many attempts should fail before the backend virtual machine is marked as unhealthy.</span></span>
10. <span data-ttu-id="af1b0-141">Kliknutím na **OK** vytvořte test.</span><span class="sxs-lookup"><span data-stu-id="af1b0-141">Click **OK** to create probe.</span></span>

### <a name="step-3-configure-load-balancing-rules"></a><span data-ttu-id="af1b0-142">Krok 3: Konfigurace pravidel vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="af1b0-142">Step 3: Configure load balancing rules</span></span>

1. <span data-ttu-id="af1b0-143">Na webu Azure Portal klikněte na **Procházet** > **Nástroje pro vyrovnávání zatížení** a potom klikněte na nástroj pro vyrovnávání zatížení, který jste vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="af1b0-143">In the Azure portal, click **Browse** > **Load balancers**, and then click the load balancer you created above.</span></span>
2. <span data-ttu-id="af1b0-144">V okně **Nastavení** klikněte na **Pravidla vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-144">In the **Settings** blade, click **Load balancing rules**.</span></span>
3. <span data-ttu-id="af1b0-145">V okně **Pravidla vyrovnávání zatížení** klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-145">In the **Load balancing rules** blade, click **Add**.</span></span>
4. <span data-ttu-id="af1b0-146">V okně **Přidat pravidlo vyrovnávání zatížení** zadejte **Název** pravidla.</span><span class="sxs-lookup"><span data-stu-id="af1b0-146">In the **Add load balancing rule** blade, enter a **Name** for the rule.</span></span>
5. <span data-ttu-id="af1b0-147">V části **Protokol** vyberte **HTTP** (pro weby) nebo **TCP** (pro ostatní aplikace založené na protokolu TCP).</span><span class="sxs-lookup"><span data-stu-id="af1b0-147">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="af1b0-148">V části **Port** zadejte port, ke kterému se připojují klienti v nástroji pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="af1b0-148">Under **Port**, specify the port clients connect to in the load balancer.</span></span>
7. <span data-ttu-id="af1b0-149">V části **Back-endový port** zadejte port, který se má použít v back-endovém fondu (port nástroje pro vyrovnávání zatížení a back-endový port jsou obvykle stejné).</span><span class="sxs-lookup"><span data-stu-id="af1b0-149">Under **Backend port**, specify the port to be used in the backend pool (usually, the load balancer port and the backend port are the same).</span></span>
8. <span data-ttu-id="af1b0-150">V části **Back-endový fond** vyberte back-endový fond, který jste vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="af1b0-150">Under **Backend pool**, select the backend pool you created above.</span></span>
9. <span data-ttu-id="af1b0-151">V části **Trvalost relace** vyberte, jak chcete uchovávat relace.</span><span class="sxs-lookup"><span data-stu-id="af1b0-151">Under **Session persistence**, select how you want sessions to persist.</span></span>
10. <span data-ttu-id="af1b0-152">V části **Časový limit nečinnosti (v minutách)** zadejte časový limit nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="af1b0-152">Under **Idle timeout (minutes)**, specify the idle timeout.</span></span>
11. <span data-ttu-id="af1b0-153">V části **Plovoucí IP (přímá odpověď ze serveru)** klikněte na **Zakázáno** nebo **Povoleno**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-153">Under **Floating IP (direct server return)**, click **Disabled** or **Enabled**.</span></span>
12. <span data-ttu-id="af1b0-154">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="af1b0-154">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af1b0-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af1b0-155">Next steps</span></span>

[<span data-ttu-id="af1b0-156">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="af1b0-156">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="af1b0-157">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="af1b0-157">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

