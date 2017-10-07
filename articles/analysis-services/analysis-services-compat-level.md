---
title: "úroveň kompatibility modelu aaaData ve službě Azure Analysis Services | Microsoft Docs"
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
ms.openlocfilehash: bfaf0c60666729d1e6e0baf082c046ea9faa4e86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Úroveň kompatibility pro tabulkové modely služby Analysis Services

*Úroveň kompatibility* odkazuje toorelease konkrétní chování v hello stroje služby Analysis Services. Úroveň kompatibility toohello změny obvykle shodovat se hlavní verze systému SQL Server. Tyto změny jsou také implementované v Azure Analysis Services toomaintain rozdíly mezi obě platformy. Kompatibilita úrovně ovlivní také funkce dostupné v tabulkové modely. Například DirectQuery a metadat pro tabulkový objektový mají různé implementace v závislosti na úrovni kompatibility hello. 

Azure Analysis Services podporuje tabulkové modely na úrovní kompatibility 1200 a 1400 hello.

nejnovější úroveň kompatibility Hello je 1400. Tato úroveň se shoduje s SQL Server 2017 Analysis Services. Hlavní funkce v úroveň kompatibility 1400 hello:

*  Novou infrastrukturu pro datové připojení a import do tabulkové modely s podporou pro rozhraní API TNÍ a TMSL skriptování. Tato nová funkce umožňuje podporu pro další datové zdroje, jako je například úložiště objektů Blob v Azure.
*  Transformace dat a možnosti data hybridní webové aplikace pomocí výrazy načíst Data a M.
*  Míry podporovat vlastnost řádky s podrobnostmi s výraz DAX. Tato vlastnost umožňuje nástroje klienta, například aplikace Microsoft Excel toodrill dolů toodetailed data ze souhrnné sestavy. Například pokud uživatelé zobrazit celkový prodej oblasti a měsíc, mohou prohlížet hello související podrobnosti pořadí. 
*  Zabezpečení na úrovni objektu pro tabulky a sloupce názvy kromě toohello dat mezi nimi.
*  Vylepšená podpora pro nepravidelné hierarchie.
*  Výkon a vylepšení sledování.
  
## <a name="set-compatibility-level"></a>Úroveň kompatibility sady 
 Při vytváření nového projektu tabulkový model v sadě SSDT, můžete zadat úroveň kompatibility hello na hello **tabulkový model designer** dialogové okno. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Pokud vyberete hello **tuto zprávu již příště nezobrazovat** možnost, všechny následné projekty pomocí úroveň kompatibility hello jste zadali jako výchozí hello. Můžete změnit úroveň kompatibility výchozí hello v sadě SSDT v **nástroje** > **možnosti**.  
  
 tooupgrade existujícího projektu tabulkový model v sadě SSDT, sada hello **úroveň kompatibility** vlastnost v modelu hello **vlastnosti** okno. Zachovat v paměti, upgrade úroveň kompatibility hello je nevratné.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>Zkontrolujte úroveň kompatibility pro databázi tabulkového modelu v aplikaci SQL Server Management Studio 
 V aplikaci SSMS, klikněte pravým tlačítkem na název databáze hello > **vlastnosti** > **úroveň kompatibility**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>Zkontrolujte úroveň kompatibility podporované pro server v aplikaci SSMS  
 V aplikaci SSMS, klikněte pravým tlačítkem na název serveru hello > **vlastnosti** > **podporované úroveň kompatibility**.  
  
 Tato vlastnost určuje hello nejvyšší úroveň kompatibility databáze, která se spustí na serveru hello (s výjimkou preview). nelze změnit úroveň kompatibility Hello podporována.  

## <a name="next-steps"></a>Další kroky
  [Vytvoření modelu na portálu Azure](analysis-services-create-model-portal.md)   
  [Správa služby Analysis Services](analysis-services-manage.md)  
