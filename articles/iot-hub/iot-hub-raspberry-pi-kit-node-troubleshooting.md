---
title: "Connect Raspberry PI (C) tooAzure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro prostředí malin pí Node.js hello"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "problémy s IOT, internet věcí problémů"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 22cf50dc-8206-42a2-a1fc-f75fa85135fa
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8f0807104550e8e53a132f7741564b33f1db17ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Řešení potíží
## <a name="hardware-issues"></a>Problémy s hardwarem
### <a name="hello-application-runs-well-but-hello-led-is-not-blinking"></a>aplikace Hello spustí i ale hello DIODU není blikat
Tento problém je vždy související toohardware okruhu připojení. Použijte následující kroky tooidentify problémy hello:

1. Zkontrolujte, že jste vybrali správný hello **GPIO** vaší karty. Hello dva porty by měl být **GPIO zem (Pin 6)** a **GPIO 04 (Pin 7)**.
2. Zkontrolujte správnost hello polarita z vaší Indikátor. Hello delší větev by měl být uveden hello **kladné**, anod pin.
3. Použití hello **3.3v připnout** a **zem Pin** na malin pí 3. Pi považovat za hello power řadiče domény. Zkontrolujte, zda že tento hello DIODU funguje bez problémů.

![Specifikace DIODU](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Další potíže s hardwarem
Informace o řešení běžných problémů na 3 malin platformy najdete v tématu hello [oficiální stránka řešení potíží](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Problémy balíčku Node.js
### <a name="no-response-during-gulp-tasks"></a>Žádná odpověď během gulp úlohy
Pokud narazíte na problémy ve spuštěné úkoly gulp, můžete přidat hello `--verbose` možnost pro ladění. Zkuste tooterminate aktuální gulp úlohy pomocí kombinace kláves Ctrl + C a poté spusťte následující příkaz v vaše zprávy ladění toosee okna konzoly hello. Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problémy zjišťování zařízení
Pomoc při řešení běžných potíží s hello `devdisco` příkaz, zkontrolujte hello [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="npm-issues"></a>npm problémy
Zkuste tooupdate vašeho balíčku npm pomocí hello následující příkaz:

```bash
npm install -g npm
```

Pokud problém hello stále existuje, ponechte komentáře na konci hello tohoto článku nebo vytvořit problém Githubu v našem [úložiště ukázkové](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).

## <a name="remote-debugging"></a>Vzdálené ladění
### <a name="run-hello-sample-application-in-debug-mode"></a>Spuštění ukázkové aplikace hello v režimu ladění
```bash
gulp run --debug
```

Když modul ladění hello je připraven, měli byste vidět ```Debugger listening on port 5858``` ve výstupu konzoly hello.

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a>Konfigurace Visual Studio Code tooconnect toohello vzdáleném zařízení
1. Otevřete hello **ladění** panely na levé straně hello.
2. Klikněte na zelenou hello **spustit ladění** tlačítko (F5). Visual Studio Code otevře soubor launch.json.
3. Aktualizujte soubor launch.json hello hello následující obsah. Nahraďte `[device hostname or IP address]` s hello skutečné zařízení IP adresu nebo název hostitele.

> [!NOTE]
> Další informace o ladění Visual Studio, hello toolearn naleznete příliš[ladění ve Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).


```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Konfigurace vzdáleného ladění](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-toohello-remote-application"></a>Připojte toohello vzdálené aplikace
Klikněte na zelenou hello **spustit ladění** ladění toostart tlačítko (F5).

Čtení [JavaScript v produktu VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn více informací o hello ladicí program.

![Vzdálené ladění interaktivní](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure CLI problémy
Hello rozhraní příkazového řádku Azure (Azure CLI) je buildu preview. tooseek řešení, můžete použít hello [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Pokud narazíte na všechny chyby nástrojem hello, soubor [problém](https://github.com/Azure/azure-cli/issues) v hello **problémy** část úložiště GitHub hello.

Pomoc při řešení běžných potíží, zkontrolujte hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Problémy instalace Python
### <a name="legacy-installation-issues-macos"></a>Problémy instalace starší verze (macOS)
Když instalujete pip, oprávnění chyba se vyvolá, když starší balíčky jsou nainstalované s **su** oprávnění. K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována. Některé balíčky pip z předchozí instalace byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění hello. Hello řešení je tooremove tyto balíčky nainstalované pomocí kořenového. Tento úkol použijte následující postup toocomplete hello:

1. Přejděte na: /usr/local/lib/python2.7/site-packages
2. Seznam balíčky vytvořené pomocí kořenového:`ls -l | grep root`
3. Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`
4. Přeinstalujte Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT Hub problémy
Pokud jste úspěšně zřídit služby Azure IoT hub pomocí rozhraní příkazového řádku Azure, a je nutné zařízení hello toomanage nástroj, kteří se připojují tooyour IoT hub, zkuste hello následující nástroje.

### <a name="device-explorer"></a>Průzkumník zařízení
Hello [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nástroj spouští na místní počítač se systémem Windows a připojí tooyour IoT hub v Azure. Komunikuje s následující hello [koncové body centra IoT](iot-hub-devguide.md):


* *Správa identit zařízení* tooprovision a spravovat zařízení zaregistrovaná službou IoT hub.
* *Zobrazí zařízení cloud* , můžete monitorovat zprávy odeslané ze zařízení služby IoT hub tooyour.
* *Odeslat cloud zařízení* tak zprávy lze odesílat tooyour zařízení ze služby IoT hub.

Konfigurace připojovacího řetězce centra IoT v rámci této toouse nástroj všechny jeho možnosti.

### <a name="iothub-explorer"></a>iothub-explorer
[iothub-explorer](https://github.com/Azure/iothub-explorer) je nástroj příkazového řádku s více platformami ukázkové toomanage zařízení. Můžete používat hello nástroj toomanage hello zařízení v registru identit hello, sledování zpráv typu zařízení cloud a odesílání zpráv typu cloud zařízení.

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

## <a name="azure-storage-issues"></a>Azure problémů s úložištěm
[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od Microsoftu, které můžete toowork s daty Azure Storage ve Windows, systému macOS a Linux. Pomocí tohoto nástroje můžete připojit tooyour tabulky a zobrazit data hello v ní. Můžete použít tento nástroj tootroubleshoot vaše problémů s úložištěm Azure.

