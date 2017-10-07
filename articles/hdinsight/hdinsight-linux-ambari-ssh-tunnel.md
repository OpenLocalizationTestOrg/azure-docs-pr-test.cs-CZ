---
title: aaaUse SSH tunelu tooaccess Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak procházet toouse toosecurely tunelového propojení SSH webové prostředky, které jsou hostované na uzly HDInsight se systémem Linux."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 879834a4-52d0-499c-a3ae-8d28863abf65
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 8a315c53751bbe6950a182674f4108c67c2f2f8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-ssh-tunneling-tooaccess-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a>Použít webovému uživatelskému rozhraní Ambari tooaccess tunelového propojení SSH, JobHistory, NameNode, Oozie a jiným webovým uživatelská rozhraní

Clustery se systémem Linux HDInsight poskytují přístup tooAmbari webového uživatelského rozhraní nad hello Internet, ale nejsou některé funkce hello uživatelského rozhraní. Například hello webového uživatelského rozhraní pro jiné služby, které jsou prezentované pomocí Ambari. Pro plnou funkčnost hello webovému uživatelskému rozhraní Ambari je nutné použít SSH tunel toohello clusteru head.

## <a name="why-use-an-ssh-tunnel"></a>Proč používat tunelového propojení SSH

Několik nabídek hello v Ambari fungovat pouze pomocí tunelového propojení SSH. Tyto nabídky spoléhají na weby a služby běžící na jiné typy uzlů, například uzlů pracovního procesu. Často tyto weby nejsou zabezpečené, takže není bezpečné toodirectly zveřejněte je na Internetu hello.

Hello následující webové uživatelská vyžadovat tunelového propojení SSH:

* JobHistory
* NameNode
* Zásobníky vlákna
* Oozie webového uživatelského rozhraní
* Hlavní HBase a protokoly uživatelského rozhraní

Pokud používáte cluster toocustomize akcí skriptů, vyžadují jakékoli služby nebo nástroje, které nainstalujete, které zveřejňují webového uživatelského rozhraní tunelového propojení SSH. Například pokud nainstalujete Hue pomocí akce skriptu, musíte použít SSH tunel tooaccess hello uživatelské rozhraní webu Hue.

> [!IMPORTANT]
> Pokud máte tooHDInsight přímý přístup přes virtuální síť, není nutné toouse tunely SSH. Příklad přímo přístup k HDInsight prostřednictvím virtuální sítě, naleznete v části hello [připojit HDInsight tooyour do místní sítě](connect-on-premises-network.md) dokumentu.

## <a name="what-is-an-ssh-tunnel"></a>Co je tunelového propojení SSH

[Secure Shell (SSH) tunelování](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) směruje provoz odeslaný tooa port na místní pracovní stanici. Hello provoz se směruje pomocí SSH připojení tooyour hlavního uzlu clusteru HDInsight. žádost o Hello se vyřeší, jako by bylo provedeno hello hlavního uzlu. odpověď Hello je pak směrovat zpět pomocí hello tunel tooyour pracovní stanice.

## <a name="prerequisites"></a>Požadavky

* Klientem SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* Webový prohlížeč, který může být nakonfigurován toouse SOCKS5 proxy.

    > [!WARNING]
    > Podpora proxy SOCKS Hello je součástí systému Windows nepodporuje SOCKS5 a nefunguje s hello kroky v tomto dokumentu. Hello následujících prohlížečů závisí na nastavení proxy serveru systému Windows a ne aktuálně pracují s hello kroky v tomto dokumentu:
    >
    > * Microsoft Edge
    > * Aplikace Microsoft Internet Explorer
    >
    > Google Chrome také závisí na nastavení proxy serveru pro Windows hello. Můžete však nainstalovat rozšíření, které podporují SOCKS5. Doporučujeme, abyste [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).

## <a name="usessh"></a>Vytvořit tunelové propojení příkazu SSH hello

Použití hello následující příkaz toocreate tunelového propojení SSH pomocí hello `ssh` příkaz. Nahraďte **uživatelské jméno** s uživatelem SSH pro váš HDInsight cluster a nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight:

```bash
ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
```

Tento příkaz vytvoří připojení, který směruje provoz toolocal port 9876 toohello clusteru prostřednictvím SSH. Hello možnosti jsou:

* **D 9876** -hello místního portu, který směruje provoz tunelem hello.
* **C** -komprimovat všechna data, protože webový provoz je většinou text.
* **2** -force SSH tootry protocol verze 2 pouze.
* **q** -tichém režimu.
* **T** -zakázat pseudo tty přidělení, protože jsme se právě předávání port.
* **n**-Zabránit čtení STDIN, protože jsme se právě předávání port.
* **N** -nespouštějte vzdáleného příkazu, protože jsme se právě předávání port.
* **f** -spustit hello pozadí.

Po dokončení příkazu hello je provoz odeslaný tooport 9876 na místním počítači hello směrované toohello hlavního uzlu clusteru.

## <a name="useputty"></a>Vytvořit tunelové propojení PuTTY

[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) je grafický klient SSH pro systém Windows. Použijte následující postup toocreate tunelového propojení SSH pomocí klienta PuTTY hello:

1. Otevřete PuTTY a zadejte informace o připojení. Pokud nejste obeznámeni s PuTTY, najdete v části hello [PuTTY dokumentace (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).

2. V hello **kategorie** levé části toohello hello dialogového okna, rozbalte položku **připojení**, rozbalte položku **SSH**a potom vyberte **tunely**.

3. Zadejte následující informace na hello hello **možnosti řízení SSH port předávání** formuláře:
   
   * **Zdrojový port** -hello na klienta hello chcete tooforward port. Například **9876**.

   * **Cílový** -hello adresa SSH pro cluster HDInsight se systémem Linux hello. Například **mycluster-ssh.azurehdinsight.net**.

   * **Dynamicky** – umožňuje dynamické směrování proxy SOCKS.
     
     ![Obrázek možnosti tunelové propojení](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. Klikněte na tlačítko **přidat** tooadd hello nastavení a potom klikněte na **otevřete** tooopen připojení SSH.

5. Pokud budete vyzváni, přihlaste se toohello serveru.

## <a name="use-hello-tunnel-from-your-browser"></a>Použití hello tunel z prohlížeče

> [!IMPORTANT]
> Hello kroky v této části použijte hello prohlížeče Mozilla FireFox, jak stejné nastavení proxy serveru poskytuje hello ve všech platformách. Jiné moderní prohlížeče, jako je například Google Chrome, může vyžadovat rozšíření například FoxyProxy toowork s hello tunel.

1. Konfigurace prohlížeče toouse hello **localhost** a port hello použité při vytváření tunelového propojení hello jako **SOCKS v5** proxy serveru. Zde je, jak vypadají hello Firefox nastavení. Pokud jste použili jiný port než 9876, změna toohello hello port, kterého jste jeden:
   
    ![Obrázek nastavení Firefox](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > Výběr **DNS vzdálené** přeloží požadavky systému DNS (Domain Name) pomocí hello clusteru HDInsight. Toto nastavení vyřeší DNS pomocí hello hlavního uzlu clusteru hello.

2. Ověřte, že hello tunel funguje návštěvou lokality [http://www.whatismyip.com/](http://www.whatismyip.com/). Hello IP vrátí, má být jeden používána datacenter hello Microsoft Azure.

## <a name="verify-with-ambari-web-ui"></a>Ověřte si u webovému uživatelskému rozhraní Ambari

Po vytvoření clusteru hello, použijte následující postup tooverify, že máte přístup z hello Ambari Web webové služby uživatelská rozhraní hello:

1. V prohlížeči přejděte toohttp://headnodehost:8080. Hello `headnodehost` adresu odesílané přes hello tunel toohello clusteru a vyřešit headnode toohello, která Ambari běží na. Po zobrazení výzvy zadejte uživatelské jméno správce hello (správce) a heslo pro váš cluster. Můžete být vyzváni podruhé podle webovému uživatelskému rozhraní Ambari hello. Pokud ano, znovu zadejte informace hello.

   > [!NOTE]
   > Při použití hello http://headnodehost:8080 adresu tooconnect toohello clusteru, připojení prostřednictvím tunelového propojení hello. Komunikace je zabezpečena pomocí tunelu SSH hello místo protokolu HTTPS. tooconnect přes hello internet pomocí protokolu HTTPS, použijte https://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je název hello hello clusteru.

2. Hello webové uživatelské rozhraní Ambari vyberte ze seznamu hello na hello nalevo od stránku hello HDFS.

    ![Bitové kopie s HDFS vybrané](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. Když se zobrazí informace o službě HDFS hello, vyberte **rychlé odkazy**. Zobrazí se seznam hlavních uzlech clusteru hello. Vyberte jeden z hlavních uzlech hello a pak vyberte **uživatelského rozhraní NameNode**.

    ![Rozšířit bitové kopie s hello rychlé odkazy nabídky](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]
   > Když vyberete __rychlé odkazy__, může dojít k čekání indikátoru. K tomuto stavu může dojít, pokud máte pomalé připojení k Internetu. Počkejte minutu nebo dvě pro toobe hello dat přijatých ze serveru hello, a akci opakujte hello seznamu.
   >
   > Některé položky v hello **rychlé odkazy** nabídky, může být zkrácené hello pravé straně úvodní obrazovka. Pokud ano, rozbalte položku nabídky hello pomocí myši a použít hello šipku vpravo klíče tooscroll hello obrazovky toohello správné toosee hello zbytek hello nabídky.

4. Zobrazí se stránka podobné toohello následující bitové kopie:

    ![Obrázek hello NameNode uživatelského rozhraní](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]
   > Všimněte si hello adresu URL pro tuto stránku; by mělo být podobné příliš**http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/clusteru**. Tento identifikátor URI hello interní plně kvalifikovaný název domény (FQDN) uzlu hello používá a je k dispozici pouze při používání tunelového propojení SSH.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak toocreate a používání tunelového propojení SSH, najdete v následujícím dokumentu pro jiné způsoby toouse Ambari hello:

* [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md)

Další informace o používání SSH s HDInsight, naleznete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

