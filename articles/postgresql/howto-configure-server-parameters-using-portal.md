---
title: "Konfigurovat parametry serveru v databázi Azure pro PostgreSQL prostřednictvím portálu Azure | Microsoft Docs"
description: "Tento článek popisuje postup konfigurace serveru parametry v databáze Azure pro PostgreSQL prostřednictvím portálu Azure."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/08/2017
ms.openlocfilehash: 9e8262fbfcde2e69a656e356a7ab241f2d5043ad
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="configure-server-parameters-in-azure-portal"></a>Konfigurovat parametry serveru na portálu Azure
Můžete zobrazit seznam, zobrazit a aktualizovat parametry konfigurace pro databázi Azure pro server PostgreSQL prostřednictvím portálu Azure.

## <a name="prerequisites"></a>Požadavky
Pro jednotlivé kroky tento postup průvodce budete potřebovat:
- [Azure databázi PostgreSQL serveru.](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Prohlížení a úpravy parametry
1. Otevřete web [Azure Portal](https://portal.azure.com).

2. Vyberte svou databázi Azure pro PostgreSQL server.

3. V části **nastavení** vyberte **parametry serveru**. Stránce zobrazuje seznam parametrů, jejich hodnoty a popisy.
![Přehledová stránka parametry](./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png)

4. Vyberte **rozevírací nabídku** tlačítko zobrazíte možné hodnoty pro parametry uvedené typu jako client_min_messages.
![Zobrazení výčtu rozevírací dolů](./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png)

5. Vyberte nebo přejděte myší **i** tlačítko (informace o) najdete v části rozsahu možných hodnot pro číselné parametry jako cpu_index_tuple_cost.
![tlačítko informace](./media/howto-configure-server-parameters-in-portal/4-information-button.png)

6. V případě potřeby použijte **vyhledávacího pole** zúžit zaměření na konkrétní parametr. Hledání je na název a popis parametrů.
![Výsledky hledání](./media/howto-configure-server-parameters-in-portal/5-search.png)

7. Změňte hodnoty parametrů, které chcete upravit. Všechny změny provedené v relaci se zvýrazní zeleně. Po změně hodnoty se můžete vybrat **Uložit**. Nebo můžete **zahodit** změny.
![Uložit nebo zahodit změny](./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png)

8. Pokud jste uložili nové hodnoty pro parametry, můžete vždy obnovit vše zpět na výchozí hodnoty výběrem **resetovat všechny výchozí**.
![Všechny resetovat na výchozí](./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png)

## <a name="next-steps"></a>Další kroky
Další informace o:
- [Přehled parametry serveru v databázi Azure pro PostgreSQL](concepts-servers.md)
- [Konfigurace parametrů pomocí rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md)
