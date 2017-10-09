---
title: "aaaCreate Vyrovnávání zatížení interní – portál Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate interní nástroj pro vyrovnávání ve Správci prostředků pomocí portálu Azure hello zatížení"
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
ms.openlocfilehash: 80124217a84857b542eb41cb814ec97234176dd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-in-hello-azure-portal"></a><span data-ttu-id="3e5bf-103">Vytvořit interní nástroj v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3e5bf-103">Create an Internal load balancer in hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="3e5bf-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3e5bf-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="3e5bf-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3e5bf-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="3e5bf-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3e5bf-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="3e5bf-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="3e5bf-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="3e5bf-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3e5bf-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="3e5bf-109">Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="3e5bf-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a><span data-ttu-id="3e5bf-110">Začínáme vytvářet interní nástroj pro vyrovnávání zatížení pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3e5bf-110">Get started creating an Internal load balancer using Azure portal</span></span>

<span data-ttu-id="3e5bf-111">Pomocí následujících kroků toocreate interní nástroj z portálu Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-111">Use hello following steps toocreate an internal load balancer from hello Azure Portal.</span></span>

1. <span data-ttu-id="3e5bf-112">Otevřete prohlížeč, přejít toohello [portál Azure](http://portal.azure.com)a přihlaste se pomocí účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-112">Open a browser, navigate toohello [Azure portal](http://portal.azure.com), and sign in with your Azure account.</span></span>
2. <span data-ttu-id="3e5bf-113">V levém horním straně hello úvodní obrazovka, klikněte na **nový** > **sítě** > **nástroj pro vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-113">In hello upper left hand side of hello screen, click **New** > **Networking** > **Load balancer**.</span></span>
3. <span data-ttu-id="3e5bf-114">V hello **vytvořit nástroj pro vyrovnávání zatížení** okno, zadejte **název** pro nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-114">In hello **Create load balancer** blade, enter a **Name** for your load balancer.</span></span>
4. <span data-ttu-id="3e5bf-115">V části **Schéma** klikněte na **Interní**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-115">Under **Scheme**, click **Internal**.</span></span>
5. <span data-ttu-id="3e5bf-116">Klikněte na tlačítko **virtuální síť**, a pak vyberte hello virtuální sítě, kam chcete nástroj pro vyrovnávání zatížení toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-116">Click **Virtual network**, and then select hello virtual network where you want toocreate hello load balancer.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3e5bf-117">Pokud se chcete toouse virtuální sítě hello nezobrazí, zkontrolujte hello **umístění** používají nástroje pro vyrovnávání zatížení hello a změňte odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-117">If you do not see hello virtual network you want toouse, check hello **Location** you are using for hello load balancer, and change it accordingly.</span></span>

6. <span data-ttu-id="3e5bf-118">Klikněte na tlačítko **podsíť**a potom vyberte místo, kam chcete nástroj pro vyrovnávání zatížení toocreate hello podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-118">Click **Subnet**, and then select hello subnet where you want toocreate hello load balancer.</span></span>
7. <span data-ttu-id="3e5bf-119">V části **přiřazení IP adresy**, klikněte na možnost **dynamické** nebo **statické**, v závislosti na tom, jestli chcete hello IP adresu pro hello zatížení vyrovnávání toobe fixed (statické) nebo ne.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-119">Under **IP address assignment**, click either **Dynamic** or **Static**, depending on whether you want hello IP address for hello load balancer toobe fixed (static) or not.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3e5bf-120">Pokud vyberete toouse statickou IP adresu, bude mít tooprovide adresu pro nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-120">If you select toouse a static IP address, you will have tooprovide an address for hello load balancer.</span></span>

8. <span data-ttu-id="3e5bf-121">V části **skupiny prostředků** zadejte hello název nové skupiny prostředků nástroje pro vyrovnávání zatížení hello nebo klikněte na tlačítko **vyberte existující** a vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-121">Under **Resource group** either specify hello name of a new resource group for hello load balancer, or click **select existing** and select an existing resource group.</span></span>
9. <span data-ttu-id="3e5bf-122">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-122">Click **Create**.</span></span>

## <a name="configure-load-balancing-rules"></a><span data-ttu-id="3e5bf-123">Konfigurace pravidel vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="3e5bf-123">Configure load balancing rules</span></span>

<span data-ttu-id="3e5bf-124">Po hello načíst vyrovnávání vytvoření, přejděte tooconfigure prostředků nástroje pro vyrovnávání zatížení toohello ho.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-124">After hello load balancer creation, navigate toohello load balancer resource tooconfigure it.</span></span>
<span data-ttu-id="3e5bf-125">Potřebujete tooconfigure nejprve fond back-end adresy a sondu před konfigurací pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-125">You need tooconfigure first a back-end address pool and a probe before configuring a load balancing rule.</span></span>

### <a name="step-1-configure-a-back-end-pool"></a><span data-ttu-id="3e5bf-126">Krok 1: Konfigurace back-endového fondu</span><span class="sxs-lookup"><span data-stu-id="3e5bf-126">Step 1: Configure a back-end pool</span></span>

1. <span data-ttu-id="3e5bf-127">V hello portálu Azure, klikněte na **Procházet** > **nástroje pro vyrovnávání zatížení**a potom klikněte na nástroj pro vyrovnávání zatížení hello vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-127">In hello Azure portal, click **Browse** > **Load balancers**, and then click hello load balancer you created above.</span></span>
2. <span data-ttu-id="3e5bf-128">V hello **nastavení** okně klikněte na tlačítko **back-endové fondy**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-128">In hello **Settings** blade, click **Backend pools**.</span></span>
3. <span data-ttu-id="3e5bf-129">V hello **back-endové fondy adres** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-129">In hello **Backend address pools** blade, click **Add**.</span></span>
4. <span data-ttu-id="3e5bf-130">V hello **přidejte fond back-end** okno, zadejte **název** hello back-endového fondu a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-130">In hello **Add backend pool** blade, enter a **Name** for hello backend pool, and then click **OK**.</span></span>

### <a name="step-2-configure-a-probe"></a><span data-ttu-id="3e5bf-131">Krok 2: Konfigurace testu</span><span class="sxs-lookup"><span data-stu-id="3e5bf-131">Step 2: Configure a probe</span></span>

1. <span data-ttu-id="3e5bf-132">V hello portálu Azure, klikněte na **Procházet** > **nástroje pro vyrovnávání zatížení**a potom klikněte na nástroj pro vyrovnávání zatížení hello vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-132">In hello Azure portal, click **Browse** > **Load balancers**, and then click hello load balancer you created above.</span></span>
2. <span data-ttu-id="3e5bf-133">V hello **nastavení** okně klikněte na tlačítko **sondy**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-133">In hello **Settings** blade, click **Probes**.</span></span>
3. <span data-ttu-id="3e5bf-134">V hello **sondy** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-134">In hello **Probes**  blade, click **Add**.</span></span>
4. <span data-ttu-id="3e5bf-135">V hello **přidat test** okno, zadejte **název** u testu paměti hello.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-135">In hello **Add probe** blade, enter a **Name** for hello probe.</span></span>
5. <span data-ttu-id="3e5bf-136">V části **Protokol** vyberte **HTTP** (pro weby) nebo **TCP** (pro ostatní aplikace založené na protokolu TCP).</span><span class="sxs-lookup"><span data-stu-id="3e5bf-136">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="3e5bf-137">V části **Port**, zadejte hello port toouse při přístupu k testu hello.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-137">Under **Port**, specify hello port toouse when accessing hello probe.</span></span>
7. <span data-ttu-id="3e5bf-138">V části **cesta** (pro HTTP jenom sondy), zadejte jako sondu toouse cesta hello.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-138">Under **Path** (for HTTP probes only), specify hello path toouse as a probe.</span></span>
8. <span data-ttu-id="3e5bf-139">V části **Interval** zadejte, jak často tooprobe hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-139">Under **Interval** specify how frequently tooprobe hello application.</span></span>
9. <span data-ttu-id="3e5bf-140">V části **prahová hodnota špatného stavu**, zadejte počet pokusů o má selhat, než je označena hello back-end virtuálního počítače není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-140">Under **Unhealthy threshold**, specify how many attempts should fail before hello backend virtual machine is marked as unhealthy.</span></span>
10. <span data-ttu-id="3e5bf-141">Klikněte na tlačítko **OK** toocreate testu.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-141">Click **OK** toocreate probe.</span></span>

### <a name="step-3-configure-load-balancing-rules"></a><span data-ttu-id="3e5bf-142">Krok 3: Konfigurace pravidel vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="3e5bf-142">Step 3: Configure load balancing rules</span></span>

1. <span data-ttu-id="3e5bf-143">V hello portálu Azure, klikněte na **Procházet** > **nástroje pro vyrovnávání zatížení**a potom klikněte na nástroj pro vyrovnávání zatížení hello vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-143">In hello Azure portal, click **Browse** > **Load balancers**, and then click hello load balancer you created above.</span></span>
2. <span data-ttu-id="3e5bf-144">V hello **nastavení** okně klikněte na tlačítko **pravidla Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-144">In hello **Settings** blade, click **Load balancing rules**.</span></span>
3. <span data-ttu-id="3e5bf-145">V hello **pravidla Vyrovnávání zatížení** okně klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-145">In hello **Load balancing rules** blade, click **Add**.</span></span>
4. <span data-ttu-id="3e5bf-146">V hello **pravidlo Vyrovnávání zatížení přidat** okno, zadejte **název** pro pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-146">In hello **Add load balancing rule** blade, enter a **Name** for hello rule.</span></span>
5. <span data-ttu-id="3e5bf-147">V části **Protokol** vyberte **HTTP** (pro weby) nebo **TCP** (pro ostatní aplikace založené na protokolu TCP).</span><span class="sxs-lookup"><span data-stu-id="3e5bf-147">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="3e5bf-148">V části **Port**, zadejte hello port klienti připojit nástroj pro vyrovnávání zatížení tooin hello.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-148">Under **Port**, specify hello port clients connect tooin hello load balancer.</span></span>
7. <span data-ttu-id="3e5bf-149">V části **back-endový port**, zadejte toobe port hello používá ve fondu back-end hello (obvykle portů nástroje pro vyrovnávání zatížení hello a hello back-end jsou hello stejné).</span><span class="sxs-lookup"><span data-stu-id="3e5bf-149">Under **Backend port**, specify hello port toobe used in hello backend pool (usually, hello load balancer port and hello backend port are hello same).</span></span>
8. <span data-ttu-id="3e5bf-150">V části **fond back-end**, vyberte fond back-end hello, kterou jste vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-150">Under **Backend pool**, select hello backend pool you created above.</span></span>
9. <span data-ttu-id="3e5bf-151">V části **trvalost relace**vyberte, jak chcete toopersist relací.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-151">Under **Session persistence**, select how you want sessions toopersist.</span></span>
10. <span data-ttu-id="3e5bf-152">V části **časový limit nečinnosti (v minutách)**, zadejte časový limit nečinnosti hello.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-152">Under **Idle timeout (minutes)**, specify hello idle timeout.</span></span>
11. <span data-ttu-id="3e5bf-153">V části **Plovoucí IP (přímá odpověď ze serveru)** klikněte na **Zakázáno** nebo **Povoleno**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-153">Under **Floating IP (direct server return)**, click **Disabled** or **Enabled**.</span></span>
12. <span data-ttu-id="3e5bf-154">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3e5bf-154">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e5bf-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e5bf-155">Next steps</span></span>

[<span data-ttu-id="3e5bf-156">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="3e5bf-156">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="3e5bf-157">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="3e5bf-157">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

