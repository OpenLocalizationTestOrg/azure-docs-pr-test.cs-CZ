---
title: "aaaIntroduction tooconnectivity změnami sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled hello možností připojení sledovací proces sítě"
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
ms.date: 07/11/2017
ms.author: gwallace
ms.openlocfilehash: 52fc4547f167cea2992a046859dc0550d136e80d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooconnectivity-check-in-azure-network-watcher"></a>Úvod tooconnectivity změnami sledovací proces sítě Azure

Hello připojení funkce sledovací proces sítě poskytuje schopnost toocheck hello přímé připojení TCP z virtuálního počítače tooa virtuálního počítače (VM), plně kvalifikovaný název domény (FQDN), identifikátor URI, nebo IPv4 adresu. Scénáře sítě jsou komplexní, jsou implementovány pomocí skupin zabezpečení sítě, brány firewall, trasy definované uživatelem a prostředky, které poskytuje Azure. Komplexní konfigurace zkontrolujte problémy s připojením k řešení potíží náročné. Sledovací proces sítě pomáhá snížit hello množství času toofind a rozpoznat problémy s připojením. Hello výsledky vrácené může poskytovat přehled o tom, jestli potíže s připojením je z důvodu tooa platformy nebo o problém s konfigurací uživatele. Připojení může být kontrolována pomocí [prostředí PowerShell](network-watcher-connectivity-powershell.md), [rozhraní příkazového řádku Azure](network-watcher-connectivity-cli.md), a [REST API](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> Kontrola připojení vyžaduje rozšíření virtuálního počítače `AzureNetworkWatcherExtension`. Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="response"></a>Odpověď

Hello následující tabulka zobrazuje vlastnosti hello vracený, když kontrolu připojení bylo dokončeno.

|Vlastnost  |Popis  |
|---------|---------|
|ConnectionStatus     | Hello stav kontroly hello připojení. Možné výsledky **dostupné** a **Unreachable**.        |
|AvgLatencyInMs     | Průměrná latence během kontroly hello připojení v milisekundách. (Zobrazují pouze pokud je dostupný zkontrolujte stav)        |
|MinLatencyInMs     | Minimální latence během připojení hello zkontrolujte v milisekundách. (Zobrazují pouze pokud je dostupný zkontrolujte stav)        |
|MaxLatencyInMs     | Maximální latence během připojení hello zkontrolujte v milisekundách. (Zobrazují pouze pokud je dostupný zkontrolujte stav)        |
|ProbesSent     | Počet odeslaných během kontroly hello sondy. Maximální hodnota je 100.        |
|ProbesFailed     | Počet sondy, kteří nevyhověli při kontrole hello. Maximální hodnota je 100.        |
|Směrování     | Směrování podle segmentu cesty ze zdroje toodestination.        |
|Segmentů směrování []. Typ     | Typ prostředku. Možné hodnoty jsou **zdroj**, **VirtualAppliance**, **VnetLocal**, a **Internet**.        |
|Segmentů směrování []. ID | Jedinečný identifikátor hello směrování.|
|Segmentů směrování []. Adresa | IP adresa hello směrování.|
|Segmentů směrování []. ID prostředku | ResourceID hello směrování, pokud směrování hello je prostředek služby Azure. Pokud je zdroj v Internetu, je ResourceID **Internet**. |
|Segmentů směrování []. NextHopIds | Hello jedinečný identifikátor hello provedeny další směrování.|
|Segmentů směrování []. Problémy | Kolekce problémy, které se vyskytly během kontroly hello v tomto segmentu. Pokud nebyly žádné problémy, hello hodnota je prázdná.|
|Segmentů směrování []. Vydá []. Počátek | V hello aktuální směrování, kde došlo k potížím. Možné hodnoty:<br/> **Příchozí** -problém je na hello odkaz z hello předchozí směrování toohello aktuální směrování<br/>**Odchozí** -problém je na hello odkaz z hello aktuální směrování toohello další směrování<br/>**Místní** -problém je v aktuálním segmentu hello.|
|Segmentů směrování []. Vydá []. Závažnost | závažnost Hello hello zjištěném problému. Možné hodnoty jsou **chyba** a **upozornění**. |
|Segmentů směrování []. Vydá []. Typ |Typ Hello zjistil se problém. Možné hodnoty: <br/>**VYUŽITÍ PROCESORU**<br/>**Paměť**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|Segmentů směrování []. Vydá []. Kontext |Podrobnosti týkající se zjistil se problém hello.|
|Segmentů směrování []. Vydá []. Kontext [] s příponou Key |Klíč pár klíčových hodnot hello vrácený.|
|Segmentů směrování []. Vydá []. [] .value kontextu |Hodnota vrácená pár klíčových hodnot hello.|

Hello následuje příklad problém najít na směrování.

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

Zkontrolujte připojení Hello vrací chybu typů o hello připojení. Hello následující tabulka obsahuje seznam hello aktuální vrátil typů chyb.

|Typ  |Popis  |
|---------|---------|
|Procesor     | Vysoké využití procesoru.       |
|Memory (Paměť)     | Vysoké využití paměti.       |
|GuestFirewall     | Přenos dat je blokován kvůli konfiguraci brány firewall tooa virtuálního počítače.        |
|DNSResolution     | Nezdařil se překlad DNS pro cílovou adresu hello.        |
|NetworkSecurityRule    | Přenos dat je blokován nastavením pravidlo NSG (pravidlo je vrácen)        |
|UserDefinedRoute|Provoz je vyřazena z důvodu definované uživatelem tooa nebo systémová trasa. |

### <a name="next-steps"></a>Další kroky

Zjistěte, jak tooverify připojení tooa prostředků navštivte stránky: [zkontrolujte připojení s sledovací proces sítě Azure](network-watcher-connectivity-powershell.md).

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png

