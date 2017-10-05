---
title: "Diagnostikování selhání artefaktů ve virtuálním počítači Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak vyřešit selhání artefaktů v DevTest Labs"
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
ms.openlocfilehash: e4f2946d0ba0756f36622aded0e8594acabb9527
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="diagnose-artifact-failures-in-the-lab"></a>Diagnostikování selhání artefaktů v testovacím prostředí 
Po vytvoření artefakt, můžete zkontrolovat v tématu, pokud byla úspěšná nebo neúspěšná. Protokoly artefaktů v DevTest Labs poskytují informace, které můžete použít k diagnostikování selhání artefaktů. Existuje několik způsobů, můžete zobrazit informace o protokolu artefaktů pro virtuální počítač s Windows.

> [!NOTE]
> Zajistit, že chyby jsou správně identifikovat a vysvětlení, je důležité, že je správně strukturován artefaktu. Informace o tom, jak správně vytvořit artefakt najdete v tématu [vytvoření vlastních artefaktů](devtest-lab-artifact-author.md). A pokud chcete zobrazit příklad správně strukturovaný artefaktů, podívejte se na to [typy parametrů testovací](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artefaktů.

## <a name="troubleshoot-artifact-failures-using-the-azure-portal"></a>Řešení potíží s artefaktů selhání pomocí portálu Azure
Portál Azure k diagnostikování selhání během vytváření artefaktů, postupujte podle těchto kroků:

1. V seznamu zdrojů vyberte testovacího prostředí.

2. Vyberte virtuální počítač Windows, který zahrnuje artefaktu, který chcete prozkoumat.

3. V levém podokně v části **Obecné**, zvolte **artefakty**. Zobrazí se seznam artefakty související s tohoto virtuálního počítače, oznamující název artefaktu a jeho stav.

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. Zvolte artefakt zobrazující stav **se nezdařilo**. Artefaktu otevře a zobrazí zprávu rozšíření, která obsahuje podrobnosti o selhání artefaktu.

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-the-vm"></a>Řešení potíží s selhání artefaktů z virtuálního počítače
Pokud chcete zobrazit protokoly artefaktů z virtuálního počítače, postupujte takto:

1. Přihlaste se k virtuálním počítači, který obsahuje artefaktů, které chcete diagnostikovat.

2. Přejděte na C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status kde "1.9 je číslo verze rozšíření na straně klienta.

   ![Příklad úložišti git artefaktů](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. Otevřete **stav** soubor k zobrazení informací, že pomáhá diagnostikování selhání artefaktů tohoto virtuálního počítače.




[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související
* [Připojení virtuálního počítače do existující domény AD pomocí šablony správce prostředků v Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [přidejte úložiště Git do testovacího prostředí](devtest-lab-add-artifact-repo.md).

