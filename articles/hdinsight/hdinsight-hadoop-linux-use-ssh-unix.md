---
title: aaaUse SSH s Hadoop - Azure HDInsight | Microsoft Docs
description: "Pro přístup k systému HDInsight můžete použít Secure Shell (SSH). Tento dokument obsahuje informace o připojení z klientů Windows, Linux, Unix nebo systému macOS tooHDInsight pomocí hello ssh a scp příkazy."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "příkazy hadoop v linuxu, příkazy hadoop linux, hadoop macos, ssh hadoop, ssh hadoop cluster"
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: ac9e70ce3c70693c1b81c9514ba4fd47686070ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toohdinsight-hadoop-using-ssh"></a>Připojit tooHDInsight (Hadoop) pomocí protokolu SSH

Zjistěte, jak toouse [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) toosecurely připojit tooHadoop v Azure HDInsight. 

HDInsight můžete použít Linux (Ubuntu) jako hello operační systém pro uzly v clusteru Hadoop hello. Hello následující tabulka obsahuje hello adresu a port informace potřebné pro připojení na základě tooLinux HDInsight pomocí SSH klienta:

| Adresa | Port | Připojení... |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | 22 | Hraniční uzel (R Server v HDInsightu) |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | Hraniční uzel (všechny ostatní typy clusteru, pokud hraniční uzel existuje) |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Primární hlavní uzel |
| `<clustername>-ssh.azurehdinsight.net` | 23 | Sekundární hlavní uzel |

> [!NOTE]
> Nahraďte `<edgenodename>` s názvem hello hello hraniční uzel.
>
> Nahraďte `<clustername>` s hello názvem vašeho clusteru.
>
> Pokud cluster obsahuje hraniční uzel, doporučujeme vám __toohello hraniční uzel se vždy připojují__ pomocí protokolu SSH. Hello hlavních uzlech hostitel služby, které jsou kritické toohello stavu systému Hadoop. Hello hraniční uzel se spustí pouze co zadávat na něm.
>
> Další informace o použití hraničních uzlů najdete v tématu věnovaném [použití hraničních uzlů v HDInsightu](hdinsight-apps-use-edge-node.md#access-an-edge-node).

## <a name="ssh-clients"></a>Klienti SSH

Systémy Linux, Unix a systému macOS zadejte hello `ssh` a `scp` příkazy. Hello `ssh` klienta je běžně používané toocreate vzdálenou relaci příkazového řádku systému Unix nebo Linux. Hello `scp` klienta je použité toosecurely kopírovat soubory mezi klientem a hello vzdáleného systému.

Microsoft Windows ve výchozím nastavení neposkytuje žádné klienty SSH. Hello `ssh` a `scp` klienti jsou k dispozici pro systém Windows prostřednictvím hello následující balíčky:

* [Prostředí Azure Cloud](../cloud-shell/quickstart.md): hello cloudové prostředí poskytuje prostředí Bash ve vašem prohlížeči a poskytuje hello `ssh`, `scp`a další běžné příkazy Linux.

* [Bash na Ubuntu na Windows 10](https://msdn.microsoft.com/commandline/wsl/about): hello `ssh` a `scp` jsou k dispozici prostřednictvím na příkazovém řádku Windows hello Bash příkazy.

* [Git (https://git-scm.com/)](https://git-scm.com/): hello `ssh` a `scp` jsou k dispozici prostřednictvím příkazového řádku hello Git Bash příkazy.

* [Plocha GitHub (https://desktop.github.com/)](https://desktop.github.com/) hello `ssh` a `scp` příkazy jsou k dispozici prostřednictvím hello Githubu příkazového řádku. GitHub plochy může být nakonfigurované toouse Bash, hello příkazového řádku systému Windows nebo prostředí PowerShell jako hello příkazového řádku pro hello Git prostředí.

* [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): tým hello prostředí PowerShell je portování OpenSSH tooWindows a poskytuje zkušební verze.

    > [!WARNING]
    > Hello OpenSSH balíček obsahuje součást serveru hello SSH, `sshd`. Tato součást spustí serverem SSH v systému, jiné povolení tooconnect tooit. Tuto komponentu nakonfigurovat nebo otevřít port 22, pokud chcete, aby toohost serverem SSH v systému. Není požadováno toocommunicate s HDInsight.

Existuje také několik grafických klientů SSH, jako je [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) a [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/). Tito klienti mohou být použité tooconnect tooHDInsight, hello proces připojení se liší od používání hello `ssh` nástroj. Další informace najdete v tématu naleznete v dokumentaci hello hello grafický klient, který používáte.

## <a id="sshkey"></a>Ověřování: Klíče SSH

SSH klíče používat [Public key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) tooauthenticate relace SSH. Klíče SSH jsou bezpečnější než hesla a poskytnout clusteru Hadoop tooyour služby snadno toosecure přístup.

Váš účet SSH je zabezpečena pomocí klíče, musí klient hello poskytnout hello odpovídající privátní klíč při připojení:

* Většina klienti mohou být nakonfigurované toouse __výchozí klíč__. Například hello `ssh` hledá privátní klíč v `~/.ssh/id_rsa` v prostředích Linux a Unix.

* Můžete zadat hello __cesta tooa privátní klíč__. S hello `ssh` klienta, hello `-i` parametr je použité toospecify hello cesta tooprivate klíč. Například, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Pokud máte __více privátních klíčů__, které používáte na různých serverech, zvažte používání nástroje, jako je [SSH agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent). Hello `ssh-agent` nástroj může být použité tooautomatically vyberte hello klíče toouse při navazování relace SSH.

> [!IMPORTANT]
>
> Pokud se zabezpečením váš privátní klíč pomocí přístupového hesla, je nutné zadat heslo hello při použití klíče hello. Nástroje, jako `ssh-agent` hello hesla pro usnadnění vaší práce může do mezipaměti.

### <a name="create-an-ssh-key-pair"></a>Vytvoření páru klíčů SSH

Použití hello `ssh-keygen` příkaz toocreate soubory veřejného a privátního klíče. Hello následující příkaz generuje 2048 bitů pár klíčů RSA, který lze použít s HDInsight:

    ssh-keygen -t rsa -b 2048

Zobrazí se výzva informace během procesu vytvoření klíče hello. Například, kde jsou uloženy hello klíče nebo jestli toouse přístupové heslo. Po dokončení procesu hello jsou vytvořeny dva soubory; veřejný klíč a soukromý klíč.

* Hello __veřejný klíč__ je použité toocreate clusteru služby HDInsight. veřejný klíč Hello s příponou `.pub`.

* Hello __privátní klíč__ je použité tooauthenticate clusteru HDInsight toohello klienta.

> [!IMPORTANT]
> Klíče můžete zabezpečit pomocí přístupového hesla. Přístupové heslo je ve skutečnosti heslo k privátnímu klíči. I když někdo získá privátního klíče, musí mít hello přístupové heslo toouse hello klíč.

### <a name="create-hdinsight-using-hello-public-key"></a>Vytvoření HDInsight pomocí veřejného klíče hello

| Metoda vytvoření | Jak toouse hello veřejný klíč |
| ------- | ------- |
| **Azure Portal** | Zrušte zaškrtnutí políčka __použijte stejné heslo jako přihlašovací údaje clusteru__a potom vyberte __veřejný klíč__ jako hello typ ověřování SSH. Nakonec vyberte soubor veřejného klíče hello nebo vložte obsah textu hello hello souboru hello __veřejný klíč SSH__ pole.</br>![Dialogové okno Veřejný klíč SSH při vytváření clusteru HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| **Azure PowerShell** | Použití hello `-SshPublicKey` parametr hello `New-AzureRmHdinsightCluster` rutiny a předejte jí obsah hello hello veřejný klíč jako řetězec.|
| **Azure CLI 1.0** | Použití hello `--sshPublicKey` parametr hello `azure hdinsight cluster create` příkazů a předat hello obsah hello veřejný klíč jako řetězec. |
| **Šablona Resource Manageru** | Příklad použití klíčů SSH s využití šablony najdete v části věnované [nasazení HDInsightu v Linuxu pomocí klíče SSH](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/). Hello `publicKeys` element v hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) soubor je tooAzure klíče hello toopass použité při vytváření clusteru hello. |

## <a id="sshpassword"></a>Ověřování: Heslo

Účty SSH je možné zabezpečit pomocí hesla. Při připojení pomocí protokolu SSH tooHDInsight jste výzvami tooenter hello heslo.

> [!WARNING]
> Ověřování heslem pro SSH nedoporučujeme. Hesla můžete uhádnout a jsou snadno napadnutelný toobrute útokům. Místo toho doporučujeme využívat [klíče SSH k ověřování](#sshkey).

### <a name="create-hdinsight-using-a-password"></a>Vytvoření HDInsight s využitím hesla

| Metoda vytvoření | Jak toospecify hello heslo |
| --------------- | ---------------- |
| **Azure Portal** | Ve výchozím nastavení, hello SSH uživatelský účet má hello stejné heslo jako účet pro přihlášení hello clusteru. Zrušte zaškrtnutí políčka toouse jiné heslo, __použijte stejné heslo jako přihlašovací údaje clusteru__a pak zadejte heslo hello v hello __heslo SSH__ pole.</br>![Dialogové okno Heslo SSH při vytváření clusteru HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| **Azure PowerShell** | Použití hello `--SshCredential` parametr hello `New-AzureRmHdinsightCluster` rutiny a předejte `PSCredential` objekt, který obsahuje název hello SSH uživatelského účtu a heslo. |
| **Azure CLI 1.0** | Použití hello `--sshPassword` parametr hello `azure hdinsight cluster create` příkazů a zadejte hodnotu hello heslo. |
| **Šablona Resource Manageru** | Příklad použití hesla s využitím šablony najdete v části věnované [nasazení HDInsightu v Linuxu pomocí hesla SSH](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/). Hello `linuxOperatingSystemProfile` element v hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) soubor je použité toopass hello SSH účet jméno a heslo tooAzure při vytváření clusteru hello.|

### <a name="change-hello-ssh-password"></a>Změnit heslo SSH hello

Informace o změně hesla uživatelského účtu hello SSH naleznete v tématu hello __změnit hesla__ části hello [spravovat HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords) dokumentu.

## <a id="domainjoined"></a>Ověřování: HDInsight připojený k doméně

Pokud používáte __připojený k doméně clusteru HDInsight__, je nutné použít hello `kinit` příkaz po připojení pomocí protokolu SSH. Tento příkaz vás vyzve k zadání uživatele domény a heslo a ověřuje platnost vaší relace s doménou služby Active Directory Azure hello přidruženého k hello clusteru.

Další informace najdete v tématu [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).

## <a name="connect-toonodes"></a>Připojit toonodes

Hello hlavních uzlech a hraniční uzel (pokud existuje) je přístupná přes hello internet na portech 22 a 23.

* Při připojování toohello __hlavní uzly__, použití portu __22__ tooconnect toohello primární hlavního uzlu a port __23__ tooconnect toohello sekundárního hlavního uzlu. je technologie Hello toouse název plně kvalifikované domény `clustername-ssh.azurehdinsight.net`, kde `clustername` je hello názvem vašeho clusteru.

    ```bash
    # Connect tooprimary head node
    # port not specified since 22 is hello default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect toosecondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```
    
* Když connectiung toohello __hraniční uzel__, používat port 22. Hello plně kvalifikovaný název domény je `edgenodename.clustername-ssh.azurehdinsight.net`, kde `edgenodename` je název, který jste zadali při vytváření hello hraniční uzel. `clustername`je název hello hello clusteru.

    ```bash
    # Connect tooedge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]
> předchozí příklady Hello předpokládají, že používáte ověřování hesla nebo, certifikát ověření dochází automaticky. Pokud použijete pro ověřování SSH dvojici klíčů a certifikátů hello nepoužívá automaticky, použijte hello `-i` parametr toospecify hello privátní klíč. Například, `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.

Po připojení se mění hello řádku tooindicate hello SSH uživatelské jméno a hello uzlu, které jste připojeni k. Například při připojení toohello primární hlavního uzlu jako `sshuser`, výzva hello je `sshuser@hn0-clustername:~$`.

### <a name="connect-tooworker-and-zookeeper-nodes"></a>Připojit tooworker a uzly Zookeeper

Hello uzlů pracovního procesu a hello Zookeeper uzly nejsou přímo přístupné z Internetu. Lze k nim z hlavních uzlech clusteru hello nebo uzly okraj. Hello následují hello obecné kroky tooconnect tooother uzly:

1. Použijte uzel head nebo Microsoft edge tooconnect tooa SSH:

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. Hello head toohello připojení SSH nebo hraniční uzel, použije hello `ssh` příkaz tooconnect tooa pracovního uzlu v clusteru hello:

        ssh sshuser@wn0-myhdi

    tooretrieve seznam názvů domén hello hello uzlů v clusteru hello, najdete v části hello [hello spravovat HDInsight pomocí Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) dokumentu.

Pokud hello účtu SSH zabezpečené pomocí __heslo__, zadejte heslo hello, pokud se připojujete.

Pokud hello účtu SSH zabezpečené pomocí __klíče SSH__, ujistěte se, zda je povoleno předávání SSH na klientovi hello.

> [!NOTE]
> Dalším způsobem toodirectly přístup všechny uzly v clusteru hello je tooinstall HDInsight do Azure Virtual Network. Pak můžete připojit vaše toohello vzdáleném počítači stejný virtuální sítě a přímý přístup k všechny uzly v clusteru hello.
>
> Další informace najdete v tématu [Použití virtuální sítě s HDInsightem](hdinsight-extend-hadoop-virtual-network.md).

### <a name="configure-ssh-agent-forwarding"></a>Konfigurace přesměrování agenta SSH

> [!IMPORTANT]
> Hello následující kroky předpokládají systému UNIX nebo Linux a pracovat s Bash ve Windows 10. Pokud tyto kroky nefungují pro váš systém, může být nutné tooconsult hello dokumentace pro vašeho klienta SSH.

1. V textovém editoru otevřete `~/.ssh/config`. Pokud tento soubor neexistuje, můžete ho vytvořit tak, že na příkazovém řádku zadáte `touch ~/.ssh/config`.

2. Přidejte následující text toohello hello `config` souboru.

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    Nahraďte hello __hostitele__ informace s adresou hello hello uzlu připojíte toousing SSH. předchozí příklad Hello používá hello hraniční uzel. Tato položka se nakonfiguruje přesměrování agenta SSH pro hello určeného uzlu.

3. Test pomocí následujících příkazu z terminálu hello hello přesměrování agenta SSH:

        echo "$SSH_AUTH_SOCK"

    Tento příkaz vrátí informace podobné toohello následující text:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Když se nic nevrátí, znamená to, že nástroj `ssh-agent` není spuštěný. Další informace najdete v tématu hello agenta spuštění skriptů informace [Using ssh-agent s ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) nebo dokumentaci klienta SSH.

4. Jakmile si ověříte, **ssh-agent** je spuštěna, následující tooadd hello použití privátního klíče toohello agenta SSH:

        ssh-add ~/.ssh/id_rsa

    Pokud váš privátní klíč je uložený v jiném souboru, nahraďte `~/.ssh/id_rsa` s toohello hello cestě k souboru.

5. Připojte toohello clusteru hraniční uzel nebo hlavních uzlech pomocí protokolu SSH. Pak pomocí hello SSH příkaz tooconnect tooa worker nebo zookeeper uzlu. Hello připojení pomocí hello předávaných klíče.

## <a name="copy-files"></a>Kopírování souborů

Hello `scp` nástroj lze použít toocopy tooand soubory z jednotlivých uzlů v clusteru hello. Například hello následující příkaz kopie hello `test.txt` adresář z hello místní systém toohello primární hlavního uzlu:

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

Vzhledem k tomu, že není zadána žádná cesta po hello `:`, hello soubor je umístěn v hello `sshuser` domovský adresář.

Následující příklad kopie hello Hello `test.txt` soubor z hello `sshuser` domovský adresář v místním systému toohello hello primární hlavního uzlu:

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

> [!IMPORTANT]
> `scp`přístup jenom k systému souborů hello jednotlivých uzlů v clusteru hello. Nejde použít tooaccess data v hello HDFS kompatibilní úložiště pro hello cluster.
>
> Použít `scp` když potřebujete tooupload prostředku pro použití z relace SSH. Například odeslat skript v jazyce Python a pak spusťte skript hello z relace SSH.
>
> Informace k přímo načítání dat do hello HDFS kompatibilní úložiště najdete v tématu hello následující dokumenty:
>
> * [HDInsight využívající Azure Storage](hdinsight-hadoop-use-blob-storage.md)
>
> * [HDInsight využívající Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md)

## <a name="next-steps"></a>Další kroky

* [Použití tunelování SSH s HDInsightem](hdinsight-linux-ambari-ssh-tunnel.md)
* [Použití virtuální sítě s HDInsightem](hdinsight-extend-hadoop-virtual-network.md)
* [Použití hraničních uzlů v HDInsightu](hdinsight-apps-use-edge-node.md#access-an-edge-node)