---
title: "Konfigurace a přístup k protokolům server pro databázi Azure pro databázi MySQL v Azure Portal | Microsoft Docs"
description: "Tento článek popisuje postup konfigurace a přístup v protokolech serveru ve službě Azure Database pro databázi MySQL z portálu Azure."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/19/2017
ms.openlocfilehash: 89674c133b458c16fbdacd771be24830624dde7c
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/20/2017
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Konfigurace a v protokolech serveru přístup k portálu Azure

Můžete nakonfigurovat, seznamu a stáhnout [databáze Azure pro protokoly serveru MySQL](concepts-server-logs.md) z portálu Azure.

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- [Databáze MySQL serveru Azure](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Konfigurace protokolování
Konfigurace přístupu k protokolu pomalé dotazu MySQL. 

1. Přihlaste se k webu [Azure Portal](http://portal.azure.com/).

2. Vyberte svou databázi Azure pro server databáze MySQL.

3. V části **monitorování** části na bočním panelu, vyberte **protokoly serveru**. 
   ![Vyberte protokoly serveru, klikněte na tlačítko Konfigurovat](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Vyberte záhlaví **kliknutím sem povolení protokolů a konfigurace protokolu parametrů** zobrazíte parametry serveru.

5. Vyberte **zobrazit další** expander delší seznam dostupných parametrů. 

   Další informace o definicích parametry, naleznete v dokumentaci k MySQL na [protokoly](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

   ![Klikněte na Zobrazit více pro delší seznam](./media/howto-configure-server-logs-in-portal/2-show-more.png)

6. Změňte parametry, které je potřeba upravit. Všechny změny provedené v této relaci se zvýrazní zeleně. 

   Jakmile změníte parametry, můžete kliknout na **Uložit**. Nebo můžete **zahodit** změny.

   ![Klikněte na Uložit nebo zrušení](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

7. Vrátit do seznamu protokolů kliknutím **tlačítko Zavřít** (X ikonu) na **parametry serveru** stránky.

## <a name="view-list-and-download-logs"></a>Zobrazení seznamu a stažení protokolů
Jakmile začne protokolování, můžete zobrazit seznam dostupných protokolů a stáhnout jednotlivých protokolových souborů v podokně protokoly serveru. 

1. Otevřete portál Azure.

2. Vyberte svou databázi Azure pro server databáze MySQL.

3. V části **monitorování** části na bočním panelu, vyberte **protokoly serveru**. Stránce zobrazuje seznam souborů protokolu, jak je znázorněno:

   ![Seznam protokolů](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Zásady vytváření názvů protokolu je **mysql - pomalé – < název serveru >-yyyymmddhh.log**. Datum a čas používat v názvu souboru je čas je datum vystavení v protokolu. Soubory protokolů otáčejí každých 24 hodin nebo 7.5 GB, co nastane dříve.

4. V případě potřeby použijte **vyhledávacího pole** rychle zúžit zaměření na specifickém protokolu podle data a času. Hledání je na název protokolu.

5. Stáhnout pomocí jednotlivých protokolových souborů **Stáhnout** tlačítko (dolů ikonu šipky) vedle jednotlivých souborů protokolu v řádku tabulky, jak je znázorněno:

   ![Klikněte na ikonu stahování](./media/howto-configure-server-logs-in-portal/5-download.png)


## <a name="next-steps"></a>Další kroky
- V tématu [protokoly serveru přístup v rozhraní příkazového řádku](howto-configure-server-logs-in-cli.md) se dozvíte, jak ke stažení protokolů prostřednictvím kódu programu.
- Další informace o [protokoly serveru](concepts-server-logs.md) ve službě Azure Database pro databázi MySQL. 
- Další informace o protokolování MySQL a definicemi parametrů naleznete v dokumentaci k MySQL na [protokoly](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

