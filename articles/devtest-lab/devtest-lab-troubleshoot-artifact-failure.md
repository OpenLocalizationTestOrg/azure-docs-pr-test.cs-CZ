---
title: "aaaDiagnose artefaktů selhání ve virtuálním počítači Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot artefaktů selhání v DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: tarcher
ms.openlocfilehash: 40b3cea72cf071cc5d9a6d002d309d923c3d3084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-artifact-failures-in-hello-lab"></a>Diagnostikování selhání artefaktů v testovacím prostředí hello 
Po vytvoření artefakt, můžete zkontrolovat toosee, pokud ji byla úspěšná nebo neúspěšná. Protokoly artefaktů v DevTest Labs poskytují informace, které můžete použít toodiagnose chybu artefaktů. Zobrazí se informace protokolu hello artefaktů pro virtuální počítač s Windows několik různými způsoby.

> [!NOTE]
> tooensure, že chyby jsou správně identifikovat a vysvětlení, je důležité, zda je správně strukturu této hello artefaktů. Informace o tom, jak toocorrectly Vytvořit artefakt najdete v tématu [vytvoření vlastních artefaktů](devtest-lab-artifact-author.md). A toosee příkladem správně strukturovaný artefaktů, podívejte se na to [typy parametrů testovací](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artefaktů.

## <a name="troubleshoot-artifact-failures-using-hello-azure-portal"></a>Řešení potíží s artefaktů selhání pomocí hello portálu Azure
toouse hello Azure portálu toodiagnose selhání během vytváření artefaktů, postupujte takto:

1. Hello seznamu prostředků vyberte testovacího prostředí.

2. Vyberte virtuální počítač Windows, který zahrnuje hello artefaktů chcete tooinvestigate hello.

3. V levém panelu hello pod **Obecné**, zvolte **artefakty**. Artefakty související s tohoto virtuálního počítače se zobrazí seznam, která udává název hello hello artefaktů a jeho stav.

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. Zvolte artefakt zobrazující stav **se nezdařilo**. Hello artefaktů otevře a zobrazí zprávu rozšíření, který obsahuje podrobnosti o selhání hello artefakt hello.

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-hello-vm"></a>Řešení potíží s selhání artefaktů z v rámci hello virtuálních počítačů
tooview protokoly hello artefaktů z hello virtuálním počítači, postupujte takto:

1. Přihlaste se toohello virtuální počítač, který obsahuje hello artefaktů chcete toodiagnose.

2. Přejděte tooC:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status kde "1.9 je hello číslo verze rozšíření na straně klienta.

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. Otevřete hello **stav** souboru tooview informace, že pomáhá diagnostikování selhání artefaktů tohoto virtuálního počítače.




[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související
* [Připojení k tooexisting virtuálního počítače domény AD pomocí šablony správce prostředků v Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[přidání testovacího prostředí tooa úložiště Git](devtest-lab-add-artifact-repo.md).

