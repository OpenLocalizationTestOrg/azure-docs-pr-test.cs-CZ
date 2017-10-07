---
title: "provoz aaaVerify s Azure sítě sledovacích procesů IP toku ověřit - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toocheck Pokud tooor provoz z virtuálního počítače povolený nebo zakázaný pomocí rozhraní příkazového řádku Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 92b857ed-c834-4c1b-8ee9-538e7ae7391d
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: c6becc5c142837b04d15490b2b3bd11124434570
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


Tok IP ověřte je funkce sledovací proces sítě, která umožňuje tooverify Pokud provoz je povolený tooor z virtuálního počítače. Tento scénář je užitečné tooget aktuálním stavu o tom, jestli virtuální počítač může kontaktovat tooan externí prostředek nebo back-end. Tok IP ověření může být použité tooverify, pokud vaše skupina zabezpečení sítě (NSG) pravidla jsou správně nakonfigurovány a řešení potíží s toky, kteří jsou Blokovaní pravidla NSG. Dalším důvodem pro použití IP tok ověření je tooensure přenosy, které chcete blokovat je správně blokován nastavením hello NSG.

Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.

## <a name="before-you-begin"></a>Než začnete

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě nebo existující instanci sledovací proces sítě. scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.

## <a name="scenario"></a>Scénář

Tento scénář používá tok ověření IP tooverify, pokud virtuální počítač může kontaktovat tooa známé Bing IP adresu. Pokud je odepřená hello provoz, vrátí hello pravidlo zabezpečení, které je odepřen, aby provoz. toolearn Další informace o toku ověřit IP, navštivte [přehled tok ověření IP](network-watcher-ip-flow-verify-overview.md)


## <a name="get-a-vm"></a>Získat virtuální počítač

Tok IP ověřte testy tooor provoz z IP adresy na virtuální počítač tooor z vzdálený cíl. Id virtuálního počítače je vyžadováno pro rutinu hello. Pokud už znáte ID hello toouse hello virtuálního počítače, můžete tento krok přeskočit.

```
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="get-hello-nics"></a>Získat hello síťové karty

v tomto příkladu, nemůžeme načíst hello síťové adaptéry na virtuálním počítači, je potřeba Hello IP adresa síťového adaptéru na virtuálním počítači hello. Pokud už znáte hello IP adresu, kterou chcete tootest hello virtuálního počítače, můžete tento krok přeskočit.

```
azure network nic show -g resourceGroupName -n nicName
```

## <a name="run-ip-flow-verify"></a>Ověření spuštění toku IP

Teď, když máme hello informace potřebné toorun hello rutiny, spustíme hello `network watcher ip-flow-verify` rutiny tootest hello provoz. V tomto příkladu používáme hello první IP adresou na síťovém adaptéru hello první.

```
azure network watcher ip-flow-verify -g resourceGroupName -n networkWatcherName -t targetResourceId -d directionInboundorOutbound -p protocolTCPorUDP -o localPort -m remotePort -l localIpAddr -r remoteIpAddr
```

> [!NOTE]
> Tok IP ověření vyžaduje, aby hello prostředků virtuálního počítače je přidělená toorun.

## <a name="review-results"></a>Zkontrolujte výsledky

Po spuštění `network watcher ip-flow-verify` hello výsledky se vrátí, hello následující příklad je hello výsledky vrácené z hello předchozím kroku.

```
data:    Access                          : Deny
data:    Rule Name                       : defaultSecurityRules/DefaultInboundDenyAll
info:    network watcher ip-flow-verify command OK
```

## <a name="next-steps"></a>Další kroky

Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které jsou definovány.

Další nastavení NSG tooaudit navštivte stránky [auditování zabezpečení sítě skupiny (NSG) s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md).

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
