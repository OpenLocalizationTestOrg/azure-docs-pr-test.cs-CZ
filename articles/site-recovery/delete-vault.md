---
title: "Odstranění trezoru Site Recovery"
description: "Zjistěte, jak odstranit trezoru Azure Site Recovery v závislosti na scénáři Site Recovery."
service: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: b95b9defa0a037f7d7d3ef36b99bc7c53c751050
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="delete-a-site-recovery-vault"></a><span data-ttu-id="a058c-103">Odstranění trezoru Site Recovery</span><span class="sxs-lookup"><span data-stu-id="a058c-103">Delete a Site Recovery vault</span></span>
<span data-ttu-id="a058c-104">Odstranění trezoru služby Azure Site Recovery můžete zabránit závislosti.</span><span class="sxs-lookup"><span data-stu-id="a058c-104">Dependencies can prevent you from deleting an Azure Site Recovery vault.</span></span> <span data-ttu-id="a058c-105">Akce, které je třeba provést lišit v závislosti na scénáři Site Recovery: VMware do Azure, technologie Hyper-V (s nebo bez System Center Virtual Machine Manager) a Azure, Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a058c-105">The actions you need to take vary based on the Site Recovery scenario: VMware to Azure, Hyper-V (with and without System Center Virtual Machine Manager) to Azure, and Azure Backup.</span></span> <span data-ttu-id="a058c-106">Pokud chcete odstranit úložiště použitého v zálohování Azure, najdete v části [odstranit úložiště záloh v Azure](../backup/backup-azure-delete-vault.md).</span><span class="sxs-lookup"><span data-stu-id="a058c-106">To delete a vault used in Azure Backup, see [Delete a Backup vault in Azure](../backup/backup-azure-delete-vault.md).</span></span>

>[!Important]
><span data-ttu-id="a058c-107">Pokud jste testování produktu a nezáleží ztrátě dat, použijte metodu delete force rychle odebrat trezoru a všechny jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="a058c-107">If you're testing the product and aren't concerned about data loss, use the force delete method to rapidly remove the vault and all its dependencies.</span></span>

> <span data-ttu-id="a058c-108">Příkaz prostředí PowerShell odstraní veškerý obsah v úložišti a není reverzibilního.</span><span class="sxs-lookup"><span data-stu-id="a058c-108">The PowerShell command deletes all the contents of the vault and is not reversible.</span></span>

## <a name="use-powershell-to-force-delete-the-vault"></a><span data-ttu-id="a058c-109">Použití prostředí PowerShell můžete vynutit odstranění trezoru</span><span class="sxs-lookup"><span data-stu-id="a058c-109">Use PowerShell to force delete the vault</span></span> 

<span data-ttu-id="a058c-110">K odstranění trezoru Site Recovery, i když jsou chráněné položky, použijte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="a058c-110">To delete the Site Recovery vault even if there are protected items, use these commands:</span></span>

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault


## <a name="delete-a-site-recovery-vault"></a><span data-ttu-id="a058c-111">Odstranění trezoru Site Recovery</span><span class="sxs-lookup"><span data-stu-id="a058c-111">Delete a Site Recovery vault</span></span> 
<span data-ttu-id="a058c-112">Odstranění trezoru, postupujte podle kroků doporučených pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="a058c-112">To delete the vault, follow the recommended steps for your scenario.</span></span>

### <a name="vmware-vms-to-azure"></a><span data-ttu-id="a058c-113">Virtuální počítače VMware do Azure</span><span class="sxs-lookup"><span data-stu-id="a058c-113">VMware VMs to Azure</span></span>

1. <span data-ttu-id="a058c-114">Odstraňte všechny chráněné virtuální počítače pomocí následujících kroků v [zakažte ochranu pro VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="a058c-114">Delete all protected VMs by following the steps in [Disable protection for a VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="a058c-115">Odstranit všechny zásady replikace pomocí následujících kroků v [odstranit zásadu replikace](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="a058c-115">Delete all replication policies by following the steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3. <span data-ttu-id="a058c-116">Odstranit odkazy na vCenter podle kroků v [odstraňte vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).</span><span class="sxs-lookup"><span data-stu-id="a058c-116">Delete references to vCenter by following the steps in [Delete a vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).</span></span>

4. <span data-ttu-id="a058c-117">Odstraňte konfigurační server pomocí následujících kroků v [vyřadit server konfigurace](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="a058c-117">Delete the configuration server by following the steps in [Decommission a configuration server](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).</span></span>

5. <span data-ttu-id="a058c-118">Odstranění trezoru.</span><span class="sxs-lookup"><span data-stu-id="a058c-118">Delete the vault.</span></span>


### <a name="hyper-v-vms-with-virtual-machine-manager-to-azure"></a><span data-ttu-id="a058c-119">Virtuálních počítačů Hyper-V (s nástrojem Virtual Machine Manager) do Azure</span><span class="sxs-lookup"><span data-stu-id="a058c-119">Hyper-V VMs (with Virtual Machine Manager) to Azure</span></span>
1. <span data-ttu-id="a058c-120">Odstraňte všechny chráněné virtuální počítače pomocí následujících kroků v [zakázat ochranu pro virtuální počítač VMware nebo fyzický server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="a058c-120">Delete all protected VMs by following the steps in [Disable protection for a VMware VM or physical server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="a058c-121">Odstranit všechny zásady replikace pomocí následujících kroků v [odstranit zásadu replikace](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="a058c-121">Delete all replication policies by following the steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3.  <span data-ttu-id="a058c-122">Odstranit odkazy na servery nástroje Virtual Machine Manager pomocí kroků v [zrušit registraci serveru VMM připojené](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).</span><span class="sxs-lookup"><span data-stu-id="a058c-122">Delete references to Virtual Machine Manager servers by following the steps in [Unregister a connected VMM server](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).</span></span>

4.  <span data-ttu-id="a058c-123">Odstranění trezoru.</span><span class="sxs-lookup"><span data-stu-id="a058c-123">Delete the vault.</span></span>

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a><span data-ttu-id="a058c-124">Virtuálních počítačů Hyper-V (bez Virtual Machine Manager) do Azure</span><span class="sxs-lookup"><span data-stu-id="a058c-124">Hyper-V VMs (without Virtual Machine Manager) to Azure</span></span>
1. <span data-ttu-id="a058c-125">Odstraňte všechny chráněné virtuální počítače pomocí následujících kroků v [zakázat ochranu pro virtuální počítač VMware nebo fyzický server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span><span class="sxs-lookup"><span data-stu-id="a058c-125">Delete all protected VMs by following the steps in [Disable protection for a VMware VM or physical server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).</span></span>

2. <span data-ttu-id="a058c-126">Odstranit všechny zásady replikace pomocí následujících kroků v [odstranit zásadu replikace](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span><span class="sxs-lookup"><span data-stu-id="a058c-126">Delete all replication policies by following the steps in [Delete a replication policy](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).</span></span>

3. <span data-ttu-id="a058c-127">Odstranit odkazy na servery Hyper-V pomocí následujících kroků v [hostitele Hyper-V se zrušit registraci](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).</span><span class="sxs-lookup"><span data-stu-id="a058c-127">Delete references to Hyper-V servers by following the steps in [Unregister a Hyper-V host](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).</span></span>

4. <span data-ttu-id="a058c-128">Odstraníte web Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="a058c-128">Delete the Hyper-V site.</span></span>

5. <span data-ttu-id="a058c-129">Odstranění trezoru.</span><span class="sxs-lookup"><span data-stu-id="a058c-129">Delete the vault.</span></span>
