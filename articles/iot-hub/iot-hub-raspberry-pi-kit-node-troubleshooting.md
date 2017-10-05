---
title: "Malinová Pi (C) připojit k Azure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro prostředí malin pí Node.js"
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
ms.openlocfilehash: f9058068972ddbb674d3cd159948835dc88c4451
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a>Řešení potíží
## <a name="hardware-issues"></a>Problémy s hardwarem
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Aplikace běží správně, ale není blikat indikátor LED
Tento problém je vždy související s konektivitou okruhu hardwaru. K identifikaci problémů použijte následující kroky:

1. Zkontrolujte, že jste vybrali správný **GPIO** vaší karty. Dva porty musí být **GPIO zem (Pin 6)** a **GPIO 04 (Pin 7)**.
2. Zkontrolujte správnost polarita z vaší Indikátor. Delší větev by měl být uveden **kladné**, anod pin.
3. Použití **3.3v připnout** a **zem Pin** na malin pí 3. Pi považovat za power řadiče domény. Zkontrolujte, jestli DIODU funguje bez problémů.

![Specifikace DIODU](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Další potíže s hardwarem
Informace o řešení běžných problémů na 3 malin platformy najdete v tématu [oficiální stránka řešení potíží](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Problémy balíčku Node.js
### <a name="no-response-during-gulp-tasks"></a>Žádná odpověď během gulp úlohy
Pokud narazíte na problémy ve spuštěné úkoly gulp, můžete přidat `--verbose` možnost pro ladění. Zkuste ukončit aktuální gulp úlohy pomocí kombinace kláves Ctrl + C, a poté spusťte následující příkaz v okně konzoly zobrazíte zprávy ladění. Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problémy zjišťování zařízení
Pomoc při řešení běžných potíží s `devdisco` příkazu, zkontrolujte [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="npm-issues"></a>npm problémy
Zkuste aktualizovat vašeho balíčku npm pomocí následujícího příkazu:

```bash
npm install -g npm
```

Pokud problém přetrvává, nechte komentáře na konci tohoto článku nebo vytvořte potíže Githubu v našem [úložiště ukázkové](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).

## <a name="remote-debugging"></a>Vzdálené ladění
### <a name="run-the-sample-application-in-debug-mode"></a>Spuštění ukázkové aplikace v režimu ladění
```bash
gulp run --debug
```

Když modul ladění je připraven, měli byste vidět ```Debugger listening on port 5858``` ve výstupu konzoly.

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a>Konfigurace Visual Studio Code pro připojení k vzdálené zařízení
1. Otevřete **ladění** panely na levé straně.
2. Klikněte na tlačítko se zeleným **spustit ladění** tlačítko (F5). Visual Studio Code otevře soubor launch.json.
3. Aktualizujte soubor launch.json s následujícím obsahem. Nahraďte `[device hostname or IP address]` s skutečné zařízení IP adresu nebo název hostitele.

> [!NOTE]
> Můžete najít další informace o Visual Studio, ladění, [ladění ve Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).


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

### <a name="attach-to-the-remote-application"></a>Připojení k vzdálené aplikaci
Klikněte na tlačítko se zeleným **spustit ladění** (F5) tlačítko Spustit ladění.

Čtení [JavaScript v produktu VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) Další informace o ladicího programu.

![Vzdálené ladění interaktivní](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure CLI problémy
Rozhraní příkazového řádku Azure (Azure CLI) je buildu preview. K vyhledání řešení, můžete použít [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Pokud narazíte na všechny chyby pomocí nástroje souboru [problém](https://github.com/Azure/azure-cli/issues) v **problémy** část úložiště GitHub.

Pomoc při řešení běžných potíží, najdete [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Problémy instalace Python
### <a name="legacy-installation-issues-macos"></a>Problémy instalace starší verze (macOS)
Když instalujete pip, oprávnění chyba se vyvolá, když starší balíčky jsou nainstalované s **su** oprávnění. K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována. Některé balíčky pip z předchozí instalace byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění. Řešení je odebrat tyto balíčky nainstalované pomocí kořenového. Tuto úlohu dokončit pomocí následujících kroků:

1. Přejděte na: /usr/local/lib/python2.7/site-packages
2. Seznam balíčky vytvořené pomocí kořenového:`ls -l | grep root`
3. Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`
4. Přeinstalujte Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT Hub problémy
Pokud jste úspěšně zřídit služby Azure IoT hub pomocí rozhraní příkazového řádku Azure, musíte nástroj pro správu zařízení, které se připojují ke službě IoT hub, zkuste následující nástroje.

### <a name="device-explorer"></a>Průzkumník zařízení
[Explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nástroj spouští na místní počítač se systémem Windows a připojí se ke službě IoT hub v Azure. Komunikuje s následující [koncové body centra IoT](iot-hub-devguide.md):


* *Správa identit zařízení* zřizovat a spravovat zařízení zaregistrovaná službou IoT hub.
* *Zobrazí zařízení cloud* , můžete monitorovat zprávy odeslané ze zařízení do služby IoT hub.
* *Odeslat cloud zařízení* tak mohou zasílat zprávy do zařízení ze služby IoT hub.

Konfigurace připojovacího řetězce IoT hub v rámci tohoto nástroje můžete použít všechny jeho možnosti.

### <a name="iothub-explorer"></a>iothub-explorer
[iothub-explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku ke správě zařízení. Nástroj můžete použít ke správě zařízení v registru identit, sledování zpráv typu zařízení cloud a odesílání zpráv typu cloud zařízení.

Chcete-li nainstalovat nejnovější verzi (předprodejní) nástroj iothub Průzkumník, spusťte následující příkaz v prostředí příkazového řádku:

```bash
npm install -g iothub-explorer@latest
```

Chcete-li získat další informace o tom všechny příkazy iothub-explorer a jejich parametrů můžete následující příkaz:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>portál Azure
Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure. Můžete také použít [portál Azure](../azure-portal-overview.md) pomoci zřizování, spravovat a ladit vašich prostředků Azure.

## <a name="azure-storage-issues"></a>Azure problémů s úložištěm
[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od společnosti Microsoft, který můžete použít pro práci s daty Azure Storage ve Windows, systému macOS a Linux. Pomocí tohoto nástroje můžete připojit k tabulku a zobrazit data v ní. Tento nástroj slouží k řešení potíží vašeho úložiště Azure.

