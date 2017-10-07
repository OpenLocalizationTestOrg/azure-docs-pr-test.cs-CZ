---
title: "aaaGet pracovat s nástroji elastické databáze | Microsoft Docs"
description: "Základní vysvětlení funkce nástroje pro elastické databáze hello databáze SQL Azure, včetně Snadné spuštění ukázkové aplikace."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: CarlRabeler
ms.assetid: b6911f8d-2bae-4d04-9fa8-f79a3db7129d
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.openlocfilehash: a84e05c39dffbaef440538602f898acee6e0483a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-elastic-database-tools"></a>Začínáme s nástroje elastické databáze
Toto téma představuje toohello vývojáře prostředí tím, že vám pomáhá toorun hello ukázkovou aplikaci. Ukázka Hello vytvoří jednoduchou aplikaci horizontálně dělené a jsou zde popsány klíčových funkcí nástroje elastické databáze. Hello příklad ukazuje funkce hello [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md).

tooinstall hello knihovně přejděte příliš[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Knihovna Hello je nainstalována hello ukázkovou aplikaci, která je popsána v následující části hello.

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2012 nebo novějším s C#. Stáhněte si bezplatnou verzi na [Visual Studio stáhne](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 nebo vyšší. tooget hello nejnovější verzi, najdete v části [instalace NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="download-and-run-hello-sample-app"></a>Stáhněte a spusťte ukázkové aplikace hello
Hello **elastické databáze nástroje pro Azure SQL – Začínáme** ukázkovou aplikaci znázorňuje hello nejdůležitějších aspektů hello vývojového prostředí pro horizontálně dělenou aplikace, které používejte nástroje elastické databáze. Zaměřuje se na případů použití klíče pro [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md), [závislé na data směrování](sql-database-elastic-scale-data-dependent-routing.md), a [víc horizontálních dotazování](sql-database-elastic-scale-multishard-querying.md). toodownload a spuštění hello ukázková, postupujte takto: 

1. Stáhnout hello [elastické databáze nástroje pro Azure SQL – ukázka Začínáme](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) z webu MSDN. Rozbalte hello ukázka tooa umístění, které zvolíte.

2. toocreate projekt, otevřete hello **ElasticScaleStarterKit.sln** řešení z hello **C#** adresáře.

3. V hello řešení pro hello ukázkový projekt, otevřete hello **app.config** souboru. Název serveru Azure SQL Database a vaše přihlašovací údaje (uživatelské jméno a heslo), postupujte podle pokynů hello v souboru tooadd hello.

4. Sestavení a spuštění aplikace hello. Po zobrazení výzvy, povolte balíčků NuGet aplikace Visual Studio toorestore hello hello řešení. Tím se stáhne nejnovější verzi klientské knihovny pro elastické databáze hello hello z NuGet.

5. Experimentujte s hello různé možnosti toolearn Další informace o možnosti knihovny klienta hello. Všimněte si kroky hello hello trvá aplikace ve výstupu konzoly hello a myslíte, že kód hello volné tooexplore pozadí hello.
   
    ![Průběh][4]

Blahopřejeme k – jste úspěšně vytvořen a spuštění vaší první horizontálně dělené aplikace pomocí nástroje elastické databáze v databázi SQL. Použít Visual Studio nebo SQL Server Management Studio tooconnect tooyour SQL database a rychle zobrazit v hello horizontálních oddílů, které hello ukázka vytvořili. Všimnete si nové ukázkové databáze horizontálního oddílu a databázi horizontálního oddílu mapy manager vytvořil tento ukázkový hello.

> [!IMPORTANT]
> Doporučujeme, aby vám zůstane synchronizovaná se aktualizace tooAzure a SQL Database vždy použít nejnovější verzi hello Management Studio. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="key-pieces-of-hello-code-sample"></a>Klíče kusy hello ukázka kódu
* **Správa horizontálních oddílů a horizontálního oddílu map**: hello kódu ukazuje, jak toowork horizontálních oddílů, rozsahy a mapování v souboru hello **ShardManagementUtils.cs**. Další informace najdete v tématu [škálování databáze s hello horizontálního oddílu mapy manager](http://go.microsoft.com/?linkid=9862595).  

* **Závislé na data směrování**: směrování správné horizontálního oddílu toohello transakce se zobrazí v **DataDependentRoutingSample.cs**. Další informace najdete v tématu [závislé na Data směrování](http://go.microsoft.com/?linkid=9862596). 

* **Dotazování přes víc horizontálních oddílů**: dotazování napříč horizontálních oddílů je znázorněna v souboru hello **MultiShardQuerySample.cs**. Další informace najdete v tématu [víc horizontálních dotazování](http://go.microsoft.com/?linkid=9862597).

* **Přidání prázdný horizontálních oddílů**: hello iterativní přidat nový prázdný horizontálních oddílů provádí hello kód v souboru hello **CreateShardSample.cs**. Další informace najdete v tématu [škálování databáze s hello horizontálního oddílu mapy manager](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Jiné operace elastické škálování
* **Rozdělení existující horizontálního oddílu**: hello schopností toosplit horizontálních oddílů je poskytovaný hello **nástroji pro sloučení rozdělení**. Další informace najdete v tématu [přesouvání dat mezi instancemi cloudu databázemi](sql-database-elastic-scale-overview-split-and-merge.md).

* **Slučování existující horizontálních oddílů**: sloučení horizontálního oddílu jsou také provést pomocí hello **nástroji pro sloučení rozdělení**. Další informace najdete v tématu [přesouvání dat mezi instancemi cloudu databázemi](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Náklady
Hello nástroje elastické databáze jsou volné. Při použití nástroje elastické databáze jste neobdrželi jakýchkoli dalších poplatků nad náklady hello vašeho Azure využití. 

Například hello ukázkovou aplikaci vytvoří nové databáze. Hello nákladů pro tuto závisí na edici SQL Database hello, vybraných a hello Azure využití vaší aplikace.

Informace o cenách najdete v části [podrobnosti o cenách SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Další kroky
Další informace o nástroje elastické databáze najdete v tématu hello následující stránky:

* Ukázky kódu: 
  * [Nástroje pro elastické databáze Azure SQL – Začínáme](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
  * [Nástroje pro elastické databáze Azure SQL – integrace Entity Framework](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Pružnost horizontálního oddílu v Centru skriptů](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blog: [elastické škálování oznámení](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* Microsoft Virtual Academy: [implementace Škálováním na více systémů pomocí horizontálního dělení s hello elastické databáze klienta knihovny Video](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554?l=lWyQhF1fC_6306218965) 
* Kanál 9: [elastické škálování přehled – video](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Diskusní fórum: [fórum Azure SQL Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* výkon toomeasure: [čítače výkonu pro správce horizontálního oddílu mapy](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[hello Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run hello Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png

