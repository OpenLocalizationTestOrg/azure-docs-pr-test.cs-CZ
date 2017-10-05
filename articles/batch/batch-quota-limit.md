---
title: "Služba kvóty a omezení pro Azure Batch | Microsoft Docs"
description: "Další informace o výchozích Azure Batch kvót, omezení a omezení a zvyšuje jak požádat o kvótu"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3f69ed8d3a985afe07e648e7512a88b25278ced
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="d3918-103">Kvóty a omezení služby Batch</span><span class="sxs-lookup"><span data-stu-id="d3918-103">Batch service quotas and limits</span></span>

<span data-ttu-id="d3918-104">Jako s jinými službami Azure neexistují omezení na některé prostředky spojené s touto službou Batch.</span><span class="sxs-lookup"><span data-stu-id="d3918-104">As with other Azure services, there are limits on certain resources associated with the Batch service.</span></span> <span data-ttu-id="d3918-105">Mnoho z těchto omezení je výchozí kvóty používané službou Azure na úrovni účet nebo předplatné.</span><span class="sxs-lookup"><span data-stu-id="d3918-105">Many of these limits are default quotas applied by Azure at the subscription or account level.</span></span> <span data-ttu-id="d3918-106">Tento článek popisuje tyto výchozí hodnoty a jak můžete vyžádat kvóty zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="d3918-106">This article discusses those defaults, and how you can request quota increases.</span></span>

<span data-ttu-id="d3918-107">Pamatujte na tyto kvóty při navrhování a škálování zatížení vašich úloh Batch.</span><span class="sxs-lookup"><span data-stu-id="d3918-107">Keep these quotas in mind as you are designing and scaling up your Batch workloads.</span></span> <span data-ttu-id="d3918-108">Například pokud váš fond není dosažení cílovým počtem výpočetních uzlů, které jste určili, může bylo dosaženo maximální kvóta základní pro vašeho účtu Batch nebo regionální kvóta jader virtuálních počítačů pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="d3918-108">For example, if your pool isn't reaching the target number of compute nodes you've specified, you might have reached the core quota limit for your Batch account, or a regional VM cores quota for your subscription.</span></span>

<span data-ttu-id="d3918-109">Můžete spustit několik dávkových úloh služby Batch v jednom účtu Batch najednou, nebo můžete úlohy rozložit mezi více účtů Batch, které jsou v jednom předplatném, ale v různých oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="d3918-109">You can run multiple Batch workloads in a single Batch account, or distribute your workloads among Batch accounts that are in the same subscription, but in different Azure regions.</span></span>

<span data-ttu-id="d3918-110">Pokud plánujete spouštět úlohy v produkčním prostředí ve službě Batch, musíte zvýšit jeden nebo více kvót nad výchozí.</span><span class="sxs-lookup"><span data-stu-id="d3918-110">If you plan to run production workloads in Batch, you may need to increase one or more of the quotas above the default.</span></span> <span data-ttu-id="d3918-111">Pokud chcete kvótu zvýšit, můžete otevřít online [zákazníka žádost o podporu](#increase-a-quota) zdarma.</span><span class="sxs-lookup"><span data-stu-id="d3918-111">If you want to raise a quota, you can open an online [customer support request](#increase-a-quota) at no charge.</span></span>

> [!NOTE]
> <span data-ttu-id="d3918-112">Kvótu je limit platební, není záruku kapacity.</span><span class="sxs-lookup"><span data-stu-id="d3918-112">A quota is a credit limit, not a capacity guarantee.</span></span> <span data-ttu-id="d3918-113">Pokud máte požadavků na kapacitu ve velkém měřítku, obraťte se na podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="d3918-113">If you have large-scale capacity needs, please contact Azure support.</span></span>
> 
> 

## <a name="resource-quotas"></a><span data-ttu-id="d3918-114">Kvóty prostředků</span><span class="sxs-lookup"><span data-stu-id="d3918-114">Resource quotas</span></span>
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a><span data-ttu-id="d3918-115">Kvóty v uživatelském režimu předplatného</span><span class="sxs-lookup"><span data-stu-id="d3918-115">Quotas in user subscription mode</span></span>

<span data-ttu-id="d3918-116">Pro účet Batch se režim přidělení fondu nastavený na **uživatele předplatné**, dávky virtuální počítače a další prostředky, například účty úložiště, jsou vytvořené přímo v rámci vašeho předplatného, když je vytvořen fond.</span><span class="sxs-lookup"><span data-stu-id="d3918-116">For a Batch account with pool allocation mode set to **user subscription**, Batch VMs and other resources, such as storage accounts, are created directly in your subscription when a pool is created.</span></span> <span data-ttu-id="d3918-117">Kvóta jader Azure Batch se nevztahuje na účet je vytvořen v tomto režimu.</span><span class="sxs-lookup"><span data-stu-id="d3918-117">The Azure Batch cores quota does not apply to an account created in this mode.</span></span> <span data-ttu-id="d3918-118">Místo toho kvóty pro vaše předplatné pro místní výpočetní jader a dalším prostředkům se použijí.</span><span class="sxs-lookup"><span data-stu-id="d3918-118">Instead, the quotas in your subscription for regional compute cores and other resources are applied.</span></span> <span data-ttu-id="d3918-119">Další informace o těchto kvót v [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="d3918-119">Learn more about these quotas in [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="d3918-120">Při plánování využití prostředků pro účet je vytvořen v uživatelském režimu předplatné, Všimněte si, že v následujících zdrojích informací Batch (kromě výpočetní jádra) jsou vyžadovány pro každých 40 virtuální počítače s Linuxem nebo 20 virtuálních počítačů Windows:</span><span class="sxs-lookup"><span data-stu-id="d3918-120">When planning resource usage for an account created in user subscription mode, note the following Batch resources (in addition to compute cores) are required for every 40 Linux VMs, or 20 Windows VMs:</span></span>

| <span data-ttu-id="d3918-121">Prostředek</span><span class="sxs-lookup"><span data-stu-id="d3918-121">Resource</span></span> | <span data-ttu-id="d3918-122">Kvóta</span><span class="sxs-lookup"><span data-stu-id="d3918-122">Quota</span></span> | <span data-ttu-id="d3918-123">Poskytovatel</span><span class="sxs-lookup"><span data-stu-id="d3918-123">Provider</span></span> |
| --- | ---| --- |
| <span data-ttu-id="d3918-124">Jeden účet úložiště</span><span class="sxs-lookup"><span data-stu-id="d3918-124">One storage account</span></span> | <span data-ttu-id="d3918-125">Účty úložiště</span><span class="sxs-lookup"><span data-stu-id="d3918-125">Storage Accounts</span></span> | <span data-ttu-id="d3918-126">Microsoft.Storage</span><span class="sxs-lookup"><span data-stu-id="d3918-126">Microsoft.Storage</span></span> |
| <span data-ttu-id="d3918-127">Jedna veřejná IP adresa</span><span class="sxs-lookup"><span data-stu-id="d3918-127">One public IP address</span></span> | <span data-ttu-id="d3918-128">Veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="d3918-128">Public IP Addresses</span></span> | <span data-ttu-id="d3918-129">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="d3918-129">Microsoft.Network</span></span> | 
| <span data-ttu-id="d3918-130">Jedna virtuální síť</span><span class="sxs-lookup"><span data-stu-id="d3918-130">One virtual network</span></span> | <span data-ttu-id="d3918-131">Virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="d3918-131">Virtual Networks</span></span> | <span data-ttu-id="d3918-132">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="d3918-132">Microsoft.Network</span></span> | 
| <span data-ttu-id="d3918-133">Jedna síť skupiny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="d3918-133">One network security group</span></span> | <span data-ttu-id="d3918-134">Network Security Groups (Skupiny zabezpečení sítě)</span><span class="sxs-lookup"><span data-stu-id="d3918-134">Network Security Groups</span></span> | <span data-ttu-id="d3918-135">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="d3918-135">Microsoft.Network</span></span> | 
| <span data-ttu-id="d3918-136">Jeden škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d3918-136">One virtual machine scale set</span></span> | <span data-ttu-id="d3918-137">Škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d3918-137">Virtual Machine Scale Sets</span></span> | <span data-ttu-id="d3918-138">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="d3918-138">Microsoft.Compute</span></span> | 
| <span data-ttu-id="d3918-139">Jedna služba Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="d3918-139">One load balancer</span></span> | <span data-ttu-id="d3918-140">Nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="d3918-140">Load Balancers</span></span> | <span data-ttu-id="d3918-141">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="d3918-141">Microsoft.Network</span></span> | 

<span data-ttu-id="d3918-142">Kvóta jader na místní úrovni nebo na virtuální počítač rodiny měli nastavit podle velikosti virtuálního počítače, které jsou vyžadovány pro fondu Batch nebo fondy:</span><span class="sxs-lookup"><span data-stu-id="d3918-142">The cores quota at a regional level or per VM family should be set according to the VM size required for your Batch pool or pools:</span></span>

| <span data-ttu-id="d3918-143">Kvóta</span><span class="sxs-lookup"><span data-stu-id="d3918-143">Quota</span></span> | <span data-ttu-id="d3918-144">Poskytovatel</span><span class="sxs-lookup"><span data-stu-id="d3918-144">Provider</span></span> |
| --- | ---- |
| <span data-ttu-id="d3918-145">Celkový počet jader místní</span><span class="sxs-lookup"><span data-stu-id="d3918-145">Total Regional Cores</span></span> | <span data-ttu-id="d3918-146">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="d3918-146">Microsoft.Compute</span></span> |
| <span data-ttu-id="d3918-147">…</span><span class="sxs-lookup"><span data-stu-id="d3918-147">…</span></span> <span data-ttu-id="d3918-148">Rodiny jader</span><span class="sxs-lookup"><span data-stu-id="d3918-148">Family Cores</span></span> | <span data-ttu-id="d3918-149">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="d3918-149">Microsoft.Compute</span></span> |



## <a name="other-limits"></a><span data-ttu-id="d3918-150">Další omezení</span><span class="sxs-lookup"><span data-stu-id="d3918-150">Other limits</span></span>
| <span data-ttu-id="d3918-151">**Prostředek**</span><span class="sxs-lookup"><span data-stu-id="d3918-151">**Resource**</span></span> | <span data-ttu-id="d3918-152">**Maximální omezení**</span><span class="sxs-lookup"><span data-stu-id="d3918-152">**Maximum Limit**</span></span> |
| --- | --- |
| <span data-ttu-id="d3918-153">[Souběžné úlohy](batch-parallel-node-tasks.md) na výpočetním uzlu</span><span class="sxs-lookup"><span data-stu-id="d3918-153">[Concurrent tasks](batch-parallel-node-tasks.md) per compute node</span></span> |<span data-ttu-id="d3918-154">4 x počet jader na uzel</span><span class="sxs-lookup"><span data-stu-id="d3918-154">4 x number of node cores</span></span> |
| <span data-ttu-id="d3918-155">[Aplikace](batch-application-packages.md) na účtu Batch</span><span class="sxs-lookup"><span data-stu-id="d3918-155">[Applications](batch-application-packages.md) per Batch account</span></span> |<span data-ttu-id="d3918-156">20</span><span class="sxs-lookup"><span data-stu-id="d3918-156">20</span></span> |
| <span data-ttu-id="d3918-157">Balíčky aplikací na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3918-157">Application packages per application</span></span> |<span data-ttu-id="d3918-158">40</span><span class="sxs-lookup"><span data-stu-id="d3918-158">40</span></span> |
| <span data-ttu-id="d3918-159">Velikost balíčku aplikace, (všechny)</span><span class="sxs-lookup"><span data-stu-id="d3918-159">Application package size (each)</span></span> |<span data-ttu-id="d3918-160">Poli 195GB<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="d3918-160">Approx. 195GB<sup>1</sup></span></span> |
| <span data-ttu-id="d3918-161">Velikost maximální spuštění úloh</span><span class="sxs-lookup"><span data-stu-id="d3918-161">Maximum start task size</span></span> | <span data-ttu-id="d3918-162">32768 znaků<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="d3918-162">32768 characters<sup>2</sup></span></span> |

<span data-ttu-id="d3918-163"><sup>1</sup> azure Storage limit pro velikost objektu blob maximální bloku</span><span class="sxs-lookup"><span data-stu-id="d3918-163"><sup>1</sup> Azure Storage limit for maximum block blob size</span></span><br /><span data-ttu-id="d3918-164">
<sup>2</sup> zahrnuje soubory prostředků a proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="d3918-164">
<sup>2</sup> Includes resource files and environment variables</span></span>

## <a name="view-batch-quotas"></a><span data-ttu-id="d3918-165">Zobrazení dávky kvóty</span><span class="sxs-lookup"><span data-stu-id="d3918-165">View Batch quotas</span></span>
<span data-ttu-id="d3918-166">Zobrazení vaší kvóty účtu Batch [portál Azure][portal].</span><span class="sxs-lookup"><span data-stu-id="d3918-166">View your Batch account quotas in the [Azure portal][portal].</span></span>

1. <span data-ttu-id="d3918-167">Vyberte **účty Batch** na portálu, pak vyberte účet Batch, co vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="d3918-167">Select **Batch accounts** in the portal, then select the Batch account you're interested in.</span></span>
2. <span data-ttu-id="d3918-168">Vyberte **vlastnosti** v okně účtu Batch nabídky.</span><span class="sxs-lookup"><span data-stu-id="d3918-168">Select **Properties** on the Batch account's menu blade.</span></span>
3. <span data-ttu-id="d3918-169">Zobrazí v okně Vlastnosti **kvóty** aktuálně použité k účtu Batch</span><span class="sxs-lookup"><span data-stu-id="d3918-169">The Properties blade displays the **quotas** currently applied to the Batch account</span></span>
   
    ![Kvóty účtu batch][account_quotas]

<span data-ttu-id="d3918-171">Pro účet Batch vytvořit v uživatelském režimu předplatné můžete zobrazte kvóty související předplatné na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d3918-171">For a Batch account created in user subscription mode, view the related subscription quotas in the Azure Portal.</span></span>

1. <span data-ttu-id="d3918-172">Vyberte **odběry**a vyberte odběr, který používáte pro účet Batch.</span><span class="sxs-lookup"><span data-stu-id="d3918-172">Select **Subscriptions**, and select the subscription you are using for the Batch account.</span></span>

2. <span data-ttu-id="d3918-173">Na **předplatné** vyberte **využití + kvóty**.</span><span class="sxs-lookup"><span data-stu-id="d3918-173">On the **Subscription** blade, select **Usage + quotas**.</span></span>



## <a name="increase-a-quota"></a><span data-ttu-id="d3918-174">Navýšení kvóty</span><span class="sxs-lookup"><span data-stu-id="d3918-174">Increase a quota</span></span>
<span data-ttu-id="d3918-175">Postupujte podle těchto kroků k vyžádání kvótu zvýšit vašeho účtu Batch nebo předplatnému pomocí [portál Azure][portal].</span><span class="sxs-lookup"><span data-stu-id="d3918-175">Follow these steps to request a quota increase for your Batch account or your subscription using the [Azure portal][portal].</span></span> <span data-ttu-id="d3918-176">Typ zvýšení kvóty závisí na režimu přidělení fondu vašeho účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="d3918-176">The type of quota increase depends on the pool allocation mode of your Batch account.</span></span>

### <a name="increase-a-batch-cores-quota"></a><span data-ttu-id="d3918-177">Navýšení kvóty jader Batch</span><span class="sxs-lookup"><span data-stu-id="d3918-177">Increase a Batch cores quota</span></span> 

<span data-ttu-id="d3918-178">Pokud je váš účet Batch vytvořený v **služba Batch** režim, postupujte podle těchto kroků k vyžádání Batch jader kvótu zvýšit:</span><span class="sxs-lookup"><span data-stu-id="d3918-178">If your Batch account was created in **Batch service** mode, follow these steps to request a Batch cores quota increase:</span></span>

1. <span data-ttu-id="d3918-179">Vyberte **Nápověda a podpora** dlaždici řídicího panelu portálu nebo otazník (**?**) v pravém horním rohu portálu.</span><span class="sxs-lookup"><span data-stu-id="d3918-179">Select the **Help + support** tile on your portal dashboard, or the question mark (**?**) in the upper-right corner of the portal.</span></span>
2. <span data-ttu-id="d3918-180">Vyberte **nová žádost o podporu** > **Základy**.</span><span class="sxs-lookup"><span data-stu-id="d3918-180">Select **New support request** > **Basics**.</span></span>
3. <span data-ttu-id="d3918-181">Na **Základy** okno:</span><span class="sxs-lookup"><span data-stu-id="d3918-181">On the **Basics** blade:</span></span>
   
    <span data-ttu-id="d3918-182">a.</span><span class="sxs-lookup"><span data-stu-id="d3918-182">a.</span></span> <span data-ttu-id="d3918-183">**Vydávání typu** > **kvóty**</span><span class="sxs-lookup"><span data-stu-id="d3918-183">**Issue Type** > **Quota**</span></span>
   
    <span data-ttu-id="d3918-184">b.</span><span class="sxs-lookup"><span data-stu-id="d3918-184">b.</span></span> <span data-ttu-id="d3918-185">Vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="d3918-185">Select your subscription.</span></span>
   
    <span data-ttu-id="d3918-186">c.</span><span class="sxs-lookup"><span data-stu-id="d3918-186">c.</span></span> <span data-ttu-id="d3918-187">**Typ kvóty** > **Batch**</span><span class="sxs-lookup"><span data-stu-id="d3918-187">**Quota type** > **Batch**</span></span>
   
    <span data-ttu-id="d3918-188">d.</span><span class="sxs-lookup"><span data-stu-id="d3918-188">d.</span></span> <span data-ttu-id="d3918-189">**Podporují plán** > **kvóty podporu - zahrnuté**</span><span class="sxs-lookup"><span data-stu-id="d3918-189">**Support plan** > **Quota support - Included**</span></span>
   
    <span data-ttu-id="d3918-190">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3918-190">Click **Next**.</span></span>
4. <span data-ttu-id="d3918-191">Na **problém** okno:</span><span class="sxs-lookup"><span data-stu-id="d3918-191">On the **Problem** blade:</span></span>
   
    <span data-ttu-id="d3918-192">a.</span><span class="sxs-lookup"><span data-stu-id="d3918-192">a.</span></span> <span data-ttu-id="d3918-193">Vyberte **závažnost** podle vaší [dopad na chod firmy][support_sev].</span><span class="sxs-lookup"><span data-stu-id="d3918-193">Select a **Severity** according to your [business impact][support_sev].</span></span>
   
    <span data-ttu-id="d3918-194">b.</span><span class="sxs-lookup"><span data-stu-id="d3918-194">b.</span></span> <span data-ttu-id="d3918-195">V **podrobnosti**, zadejte každé kvóty, které chcete změnit, název účtu Batch a nový limit.</span><span class="sxs-lookup"><span data-stu-id="d3918-195">In **Details**, specify each quota you want to change, the Batch account name, and the new limit.</span></span>
   
    <span data-ttu-id="d3918-196">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d3918-196">Click **Next**.</span></span>
5. <span data-ttu-id="d3918-197">Na **kontaktní informace** okno:</span><span class="sxs-lookup"><span data-stu-id="d3918-197">On the **Contact information** blade:</span></span>
   
    <span data-ttu-id="d3918-198">a.</span><span class="sxs-lookup"><span data-stu-id="d3918-198">a.</span></span> <span data-ttu-id="d3918-199">Vyberte **způsob kontaktu preferované**.</span><span class="sxs-lookup"><span data-stu-id="d3918-199">Select a **Preferred contact method**.</span></span>
   
    <span data-ttu-id="d3918-200">b.</span><span class="sxs-lookup"><span data-stu-id="d3918-200">b.</span></span> <span data-ttu-id="d3918-201">Zkontrolujte a zadejte požadované kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="d3918-201">Verify and enter the required contact details.</span></span>
   
    <span data-ttu-id="d3918-202">Kliknutím na **Vytvořit** odešlete žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="d3918-202">Click **Create** to submit the support request.</span></span>

<span data-ttu-id="d3918-203">Jakmile jste odeslali vaši žádost o podporu, bude vás kontaktovat podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="d3918-203">Once you've submitted your support request, Azure support will contact you.</span></span> <span data-ttu-id="d3918-204">Všimněte si, že dokončení požadavku může trvat až 2 pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="d3918-204">Note that completing the request can take up to 2 business days.</span></span>

### <a name="increase-a-subscription-cores-quota"></a><span data-ttu-id="d3918-205">Navýšení kvóty předplatného jader</span><span class="sxs-lookup"><span data-stu-id="d3918-205">Increase a subscription cores quota</span></span>

<span data-ttu-id="d3918-206">Pokud váš účet Batch byl vytvořen v **uživatele předplatné** režimu a potřebujete další místní nebo rodiny jader virtuálního počítače, žádost a kvótu zvýšit v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="d3918-206">If your Batch account was created in **user subscription** mode and you need additional regional or VM family cores, request a quota increase in your subscription.</span></span> <span data-ttu-id="d3918-207">Pokyny najdete v tématu [základní kvóta správce prostředků zvýšit požadavky](../azure-supportability/resource-manager-core-quotas-request.md).</span><span class="sxs-lookup"><span data-stu-id="d3918-207">For steps, see [Resource Manager core quota increase requests](../azure-supportability/resource-manager-core-quotas-request.md).</span></span>



## <a name="related-topics"></a><span data-ttu-id="d3918-208">Související témata</span><span class="sxs-lookup"><span data-stu-id="d3918-208">Related topics</span></span>
* [<span data-ttu-id="d3918-209">Vytvoření účtu Azure Batch pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d3918-209">Create an Azure Batch account using the Azure portal</span></span>](batch-account-create-portal.md)
* [<span data-ttu-id="d3918-210">Přehled funkcí Azure Batch</span><span class="sxs-lookup"><span data-stu-id="d3918-210">Azure Batch feature overview</span></span>](batch-api-basics.md)
* [<span data-ttu-id="d3918-211">Předplatné Azure a omezení služby, kvóty a omezení</span><span class="sxs-lookup"><span data-stu-id="d3918-211">Azure subscription and service limits, quotas, and constraints</span></span>](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
