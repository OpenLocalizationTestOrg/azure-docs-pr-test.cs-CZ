---
title: "Datový model úroveň kompatibility v Azure Analysis Services | Microsoft Docs"
description: "Seznámení s úrovní kompatibility tabulková data modelu."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/16/2017
ms.author: owend
ms.openlocfilehash: b11ba54c2cdc2675ec535368e7076613a5290212
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Úroveň kompatibility pro tabulkové modely služby Analysis Services

*Úroveň kompatibility* odkazuje na konkrétní verzi chování v rámci stroje služby Analysis Services. Změny úrovně kompatibility se obvykle shodují s hlavní verze systému SQL Server. Tyto změny jsou také implementované v Azure Analysis Services k udržování rozdíly mezi obě platformy. Kompatibilita úrovně ovlivní také funkce dostupné v tabulkové modely. Například DirectQuery a metadat pro tabulkový objektový mají různé implementace v závislosti na úrovni kompatibility. 

Azure Analysis Services podporuje tabulkové modely na úrovni kompatibility 1200 nebo 1400.

Nejnovější úroveň kompatibility je 1400. Tato úroveň se shoduje s SQL Server 2017 Analysis Services. Mezi hlavní funkce na úrovni kompatibility 1400 patří:

*  Novou infrastrukturu pro datové připojení a import do tabulkové modely s podporou pro rozhraní API TNÍ a TMSL skriptování. Tato nová funkce umožňuje podporu pro další datové zdroje, jako je například úložiště objektů Blob v Azure.
*  Transformace dat a možnosti data hybridní webové aplikace pomocí výrazy načíst Data a M.
*  Míry podporovat vlastnost řádky s podrobnostmi s výraz DAX. Tato vlastnost umožňuje nástroje klienta, například aplikace Microsoft Excel k podrobnostem podrobná data ze souhrnné sestavy. Když uživatelé zobrazit celkový prodej oblasti a měsíc, mohou prohlížet podrobnosti související pořadí. 
*  Zabezpečení na úrovni objektu pro názvy tabulek a sloupců, kromě dat mezi nimi.
*  Vylepšená podpora pro nepravidelné hierarchie.
*  Výkon a vylepšení sledování.
  
## <a name="set-compatibility-level"></a>Úroveň kompatibility sady 
 Při vytváření nového projektu tabulkový model v sadě SSDT, můžete zadat úroveň kompatibility na **tabulkový model designer** dialogové okno. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Pokud jste vybrali **tuto zprávu již příště nezobrazovat** možnost, všechny následné projekty pomocí úroveň kompatibility, který jste zadali jako výchozí. Můžete změnit výchozí úroveň kompatibility v sadě SSDT v **nástroje** > **možnosti**.  
  
 Pokud chcete upgradovat existující tabulkový model projekt v sadě SSDT, nastavte **úroveň kompatibility** vlastnost v modelu **vlastnosti** okno. Zachovat v paměti, upgrade úroveň kompatibility je nevratné.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>Zkontrolujte úroveň kompatibility pro databázi tabulkového modelu v aplikaci SQL Server Management Studio 
 V aplikaci SSMS, klikněte pravým tlačítkem na název databáze > **vlastnosti** > **úroveň kompatibility**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>Zkontrolujte úroveň kompatibility podporované pro server v aplikaci SSMS  
 V aplikaci SSMS, klikněte pravým tlačítkem na název serveru > **vlastnosti** > **podporované úroveň kompatibility**.  
  
 Tato vlastnost určuje nejvyšší úroveň kompatibility databáze, která se spustí na serveru (s výjimkou preview). Nelze změnit úroveň kompatibility podporované.  

## <a name="next-steps"></a>Další kroky
  [Vytvoření modelu na portálu Azure](analysis-services-create-model-portal.md)   
  [Správa služby Analysis Services](analysis-services-manage.md)  
