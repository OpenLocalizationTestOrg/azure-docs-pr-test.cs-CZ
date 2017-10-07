---
title: "aaaCheck připojení s sledovací proces sítě Azure - portálu Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak zkontrolovat připojení toouse s sledovací proces sítě pomocí hello portálu Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: gwallace
ms.openlocfilehash: ef6ecccd688f06f70003a5b59771c15bcbe8f3e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-hello-azure-portal"></a>Zkontrolujte připojení s sledovací proces sítě Azure pomocí hello portálu Azure

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [CLI 2.0](network-watcher-connectivity-cli.md)
> - [Rozhraní API Azure REST](network-watcher-connectivity-rest.md)

Zjistěte, jak by bylo možné navázat připojení tooverify toouse Pokud přímé připojení TCP z virtuálního počítače tooa, zadaný koncový bod.

## <a name="before-you-begin"></a>Než začnete

Tento článek předpokládá, že máte hello následující prostředky:

* Instance sledovací proces sítě v hello oblasti, kterou chcete toocheck připojení.

* Virtuální počítače připojení toocheck s.

> [!IMPORTANT]
> Kontrola připojení vyžaduje rozšíření virtuálního počítače `AzureNetworkWatcherExtension`. Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="check-connectivity-tooa-virtual-machine"></a>Zkontrolujte připojení k tooa virtuálního počítače

Tento příklad zkontroluje připojení tooa cílového virtuálního počítače přes port 80.

Přejděte tooyour sledovací proces sítě a klikněte na tlačítko **Kontrola připojení (Preview)**. Vyberte ze hello k toocheck virtuálního počítače. V hello **cílové** vyberte **vyberte virtuální počítač** a zvolte hello správný virtuální počítač a tootest portu.

Po kliknutí na tlačítko **zkontrolujte**, se kontroluje hello připojení mezi virtuálními počítači hello na zadaný port hello. V příkladu hello hello cílový počítač nedostupný, zobrazí se seznam všech segmentů směrování.

![Výsledky kontroly připojení pro virtuální počítač][1]

## <a name="check-remote-endpoint-connectivity"></a>Zkontrolujte připojení vzdáleného koncového bodu

toocheck hello připojení a latence tooa vzdálený koncový bod, vyberte hello **ručně zadejte** přepínač v hello **cílové** části, zadejte hello adresy url a hello port a klikněte na tlačítko **zkontrolujte** .  Používá se pro vzdálené koncové body jako koncové body weby a úložiště.

![Výsledky kontroly připojení pro webovou stránku][2]

## <a name="next-steps"></a>Další kroky

Zjistěte, jak zaznamená tooautomate paketů s výstrahami, virtuální počítač zobrazením [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)

Najít, pokud určité provoz je povolený v nebo z virtuálního počítače navštivte stránky [zkontrolujte IP tok ověření](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png
