---
title: "aaaAttach tooa datového disku virtuálního počítače s Linuxem | Microsoft Docs"
description: "Jak tooattach nových nebo existujících dat disku tooa virtuálního počítače s Linuxem v Azure pomocí portálu hello hello modelu nasazení Resource Manager."
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
ms.openlocfilehash: 069c3c6f5e71a8c495342e6d0c6f3d92c4ed5053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-data-disk-tooa-linux-vm-in-hello-azure-portal"></a>Jak tooattach datový disk tooa virtuálního počítače s Linuxem v hello portálu Azure
Tento článek ukazuje, jak tooattach nové i stávající disky tooa Linux virtuálnímu počítači prostřednictvím hello portálu Azure. Můžete také [připojit tooa disku data virtuálního počítače s Windows v portálu Azure hello](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Toouse můžete zvolit buď disky Azure spravované nebo nespravované disky. Spravované disky jsou zpracovávány hello platformy Azure a nevyžadují, aby všechny přípravné nebo umístění toostore je. Nespravované disky se vyžaduje účet úložiště a mají některé [kvóty a omezení, které se vztahují](../../azure-subscription-service-limits.md#storage-limits). Další informace o službě Azure Managed Disks najdete v tématu [Přehled služby Azure Managed Disks](../windows/managed-disks-overview.md).

Než připojíte tooyour disky virtuálního počítače, projděte si tyto tipy:

* velikost Hello hello virtuálního počítače určuje, kolik datových disků můžete připojit. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* toouse storage úrovně Premium, budete potřebovat DS-series nebo GS-series virtuálního počítače. Pomocí těchto virtuálních počítačů můžete použít Premium i standardní disky. Storage úrovně Premium je k dispozici v určité oblasti. Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Disky připojené toovirtual počítače jsou ve skutečnosti soubory VHD uložených v Azure. Podrobnosti najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="find-hello-virtual-machine"></a>Najít hello virtuální počítač
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V nabídce centra hello, klikněte na tlačítko **virtuální počítače**.
3. Vyberte virtuální počítač hello hello seznamu.
4. toohello virtuální počítače, v okně **Essentials**, klikněte na tlačítko **disky**.
   
    ![Otevřete nastavení disku](./media/attach-disk-portal/find-disk-settings.png)

Pokračujte podle pokynů pro připojení buď [spravovaných disků na](#use-azure-managed-disks) nebo [nespravované disku](#use-unmanaged-disks).

## <a name="use-azure-managed-disks"></a>Použití spravovaných disky systému Azure

### <a name="attach-a-new-disk"></a>Připojit nový disk

1. Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. Klikněte na rozevírací nabídku hello **název** a vyberte **vytvořit disk**:

    ![Vytvoření Azure spravovaných disků](./media/attach-disk-portal/create-new-md.png)

3. Zadejte název pro spravované disk. Zkontrolujte hello výchozí nastavení, aktualizovat podle potřeby a pak klikněte na tlačítko **vytvořit**.
   
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/create-new-md-settings.png)

4. Klikněte na tlačítko **Uložit** toocreate hello spravované disku a aktualizaci konfigurace virtuálního počítače hello:

   ![Uložit nový Disk spravované Azure](./media/attach-disk-portal/confirm-create-new-md.png)

5. Jakmile Azure vytvoří hello disk a připojí jej toohello virtuálního počítače, hello nového disku je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**. Prostředek nejvyšší úrovně jsou spravované disky hello disk se zobrazí v kořenu hello hello skupiny prostředků:

   ![Disk Azure spravované ve skupině prostředků](./media/attach-disk-portal/view-md-resource-group.png)

### <a name="attach-an-existing-disk"></a>Připojení stávajícího disku
1. Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. Klikněte na rozevírací nabídku hello **název** tooview seznam existujících spravované tooyour dostupné disky předplatného Azure. Vyberte hello spravované tooattach disku:

   ![Připojit stávající Disk spravované Azure](./media/attach-disk-portal/select-existing-md.png)

3. Klikněte na tlačítko **Uložit** tooattach hello existující spravované disku a aktualizaci konfigurace virtuálního počítače hello:
   
   ![Uložte aktualizace disku spravované Azure](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. Po Azure připojí hello disku toohello virtuálního počítače, je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.

## <a name="use-unmanaged-disks"></a>Použití nespravovaného disky

### <a name="attach-a-new-disk"></a>Připojit nový disk

1. Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. Zkontrolujte hello výchozí nastavení, aktualizovat podle potřeby a pak klikněte na tlačítko **OK**.
   
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-new.png)
3. Jakmile Azure vytvoří hello disk a připojí jej toohello virtuálního počítače, hello nového disku je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.

### <a name="attach-an-existing-disk"></a>Připojení stávajícího disku
1. Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. V části **připojit stávající disk**, klikněte na tlačítko **souboru virtuálního pevného disku**.
   
   ![Připojit stávající disk](./media/attach-disk-portal/attach-existing.png)
3. V části **účty úložiště**, vyberte účet hello a kontejner, který obsahuje soubor VHD hello.
   
   ![Vyhledejte umístění virtuálního pevného disku](./media/attach-disk-portal/find-storage-container.png)
4. Vyberte soubor VHD hello.
5. V části **připojit stávající disk**, právě vybraný soubor hello je uveden v části **souboru virtuálního pevného disku**. Klikněte na **OK**.
6. Po Azure připojí hello disku toohello virtuálního počítače, je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.


## <a name="next-steps"></a>Další kroky
Po přidání disku hello potřebujete tooprepare ho. Další informace najdete v tématu [postupy: Inicializace nový datový disk v systému Linux](add-disk.md).
