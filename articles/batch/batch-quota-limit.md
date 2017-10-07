---
title: "aaaService kvóty a omezení pro Azure Batch | Microsoft Docs"
description: "Další informace o výchozích Azure Batch kvót, omezení a omezení a jak zvyšuje toorequest kvóty"
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
ms.openlocfilehash: 6035d1c7618cfe97ebca3780e02a4ee34f54e534
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="8c326-103">Kvóty a omezení služby Batch</span><span class="sxs-lookup"><span data-stu-id="8c326-103">Batch service quotas and limits</span></span>

<span data-ttu-id="8c326-104">Jako s jinými službami Azure neexistují omezení na některé prostředky přidružené k hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="8c326-104">As with other Azure services, there are limits on certain resources associated with hello Batch service.</span></span> <span data-ttu-id="8c326-105">Mnoho z těchto omezení je výchozí kvóty používané službou Azure na úrovni účtu nebo hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="8c326-105">Many of these limits are default quotas applied by Azure at hello subscription or account level.</span></span> <span data-ttu-id="8c326-106">Tento článek popisuje tyto výchozí hodnoty a jak můžete vyžádat kvóty zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="8c326-106">This article discusses those defaults, and how you can request quota increases.</span></span>

<span data-ttu-id="8c326-107">Pamatujte na tyto kvóty při navrhování a škálování zatížení vašich úloh Batch.</span><span class="sxs-lookup"><span data-stu-id="8c326-107">Keep these quotas in mind as you are designing and scaling up your Batch workloads.</span></span> <span data-ttu-id="8c326-108">Například pokud váš fond není dosažení hello cílovým počtem výpočetních uzlů, které jste určili, může bylo dosaženo maximální kvóta základní hello vašeho účtu Batch nebo regionální kvóta jader virtuálních počítačů pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="8c326-108">For example, if your pool isn't reaching hello target number of compute nodes you've specified, you might have reached hello core quota limit for your Batch account, or a regional VM cores quota for your subscription.</span></span>

<span data-ttu-id="8c326-109">Můžete spustit několik úloh služby Batch v jednom účtu Batch nebo úlohy distribuovat mezi účty Batch, které jsou v hello stejného předplatného, ale v různých oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="8c326-109">You can run multiple Batch workloads in a single Batch account, or distribute your workloads among Batch accounts that are in hello same subscription, but in different Azure regions.</span></span>

<span data-ttu-id="8c326-110">Pokud máte v plánu úlohy v produkčním prostředí toorun v dávce, musíte tooincrease jeden nebo více hello kvóty nad výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="8c326-110">If you plan toorun production workloads in Batch, you may need tooincrease one or more of hello quotas above hello default.</span></span> <span data-ttu-id="8c326-111">Pokud chcete tooraise kvótu, můžete otevřít online [zákazníka žádost o podporu](#increase-a-quota) zdarma.</span><span class="sxs-lookup"><span data-stu-id="8c326-111">If you want tooraise a quota, you can open an online [customer support request](#increase-a-quota) at no charge.</span></span>

> [!NOTE]
> <span data-ttu-id="8c326-112">Kvótu je limit platební, není záruku kapacity.</span><span class="sxs-lookup"><span data-stu-id="8c326-112">A quota is a credit limit, not a capacity guarantee.</span></span> <span data-ttu-id="8c326-113">Pokud máte požadavků na kapacitu ve velkém měřítku, obraťte se na podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="8c326-113">If you have large-scale capacity needs, please contact Azure support.</span></span>
> 
> 

## <a name="resource-quotas"></a><span data-ttu-id="8c326-114">Kvóty prostředků</span><span class="sxs-lookup"><span data-stu-id="8c326-114">Resource quotas</span></span>
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a><span data-ttu-id="8c326-115">Kvóty v uživatelském režimu předplatného</span><span class="sxs-lookup"><span data-stu-id="8c326-115">Quotas in user subscription mode</span></span>

<span data-ttu-id="8c326-116">Pro účet Batch s režimem přidělení fondu nastavit příliš**uživatele předplatné**, dávky virtuální počítače a další prostředky, například účty úložiště, jsou vytvořené přímo v rámci vašeho předplatného, když je vytvořen fond.</span><span class="sxs-lookup"><span data-stu-id="8c326-116">For a Batch account with pool allocation mode set too**user subscription**, Batch VMs and other resources, such as storage accounts, are created directly in your subscription when a pool is created.</span></span> <span data-ttu-id="8c326-117">kvóta jader Azure Batch Hello nevztahuje tooan účet vytvořený v tomto režimu.</span><span class="sxs-lookup"><span data-stu-id="8c326-117">hello Azure Batch cores quota does not apply tooan account created in this mode.</span></span> <span data-ttu-id="8c326-118">Místo toho se použijí hello kvóty ve vašem předplatném pro místní výpočetní jader a dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="8c326-118">Instead, hello quotas in your subscription for regional compute cores and other resources are applied.</span></span> <span data-ttu-id="8c326-119">Další informace o těchto kvót v [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="8c326-119">Learn more about these quotas in [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="8c326-120">Při plánování využití prostředků pro účet je vytvořen v uživatelském režimu předplatné, jsou vyžadovány pro každých 40 virtuální počítače s Linuxem nebo 20 virtuálních počítačů Windows hello Poznámka následující prostředky Batch (v přidání toocompute jádra):</span><span class="sxs-lookup"><span data-stu-id="8c326-120">When planning resource usage for an account created in user subscription mode, note hello following Batch resources (in addition toocompute cores) are required for every 40 Linux VMs, or 20 Windows VMs:</span></span>

| <span data-ttu-id="8c326-121">Prostředek</span><span class="sxs-lookup"><span data-stu-id="8c326-121">Resource</span></span> | <span data-ttu-id="8c326-122">Kvóta</span><span class="sxs-lookup"><span data-stu-id="8c326-122">Quota</span></span> | <span data-ttu-id="8c326-123">Poskytovatel</span><span class="sxs-lookup"><span data-stu-id="8c326-123">Provider</span></span> |
| --- | ---| --- |
| <span data-ttu-id="8c326-124">Jeden účet úložiště</span><span class="sxs-lookup"><span data-stu-id="8c326-124">One storage account</span></span> | <span data-ttu-id="8c326-125">Účty úložiště</span><span class="sxs-lookup"><span data-stu-id="8c326-125">Storage Accounts</span></span> | <span data-ttu-id="8c326-126">Microsoft.Storage</span><span class="sxs-lookup"><span data-stu-id="8c326-126">Microsoft.Storage</span></span> |
| <span data-ttu-id="8c326-127">Jedna veřejná IP adresa</span><span class="sxs-lookup"><span data-stu-id="8c326-127">One public IP address</span></span> | <span data-ttu-id="8c326-128">Veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="8c326-128">Public IP Addresses</span></span> | <span data-ttu-id="8c326-129">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="8c326-129">Microsoft.Network</span></span> | 
| <span data-ttu-id="8c326-130">Jedna virtuální síť</span><span class="sxs-lookup"><span data-stu-id="8c326-130">One virtual network</span></span> | <span data-ttu-id="8c326-131">Virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="8c326-131">Virtual Networks</span></span> | <span data-ttu-id="8c326-132">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="8c326-132">Microsoft.Network</span></span> | 
| <span data-ttu-id="8c326-133">Jedna síť skupiny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="8c326-133">One network security group</span></span> | <span data-ttu-id="8c326-134">Network Security Groups (Skupiny zabezpečení sítě)</span><span class="sxs-lookup"><span data-stu-id="8c326-134">Network Security Groups</span></span> | <span data-ttu-id="8c326-135">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="8c326-135">Microsoft.Network</span></span> | 
| <span data-ttu-id="8c326-136">Jeden škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8c326-136">One virtual machine scale set</span></span> | <span data-ttu-id="8c326-137">Škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8c326-137">Virtual Machine Scale Sets</span></span> | <span data-ttu-id="8c326-138">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="8c326-138">Microsoft.Compute</span></span> | 
| <span data-ttu-id="8c326-139">Jedna služba Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="8c326-139">One load balancer</span></span> | <span data-ttu-id="8c326-140">Nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="8c326-140">Load Balancers</span></span> | <span data-ttu-id="8c326-141">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="8c326-141">Microsoft.Network</span></span> | 

<span data-ttu-id="8c326-142">Hello jader kvóta na místní úrovni nebo na virtuální počítač rodiny by měla být sada podle toohello velikost virtuálního počítače vyžaduje pro fondu Batch nebo fondy:</span><span class="sxs-lookup"><span data-stu-id="8c326-142">hello cores quota at a regional level or per VM family should be set according toohello VM size required for your Batch pool or pools:</span></span>

| <span data-ttu-id="8c326-143">Kvóta</span><span class="sxs-lookup"><span data-stu-id="8c326-143">Quota</span></span> | <span data-ttu-id="8c326-144">Poskytovatel</span><span class="sxs-lookup"><span data-stu-id="8c326-144">Provider</span></span> |
| --- | ---- |
| <span data-ttu-id="8c326-145">Celkový počet jader místní</span><span class="sxs-lookup"><span data-stu-id="8c326-145">Total Regional Cores</span></span> | <span data-ttu-id="8c326-146">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="8c326-146">Microsoft.Compute</span></span> |
| <span data-ttu-id="8c326-147">…</span><span class="sxs-lookup"><span data-stu-id="8c326-147">…</span></span> <span data-ttu-id="8c326-148">Rodiny jader</span><span class="sxs-lookup"><span data-stu-id="8c326-148">Family Cores</span></span> | <span data-ttu-id="8c326-149">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="8c326-149">Microsoft.Compute</span></span> |



## <a name="other-limits"></a><span data-ttu-id="8c326-150">Další omezení</span><span class="sxs-lookup"><span data-stu-id="8c326-150">Other limits</span></span>
| <span data-ttu-id="8c326-151">**Prostředek**</span><span class="sxs-lookup"><span data-stu-id="8c326-151">**Resource**</span></span> | <span data-ttu-id="8c326-152">**Maximální omezení**</span><span class="sxs-lookup"><span data-stu-id="8c326-152">**Maximum Limit**</span></span> |
| --- | --- |
| <span data-ttu-id="8c326-153">[Souběžné úlohy](batch-parallel-node-tasks.md) na výpočetním uzlu</span><span class="sxs-lookup"><span data-stu-id="8c326-153">[Concurrent tasks](batch-parallel-node-tasks.md) per compute node</span></span> |<span data-ttu-id="8c326-154">4 x počet jader na uzel</span><span class="sxs-lookup"><span data-stu-id="8c326-154">4 x number of node cores</span></span> |
| <span data-ttu-id="8c326-155">[Aplikace](batch-application-packages.md) na účtu Batch</span><span class="sxs-lookup"><span data-stu-id="8c326-155">[Applications](batch-application-packages.md) per Batch account</span></span> |<span data-ttu-id="8c326-156">20</span><span class="sxs-lookup"><span data-stu-id="8c326-156">20</span></span> |
| <span data-ttu-id="8c326-157">Balíčky aplikací na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8c326-157">Application packages per application</span></span> |<span data-ttu-id="8c326-158">40</span><span class="sxs-lookup"><span data-stu-id="8c326-158">40</span></span> |
| <span data-ttu-id="8c326-159">Velikost balíčku aplikace, (všechny)</span><span class="sxs-lookup"><span data-stu-id="8c326-159">Application package size (each)</span></span> |<span data-ttu-id="8c326-160">Poli 195GB<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="8c326-160">Approx. 195GB<sup>1</sup></span></span> |
| <span data-ttu-id="8c326-161">Velikost maximální spuštění úloh</span><span class="sxs-lookup"><span data-stu-id="8c326-161">Maximum start task size</span></span> | <span data-ttu-id="8c326-162">32768 znaků<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="8c326-162">32768 characters<sup>2</sup></span></span> |

<span data-ttu-id="8c326-163"><sup>1</sup> azure Storage limit pro velikost objektu blob maximální bloku</span><span class="sxs-lookup"><span data-stu-id="8c326-163"><sup>1</sup> Azure Storage limit for maximum block blob size</span></span><br /><span data-ttu-id="8c326-164">
<sup>2</sup> zahrnuje soubory prostředků a proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="8c326-164">
<sup>2</sup> Includes resource files and environment variables</span></span>

## <a name="view-batch-quotas"></a><span data-ttu-id="8c326-165">Zobrazení dávky kvóty</span><span class="sxs-lookup"><span data-stu-id="8c326-165">View Batch quotas</span></span>
<span data-ttu-id="8c326-166">Zobrazení vaší kvóty účtu Batch v hello [portál Azure][portal].</span><span class="sxs-lookup"><span data-stu-id="8c326-166">View your Batch account quotas in hello [Azure portal][portal].</span></span>

1. <span data-ttu-id="8c326-167">Vyberte **účty Batch** hello portálu, pak vyberte účet Batch hello vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="8c326-167">Select **Batch accounts** in hello portal, then select hello Batch account you're interested in.</span></span>
2. <span data-ttu-id="8c326-168">Vyberte **vlastnosti** v okně účtu Batch hello nabídky.</span><span class="sxs-lookup"><span data-stu-id="8c326-168">Select **Properties** on hello Batch account's menu blade.</span></span>
3. <span data-ttu-id="8c326-169">Zobrazí okno Vlastnosti Hello hello **kvóty** aktuálně použité toohello účtu Batch</span><span class="sxs-lookup"><span data-stu-id="8c326-169">hello Properties blade displays hello **quotas** currently applied toohello Batch account</span></span>
   
    ![Kvóty účtu batch][account_quotas]

<span data-ttu-id="8c326-171">Pro účet Batch vytvořit v uživatelském režimu předplatné hello zobrazení související s kvóty předplatného v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8c326-171">For a Batch account created in user subscription mode, view hello related subscription quotas in hello Azure Portal.</span></span>

1. <span data-ttu-id="8c326-172">Vyberte **odběry**a vyberte hello předplatné, který používáte pro hello účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="8c326-172">Select **Subscriptions**, and select hello subscription you are using for hello Batch account.</span></span>

2. <span data-ttu-id="8c326-173">Na hello **předplatné** vyberte **využití + kvóty**.</span><span class="sxs-lookup"><span data-stu-id="8c326-173">On hello **Subscription** blade, select **Usage + quotas**.</span></span>



## <a name="increase-a-quota"></a><span data-ttu-id="8c326-174">Navýšení kvóty</span><span class="sxs-lookup"><span data-stu-id="8c326-174">Increase a quota</span></span>
<span data-ttu-id="8c326-175">Postupujte podle těchto kroků toorequest zvýšit kvótu pro vašeho účtu Batch nebo předplatné pomocí hello [portál Azure][portal].</span><span class="sxs-lookup"><span data-stu-id="8c326-175">Follow these steps toorequest a quota increase for your Batch account or your subscription using hello [Azure portal][portal].</span></span> <span data-ttu-id="8c326-176">Typ Hello zvýšení kvóty závisí na režimu přidělení fondu hello účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="8c326-176">hello type of quota increase depends on hello pool allocation mode of your Batch account.</span></span>

### <a name="increase-a-batch-cores-quota"></a><span data-ttu-id="8c326-177">Navýšení kvóty jader Batch</span><span class="sxs-lookup"><span data-stu-id="8c326-177">Increase a Batch cores quota</span></span> 

<span data-ttu-id="8c326-178">Pokud je váš účet Batch vytvořený v **služba Batch** režimu, postupujte podle těchto kroků toorequest zvýšení kvóty jader Batch:</span><span class="sxs-lookup"><span data-stu-id="8c326-178">If your Batch account was created in **Batch service** mode, follow these steps toorequest a Batch cores quota increase:</span></span>

1. <span data-ttu-id="8c326-179">Vyberte hello **Nápověda a podpora** dlaždici na řídicím panelu portálu nebo hello otazník (**?**) v pravém horním rohu hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="8c326-179">Select hello **Help + support** tile on your portal dashboard, or hello question mark (**?**) in hello upper-right corner of hello portal.</span></span>
2. <span data-ttu-id="8c326-180">Vyberte **nová žádost o podporu** > **Základy**.</span><span class="sxs-lookup"><span data-stu-id="8c326-180">Select **New support request** > **Basics**.</span></span>
3. <span data-ttu-id="8c326-181">Na hello **Základy** okno:</span><span class="sxs-lookup"><span data-stu-id="8c326-181">On hello **Basics** blade:</span></span>
   
    <span data-ttu-id="8c326-182">a.</span><span class="sxs-lookup"><span data-stu-id="8c326-182">a.</span></span> <span data-ttu-id="8c326-183">**Vydávání typu** > **kvóty**</span><span class="sxs-lookup"><span data-stu-id="8c326-183">**Issue Type** > **Quota**</span></span>
   
    <span data-ttu-id="8c326-184">b.</span><span class="sxs-lookup"><span data-stu-id="8c326-184">b.</span></span> <span data-ttu-id="8c326-185">Vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="8c326-185">Select your subscription.</span></span>
   
    <span data-ttu-id="8c326-186">c.</span><span class="sxs-lookup"><span data-stu-id="8c326-186">c.</span></span> <span data-ttu-id="8c326-187">**Typ kvóty** > **Batch**</span><span class="sxs-lookup"><span data-stu-id="8c326-187">**Quota type** > **Batch**</span></span>
   
    <span data-ttu-id="8c326-188">d.</span><span class="sxs-lookup"><span data-stu-id="8c326-188">d.</span></span> <span data-ttu-id="8c326-189">**Podporují plán** > **kvóty podporu - zahrnuté**</span><span class="sxs-lookup"><span data-stu-id="8c326-189">**Support plan** > **Quota support - Included**</span></span>
   
    <span data-ttu-id="8c326-190">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="8c326-190">Click **Next**.</span></span>
4. <span data-ttu-id="8c326-191">Na hello **problém** okno:</span><span class="sxs-lookup"><span data-stu-id="8c326-191">On hello **Problem** blade:</span></span>
   
    <span data-ttu-id="8c326-192">a.</span><span class="sxs-lookup"><span data-stu-id="8c326-192">a.</span></span> <span data-ttu-id="8c326-193">Vyberte **závažnost** podle tooyour [dopad na chod firmy][support_sev].</span><span class="sxs-lookup"><span data-stu-id="8c326-193">Select a **Severity** according tooyour [business impact][support_sev].</span></span>
   
    <span data-ttu-id="8c326-194">b.</span><span class="sxs-lookup"><span data-stu-id="8c326-194">b.</span></span> <span data-ttu-id="8c326-195">V **podrobnosti**, zadejte každé kvótu chcete toochange, název účtu Batch hello a hello nový limit.</span><span class="sxs-lookup"><span data-stu-id="8c326-195">In **Details**, specify each quota you want toochange, hello Batch account name, and hello new limit.</span></span>
   
    <span data-ttu-id="8c326-196">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="8c326-196">Click **Next**.</span></span>
5. <span data-ttu-id="8c326-197">Na hello **kontaktní informace** okno:</span><span class="sxs-lookup"><span data-stu-id="8c326-197">On hello **Contact information** blade:</span></span>
   
    <span data-ttu-id="8c326-198">a.</span><span class="sxs-lookup"><span data-stu-id="8c326-198">a.</span></span> <span data-ttu-id="8c326-199">Vyberte **způsob kontaktu preferované**.</span><span class="sxs-lookup"><span data-stu-id="8c326-199">Select a **Preferred contact method**.</span></span>
   
    <span data-ttu-id="8c326-200">b.</span><span class="sxs-lookup"><span data-stu-id="8c326-200">b.</span></span> <span data-ttu-id="8c326-201">Zkontrolujte a zadejte požadované hello kontaktní údaje.</span><span class="sxs-lookup"><span data-stu-id="8c326-201">Verify and enter hello required contact details.</span></span>
   
    <span data-ttu-id="8c326-202">Klikněte na tlačítko **vytvořit** žádost o podporu toosubmit hello.</span><span class="sxs-lookup"><span data-stu-id="8c326-202">Click **Create** toosubmit hello support request.</span></span>

<span data-ttu-id="8c326-203">Jakmile jste odeslali vaši žádost o podporu, bude vás kontaktovat podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="8c326-203">Once you've submitted your support request, Azure support will contact you.</span></span> <span data-ttu-id="8c326-204">Všimněte si, že dokončení požadavku hello může trvat až too2 pracovních dnů.</span><span class="sxs-lookup"><span data-stu-id="8c326-204">Note that completing hello request can take up too2 business days.</span></span>

### <a name="increase-a-subscription-cores-quota"></a><span data-ttu-id="8c326-205">Navýšení kvóty předplatného jader</span><span class="sxs-lookup"><span data-stu-id="8c326-205">Increase a subscription cores quota</span></span>

<span data-ttu-id="8c326-206">Pokud váš účet Batch byl vytvořen v **uživatele předplatné** režimu a potřebujete další místní nebo rodiny jader virtuálního počítače, žádost a kvótu zvýšit v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="8c326-206">If your Batch account was created in **user subscription** mode and you need additional regional or VM family cores, request a quota increase in your subscription.</span></span> <span data-ttu-id="8c326-207">Pokyny najdete v tématu [základní kvóta správce prostředků zvýšit požadavky](../azure-supportability/resource-manager-core-quotas-request.md).</span><span class="sxs-lookup"><span data-stu-id="8c326-207">For steps, see [Resource Manager core quota increase requests](../azure-supportability/resource-manager-core-quotas-request.md).</span></span>



## <a name="related-topics"></a><span data-ttu-id="8c326-208">Související témata</span><span class="sxs-lookup"><span data-stu-id="8c326-208">Related topics</span></span>
* [<span data-ttu-id="8c326-209">Vytvoření účtu Azure Batch pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8c326-209">Create an Azure Batch account using hello Azure portal</span></span>](batch-account-create-portal.md)
* [<span data-ttu-id="8c326-210">Přehled funkcí Azure Batch</span><span class="sxs-lookup"><span data-stu-id="8c326-210">Azure Batch feature overview</span></span>](batch-api-basics.md)
* [<span data-ttu-id="8c326-211">Předplatné Azure a omezení služby, kvóty a omezení</span><span class="sxs-lookup"><span data-stu-id="8c326-211">Azure subscription and service limits, quotas, and constraints</span></span>](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
