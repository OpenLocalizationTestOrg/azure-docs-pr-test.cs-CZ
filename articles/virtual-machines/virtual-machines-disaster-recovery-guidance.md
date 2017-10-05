---
title: "Scénáře zotavení po havárii pro virtuální počítače Azure | Microsoft Docs"
description: "Zjistěte, co dělat v případě, že přerušení služby Azure má dopad na virtuálních počítačích Azure."
services: virtual-machines
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 65272148-ff06-4bce-91f1-851d706d4d40
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: kmouss;aglick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fb986a41e33501ee71c93a48457ac4114e33c671
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-vms"></a><span data-ttu-id="06272-103">Co dělat v případě, že přerušení služby Azure má dopad na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="06272-103">What to do in the event that an Azure service disruption impacts Azure VMs</span></span>
<span data-ttu-id="06272-104">Ve společnosti Microsoft můžeme fungovat pevného a ujistěte se, že našich služeb jsou vždy k dispozici a když je potřebujete.</span><span class="sxs-lookup"><span data-stu-id="06272-104">At Microsoft, we work hard to make sure that our services are always available to you when you need them.</span></span> <span data-ttu-id="06272-105">Vynutí nad rámec naše řízení někdy vliv nám způsoby, které způsobit přerušení poskytování služeb neplánované.</span><span class="sxs-lookup"><span data-stu-id="06272-105">Forces beyond our control sometimes impact us in ways that cause unplanned service disruptions.</span></span>

<span data-ttu-id="06272-106">Společnost Microsoft poskytuje smlouvy úroveň služeb (SLA) pro jeho služby jako závazek provozu a připojení.</span><span class="sxs-lookup"><span data-stu-id="06272-106">Microsoft provides a Service Level Agreement (SLA) for its services as a commitment for uptime and connectivity.</span></span> <span data-ttu-id="06272-107">Smlouva SLA pro jednotlivé služby Azure najdete na [smlouvy o úrovni služeb Azure](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="06272-107">The SLA for individual Azure services can be found at [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

<span data-ttu-id="06272-108">Azure již obsahuje mnoho funkcí integrovanou platformu, které podporují aplikace s vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="06272-108">Azure already has many built-in platform features that support highly available applications.</span></span> <span data-ttu-id="06272-109">Další informace o těchto službách, přečtěte si [zotavení po havárii a vysoká dostupnost pro aplikace Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).</span><span class="sxs-lookup"><span data-stu-id="06272-109">For more about these services, read [Disaster recovery and high availability for Azure applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).</span></span>

<span data-ttu-id="06272-110">Tento článek se zabývá true zotavení po havárii, když dojde výpadku způsobeného hlavní přírodní katastrofě nebo přerušení služeb rozšířeným celou oblast.</span><span class="sxs-lookup"><span data-stu-id="06272-110">This article covers a true disaster recovery scenario, when a whole region experiences an outage due to major natural disaster or widespread service interruption.</span></span> <span data-ttu-id="06272-111">Toto jsou výjimečných výskytů, ale je nutné připravit možnost, že je k výpadku celou oblast.</span><span class="sxs-lookup"><span data-stu-id="06272-111">These are rare occurrences, but you must prepare for the possibility that there is an outage of an entire region.</span></span> <span data-ttu-id="06272-112">Když celou oblast dojde přerušení služby, místně redundantní kopie dat by být dočasně k dispozici.</span><span class="sxs-lookup"><span data-stu-id="06272-112">If an entire region experiences a service disruption, the locally redundant copies of your data would temporarily be unavailable.</span></span> <span data-ttu-id="06272-113">Pokud jste povolili geografická replikace, tři další kopie objektů BLOB služby Azure Storage a tabulek jsou uložené v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="06272-113">If you have enabled geo-replication, three additional copies of your Azure Storage blobs and tables are stored in a different region.</span></span> <span data-ttu-id="06272-114">V případě výpadku dokončení místní nebo havárii, ve kterém není použitelná pro obnovení primární oblasti znovu mapuje Azure, všechny položky DNS pro danou oblast geograficky replikované.</span><span class="sxs-lookup"><span data-stu-id="06272-114">In the event of a complete regional outage or a disaster in which the primary region is not recoverable, Azure remaps all of the DNS entries to the geo-replicated region.</span></span>

<span data-ttu-id="06272-115">Chcete-li zpracovat tyto výjimečných výskytů, poskytujeme následující pokyny pro virtuální počítače Azure v případě přerušení služby celé oblasti, kde je virtuální počítač Azure aplikaci nasadit.</span><span class="sxs-lookup"><span data-stu-id="06272-115">To help you handle these rare occurrences, we provide the following guidance for Azure virtual machines in the case of a service disruption of the entire region where your Azure virtual machine application is deployed.</span></span>

## <a name="option-1-initiate-a-failover-by-using-azure-site-recovery"></a><span data-ttu-id="06272-116">Možnost 1: Spuštění převzetí služeb při selhání pomocí Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="06272-116">Option 1: Initiate a failover by using Azure Site Recovery</span></span>
<span data-ttu-id="06272-117">Azure Site Recovery pro virtuální počítače můžete nakonfigurovat tak, aby vaše aplikace s jedním kliknutím v několika minut můžete obnovit.</span><span class="sxs-lookup"><span data-stu-id="06272-117">You can configure Azure Site Recovery for your VMs so that you can recover your application with a single click in matter of minutes.</span></span> <span data-ttu-id="06272-118">Můžete replikovat do oblasti Azure podle svého výběru a není omezen na spárované oblasti.</span><span class="sxs-lookup"><span data-stu-id="06272-118">You can replicate to Azure region of your choice and not restricted to paired regions.</span></span> <span data-ttu-id="06272-119">Abyste mohli začít podle [replikaci virtuálních počítačů](https://aka.ms/a2a-getting-started).</span><span class="sxs-lookup"><span data-stu-id="06272-119">You can get started by [replicating your virtual machines](https://aka.ms/a2a-getting-started).</span></span> <span data-ttu-id="06272-120">Můžete [vytvoření plánu obnovení](../site-recovery/site-recovery-create-recovery-plans.md) tak, aby můžete automatizovat proces celý převzetí služeb při selhání pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="06272-120">You can [create a recovery plan](../site-recovery/site-recovery-create-recovery-plans.md) so that you can automate the entire failover process for your application.</span></span> <span data-ttu-id="06272-121">Můžete [testování vaší převzetí služeb při selhání](../site-recovery/site-recovery-test-failover-to-azure.md) předem, bez dopadu na produkční aplikace nebo probíhající replikace.</span><span class="sxs-lookup"><span data-stu-id="06272-121">You can [test your failovers](../site-recovery/site-recovery-test-failover-to-azure.md) beforehand without impacting production application or the ongoing replication.</span></span> <span data-ttu-id="06272-122">V případě přerušení primární oblasti, jste právě [zahájení převzetí služeb při selhání](../site-recovery/site-recovery-failover.md) a převeďte aplikace v cílové oblasti.</span><span class="sxs-lookup"><span data-stu-id="06272-122">In the event of a primary region disruption, you just [initiate a failover](../site-recovery/site-recovery-failover.md) and bring your application in target region.</span></span>


## <a name="option-2-wait-for-recovery"></a><span data-ttu-id="06272-123">Možnost 2: Čekání pro obnovení</span><span class="sxs-lookup"><span data-stu-id="06272-123">Option 2: Wait for recovery</span></span>
<span data-ttu-id="06272-124">V takovém případě není třeba žádné akce z vaší strany.</span><span class="sxs-lookup"><span data-stu-id="06272-124">In this case, no action on your part is required.</span></span> <span data-ttu-id="06272-125">Vědět, že pracujeme usilovně obnovit dostupnost služeb.</span><span class="sxs-lookup"><span data-stu-id="06272-125">Know that we are working diligently to restore service availability.</span></span> <span data-ttu-id="06272-126">Zobrazí aktuální stav služby na našem [řídicím panelu stavu služeb Azure](https://azure.microsoft.com/status/).</span><span class="sxs-lookup"><span data-stu-id="06272-126">You can see the current service status on our [Azure Service Health Dashboard](https://azure.microsoft.com/status/).</span></span>

<span data-ttu-id="06272-127">Toto je nejlepší možnost, pokud jste nenastavili Azure Site Recovery, geograficky redundantní úložiště s přístupem pro čtení nebo geograficky redundantní úložiště před přerušení.</span><span class="sxs-lookup"><span data-stu-id="06272-127">This is the best option if you have not set up Azure Site Recovery, read-access geo-redundant storage, or geo-redundant storage prior to the disruption.</span></span> <span data-ttu-id="06272-128">Pokud jste se nastavili geograficky redundantní úložiště nebo geograficky redundantní úložiště s přístupem pro čtení pro účet úložiště, kde jsou uloženy vaše virtuální počítač virtuální pevné disky (VHD), můžete zobrazovat obnovit základní image virtuálního pevného disku a opakujte ke zřízení nového virtuálního počítače z něj.</span><span class="sxs-lookup"><span data-stu-id="06272-128">If you have set up geo-redundant storage or read-access geo-redundant storage for the storage account where your VM virtual hard drives (VHDs) are stored, you can look to recover the base image VHD and try to provision a new VM from it.</span></span> <span data-ttu-id="06272-129">Toto není upřednostňovanou možnost, protože nejsou k dispozici žádné záruky synchronizace dat.</span><span class="sxs-lookup"><span data-stu-id="06272-129">This is not a preferred option because there are no guarantees of synchronization of data.</span></span> <span data-ttu-id="06272-130">V důsledku toho tato možnost není zaručena.</span><span class="sxs-lookup"><span data-stu-id="06272-130">Consequently, this option is not guaranteed to work.</span></span>


> [!NOTE]
> <span data-ttu-id="06272-131">Uvědomte si, že nemáte žádné kontrolu nad tento proces a se vztahuje pouze na přerušení služeb celou oblast.</span><span class="sxs-lookup"><span data-stu-id="06272-131">Be aware that you do not have any control over this process, and it will only occur for region-wide service disruptions.</span></span> <span data-ttu-id="06272-132">Z tohoto důvodu musíte také spoléhat na další specifické pro aplikaci Zálohování strategie k dosažení nejvyšší úroveň dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="06272-132">Because of this, you must also rely on other application-specific backup strategies to achieve the highest level of availability.</span></span> <span data-ttu-id="06272-133">Další informace najdete v části na [Data strategie zotavení po havárii](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications#data-strategies-for-disaster-recovery).</span><span class="sxs-lookup"><span data-stu-id="06272-133">For more information, see the section on [Data strategies for disaster recovery](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications#data-strategies-for-disaster-recovery).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="06272-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="06272-134">Next steps</span></span>

- <span data-ttu-id="06272-135">Spustit [chrání vaše aplikace spuštěné na virtuálních počítačích Azure](https://aka.ms/a2a-getting-started) pomocí Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="06272-135">Start [protecting your applications running on Azure virtual machines](https://aka.ms/a2a-getting-started) using Azure Site Recovery</span></span>

- <span data-ttu-id="06272-136">Další informace o tom, jak implementovat strategie vysoké dostupnosti a zotavení po havárii, najdete v části [zotavení po havárii a vysoká dostupnost pro aplikace Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).</span><span class="sxs-lookup"><span data-stu-id="06272-136">To learn more about how to implement a disaster recovery and high availability strategy, see [Disaster recovery and high availability for Azure applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).</span></span>

- <span data-ttu-id="06272-137">K vývoji podrobné technické vysvětlení funkcí Cloudová platforma, najdete v části [technické pokyny Azure odolnosti](../resiliency/resiliency-technical-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="06272-137">To develop a detailed technical understanding of a cloud platform’s capabilities, see [Azure resiliency technical guidance](../resiliency/resiliency-technical-guidance.md).</span></span>


- <span data-ttu-id="06272-138">Pokud tyto pokyny jsou vymazat, nebo pokud chcete, aby operace vaším jménem, obraťte se na [zákaznickou podporu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="06272-138">If the instructions are not clear, or if you would like Microsoft to do the operations on your behalf, contact [Customer Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>