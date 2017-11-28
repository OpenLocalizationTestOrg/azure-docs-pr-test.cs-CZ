---
title: "aaaMigrate virtuální počítače z AWS tooAzure | Microsoft Docs"
description: "Tento článek popisuje, jak toomigrate virtuální počítače běží v tooAzure Amazon Web Services (AWS) pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: bsiva
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: c99b781ec9cca5b8f9a847d3fc48408062b120b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-tooazure-with-azure-site-recovery"></a><span data-ttu-id="d7563-103">Migrovat virtuální počítače v tooAzure Amazon Web Services (AWS) s Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="d7563-103">Migrate virtual machines in Amazon Web Services (AWS) tooAzure with Azure Site Recovery</span></span>

<span data-ttu-id="d7563-104">Tento článek popisuje, jak toomigrate AWS Windows instance tooAzure virtuálních počítačů s hello [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="d7563-104">This article describes how toomigrate AWS Windows instances tooAzure virtual machines with hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="d7563-105">Migrace je efektivně převzetí služeb při selhání z AWS tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d7563-105">Migration is effectively a failover from AWS tooAzure.</span></span> <span data-ttu-id="d7563-106">Nelze navrácení služeb po obnovení počítače tooAWS a neexistuje žádný probíhající replikace.</span><span class="sxs-lookup"><span data-stu-id="d7563-106">You can't failback machines tooAWS, and there's no ongoing replication.</span></span> <span data-ttu-id="d7563-107">Tento článek popisuje hello kroky pro migraci v hello portál Azure a jsou založené na hello pokyny pro [replikace fyzický počítač tooAzure](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="d7563-107">This article describes hello steps for migration in hello Azure portal, and are based on hello instructions for [replicating a physical machine tooAzure](site-recovery-vmware-to-azure.md).</span></span>

<span data-ttu-id="d7563-108">POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span><span class="sxs-lookup"><span data-stu-id="d7563-108">Post any comments or questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="d7563-109">Podporované operační systémy</span><span class="sxs-lookup"><span data-stu-id="d7563-109">Supported operating systems</span></span>

<span data-ttu-id="d7563-110">Obnovení lokality lze použít toomigrate EC2 instancí s některým z následujících operačních systémů hello:</span><span class="sxs-lookup"><span data-stu-id="d7563-110">Site Recovery can be used toomigrate EC2 instances running any of hello following operating systems:</span></span>

- <span data-ttu-id="d7563-111">Windows (pouze 64bitová verze)</span><span class="sxs-lookup"><span data-stu-id="d7563-111">Windows(64 bit only)</span></span>
    - <span data-ttu-id="d7563-112">Windows Server 2008 R2 SP1 + (Citrix PV ovladače nebo pouze ovladače AWS PV.</span><span class="sxs-lookup"><span data-stu-id="d7563-112">Windows Server 2008 R2 SP1+ (Citrix PV drivers or AWS PV drivers only.</span></span> <span data-ttu-id="d7563-113">**Nejsou podporovány instance systémem RedHat PV ovladače**) Windows serveru 2012 systému Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="d7563-113">**Instances running RedHat PV drivers are not supported**) Windows Server 2012 Windows Server 2012 R2</span></span>
- <span data-ttu-id="d7563-114">Linux (pouze 64bitová verze)</span><span class="sxs-lookup"><span data-stu-id="d7563-114">Linux (64 bit only)</span></span>
    - <span data-ttu-id="d7563-115">Red Hat Enterprise Linux 6.7 (pouze HVM virtualizované instance)</span><span class="sxs-lookup"><span data-stu-id="d7563-115">Red Hat Enterprise Linux 6.7 (HVM virtualized instances only)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7563-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d7563-116">Prerequisites</span></span>

<span data-ttu-id="d7563-117">Zde je nutné pro toto nasazení:</span><span class="sxs-lookup"><span data-stu-id="d7563-117">Here's what you need for this deployment:</span></span>

* <span data-ttu-id="d7563-118">**Konfigurační server**: virtuální počítač Amazon EC2, systém Windows Server 2012 R2 je nasazen jako hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="d7563-118">**Configuration server**: An Amazon EC2 VM running Windows Server 2012 R2 is deployed as hello configuration server.</span></span> <span data-ttu-id="d7563-119">Ve výchozím nastavení hello součásti Azure Site Recovery (procesový server a hlavní cílový server) jsou nainstalovány při nasazování hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="d7563-119">By default, hello other Azure Site Recovery components (process server and master target server) are installed when you deploy hello configuration server.</span></span> <span data-ttu-id="d7563-120">Tento článek popisuje hello kroky pro migraci v hello portál Azure a jsou založené na hello pokyny [Další informace](site-recovery-components.md)</span><span class="sxs-lookup"><span data-stu-id="d7563-120">This article describes hello steps for migration in hello Azure portal, and are based on hello instructions for  [Learn more](site-recovery-components.md)</span></span>

* <span data-ttu-id="d7563-121">**Instance EC2**: hello Amazon EC2 instancí virtuálních počítačů, které chcete toomigrate.</span><span class="sxs-lookup"><span data-stu-id="d7563-121">**EC2 instances**: hello Amazon EC2 virtual machines instances that you want toomigrate.</span></span>

## <a name="deployment-steps"></a><span data-ttu-id="d7563-122">Kroky nasazení</span><span class="sxs-lookup"><span data-stu-id="d7563-122">Deployment steps</span></span>

1. <span data-ttu-id="d7563-123">Vytvořte trezor služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="d7563-123">Create a Recovery Services vault.</span></span>
2. <span data-ttu-id="d7563-124">Hello skupiny zabezpečení vašich instancí EC2 by měl mít nakonfigurovaná pravidla tooallow komunikace mezi instance EC2 hello chcete toomigrate a hello instanci, na který budete toodeploy hello konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="d7563-124">hello Security Group of your EC2 instances should have rules configured tooallow communication between hello EC2 instance that you want toomigrate, and hello instance on which you plan toodeploy hello Configuration Server.</span></span>

3. <span data-ttu-id="d7563-125">Na hello stejné Amazon virtuální privátní Cloud jako vaše instance EC2 nasadit server konfigurace automatické obnovení systému.</span><span class="sxs-lookup"><span data-stu-id="d7563-125">On hello same Amazon Virtual Private Cloud as your EC2 instances, deploy an ASR configuration server.</span></span> <span data-ttu-id="d7563-126">Odkazovat hello VMware / fyzické tooAzure požadavky pro nasazení požadavky na konfiguraci serveru.</span><span class="sxs-lookup"><span data-stu-id="d7563-126">Refer hello VMware / Physical tooAzure prerequisites for configuration server deployment requirements.</span></span>

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  <span data-ttu-id="d7563-128">Jakmile konfigurační server je nasazena v AWS a zaregistrována trezoru služeb zotavení, byste měli vidět hello konfigurační server a procesový server v rámci infrastruktury Site Recovery, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="d7563-128">Once your configuration server is deployed in AWS and registered with your Recovery Services vault, you should see hello configuration server and process server under Site Recovery infrastructure as shown below:</span></span>

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. <span data-ttu-id="d7563-130">Poté, co nasadíte hello konfigurační server (to může trvat až too15 minustes pro něj tooappear), ověřte, že může komunikovat se službou hello virtuálních počítačů, které chcete toomigrate.</span><span class="sxs-lookup"><span data-stu-id="d7563-130">After you've deployed hello configuration server (it might take up too15 minustes for it tooappear), validate that it can communicate with hello VMs that you want toomigrate.</span></span>

6. <span data-ttu-id="d7563-131">[Nakonfigurování nastavení replikace](site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="d7563-131">[Set up replication settings](site-recovery-setup-replication-settings-vmware.md).</span></span>

7. <span data-ttu-id="d7563-132">Povolení replikace: povolení replikace pro virtuální počítače chcete toomigrate hello.</span><span class="sxs-lookup"><span data-stu-id="d7563-132">Enable replication: Enable replication for hello VMs you want toomigrate.</span></span> <span data-ttu-id="d7563-133">Může zjišťovat hello EC2 instancí pomocí hello privátní IP adresy, které můžete získat z konzoly EC2 hello.</span><span class="sxs-lookup"><span data-stu-id="d7563-133">You can discover hello EC2 instances using hello private IP addresses, which you can get from hello EC2 console.</span></span>

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. <span data-ttu-id="d7563-135">Poté, co byly chráněny hello EC2 instance a tooAzure hello replikace skončí, [spustit testovací převzetí služeb](site-recovery-test-failover-to-azure.md) toovalidate výkon aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="d7563-135">Once hello EC2 instances have been protected and hello replication tooAzure is complete, [run a Test Failover](site-recovery-test-failover-to-azure.md) toovalidate your application's performance in Azure.</span></span>

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. <span data-ttu-id="d7563-137">Pro každý virtuální počítač spusťte převzetí služeb při selhání z AWS tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d7563-137">Run a Failover from AWS tooAzure for each VM.</span></span> <span data-ttu-id="d7563-138">Volitelně můžete vytvořit plán obnovení a převzetí služeb, toomigrate spustit více virtuálních počítačů z AWS tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d7563-138">Optionally, you can create a recovery plan and run a Failover, toomigrate multiple virtual machines from AWS tooAzure.</span></span> <span data-ttu-id="d7563-139">[Další informace](site-recovery-create-recovery-plans.md) o plány obnovení.</span><span class="sxs-lookup"><span data-stu-id="d7563-139">[Learn more](site-recovery-create-recovery-plans.md) about recovery plans.</span></span>

10. <span data-ttu-id="d7563-140">Pro migraci nepotřebujete toocommit převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="d7563-140">For migration, you don't need toocommit a failover.</span></span> <span data-ttu-id="d7563-141">Místo toho vybrat možnost provedení migrace hello pro každý počítač má toomigrate.</span><span class="sxs-lookup"><span data-stu-id="d7563-141">Instead, you select hello Complete Migration option for each machine you want toomigrate.</span></span> <span data-ttu-id="d7563-142">Hello dokončení migrace akce dokončí se proces migrace hello odebere replikaci pro počítač hello a zastaví Site Recovery fakturace pro počítač hello.</span><span class="sxs-lookup"><span data-stu-id="d7563-142">hello Complete Migration action finishes up hello migration process, removes replication for hello machine, and stops Site Recovery billing for hello machine.</span></span>

    ![Migrace](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a><span data-ttu-id="d7563-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7563-144">Next steps</span></span>

- <span data-ttu-id="d7563-145">[Příprava migrované počítače tooenable replikace](site-recovery-azure-to-azure-after-migration.md) tooanother oblast potřebovat obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="d7563-145">[Prepare migrated machines tooenable replication](site-recovery-azure-to-azure-after-migration.md) tooanother region for disaster recovery needs.</span></span>
- <span data-ttu-id="d7563-146">Začněte chránit svoje úlohy pomocí [replikace virtuálních počítačů Azure](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="d7563-146">Start protecting your workloads by [replicating Azure virtual machines.](site-recovery-azure-to-azure.md)</span></span>
