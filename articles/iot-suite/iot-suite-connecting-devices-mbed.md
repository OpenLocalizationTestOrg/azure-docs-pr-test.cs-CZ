---
title: "aaaConnect zařízení pomocí jazyka C na mbed | Microsoft Docs"
description: "Popisuje, jak tooconnect toohello zařízení Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v jazyce C spuštění v zařízení s mbed."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 9551075e-dcf9-488f-943e-d0eb0e6260be
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: dobett
ROBOTS: NOINDEX
ms.openlocfilehash: dcd1e74635e8dec678a59bff060a73f7cfabd124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-mbed"></a>Připojit vaše zařízení toohello (mbed) předkonfigurovanému řešení vzdáleného monitorování

## <a name="scenario-overview"></a>Přehled scénáře
V tomto scénáři vytvoříte zařízení, které odesílá hello následující telemetrie toohello vzdálené monitorování [předkonfigurované řešení][lnk-what-are-preconfig-solutions]:

* Venkovní teplota
* Vnitřní teplota
* Vlhkost

Pro jednoduchost hello kódu na zařízení hello generuje ukázkové hodnoty, ale doporučujeme vám tooextend hello ukázka připojením zařízení tooyour skutečné senzory a odesláním skutečné telemetrie.

Hello zařízení je také možné toorespond toomethods volat z řídicí panel řešení hello a potřeby vlastnost hodnotami nastavenými v řídicí panel řešení hello.

toocomplete tohoto kurzu potřebujete aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].

## <a name="before-you-start"></a>Než začnete
Než začnete psát kód pro zařízení, je nutné zřídit předkonfigurované řešení vzdáleného monitorování a v něm zřídit nové vlastní zařízení.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Zřízení předkonfigurovaného řešení vzdáleného monitorování
Hello zařízení v tomto kurzu vytvoříte odesílá data tooan instanci hello [vzdálené monitorování] [ lnk-remote-monitoring] předkonfigurované řešení. Pokud jste ještě nezřídili hello vzdálené monitorování předkonfigurované řešení v účtu Azure, použijte hello následující kroky:

1. Na hello <https://www.azureiotsuite.com/> klikněte na tlačítko  **+**  toocreate řešení.
2. Klikněte na tlačítko **vyberte** na hello **vzdálené monitorování** panelu toocreate řešení.
3. Na hello **vytvořit řešení vzdáleného sledování** stránky, zadejte **název řešení** podle vaší volby, vyberte hello **oblast** toodeploy do mají a vyberte hello Azure toouse toowant předplatné. Potom klikněte na **Vytvořit řešení**.
4. Počkejte na dokončení procesu zřizování hello.

> [!WARNING]
> Hello předkonfigurované řešení využívají fakturovatelný služby Azure. Ujistěte se, tooremove hello předkonfigurované řešení ze svého předplatného, když jste hotovi s ním tooavoid všechny nepotřebné poplatky. Předkonfigurované řešení můžete úplně odebrat ze svého předplatného návštěvou hello <https://www.azureiotsuite.com/> stránky.
> 
> 

Po dokončení hello zřizování pro hello řešení vzdáleného sledování, klikněte na tlačítko **spusťte** řídicí panel řešení hello tooopen v prohlížeči.

![Řídicí panel řešení][img-dashboard]

### <a name="provision-your-device-in-hello-remote-monitoring-solution"></a>Zřízení zařízení v řešení vzdáleného sledování hello
> [!NOTE]
> Pokud jste už ve svém řešení zařízení zřídili, můžete tento krok přeskočit. Přihlašovací údaje tooknow hello zařízení musíte při vytváření hello klientské aplikace.
> 
> 

Pro zařízení tooconnect toohello předkonfigurované řešení, se musí identifikovat tooIoT centra pomocí platných přihlašovacích údajů. Přihlašovací údaje hello zařízení můžete načíst z řídicí panel řešení hello. Přihlašovací údaje zařízení hello zahrnete do klientské aplikace později v tomto kurzu.

tooadd zařízení tooyour řešení vzdáleného monitorování, dokončení hello následující kroky v řídicí panel řešení hello:

1. V hello levém dolním rohu hello řídicí panel, klikněte na **přidání zařízení**.
   
   ![Přidání zařízení][1]
2. V hello **vlastní zařízení** panelu, klikněte na tlačítko **přidat nový**.
   
   ![Přidání vlastního zařízení][2]
3. Vyberte možnost **Definovat vlastní ID zařízení**. Zadejte ID zařízení, jako **mydevice**, klikněte na tlačítko **Zkontrolujte ID** tooverify tento název již není používána a pak klikněte na tlačítko **vytvořit** tooprovision hello zařízení.
   
   ![Přidání ID zařízení][3]
4. Nastavit hello zařízení Poznámka: přihlašovací údaje (ID zařízení, název hostitele centra IoT a klíč zařízení). Klientské aplikace, musí tyto hodnoty tooconnect toohello řešení vzdáleného sledování. Potom klikněte na **Done** (Hotovo).
   
    ![Zobrazení přihlašovacích údajů zařízení][4]
5. Vyberte zařízení v seznamu zařízení hello v řídicí panel řešení hello. Potom v hello **podrobnosti o zařízení** panelu, klikněte na tlačítko **povolit zařízení**. Hello stav zařízení je nyní **systémem**. řešení vzdáleného monitorování Hello teď můžete přijímat telemetrická data ze zařízení a volat metody na hello zařízení.

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-hello-c-sample-solution"></a>Sestavte a spusťte ukázkové řešení hello C

Hello následující pokyny popisují postup hello připojení [povoleno mbed FRDM K64F Freescale] [ lnk-mbed-home] zařízení toohello řešení vzdáleného sledování.

### <a name="connect-hello-mbed-device-tooyour-network-and-desktop-machine"></a>Připojit hello mbed zařízení tooyour sítě a stolní počítače

1. Připojte hello mbed zařízení tooyour síti pomocí kabelu Ethernet. Tento krok je nezbytný, protože hello ukázkové aplikace vyžaduje přístup k Internetu.

1. V tématu [Začínáme s mbed] [ lnk-mbed-getstarted] tooconnect mbed zařízení tooyour ploše počítače.

1. Pokud počítače se systémem Windows, přečtěte si téma [konfigurace počítače] [ lnk-mbed-pcconnect] tooconfigure sériového portu přístup tooyour mbed zařízení.

### <a name="create-an-mbed-project-and-import-hello-sample-code"></a>Vytvoření projektu mbed a importovat hello ukázkový kód

Postupujte podle těchto kroků tooadd některé ukázkový kód tooan mbed projekt. Import projektu starter vzdálené monitorování hello a poté změňte hello projektu toouse hello MQTT protokol místo hello protokolu AMQP. V současné době je nutné toouse hello MQTT protokol toouse hello funkce správy zařízení služby IoT Hub.

1. Ve webovém prohlížeči, přejděte toohello mbed.org [vývojáře lokality](https://developer.mbed.org/). Pokud nemáte registraci, zobrazí toocreate možnost účet (je to zdarma). Jinak Přihlaste se pomocí přihlašovacích údajů účtu. Pak klikněte na tlačítko **kompilátoru** v horním pravém horním rohu stránky hello hello. Tato akce přináší, toohello *prostoru* rozhraní.

1. Zajistěte, aby se zobrazí v horním pravém horním rohu okna hello hello hello hardwarová platforma, kterou používáte, nebo klikněte na tlačítko hello ikonu v pravém rohu tooselect hello hardwarová platforma.

1. Klikněte na tlačítko **Import** v hlavní nabídce hello. Pak klikněte na tlačítko **tooimport z adresy URL, klikněte sem**.
   
    ![Spusťte import toombed prostoru][6]

1. V místním okně hello, zadejte hello odkaz pro hello ukázkový kód https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ a klikněte na **Import**.
   
    ![Import ukázkový kód toombed prostoru][7]

1. Zobrazí se v okně kompilátoru mbed hello, import tento projekt taky importuje různých knihoven. Některé zadané a udržovat tým Azure IoT hello ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), zatímco jiné jsou dostupné v katalogu knihoven mbed hello knihovny třetích stran.
   
    ![Zobrazení mbed projektu][8]

1. V hello **prostoru programu**, hello klikněte pravým tlačítkem na **iothub\_amqp\_přenosu** knihovny, klikněte na tlačítko **odstranit**a potom klikněte na **OK** tooconfirm.

1. V hello **prostoru programu**, klikněte pravým tlačítkem na hello **azure\_amqp\_c** knihovny, klikněte na tlačítko **odstranit**a potom klikněte na **OK**  tooconfirm.

1. Klikněte pravým tlačítkem na hello **remote_monitoring** projekt v hello **prostoru programu**, vyberte **knihovny importu**, pak vyberte **z adresy URL**.
   
    ![Spuštění knihovny importu toombed prostoru][6]

1. V místním okně hello, zadejte hello odkaz pro hello MQTT přenosu knihovny https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_přenosu / klikněte **Import**.
   
    ![Importovat knihovny toombed prostoru][12]

1. Hello zopakujte předchozí krok tooadd hello MQTT knihovny z https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.

1. Pracovní prostor se teď vypadá jako hello následující:

    ![Zobrazení mbed prostoru][13]

1. Otevřete hello vzdálené\_monitoring\remote_monitoring.c souboru a nahradit stávající hello `#include` příkazy s hello následující kód:

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"

    #ifdef MBED_BUILD_TIMESTAMP
    #include "certs.h"
    #endif // MBED_BUILD_TIMESTAMP
    ```
1. Odstranit všechny hello zbývající kód v hello vzdálené\_monitoring\remote\_monitoring.c souboru.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a>Sestavení a spuštění ukázkových hello

Přidat kód tooinvoke hello **vzdáleného\_monitorování\_spustit** funkce a potom sestavení a spuštění aplikace hello zařízení.

1. Přidat **hlavní** funkce s následujícím kódem na konci hello hello vzdálené\_monitoring.c souboru tooinvoke hello **vzdáleného\_monitorování\_spustit** funkce:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Klikněte na tlačítko **zkompilovat** toobuild programu hello. Můžete bezpečně ignorovat všechna upozornění, ale pokud hello sestavení generuje chyby, opravte je než budete pokračovat.

1. Pokud sestavení hello úspěšné, hello mbed kompilátoru web generuje soubor .bin s hello název projektu a ji stáhne tooyour místního počítače. Zkopírujte hello .bin souboru toohello zařízení. Ukládání zařízení toohello soubor .bin hello způsobí, že zařízení toorestart hello a spusťte program hello obsažené v souboru .bin hello. Kdykoli můžete ručně restartovat programu hello stisknutím tlačítka resetování hello v hello mbed zařízení.

1. Připojte zařízení toohello pomocí SSH klienta aplikace, jako je například PuTTY. Můžete určit hello sériového portu, který zařízení používá kontrolou Správci zařízení.
   
    ![][11]

1. V PuTTY, klikněte na tlačítko hello **sériové** typ připojení. Hello zařízení obvykle připojí na 9600 přenosová, takže zadejte 9600 hello **rychlost** pole. Pak klikněte na tlačítko **otevřete**.

1. spuštění programu Hello provádění. Pokud hello program nespustí automaticky při připojení, mohou mít tooreset hello Tabule (stiskněte kombinaci kláves CTRL + Break nebo Tabule hello stiskněte tlačítko pro obnovení).
   
    ![][10]

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png
[12]: ./media/iot-suite-connecting-devices-mbed/mbed7.png
[13]: ./media/iot-suite-connecting-devices-mbed/mbed8.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
