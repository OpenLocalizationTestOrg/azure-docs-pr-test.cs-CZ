---
title: "aaaBoot diagnostiky pro virtuální počítače s Linuxem v Azure | Microsoft dokumentů"
description: "Přehled hello dvě funkce ladění pro virtuální počítače s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: delhan
ms.openlocfilehash: d355d512de09d2f1d7a2718e3db3fb99c9dd9e24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-boot-diagnostics-tootroubleshoot-linux-virtual-machines-in-azure"></a>Jak toouse spouštění diagnostiky tootroubleshoot Linuxové virtuální počítače v Azure

V Azure je teď dostupná podpora dvou funkcí ladění: Podpora výstupu konzoly a snímku obrazovky pro model nasazení virtuálních počítačů Azure Resource Manager. 

Při zpětném přepnutí vlastní image tooAzure nebo i spouštění jeden z Image platformy hello, může být mnoha důvodů, proč se virtuální počítač získá do jiných spouštěcího stavu. Tyto funkce povolit tooeasily jste diagnostiku a obnovení virtuálních počítačů v případě selhání spuštění.

Pro virtuální počítače s Linuxem, můžete snadno zobrazit výstup hello protokolu konzoly z hello portálu:

![portál Azure](./media/boot-diagnostics/screenshot1.png)
 
Pro systém Windows a virtuální počítače s Linuxem, je však Azure také umožňuje toosee snímek hello virtuálních počítačů ze hypervisoru hello:

![Chyba](./media/boot-diagnostics/screenshot2.png)

Obě tyto funkce jsou podporované pro virtuální počítače Azure ve všech oblastech. Všimněte si, snímky a výstup může trvat až minut tooappear too10 ve vašem účtu úložiště.

## <a name="common-boot-errors"></a>Běžné chyby spuštění

- [Problémy systému souborů](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [Problémy jádra](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [FSTAB chyby](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Povolení diagnostiky na novém virtuálním počítači
1. Při vytváření nového virtuálního počítače z hello portál Preview, vyberte hello **Azure Resource Manager** z rozevíracího seznamu model nasazení hello:
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. Nakonfigurujte hello monitorování možnost tooselect hello účet úložiště kam chcete tooplace tyto diagnostické soubory.
 
    ![Vytvoření virtuálního počítače](./media/boot-diagnostics/screenshot4.jpg)

3. Pokud nasazujete z šablony Azure Resource Manager, přejděte tooyour prostředek virtuálního počítače a připojte oddíl profilu hello diagnostiky. Mějte na paměti, hlavičkou verze rozhraní API "2015-06-15" hello toouse.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Hello diagnostického profilu můžete účet úložiště hello tooselect místo tooput tyto protokoly.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

## <a name="update-an-existing-virtual-machine"></a>Aktualizace stávajícího virtuálního počítače

Diagnostika spouštění tooenable prostřednictvím hello portálu, můžete také aktualizovat existující virtuální počítač prostřednictvím portálu hello. Diagnostika spouštění vyberte hello možnost a uložte. Restartujte vliv tootake hello virtuálních počítačů.

![Aktualizace stávajícího virtuálního počítače](./media/boot-diagnostics/screenshot5.png)