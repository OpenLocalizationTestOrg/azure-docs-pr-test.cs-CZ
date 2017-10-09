---
title: "aaaLearn pomocí izolovaném prostoru – emulátor – Azure HDInsight Hadoop | Microsoft Docs"
description: "získávání informací o použití toostart hello ekosystém Hadoop, můžete nastavit izolovaného prostoru Hadoop z Hortonworks na virtuální počítač Azure. "
keywords: "hadoop emulátoru, izolovaného prostoru hadoop"
editor: cgronlun
manager: jhubbard
services: hdinsight
author: nitinme
documentationcenter: 
tags: azure-portal
ms.assetid: 6ad5bb58-8215-4e3d-a07f-07fcd8839cc6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 91e74f0823fd02e9bb812155a7d09357a77b0736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a>Začínáme s Hadoop izolovaném prostoru, emulátoru na virtuálním počítači

Zjistěte, jak tooinstall hello izolovaného prostoru Hadoop z Hortonworks na virtuální počítač toolearn o ekosystém Hadoop hello. Hello izolovaného prostoru obsahuje místním vývojovém prostředí toolearn o Hadoop, Hadoop Distributed File System (HDFS) a odeslání úlohy. Jakmile se seznámíte s Hadoop, můžete začít používat Hadoop v Azure vytvořením clusteru služby HDInsight. Další informace o tom, jak spustit tooget najdete v tématu [Začínáme s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

## <a name="prerequisites"></a>Požadavky
* [Oracle VirtualBox](https://www.virtualbox.org/). Stáhněte a nainstalujte ji z [zde](https://www.virtualbox.org/wiki/Downloads).



## <a name="download-and-install-hello-virtual-machine"></a>Stáhněte a nainstalujte hello virtuálního počítače
1. Procházet toohello [Hortonworks stáhne](http://hortonworks.com/downloads/#sandbox).

2. Klikněte na tlačítko **stáhnout VIRTUALBOX** toodownload hello nejnovější Hortonworks karanténě na virtuálním počítači. Před zahájením hello stažení jste výzvami tooregister s Hortonworks. Jak dlouho trvá jeden toodownload tootwo hodin v závislosti na rychlosti sítě.
   
    ![Propojit bitovou kopii pro stažení Hortonworks karanténě pro VirtualBox](./media/hdinsight-hadoop-emulator-get-started/download-sandbox.png)
3. Z hello stejné webové stránce, klikněte na tlačítko hello **Import na virtuální,** odkaz toodownload PDF, který obsahuje pokyny k instalaci pro virtuální počítač hello.

toodownload starší HDP verze izolovaného, rozbalte archivu hello:

![Hortonworks karanténě archivu](./media/hdinsight-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-hello-virtual-machine"></a>Spustit virtuální počítač hello

1. Otevřete VirtualBox Oracle virtuálních počítačů.
2. Z hello **soubor** nabídky, klikněte na tlačítko **Import zařízení**a pak zadejte hello Hortonworks karanténě image.
1. Klikněte na tlačítko vyberte hello Hortonworks karanténě **spustit**a potom **normální spuštění**. Po dokončení procesu spouštění hello hello virtuální počítač, zobrazí pokyny přihlášení.
   
    ![Normální spuštění](./media/hdinsight-hadoop-emulator-get-started/normal-start.png)
2. Otevřete webový prohlížeč a přejděte toohello adresu URL zobrazené (obvykle http://127.0.0.1:8888).

## <a name="set-sandbox-passwords"></a>Nastavit heslo izolovaného prostoru

1. Z hello **Začínáme** krok hello Hortonworks karanténě stránky, vyberte **pokročilé možnosti zobrazení**. Použijte hello informace na této stránce toolog v izolovaném prostoru toohello pomocí protokolu SSH. Použijte název hello a zadat heslo.
   
   > [!NOTE]
   > Pokud nemáte nainstalovaného klienta SSH, můžete použít hello webové SSH uvedených v hello virtuální počítač na **http://localhost:4200 /**.
   > 
   
    Hello poprvé, ke kterým se připojujete pomocí protokolu SSH, jste výzvami toochange hello heslo pro hello kořenového účtu. Zadejte nové heslo, které můžete použít při přihlášení pomocí protokolu SSH.

2. Po přihlášení, zadejte následující příkaz hello:
   
        ambari-admin-password-reset
   
    Po zobrazení výzvy zadejte heslo pro účet správce Ambari hello. Používá se při přístupu k hello webové uživatelské rozhraní Ambari.

## <a name="use-hive-commands"></a>Použití Hive příkazů

1. Ze SSH připojení toohello karantény použijte následující příkaz toostart hello Hive prostředí hello:
   
        hive
2. Po zahájení hello prostředí, použijte hello následující tooview hello tabulek, které jsou k dispozici s hello izolovaného prostoru:
   
        show tables;
3. Použití hello následujících tooretrieve 10 řádků z hello `sample_07` tabulky:
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a>Další kroky
* [Zjistěte, jak toouse Visual Studio s hello Hortonworks karanténě](hdinsight-hadoop-emulator-visual-studio.md)
* [Learning hello LAN Dobrý den Hortonworks karanténě](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop kurz – Začínáme s HDP](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

