---
title: "aaaInstall hello službu Mobility u replikace VMware tooAzure | Microsoft Docs"
description: "Tento článek popisuje, jak tooinstall hello agenta služby Mobility pro replikaci tooAzure VMware službou Azure Site Recovery hello."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 3189fbcd-6b5b-4ffb-b5a9-e2080c37f9d9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: d3b7bc9c4d84d13317e0b0b47adcf38e8c41d0bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-install-hello-mobility-service"></a><span data-ttu-id="9dab9-103">Krok 10: Instalace služby Mobility hello</span><span class="sxs-lookup"><span data-stu-id="9dab9-103">Step 10: Install hello Mobility service</span></span>


<span data-ttu-id="9dab9-104">Tento článek popisuje, jak tooconfigure zdrojové a cílové nastavení při replikaci místně tooAzure virtuální počítače VMware, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9dab9-104">This article describes how tooconfigure source and target settings when replicating on-premises VMware virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="9dab9-105">Hello služba Mobility zaznamenává datové zápisy na počítači a předává je toohello procesový server.</span><span class="sxs-lookup"><span data-stu-id="9dab9-105">hello Mobility service captures data writes on a machine, and forwards them toohello process server.</span></span> <span data-ttu-id="9dab9-106">Je třeba jej nainstalovat na každém počítači, které chcete tooreplicate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="9dab9-106">It should be installed on each machine that you want tooreplicate tooAzure.</span></span>

<span data-ttu-id="9dab9-107">Můžete nainstalovat ručně službu Mobility hello, pomocí nabízené instalace z hello Site Recovery procesový server, pokud je zapnutá replikace, nebo pomocí nástroje System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="9dab9-107">You can install hello Mobility service manual, using a push installation from hello Site Recovery process server when replication is enabled, or use a tool System Center Configuration Manager.</span></span> <span data-ttu-id="9dab9-108">Pokud používáte nabízenou instalaci, hello je nainstalovaná služba hello virtuálních počítačů při je zapnutá replikace.</span><span class="sxs-lookup"><span data-stu-id="9dab9-108">If you use push installation, hello service is installed on hello VM when replication is enabled.</span></span>

<span data-ttu-id="9dab9-109">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="9dab9-109">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="install-manually"></a><span data-ttu-id="9dab9-110">Ruční instalace</span><span class="sxs-lookup"><span data-stu-id="9dab9-110">Install manually</span></span>

1. <span data-ttu-id="9dab9-111">Zkontrolujte hello [požadavky](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) pro ruční instalaci.</span><span class="sxs-lookup"><span data-stu-id="9dab9-111">Check hello [prerequisites](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) for manual installation.</span></span>
2. <span data-ttu-id="9dab9-112">Postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) pro ruční instalaci pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="9dab9-112">Follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) for manual installation using hello portal.</span></span>
3. <span data-ttu-id="9dab9-113">Pokud dáváte přednost tooinstall z hello příkazového řádku, postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span><span class="sxs-lookup"><span data-stu-id="9dab9-113">If you prefer tooinstall from hello command line, follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span></span>

## <a name="install-from-hello-process-server"></a><span data-ttu-id="9dab9-114">Nainstalujte z hello procesového serveru</span><span class="sxs-lookup"><span data-stu-id="9dab9-114">Install from hello process server</span></span>

<span data-ttu-id="9dab9-115">Pokud chcete instalace služby Mobility toopush hello z hello procesový server, když povolíte replikaci pro virtuální počítač, je třeba účet, který mohou být využívána hello proces serveru tooaccess hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9dab9-115">If you want toopush hello Mobility service installation from hello process server when you enable replication for a VM, you need an account that can be used by hello process server tooaccess hello VM.</span></span> <span data-ttu-id="9dab9-116">Hello účet se používá pouze pro hello nabízenou instalaci.</span><span class="sxs-lookup"><span data-stu-id="9dab9-116">hello account is only used for hello push installation.</span></span>

1. <span data-ttu-id="9dab9-117">Měli byste mít [vytvořili účet](vmware-walkthrough-prepare-vmware.md) který lze použít pro nabízenou instalaci.</span><span class="sxs-lookup"><span data-stu-id="9dab9-117">You should have [created an account](vmware-walkthrough-prepare-vmware.md) that can be used for push installation.</span></span> <span data-ttu-id="9dab9-118">Potom zadejte hello účet, že který má toouse při konfiguraci nastavení zdroje během nasazování Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9dab9-118">You then specify hello account you want toouse when you configure source settings during Site Recovery deployment.</span></span>
2. <span data-ttu-id="9dab9-119">Potom postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Pokud chcete, aby služba Mobility hello toopush na virtuální počítače se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="9dab9-119">Then follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) if you want toopush hello Mobility service on VMs running Windows or Linux.</span></span>

## <a name="other-methods"></a><span data-ttu-id="9dab9-120">Ostatní metody</span><span class="sxs-lookup"><span data-stu-id="9dab9-120">Other methods</span></span>

<span data-ttu-id="9dab9-121">Další informace o [instalaci služby Mobility hello pomocí nástroje Configuration Manager](site-recovery-install-mobility-service-using-sccm.md), nebo pomocí [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span><span class="sxs-lookup"><span data-stu-id="9dab9-121">Learn more about [installing hello Mobility service using Configuration Manager](site-recovery-install-mobility-service-using-sccm.md), or using [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9dab9-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9dab9-122">Next steps</span></span>

<span data-ttu-id="9dab9-123">Přejděte příliš[krok 11: povolení replikace](vmware-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="9dab9-123">Go too[Step 11: Enable replication](vmware-walkthrough-enable-replication.md)</span></span>
