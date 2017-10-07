---
title: "tok tooIP aaaIntroduction ověřit v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled o hello IP sledovací proces sítě ověřte tok funkce"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b648a4816a7ffdc6ca54462944b574e2395e8298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooip-flow-verify-in-azure-network-watcher"></a>Úvod tooIP toku ověřit v sledovací proces sítě Azure

Tok IP ověřte kontroluje, zda paket povolený nebo zakázaný tooor z virtuálního počítače na základě informací o 5 řazené kolekce členů. Tyto informace se skládá z směr, protokol, místní IP, vzdálené IP, místního portu a vzdáleného portu. Pokud paket hello je zakázané skupiny zabezpečení, je vrácen název hello hello pravidlo, které odepřen hello paketů. Zatímco můžete vybrat všechny zdrojové i cílové adresy IP, tato funkce vám pomůže rychle diagnostikovat problémy s připojením z nebo toohello správci internet a z nebo toohello v místním prostředí.

Tok IP ověřte cílem síťové rozhraní virtuálního počítače. Tok přenosů dat je pak ověřit podle hello nakonfigurované nastavení tooor z rozhraní sítě. Tato možnost je užitečná při potvrzení, zda pravidla v skupinu zabezpečení sítě neblokuje inzerování vstupních nebo výstupních tooor provoz z virtuálního počítače.

Instance toobe potřebám sledovací proces sítě vytvořené v všech oblastech, abyste naplánovali toorun IP tok ověření. Sledovací proces sítě je služba, místní a může být spustili jenom s prostředky v hello stejné oblasti. To nemá vliv hello výsledky IP toku ověřte jako hello trasy přidružené hello síťový adaptér bude stále vrácen.

![1][1]

## <a name="next-steps"></a>Další kroky

Navštivte hello následující článek toolearn, pokud je paket povolený nebo zakázaný pro konkrétní virtuální počítač prostřednictvím portálu hello. [Zkontrolovat, pokud provoz je povolené na virtuálním počítači s IP tok ověření pomocí portálu hello](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












