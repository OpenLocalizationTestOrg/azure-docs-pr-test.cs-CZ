---
title: "aaaInstall hello službu Mobility u fyzického serveru tooAzure replikace | Microsoft Docs"
description: "Tento článek popisuje, jak tooinstall hello agenta služby Mobility na fyzických serverech replikace tooAzure službou Azure Site Recovery hello."
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
ms.openlocfilehash: 48fd2c0ffe67875ed446c8167c2ae7f90d3f537c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-install-hello-mobility-service"></a><span data-ttu-id="2c693-103">Krok 9: Instalace služby Mobility hello</span><span class="sxs-lookup"><span data-stu-id="2c693-103">Step 9: Install hello Mobility service</span></span>


<span data-ttu-id="2c693-104">Tento článek popisuje, jak tooinstall hello službu mobility při replikaci místní tooAzure fyzických serverů Windows nebo Linuxem, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2c693-104">This article describes how tooinstall hello Mobility service component when replicating on-premises Windows/Linux physical servers tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="2c693-105">Hello služba Mobility zaznamenává datové zápisy na počítači a předává je toohello procesový server.</span><span class="sxs-lookup"><span data-stu-id="2c693-105">hello Mobility service captures data writes on a machine, and forwards them toohello process server.</span></span> <span data-ttu-id="2c693-106">Je třeba jej nainstalovat na každém serveru, které chcete tooreplicate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2c693-106">It should be installed on each server that you want tooreplicate tooAzure.</span></span>

<span data-ttu-id="2c693-107">Služba Mobility hello můžete nainstalovat ručně, nebo pomocí nabízené instalace z hello Site Recovery zpracovat, server, pokud je zapnutá replikace, nebo pomocí nástroje, jako je například System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="2c693-107">You can install hello Mobility service manually, or using a push installation from hello Site Recovery process server when replication is enabled, or using a tool such as System Center Configuration Manager.</span></span> <span data-ttu-id="2c693-108">Pokud používáte nabízenou instalaci, hello je nainstalována služba na serveru hello při povolení replikace.</span><span class="sxs-lookup"><span data-stu-id="2c693-108">If you use push installation, hello service is installed on hello server when you enable replication.</span></span>

<span data-ttu-id="2c693-109">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="2c693-109">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="install-manually"></a><span data-ttu-id="2c693-110">Ruční instalace</span><span class="sxs-lookup"><span data-stu-id="2c693-110">Install manually</span></span>

1. <span data-ttu-id="2c693-111">Zkontrolujte hello [požadavky](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) pro ruční instalaci.</span><span class="sxs-lookup"><span data-stu-id="2c693-111">Check hello [prerequisites](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) for manual installation.</span></span>
2. <span data-ttu-id="2c693-112">Postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) pro ruční instalaci pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="2c693-112">Follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) for manual installation using hello portal.</span></span>
3. <span data-ttu-id="2c693-113">Pokud dáváte přednost tooinstall z hello příkazového řádku, postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span><span class="sxs-lookup"><span data-stu-id="2c693-113">If you prefer tooinstall from hello command line, follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span></span>

## <a name="install-from-hello-process-server"></a><span data-ttu-id="2c693-114">Nainstalujte z hello procesového serveru</span><span class="sxs-lookup"><span data-stu-id="2c693-114">Install from hello process server</span></span>

<span data-ttu-id="2c693-115">Pokud chcete toopush hello instalace služby Mobility ze serveru hello procesu, když povolíte replikaci pro počítač, je třeba účet, který lze použít hello proces serveru tooaccess hello počítačem.</span><span class="sxs-lookup"><span data-stu-id="2c693-115">If you want toopush hello Mobility service installation from hello process server when you enable replication for a machine, you need an account that can be used by hello process server tooaccess hello machine.</span></span> <span data-ttu-id="2c693-116">Hello účet se používá pouze pro hello nabízenou instalaci.</span><span class="sxs-lookup"><span data-stu-id="2c693-116">hello account is only used for hello push installation.</span></span>

1. <span data-ttu-id="2c693-117">Pokud jste nevytvořili účet, to udělat pomocí těchto pokynů:</span><span class="sxs-lookup"><span data-stu-id="2c693-117">If you haven't created an account, do so using these guidelines:</span></span>

    - <span data-ttu-id="2c693-118">Můžete použít domény nebo místní účet</span><span class="sxs-lookup"><span data-stu-id="2c693-118">You can use a domain or local account</span></span>
    - <span data-ttu-id="2c693-119">Pro Windows Pokud nepoužíváte účet domény, budete potřebovat toodisable řízení vzdáleného přístupu uživatele v místním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="2c693-119">For Windows, if you're not using a domain account, you need toodisable Remote User Access control on hello local machine.</span></span> <span data-ttu-id="2c693-120">toodo, informace v hello zaregistrovat v rámci **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD hello **LocalAccountTokenFilterPolicy**, s hodnotou 1.</span><span class="sxs-lookup"><span data-stu-id="2c693-120">toodo this, in hello register under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, add hello DWORD entry **LocalAccountTokenFilterPolicy**, with a value of 1.</span></span>
    - <span data-ttu-id="2c693-121">Pokud chcete položku registru hello tooadd pro Windows z rozhraní příkazového řádku, zadejte:</span><span class="sxs-lookup"><span data-stu-id="2c693-121">If you want tooadd hello registry entry for Windows from a CLI, type:</span></span>

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - <span data-ttu-id="2c693-122">Pro systémy Linux hello účet by měl být kořenový na zdrojovém serveru se Linux hello.</span><span class="sxs-lookup"><span data-stu-id="2c693-122">For Linux, hello account should be root on hello source Linux server.</span></span>

2. <span data-ttu-id="2c693-123">Potom postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Pokud chcete, aby služba Mobility hello toopush na virtuální počítače se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="2c693-123">Then follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) if you want toopush hello Mobility service on VMs running Windows or Linux.</span></span>

## <a name="other-installation-methods"></a><span data-ttu-id="2c693-124">Jiné metody instalace</span><span class="sxs-lookup"><span data-stu-id="2c693-124">Other installation methods</span></span>

- <span data-ttu-id="2c693-125">[Další informace o](site-recovery-install-mobility-service-using-sccm.md) instalaci služby Mobility hello pomocí nástroje Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="2c693-125">[Learn about](site-recovery-install-mobility-service-using-sccm.md) installing hello Mobility service using Configuration Manager</span></span>
- <span data-ttu-id="2c693-126">[Další informace o](site-recovery-automate-mobility-service-install.md) instalace s Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="2c693-126">[Learn about](site-recovery-automate-mobility-service-install.md) installing with Azure Automation DSC.</span></span>


## <a name="next-steps"></a><span data-ttu-id="2c693-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c693-127">Next steps</span></span>

<span data-ttu-id="2c693-128">Přejděte příliš[krok 10: povolení replikace](physical-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="2c693-128">Go too[Step 10: Enable replication](physical-walkthrough-enable-replication.md)</span></span>
