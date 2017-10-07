---
title: "aaaHow tooConfigure parametry serveru ve službě Azure Database pro databázi MySQL | Microsoft Docs"
description: "Tento článek popisuje, jak hello tooconfigure dostupný server pro parametry v Azure databázi MySQL pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/19/2017
ms.openlocfilehash: 8292c8eda605854a06b6a9c82185c857bd353cfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-server-parameters-in-azure-database-for-mysql-using-hello-azure-portal"></a>Jak tooconfigure parametry serveru v databázi Azure pro používání MySQL hello portálu Azure

Azure databáze pro databázi MySQL podporuje konfiguraci některé parametry serveru. Tento článek popisuje, jak tooconfigure tyto parametry s využitím hello portál Azure a seznamy hello podporované parametry, hello výchozí hodnoty a hello rozsah platných hodnot. Ne všechny parametry serveru můžete upravit. Jsou podporovány pouze hello ty, které jsou zde uvedeny.

## <a name="navigate-tooserver-parameters-blade-on-azure-portal"></a>Přejděte tooServer parametry okno na portálu Azure

Přihlaste se toohello portál Azure a pak klikněte na vaší databázi Azure pro název serveru databáze MySQL. V části hello **nastavení** klikněte na tlačítko **parametry serveru** tooopen hello serveru parametry okně hello Azure Database pro databázi MySQL.

![Okno Parametry server portálu Azure](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Seznam parametrů konfigurovat server

Hello následující tabulka obsahuje parametry server hello aktuálně podporované. Hello parametry lze nakonfigurovat podle tooyour požadavky aplikací.

> [!div class="mx-tableFixed"]
|Název parametru|Výchozí hodnota|rozsah|Popis|
|---|---|---|---|
|binlog_group_commit_sync_delay|1000|0, 11-1000000|Ovládací prvky kolik mikrosekundách hello binární protokol potvrzení čeká před synchronizací toodisk souboru binárního protokolu hello.|
|binlog_group_commit_sync_no_delay_count|0|0-1000000|Hello maximální počet transakcí toowait pro před ukončením hello aktuální zpoždění podle specifikace binlog skupiny – potvrzení sync-delay.|
|character_set_server|LATIN1|BIG5, UTF8MB4, atd.|Použijte charset_name jako hello výchozí server znakovou sadu.|
|div_precision_increment|4|0-30|Počet číslic, ve které tooincrease hello škále hello výsledek operace dělení.|
|group_concat_max_len|1024|4-16777216|Maximální povolená délka výsledek v bajtech pro hello GROUP_CONCAT().|
|innodb_adaptive_hash_index|ON|VYPNUTÝ|Indexy hash adaptivní innodb jestli jsou povolená nebo zakázaná.|
|innodb_lock_wait_timeout|50|1-3600|Hello dobu v sekundách transakci InnoDB čeká na zámek řádků, než zkoušet.|
|interactive_timeout|1800|10-1800|Počet sekund hello server čeká na aktivitu na připojení k interaktivní před zavřením.|
|log_bin_trust_function_creators|OFF|VYPNUTÝ|Tato proměnná platí, pokud je povoleno binární protokolování. Určuje, zda uložené funkce, které může být creators důvěryhodné není toocreate uložené funkce, které způsobí toobe unsafe události zapisovat toohello binární protokol.|
|log_queries_not_using_indexes|OFF|VYPNUTÝ|Protokoly dotazy, které jsou očekávané tooretrieve všechny řádky tooslow dotazu protokolu.|
|log_slow_admin_statements|OFF|VYPNUTÝ|V příkazech hello zapisovat protokol pomalé dotazu toohello zahrnují pomalé příkazy pro správu.|
|log_throttle_queries_not_using_indexes|0|0-4294967295|Omezení hello počet takové dotazy za minutu, které může být napsán protokol toohello pomalé dotazu.|
|long_query_time|10|1-1E + 100|Pokud dotaz trvá déle, než tento počet sekund, hello server zvýší hello Slow_queries stav proměnné.|
|max_allowed_packet|536870912|1024-1073741824|maximální velikost Hello jeden paket nebo libovolný řetězec generován/zprostředkující nebo libovolný parametr poslal hello mysql_stmt_send_long_data() funkce rozhraní API jazyka C|
|min_examined_row_limit|0|0-18446744073709551615|Zaznamená dotazy, které mají větší než hello nakonfigurovaný počet řádků, do protokolu dotazu pomalé hello. |
|server_id|3293747068|1000-4294967295|ID serveru Hello, používá v replikace toogive každou hlavní a slave jedinečnou identitu.|
|slave_net_timeout|60|30-3600|Hello počet sekund toowait další data z hlavní hello před hello podřízený zvažuje hello připojení porušené, zruší hello čtení a pokusí tooreconnect.|
|slow_query_log|OFF|VYPNUTÝ|Povolit nebo zakázat protokol hello pomalé dotazu.|
|sql_mode|vybrané 0|ALLOW_INVALID_DATES, IGNORE_SPACE, atd.|Hello aktuální režim systému SQL server.|
|time_zone|SYSTÉM|Příklady: -8:00, +05: 30|časové pásmo serveru Hello.|
|wait_timeout|120|60-240|Hello počet sekund hello server čeká na aktivitu na neinteraktivní připojení před jeho zavřením ho.|

## <a name="next-steps"></a>Další kroky
- [Knihovny připojení pro databázi Azure pro databázi MySQL](concepts-connection-libraries.md)
