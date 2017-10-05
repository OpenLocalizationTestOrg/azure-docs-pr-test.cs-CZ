---
title: "Přehled vývoje aplikace databáze pro databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Zavádí aspekty návrhu, které vývojář při psaní kódu pro aplikace pro připojení k databázi Azure pro databázi MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 350dd775e172120d806d1193877a34d94f4d3f6a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Přehled vývoje aplikací pro databázi Azure pro databázi MySQL 
Tento článek popisuje aspekty návrhu, které vývojář při psaní kódu pro aplikace pro připojení k databázi Azure pro databázi MySQL 

> [!TIP]
> Kurz ukazuje, jak vytvořit server, vytvoření brány firewall na serveru, zobrazení vlastností serveru, vytvořit databázi, připojení a dotazování pomocí workbench a mysql.exe, najdete v části [navrhnout první databáze MySQL na Azure](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Jazyk a platforma
K dispozici jsou ukázky kódu pro různé programovací jazyky a platformy. Můžete najít odkazy na ukázky kódu v: [připojení knihovny používané pro připojení k databázi Azure pro databázi MySQL](concepts-connection-libraries.md)

## <a name="tools"></a>Nástroje
MySQL community verze, kompatibilní s MySQL běžné nástroje pro správu jako je například nástroje Workbench nebo MySQL, jako je například mysql.exe, používá Azure databáze pro databázi MySQL [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql)a další. Také můžete portál Azure, rozhraní příkazového řádku Azure a rozhraní REST API pro interakci s službu databáze.

## <a name="resource-limitations"></a>Omezení prostředků
Databáze MySQL Azure spravuje prostředky k dispozici pro server pomocí dvou různých mechanismů: 
- Zásady správného řízení prostředků 
- Vynucení omezení.

## <a name="security"></a>Zabezpečení
Databáze MySQL Azure poskytuje prostředky pro omezení přístupu, ochranu dat, konfigurace uživatele a role a monitorování aktivit v databázi MySQL.

## <a name="authentication"></a>Authentication
Databáze MySQL Azure podporuje serveru ověřování uživatelů a přihlášení.

## <a name="resiliency"></a>Odolnost
Dojde-li k přechodné chybě při připojování k databázi MySQL, by měl váš kód opakujte volání. Doporučujeme použít logiku opakování regrese logiku, tak, aby ji není zahlcovat databázi SQL s více klienty najednou opakování.

- Ukázky kódu jsou: ukázky kódu, které ilustrují logika opakovaných pokusů, najdete v části Ukázky pro jazyk podle vašeho výběru v: [připojení knihovny používané pro připojení k databázi Azure pro databázi MySQL](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Správa připojení
Databázová připojení jsou omezené prostředku, proto doporučujeme rozumný použití připojení při přístupu k databázi MySQL můžete dosáhnout lepšího výkonu.
- Přístup k databázi pomocí sdružování připojení nebo trvalé připojení.
- Přístup k databázi pomocí připojení krátkou životnost. 
- Použít logika opakovaných pokusů v aplikaci v místě pokus o připojení, k zachycení chyb z důvodu souběžných připojení bylo dosaženo maximální povolený počet. V logice opakování nastavit chvíli trvat a potom počkejte, než pro náhodné dobu před pokusy o další připojení.