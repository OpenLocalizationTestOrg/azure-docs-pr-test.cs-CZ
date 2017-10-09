---
title: "aaaCreate vlastní obrázek z virtuálního počítače Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak hello toocreate vlastní image v Azure DevTest Labs z zřízeného virtuálního počítače pomocí portálu Azure"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 7dccb79d3db4aae676c7bd2f6b800301210491e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vm"></a>Vytvořit vlastní image z virtuálního počítače

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Podrobné pokyny

Můžete vytvořit vlastní image ze zřízeného virtuálního počítače a pak použít tento vlastní image toocreate identické virtuální počítače. Hello následující kroky popisují, jak toocreate vlastní obrázek z virtuálního počítače:

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.

1. Ze seznamu hello labs vyberte požadované prostředí hello.  

1. V okně prostředí hello vyberte **Můj virtuální počítače**.
 
1. Na hello **Můj virtuální počítače** okně vyberte hello virtuálních počítačů, ze kterého mají být toocreate hello vlastní image.

1. V okně hello Virtuálního počítače, vyberte **vytvořit vlastní image (VHD)**.

    ![Vytvoření vlastní image položky nabídky](./media/devtest-lab-create-template/create-custom-image.png)

1. Na hello **vytvořit image** okno, zadejte název a popis pro vlastní bitovou kopii. Tyto informace se zobrazí v seznamu hello základů při vytváření virtuálního počítače.

    ![Vytvořit vlastní image okno](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Vyberte, zda byl spuštěn nástroj sysprep hello virtuálních počítačů. Pokud na hello virtuální počítač nebyl spuštěn nástroj sysprep hello, určete, jestli má nástroj sysprep, které jsou spuštěny při vytvoření virtuálního počítače z této vlastní image.

1. Vyberte **OK** při dokončení toocreate hello vlastní image.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související

- [Vlastní Image nebo vzorce?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopírování vlastních bitových kopií mezi Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Další kroky

- [Přidání testovacího prostředí tooyour virtuálních počítačů](./devtest-lab-add-vm-with-artifacts.md)
