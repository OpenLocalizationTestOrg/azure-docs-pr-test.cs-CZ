---
title: "aaaProvision hello datové vědy virtuálního počítače pro Linux (Ubuntu) v Azure | Microsoft Docs"
description: "Konfigurace a vytvořit na datové vědě virtuálního počítače pro Linux (Ubuntu) na Azure toodo analýzy a strojového učení."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 3bab0ab9-3ea5-41a6-a62a-8c44fdbae43b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 037c126c0a35d8065fc89c591089df73d2b91425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provision-hello-data-science-virtual-machine-for-linux-ubuntu"></a>Zřídit hello datové vědy virtuálního počítače pro Linux (Ubuntu)
Hello datové vědy virtuálního počítače pro Linux je Ubuntu na základě bitové kopie virtuálního počítače, který umožňuje snadno tooget začít s hloubkovým učení v Azure. Hloubkové learning nástroje patří:

  * [Caffe](http://caffe.berkeleyvision.org/): rozhraní hloubkové learning vytvořené pro rychlosti, expressivity a modularitu
  * [Caffe2](https://github.com/caffe2/caffe2): napříč platformami verzi Caffe
  * [Výpočetní sítě Toolkit (CNTK)](https://github.com/Microsoft/CNTK): hluboká učení softwaru nástrojů Microsoft Research
  * [H2O](https://www.h2o.ai/): služby platformy velkých objemů dat open source a grafické uživatelské rozhraní
  * [Keras](https://keras.io/): vysoké úrovně neuronové sítě rozhraní API v Pythonu pro Theano a TensorFlow
  * [MXNet](http://mxnet.io/): knihovnu flexibilní a efektivní hloubkové learning s mnoha jazyk vazby
  * [NVIDIA ČÍSLIC](https://developer.nvidia.com/digits): grafického rozhraní systému, který zjednodušuje běžné úlohy hloubkového učení
  * [TensorFlow](https://www.tensorflow.org/): knihovny open source pro počítač intelligence z Google
  * [Theano](http://deeplearning.net/software/theano/): Knihovna A Python pro definování, optimalizace a efektivně vyhodnocení matematickém výrazu obsahujícího vícerozměrných polí
  * [Svítilnou](http://torch.ch/): scientific výpočetní rozhraní s podporou wide pro algoritmy strojového učení
  * CUDA, cuDNN a ovladač NVIDIA hello
  * Mnoho ukázka poznámkové bloky Jupyter

Všechny knihovny jsou hello GPU verze, i když bude spustit také na hello procesoru.

Hello datové vědy virtuálního počítače pro Linux také obsahuje oblíbených nástrojů pro datové vědy a vývoj aktivity, včetně:

* Microsoft R Server Developer Edition s Microsoft R Open
* Anaconda distribuci jazyka Python (verze 2.7 a 3.5), včetně dat Oblíbené knihovny analýz
* JuliaPro - kurátorované distribuční jazyka Dita pomocí Oblíbené knihovny analytics scientific a data
* Spark samostatné instance a jednoho uzlu Hadoop (HDFS, Yarn)
* JupyterHub - víceuživatelská server poznámkového bloku Jupyter s podporou R, Python, PySpark, Dita jádra
* Azure Storage Explorer
* Rozhraní příkazového řádku Azure (CLI) ke správě prostředků Azure
* Nástroje Machine learning
  * [K dispozici Vowpal](https://github.com/JohnLangford/vowpal_wabbit): rychlé strojového učení systému, který podporuje, jako jsou online a hash, allreduce, snížení, learning2search, aktivní a interaktivní učení
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): nástroj poskytuje rychlé a přesné boosted stromu implementace
  * [Rattle](http://rattle.togaware.com/): grafický nástroj, který umožňuje Začínáme s data analýzy a strojového učení v R snadno
  * [LightGBM](https://github.com/Microsoft/LightGBM): rychlý, distribuované, vysoce výkonné přechodu zvyšovat skóre framework
* Azure SDK v jazyce Java, Python, node.js, Ruby, PHP
* Knihovny v R a Python pro použití v Azure Machine Learning a jinými službami Azure
* Nástroje pro vývoj a editory (Rstudia, PyCharm, IntelliJ, Emacs, vim)


Provádění vědecké zpracování dat zahrnuje iterace v pořadí úloh:

1. Hledání, načítání a předem zpracování dat.
2. Vytváření a testování modely
3. Nasazení hello modely pro používání v inteligentní aplikace

Datových vědců pomocí různých nástrojů toocomplete tyto úlohy. Může být časově velmi náročná toofind hello příslušné verze hello softwaru, a pak toodownload, kompilace a nainstalovat tyto verze.

Hello datové vědy virtuálního počítače pro Linux můžete podstatně usnadňují tato zatížení. Použít toojump spuštění projektu analytics. Umožní vám toowork úloh v různých jazycích, včetně R, Python, SQL, Java a C++. Hello součástí hello virtuálních počítačů Azure SDK vám umožní toobuild vaší aplikace pomocí různých služeb v systému Linux pro Cloudová platforma Microsoft hello. Kromě toho máte přístup jazyky tooother jako Ruby, Perl, PHP nebo node.js, které jsou také předem nainstalovány.

Neexistují žádné poplatky softwaru pro tuto bitovou kopii dat vědecké účely virtuálních počítačů. Platíte jenom hello Azure hardwaru využití poplatků, které jsou hodnotí na základě velikosti hello hello virtuálního počítače, který budete zřizovat. Další informace o hello výpočetní poplatky lze najít v hello [stránky Seznam virtuálních počítačů na hello Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-hello-data-science-virtual-machine"></a>Jiné verze hello datové vědy virtuálního počítače
A [CentOS](machine-learning-data-science-linux-dsvm-intro.md) obrázek k dispozici, s mnoha hello stejné nástroje jako hello Ubuntu image. A [Windows](machine-learning-data-science-provision-vm.md) bitové kopie je také k dispozici.

## <a name="prerequisites"></a>Požadavky
Před vytvořením datové vědy virtuálního počítače pro Linux, musí mít předplatné Azure. tooobtain, najdete v části [získání bezplatné zkušební verze Azure](https://azure.microsoft.com/free/).

## <a name="create-your-data-science-virtual-machine-for-linux"></a>Vytvoření virtuálního počítače vědecké účely Data pro Linux
Zde jsou kroky toocreate hello instanci hello datové vědy virtuálního počítače pro Linux:

1. Přejděte na hello výpis toohello virtuálního počítače [portál Azure](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vm-ubuntulinuxdsvmubuntu).
2. Klikněte na tlačítko **vytvořit** (v dolním hello) toobring hello průvodce.![ Konfigurace data-vědecké účely vm](./media/machine-learning-data-science-dsvm-ubuntu-intro/configure-data-science-virtual-machine.png)
3. Hello následující oddíly poskytují hello vstupy pro každou hello kroků v Průvodci hello (uvedené na hello napravo od hello předcházející obrázek) používá toocreate hello Microsoft Data vědecké účely virtuálního počítače. Zde jsou hello vstupy potřeby tooconfigure každý z těchto kroků:
   
   a. **Základy**:
   
   * **Název**: název vašeho serveru vědecké účely data vytváříte.
   * **Uživatelské jméno**: první účet přihlásit ID.
   * **Heslo**: první heslo účtu (veřejný klíč SSH můžete použít místo hesla).
   * **Předplatné**: Pokud máte více než jedno předplatné, vyberte hello jednou na které hello počítače je toobe vytvořen a účtují. Musíte mít oprávnění vytvoření prostředku pro toto předplatné.
   * **Skupina prostředků**: můžete vytvořit novou nebo použít stávající skupinu.
   * **Umístění**: Vyberte hello datového centra, která je nejvhodnější. Obvykle je hello datového centra, který má většina vašich dat, nebo je nejbližší fyzické umístění tooyour pro nejrychlejší přístup k síti.
   
   b. **Velikost**:
   
   * Vyberte jeden z typů hello serveru, které splňuje požadavek na funkční a náklady na omezení. Vyberte **Zobrazit vše** toosee další možnosti velikostí virtuálních počítačů. Vyberte virtuální počítač NC třídy pro GPU školení.
   
   c. **Nastavení**:
   
   * **Typ disku**: Zvolte **Premium** Pokud dáváte přednost jednotky SSD (SSD). Jinak, vyberte **standardní**. Virtuální grafický procesor počítače vyžadují standardní disku.
   * **Účet úložiště**: můžete vytvořit nový účet úložiště Azure v rámci vašeho předplatného, nebo použijte existující hello stejné umístění, které jste vybrali na hello **Základy** kroku průvodce hello.
   * **Další parametry**: ve většině případů stačí použít hello výchozí hodnoty. tooconsider jiné než výchozí hodnoty, hover přes odkaz informační hello nápovědy na konkrétních polí hello.
   
   d. **Souhrn**:
   
   * Ověřte, že všechny informace, které jste zadali správné.
   
   e. **Kupte si**:
   
   * toostart hello zřizování, klikněte na tlačítko **koupit**. Podmínky toohello hello transakce je k dispozici odkaz. Hello virtuální počítač nemá jakýchkoli dalších poplatků nad rámec hello výpočetní pro velikost server hello jste zvolili v hello **velikost** krok.

zřizování Hello zabere asi 5 až 10 minut. Hello Stav zřizování hello se zobrazí na portálu Azure hello.

## <a name="how-tooaccess-hello-data-science-virtual-machine-for-linux"></a>Jak tooaccess hello datové vědy virtuálního počítače pro Linux
Po hello, které vytvoří virtuální počítač je tooit přihlásit pomocí protokolu SSH. Pomocí přihlašovacích údajů účtu hello, které jste vytvořili v hello **Základy** části kroku 3 pro rozhraní prostředí textu hello. V systému Windows, si můžete stáhnout nástroj pro klienta na SSH jako [Putty](http://www.putty.org). Pokud dáváte přednost grafické desktop (systém Windows X), můžete použít X11 předávání na Putty nebo instalace klienta X2Go hello.

> [!NOTE]
> Hello X2Go klienta provést výrazně lepší, než X11 předávání v testování. Doporučujeme používat hello X2Go klienta pro grafické rozhraní plochy.
> 
> 

## <a name="installing-and-configuring-x2go-client"></a>Instalace a konfigurace klienta X2Go
Hello virtuálního počítače s Linuxem je již zřízeno s X2Go serveru připravené tooaccept připojení a klientů. tooconnect toohello grafické ploše virtuálního počítače s Linuxem hello následující na vašeho klienta:

1. Stáhněte a nainstalujte hello X2Go klienta pro vaši platformu klienta z [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Spusťte hello X2Go klienta a vyberte **novou relaci**. Otevře okno Konfigurace s několik karet. Zadejte hello následující konfigurační parametry:
   * **Karta relace**:
     * **Hostitele**: hello název hostitele nebo IP adresy virtuálním počítačům s Linuxem dat vědecké účely.
     * **Přihlášení**: uživatelské jméno v hello virtuálního počítače s Linuxem.
     * **SSH Port**: nechat v 22, výchozí hodnota hello.
     * **Typ relace**: Změna hello hodnotu tooXFCE. Virtuální počítač s Linuxem hello aktuálně podporuje jenom XFCE plochy.
   * **Karta média**: můžete vypnout zvukové podporu a klienta, tisk, pokud nepotřebujete toouse je.
   * **Sdílené složky**: Pokud chcete adresáře z vaší klientské počítače připojené na hello virtuálního počítače s Linuxem, hello klientské počítače adresáře, které chcete tooshare s hello virtuálních počítačů na této kartě přidat.

Po přihlášení toohello virtuálních počítačů pomocí klienta SSH hello nebo XFCE grafické plochy prostřednictvím klienta X2Go hello jste připravené toostart pomocí hello nástroje, které jsou nainstalováno a nakonfigurováno na hello virtuálních počítačů. Na XFCE můžete zobrazit zástupce aplikace nabídky a ikony na ploše pro řadu nástrojů hello.

## <a name="tools-installed-on-hello-data-science-virtual-machine-for-linux"></a>Nástroje nainstalované na hello datové vědy virtuálního počítače pro Linux
### <a name="deep-learning-libraries"></a>Hloubkové Learning knihovny

#### <a name="cntk"></a>CNTK
Hello Microsoft kognitivní Toolki - CNTK - je open source, hloubkové učení toolkit. Python vazby jsou k dispozici v prostředích Conda kořenové a py35 hello. Je také nástroj příkazového řádku (cntk), který je již v hello CESTU.

Ukázka Python poznámkových bloků jsou k dispozici v JupyterHub. toorun základní ukázka hello příkazového řádku, spusťte následující příkazy v prostředí hello hello:

    cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
    cntk configFile=lr_bs.cntk makeMode=false command=Train

Další informace najdete v tématu hello části CNTK [Githubu](https://github.com/Microsoft/CNTK)a hello [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).

#### <a name="caffe"></a>Caffe
Caffe je hluboká učení framework z hello Berkeley vize a výukové centrum. Je k dispozici v /opt/caffe. Příklady najdete v /opt/caffe/examples.

#### <a name="caffe2"></a>Caffe2
Caffe2 je architektura hloubkové learning ze sítě Facebook, která je založená na Caffe. Je k dispozici v Python 2.7 v hello Conda kořenové prostředí. tooactivate ji spustit z prostředí hello hello následující:

    source /anaconda/bin/activate root

Některé poznámkových bloků příkladu jsou k dispozici v JupyterHub.

#### <a name="h2o"></a>H2O
H2O je rychlé, v paměti, distribuované strojového učení a platformy prediktivní analýzy. Balíček Python je nainstalována v obou prostředích hello kořenové a py35 Anaconda. Balíček R je také nainstalován. toostart H2O z příkazového řádku hello spustit `java -jar /dsvm/tools/h2o/current/h2o.jar`; existují různé [možnosti příkazového řádku](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/starting-h2o.html#from-the-command-line) , že chcete tooconfigure. Hello toku webové uživatelské rozhraní je přístupná procházením toohttp://localhost:54321 tooget spuštěna. Ukázka poznámkových bloků jsou také k dispozici v JupyterHub.

#### <a name="keras"></a>Keras
Keras je nejdůležitější neuronové sítě rozhraní API v Python, která umožňuje spustit v horní části Tensorflow nebo Theano. Je k dispozici v prostředí Python kořenové a py35 hello. 

#### <a name="mxnet"></a>MXNet
MXNet je hloubkové learning rozhraní určené pro efektivitu a flexibilitu. Má R a Python vazby, které jsou zahrnuty v hello DSVM. Ukázka poznámkových bloků jsou součástí JupyterHub a ukázkový kód je k dispozici v /dsvm/samples/mxnet.

#### <a name="nvidia-digits"></a>NVIDIA ČÍSLIC
Hello NVIDIA hloubkové učení GPU školení systém označuje jako ČÍSLIC, je systému toosimplify běžné hloubkové learning úkoly, jako správy dat, navrhování a cvičení neuronové sítě v systémech GPU a sledování výkonu v reálném čase s pokročilé vizualizace. 

ČÍSLIC je k dispozici jako služba, názvem číslic. Spusťte službu hello a procházet toohttp://localhost:5000 tooget spuštěna.

ČÍSLIC je také nainstalován jako modul Python v hello Conda kořenové prostředí.

#### <a name="tensorflow"></a>TensorFlow
TensorFlow je knihovna hloubkové learning Google. Otevřeným zdrojem softwarová knihovna pro číselné výpočty pomocí grafů toku dat je. Je k dispozici v prostředí Python py35 hello TensorFlow a některé ukázkové poznámkových bloků jsou součástí JupyterHub.

#### <a name="theano"></a>Theano
Theano je knihovna Python pro efektivní číselné výpočty. Je k dispozici v prostředí Python kořenové a py35 hello. 

#### <a name="torch"></a>Torch
Svítilnou je scientific výpočetní architektura s wide Podpora algoritmů strojového učení. Je k dispozici v /dsvm/tools/torch a jsou k dispozici v příkazovém řádku hello hello tý interaktivní relace a luarocks Správce balíčků. Příklady jsou k dispozici v /dsvm/samples/torch.

PyTorch je také dostupná v prostředí Anaconda kořenové hello. Příklady jsou /dsvm/samples/pytorch.

### <a name="microsoft-r-server"></a>Server Microsoft R
R je jedním z hello Nejoblíbenější jazyky pro analýzu dat a strojové učení. Pokud chcete pro analytické údaje toouse R, hello virtuálních počítačů má Microsoft R Server (Paní) s hello Microsoft R otevřete (MRO) a matematické jádra knihovny (MKL). Hello MKL optimalizuje matematické operace v analytical algoritmy běžné. MRO je kompatibilní s CRAN r. 100 procent a některé z knihovny hello R publikované v CRAN se dá nainstalovat na hello MRO. PANÍ vám dává škálování a operationalization R modelů do webové služby. Můžete upravit programy R v jednom z výchozí editory hello, jako je Rstudia, vi nebo Emacs. Pokud používáte hello Emacs editor, Všimněte si, že balíček Emacs hello ESS (mluví statistiky Emacs), což zjednodušuje práce se soubory R v rámci hello Emacs editor, předem nainstalován.

toolaunch R konzoly, stačí zadat **R** v prostředí hello. Tím přejdete tooan interaktivní prostředí. toodevelop váš program R, obvykle pomocí editoru jako Emacs nebo vi a potom spusťte hello skripty v rámci R. S Rstudia máte plný grafické toodevelop prostředí IDE váš R program.

Je také skript jazyka R pro tooinstall hello [balíčky R 20 horní](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) podle potřeby. Tento skript můžete spustit, jakmile se v interaktivní rozhraní hello R, kterou lze zadat (jak je uvedeno) tak, že zadáte **R** v prostředí hello.  

### <a name="python"></a>Python
Pro vývoj pomocí Python byl nainstalován distribuce Anaconda Python 2.7 a 3.5. Toto rozdělení obsahuje hello základní Python společně s přibližně 300 hello nejoblíbenější matematické, technici a data balíčků analytics. Můžete použít hello výchozí textové editory. Kromě toho můžete Spyder IDE Python, který je instalován s distribucí Anaconda Python. Spyder musí grafické plocha nebo X11 předávání. Zástupce tooSpyder je součástí grafické plochy hello.

Vzhledem k tomu, že máme Python 2.7 a 3.5, je nutné toospecifically aktivovat požadovaného hello Python verzi (conda prostředí) chcete toowork na v hello aktuální relaci. proces aktivace Hello nastaví hello cesta proměnné toohello požadované verzi Pythonu.

tooactivate hello Python 2.7 conda prostředí, spusťte z prostředí hello hello následující:

    source /anaconda/bin/activate root

Python 2.7 je nainstalována v */anaconda/bin*.

tooactivate hello Python 3.5 conda prostředí, spusťte z prostředí hello hello následující:

    source /anaconda/bin/activate py35


Python 3.5 je nainstalována v */anaconda/envs/py35/bin*.

tooinvoke interaktivní relaci Python, stačí zadat **python** v prostředí hello. Pokud jsou na grafické rozhraní nebo mají X11 předávání sadu nahoru, můžete zadat **pycharm** toolaunch hello PyCharm Python IDE.

tooinstall další knihovny Python, budete potřebovat toorun ```conda``` nebo ````pip```` příkazu v části sudo a zadejte úplnou cestu Správce balíčků Python hello (conda nebo pip) tooinstall toohello správné prostředí Python. Například:

    sudo /anaconda/bin/pip install <package> #for Python 2.7 environment
    sudo /anaconda/envs/py35/bin/pip install <package> # for Python 3.5 environment


### <a name="jupyter-notebook"></a>Poznámkový blok Jupyter
Hello Anaconda distribuční také obsahuje poznámkového bloku Jupyter, kód tooshare prostředí a analýzy. Poznámkový blok Jupyter Hello je přístupné přes JupyterHub. Přihlášení pomocí místní Linux uživatelské jméno a heslo.

server poznámkového bloku Jupyter Hello předem nakonfigurovaný s Python 2, Python 3 a R jádra. Není ikony na ploše s názvem "Poznámkový blok Jupyter" toolaunch hello prohlížeče tooaccess hello poznámkového bloku serveru. Pokud jste na hello virtuálních počítačů pomocí protokolu SSH nebo X2Go klienta, můžete také navštívit [https://localhost:8000 /](https://localhost:8000/) tooaccess hello serveru poznámkového bloku Jupyter.

> [!NOTE]
> Pokračujte, pokud chcete získat všechna upozornění certifikátu.
> 
> 

Server poznámkového bloku Jupyter hello můžete přistupovat z libovolného hostitele. Stačí zadat *https://\<název DNS virtuálního počítače nebo IP adresu\>: 8000 /*

> [!NOTE]
> Port 8000 je otevřen v bráně firewall hello ve výchozím nastavení při zřízení hello virtuálních počítačů.
> 
> 

Budeme mít zabalené poznámkových bloků ukázka – jednu v Pythonu a jeden v jazyce R. Uvidíte ukázky toohello hello odkaz na domovskou stránku hello Poznámkový blok, po ověření toohello Poznámkový blok Jupyter s použitím místní Linux uživatelské jméno a heslo. Můžete vytvořit nový poznámkový blok výběrem **nový**a pak jádra hello příslušný jazyk. Pokud nevidíte hello **nový** tlačítko, klikněte na tlačítko hello **Jupyter** ikonu na hello nejvyšší levém toogo toohello domovskou stránku hello serveru poznámkového bloku.

### <a name="apache-spark-standalone"></a>Apache Spark samostatné 
Samostatnou instanci systému Apache Spark je předinstalován v hello Linux DSVM toohelp nejprve vývoji aplikací Spark místně před testování a nasazení na velkých clusterech. PySpark programy můžete spustit prostřednictvím hello Jupyter jádra. Když otevřete Jupyter a klikněte na tlačítko "New" hello zobrazí se seznam dostupných jádra. Hello "Spark – Python" je hello jádra PySpark, které vám umožní vytvářet Spark aplikace pomocí jazyka Python. Můžete také použít Python IDE jako PyCharm nebo Spyder toobuild jste Spark programu. Od to je samostatné instance, zásobník Spark hello běží v rámci hello volání program klienta. To umožňuje rychlejší a snazší problémy tootroubleshoot porovná toodeveloping na clusteru Spark. 

Ukázkový PySpark Poznámkový blok je k dispozici na Jupyter, které můžete najít v adresáři "SparkML" hello pod hello domovskému adresáři Jupyter ($ DOMOVSKÉ/poznámkových bloků/SparkML/pySpark). 

Pokud programujete v R pro Spark, můžete použít Microsoft R Server SparkR nebo sparklyr. 

Před spuštěním v kontextu Spark Microsoft R Server, musíte toodo jeden čas instalace krok tooenable místní jednoho uzlu Hadoop HDFS a Yarn instance. Ve výchozím nastavení jsou služby Hadoop nainstalován, ale na hello DSVM zakázáno. Pořadí tooenable, je třeba toorun hello jako kořenové hello poprvé následující příkazy:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

Můžete zastavit hello Hadoop související služby, když je nepotřebujete spuštěním ````systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```` ukázku ukázka, jak paní toodevelop a testování v vzdálené kontextu Spark (což je hello samostatnou instanci Spark na hello DSVM) je zadaná a k dispozici v hello `/dsvm/samples/MRS` adresáře. 

### <a name="ides-and-editors"></a>Editory a integrovaného vývojového prostředí
Musíte vybrat několik editory kódu. To zahrnuje vi/VIM, Emacs, PyCharm, Rstudia a IntelliJ. IntelliJ, Rstudia a PyCharm jsou grafické editory a budete potřebovat toobe přihlášení tooa grafické plochy toouse je. Tyto editory obsahují desktopových a aplikačních nabídky zástupců toolaunch je.

**VIM** a **Emacs** jsou textové editory. Do Emacs jsme nainstalovali balíček rozšíření názvem Emacs mluví statistiky (ESS), který usnadňuje práci s R v rámci hello Emacs editor. Další informace najdete na [ESS](http://ess.r-project.org/).

**Latexu** je nainstalován prostřednictvím hello texlive balíčku společně s doplněk Emacs [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) balíček, který zjednodušuje vytváření latexu dokumentů v rámci Emacs.  

### <a name="databases"></a>Databáze

#### <a name="graphical-sql-client"></a>Grafický klient SQL
**SQuirrel SQL**, grafický klient SQL, bylo zadáno tooconnect toodifferent databáze (například Microsoft SQL Server a MySQL) a toorun dotazy SQL. Když spustíte z grafického relace plochy (například pomocí hello X2Go klienta). tooinvoke SQuirrel SQL, můžete spustit z hello ikony na ploše hello nebo spusťte následující příkaz na prostředí hello hello.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Před hello první použití, nastavení ovladače a aliasy databáze. Hello JDBC ovladače jsou umístěné na adrese:

*/USR/share/Java/jdbcdrivers*

Další informace najdete v tématu [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Nástroje příkazového řádku pro přístup k systému Microsoft SQL Server
Hello balíček ovladače ODBC pro SQL Server také obsahuje dva nástroje příkazového řádku:

**BCP**: hromadné nástroj bcp hello zkopíruje data mezi instance systému Microsoft SQL Server a datový soubor ve formátu zadaného uživatelem. Nástroj bcp Hello se dá použít tooimport velký počet nových řádků do tabulky serveru SQL Server nebo tooexport data z tabulek do datových souborů. tooimport data do tabulky, musíte buď použijte soubor ve formátu vytvořené pro tuto tabulku, nebo pochopit strukturu hello hello tabulky a hello typy dat, které jsou platné pro její sloupce.

Další informace najdete v tématu [připojení pomocí bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**SqlCmd**: můžete zadat příkazy jazyka Transact-SQL s Nástroj sqlcmd hello, také s postupy systému a soubory na příkazovém řádku hello skriptů. Tento nástroj používá dávek ODBC tooexecute Transact-SQL.

Další informace najdete v tématu [připojení pomocí sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Existují určité rozdíly v tento nástroj mezi platformy Linux a Windows. Podrobnosti naleznete v dokumentaci hello.
> 
> 

#### <a name="database-access-libraries"></a>Databáze access knihovny
Nejsou k dispozici v R a Python tooaccess databáze knihovny.

* V R, hello **RODBC** balíček nebo **dplyr** balíček vám umožní tooquery nebo spouštění příkazů jazyka SQL na serveru databáze hello.
* V Python, hello **pyodbc** knihovna poskytuje přístup k databázi s rozhraním ODBC hello základní vrstvě.  

### <a name="azure-tools"></a>Nástroje Azure
Hello následující nástroje Azure jsou nainstalovány na hello virtuálních počítačů:

* **Rozhraní příkazového řádku Azure**: hello rozhraní příkazového řádku Azure vám umožní toocreate a správu prostředků Azure pomocí příkazů prostředí. tooinvoke hello nástroje Azure, stačí zadat **azure nápovědy**. Další informace najdete v tématu hello [stránce dokumentace Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Microsoft Azure Storage Explorer**: Microsoft Azure Storage Explorer je grafický nástroj, který je použité toobrowse prostřednictvím hello objekty, které jsou uloženy v účtu úložiště Azure a tooupload a stahování tooand dat z Azure BLOB. Storage Explorer můžete přistupovat z hello zástupce na ploše ikona. Můžete ji použít z řádku prostředí zadáním **StorageExplorer**. Třeba toobe přihlášení z klienta X2Go nebo mít X11 předávání sadu nahoru.
* **Knihovny Azure**: hello Toto jsou některé hello předem nainstalovaná knihoven.
  
  * **Python**: hello souvisejících s Azure knihovny v Pythonu, zda jsou nainstalovány jsou **azure**, **azureml**, **pydocumentdb v tuto**, a **pyodbc** . S hello první tři knihovny, můžete přístup služby Azure storage, Azure Machine Learning a Azure DB Cosmos (databáze NoSQL v Azure). čtvrtý knihovna Hello pyodbc (spolu s hello ovladač Microsoft ODBC pro SQL Server), umožňuje přístup tooSQL serveru Azure SQL Database a Azure SQL Data Warehouse z Pythonu pomocí rozhraní ODBC. Zadejte **pip seznamu** toosee všechny hello uvedené knihovny. Zda toorun být tento příkaz v obou hello Python 2.7 a 3.5 prostředí.
  * **R**: hello souvisejících s Azure knihoven v R nainstalované **AzureML** a **RODBC**.
  * **Java**: hello seznam knihoven Azure Java lze nalézt v adresáři hello **/dsvm/sdk/AzureSDKJava** na hello virtuálních počítačů. Hello klíče knihovny jsou Azure úložiště a správu rozhraní API, databáze Cosmos Azure a JDBC ovladače systému SQL Server.  

Dostanete hello [portál Azure](https://portal.azure.com) z prohlížeče Firefox předem nainstalovaná hello. Na hello portálu Azure můžete vytvořit, spravovat a monitorovat prostředků Azure.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning je plně spravovaná Cloudová služba, která vám umožní toobuild, nasazení a sdílet řešení prediktivní analýzy. Sestavit experimentů a modely z Azure Machine Learning Studio. Byla přístupná z webového prohlížeče na hello datové vědy virtuálním počítači navštivte stránky [Microsoft Azure Machine Learning](https://studio.azureml.net).

Po přihlášení tooAzure Machine Learning Studio, máte přístup tooan experimentování plátno, kde můžete vytvořit logický tok pro hello algoritmů strojového učení. Také jste poznámkový blok Jupyter tooa přístup hostované v Azure Machine Learning a může bezproblémově pracovat hello experimentů v nástroji Machine Learning Studio. Zprovoznit hello strojového učení modely, které jste vytvořili pomocí zabalení je v rozhraní webových služeb. To umožňuje klientům napsané v jakékoli jazyk tooinvoke predikcím ze hello modelů strojového učení. Další informace najdete v tématu hello [Machine Learning dokumentaci](https://azure.microsoft.com/documentation/services/machine-learning/).

Můžete také vytvořit modely v R nebo Python na hello virtuálních počítačů a poté ji nasadit v provozním prostředí v Azure Machine Learning. Jsme nainstalovali knihovny v R (**AzureML**) a Python (**azureml**) tooenable tuto funkci.

Informace o tom, jak toodeploy modelů v R a Python do Azure Machine Learning najdete v tématu [deset způsobů, jak na hello vědecké zpracování dat virtuálního počítače](machine-learning-data-science-vm-do-ten-things.md) (zejména část hello "vytvářet modely pomocí R nebo Python a zprovoznit je s použitím Azure Machine Learning").

> [!NOTE]
> Tyto pokyny byly napsány pro Windows hello verzi hello vědecké účely dat virtuálních počítačů. Ale hello informace existuje na nasazení tooAzure modely Machine Learning je použít toohello virtuálního počítače s Linuxem.
> 
> 

### <a name="machine-learning-tools"></a>Nástroje Machine learning
Hello virtuální počítač obsahuje několik strojového učení nástroje a algoritmy, které byly předem zkompilovat a předem nainstalovány místně. Mezi ně patří:

* **K dispozici Vowpal**: algoritmus rychlého online učení.
* **xgboost**: nástroj, který poskytuje optimalizovanou, boosted stromu algoritmy.
* **Rattle**: na základě R grafický nástroj pro zkoumání dat snadno a modelování.
* **Python**: Anaconda Python se dodává s algoritmy strojového učení s knihovnami jako další Scikit připojené. Můžete nainstalovat další knihovny pomocí hello `pip install` příkaz.
* **LightGBM**: Přechod rychlé, distribuované a vysoce výkonných zvyšovat skóre framework podle algoritmy stromu rozhodnutí.
* **R**: knihovnu bohaté funkce machine learning je k dispozici pro R. Některé hello knihoven, které jsou předem nainstalovány jsou lm, glm, randomForest, rpart. Další knihovny můžete nainstalovat spuštěním:
  
        install.packages(<lib name>)

Tady je nějaké další informace o hello první tři nástroje machine learning v seznamu hello.

#### <a name="vowpal-wabbit"></a>K dispozici Vowpal
K dispozici Vowpal je machine learning systému, který používá techniky, jako je online a hash, allreduce, snížení, learning2search, aktivní a interaktivní učení.

Nástroj hello toorun na velmi základní příklad hello následující:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Existují další, větší ukázky v tomto adresáři. Další informace o zobrazit najdete v tématu [v této části GitHub](https://github.com/JohnLangford/vowpal_wabbit)a hello [Vowpal k dispozici wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Toto je do knihovny, která je navržena a optimalizována pro algoritmy boosted (stromu). Hello cílem této knihovny je, že toopush hello výpočetní omezení počítače toohello extrémní tooprovide rozsáhlé stromové struktury zvyšovat skóre je škálovatelný, přenosných a přesná.

Je poskytována jako příkazového řádku a také R knihovny.

toouse v R této knihovny, můžete spustit relaci interaktivní R (tak, že zadáte **R** v prostředí hello) a zatížení hello knihovně.

Zde je jednoduchý příklad, který můžete spustit v R řádku:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

toorun hello xgboost příkazového řádku, tady jsou příkazy tooexecute hello v prostředí hello:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Zadaný adresář toohello dojde k zapsání .model – souboru. Informace o tomto příkladu ukázku najdete [na Githubu](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Další informace o xgboost najdete v tématu hello [stránky dokumentace, která xgboost](https://xgboost.readthedocs.org/en/latest/)a jeho [úložiště GitHub](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rattle
Rattle (hello **R** **A**nalytical **T**ukopis **T**o **L**vám **E** asily) používá zkoumání dat založených na grafickém uživatelském rozhraní a modelování. Uvede statistické visual souhrnů dat, transformace dat, který je možné snadno modelovat, sestavení dozorovaných i u nedozorovaných modely z hello dat, uvede graficky hello výkonu modelů a nastaví skóre nová data. Rovněž vygeneruje kód R, replikace hello operace v hello uživatelského rozhraní, které můžete spustit přímo v R nebo používají jako výchozí bod pro další analýzu.

toorun Rattle, musíte toobe v grafické přihlášení relaci plochy. V terminálu hello zadejte ```R``` tooenter hello R prostředí. Na příkazovém řádku hello R zadejte hello následující příkazy:

    library(rattle)
    rattle()

Nyní grafické rozhraní otevře sadu karet. Tady jsou hello úvodní kroky v toouse Rattle potřeby ukázkové počasí datové sady a vytvoření modelu. V některé z následujících kroků hello jsou výzvami tooautomatically instalaci a spouštění některé požadované balíčky R, které ještě nejsou v systému hello.

> [!NOTE]
> Pokud nemáte přístup tooinstall hello balíčku v adresáři systému hello (hello výchozí nastavení), může zobrazit výzva na své R konzoly okno tooinstall balíčky tooyour osobní knihovny. Odpověď *y* Pokud se zobrazí tyto výzvy.
> 
> 

1. Klikněte na tlačítko **Spustit**.
2. Zobrazí se dialogové okno se zobrazí, s dotazem, pokud se vám líbí toouse hello příklad počasí datové sady. Klikněte na tlačítko **Ano** tooload hello příklad.
3. Klikněte na tlačítko hello **modelu** kartě.
4. Klikněte na tlačítko **Execute** toobuild rozhodovací strom.
5. Klikněte na tlačítko **kreslení** toodisplay hello rozhodovací strom.
6. Klikněte na tlačítko hello **doménové struktury** přepínač a klikněte na tlačítko **Execute** toobuild náhodných doménové struktury.
7. Klikněte na tlačítko hello **Evaluate** kartě.
8. Klikněte na tlačítko hello **riziko** přepínač a klikněte na tlačítko **Execute** pozemků výkonu toodisplay dva riziko (kumulativní).
9. Klikněte na tlačítko hello **protokolu** kartě tooshow hello generovat kód R pro hello předcházející operace.
   (Z důvodu chyb tooa v aktuální verzi Rattle hello, budete potřebovat tooinsert  *#*  znak před *exportovat tento protokol...*  v textu hello hello protokolu.)
10. Klikněte na tlačítko hello **exportovat** tlačítko toosave hello R skript soubor s názvem *weather_script. R* toohello domovskou složku.

Můžete ukončit Rattle a R. Teď můžete upravit skript jazyka R hello generované nebo ho použít, protože je toorun ho kdykoli toorepeat vše, co bylo provedeno v rámci hello Rattle uživatelského rozhraní. Zejména pro začátečníky v R jde o snadný způsob tooquickly provést analýzy a strojového učení v jednoduchého grafického rozhraní, při automatické generování kódu v R toomodify a další.

## <a name="next-steps"></a>Další kroky
Zde je, jak můžete dál učení a zkoumání:

* Hello [vědecké zpracování dat na hello datové vědy virtuálního počítače pro Linux](machine-learning-data-science-linux-dsvm-walkthrough.md) návod ukazuje, jak tooperform několik běžných vědecké zpracování dat úlohy s hello Data vědecké účely virtuálního počítače s Linuxem zřízený sem. 
* Prozkoumejte hello různé vědě nástrojů data na hello vědecké zpracování dat virtuálního počítače tak, že zkusíte se hello nástrojů popsaných v tomto článku. Můžete také spouštět *dsvm. Další informace* na hello prostředí v rámci hello virtuálního počítače základní informace toomore úvod a ukazatele o hello nástroje nainstalované na hello virtuálních počítačů.  
* Zjistěte, jak toobuild začátku do konce analytická řešení systematičtěji pomocí hello [proces vědecké účely dat Team](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).
* Navštivte hello [Cortana Analytics Gallery](http://gallery.cortanaanalytics.com) pro machine learning a data analýzy ukázky tohoto použití hello Cortana Analytics Suite.

