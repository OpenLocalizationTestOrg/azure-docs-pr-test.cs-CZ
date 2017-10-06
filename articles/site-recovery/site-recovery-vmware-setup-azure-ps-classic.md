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
# <a name="manage-a-process-server-running-in-azure-classic"></a>Správa procesu serveru běží v Azure (klasický)
> [!div class="op_single_selector"]
> * [Portál Azure Classic](./site-recovery-vmware-setup-azure-ps-classic.md)
> * [Resource Manager](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

Během navrácení služeb po obnovení je doporučeno toodeploy procesní Server v Azure při vysoké latenci mezi hello Azure Virtual Network a v místní síti. Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat servery proces hello v Azure.

> [!NOTE]
> Tento článek je toobe použít, pokud jste použili Classic jako model nasazení hello hello virtuálních počítačů během převzetí služeb při selhání. Pokud jste použili správce prostředků jako hello kroky hello postupujte podle modelu nasazení v [jak tooset až & nakonfigurovat procesový Server navrácení služeb po obnovení (Resource Manager)](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [site-recovery-vmware-process-server-prereq](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>Nasadit procesový Server na platformě Azure

1. V Azure Marketplace, vytvoření virtuálního počítače pomocí hello **Microsoft Azure Site obnovení proces serveru V2** </br>
    ![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)
2. Ujistěte se, že vyberete model nasazení hello jako **Classic** </br>
  ![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)
3. V Průvodci virtuálního počítače vytvořit hello > Základní nastavení, ujistěte se, vyberte předplatné a umístění toowhere hello převzal hello virtuálních počítačů.</br>
  ![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)
4. Ujistěte se, že hello virtuální počítač je připojen toohello Azure Virtual Network toowhich hello při selhání virtuálního počítače je připojen.</br>
  ![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)
5. Po zřízení hello procesového serveru virtuálního počítače, potřebujete toolog v a zaregistrovat ji pomocí hello konfigurační Server.

> [!NOTE]
> možné toouse toobe tento procesový Server navrácení služeb po obnovení, je nutné tooregister její hello místní konfigurační server.

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a>Registrace hello procesového serveru (spuštění v Azure) tooa konfigurační Server (s místní)

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a>Upgrade verze toolatest procesový Server hello.

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>Zrušení registrace hello procesového serveru (spuštění v Azure) z konfigurace serveru (místní)

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
