---
title: "Úvod k připojení se změnami sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled možností připojení sledovací proces sítě"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.openlocfilehash: 16ceef9c923b6a933a5caf752991b466346e0ebc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-connectivity-check-in-azure-network-watcher"></a>Úvod k připojení k vrácení se změnami sledovací proces sítě Azure

Připojení k funkci sledovací proces sítě poskytuje možnost kontroly přímé připojení TCP z virtuálního počítače na virtuální počítač (VM), plně kvalifikovaný název domény (FQDN), identifikátor URI, nebo IPv4 adresu. Scénáře sítě jsou komplexní, jsou implementovány pomocí skupin zabezpečení sítě, brány firewall, trasy definované uživatelem a prostředky, které poskytuje Azure. Komplexní konfigurace zkontrolujte problémy s připojením k řešení potíží náročné. Sledovací proces sítě pomáhá snížit množství času, najít a rozpoznat problémy s připojením. Výsledky vrácené může poskytovat přehled o tom, jestli potíže s připojením je z důvodu platformu nebo o problém s konfigurací uživatele. Připojení může být kontrolována pomocí [prostředí PowerShell](network-watcher-connectivity-powershell.md), [rozhraní příkazového řádku Azure](network-watcher-connectivity-cli.md), a [REST API](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Kontrola připojení vyžaduje rozšíření virtuálního počítače `AzureNetworkWatcherExtension`. Instalaci rozšíření na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="response"></a>Odpověď

Následující tabulka uvádí vlastnosti vracený, když kontrolu připojení bylo dokončeno.

|Vlastnost  |Popis  |
|---------|---------|
|ConnectionStatus     | Stav zkontrolujte připojení. Možné výsledky **dostupné** a **Unreachable**.        |
|AvgLatencyInMs     | Průměrná latence při kontrole připojení v milisekundách. (Zobrazují pouze pokud je dostupný zkontrolujte stav)        |
|MinLatencyInMs     | Minimální latence při kontrole připojení v milisekundách. (Zobrazují pouze pokud je dostupný zkontrolujte stav)        |
|MaxLatencyInMs     | Maximální latence při kontrole připojení v milisekundách. (Zobrazují pouze pokud je dostupný zkontrolujte stav)        |
|ProbesSent     | Počet sondy odeslaných během kontroly. Maximální hodnota je 100.        |
|ProbesFailed     | Počet sondy, které během kontroly se nezdařilo. Maximální hodnota je 100.        |
|Směrování     | Směrování podle segmentu cesty ze zdroje do cíle.        |
|Segmentů směrování []. Typ     | Typ prostředku. Možné hodnoty jsou **zdroj**, **VirtualAppliance**, **VnetLocal**, a **Internet**.        |
|Segmentů směrování []. ID | Jedinečný identifikátor směrování.|
|Segmentů směrování []. Adresa | IP adresa směrování.|
|Segmentů směrování []. ID prostředku | ResourceID směrování, pokud je směrování je prostředek služby Azure. Pokud je zdroj v Internetu, je ResourceID **Internet**. |
|Segmentů směrování []. NextHopIds | Jedinečný identifikátor prováděné dalšího směrování.|
|Segmentů směrování []. Problémy | Kolekce problémy, které se vyskytly během kontroly v tomto segmentu. Pokud nebyly žádné problémy, hodnota je prázdná.|
|Segmentů směrování []. Vydá []. Počátek | V aktuální hop, kde došlo k potížím. Možné hodnoty:<br/> **Příchozí** -problém se na tento odkaz z předchozí směrování aktuální směrování<br/>**Odchozí** -problém se na tento odkaz z aktuální směrování dalšího směrování<br/>**Místní** -problém je v aktuálním segmentu.|
|Segmentů směrování []. Vydá []. Závažnost | Závažnost zjištěném problému. Možné hodnoty jsou **chyba** a **upozornění**. |
|Segmentů směrování []. Vydá []. Typ |Typ zjistil se problém. Možné hodnoty: <br/>**VYUŽITÍ PROCESORU**<br/>**Paměť**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|Segmentů směrování []. Vydá []. Kontext |Podrobnosti týkající se problém nalezeno.|
|Segmentů směrování []. Vydá []. Kontext [] s příponou Key |Vrátí klíč pár klíčových hodnot.|
|Segmentů směrování []. Vydá []. [] .value kontextu |Hodnota vrácená pár klíčových hodnot.|

Následuje příklad problém najít na směrování.

```json
"Issues": [
    {
        "Origin": "Outbound",
        "Severity": "Error",
        "Type": "NetworkSecurityRule",
        "Context": [
            {
                "key": "RuleName",
                "value": "UserRule_Port80"
            }
        ]
    }
]
```
## <a name="fault-types"></a>Typy chyb

Zkontrolujte připojení vrátí chybu typy o připojení. Následující tabulka obsahuje seznam selhání typů aktuální vrátila.

|Typ  |Popis  |
|---------|---------|
|Procesor     | Vysoké využití procesoru.       |
|Memory (Paměť)     | Vysoké využití paměti.       |
|GuestFirewall     | Přenos dat je blokován kvůli konfiguraci brány firewall virtuálního počítače.        |
|DNSResolution     | Nezdařil se překlad DNS pro cílovou adresu.        |
|NetworkSecurityRule    | Přenos dat je blokován nastavením pravidlo NSG (pravidlo je vrácen)        |
|UserDefinedRoute|Provoz je vyřazena z důvodu definované uživatelem nebo systémová trasa. |

### <a name="next-steps"></a>Další kroky

Zjistěte, jak ověřit připojení k prostředku, navštivte stránky: [zkontrolujte připojení s sledovací proces sítě Azure](network-watcher-connectivity-powershell.md).

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png

