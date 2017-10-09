---
title: aaaIntroduction tooR Server v Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toouse R Server na toocreate aplikace HDInsight pro analýzu velkých objemů dat."
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6dc21bf5-4429-435f-a0fb-eea856e0ea96
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: daf7b70a15748d66510a04da370f39c5f26eb4ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#<a name="introduction-toor-server-and-open-source-r-capabilities-on-hdinsight"></a>Úvod tooR serveru a možností R open source v HDInsight

Microsoft R Server je k dispozici jako možnost nasazení, při vytváření clusterů HDInsight v Azure. Tato nová funkce poskytuje datových vědců statistikami a R programátory s tooscalable přístup na vyžádání, distribuované metody analýz v HDInsight.

Může být clustery odpovídající velikost toohello projekty a úkoly v ručně a pak bylo odstraněno. Pokud jste už nepotřebují. Vzhledem k tomu, že jsou součástí Azure HDInsight, tyto clustery jsou součástí podnikové úrovni 24 hodin denně 7 podporu SLA 99,9 % doby provozu a hello možnost toointegrate s ostatními součástmi v hello ekosystému Azure.

R serverem v HDInsight obsahuje nejnovější funkce hello R na základě analýzy datových sad prakticky jakékoli velikosti, načíst tooeither úložiště objektů Blob v Azure nebo Data Lake. Vzhledem k tomu, že R Server je založený na R s otevřeným zdrojem, můžete využít hello R aplikacím, které vytvoříte, žádný z hello 8000 + R s otevřeným zdrojem balíčků. rutiny Hello v ScaleR, balíček analýzy velkých objemů dat společnosti Microsoft součástí R Server, jsou k dispozici.

Hello hraniční uzel clusteru poskytuje vhodné místo tooconnect toohello clusteru a toorun skripty R. Hraniční uzel máte hello možnost spuštěné hello paralelizovaná málo distribuované funkce ScaleR mezi hello jader hello hraniční uzel serveru. Můžete také spustit je mezi uzly hello hello clusteru pomocí Hadoop mapy snížit nebo Spark na ScaleR výpočetní kontexty.

modely Hello nebo předpovědi, které výsledek z analýzy se dá stáhnout pro pomocí místní. Se může také být operationalized jinde v Azure, konkrétně prostřednictvím [Azure Machine Learning Studio](http://studio.azureml.net) [webovou službu](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>Začínáme s R v HDInsight
tooinclude R Server v clusteru služby HDInsight, při vytváření clusteru HDInsight pomocí hello portál Azure je nutné vybrat typ clusteru R Server hello. Hello typ clusteru R Server zahrnuje R Server na hello datové uzly clusteru hello a na hraniční uzel, který slouží jako cílová zóny na základě R Server analýzy. V tématu [Začínáme s R serverem v HDInsight](hdinsight-hadoop-r-server-get-started.md) podrobný jak toocreate hello clusteru.

## <a name="learn-about-data-storage-options"></a>Další informace o možnosti ukládání dat
Výchozí úložiště pro clustery HDInsight systému souborů HDFS hello může být přidruženy k účtu Azure Storage nebo úložiště služby Azure Data Lake. Toto přidružení zajistí, že ať data jsou odeslána úložiště clusteru toohello během analýzy se provádí trvalé. Existují různé nástroje pro zpracování hello přenos toohello úložiště možnost dat, kterou vyberete, včetně zařízení založené na portálu nahrávání hello hello účet úložiště a hello [AzCopy](../storage/common/storage-use-azcopy.md) nástroj.

Máte možnost hello přidání přístupu tooadditional objektů Blob a Data lake úložiště během procesu bez ohledu na to možnost primárního úložiště hello používá zřizování clusteru hello. V tématu [Začínáme s R serverem v HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) informace o přidání tooadditional účty pro přístup. V tématu hello doplňující [možnosti úložiště Azure pro R Server v HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) článku toolearn Další informace o používání více účtů úložiště.

Můžete také použít [Azure Files](../storage/files/storage-how-to-use-files-linux.md) jako řešením úložiště pro použití v uzlu edge hello. Soubory Azure umožňuje toomount sdílené složky, která byla vytvořena v Azure Storage toohello souboru systému Linux. Další informace o těchto možnostech úložiště dat pro R Server v clusteru HDInsight najdete v tématu [Azure Storage možnosti pro R serverem v HDInsight clustery](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-hello-cluster"></a>Přístup k serveru R na clusteru hello
TooR Server na uzlu edge hello pomocí prohlížeče, jste zadali jste vybrali tooinclude Rstudia serveru během procesu zřizování hello se můžete připojit. Pokud jste nenainstalovali se při zřizování clusteru hello, můžete jej přidat později. Informace o instalaci serveru pro Rstudia po vytvoření clusteru s podporou najdete v tématu [instalace serveru Rstudia v clusterech HDInsight](hdinsight-hadoop-r-server-install-r-studio.md). Toohello R Server můžete také připojit pomocí protokolu SSH/PuTTY tooaccess hello R konzoly. 

## <a name="develop-and-run-r-scripts"></a>Vývoj a spouštět skripty R
Hello R skripty můžete vytvořit a spustit můžete použít některou z hello 8000 + R s otevřeným zdrojem balíčků v přidání toohello paralelizovaná málo a distribuovaných rutiny, která je k dispozici v knihovně ScaleR hello. Obecně platí skript, který běží na uzlu edge hello s R Server běží v rámci překladač hello R v tomto uzlu. Hello výjimky jsou tyto kroky, které je třeba toocall ScaleR funkce s kontextem výpočetní, který je nastavený tooHadoop mapy snižte (RxHadoopMR) nebo Spark (RxSpark). V takovém případě hello funkce běží v distribuované způsobem napříč dat (úlohy) uzlů clusteru hello, které jsou spojeny s daty hello odkazuje. Další informace o možnostech kontextu různými výpočetními hello najdete v tématu [výpočetní kontextu možnosti pro R Server v HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Zprovoznit model
Po dokončení modelování vaše data můžete zprovoznit předpovědi toomake hello model na nová data buď z Azure a místní. Tento proces se označuje jako vyhodnocování. Vyhodnocování lze provést v prostředí HDInsight, Azure Machine Learning nebo místně.

### <a name="score-in-hdinsight"></a>Skóre v HDInsight
tooscore v HDInsight, zapisovat R funkce, která volá váš model toomake předpovědi do nového datového souboru, že jste načíst tooyour účet úložiště. Potom uložte hello předpovědi back toohello účet úložiště. Hello rutiny na vyžádání můžete spustit na hello hraniční uzel clusteru nebo pomocí naplánované úlohy.  

### <a name="score-in-azure-machine-learning-aml"></a>Skóre v Azure strojového učení (AML)
tooscore pomocí AML webové služby, použijte hello otevřít zdrojový balíček Azure Machine Learning R známé jako [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) toopublish váš model jako Azure webové služby. Pro větší pohodlí si tento balíček je předem nainstalovaná na uzlu edge hello. V dalším kroku použít hello zařízení v Machine Learning toocreate uživatelské rozhraní pro hello webovou službu a pak zavolají hello webové služby podle potřeby pro vyhodnocování.

Pokud zvolíte tuto možnost, musíte tooconvert žádné ScaleR modelu objektů tooequivalent open-source objekty modelu pro použití s hello webové služby. Pomocí funkce Převod ScaleR, jako například `as.randomForest()` pro modely na základě komplet, pro tento převod.

### <a name="score-on-premises"></a>Skóre na místě
tooscore místní po vytvoření modelu, může serializovat hello model v R, stahování, deserializovat ho a následně použít pro výpočet skóre nová data. Vám může skóre pro nová data pomocí hello přístup popsané výše v [vyhodnocování v HDInsight](#scoring-in-hdinsight) nebo pomocí [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-hello-cluster"></a>Udržovat hello clusteru
### <a name="install-and-maintain-r-packages"></a>Instalaci a údržbě balíčky R
Většina hello R balíčky, které používáte je nutné hello hraniční uzel, od většinu kroků skripty R spustit existuje. tooinstall další balíčky R na hello hraniční uzel, můžete použít hello obvyklé `install.packages()` metody v jazyce R.

Pokud právě používáte rutiny z knihovny ScaleR hello napříč hello clusteru, není nutné obvykle tooinstall další balíčky R na hello datové uzly. Však může být zapotřebí další balíčky toosupport použití hello **rxExec** nebo **RxDataStep** provádění na hello datové uzly.

Po vytvoření clusteru hello, lze v těchto případech nainstalovat další balíčky hello pomocí akce skriptu. Další informace najdete v tématu [vytváření clusteru služby HDInsight s R Server](hdinsight-hadoop-r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Změňte nastavení paměti snížit Hadoop mapy
Cluster může být upravený toochange hello množství paměti, která je k dispozici tooR serveru při spuštění úlohy snížit mapy. toomodify cluster, použijte hello uživatelského rozhraní Apache Ambari, který je k dispozici prostřednictvím hello Azure okno portálu pro váš cluster. Pokyny o tom, jak tooaccess hello uživatelského rozhraní Ambari pro váš cluster najdete v tématu [clusterů HDInsight spravovat pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md).

Je také možné toochange hello množství paměti, která je k dispozici tooR serveru pomocí přepínačů Hadoop v hello volání příliš**RxHadoopMR** následujícím způsobem:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Škálování clusteru
Existující cluster můžete škálovat nahoru nebo dolů prostřednictvím portálu hello. Škálování, můžete získat hello dodatečnou kapacitu, které byste mohli potřebovat pro větší úlohy zpracování, nebo je možné škálovat zpět cluster nečinný. Pokyny najdete v části tooscale cluster s podporou [Správa clusterů HDInsight](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-hello-system"></a>Udržovat systém hello
Opravy operačního systému tooapply údržby a další aktualizace se provádí na základní virtuální počítače s Linuxem v clusteru služby HDInsight, která hello. Obvykle údržby se provádí na 3:30 AM (podle místního času hello hello virtuálních počítačů) každé pondělí a čtvrtek. Aktualizace se provádějí v tak, aby se nemáte vliv více než čtvrtletí hello clusteru současně.  

Vzhledem k tomu, že jsou redundantní hello head uzly a dopad na všechny uzly dat, všechny úlohy, které jsou spuštěné během této doby může zpomalit. Toocompletion, budou i nadále by měla spustit ale. Všechny vlastní software nebo místní data, zda máte se zachová napříč tyto události údržby, pokud dojde k závažné chybě, která vyžaduje nové vytvoření clusteru.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Další informace o možnosti IDE pro R Server v clusteru HDInsight
Hello Linux hraniční uzel clusteru HDInsight je cílová stránka zóny pro analýzu na základě R hello. Nejnovější verze služby HDInsight poskytují výchozí možnost pro instalaci verze komunity hello [Rstudia Server](https://www.rstudio.com/products/rstudio-server/) na uzlu edge hello jako IDE se založené na prohlížeči. Použití serveru Rstudia jako rozhraní IDE pro vývoj hello a spouštění skriptů R mohou být značně zvýšit produktivitu, než jen pomocí konzoly hello R. Pokud jste zvolili tooadd Rstudia Server při vytváření hello clusteru, ale byste chtěli tooadd ho později, pak najdete v části [instalace serveru R Studio na clustery HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-install-r-studio). +

Další možnost Úplná IDE je tooinstall plochy IDE a použít ho tooaccess hello clusteru prostřednictvím použití vzdáleného kontextu výpočetní mapy snížit nebo Spark. Mezi možnosti patří společnosti Microsoft [R Tools pro Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS), Rstudia a Walware adresy, na základě Eclipse [StatET](http://www.walware.de/goto/statet).

Nakonec získat přístup ke konzole R Server hello na uzlu edge hello zadáním **R** hello Linux příkazového řádku po připojení prostřednictvím protokolu SSH nebo PuTY. Pokud používáte rozhraní konzoly hello, je vhodné toorun textového editoru pro vývoj skriptu R v jiném okně a vyjmout a vložit části souboru skriptu do konzoly hello R podle potřeby.

## <a name="learn-about-pricing"></a>Další informace o cenách
Hello poplatků, které jsou přidruženy HDInsight, cluster s serveru R jsou strukturovaná podobně jako toohello poplatky za hello standardní clusterů HDInsight. Jsou založené na velikost hello hello základní virtuální počítače napříč hello název, data a uzly okraj, uveďte hello zdvih základní hodinu. Další informace o cenách prostředí HDInsight a dostupnost hello 30denní bezplatné zkušební verze najdete v tématu [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Další kroky
toolearn Další informace o tom, jak clusterů toouse R Server s HDInsight, najdete v části hello následující témata:

* [Začínáme s R serverem v HDInsight](hdinsight-hadoop-r-server-get-started.md)
* [Přidat Server Rstudia tooHDInsight (Pokud není nainstalován během vytváření clusteru)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Možnosti výpočetního kontextu pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Možnosti služby Azure Storage pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-storage.md)
