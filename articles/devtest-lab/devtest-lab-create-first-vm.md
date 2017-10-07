---
title: "aaaCreate vaše první virtuální počítač v testovacím prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak toocreate prvním virtuálním počítači v testovacím prostředí v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2017
ms.author: tarcher
ms.openlocfilehash: 4c3257efca9be6fdd190eaac1db731464e07fcfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Vytvoření vaší první virtuální počítač v testovacím prostředí v Azure DevTest Labs

Pokud jste původně přístup DevTest Labs a chcete toocreate vaše první virtuální počítač, bude pravděpodobně uděláte pomocí předem načtený [image základní marketplace](devtest-lab-configure-marketplace-images.md). Později na taky budete moct toochoose z [vlastní image a vzorce](devtest-lab-add-vm.md) při vytváření více virtuálních počítačů. 

Tento kurz vás provede pomocí hello Azure portálu tooadd první testovacího virtuálního počítače tooa prostředí v DevTest Labs.

## <a name="steps-tooadd-your-first-vm-tooa-lab-in-azure-devtest-labs"></a>Kroky tooadd první testovacího prostředí tooa virtuálních počítačů v Azure DevTest Labs
1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
1. Ze seznamu hello labs vyberte hello testovacího prostředí, ve kterém chcete toocreate hello virtuálních počítačů.  
1. V testovacím hello **přehled** vyberte **+ přidat**.  

    ![Virtuální počítač tlačítko Přidat](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Na hello **zvolte na základní** okně, vyberte bitovou kopii na trhu hello virtuálních počítačů.
1. Na hello **virtuálního počítače** okno, zadejte název pro nový virtuální počítač hello v hello **název virtuálního počítače** textové pole.

    ![Okno prostředí virtuálních počítačů](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. Zadejte **uživatelské jméno** , jsou udělena oprávnění správce na virtuálním počítači hello.  
1. Zadejte heslo do hello textové pole s názvem bez přípony **zadejte hodnotu**.
1. Hello **typ disku virtuálního počítače** Určuje, jaký typ úložiště disku je povoleno pro hello virtuálních počítačů v testovacím prostředí hello.
1. Vyberte **velikost virtuálního počítače** a vyberte jednu z hello předdefinované položky, které zadejte hello jader procesoru, velikost paměti RAM a velikost pevného disku hello toocreate hello virtuálních počítačů.
1. Vyberte **artefakty** a - z hello seznam artefakty - vyberte a nakonfigurujte hello artefakty, že chcete tooadd toohello základní image.
    **Poznámka:** nové Labs tooDevTest nebo toohello konfigurace artefaktů, najdete v [přidat existující artefaktů tooa virtuálních počítačů](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) části a vraťte se sem po dokončení.
1. Vyberte **vytvořit** tooadd hello zadaný testovacím toohello virtuálních počítačů.

   Hello testovacím zobrazuje hello stav vytvoření hello VM - nejprve jako **vytváření**, pak jako **systémem** po hello virtuálního počítače byla spuštěna.

## <a name="next-steps"></a>Další kroky
* Jednou hello vytvořil virtuální počítač, můžete připojit toohello virtuálních počítačů tak, že vyberete **Connect** v okně hello Virtuálního počítače.
* Podívejte se na [přidání testovacího prostředí virtuálních počítačů tooa](devtest-lab-add-vm.md) podrobnější informace o přidávání dalších virtuálních počítačů ve svém testovacím prostředí.
* Prozkoumejte hello [Galerie šablon DevTest Labs Azure Resource Manager QuickStart](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).
