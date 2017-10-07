---
title: "aaaCreate vlastní obrázek ze souboru virtuálního pevného disku Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak hello toocreate vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí portálu Azure"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 80af8ea1cb72380f868df0a76c4a0dcd92e63cf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Vytvořit vlastní image ze souboru virtuálního pevného disku

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Podrobné pokyny

Hello následující postup vás provede procesem vytvoření vlastní image ze souboru virtuálního pevného disku pomocí hello portálu Azure:

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.

1. Ze seznamu hello labs vyberte požadované prostředí hello.  

1. V okně prostředí hello vyberte **konfigurace**. 

1. V testovacím hello **konfigurace** vyberte **vlastní Image (VHD)**.

1. Na hello **vlastní image** vyberte **+ přidat**.

    ![Přidat vlastní obrázek](./media/devtest-lab-create-template/add-custom-image.png)

1. Zadejte název hello hello vlastní Image. Tento název se zobrazí v seznamu hello základní Image při vytváření virtuálního počítače.

1. Zadejte popis hello hello vlastní image. Tento popis se zobrazí v seznamu hello základní Image při vytváření virtuálního počítače.

1. Vyberte **virtuálního pevného disku**.

1. Z hello **virtuálního pevného disku** okně, vyberte hello požadovaného souboru virtuálního pevného disku.

1. Vyberte **OK** tooclose hello **virtuálního pevného disku** okno.

1. Vyberte **konfigurace operačního systému**.

1. Na hello **konfigurace operačního systému** , vyberte buď **Windows** nebo **Linux**.

1. Pokud **Windows** je vybrána, zadejte prostřednictvím hello políčko zda *Sysprep* byl spuštěn na počítači hello. 

1. Vyberte **OK** tooclose hello **konfigurace operačního systému** okno.

1. Vyberte **OK** toocreate hello vlastní image.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související

- [Vlastní Image nebo vzorce?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopírování vlastních bitových kopií mezi Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Další kroky

- [Přidání testovacího prostředí tooyour virtuálních počítačů](./devtest-lab-add-vm-with-artifacts.md)
