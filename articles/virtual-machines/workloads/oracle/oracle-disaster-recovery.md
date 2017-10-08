---
title: "aaaOverview Oracle zotavení po havárii v prostředí Azure | Microsoft Docs"
description: "Na scénář zotavení po havárii pro databázi Oracle Database 12c v prostředí Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/2/2017
ms.author: rclaus
ms.openlocfilehash: 1fa69e1ba044b46b27695fec92fd9ca82df796f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-for-an-oracle-database-12c-database-in-an-azure-environment"></a>Zotavení po havárii pro databázi Oracle Database 12c v prostředí Azure

## <a name="assumptions"></a>Předpoklady

- Máte představu o Oracle Data Guard návrhu a prostředí Azure.


## <a name="goals"></a>Cíle
- Návrh hello topologie a konfigurace, které splňují vaše požadavky na zotavení po havárii.

## <a name="scenario-1-primary-and-dr-sites-on-azure"></a>Scénář 1: Primární a zotavení po Havárii weby na Azure

Zákazník má Oracle databáze sadu nahoru v primární lokalitě hello. Zotavení po Havárii A lokalita je v jiné oblasti. Zákazník Hello používá Oracle Data Guard pro rychlé obnovení mezi těmito lokalitami. primární lokality Hello má také sekundární databáze pro vytváření sestav a jiné účely. 

### <a name="topology"></a>topologie

Zde je souhrn hello instalace nástroje Azure:

- Dvěma lokalitami (primární lokalitou a lokalitou zotavení po Havárii)
- Dvě virtuální sítě
- Dvě databáze Oracle se Data Guard (primární i pohotovostní režim)
- Dvě databáze Oracle se Golden brány nebo Data Guard (pouze primární lokalita)
- Dvě aplikace služby, jeden primární a jeden v lokalitě hello zotavení po Havárii
- *Nastavení dostupnosti,* sloužící pro databázi a aplikace služby v primární lokalitě hello
- Jeden jumpbox v každé lokalitě, která omezuje přístup toohello privátní síti a umožňuje pouze přihlášení správce
- Jumpbox, služba aplikace, databáze a brány VPN oddělené podsítě
- Skupina NSG vynucená u aplikace a databáze podsítě

![Snímek obrazovky stránky topologie hello zotavení po Havárii](./media/oracle-disaster-recovery/oracle_topology_01.png)

## <a name="scenario-2-primary-site-on-premises-and-dr-site-on-azure"></a>Scénář 2: Primární lokality na místě a zotavení po Havárii webu v Azure

Zákazník má nastavení databáze Oracle místní (primární lokalita). Zotavení po Havárii A lokalita je v Azure. Oracle Data Guard se používá pro rychlé obnovení mezi těmito lokalitami. primární lokality Hello má také sekundární databáze pro vytváření sestav a jiné účely. 

Existují dva přístupy pro tento instalační program.

### <a name="approach-1-direct-connections-between-on-premises-and-azure-requiring-open-tcp-ports-on-hello-firewall"></a>Způsob 1: Přímé připojení mezi místními a Azure, vyžadování v hello firewall otevřete porty TCP 

Přímé připojení nedoporučujeme, protože potenciálně zobrazovat toohello porty TCP hello mimo world.

#### <a name="topology"></a>topologie

Následuje souhrn hello instalace nástroje Azure:

- Jeden DR lokality 
- Jedna virtuální síť
- Jedna databáze Oracle s Data Guard (aktivní)
- Jednu aplikaci služby v lokalitě hello zotavení po Havárii
- Jeden jumpbox, která omezuje přístup toohello privátní síti a umožňuje pouze přihlášení správce
- Jumpbox, služba aplikace, databáze a brány VPN oddělené podsítě
- Skupina NSG vynucená u aplikace a databáze podsítě
- Tooallow zásady nebo pravidla NSG příchozí TCP port 1521 (nebo na uživatelem definované port)
- Skupina NSG pravidlo pro zásady toorestrict pouze hello IP adresa nebo adresy místní (DB nebo aplikace) tooaccess hello virtuální sítě

![Snímek obrazovky stránky topologie hello zotavení po Havárii](./media/oracle-disaster-recovery/oracle_topology_02.png)

### <a name="approach-2-site-to-site-vpn"></a>Způsob 2: Site-to-site VPN
Site-to-site VPN je lepší přístup. Další informace o nastavení sítě VPN najdete v tématu [vytvoření virtuální sítě pomocí připojení Site-to-Site VPN pomocí rozhraní příkazového řádku](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).

#### <a name="topology"></a>topologie

Následuje souhrn hello instalace nástroje Azure:

- Jeden DR lokality 
- Jedna virtuální síť 
- Jedna databáze Oracle s Data Guard (aktivní)
- Jednu aplikaci služby v lokalitě hello zotavení po Havárii
- Jeden jumpbox, která omezuje přístup toohello privátní síti a umožňuje pouze přihlášení správce
- Jumpbox, služba aplikace, databáze a brány VPN jsou v samostatných podsítích
- Skupina NSG vynucená u aplikace a databáze podsítě
- Připojení Site-to-site VPN mezi místními a Azure

![Snímek obrazovky stránky topologie hello zotavení po Havárii](./media/oracle-disaster-recovery/oracle_topology_03.png)

## <a name="additional-reading"></a>Další čtení

- [Návrh a implementaci databáze Oracle na Azure](oracle-design.md)
- [Konfigurace Oracle Data Guard](configure-oracle-dataguard.md)
- [Konfigurace brány Golden Oracle](configure-oracle-golden-gate.md)
- [Oracle zálohování a obnovení](oracle-backup-recovery.md)


## <a name="next-steps"></a>Další kroky

- [Kurz: Vytvoření vysoce dostupné virtuální počítače](../../linux/create-cli-complete.md)
- [Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů](../../linux/cli-samples.md)
