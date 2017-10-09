---
title: "aaaTroubleshoot klasický nasazení virtuálního počítače s Linuxem | Microsoft Docs"
description: "Řešení problémů nasazení classic, když vytvoříte nový virtuální počítač Linux v Azure"
services: virtual-machines-linux
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: c8a963fa-6b2a-4c7a-a1f4-7793adb02b19
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: cjiang
ms.openlocfilehash: a642bfd9a543cd6d2104b03fe04193d9352ccae1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Řešení problémů nasazení classic s vytvoření nového virtuálního počítače Linux v Azure
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Hello Resource Manager verzi v tomto článku, naleznete v části [zde](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Protokoly auditu shromáždit
řešení potíží, toostart auditu shromáždit hello zaznamená tooidentify hello chyby související s problémem hello.

V hello portálu Azure, klikněte na **Procházet** > **virtuální počítače** > *virtuálního počítače Windows*  >   **Nastavení** > **protokoly auditu**.

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**/ Y:** Pokud hello operačního systému Linux zobecněn a se nahrál nebo zachytit pomocí hello zobecněn nastavení, potom nebudou všechny chyby. Podobně pokud hello operačního systému Linux specializuje a je nahrál nebo zachytit pomocí hello speciální nastavení, pak nebude existovat žádné chyby.

**Odešlete chyby:**

**N<sup>1</sup>:** Pokud hello operačního systému Linux zobecněn, a nahraje jako specializovaný, zobrazí se zřizování vypršení časového limitu protože hello virtuálních počítačů zasekl ve fázi zřizování hello.

**N<sup>2</sup>:** Pokud hello operačního systému Linux specializuje a nahraje jako zobecněn, zřizování selhání chyba se zobrazí, protože hello nový virtuální počítač je spuštěn s hello původní název počítače, uživatelské jméno a heslo.

**Řešení:**

tooresolve obě tyto chyby, odeslat hello původní virtuální pevný disk, k dispozici místní, s hello stejné nastavení jako hello operačního systému (zobecněn/specializuje). tooupload jako zobecněn, mějte na paměti, toorun-nejprve zrušit jejich zřízení. V tématu [vytvoření a nahrání virtuálního pevného disku tohoto hello obsahuje operační systém Linux](create-upload-vhd.md) Další informace.

**Zaznamenat chyby:**

**N<sup>3</sup>:** Pokud hello operačního systému Linux zobecněn, a se zaznamená jako specializovaný, zobrazí se zřizování vypršení časového limitu protože hello původní virtuální počítač je nepoužitelný je označen jako zobecněn.

**N<sup>4</sup>:** Pokud hello operačního systému Linux specializuje a z něj zachytí jako zobecněn, zřizování selhání chyba se zobrazí, protože hello nový virtuální počítač je spuštěn s hello původní název počítače, uživatelské jméno a heslo. Navíc hello původní virtuální počítač je nepoužitelný, protože je označeno jako specializované.

**Řešení:**

tooresolve obě tyto chyby odstranit hello aktuální image z portálu hello a [kopii z hello aktuální virtuální pevné disky](capture-image.md) s hello stejné nastavení jako hello operačního systému (zobecněn/specializuje).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problém: Vlastní nebo Galerie / marketplace obrázku; došlo k chybě přidělení
Tato chyba nastane v situacích, při hello novou žádost o virtuální počítač je odeslání tooa clusteru, který buď nemá dostupné volné místo tooaccommodate hello požadavek, nebo nepodporuje požadovanou velikost virtuálního počítače hello. Není možné toomix jinou sérii virtuálních počítačů v hello stejné cloudové služby. Takže pokud chcete toocreate nový virtuální počítač je jiný než co cloudové služby může podporovat velikost, požadavek na výpočetní hello se nezdaří.

V závislosti na omezení hello hello cloudové služby používáte toocreate hello nového virtuálního počítače, může dojít k chybě způsobené dvou situacích.

**Příčina 1:** hello Cloudová služba se definovaného tooa konkrétní clusteru, nebo je propojené tooan skupiny vztahů a proto definovaného tooa konkrétní cluster záměrné. Proto si vyžádá novou výpočetních prostředků v této skupině vztahů jsou použity v hello stejný cluster, kde jsou hostované hello existující prostředky. Nicméně hello stejného clusteru může buď není podpora hello požadovaná velikost virtuálního počítače nebo mít není dostatek volného místa, což vede k chybě přidělení. To platí, jestli jsou vytvořeny nové prostředky hello prostřednictvím novou cloudovou službu nebo stávající cloudovou službu.

**Řešení 1:**

* Vytvořte novou cloudovou službu a přidružit ji k oblasti nebo na základě oblast virtuální sítě.
* Vytvoření nového virtuálního počítače v hello novou cloudovou službu.
  Pokud dojde k chybě při pokusu o toocreate novou cloudovou službu, opakujte později nebo změnit hello oblast hello cloudové služby.

> [!IMPORTANT]
> Pokud se pokoušeli toocreate nového virtuálního počítače ve stávající cloudovou službu, ale nelze a měl toocreate novou cloudovou službu pro nový virtuální počítač, můžete tooconsolidate všechny virtuální počítače v hello stejné cloudové služby. toodo Ano, odstraňte hello virtuálních počítačů v hello stávající cloudovou službu a jejich kopii z jejich disků ve hello novou cloudovou službu. Je však důležité tooremember, hello nová Cloudová služba bude mít nový název a VIP, takže bude potřeba tooupdate tyto pro všechny hello závislosti, které používají tyto informace pro hello stávající cloudovou službu.
> 
> 

**Příčina 2:** hello Cloudová služba je přidružený virtuální síť, která je propojená tooan skupin vztahů, tak, aby byl definovaného tooa konkrétní clusteru záměrné. Všechny nové požadavky výpočetních prostředků v této skupině vztahů jsou proto se pokusila hello stejný cluster, kde jsou hostované hello existující prostředky. Nicméně hello stejného clusteru může buď není podpora hello požadovaná velikost virtuálního počítače nebo mít není dostatek volného místa, což vede k chybě přidělení. To platí, jestli jsou vytvořeny nové prostředky hello prostřednictvím novou cloudovou službu nebo stávající cloudovou službu.

**Řešení 2:**

* Vytvořte nový regionální virtuální síť.
* Vytvořit nový virtuální počítač v nové virtuální sítě hello hello.
* [Připojit existující virtuální síť](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) toohello nové virtuální sítě. Další informace [regionálních virtuálních sítí](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Alternativně můžete [migrace vaší virtuální sítě na základě vztahů skupiny tooa regionální virtuální síť](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)a poté vytvořit hello nového virtuálního počítače.

## <a name="next-steps"></a>Další kroky
Pokud dojde k potížím při spuštění zastavený virtuální počítač s Linuxem nebo změnit jeho velikost existující virtuální počítač s Linuxem v Azure, najdete v části [nasazení classic potíží s restartováním nebo změnou velikosti existující virtuální počítač Linux v Azure](restart-resize-error-troubleshooting.md).

