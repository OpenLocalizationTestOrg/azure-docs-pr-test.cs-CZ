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
# <a name="manage-a-process-server-running-in-azure-resource-manager"></a>Správa procesu serveru běží v Azure (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](./site-recovery-vmware-setup-azure-ps-resource-manager.md)
> * [Classic](./site-recovery-vmware-setup-azure-ps-classic.md)

Během navrácení služeb po obnovení je doporučeno toodeploy procesní server v Azure při vysoké latenci mezi hello Azure Virtual Network a v místní síti. Tento článek popisuje, jak můžete nastavit, konfigurovat a spravovat servery proces hello v Azure.

> [!NOTE]
> Tento článek je toobe použít, pokud jste použili **Resource Manager** jako model nasazení hello hello virtuálních počítačů během převzetí služeb při selhání. Pokud jste použili **Classic** jako model nasazení hello, postupujte podle kroků hello v [jak tooset až & nakonfigurovat procesový server navrácení služeb po obnovení (klasické)](./site-recovery-vmware-setup-azure-ps-classic.md)

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>Nasadit procesový server na platformě Azure
1. V hello trezoru > **infrastruktura Site Recovery** (pod nadpisem "Manage" hello) > **konfigurační servery** (pod nadpisem "Pro VMware a fyzické počítače"), vyberte hello konfigurační server.
2. Na stránce s podrobnostmi o konfigurační Server hello otevřeném klikněte na "+ zpracovat serveru.

  ![Přidat server galerie procesu](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps.png)

3.  Na hello **přidat procesový server** stránky, vyberte hello následující hodnoty

  ![Přidat položku Galerie procesového serveru](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-1.png)
|**Název pole**|**Hodnota**|
|-|-|
|Vyberte, kam chcete toodeploy, procesového serveru|Vyberte hodnotu hello **nasadit procesový server navrácení služeb po obnovení v Azure** |
|Předplatné|Vyberte hello předplatné Azure, kde převzal hello virtuální počítače|
|Skupina prostředků|Můžete vytvořit skupinu prostředků toodeploy tento proces server nebo zvolte toodeploy hello procesový server v existující skupinu prostředků.|
|Umístění|Vyberte datové centrum Azure hello do virtuálních počítačů hello kde převzetí služeb při selhání do|
|Síť Azure|Vyberte hello virtuální Network(VNet) Azure, který hello virtuální počítače kde převzetí služeb při selhání do. Pokud se při selhání virtuálního počítače do více virtuálními sítěmi Azure budete potřebovat procesový server nasadit na virtuální síť|

4. Vyplňte hello zbytek hello vlastnosti hello procesového serveru

  ![Přidejte procesový server souhrn](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-2.png)
|**Název pole**|**Hodnota**|
|-|-|
|Název serveru|Zobrazovaný název a název hostitele pro virtuální počítač serveru procesu|
| Uživatelské jméno|Uživatelské jméno, který se stává správcem na tomto virtuálním počítači|
|Účet úložiště|Název účtu úložiště, kde jsou umístěny hello virtuálního počítače virtuální disk hello|
|Podsíť|podsíť Hello hello virtuální síť Azure toowhich hello virtuálního počítače je připojen.|
| IP adresa|IP adresy, které chcete hello proces serveru tooassume, po spuštění|
5. Klikněte na tlačítko toostart tlačítko OK hello nasazení virtuálního počítače hello procesu serveru.

> [!NOTE]
> možné toouse toobe tento procesový server navrácení služeb po obnovení, je nutné tooregister její hello místní konfigurační server.

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a>Registrace hello procesu serveru (spuštění v Azure) tooa konfigurační Server (s místní)

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a>Upgrade verze toolatest hello procesového serveru.

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>Zrušení registrace hello procesového serveru (spuštění v Azure) z konfigurace serveru (místní)

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
