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
ms.date: 05/10/2017
ms.openlocfilehash: 10f6c490a4fdb4c70cb80b035eaebb9d2ad2e699
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>Server v protokolech v Azure databázi PostgreSQL 
Generuje Azure databázi PostgreSQL dotazu a chybové protokoly. Přístup k protokoly transakcí však není podporován. Tyto protokoly můžete použít k identifikaci, řešení potíží a opravte chyby konfigurace a optimální výkon. Další informace najdete v tématu [Chyba protokolování a generování sestav](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Přístup k serveru protokolům
Můžete zobrazit seznam a stáhnout protokoly chyb serveru Azure PostgreSQL pomocí portálu Azure [rozhraní příkazového řádku Azure](howto-configure-server-logs-using-cli.md) a rozhraní API REST Azure.

## <a name="log-retention"></a>Uchování protokolu
Můžete nastavit dobu uchování pro protokolů systému pomocí **protokolu\_uchování\_období** parametr přidružené k serveru. Jednotka pro tento parametr je dní (třeba k potvrzení). Výchozí hodnota je tři dny. Maximální hodnota je 7 dní. Všimněte si, že váš server musí mít dostatek přidělené úložiště tak, aby obsahovala zachované protokolové soubory.
Soubory protokolu se otočit každých 1 hodinu nebo velikost 100MB, nastane dříve.

## <a name="configure-logging-for-azure-postgresql-server"></a>Konfigurace protokolování pro server Azure PostgreSQL
Pro váš server můžete povolit protokolování dotazu a protokolování chyb. Protokoly chyb může obsahovat automaticky vakuu, připojení a informace o kontrolní body.

Můžete povolit protokolování dotazu pro instanci databáze PostgreSQL tak dva parametry serveru: protokolu\_prohlášení a protokolu\_min\_doba trvání\_příkaz.

**Protokolu\_příkaz** parametr řídí, jaké příkazy SQL se zaznamenávají. Doporučujeme, abyste nastavení tohoto parametru na ***všechny*** do protokolu všechny příkazy; výchozí hodnota je žádný (třeba k potvrzení).

**Protokolu\_min\_doba trvání\_příkaz** parametr nastaví limit v milisekundách příkazu zaznamenávat do protokolu. Všechny příkazy SQL, které se spouštět déle než nastavení pro parametr přihlášeni. Tento parametr je zakázána a nastavena na minus 1 (-1) ve výchozím nastavení (třeba k potvrzení). Povolení tento parametr může být užitečné v sledování dolů neoptimalizované dotazů ve svých aplikacích.

**Protokolu\_min\_zprávy** umožňuje můžete řídit, která zprávy úrovně se zapisují do protokolu serveru. Výchozí hodnota je upozornění. (musí potvrdit)

Další informace o těchto nastaveních najdete v tématu [Chyba protokolování a generování sestav](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) dokumentaci. Zejména konfigurace databáze Azure pro PostgreSQL parametry serveru, najdete v části [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md).

## <a name="next-steps"></a>Další kroky
- Přístup k protokolům pomocí rozhraní příkazového řádku Azure CLI, najdete v části [konfigurace a přístup protokolů serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-logs-using-cli.md)
- Další informace o parametry serveru, najdete v části [přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md).