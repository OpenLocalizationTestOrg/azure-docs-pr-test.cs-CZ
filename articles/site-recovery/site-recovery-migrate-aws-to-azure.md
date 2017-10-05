---
title: "Migrovat virtuální počítače z AWS do Azure | Microsoft Docs"
description: "Tento článek popisuje, jak migrovat virtuální počítače běžící v Amazon Web Services (AWS) do Azure pomocí Azure Site Recovery."
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
ms.openlocfilehash: b3c0727a279649f4f7dae30d41027129ce5b04ee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a><span data-ttu-id="cca71-103">Migrovat virtuální počítače v Amazon Web Services (AWS) do Azure s Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="cca71-103">Migrate virtual machines in Amazon Web Services (AWS) to Azure with Azure Site Recovery</span></span>

<span data-ttu-id="cca71-104">Tento článek popisuje, jak migrovat instance AWS Windows na Azure virtuální počítače s [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="cca71-104">This article describes how to migrate AWS Windows instances to Azure virtual machines with the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="cca71-105">Migrace je efektivně převzetí služeb při selhání AWS do Azure.</span><span class="sxs-lookup"><span data-stu-id="cca71-105">Migration is effectively a failover from AWS to Azure.</span></span> <span data-ttu-id="cca71-106">Nelze navrácení služeb po obnovení počítače do AWS a neexistuje žádný probíhající replikace.</span><span class="sxs-lookup"><span data-stu-id="cca71-106">You can't failback machines to AWS, and there's no ongoing replication.</span></span> <span data-ttu-id="cca71-107">Tento článek popisuje kroky pro migraci na portálu Azure a jsou založené na pokyny [replikace fyzický počítač do Azure](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="cca71-107">This article describes the steps for migration in the Azure portal, and are based on the instructions for [replicating a physical machine to Azure](site-recovery-vmware-to-azure.md).</span></span>

<span data-ttu-id="cca71-108">POST jakékoli dotazy nebo připomínky na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span><span class="sxs-lookup"><span data-stu-id="cca71-108">Post any comments or questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="cca71-109">Podporované operační systémy</span><span class="sxs-lookup"><span data-stu-id="cca71-109">Supported operating systems</span></span>

<span data-ttu-id="cca71-110">Obnovení lokality lze použít k migraci EC2 instancí s některým z následujících operačních systémů:</span><span class="sxs-lookup"><span data-stu-id="cca71-110">Site Recovery can be used to migrate EC2 instances running any of the following operating systems:</span></span>

- <span data-ttu-id="cca71-111">Windows (pouze 64bitová verze)</span><span class="sxs-lookup"><span data-stu-id="cca71-111">Windows(64 bit only)</span></span>
    - <span data-ttu-id="cca71-112">Windows Server 2008 R2 SP1 + (Citrix PV ovladače nebo pouze ovladače AWS PV.</span><span class="sxs-lookup"><span data-stu-id="cca71-112">Windows Server 2008 R2 SP1+ (Citrix PV drivers or AWS PV drivers only.</span></span> <span data-ttu-id="cca71-113">**Nejsou podporovány instance systémem RedHat PV ovladače**) Windows serveru 2012 systému Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="cca71-113">**Instances running RedHat PV drivers are not supported**) Windows Server 2012 Windows Server 2012 R2</span></span>
- <span data-ttu-id="cca71-114">Linux (pouze 64bitová verze)</span><span class="sxs-lookup"><span data-stu-id="cca71-114">Linux (64 bit only)</span></span>
    - <span data-ttu-id="cca71-115">Red Hat Enterprise Linux 6.7 (pouze HVM virtualizované instance)</span><span class="sxs-lookup"><span data-stu-id="cca71-115">Red Hat Enterprise Linux 6.7 (HVM virtualized instances only)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cca71-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cca71-116">Prerequisites</span></span>

<span data-ttu-id="cca71-117">Zde je nutné pro toto nasazení:</span><span class="sxs-lookup"><span data-stu-id="cca71-117">Here's what you need for this deployment:</span></span>

* <span data-ttu-id="cca71-118">**Konfigurační server**: virtuální počítač Amazon EC2, systém Windows Server 2012 R2 je nasazen jako konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="cca71-118">**Configuration server**: An Amazon EC2 VM running Windows Server 2012 R2 is deployed as the configuration server.</span></span> <span data-ttu-id="cca71-119">Ve výchozím nastavení jsou nainstalované ostatní Azure Site Recovery součásti (procesový server a hlavní cílový server) při nasazování konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="cca71-119">By default, the other Azure Site Recovery components (process server and master target server) are installed when you deploy the configuration server.</span></span> <span data-ttu-id="cca71-120">Tento článek popisuje kroky pro migraci na portálu Azure a jsou založené na pokyny [Další informace](site-recovery-components.md)</span><span class="sxs-lookup"><span data-stu-id="cca71-120">This article describes the steps for migration in the Azure portal, and are based on the instructions for  [Learn more](site-recovery-components.md)</span></span>

* <span data-ttu-id="cca71-121">**Instance EC2**: The Amazon EC2 instancí virtuálních počítačů, které chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="cca71-121">**EC2 instances**: The Amazon EC2 virtual machines instances that you want to migrate.</span></span>

## <a name="deployment-steps"></a><span data-ttu-id="cca71-122">Kroky nasazení</span><span class="sxs-lookup"><span data-stu-id="cca71-122">Deployment steps</span></span>

1. <span data-ttu-id="cca71-123">Vytvořte trezor služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="cca71-123">Create a Recovery Services vault.</span></span>
2. <span data-ttu-id="cca71-124">Skupina zabezpečení vašich EC2 instancí by měla obsahovat pravidla nakonfigurovaný tak, aby umožňovala komunikaci mezi EC2 instanci, která chcete migrovat a instance, na kterém plánujete nasadit konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="cca71-124">The Security Group of your EC2 instances should have rules configured to allow communication between the EC2 instance that you want to migrate, and the instance on which you plan to deploy the Configuration Server.</span></span>

3. <span data-ttu-id="cca71-125">Na stejném Amazon virtuální privátní Cloud jako vaše EC2 instance nasaďte server konfigurace automatické obnovení systému.</span><span class="sxs-lookup"><span data-stu-id="cca71-125">On the same Amazon Virtual Private Cloud as your EC2 instances, deploy an ASR configuration server.</span></span> <span data-ttu-id="cca71-126">Odkazovat VMware / z fyzických na Azure požadavky pro nasazení požadavky na konfiguraci serveru.</span><span class="sxs-lookup"><span data-stu-id="cca71-126">Refer the VMware / Physical to Azure prerequisites for configuration server deployment requirements.</span></span>

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  <span data-ttu-id="cca71-128">Jakmile konfigurační server je nasazena v AWS a zaregistrována trezoru služeb zotavení, byste měli vidět konfigurační server a procesový server v rámci infrastruktury Site Recovery, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="cca71-128">Once your configuration server is deployed in AWS and registered with your Recovery Services vault, you should see the configuration server and process server under Site Recovery infrastructure as shown below:</span></span>

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. <span data-ttu-id="cca71-130">Poté, co nasadíte konfigurační server (může trvat až 15 minustes pro zobrazí), ověření, že může komunikovat s virtuálními počítači, které chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="cca71-130">After you've deployed the configuration server (it might take up to 15 minustes for it to appear), validate that it can communicate with the VMs that you want to migrate.</span></span>

6. <span data-ttu-id="cca71-131">[Nakonfigurování nastavení replikace](site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="cca71-131">[Set up replication settings](site-recovery-setup-replication-settings-vmware.md).</span></span>

7. <span data-ttu-id="cca71-132">Povolení replikace: povolení replikace pro virtuální počítače, které chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="cca71-132">Enable replication: Enable replication for the VMs you want to migrate.</span></span> <span data-ttu-id="cca71-133">Můžete zjistit pomocí privátních IP adres, které můžete získat z konzole EC2 EC2 instance.</span><span class="sxs-lookup"><span data-stu-id="cca71-133">You can discover the EC2 instances using the private IP addresses, which you can get from the EC2 console.</span></span>

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. <span data-ttu-id="cca71-135">Jakmile chráněné instance EC2 a replikaci do Azure se dokončí, [spustit testovací převzetí služeb](site-recovery-test-failover-to-azure.md) ověření výkon aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="cca71-135">Once the EC2 instances have been protected and the replication to Azure is complete, [run a Test Failover](site-recovery-test-failover-to-azure.md) to validate your application's performance in Azure.</span></span>

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. <span data-ttu-id="cca71-137">Spuštění převzetí služeb při selhání z AWS do Azure pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cca71-137">Run a Failover from AWS to Azure for each VM.</span></span> <span data-ttu-id="cca71-138">Volitelně můžete vytvořit plán obnovení a spusťte převzetí služeb při selhání, migrovat několik virtuálních počítačů z AWS do Azure.</span><span class="sxs-lookup"><span data-stu-id="cca71-138">Optionally, you can create a recovery plan and run a Failover, to migrate multiple virtual machines from AWS to Azure.</span></span> <span data-ttu-id="cca71-139">[Další informace](site-recovery-create-recovery-plans.md) o plány obnovení.</span><span class="sxs-lookup"><span data-stu-id="cca71-139">[Learn more](site-recovery-create-recovery-plans.md) about recovery plans.</span></span>

10. <span data-ttu-id="cca71-140">U migrace není nutné provádět převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="cca71-140">For migration, you don't need to commit a failover.</span></span> <span data-ttu-id="cca71-141">Místo toho vybrat možnost provedení migrace pro každý počítač, který chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="cca71-141">Instead, you select the Complete Migration option for each machine you want to migrate.</span></span> <span data-ttu-id="cca71-142">Dokončení migrace akci dokončí se proces migrace, odebere replikaci pro počítač a zastaví fakturace Site Recovery pro tento počítač.</span><span class="sxs-lookup"><span data-stu-id="cca71-142">The Complete Migration action finishes up the migration process, removes replication for the machine, and stops Site Recovery billing for the machine.</span></span>

    ![Migrace](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a><span data-ttu-id="cca71-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cca71-144">Next steps</span></span>

- <span data-ttu-id="cca71-145">[Připravte migrované počítače na povolení replikace](site-recovery-azure-to-azure-after-migration.md) do jiné oblasti pro případ, že by bylo nutné je zotavit po havárii.</span><span class="sxs-lookup"><span data-stu-id="cca71-145">[Prepare migrated machines to enable replication](site-recovery-azure-to-azure-after-migration.md) to another region for disaster recovery needs.</span></span>
- <span data-ttu-id="cca71-146">Začněte chránit svoje úlohy pomocí [replikace virtuálních počítačů Azure](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="cca71-146">Start protecting your workloads by [replicating Azure virtual machines.](site-recovery-azure-to-azure.md)</span></span>
