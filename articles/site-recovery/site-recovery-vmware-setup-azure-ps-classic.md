---
title: " Spravovat proces serveru spuštěného v Azure(Classic) | Microsoft Docs"
description: "Tento článek popisuje, jak tooset nahoru navrácení služeb po obnovení procesu Server(Classic) v Azure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: eadcc0236c77c9ebbbc885c4a7ee81098f1f4e72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-process-server-running-in-azure-classic"></a><span data-ttu-id="790a1-103">Správa procesu serveru běží v Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="790a1-103">Manage a Process Server running in Azure (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="790a1-104">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="790a1-104">Azure Classic </span></span>](./site-recovery-vmware-setup-azure-ps-classic.md)
> * [<span data-ttu-id="790a1-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="790a1-105">Resource Manager</span></span>](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

<span data-ttu-id="790a1-106">Během navrácení služeb po obnovení je doporučeno toodeploy procesní Server v Azure při vysoké latenci mezi hello Azure Virtual Network a v místní síti.</span><span class="sxs-lookup"><span data-stu-id="790a1-106">During failback, it is recommended toodeploy Process Server in Azure if there is high latency between hello Azure Virtual Network and your on-premises network.</span></span> <span data-ttu-id="790a1-107">Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat servery proces hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="790a1-107">This article describes how you can set up, configure, and manage hello process servers running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="790a1-108">Tento článek je toobe použít, pokud jste použili Classic jako model nasazení hello hello virtuálních počítačů během převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="790a1-108">This article is toobe used if you used Classic as hello deployment model for hello virtual machines during failover.</span></span> <span data-ttu-id="790a1-109">Pokud jste použili správce prostředků jako hello kroky hello postupujte podle modelu nasazení v [jak tooset až & nakonfigurovat procesový Server navrácení služeb po obnovení (Resource Manager)](./site-recovery-vmware-setup-azure-ps-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="790a1-109">If you used Resource Manager as hello deployment model follow hello steps in [How tooset up & configure a Failback Process Server (Resource Manager)](./site-recovery-vmware-setup-azure-ps-resource-manager.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="790a1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="790a1-110">Prerequisites</span></span>

[!INCLUDE [site-recovery-vmware-process-server-prereq](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a><span data-ttu-id="790a1-111">Nasadit procesový Server na platformě Azure</span><span class="sxs-lookup"><span data-stu-id="790a1-111">Deploy a Process Server on Azure</span></span>

1. <span data-ttu-id="790a1-112">V Azure Marketplace, vytvoření virtuálního počítače pomocí hello **Microsoft Azure Site obnovení proces serveru V2**</span><span class="sxs-lookup"><span data-stu-id="790a1-112">In Azure Marketplace, create a virtual machine using hello **Microsoft Azure Site Recovery Process Server V2**</span></span> </br>
    <span data-ttu-id="790a1-113">![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)</span><span class="sxs-lookup"><span data-stu-id="790a1-113">![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)</span></span>
2. <span data-ttu-id="790a1-114">Ujistěte se, že vyberete model nasazení hello jako **Classic**</span><span class="sxs-lookup"><span data-stu-id="790a1-114">Ensure that you select hello deployment model as **Classic**</span></span> </br><span data-ttu-id="790a1-115">
  ![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)</span><span class="sxs-lookup"><span data-stu-id="790a1-115">
![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)</span></span>
3. <span data-ttu-id="790a1-116">V Průvodci virtuálního počítače vytvořit hello > Základní nastavení, ujistěte se, vyberte předplatné a umístění toowhere hello převzal hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="790a1-116">In hello Create virtual machine wizard > Basic Settings, ensure you select hello Subscription and Location toowhere you failed over hello virtual machines.</span></span></br><span data-ttu-id="790a1-117">
  ![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)</span><span class="sxs-lookup"><span data-stu-id="790a1-117">
![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)</span></span>
4. <span data-ttu-id="790a1-118">Ujistěte se, že hello virtuální počítač je připojen toohello Azure Virtual Network toowhich hello při selhání virtuálního počítače je připojen.</span><span class="sxs-lookup"><span data-stu-id="790a1-118">Ensure that hello virtual machine is connected toohello Azure Virtual Network toowhich hello failed over virtual machine is connected.</span></span></br><span data-ttu-id="790a1-119">
  ![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)</span><span class="sxs-lookup"><span data-stu-id="790a1-119">
![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)</span></span>
5. <span data-ttu-id="790a1-120">Po zřízení hello procesového serveru virtuálního počítače, potřebujete toolog v a zaregistrovat ji pomocí hello konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="790a1-120">Once hello Process Server virtual machine is provisioned, you need toolog in and register it with hello Configuration Server.</span></span>

> [!NOTE]
> <span data-ttu-id="790a1-121">možné toouse toobe tento procesový Server navrácení služeb po obnovení, je nutné tooregister její hello místní konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="790a1-121">toobe able toouse this Process Server for failback, you need tooregister it with hello on-premises configuration server.</span></span>

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a><span data-ttu-id="790a1-122">Registrace hello procesového serveru (spuštění v Azure) tooa konfigurační Server (s místní)</span><span class="sxs-lookup"><span data-stu-id="790a1-122">Registering hello Process Server (running in Azure) tooa Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a><span data-ttu-id="790a1-123">Upgrade verze toolatest procesový Server hello.</span><span class="sxs-lookup"><span data-stu-id="790a1-123">Upgrading hello Process Server toolatest version.</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a><span data-ttu-id="790a1-124">Zrušení registrace hello procesového serveru (spuštění v Azure) z konfigurace serveru (místní)</span><span class="sxs-lookup"><span data-stu-id="790a1-124">Unregistering hello Process Server (running in Azure) from a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
