---
title: "Connect Raspberry PI (C) tooAzure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro Node.js pí malin prostředí"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "problémy s IOT, internet věcí problémů"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 3653c79b-8a0c-41d4-b0bf-f6b4edb4d233
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4f1ea81dd25d10a39c2939f5ee5f19f6d2ba2b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Řešení potíží
## <a name="hardware-issues"></a>Problémy s hardwarem
### <a name="hello-application-runs-well-but-hello-led-is-not-blinking"></a>aplikace Hello spustí i ale hello DIODU není blikat
Tento problém je vždy související toohello hardwaru okruh připojení. Použijte následující kroky tooidentify problémy hello.

1. Zkontrolujte, že jste vybrali správný hello **GPIO** vaší karty. Hello dva porty by měl být **GPIO zem (Pin 6)** a **GPIO 04 (Pin 7)**.
2. Zkontrolujte správnost hello polarita z vaší Indikátor. Hello delší větev by měl být uveden hello **kladné**, anod pin.
3. Použití hello **3.3v připnout** a **zem Pin** na malin pí 3. Pi považovat za hello power řadiče domény. Zkontrolujte, zda že tento hello DIODU funguje bez problémů.

![Specifikace DIODU](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Další potíže s hardwarem
Informace o řešení běžných problémů na 3 malin platformy najdete v tématu hello [oficiální stránka řešení potíží](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Problémy balíčku Node.js
### <a name="no-response-during-gulp-tasks"></a>Žádná odpověď během gulp úlohy
Pokud narazíte na potíže se spouštěním gulp úlohy, můžete přidat hello `--verbose` možnost pro ladění. Zkuste tooterminate aktuální gulp úlohy pomocí `Ctrl + C`, a pak spusťte hello následující příkaz v vaše zprávy ladění toosee okna konzoly. Může se zobrazit podrobné chybové zprávy ve výstupu konzoly. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problémy zjišťování zařízení
Pomoc při řešení běžných potíží s hello `devdisco` příkaz, zkontrolujte hello [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="npm-issues"></a>NPM problémy
Zkuste tooupdate vašeho balíčku NPM s hello následující příkaz:

```bash
npm install -g npm
```

Pokud problém hello stále existuje, ponechte komentáře na konci hello tohoto článku nebo vytvořit problém Githubu v našem [Ukázka úložiště](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)

## <a name="remote-debugging"></a>Vzdálené ladění

Vzdálené ladění podpora bude brzy k dispozici v rozšíření kódu C/C++ sady Visual Studio k dispozici.

V současně můžete GDB pomocí Oblíbené terminálu SSH:

```bash
cd c-pi-lesson-x
sudo gdb app
```

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
[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí tooyour IoT hub v Azure. Komunikuje s následující hello [koncové body centra IoT](iot-hub-devguide.md):

* *Správa identit zařízení* tooprovision a spravovat zařízení zaregistrovaná službou IoT hub.
* *Zobrazí zařízení cloud* , můžete monitorovat zprávy odeslané ze zařízení služby IoT hub tooyour.
* *Odeslat cloud zařízení* tak zprávy lze odesílat tooyour zařízení ze služby IoT hub.

Konfigurace vaší `IoT hub connection string` v rámci této nástroj toouse všechny jeho možnosti.

### <a name="iot-hub-explorer"></a>Centrum IoT Explorer
[Centrum IoT Explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku toomanage klientů zařízení. Můžete používat hello nástroj toomanage hello zařízení v registru identit hello, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.

tooinstall hello poslední (předprodejní) verzi nástroje iothub-explorer hello, spusťte následující příkaz v prostředí příkazového řádku hello:

```
npm install -g iothub-explorer@latest
```

Můžete použít následující příkaz, že tooget další nápovědu o všech hello iothub-explorer spolu s jejich parametry hello:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>portál Azure
Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure. Můžete také toouse hello [portál Azure](../azure-portal-overview.md) toohelp zřizovat, spravovat a ladit vašich prostředků Azure.

## <a name="azure-storage-issues"></a>Problémů s úložištěm Azure
[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od Microsoftu, které můžete toowork s daty Azure Storage ve Windows, systému macOS a Linux. Pomocí tohoto nástroje můžete připojit tooyour tabulky a zobrazit data hello v ní. Můžete použít tento nástroj tootroubleshoot vaše problémů s úložištěm Azure.
