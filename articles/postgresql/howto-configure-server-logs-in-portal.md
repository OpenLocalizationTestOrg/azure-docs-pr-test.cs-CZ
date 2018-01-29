---
title: "Konfigurace a přístup k protokolům serveru pro PostgreSQL na portálu Azure | Microsoft Docs"
description: "Tento článek popisuje postup konfigurace a přístup v protokolech serveru v Azure databázi PostgreSQL z portálu Azure."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/19/2017
ms.openlocfilehash: a2f67b21293a1a0456b27cad9043be01fdd5274a
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Konfigurace a v protokolech serveru přístup k portálu Azure

Můžete nakonfigurovat, seznamu a stáhnout [databáze Azure pro protokoly serveru PostgreSQL](concepts-server-logs.md) z portálu Azure.

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- [Azure databázi PostgreSQL serveru.](quickstart-create-server-database-portal.md)

## <a name="configure-logging"></a>Konfigurace protokolování
Konfigurace přístupu k dotazu protokoly a protokoly chyb. 

1. Přihlaste se k [portálu Azure](http://portal.azure.com/).

2. Vyberte svou databázi Azure pro PostgreSQL server.

3. V části **monitorování** části na bočním panelu, vyberte **protokoly serveru**. 

   ![Vyberte protokoly serveru a vyberte možnost "povolit... Kliknutím sem.](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Vyberte záhlaví **kliknutím sem povolení protokolů a konfigurace protokolu parametrů** zobrazíte parametry serveru.

5. Vyberte **zobrazit další** expander delší seznam dostupných parametrů. 

   Další informace o definicích parametry, naleznete v dokumentaci k PostgreSQL na [Chyba protokolování a generování sestav](https://www.postgresql.org/docs/current/static/runtime-config-logging.html).

   ![Krátké seznam parametry protokolu. Klikněte na Zobrazit více pro dlouho](./media/howto-configure-server-logs-in-portal/2-show-more.png)

6. Změňte parametry, které je potřeba upravit. Všechny změny provedené v této relaci se zvýrazní zeleně.

   Jakmile změníte parametry, můžete kliknout na **Uložit**. Nebo můžete **zahodit** změny. 

   ![Dlouhý seznam parametrů s změny uložte nebo zahoďte](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

7. Vrátit do seznamu protokolů kliknutím **tlačítko Zavřít** (X ikonu) na **parametry serveru** stránky.

## <a name="view-list-and-download-logs"></a>Zobrazení seznamu a stažení protokolů
Jakmile začne protokolování, můžete zobrazit seznam dostupných protokolů a stáhnout jednotlivých protokolových souborů v podokně protokoly serveru. 

1. Otevřete web Azure Portal.

2. Vyberte svou databázi Azure pro PostgreSQL server.

3. V části **monitorování** části na bočním panelu, vyberte **protokoly serveru**. Stránce zobrazuje seznam souborů protokolu, jak je znázorněno:

   ![Seznam serverů protokoly](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Zásady vytváření názvů protokolu je **postgresql rrrr mm-dd_hh0000.log**. Datum a čas používat v názvu souboru je čas je datum vystavení v protokolu. Soubory protokolů otočit každou hodinu nebo velikost 100 MB, nastane dříve.

4. V případě potřeby použijte **vyhledávacího pole** rychle zúžit zaměření na specifickém protokolu podle data a času. Hledání je na název protokolu.

   ![Příklad vyhledávání v protokolu názvy](./media/howto-configure-server-logs-in-portal/5-search.png)

5. Stáhnout pomocí jednotlivých protokolových souborů **Stáhnout** tlačítko (dolů ikonu šipky) vedle jednotlivých souborů protokolu v řádku tabulky, jak je znázorněno:

   ![Klikněte na ikonu stahování](./media/howto-configure-server-logs-in-portal/6-download.png)

## <a name="next-steps"></a>Další postup
- V tématu [protokoly serveru přístup v rozhraní příkazového řádku](howto-configure-server-logs-using-cli.md) se dozvíte, jak ke stažení protokolů prostřednictvím kódu programu.
- Další informace o [protokoly serveru](concepts-server-logs.md) v Azure DB pro PostgreSQL. 
- Další informace o protokolování PostgreSQL a definicemi parametrů naleznete v dokumentaci k PostgreSQL na [zasílání zpráv o chybách a protokolování](https://www.postgresql.org/docs/current/static/runtime-config-logging.html).

