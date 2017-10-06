---
title: "zachycení tooPacket aaaIntroduction v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled funkce zachytávání paketů sledovací proces sítě hello"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3a81afaa-ecd9-4004-b68e-69ab56913356
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 2ce01b391b9c1a1c19aa29c8620628c55586df03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toovariable-packet-capture-in-azure-network-watcher"></a>Úvod toovariable zachytáváním paketů v sledovací proces sítě Azure

Zachycení dat ze sítě sledovacích procesů proměnné paketů umožňuje zaznamenat toocreate paketů relace tootrack tooand na provoz z virtuálního počítače. Zachytáváním paketů pomáhá sítě anomálií toodiagnose obou reaktivně a proactivity. Mezi další použití patří shromažďování statistiku sítě, získá informace o síti vniknutí, toodebug klient server komunikace a mnoho dalšího.

Zachytáváním paketů je rozšíření virtuálního počítače, který se spustil vzdáleně přes sledovací proces sítě. Tato funkce snižuje zátěž hello spuštěných zachytáváním paketů ručně na hello potřeby virtuální počítač, který úspora času. Zachytáváním paketů můžete spustit prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API. Je jeden příklad, jak můžete spustit zachytáváním paketů s výstrahami virtuálního počítače. Filtry jsou k dispozici pro hello zaznamenání relace tooensure můžete zaznamenávat provoz, které chcete toomonitor. Filtry jsou založené na 5-n-tice (protokol, místní IP adresu, vzdálené IP adresy, místního portu a vzdáleného portu) informace. Hello zachytit data se ukládají v místním disku hello nebo objekt blob úložiště. Existuje omezení 10 zachytávání relací paketů na oblast na předplatné. Toto omezení platí pouze toohello relace a doporučení se netýká toohello uložené soubory zachytávání paketů místně na hello virtuálních počítačů nebo v účtu úložiště.

> [!IMPORTANT]
> Rozšíření virtuálního počítače vyžaduje zachytáváním paketů `AzureNetworkWatcherExtension`. Instaluje se rozšíření hello na virtuální počítač s Windows najdete v článku [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md) a u virtuálního počítače s Linuxem, navštivte [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux](../virtual-machines/linux/extensions-nwa.md).

informace o hello tooreduce zaznamenáte tooonly hello informace, které chcete, hello jsou dostupné následující možnosti pro relaci zachytávání paketů:

**Zachycení konfigurace**

|Vlastnost|Popis|
|---|---|
|**Maximální počet bajtů paketu (bajty)** | Hello počet bajtů z jednotlivých paketů, které jsou zachyceny, všechny bajty zachyceny, pokud je ponecháno prázdné. Hello počet bajtů z jednotlivých paketů, které jsou zachyceny, všechny bajty zachyceny, pokud je ponecháno prázdné. Pokud budete potřebovat pouze hlavičce IPv4 hello – označuje 34 sem |
|**Maximální počet bajtů za relace (bajty)** | Celkový počet bajtů v tom, že jsou zachyceny po dosažení neukončí relace hello hello hodnoty.|
|**Časový limit (sekundy)** | Sady času omezení hello paketu zaznamenání relace. Hello výchozí hodnota je 18000 sekund nebo 5 hodin.|

**Filtrování (volitelné)**

|Vlastnost|Popis|
|---|---|
|**Protokol** | zaznamenat Hello protokol toofilter pro paket hello. Hello dostupné jsou hodnoty TCP, UDP a všechny.|
|**Místní IP adresu** | Tato hodnota filtruje toopackets zachytávání paketů hello, kde hello místní IP adresa odpovídá této hodnotě filtru.|
|**Místního portu** | Tato hodnota filtry hello paketu zachytit toopackets kde místního portu hello odpovídá této hodnotě filtru.|
|**Vzdálené IP adresy** | Tato hodnota filtry hello paketu zachytit toopackets kde hello vzdálené IP odpovídá této hodnotě filtru.|
|**Vzdálený port** | Tato hodnota filtry hello paketu zachytit toopackets kde vzdálených portů hello odpovídá této hodnotě filtru.|

### <a name="next-steps"></a>Další kroky

Zjistěte, jak můžete spravovat zachycení paketů přes portál hello navštivte stránky [spravovat zachytáváním paketů v hello portál Azure](network-watcher-packet-capture-manage-portal.md) nebo pomocí prostředí PowerShell, navštivte stránky [spravovat pomocí prostředí PowerShell zachytávání paketů](network-watcher-packet-capture-manage-powershell.md).

Zjistěte, jak proaktivní paketu toocreate zaznamená založeny na výstrahách virtuálního počítače, navštivte stránky [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













