---
title: "Přístup k Azure virtuální počítače z Průzkumníka serveru | Microsoft Docs"
description: "Get přehled o tom, jak zobrazit vytvářet a spravovat virtuální počítače Azure (VM) v Průzkumníku serveru v sadě Visual Studio."
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
ms.openlocfilehash: fcbb00cc2f00691e25ea84333e8c418b08210a67
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Přístup k Azure virtuální počítače z Průzkumníka serveru
Pomocí Průzkumníka serveru v sadě Visual Studio, můžete zobrazit informace o virtuální počítače hostované v Azure.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Přístup k virtuálním počítačům v Průzkumníku serveru
Pokud máte virtuální počítače hostované v Azure, můžete k dispozici v Průzkumníku serveru. Musíte nejprve se přihlásit k vašemu předplatnému Azure Chcete-li zobrazit mobilní služby. Pokud chcete přihlásit, otevřete místní nabídky pro Azure uzel v Průzkumníku serveru a zvolte **připojit k Microsoft Azure**.

### <a name="to-get-information-about-your-virtual-machines"></a>Chcete-li získat informace o virtuálních počítačích
1. V Průzkumníku serveru vyberte virtuální počítač a potom vyberte klávesy F4 zobrazíte její vlastnosti – okno.
   
    Následující tabulka uvádí, které vlastnosti jsou k dispozici, ale jsou všechny jen pro čtení. Chcete-li změnit, použijte [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
   
   | Vlastnost | Popis |
   | --- | --- |
   | Název DNS |Adresa URL s Internetovou adresu virtuálního počítače. |
   | Prostředí |Pro virtuální počítač hodnota této vlastnosti je vždy produkční. |
   | Name (Název) |Název virtuálního počítače. |
   | Velikost |Velikost virtuálního počítače, které odráží množství paměti a místa na disku, je k dispozici. Další informace najdete v tématu Postupy: Konfigurace velikostí virtuálních počítačů. |
   | Status |Hodnoty zahrnují spouštění, spuštěno, zastavit, zastaveno a načítání stavu. Pokud se zobrazí stav načítání, aktuální stav neznámý. Hodnoty pro tuto vlastnost se liší od hodnoty, které jsou použity na [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885). |
   | ID předplatného |ID předplatného pro váš účet Azure. Tyto informace můžete zobrazit na [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885) zobrazením vlastností pro předplatné. |
2. Zvolte do uzlu koncového bodu a následně zobrazit **vlastnosti** okno.
3. Následující tabulka popisuje dostupné vlastnosti koncových bodů, ale jsou jen pro čtení. Chcete-li přidat nebo upravit koncové body pro virtuální počítač, použijte [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885). 
   
   | Vlastnost | Popis |
   | --- | --- |
   | Name (Název) |Identifikátor pro koncový bod. |
   | Privátní Port |Port pro přístup k síti interní do vaší aplikace. |
   | Protocol (Protokol) |Protokol, který používá přenosové vrstvy pro tento koncový bod, TCP nebo UDP. |
   | Veřejný port |Port, který se používá pro veřejný přístup k vaší aplikaci. |

## <a name="next-steps"></a>Další kroky
Další informace o používání Azure role v sadě Visual Studio najdete v tématu [pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md).

