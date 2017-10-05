---
title: "Nainstalujte službu Mobility u VMware do Azure replikace | Microsoft Docs"
description: "Tento článek popisuje, jak nainstalovat agenta služby Mobility pro VMware do Azure s replikací pomocí služby Azure Site Recovery."
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
ms.openlocfilehash: bc520bd2ea54208889861a7a3b275e3008a05d53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-10-install-the-mobility-service"></a><span data-ttu-id="15cb8-103">Krok 10: Nainstalujte službu Mobility</span><span class="sxs-lookup"><span data-stu-id="15cb8-103">Step 10: Install the Mobility service</span></span>


<span data-ttu-id="15cb8-104">Tento článek popisuje postup konfigurace nastavení zdrojové a cílové při replikaci na lokální virtuální počítače VMware do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="15cb8-104">This article describes how to configure source and target settings when replicating on-premises VMware virtual machines to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="15cb8-105">Služba Mobility zaznamenává datové zápisy na počítači a předává je na procesní server.</span><span class="sxs-lookup"><span data-stu-id="15cb8-105">The Mobility service captures data writes on a machine, and forwards them to the process server.</span></span> <span data-ttu-id="15cb8-106">Musí být nainstalován na každý počítač, který chcete replikovat do Azure.</span><span class="sxs-lookup"><span data-stu-id="15cb8-106">It should be installed on each machine that you want to replicate to Azure.</span></span>

<span data-ttu-id="15cb8-107">Můžete nainstalovat ručně službu Mobility, pomocí nabízené instalace z procesového serveru Site Recovery, pokud je zapnutá replikace, nebo pomocí nástroje System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="15cb8-107">You can install the Mobility service manual, using a push installation from the Site Recovery process server when replication is enabled, or use a tool System Center Configuration Manager.</span></span> <span data-ttu-id="15cb8-108">Pokud používáte nabízenou instalaci, služba je nainstalovaná ve virtuálním počítači, pokud je zapnutá replikace.</span><span class="sxs-lookup"><span data-stu-id="15cb8-108">If you use push installation, the service is installed on the VM when replication is enabled.</span></span>

<span data-ttu-id="15cb8-109">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="15cb8-109">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="install-manually"></a><span data-ttu-id="15cb8-110">Ruční instalace</span><span class="sxs-lookup"><span data-stu-id="15cb8-110">Install manually</span></span>

1. <span data-ttu-id="15cb8-111">Zkontrolujte [požadavky](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) pro ruční instalaci.</span><span class="sxs-lookup"><span data-stu-id="15cb8-111">Check the [prerequisites](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) for manual installation.</span></span>
2. <span data-ttu-id="15cb8-112">Postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) pro ruční instalaci pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="15cb8-112">Follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) for manual installation using the portal.</span></span>
3. <span data-ttu-id="15cb8-113">Pokud preferujete instalaci z příkazového řádku, postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span><span class="sxs-lookup"><span data-stu-id="15cb8-113">If you prefer to install from the command line, follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span></span>

## <a name="install-from-the-process-server"></a><span data-ttu-id="15cb8-114">Instalace z procesového serveru</span><span class="sxs-lookup"><span data-stu-id="15cb8-114">Install from the process server</span></span>

<span data-ttu-id="15cb8-115">Pokud chcete nabízená instalace služby Mobility z procesového serveru, když povolíte replikaci pro virtuální počítač, je třeba účet, který lze použít procesní server přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="15cb8-115">If you want to push the Mobility service installation from the process server when you enable replication for a VM, you need an account that can be used by the process server to access the VM.</span></span> <span data-ttu-id="15cb8-116">Účet se používá pouze pro nabízené instalace.</span><span class="sxs-lookup"><span data-stu-id="15cb8-116">The account is only used for the push installation.</span></span>

1. <span data-ttu-id="15cb8-117">Měli byste mít [vytvořili účet](vmware-walkthrough-prepare-vmware.md) který lze použít pro nabízenou instalaci.</span><span class="sxs-lookup"><span data-stu-id="15cb8-117">You should have [created an account](vmware-walkthrough-prepare-vmware.md) that can be used for push installation.</span></span> <span data-ttu-id="15cb8-118">Potom zadejte účet, který chcete použít při konfiguraci nastavení zdroje během nasazování Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="15cb8-118">You then specify the account you want to use when you configure source settings during Site Recovery deployment.</span></span>
2. <span data-ttu-id="15cb8-119">Potom postupujte podle [tyto pokyny](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Pokud chcete k nabízení služby Mobility na virtuální počítače se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="15cb8-119">Then follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) if you want to push the Mobility service on VMs running Windows or Linux.</span></span>

## <a name="other-methods"></a><span data-ttu-id="15cb8-120">Ostatní metody</span><span class="sxs-lookup"><span data-stu-id="15cb8-120">Other methods</span></span>

<span data-ttu-id="15cb8-121">Další informace o [instalaci služby Mobility, pomocí nástroje Configuration Manager](site-recovery-install-mobility-service-using-sccm.md), nebo pomocí [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span><span class="sxs-lookup"><span data-stu-id="15cb8-121">Learn more about [installing the Mobility service using Configuration Manager](site-recovery-install-mobility-service-using-sccm.md), or using [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="15cb8-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15cb8-122">Next steps</span></span>

<span data-ttu-id="15cb8-123">Přejděte na [krok 11: povolení replikace](vmware-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="15cb8-123">Go to [Step 11: Enable replication](vmware-walkthrough-enable-replication.md)</span></span>
