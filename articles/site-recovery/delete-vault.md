---
title: aaaDelete trezoru Site Recovery
description: "Zjistěte, jak toodelete trezoru služby Azure Site Recovery na základě scénáře hello Site Recovery."
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
ms.openlocfilehash: 36db202d8b790bb5d31d1348fb72f51acb5d559c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-site-recovery-vault"></a>Odstranění trezoru Site Recovery
Odstranění trezoru služby Azure Site Recovery můžete zabránit závislosti. Hello akce je nutné tootake lišit v závislosti na scénáři Site Recovery hello: VMware tooAzure tooAzure technologie Hyper-V (s nebo bez System Center Virtual Machine Manager) a Azure Backup. toodelete úložiště použitého v zálohování Azure, najdete v části [odstranit úložiště záloh v Azure](../backup/backup-azure-delete-vault.md).

>[!Important]
>Pokud jste testování hello produktu a nezáleží ztrátě dat, odeberte pomocí hello Vynutit odstranění metoda toorapidly hello trezoru a všechny jeho závislé součásti.

> Hello příkaz prostředí PowerShell odstraní veškerý obsah hello hello úložiště a není reverzibilního.

## <a name="use-powershell-tooforce-delete-hello-vault"></a>Použití prostředí PowerShell tooforce hello trezor odstranit 

hello toodelete trezoru Site Recovery, i když jsou chráněné položky, použijte tyto příkazy:

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault


## <a name="delete-a-site-recovery-vault"></a>Odstranění trezoru Site Recovery 
toodelete hello trezoru, postupujte podle hello doporučené kroky pro váš scénář.

### <a name="vmware-vms-tooazure"></a>Virtuální počítače VMware tooAzure

1. Odstraňte všechny chráněné virtuální počítače pomocí následujících kroků hello v [zakažte ochranu pro VMware](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Odstranit všechny zásady replikace pomocí následujících kroků hello v [odstranit zásadu replikace](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Odstraňte odkazy toovCenter podle následující hello kroky v [odstraňte vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).

4. Odstraňte konfigurační server hello podle následujících kroků hello v [vyřadit server konfigurace](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).

5. Odstraňte trezor hello.


### <a name="hyper-v-vms-with-virtual-machine-manager-tooazure"></a>TooAzure virtuálních počítačů Hyper-V (s nástrojem Virtual Machine Manager)
1. Odstraňte všechny chráněné virtuální počítače pomocí následujících kroků hello v [zakázat ochranu pro virtuální počítač VMware nebo fyzický server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Odstranit všechny zásady replikace pomocí následujících kroků hello v [odstranit zásadu replikace](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3.  Odstranění odkazuje serverům tooVirtual Machine Manager pomocí následujících kroků hello v [zrušit registraci serveru VMM připojené](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server).

4.  Odstraňte trezor hello.

### <a name="hyper-v-vms-without-virtual-machine-manager-tooazure"></a>TooAzure virtuálních počítačů Hyper-V (bez nástroje Virtual Machine Manager)
1. Odstraňte všechny chráněné virtuální počítače pomocí následujících kroků hello v [zakázat ochranu pro virtuální počítač VMware nebo fyzický server](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).

2. Odstranit všechny zásady replikace pomocí následujících kroků hello v [odstranit zásadu replikace](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. Odstranění odkazuje tooHyper-V serverů pomocí následujících kroků hello v [hostitele Hyper-V se zrušit registraci](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).

4. Odstranění lokality hello technologie Hyper-V.

5. Odstraňte trezor hello.
