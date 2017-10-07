---
title: "Další úložiště Azure aaaAdd účty tooHDInsight | Microsoft Docs"
description: "Zjistěte, jak další úložiště Azure tooadd účty tooan stávajícího clusteru HDInsight."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/04/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: ce5acfa4b61bf7e83b1fb374d64a1eaa3182fbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-additional-storage-accounts-toohdinsight"></a>Přidejte další úložiště účtů tooHDInsight

Zjistěte, jak toouse skript akce tooadd další úložiště Azure účty tooHDInsight. Hello kroky v tomto dokumentu přidejte účet tooan existující HDInsight se systémem Linux clusteru úložiště.

> [!IMPORTANT]
> Hello informace v tomto dokumentu je o přidání dalšího úložiště tooa clusteru po jeho vytvoření. Informace o přidání účty úložiště při vytváření clusteru najdete v tématu [nastavit clusterů v HDInsight Hadoop, Spark, Kafka a dalšími](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="how-it-works"></a>Jak to funguje

Tento skript má hello následující parametry:

* __Název účtu úložiště Azure__: název hello hello úložiště účet tooadd toohello clusteru HDInsight. Po spuštění skriptu hello, HDInsight můžete číst a zapisovat data uložená v rámci tohoto účtu úložiště.

* __Klíč účtu úložiště Azure__: klíč, který uděluje přístup k účtu úložiště toohello.

* __-p__ (volitelné):-li zadána, hello klíč není šifrován a je uložen v souboru core-site.xml hello jako prostý text.

Během zpracování hello skript provede hello následující akce:

* Pokud hello účet úložiště už existuje v konfiguraci core-site.xml hello hello clusteru, ukončí hello skriptu a byly provedeny žádné další akce.

* Ověřuje, že účet úložiště hello existuje a je přístupný pomocí klíče hello.

* Zašifruje pomocí přihlašovacích údajů clusteru hello klíč hello.

* Přidá soubor základní site.xml toohello účet úložiště hello.

* Zastaví a restartuje hello Oozie, YARN, MapReduce2 a HDFS služby. Zastavení a spuštění těchto služeb jim umožňuje toouse hello nový účet úložiště.

> [!WARNING]
> Použití účtu úložiště v jiném umístění než hello HDInsight cluster není podporováno.

## <a name="hello-script"></a>skript Hello

__Skript umístění__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)

__Požadavky na__:

* skript Hello se musí použít na hello __hlavní uzly__.

## <a name="toouse-hello-script"></a>toouse hello skriptu

Tento skript lze z hello portál Azure, Azure PowerShell nebo hello Azure CLI 1.0. Další informace najdete v tématu hello [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentu.

> [!IMPORTANT]
> Při použití hello kroků uvedených v dokumentu hello přizpůsobení, použijte následující informace tooapply hello tento skript:
>
> * Nahradí všechny akce skriptu příklad URI hello identifikátor URI pro tento skript (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).
> * Nahradí všechny parametry příklad hello název účtu úložiště Azure a klíč hello úložiště účet toobe přidané toohello clusteru. Pokud pomocí hello portálu Azure, musí být tyto parametry oddělené mezerou.
> * Není nutné toomark tento skript jako __trvalé__, jak přímo aktualizuje hello Ambari konfiguraci pro hello cluster.

## <a name="known-issues"></a>Známé problémy

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a>Nezobrazuje se v portálu Azure nebo nástroje pro účty úložiště

Při zobrazení hello HDInsight cluster v hello portálu Azure, výběr hello __účty úložiště__ položky v rámci __vlastnosti__ nezobrazí účty úložiště, které jsou přidány prostřednictvím této akce skriptu. Azure PowerShell a rozhraní příkazového řádku Azure nezobrazují účtu další úložiště hello buď.

informace o Hello úložiště není zobrazit, protože skript hello pouze upraví konfiguraci core-site.xml hello hello clusteru. Tyto informace se nepoužívá při načítání informací o hello clusteru pomocí Azure rozhraní API pro správu.

informace o účtu úložiště tooview přidat toohello clusteru pomocí tohoto skriptu, použijte hello Ambari REST API. Použijte následující příkazy tooretrieve hello tyto informace pro váš cluster:

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter hello cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> Nastavit `$clusterName` toohello název clusteru HDInsight hello. Nastavit `$storageAccountName` toohello název účtu úložiště hello. Po zobrazení výzvy zadejte přihlašovací jméno clusteru hello (správce) a heslo.

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> Nastavit `$PASSWORD` heslo účtu přihlášení (správce) toohello clusteru. Nastavit `$CLUSTERNAME` toohello název clusteru HDInsight hello. Nastavit `$STORAGEACCOUNTNAME` toohello název účtu úložiště hello.
>
> Tento příklad používá [curl (http://curl.haxx.se/)](http://curl.haxx.se/) a [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) tooretrieve a analýzu dat JSON.

Při použití tohoto příkazu, nahraďte __CLUSTERNAME__ s názvem hello hello clusteru HDInsight. Nahraďte __heslo__ heslem hello HTTP přihlášení pro hello cluster. Nahraďte __STORAGEACCOUNT__ hello název účtu úložiště hello přidána pomocí akce skriptu. Informace o vrácená z tohoto příkazu se zobrazí podobné toohello následující text:

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

Tento text je příkladem šifrovaný klíč, který se používá tooaccess hello účet úložiště.

### <a name="unable-tooaccess-storage-after-changing-key"></a>Úložiště nelze tooaccess po změně klíč

Pokud změníte hello klíč pro účet úložiště, HDInsight mít nadále přístup k účtu úložiště hello. HDInsight používá kopii klíče uložené v mezipaměti v hello core-site.xml pro hello cluster. Tato kopie v mezipaměti musí být aktualizované toomatch hello nový klíč.

Spuštění skriptu akci hello nemá __není__ aktualizovat hello klíč, protože hello skript kontroluje toosee, pokud již existuje položka pro účet úložiště hello. Pokud položka již existuje, nebyly provedeny žádné změny.

toowork tento problém vyřešit, je nutné odebrat hello existující položku pro účet úložiště hello. Použijte následující postup tooremove hello existující položku hello:

1. Ve webovém prohlížeči otevřete hello webové uživatelské rozhraní Ambari pro váš cluster HDInsight. Hello identifikátor URI je https://CLUSTERNAME.azurehdinsight.net. Nahraďte __CLUSTERNAME__ s hello názvem vašeho clusteru.

    Po zobrazení výzvy zadejte hello HTTP přihlášení uživatele a heslo pro váš cluster.

2. Hello seznamu služeb na levé straně hello hello stránky, vyberte __HDFS__. Potom vyberte hello __konfigurací__ ve středu hello hello stránky.

3. V hello __filtru...__  pole, zadejte hodnotu __fs.azure.account__. Tento příkaz vrátí položky pro všechny účty dalšího úložiště, které byly přidány toohello clusteru. Existují dva typy položek; __keyprovider__ a __klíč__. Oba obsahují hello název účtu úložiště hello jako součást hello název klíče.

    Hello se následující příklad položky pro účet úložiště s názvem __mystorage__:

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. Po zjištění hello klíče pro účet úložiště hello potřebujete tooremove, použijte hello red '-' toohello ikony napravo od hello položka toodelete ho. Potom pomocí hello __Uložit__ tlačítko toosave změny.

5. Po uložení změn, použijte hello skript akce tooadd hello účet úložiště a nový cluster toohello hodnotu klíče.

### <a name="poor-performance"></a>Snížený výkon

Pokud účet úložiště hello v jiné oblasti než hello clusteru HDInsight se můžete setkat snížený výkon. Přístupu k datům v provozu sítě jiné oblasti zasílá, mimo hello místní datové centrum Azure a napříč hello veřejného Internetu, které můžou představovat latence.

> [!WARNING]
> Použití účtu úložiště v jiné oblasti než hello HDInsight cluster není podporováno.

### <a name="additional-charges"></a>Další poplatky.

Pokud účet úložiště hello je v jiné oblasti než hello clusteru HDInsight, můžete si všimnout další nimi spojeným nákladům na vaši fakturaci Azure. Poplatek za odchozí data se použije v případě, že data ponechá místního datového centra. Tento poplatek za platí i v případě, že provoz hello je určené pro jiné datové centrum Azure v jiné oblasti.

> [!WARNING]
> Použití účtu úložiště v jiné oblasti než hello HDInsight cluster není podporováno.

## <a name="next-steps"></a>Další kroky

Jste se naučili, jak účtů úložiště další tooadd tooan stávajícího clusteru HDInsight. Další informace o akcí skriptů naleznete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md)
