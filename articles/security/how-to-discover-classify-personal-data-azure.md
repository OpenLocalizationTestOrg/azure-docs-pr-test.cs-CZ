---
title: "aaaDiscover, identifikovat a klasifikovat osobních údajů v Microsoft Azure | Microsoft Docs"
description: "Další informace o vyhledávání, klasifikace, zjišťování a identifikaci dat"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: af4ced1c57699dc751d55cfdf3229c7d294648a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="discover-identify-and-classify-personal-data-in-microsoft-azure"></a>Zjistit, identifikovat a klasifikovat osobních údajů v Microsoft Azure

Tento článek obsahuje pokyny k tom, jak identifikovat toodiscover a klasifikovat osobních údajů v několik nástrojů Azure a službami, včetně pomocí Azure Data Catalog, Azure Active Directory, SQL Database, Power Query pro clustery systému Hadoop v Azure HDInsight, Azure Information Protection, Azure Search a SQL dotazy pro Azure Cosmos DB.

## <a name="scenario-problem-statement-and-goal"></a>Scénář, formulace problému a cíle

Na základě USA sportu společnosti shromažďuje řadu osobní a další data. To zahrnuje zákazníků a údaje o zaměstnancích. Hello společnosti udržuje ho v několika databází a ukládá je v několika různých místech v jejich prostředí Azure. Kromě toho tooselling sportovní vybavení, také hostitele a spravovat registraci pro Elitní sportovní události kolem hello, world, včetně v hello Evropa, a v některých případech hello shromážděná data zákazníků obsahuje lékařské informace.

Vzhledem k tomu, že hostitelem mnoho mezinárodní bicycling prohlídka každý rok po a má externí pracovníky v umístění kolem hello zeměkouli hello společnosti, jsou poměrně značnou několika hello datových sad. Hello společnost má také vytvořené vývojáře aplikací, které jsou používány zákazníky a zaměstnanci.

Hello společnost chce tooaddress hello následující problémy:

- Osobní data zákazníků a musí být klasifikované nebo rozlišující z hello dalším firemním hello data shromažďuje v pořadí tooensure správné přístupu a zabezpečení.
- Hello data správce musí tooeasily zjistit umístění hello zákazníka osobních údajů v různých oblastech hello prostředí Azure.
- Zákazníků a osobní data, která se zobrazí v sdílené dokumenty a e-mailovou komunikaci, musí být označeny toohelp zajistěte, aby se zachovala zabezpečené.
- Vývojáři aplikací společnosti Hello potřebovat způsob, jak tooeasily, vyhledejte osobní data zákazníků a v jejich webových a mobilních aplikací.
- Vývojáři také potřebovat tooquery jejich databázi dokumentů pro osobní data.

### <a name="company-goals"></a>Cíle společnosti

- Všechny osobní data zákazníků a musí být označené/opatřeny poznámkami v Azure Data Catalog, proto je možné snadno najít. V ideálním případě osobní data zákazníků a jsou označené poznámkou samostatně.
- Osobních dat od zákazníků a profily uživatelů a pracovní informace, které se nacházejí v Azure Active Directory musí být snadno najít.
- Osobní údaje, které se nacházejí v několika databází SQL musí být snadno dotazována. 
- Některé společnosti hello velkých datových sad jsou spravované prostřednictvím Azure HDInsight a uložené v Hadoop. Budou se musí importovat do aplikace Excel, může být dotazován pro osobní data.
- Osobní údaje, které jsou sdíleny v dokumentů a e-mailovou komunikaci musí být klasifikované, s názvem bez přípony a zabezpečen s Azure Information Protection.
- Hello vývojáři aplikace společnosti musí být schopný toodiscover data zákazníků a osobní v hello aplikací, které jsou vytvořené, které můžete provést s Azure Search.
- Vývojáři musí být schopný toofind osobních údajů v jejich databázi dokumentů.

## <a name="azure-active-directory-data-discovery"></a>Azure Active Directory: Data zjišťování

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) je společnosti Microsoft založená na cloudu, víceklientské adresáře a identity management service. Můžete vyhledat zákazníka a samotnými zaměstnanci uživatelských profilů a pracovní informace o uživateli, které obsahují osobní údaje v vaše [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) prostředí pomocí hello [portál Azure](https://portal.azure.com/).

To je zvláště užitečné, pokud chcete toofind nebo změnit osobních údajů pro konkrétního uživatele. Můžete také přidat nebo změnit uživatelský profil a informace o práci. Přihlaste se pod účtem, který je globálním správcem adresáře hello.

### <a name="how-do-i-locate-or-view-user-profile-and-work-information"></a>Jak najít nebo zobrazit uživatelský profil a informace o práci?

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.

2. Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.

   ![jak najít profil uživatele a informace o práci](media/how-to-discover-classify-personal-data-azure/user-profile.png)

3. Na hello **uživatelů a skupin** vyberte **uživatelé**.

  ![Otevírání uživatelů a skupin](media/how-to-discover-classify-personal-data-azure/users-groups.png)

4. Na hello **uživatelé a skupiny - Uživatelé** okně vyberte uživatele ze seznamu hello a poté v okně hello hello vybraného uživatele, vyberte možnost **profil** tooview informace o profilu uživatele, může obsahovat osobní data .

  ![Vyberte uživatele](media/how-to-discover-classify-personal-data-azure/select-user.png)

5. Pokud potřebujete tooadd nebo změnit informace o profilu uživatele, můžete tak učinit a na panelu příkazů hello, vyberte **uložit.**
6. V okně hello hello vybraného uživatele, vyberte **fungovat informace** tooview uživatele pracovní informace, které mohou obsahovat osobní data.

 ![informace o zobrazení zaměstnání](media/how-to-discover-classify-personal-data-azure/work-info.png)

7. Pokud potřebujete tooadd nebo změnit informace o práci uživatelů, můžete tak učinit a na panelu příkazů hello, vyberte **uložit.**

## <a name="azure-sql-database-data-discovery"></a>Azure SQL Database: Data zjišťování

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) je databáze cloudu, která pomáhá vývojářům vytvářet a udržovat aplikace. Osobní údaje lze nalézt v [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) pomocí standardní dotazů SQL. Azure elastické dotazu SQL (preview) umožňuje uživatelům tooperform cross databázové dotazy.

Podrobné [databáze SQL](../sql-database/sql-database-technical-overview.md) kurz vysvětluje mnoho aspektů použití databáze systému SQL, včetně jak toobuild jeden a jak toorun dotazy na data. Hello Následuje souhrn hello informace, které jsou k dispozici v kurzu hello s odkazy toospecific oddíly.

### <a name="how-do-i-build-a-sql-database"></a>Jak lze vytvořit databázi SQL?

Existují tři způsoby toodo ho:

- Azure SQL database lze vytvořit v hello [portál Azure](https://portal.azure.com/). V kurzu hello použijete určitou sadu výpočetní a úložnou kapacitu v rámci skupiny prostředků a logického serveru. Ukázková data z fiktivní společnosti AdventureWorks budete používat. Pokud vytvoříte pravidla brány firewall na úrovni serveru. toolearn jak toodo tento, navštivte hello [vytvoření Azure SQL database v hello portál Azure](../sql-database/sql-database-get-started-portal.md) kurzu.

  ![Vytvoření databáze Azure SQL](media/how-to-discover-classify-personal-data-azure/create-database.png)
- Databáze SQL můžete také vytvořit v hello [prostředí cloudu Azure](https://azure.microsoft.com/features/cloud-shell/) rozhraní příkazového řádku, nástroj příkazového řádku založené na prohlížeči. Nástroj Hello je k dispozici v hello portál Azure a můžete spustit přímo z ní. V tomto kurzu spusťte nástroj hello, definovat skriptu proměnné, vytvořte skupinu prostředků a logického serveru a nakonfigurujte pravidlo brány firewall serveru. Potom můžete vytvořit databázi s ukázkovými daty. toolearn jak toocreate databáze tímto způsobem, navštivte hello [vytvářet izolované databáze Azure SQL pomocí rozhraní příkazového řádku Azure hello](../sql-database/sql-database-get-started-cli.md) kurzu.

  ![Kurz CLIT](media/how-to-discover-classify-personal-data-azure/cli-tutorial.png)

>[!NOTE]
Rozhraní příkazového řádku Azure se často používá Linux správci a vývojáři. Někteří uživatelé ji najít jednodušší a intuitivnější než prostředí PowerShell, což je třetí možnost.

- Nakonec můžete vytvořit databázi SQL pomocí prostředí PowerShell, který je toocreate použít nástroj příkazového řádku nebo skriptu a spravovat Azure a dalším prostředkům. V tomto kurzu spusťte nástroj hello, definovat skriptu proměnné, vytvořte skupinu prostředků a logického serveru a nakonfigurujte pravidlo brány firewall serveru. Pak vytvoříte databázi s ukázkovými daty.

kurz Hello vyžaduje hello prostředí Azure PowerShell verze modulu 4.0 nebo novější. Spusťte Get-Module - ListAvailable AzureRM toofind vaší verzí. Pokud potřebujete tooinstall nebo aktualizace, najdete v části modulu nainstalovat Azure PowerShell.

```PowerShell
New-AzureRmSQLDatabase -ResourceGroupName $resourcegroupname `
-ServerName $servername `
-DatabaseName $databasename `
-RequestedServiceObjectiveName "s0"
```

toolearn jak toocreate databáze tímto způsobem, navštivte hello [vytvářet izolované databáze Azure SQL pomocí prostředí Powershell](../sql-database/sql-database-get-started-powershell.md) kurzu.

>[!Note]
Správci Windows mívají toouse prostředí PowerShell, ale některé z nich raději rozhraní příkazového řádku Azure.

### <a name="how-do-i-search-for-personal-data-in-sql-database-in-hello-azure-portal"></a>Jak je vyhledávání pro osobní data v databázi SQL v hello portál Azure? **

Hello integrovaného dotazu editor nástroj uvnitř hello Azure portálu toosearch můžete použít pro osobní data. Budete protokolu v nástroji toohello vaše přihlašovací jméno správce serveru SQL a heslo a potom zadejte dotaz.

  ![vyhledávání sql pomocí portálu hello](media/how-to-discover-classify-personal-data-azure/search-sql-portal.png)

Kroku 5 tohoto kurzu hello ukazuje příklad dotazu v podokně editor hello dotazu, ale není soustředit na osobních nebo citlivých informací (také zkombinuje data ze dvou tabulek a vytvoří aliasy pro zdrojový sloupec hello v sadě dat hello nevrátila). Hello následující snímek obrazovky ukazuje hello dotazu z kroku 5, jakož i hello podokně výsledků, která je vrácena:

  ![editor dotazů](media/how-to-discover-classify-personal-data-azure/query-editor.png)

Pokud vaše databáze byla volána MyTable, ukázkový dotaz osobní informace může obsahovat název, číslo sociálního pojištění a identifikační číslo a bude vypadat takto:

"Vyberte název, číslo sociálního pojištění, ID číslo z MyTable"

By spustit hello dotaz a pak zobrazte výsledky hello v hello **výsledky** podokně.

Další informace o tom, jak tooquery SQL databáze v hello portálu Azure, najdete v článku hello [hello dotaz do databáze SQL](../sql-database/sql-database-get-started-portal.md) části kurzu hello.

### <a name="how-do-i-search-for-data-across-multiple-databases"></a>Jak vyhledávat data napříč více databází?

Elastické dotazu SQL (preview) umožňuje vám tooperform mezidatabázové a více dotazů databáze a vrátí jeden výsledek. Hello [přehled kurzu](../sql-database/sql-database-elastic-query-overview.md) obsahuje podrobný popis scénáře a vysvětluje hello rozdíl mezi databáze svislého a vodorovného rozdělení do oddílů. Vodorovné rozdělení do oddílů se označuje jako "horizontálního dělení."

  ![Vertikální dělení](media/how-to-discover-classify-personal-data-azure/vertical-partition.png)

  ![vodorovné rozdělení do oddílů](media/how-to-discover-classify-personal-data-azure/horizontal.png)

tooget začít, navštivte hello [přehled elastické dotazu Azure SQL Database (preview)](../sql-database/sql-database-elastic-query-overview.md) stránky.

#### <a name="power-query-for-importing-azure-hdinsight-hadoop-clusters-data-discovery-for-large-data-sets"></a>Power Query (pro import clusterů systému Azure HDInsight Hadoop): data zjišťování pro velké sady dat

Hadoop je typu open source Apache úložiště a zpracování služby pro velké sady dat, které jsou analyzovány a uložené v clusterů systému Hadoop. [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) umožňuje uživatelům toowork s Hadoop clusterů v Azure. Power Query je doplněk aplikace Excel, mimo jiné, pomáhají uživatelům zjistit data z různých zdrojů.

Osobní data související s clustery systému Hadoop v prostředí Azure HDInsight mohou být importované tooExcel doplněk Power Query. Po hello data jsou v aplikaci Excel můžete dotazu tooidentify ho.

#### <a name="how-do-i-use-excel-power-query-tooimport-hadoop-clusters-in-azure-hdinsight-into-excel"></a>Použití doplňku Power Query pro Excel tooimport Hadoop clusterů v Azure HDInsight do Excelu?

HDInsight kurz vás provede tento celý proces. Popisuje požadavky a zahrnuje odkaz tooa [Začínáme s Azure HDInsight](../hdinsight/hdinsight-hadoop-linux-tutorial-get-started.md) kurzu. Pokyny zahrnují Excel 2016 a také 2013 a 2010 (postup je mírně odlišný pro starší verze Excelu hello). Pokud nemáte doplňku Power Query aplikace Excel hello, hello kurz ukazuje, jak tooget ho. Budete úvodní kurz hello v aplikaci Excel a bude nutné toohave účet úložiště objektů Blob Azure přidruženého k vašemu clusteru.

  ![Dotaz v aplikaci Excel](media/how-to-discover-classify-personal-data-azure/excel.png)

toolearn jak toodo tento, navštivte hello [tooHadoop připojení aplikace Excel pomocí doplňku Power Query](../hdinsight/hdinsight-connect-excel-power-query.md) kurzu.

Zdroj: [tooHadoop připojení aplikace Excel pomocí doplňku Power Query](../hdinsight/hdinsight-connect-excel-power-query.md)

## <a name="azure-information-protection-personal-data-classification-for-documents-and-email"></a>Azure Information Protection: osobní data klasifikace pro dokumenty a e-mailu

[Azure Information Protection](https://www.microsoft.com/cloud-platform/azure-information-protection) může pomoci Azure zákazníků použít popisky tooclassify a chránit interně nebo externě sdílené dokumenty a e-mailu komunikace. Některé z těchto položek může obsahovat osobní údaje zákazníků nebo zaměstnanců. Pravidla a podmínky je možné definovat automaticky nebo ručně, správci nebo uživatelé. Například pokud uživatel je ukládání dokumentu, který obsahuje informace o kreditní kartě, potvrdí by zobrazí popisek doporučení, která byla nakonfigurována správcem hello.

### <a name="how-do-i-try-it"></a>Jak zkuste ho?

Pokud chcete toogive Azure Information Protection toosee zkuste je možné přizpůsobit pro vaši organizaci, navštivte hello [rychlý úvodní kurz](https://docs.microsoft.com/information-protection/get-started/infoprotect-quick-start-tutorial). Provede vás prostřednictvím 5 základních kroků – z instalace tooconfiguring zásad tooseeing klasifikace, označování a sdílení v akci – a provést méně než půl hodiny.

### <a name="how-do-i-deploy-it"></a>Jak můžu tuto službu nasadit?

Pokud chcete toodeploy Azure Information Protection ve vaší organizaci, navštivte hello [plán nasazení pro klasifikaci, označování a ochranu](https://docs.microsoft.com/information-protection/plan-design/deployment-roadmap).

### <a name="is-there-anything-else-i-should-know"></a>Další informace je třeba vědět?

Doplňující informace, které vám pomohou pečlivě promyslete jak se tooset ho, navštivte hello [připravené, nastavit, ochrana!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
příspěvku na blogu. A kontrola hello Další Další odkazy níže uvedené další informace o Azure Information Protection.

## <a name="azure-search-data-discovery-for-developer-apps"></a>Vyhledávání systému Azure: zjišťování dat pro vývojáře aplikací

[Služba Azure Search](https://azure.microsoft.com/services/search/) je cloudové řešení vyhledávání pro vývojáře a nabízí bohaté data vyhledávání prostředí pro vaše aplikace. Služba Azure Search umožňuje toolocate dat napříč uživatelem definované indexy, která byla vytvořena z Azure kosmetický salón DB, databáze SQL Azure, Azure Blob Storage, Azure Table storage nebo zákazník vlastní JSON data. Můžete také struktury dotazů Lucene pomocí hello REST API služby Azure Search toosearch pro typy osobní údaje nebo osobní data hello konkrétní osoby. Funkce zahrnují fulltextové vyhledávání, jednoduchá syntaxe dotazů a syntaxe dotazů Lucene. 

## <a name="how-do-i-use-sql-tooquery-data"></a>Použití dat tooquery SQL

toobegin s hello základy, navštivte hello [CosmosD databázi Azure: jak tooquery pomocí SQL](../cosmos-db/tutorial-query-documentdb.md) kurzu. Hello výukový ukázka dokumentu a dva ukázkové dotazy SQL a výsledky.

Další podrobné pokyny k vytváření dotazů SQL, najdete v článku [dotazy SQL pro rozhraní API služby Azure Cosmos DB dokumentu DB.](../cosmos-db/documentdb-sql-query.md)

Pokud jste nový tooAzure Cosmos DB a by jako toolearn jak toocreate databáze, přidejte kolekce a přidejte data, navštivte hello [Cosmos databázi Azure: vytvoření webové aplikace DocumentDB API](../cosmos-db/create-documentdb-dotnet.md) rychlý úvodní kurz. Pokud chcete toodo v jiném jazyce než v rozhraní .NET, například Java nebo Python, zvolte pouze preferovaný jazyk po získání toohello lokality.

## <a name="next-steps"></a>Další kroky

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50)

[Co je SQL Database?](../sql-database/sql-database-technical-overview.md)

[SQL databáze Editor dotazů na portálu Azure k dispozici] (https://azure.microsoft.com/blog/t-sql-query-editor-in-browser-azure-portal/)

[Co je Azure Information Protection?](https://docs.microsoft.com/information-protection/understand-explore/what-is-information-protection)

[Co je Azure Rights Management?](https://docs.microsoft.com/information-protection/understand-explore/what-is-azure-rms)

[Azure Information Protection: Připraveni, nastavit, ochrana!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
