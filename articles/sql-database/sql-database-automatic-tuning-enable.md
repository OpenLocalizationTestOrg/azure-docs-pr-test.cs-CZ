---
title: "aaaEnable automatické ladění pro Azure SQL Database | Microsoft Docs"
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
ms.openlocfilehash: af9da161eabc0f8c4cb100c050288f234efb8093
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-automatic-tuning"></a>Povolení automatického ladění

Databáze SQL Azure je služba automaticky spravovaná data, která neustále monitoruje vaše dotazy a identifikuje hello akce, že můžete provádět tooimprove výkon vašich úloh. Můžete zkontrolovat doporučení a ručně je použít nebo to Azure SQL Database automaticky použít opravné akce – to se označuje jako **automatické ladění režimu**. Automatické ladění se dá nastavit na úrovni databáze hello nebo hello server.

## <a name="enable-automatic-tuning-on-server"></a>Povolit automatické ladění na serveru

tooenable automatické ladění na serveru Azure SQL Database, přejděte toohello server v Azure portálu a pak vyberte **automatické ladění** v nabídce hello. Vyberte hello automatické ladění možnosti tooenable a vyberte **použít**:

![Server](./media/sql-database-automatic-tuning-enable/server.png)

Automatické ladění možnosti na serveru jsou použité tooall databáze na serveru hello. Ve výchozím nastavení všechny databáze Zdědit konfiguraci hello z jejich nadřízeného serveru, ale to můžete přepsat a zadat pro každou databázi jednotlivě.

## <a name="configure-automatic-tuning-on-database"></a>Konfigurovat automatické ladění databáze

Hello Azure portálu umožňuje tooindividually můžete zadat automatickou konfiguraci ladění hello na každou databázi.

> [!NOTE]
> Obecné doporučení Hello je toomanage hello automatické ladění konfigurace na úrovni serveru Ano hello stejné nastavení konfigurace se dá použít na každou databázi automaticky. Konfigurujte automatické ladění na jednotlivé databáze Pokud hello databázi se liší, že jiné na stejný server hello.
>

toohello databáze v hello portálu Azure přejděte tooenable automatické ladění na jedné databáze a pak vyberte **automatické ladění**. Můžete nakonfigurovat jedné databáze tooinherit hello nastavení z databáze hello výběrem zaškrtávacího políčka hello nebo můžete zadat hello konfigurace pro databázi jednotlivě.

![Databáze](./media/sql-database-automatic-tuning-enable/database.png)

Jakmile vyberete odpovídající konfiguraci, klikněte na tlačítko **použít**.

## <a name="next-steps"></a>Další kroky
* Čtení hello [automatické ladění článku](sql-database-automatic-tuning.md) toolearn Další informace o automatické ladění a jak se vám může pomoct zlepšit výkon.
* V tématu [výkonu doporučení](sql-database-advisor.md) přehled o výkonu doporučení Azure SQL Database.
* V tématu [Query Performance Insight](sql-database-query-performance.md) toolearn o zobrazení hello vlivu na výkon nejčastějších dotazů.
