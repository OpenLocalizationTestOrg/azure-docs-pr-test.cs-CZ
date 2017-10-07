---
title: "aaaCustomize Hadoop clusterů pro hello proces vědecké účely dat Team | Microsoft Docs"
description: "K dispozici ve vlastní clusterů systému Azure HDInsight Hadoop oblíbených modulů Python."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0c115dca-2565-4e7a-9536-6002af5c786a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: e192542dd39f71bccbb5163382b4050d0f12ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-hello-team-data-science-process"></a>Přizpůsobení clusterů systému Azure HDInsight Hadoop pro hello procesu Team dat vědecké účely
Tento článek popisuje jak toocustomize HDInsight Hadoop clusteru nainstalováním Anaconda 64-bit (Python 2.7) na každém uzlu při zřízení clusteru hello jako služba HDInsight. Také ukazuje, jak tooaccess hello headnode toosubmit vlastní úlohy toohello clusteru. Toto vlastní nastavení díky mnoha oblíbených Python moduly, které jsou součástí Anaconda, pohodlně k dispozici pro použití v uživatele definovaných funkcí (UDF), které jsou navržené tooprocess záznamy Hive v clusteru hello. Pokyny o postupech hello používá v tomto scénáři najdete v tématu [jak dotazů Hive toosubmit](machine-learning-data-science-move-hive-tables.md#submit).

Hello následující nabídky odkazy tootopics, které popisují, jak tooset až hello různé vědě prostředí datových používají v hello [tým datové vědy procesu (TDSP)](data-science-process-overview.md).

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

## <a name="customize"></a>Přizpůsobení clusteru Azure HDInsight Hadoop
toocreate vlastní cluster HDInsight Hadoop, začněte tím, že protokolování příliš[**portál Azure classic**](https://manage.windowsazure.com/), klikněte na tlačítko **nový** v hello zbývajících dolního rohu a pak vyberte datové služby -> HDINSIGHT -> **vytvořit vlastní** toobring až hello **podrobnosti o clusteru** okno. 

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Zadejte název hello toobe hello clusteru vytvořit na stránce konfigurace 1 a přijměte výchozí hodnoty pro hello další pole. Klikněte na tlačítko hello šipku toogo toohello další konfigurační stránku. 

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Na stránce konfigurace 2, vstupní hello počet **datové uzly**, vyberte hello **oblasti nebo virtuální síť**a vyberte hello velikosti hello **hlavní uzel** a hello **Datový uzel**. Klikněte na tlačítko hello šipku toogo toohello další konfigurační stránku.

> [!NOTE]
> Hello **oblasti nebo virtuální síť** má toobe hello stejné jako hello oblast hello účet úložiště, který se bude používat pro hello clusteru HDInsight Hadoop toobe. Hello čtvrté stránce konfigurace, jinak hodnota hello účet úložiště se nezobrazí na rozevírací seznam hello **název účtu**.
> 
> 

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Na stránce konfigurace 3 zadejte uživatelské jméno a heslo pro hello clusteru HDInsight Hadoop. **Nechcete** vyberte hello *Enter hello Metaúložiště Hive nebo Oozie*. Potom klikněte na hello šipku toogo toohello další konfigurační stránku. 

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Na stránce konfigurace 4 zadejte název účtu úložiště hello, hello výchozí kontejner hello clusteru HDInsight Hadoop. Pokud vyberete *vytvořit výchozí kontejner* v hello **výchozí kontejner** rozevíracího seznamu, kontejner s hello stejný název jako hello clusteru bude vytvořena. Klikněte na tlačítko hello šipku toogo toohello poslední stránku konfigurace.

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Na poslední hello **akcí skriptů** stránku konfigurace, klikněte na tlačítko **přidat akce skriptu** tlačítko a vyplníte pole textu hello hello následující hodnoty.

* **NÁZEV** -libovolný řetězec jako název hello této akce skriptu
* **Typ uzlu** – vyberte **všechny uzly**
* **Identifikátor URI skriptu** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
  * *publicscripts* je veřejném kontejneru v účtu úložiště hello 
  * *getgoing* používáme tooshare prostředí PowerShell skriptu soubory toofacilitate práci uživatelů v Azure
* **Parametry** -(ponechte prázdné)

Nakonec klikněte na hello zaškrtnutí toostart hello vytváření clusteru HDInsight Hadoop hello přizpůsobit. 

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Přístup k hello uzlu Head clusteru Hadoop
Než se dostanete k hlavnímu uzlu clusteru Hadoop hello hello prostřednictvím protokolu RDP, je nutné povolit clusteru Hadoop toohello vzdáleného přístupu v Azure. 

1. Přihlaste se toohello [ **portál Azure classic**](https://manage.windowsazure.com/), vyberte **HDInsight** na levé straně hello vyberte Hadoop cluster hello seznamu clusterů, klikněte na hello  **KONFIGURACE** a pak klikněte hello **povolit vzdálené** ikonu v hello dolní části stránky hello.
   
    ![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)
2. V hello **konfigurace vzdálené plochy** okno, zadejte hello uživatelské jméno a heslo pole a zvolte datum vypršení platnosti hello pro vzdálený přístup. Klikněte na tlačítko hello zaškrtnutí tooenable hello vzdáleného přístupu toohello hlavního uzlu v clusteru Hadoop hello.
   
    ![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> Hello uživatelské jméno a heslo pro vzdálený přístup hello nejsou hello uživatelské jméno a heslo, které používáte při vytváření clusteru Hadoop hello. Toto je samostatnou sadu přihlašovacích údajů. Datum vypršení platnosti hello hello vzdáleného přístupu má taky, toobe do 7 dnů od hello aktuální datum.
> 
> 

Po povolení vzdáleného přístupu klikněte na tlačítko **CONNECT** dole hello tooremote stránku hello do hlavního uzlu hello. Přihlášení toohello hlavního uzlu v clusteru Hadoop hello zadáním hello pověření pro uživatele hello vzdáleného přístupu, který jste dřív zadali.

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

Hello další kroky v hello pokročilé analýzy procesu jsou namapované v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může obsahovat kroky, které přesun dat do HDInsight, pak zpracovat a ukázkové ho existuje v rámci přípravy učení se z dat hello pomocí Azure Machine Learning.

V tématu [jak dotazů Hive toosubmit](machine-learning-data-science-move-hive-tables.md#submit) pokyny, jak tooaccess hello moduly jazyka Python, které jsou součástí Anaconda z hlavního uzlu clusteru hello v uživatelsky definované funkce (UDF), které jsou používané tooprocess hello Hive záznamy uložené v clusteru hello.

