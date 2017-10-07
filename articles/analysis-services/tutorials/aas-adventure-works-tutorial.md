---
title: AAA "Azure Analysis Services Adventure Works kurzu | Microsoft Docs"
description: "Zavádí hello společnosti Adventure Works kurz pro Azure Analysis Services"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.openlocfilehash: 2df8b3ab4e8c4ffbe0086418d60fd2e2abd35e7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-analysis-services---adventure-works-tutorial"></a>Azure Analysis Services – Kurz Adventure Works

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Tento kurz obsahuje lekce toocreate a nasazovat s využitím tabulkový model na úroveň kompatibility hello 1400 [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  

Pokud jste nové služby tooAnalysis a tabulkové modelování, dokončení tohoto kurzu je nejrychlejší způsob, jak toolearn hello jak toocreate a nasazení základní tabulkový model. Jakmile máte hello požadavky na místě, má mezi dvěma toocomplete hodin toothree trvat.  
  
## <a name="what-you-learn"></a>Co se naučíte   
  
-   Jak toocreate nový tabulkový model projektu na hello **úroveň kompatibility 1400** v sadě SSDT.
  
-   Jak tooimport data z relační databáze do projektu tabulkový model.  
  
-   Jak toocreate a spravovat vztahy mezi tabulkami v modelu hello.  
  
-   Jak toocreate počítané sloupce, míry a klíčové ukazatele výkonu, které pomáhají uživatelům analyzovat kritické obchodní metriky.  
  
-   Jak toocreate a spravovat perspektivy a hierarchií, které pomáhají uživatelům snadno procházet data modelu tím, že poskytuje firmy a hlediska specifické pro aplikaci.  
  
-   Jak toocreate oddíly které rozdělují data tabulky do menších logických částí, které lze zpracovat nezávisle z jiných oddílů.  
  
-   Jak toosecure model objektů a dat po vytvoření se členy uživatelské role.  
  
-   Jak toodeploy tabulkový model tooan **Azure Analysis Services** server nebo místní server SQL Server 2017 Analysis Services.  
  
## <a name="prerequisites"></a>Požadavky  
toocomplete tohoto kurzu potřebujete:  
  
-   Azure Analysis Services nebo SQL Server 2017 Analysis Services instanci toodeploy váš model. Zaregistrujte si bezplatnou [zkušební verzi Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) a [vytvořte server](../analysis-services-create-server.md). Nebo si zaregistrujte a stáhněte verzi [SQL Server 2017 Community Technology Preview](https://www.microsoft.com/evalcenter/evaluate-sql-server-vnext-ctp). 

-   Datový sklad SQL Server nebo Azure SQL Data Warehouse s hello [AdventureWorksDW2014 ukázkové databáze](http://go.microsoft.com/fwlink/?LinkID=335807). Tato ukázková databáze obsahuje nezbytné toocomplete hello data v tomto kurzu. Stáhněte si [bezplatnou edici SQL Serveru](https://www.microsoft.com/sql-server/sql-server-downloads). Nebo si zaregistrujte bezplatnou [zkušební verzi Azure SQL Database](https://azure.microsoft.com/services/sql-database/). 

    **Důležité:** Pokud instalace hello ukázkové databáze na serveru SQL na místě a nasazení serveru Azure Analysis Services tooan modelu, [místní brána dat](../analysis-services-gateway.md) je vyžadován.

-   nejnovější verze Hello [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).

-   nejnovější verze Hello [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).    

-   Klientskou aplikaci, jako je [Power BI Desktop](https://powerbi.microsoft.com/desktop/) nebo Excel. 

## <a name="scenario"></a>Scénář  
Tento kurz je založený na fiktivní společnosti Adventure Works Cycles. Společnosti Adventure Works je velké mezinárodní výrobní společnost, která vytvoří a distribuuje jízdních kol a části, příslušenství toocommercial trhů v Severní Americe, Evropy a Asie. Hello společnosti využívá 500 pracovních procesů. Kromě toho Adventure Works zaměstnává několik regionálních prodejních týmů na všech základních trzích. Projekt je toocreate tabulkový model pro tooanalyze prodeje a marketingu uživatelů Internetu prodejní data v databázi AdventureWorksDW hello.  
  
toocomplete hello kurz, musíte dokončit různé lekce. Každá lekce zahrnuje úkoly. Dokončení každý úkol v pořadí, je nutné pro dokončení lekce hello. Některé lekce obsahují několik úkolů, jejichž výsledek je podobný, ale způsob jejich dokončení se mírně liší. Tato metoda zobrazí často je více než jedním ze způsobů toocomplete úlohu a toochallenge naučili jste se s použitím znalostí jste v předchozím lekce a úlohy.  
  
účelem Hello lekce hello je tooguide vytvářením základní tabulkový model pomocí mnoho funkcí hello zahrnutých v sadě SSDT. Protože každé lekce navazuje na předchozí lekci hello, musíte provést hello lekce v pořadí.
  
V tomto kurzu neposkytuje poznatky o správu serveru na portálu Azure, Správa serveru nebo v databázi pomocí aplikace SSMS, nebo pomocí klienta aplikace data modelu toobrowse. 


## <a name="lessons"></a>Lekce  
Tento kurz obsahuje následující lekce hello:  
  
|Lekce|Odhadovaná doba toocomplete|  
|----------|------------------------------|  
|[1 – Vytvoření nového projektu s tabelárním modelem](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md)|10 minut|  
|[2 – Získání dat](../tutorials/aas-lesson-2-get-data.md)|10 minut|  
|[3 – Označení jako datové tabulky](../tutorials/aas-lesson-3-mark-as-date-table.md)|3 minuty|  
|[4 – Vytvoření relací](../tutorials/aas-lesson-4-create-relationships.md)|10 minut|  
|[5 – Vytvoření počítaných sloupců](../tutorials/aas-lesson-5-create-calculated-columns.md)|15 minut|
|[6 – Vytvoření měr](../tutorials/aas-lesson-6-create-measures.md)|30 minut|  
|[7 – Vytvoření klíčových ukazatelů výkonu (KPI)](../tutorials/aas-lesson-7-create-key-performance-indicators.md)|15 minut|  
|[8 – Vytvoření perspektiv](../tutorials/aas-lesson-8-create-perspectives.md)|5 minut|  
|[9 – Vytvoření hierarchií](../tutorials/aas-lesson-9-create-hierarchies.md)|20 minut|  
|[10 – Vytvoření oddílů](../tutorials/aas-lesson-10-create-partitions.md)|15 minut|  
|[11 – Vytvoření rolí](../tutorials/aas-lesson-11-create-roles.md)|15 minut|  
|[12 – Analýza v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md)|5 minut| 
|[13 – Nasazení](../tutorials/aas-lesson-13-deploy.md)|5 minut|  
  
## <a name="supplemental-lessons"></a>Doplňkové lekce  
Tyto poznatky získají nejsou požadované toocomplete hello kurzu, ale může být užitečné pro lepší pochopení pokročilé tabulkový model. vytváření funkcí.  
  
|Lekce|Odhadovaná doba toocomplete|  
|----------|------------------------------|  
|[Řádky podrobností](../tutorials/aas-supplemental-lesson-detail-rows.md)|10 minut|
|[Dynamické zabezpečení](../tutorials/aas-supplemental-lesson-dynamic-security.md)|30 minut|
|[Nepravidelné hierarchie](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)|20 minut| 

  
## <a name="next-steps"></a>Další kroky  
tooget začít, najdete v části [lekci 1: vytvoření nového projektu tabulkový Model](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
  
  

