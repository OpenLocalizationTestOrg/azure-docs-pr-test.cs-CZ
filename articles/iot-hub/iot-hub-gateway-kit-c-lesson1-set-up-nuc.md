---
title: "Zařízení SensorTag & brány Azure IoT - lekci 1: nastavení Intel NUC | Microsoft Docs"
description: "Nastavte Intel NUC toowork jako bránu IoT mezi senzor a Azure IoT Hub toocollect senzor informace a odeslat tooIoT rozbočovače."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT brány, intel nuc nuc počítače, DE3815TYKE"
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7c3ab3b014713c7facb86b8e8622d70e60a960e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Nastavit Intel NUC jako bránu IoT
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a>Co provedete

- Nastavte Intel NUC jako bránu IoT.
- Nainstalujte balíček Azure IoT Edge hello hello Intel NUC.
- Spuštění ukázkové aplikace na "hello_world" hello funkci Intel NUC tooverify hello brány.

  > Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Jak tooconnect Intel NUC s periferních zařízení.
- Jak tooinstall a aktualizace hello požadované balíčky na Intel NUC pomocí hello inteligentní Správce balíčků.
- Jak toorun hello "hello_world" ukázkové aplikace tooverify hello brány funkce.

## <a name="what-you-need"></a>Co potřebujete

- Intel NUC Kit DE3815TYKE s hello Intel IoT brány softwaru Suite (Linux větru oblasti * 7.0.0.13) předinstalována. [Kliknutím sem toopurchase Groove IoT komerční brány Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).
- Kabel Ethernet.
- Klávesnice.
- HDMI nebo VGA kabel.
- Monitorování s k portu HDMI nebo VGA.
- Volitelné: [Texas Instruments Sensortag (CC2650STK)](http://www.ti.com/tool/cc2650stk)

![Brána Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a>Připojit Intel NUC s hello periferních zařízení

Následující obrázek Hello je příkladem NUC Intel, který je připojen k různým periferních zařízení:

1. Připojené tooa klávesnice.
2. Připojený tooa monitorování kabel VGA nebo HDMI kabel.
3. Připojené tooa drátové síti pomocí kabelu Ethernet.
4. Zdroj napájení připojené tooa s napájecí kabel.

![Intel NUC připojené tooperipherals](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Připojení systému Intel NUC toohello od hostitelského počítače pomocí protokolu Secure Shell (SSH)

Budete potřebovat klávesnici a monitorování tooget hello IP adresu vašeho zařízení Intel NUC. Pokud už znáte hello IP adresu, můžete přeskočit napřed toostep 3 v této části.

1. Zapněte hello Intel NUC stisknutím tlačítka napájení hello a znovu přihlásili.

   Hello výchozí uživatelské jméno a heslo jsou obě `root`.

       > Hit hello enter key on your keyboard if you see either of hello following errors when you boot: 'A TPM error (7) occurred attempting tooread a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. Získat IP adresu hello hello Intel NUC spuštěním hello `ifconfig` příkazu na zařízení Intel NUC hello.

   Tady je příklad výstupu příkazu hello.

   ![výstup ifconfig zobrazující Intel NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   V tomto příkladu hello hodnotu, která odpovídá `inet addr:` je hello IP adresu, která je nutné, když připojujete toohello Intel NUC z hostitelského počítače.

3. Použijte jeden z následujících klientů SSH z vaší hostitele počítače tooconnect tooIntel NUC hello.

    - [PuTTY](http://www.putty.org/) pro systém Windows.
    - Hello integrovaného klienta SSH na Ubuntu nebo systému macOS.

   Je efektivnější a produktivitu toooperate Intel NUC z hostitelského počítače. Budete potřebovat hello Intel NUC IP adresu, uživatelské jméno a heslo tooit tooconnect prostřednictvím klientem SSH. Tady je příklad, který používá klienta SSH v systému macOS.
   ![Klient SSH, které jsou spuštěné v systému macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-hello-azure-iot-edge-package"></a>Nainstalovat balíček Azure IoT Edge hello

balíček Azure IoT Edge Hello obsahuje předem kompilovaném binární soubory hello IoT okraj a jeho závislé součásti. Tyto binární soubory jsou Azure IoT Edge, hello sady SDK Azure IoT a odpovídající nástroje hello. Hello balíček obsahuje také "hello_world" Vzorová aplikace je funkce použité toovalidate hello brány. IoT okraj je hello základní součástí hello brány. 

Postupujte podle těchto kroků tooinstall hello balíčku.

1. Spuštěním následujících příkazů v okně terminálu hello přidejte hello IoT cloudové úložiště:

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > Zadejte "y", když budete vyzváni too'Include tento kanál? "
   
   Pokud se zobrazí `import read failed(-1)` chybu, hello použijte následující příkazy tooresolve hello problému:
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   Hello `rpm` příkaz importuje hello klíč ot. / min. Hello `smart channel` příkaz přidá hello rpm kanálu toohello inteligentní Správce balíčků. Před spuštěním hello `smart update` příkaz, zobrazí se výstup jako níže.

   ![ot. / min a inteligentní kanál příkazy výstup](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. Spusťte příkaz hello inteligentní aktualizace:

   ```bash
   smart update
   ```

3. Instalovat balíček brány Azure IoT hello spuštěním hello následující příkaz:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`je název hello hello balíčku. Hello `smart install` příkaz je použité tooinstall hello balíčku.

    > Hello spusťte následující příkaz, pokud se zobrazí tato chyba: veřejný klíč není k dispozici

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > Restartujte hello Intel NUC, pokud se zobrazí tato chyba: 'žádný balíček poskytuje util. linux vývojářů.

   Po instalaci balíčku hello Intel NUC je připraven toofunction jako brána.

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a>Spustit hello Azure IoT Edge "hello_world" ukázkové aplikace

Následující ukázkové aplikace Hello vytvoří brány z `hello_world.json` soubor a používá hello základní součásti služby Azure IoT Edge architektura toolog soubor hello world zprávy tooa (log.txt) každých 5 sekund.

Ukázka programu hello Hello World můžete spustit spuštěním hello následující příkazy:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Umožní aplikaci Hello World hello spustit několik minut a poté stiskněte klíče toostop hello zadejte ji.
![výstup aplikace](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

> Můžete ignorovat všechny 'handle(NULL) neplatný argument' chyby, které se zobrazí po kliknutí zadejte.

Můžete ověřit, že brány hello proběhla úspěšně otevřením hello log.txt souboru, který je teď ve složce hello_world ![log.txt zobrazení adresáře](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)

Otevřete log.txt pomocí hello následující příkaz:

```bash
vim log.txt
```

Zobrazí se obsah hello log.txt, který bude výstup naformátovaný JSON hello protokolování zpráv, které byly napsané každých 5 sekund modulem Hello, World hello brány.
![zobrazení adresáře log.txt](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Souhrn

Blahopřejeme! Dokončili jste nastavení Intel NUC jako brána. Nyní jste připraveni toomove na toohello Další lekce tooset až hostitelského počítače, vytvořte Azure IoT Hub a zaregistrovat logické zařízení Azure IoT Hub.

## <a name="next-steps"></a>Další kroky
[Pomocí IoT brány tooconnect tooAzure zařízení IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

