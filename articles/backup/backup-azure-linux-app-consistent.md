---
title: "Azure Backup: zálohování konzistentní s aplikací virtuálních počítačů Linux | Microsoft Docs"
description: "Skripty tooguarantee zálohování konzistentní s aplikací tooAzure, používáte pro virtuální počítače s Linuxem. skripty Hello platí pouze tooLinux virtuálních počítačů v nasazení Resource Manager; skripty Hello se nevztahují tooWindows virtuálních počítačů nebo nasazení service manager. Tento článek vás provede hello kroky pro konfiguraci hello skripty, včetně řešení potíží."
services: backup
documentationcenter: dev-center-name
author: anuragmehrotra
manager: shivamg
keywords: "zálohování konzistentní s aplikací; zálohování konzistentní s aplikací virtuálního počítače Azure; Zálohování virtuálních počítačů Linux; Zálohování Azure"
ms.assetid: bbb99cf2-d8c7-4b3d-8b29-eadc0fed3bef
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 4/12/2017
ms.author: anuragm;markgal
ms.openlocfilehash: d557dd973364d79bb4d8ce954f648de835dd345f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-consistent-backup-of-azure-linux-vms-preview"></a>Zálohování konzistentní s aplikací virtuálních počítačů Linux Azure (preview)

V tomto článku bude zmíněn hello Linux skript před a po skriptu framework a jak může být zálohování konzistentní s aplikací používaných tootake virtuálních počítačů Linux Azure.

> [!Note]
> skript před a po skript framework Hello je podporována pouze pro virtuální počítače s Linuxem nasazení Azure Resource Manager. Skripty pro konzistence aplikací nejsou podporovány pro nasazení portálu Service Manager virtuální počítače nebo virtuální počítače s Windows.
>

## <a name="how-hello-framework-works"></a>Jak funguje hello framework

Hello framework poskytuje toorun možnost vlastní skripty před a po skriptů při přenášíte snímky virtuálních počítačů. Předběžné skripty se spouštějí těsně před trvat hello snímku virtuálního počítače, a po skripty se spouštějí ihned po trvat hello snímku virtuálního počítače. Tato poskytuje můžete hello flexibilitu toocontrol aplikaci a prostředí při přenášíte snímky virtuálních počítačů.

V tomto scénáři je důležité tooensure zálohování konzistentní s aplikací virtuálního počítače. Hello předběžné skriptu můžete vyvolat nativní aplikace rozhraní API tooquiesce hello IOs a vyprázdnění disku toohello obsahu v paměti. To zajistí, že tento snímek hello je konzistentní s aplikacemi (to znamená, hello aplikace zobrazí, když hello virtuální počítač se spustí po obnovení). Po skriptu lze použít toothaw hello IOs. Dělá to pomocí rozhraní API nativní aplikace, aby aplikace hello možné obnovit normální provozní podmínky post-VM snímku.

## <a name="steps-tooconfigure-pre-script-and-post-script"></a>Kroky tooconfigure skript před a po skript

1. Zaregistrujte v hello kořenové uživatele toohello virtuálního počítače s Linuxem, které chcete tooback nahoru.

2. Stáhnout **VMSnapshotScriptPluginConfig.json** z [Githubu](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig)a potom ho zkopírujete toohello **/etc/azure** složku všechny hello virtuálních počítačů, které budete tooback nahoru. Vytvoření hello **/etc/azure** adresáře, pokud již neexistuje.

3. Zkopírujte hello skript před a po skript aplikace na všechny hello virtuální počítače, který máte v plánu tooback nahoru. Můžete zkopírovat hello skripty tooany umístění na hello virtuálních počítačů. Být úplná cesta k zda tooupdate hello hello souborů skriptů v hello **VMSnapshotScriptPluginConfig.json** souboru.

4. Ujistěte se, hello následující oprávnění pro tyto soubory:

   - **VMSnapshotScriptPluginConfig.json**: oprávnění "600." Například pouze "root" uživatel by měl mít soubor toothis oprávnění "číst" a "zápisu" a "spustit" oprávnění by měl mít žádný uživatel.

   - **Soubor skriptu před**: oprávnění "700."  Například by měl mít jenom uživatel "root" "číst", "zápisu" a "spustit" soubor toothis oprávnění.
  
   - **Po skriptu** oprávnění "700." Například by měl mít jenom uživatel "root" "číst", "zápisu" a "spustit" soubor toothis oprávnění.

   > [!Important]
   > Hello framework poskytuje uživatelům spoustu napájení. Je důležité, aby je bezpečné a pouze "root" má přístup k souborům JSON a skript toocritical.
   > Pokud hello předchozí požadavky nejsou splněny, hello skript není spuštěn. Výsledkem je konzistentní zálohování nebo selhání systému souborů.
   >

5. Konfigurace **VMSnapshotScriptPluginConfig.json** podle postupu popsaného tady:
    - **pluginName**: Toto pole, jako je zůstat nebo skripty nemusí fungovat podle očekávání.

    - **preScriptLocation**: Zadejte úplnou cestu hello hello předběžné skriptu na hello to je toobe probíhající zálohovat virtuální počítač.

    - **postScriptLocation**: Zadejte úplnou cestu hello hello po skriptu na hello to je toobe probíhající zálohovat virtuální počítač.

    - **preScriptParams**: Zadejte hello volitelné parametry, které je třeba toobe předán toohello před skriptu. Všechny parametry musí být v uvozovkách a musí být oddělené čárkami, pokud existuje více parametrů.

    - **postScriptParams**: Zadejte hello volitelné parametry, které je třeba toobe předán toohello po skriptu. Všechny parametry musí být v uvozovkách a musí být oddělené čárkami, pokud existuje více parametrů.

    - **preScriptNoOfRetries**: nastavíte hello počet hello předběžné skriptu by měl zopakován, pokud je před ukončením chyby. Zkuste pouze jeden nula znamená a žádné opakování. Pokud dojde k selhání.

    - **postScriptNoOfRetries**: nastavte hello počet, kolikrát po skriptu hello je třeba opakovat pokud chybě před ukončením. Zkuste pouze jeden nula znamená a žádné opakování. Pokud dojde k selhání.
    
    - **časový_limit_v_sekundách**: Zadejte jednotlivé časové limity pro hello předběžné skript a skript po hello.

    - **continueBackupOnFailure**: nastavení této hodnoty příliš**true** Pokud má Azure Backup toofall back tooa zálohu systému souborů konzistentní nebo havárie konzistentní, je-li skript před nebo po skript selže. Toto nastavení příliš**false** selže hello zálohování v případě selhání skriptu (s výjimkou Pokud máte jeden disk virtuální počítač vrátí zpět toocrash konzistentní zálohování bez ohledu na toto nastavení).

    - **fsFreezeEnabled**: Zadejte, zda Linux fsfreeze by měla být volána při přenášíte konzistence systému souborů tooensure snímku hello virtuálních počítačů. Doporučujeme zachovat toto nastavení nastavit příliš**true** Pokud vaše aplikace je závislý na zakázání fsfreeze.

6. skript framework Hello je nyní nakonfigurována. Pokud už je nakonfigurované hello zálohování virtuálních počítačů, hello další zálohování vyvolá hello skripty a aktivuje zálohování konzistentní s aplikací. Pokud není nakonfigurováno hello zálohování virtuálních počítačů, nakonfigurovat ji pomocí [zálohování trezory služeb tooRecovery virtuální počítače Azure.](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

## <a name="troubleshooting"></a>Řešení potíží

Ujistěte se, že přidáte příslušné protokolování při psaní skriptů před a po skript a zkontrolujte protokoly toofix váš skript problémy skriptu. Pokud máte potíže se spouštěním skripty, podívejte se na toohello následující tabulka pro další informace.

| Chyba | Chybová zpráva | Doporučená akce |
| ------------------------ | -------------- | ------------------ |
| Před ScriptExecutionFailed |předběžné skriptu Hello vrátila chybu, proto nemusí být zálohování konzistentní s aplikací. | Podívejte se na hello selhání protokoly pro váš skript toofix hello problém.|  
|   POST-ScriptExecutionFailed |    Po skriptu Hello vrátila chybu, která může mít vliv na stav aplikace. |  V protokolech selhání hello svého skriptu toofix hello problému a zkontrolujte stav aplikace hello. |
| Před ScriptNotFound |  Hello předběžné skript nebyl nalezen v umístění hello, který je uveden v hello **VMSnapshotScriptPluginConfig.json** konfiguračního souboru. | Zkontrolujte, jestli to před skript se nachází v cestě hello, který je uveden v zálohování hello do konfiguračního souboru tooensure konzistentní s aplikací.|
| POST-ScriptNotFound | Hello po skript nebyl nalezen v umístění hello, který je uveden v hello **VMSnapshotScriptPluginConfig.json** konfiguračního souboru. | Zkontrolujte, že to po skript se nachází v cestě hello, který je uveden v zálohování hello do konfiguračního souboru tooensure konzistentní s aplikací.|
| IncorrectPluginhostFile | Hello **Pluginhost** souboru, který se dodává s hello VmSnapshotLinux rozšíření, je poškozený, takže nelze spustit skript před a po skript a nebude hello zálohování konzistentní s aplikací.   | Odinstalujte hello **VmSnapshotLinux** rozšíření a bude nutné přeinstalovat automaticky s hello další zálohy toofix hello problém. |
| IncorrectJSONConfigFile | Hello **VMSnapshotScriptPluginConfig.json** souboru je nesprávná, takže před skript a po skriptu nelze spustit a nebude hello zálohování konzistentní s aplikací. | Stáhněte si kopii hello z [Githubu](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) a nakonfigurujte ji znovu. |
| InsufficientPermissionforPre skriptu | Ke spouštění skriptů, "root" uživatel by měl být hello vlastníka souboru hello a hello soubor by měl mít oprávnění "700" (to znamená, musí mít pouze "vlastník" "číst", "zápisu" a "spustit" oprávnění). | Zajistěte, aby uživatel "root" hello "vlastník" hello soubor skriptu a že pouze "vlastník" má "oprávnění ke čtení", "zápisu" a "spustit". |
| InsufficientPermissionforPost skriptu | Ke spouštění skriptů, kořenový uživatel by měl být hello vlastníka souboru hello a hello soubor by měl mít oprávnění "700" (to znamená, musí mít pouze "vlastník" "číst", "zápisu" a "spustit" oprávnění). | Zajistěte, aby uživatel "root" hello "vlastník" hello soubor skriptu a že pouze "vlastník" má "oprávnění ke čtení", "zápisu" a "spustit". |
| Před ScriptTimeout | Hello provedení hello konzistentní s aplikací zálohování před skriptu vypršel. | Zkontrolujte skript hello a zvýšit časový limit hello v hello **VMSnapshotScriptPluginConfig.json** soubor, který se nachází v **/etc/azure**. |
| POST-ScriptTimeout | Vypršel časový limit spuštění Hello hello konzistentní s aplikací po zálohování skriptu. | Zkontrolujte skript hello a zvýšit časový limit hello v hello **VMSnapshotScriptPluginConfig.json** soubor, který se nachází v **/etc/azure**. |

## <a name="next-steps"></a>Další kroky
[Konfigurace trezoru služeb zotavení tooa zálohování virtuálních počítačů](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
