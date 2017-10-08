---
title: "aaaSupportability přidávání stávající sadu dostupnosti virtuálních počítačů Azure tooan | Microsoft Docs"
description: "Možnosti podpory přidávání virtuálních počítačích Azure tooan stávající sadu dostupnosti."
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
ms.openlocfilehash: dc2bd86b916f1d1a0a0d4c9e870df829434c96b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="supportability-of-adding-azure-vms-tooan-existing-availability-set"></a>Možnosti podpory přidávání virtuálních počítačích Azure tooan stávající sadu dostupnosti

Někdy můžete setkat s omezení při přidání nové virtuální počítače (VM) tooan stávající sady dostupnosti. Následující Hello grafu podrobnosti hello které řady virtuálních počítačů je možné kombinovat ve stejné skupině dostupnosti.

Tady je hello sahat matice toomix různé typy virtuálních počítačů:

Ná & sady dostupnosti.|Druhý virtuální počítač|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|První virtuální počítač|||||||
|A||OK|OK|OK|OK|OK|
|Av2||OK|OK|OK|OK|OK|
|D||OK|OK|OK|OK|OK|
|Dv2||OK|OK|OK|OK|OK|
|Dv3||OK|OK|OK|OK|OK|

Všechny ostatní řady nelze v hello stejné dostupnosti nastavit, protože vyžadují konkrétní hardware.
