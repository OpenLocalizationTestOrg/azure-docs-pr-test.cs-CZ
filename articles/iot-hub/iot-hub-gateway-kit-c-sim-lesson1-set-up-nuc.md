---
title: "Simulované zařízení & brány Azure IoT - lekci 1: nastavení NUC | Microsoft Docs"
description: "Nastavte Intel NUC toowork jako bránu IoT mezi senzor a Azure IoT Hub toocollect senzor informace a odeslat tooIoT rozbočovače."
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
ms.openlocfilehash: c2146c7cd8ca54adbd0af279364ec8484be1b45b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Nastavit Intel NUC jako bránu IoT

## <a name="what-you-will-do"></a>Co provedete

- Nastavte Intel NUC jako bránu IoT.
- Nainstalujte balíček Azure IoT Edge hello Intel NUC.
- Spuštění ukázkové aplikace na "hello_world" funkci Intel NUC tooverify hello brány.
Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Jak tooconnect Intel NUC s periferních zařízení.
- Jak tooinstall a aktualizace hello požadované balíčky na Intel NUC pomocí hello inteligentní Správce balíčků.
- Jak toorun hello "hello_world" ukázkové aplikace tooverify hello brány funkce.

## <a name="what-you-need"></a>Co potřebujete

- Intel NUC Kit DE3815TYKE s hello Intel IoT brány softwaru Suite (Linux větru oblasti * 7.0.0.13) předinstalována.
- Kabel Ethernet.
- Klávesnice.
- HDMI nebo VGA kabel.
- Monitorování s k portu HDMI nebo VGA.

![Brána Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a>Připojit Intel NUC s hello periferních zařízení

Následující obrázek Hello je příkladem NUC Intel, který je připojen k různým periferních zařízení:

1. Připojené tooa klávesnice.
2. Připojení toohello monitorování kabel VGA nebo HDMI kabel.
3. Připojené tooa drátové síti pomocí kabelu Ethernet.
4. Zdroj napájení připojené toohello s napájecí kabel.

![Intel NUC připojené tooperipherals](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Připojení systému Intel NUC toohello od hostitelského počítače pomocí protokolu Secure Shell (SSH)

Zde je třeba klávesnice a monitorování tooget hello IP adresa vašeho NUC zařízení. Pokud už znáte hello IP adresu, můžete přeskočit toostep 3 v této části.

1. Zapněte Intel NUC stisknutím tlačítka napájení hello a protokolu systému hello.

   Hello výchozí uživatelské jméno a heslo jsou obě `root`.

2. Získat IP adresu hello NUC spuštěním hello `ifconfig` příkaz. Tento krok se provádí na hello NUC zařízení.

   Tady je příklad výstupu příkazu hello.

   ![výstup ifconfig zobrazující NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   V tomto příkladu hello hodnotu, která odpovídá `inet addr:` je hello IP adresu, které musíte při plánování tooconnect vzdáleně z počítače tooIntel hostitele NUC.

3. Použijte jeden z následujících klientů SSH z vaší hostitele počítače tooconnect tooIntel NUC hello.

   - [PuTTY](http://www.putty.org/) pro systém Windows.
   - Hello sestavení v klient SSH na Ubuntu nebo systému macOS.

   Je efektivnější a produktivitu toooperate na Intel NUC z hostitelského počítače. Budete potřebovat hello hello IP adresu, uživatelské jméno a heslo tooconnect hello NUC prostřednictvím klient SSH. Zde je klient hello příklad použití SSH v systému macOS.
   ![Klient SSH, které jsou spuštěné v systému macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-hello-azure-iot-edge-package"></a>Nainstalovat balíček Azure IoT Edge hello

balíček Azure IoT Edge Hello obsahuje hello předem kompilovaném binární soubory hello SDK a jeho závislosti. Tyto binární soubory jsou Azure IoT Edge, hello sady SDK Azure IoT a odpovídající nástroje hello. balíček Hello také obsahuje ukázkovou aplikaci "hello_world", která je funkcí brány použité toovalidate hello. IoT okraj je hello základní součástí hello brány. tooinstall hello balíček, postupujte takto:

1. Spuštěním následujících příkazů v okně terminálu hello přidejte hello IoT cloudové úložiště:

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   Hello `rpm` příkaz importuje hello klíč ot. / min. Hello `smart channel` příkaz přidá hello rpm kanálu toohello inteligentní Správce balíčků. Před spuštěním hello `smart update` příkazu, uvidíte výstup jako níže.

   ![ot. / min a inteligentní kanál příkazy výstup](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. Instalovat balíček hello spuštěním hello následující příkaz:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`je název hello hello balíčku. Hello `smart install` příkaz je použité tooinstall hello balíčku.

   Po instalaci balíčku hello Intel NUC je očekávané toowork jako brána.

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a>Spustit hello Azure IoT Edge "hello_world" ukázkové aplikace

Přejděte příliš`azureiotgatewaysdk/samples` a spusťte hello ukázka "hello_world" ukázkovou aplikaci. Tato ukázková aplikace vytvoří brány z hello `hello_world.json` soubor a používá základní součástí hello Azure IoT Edge architektura toolog souboru tooa zprávy hello world hello každých 5 sekund.

Hello ukázka "hello_world" ukázkovou aplikaci můžete spustit spuštěním hello následující příkaz:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Ukázková aplikace Hello vytváří hello následující výstup, pokud funkci brány hello funguje správně:

![výstup aplikace](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Souhrn

Blahopřejeme! Dokončili jste nastavení Intel NUC jako brána. Nyní jste připraveni toomove na toohello Další lekce tooset až hostitelského počítače vytvoření služby Azure IoT hub a zaregistrujte logické zařízení Azure IoT hub.

## <a name="next-steps"></a>Další kroky
[Příprava hostitelský počítač a Azure IoT hub](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
