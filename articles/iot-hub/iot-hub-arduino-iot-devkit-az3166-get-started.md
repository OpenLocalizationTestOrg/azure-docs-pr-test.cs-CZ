---
title: "aaaIoT DevKit toocloud - připojení DevKit AZ3166 IoT tooAzure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte toosend data toohello cloudu Azure platformy IoT DevKit AZ3166 tooAzure IoT Hub pro něj v tomto kurzu."
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
ms.openlocfilehash: fd36abaed1fd9f73028b84312bca9e900fb9c004
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-iot-devkit-az3166-tooazure-iot-hub-in-hello-cloud"></a>Připojit IoT DevKit AZ3166 tooAzure IoT Hub v cloudu hello

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Hello [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) lze použít toodevelop a prototypu využití služby Microsoft Azure řešení Internetu věcí (IoT). Obsahuje kompatibilní panelu s bohatou periferních zařízení a senzorů, balíček Tabule open source a rozšiřujících se Arduino [projekty katalogu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Co dělat
Připojit [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) tooan Azure IoT hub shromažďování hello teploty a vlhkosti data ze senzorů vytvořit a odeslat hello data tooIoT rozbočovače.

Nemáte DevKit ještě? Získat novou [zde](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>Co se naučíte

* Jak tooconnect IoT DevKit tooWireless přístup bodu a příprava vývojového prostředí.
* Jak toocreate služby IoT hub a registrovat zařízení pro MXChip IoT DevKit.
* Jak data snímačů toocollect spuštěním ukázkovou aplikaci na MXChip IoT DevKit.
* Jak toosend hello senzor data tooyour IoT hub.

## <a name="what-you-need"></a>Co potřebujete

* MXChip IoT DevKit fórum s malých kabelu USB. [Získejte nyní](https://aka.ms/iot-devkit-purchase)
* Počítač se systémem Windows 10 nebo systému macOS 10.10 +
* Aktivní předplatné Azure
  * Aktivovat [Bezplatný zkušební účet Microsoft Azure 30 dnů](https://azureinfo.microsoft.com/us-freetrial.html)

## <a name="prepare-your-hardware"></a>Připravte svůj hardware

Zapojení hello hardwaru tooyour počítače.

### <a name="hardware-you-need"></a>Hardware, které potřebujete

* DevKit panelu
* Malých kabelu USB

![získávání spuštění – hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

### <a name="connect-devkit-tooyour-computer"></a>Připojte počítač tooyour DevKit

1. Připojení USB end tooyour počítače
2. Připojení Micro USB end toohello DevKit
3. Další toopower Hello zelená DIODU potvrdí připojení

![získávání spuštění – připojení](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wifi"></a>Konfigurace sítě Wi-Fi

Projekty IoT spoléhají na připojení k Internetu. Použijte následující pokyny tooconfigure hello DevKit tooconnect tooWiFi hello.

### <a name="enter-ap-mode"></a>Zadejte režim Asie a Tichomoří

Podržte tlačítko B, poté resetujte nabízení a verze hello tlačítko a potom na tlačítko pro uvolnění B. Vaše DevKit zadá režimu přístupový bod pro konfiguraci sítě Wi-Fi. úvodní obrazovka se zobrazí hello služby nastavit Identifier(SSID) DevKit Dobrý den, jakož i hello konfigurace portálu IP adresa:

![získávání spuštění-Wi-Fi-Asie a Tichomoří](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-toodevkit-ap"></a>Asie a Tichomoří tooDevKit připojení

Nyní použijte jiný povolit Wi-Fi zařízení (počítač nebo mobilní telefon) tooconnect toohello DevKit SSID (zvýrazněných v výše uvedený snímek obrazovky hello), ponechte prázdné heslo hello.

![získávání spuštění ssid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wifi-for-devkit"></a>Konfigurace sítě Wi-Fi pro DevKit

Otevřete hello IP adresu na obrazovce DevKit hello v počítači nebo v prohlížeči mobilního telefonu, vyberte hello Wi-Fi sítě, kterou chcete hello DevKit tooconnect na a pak zadejte heslo hello. Klikněte na tlačítko **připojit** toocomplete:

![získávání spuštění-Wi-Fi portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Jakmile hello připojení úspěšné, hello DevKit restartuje za několik sekund. Pokud byly úspěšné, zobrazí se název hello Wi-Fi a IP adresu na úvodní obrazovka:

![získávání spuštění-Wi-Fi ip](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
zobrazená v hello fotografií Hello IP adresa nemusí odpovídat skutečné IP hello přiřazené a zobrazené na obrazovce DevKit hello. To je normální jako Wi-Fi používá DHCP toodynamically přiřazovat IP adresy.

Po dokončení konfigurace sítě Wi-Fi, bude vaše přihlašovací údaje trvalé hello zařízení pro toto připojení, i v případě, že byl odpojen. Například pokud jste nakonfigurovali hello DevKit pro Wi-Fi v domácnosti a pak trvalo hello DevKit toohello office, budete potřebovat tooreconfigure Asie režimu (od kroku **zadejte režim Asie**) tooconnect hello DevKit tooyour office Wi-Fi. 

## <a name="start-using-devkit"></a>Začít používat DevKit

Hello výchozí aplikaci spuštěnou na DevKit zkontroluje hello nejnovější verzi firmwaru hello a zobrazí některé senzor diagnostická data za vás.

### <a name="upgrade-toohello-latest-firmware"></a>Upgradovat nejnovější firmware toohello

Zobrazí se výzva na úvodní obrazovka obě hello firmware aktuální a nejnovější verze, pokud je potřeba upgrade. Postupujte podle [upgradovat firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) Průvodce tooupgrade ho.

![získávání spuštění firmwaru](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
Toto je jednorázové úsilí, jakmile začnete vývoji na hello DevKit a nahrát aplikaci, budete mít nejnovější firmware hello pocházet s vaší aplikací.

### <a name="test-various-sensors"></a>Testování různých senzorů

Stiskněte tlačítko B tootest senzorů, pokračujte stisknutím a uvolněním hello B tlačítko toocycle prostřednictvím každý senzor.

![získávání spuštění senzorů](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-development-environment"></a>Příprava vývojového prostředí

Nyní je čas tooset hello vývojového prostředí: nástroje a balíčky pro toobuild omráčení aplikace či aplikace IoT. Můžete vybrat Windows nebo verze systému macOS podle tooyour operačního systému.


### <a name="windows"></a>Windows

Doporučujeme vám toouse hello instalace balíčku tooprepare hello vývojové prostředí. Pokud narazíte na problémy, můžete podle hello [ruční kroky](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) tooget ho provést.

#### <a name="download-latest-package"></a>Stáhněte si nejnovější balíček

Hello `.zip` si stáhnout soubor obsahuje všechny nezbytné nástroje a balíčky, které jsou potřebné pro vývoj DevKit.

> [!div class="button"]
[Stáhnout](https://azureboard.azureedge.net/prod/installpackage/devkit_install_1.0.2.zip)


> [!NOTE] 
> Hello `.zip` soubor obsahuje následující hello nástroje a balíčků. Pokud již máte některé součásti nainstalované, skript hello rozpozná a je přeskočit.
> * Node.js a Yarn: modul Runtime pro hello instalační skript a automatizované úlohy
> * [Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) -prostředí příkazového řádku a platformy pro správu prostředků Azure, hello MSI obsahuje závislé Python a pip.
> * [Visual Studio Code](https://code.visualstudio.com/): editor lehký kód pro vývoj DevKit
> * [Rozšíření sady Visual Studio Code pro Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): vývoj umožňuje Arduino v produktu VS Code
> * [Arduino IDE](https://www.arduino.cc/en/Main/Software): hello rozšíření pro Arduino spoléhá na tento nástroj
> * Balíček DevKit Tabule: Nástroj řetězy, knihovny a projekty doplňku hello DevKit
> * Nástroj ST odkaz: Základní nástroje a ovladače

#### <a name="run-installation-script"></a>Spusťte instalační skript

V Průzkumníku souborů Windows, vyhledejte hello `.zip` a rozbalte ho, vyhledejte `install.cmd`, klikněte pravým tlačítkem a vyberte **"Spustit jako správce"** toostart.

![získávání spuštění spustit správce.](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

Během instalace zobrazí se průběh hello každý nástroj nebo balíčku.

![získávání spuštění instalace](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

#### <a name="confirm-tooinstall-drivers"></a>Potvrďte tooinstall ovladače

Hello VS Code pro rozšíření Arduino spoléhá na hello Arduino IDE. Pokud je to hello poprvé instalujete hello Arduino IDE, bude výzvami tooinstall příslušné ovladače:

![získávání spuštění – ovladače](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

Má trvat přibližně 10 minut toofinish instalace v závislosti na rychlosti sítě Internet. Po dokončení instalace hello byste měli vidět Visual Studio Code a Arduino IDE zástupce na ploše.

> [!NOTE] 
V některých případech při spuštění VS Code, zobrazí se výzva k chybě, který nemůže najít Arduino IDE nebo balíček Příbuzná panelu. toosolve ji zavřít VS Code Arduino IDE spustit jednou a VS kód by měl vyhledejte Arduino IDE cestu správně.


### <a name="macos-preview"></a>systému macOS (Preview)

Postupujte podle těchto kroků tooprepare vývojového prostředí v systému macOS.

#### <a name="install-azure-cli-20"></a>Instalace Azure CLI 2.0

Postupujte podle hello [oficiální průvodce](https://docs.microsoft.com//cli/azure/install-azure-cli) tooinstall Azure CLI 2.0:

Nainstalovat Azure CLI 2.0 s jedním `curl` příkaz:

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

A znovu spusťte váš příkazové prostředí pro efekt tootake změny:

```bash
exec -l $SHELL
```

#### <a name="install-arduino-ide"></a>Nainstalujte Arduino IDE

Hello rozšíření Visual Studio Code Arduino spoléhá na hello Arduino IDE. Stáhněte a nainstalujte hello [Arduino IDE pro systému macOS](https://www.arduino.cc/en/Main/Software).

#### <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code

Stáhněte a nainstalujte [Visual Studio Code pro systému macOS](https://code.visualstudio.com/). Bude jím hello primární vývojový nástroj k vytváření aplikací DevKit IoT.

####  <a name="download-latest-package"></a>Stáhněte si nejnovější balíček

1. Nainstalujte Node.js. Můžete použít Správce balíčků oblíbených systému macOS [Homebrew](https://brew.sh/) nebo [předem vytvořené instalační program](https://nodejs.org/en/download/) tooinstall ho.

2. Stáhněte si `.zip` souboru, který obsahuje skripty úloh, které jsou potřebné pro vývoj DevKit v produktu VS Code.

   > [!div class="button"]
   [Stáhnout](https://azureboard.azureedge.net/installpackage/devkit_tasks_1.0.2.zip)

   Vyhledejte hello `.zip` a rozbalte ho. Spusťte **Terminálové** aplikace a spusťte hello následující příkazy tooconfigure:

   Přesunutí složky uživatele systému macOS tooyour extrahovanou složku:
   ```bash
   mv [.zip extracted folder]/azure-board-cli ~/. ; cd ~/azure-board-cli
   ```
  
   Nainstalujte `npm` balíčky:
   ```
   npm install
   ```

#### <a name="install-vs-code-extension-for-arduino"></a>Instalace rozšíření VS Code pro Arduino

Visual Studio Code můžete rozšíření Marketplace tooinstall přímo v nástroji hello, jednoduše klikněte na ikonu rozšíření hello v levé nabídce podokně hello a pak vyhledejte `Arduino` tooinstall:

![instalace rozšíření](media/iot-hub-arduino-devkit-az3166-get-started/installation-extensions-mac.png)

#### <a name="install-devkit-board-package"></a>Instalovat balíček DevKit panelu

Budete potřebovat tooadd hello DevKit panel pomocí hello panelu Správce v kódu Visual Studio.

1. Použití `Cmd+Shift+P` tooinvoke příkaz palety a typ **Arduino** potom najděte a vyberte **Arduino: Tabule Manager**.

2. Klikněte na tlačítko **další adresy URL** v pravé dolní části hello.
   ![instalace další adresy URL](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)

3. V hello `settings.json` soubor, přidejte řádek na konec hello `USER SETTINGS` podokně a uložte.
   ```json
   "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
   ```
   ![instalace. nastavení json](media/iot-hub-arduino-devkit-az3166-get-started/installation-settings-json-mac.png)

4. Nyní v hello Manager panelu Hledat 'az3166' a nainstalujte nejnovější verzi hello.
   ![instalace az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)

Nyní máte všechny potřebné nástroje hello a balíčky pro systému macOS nainstalována.


## <a name="open-project-folder"></a>Otevřít projekt složky

### <a name="launch-vs-code"></a>Spusťte VS Code

Ujistěte se, že vaše DevKit není připojený. Nejprve spusťte VS Code a připojte počítač tooyour DevKit hello. VS Code automaticky ji najít a Překryvné úvodní stránka:

![Mini solution vscode](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
V některých případech při spuštění VS Code, zobrazí se výzva s chybou, který nemůže najít Arduino IDE nebo balíček Příbuzná panelu. toosolve ji zavřít VS Code, znovu spusťte Arduino IDE a VS kód by měl vyhledejte Arduino IDE cestu správně.

### <a name="open-arduino-examples-folder"></a>Otevřít složku Arduino příklady

Přepínač příliš**"Arduino příklady"** kartě, přejděte příliš`Examples for MXCHIP AZ3166 > AzureIoT` a klikněte na `GetStarted`.

![Mini solution příklady](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

Pokud jste tooclose hello podokně tooreload ji, použijte `Ctrl+Shift+P` (systému macOS: `Cmd+Shift+P`) tooinvoke příkaz palety a typ **Arduino** toofind a vyberte **Arduino: Příklady**.

## <a name="provision-azure-services"></a>Zřídit služby Azure

V okně řešení hello spusťte úlohu `Ctrl+P` (systému macOS: `Cmd+P`) tak, že zadáte 'úkolů cloudu provision':

Hello VS Code terminálu, že interaktivního příkazového řádku vás provede zřizování hello vyžadovat služeb Azure:

![Mini-solution-cloud-provision](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-arduino-sketch"></a>Vytvoření a nahrání Arduino nákresu

### <a name="install-required-library"></a>Nainstalujte požadované knihovny

1. Stiskněte klávesu `F1` nebo `Ctrl+Shift+P` (systému macOS: `Cmd+Shift+P`) tooinvoke příkaz palety a typ **Arduino** potom najděte a vyberte **Arduino: Správce knihovny**.

2. Vyhledejte `ArduinoJson` knihovnu a klikněte na **instalace**

### <a name="build-and-upload-hello-device-code"></a>Vytvoření a nahrání hello zařízení kódu

Použití `Ctrl+P` (systému macOS: `Cmd+P`) toorun 'úloh zařízení – nahrání'. Hello terminálu vyzve jste tooenter režim konfigurace. toodo Ano, podržte tlačítko A potom tlačítko Obnovit hello nabízení a verzi. úvodní obrazovka se zobrazí "Konfigurace". Toto je tooset hello připojovací řetězec, který načte z kroku 'úloh cloudu provision'.

Potom se spustí, ověření a odeslání nákresu Arduino hello:

![Mini-solution – zařízení – nahrání](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

Hello DevKit bude restartujte počítač a spustit hello kódu.

## <a name="test-hello-project"></a>Test hello projektu

V produktu VS Code klepněte na ikonu moduly power hello na hello stavovém řádku tooopen hello sériové monitorování.

Ukázková aplikace Hello pracuje správně, až uvidíte hello následující výsledky:

* Dobrý den zobrazí sériové monitorování hello stejné informace jako obsah hello hello snímku obrazovky níže.
* Hello DIODU na MXChip IoT DevKit bliká.

![Závěrečný výstup v produktu VS Code](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Problémy a zpětné vazby

Můžete najít [nejčastější dotazy k](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) Pokud dojde k potížím nebo oslovení toous z hello kanály níže.

## <a name="next-steps"></a>Další kroky

Úspěšně jste se připojili MXChip IoT DevKit tooyour IoT Hub a odeslané hello zachytit senzor data tooyour IoT hub.

toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:

- [Správa zasílání zpráv cloudových zařízení pomocí iothub-exploreru](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [Uložit IoT Hub zprávy tooAzure úložiště dat](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [Použít data snímačů v reálném čase toovisualize Power BI ze služby Azure IoT Hub](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [Použití Azure Web Apps toovisualize snímačů v reálném čase dat z Azure IoT Hub](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [Předpověď počasí pomocí hello senzor dat z centra IoT v Azure Machine Learning](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [Správa zařízení s využitím iothub-exploreru](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [Vzdálené monitorování a oznámení s využitím Logic Apps](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)