---
title: "nastavit aaaCreate dostupnosti virtuálních počítačů v Azure | Microsoft Docs"
description: "Zjistěte, jak nastavit toocreate spravované dostupnosti nebo nespravované dostupnosti pro virtuální počítače pomocí prostředí Azure PowerShell nastavit nebo hello portálu v modelu nasazení Resource Manager hello."
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
ms.openlocfilehash: eadcdfcd28bb2fa21a4647f207b390c33e022ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="increase-vm-availability-by-creating-an-azure-availability-set"></a>Zvýšení dostupnosti virtuálních počítačů tak, že vytvoříte skupinu dostupnosti Azure 
Skupiny dostupnosti poskytují redundanci tooyour aplikace. Doporučujeme seskupit dva nebo více virtuálních počítačů v nastavení dostupnosti. Tato konfigurace zajistí, že během buď plánované i neplánované údržby alespoň jeden virtuální počítač bude k dispozici a plnění hello 99,95 % smlouva Azure SLA. Další informace najdete v tématu hello [SLA pro virtuální počítače](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Virtuální počítače musí být vytvořen v hello stejné skupině prostředků jako skupinu dostupnosti hello.
> 

Pokud chcete, aby váš virtuální počítač toobe součástí skupiny dostupnosti, je třeba toocreate hello dostupnosti nastavit první nebo při vytváření vašeho prvního virtuálního počítače v sadě hello. Pokud virtuální počítač bude používat spravované disků, musí být vytvořeny skupinu dostupnosti hello jako sadu spravovaných dostupnosti.

Další informace o vytváření a použití skupiny dostupnosti najdete v tématu [spravovat hello dostupnosti virtuálních počítačů](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="use-hello-portal-toocreate-an-availability-set-before-creating-your-vms"></a>Použití portálu toocreate hello před vytvořením virtuálních počítačů sadu dostupnosti
1. V nabídce centra hello, klikněte na **Procházet** a vyberte **sady dostupnosti**.
2. Na hello **sady dostupnosti okno**, klikněte na tlačítko **přidat**.
   
    ![Snímek obrazovky zobrazující hello přidat tlačítko pro vytvoření nové sady dostupnosti.](./media/create-availability-set/add-availability-set.png)
3. Na hello **vytvořit skupinu dostupnosti** okno, informace o dokončení hello vaší sady.
   
    ![Snímek obrazovky, ukazuje hello informace tooenter toocreate hello dostupnosti je potřeba nastavit.](./media/create-availability-set/create-availability-set.png)
   
   * **Název** -hello název by měl být 1-80 znaků skládá z čísla, písmena, tečky, podtržítka a pomlčky. Hello první znak musí být písmeno nebo číslice. Hello poslední znak musí být písmeno, číslo nebo podtržítko.
   * **Poruch domén** -domén selhání definovat hello skupinu virtuálních počítačů, které sdílejí společné přepínač zdroje a sítě power. Ve výchozím nastavení hello virtuální počítače jsou oddělené v rámci až toothree domén selhání a může být změněné toobetween 1 a 3.
   * **Aktualizovat domén** – ve výchozím nastavení jsou přiřazeny pět domén aktualizace, a to je možné nastavit toobetween 1 až 20. Aktualizace domén označují skupiny virtuálních počítačů a základní fyzický hardware, který může být restartován v hello stejnou dobu. Například pokud bychom zadat pět aktualizace, které domény, když je více než pět virtuální počítače jsou nastaveny v rámci jedné dostupnosti sadě, hello šesté virtuální počítač se umístí do hello jedné aktualizační doméně jako hello první virtuální počítač hello sedmá v hello, stejné UD jako hello druhý virtuální počítač a tak dále. Hello pořadí hello restartování nemusí být po sobě jdoucích, ale bude restartován pouze jednu aktualizační doménu najednou.
   * **Předplatné** -vyberte hello toouse předplatné, pokud máte více než jednou.
   * **Skupina prostředků** -vyberte existující skupinu prostředků, klikněte na šipku hello a výběrem skupinu prostředků z hello rozevírací nabídku. Můžete také vytvořit novou skupinu prostředků tak, že zadáte v názvu. Hello název může obsahovat jakýkoli z hello následující znaky: písmena, číslice, tečky, pomlčky, podtržítka a otevírání nebo kulaté závorky. Hello název nemůže končit tečkou. Všechny hello virtuální počítače ve skupině dostupnosti hello musí toobe vytvořené v hello stejné skupině prostředků jako skupinu dostupnosti hello.
   * **Umístění** -vyberte z rozevíracího seznamu hello umístění.
   * **Spravované** – vyberte *Ano* toocreate spravované dostupnosti nastavit toouse s virtuálními počítači, které používají spravovaný disků pro úložiště. Vyberte **ne** Pokud hello virtuálních počítačů, které budou v sadě hello použijte nespravované disky v účtu úložiště.
   
4. Při zadávání informací hello skončíte, klikněte na tlačítko **vytvořit**. 

## <a name="use-hello-portal-toocreate-a-virtual-machine-and-an-availability-set-at-hello-same-time"></a>Použití portálu toocreate hello virtuální počítač a dostupnosti nastavit na hello stejný čas
Pokud vytváříte nový virtuální počítač pomocí hello portálu, můžete také vytvořit novou sadu dostupnosti pro hello virtuálních počítačů při vytváření hello první virtuální počítač v sadě hello. Pokud si zvolíte toouse spravované disky pro virtuální počítač, bude vytvořen sadu spravovaných dostupnosti.

![Snímek obrazovky zobrazující hello proces vytvoření nového dostupnosti nastavit při vytváření hello virtuálních počítačů.](./media/create-availability-set/new-vm-avail-set.png)

## <a name="add-a-new-vm-tooan-existing-availability-set-in-hello-portal"></a>Přidat nový virtuální počítač tooan stávající sadu dostupnosti hello portálu
Pro každý další virtuální počítač, který vytvoříte, by měl patřit sady hello, ujistěte se, vytvořte ji v hello stejné **skupiny prostředků** a pak vyberte hello existující dostupnosti v kroku 3. 

![Snímek obrazovky ukazující, jak nastavit tooselect existující dostupnosti toouse pro virtuální počítač.](./media/create-availability-set/add-vm-to-set.png)

## <a name="use-powershell-toocreate-an-availability-set"></a>Pomocí prostředí PowerShell toocreate dostupnosti nastavit
Tento příklad vytvoří sadu s názvem dostupnosti **myAvailabilitySet** v hello **myResourceGroup** skupiny prostředků v hello **západní USA** umístění. Tento krok je nutné provést před vytvořením hello toobe první virtuální počítač, který bude v sadě hello.

Než začnete, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello. Spustit hello následující příkaz tooinstall.

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
* Při vytváření virtuálního počítače, pokud skupina dostupnosti hello, které chcete není v rozevíracím seznamu hello portálu hello pravděpodobně vytvořena ho v jiné skupině prostředků. Pokud si nejste jisti hello skupinu prostředků pro vaše dostupnosti nastavit, přejděte v nabídce centra toohello a klikněte na tlačítko Procházet > toosee nastaví seznam vaší dostupnosti a které skupiny prostředků, které patří do sady dostupnosti.

## <a name="next-steps"></a>Další kroky
Přidejte další úložiště tooyour virtuálních počítačů tak, že přidáte další [datový disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

