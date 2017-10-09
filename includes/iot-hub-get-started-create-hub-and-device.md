## <a name="create-an-iot-hub"></a>Vytvoření centra IoT

1. V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **Internet věcí** > **IoT Hub**.

   ![Vytvoření služby IoT hub v portálu Azure hello](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
2. V hello **služby IoT hub** podokně zadejte následující informace pro službu IoT hub hello:

     **Název**: Zadejte název hello služby IoT hub. Pokud zadáte název hello je platný, se zobrazí zelená značka zaškrtnutí.

     **Úroveň ceny a škálování**: Vyberte hello **F1 - volné** vrstvy. Tato možnost je pro tuto ukázku dostačující. Další informace najdete v tématu hello [cenovou a škálovací úroveň](https://azure.microsoft.com/pricing/details/iot-hub/).

     **Skupina prostředků**: vytvoření IoT hub prostředků skupiny toohost hello nebo použijte existující. Další informace najdete v tématu [skupiny zdrojů použití toomanage vašich prostředků Azure](../articles/azure-resource-manager/resource-group-portal.md).

     **Umístění**: Vyberte hello nejbližší tooyou umístění, kde se má vytvořit hello IoT hub.

     **PIN kód toodashboard**: tuto možnost vyberte pro Centrum IoT tooyour snadný přístup z řídicího panelu hello.

   ![Zadejte informace o toocreate služby IoT hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

3. Klikněte na možnost **Vytvořit**. Služby IoT hub může trvat několik minut toocreate. Zobrazí se průběh hello **oznámení** podokně.

   ![Zobrazení oznámení o průběhu pro centrum IoT](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

4. Po vytvoření služby IoT hub na ni kliknete na řídicím panelu hello. Poznamenejte si hello **Hostname**a potom klikněte na **zásady sdíleného přístupu**.

   ![Získat název hostitele hello služby IoT hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

5. V hello **zásady sdíleného přístupu** podokně klikněte na tlačítko hello **iothubowner** zásady a pak kopírování a zaznamenání hello **připojovací řetězec** služby IoT hub. Další informace najdete v tématu [řízení přístupu tooIoT rozbočovače](../articles/iot-hub/iot-hub-devguide-security.md).

> [!NOTE] 
Pro tento kurz nastavení nebudete připojovací řetězec iothubowner potřebovat. Ale musíte ho pro některé hello kurzy o různé scénáře IoT po dokončení tohoto nastavení.

   ![Získání připojovacího řetězce centra IoT](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-hello-iot-hub-for-your-device"></a>Registrovat zařízení ve hello IoT hub pro vaše zařízení

1. V hello [portál Azure](https://portal.azure.com/), otevřete své služby IoT hub.

2. Klikněte na **Průzkumník zařízení**.
3. V podokně Průzkumník zařízení hello, klikněte na **přidat** tooadd zařízení tooyour IoT hub. Potom hello následující:

   **ID zařízení**: Zadejte ID hello hello nového zařízení. V ID zařízení se rozlišují malá a velká písmena.

   **Typ ověřování:** Vyberte **Symetrický klíč**.

   **Automaticky generovat klíče:** Zaškrtněte toto políčko.

   **Připojení zařízení tooIoT rozbočovače**: klikněte na tlačítko **povolit**.

   ![Přidání zařízení v hello Explorer zařízení služby IoT hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. Klikněte na **Uložit**.
5. Po vytvoření hello zařízení otevřete hello zařízení v hello **Explorer zařízení** podokně.
6. Poznamenejte si primární klíč hello hello připojovacího řetězce.

   ![Získat hello zařízení připojovací řetězec](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)
