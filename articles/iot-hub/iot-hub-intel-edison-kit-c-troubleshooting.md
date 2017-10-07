---
title: "Connect Intel Edison (C) tooAzure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro prostředí Intel Edison C"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "řešení potíží s arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: fe20f2fe-490c-4910-82e1-578ed504ae86
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c4a71983e3906cfc5ce7c832cf858852b9bd744a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Řešení potíží
## <a name="hardware-issues"></a>Problémy s hardwarem
Informace o řešení běžných problémů na Intel Edison najdete v tématu hello [oficiální stránka řešení potíží](https://software.intel.com/en-us/node/637974).

## <a name="nodejs-package-issues"></a>Problémy balíčku Node.js
### <a name="no-response-during-gulp-tasks"></a>Žádná odpověď během gulp úlohy
Pokud narazíte na potíže se spouštěním gulp úlohy, můžete přidat hello `--verbose` možnost pro ladění. Zkuste tooterminate aktuální gulp úlohy pomocí `Ctrl + C`, a pak spusťte hello následující příkaz v vaše zprávy ladění toosee okna konzoly. Může se zobrazit podrobné chybové zprávy ve výstupu konzoly. 

```bash
gulp --verbose
```

### <a name="npm-issues"></a>NPM problémy
Zkuste tooupdate vašeho balíčku NPM s hello následující příkaz:

```bash
npm install -g npm
```

Pokud problém hello stále existuje, ponechte komentáře na konci hello tohoto článku nebo vytvořit problém Githubu v našem [úložiště ukázkové][sample-repository].

## <a name="azure-cli-issues"></a>Problémy rozhraní příkazového řádku Azure
Hello rozhraní příkazového řádku Azure (Azure CLI) je buildu preview. Vyhledejte řešení v hello [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek řešení. Zkuste tooupgrade rozhraní příkazového řádku Azure toolatest verze při příkazy nefungují podle očekávání.

Pokud narazíte na všechny chyby nástrojem hello, soubor [problém](https://github.com/Azure/azure-cli/issues) v hello **problémy** část úložiště GitHub hello.

Nápovědu k řešení běžných potíží s zkontrolujte hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

Pokud jsou splněny "Nelze najít na verzi, která by splnila požadavek hello", prosím hello spusťte následující příkaz tooupgrade pip toolastest verze.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Problémy instalace Python
### <a name="legacy-installation-issues-macos"></a>Problémy instalace starší verze (macOS)
Když instalujete **pip**, oprávnění chyba se vyvolá, když starší balíčky, které jsou nainstalovány s **su** oprávnění. K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována. Některé **pip** balíčky z předchozí instalace, které byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění hello. Hello řešení je tooremove tyto balíčky nainstalované pomocí kořenového. Tento úkol použijte následující postup toocomplete hello:

1. Přejděte na: /usr/local/lib/python2.7/site-packages
2. Seznam balíčků vytvořit pomocí kořenového:`ls -l | grep root`
3. Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`
4. Přeinstalujte Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT Hub problémy
Pokud jste úspěšně zřídit služby Azure IoT hub s `azure-cli`, a je třeba zařízení hello toomanage nástroj, kteří se připojují tooyour IoT hub, zkuste hello následující nástroje:

### <a name="device-explorer"></a>Průzkumník zařízení
[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí tooyour IoT hub v Azure. Komunikuje s následující hello [koncové body centra IoT](iot-hub-devguide.md):

- _Správa identit zařízení_ tooprovision a spravovat zařízení zaregistrovaná službou IoT hub.
- _Zobrazí zařízení cloud_ , můžete monitorovat zprávy odeslané ze zařízení služby IoT hub tooyour.
- _Odeslat cloud zařízení_ tak zprávy lze odesílat tooyour zařízení ze služby IoT hub.

Konfigurace vaší `IoT hub connection string` v rámci této nástroj toouse všechny jeho možnosti.

### <a name="iot-hub-explorer"></a>Centrum IoT Explorer
[Centrum IoT Explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku toomanage klientů zařízení. Můžete používat hello nástroj toomanage hello zařízení v registru identit hello, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.

tooinstall hello poslední (předprodejní) verzi nástroje iothub-explorer hello, spusťte následující příkaz v prostředí příkazového řádku hello:

```bash
npm install -g iothub-explorer@latest
```

Můžete použít následující příkaz, že tooget další nápovědu o všech hello iothub-explorer spolu s jejich parametry hello:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>portál Azure
Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure. Můžete také toouse hello [portál Azure](../azure-portal-overview.md) toohelp zřizovat, spravovat a ladit vašich prostředků Azure.

## <a name="azure-storage-issues"></a>Problémů s úložištěm Azure
[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od společnosti Microsoft, který můžete použít toowork s [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) dat v systému Windows, systému macOS a Linux. Pomocí tohoto nástroje můžete připojit tooyour tabulky a zobrazit data hello v ní. Můžete použít tento nástroj tootroubleshoot vaše problémů s úložištěm Azure.

## <a name="next-steps"></a>Další kroky
Tato stránka obsahuje pouze hello nejběžnější problémy Intel Edison Kit. Můžete také ponechat dolní komentáře tooreport problémy pro odstraňování potíží.

Přejděte zpět příliš[začít pracovat s Intel Edison (C)](iot-hub-intel-edison-kit-c-get-started.md)

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-c-edison-getting-started