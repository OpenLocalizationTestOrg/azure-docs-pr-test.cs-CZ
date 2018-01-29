---
title: "Restartování virtuálního počítače v testovacím prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Naučte se restartování virtuálního počítače v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 8460f09e-482f-48ba-a57a-c95fe8afa001
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: v-craic
ms.openlocfilehash: ffa669e2d07a93e9e5071ac8aab81ec14ea24eab
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Restartování virtuálního počítače v testovacím prostředí v Azure DevTest Labs
Můžete rychle a snadno restartování virtuálního počítače v DevTest Labs podle kroků v tomto článku. Vezměte v úvahu následující před restartováním virtuálního počítače:

- Virtuální počítač musí být spuštěn pro funkci restartování, aby byl povolen.
- Pokud je uživatel připojen k spuštění virtuálního počítače při vykonávání restartování, se musíte znovu připojit ke k virtuálnímu počítači po jeho spuštění zálohování.
- Pokud se aplikuje artefakt při dalším spuštění virtuálního počítače, zobrazí se upozornění nemusí použijí artefakt. 

    ![Upozornění při restartování při použití artefaktů](./media/devtest-lab-restart-vm/devtest-lab-restart-vm-apply-artifacts.png)


   > [!NOTE]
   > Pokud při použití artefakt zastaví virtuální počítač, můžete funkci restartování virtuálního počítače jako potenciální způsob, jak problém vyřešit.
   >
   >

## <a name="steps-to-restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Kroky pro restartování virtuálního počítače v testovacím prostředí v Azure DevTest Labs
1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.
1. Ze seznamu labs vyberte testovací prostředí, která obsahuje virtuální počítač, který chcete restartovat.  
1. V levém panelu, vyberte **Moje virtuálních počítačů**. 
1. Ze seznamu virtuálních počítačů vyberte spuštění virtuálního počítače.
1. V horní části podokna Správa virtuálních počítačů, vyberte **restartujte**.  

    ![Restartujte počítač tlačítko](./media/devtest-lab-restart-vm/devtest-lab-restart-vm.png)

1. Monitorování stavu restartování výběrem **oznámení** horním rohu okna.

    ![Zobrazení stavu restartování virtuálního počítače](./media/devtest-lab-restart-vm/devtest-lab-restart-notification.png)

Spuštění virtuálního počítače můžete také restartovat jeho třemi tečkami (...), vyberte v seznamu **Moje virtuálních počítačů**.

![Restartování virtuálního počítače prostřednictvím symbol tří teček](./media/devtest-lab-restart-vm/devtest-lab-restart-elipses.png)

## <a name="next-steps"></a>Další postup
* Po jeho restartování, je možné znovu připojit k virtuálnímu počítači tak, že vyberete **Connect** na jeho podokně Správa.
* Prozkoumejte [šablony galerii pro rychlý start DevTest Labs Azure Resource Manager](https://github.com/Azure/azure-devtestlab/tree/master/Samples)
