---
title: "Ověřte aaaVerify provoz s tokem IP sledovací proces sítě Azure - portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toocheck Pokud tooor provoz z virtuálního počítače povolený nebo zakázaný"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e0e3e9a8-70eb-409a-a744-0ce9deb27148
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: abf639f36d32f3416dd927e66b635267b746e62f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Zkontrolujte, zda je provoz povolen nebo odepřen tooor z virtuálního počítače s tok ověření IP a součást sledovací proces sítě Azure

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Rozhraní API Azure REST](network-watcher-check-ip-flow-verify-rest.md)


Tok IP ověřte, že je funkce sledovací proces sítě, která umožňuje tooverify Pokud provoz je povolený tooor z virtuálního počítače. ověření Hello lze spustit pro příchozí nebo odchozí provoz. Tento scénář je užitečné tooget aktuálním stavu o tom, jestli virtuální počítač může kontaktovat tooan externí prostředek nebo jiný prostředek. Tok IP ověření může být použité tooverify, pokud vaše skupina zabezpečení sítě (NSG) pravidla jsou správně nakonfigurovány a řešení potíží s toky, kteří jsou Blokovaní pravidla NSG. Dalším důvodem pro použití IP tok ověření je tooensure přenosy, které chcete blokovat je správně blokován nastavením hello NSG.

## <a name="before-you-begin"></a>Než začnete

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě nebo existující instanci sledovací proces sítě. scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.

## <a name="scenario"></a>Scénář

Tento scénář používá tok ověření IP tooverify, pokud virtuální počítač může kontaktovat počítač tooanother přes port 443. Pokud je odepřená hello provoz, vrátí hello pravidlo zabezpečení, které je odepřen, aby provoz. toolearn Další informace o toku ověřit IP, navštivte [přehled tok ověření IP](network-watcher-ip-flow-verify-overview.md)

### <a name="run-ip-flow-verify"></a>Ověření spuštění toku IP

Přejděte tooyour sledovací proces sítě a klikněte na tlačítko **IP tok ověření**. Vyberte virtuální počítač hello a síťové rozhraní, které chcete tooverify provoz z. Zadejte veškeré další filtrování informace a klikněte na tlačítko **zkontrolujte**.

Po kliknutí na tlačítko **zkontrolujte**, se kontroluje hello toku na základě kritérií hello jste zadali. výsledek Hello je buď **povoleného přístupu** nebo **byl odepřen přístup**. Pokud byl odepřen přístup, hello skupina zabezpečení sítě (NSG) a pravidlo zabezpečení, která blokuje komunikaci, je k dispozici. Pokud hello odmítnutí provoz je očekávané chování, hello pravidlo bylo úspěšné.

> [!NOTE]
> Tok IP ověření vyžaduje, aby hello prostředků virtuálního počítače je přidělená.

Jak vidíte z hello následující bitové kopie, byla povolena hello odchozí komunikaci přes protokol HTTPS.

![tok IP ověřte – přehled][1]

Jak je vidět v hello následující bitové kopie, provoz mění tooinbound a hello příchozí too123 port změnit. Provoz je nyní odepřen, uvítací zprávu "Přístup byl odepřen" je k dispozici společně s hello pravidla zabezpečení sítě skupiny a zabezpečení, zakážou provoz hello.

![výsledky toku IP][2]

## <a name="next-steps"></a>Další kroky

Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které jsou definovány.

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













