---
title: "Arduino (C) se připojit k Azure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro Adafruit prolnutí M0 Wi-Fi Arduino prostředí"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "řešení potíží s arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: fdcc56ff-4420-463c-8a0e-5a1d215a874f
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 3bb369da9148300a80e7d351522a4d908e926996
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a>Řešení potíží
## <a name="hardware-issues"></a>Problémy s hardwarem
Informace o řešení běžných problémů na vaší kartě Adafruit prolnutí M0 Wi-Fi Arduino najdete v tématu [oficiální stránka řešení potíží](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).

## <a name="nodejs-package-issues"></a>Problémy balíčku Node.js
### <a name="no-response-during-gulp-tasks"></a>Žádná odpověď během gulp úlohy
Pokud narazíte na potíže se spouštěním gulp úlohy, můžete přidat `--verbose` možnost pro ladění. Zkuste ukončit aktuální úlohy gulp pomocí `Ctrl + C`a potom spusťte následující příkaz v okně konzoly zobrazíte zprávy ladění. Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.

```bash
gulp --verbose
```

Nebo můžete přidat `--listen` otevřete sériového portu na informace o protokolu výstupní zařízení.

```bash
gulp --listen
``` 

### <a name="npm-issues"></a>NPM problémy
Došlo k pokusu o aktualizaci vašeho balíčku NPM pomocí následujícího příkazu:

```bash
npm install -g npm
```

Pokud problém přetrvává, nechte komentáře na konci tohoto článku nebo vytvořte potíže Githubu v našem [úložiště ukázkové][sample-repository].

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
[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí se ke službě IoT hub v Azure. Komunikuje s následující [koncové body centra IoT](iot-hub-devguide.md):

* *Správa identit zařízení* zřizovat a spravovat zařízení zaregistrovaná službou IoT hub.
* *Zobrazí zařízení cloud* , můžete monitorovat zprávy odeslané ze zařízení do služby IoT hub.
* *Odeslat cloud zařízení* tak mohou zasílat zprávy do zařízení ze služby IoT hub.

Konfigurace vaší `IoT hub connection string` v rámci tohoto nástroje můžete použít všechny jeho možnosti.

### <a name="iot-hub-explorer"></a>Centrum IoT Explorer
[Centrum IoT Explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku ke správě klientů zařízení. Nástroj můžete použít ke správě zařízení v registru identit, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.


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

## <a name="azure-storage-issues"></a>Problémů s úložištěm Azure
[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od společnosti Microsoft, který můžete použít pro práci s daty Azure Storage ve Windows, systému macOS a Linux. Pomocí tohoto nástroje můžete připojit k tabulku a zobrazit data v ní. Tento nástroj slouží k řešení potíží vašeho úložiště Azure.

<!-- Images and links -->

[sample-repository]: https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md