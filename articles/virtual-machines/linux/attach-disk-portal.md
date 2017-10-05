---
title: "Připojit datový disk do virtuálního počítače s Linuxem | Microsoft Docs"
description: "Tom, jak připojit nové nebo stávající datový disk do virtuálního počítače s Linuxem v portálu Azure pomocí modelu nasazení Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: cynthn
ms.openlocfilehash: 1599ee241c3d9fb3623ebd89ae30f2795cae1930
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Tom, jak připojit datový disk do virtuálního počítače s Linuxem na portálu Azure
Tento článek ukazuje, jak připojit nové i stávající disky pro virtuální počítač s Linuxem pomocí portálu Azure. Můžete také [připojit datový disk k virtuálnímu počítači Windows na portálu Azure](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Můžete použít buď disky Azure spravované nebo nespravované disky. Spravované disky jsou zpracovávány platformy Azure a nevyžadují, aby všechny přípravné nebo umístění pro uložení. Nespravované disky se vyžaduje účet úložiště a mají některé [kvóty a omezení, které se vztahují](../../azure-subscription-service-limits.md#storage-limits). Další informace o službě Azure Managed Disks najdete v tématu [Přehled služby Azure Managed Disks](../windows/managed-disks-overview.md).

Než připojíte k virtuálnímu počítači disky, projděte si tyto tipy:

* Velikost virtuálního počítače určuje, kolik datových disků můžete připojit. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Chcete-li používat úložiště úrovně Premium, je třeba DS-series nebo GS-series virtuálního počítače. Pomocí těchto virtuálních počítačů můžete použít Premium i standardní disky. Storage úrovně Premium je k dispozici v určité oblasti. Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Disky připojené k virtuální počítače jsou ve skutečnosti soubory VHD uložených v Azure. Podrobnosti najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="find-the-virtual-machine"></a>Najít virtuální počítač
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. V nabídce centra klikněte na **Virtuální počítače**.
3. Ze seznamu vyberte virtuální počítač.
4. K virtuální počítače, v okně **Essentials**, klikněte na tlačítko **disky**.
   
    ![Otevřete nastavení disku](./media/attach-disk-portal/find-disk-settings.png)

Pokračujte podle pokynů pro připojení buď [spravovaných disků na](#use-azure-managed-disks) nebo [nespravované disku](#use-unmanaged-disks).

## <a name="use-azure-managed-disks"></a>Použití spravovaných disky systému Azure

### <a name="attach-a-new-disk"></a>Připojit nový disk

1. Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. Klikněte na rozevírací nabídku pro **název** a vyberte **vytvořit disk**:

    ![Vytvoření Azure spravovaných disků](./media/attach-disk-portal/create-new-md.png)

3. Zadejte název pro spravované disk. Zkontrolujte výchozí nastavení, aktualizovat podle potřeby a pak klikněte na tlačítko **vytvořit**.
   
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/create-new-md-settings.png)

4. Klikněte na tlačítko **Uložit** k vytvoření spravovaného disku a aktualizaci konfigurace virtuálního počítače:

   ![Uložit nový Disk spravované Azure](./media/attach-disk-portal/confirm-create-new-md.png)

5. Jakmile Azure vytvoří disk a připojí jej k virtuálnímu počítači, na nový disk, je uvedena ve nastavení disku virtuálního počítače v části **datových disků**. Nejvyšší úrovně prostředků jsou spravovaných disků disk se zobrazí v kořenovém adresáři skupiny prostředků:

   ![Disk Azure spravované ve skupině prostředků](./media/attach-disk-portal/view-md-resource-group.png)

### <a name="attach-an-existing-disk"></a>Připojení stávajícího disku
1. Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. Klikněte na rozevírací nabídku pro **název** zobrazíte seznam existující spravovaných disků, které jsou přístupné pro vaše předplatné Azure. Vyberte spravované disk připojit:

   ![Připojit stávající Disk spravované Azure](./media/attach-disk-portal/select-existing-md.png)

3. Klikněte na tlačítko **Uložit** připojit stávající disk spravované a aktualizaci konfigurace virtuálního počítače:
   
   ![Uložte aktualizace disku spravované Azure](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. Po Azure připojí disk k virtuálnímu počítači, je uvedena v nastavení disku virtuálního počítače v části **datových disků**.

## <a name="use-unmanaged-disks"></a>Použití nespravovaného disky

### <a name="attach-a-new-disk"></a>Připojit nový disk

1. Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. Zkontrolujte výchozí nastavení, aktualizovat podle potřeby a pak klikněte na tlačítko **OK**.
   
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-new.png)
3. Jakmile Azure vytvoří disk a připojí jej k virtuálnímu počítači, na nový disk, je uvedena ve nastavení disku virtuálního počítače v části **datových disků**.

### <a name="attach-an-existing-disk"></a>Připojení stávajícího disku
1. Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. V části **připojit stávající disk**, klikněte na tlačítko **souboru virtuálního pevného disku**.
   
   ![Připojit stávající disk](./media/attach-disk-portal/attach-existing.png)
3. V části **účty úložiště**, vyberte účet a kontejner, který obsahuje soubor VHD.
   
   ![Vyhledejte umístění virtuálního pevného disku](./media/attach-disk-portal/find-storage-container.png)
4. Vyberte soubor VHD.
5. V části **připojit stávající disk**, právě vybraný soubor je uveden v části **souboru virtuálního pevného disku**. Klikněte na **OK**.
6. Po Azure připojí disk k virtuálnímu počítači, je uvedena v nastavení disku virtuálního počítače v části **datových disků**.


## <a name="next-steps"></a>Další kroky
Po přidání disku budete muset připravit k použití. Další informace najdete v tématu [postupy: Inicializace nový datový disk v systému Linux](add-disk.md).
