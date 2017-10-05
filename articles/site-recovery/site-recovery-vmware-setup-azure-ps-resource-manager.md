---
title: " Správa procesu serveru běží v Azure (Resource Manager) | Microsoft Docs"
description: "Tento článek popisuje, jak nastavit procesový server navrácení služeb po obnovení (Resource Manager) v Azure."
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
ms.openlocfilehash: 2b9b31abd5d11d02935a74e47d26be9803cdc920
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-a-process-server-running-in-azure-resource-manager"></a><span data-ttu-id="54438-103">Správa procesu serveru běží v Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="54438-103">Manage a process server running in Azure (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="54438-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="54438-104">Resource Manager</span></span>](./site-recovery-vmware-setup-azure-ps-resource-manager.md)
> * [<span data-ttu-id="54438-105">Classic</span><span class="sxs-lookup"><span data-stu-id="54438-105">Classic </span></span>](./site-recovery-vmware-setup-azure-ps-classic.md)

<span data-ttu-id="54438-106">Během navrácení služeb po obnovení doporučujeme nasadit procesový server v Azure, když je vysoké latenci mezi virtuální síť Azure a v místní síti.</span><span class="sxs-lookup"><span data-stu-id="54438-106">During failback, it is recommended to deploy process server in Azure if there is high latency between the Azure Virtual Network and your on-premises network.</span></span> <span data-ttu-id="54438-107">Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat servery proces běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="54438-107">This article describes how you can set up, configure, and manage the process servers running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="54438-108">Tento článek má být použita, pokud jste použili **Resource Manager** jako model nasazení pro virtuální počítače během převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="54438-108">This article is to be used if you used **Resource Manager** as the deployment model for the virtual machines during failover.</span></span> <span data-ttu-id="54438-109">Pokud jste použili **Classic** jako model nasazení, postupujte podle kroků v [jak nastavit a konfigurovat procesový server navrácení služeb po obnovení (klasické)](./site-recovery-vmware-setup-azure-ps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="54438-109">If you used **Classic** as the deployment model, follow the steps in [How to set up & configure a Failback process server (Classic)](./site-recovery-vmware-setup-azure-ps-classic.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54438-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="54438-110">Prerequisites</span></span>

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a><span data-ttu-id="54438-111">Nasadit procesový server na platformě Azure</span><span class="sxs-lookup"><span data-stu-id="54438-111">Deploy a process server on Azure</span></span>
1. <span data-ttu-id="54438-112">V trezoru > **infrastruktura Site Recovery** (v části "Manage") > **konfigurační servery** (pod nadpisem "Pro VMware a fyzické počítače"), vyberte konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="54438-112">In the Vault > **Site Recovery Infrastructure** (under the "Manage" heading) > **Configuration Servers** (under "For VMware and Physical Machines" heading), select the configuration server.</span></span>
2. <span data-ttu-id="54438-113">Na stránce podrobností konfigurace serveru, které se otevře, klikněte na tlačítko "+ zpracovat serveru.</span><span class="sxs-lookup"><span data-stu-id="54438-113">In the Configuration Server details page that opens click "+ Process server"</span></span>

  ![Přidat server galerie procesu](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps.png)

3.  <span data-ttu-id="54438-115">Na **přidat procesový server** vyberte následující hodnoty</span><span class="sxs-lookup"><span data-stu-id="54438-115">On the **Add process server** page, select the following values</span></span>

  ![Přidat položku Galerie procesového serveru](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-1.png)
|<span data-ttu-id="54438-117">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="54438-117">**Field Name**</span></span>|<span data-ttu-id="54438-118">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="54438-118">**Value**</span></span>|
|-|-|
|<span data-ttu-id="54438-119">Zvolte, kam chcete procesový server nasadit.</span><span class="sxs-lookup"><span data-stu-id="54438-119">Choose where you want to deploy your process server</span></span>|<span data-ttu-id="54438-120">Vyberte hodnotu **nasadit procesový server navrácení služeb po obnovení v Azure**</span><span class="sxs-lookup"><span data-stu-id="54438-120">Select the value **Deploy a failback process server in Azure**</span></span> |
|<span data-ttu-id="54438-121">Předplatné</span><span class="sxs-lookup"><span data-stu-id="54438-121">Subscription</span></span>|<span data-ttu-id="54438-122">Vyberte předplatné Azure, kde převzal virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="54438-122">Select the Azure Subscription where you failed over the virtual machines</span></span>|
|<span data-ttu-id="54438-123">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="54438-123">Resource Group</span></span>|<span data-ttu-id="54438-124">Můžete vytvořit skupinu prostředků a nasadit server tento proces, nebo můžete nasadit procesový server v existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="54438-124">You can create a Resource Group to deploy this process server or choose to deploy the process server in an existing Resource Group</span></span>|
|<span data-ttu-id="54438-125">Umístění</span><span class="sxs-lookup"><span data-stu-id="54438-125">Location</span></span>|<span data-ttu-id="54438-126">Vyberte datové centrum Azure, do kterého virtuální počítače kde převzetí služeb při selhání do</span><span class="sxs-lookup"><span data-stu-id="54438-126">Select the Azure Data Center into which the virtual machines where failed over into</span></span>|
|<span data-ttu-id="54438-127">Síť Azure</span><span class="sxs-lookup"><span data-stu-id="54438-127">Azure Network</span></span>|<span data-ttu-id="54438-128">Vyberte virtuální Network(VNet) Azure, virtuální počítače kde převzetí služeb při selhání do.</span><span class="sxs-lookup"><span data-stu-id="54438-128">Select the Azure Virtual Network(VNet) that the virtual machines where failed over into.</span></span> <span data-ttu-id="54438-129">Pokud se při selhání virtuálního počítače do více virtuálními sítěmi Azure budete potřebovat procesový server nasadit na virtuální síť</span><span class="sxs-lookup"><span data-stu-id="54438-129">If you failed over virtual machines into multiple Azure VNets, then you need a process server deployed per VNet</span></span>|

4. <span data-ttu-id="54438-130">Zadejte zbývající vlastnosti u procesového serveru</span><span class="sxs-lookup"><span data-stu-id="54438-130">Fill in the rest of the properties for the process server</span></span>

  ![Přidejte procesový server souhrn](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-2.png)
|<span data-ttu-id="54438-132">**Název pole**</span><span class="sxs-lookup"><span data-stu-id="54438-132">**Field Name**</span></span>|<span data-ttu-id="54438-133">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="54438-133">**Value**</span></span>|
|-|-|
|<span data-ttu-id="54438-134">Název serveru</span><span class="sxs-lookup"><span data-stu-id="54438-134">Server Name</span></span>|<span data-ttu-id="54438-135">Zobrazovaný název a název hostitele pro virtuální počítač serveru procesu</span><span class="sxs-lookup"><span data-stu-id="54438-135">Display name & Host name for your process server virtual machine</span></span>|
| <span data-ttu-id="54438-136">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="54438-136">User Name</span></span>|<span data-ttu-id="54438-137">Uživatelské jméno, který se stává správcem na tomto virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="54438-137">A user name that becomes an Administrator on that virtual machine</span></span>|
|<span data-ttu-id="54438-138">Účet úložiště</span><span class="sxs-lookup"><span data-stu-id="54438-138">Storage Account</span></span>|<span data-ttu-id="54438-139">Název účtu úložiště, kde jsou umístěny virtuální počítač virtuálního disku</span><span class="sxs-lookup"><span data-stu-id="54438-139">Name of the Storage Account where the virtual machine's virtual disk's are placed</span></span>|
|<span data-ttu-id="54438-140">Podsíť</span><span class="sxs-lookup"><span data-stu-id="54438-140">Subnet</span></span>|<span data-ttu-id="54438-141">Podsíť virtuální sítě Azure, ke kterému je připojený virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="54438-141">The subnet of the Azure VNet to which the virtual machine is connected</span></span>|
| <span data-ttu-id="54438-142">IP adresa</span><span class="sxs-lookup"><span data-stu-id="54438-142">IP Address</span></span>|<span data-ttu-id="54438-143">IP adresy, které chcete procesový server předpokládat, že po jeho spuštění</span><span class="sxs-lookup"><span data-stu-id="54438-143">IP Address that you would like the process server to assume once it boots up</span></span>|
5. <span data-ttu-id="54438-144">Klikněte na tlačítko OK zahájíte nasazení proces serveru virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="54438-144">Click the OK button to start deploying the process server virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="54438-145">Abyste mohli použít tento procesový server navrácení služeb po obnovení, musíte zaregistrovat ji pomocí místní konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="54438-145">To be able to use this process server for failback, you need to register it with the on-premises configuration server.</span></span>

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a><span data-ttu-id="54438-146">Registrace serveru procesu (spuštění v Azure) konfigurace serveru (s místní)</span><span class="sxs-lookup"><span data-stu-id="54438-146">Registering the process server (running in Azure) to a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-the-process-server-to-latest-version"></a><span data-ttu-id="54438-147">Upgrade na nejnovější verzi procesového serveru.</span><span class="sxs-lookup"><span data-stu-id="54438-147">Upgrading the process server to latest version.</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-the-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a><span data-ttu-id="54438-148">Zrušení registrace serveru procesu (spuštění v Azure) z konfigurace serveru (místní)</span><span class="sxs-lookup"><span data-stu-id="54438-148">Unregistering the process server (running in Azure) from a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
