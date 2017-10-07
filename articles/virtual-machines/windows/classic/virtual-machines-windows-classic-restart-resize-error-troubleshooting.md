---
title: "aaaVM restartováním nebo změnou velikosti problémy | Microsoft Docs"
description: "Řešení problémů nasazení classic s restartováním nebo změnou velikosti existující virtuální počítač Windows v Azure"
services: virtual-machines-windows
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 06/13/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: 3d00ba17d9558941a37a29034604cb15e0803e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Řešení problémů nasazení classic s restartováním nebo změnou velikosti existující virtuální počítač Windows v Azure
> [!div class="op_single_selector"]
> * [Classic](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [Resource Manager](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

Zkuste toostart ukončeno virtuálního počítače (virtuální počítač Azure) nebo přizpůsobit existující virtuální počítač Azure, hello běžnou chybou, které zaznamenáte při došlo k chybě přidělení. Tato chyba nastává hello clusteru nebo oblast buď nemá k dispozici prostředky nebo nelze podporu hello požadovaná velikost virtuálního počítače.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../../../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Protokoly auditu shromáždit
řešení potíží, toostart auditu shromáždit hello zaznamená tooidentify hello chyby související s problémem hello.

V hello portálu Azure, klikněte na **Procházet** > **virtuální počítače** > *virtuálního počítače Windows*  >   **Nastavení** > **protokoly auditu**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problém: Chyba při spouštění zastaveného virtuálního počítače
Zkuste toostart zastaveného virtuálního počítače, ale získat došlo k chybě přidělení.

### <a name="cause"></a>Příčina
žádost Hello toostart hello zastavena virtuálních počítačů má toobe na hello původní cluster, který je hostitelem hello cloudové služby. Hello cluster však nebude mít volné místo k dispozici toofulfill hello požadavku.

### <a name="resolution"></a>Řešení
* Vytvořte novou cloudovou službu a přidružte ji k buď oblasti nebo na základě oblast virtuální sítě, ale není skupiny vztahů.
* Odstranit hello zastavit virtuální počítač.
* Znovu vytvořte hello virtuálních počítačů v hello novou cloudovou službu pomocí hello disky.
* Spustit hello znovu vytvořit virtuální počítač.

Pokud dojde k chybě při pokusu o toocreate novou cloudovou službu, opakujte akci později nebo změnit hello oblast hello cloudové služby.

> [!IMPORTANT]
> Hello nová Cloudová služba bude mít nový název a VIP, takže bude potřeba toochange tyto informace pro všechny závislosti hello, které používají tyto informace pro hello stávající cloudovou službu.
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problém: Chyba při změně velikosti stávajícího virtuálního počítače
Zkuste tooresize existující virtuální počítač ale získat došlo k chybě přidělení.

### <a name="cause"></a>Příčina
Hello tooresize hello virtuálních počítačů má toobe pokusem o zpracování požadavku v původním clusteru hello dané hostitelů hello cloudové služby. Ale hello clusteru nepodporuje hello požadovaná velikost virtuálního počítače.

### <a name="resolution"></a>Řešení
Snižte hello požadovaná velikost virtuálního počítače a opakujte hello změnit velikost žádosti.

* Klikněte na tlačítko **procházet všechny** > **virtuálních počítačů (klasické)** > *virtuálního počítače* > **nastavení**  >  **Velikost**. Podrobné pokyny najdete v tématu [změnit velikost virtuálního počítače hello](https://msdn.microsoft.com/library/dn168976.aspx).

Pokud není možné tooreduce hello velikost virtuálního počítače, postupujte takto:

* Vytvořte novou cloudovou službu, zajistíte, že není propojená skupiny vztahů tooan a není přidružen k virtuální síti, která je propojená tooan skupiny vztahů.
* Vytvoření nové, větší velikost virtuálního počítače v ní.

Můžete sloučit všechny virtuální počítače v hello stejné cloudové služby. Pokud je přidružený k virtuální síti na základě oblast stávající cloudovou službu, můžete se připojit hello nové cloudové služby toohello existující virtuální sítě.

Pokud není přidružený k virtuální síti na základě oblast hello stávající cloudovou službu, pak máte toodelete hello virtuálních počítačů v hello stávající cloudovou službu a je v hello novou cloudovou službu z jejich disky znovu vytvořit. Je však důležité tooremember, hello nová Cloudová služba bude mít nový název a VIP, takže bude potřeba tooupdate tyto pro všechny hello závislosti, které používají tyto informace pro hello stávající cloudovou službu.

## <a name="next-steps"></a>Další kroky
Pokud dojde k potížím při vytvoření virtuálního počítače s Windows v Azure, najdete v části [řešení potíží s nasazením s vytvoření virtuálního počítače s Windows v Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

