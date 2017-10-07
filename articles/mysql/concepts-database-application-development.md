---
title: "Přehled vývoje aplikace aaaDatabase pro databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Zavádí aspekty návrhu, které vývojář při psaní aplikace kódu tooconnect tooAzure databáze pro databázi MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: f08df605eba21b4ba4b43565c0a7ded95779a171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Přehled vývoje aplikací pro databázi Azure pro databázi MySQL 
Tento článek popisuje aspekty návrhu, které vývojář při psaní aplikace kódu tooconnect tooAzure databáze pro databázi MySQL 

> [!TIP]
> Kurz znázorňující, jak vytvořit toocreate serveru, brána firewall na serveru, zobrazení vlastností serveru, vytvoříte databázi, připojení a dotazování pomocí workbench a mysql.exe, najdete v části [navrhnout první databáze MySQL na Azure](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Jazyk a platforma
K dispozici jsou ukázky kódu pro různé programovací jazyky a platformy. Odkazy můžete najít ukázky kódu toohello v: [používat knihovny připojení tooconnect tooAzure databáze pro databázi MySQL](concepts-connection-libraries.md)

## <a name="tools"></a>Nástroje
MySQL community hello, verze kompatibilní s MySQL běžné nástroje pro správu jako je například nástroje Workbench nebo MySQL, jako je například mysql.exe, používá Azure databáze pro databázi MySQL [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql)a další. Můžete taky hello portálu Azure, rozhraní příkazového řádku Azure a rozhraní REST API toointeract službou hello databáze.

## <a name="resource-limitations"></a>Omezení prostředků
Databáze MySQL Azure spravuje hello prostředky k dispozici tooa serveru pomocí dvou různých mechanismů: 
- Zásady správného řízení prostředků 
- Vynucení omezení.

## <a name="security"></a>Zabezpečení
Databáze MySQL Azure poskytuje prostředky pro omezení přístupu, ochranu dat, konfigurace uživatele a role a monitorování aktivit v databázi MySQL.

## <a name="authentication"></a>Authentication
Databáze MySQL Azure podporuje serveru ověřování uživatelů a přihlášení.

## <a name="resiliency"></a>Odolnost
V případě přechodná chyba při připojování tooMySQL databáze by měl váš kód opakujte volání hello. Doporučujeme použít logiku opakování hello stáhnul logiku, tak, aby ji není zahlcovat hello databázi SQL s více klienty najednou opakování.

- Ukázky kódu jsou: ukázky kódu, které ilustrují logika opakovaných pokusů, najdete v části Ukázky pro jazyk hello zvoleného v: [používat knihovny připojení tooconnect tooAzure databáze pro databázi MySQL](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Správa připojení
Databázová připojení jsou omezené prostředku, proto doporučujeme rozumný použití připojení při přístupu k databázi MySQL tooachieve lepší výkon.
- Databáze Access hello pomocí sdružování připojení nebo trvalé připojení.
- Databáze Access hello pomocí připojení krátkou životnost. 
- Logika opakovaných pokusů pro použití ve vaší aplikaci v okamžiku hello hello pokusu o připojení, toocatch selhání kvůli tooconcurrent připojení bylo dosaženo maximální povolený počet hello. V hello logika opakovaných pokusů, nastavte chvíli trvat a potom počkejte, než pro náhodné dobu před pokusy o hello další připojení.
