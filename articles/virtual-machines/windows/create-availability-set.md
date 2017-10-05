---
title: "Vytvoření dostupnosti virtuálních počítačů v Azure | Microsoft Docs"
description: "Naučte se vytvořit skupinu dostupnosti spravované nebo nespravované sadu dostupnosti pro virtuální počítače pomocí prostředí Azure PowerShell nebo portálu v modelu nasazení Resource Manager."
keywords: Sady dostupnosti.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a3db8659-ace8-4e78-8b8c-1e75c04c042c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e813ade8e105169f21138ed8a8eafda1ff39f42e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="increase-vm-availability-by-creating-an-azure-availability-set"></a>Zvýšení dostupnosti virtuálních počítačů tak, že vytvoříte skupinu dostupnosti Azure 
Skupiny dostupnosti poskytnout redundanci do vaší aplikace. Doporučujeme seskupit dva nebo více virtuálních počítačů v nastavení dostupnosti. Tato konfigurace zajistí, že během buď plánované i neplánované údržby alespoň jeden virtuální počítač bude k dispozici a splňují podmínku 99.95 % smlouva Azure SLA. Další informace najdete v tématu [SLA pro virtuální počítače](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Virtuální počítače musí být vytvořeny ve stejné skupině prostředků jako skupiny dostupnosti.
> 

Pokud chcete virtuální počítač jako součást skupiny dostupnosti, je nutné vytvořit skupinu dostupnosti nejprve nebo při vytváření vašeho prvního virtuálního počítače v sadě. Pokud virtuální počítač bude používat spravované disky, je nutné vytvořit skupinu dostupnosti jako sadu spravovaných dostupnosti.

Další informace o vytváření a použití skupiny dostupnosti najdete v tématu [Správa dostupnosti virtuálních počítačů](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>Vytvořit sadu před vytvořením virtuálních počítačů dostupnosti pomocí portálu
1. V nabídce centra klikněte na **Procházet** a vyberte **sady dostupnosti**.
2. Na **sady dostupnosti okno**, klikněte na tlačítko **přidat**.
   
    ![Snímek obrazovky, který ukazuje na tlačítko Přidat pro vytvoření nové dostupnosti nastavit.](./media/create-availability-set/add-availability-set.png)
3. Na **vytvořit skupinu dostupnosti** okno, zadejte informace o vaší sady.
   
    ![Snímek obrazovky s informacemi, budete muset zadat pro vytvoření sady dostupnosti.](./media/create-availability-set/create-availability-set.png)
   
   * **Název** -název musí být 1-80 znaků skládá z čísla, písmena, tečky, podtržítka a pomlčky. První znak musí být písmeno nebo číslice. Poslední znak musí být písmeno, číslo nebo podtržítko.
   * **Poruch domén** -domén selhání definování skupiny virtuálních počítačů, které sdílejí společné přepínač zdroje a sítě power. Ve výchozím nastavení virtuální počítače jsou oddělené v rámci až tři domén selhání a můžete změnit tak, aby mezi 1 a 3.
   * **Aktualizovat domén** – ve výchozím nastavení jsou přiřazeny pět domén aktualizace, a to může být nastaven na 1 až 20. Aktualizace domén označují skupiny virtuálních počítačů a základní fyzický hardware, který může být restartován ve stejnou dobu. Například pokud bychom zadat, že pět aktualizovat domén, když více než pět virtuální počítače jsou nastaveny v rámci jedné skupiny dostupnosti, šesté virtuální počítač se umístí do jedné aktualizační doméně jako první virtuální počítač, sedmý ve stejné UD jako druhý virtuální počítač a tak dále. Pořadí restartování počítače nemusí být po sobě jdoucích, ale bude restartován pouze jednu aktualizační doménu najednou.
   * **Předplatné** -vyberte odběr, který má použít, pokud máte více než jeden.
   * **Skupina prostředků** – klikněte na šipku a výběrem skupiny prostředků v rozevíracím seznamu vyberte existující skupinu prostředků. Můžete také vytvořit novou skupinu prostředků tak, že zadáte v názvu. Název může obsahovat následující znaky: písmena, číslice, tečky, pomlčky, podtržítka a otevírání nebo kulaté závorky. Název nemůže končit tečkou. Všechny virtuální počítače ve skupině dostupnosti potřeba vytvořit ve stejné skupině prostředků jako skupiny dostupnosti.
   * **Umístění** – z rozevíracího seznamu vyberte umístění.
   * **Spravované** – vyberte *Ano* k vytvoření spravovaného dostupnosti nastavení aplikaci virtuálních počítačů, které používají spravovaný disků pro úložiště. Vyberte **ne** Pokud virtuální počítače, které budou v sadě používat nespravované disky v účtu úložiště.
   
4. Po dokončení zadáváte informace, klikněte na tlačítko **vytvořit**. 

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>Použití portálu k vytvoření virtuálního počítače a současně sadu dostupnosti
Při vytváření nového virtuálního počítače pomocí portálu, můžete také vytvořit novou sadu dostupnosti pro virtuální počítač při vytváření první virtuální počítač v sadě. Pokud se rozhodnete používat spravované disky pro virtuální počítač, bude vytvořen sadu spravovaných dostupnosti.

![Snímek obrazovky, který ukazuje postup pro vytvoření nové dostupnosti nastavit při vytváření virtuálního počítače.](./media/create-availability-set/new-vm-avail-set.png)

## <a name="add-a-new-vm-to-an-existing-availability-set-in-the-portal"></a>Přidat nový virtuální počítač do existující dostupnosti nastaveným na portálu
Každý další virtuální počítač, který vytvoříte, by měly patřit v sadě, je nutné vytvořit ve stejné **skupiny prostředků** a potom vyberte existující sadu v kroku 3 dostupnosti. 

![Snímek obrazovky ukazující, jak vybrat existující sadu používat pro virtuální počítač dostupnosti.](./media/create-availability-set/add-vm-to-set.png)

## <a name="use-powershell-to-create-an-availability-set"></a>Použití prostředí PowerShell vytvořit skupinu dostupnosti
Tento příklad vytvoří sadu s názvem dostupnosti **myAvailabilitySet** v **myResourceGroup** skupinu prostředků **západní USA** umístění. To je potřeba provést před vytvořením první virtuální počítač, který bude v sadě.

Než začnete, ujistěte se, že máte nejnovější verzi modulu AzureRM.Compute prostředí PowerShell. Spusťte následující příkaz k její instalaci.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).


Pokud používáte spravované disky pro virtuální počítače, zadejte:

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" -managed
```

Pokud používáte vlastní účty úložiště pro virtuální počítače, zadejte:

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" 
```

Další informace najdete v tématu [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).

## <a name="troubleshooting"></a>Řešení potíží
* Při vytváření virtuálního počítače, není-li skupinu dostupnosti, které chcete v rozevíracím seznamu v portálu pravděpodobně jste vytvořili ho v jiné skupině prostředků. Pokud si nejste jisti, skupinu prostředků pro vaše dostupnosti nastavit, přejděte do nabídky rozbočovače a klikněte na tlačítko Procházet > sady dostupnosti zobrazte seznam skupiny dostupnosti a který patří do skupiny prostředků.

## <a name="next-steps"></a>Další kroky
Přidejte další úložiště k virtuálnímu počítači tak, že přidáte další [datový disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

