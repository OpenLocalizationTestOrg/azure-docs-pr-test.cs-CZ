---
title: aaaInstall Rstudia s R serverem v HDInsight - Azure | Microsoft Docs
description: Jak tooinstall Rstudia s R serverem v HDInsight.
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 918abb0d-8248-4bc5-98dc-089c0e007d49
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: b3a23021fcf99217e8f551f8b2e89bf1f1e5b967
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a>Instalace Rstudia s R serverem v HDInsight

Tento článek popisuje, jak tooinstall hello community (zdarma) verze [Rstudia Server](https://www.rstudio.com/products/rstudio-server/) hello edge uzlu clusteru pomocí vlastního skriptu. Rstudia Server poskytuje IDE se založené na prohlížeči pro vzdálené klienty a se často používá v systému Linux. Existuje více integrované vývojové prostředí (integrovaného vývojového prostředí) k dispozici pro R dnes, včetně:

- Společnosti Microsoft [R Tools pro Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) 
- [Rstudia serveru](https://www.rstudio.com/products/rstudio-server/) 
- Walware je na základě Eclipse [StatET](http://www.walware.de/goto/statet).

Výhodou Hello nainstalovat Rstudia Server hello hraniční uzel clusteru HDInsight je, že zajišťuje úplné prostředí IDE pro vývoj hello a spouštění skriptů R s R Server v clusteru hello. Tato konfigurace může být výrazně zvýšit produktivitu, než výchozí použití hello R konzoly.

> [!NOTE]
> Hello postup popsaný v tomto článku je pouze v případě, že jste nevybrali tooinstall edice community Rstudia serveru při zřizování clusteru. Pokud jste přidali při zřizování, pak tooaccess ho kliknutím na hello **řídicí panely serveru R** dlaždice v hello Azure portálu položka pro váš cluster a potom na hello **R Studio Server** dlaždici. 

Pokud dáváte přednost toouse hello komerčně licenci Pro verze Rstudia serveru, postupujte podle hello pokyny k instalaci z [Rstudia Server](https://www.rstudio.com/products/rstudio/download-server/).

> [!NOTE]
> Pokud používáte cluster služby HDInsight, pro který byla nainstalována R pomocí hello [nainstalovat akce skriptu R](hdinsight-hadoop-r-scripts-linux.md), hello kroky v tomto dokumentu nebude fungovat správně, protože vyžadují serveru R na clusteru HDInsight hello.
>
> 

## <a name="prerequisites"></a>Požadavky

* Cluster Azure HDInsight s nainstalovaným serverem R. Pokyny najdete v tématu [začít pracovat s R Server v clusterech HDInsight](hdinsight-hadoop-r-server-get-started.md).
* Klientem SSH. Pro distribuce operačních systémů Linux a Unix nebo Macintosh OS X hello `ssh` příkaz se poskytuje s hello operačního systému. Pro systém Windows, doporučujeme, abyste [emulaci](http://www.redhat.com/services/custom/cygwin/) s hello [OpenSSH možnost](https://www.youtube.com/watch?v=CwYSvvGaiWU), nebo [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  

## <a name="install-rstudio-on-hello-cluster-using-a-custom-script"></a>Nainstalujte Rstudia hello clusteru pomocí vlastního skriptu

Zde jsou kroky hello:

1. Identifikujte hello hraniční uzel clusteru hello. Pro cluster služby HDInsight s R Server následuje hello zásady vytváření názvů pro hlavní uzel a hraniční uzel.
   * Hlavní uzel`CLUSTERNAME-ssh.azurehdinsight.net`
   * Hraniční uzel`CLUSTERNAME-ed-ssh.azurehdinsight.net` 

2. SSH do hello hraniční uzel clusteru hello pomocí vzoru pro pojmenovávání hello zadané v kroku 1. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Jakmile se připojíte, stát uživatele root na hello clusteru. V relaci SSH hello použijte následující příkaz hello:

        sudo su -

4. Stáhněte si hello vlastní skript tooinstall Rstudia. Hello použijte následující příkaz:

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Změna hello oprávnění u souboru hello vlastní skript a spusťte skript hello. Hello použijte následující příkazy:

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Pokud jste použili heslo k SSH při vytváření clusteru HDInsight s R Server, můžete tento krok přeskočit a přejít toohello Další. Pokud jste použili klíče SSH místo toocreate hello clusteru, musíte nastavit heslo pro vaše uživatele SSH. Při připojování tooRStudio musíte toto heslo. Spusťte následující příkazy hello:

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. Po zobrazení výzvy k **aktuální Kerberos heslo**, stiskněte klávesu **ENTER**.  Všimněte si, že je třeba nahradit `USERNAME` s uživatelem SSH pro váš cluster HDInsight. Pokud je heslo úspěšně nastavená, měli byste vidět hello následující zprávou:

        passwd: password updated successfully

    Ukončení relace SSH hello.

8. Vytvoření clusteru toohello tunelového propojení SSH pomocí mapování `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` v hello HDInsight clusteru toohello klientský počítač. Tunelové propojení SSH je třeba vytvořit před otevřením novou relaci prohlížeče.

   * U klienta Linux nebo klienta Windows s [emulaci](http://www.redhat.com/services/custom/cygwin/), otevřete relaci terminálu a použít hello následující příkaz:

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       Nahraďte **uživatelské jméno** s uživatelem SSH pro váš HDInsight cluster a nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight.
       Můžete také použít klíč SSH, nikoli heslo přidáním `-i id_rsa_key`.        
   * Pokud klient systému Windows pomocí klienta PuTTY pak

     1. Otevřete PuTTY a zadejte informace o připojení.
     2. V hello **kategorie** levé části toohello hello dialogového okna, rozbalte položku **připojení**, rozbalte položku **SSH**a potom vyberte **tunely**.
     3. Zadejte následující informace na hello hello **možnosti řízení SSH port předávání** formuláře:

        * **Zdrojový port** -hello na klienta hello chcete tooforward port. Například **8787**.
        * **Cílový** – hello cílového umístění, které musí být namapovaný toohello místní klientský počítač. Například **localhost:8787**.

            ![Vytvoření tunelu SSH](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "vytvoření tunelu SSH")

     4. Klikněte na tlačítko **přidat** tooadd hello nastavení a potom klikněte na **otevřete** tooopen připojení SSH.
     5. Po zobrazení výzvy, přihlaste se toohello server tooestablish tunelu SSH relace a povolit hello.

9. Otevřete webový prohlížeč a zadejte následující adresu URL na hello port, které jste zadali pro tunelové propojení hello hello:

        http://localhost:8787/ 

10. Jste výzvami tooenter hello SSH uživatelské jméno a heslo tooconnect toohello clusteru. Pokud jste použili klíč SSH při vytváření clusteru hello, je nutné zadat heslo hello, kterou jste vytvořili v kroku 5.

    ![Připojit tooR Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "vytvoření tunelu SSH")

11. tootest zda hello Rstudia instalace byla úspěšná, můžete spustit test skript, který spouští úlohy MapReduce a Spark na základě R na clusteru hello. toodownload hello testovací skriptu toorun v Rstudia, toohello SSH konzole přejděte zpět a zadejte hello následující příkazy:

    *    Pokud jste vytvořili clusteru Hadoop s R, použijte tento příkaz:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r
    *    Když vytvoříte Spark cluster s R, použijte tento příkaz:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

12. V Rstudia najdete v části hello testování skript, který jste stáhli. Poklikejte na soubor tooopen hello, vyberte obsah hello hello souboru a pak klikněte na tlačítko **spustit**. Měli byste vidět výstup hello v hello **konzoly** podokně:

   ![Testování instalace hello](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "otestovat instalaci hello")

Další možností by tootype `source(testhdi.r)` nebo `source(testhdi_spark.r)` tooexecute hello skriptu.

## <a name="see-also"></a>Viz také

* [Výpočetní kontextu možnosti pro R Server v clusterech prostředí HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Možnosti služby Azure Storage pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-storage.md)

