---
title: "aaaIdentify pokročilé analýzy scénáře pro Azure Machine Learning | Microsoft Docs"
description: "Vyberte odpovídající scénáře hello to advanced prediktivní analýzy s hello proces vědecké účely dat Team."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 53aecc1e-5089-42cf-8d44-77678653f92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 52c6bb10d6df4f640a4f66cf17cf4993cc1067b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Scénáře pro pokročilé analýzy ve službě Azure Machine Learning
Tento článek popisuje různé hello ukázkových zdrojů dat a cílové scénáře, které mohou být zpracována hello [tým datové vědy procesu (TDSP)](data-science-process-overview.md). Hello TDSP poskytuje systematicky pro týmy toocollaborate na budování inteligentní aplikace. scénáře Hello uvedeny v tomto tématu ilustrují možnosti dostupné v pracovním postupu hello zpracování dat, které jsou závislé na vlastností hello dat, umístění zdrojových souborů a cílové úložiště v Azure.

Hello **rozhodovací strom** pro výběr hello ukázkových scénářů, které jsou vhodné pro vaše data a cíl se zobrazí v poslední části hello.

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

Každý z následujících částí hello uvede vzorový scénář. Pro každý scénář, vědecké zpracování možných dat nebo pokročilou analýzu jsou uvedeny toku a podpůrné prostředků Azure.

> [!NOTE]
> **Pro všechny hello následující scénáře budete muset:**
> <br/>
> 
> * [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md)
>   <br/>
> * [Vytvořit pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md)
> 
> 

## <a name="smalllocal"></a>Scénář \#1: malý toomedium tabulkové datovou sadu v místních souborů
![Malé toomedium místních souborů][1]

#### <a name="additional-azure-resources-none"></a>Další prostředky Azure: žádné
1. Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
2. Nahrání datové sady.
3. Vytvoření experimentu tok Azure Machine Learning, počínaje nahrané datových sad.

## <a name="smalllocalprocess"></a>Scénář \#2: malé toomedium datové sady místních souborů, které vyžadují zpracování
![Malé toomedium místních souborů s zpracování][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Další prostředky Azure: virtuální počítač Azure (server IPython Poznámkový blok)
1. Vytvořte virtuální počítač Azure s IPython Poznámkový blok.
2. Nahrajte kontejner dat tooan úložiště Azure.
3. Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z kontejneru úložiště Azure.
4. Transformace dat toocleaned, formě tabulky.
5. Uložte Transformovaná data do objektů BLOB Azure.
6. Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
7. Čtení dat hello z Azure BLOB pomocí hello [importovat Data] [ import-data] modulu.
8. Vytvoření experimentu tok Azure Machine Learning, počínaje ingestovaný datových sad.

## <a name="largelocal"></a>Scénář \#3: velké datové sady místních souborů, cílení objektů BLOB služby Azure
![Velké místních souborů][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Další prostředky Azure: virtuální počítač Azure (server IPython Poznámkový blok)
1. Vytvořte virtuální počítač Azure s IPython Poznámkový blok.
2. Nahrajte kontejner dat tooan úložiště Azure.
3. Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z Azure BLOB.
4. Transformace dat toocleaned, formě tabulky, v případě potřeby.
5. Prozkoumejte data a podle potřeby vytvářet funkce.
6. Extrahujte vzorku dat. malé a střední.
7. Uložení hello vzorků dat v Azure BLOB.
8. Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
9. Čtení dat hello z Azure BLOB pomocí hello [importovat Data] [ import-data] modulu.
10. Sestavení Azure Machine Learning experimentu toku počínaje ingestovaný datových sad.

## <a name="smalllocaltodb"></a>Scénář \#4: malá toomedium datové sady místních souborů, cílení na SQL Server virtuálním počítači Azure
![Malé toomedium místních souborů tooSQL DB v Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)
1. Vytvořte virtuální počítač Azure systémem SQL Server + IPython Poznámkový blok.
2. Nahrajte kontejner dat tooan úložiště Azure.
3. Předběžně zpracovat a vyčistit data v kontejneru úložiště Azure pomocí IPython Poznámkový blok.
4. Transformace dat toocleaned, formě tabulky, v případě potřeby.
5. Uložte soubory tooVM místní data (IPython poznámkového bloku běží na virtuálním počítači, místní jednotky získáte tooVM jednotek).
6. Načtěte data tooSQL serveru databázi spuštěna na virtuálním počítači Azure.
   
   Možnost \#1: pomocí nástroje SQL Server Management Studio.
   
   * Přihlášení tooSQL virtuální počítač Server
   * Spusťte SQL Server Management Studio.
   * Vytváření tabulek databáze a cíle.
   * Použijte jeden z hello hromadné importovat metody tooload hello data z virtuálního počítače – místní soubory.
   
   Možnost \#2: pomocí IPython poznámkového bloku – není vhodné pro střední a větší datové sady
   
   <!-- -->    
   * Pomocí rozhraní ODBC připojovací řetězec tooaccess systému SQL Server na virtuálním počítači.
   * Vytváření tabulek databáze a cíle.
   * Použijte jeden z hello hromadné importovat metody tooload hello data z virtuálního počítače – místní soubory.
7. Prozkoumejte data, podle potřeby vytvářet funkce. Všimněte si, že funkce hello nemusí toobe materializována v tabulkách databáze hello. Pouze Všimněte si toocreate nezbytné dotazu hello je.
8. Vyberte velikost vzorku dat, pokud potřebné a/nebo potřeby.
9. Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
10. Čtení hello data přímo z hello systému SQL Server pomocí hello [importovat Data] [ import-data] modulu. Hello vložení nezbytné se dotaz, který extrahuje pole, vytvoří funkce a v případě potřeby přímo v hello – ukázky data [importovat Data] [ import-data] dotazu.
11. Sestavení Azure Machine Learning experimentu toku počínaje ingestovaný datových sad.

## <a name="largelocaltodb"></a>Scénář \#5: velké datové sady v místních souborů cíle systému SQL Server ve virtuálním počítači Azure
![TooSQL velké místních souborů databáze v Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)
1. Vytvořte virtuální počítač Azure systémem SQL Server a server IPython Poznámkový blok.
2. Nahrajte kontejner dat tooan úložiště Azure.
3. (Volitelné) Předběžně zpracovat a vyčistit data.
   
   a.  Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z Azure
   
       blobs.
   
   b.  Transformace dat toocleaned, formě tabulky, v případě potřeby.
   
   c.  Uložte soubory tooVM místní data (IPython poznámkového bloku běží na virtuálním počítači, místní jednotky získáte tooVM jednotek).
4. Načtěte data tooSQL serveru databázi spuštěna na virtuálním počítači Azure.
   
   a.  Přihlášení tooSQL virtuální počítač Server.
   
   b.  Pokud data nebyla uložena již, stahování datových souborů z Azure
   
       storage container toolocal-VM folder.
   
   c.  Spusťte SQL Server Management Studio.
   
   d.  Vytváření tabulek databáze a cíle.
   
   e.  Použijte jeden z hello hromadně importovat data hello tooload metody.
   
   f.  Pokud spoje tabulky platná, vytvořte spojení tooexpedite indexy.
   
   > [!NOTE]
   > Rychlejší načítání velikostí velkých objemů dat, se doporučuje vytvořit dělené tabulky a hromadně importovat data hello paralelně. Další informace najdete v tématu [Parallel Data importovat tooSQL rozdělena na oddíly tabulky](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).
   > 
   > 
5. Prozkoumejte data, podle potřeby vytvářet funkce. Všimněte si, že funkce hello nemusí toobe materializována v tabulkách databáze hello. Pouze Všimněte si toocreate nezbytné dotazu hello je.
6. Vyberte velikost vzorku dat, pokud potřebné a/nebo potřeby.
7. Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
8. Čtení hello data přímo z hello systému SQL Server pomocí hello [importovat Data] [ import-data] modulu. Hello vložení nezbytné se dotaz, který extrahuje pole, vytvoří funkce a v případě potřeby přímo v hello – ukázky data [importovat Data] [ import-data] dotazu.
9. Jednoduchý experiment toku Azure Machine Learning počínaje nahrané datové sady

## <a name="largedbtodb"></a>Scénář \#6: velké datové sady v SQL Server databáze místní, cílení na SQL Server virtuálním počítači Azure
![TooSQL velké místní databáze SQL DB v Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)
1. Vytvořte virtuální počítač Azure systémem SQL Server a server IPython Poznámkový blok.
2. Použijte jeden z datových hello exportovat metody tooexport hello data z toodump soubory systému SQL Server.
   
   > [!NOTE]
   > Pokud se rozhodnete toomove všechna data z hello místní databázi, alternativní (rychlejší) metoda toomove hello úplné databáze toothe instanci systému SQL Server v Azure. Přeskočit hello kroky tooexport dat, vytvoření databáze a zatížení nebo importovat data toohello cílovou databázi a postupujte podle alternativní metodu hello.
   > 
   > 
3. Nahrajte kontejner úložiště tooAzure odkládacích souborů.
4. Načíst hello data tooa databáze systému SQL Server spuštěna na virtuální počítač Azure.
   
   a.  Přihlášení toohello virtuální počítač SQL Server.
   
   b.  Stahování datových souborů ze složky místní VM toohello kontejneru úložiště Azure.
   
   c.  Spusťte SQL Server Management Studio.
   
   d.  Vytváření tabulek databáze a cíle.
   
   e.  Použijte jeden z hello hromadně importovat data hello tooload metody.
   
   f.  Pokud spoje tabulky platná, vytvořte spojení tooexpedite indexy.
   
   > [!NOTE]
   > Pro rychlejší načítání velikostí velkých objemů dat, vytvoření oddílů tabulky a toobulk importovat hello data paralelně. Další informace najdete v tématu [Parallel Data importovat tooSQL rozdělena na oddíly tabulky](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).
   > 
   > 
5. Prozkoumejte data, podle potřeby vytvářet funkce. Všimněte si, že funkce hello nemusí toobe materializována v tabulkách databáze hello. Pouze Všimněte si toocreate nezbytné dotazu hello je.
6. Vyberte velikost vzorku dat, pokud potřebné a/nebo potřeby.
7. Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
8. Čtení hello data přímo z hello systému SQL Server pomocí hello [importovat Data] [ import-data] modulu. Hello vložení nezbytné se dotaz, který extrahuje pole, vytvoří funkce a v případě potřeby přímo v hello – ukázky data [importovat Data] [ import-data] dotazu.
9. Jednoduché Azure Machine Learning experimentu toku počínaje nahrané datovou sadu.

### <a name="alternate-method-toocopy-a-full-database-from-an-on-premises--sql-server-tooazure-sql-database"></a>Alternativní metoda toocopy úplné databáze z SQL serveru tooAzure místní databáze SQL
![Místní databáze odpojit a připojit tooSQL DB v Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)
tooreplicate hello celou databázi SQL serveru ve vaší virtuální počítač SQL Server, měli byste zkopírovat databázi z jednoho serveru nebo umístění tooanother, za předpokladu, že hello databázi můžete provedeny dočasně offline. Můžete to udělat v hello Průzkumník objektů systému SQL Server Management Studio nebo pomocí hello ekvivalentní příkazy jazyka Transact-SQL.

1. Odpojení databáze hello v umístění zdroje hello. Další informace najdete v tématu [odpojení databáze](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).
2. V okně Průzkumníka Windows nebo příkazového řádku Windows hello kopie odpojit soubor databáze nebo soubory a soubor protokolu nebo soubory toohello cílové umístění na hello virtuální počítač s SQL serverem v Azure.
3. Připojte instanci systému SQL Server hello zkopírovat soubory toohello cíl. Další informace najdete v tématu [připojit databázi](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).

[Přesuňte databázi pomocí odpojit a připojit (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)

## <a name="largedbtohive"></a>Scénář \#7: velkých objemů dat v místních souborech cílové databáze Hive v Azure HDInsight Hadoop clusterů
![Velké objemy dat v místní cíl Hive][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Další prostředky Azure: clusteru Azure HDInsight Hadoop a virtuální počítač Azure (server IPython Poznámkový blok)
1. Vytvořte virtuální počítač Azure serverem IPython Poznámkový blok.
2. Vytvoření clusteru Azure HDInsight Hadoop.
3. (Volitelné) Předběžně zpracovat a vyčistit data.
   
   a.  Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z Azure
   
       blobs.
   
   b.  Transformace dat toocleaned, formě tabulky, v případě potřeby.
   
   c.  Uložte soubory tooVM místní data (IPython poznámkového bloku běží na virtuálním počítači, místní jednotky získáte tooVM jednotek).
4. Nahrání dat toohello výchozí kontejner clusteru Hadoop hello vybrali v kroku hello 2.
5. Načíst data tooHive databázi v clusteru Azure HDInsight Hadoop.
   
   a.  Přihlaste se toohello hlavního uzlu v clusteru Hadoop hello
   
   b.  Otevřete hello Hadoop příkazového řádku.
   
   c.  Zadejte kořenový adresář hello Hive pomocí příkazu `cd %hive_home%\bin` v Hadoop příkazového řádku.
   
   d.  Spusťte hello Hive dotazy toocreate databáze a tabulky a načtení dat z tabulky tooHive úložiště objektů blob.
   
   > [!NOTE]
   > Pokud je velkých objemů dat hello, uživatelé mohou vytvářet hello Hive tabulky s oddíly. Uživatelé pak mohou používat `for` smyčky v hello Hadoop příkazového řádku na hello hlavního uzlu tooload data do tabulky Hive hello rozdělena na oddíly oddílem.
   > 
   > 
6. Prozkoumejte data a vytvářet funkcí podle potřeby v Hadoop příkazového řádku. Všimněte si, že funkce hello nemusí toobe materializována v tabulkách databáze hello. Pouze Všimněte si toocreate nezbytné dotazu hello je.
   
   a.  Přihlaste se toohello hlavního uzlu v clusteru Hadoop hello
   
   b.  Otevřete hello Hadoop příkazového řádku.
   
   c.  Zadejte kořenový adresář hello Hive pomocí příkazu `cd %hive_home%\bin` v Hadoop příkazového řádku.
   
   d.  Spouštění dotazů Hive hello v Hadoop příkazového řádku hello hlavního uzlu hello Hadoop clusteru tooexplore hello dat a vytvořte funkcí podle potřeby.
7. Pokud potřebné a/nebo potřeby, ukázkové hello toofit dat v Azure Machine Learning Studio.
8. Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).
9. Čtení dat hello přímo z hello `Hive Queries` pomocí hello [importovat Data] [ import-data] modulu. Hello vložení nezbytné se dotaz, který extrahuje pole, vytvoří funkce a v případě potřeby přímo v hello – ukázky data [importovat Data] [ import-data] dotazu.
10. Jednoduché Azure Machine Learning experimentu toku počínaje nahrané datovou sadu.

## <a name="decisiontree"></a>Rozhodovací strom pro výběr scénář
- - -
Hello následující diagram shrnuje hello scénáře popsané výše a hello pokročilé analýzy proces a technologie možnosti zvolené vyžadující tooeach hello uvedeno scénáře. Všimněte si, že může trvat zpracování dat, průzkum, funkce inženýrství a vzorkování umístit do jednoho nebo více metoda prostředí – u hello zdroje, zprostředkující, nebo cílové prostředí – a může pokračovat interaktivně, podle potřeby. Hello diagram pouze slouží jako ilustraci některé možné toků a neposkytuje vyčerpávající výčet.

![Vzorové DS proces návod scénáře][8]

### <a name="advanced-analytics-in-action-examples"></a>Pokročilé analýzy v akci příklady
Pro návody začátku do konce Azure Machine Learning, která využívají hello pokročilé analýzy proces a technologie pomocí veřejné datové sady najdete v tématu:

* [Tým proces vědecké účely dat v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md).
* [Tým proces vědecké účely dat v akci: pomocí clusterů systému HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md).

[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
