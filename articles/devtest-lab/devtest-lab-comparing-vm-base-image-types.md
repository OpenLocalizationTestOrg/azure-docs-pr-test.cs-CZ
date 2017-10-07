---
title: "aaaComparing vlastních bitových kopií a vzorce v DevTest Labs | Microsoft Docs"
description: "Další informace o hello rozdíly mezi vlastních bitových kopií a vzorce jako virtuální počítač základny, abyste se mohli rozhodnout, které z nich nejlépe vyhovuje prostředí."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a3cb259a-7d80-40ec-8ee8-45105704d589
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: tarcher
ms.openlocfilehash: 3c1d88dfe0ff94b8e825bb7a0b4aca3341c9330d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>Porovnání vlastních bitových kopií a vzorce v DevTest Labs
Obě [vlastních bitových kopií](devtest-lab-create-template.md) a [vzorce](devtest-lab-manage-formulas.md) lze použít jako základ pro [vytvořit nové virtuální počítače](devtest-lab-add-vm-with-artifacts.md). Ale hello mezi vlastních bitových kopií a vzorce klíče rozdíl je, že vlastní image je jednoduše image podle virtuální pevný disk, když vzorec je obrázek, který založené na virtuální pevný disk *kromě* předkonfigurované nastavení – například velikost virtuálního počítače, virtuální sítě podsíť a artefakty. Tyto předem nakonfigurovaných nastavení se nastaví se výchozí hodnoty, které bude možné přepsat na hello čas vytvoření virtuálního počítače. Tento článek vysvětluje některé výhody hello (specialisté) a nevýhody (cons) toousing vlastních bitových kopií a kdy vzorce.

## <a name="custom-image-pros-and-cons"></a>Vlastní image výhody a nevýhody
Vlastní Image poskytují toocreate statické a neměnné způsob, jak virtuální počítače z požadované prostředí. 

**Odborníci na**

* Zřizování z vlastní image virtuálních počítačů je rychlé, protože nic změny po hello, které se spouštějí, virtuální počítač z bitové kopie hello. Jinými slovy nejsou žádné nastavení tooapply jako vlastní image hello je právě obrázek bez nastavení. 
* Virtuální počítače vytvořené z jedné vlastní image jsou identické.

**Cons**

* Pokud potřebujete tooupdate některých aspektů hello vlastní image, je nutné znovu vytvořit bitovou kopii hello.  

## <a name="formula-pros-and-cons"></a>Vzorce výhody a nevýhody
Vzorce zadejte dynamické způsob toocreate virtuální počítače z hello požadovaného nastavení.

**Odborníci na**

* Změny v prostředí hello se dají zachytit v chodu hello prostřednictvím artefakty. Například pokud chcete nainstalovat s nejnovější bity hello z vaší verze kanálu virtuálního počítače nebo zařazení hello nejnovější kód z úložiště, můžete jednoduše zadat artefakt nasadí nejnovější bits hello nebo využívá hello nejnovější kód ve vzorci hello společně se službou cíl základní bitovou kopii. Vždy, když tento vzorec je použité toocreate virtuální počítače, kód nejnovější bits hello jsou nasazené zařazen toohello virtuálních počítačů. 
* Vzorce můžete definovat výchozí nastavení, které nemůžete zadat vlastní Image – například velikosti virtuálních počítačů a nastavení virtuální sítě. 
* Hello nastavení uložená v vzorec se zobrazují jako výchozí hodnoty, ale můžete upravovat, když je vytvořena hello virtuálních počítačů. 

**Cons**

* Vytvoření virtuálního počítače ze vzorce může trvat delší dobu než vytvoření virtuálního počítače z vlastní image.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související
* [Vlastní Image nebo vzorce?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Další kroky
- [DevTest Labs – nejčastější dotazy](devtest-lab-faq.md)