---
title: "Nastavit možnosti přidávání virtuálních počítačích Azure do existující dostupnosti | Microsoft Docs"
description: "Možnosti podpory přidávání virtuálních počítačích Azure do stávající sadu dostupnosti."
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/15/2017
ms.author: delhan
ms.openlocfilehash: 3ce9b8a79108cb9e57df14bcb3354cc637193233
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Možnosti podpory přidávání virtuálních počítačích Azure do stávající sadu dostupnosti

Někdy můžete setkat s omezení při přidání nových virtuálních počítačů (VM) do stávající sady dostupnosti. Následující graf zobrazuje podrobnosti o které řady virtuálních počítačů je možné kombinovat ve stejné sadě dostupnosti.

Tady je podpoře matice do kombinovat různé typy virtuálních počítačů:

Ná & sady dostupnosti.|Druhý virtuální počítač|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|První virtuální počítač|||||||
|A||OK|OK|OK|OK|OK|
|Av2||OK|OK|OK|OK|OK|
|D||OK|OK|OK|OK|OK|
|Dv2||OK|OK|OK|OK|OK|
|Dv3||OK|OK|OK|OK|OK|

Všechny ostatní řady nemůže být ve stejné dostupnosti nastavit, protože vyžadují konkrétní hardware.