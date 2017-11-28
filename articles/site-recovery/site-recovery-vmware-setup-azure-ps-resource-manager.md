---
title: " Správa procesu serveru běží v Azure (Resource Manager) | Microsoft Docs"
description: "Tento článek popisuje, jak zpracovat tooset nahoru navrácení služeb po obnovení serveru (Resource Manager) v Azure."
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
ms.openlocfilehash: 70426b96095cc42befff6c4114fb56536284a667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-process-server-running-in-azure-resource-manager"></a><span data-ttu-id="a1ce0-103">Správa procesu serveru běží v Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="a1ce0-103">Manage a process server running in Azure (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a1ce0-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a1ce0-104">Resource Manager</span></span>](./site-recovery-vmware-setup-azure-ps-resource-manager.md)
> * [<span data-ttu-id="a1ce0-105">Classic</span><span class="sxs-lookup"><span data-stu-id="a1ce0-105">Classic </span></span>](./site-recovery-vmware-setup-azure-ps-classic.md)

<span data-ttu-id="a1ce0-106">Během navrácení služeb po obnovení je doporučeno toodeploy procesní server v Azure při vysoké latenci mezi hello Azure Virtual Network a v místní síti.</span><span class="sxs-lookup"><span data-stu-id="a1ce0-106">During failback, it is recommended toodeploy process server in Azure if there is high latency between hello Azure Virtual Network and your on-premises network.</span></span> <span data-ttu-id="a1ce0-107">Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat servery proces hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="a1ce0-107">This article describes how you can set up, configure, and manage hello process servers running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="a1ce0-108">Tento článek je toobe použít, pokud jste použili **Resource Manager** jako model nasazení hello hello virtuálních počítačů během převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="a1ce0-108">This article is toobe used if you used **Resource Manager** as hello deployment model for hello virtual machines during failover.</span></span> <span data-ttu-id="a1ce0-109">Pokud jste použili **Classic** jako model nasazení hello, postupujte podle kroků hello v [jak tooset až & nakonfigurovat procesový server navrácení služeb po obnovení (klasické)](./site-recovery-vmware-setup-azure-ps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="a1ce0-109">If you used **Classic** as hello deployment model, follow hello steps in [How tooset up & configure a Failback process server (Classic)](./site-recovery-vmware-setup-azure-ps-classic.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1ce0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a1ce0-110">Prerequisites</span></span>

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a><span data-ttu-id="a1ce0-111">Nasadit procesový server na platformě Azure</span><span class="sxs-lookup"><span data-stu-id="a1ce0-111">Deploy a process server on Azure</span></span>
1. <span data-ttu-id="a1ce0-112">V hello trezoru > **infrastruktura Site Recovery** (pod nadpisem "Manage" hello) > **konfigurační servery** (pod nadpisem "Pro VMware a fyzické počítače"), vyberte hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="a1ce0-112">In hello Vault > **Site Recovery Infrastructure** (under hello "Manage" heading) > **Configuration Servers** (under "For VMware and Physical Machines" heading), select hello configuration server.</span></span>
2. <span data-ttu-id="a1ce0-113">Na stránce s podrobnostmi o konfigurační Server hello otevřeném klikněte na "+ zpracovat serveru.</span><span class="sxs-lookup"><span data-stu-id="a1ce0-113">In hello Configuration Server details page that opens click "+ Process server"</span></span>

  ![Přidat server galerie procesu](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps.png)

3.  <span data-ttu-id="a1ce0-115">Na hello **přidat procesový server** stránky, vyberte hello následující hodnoty</span><span class="sxs-lookup"><span data-stu-id="a1ce0-115">On hello **Add process server** page, select hello following values</span></span>

  ![Přidat položku Galerie procesového serveru](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-1.png)
|<span data-ttu-id="a1ce0-117">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="a1ce0-117">**Field Name**</span></span>|<span data-ttu-id="a1ce0-118">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="a1ce0-118">**Value**</span></span>|
|-|-|
|<span data-ttu-id="a1ce0-119">Vyberte, kam chcete toodeploy, procesového serveru</span><span class="sxs-lookup"><span data-stu-id="a1ce0-119">Choose where you want toodeploy your process server</span></span>|<span data-ttu-id="a1ce0-120">Vyberte hodnotu hello **nasadit procesový server navrácení služeb po obnovení v Azure**</span><span class="sxs-lookup"><span data-stu-id="a1ce0-120">Select hello value **Deploy a failback process server in Azure**</span></span> |
|<span data-ttu-id="a1ce0-121">Předplatné</span><span class="sxs-lookup"><span data-stu-id="a1ce0-121">Subscription</span></span>|<span data-ttu-id="a1ce0-122">Vyberte hello předplatné Azure, kde převzal hello virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="a1ce0-122">Select hello Azure Subscription where you failed over hello virtual machines</span></span>|
|<span data-ttu-id="a1ce0-123">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="a1ce0-123">Resource Group</span></span>|<span data-ttu-id="a1ce0-124">Můžete vytvořit skupinu prostředků toodeploy tento proces server nebo zvolte toodeploy hello procesový server v existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a1ce0-124">You can create a Resource Group toodeploy this process server or choose toodeploy hello process server in an existing Resource Group</span></span>|
|<span data-ttu-id="a1ce0-125">Umístění</span><span class="sxs-lookup"><span data-stu-id="a1ce0-125">Location</span></span>|<span data-ttu-id="a1ce0-126">Vyberte datové centrum Azure hello do virtuálních počítačů hello kde převzetí služeb při selhání do</span><span class="sxs-lookup"><span data-stu-id="a1ce0-126">Select hello Azure Data Center into which hello virtual machines where failed over into</span></span>|
|<span data-ttu-id="a1ce0-127">Síť Azure</span><span class="sxs-lookup"><span data-stu-id="a1ce0-127">Azure Network</span></span>|<span data-ttu-id="a1ce0-128">Vyberte hello virtuální Network(VNet) Azure, který hello virtuální počítače kde převzetí služeb při selhání do.</span><span class="sxs-lookup"><span data-stu-id="a1ce0-128">Select hello Azure Virtual Network(VNet) that hello virtual machines where failed over into.</span></span> <span data-ttu-id="a1ce0-129">Pokud se při selhání virtuálního počítače do více virtuálními sítěmi Azure budete potřebovat procesový server nasadit na virtuální síť</span><span class="sxs-lookup"><span data-stu-id="a1ce0-129">If you failed over virtual machines into multiple Azure VNets, then you need a process server deployed per VNet</span></span>|

4. <span data-ttu-id="a1ce0-130">Vyplňte hello zbytek hello vlastnosti hello procesového serveru</span><span class="sxs-lookup"><span data-stu-id="a1ce0-130">Fill in hello rest of hello properties for hello process server</span></span>

  ![Přidejte procesový server souhrn](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-2.png)
|<span data-ttu-id="a1ce0-132">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="a1ce0-132">**Field Name**</span></span>|<span data-ttu-id="a1ce0-133">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="a1ce0-133">**Value**</span></span>|
|-|-|
|<span data-ttu-id="a1ce0-134">Název serveru</span><span class="sxs-lookup"><span data-stu-id="a1ce0-134">Server Name</span></span>|<span data-ttu-id="a1ce0-135">Zobrazovaný název a název hostitele pro virtuální počítač serveru procesu</span><span class="sxs-lookup"><span data-stu-id="a1ce0-135">Display name & Host name for your process server virtual machine</span></span>|
| <span data-ttu-id="a1ce0-136">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="a1ce0-136">User Name</span></span>|<span data-ttu-id="a1ce0-137">Uživatelské jméno, který se stává správcem na tomto virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="a1ce0-137">A user name that becomes an Administrator on that virtual machine</span></span>|
|<span data-ttu-id="a1ce0-138">Účet úložiště</span><span class="sxs-lookup"><span data-stu-id="a1ce0-138">Storage Account</span></span>|<span data-ttu-id="a1ce0-139">Název účtu úložiště, kde jsou umístěny hello virtuálního počítače virtuální disk hello</span><span class="sxs-lookup"><span data-stu-id="a1ce0-139">Name of hello Storage Account where hello virtual machine's virtual disk's are placed</span></span>|
|<span data-ttu-id="a1ce0-140">Podsíť</span><span class="sxs-lookup"><span data-stu-id="a1ce0-140">Subnet</span></span>|<span data-ttu-id="a1ce0-141">podsíť Hello hello virtuální síť Azure toowhich hello virtuálního počítače je připojen.</span><span class="sxs-lookup"><span data-stu-id="a1ce0-141">hello subnet of hello Azure VNet toowhich hello virtual machine is connected</span></span>|
| <span data-ttu-id="a1ce0-142">IP adresa</span><span class="sxs-lookup"><span data-stu-id="a1ce0-142">IP Address</span></span>|<span data-ttu-id="a1ce0-143">IP adresy, které chcete hello proces serveru tooassume, po spuštění</span><span class="sxs-lookup"><span data-stu-id="a1ce0-143">IP Address that you would like hello process server tooassume once it boots up</span></span>|
5. <span data-ttu-id="a1ce0-144">Klikněte na tlačítko toostart tlačítko OK hello nasazení virtuálního počítače hello procesu serveru.</span><span class="sxs-lookup"><span data-stu-id="a1ce0-144">Click hello OK button toostart deploying hello process server virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="a1ce0-145">možné toouse toobe tento procesový server navrácení služeb po obnovení, je nutné tooregister její hello místní konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="a1ce0-145">toobe able toouse this process server for failback, you need tooregister it with hello on-premises configuration server.</span></span>

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a><span data-ttu-id="a1ce0-146">Registrace hello procesu serveru (spuštění v Azure) tooa konfigurační Server (s místní)</span><span class="sxs-lookup"><span data-stu-id="a1ce0-146">Registering hello process server (running in Azure) tooa Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a><span data-ttu-id="a1ce0-147">Upgrade verze toolatest hello procesového serveru.</span><span class="sxs-lookup"><span data-stu-id="a1ce0-147">Upgrading hello process server toolatest version.</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a><span data-ttu-id="a1ce0-148">Zrušení registrace hello procesového serveru (spuštění v Azure) z konfigurace serveru (místní)</span><span class="sxs-lookup"><span data-stu-id="a1ce0-148">Unregistering hello process server (running in Azure) from a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
