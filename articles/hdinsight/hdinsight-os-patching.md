---
title: "plán oprav aaaConfigure operačního systému pro clustery HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure OS oprav plán pro HDInsight se systémem Linux clusterů."
services: hdinsight
documentationcenter: 
author: bprakash
manager: asadk
editor: bprakash
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: bhanupr
ms.openlocfilehash: 1598d64e594d7e8a68573fc63dd86051a5a9d025
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="os-patching-for-hdinsight"></a>Opravy chyb pro HDInsight operačního systému 
Služba se spravuje Hadoop HDInsight má na starosti opravy hello OS hello základní virtuální počítače používané clustery HDInsight. Od 1. srpna 2016 jsme změnili oprav zásad hello hostovaného operačního systému pro clustery HDInsight se systémem Linux (verze 3.4 nebo novější). cílem Hello nové zásady hello je toosignificantly snížit hello počet restartování kvůli toopatching. nové zásady Hello bude pokračovat toopatch virtuální počítače (VM) v systému Linux clusterů každé pondělí a čtvrtek začínající na 12: 00 UTC postupný způsobem mezi uzly v jakémkoliv daného clusteru. Všechny daného virtuálního počítače se však pouze restartuje maximálně jednou za 30 dní z důvodu opravy tooguest operačního systému. Kromě toho hello první restartování pro nově vytvořený cluster neprovede dřív než 30 dní od data vytvoření clusteru hello. Jakmile hello virtuální počítače se restartují, bude platit opravy.

## <a name="how-tooconfigure-hello-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Jak tooconfigure hello plánu oprav operačního systému pro clustery HDInsight se systémem Linux
třeba Hello virtuální počítače v clusteru služby HDInsight toobe příležitostně restartovat, aby se opravy důležité zabezpečení můžete nainstalovat. Od 1. srpna 2016 se restartují nových clusterů HDInsight se systémem Linux (verze 3.4 nebo novější,), pomocí následujících plán hello:

1. Virtuální počítač v clusteru hello můžete pouze po restartování počítače opravy maximálně jednou v období 30 dnů.
2. Hello restartování nastane začínající UTC 12: 00.
3. Hello restartování procesu rozložena mezi virtuální počítače v clusteru hello, takže hello clusteru je stále k dispozici během procesu restartování hello.
4. Hello první restartování pro nově vytvořený cluster neprovede dřív než 30 dní od data vytvoření clusteru hello.

Pomocí akce skriptu hello popsané v tomto článku, můžete upravit plán oprav hello OS následujícím způsobem:
1. Povolit nebo zakázat automatické restartování počítače
2. Frekvence hello sadu restartuje (ve dnech mezi jednotlivými restartováními)
3. Nastavit hello den týdnu hello, pokud dojde k restartu

> [!NOTE]
> Tato akce skriptu budou fungovat jenom s clustery HDInsight se systémem Linux, které jsou vytvořené po 1. srpna 2016. Opravy bude platit pouze v případě, že jsou virtuální počítače restartovat. 
>

## <a name="how-toouse-hello-script"></a>Jak toouse hello skriptu 

Při použití tohoto skriptu vyžaduje hello následující informace:
1. Hello umístění skriptu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.  HDInsight používá tento identifikátor URI toofind a spusťte skript hello na všechny hello virtuální počítače v clusteru hello.
  
2. Hello typy uzlů clusteru, použité hello skriptu: headnode, workernode, zookeeper. Tento skript musí být použité tooall typy uzlů v clusteru hello. Pokud není typ uzlu použité tooa pak hello virtuálních počítačů pro daný typ uzlu bude toouse hello předchozí oprav plán.


3.  Parametr: Tento skript přijímá tři číselné parametry:

    | Parametr | Definice |
    | --- | --- |
    | Povolit nebo zakázat automatické restartování počítače |0 nebo 1. Hodnota 0 zakáže automatické restartování počítače během 1 povolí automatické restartování počítače. |
    | frekvence |7 too90 (včetně). Hello počet dní toowait před restartování hello virtuálních počítačů pro opravy, které vyžadují restart počítače. |
    | Den v týdnu |1 too7 (včetně). Hodnota 1 znamená hello restartování provedeno v pondělí a 7 označuje Sunday.For příklad, pomocí parametrů 1 60 2 výsledky v automaticky restartuje každých 60 dnů (maximálně) úterý. |
    | Trvalost |Při použití existující cluster skript akce tooan, můžete označit hello skriptu jako trvalé. Trvalý skripty se používají při přidání nové workernodes toohello clusteru prostřednictvím operace škálování. |

> [!NOTE]
> Tento skript je třeba označit jako trvalý při použití tooan existujícího clusteru. Všechny nové uzly, které jsou vytvořené pomocí operace škálování, jinak hodnota použije výchozí hello opravy plán.
Pokud použijete hello skript jako součást procesu vytváření clusteru hello, je automaticky trvalá.
>

## <a name="next-steps"></a>Další kroky

Konkrétní kroky pomocí akce skriptu hello, naleznete v následujících částech hello hello [HDInsight se systémem Linuz přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md):

* [Použití akce skriptu při vytváření clusteru](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Použít tooa akce skriptu spuštění clusteru](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
