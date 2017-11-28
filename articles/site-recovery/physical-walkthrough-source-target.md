---
title: "aaaSet hello zdroj a cíl pro fyzický server tooAzure replikace s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky tooset hello nastavení zdroj a cíl pro replikaci úložiště tooAzure fyzických serverů s hello služba Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 2e3d03bc-4e53-4590-8a01-f702d3cc9500
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e75ef23174d77509bf54380eba8f251a7337a45f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-hello-source-and-target-for-physical-server-replication-tooazure"></a><span data-ttu-id="4da26-103">Krok 7: Nastavení hello zdroj a cíl pro tooAzure replikace fyzického serveru</span><span class="sxs-lookup"><span data-stu-id="4da26-103">Step 7: Set up hello source and target for physical server replication tooAzure</span></span>

<span data-ttu-id="4da26-104">Tento článek popisuje, jak tooconfigure zdrojové a cílové nastavení při replikaci místně tooAzure fyzických serverů pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4da26-104">This article describes how tooconfigure source and target settings when replicating on-premises physical servers tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="4da26-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="4da26-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="4da26-106">Nastavit hello zdrojové prostředí</span><span class="sxs-lookup"><span data-stu-id="4da26-106">Set up hello source environment</span></span>

<span data-ttu-id="4da26-107">Nastavení konfigurace serveru hello, zaregistrovat v trezoru hello a zjistit počítače.</span><span class="sxs-lookup"><span data-stu-id="4da26-107">Set up hello configuration server, register it in hello vault, and discover machines.</span></span>

1. <span data-ttu-id="4da26-108">Klikněte na tlačítko **lokality obnovení** > **krok 1: Příprava infrastruktury** > **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="4da26-108">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Source**.</span></span>
2. <span data-ttu-id="4da26-109">Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server**.</span><span class="sxs-lookup"><span data-stu-id="4da26-109">If you don’t have a configuration server, click **+Configuration server**.</span></span>
3. <span data-ttu-id="4da26-110">V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.</span><span class="sxs-lookup"><span data-stu-id="4da26-110">In **Add Server**, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="4da26-111">Stáhněte si soubor instalace hello Unified instalace nástroje Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="4da26-111">Download hello Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="4da26-112">Stáhněte si registrační klíč trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="4da26-112">Download hello vault registration key.</span></span> <span data-ttu-id="4da26-113">Tuto funkci potřebujete po spuštění Unified instalace.</span><span class="sxs-lookup"><span data-stu-id="4da26-113">You need this when you run Unified Setup.</span></span> <span data-ttu-id="4da26-114">Hello klíč je platný pět dní od jeho vygenerování.</span><span class="sxs-lookup"><span data-stu-id="4da26-114">hello key is valid for five days after you generate it.</span></span>

   ![Nastavení zdroje](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a><span data-ttu-id="4da26-116">Zaregistrujte konfigurační server hello v trezoru hello</span><span class="sxs-lookup"><span data-stu-id="4da26-116">Register hello configuration server in hello vault</span></span>

<span data-ttu-id="4da26-117">Následující hello před start a potom spusťte tooinstall hello konfigurační server, hello procesový server a hlavní cílový server hello sjednocený instalační program.</span><span class="sxs-lookup"><span data-stu-id="4da26-117">Do hello following before you start, then run Unified Setup tooinstall hello configuration server, hello process server, and hello master target server.</span></span> <span data-ttu-id="4da26-118">Hello video popisuje vytvoření hello komponenty pro virtuální počítač VMware tooAzure replikace.</span><span class="sxs-lookup"><span data-stu-id="4da26-118">hello video describes setting up hello components for VMware VM tooAzure replication.</span></span> <span data-ttu-id="4da26-119">Hello stejný postup je však platný pro replikaci tooAzure fyzický server.</span><span class="sxs-lookup"><span data-stu-id="4da26-119">However, hello same process is valid for physical server tooAzure replication.</span></span>

- <span data-ttu-id="4da26-120">Ujistěte se, že hello systémové hodiny synchronizované s na konfigurační server hello virtuálních počítačů, [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span><span class="sxs-lookup"><span data-stu-id="4da26-120">On hello configuration server VM, make sure that hello system clock is synchronized with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span></span> <span data-ttu-id="4da26-121">Měla by se shodovat.</span><span class="sxs-lookup"><span data-stu-id="4da26-121">It should match.</span></span> <span data-ttu-id="4da26-122">Pokud je 15 minut před nebo za instalace může selhat.</span><span class="sxs-lookup"><span data-stu-id="4da26-122">If it's 15 minutes in front or behind, setup might fail.</span></span>
- <span data-ttu-id="4da26-123">Spusťte instalační program jako místní správce na počítači serveru konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="4da26-123">Run setup as a Local Administrator on hello configuration server machine.</span></span>
- <span data-ttu-id="4da26-124">Ujistěte se, že je povolená TLS 1.0 na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4da26-124">Make sure TLS 1.0 is enabled on hello VM.</span></span>


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="4da26-125">Hello konfigurační server lze také nainstalovat [z příkazového řádku hello](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="4da26-125">hello configuration server can also be installed [from hello command line](http://aka.ms/installconfigsrv).</span></span>




## <a name="set-up-hello-target-environment"></a><span data-ttu-id="4da26-126">Nastavení cílového prostředí hello</span><span class="sxs-lookup"><span data-stu-id="4da26-126">Set up hello target environment</span></span>

<span data-ttu-id="4da26-127">Než nastavíte hello cílové prostředí, ujistěte se, že máte účet úložiště Azure a virtuální sítě nastavit.</span><span class="sxs-lookup"><span data-stu-id="4da26-127">Before you set up hello target environment, make sure you have an Azure storage account and virtual network set up.</span></span>

1. <span data-ttu-id="4da26-128">Klikněte na tlačítko **připravit infrastrukturu** > **cíl**, a vyberte hello chcete toouse předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="4da26-128">Click **Prepare infrastructure** > **Target**, and select hello Azure subscription you want toouse.</span></span>
2. <span data-ttu-id="4da26-129">Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.</span><span class="sxs-lookup"><span data-stu-id="4da26-129">Specify whether your target deployment model is Resource Manager-based, or classic.</span></span>
3. <span data-ttu-id="4da26-130">Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="4da26-130">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

   ![cíl](./media/physical-walkthrough-source-target/gs-target.png)

4. <span data-ttu-id="4da26-132">Pokud jste dosud nevytvořili účet úložiště nebo sítě, klikněte na tlačítko **+ účet úložiště** nebo **+ síť**, toocreate správce prostředků účtu nebo síti vložené.</span><span class="sxs-lookup"><span data-stu-id="4da26-132">If you haven't created a storage account or network, click **+Storage account** or **+Network**, toocreate a Resource Manager account or network inline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4da26-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4da26-133">Next steps</span></span>

<span data-ttu-id="4da26-134">Přejděte příliš[krok 8: nastavení zásad replikace](physical-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="4da26-134">Go too[Step 8: Set up a replication policy](physical-walkthrough-replication.md)</span></span>
