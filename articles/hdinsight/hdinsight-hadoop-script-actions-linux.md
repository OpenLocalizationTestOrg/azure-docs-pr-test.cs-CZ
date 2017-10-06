---
title: "vývoj akcí aaaScript s HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Bash skriptů toocustomize clustery HDInsight se systémem Linux. Hello funkce akce skriptu HDInsight umožňuje skripty toorun během nebo po vytvoření clusteru. Skripty můžete být použité toochange nastavení konfigurace clusteru nebo nainstalovat další software."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf4c89cd-f7da-4a10-857f-838004965d3e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 1f504b00365df5f4cfb3ae19ad55ff7630342650
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="script-action-development-with-hdinsight"></a>Vývoj akcí skriptů v prostředí HDInsight

Zjistěte, jak pomocí clusteru HDInsight Bash toocustomize skripty. Akce skriptů jsou způsob toocustomize HDInsight během nebo po vytvoření clusteru.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="what-are-script-actions"></a>Jaké jsou akce skriptu

Skript akce se skripty Bash, které Azure je spuštěná na změny konfigurace toomake uzly clusteru hello nebo instalovat software. Akce skriptu se spustí jako kořenového adresáře a poskytuje plný přístup uzly clusteru toohello práva.

Skript akce se dá použít prostřednictvím hello následující metody:

| Pomocí této metody tooapply skript... | Při vytvoření clusteru... | Na clusteru s podporou spuštěné... |
| --- |:---:|:---:|
| portál Azure |✓ |✓ |
| Azure PowerShell |✓ |✓ |
| Azure CLI |&nbsp; |✓ |
| Sada HDInsight .NET SDK |✓ |✓ |
| Šablona Azure Resource Manageru |✓ |&nbsp; |

Další informace o použití akce skriptu tooapply těchto metod najdete v části [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Osvědčené postupy pro vývoj skriptů

Při vývoji vlastních skriptů pro cluster služby HDInsight, existuje několik osvědčených postupů tookeep nezapomeňte:

* [Cílová verze Hadoop hello](#bPS1)
* [Cíl hello verze operačního systému](#bps10)
* [Zadejte, že stabilní odkazy tooscript prostředky](#bPS2)
* [Použití předem zkompilovat prostředky](#bPS4)
* [Zajistěte, aby hello clusteru přizpůsobení skriptu idempotent](#bPS3)
* [Ujistěte se, vysokou dostupnost clusteru architektury hello](#bPS5)
* [Konfigurace úložiště objektů Azure Blob toouse hello vlastní komponenty](#bPS6)
* [Zápis tooSTDOUT informace a STDERR](#bPS7)
* [Ukládat soubory ve formátu ASCII s LF konce řádků](#bps8)
* [Použití opakování logiku toorecover z přechodné chyby](#bps9)

> [!IMPORTANT]
> Akce skriptu musíte dokončit během 60 minut nebo hello selže. Při zřizování uzlu, hello skript spustí souběžně jiné procesy instalací a konfigurací. Soutěž o zdroje, jako je například šířky pásma procesoru čas nebo v síti může způsobit hello skriptu tootake delší toofinish než ve vašem vývojovém prostředí.

### <a name="bPS1"></a>Cílová verze Hadoop hello

Různé verze HDInsight mají různé verze služby Hadoop a nainstalovány součásti. Pokud váš skript očekává na konkrétní verzi služeb nebo součásti, měli používat jenom hello skriptu hello verzi HDInsight, které obsahuje hello požadované součásti. Informace můžete najít na verze součástí, které jsou součástí HDInsight pomocí hello [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.

### <a name="bps10"></a>Cílová verze hello operačního systému

HDInsight se systémem Linux je založena na hello distribuční Ubuntu Linux. Různé verze HDInsight využívají různé verze Ubuntu, což může změnit chování vašeho skriptu. Například vychází HDInsight 3.4 a starší verze Ubuntu, které používají Upstart. Ubuntu 16.04, který používá Systemd podle verze 3.5. Systemd a Upstart závisí na jiné příkazy, takže váš skript zasílány toowork s oběma.

Další důležité rozdíl mezi HDInsight 3.4 a 3.5 je, že `JAVA_HOME` nyní body tooJava 8.

Verze hello operačního systému můžete zkontrolovat pomocí `lsb_release`. Hello následující kód ukazuje, jak toodetermine Pokud hello skriptu běží na Ubuntu 14 nebo 16:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

Můžete najít hello úplné skript, který obsahuje tyto fragmenty kódu v https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

Hello verzi Ubuntu, který se používá v prostředí HDInsight, naleznete v části hello [verze komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.

toounderstand hello rozdíly mezi Systemd a Upstart, najdete v části [Systemd pro uživatele Upstart](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Zadejte, že stabilní odkazy tooscript prostředky

Hello skript a související prostředky musí zůstat k dispozici v rámci životnosti hello hello clusteru. Pokud Přidání nových uzlů clusteru toohello během operace škálování, je nutné tyto prostředky.

Hello osvědčeným postupem je toodownload a archivaci vše v účtu Azure Storage na vaše předplatné.

> [!IMPORTANT]
> účet úložiště Hello musí být hello výchozí účet úložiště pro kontejner veřejné, jen pro čtení nebo hello clusteru na jiný účet úložiště.

Například hello ukázky od společnosti Microsoft jsou uloženy v hello [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) účet úložiště. Toto je veřejný, jen pro čtení kontejner udržovat týmem HDInsight hello.

### <a name="bPS4"></a>Použití předem zkompilovat prostředky

tooreduce hello trvá toorun hello skriptu, když, vyhněte se operace, které zkompilovat prostředky ze zdrojového kódu. Například předem zkompilovat prostředky a uložit je do objektu blob účtu Azure Storage v hello stejném datovém centru jako HDInsight.

### <a name="bPS3"></a>Zajistěte, aby hello clusteru přizpůsobení skriptu idempotent

Skripty musí být idempotent. Pokud hello skript spustí vícekrát, by měla vrátit hello stejné stavu pokaždé, když toohello clusteru.

Například skript, který upravuje konfigurační soubory by neměly přidat duplicitní položky, pokud byl spuštěn vícekrát.

### <a name="bPS5"></a>Ujistěte se, vysokou dostupnost clusteru architektury hello

Linuxové clustery HDInsight poskytují dva head uzly, které jsou aktivní v rámci clusteru hello a spusťte skript akce v obou uzlech. Pokud součásti hello očekává pouze jeden hlavní uzel, neinstalujte součásti hello v obou uzlech head.

> [!IMPORTANT]
> Služby, které jsou k dispozici jako součást služby HDInsight jsou navrženou toofail přes mezi dvěma uzly head hello podle potřeby. Tato funkce není rozšířeno toocustom součásti nainstalované prostřednictvím akce skriptu. Pokud potřebujete vysokou dostupnost pro vlastní součásti, je nutné implementovat vlastní mechanismus převzetí služeb při selhání.

### <a name="bPS6"></a>Konfigurace úložiště objektů Azure Blob toouse hello vlastní komponenty

Součásti, které instalujete na clusteru hello může mít výchozí konfigurace, který používá Hadoop Distributed File System (HDFS) úložiště. HDInsight používá jako hello výchozí úložiště Azure Storage nebo Data Lake Store. Zadejte oba systém HDFS kompatibilní souborů, která je uchována data i v případě odstranění clusteru hello. Může být nutné tooconfigure součásti instalujete toouse WASB nebo ADL místo HDFS.

Pro většinu operací není nutné systém souborů toospecify hello. Například následující hello zkopíruje soubor giraph-examples.jar hello z úložiště toocluster systému hello místního souboru:

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

V tomto příkladu hello `hdfs` příkaz transparentně používá úložiště clusteru výchozí hello. Pro některé operace pravděpodobně bude třeba toospecify hello identifikátor URI. Například `adl:///example/jars` pro Data Lake Store nebo `wasb:///example/jars` pro Azure Storage.

### <a name="bPS7"></a>Zápis tooSTDOUT informace a STDERR

HDInsight zaznamená výstup skriptu, který je zapsaný tooSTDOUT a STDERR. Můžete zobrazit tyto informace pomocí webovému uživatelskému rozhraní Ambari hello.

> [!NOTE]
> Ambari je dostupné pouze při hello cluster se úspěšně vytvořil. Pokud použijete akci skriptu během vytváření clusteru a vytvoření selže, najdete v části řešení potíží v hello [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) pro další způsoby, jak přístup k informacím o protokolu.

Většina nástrojů a instalační balíčky již zapsat informace tooSTDOUT a STDERR, ale může být vhodné tooadd další protokolování. toosend text tooSTDOUT, použijte `echo`. Například:

```bash
echo "Getting ready tooinstall Foo"
```

Ve výchozím nastavení `echo` zasílá hello tooSTDOUT řetězec. toodirect ho tooSTDERR, přidejte `>&2` před `echo`. Například:

```bash
>&2 echo "An error occurred installing Foo"
```

To přesměruje informace místo zapsané tooSTDOUT tooSTDERR (2). Další informace o přesměrování vstupů/výstupů, najdete v části [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Další informace o zobrazení informací zaznamenaných akcí skriptů naleznete v tématu [clusterů HDInsight přizpůsobit pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

### <a name="bps8"></a>Ukládat soubory ve formátu ASCII s LF konce řádků

Skripty bash být uloženy ve formě formátu ASCII, se ukončila příkazem LF řádky. Soubory, které jsou uloženy jako UTF-8, nebo použijte Line FEED jako hello řádku ukončuje může selhat s hello následující chybě:

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <a name="bps9"></a>Použití opakování logiku toorecover z přechodné chyby

Při stahování souborů, instalace balíčků pomocí výstižný get nebo jiné akce, které přenášet data přes hello internet, hello akce se nemusí zdařit kvůli tootransient chyby sítě. Hello vzdálený prostředek, který komunikuje se může být například v procesu hello neúspěšného přes tooa zálohování uzlu.

toomake vaše odolné tootransient chyby skriptu, můžete implementovat logiku opakovaných pokusů. Následující funkce Hello ukazuje, jak logika opakovaných pokusů tooimplement. Se pokusí hello operaci třikrát než selže.

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

Hello následující příklady ukazují, jak toouse tuto funkci.

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <a name="helpermethods"></a>Pomocné metody pro vlastní skripty

Pomocné metody akcí skriptů jsou nástroje, které můžete použít při zápisu vlastních skriptů. Tyto metody jsou součástí[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) skriptu. Použijte následující toodownload hello a použít je jako součást vašeho skriptu:

```bash
# Import hello helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

Hello následující pomocné rutiny, které jsou k dispozici pro použití ve vašem skriptu:

| Použití pomocné rutiny | Popis |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |Stáhne soubor hello zdroj URI toohello zadaná cesta k souboru. Ve výchozím nastavení je nepřepíše existující soubor. |
| `untar_file TARFILE DESTDIR` |Extrahuje vkládání soubor (pomocí `-xf`) toohello cílový adresář. |
| `test_is_headnode` |Pokud byla spuštěna hlavního uzlu clusteru, návratový 1; jinak hodnota 0. |
| `test_is_datanode` |Pokud aktuální uzel hello je uzel dat (pracovník), vrátí 1; jinak hodnota 0. |
| `test_is_first_datanode` |Pokud je aktuální uzel hello hello první dat (pracovník) uzlu (s názvem workernode0) vrátí 1; jinak hodnota 0. |
| `get_headnodes` |Vrátí hello plně kvalifikovaný název domény hello headnodes v clusteru hello. Názvy jsou oddělený čárkami. Chyba se vrátí prázdný řetězec. |
| `get_primary_headnode` |Získá hello plně kvalifikovaný název domény primární headnode hello. Chyba se vrátí prázdný řetězec. |
| `get_secondary_headnode` |Získá hello plně kvalifikovaný název domény sekundární headnode hello. Chyba se vrátí prázdný řetězec. |
| `get_primary_headnode_number` |Získá číselnou příponu hello primární headnode hello. Chyba se vrátí prázdný řetězec. |
| `get_secondary_headnode_number` |Získá číselnou příponu hello sekundární headnode hello. Chyba se vrátí prázdný řetězec. |

## <a name="commonusage"></a>Obecné vzory využití

Tato část obsahuje pokyny k implementaci některé hello běžné využití vzory, které se mohou vyskytnout při zápisu vlastních skriptů.

### <a name="passing-parameters-tooa-script"></a>Předávání parametrů tooa skriptu

V některých případech může váš skript vyžadovat parametry. Například můžete potřebovat heslo správce hello k hello clusteru při použití hello Ambari REST API.

Parametry předané toohello skriptu se označují jako *poziční parametry*a jsou přiřazeny příliš`$1` prvního parametru hello `$2` pro hello druhé a proto v. `$0`obsahuje název hello samotný skript hello.

Hodnoty předány jako parametry toohello skriptu by se měla uvádět v jednoduchých uvozovkách ('). Tím zajistíte, že hello předaná hodnota je považován za literál.

### <a name="setting-environment-variables"></a>Nastavení proměnných prostředí

Nastavení proměnné prostředí provádí hello následující příkaz:

    VARIABLENAME=value

Kde NÁZEVPROMĚNNÉ je hello název proměnné hello. použití proměnné, hello tooaccess `$VARIABLENAME`. Například tooassign hodnotu poskytovanou infrastrukturou poziční parametr jako proměnnou prostředí s názvem heslo, využije hello následující příkaz:

    PASSWORD=$1

Pak použít informace o dalších access toohello `$PASSWORD`.

Proměnné prostředí nastavit v rámci skriptu hello existovat pouze v rámci oboru hello hello skriptu. V některých případech může být nutné tooadd systémové proměnné, které se uchová po dokončení skriptu hello. proměnné prostředí systémové tooadd, přidat proměnnou hello příliš`/etc/environment`. Například hello následující příkaz přidá `HADOOP_CONF_DIR`:

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-toolocations-where-hello-custom-scripts-are-stored"></a>Přístup k toolocations, kde jsou uloženy vlastní skripty hello

Skripty použité toocustomize cluster potřebuje toobe uložené v jednom z následujících umístění hello:

* __Účet úložiště Azure__ který je přidružen hello cluster.

* __Účtu další úložiště__ přidruženého k hello clusteru.

* A __veřejně čitelné URI__. Například adresu URL toodata uložené na OneDrive, Dropbox nebo jiný soubor, který je hostitelem služby.

* __Účtu Azure Data Lake Store__ který je přidružen hello HDInsight cluster. Další informace o používání Azure Data Lake Store s HDInsight naleznete v tématu [vytvoření clusteru HDInsight s Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    > [!NOTE]
    > Hello služby hlavní HDInsight používá tooaccess Data Lake Store, musí mít přístup pro čtení toohello skriptu.

Prostředky využívané třídou hello skriptu musí být také veřejně dostupné.

Ukládání souborů hello v účtu úložiště Azure nebo Azure Data Lake Store poskytuje rychlý přístup, jako i v rámci hello síť Azure.

> [!NOTE]
> Hello URI formátu, který používá tooreference hello skriptu se liší podle hello služba používá. Pro účty úložiště přidružené ke clusteru HDInsight hello, použijte `wasb://` nebo `wasbs://`. Veřejně čitelné identifikátory URI, použijte `http://` nebo `https://`. Pro Data Lake Store, použijte `adl://`.

### <a name="checking-hello-operating-system-version"></a>Kontrola, zda text hello verze operačního systému

Různé verze HDInsight spoléhají na určité verze Ubuntu. Mohou existovat rozdíly mezi verzemi operačního systému, které musí vyhledat ve vašem skriptu. Například může být nutné tooinstall binární soubor, který je vázané toohello verze Ubuntu.

verze toocheck hello operačního systému, použijte `lsb_release`. Například následující skript hello ukazuje, jak tooreference konkrétní vkládání souborů v závislosti na hello verze operačního systému:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

## <a name="deployScript"></a>Kontrolní seznam pro nasazení akce skriptu

Zde jsou hello kroky, které jsme trvalo při přípravě toodeploy tyto skripty:

* Uveďte hello soubory, které obsahují vlastní skripty hello na místě, které je přístupné uzly clusteru hello během nasazení. Například hello výchozí úložiště pro hello cluster. Soubory můžete také uloženy ve veřejně čitelné hostitelských služeb.
* Ověřte, zda text hello skriptu impotent. To uděláte toobe hello skript spustit několikrát na hello, stejný uzel.
* Použití dočasný soubor directory TMP tookeep hello stažené soubory používané hello skripty a pak vyčištění je po mají spouštět skripty.
* Pokud se změní nastavení na úrovni operačního systému nebo soubory konfigurace služby Hadoop, můžete chtít toorestart služby HDInsight.

## <a name="runScriptAction"></a>Jak toorun akce skriptu

Můžete použít skript akce toocustomize clusterů HDInsight pomocí hello následující metody:

* portál Azure
* Azure PowerShell
* Šablony Azure Resource Manageru
* Hello HDInsight .NET SDK.

Další informace o použití jednotlivých metod najdete v tématu [jak toouse skript akce](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Ukázky vlastních skriptů

Společnost Microsoft poskytuje ukázkové skripty tooinstall součásti v clusteru HDInsight. V tématu hello následující odkazy na další příklad akce skriptu.

* [Nainstalovat a používat Hue clustery prostředí HDInsight](hdinsight-hadoop-hue-linux.md)
* [Nainstalovat a používat Solr clustery prostředí HDInsight](hdinsight-hadoop-solr-install-linux.md)
* [Nainstalovat a používat Giraph clustery prostředí HDInsight](hdinsight-hadoop-giraph-install-linux.md)
* [Nainstalovat nebo upgradovat Mono na clustery HDInsight](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a>Řešení potíží

Hello následují chyb, které se můžete setkat při používání skripty, které jste si vytvořili:

**Chyba**: `$'\r': command not found`. Někdy následuje `syntax error: unexpected end of file`.

*Příčina*: k této chybě dojde, pokud hello řádků ve skriptu končit Line FEED. Systémy UNIX očekávat pouze LF jako hello ukončování řádků.

Tento problém nejčastěji nastane, když je skript hello vytvořené v prostředí Windows jako Line FEED je běžné řádku ukončení pro mnoho textové editory v systému Windows.

*Řešení*: Pokud je možnost v textovém editoru, vyberte formát operačního systému Unix nebo LF pro ukončování řádků hello. Můžete také použít následující příkazy na Unix systému toochange hello Line FEED tooan LF hello:

> [!NOTE]
> Hello následující příkazy jsou přibližně ekvivalentem v tom, že by se měl změnit tooLF konců řádku Line FEED hello. Vyberte jeden podle hello nástrojů dostupných v systému.

| Příkaz | Poznámky |
| --- | --- |
| `unix2dos -b INFILE` |původní soubor Hello je zálohovaný s. BAK rozšíření |
| `tr -d '\r' < INFILE > OUTFILE` |VÝSTUPNÍ_SOUBOR obsahuje verzi s pouze LF zakončení |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Upraví soubor hello přímo |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |VÝSTUPNÍ_SOUBOR obsahuje verzi s pouze LF zakončení. |

**Chyba**: `line 1: #!/usr/bin/env: No such file or directory`.

*Příčina*: Tato chyba nastane, když hello skriptu se uložil jako UTF-8 s značky pořadí bajtů (BOM).

*Řešení*: uložit hello soubor ve formátu ASCII nebo jako UTF-8 bez Kusovník. Můžete také použít následující příkaz na Linux nebo Unix systému toocreate soubor bez hello BOM hello:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Nahraďte `INFILE` souborem hello obsahující hello BOM. `OUTFILE`musí být nový název souboru, který obsahuje skript hello bez hello BOM.

## <a name="seeAlso"></a>Další kroky

* Zjistěte, jak příliš[clusterů HDInsight přizpůsobit pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md)
* Použití hello [referenční informace sady SDK rozhraní .NET HDInsight](https://msdn.microsoft.com/library/mt271028.aspx) toolearn Další informace o vytvoření aplikace .NET, které spravují HDInsight
* Použití hello [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) toolearn jak toouse REST tooperform akce správy v HDInsight clustery.
