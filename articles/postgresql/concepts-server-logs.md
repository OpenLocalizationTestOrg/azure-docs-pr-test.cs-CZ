---
title: "Server v protokolech v Azure databázi PostgreSQL | Microsoft Docs"
description: "Generuje protokoly dotazu a chyby v databázi Azure pro PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 696af85cd5609171a719a7e77efbfcdeba0aaaaa
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>Server v protokolech v Azure databázi PostgreSQL 
Generuje Azure databázi PostgreSQL dotazu a chybové protokoly. Přístup k protokoly transakcí však není podporován. Protokoly dotazu a chyba lze identifikovat, odstraňování potíží a opravte chyby konfigurace a zhoršené výkonu. Další informace najdete v tématu [Chyba protokolování a generování sestav](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Přístup k protokolům serveru
Můžete zobrazit seznam a stáhnout protokoly chyb serveru Azure PostgreSQL pomocí portálu Azure [rozhraní příkazového řádku Azure](howto-configure-server-logs-using-cli.md)a rozhraní API REST Azure.

## <a name="log-retention"></a>Uchování protokolu
Můžete nastavit dobu uchování pro protokolů systému pomocí **protokolu\_uchování\_období** parametr přidružené k serveru. Jednotka pro tento parametr je dny. Výchozí hodnota je 3 dny. Maximální hodnota je 7 dní. Váš server musí mít dostatek přidělené úložiště tak, aby obsahovala zachované protokolové soubory.
Soubory protokolů otočit každou hodinu nebo velikost 100 MB, nastane dříve.

## <a name="configure-logging-for-azure-postgresql-server"></a>Konfigurace protokolování pro server Azure PostgreSQL
Pro váš server můžete povolit protokolování dotazu a protokolování chyb. Protokoly chyb může obsahovat informace o automatické vakuu, připojení a kontrolní body.

Můžete povolit protokolování dotazu pro instanci databáze PostgreSQL tak dva parametry serveru: `log\_statement` a `log\_min\_duration\_statement`.

**Protokolu\_příkaz** parametr řídí, jaké příkazy SQL se zaznamenávají. Doporučujeme, abyste nastavení tohoto parametru na ***všechny*** do protokolu všechny příkazy; výchozí hodnota je none.

**Protokolu\_min\_doba trvání\_příkaz** parametr nastaví limit v milisekundách příkazu zaznamenávat do protokolu. Všechny příkazy SQL, které se spouštět déle než nastavení pro parametr přihlášeni. Tento parametr je zakázána a nastavena na minus 1 (-1) ve výchozím nastavení. Povolení tento parametr může být užitečné v sledování dolů neoptimalizované dotazů ve svých aplikacích.

**Protokolu\_min\_zprávy** umožňuje můžete řídit, která zprávy úrovně se zapisují do protokolu serveru. Výchozí hodnota je upozornění. 

Další informace o těchto nastaveních najdete v tématu [Chyba protokolování a generování sestav](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) dokumentaci. Zejména konfigurace databáze Azure pro PostgreSQL parametry serveru, najdete v části [přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md).

## <a name="next-steps"></a>Další kroky
- Přístup k protokolům pomocí rozhraní příkazového řádku Azure CLI, najdete v tématu [konfigurace a přístup protokolů serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-logs-using-cli.md).
- Další informace o parametry serveru, najdete v části [přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md).
