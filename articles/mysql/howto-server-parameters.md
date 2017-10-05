---
title: "Postup konfigurace serveru parametry v databáze Azure pro databázi MySQL | Microsoft Docs"
description: "Tento článek popisuje, jak nakonfigurovat parametry k dispozici serveru v Azure Database pro databázi MySQL pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/19/2017
ms.openlocfilehash: 718bbf359253849fbc989c563ffd6d1099f92348
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-using-the-azure-portal"></a>Postup konfigurace serveru parametry v Azure Database pro databázi MySQL pomocí portálu Azure

Azure databáze pro databázi MySQL podporuje konfiguraci některé parametry serveru. Tento článek popisuje, jak konfigurovat tyto parametry pomocí portálu Azure a uvádí podporované parametry, výchozí hodnoty a rozsah platných hodnot. Ne všechny parametry serveru můžete upravit. Jsou podporovány pouze ty, které jsou zde uvedeny.

## <a name="navigate-to-server-parameters-blade-on-azure-portal"></a>Přejděte do okna parametry serveru, na portálu Azure

Přihlaste se k portálu Azure a pak klikněte na vaší databázi Azure pro název serveru databáze MySQL. V části **nastavení** klikněte na tlačítko **parametry serveru** otevřete okno Parametry Server pro databázi Azure pro databázi MySQL.

![Okno Parametry server portálu Azure](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Seznam parametrů konfigurovat server

Následující tabulka uvádí aktuálně podporovaný server s parametry. Parametry lze nakonfigurovat podle požadavků vaší aplikace.

> [!div class="mx-tableFixed"]
|Název parametru|Výchozí hodnota|rozsah|Popis|
|---|---|---|---|
|binlog_group_commit_sync_delay|1000|0, 11-1000000|Ovládací prvky kolik mikrosekundách potvrzení binární protokol čeká, než synchronizace binární soubor protokolu na disk.|
|binlog_group_commit_sync_no_delay_count|0|0-1000000|Maximální počet transakcí čekat před ukončením aktuální zpoždění podle specifikace binlog skupiny – potvrzení sync-delay.|
|character_set_server|LATIN1|BIG5, UTF8MB4, atd.|Použijte charset_name jako výchozí server znakovou sadu.|
|div_precision_increment|4|0-30|Počet číslic, podle kterého chcete zvětšit proporce výsledek operace dělení.|
|group_concat_max_len|1024|4-16777216|Maximální povolená délka výsledek v bajtech pro GROUP_CONCAT().|
|innodb_adaptive_hash_index|ON|VYPNUTÝ|Indexy hash adaptivní innodb jestli jsou povolená nebo zakázaná.|
|innodb_lock_wait_timeout|50|1-3600|Dobu v sekundách transakci InnoDB čeká na zámek řádků, než zkoušet.|
|interactive_timeout|1800|10-1800|Počet sekund, po kterou server čeká aktivity na připojení k interaktivní před zavřením.|
|log_bin_trust_function_creators|OFF|VYPNUTÝ|Tato proměnná platí, pokud je povoleno binární protokolování. Určuje, zda uložené funkce creators důvěryhodné nevytvářet uložených funkcí, které způsobují nezabezpečené události zapisovaly do binárního protokolu.|
|log_queries_not_using_indexes|OFF|VYPNUTÝ|Zaznamená dotazy, které se očekává, že načíst všechny řádky do protokolu pomalé dotazu.|
|log_slow_admin_statements|OFF|VYPNUTÝ|V příkazech zapíšou do protokolu pomalé dotazu zahrňte pomalé příkazy pro správu.|
|log_throttle_queries_not_using_indexes|0|0-4294967295|Omezí počet takové dotazy za minutu, které je možné zapsat do protokolu pomalé dotazu.|
|long_query_time|10|1-1E + 100|Pokud dotaz trvá déle, než tento počet sekund, server zvýší Slow_queries proměnné stavu.|
|max_allowed_packet|536870912|1024-1073741824|Maximální velikost jednoho paketu nebo všechny vygenerované/zprostředkující řetězec nebo libovolný parametr poslal mysql_stmt_send_long_data() funkce rozhraní API jazyka C.|
|min_examined_row_limit|0|0-18446744073709551615|Zaznamená dotazy, které mají větší než nakonfigurovaný počet řádků, do protokolu pomalé dotazu. |
|server_id|3293747068|1000-4294967295|ID serveru umožňuje v replikaci zadejte pro každou hlavní a slave jedinečnou identitu.|
|slave_net_timeout|60|30-3600|Počet sekund před připojení porušené, že podřízená za čekat na další data z hlavní zruší čtení a pokusí se znovu připojit.|
|slow_query_log|OFF|VYPNUTÝ|Povolit nebo zakázat protokol pomalé dotazu.|
|sql_mode|vybrané 0|ALLOW_INVALID_DATES, IGNORE_SPACE, atd.|Aktuální režim SQL server.|
|time_zone|SYSTÉM|Příklady: -8:00, +05: 30|Časové pásmo serveru.|
|wait_timeout|120|60-240|Počet sekund, po které server čeká aktivity na neinteraktivní připojení před jeho zavřením ho.|

## <a name="next-steps"></a>Další kroky
- [Knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)
