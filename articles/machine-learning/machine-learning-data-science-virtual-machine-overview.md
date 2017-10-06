---
title: "aaaWhat je virtuální počítač vědecké účely dat? | Dokumentace Microsoftu"
description: "Jak tooget spustit provádění scénáře klíče analytics s Data vědecké účely virtuálních počítačů."
keywords: "datové vědy nástroje, datové vědy virtuálního počítače, nástroje pro vědecké zpracování dat, vědecké zpracování dat linux"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: 18c7a75208671c663f3b6be6ee8d0bf666772e01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Úvod toohello cloudových dat vědecké účely virtuálního počítače pro systémy Linux a Windows
Hello datové vědy virtuálního počítače (DSVM) je v cloudu společnosti Microsoft Azure vytvořené speciálně pro provádění vědecké zpracování dat vlastní image virtuálního počítače. Obsahuje mnoho vědecké zpracování oblíbených dat a dalších nástrojů předem nainstalovaná a předem nakonfigurované toojump-start vytváření inteligentní aplikace pro pokročilou analýzu. Je k dispozici pro Windows Server a Linux. Edici virtuálního počítače pro datové vědy pro Windows nabízíme ve verzích pro Server 2016 a Server 2012. Nabízíme edici hello DSVM na Ubuntu 16.04 LTS a na základě OpenLogic 7.2 CentOS Linuxových distribucích systému Linux. 

Toto téma popisuje, co můžete dělat s hello vědecké účely dat virtuálních počítačů, popisuje některé z hello klíčové scénáře použití hello virtuálních počítačů, rozepisuje hello klíčové funkce dostupné v systému Windows hello a verze Linux a obsahuje pokyny, jak spustit tooget jejich používání.

## <a name="what-can-i-do-with-hello-data-science-virtual-machine"></a>Co můžete dělat s hello datové vědy virtuálního počítače?
cílem Hello hello datové vědy virtuálního počítače je Odborníci v oblasti dat tooprovide na všech úrovních dovedností a rolí s prostředím data bez třecí vědecké účely. Tento virtuální počítač uloží významnou čas strávený by pokud byl nasazen porovnatelný z hlediska prostředí sami. Místo toho spuštění projektu vědecké účely data okamžitě v nově vytvořená instance virtuálních počítačů. 

Hello vědecké účely dat virtuálních počítačů a konfigurují pro práci s scénáře obecné použití. Prostředí můžete škálovat nahoru nebo dolů podle projektu změn. Budete se moct toouse úlohami upřednostňovaný jazyk data tooprogram vědecké účely. Můžete nainstalovat další nástroje a přizpůsobení hello systému pro vašim konkrétním potřebám.

## <a name="key-scenarios"></a>Klíčové scénáře
Tato část navrhuje některých klíčových scénářů, pro které hello se dá nasadit vědecké účely dat virtuálních počítačů.

### <a name="preconfigured-analytics-desktop-in-hello-cloud"></a>Předkonfigurované plochy analýzy v cloudu hello
Hello vědecké účely dat virtuálních počítačů poskytuje standardních hodnot konfigurace pro data vědecké účely týmům tooreplace jejich místní počítačů s plocha spravovaný cloudu. Tyto standardní hodnoty zajišťuje, že všechny hello datových vědců na tým konzistentní instalační program, pomocí které tooverify experimenty a zlepšení spolupráce. Ukládání na hello času potřeba tooevaluate, instalaci a údržbě hello různé softwarové balíčky třeba toodo pokročilou analýzu a také snižuje náklady na snížením hello sysadmin zatížení.  

### <a name="data-science-training-and-education"></a>Školení a vzdělávání v oblasti datové vědy
Enterprise školitele a lektorům, které obvykle naučit datové vědy třídy poskytují image tooensure virtuální počítač, zda jejich studenty instalace s konzistentní a předvídatelné fungovat hello ukázky. Hello vědecké účely Data virtuálního počítače vytvoří prostředí na vyžádání s konzistentní instalačního programu, která usnadňuje podporu a nekompatibilita výzvy hello. Případech, kde těchto prostředí musí toobe vytvořené často, zejména pro kratší školení třídy, podstatně využívat.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Elastické kapacity na vyžádání u rozsáhlých projektů
Data vědecké účely hackathons/soutěže nebo modelování ve velkém rozsahu dat a zkoumání vyžadují škálovat na více systémů kapacitu hardwaru, obvykle pro krátké doby trvání. Hello vědecké účely dat virtuálních počítačů může pomoci replikovat hello datové vědy prostředí rychle na vyžádání, na škálovaný serverech, které umožňují experimenty nutnosti vysokým výkonem výpočetní prostředky toobe spustit.

### <a name="short-term-experimentation-and-evaluation"></a>Krátkodobé experimentování a vyhodnocení
Hello vědecké účely dat virtuálních počítačů lze použít tooevaluate nebo další nástroje třeba Microsoft R Server, SQL Server, Visual Studio nástroje, Jupyter, hluboké učení / ML sadách a nových nástrojů oblíbených v hello community s minimální instalací úsilí. Vzhledem k tomu, že hello vědecké účely dat virtuálních počítačů můžete rychle nastavit tak, můžete použít v jiné krátkodobé scénáře použití například replikace publikované experimenty, provádění ukázky, následující kurzy v online relací nebo kurzy konference.

### <a name="deep-learning"></a>Hloubkové učení
Hello vědecké zpracování dat virtuálních počítačů lze použít pro model školení pomocí hloubkové learning algoritmů na základě hardwaru grafický procesor (grafiky zpracování jednotky). Použití virtuálních počítačů škálování možnosti cloudu Azure, DSVM vám pomůže používat grafického procesoru na základě hardwaru v cloudu hello podle potřeby. Jeden můžete přepnout tooa grafického procesoru na základě virtuálních počítačů při tréninku velké modely nebo potřebovat hello vysokorychlostní výpočty při zachování stejné disk operačního systému.  edice systému Windows Server 2016 Hello DSVM dodává předinstalované GPU ovladače, architektury a GPU verzi hello hloubkové učení algoritmy. Na hello Linux, přímým učení na grafického procesoru je povolen pouze na hello [datové vědy virtuálního počítače pro Linux (Ubuntu) edici](http://aka.ms/dsvm/ubuntu). Ubuntu/Windows-2016 hello ve verzi datové vědy VM toonon grafického procesoru na základě virtuálního počítače Azure můžete nasadit v takovém případě budou všechna rozhraní hloubkové learning hello režimu záložní toohello procesoru. Dříve, pro Windows Server 2012 jsme publikována [hloubkové učení toolkit](http://aka.ms/dsvm/deeplearning) , ale nyní vám doporučujeme používat systém Windows Server 2016 pro Windows na základě hloubkové learning úlohy. na základě CentOS Linux Hello ve verzi hello DSVM obsahuje pouze hello procesoru sestavení některých hello hloubkové výukové nástroje (CNTK, Tensorflow, MXNet), ale nepochází předinstalované s hello GPU ovladače a rozhraní. 

## <a name="whats-included-in-hello-data-science-vm"></a>Co je součástí hello vědecké účely Data virtuálního počítače?
Hello datové vědy virtuální počítač má mnoho oblíbených datové vědy a přímý výukové nástroje již nainstalován a nakonfigurován. Zahrnuje také nástroje, které umožňují snadno toowork s různými Azure data a analýzy produkty. Vám umožní zkoumat a vytvářet prediktivní modely v rozsáhlých datových sad pomocí hello Microsoft R Server nebo pomocí SQL Server 2016. Hostitel z dalších nástrojů hello open source Community a od společnosti Microsoft jsou také zahrnuté, stejně jako ukázku kódu a poznámkových bloků. Hello následující tabulka rozepisuje a porovná hello hlavní součásti součástí hello Windows a Linux edicích hello datové vědy virtuálního počítače.


| **Nástroj**                                                           | **Edice Windows** | **Edice systému Linux** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R otevřete](https://mran.microsoft.com/open/) pomocí Oblíbené balíčků předinstalovaným   |Ano                      | Ano             |
| [Microsoft R Server](https://msdn.microsoft.com/microsoft-r/) zahrnuje Developer Edition, <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-getting-started) framework paralelní a distribuované vysoký výkon R<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://msdn.microsoft.com/microsoft-r/microsoftml-introduction) -nových algoritmů ML stavu techniky od společnosti Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [R Operationalization](https://msdn.microsoft.com/microsoft-r/operationalize/about)                                            |Ano                      | Ano <br/> (MicrosoftML ještě není k dispozici)|
| [Aplikace Microsoft Office](https://products.office.com/en-us/business/office-365-proplus-business-software) poměrně Plus s sdílené aktivace – aplikace Excel, Word a PowerPoint   |Ano                      |N              |
| [Anaconda Python](https://www.continuum.io/) 2.7, 3.5 pomocí Oblíbené balíčků předinstalovaným    |Ano                      |Ano              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) pomocí Oblíbené balíčků pro jazyk Dita předinstalovaným                         |Ano                      |Ano              |
| Relační databáze                                                            | [SQL Server 2016 SP1](https://www.microsoft.com/sql-server/sql-server-2016) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) |
| Databáze nástroje                                                       | * SQL Server Management Studio <br/>* SQL Server Integration Services<br/>* [BCP, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * Rozhraní ODBC/JDBC ovladače| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (dotazování nástroj), <br /> * bcp, sqlcmd <br /> * Rozhraní ODBC/JDBC ovladače|
| Škálovatelné analýzy v databázi s služby SQL Server R | Ano     |N              |
| **[Server poznámkového bloku Jupyter](http://jupyter.org/) s následující jádra**                                  | Ano     | Ano |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | Ano | Ano |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python 2.7 & 3.5 | Ano | Ano |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Dita | Ano | Ano |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | N | Ano |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | Y (pouze Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | Ano |
| JupyterHub (server poznámkových bloků více uživatelů)| N | Ano |
| **Nástroje pro vývoj, editory integrovaného vývojového prostředí a kódu**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio 2017 (Community Edition)](https://www.visualstudio.com/community/) > pomocí modulu plug-in Git, Azure HDInsight (Hadoop), Data Lake, SQL Server Data tools [Node.js](https://github.com/Microsoft/nodejstools), [Python](http://aka.ms/ptvs), a [R Nástroje pro sadu Visual Studio (RTVS)](http://microsoft.github.io/RTVS-docs/) | Ano | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio Code](https://code.visualstudio.com/) | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rstudia plochy](https://www.rstudio.com/products/rstudio/#Desktop) | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rstudia serveru](https://www.rstudio.com/products/rstudio/#Server) | N | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm](https://www.jetbrains.com/pycharm/) | N | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Atom](https://atom.io/) | N | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Dita IDE)](http://junolab.org/)| Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim a Emacs | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git a Git Bash | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* Rozhraní .net framework | Ano | N |
| PowerBI Desktop | Ano | N |
| Tooaccess sady SDK služby Azure a Cortana Intelligence Suite služeb | Ano | Ano |
| **Přesun dat a nástroje pro správu** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Storage Explorer | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview) | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Powershell | Ano | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | Ano | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | Ano | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Nástroj pro migraci dat DocDB](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | Ano | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Brána pro správu dat](https://msdn.microsoft.com/library/dn879362.aspx) : Přesun dat mezi místní a Cloud | Ano | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* Nástroje příkazového řádku systému Unix/Linux | Ano | Ano |
| [Rozbalení Apache](http://drill.apache.org) pro zkoumání dat | Ano | Ano |
| **Nástroje Machine Learning** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* Integrace s [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [K dispozici Vowpal](https://github.com/JohnLangford/vowpal_wabbit) | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](http://www.cs.waikato.ac.nz/ml/weka/) | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rattle](http://rattle.togaware.com/) | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | N | Y (pouze Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/) | N | Y (pouze Ubuntu) |
| **Grafický procesor na základě hloubkové výukové nástroje** |Edice systému Windows Server 2016  |Ubuntu edition |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Sada nástrojů pro kognitivní (CNTK)](http://cntk.ai) | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Tensorflow](https://www.tensorflow.org/) | Ano | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](http://mxnet.io/) | Ano | Ano|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe & Caffe2](https://github.com/caffe2/caffe2) | N | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Svítilnou](http://torch.ch/) | N | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | N | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| N | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [NVidia číslic](https://github.com/NVIDIA/DIGITS) | N | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Ovladač Nvidia CUDA, CUDNN,](https://developer.nvidia.com/cuda-toolkit) | Ano | Ano |
| **Platforma velké objemy dat (pouze Devtest)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* Místní [Spark](http://spark.apache.org/) samostatné | N | Ano |
| &nbsp;&nbsp;&nbsp;&nbsp;* Místní [Hadoop](http://hadoop.apache.org/) (HDFS, YARN) | N | Ano |



## <a name="how-tooget-started-with-hello-windows-data-science-vm"></a>Jak tooget pracovat s hello virtuální počítač s Windows dat vědecké účely
* Vytvořte instanci edice Windows DSVM hello potřeby tak, že přejdete na
  * [Windows Server 2016 na základě DSVM](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.windows-data-science-vm)
  
  nebo 
  * [Windows Server 2012 na základě DSVM](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) 
* Klikněte na tlačítko hello **získat IT teď** tlačítko.
* Přihlaste se toohello virtuálních počítačů z vzdálené plochy pomocí přihlašovacích údajů hello jste zadali při vytvoření hello virtuálních počítačů.
* toodiscover a spuštění nástroje hello k dispozici, klikněte na tlačítko hello **spustit** nabídky.

## <a name="get-started-with-hello-linux-data-science-vm"></a>Začínáme s hello virtuálního počítače s Linuxem dat vědecké účely
* Vytvořit instanci požadovaného hello Linux DSVM edice přechodem příliš
  * [Ubuntu na základě DSVM](http://aka.ms/dsvm/ubuntu)

  nebo

  * [OpenLogic CentOS na základě DSVM](http://aka.ms/dsvm/centos)

  
* Klikněte na tlačítko hello **ho získat** tlačítko.
* Přihlaste se toohello virtuálních počítačů z klienta SSH, jako je například Putty nebo příkaz SSH, pomocí přihlašovacích údajů hello jste zadali při vytvoření hello virtuálních počítačů.
* V řádku prostředí hello zadejte dsvm. Další informace.
* Grafické stolní počítač, stáhněte si hello X2Go klienta pro vaše klientská platforma [sem](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) a postupujte podle pokynů hello v dokumentu virtuálního počítače s Linuxem datové vědy hello [zřídit hello Linux datové vědy virtuálního počítače](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).

## <a name="next-steps"></a>Další kroky
### <a name="for-hello-windows-data-science-vm"></a>Pro virtuální počítač s Windows datové vědy hello
* Další informace o způsobu nástroje dostupné ve verzi Windows hello toorun konkrétní najdete v tématu [hello zřídit virtuální počítač Microsoft Data vědecké účely](machine-learning-data-science-provision-vm.md) a
* Další informace o tom, jak tooperform různé úlohy, které jsou potřebné pro svůj projekt vědecké účely data na hello virtuální počítač s Windows, najdete v části [deset způsobů, jak na hello vědecké zpracování dat virtuálního počítače](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-hello-linux-data-science-vm"></a>Pro hello virtuálního počítače s Linuxem dat vědecké účely
* Další informace o jak nástroje dostupné ve verzi systému Linux hello toorun konkrétní najdete v tématu [zřídit hello Linux datové vědy virtuální počítač](machine-learning-data-science-linux-dsvm-intro.md).
* Návod, který ukazuje, jak tooperform několik běžných vědecké zpracování dat úlohy s hello virtuálního počítače s Linuxem najdete v tématu [vědecké zpracování dat na hello Linux datové vědy virtuální počítač](machine-learning-data-science-linux-dsvm-walkthrough.md).

