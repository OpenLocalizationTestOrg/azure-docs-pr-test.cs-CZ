---
title: "Připojení zařízení pomocí jazyka C na mbed | Microsoft Docs"
description: "Popisuje, jak se připojit k Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v jazyce C spuštění v zařízení s mbed zařízení."
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
ms.openlocfilehash: ef7b78f85a787f8fbe22c0e26aa34f0cd1685d58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Připojte zařízení k monitorování předkonfigurované řešení vzdáleného (mbed)

## <a name="scenario-overview"></a>Přehled scénáře
V tomto scénáři vytvoříte zařízení, které odesílá následující telemetrii do [předkonfigurovaného řešení][lnk-what-are-preconfig-solutions] vzdáleného monitorování:

* Venkovní teplota
* Vnitřní teplota
* Vlhkost

Kód v zařízení pro zjednodušení generuje ukázkové hodnoty, ale doporučujeme vám ukázku rozšířit připojením skutečných senzorů k zařízení a odesíláním skutečné telemetrie.

Zařízení je také schopné odpovídat na metody vyvolané z řídicího panelu řešení a na požadované hodnoty vlastností nastavené na řídicím panelu řešení.

K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].

## <a name="before-you-start"></a>Než začnete
Než začnete psát kód pro zařízení, je nutné zřídit předkonfigurované řešení vzdáleného monitorování a v něm zřídit nové vlastní zařízení.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Zřízení předkonfigurovaného řešení vzdáleného monitorování
Zařízení, které v tomto kurzu vytvoříte, odesílá data do instance předkonfigurovaného řešení [vzdáleného monitorování][lnk-remote-monitoring]. Pokud jste ve svém účtu Azure ještě nezřídili předkonfigurované řešení vzdáleného monitorování, použijte následující postup:

1. Na stránce <https://www.azureiotsuite.com/> můžete vytvořit řešení kliknutím na **+**.
2. Kliknutím na **Vybrat** na panelu **Vzdálené monitorování** vytvořte řešení.
3. Na stránce **Vytvořit řešení vzdáleného monitorování** zadejte **Název řešení** podle vašeho výběru, vyberte **Oblast**, do které chcete řešení nasadit, a vyberte předplatné Azure, které chcete použít. Potom klikněte na **Vytvořit řešení**.
4. Počkejte, dokud proces zřizování neskončí.

> [!WARNING]
> Předkonfigurovaná řešení využívají fakturovatelné služby Azure. Abyste se vyhnuli zbytečným poplatkům, nezapomeňte předkonfigurované řešení odebrat ze svého předplatného, jakmile s ním budete hotovi. Předkonfigurované řešení můžete ze svého předplatného úplně odebrat na stránce <https://www.azureiotsuite.com/>.
> 
> 

Po skončení procesu zřizování řešení vzdáleného monitorování klikněte na **Spustit**. Ve vašem prohlížeči se otevře řídicí panel řešení.

![Řídicí panel řešení][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Zřízení zařízení v řešení vzdáleného monitorování
> [!NOTE]
> Pokud jste už ve svém řešení zařízení zřídili, můžete tento krok přeskočit. Při vytváření klientské aplikace potřebujete znát přihlašovací údaje zařízení.
> 
> 

Aby se zařízení mohlo připojit k předkonfigurovanému řešení, musí se identifikovat ve službě IoT Hub pomocí platných přihlašovacích údajů. Přihlašovací údaje zařízení můžete zjistit z řídicího panelu řešení. Přihlašovací údaje zařízení vložíte do klientské aplikace později v tomto kurzu.

Pokud chcete přidat zařízení do řešení vzdáleného monitorování, proveďte na řídicím panelu řešení následující kroky:

1. V levém dolním rohu řídicího panelu klikněte na **Přidat zařízení**.
   
   ![Přidání zařízení][1]
2. Na panelu **Vlastní zařízení** klikněte na **Přidat nové**.
   
   ![Přidání vlastního zařízení][2]
3. Vyberte možnost **Definovat vlastní ID zařízení**. Zadejte ID zařízení, třeba **mydevice**, klikněte na **Zkontrolovat ID** pro ověření, že se tento název ještě nepoužívá, a potom zřiďte zařízení kliknutím na **Vytvořit**.
   
   ![Přidání ID zařízení][3]
4. Poznamenejte si přihlašovací údaje zařízení (ID zařízení, název hostitele služby IoT Hub a Klíč zařízení). Klientská aplikace potřebuje tyto hodnoty pro připojení k řešení vzdáleného monitorování. Potom klikněte na **Done** (Hotovo).
   
    ![Zobrazení přihlašovacích údajů zařízení][4]
5. V seznamu zařízení na řídicím panelu řešení vyberte své zařízení. Pak na panelu **Podrobnosti o zařízení** klikněte na **Povolit zařízení**. Stav vašeho zařízení je teď **Spuštěno**. Řešení vzdáleného monitorování teď může z vašeho zařízení přijímat telemetrii a vyvolávat v něm metody.

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-the-c-sample-solution"></a>Sestavte a spusťte ukázkové řešení C

Následující pokyny popisují kroky pro připojení [povoleno mbed FRDM K64F Freescale] [ lnk-mbed-home] zařízení do řešení vzdáleného monitorování.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Zařízení s mbed připojte k počítači sítě a vzdálené ploše

1. Připojte zařízení mbed k síti pomocí kabelu Ethernet. Tento krok je nezbytný, protože ukázkové aplikace vyžaduje přístup k Internetu.

1. V tématu [Začínáme s mbed] [ lnk-mbed-getstarted] k připojení zařízení mbed pro stolní počítač.

1. Pokud počítače se systémem Windows, přečtěte si téma [konfigurace počítače] [ lnk-mbed-pcconnect] nakonfigurujte sériového portu zařízení mbed.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Vytvoření projektu mbed a importovat ukázkový kód

Postupujte podle těchto kroků pro přidání do projektu mbed některé ukázkový kód. Import vzdálené monitorování starter projekt a poté změňte projekt, který používá protokol MQTT místo protokolu AMQP. Nyní budete muset použít protokol MQTT používat funkce správy zařízení služby IoT Hub.

1. Ve webovém prohlížeči, přejděte mbed.org [vývojáře lokality](https://developer.mbed.org/). Pokud nemáte registraci, zobrazí se možnost vytvoření účtu (je to zdarma). Jinak Přihlaste se pomocí přihlašovacích údajů účtu. Pak klikněte na tlačítko **kompilátoru** v pravém horním rohu stránky. Tato akce integruje do *prostoru* rozhraní.

1. Ujistěte se, že se zobrazí v pravém horním rohu okna hardwaru platformu, kterou používáte, nebo klikněte na ikonu v pravém rohu vyberte platformu hardwaru.

1. Klikněte na tlačítko **Import** v hlavní nabídce. Pak klikněte na tlačítko **kliknutím sem importovat z adresy URL**.
   
    ![Spusťte import do pracovního prostoru mbed][6]

1. Zadejte odkaz pro https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ kód ukázka v místním okně, pak klikněte na **Import**.
   
    ![Import ukázkový kód do pracovního prostoru mbed][7]

1. Zobrazí se v okně kompilátoru mbed, import tento projekt taky importuje různých knihoven. Některé zadané a udržovat tým Azure IoT ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), zatímco jiné jsou dostupné v katalogu knihoven mbed knihovny třetích stran.
   
    ![Zobrazení mbed projektu][8]

1. V **prostoru programu**, klikněte pravým tlačítkem myši **iothub\_amqp\_přenosu** knihovny, klikněte na tlačítko **odstranit**a potom klikněte na  **OK** k potvrzení.

1. V **prostoru programu**, klikněte pravým tlačítkem myši **azure\_amqp\_c** knihovny, klikněte na tlačítko **odstranit**a potom klikněte na **OK** k potvrzení.

1. Klikněte pravým tlačítkem myši **remote_monitoring** projektu v **prostoru programu**, vyberte **knihovny importu**, pak vyberte **z adresy URL**.
   
    ![Spuštění knihovny importu do pracovního prostoru mbed][6]

1. V místním okně, zadejte odkaz pro https://developer.mbed.org/users/AzureIoTClient/code/iothub knihovny přenosu MQTT\_mqtt\_přenosu / klikněte **Import**.
   
    ![Import knihovny do pracovního prostoru mbed][12]

1. Opakujte předchozí krok pro přidání knihovně MQTT z https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.

1. Pracovní prostor teď vypadá takto:

    ![Zobrazení mbed prostoru][13]

1. Otevřete vzdálených\_monitoring\remote_monitoring.c souboru a nahradit stávající `#include` příkazy následujícím kódem:

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
1. Odstranit všechny zbývající kód ve vzdáleném\_monitoring\remote\_monitoring.c souboru.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Sestavit a spustit ukázku

Přidejte kód, který má být vyvolán **vzdáleného\_monitorování\_spustit** funkce a potom sestavit a spustit aplikaci zařízení.

1. Přidat **hlavní** funkce s následujícím kódem na konci vzdálených\_monitoring.c soubor má být vyvolán **vzdáleného\_monitorování\_spustit** funkce:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Klikněte na tlačítko **zkompilovat** k vytvoření programu. Můžete bezpečně ignorovat všechna upozornění, ale pokud sestavení generuje chyby, opravte je než budete pokračovat.

1. Pokud sestavení úspěšné, web kompilátoru mbed generuje soubor .bin s názvem vašeho projektu a soubory ke stažení do místního počítače. Zkopírujte soubor .bin do zařízení. Ukládání souboru .bin do zařízení způsobí, že zařízení restartovat a poté spusťte program obsažené v souboru .bin. Stisknutím tlačítka Obnovit v zařízení mbed můžete ručně restartovat program kdykoli.

1. Připojte k zařízení pomocí protokolu SSH klienta aplikace, jako je například PuTTY. Můžete určit sériového portu, který zařízení používá kontrolou Správci zařízení.
   
    ![][11]

1. V PuTTY, klikněte na **sériové** typ připojení. V 9600 přenosová obvykle připojení zařízení, takže zadejte 9600 v **rychlost** pole. Pak klikněte na tlačítko **otevřete**.

1. Program spustí provádění. Možná budete muset resetovat panel (stiskněte kombinaci kláves CTRL + Break nebo stiskněte tlačítko Obnovit panelu) Pokud program nespustí automaticky při připojení.
   
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
