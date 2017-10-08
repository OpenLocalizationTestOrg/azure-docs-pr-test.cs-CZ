---
title: "aaaAccessing virtuálních počítačů Azure z Průzkumníka serveru | Microsoft Docs"
description: "Získáte přehled o tom, jak tooview vytvářet a spravovat virtuální počítače Azure (VM) v Průzkumníku serveru v sadě Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: eb3afde6-ba90-4308-9ac1-3cc29da4ede0
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: f8326aed105a64ca558f766d712cc68701f82c15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Přístup k Azure virtuální počítače z Průzkumníka serveru
Pomocí Průzkumníka serveru v sadě Visual Studio, můžete zobrazit informace o virtuální počítače hostované v Azure.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Přístup k virtuálním počítačům v Průzkumníku serveru
Pokud máte virtuální počítače hostované v Azure, můžete k dispozici v Průzkumníku serveru. Je nutné se přihlásit v tooyour předplatného Azure tooview mobilní služby. toosign, otevřete hello místní nabídku pro hello Azure uzlu v Průzkumníku serveru a zvolte **připojit tooMicrosoft Azure**.

### <a name="tooget-information-about-your-virtual-machines"></a>tooget informace o virtuálních počítačích
1. V Průzkumníku serveru vyberte virtuální počítač a zvolte klíče tooshow hello F4 jeho vlastnosti – okno.
   
    Hello následující tabulka ukazuje, jaké vlastnosti jsou k dispozici, ale jsou všechny jen pro čtení. toochange, použijte hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
   
   | Vlastnost | Popis |
   | --- | --- |
   | Název DNS |Adresa URL Hello s hello internetové adresy hello virtuálního počítače. |
   | Prostředí |Pro virtuální počítač hello hodnota této vlastnosti je vždy produkční. |
   | Name (Název) |Název Hello hello virtuálního počítače. |
   | Velikost |velikost Hello hello virtuálního počítače, které odráží hello množství paměti a místa na disku, je k dispozici. Další informace najdete v tématu Postupy: Konfigurace velikostí virtuálních počítačů. |
   | Status |Hodnoty zahrnují spouštění, spuštěno, zastavit, zastaveno a načítání stavu. Pokud se zobrazí stav načítání, aktuální stav hello neznámý. Hello hodnoty této vlastnosti se liší od hello hodnoty, které se používají na hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885). |
   | ID předplatného |ID předplatného Hello k účtu Azure. Tyto informace můžete zobrazit na hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885) zobrazením hello vlastnosti pro předplatné. |
2. Zvolte do uzlu koncový bod a potom zobrazit hello **vlastnosti** okno.
3. Hello následující tabulka popisuje dostupné vlastnosti hello koncových bodů, ale jsou jen pro čtení. tooadd nebo upravit koncových bodů hello pro virtuální počítač, použijte hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885). 
   
   | Vlastnost | Popis |
   | --- | --- |
   | Name (Název) |Identifikátor pro koncový bod hello. |
   | Privátní Port |Hello portu pro síťový přístup interní tooyour aplikaci. |
   | Protocol (Protokol) |Hello protokol, který hello přenosové vrstvy pro tento koncový bod používá, TCP nebo UDP. |
   | Veřejný port |Hello port, který se používá pro aplikaci tooyour veřejný přístup. |

## <a name="next-steps"></a>Další kroky
toolearn Další informace o použití Azure rolí v sadě Visual Studio, najdete v části [pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md).

