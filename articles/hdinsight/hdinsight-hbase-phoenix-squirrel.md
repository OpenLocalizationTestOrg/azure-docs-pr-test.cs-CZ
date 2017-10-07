---
title: "aaaUse Apache Phoenix a SQuirreL s HDInsight se systémem Windows Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Apache Phoenix v HDInsight a jak tooinstall a konfigurace SQuirreL na clusteru pracovní stanice tooconnect tooan HBase v HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1a756e98-75c1-44cd-a178-c5119683b7b7
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 147ac35fa882fd1bedbc5361ac804c36a4d56de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a>Použití nástrojů Apache Phoenix a SQuirreL s clustery založené na Windows HBase v HDInsight
Zjistěte, jak toouse [Apache Phoenix](http://phoenix.apache.org/) v HDInsight a jak tooinstall a konfigurace SQuirreL na clusteru pracovní stanice tooconnect tooan HBase v HDInsight. Další informace o Phoenix najdete v tématu [Phoenix za 15 minut nebo méně](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Hello gramatika Phoenix, najdete v části [Phoenix gramatika](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Hello Phoenix informace o verzi v HDInsight, naleznete v části [co je nového ve verzích clusterů systému Hadoop hello poskytovaných v HDInsight?](hdinsight-component-versioning.md).
>

> [!IMPORTANT]
> Hello kroky v této dokumentu funguje jenom pro clustery HDInsight se systémem Windows. HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Informace o používání Phoenix na HDInsight se systémem Linux najdete v tématu [clusterů použití nástrojů Apache Phoenix se systémem Linux HBase v HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).
>



## <a name="use-sqlline"></a>Použití SQLLine
[SQLLine](http://sqlline.sourceforge.net/) je tooexecute nástroj příkazového řádku SQL.

### <a name="prerequisites"></a>Požadavky
Než budete moci použít SQLLine, musíte mít následující hello:

* **Cluster HBase v HDInsight**. Informace o zřizování HBase clusteru najdete v tématu [začít pracovat s Apache HBase v HDInsight][hdinsight-hbase-get-started].
* **Připojit cluster HBase toohello prostřednictvím protokolu vzdálené plochy hello**. Pokyny najdete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure Classic hello][hdinsight-manage-portal].

**toofind se název hostitele hello**

1. Otevřete **Hadoop příkazového řádku** z plochy hello.
2. Spusťte následující příkaz tooget hello DNS přípona hello:

        ipconfig

    Zapište **příponu DNS specifickou pro připojení**. Například *myhbasecluster.f5.internal.cloudapp.net*. Když připojíte tooan HBase cluster, budete potřebovat tooconnect tooone z hello Zookeepers pomocí plně kvalifikovaného názvu domény. Každý cluster HDInsight má 3 Zookeepers. Jsou *zookeeper0*, *zookeeper1*, a *zookeeper2*. Hello plně kvalifikovaný název domény bude vypadat přibližně jako *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.

**toouse SQLLine**

1. Otevřete **Hadoop příkazového řádku** z plochy hello.
2. Spusťte následující příkazy tooopen SQLLine hello:

        cd %phoenix_home%\bin
        sqlline.py [hello FQDN of one of hello Zookeepers]

    ![HDInsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    příkazy Hello používá v ukázce hello:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

Další informace najdete v tématu [SQLLine ruční](http://sqlline.sourceforge.net/#manual) a [Phoenix gramatika](http://phoenix.apache.org/language/index.html).

## <a name="use-squirrel"></a>Použití SQuirreL
[SQuirreL klienta SQL](http://squirrel-sql.sourceforge.net/) je grafické Java program, který umožní tooview hello struktura databáze kompatibilní JDBC, procházet hello data v tabulkách, vydávat příkazy SQL atd. Dá se použít tooconnect tooApache Phoenix v HDInsight.

V této části se dozvíte, jak tooinstall a konfigurace SQuirreL na vaše pracovní stanice tooconnect tooan cluster HBase v HDInsight prostřednictvím sítě VPN.

### <a name="prerequisites"></a>Požadavky
Než budete postupovat hello postupy, musíte mít následující hello:

* HBase cluster nasadit tooan virtuální síť Azure s virtuálním počítačem DNS.  Pokyny najdete v tématu [vytvořte HBase clusterů ve virtuální síti Azure][hdinsight-hbase-provision-vnet].

* Získáte přípona DNS podle připojení clusteru cluster HBase hello. tooget ho RDP do hello clusteru, a poté spusťte příkaz IPConfig.  přípona DNS Hello je podobná:

        myhbase.b7.internal.cloudapp.net
* Stáhněte a nainstalujte [Microsoft Visual Studio Express pro Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) na pracovní stanici. Je nutné makecert z balíčku toocreate hello svůj certifikát.  
* Stáhněte a nainstalujte [prostředí Java Runtime](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) na pracovní stanici.  SQuirreL SQL klientská verze 3.0 nebo vyšší vyžaduje prostředí JRE 1.6 nebo vyšší verze.  

### <a name="configure-a-point-to-site-vpn-connection-toohello-azure-virtual-network"></a>Konfigurace toohello připojení Point-to-Site VPN virtuální síť Azure
Související se situací konfiguraci připojení point-to-site VPN jsou 3 kroky:

1. [Konfigurace virtuální sítě a brány dynamického směrování](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [Vytvoření certifikáty](#Create-your-certificates)
3. [Konfigurace klienta VPN](#Configure-your-VPN-client)

V tématu [konfigurace tooan připojení Point-to-Site VPN Azure Virtual Network](../vpn-gateway/vpn-gateway-point-to-site-create.md) Další informace.

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a>Konfigurace virtuální sítě a brány dynamického směrování
Zajistit, když máte zřízenou cluster HBase v virtuální sítě Azure (viz hello požadavky pro tento oddíl). dalším krokem Hello je tooconfigure připojení point-to-site.

**připojení point-to-site tooconfigure hello**

1. Přihlaste se toohello [portálu Azure Classic][azure-portal].
2. Na levé straně hello, klikněte na tlačítko **sítě**.
3. Klikněte na virtuální síť hello jste vytvořili (v tématu [HBase zřizování clusterů ve virtuální síti Azure][hdinsight-hbase-provision-vnet]).
4. Klikněte na tlačítko **konfigurace** shora hello.
5. V hello **připojení point-to-site** vyberte **konfiguraci připojení point-to-site**.
6. Konfigurace **počáteční IP** a **CIDR** toospecify hello IP adresu rozsahu, ze kterého klienti sítě VPN obdrží IP adres při připojení. rozsah Hello se nesmí překrývat s žádným z rozsahů ve vaší místní sítě a virtuální síť Azure, které se připojují k hello hello. Například. Pokud jste vybrali 10.0.0.0/20 hello virtuální sítě, můžete vybrat 10.1.0.0/24 pro hello klienta adresní prostor. V tématu hello [připojeníPoint-To-Site] [ vnet-point-to-site-connectivity] stránka Další informace.
7. V části prostory adres hello virtuální sítě, klikněte na tlačítko **přidat podsíť brány**.
8. Klikněte na tlačítko **Uložit** na hello dolní části stránky hello.
9. Klikněte na tlačítko **Ano** tooconfirm hello změnu. Počkejte, dokud hello systém dokončil provádění hello změnit než budete pokračovat dalším postupu toohello.

**toocreate brány dynamického směrování**

1. Z portálu Azure Classic hello, klikněte na tlačítko **řídicí panel** z hello horní části stránky hello.
2. Klikněte na tlačítko **vytvořit BRÁNU** hello dolní části stránky hello.
3. Klikněte na tlačítko **Ano** tooconfirm. Počkejte na vytvoření brány hello.
4. Klikněte na tlačítko **řídicí panel** shora hello.  Zobrazí se visual diagram hello virtuální sítě:

    ![Virtuální diagram point-to-site virtuální síť Azure][img-vnet-diagram]

    Hello diagram znázorňuje 0 připojení klientů. Když provedete toohello připojení virtuální sítě, počet hello bude aktualizované tooone.

#### <a name="create-your-certificates"></a>Vytvoření certifikáty
Jedním ze způsobů toocreate certifikát X.509 je pomocí hello nástroje vytvoření certifikátu (makecert.exe), která se dodává s [Microsoft Visual Studio Express pro Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).

**toocreate certifikát podepsaný svým držitelem kořenové**

1. Pracovní stanice otevřete okno příkazového řádku.
2. Přejděte toohello složka nástrojů pro Visual Studio.
3. Následující příkaz v následujícím příkladu hello Hello vytvoříte a instalace kořenového certifikátu v úložišti osobních certifikátů hello na pracovní stanici a také vytvořit odpovídající soubor .cer, že později nahrajete toohello portálu Azure Classic.

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    Změnit toohello adresář, který chcete hello toobe soubor .cer umístěny v, kde HBaseVnetVPNRootCertificate je název hello chcete toouse hello certifikátu.

    Neuzavírejte hello příkazového řádku.  Budete ho potřebovat v dalším postupu hello.

   > [!NOTE]
   > Vzhledem k tomu, že jste vytvořili kořenový certifikát, ze kterého se budou generovat klientské certifikáty, může tooexport společně s jeho privátní klíč tohoto certifikátu a uložte ho tooa bezpečného umístění, kde ho bude možné obnovit.
   >
   >

**toocreate certifikát klienta**

* Z hello stejné příkazového řádku (má toobe na hello stejný počítač, kde jste vytvořili hello kořenový certifikát. Hello klientský certifikát musí být vygenerovaný z hello kořenového certifikátu), spusťte hello následující příkaz:

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    HBaseVnetVPNRootCertificate je název kořenového certifikátu hello.  Obsahuje název toomatch hello kořenového certifikátu.  

    Hello kořenový certifikát a certifikát klienta hello jsou uloženy v úložišti osobních certifikátů v počítači. Použijte certmgr.msc tooverify.

    ![Certifikát sítě VPN point-to-site virtuální síť Azure][img-certificate]

    Klientský certifikát musí nainstalovat na každém počítači, které chcete tooconnect toohello virtuální sítě. Doporučujeme vám vytvořit jedinečný klientský certifikát pro každý počítač, které chcete tooconnect toohello virtuální sítě. tooexport hello klientské certifikáty pomocí certmgr.msc.

**tooupload hello kořenový certifikát toohello portálu Azure Classic**

1. Z portálu Azure Classic hello, klikněte na tlačítko **sítě** na levé straně hello.
2. Klikněte na tlačítko hello virtuální síť, kde je nasazen HBase cluster na.
3. Klikněte na tlačítko **certifikáty** shora hello.
4. Klikněte na tlačítko **NAHRÁT** z hello dolů a zadejte soubor kořenového certifikátu hello jste vytvořili v postupu hello před posledním. Počkejte, dokud hello certifikát nebyl importován.
5. Klikněte na tlačítko **řídicí panel** hello nahoře.  Hello virtuální diagram zobrazuje stav hello.

#### <a name="configure-your-vpn-client"></a>Konfigurace klienta VPN
**toodownload a nainstalujte hello balíčku klienta VPN**

1. Na stránce řídicího PANELU hello hello virtuální sítě, v části rychlého přehledu hello, klikněte na možnost **hello stažení balíčku klienta VPN 64-bit** nebo **hello stažení balíčku klienta VPN 32-bit** na základě vaší verze operačního systému pracovní stanice.
2. Klikněte na tlačítko **spustit** tooinstall hello balíčku.
3. Na řádku hello zabezpečení, klikněte na tlačítko **Další informace o**a potom klikněte na **přesto spustit**.
4. Klikněte na tlačítko **Ano** dvakrát.

**tooconnect tooVPN**

1. Na ploše hello pracovní stanice klikněte na ikonu sítě hello na hlavním panelu hello. Zobrazí se připojení VPN s názvem vaší virtuální sítě.
2. Klikněte na název připojení VPN hello.
3. Klikněte na **Připojit**.

**tootest hello VPN připojení a domény překlad**

* Z hello pracovní stanice, otevřete příkazový řádek a otestování příkazem ping jeden hello následující názvy zadané hello HBase cluster přípona DNS je myhbase.b7.internal.cloudapp.net:

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a>Instalace a konfigurace SQuirreL na pracovní stanici
**tooinstall SQuirreL**

1. Stáhněte si soubor jar klienta hello SQuirreL SQL z [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).
2. Soubor jar hello otevření a spuštění. Vyžaduje hello [prostředí Java Runtime](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).
3. Klikněte na tlačítko **Další** dvakrát.
4. Zadejte cestu, kde máte hello oprávnění zapisovat a potom klikněte na **Další**.

  > [!NOTE]
  > Hello výchozí instalační složka je ve složce C:\Program Files\squirrel-sql-3.6 hello.  V cestě toothis toowrite pořadí instalační program hello musejí mít oprávnění hello správce. Otevřete příkazový řádek jako správce, přejděte na tooJava složka bin a spusťte:
  >
  >     Java.exe-jar [hello cesta soubor jar SQuirreL hello]
5. Klikněte na tlačítko **OK** tooconfirm vytváření hello cílový adresář.
6. Hello výchozí nastavení je tooinstall hello základní a standardní balíčky.  Klikněte na **Další**.
7. Klikněte na tlačítko **Další** dvakrát a potom klikněte na **provádí**.

**tooinstall hello Phoenix ovladačů**

soubor jar ovladače phoenix Hello se nachází na hello HBase cluster. Cesta Hello je podobné následující toohello podle verze hello:

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
Je třeba toocopy ho tooyour pracovní stanice v rámci hello [Instalační složka SQuirreL] / lib cestu.  Hello nejjednodušší způsob je tooRDP do hello clusteru a potom použijte soubor kopírování a vkládání (CTRL + C a CTRL + V) toocopy ho tooyour pracovní stanice.

**tooadd tooSQuirreL Phoenix ovladačů**

1. Otevřete SQuirreL SQL klienta z pracovní stanice.
2. Klikněte na tlačítko hello **ovladač** karty na levé straně hello.
3. Z hello **ovladače** nabídky, klikněte na tlačítko **nový ovladač**.
4. Zadejte hello následující informace:

   * **Název**: Phoenix
   * **Příklad adresy URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net
   * **Název třídy**: org.apache.phoenix.jdbc.PhoenixDriver

     > [!WARNING]
     > Uživatel všechny malá písmena v hello Příklad adresy URL. Můžete použít jejich úplné zookeeper kvora v případě, že jeden z nich je vypnutý.  názvy hostitelů Hello jsou zookeeper0, zookeeper1 a zookeeper2.
     >
     >

     ![HDInsight HBase Phoenix SQuirreL ovladačů][img-squirrel-driver]
5. Klikněte na **OK**.

**toocreate alias toohello HBase cluster**

1. Z SQuirreL, klikněte na tlačítko hello **aliasy** karty na levé straně hello.
2. Z hello **aliasy** nabídky, klikněte na tlačítko **nový Alias**.
3. Zadejte hello následující informace:

   * **Název**: hello název clusteru HBase hello nebo libovolný název, který preferujete.
   * **Ovladač**: Phoenix.  Toto musí odpovídat názvu ovladače hello, kterou jste vytvořili v posledním postupu hello.
   * **Adresa URL**: hello adresa URL je zkopírována z konfiguraci ovladačů. Ujistěte se, že toouser všechny malá písmena.
   * **Uživatelské jméno**: může být jakýkoli text.  Vzhledem k tomu, že používáte připojení VPN, hello uživatelské jméno se nepoužívá.
   * **Heslo**: může být jakýkoli text.

     ![HDInsight HBase Phoenix SQuirreL ovladačů][img-squirrel-alias]
4. Klikněte na tlačítko **Test**.
5. Klikněte na **Připojit**. Pokud se provede hello připojení, SQuirreL vypadá takto:

    ![HBase Phoenix SQuirreL][img-squirrel]

**toorun testu**

1. Klikněte na tlačítko hello **SQL** kartě právo další toohello **objekty** kartě.
2. Zkopírujte a vložte následující kód hello:

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. Klikněte na tlačítko Spustit hello.

    ![HBase Phoenix SQuirreL][img-squirrel-sql]
4. Přepnout zpět toohello **objekty** kartě.
5. Rozbalte název aliasu hello a potom rozbalte **tabulky**.  Zobrazí se nové tabulky hello uvedený v seznamu.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak toouse Apache Phoenix v HDInsight.  Další, najdete v části toolearn

* [Přehled HBase ve službě HDInsight][hdinsight-hbase-overview]: HBase je NoSQL open source databáze Apache postavená na systému Hadoop, která poskytuje náhodný přístup a silnou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat.
* [Zřídit clustery HBase v Azure Virtual Network][hdinsight-hbase-provision-vnet]: clustery HBase integrace virtuální sítě, může být nasazený toohello stejné virtuální sítě jako aplikace tak, že aplikace může komunikovat s HBase přímo.
* [Konfigurace replikace HBase v HDInsight](hdinsight-hbase-replication.md): Zjistěte, jak tooconfigure HBase replikace mezi dvěma datových centrech Azure.
* [Analýza sentimentu Twitter s HBase v HDInsight][hbase-twitter-sentiment]: Zjistěte, jak v reálném čase toodo [postojích analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) velkých objemů dat pomocí HBase v clusteru Hadoop v HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
