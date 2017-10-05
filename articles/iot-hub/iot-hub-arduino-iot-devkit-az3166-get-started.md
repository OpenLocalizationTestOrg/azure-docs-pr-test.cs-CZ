---
title: "IoT DevKit do cloudu - připojit ke službě Azure IoT Hub IoT DevKit AZ3166 | Microsoft Docs"
description: "Zjistěte, jak nastavit a IoT DevKit AZ3166 se připojit ke službě Azure IoT Hub pro něj k odesílání dat do Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: xshi
ms.openlocfilehash: 122fac584ac5b954ef7b33a3121bee2c502ebc63
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub-in-the-cloud"></a>IoT DevKit AZ3166 se připojit ke službě Azure IoT Hub v cloudu

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

[MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) lze použít k vývoji a řešení Internetu věcí (IoT) prototypu využití služby Microsoft Azure. Obsahuje kompatibilní panelu s bohatou periferních zařízení a senzorů, balíček Tabule open source a rozšiřujících se Arduino [projekty katalogu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Co dělat
Připojit [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) do služby Azure IoT hub, který vytvoříte, shromažďování dat teploty a vlhkosti ze senzorů a odesílání dat do služby IoT hub.

Nemáte DevKit ještě? Získat novou [zde](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>Co se naučíte

* Postup připojení IoT DevKit na bezdrátový přístupový bod a příprava vývojového prostředí.
* Postup vytvoření služby IoT hub a registrovat zařízení pro MXChip IoT DevKit.
* Postup shromažďování dat snímačů spuštěním ukázkovou aplikaci na MXChip IoT DevKit.
* Jak odesílat data snímačů do služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

* MXChip IoT DevKit fórum s malých kabelu USB. [Získejte nyní](https://aka.ms/iot-devkit-purchase)
* Počítač se systémem Windows 10 nebo systému macOS 10.10 +
* Aktivní předplatné Azure
  * Aktivovat [Bezplatný zkušební účet Microsoft Azure 30 dnů](https://azureinfo.microsoft.com/us-freetrial.html)

## <a name="prepare-your-hardware"></a>Připravte svůj hardware

Propojte hardwaru do vašeho počítače.

### <a name="hardware-you-need"></a>Hardware, které potřebujete

* DevKit panelu
* Malých kabelu USB

![získávání spuštění – hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

### <a name="connect-devkit-to-your-computer"></a>Připojte k počítači DevKit

1. Připojte USB end k počítači
2. Připojení k DevKit end Micro USB
3. Zelená DIODU vedle power potvrdí připojení

![získávání spuštění – připojení](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wifi"></a>Konfigurace sítě Wi-Fi

Projekty IoT spoléhají na připojení k Internetu. Použijte následující pokyny ke konfiguraci DevKit pro připojení k Wi-Fi.

### <a name="enter-ap-mode"></a>Zadejte režim Asie a Tichomoří

Podržte tlačítko B, pak push a verze na tlačítko obnovit, a uvolněte tlačítko B. Vaše DevKit zadá režimu přístupový bod pro konfiguraci sítě Wi-Fi. Na obrazovce zobrazí Identifier(SSID) nastavení služby DevKit, jakož i Konfigurace portálu IP adresa:

![získávání spuštění-Wi-Fi-Asie a Tichomoří](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-to-devkit-ap"></a>Připojení k DevKit Asie a Tichomoří

Nyní použijte jiné zařízení Wi-Fi povoleno (počítač nebo mobilní telefon), se připojit k DevKit SSID (zvýrazněných v výše uvedený snímek obrazovky), ponechte prázdné heslo.

![získávání spuštění ssid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wifi-for-devkit"></a>Konfigurace sítě Wi-Fi pro DevKit

Otevřete adresu IP uvedené na obrazovce DevKit v prohlížeči počítače nebo mobilní telefon, vyberte chcete DevKit pro připojení k síti Wi-Fi a pak zadejte heslo. Klikněte na tlačítko **Connect** k dokončení:

![získávání spuštění-Wi-Fi portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Jakmile je připojení úspěšné, DevKit restartuje za několik sekund. Pokud byly úspěšné, zobrazí se název sítě Wi-Fi a IP adresu na obrazovce:

![získávání spuštění-Wi-Fi ip](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
IP adresa, zobrazí v fotografie nemusí odpovídat skutečné IP přiřazeny a zobrazují na obrazovce DevKit. To je normální, protože Wi-Fi používá protokol DHCP k dynamickému přidělení IP adresy.

Po dokončení konfigurace sítě Wi-Fi, bude vaše přihlašovací údaje trvalé na zařízení pro toto připojení, i v případě, že byl odpojen. Například pokud jste nakonfigurovali DevKit pro Wi-Fi v domácnosti a DevKit trvalo office, budete muset překonfigurovat Asie režimu (od kroku **zadejte režim Asie**) pro připojení DevKit pobočku Wi-Fi. 

## <a name="start-using-devkit"></a>Začít používat DevKit

Výchozí aplikace spuštěné v DevKit zkontroluje nejnovější verzi firmwaru a zobrazí některé senzor diagnostická data za vás.

### <a name="upgrade-to-the-latest-firmware"></a>Upgrade na nejnovější firmware

Zobrazí se výzva na obrazovce potřeby obou aktuální a nejnovější verzi firmwaru Pokud dojde upgradu. Postupujte podle [upgradovat firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) průvodce ho upgradovat.

![získávání spuštění firmwaru](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
Toto je jednorázové úsilí, jakmile začnete vývoji na DevKit a nahrát aplikaci, budete mít nejnovější firmware pocházet s vaší aplikací.

### <a name="test-various-sensors"></a>Testování různých senzorů

Stisknutím tlačítka B test senzorů, pokračujte stisknutím a uvolněním tlačítka B cyklicky každý senzor.

![získávání spuštění senzorů](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-development-environment"></a>Příprava vývojového prostředí

Nyní je čas k nastavení vývojového prostředí: nástroje a balíčky pro sestavit poutavých aplikace či aplikace IoT. Můžete podle operačního systému Windows nebo systému macOS verze.


### <a name="windows"></a>Windows

Doporučujeme použít instalační balíček Příprava vývojového prostředí. Pokud narazíte na problémy, můžete podle [ruční kroky](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) ho provést.

#### <a name="download-latest-package"></a>Stáhněte si nejnovější balíček

`.zip` Si stáhnout soubor obsahuje všechny nezbytné nástroje a balíčky, které jsou potřebné pro vývoj DevKit.

> [!div class="button"]
[Stáhnout](https://azureboard.azureedge.net/prod/installpackage/devkit_install_1.0.2.zip)


> [!NOTE] 
> `.zip` Soubor obsahuje následující nástroje a balíčků. Pokud již máte některé součásti nainstalované, skript rozpozná a jejich přeskočit.
> * Node.js a Yarn: modul Runtime pro instalační skript a automatizované úlohy
> * [Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) – možnosti příkazového řádku a platformy pro správu prostředků Azure, soubor MSI obsahuje závislé Python a pip.
> * [Visual Studio Code](https://code.visualstudio.com/): editor lehký kód pro vývoj DevKit
> * [Rozšíření sady Visual Studio Code pro Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): vývoj umožňuje Arduino v produktu VS Code
> * [Arduino IDE](https://www.arduino.cc/en/Main/Software): rozšíření pro Arduino spoléhá na tento nástroj
> * Balíček DevKit Tabule: Nástroj řetězy, knihovny a projekty doplňku DevKit
> * Nástroj ST odkaz: Základní nástroje a ovladače

#### <a name="run-installation-script"></a>Spusťte instalační skript

V Průzkumníku souborů Windows, vyhledejte `.zip` a rozbalte ho, vyhledejte `install.cmd`, klikněte pravým tlačítkem a vyberte **"Spustit jako správce"** spustit.

![získávání spuštění spustit správce.](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

Během instalace zobrazí se průběh každé nástroj nebo balíčku.

![získávání spuštění instalace](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

#### <a name="confirm-to-install-drivers"></a>Potvrzení instalace ovladačů

Kód VS pro rozšíření Arduino spoléhá na Arduino IDE. Pokud instalujete Arduino IDE poprvé, budete vyzváni k instalaci příslušné ovladače:

![získávání spuštění – ovladače](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

Má trvat přibližně 10 minut na dokončení instalace v závislosti na rychlosti sítě Internet. Po dokončení instalace byste měli vidět Visual Studio Code a Arduino IDE zástupce na ploše.

> [!NOTE] 
V některých případech při spuštění VS Code, zobrazí se výzva k chybě, který nemůže najít Arduino IDE nebo balíček Příbuzná panelu. K jeho řešení, zavřete VS Code, spusťte jednou Arduino IDE a VS Code by měl vyhledejte Arduino IDE cestu správně.


### <a name="macos-preview"></a>systému macOS (Preview)

Postupujte podle následujících kroků připravíte vývojového prostředí v systému macOS.

#### <a name="install-azure-cli-20"></a>Instalace Azure CLI 2.0

Postupujte podle [oficiální průvodce](https://docs.microsoft.com//cli/azure/install-azure-cli) nainstalovat Azure CLI 2.0:

Nainstalovat Azure CLI 2.0 s jedním `curl` příkaz:

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

A znovu spusťte váš příkazové prostředí pro změny se projeví:

```bash
exec -l $SHELL
```

#### <a name="install-arduino-ide"></a>Nainstalujte Arduino IDE

Rozšíření Visual Studio Code Arduino spoléhá na Arduino IDE. Stáhněte a nainstalujte [Arduino IDE pro systému macOS](https://www.arduino.cc/en/Main/Software).

#### <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code

Stáhněte a nainstalujte [Visual Studio Code pro systému macOS](https://code.visualstudio.com/). Bude primární vývojový nástroj pro tvorbu DevKit IoT aplikací.

####  <a name="download-latest-package"></a>Stáhněte si nejnovější balíček

1. Nainstalujte Node.js. Můžete použít Správce balíčků oblíbených systému macOS [Homebrew](https://brew.sh/) nebo [předem vytvořené instalační program](https://nodejs.org/en/download/) k její instalaci.

2. Stáhněte si `.zip` souboru, který obsahuje skripty úloh, které jsou potřebné pro vývoj DevKit v produktu VS Code.

   > [!div class="button"]
   [Stáhnout](https://azureboard.azureedge.net/installpackage/devkit_tasks_1.0.2.zip)

   Vyhledejte `.zip` a rozbalte ho. Spusťte **Terminálové** aplikace a spusťte následující příkazy ke konfiguraci:

   Extrahovanou složku přesunete do složky systému macOS uživatele:
   ```bash
   mv [.zip extracted folder]/azure-board-cli ~/. ; cd ~/azure-board-cli
   ```
  
   Nainstalujte `npm` balíčky:
   ```
   npm install
   ```

#### <a name="install-vs-code-extension-for-arduino"></a>Instalace rozšíření VS Code pro Arduino

Visual Studio Code umožňuje instalaci rozšíření Marketplace přímo v nástroji, jednoduše klikněte na ikonu rozšíření v levé nabídce podokně a pak vyhledejte `Arduino` k instalaci:

![instalace rozšíření](media/iot-hub-arduino-devkit-az3166-get-started/installation-extensions-mac.png)

#### <a name="install-devkit-board-package"></a>Instalovat balíček DevKit panelu

Musíte přidat pomocí panelu Správce ve Visual Studio Code DevKit panelu.

1. Použití `Cmd+Shift+P` k vyvolání příkazu palety a typ **Arduino** potom najděte a vyberte **Arduino: Tabule Manager**.

2. Klikněte na tlačítko **další adresy URL** v pravé dolní části.
   ![instalace další adresy URL](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)

3. V `settings.json` soubor, přidejte řádek v dolní části `USER SETTINGS` podokně a uložte.
   ```json
   "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
   ```
   ![instalace. nastavení json](media/iot-hub-arduino-devkit-az3166-get-started/installation-settings-json-mac.png)

4. Nyní ve Správci panelu Hledat 'az3166' a nainstalujte nejnovější verzi.
   ![instalace az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)

Nyní máte všechny potřebné nástroje a balíčky pro systému macOS nainstalována.


## <a name="open-project-folder"></a>Otevřít projekt složky

### <a name="launch-vs-code"></a>Spusťte VS Code

Ujistěte se, že vaše DevKit není připojený. Nejprve spusťte VS Code a připojte DevKit k počítači. VS Code automaticky ji najít a Překryvné úvodní stránka:

![Mini solution vscode](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
V některých případech při spuštění VS Code, zobrazí se výzva s chybou, který nemůže najít Arduino IDE nebo balíček Příbuzná panelu. K jeho řešení, zavřete VS Code, znovu spusťte Arduino IDE a VS kód by měl vyhledejte Arduino IDE cestu správně.

### <a name="open-arduino-examples-folder"></a>Otevřít složku Arduino příklady

Přepnout na **"Arduino příklady"** kartě, přejděte na `Examples for MXCHIP AZ3166 > AzureIoT` a klikněte na `GetStarted`.

![Mini solution příklady](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

Pokud stát zavřete podokno znovu načíst, použijte `Ctrl+Shift+P` (systému macOS: `Cmd+Shift+P`) k vyvolání příkazu palety a typ **Arduino** a vyberte **Arduino: Příklady**.

## <a name="provision-azure-services"></a>Zřídit služby Azure

V okně řešení spuštění úkolu prostřednictvím `Ctrl+P` (systému macOS: `Cmd+P`) tak, že zadáte 'úkolů cloudu provision':

V produktu VS Code terminálu interaktivního příkazového řádku vás provede zřizování požadované služby Azure:

![Mini-solution-cloud-provision](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-arduino-sketch"></a>Vytvoření a nahrání Arduino nákresu

### <a name="install-required-library"></a>Nainstalujte požadované knihovny

1. Stiskněte klávesu `F1` nebo `Ctrl+Shift+P` (systému macOS: `Cmd+Shift+P`) k vyvolání příkazu palety a typ **Arduino** potom najděte a vyberte **Arduino: Správce knihovny**.

2. Vyhledejte `ArduinoJson` knihovnu a klikněte na **instalace**

### <a name="build-and-upload-the-device-code"></a>Vytvořit a odeslat kód zařízení

Použití `Ctrl+P` (systému macOS: `Cmd+P`) ke spuštění 'úloha zařízení – nahrání'. Terminálu zobrazí výzvu k zadání režimu konfigurace. Uděláte to tak, podržte tlačítko A pak push a verzí na tlačítko Obnovit. Na obrazovce zobrazí "Konfigurace". To je nastavit připojovací řetězec, který načte z kroku 'úloh cloudu provision'.

Potom se spustí, ověření a odeslání nákresu Arduino:

![Mini-solution – zařízení – nahrání](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

DevKit bude restartujte počítač a spustit kód.

## <a name="test-the-project"></a>Testování projektu

V produktu VS Code klikněte na ikonu power moduly na stavovém řádku otevřete sériové monitorování.

Ukázkové aplikace pracuje správně, pokud jste se zobrazit následující výsledky:

* Sériové monitorování zobrazuje stejné informace jako obsah na tomto snímku obrazovky.
* Indikátor LED na MXChip IoT DevKit bliká.

![Závěrečný výstup v produktu VS Code](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Problémy a zpětné vazby

Můžete najít [nejčastější dotazy k](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) Pokud dojde k potížím nebo oslovení nám níže kanály.

## <a name="next-steps"></a>Další kroky

Máte úspěšně připojen IoT DevKit MXChip do služby IoT Hub a data zaznamenaná senzor odeslané do služby IoT hub.

Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:

- [Správa zasílání zpráv cloudových zařízení pomocí iothub-exploreru](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [Uložení zpráv IoT Hub do úložiště dat Azure](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [Vizualizovat data snímačů v reálném čase ze služby Azure IoT Hub pomocí Power BI](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [Použít Azure Web Apps k vizualizaci dat snímačů v reálném čase ze služby Azure IoT Hub](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [Počasí prognózy používající senzor data ze služby IoT hub v Azure Machine Learning](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [Správa zařízení s využitím iothub-exploreru](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [Vzdálené monitorování a oznámení s využitím Logic Apps](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)