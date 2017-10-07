---
title: "virtuální počítač s Linuxem pomocí rozšíření virtuálního počítače aaaMonitoring | Microsoft Docs"
description: "Zjistěte, jak toouse hello rozšíření diagnostiky Linux toomonitor hello výkon a diagnostických dat virtuálního počítače s Linuxem v Azure."
services: virtual-machines-linux
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f54a11c5-5a0e-40ff-af6c-e60bd464058b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: Ning
ms.openlocfilehash: cf7bfebca8c0367941f7a975417f60fe2e2dab25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-linux-diagnostic-extension-toomonitor-hello-performance-and-diagnostic-data-of-a-linux-vm"></a>Použít hello rozšíření diagnostiky Linux toomonitor hello výkonu a diagnostických dat virtuálního počítače s Linuxem

Tento dokument popisuje 2.3 verzi hello rozšíření diagnostiky Linux.

> [!IMPORTANT]
> Tato verze je zastaralá a může být publikování kdykoli po 30. června 2018. Nahradila ji verze 3.0. Další informace najdete v tématu hello [dokumentace pro verzi 3.0 hello rozšíření diagnostiky Linux](../diagnostic-extension.md).

## <a name="introduction"></a>Úvod

(**Poznámka**: hello rozšíření diagnostiky Linux je open source na [Githubu](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) kde hello nejaktuálnější informace o rozšíření hello prvním publikování. Můžete chtít toocheck hello [GitHub stránce](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) první.)

Hello rozšíření diagnostiky Linux pomáhá hello monitorování uživatel virtuální počítače Linux, které jsou spuštěné v Microsoft Azure. Má hello následující možnosti:

* Shromažďuje a odesílá informace o výkonu systému hello z tabulky úložiště hello virtuálního počítače s Linuxem toohello uživatele, včetně informací o diagnostiky a syslog.
* Umožňuje uživatelům toocustomize hello data metriky, které bude shromážděna a nahrát.
* Umožňuje uživatelům tooupload protokol příslušné soubory tooa určené úložiště tabulky.

V aktuální verzi hello 2.3 hello data zahrnují:

* Všechny Linux Rsyslog protokoly, včetně systému, zabezpečení a protokoly aplikací.
* Všechna data systému, které je zadáno v [hello řešení System Center křížové platformy lokality](https://scx.codeplex.com/wikipage?title=xplatproviders).
* Soubory protokolu definované uživatelem.

Toto rozšíření funguje s hello classic a modelech nasazení Resource Manager.

### <a name="current-version-of-hello-extension-and-deprecation-of-old-versions"></a>Aktuální verze hello rozšíření a vyřazení staré verze

nejnovější verze rozšíření hello Hello je **2.3**, a **všechny starší verze (2.0, 2.1 a 2.2) se již nepoužívá a Nepublikováno konce tohoto roku (2017)**. Pokud jste nainstalovali hello rozšíření diagnostiky Linux automatické podverze upgradu zakázána, doporučujeme odinstalovat rozšíření hello a znovu ji nainstalujte automatické podverze upgradu povoleno. Na klasické virtuální počítače (ASM) můžete tím dosáhnout zadáním '2.*' jako verze hello, pokud instalujete rozšíření hello prostřednictvím příkazového řádku Azure XPLAT nebo Powershellu. Na virtuálních počítačů ARM, lze dosáhnout zahrnutím ' "autoUpgradeMinorVersion": true, v hello šablony nasazení virtuálního počítače. Všechny nové instalace rozšíření hello navíc by měl mít vedlejší verze aktualizace hello automatického upgradu zapnuta možnost.

## <a name="enable-hello-extension"></a>Povolit rozšíření hello

Toto rozšíření můžete povolit pomocí hello [portál Azure](https://portal.azure.com/#), prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure skripty.

tooview a konfigurovat systém hello a údaje o výkonu přímo z hello portálu Azure, postupujte podle [na hello Azure blog tyto kroky](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).

Tento článek se zaměřuje na tooenable a rozšíření hello nakonfigurovat pomocí rozhraní příkazového řádku Azure. To vám umožní tooread a zobrazení hello data přímo z tabulky úložiště hello.

Všimněte si, že hello konfigurace metody, které jsou zde popsány nebudou fungovat pro hello portálu Azure. tooview a nakonfigurovat hello výkon systému a data přímo z hello portálu Azure, musí být povolené rozšíření hello prostřednictvím portálu hello.

## <a name="prerequisites"></a>Požadavky

* **Azure Linux Agent verze 2.0.6 nebo novější**.

  Všimněte si, že většina Galerie Image virtuálních počítačů Linux Azure zahrnují verze 2.0.6 nebo novější. Můžete spustit **příkaz WAAgent-verze** tooconfirm, která verze je nainstalovaná na hello virtuálních počítačů. Pokud hello virtuální počítač běží na verzi, která je starší než 2.0.6, můžete podle [tyto pokyny na Githubu](https://github.com/Azure/WALinuxAgent "pokyny") tooupdate ho.
* **Azure CLI**. Postupujte podle [tyto pokyny pro instalaci rozhraní příkazového řádku](../../../cli-install-nodejs.md) tooset prostředí příkazového řádku Azure CLI hello na váš počítač. Po instalaci rozhraní příkazového řádku Azure, můžete použít hello **azure** příkaz vaše rozhraní příkazového řádku (Bash, Terminálové nebo příkazového řádku) tooaccess hello rozhraní příkazového řádku Azure. Například:

  * Spustit **sadu rozšíření virtuálního počítače azure – Nápověda** podrobnou nápovědu informace.
  * Spustit **přihlášení k azure** toosign v tooAzure.
  * Spustit **seznamu virtuálních počítačů azure** toolist všechny hello virtuálních počítačů, které máte v Azure.
* Úložiště toostore hello data účtu. Budete potřebovat název účtu úložiště, který byl vytvořen dříve a k přístupu klíče tooupload hello data tooyour úložiště.

## <a name="use-hello-azure-cli-command-tooenable-hello-linux-diagnostic-extension"></a>Použít hello rozhraní příkazového řádku Azure příkaz tooenable hello Linux rozšíření diagnostiky

### <a name="scenario-1-enable-hello-extension-with-hello-default-data-set"></a>Scénář 1. Povolit rozšíření hello s hello výchozí datové sady

Ve verzi 2.3 nebo novější hello výchozí data, která se budou shromažďovat zahrnuje:

* Všechny informace Rsyslog (včetně systému, zabezpečení a protokoly aplikací).  
* Základní sady dat základ systému. Všimněte si, že hello úplné datové sady je popsáno na hello [řešení System Center křížové platformy lokality](https://scx.codeplex.com/wikipage?title=xplatproviders).
  Pokud chcete, aby tooenable doplňující data, pokračujte kroky hello ve scénářích 2 a 3.

Krok 1. Vytvořte soubor s názvem PrivateConfig.json s hello následující obsah:

    {
        "storageAccountName" : "hello storage account tooreceive data",
        "storageAccountKey" : "hello key of hello account"
    }

Krok 2. Spustit  **rozšíření virtuálního počítače azure nastavit vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* – privátní config-path PrivateConfig.json**.

### <a name="scenario-2-customize-hello-performance-monitor-metrics"></a>Scénář 2. Přizpůsobení metriky monitorování výkonu hello

Tato část popisuje, jak toocustomize hello výkon a diagnostických dat tabulky.

Krok 1. Vytvořte soubor s názvem PrivateConfig.json s hello obsah, který je popsáno ve scénáři 1. Vytvořte také soubor s názvem PublicConfig.json. Zadejte hello konkrétní data, která chcete toocollect.

Pro všechny podporované zprostředkovatele a proměnné odkazují hello [řešení System Center křížové platformy lokality](https://scx.codeplex.com/wikipage?title=xplatproviders). Můžete mít více dotazů a uložit je do více tabulek přidáním další dotazy toohello skriptu.

Ve výchozím nastavení je vždy shromažďují hello Rsyslog data.

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


Krok 2. Spustit  **rozšíření virtuálního počítače azure nastavit vm_name LinuxDiagnostic Microsoft.OSTCExtensions: 2.*' – privátní config-path PrivateConfig.json – veřejné config-path PublicConfig.json**.

### <a name="scenario-3-upload-your-own-log-files"></a>Scénář 3. Odeslání souborů protokolu

Tato část popisuje, jak toocollect a nahrání konkrétní protokolové soubory tooyour účet úložiště. Je nutné toospecify obou hello tooyour protokolu souborů a hello název cesty hello tabulky, kde se má toostore protokolu. Přidáním více souborů nebo tabulky položky toohello skriptu můžete vytvořit více souborů protokolu.

Krok 1. Vytvořte soubor s názvem PrivateConfig.json s hello obsah, který je popsáno ve scénáři 1. Pak vytvořte jiný soubor s názvem PublicConfig.json s hello následující obsah:

```json
{
    "fileCfg" :
    [
        {
            "file" : "/var/log/mysql.err",
            "table" : "mysqlerr"
            }
    ]
}
```

Krok 2. Spusťte `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.

Všimněte si, že s tímto nastavením na hello rozšíření verze předchozí too2.3, všechny protokoly zapisují příliš`/var/log/mysql.err` může být příliš duplikován`/var/log/syslog` (nebo `/var/log/messages` v závislosti na hello Linux distro) také. Pokud chcete tooavoid tento duplicitní protokolování, můžete je vyloučit protokolování `local6` protokolů zařízení ve vaší konfiguraci rsyslog. Závisí na hello Linux distro, ale u systému Ubuntu 14.04, je soubor toomodify hello `/etc/rsyslog.d/50-default.conf` a můžete nahradit hello řádku `*.*;auth,authpriv.none -/var/log/syslog` příliš`*.*;auth,authpriv,local6.none -/var/log/syslog`. Tento problém vyřešen v hello nejnovější opravy hotfix verze 2.3 (2.3.9007), takže pokud máte rozšíření hello verze 2.3, tento problém došlo k neočekávané chybě. Pokud k tomu ještě i po restartování virtuálního počítače, kontaktujte nás a Pomozte nám Poradce při potížích se hello nejnovější opravy hotfix není automaticky instalován.

### <a name="scenario-4-stop-hello-extension-from-collecting-any-logs"></a>Scénář 4. Zastavit shromažďování žádné protokoly rozšíření hello

Tato část popisuje, jak rozšíření hello toostop z shromažďování protokolů. Všimněte si, že hello monitorování procesu agenta bude dál spuštěný a funkční i přes tuto změnu konfigurace. Pokud chcete monitorování procesu agenta zcela hello toostop, můžete tak učinit zakázáním rozšíření hello. Hello příkaz toodisable hello rozšíření je `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.

Krok 1. Vytvořte soubor s názvem PrivateConfig.json s hello obsah, který je popsáno ve scénáři 1. Vytvořte jiný soubor s názvem PublicConfig.json s hello následující obsah:

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


Krok 2. Spustit  **rozšíření virtuálního počítače azure nastavit vm_name LinuxDiagnostic Microsoft.OSTCExtensions: 2.*' – privátní config-path PrivateConfig.json – veřejné config-path PublicConfig.json**.

## <a name="review-your-data"></a>Zkontrolujte vaše data

Hello výkonu a diagnostických dat jsou uložené v tabulce Azure Storage. Zkontrolujte [jak toouse Azure Table Storage z Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) toolearn jak tooaccess hello dat v úložišti hello tabulky pomocí rozhraní příkazového řádku Azure skriptů.

Kromě toho můžete použít následující uživatelské rozhraní nástroje tooaccess hello dat:

1. Průzkumníka serveru Visual Studio. Přejděte tooyour účet úložiště. Po dobu asi 5 minut spuštění hello virtuálních počítačů, uvidíte hello čtyři výchozí tabulky: "LinuxCpu", "LinuxDisk", "LinuxMemory" a "Linuxsyslog". Dvakrát klikněte na hello tabulky názvy tooview hello data.
1. [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").

![Bitové kopie](./media/diagnostic-extension/no1.png)

Pokud jste povolili fileCfg nebo perfCfg (jak je popsáno v scénáře 2 a 3), můžete použít Průzkumníka serveru Visual Studia a Azure Storage Explorer tooview jiné než výchozí data.

## <a name="known-issues"></a>Známé problémy

* Hello Rsyslog informace a zákazník zadaný soubor protokolu lze přistupovat pouze pomocí skriptů.
