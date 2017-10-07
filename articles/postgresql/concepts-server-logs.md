---
title: "aaaServer protokolů v Azure databázi PostgreSQL | Microsoft Docs"
description: "Generuje protokoly dotazu a chyby v databázi Azure pro PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 22575f3882ce67fe640952f0a8dbfd8dcb4b16fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>Server v protokolech v Azure databázi PostgreSQL 
Generuje Azure databázi PostgreSQL dotazu a chybové protokoly. Přístup k protokolům tootransaction však není podporován. Tyto protokoly mohou být použité tooidentify, řešení potíží s a opravit chyby konfigurace a optimální výkon. Další informace najdete v tématu [Chyba protokolování a generování sestav](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Přístup k serveru protokolům
Můžete zobrazit seznam a stáhnout protokoly chyb serveru Azure PostgreSQL pomocí portálu Azure, hello [rozhraní příkazového řádku Azure](howto-configure-server-logs-using-cli.md) a rozhraní API REST Azure.

## <a name="log-retention"></a>Uchování protokolu
Můžete nastavit dobu uchování hello protokolů systému pomocí **protokolu\_uchování\_období** parametr přidružené k serveru. Hello jednotka pro tento parametr je dní (třeba tooconfirm). Hello výchozí hodnota je 3 dny. Maximální hodnota Hello je 7 dní. Všimněte si, že váš server musí mít dostatek hello toocontain úložiště přidělené zachovány soubory protokolu.
soubory protokolu Hello bude otočit každých 1 hodinu nebo velikost 100MB, nastane dříve.

## <a name="configure-logging-for-azure-postgresql-server"></a>Konfigurace protokolování pro server Azure PostgreSQL
Pro váš server můžete povolit protokolování dotazu a protokolování chyb. Protokoly chyb může obsahovat automaticky vakuu, připojení a informace o kontrolní body.

Můžete povolit protokolování dotazu pro instanci databáze PostgreSQL tak dva parametry serveru: protokolu\_prohlášení a protokolu\_min\_doba trvání\_příkaz.

Hello **protokolu\_příkaz** parametr řídí, jaké příkazy SQL se zaznamenávají. Doporučujeme, abyste nastavení tohoto parametru příliš***všechny*** toolog všechny příkazy; hello výchozí hodnota je žádný (třeba tooconfirm).

Hello **protokolu\_min\_doba trvání\_příkaz** parametr nastaví hello limit v milisekundách příkaz toobe, přihlášení. Všechny příkazy SQL, spuštěné déle než nastavení parametru hello přihlášeni. Tento parametr je zakázaná a nastavte toominus 1 (-1) ve výchozím nastavení (třeba tooconfirm). Povolení tento parametr může být užitečné v sledování dolů neoptimalizované dotazů ve svých aplikacích.

Hello **protokolu\_min\_zprávy** vám umožní toocontrol které zpráva úrovně se zapisují toohello serveru protokolu. Výchozí Hello je upozornění. (třeba tooconfirm)

Další informace o těchto nastaveních najdete v tématu [Chyba protokolování a generování sestav](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) dokumentaci. Zejména konfigurace databáze Azure pro PostgreSQL parametry serveru, najdete v části [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md).

## <a name="next-steps"></a>Další kroky
- tooaccess protokolů pomocí rozhraní příkazového řádku Azure CLI, najdete v části [konfigurace a přístup protokolů serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-logs-using-cli.md)
- Další informace o parametry serveru, najdete v části [přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md).