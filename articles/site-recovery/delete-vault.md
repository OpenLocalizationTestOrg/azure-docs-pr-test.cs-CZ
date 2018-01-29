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
ms.openlocfilehash: 77bc7e068629c5dbf650b39844912b1b7a6155b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="delete-a-site-recovery-vault"></a>Odstranění trezoru Site Recovery
Odstranění trezoru služby Azure Site Recovery můžete zabránit závislosti. Akce, které je třeba provést lišit v závislosti na scénáři Site Recovery: VMware do Azure, technologie Hyper-V (s nebo bez System Center Virtual Machine Manager) a Azure, Azure Backup. Pokud chcete odstranit úložiště použitého v zálohování Azure, najdete v části [odstranit úložiště záloh v Azure](../backup/backup-azure-delete-vault.md).



## <a name="delete-a-site-recovery-vault"></a>Odstranění trezoru Site Recovery 
Odstranění trezoru, postupujte podle kroků doporučených pro váš scénář.

### <a name="vmware-vms-to-azure"></a>Virtuální počítače VMware do Azure

1. Odstraňte všechny chráněné virtuální počítače pomocí následujících kroků v [zakažte ochranu pro VMware](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).

2. Odstranit všechny zásady replikace pomocí následujících kroků v [odstranit zásadu replikace](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Odstranit odkazy na vCenter podle kroků v [odstraňte vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).

4. Odstraňte konfigurační server pomocí následujících kroků v [vyřadit server konfigurace](site-recovery-vmware-to-azure-manage-configuration-server.md##delete-or-unregister-a-configuration-server).

5. Odstranění trezoru.


### <a name="hyper-v-vms-with-virtual-machine-manager-to-azure"></a>Virtuálních počítačů Hyper-V (s nástrojem Virtual Machine Manager) do Azure
1. Odstraňte všechny chráněné virtuální počítače pomocí následujících kroků v[zakažte ochranu pro replikaci virtuálního počítače technologie Hyper-V do Azure pomocí System Center VMM do Azure scénáře](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario).

2. Zrušit přidružení & procházením svůj trezor odstranit všechny zásady replikace -> **infrastruktura Site Recovery** -> **pro System Center VMM** -> **replikace Zásady**

3.  Odstranit odkazy na servery nástroje Virtual Machine Manager pomocí kroků v [zrušit registraci serveru VMM připojené](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server).

4.  Odstranění trezoru.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Virtuálních počítačů Hyper-V (bez Virtual Machine Manager) do Azure
1. Odstraňte všechny chráněné virtuální počítače pomocí následujících kroků v [zakažte ochranu pro virtuální počítač technologie Hyper-V (Hyper-V do Azure)](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure).

2. Zrušit přidružení & procházením svůj trezor odstranit všechny zásady replikace -> **infrastruktura Site Recovery** -> **pro weby technologie Hyper-V** -> **zásady replikace**

3. Odstranit odkazy na servery Hyper-V pomocí následujících kroků v [hostitele Hyper-V se zrušit registraci](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).

4. Odstraníte web Hyper-V.

5. Odstranění trezoru.


## <a name="use-powershell-to-force-delete-the-vault"></a>Použití prostředí PowerShell můžete vynutit odstranění trezoru 

> [!Important]
> Pokud jste testování produktu a nezáleží ztrátě dat, použijte metodu delete force rychle odebrat trezoru a všechny jeho závislé součásti.
> Příkaz prostředí PowerShell odstraní veškerý obsah v úložišti a je **není reverzibilního**.

K odstranění trezoru Site Recovery, i když jsou chráněné položky, použijte tyto příkazy:

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault