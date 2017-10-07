---
title: "aaaProvision hello virtuální počítač Microsoft Data vědecké účely | Microsoft Docs"
description: "Konfigurace a vytvoření virtuálního počítače vědecké účely dat v Azure pro analýzy a strojového učení."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev
ms.openlocfilehash: 907a3bdc7e480d05e8e245f5e50d632900fcf471
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provision-hello-microsoft-data-science-virtual-machine"></a>Hello zřídit virtuální počítač Microsoft Data vědecké účely
Hello Microsoft Data vědecké účely virtuálního počítače je předem nainstalovaná a nakonfigurovaná s několik oblíbených nástrojů, které se běžně používají k analýze dat a strojové učení image virtuálního počítače (VM) systému Windows Azure. jsou zahrnuty nástroje Hello:

* Microsoft R Server Developer Edition
* Anaconda distribuci jazyka Python
* Poznámkový blok Jupyter (s R, Python jádra)
* Visual Studio Community Edition
* Power BI Desktop
* SQL Server 2016 Developer Edition
* Machine learning a analýza dat nástrojů
  * [Výpočetní sítě Toolkit (CNTK)](https://github.com/Microsoft/CNTK): hluboká učení softwaru nástrojů Microsoft Research.
  * [K dispozici Vowpal](https://github.com/JohnLangford/vowpal_wabbit): rychlé strojového učení systému, který podporuje, jako jsou online a hash, allreduce, snížení, learning2search, aktivní a interaktivní učení.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): nástroj poskytuje rychlé a přesné boosted stromu implementace.
  * [Rattle](http://rattle.togaware.com/) (hello Analytical nástroj R tooLearn snadno): nástroj, který umožňuje Začínáme s data analytics a počítač učení v R snadno s zkoumání dat založených na grafickém uživatelském rozhraní a modelování pomocí automatického generování kódu R.
  * [mxnet](https://github.com/dmlc/mxnet): hloubkové learning rozhraní určené pro efektivitu a flexibilitu
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/) : dolování visual dat a strojové učení softwaru v jazyce Java.
  * [Rozbalení Apache](https://drill.apache.org/): bez schémat modul dotazů SQL pro Hadoop, NoSQL a úložiště v cloudu.  Podporuje rozhraní ODBC a JDBC tooenable rozhraní dotazování NoSQL a soubory ze standardních nástrojů BI Power BI, Excel, Tableau.
* Knihovny v R a Python pro použití v Azure Machine Learning a jinými službami Azure
* Git, včetně toowork Git Bash s včetně GitHub, Visual Studio Team Services úložišť zdrojového kódu
* Porty systému Windows nástroje pro několik oblíbených Linux příkazového řádku (včetně awk, menšit, perl, grep, najít, wget, curl atd.) přístupné prostřednictvím příkazového řádku. 

Provádění vědecké zpracování dat zahrnuje iterace v pořadí úloh:

1. Hledání, načítání a předem zpracování dat.
2. Vytváření a testování modely
3. Nasazení hello modely pro používání v inteligentní aplikace

Datových vědců pomocí různých nástrojů toocomplete tyto úlohy. Můžete být časově velmi náročná toofind hello příslušné verze hello softwaru a pak stáhnout a nainstalovat je. Hello Microsoft Data vědecké účely virtuálního počítače můžete toto zatížení usnadňují poskytováním připravených k použití bitovou kopii, která se dá zřídit v Azure s všechny několik oblíbených nástrojů předem nainstalovaná a nakonfigurovaná. 

Virtuální počítač Microsoft Data vědecké účely Hello jump-starts projektu analytics. Umožní vám toowork úloh v různých jazycích včetně R, Python, SQL a C#. Visual Studio poskytuje toodevelop IDE a otestujte svůj kód, který je snadno toouse. Hello součástí hello virtuálních počítačů Azure SDK vám umožní toobuild vaší aplikace s použitím různých služeb na Cloudová platforma společnosti Microsoft. 

Neexistují žádné poplatky softwaru pro tuto bitovou kopii dat vědecké účely virtuálních počítačů. Platíte jenom pro poplatky za Azure použití hello které závisí na velikosti hello hello virtuálního počítače, které zřízení. Další informace o hello výpočetní poplatky naleznete v části Podrobnosti o cena na hello hello [datové vědy virtuálního počítače](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) stránky. 

## <a name="other-versions-of-hello-data-science-virtual-machine"></a>Jiné verze hello datové vědy virtuálního počítače
A [CentOS](machine-learning-data-science-linux-dsvm-intro.md) obrázek k dispozici, s mnoha hello stejné nástroje jako hello bitové kopie systému Windows. [Ubuntu](machine-learning-data-science-dsvm-ubuntu-intro.md) obrázek k dispozici také, v mnoha podobné nástroje plus přímým učení architektury.

## <a name="prerequisites"></a>Požadavky
Než bude možné vytvořit virtuální počítač Microsoft Data vědecké účely, musíte mít následující hello:

* **Předplatné Azure**: jeden, viz tooobtain [získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Účet úložiště Azure**: jeden, viz toocreate [vytvořit účet úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account). Alternativně lze vytvořit účet úložiště hello jako součást procesu hello vytváření hello virtuálních počítačů, pokud nechcete, aby toouse existující účet.

## <a name="create-your-microsoft-data-science-virtual-machine"></a>Vytvořit virtuální počítač Microsoft Data vědecké účely
Zde jsou kroky toocreate hello instanci hello Microsoft Data vědecké účely virtuálního počítače:

1. Přejděte toohello virtuální počítač výpis na [portál Azure](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).
2. Vyberte hello **vytvořit** tlačítko v toobe dolní hello vzít do průvodce.![ Konfigurace data-vědecké účely vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3. Hello používá Průvodce toocreate hello Microsoft Data vědecké účely virtuální počítač vyžaduje **vstupy** pro každou hello **v pěti krocích** uvedené na hello napravo od tohoto obrázku. Zde jsou hello vstupy potřeby tooconfigure každý z těchto kroků:
   
   1. **Základy**
      
      1. **Název**: název vašeho serveru vědecké účely data vytváříte.
      2. **Uživatelské jméno**: id přihlášení účtu správce.
      3. **Heslo**: heslo účtu správce.
      4. **Předplatné**: Pokud máte více než jedno předplatné, vyberte hello jednou na které hello počítače je toobe vytvořen a účtují.
      5. **Skupina prostředků**: můžete vytvořit novou nebo použít stávající skupinu.
      6. **Umístění**: Vyberte hello datového centra, která je nejvhodnější. Obvykle je hello datové centrum, které má většina vašich dat, nebo je nejbližší fyzické umístění tooyour pro nejrychlejší přístup k síti.
   2. **Velikost**: Vyberte jeden z typů hello serveru, které splňuje požadavek na funkční a náklady na omezení. Další možnosti velikostí virtuálních počítačů můžete získat tak, že vyberete "Zobrazit vše".
   3. **Nastavení**:
      
      1. **Typ disku**: Zvolte Premium Pokud preferujete jednotku SSD (SSD), jinak vyberte "Standard".
      2. **Účet úložiště**: můžete vytvořit nový účet úložiště Azure ve vašem předplatném nebo použijte existující hello stejné *umístění* který jste vybrali na hello **Základy** kroku průvodce hello.
      3. **Další parametry**: obvykle stačí použít hello výchozí hodnoty. Můžete podržet přes propojení informační hello nápovědu k hello konkrétních polí v případě, že chcete tooconsider hello použití jiné než výchozí hodnoty.
   4. **Souhrn**: Zkontrolujte, zda jsou všechny informace, které jste zadali správné.
   5. **Kupte si**: klikněte na tlačítko **koupit** toostart hello zřizování. Podmínky toohello hello transakce je k dispozici odkaz. Hello virtuální počítač nemá jakýchkoli dalších poplatků nad rámec hello výpočetní pro velikost server hello jste zvolili v hello **velikost** krok. 

> [!NOTE]
> zřizování Hello zabere asi 10-20 minut. Hello Stav zřizování hello se zobrazí na portálu Azure hello.
> 
> 

## <a name="how-tooaccess-hello-microsoft-data-science-virtual-machine"></a>Jak tooaccess hello Microsoft Data vědecké účely virtuálního počítače
Jednou hello vytvoří virtuální počítač, můžete ke vzdálené ploše do pomocí pověření účtu správce hello, které jste nakonfigurovali v předchozím hello **Základy** části. 

Jakmile virtuálního počítače se vytvoří a zřizovat, jste připravené toostart pomocí hello nástrojů, které jsou nainstalované a nakonfigurované na něm. Existují dlaždice úvodní nabídky a ikony na ploše pro řadu nástrojů hello. 

## <a name="how-toocreate-a-strong-password-for-jupyter-and-start-hello-notebook-server"></a>Jak toocreate silné heslo pro Jupyter a spusťte hello serveru poznámkového bloku
Ve výchozím nastavení je server poznámkového bloku Jupyter hello předem nakonfigurované ale na hello virtuálních počítačů zakázáno, dokud nenastavíte Jupyter heslo. volá spuštění hello následující příkaz z příkazového řádku na hello datové vědy virtuálního počítače nebo dvakrát klikněte na zástupce na ploše hello uvádíme toocreate silné heslo pro server poznámkového bloku Jupyter hello nainstalovat na počítač hello  **Nastavit heslo Jupyter & počáteční** z účtu místního Správce virtuálních počítačů.

    C:\dsvm\tools\setup\JupyterSetPasswordAndStart.cmd

Postupujte podle zprávy hello a zvolte silné heslo po zobrazení výzvy.

Hello předchozí skript vytvoří hodnotu hash hesla a v konfiguračním souboru na hello Jupyter umístěná v úložišti: **C:\ProgramData\jupyter\jupyter_notebook_config.py** pod názvem parametr hello ***c. NotebookApp.password***.

skript Hello také umožňuje a spusťte hello Jupyter server hello pozadí. Jupyter server je vytvořen jako volána úloha windows v hello plánovače úloh systému WIndows, **Start_IPython_Notebook**.  Toowait může mít několik sekund po nastavení hello hesla před otevřením hello poznámkového bloku v prohlížeči. Projděte si část hello níže, s názvem **Poznámkový blok Jupyter** na tom, jak tooaccess hello serveru poznámkového bloku Jupyter. 


## <a name="tools-installed-on-hello-microsoft-data-science-virtual-machine"></a>Nástroje nainstalované na hello Microsoft Data vědecké účely virtuálního počítače

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Pokud chcete pro analytické údaje toouse R, hello virtuální počítač má nainstalovaný Microsoft R Server Developer edition. Microsoft R Server je platforma analytics široce nasadit podnikové třídy podle R, který je podporován, škálovatelné a zabezpečené. Podpora různých statistiky velkých objemů dat, prediktivního modelování a strojového učení možnosti, R Server podporuje celou řadu analytics – zkoumání, analýzu, vizualizace a modelování hello. Pomocí a rozšíření s otevřeným zdrojem R, Microsoft R Server je plně kompatibilní se skripty R, funkce a balíčků CRAN, data tooanalyze škálované enterprise. Přidáním paralelní a bloku zpracování dat řeší také omezení v paměti hello R otevřete zdroje. To vám umožní toorun analýzy datových mnohem větší, než co nejlépe odpovídá v hlavní paměti.  Visual Studio Community Edition zahrnuty v hello virtuální počítač obsahuje hello R nástroje pro rozšíření sady Visual Studio, který obsahuje úplnou IDE pro práci s R. Můžete také stáhnout a použít jiné integrovaného vývojového prostředí, stejně jako [Rstudia](http://www.rstudio.com). 

### <a name="python"></a>Python
Pro vývoj pomocí Python byl nainstalován distribuce Anaconda Python 2.7 a 3.5. Toto rozdělení obsahuje hello základní Python společně s přibližně 300 hello nejoblíbenější matematické, technici a data balíčků analytics. Můžete použít nástroje Python Tools pro Visual Studio (PTVS) nainstalované v rámci edice Visual Studio 2015 Community hello nebo jeden z integrovaného vývojového prostředí dodávat s Anaconda jako nečinnosti nebo Spyder hello. Můžete spustit jeden z těchto oblastí vyhledávání v panelu vyhledávání hello (**Win** + **S** klíč).

> [!NOTE]
> toopoint hello Python Tools pro Visual Studio v Anaconda Python 2.7 a 3.5, musíte pro každou verzi toocreate vlastní prostředí. tooset tyto cesty prostředí v hello Visual Studio 2015 Community Edition, přejděte příliš**nástroje** -> **Python Tools** -> **prostředí Python** a pak klikněte na **+ vlastní**. 
> 
> 

Anaconda Python 2.7 je nainstalován v části C:\Anaconda a Anaconda Python 3.5 je nainstalována v části c:\Anaconda\envs\py35. V tématu [dokumentaci k těmto nástrojům](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) podrobné pokyny. 

### <a name="jupyter-notebook"></a>Poznámkový blok Jupyter
Anaconda distribuční také obsahuje poznámkového bloku Jupyter, kód tooshare prostředí a analýzy. Server poznámkového bloku Jupyter předem nakonfigurovaný s Python 2.7, Python 3.4, Python 3.5 a R jádra. Je ikony na ploše s názvem "Poznámkový blok Jupyter toolaunch hello prohlížeče tooaccess hello serveru poznámkového bloku. Pokud jste na hello virtuálních počítačů přes vzdálenou plochu, můžete také navštívit [https://localhost:9999 /](https://localhost:9999/) tooaccess hello server poznámkového bloku Jupyter při přihlášení toohello virtuálních počítačů.

> [!NOTE]
> Pokračujte, pokud chcete získat všechna upozornění certifikátu. 
> 
> 

Budeme mít zabalené několik ukázkových poznámkových bloků v Pythonu a v jazyce R. hello zobrazit poznámkové bloky Jupyter jak toowork s Microsoft R Server, SQL Server 2016 R Services (v databázi analytics), Python, Microsoft kognitivní ToolKit (CNTK) pro přímý učení a dalších Azure technologie po přihlášení tooJupyter. Po ověření poznámkového bloku Jupyter toohello hello heslem, kterou jste vytvořili v předchozím kroku, uvidí hello odkaz toohello ukázky na domovskou stránku hello poznámkového bloku. 

### <a name="visual-studio-2015-community-edition"></a>Edice Visual Studio 2015 Community
Visual Studio Community edition nainstalovaný na hello virtuálních počítačů. Je bezplatnou verzi hello oblíbených IDE od společnosti Microsoft, který můžete použít pro účely hodnocení a pro malé týmy. Můžete zkontrolovat na hello licenční smlouvy, které [zde](https://www.visualstudio.com/support/legal/mt171547).  Otevřete Visual Studio dvojitým kliknutím na ikony na ploše hello nebo hello **spustit** nabídky. Můžete také vyhledat programy s **Win** + **S** a zadání "Visual Studio". Jakmile se tam můžete vytvořit projektů v jazyce jazycích C#, Python, R, node.js. Moduly plug-in jsou nainstalovány také usnadňující pohodlný toowork se službami Azure, jako je Azure Data Catalog, Azure HDInsight (Hadoop, Spark) a Azure Data Lake. 

> [!NOTE]
> Může se zobrazit zpráva, že se vaše zkušební období vypršelo. Zadejte přihlašovací údaje účtu Microsoft nebo vytvořte novou toohello přístup bezplatný účet tooget Visual Studio Community Edition. 
> 
> 

### <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Developer edition
Vývojáře verze systému SQL Server 2016 s R služby toorun v databázi analytics je k dispozici na hello virtuálních počítačů. R služby poskytují platformu pro vývoj a nasazení inteligentní aplikace. Můžete použít jazyk R hello bohatý a výkonný a hello mnoho balíčky z hello komunity toocreate modelů a generovat předpovědi pro data systému SQL Server. Analýza dat zavřít toohello můžete zachovat, protože R služby (v databázi) integrovat hello R jazyka SQL Server. Tím se eliminuje náklady hello a bezpečnostních rizicích spojených s přesun dat.

> [!NOTE]
> Hello SQL Server 2016 developer edition lze použít pouze pro vývoj a testování účely. Potřebujete licence toorun ho v produkčním prostředí. 
> 
> 

Hello SQL serveru můžete přistupovat spuštěním **SQL Server Management Studio**. Název virtuálního počítače se importují jako hello název serveru. Pomocí ověřování systému Windows, když se přihlásí jako správce v systému Windows hello. Jakmile jste na SQL Server Management Studio můžete vytvořit další uživatelé, vytváření databází, umožňuje importovat data a spouštět dotazy SQL. 

analytics tooenable v databázi pomocí Microsoft R, spusťte následující příkaz jako jednu hello čas akce v aplikaci SQL Server management studio po přihlášení jako správce serveru hello. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace hello %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Několik nástrojů Azure jsou nainstalovány na hello virtuálních počítačů:

* Dokumentaci k sadě Azure SDK hello není tooaccess zástupce na ploše. 
* **AzCopy**: používá toomove dat do aplikace a z účtu úložiště Microsoft Azure. toosee využití, typ **Azcopy** na použití příkazového řádku toosee hello. 
* **Microsoft Azure Storage Explorer**: používá toobrowse prostřednictvím hello objekty, které jsou uloženy v rámci vaší tooand dat účet úložiště Azure a přenos z úložiště Azure. Můžete zadat **Storage Explorer** Hledat nebo najít ho na hello tooaccess nabídky systému Windows spusťte tento nástroj. 
* **Adlcopy**: používá toomove data tooAzure Data Lake. toosee využití, typ **adlcopy** v příkazovém řádku. 
* **dtui**: používá toomove tooand dat z Azure Cosmos databáze, databáze NoSQL v cloudu hello. Typ **dtui** na příkazovém řádku. 
* **Brána pro správu dat**: umožňuje přesun dat mezi místní zdroje dat a cloudem. Používá se v rámci nástroje, například Azure Data Factory. 
* **Microsoft Azure Powershell**: nástroj používaný tooadminister vašich prostředků Azure v hello Powershell skriptovací jazyk. je také nainstalován na váš počítač. 

### <a name="power-bi"></a>Power BI
toohelp sestavování řídicích panelů a vizualizací skvělé, hello **Power BI Desktop** byl nainstalován. Použít tento nástroj toopull data z různých zdrojů, tooauthor vaše řídicí panely a sestavy a toopublish je toohello cloudu. Informace najdete v tématu hello [Power BI](http://powerbi.microsoft.com) lokality. Power BI desktop můžete najít v nabídce Start hello. 

> [!NOTE]
> Budete potřebovat Office 365 účet tooaccess Power BI. 
> 
> 

## <a name="additional-microsoft-development-tools"></a>Další vývojové nástroje společnosti Microsoft
Hello [ **instalačního programu webové platformy Microsoft** ](https://www.microsoft.com/web/downloads/platform.aspx) lze použít toodiscover a stáhnout jiné vývojové nástroje společnosti Microsoft. Je také zástupce toohello nástroj, který poskytuje na ploše hello Microsoft Data vědecké účely virtuálního počítače.  

## <a name="important-directories-on-hello-vm"></a>Důležité adresáře na hello virtuálních počítačů
| Položka | Adresář |
| --- | --- |
| Konfigurace serveru poznámkového bloku Jupyter |C:\ProgramData\jupyter |
| Domovský adresář ukázky poznámkového bloku Jupyter |c:\dsvm\notebooks |
| Další ukázky |c:\dsvm\samples |
| Anaconda (výchozí: Python 2.7) |c:\Anaconda |
| Prostředí Python 3.5 anaconda |c:\Anaconda\envs\py35 |
| R Server samostatné instance adresáře (podle R3.2.2 R výchozí instance) |C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| R Server v databázi instance adresáře (R3.2.2) |C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Instance adresáře Microsoft R otevřete (R3.3.1) |C:\Program Files\Microsoft\MRO-3.3.1 |
| Různé nástroje |c:\dsvm\tools |

> [!NOTE]
> Instance hello Microsoft Data vědecké účely virtuální počítač vytvořen před 1.5.0 (před 3. září 2016) používá mírně odlišný adresářovou strukturu, než je zadáno v předcházející tabulce hello. 
> 
> 

## <a name="next-steps"></a>Další kroky
Tady jsou některé další kroky toocontinue učení a zkoumání. 

* Prozkoumejte různé vědě nástroje data na hello vědecké zpracování dat virtuálního počítače kliknutím hello nabídka start a odhlašuje hello nástrojů uvedených v nabídce hello hello.
* Přejděte příliš**C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** ukázek pomocí knihovny RevoScaleR hello v R, který podporuje analýzy dat škálované enterprise.  
* Přečíst článek hello: [10 způsobů, jak na hello vědecké zpracování dat virtuálního počítače](http://aka.ms/dsvmtenthings)
* Zjistěte, jak toobuild end tooend analytická řešení systematičtěji pomocí hello [proces vědecké účely dat Team](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Navštivte hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com) pro machine learning a data analýzy ukázky tohoto použití hello Cortana Intelligence Suite. Taky uvádíme ikonu na hello **spustit** nabídky a na ploše hello Galerie toothis hello virtuálního počítače.

