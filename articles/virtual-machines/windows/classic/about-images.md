---
title: "aaaAbout bitových kopií pro virtuální počítače s Windows | Microsoft Docs"
description: "Další informace o použití bitové kopie s virtuální počítače s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 66ff3fab-8e7f-4dff-b8da-ab1c9c9c9af8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cynthn
ms.openlocfilehash: c7cfa1d018a5e99d5b68f559ec9ae1f14e4dec8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-images-for-windows-virtual-machines"></a>O bitových kopií pro virtuální počítače s Windows
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Informace o hledání a používání imagí v modelu Resource Manager hello najdete v tématu [zde](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a>Práce s obrázky

Můžete použít modul Azure PowerShell hello a hello Azure portálu toomanage hello bitové kopie k dispozici tooyour předplatného Azure. modul Azure PowerShell Hello poskytuje další parametry příkazu, takže lze přesně určit, jak přesné toosee nebo proveďte. Hello portál Azure poskytuje grafické rozhraní pro řadu úloh správy každý den hello.

Zde jsou některé příklady, které používají modul Azure PowerShell hello.

* **Získat všechny image**:`Get-AzureVMImage`vrátí seznam všech hello obrázků dostupné v aktuálním předplatném: vaše Image a ty poskytovaný Azure nebo partnerů. Výsledný seznam Hello může být velký. Zobrazit další příklady jak Hello tooget seznam kratší.
* **Získat image rodiny**:`Get-AzureVMImage | select ImageFamily` získá seznam image rodiny zobrazením řetězce **ImageFamily** vlastnost.
* **Získat všechny bitové kopie v konkrétní rodině**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
* **Najít Image virtuálních počítačů**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` Tato rutina funguje tak, že filtrování hello DataDiskConfiguration vlastnosti, která se vztahuje pouze tooVM bitové kopie. Tento příklad také vyfiltruje hello výstup tooonly hello label a název obrázku.
* **Uložte bitovou kopii zobecněný**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
* **Uložte bitovou kopii specializované**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`

  > [!TIP]
  > Parametr OSState Hello je požadovaná toocreate image virtuálního počítače, který zahrnuje hello disk operačního systému a datové disky připojené. Pokud nepoužijete parametr hello, hello rutina vytvoří bitovou kopii operačního systému. Hello hodnota parametru hello označuje, zda obrázek hello je zobecněn nebo specializuje, na základě na tom, jestli disk operačního systému hello připravený pro opakované použití.

* **Odstranit bitovou kopii**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`

## <a name="next-steps"></a>Další kroky
Můžete také [vytvoření počítače s Windows pomocí portálu Azure hello](tutorial.md).
