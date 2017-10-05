---
title: "Povolit automatické ladění pro Azure SQL Database | Microsoft Docs"
description: "Můžete povolit automatické ladění na vaší databázi SQL Azure, snadno."
services: sql-database
documentationcenter: 
author: vvasic
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 06/05/2016
ms.author: vvasic
ms.openlocfilehash: b391b1f7aa37c5a06fc320ce892534187deb4959
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-automatic-tuning"></a>Povolení automatického ladění

Databáze SQL Azure je služba automaticky spravovaná data, která neustále monitoruje vaše dotazy a identifikuje akci, která umožňuje zvýšit výkon vašich úloh. Můžete zkontrolovat doporučení a ručně je použít nebo to Azure SQL Database automaticky použít opravné akce – to se označuje jako **automatické ladění režimu**. Automatické ladění se dá nastavit na úrovni databáze nebo serveru.

## <a name="enable-automatic-tuning-on-server"></a>Povolit automatické ladění na serveru

Chcete-li povolit automatické ladění na serveru Azure SQL Database, přejděte na server na portálu Azure a pak vyberte **automatické ladění** v nabídce. Vyberte možnost Automatické ladění možnosti, které chcete povolit a vyberte **použít**:

![Server](./media/sql-database-automatic-tuning-enable/server.png)

Automatické možnosti ladění na serveru se použijí pro všechny databáze na serveru. Ve výchozím nastavení všechny databáze Zdědit konfiguraci z jejich nadřízeného serveru, ale to můžete přepsat a zadat pro každou databázi jednotlivě.

## <a name="configure-automatic-tuning-on-database"></a>Konfigurovat automatické ladění databáze

Portál Azure umožňuje jednotlivě zadat automatickou konfiguraci ladění na každou databázi.

> [!NOTE]
> Obecné doporučení je spravovat konfiguraci automatické ladění na úrovni serveru, aby stejné nastavení konfigurace můžete použít na každou databázi automaticky. Konfigurujte automatické ladění na jednotlivé databáze Pokud databázi se liší jiné na stejném serveru.
>

Pokud chcete povolit automatické ladění na jedné databáze, přejděte do databáze na portálu Azure a pak vyberte **automatické ladění**. Můžete nakonfigurovat jednu databázi dědit nastavení z databáze a zaškrtnutím políčka nebo můžete zadat konfiguraci pro databázi jednotlivě.

![Databáze](./media/sql-database-automatic-tuning-enable/database.png)

Jakmile vyberete odpovídající konfiguraci, klikněte na tlačítko **použít**.

## <a name="next-steps"></a>Další kroky
* Pro čtení [automatické ladění článku](sql-database-automatic-tuning.md) Další informace o automatické ladění a jak se vám může pomoct zlepšit výkon.
* V tématu [výkonu doporučení](sql-database-advisor.md) přehled o výkonu doporučení Azure SQL Database.
* V tématu [Query Performance Insight](sql-database-query-performance.md) Další informace o zobrazení dopad na výkon nejčastějších dotazů.
