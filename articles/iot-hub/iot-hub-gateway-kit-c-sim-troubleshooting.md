---
title: "Simulované zařízení & brány Azure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro bránu Intel NUC"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "problémy s IOT, internet věcí problémů"
ROBOTS: NOINDEX
ms.assetid: 3ee8f4b0-5799-40a3-8cf0-8d5aa44dbc2b
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: cc7add6273e66aadeca9a4915a44f5edf61a0a59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Řešení potíží

## <a name="hardware-issues"></a>Problémy s hardwarem

### <a name="ti-sensortag-cannot-be-connected"></a>Nelze se připojit TI SensorTag

problémy s připojením SensorTag tootroubleshoot, použijte hello [SensorTag aplikace](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).

### <a name="have-an-issue-with-intel-nuc"></a>Jít o problém s Intel NUC

tootroubleshoot spouštěcí problémy, najdete v příliš[řešení potíží s žádný spouštěcí na Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).

problémy s operačním systémem tootroubleshoot, najdete v příliš[řešení potíží s operačního systému na Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).

tootroubleshoot další problémy, najdete v příliš[Blink kódy a zvukový signál kódy pro Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).

## <a name="nodejs-package-issues"></a>Problémy balíčku Node.js

### <a name="no-response-during-gulp-tasks"></a>Žádná odpověď během gulp úlohy

Pokud narazíte na problémy ve spuštěné úkoly gulp, můžete přidat hello `--verbose` možnost pro ladění. Zkuste tooterminate aktuální gulp úlohy pomocí `Ctrl + C`, a pak spusťte hello následující příkaz v vaše zprávy ladění toosee okna konzoly. Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problémy zjišťování zařízení

Pomoc při řešení běžných potíží s hello `discover-sensortag` příkaz, zkontrolujte hello [stránce wikiwebu](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).

### <a name="npm-issues"></a>npm problémy

Zkuste tooupdate vašeho balíčku npm spuštěním hello následující příkaz:

```bash
npm install -g npm
```

Pokud problém hello stále existuje, ponechte komentáře na konci hello tohoto článku nebo vytvořit problém Githubu v našem [úložiště ukázkové](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).

## <a name="remote-debugging"></a>Vzdálené ladění
> Následující pokyny jsou určené pro ladění node.js skripty použité v tomto kurzu.
### <a name="run-hello-sample-application-in-debug-mode"></a>Spuštění ukázkové aplikace hello v režimu ladění

Spusťte hello ukázkovou aplikaci v režimu ladění hello následující příkaz:

```bash
gulp run --debug
```

Když modul ladění hello je připraven, měli byste vidět `Debugger listening on port 5858` ve výstupu konzoly hello.

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a>Konfigurace Visual Studio Code tooconnect toohello vzdáleném zařízení

1. Otevřete hello **ladění** panely na levé straně hello.
2. Klikněte na zelenou hello **spustit ladění** tlačítko (F5). Otevře se Visual Studio Code `launch.json` souboru.
3. Aktualizace hello `launch.json` soubor s hello následující obsah. Nahraďte `[device hostname or IP address]` s hello skutečné zařízení IP adresu nebo název hostitele.

   ``` json
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
            "remoteRoot": "~/ble_sample"
        }
     ]
   }
   ```

![Konfigurace vzdáleného ladění](./media/iot-hub-gateway-kit-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-toohello-remote-application"></a>Připojte toohello vzdálené aplikace

Klikněte na zelenou hello **spustit ladění** ladění toostart tlačítko (F5).

Čtení [JavaScript v produktu VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn více informací o hello ladicí program.

![Ukázka zakázat ladění](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a>Azure CLI problémy

Hello rozhraní příkazového řádku Azure (Azure CLI) je buildu preview. tooseek řešení, můžete použít hello [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Pokud narazíte na všechny chyby nástrojem hello, soubor [problém](https://github.com/Azure/azure-cli/issues) v hello **problémy** část úložiště GitHub hello.

Pomoc při řešení běžných potíží, zkontrolujte hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

Pokud jsou splněny "Nelze najít na verzi, která by splnila požadavek hello", prosím hello spusťte následující příkaz tooupgrade pip toolastest verze.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Problémy instalace Python

### <a name="legacy-installation-issues-macos"></a>Problémy instalace starší verze (macOS)

Když instalujete pip, oprávnění chyba se vyvolá, když starší balíčky jsou nainstalované s **su** oprávnění. K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována. Některé balíčky pip z předchozí instalace byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění hello. Hello řešení je tooremove tyto balíčky nainstalované pomocí kořenového. Tento úkol použijte následující postup toocomplete hello:

1. Přejděte příliš`/usr/local/lib/python2.7/site-packages`
2. Seznam balíčky vytvořené pomocí kořenového:`ls -l | grep root`
3. Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`
4. Přeinstalujte Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT Hub problémy

Pokud jste úspěšně zřídit služby Azure IoT hub s hello rozhraní příkazového řádku Azure, a je nutné zařízení hello toomanage nástroj, kteří se připojují tooyour IoT hub, zkuste hello následující nástroje.

### <a name="device-explorer"></a>Průzkumník zařízení

[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí tooyour IoT hub v Azure. Komunikuje s následující hello [koncové body centra IoT](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):

- Tooprovision správy identity zařízení a spravovat zařízení zaregistrovaná službou IoT hub.
- Přijímat zařízení cloud, takže můžete sledovat zprávy odeslané ze zařízení služby IoT hub tooyour.
- Odesláno cloud zařízení, můžete mohou zasílat zprávy tooyour zařízení ze služby IoT hub.

Konfigurace připojovacího řetězce centra IoT v rámci této toouse nástroj všechny jeho možnosti.

### <a name="iothub-explorer"></a>iothub-explorer

[iothub-explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku toomanage klientů zařízení. Můžete používat hello nástroj toomanage hello zařízení v registru identit hello, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.

tooinstall hello poslední (předprodejní) verzi nástroje hello iothub-explorer, spusťte následující příkaz hello:

```bash
npm install -g iothub-explorer@latest
```

Další nápovědu tooget o všech hello iothub-explorer spolu s jejich parametry, spusťte následující příkaz hello:

```bash
iothub-explorer help
```

### <a name="hello-azure-portal"></a>Hello portálu Azure

Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure. Můžete také toouse hello [portál Azure](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) toohelp zřizovat, spravovat a ladit vašich prostředků Azure.

## <a name="azure-storage-issues"></a>Azure problémů s úložištěm

[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com/) je samostatná aplikace od Microsoftu, které můžete toowork s daty Azure Storage ve Windows, systému macOS a Linux. Pomocí tohoto nástroje můžete připojit tooyour tabulky a zobrazit data hello v ní. Můžete použít tento nástroj tootroubleshoot vaše problémů s úložištěm Azure.
