---
title: "aaaAdd vymahatelných testovacího prostředí tooa virtuálních počítačů v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak tooadd testovacího prostředí tooa vymahatelných virtuálního počítače v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: tarcher
ms.openlocfilehash: fe6385ae2e59b9636b82aec250dc3a1f8a40ba5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a>Přidání testovacího prostředí vymahatelných tooa virtuálních počítačů v Azure DevTest Labs
Přidání testovacího prostředí vymahatelných tooa virtuálních počítačů v podobným způsobem toohow jste [přidat standardní virtuální počítač](devtest-lab-add-vm.md) – z *základní* to znamená buď [vlastní image](devtest-lab-create-template.md), [vzorec](devtest-lab-manage-formulas.md), nebo [Marketplace image](devtest-lab-configure-marketplace-images.md). Tento kurz vás provede procesem pomocí hello Azure portálu tooadd vymahatelných testovacího virtuálního počítače tooa prostředí v DevTest Labs a zobrazuje hello proces že uživatel bude postupovat tooclaim hello virtuálních počítačů.

> [!NOTE]
> Pokud nasadíte virtuální počítače testovacího prostředí prostřednictvím [šablon Azure Resource Manageru](devtest-lab-create-environment-from-arm.md), můžete vytvořit virtuální počítače vymahatelných nastavení hello **allowClaim** vlastnost tootrue v části Vlastnosti hello.
>
>

## <a name="steps-tooadd-a-claimable-vm-tooa-lab-in-azure-devtest-labs"></a>Kroky tooadd vymahatelných testovacího prostředí tooa virtuálních počítačů v Azure DevTest Labs
1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.
1. Ze seznamu hello labs, vyberte hello testovacího prostředí do kterých chcete toocreate hello vymahatelných virtuálních počítačů.  
1. V testovacím hello **přehled** vyberte **+ přidat**.  

    ![Virtuální počítač tlačítko Přidat](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Na hello **zvolte na základní** okně vyberte základ pro hello virtuálních počítačů.
1. Na hello **virtuálního počítače** okno, zadejte název pro nový virtuální počítač hello v hello **název virtuálního počítače** textové pole.

    ![Okno prostředí virtuálních počítačů](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Zadejte **uživatelské jméno** , jsou udělena oprávnění správce na virtuálním počítači hello.  
1. Pokud chcete, aby toouse heslo uložené v vaše [tajný úložiště](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), vyberte **použít uložené tajný klíč**a zadejte hodnotu klíče, která odpovídá tooyour tajný klíč (heslo). Jinak, zadejte heslo do hello textové pole s názvem bez přípony **zadejte hodnotu**.
1. Hello **typ disku virtuálního počítače** Určuje, jaký typ úložiště disku je povoleno pro hello virtuálních počítačů v testovacím prostředí hello.
1. Vyberte **velikost virtuálního počítače** a vyberte jednu z hello předdefinované položky, které zadejte hello jader procesoru, velikost paměti RAM a velikost pevného disku hello toocreate hello virtuálních počítačů.
1. Vyberte **artefakty** a ze seznamu hello artefaktů, vyberte a nakonfigurujte hello artefakty, že chcete tooadd toohello základní image. Pokud je nový Labs tooDevTest nebo toohello konfigurace artefaktů, najdete v [přidat existující artefaktů tooa virtuálních počítačů](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) části a vraťte se sem po dokončení.
1. Vyberte **upřesňující nastavení** tooconfigure hello síť virtuálních počítačů na možnosti možnosti a vypršení platnosti. V části **deklarace identity možnosti**, zvolte **Ano** toomake hello počítač vymahatelných.

  ![Zvolte toomake hello vymahatelných virtuálních počítačů.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Chcete-li tooview nebo zkopírujte hello šablony Azure Resource Manageru, najdete v toohello [šablony uložit Azure Resource Manageru](devtest-lab-add-vm.md#save-azure-resource-manager-template) části a sem vraťte po dokončení.
1. Vyberte **vytvořit** tooadd hello zadaný testovacím toohello virtuálních počítačů.
1. Hello testovacím zobrazuje hello stav vytvoření hello VM - nejprve jako **vytváření**, pak jako **systémem** po hello virtuálního počítače byla spuštěna.


## <a name="using-a-claimable-vm"></a>Pomocí vymahatelných virtuálního počítače

Uživatel může deklarace identity žádné virtuální počítače ze seznamu hello "Vymahatelných virtuální počítače" jedním z těchto kroků:

* Ze seznamu hello "Vymahatelných virtuální počítače" hello dolní části okna Přehled hello laboratoře, klikněte pravým tlačítkem na jednu z hello virtuální počítače v seznamu hello a zvolte **deklarace identity počítače**.

 ![Požádat o konkrétní vymahatelných virtuální počítač.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* Hello horní části hello **přehled** okně zvolte **všechny deklarace**. Náhodné virtuální počítač je přiřazen seznamu hello vymahatelných virtuálních počítačů.

 ![Požádat o žádné vymahatelných virtuální počítače.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Poté, co uživatel deklarací virtuálního počítače, je do jejich seznam "Moje virtuální počítače" Přesunout nahoru a již není vymahatelných jiným uživatelem.

## <a name="next-steps"></a>Další kroky
* Jednou hello vytvořil virtuální počítač, můžete připojit toohello virtuálních počítačů tak, že vyberete **Connect** v okně hello Virtuálního počítače.
* Prozkoumejte hello [Galerie šablon DevTest Labs Azure Resource Manager rychlý start](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)
