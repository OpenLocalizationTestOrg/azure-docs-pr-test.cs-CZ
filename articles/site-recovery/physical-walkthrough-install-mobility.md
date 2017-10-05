---
title: "Nainstalujte službu Mobility u fyzického serveru na Azure replikaci | Microsoft Docs"
description: "Tento článek popisuje postup instalace agenta služby Mobility na fyzických serverech replikaci do Azure se službou Azure Site Recovery."
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
ms.openlocfilehash: d73267d7a64221a3138af19e9a2d5dd15809b927
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-9-install-the-mobility-service"></a><span data-ttu-id="3b1ec-103">Krok 9: Instalace služby Mobility</span><span class="sxs-lookup"><span data-stu-id="3b1ec-103">Step 9: Install the Mobility service</span></span>


<span data-ttu-id="3b1ec-104">Tento článek popisuje, jak nainstalovat službu mobility při replikaci místní Windows nebo Linuxem fyzických serverů do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-104">This article describes how to install the Mobility service component when replicating on-premises Windows/Linux physical servers to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="3b1ec-105">Služba Mobility zaznamenává datové zápisy na počítači a předává je na procesní server.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-105">The Mobility service captures data writes on a machine, and forwards them to the process server.</span></span> <span data-ttu-id="3b1ec-106">Musí být nainstalován na každém serveru, který chcete replikovat do Azure.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-106">It should be installed on each server that you want to replicate to Azure.</span></span>

<span data-ttu-id="3b1ec-107">Můžete nainstalovat službu Mobility ručně, nebo pomocí nabízené instalace z procesového serveru Site Recovery, pokud je zapnutá replikace, nebo pomocí nástroje, jako je například System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-107">You can install the Mobility service manually, or using a push installation from the Site Recovery process server when replication is enabled, or using a tool such as System Center Configuration Manager.</span></span> <span data-ttu-id="3b1ec-108">Pokud používáte nabízenou instalaci, služba je nainstalovaná na serveru, když aktivujete replikaci.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-108">If you use push installation, the service is installed on the server when you enable replication.</span></span>

<span data-ttu-id="3b1ec-109">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="3b1ec-109">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="install-manually"></a><span data-ttu-id="3b1ec-110">Ruční instalace</span><span class="sxs-lookup"><span data-stu-id="3b1ec-110">Install manually</span></span>

1. <span data-ttu-id="3b1ec-111">Zkontrolujte [požadavky](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) pro ruční instalaci.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-111">Check the [prerequisites](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) for manual installation.</span></span>
2. <span data-ttu-id="3b1ec-112">Postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) pro ruční instalaci pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-112">Follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) for manual installation using the portal.</span></span>
3. <span data-ttu-id="3b1ec-113">Pokud preferujete instalaci z příkazového řádku, postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span><span class="sxs-lookup"><span data-stu-id="3b1ec-113">If you prefer to install from the command line, follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span></span>

## <a name="install-from-the-process-server"></a><span data-ttu-id="3b1ec-114">Instalace z procesového serveru</span><span class="sxs-lookup"><span data-stu-id="3b1ec-114">Install from the process server</span></span>

<span data-ttu-id="3b1ec-115">Pokud chcete nabízená instalace služby Mobility z procesového serveru, když povolíte replikaci pro počítač, je třeba účet, který lze použít procesní server pro přístup k počítači.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-115">If you want to push the Mobility service installation from the process server when you enable replication for a machine, you need an account that can be used by the process server to access the machine.</span></span> <span data-ttu-id="3b1ec-116">Účet se používá pouze pro nabízené instalace.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-116">The account is only used for the push installation.</span></span>

1. <span data-ttu-id="3b1ec-117">Pokud jste nevytvořili účet, to udělat pomocí těchto pokynů:</span><span class="sxs-lookup"><span data-stu-id="3b1ec-117">If you haven't created an account, do so using these guidelines:</span></span>

    - <span data-ttu-id="3b1ec-118">Můžete použít domény nebo místní účet</span><span class="sxs-lookup"><span data-stu-id="3b1ec-118">You can use a domain or local account</span></span>
    - <span data-ttu-id="3b1ec-119">Pro systém Windows Pokud nepoužíváte účet domény, je nutné zakázat řízení vzdáleného přístupu uživatele v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-119">For Windows, if you're not using a domain account, you need to disable Remote User Access control on the local machine.</span></span> <span data-ttu-id="3b1ec-120">Chcete-li to provést, v registru podle **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD **LocalAccountTokenFilterPolicy**, s hodnotou 1.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-120">To do this, in the register under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, add the DWORD entry **LocalAccountTokenFilterPolicy**, with a value of 1.</span></span>
    - <span data-ttu-id="3b1ec-121">Pokud chcete přidat položku registru pro Windows z rozhraní příkazového řádku, zadejte:</span><span class="sxs-lookup"><span data-stu-id="3b1ec-121">If you want to add the registry entry for Windows from a CLI, type:</span></span>

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - <span data-ttu-id="3b1ec-122">Pro systémy Linux musí být účet root na zdrojovém serveru Linux.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-122">For Linux, the account should be root on the source Linux server.</span></span>

2. <span data-ttu-id="3b1ec-123">Potom postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Pokud chcete k nabízení služby Mobility na virtuální počítače se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-123">Then follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) if you want to push the Mobility service on VMs running Windows or Linux.</span></span>

## <a name="other-installation-methods"></a><span data-ttu-id="3b1ec-124">Jiné metody instalace</span><span class="sxs-lookup"><span data-stu-id="3b1ec-124">Other installation methods</span></span>

- <span data-ttu-id="3b1ec-125">[Další informace o](site-recovery-install-mobility-service-using-sccm.md) instalaci služby Mobility, pomocí nástroje Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="3b1ec-125">[Learn about](site-recovery-install-mobility-service-using-sccm.md) installing the Mobility service using Configuration Manager</span></span>
- <span data-ttu-id="3b1ec-126">[Další informace o](site-recovery-automate-mobility-service-install.md) instalace s Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="3b1ec-126">[Learn about](site-recovery-automate-mobility-service-install.md) installing with Azure Automation DSC.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3b1ec-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b1ec-127">Next steps</span></span>

<span data-ttu-id="3b1ec-128">Přejděte na [krok 10: povolení replikace](physical-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="3b1ec-128">Go to [Step 10: Enable replication](physical-walkthrough-enable-replication.md)</span></span>
