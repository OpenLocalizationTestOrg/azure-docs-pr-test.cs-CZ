---
title: "Malinová Pi (C) připojit k Azure IoT – řešení potíží s | Microsoft Docs"
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
ms.openlocfilehash: 828669db23fa8d608029134fbe364033456d935a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a>Řešení potíží
## <a name="hardware-issues"></a>Problémy s hardwarem
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Aplikace běží správně, ale není blikat indikátor LED
Tento problém je vždy související s konektivitou okruhu hardwaru. Identifikaci problémů použijte následující postup.

1. Zkontrolujte, že jste vybrali správný **GPIO** vaší karty. Dva porty musí být **GPIO zem (Pin 6)** a **GPIO 04 (Pin 7)**.
2. Zkontrolujte správnost polarita z vaší Indikátor. Delší větev by měl být uveden **kladné**, anod pin.
3. Použití **3.3v připnout** a **zem Pin** na malin pí 3. Pi považovat za power řadiče domény. Zkontrolujte, jestli DIODU funguje bez problémů.

![Specifikace DIODU](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Další potíže s hardwarem
Informace o řešení běžných problémů na 3 malin platformy najdete v tématu [oficiální stránka řešení potíží](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Problémy balíčku Node.js
### <a name="no-response-during-gulp-tasks"></a>Žádná odpověď během gulp úlohy
Pokud narazíte na potíže se spouštěním gulp úlohy, můžete přidat `--verbose` možnost pro ladění. Zkuste ukončit aktuální úlohy gulp pomocí `Ctrl + C`a potom spusťte následující příkaz v okně konzoly zobrazíte zprávy ladění. Může se zobrazit podrobné chybové zprávy ve výstupu konzoly. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problémy zjišťování zařízení
Pomoc při řešení běžných potíží s `devdisco` příkazu, zkontrolujte [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="npm-issues"></a>NPM problémy
Došlo k pokusu o aktualizaci vašeho balíčku NPM pomocí následujícího příkazu:

```bash
npm install -g npm
```

Pokud problém přetrvává, nechte komentáře na konci tohoto článku nebo vytvořte potíže Githubu v našem [Ukázka úložiště](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)

## <a name="remote-debugging"></a>Vzdálené ladění

Vzdálené ladění podpora bude brzy k dispozici v rozšíření kódu C/C++ sady Visual Studio k dispozici.

V současně můžete GDB pomocí Oblíbené terminálu SSH:

```bash
cd c-pi-lesson-x
sudo gdb app
```

## <a name="azure-cli-issues"></a>Problémy rozhraní příkazového řádku Azure
Rozhraní příkazového řádku Azure (Azure CLI) je buildu preview. Vyhledejte řešení v [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) k vyhledání řešení. Se pokuste o upgrade na nejnovější verzi rozhraní příkazového řádku Azure, když příkazy nefungují podle očekávání.

Pokud narazíte na všechny chyby pomocí nástroje souboru [problém](https://github.com/Azure/azure-cli/issues) v **problémy** část úložiště GitHub.

Nápovědu k řešení běžných potíží s, zkontrolujte [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

Pokud jsou splněny "Nelze najít na verzi, která splňuje požadavek", spusťte následující příkaz pro upgrade na nejnovější verzi pip.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Problémy instalace Python
### <a name="legacy-installation-issues-macos"></a>Problémy instalace starší verze (macOS)
Když instalujete **pip**, oprávnění chyba se vyvolá, když starší balíčky, které jsou nainstalovány s **su** oprávnění. K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována. Některé **pip** balíčky z předchozí instalace, které byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění. Řešení je odebrat tyto balíčky nainstalované pomocí kořenového. Tuto úlohu dokončit pomocí následujících kroků:

1. Přejděte na: /usr/local/lib/python2.7/site-packages
2. Seznam balíčků vytvořit pomocí kořenového:`ls -l | grep root`
3. Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`
4. Přeinstalujte Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT Hub problémy
Pokud jste úspěšně zřídit služby Azure IoT hub s `azure-cli`, a je potřeba nástroj pro správu zařízení, které se připojují ke službě IoT hub, zkuste následující nástroje:

### <a name="device-explorer"></a>Průzkumník zařízení
[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí se ke službě IoT hub v Azure. Komunikuje s následující [koncové body centra IoT](iot-hub-devguide.md):

* *Správa identit zařízení* zřizovat a spravovat zařízení zaregistrovaná službou IoT hub.
* *Zobrazí zařízení cloud* , můžete monitorovat zprávy odeslané ze zařízení do služby IoT hub.
* *Odeslat cloud zařízení* tak mohou zasílat zprávy do zařízení ze služby IoT hub.

Konfigurace vaší `IoT hub connection string` v rámci tohoto nástroje můžete použít všechny jeho možnosti.

### <a name="iot-hub-explorer"></a>Centrum IoT Explorer
[Centrum IoT Explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku ke správě klientů zařízení. Nástroj můžete použít ke správě zařízení v registru identit, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.

Chcete-li nainstalovat nejnovější verzi (předprodejní) nástroj iothub Průzkumník, spusťte následující příkaz v prostředí příkazového řádku:

```
npm install -g iothub-explorer@latest
```

Chcete-li získat další informace o tom všechny příkazy iothub-explorer a jejich parametrů můžete následující příkaz:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>portál Azure
Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure. Můžete také použít [portál Azure](../azure-portal-overview.md) pomoci zřizování, spravovat a ladit vašich prostředků Azure.

## <a name="azure-storage-issues"></a>Problémů s úložištěm Azure
[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od společnosti Microsoft, který můžete použít pro práci s daty Azure Storage ve Windows, systému macOS a Linux. Pomocí tohoto nástroje můžete připojit k tabulku a zobrazit data v ní. Tento nástroj slouží k řešení potíží vašeho úložiště Azure.
