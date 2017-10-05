---
title: "Zřízení virtuálního počítače Microsoft Data vědecké účely | Microsoft Docs"
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
ms.openlocfilehash: 76cd54cd234dfe43e8f0d61f0b66f0ed0c09e8b7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="provision-the-microsoft-data-science-virtual-machine"></a>Zřízení virtuálního počítače Microsoftu pro vědecké zkoumání dat
Virtuální počítač Microsoft Data vědecké účely je předem nainstalovaná a nakonfigurovaná s několik oblíbených nástrojů, které se běžně používají k analýze dat a strojové učení image virtuálního počítače (VM) systému Windows Azure. Nástroje sady jsou:

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
  * [Rattle](http://rattle.togaware.com/) (R Analytical nástroj pro další snadno): nástroj, který umožňuje Začínáme s data analytics a počítač učení v R snadno s zkoumání dat založených na grafickém uživatelském rozhraní a modelování pomocí automatického generování kódu R.
  * [mxnet](https://github.com/dmlc/mxnet): hloubkové learning rozhraní určené pro efektivitu a flexibilitu
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/) : dolování visual dat a strojové učení softwaru v jazyce Java.
  * [Rozbalení Apache](https://drill.apache.org/): bez schémat modul dotazů SQL pro Hadoop, NoSQL a úložiště v cloudu.  Podporuje rozhraní ODBC a JDBC umožňující dotazování NoSQL a soubory ze standardních nástrojů BI Power BI, Excel, Tableau.
* Knihovny v R a Python pro použití v Azure Machine Learning a jinými službami Azure
* Git, včetně Git Bash pro práci s včetně GitHub, Visual Studio Team Services úložišť zdrojového kódu
* Porty systému Windows nástroje pro několik oblíbených Linux příkazového řádku (včetně awk, menšit, perl, grep, najít, wget, curl atd.) přístupné prostřednictvím příkazového řádku. 

Provádění vědecké zpracování dat zahrnuje iterace v pořadí úloh:

1. Hledání, načítání a předem zpracování dat.
2. Vytváření a testování modely
3. Nasazování modelů pro používání v inteligentní aplikace

Datových vědců použít celou řadu nástrojů pro dokončení těchto úloh. Může trvat poměrně dlouho najít odpovídající verze softwaru a pak stáhnout a nainstalovat je. Virtuální počítač Microsoft Data vědecké účely můžete usnadňují tato zatížení poskytováním připravených k použití bitovou kopii, která se dá zřídit v Azure s všechny několik oblíbených nástrojů předem nainstalovaná a nakonfigurovaná. 

Virtuální počítač Microsoft Data vědecké účely jump-starts projektu analytics. Umožňuje pracovat na úlohy v různých jazycích včetně R, Python, SQL a C#. Visual Studio poskytuje rozhraní IDE pro vývoj a testování vaší kód, který se snadno používá. Azure SDK zahrnuté ve virtuálním počítači umožňuje sestavení vaší aplikace s použitím různých služeb na Cloudová platforma společnosti Microsoft. 

Neexistují žádné poplatky softwaru pro tuto bitovou kopii dat vědecké účely virtuálních počítačů. Platíte jenom pro poplatky za Azure použití které závisí na velikosti virtuálního počítače, které zřízení. Další informace o poplatky za výpočetní naleznete v části Podrobnosti cena na [datové vědy virtuálního počítače](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) stránky. 

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Jiné verze datové vědy virtuálního počítače
A [CentOS](machine-learning-data-science-linux-dsvm-intro.md) bitové kopie je také k dispozici s mnoha stejné nástroje jako bitovou kopii systému Windows. [Ubuntu](machine-learning-data-science-dsvm-ubuntu-intro.md) obrázek k dispozici také, v mnoha podobné nástroje plus přímým učení architektury.

## <a name="prerequisites"></a>Požadavky
Než bude možné vytvořit virtuální počítač Microsoft Data vědecké účely, musíte mít následující:

* **Předplatné Azure**: ho získat, najdete v části [získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Účet úložiště Azure**: Chcete-li vytvořit, přečtěte si téma [vytvořit účet úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account). Alternativně lze vytvořit účet úložiště v rámci procesu vytvoření virtuálního počítače, pokud nechcete použít existující účet.

## <a name="create-your-microsoft-data-science-virtual-machine"></a>Vytvořit virtuální počítač Microsoft Data vědecké účely
Tady jsou kroky k vytvoření instance na datové vědě virtuální počítač Microsoft:

1. Přejděte k virtuálnímu počítači výpis na [portál Azure](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).
2. Vyberte **vytvořit** tlačítko dole mají být provedeny do průvodce.![ Konfigurace data-vědecké účely vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3. Průvodce slouží k vytvoření virtuálního počítače Microsoft Data vědecké účely vyžaduje **vstupy** pro každou z **v pěti krocích** uvedené na pravé straně tohoto obrázku. Zde jsou vstupy potřebná ke konfiguraci jednotlivých kroků:
   
   1. **Základy**
      
      1. **Název**: název vašeho serveru vědecké účely data vytváříte.
      2. **Uživatelské jméno**: id přihlášení účtu správce.
      3. **Heslo**: heslo účtu správce.
      4. **Předplatné**: Pokud máte více než jedno předplatné, vyberte ten, na kterém se tento počítač je vytvořen a účtují.
      5. **Skupina prostředků**: můžete vytvořit novou nebo použít stávající skupinu.
      6. **Umístění**: Vyberte datové centrum, která je nejvhodnější. Obvykle je datové centrum, které má většina vašich dat, nebo je nejblíže vašemu fyzické umístění pro nejrychlejší přístup k síti.
   2. **Velikost**: Vyberte jeden z typů serveru, které splňuje požadavek na funkční a náklady na omezení. Další možnosti velikostí virtuálních počítačů můžete získat tak, že vyberete "Zobrazit vše".
   3. **Nastavení**:
      
      1. **Typ disku**: Zvolte Premium Pokud preferujete jednotku SSD (SSD), jinak vyberte "Standard".
      2. **Účet úložiště**: můžete vytvořit nový účet úložiště Azure ve vašem předplatném nebo použijte existující ve stejné *umístění* na který jste vybrali **Základy** krok průvodce.
      3. **Další parametry**: obvykle stačí použít výchozí hodnoty. Můžete ukazatel myši přesunete na informační odkaz Nápověda v konkrétních polí v případě, že chcete zvažte použití jiné než výchozí hodnoty.
   4. **Souhrn**: Zkontrolujte, zda jsou všechny informace, které jste zadali správné.
   5. **Kupte si**: klikněte na tlačítko **koupit** zahájíte přidělení přístupových práv. Je k dispozici odkaz na podmínky transakce. Virtuální počítač nemá žádné další poplatky za výpočetní pro velikost serveru, který jste zvolili v **velikost** krok. 

> [!NOTE]
> Zajišťování zabere asi 10-20 minut. Stav zřizování se zobrazí na portálu Azure.
> 
> 

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Jak získat přístup k virtuální počítač Microsoft Data vědecké účely
Po vytvoření virtuálního počítače můžete do ní pomocí přihlašovacích údajů účtu správce, které jste nakonfigurovali v předchozím vzdálené plochy **Základy** části. 

Jakmile virtuálního počítače se vytvoří a zřizovat, jste připraveni začít používat nástroje, které jsou nainstalované a nakonfigurované na něm. Existují dlaždice úvodní nabídky a ikony na ploše pro řadu nástrojů. 

## <a name="how-to-create-a-strong-password-for-jupyter-and-start-the-notebook-server"></a>Jak vytvořit silné heslo pro Jupyter a spuštění serveru poznámkového bloku
Ve výchozím nastavení je server poznámkového bloku Jupyter předem nakonfigurované ale zakázána ve virtuálním počítači, dokud jste nastavili heslo Jupyter. Chcete-li vytvořit silné heslo pro server Poznámkový blok Jupyter v počítači nainstalován, spusťte následující příkaz z příkazového řádku na datové vědě virtuálního počítače nebo dvakrát klikněte na zástupce na ploše uvádíme názvem **Jupyter nastavit heslo & počáteční** z účtu místního Správce virtuálních počítačů.

    C:\dsvm\tools\setup\JupyterSetPasswordAndStart.cmd

Postupujte podle zprávy a zvolte silné heslo po zobrazení výzvy.

Uvedený skript vytvoří hodnotu hash hesla a v konfiguračním souboru Jupyter umístěná v úložišti: **C:\ProgramData\jupyter\jupyter_notebook_config.py** pod názvem parametr ***c.NotebookApp.password***.

Skript také umožňuje a spusťte Jupyter server na pozadí. Jupyter server je vytvořen jako windows úloh v Plánovači úloh WIndows říká **Start_IPython_Notebook**.  Možná budete muset Počkejte několik sekund po nastavení hesla před otevřením poznámkového bloku v prohlížeči. Najdete v následující části s názvem **Poznámkový blok Jupyter** o tom, jak získat přístup k serveru poznámkového bloku Jupyter. 


## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Nástroje nainstalované na Microsoft Data vědecké účely virtuálního počítače

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Pokud chcete použít pro analytické údaje R, virtuální počítač má nainstalovaný Microsoft R Server Developer edition. Microsoft R Server je platforma analytics široce nasadit podnikové třídy podle R, který je podporován, škálovatelné a zabezpečené. Podpora různých statistiky velkých objemů dat, prediktivního modelování a strojového učení možnosti, R Server podporuje celou řadu analytics – zkoumání, analýzu, vizualizace a modelování. Pomocí a rozšíření s otevřeným zdrojem R, Microsoft R Server je plně kompatibilní se skripty R, funkce a balíčků CRAN k analýze dat škálované enterprise. Přidáním bloku a paralelní zpracování dat řeší také omezení v paměti R otevřete zdroje. To umožňuje spustit analýzy dat mnohem větší, než co nejlépe odpovídá v hlavní paměti.  Visual Studio Community Edition zahrnuté do virtuálního počítače obsahuje nástroje pro R pro rozšíření sady Visual Studio, který obsahuje úplnou IDE pro práci s R. Můžete také stáhnout a použít jiné integrovaného vývojového prostředí, stejně jako [Rstudia](http://www.rstudio.com). 

### <a name="python"></a>Python
Pro vývoj pomocí Python byl nainstalován distribuce Anaconda Python 2.7 a 3.5. Toto rozdělení obsahuje základní Python společně s přibližně 300 nejoblíbenější balíčků analytics matematické, technici a data. Můžete použít nástroje Python Tools pro Visual Studio (PTVS) nainstalované v rámci edice Visual Studio 2015 Community nebo jeden z integrovaného vývojového prostředí dodávat s Anaconda jako nečinnosti nebo Spyder. Můžete spustit jeden z těchto oblastí vyhledávání v panelu vyhledávání (**Win** + **S** klíč).

> [!NOTE]
> Chcete-li bod nástroje Python Tools pro sadu Visual Studio v Anaconda Python 2.7 a 3.5, vytvořte vlastní prostředí pro každou verzi. Chcete-li nastavit tyto cesty prostředí Visual Studio 2015 Community Edition, přejděte na **nástroje** -> **Python Tools** -> **prostředí Python** a pak klikněte na **+ vlastní**. 
> 
> 

Anaconda Python 2.7 je nainstalován v části C:\Anaconda a Anaconda Python 3.5 je nainstalována v části c:\Anaconda\envs\py35. V tématu [dokumentaci k těmto nástrojům](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) podrobné pokyny. 

### <a name="jupyter-notebook"></a>Poznámkový blok Jupyter
Anaconda distribuční také obsahuje poznámkového bloku Jupyter, prostředí sdílení kódu a analýzy. Server poznámkového bloku Jupyter předem nakonfigurovaný s Python 2.7, Python 3.4, Python 3.5 a R jádra. Je ikony na ploše s názvem "Poznámkový blok Jupyter spustit prohlížeč pro přístup k serveru poznámkového bloku. Pokud jste ve virtuálním počítači přes vzdálenou plochu, můžete také navštívit [https://localhost:9999 /](https://localhost:9999/) pro přístup k serveru poznámkového bloku Jupyter při přihlášení k virtuálnímu počítači.

> [!NOTE]
> Pokračujte, pokud chcete získat všechna upozornění certifikátu. 
> 
> 

Budeme mít zabalené několik poznámkových bloků ukázka v Pythonu a v jazyce R. Jupyter notebooks ukazují, jak pracovat s Microsoft R Server, SQL Server 2016 R Services (v databázi analytics), Python, Microsoft kognitivní ToolKit (CNTK) pro přímý learning a další technologie Azure po přihlášení do Jupyter. Zobrazí se odkaz na ukázky na domovské stránce poznámkového bloku po ověření do poznámkového bloku Jupyter s heslem, kterou jste vytvořili v předchozím kroku. 

### <a name="visual-studio-2015-community-edition"></a>Edice Visual Studio 2015 Community
Visual Studio Community edition nainstalovaný na Virtuálním počítači. Je bezplatnou verzi oblíbených IDE od společnosti Microsoft, který můžete použít pro účely hodnocení a pro malé týmy. Můžete zkontrolovat si licenční podmínky [zde](https://www.visualstudio.com/support/legal/mt171547).  Dvojitým kliknutím na ikony ploše otevřete Visual Studio nebo **spustit** nabídky. Můžete také vyhledat programy s **Win** + **S** a zadání "Visual Studio". Jakmile se tam můžete vytvořit projektů v jazyce jazycích C#, Python, R, node.js. Moduly plug-in jsou nainstalovány také zajišťujících pohodlné pro práci se službami Azure, jako je Azure Data Catalog, Azure HDInsight (Hadoop, Spark) a Azure Data Lake. 

> [!NOTE]
> Může se zobrazit zpráva, že se vaše zkušební období vypršelo. Zadejte přihlašovací údaje účtu Microsoft nebo vytvořit nový bezplatný účet získat přístup k Visual Studio Community Edition. 
> 
> 

### <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Developer edition
Vývojáře verzi SQL Server 2016 se službou spuštění analýzy v databázi služeb R je k dispozici ve virtuálním počítači. R služby poskytují platformu pro vývoj a nasazení inteligentní aplikace. Jazyk R bohatý a výkonný a velký počet balíčků od komunity můžete použít k vytváření modelů a generování předpovědi pro data systému SQL Server. Analýza blízko data můžete zachovat, protože R služby (v databázi) jazyk R integraci se službou SQL Server. Tím se eliminuje náklady a bezpečnostních rizicích spojených s přesun dat.

> [!NOTE]
> Edice SQL serveru 2016 vývojáře slouží pouze pro testovací účely vývoje a. Potřebujete licenci na spuštění v produkčním prostředí. 
> 
> 

SQL server můžete přistupovat spuštěním **SQL Server Management Studio**. Název virtuálního počítače se importují jako název serveru. Pomocí ověřování systému Windows při přihlášení jako správce v systému Windows. Jakmile jste na SQL Server Management Studio můžete vytvořit další uživatelé, vytváření databází, umožňuje importovat data a spouštět dotazy SQL. 

Pokud chcete povolit analytics v databázi pomocí Microsoft R, spusťte následující příkaz jako na čas akce v aplikaci SQL Server management studio po přihlášení jako správce serveru. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Několik nástrojů Azure jsou nainstalovány ve virtuálním počítači:

* Je zástupce na ploše pro přístup k dokumentaci Azure SDK. 
* **AzCopy**: používá k přesunu dat do aplikace a z účtu úložiště Microsoft Azure. Chcete-li zobrazit využití, zadejte **Azcopy** na příkazovém řádku zobrazíte využití. 
* **Microsoft Azure Storage Explorer**: umožňuje procházet objekty, které jsou uloženy v rámci účtu úložiště Azure a přenos dat do a z úložiště Azure. Můžete zadat **Storage Explorer** v Hledat nebo najít v nabídce Start systému Windows pro přístup k tento nástroj. 
* **Adlcopy**: používá k přesunu dat do Azure Data Lake. Chcete-li zobrazit využití, zadejte **adlcopy** v příkazovém řádku. 
* **dtui**: používá k přesunu dat do a z Azure Cosmos databáze, databáze NoSQL v cloudu. Typ **dtui** na příkazovém řádku. 
* **Brána pro správu dat**: umožňuje přesun dat mezi místní zdroje dat a cloudem. Používá se v rámci nástroje, například Azure Data Factory. 
* **Microsoft Azure Powershell**: nástroj používaný ke správě prostředků Azure v prostředí Powershell skriptovací jazyk. je také nainstalován na váš počítač. 

### <a name="power-bi"></a>Power BI
Pomoc při vytváření řídicích panelů a vizualizací skvělé, **Power BI Desktop** byl nainstalován. Tento nástroj použijte k získání dat z různých zdrojů, chcete-li vytvářet řídicí panely a sestavy a publikovat je do cloudu. Informace najdete v tématu [Power BI](http://powerbi.microsoft.com) lokality. Power BI desktop můžete najít v nabídce Start. 

> [!NOTE]
> Je nutné účet Office 365 pro přístup k Power BI. 
> 
> 

## <a name="additional-microsoft-development-tools"></a>Další vývojové nástroje společnosti Microsoft
[ **Instalačního programu webové platformy Microsoft** ](https://www.microsoft.com/web/downloads/platform.aspx) lze použít ke zjištění a stáhnout jiné vývojové nástroje společnosti Microsoft. Je také zástupce nástroj, který poskytuje na ploše Microsoft Data vědecké účely virtuálního počítače.  

## <a name="important-directories-on-the-vm"></a>Důležité adresáře na virtuálním počítači
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
> Instance systému Microsoft Data vědecké účely virtuální počítač před 1.5.0 (před 3. září 2016) vytvořit použít mírně odlišné adresářovou strukturu, než je zadáno v předchozí tabulce. 
> 
> 

## <a name="next-steps"></a>Další kroky
Tady jsou některé další kroky, chcete-li pokračovat, učení a zkoumání. 

* Prozkoumejte různé vědě nástrojů data na vědecké zpracování dat virtuálního počítače tak, že kliknete nabídky start v seznamu v nabídce nástroje se odhlašuje.
* Přejděte na **C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** ukázek pomocí knihovny RevoScaleR v R, který podporuje analýzy dat škálované enterprise.  
* Přečtěte si článek: [10 způsobů, jak na vědecké zpracování dat virtuálního počítače](http://aka.ms/dsvmtenthings)
* Naučte se vytvářet koncová analytická řešení systematičtěji pomocí [proces vědecké účely dat Team](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Přejděte [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com) pro machine learning a data analýzy ukázky používající Cortana Intelligence Suite. Taky uvádíme ikonu na **spustit** nabídky a na ploše virtuálního počítače do této galerie.

