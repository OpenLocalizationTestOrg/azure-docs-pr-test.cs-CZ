---
title: "aaaIntel Edison toocloud (C) - připojení Edison Intel tooAzure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte Intel Edison tooAzure IoT Hub pro Intel Edison toosend data toohello Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Azure iot intel edison, intel edison iot hub, intel edison odesílat data toocloud, intel edison toocloud"
ms.assetid: 4885fa2c-c2ee-4253-b37f-ccd55f92b006
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/17/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0928e6c7870d724ff2044280937a45a9e032c75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-intel-edison-tooazure-iot-hub-c"></a>Připojit Intel Edison tooAzure IoT Hub (C)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

V tomto kurzu zahájíte učení hello základy práce s Intel Edison. Pak zjistíte, jak tooseamlessly propojit své cloudové toohello zařízení pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).

Nemáte sady ještě? Spustit [sem](https://azure.microsoft.com/develop/iot/starter-kits)

## <a name="what-you-do"></a>Co dělat

* Instalační program Intel Edison a a Groove moduly.
* Vytvoření služby IoT hub.
* Registrovat zařízení pro Edison ve službě IoT hub.
* Spuštění ukázkové aplikace na Edison toosend senzor data tooyour IoT hub.

Připojte Intel Edison tooan IoT hub, který vytvoříte. Potom spustíte ukázkovou aplikaci na Edison toocollect teploty a vlhkosti data z teplotní snímač Groove. Nakonec odeslat hello senzor data tooyour IoT hub.

## <a name="what-you-learn"></a>Co se naučíte

* Jak toocreate služby Azure IoT hub a získat nový připojovací řetězec zařízení.
* Jak tooconnect Edison s teplotní snímač Groove.
* Jak data snímačů toocollect spuštěním ukázkovou aplikaci na Edison.
* Jak toosend senzor data tooyour IoT hub.

## <a name="what-you-need"></a>Co potřebujete

![Co potřebujete](media/iot-hub-intel-edison-kit-c-get-started/0_kit.png)

* Hello Intel Edison panelu
* Panel Arduino rozšíření
* Aktivní předplatné Azure. Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.
* Mac nebo počítači se systémem Windows nebo Linux.
* Připojení k Internetu.
* Micro B tooType kabelu A USB
* Zdroj napájení přímo aktuální (DC). Zdroj napájení by měl být hodnocení následujícím způsobem:
  - ŘADIČ DOMÉNY 7 15V
  - Alespoň 1500mA
  - Hello center nebo vnitřní pin musí být kladná pólu hello hello napájení

Hello následující položky jsou volitelné:

* Groove základní štítu V2
* Groove - teplotní snímač
* Kabel Groove
* Jako mezeru mezi řádky nebo šrouby součástí hello balení, včetně dvěma šrouby toofasten hello modulu toohello rozšíření panelu a čtyři sady šrouby a plastové vymezovače.

> [!NOTE] 
Tyto položky jsou volitelné, protože podpora ukázkový kód hello simulated data snímačů.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a>Instalační program Intel Edison

### <a name="assemble-your-board"></a>Sestavte panel

Tato část obsahuje kroky tooattach panel Intel® Edison modulu tooyour rozšíření.

1. Místní hello Intel® Edison modul v rámci hello bílé outline na vaší kartě rozšíření zarovnání hello díry v modulu hello s hello šrouby na panelu rozšíření hello.

2. Klepnout na hello modulu pod hello slova `What will you make?` dokud si myslíte, že během.

   ![Sestavte Tabule 2](media/iot-hub-intel-edison-kit-c-get-started/1_assemble_board2.jpg)

3. Použijte hello dva šestnáctkových matice (zahrnutý v balíčku hello) toosecure hello modulu toohello rozšíření panelu.

   ![Sestavte Tabule 3](media/iot-hub-intel-edison-kit-c-get-started/2_assemble_board3.jpg)

4. Vložte šroubu v jednom z hello čtyři díry rohu na panelu rozšíření hello. Otočením a posílit mezi hello bílé plastové nosníků do šroubovacím hello.

   ![Sestavte Tabule 4](media/iot-hub-intel-edison-kit-c-get-started/3_assemble_board4.jpg)

5. Tento postup opakujte pro hello nosníků další tři rohu.

   ![Sestavte Tabule 5](media/iot-hub-intel-edison-kit-c-get-started/4_assemble_board5.jpg)

Nyní je několik panel.

   ![sestavený panelu](media/iot-hub-intel-edison-kit-c-get-started/5_assembled_board.jpg)

### <a name="connect-hello-grove-base-shield-and-hello-temperature-sensor"></a>Připojit hello štítu základní Groove a senzor teploty hello

1. Na panelu tooyour umístíte hello štítu základní Groove. Ujistěte se, že všechny kódy PIN jsou úzce připojovány panel.
   
   ![Základní štítu Groove](media/iot-hub-intel-edison-kit-c-get-started/6_grove_base_sheild.jpg)

2. Senzor teploty použití Groove kabel tooconnect Groove do hello Groove základní štítu **A0** portu.

   ![Připojit tootemperature senzor](media/iot-hub-intel-edison-kit-c-get-started/7_temperature_sensor.jpg)
   
   ![Edison a senzor připojení](media/iot-hub-intel-edison-kit-c-get-started/16_edion_sensor.png)

Vaše senzor je nyní připraven.

### <a name="power-up-edison"></a>Napájení až Edison

1. Zařaďte hello napájení.

   ![Zařadit napájení](media/iot-hub-intel-edison-kit-c-get-started/8_plug_power.jpg)

2. Zelená DIODU (s názvem bez přípony DS1 na hello Tabule rozšíření Arduino *), by měla zůstat po a light.

3. Počkejte jednu minutu toofinish Tabule hello dalším spuštění.

   > [!NOTE]
   > Pokud nemáte ke zdroji napájení řadiče domény, můžete stále panelu power hello přes USB port. V tématu `Connect Edison tooyour computer` podrobnosti. Pohánějící panel tímto způsobem může způsobit nepředvídatelné chování z vaší karty, zejména v případě, že pomocí sítě Wi-Fi nebo řídí motory.

### <a name="connect-edison-tooyour-computer"></a>Připojte počítač tooyour Edison

1. Přepnutí dolů hello mikrospínače směrem hello dva malých USB porty, takže Edison je v režimu zařízení. Rozdíly mezi zařízení režim a režim hostitele, prosím odkazovat [zde](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Přepněte dolů mikrospínače hello](media/iot-hub-intel-edison-kit-c-get-started/9_toggle_down_microswitch.jpg)

2. Připojte kabel USB malých hello portu USB horním malých hello.

   ![Horní malých portu USB](media/iot-hub-intel-edison-kit-c-get-started/10_top_usbport.jpg)

3. Moduly hello druhém konci kabelu USB k počítači.

   ![Počítač USB](media/iot-hub-intel-edison-kit-c-get-started/11_computer_usb.jpg)

4. Budete vědět, že panel úplné inicializace když se počítač připojí na nový disk (podobně jako vkládání SD karty do počítače).

## <a name="download-and-run-hello-configuration-tool"></a>Stáhněte a spusťte konfigurační nástroj hello
Získat nejnovější nástroje Konfigurace hello z [tento odkaz](https://software.intel.com/en-us/iot/hardware/edison/downloads) uvedené v části hello `Installers` záhlaví. Spusťte nástroj hello a postupujte podle jeho na obrazovce pokyny, kde je potřeba klepnutím na tlačítko Další

### <a name="flash-firmware"></a>Flash firmwaru
1. Na hello `Set up options` klikněte na tlačítko `Flash Firmware`.
2. Vyberte bitovou kopii tooflash hello na panel jedním z následujících hello:
   - toodownload a flash panel s hello nejnovější firmware bitové kopie k dispozici z Intel, vyberte `Download hello latest image version xxxx`.
   - Vyberte panel s bitovou kopií již uložení v počítači, tooflash `Select hello local image`. Procházejte tooand hello vyberte bitovou kopii tooflash tooyour panelu.
3. Instalační program nástroje Hello pokusí tooflash panel. celý proces blikající Hello může trvat až too10 minut.

### <a name="set-password"></a>Nastavit heslo
1. Na hello `Set up options` klikněte na tlačítko `Enable Security`.
2. Můžete nastavit vlastní název pro panel Intel® Edison. Tato položka je nepovinná.
3. Zadejte heslo pro panel a pak klikněte na `Set password`.
4. Známku výpadku hello hesla, které se později používá.

### <a name="connect-wi-fi"></a>Připojení Wi-Fi
1. Na hello `Set up options` klikněte na tlačítko `Connect Wi-Fi`. Počkejte, až minutu tooone jako kontrolách počítači k dispozici sítě Wi-Fi.
2. Z hello `Detected Networks` rozevíracího seznamu vyberte vaší sítě.
3. Z hello `Security` rozevíracího seznamu, vyberte hello sítě typ zabezpečení.
4. Poskytnout vaše údaje přihlašovací jméno a heslo a pak klikněte na `Configure Wi-Fi`.
5. Známku výpadku hello IP adresy, která se později používá.

> [!NOTE]
> Ujistěte se, že Edison je připojený toohello stejné sítě jako váš počítač. Váš počítač připojí tooyour Edison pomocí hello IP adresy.

   ![Připojit tootemperature senzor](media/iot-hub-intel-edison-kit-c-get-started/12_configuration_tool.png)

Blahopřejeme! Úspěšně jste nakonfigurovali Edison.

## <a name="run-a-sample-application-on-intel-edison"></a>Spuštění ukázkové aplikace na Intel Edison

### <a name="prepare-hello-azure-iot-device-sdk"></a>Příprava hello sady SDK zařízení Azure IoT

1. Použijte jeden z následujících klientů SSH z vaší hostitele počítače tooconnect tooyour Intel Edison hello. Hello IP adresa je z hello konfigurační nástroj a hello heslo je hello jeden, které jste nastavili v tohoto nástroje.
    - [PuTTY](http://www.putty.org/) pro systém Windows.
    - Hello integrovaného klienta SSH na Ubuntu nebo systému macOS (Spustit `ssh root@"hello IP address"`).

2. Klon hello ukázka klientské aplikace tooyour zařízení. 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-edison-client-app.git
   ```

3. Pak přejděte toohello úložišti složky toorun hello následující příkaz toobuild sady SDK Azure IoT

   ```bash
   cd iot-hub-c-intel-edison-client-app
   sed -i -e 's/\r$//' buildSDK.sh
   chmod 755 buildSDK.sh
   ./buildSDK.sh
   ```

### <a name="configure-hello-sample-application"></a>Konfigurace hello ukázkové aplikace

1. Otevřete konfigurační soubor hello spuštěním hello následující příkazy:

   ```bash
   nano config.h
   ```

   ![Konfigurační soubor](media/iot-hub-intel-edison-kit-c-get-started/13_configure_file.png)

   Existují dvě makra v tomto souboru můžete configurate. Hello nejprve jeden je `INTERVAL`, která definuje hello časový interval mezi dvě zprávy, které odesílají toocloud. Hello druhá `SIMULATED_DATA`, což je logickou hodnotu pro jestli toouse simulated data snímačů, nebo ne.

   Pokud jste **nemají hello senzor**, nastavte hello `SIMULATED_DATA` hodnota příliš`1` toomake hello ukázkovou aplikaci vytváření a používání dat snímačů simulované.

2. Uložte a zavřete stisknutím řízení-O > zadejte > CTRL-X.

### <a name="build-and-run-hello-sample-application"></a>Sestavení a spuštění ukázkové aplikace hello

1. Vytvoření ukázkové aplikace hello spuštěním hello následující příkaz:

   ```bash
   cmake . && make
   ```
   ![Sestavení výstupu](media/iot-hub-intel-edison-kit-c-get-started/14_build_output.png)

1. Spuštění ukázkové aplikace hello spuštěním hello následující příkaz:

   ```bash
   sudo ./app '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Zkontrolujte, zda jste způsobené kopírováním a vkládáním hello zařízení připojovací řetězec do jednoduchých uvozovek a být hello.

Měli byste vidět, že hello následující výstup, že zobrazuje hello senzor dat a hello zpráv, které jsou odeslány tooyour IoT hub.

![Výstup – senzor dat odesílaných ze služby IoT hub Intel Edison tooyour](media/iot-hub-intel-edison-kit-c-get-started/15_message_sent.png)

## <a name="next-steps"></a>Další kroky

Spustil jsem ukázková data snímačů toocollect aplikace a odešlete ji tooyour IoT hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
