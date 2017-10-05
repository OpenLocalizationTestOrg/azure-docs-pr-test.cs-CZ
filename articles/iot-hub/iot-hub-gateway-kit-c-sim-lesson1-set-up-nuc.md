---
title: "Simulované zařízení & brány Azure IoT - lekci 1: nastavení NUC | Microsoft Docs"
description: "Nastavte Intel NUC fungovat jako bránu IoT mezi senzor a Azure IoT Hub a shromažďovat informace o senzor odeslat do služby IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: yjianfeng
tags: 
keywords: "IOT brány, intel nuc nuc počítače, DE3815TYKE"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f41d6b2e-9b00-40df-90eb-17d824bea883
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b87974be9570f7d03fe84ae0a1d1fa7e346ff189
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Nastavit Intel NUC jako bránu IoT

## <a name="what-you-will-do"></a>Co provedete

- Nastavte Intel NUC jako bránu IoT.
- Nainstalujte balíček Azure IoT Edge na Intel NUC.
- Spuštění ukázkové aplikace na "hello_world" Intel NUC ověření funkci brány.
Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Postup připojení Intel NUC s periferních zařízení.
- Postup instalace a aktualizace požadované balíčky na Intel NUC pomocí inteligentní Správce balíčků.
- Postup spuštění ukázkové aplikace "hello_world" ověření funkci brány.

## <a name="what-you-need"></a>Co potřebujete

- Intel NUC Kit DE3815TYKE s sadě Intel IoT brány (Linux větru oblasti * 7.0.0.13) předinstalována.
- Kabel Ethernet.
- Klávesnice.
- HDMI nebo VGA kabel.
- Monitorování s k portu HDMI nebo VGA.

![Brána Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a>Připojit Intel NUC s periferních zařízení

Následující obrázek je příkladem NUC Intel, který je připojen k různým periferních zařízení:

1. Připojení k klávesnici.
2. Připojené k monitorování kabel VGA nebo HDMI kabel.
3. Připojený k drátové síti pomocí kabelu Ethernet.
4. Připojení k napájení s napájecí kabel.

![Intel NUC připojené k periferních zařízení](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Připojení k systému Intel NUC od hostitelského počítače pomocí protokolu Secure Shell (SSH)

Zde je třeba klávesnice a monitorování, který chcete získat IP adresu vašeho NUC zařízení. Pokud už znáte IP adresu, můžete přeskočit ke kroku 3 v této části.

1. Zapněte Intel NUC stisknutím tlačítka napájení a protokolu v systému.

   Výchozí uživatelské jméno a heslo jsou obě `root`.

2. Získat IP adresu NUC spuštěním `ifconfig` příkaz. Tento krok se provádí na NUC zařízení.

   Tady je příklad výstupu příkazu.

   ![výstup ifconfig zobrazující NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   V tomto příkladu hodnota, která odpovídá `inet addr:` je IP adresa, která musíte, když máte v plánu připojit se k Intel NUC vzdáleně z hostitelského počítače.

3. Použijte jeden z následující klienti SSH ze hostitelský počítač připojit k Intel NUC.

   - [PuTTY](http://www.putty.org/) pro systém Windows.
   - Sestavení v SSH klienta na Ubuntu nebo systému macOS.

   Je efektivnější a produktivitu pracovat Intel NUC z hostitelského počítače. Je nutné IP adresu, uživatelské jméno a heslo pro připojení NUC prostřednictvím klient SSH. Tady je příklad použití SSH klienta v systému macOS.
   ![Klient SSH, které jsou spuštěné v systému macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-the-azure-iot-edge-package"></a>Nainstalovat balíček Azure IoT Edge

Balíček Azure IoT Edge obsahuje předem kompilovaném binární soubory sady SDK a jeho závislosti. Tyto binární soubory jsou Azure IoT Edge, sady SDK Azure IoT a odpovídající nástroje. Balíček obsahuje také "hello_world" ukázkovou aplikaci, která slouží k ověření funkci brány. IoT okraj je základní součástí brány. K instalaci balíčku, postupujte takto:

1. Přidejte úložiště cloudu IoT spuštěním následujících příkazů v okně terminálu:

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   `rpm` Příkaz importuje klíč ot. / min. `smart channel` Příkaz přidá rpm kanál pro inteligentní Správce balíčků. Před spuštěním `smart update` příkazu, uvidíte výstup jako níže.

   ![ot. / min a inteligentní kanál příkazy výstup](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. Nainstalujte balíček spuštěním následujícího příkazu:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`je název balíčku. `smart install` Příkaz slouží k instalaci balíčku.

   Po instalaci balíčku Intel NUC budou fungovat jako brána.

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a>Spuštění ukázkové aplikace Azure IoT Edge "hello_world"

Přejděte na `azureiotgatewaysdk/samples` a spusťte ukázkové "hello_world" ukázkovou aplikaci. Tato ukázková aplikace vytvoří brány z `hello_world.json` soubor a používá základní součásti architektury Azure IoT okraj do protokolu zprávu hello world soubor každých 5 sekund.

Ukázka "hello_world" ukázkovou aplikaci můžete spustit tak, že spustíte následující příkaz:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Pokud funkci brány funguje správně, vytvoří ukázkovou aplikaci následující výstup:

![výstup aplikace](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Souhrn

Blahopřejeme! Dokončili jste nastavení Intel NUC jako brána. Teď jste připravení přejít Další lekce nastavit hostitelského počítače, vytvoření služby Azure IoT hub a zaregistrovat logické zařízení Azure IoT hub.

## <a name="next-steps"></a>Další kroky
[Příprava hostitelský počítač a Azure IoT hub](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
