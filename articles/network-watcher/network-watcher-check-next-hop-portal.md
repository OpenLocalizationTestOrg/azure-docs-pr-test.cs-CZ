---
title: "Další směrování aaaFind s Azure sítě sledovacích procesů další segment - portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak můžete najít co hello dalšího směrování typ je a hello ip adresu pomocí další směrování pomocí portálu Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b459dcf-4077-424e-a774-f7bfa34c5975
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b64a2a5275c15aa8bdb10601de4ae1504a9ab551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-hello-portal"></a>Zjistěte, jaký typ hello dalšího směrování je díky funkci další segment hello v sledovací proces sítě Azure pomocí portálu hello

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Rozhraní API Azure REST](network-watcher-check-next-hop-rest.md)

Další směrování je funkce sledovací proces sítě, která poskytuje možnost hello získat hello typ dalšího směrování a IP adresu podle zadaný virtuální počítač. Tato funkce je užitečná při určování, zda je brána, internet nebo cílové tooits tooget virtuální sítě prochází přes odchozího provozu z virtuálního počítače.

## <a name="before-you-begin"></a>Než začnete

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě. scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.

## <a name="scenario"></a>Scénář

scénář Hello popsaná v tomto článku používá další směrování toofind hello typ dalšího směrování a IP adresu pro prostředek. toolearn Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).

V tomto scénáři provedete následující:

* Další směrování hello načíst z virtuálního počítače.

## <a name="get-next-hop"></a>Získat další směrování

### <a name="step-1"></a>Krok 1

Přejděte tooyour sledovací proces sítě prostředku v hello portálu Azure.

### <a name="step-2"></a>Krok 2

Klikněte na tlačítko **dalšího směrování** hello navigačním podokně, vyberte hello virtuálního počítače a síťové rozhraní, vyplňte hello zdrojové a cílové IP a klikněte na tlačítko hello **dalšího směrování** tlačítko.

> [!NOTE]
> Další směrování vyžaduje, aby hello prostředků virtuálního počítače je přidělená toorun.

![získat další směrování – přehled][1]

### <a name="step-3"></a>Krok 3

Po dokončení úloh hello jsou uvedené výsledky hello. Hello IP adresy a typ zařízení hello dalšího směrování je, se zobrazí. Hello následující tabulce jsou uvedeny dostupné vrácené hodnoty hello hello portálu.

**Typ dalšího směrování**

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Žádný

Pokud vlastní trasy použité tooroute tento provoz hello trasy definované uživatelem (UDR) se také zobrazí s výsledky hello.

![získat další směrování výsledky][2]

## <a name="next-steps"></a>Další kroky

Zjistěte, jak tooreview nastavení skupiny zabezpečení sítě prostřednictvím kódu programu přechodem [NSG auditování s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-check-next-hop-portal/figure1.png
[2]: ./media/network-watcher-check-next-hop-portal/figure2.png














