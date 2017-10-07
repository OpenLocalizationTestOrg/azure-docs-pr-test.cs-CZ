---
title: "virtuální počítač s Windows v modelu nasazení classic hello - Azure aaaResize | Microsoft Docs"
description: "Změňte velikost virtuálního počítače s Windows vytvořené v modelu nasazení classic hello, pomocí Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e3038215-001c-406e-904d-e0f4e326a4c7
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: 39fab14431e2351c9515b0611e850eccfec7a798
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-windows-vm-created-in-hello-classic-deployment-model"></a>Změnit velikost virtuálního počítače Windows vytvořené v modelu nasazení classic hello
Tento článek ukazuje, jak tooresize virtuální počítač s Windows, vytvořené v modelu nasazení classic hello pomocí Azure Powershell.

Při určování hello možnost tooresize virtuální počítač, existují dvě koncepty, které řídí hello rozsahu velikosti dostupné tooresize hello virtuálního počítače. První koncept Hello je hello oblast, ve kterém je nasazený virtuální počítač. Hello seznam velikosti virtuálních počítačů v oblasti je kartě hello služby hello oblasti Azure webové stránky. druhý koncept Hello je fyzický hardware hello aktuálně hostuje virtuální počítač. Hello fyzických serverů hostuje virtuální počítače jsou seskupeny dohromady v clusterech běžné fyzický hardware. Metoda Hello změny velikost virtuálního počítače se liší v závislosti na tom, pokud hello požadovanou velikost nového virtuálního počítače je podporováno hello hardwaru cluster aktuálně hostuje hello virtuálních počítačů.

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Můžete také [změnit velikost virtuálního počítače vytvořené v modelu nasazení Resource Manager hello](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="add-your-account"></a>Přidání účtu
Musíte nakonfigurovat prostředí Azure PowerShell toowork s klasické prostředky Azure. Postupujte podle kroků hello tooconfigure prostředí Azure PowerShell toomanage klasické prostředky.

1. Zadejte v příkazovém prostředí PowerShell hello `Add-AzureAccount` a klikněte na tlačítko **Enter**. 
2. Zadejte e-mailovou adresu hello spojené s předplatným Azure a klikněte na tlačítko **pokračovat**. 
3. Zadejte heslo hello k vašemu účtu. 
4. Klikněte na tlačítko **přihlášení**. 

## <a name="resize-in-hello-same-hardware-cluster"></a>Změna velikosti v hello stejný hardware clusteru
tooresize velikost tooa virtuálního počítače, která je k dispozici v clusteru hardwaru hello hostování hello virtuálních počítačů, proveďte následující kroky hello.

1. Spusťte následující příkaz prostředí PowerShell, že k dispozici v clusteru hardwaru hello hostování hello Cloudová služba, která obsahuje velikosti virtuálních počítačů hello toolist hello virtuálních počítačů hello.
   
    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```
2. Spusťte následující příkazy tooresize hello virtuálních počítačů hello.
   
    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Změnit velikost na novém clusteru hardwaru
hello tooresize tooa velikost virtuálního počítače není k dispozici v hello hardwaru cluster hostitelem virtuálních počítačů, hello cloudové služby a všechny virtuální počítače v hello cloudové služby je nutné znovu vytvořit. Jednotlivých cloudových služeb je hostované na klastru jeden hardwaru, takže všechny virtuální počítače v hello cloudové služby musí být velikost, která je podporována v clusteru s podporou hardwaru. Hello popisují tyto kroky budou jak hello tooresize odstranit a potom znovu vytvořit virtuální počítač cloudové služby.

1. Spusťte následující prostředí PowerShell příkaz toolist hello velikosti virtuálních počítačů k dispozici v oblasti hello hello. 
   
    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```
2. Poznamenejte si všechna nastavení konfigurace pro každý virtuální počítač v hello cloudové služby, který obsahuje toobe hello virtuálních počítačů po změně velikosti. 
3. Odstraňte všechny virtuální počítače v cloudové službě hello výběr hello možnost tooretain hello disky pro každý virtuální počítač.
4. Znovu vytvořte toobe hello virtuálních počítačů po změně velikosti pomocí hello potřeby velikost virtuálního počítače.
5. Znovu vytvořte všech ostatních virtuálních počítačů, které byly v rámci cloudové služby hello pomocí velikost virtuálního počítače, který je k dispozici v clusteru hardwaru hello nyní hostitelem hello cloudové služby.

Ukázkový skript pro odstranit a znovu vytvořit cloudovou službu pomocí nové velikost virtuálního počítače lze nalézt [zde](https://github.com/Azure/azure-vm-scripts). 

## <a name="next-steps"></a>Další kroky
* [Změnit velikost virtuálního počítače vytvořené v modelu nasazení Resource Manager hello](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

