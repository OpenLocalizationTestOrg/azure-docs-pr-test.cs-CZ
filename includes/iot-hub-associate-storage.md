## <a name="associate-an-azure-storage-account-tooiot-hub"></a>Přidružit tooIoT účtu Azure Storage rozbočovače

Protože aplikaci simulovaného zařízení hello odešle soubor tooa objekt blob, musíte mít [Azure Storage](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) účtu přidruženého tooIoT rozbočovače. Když přidružíte účet úložiště Azure IoT hub, hello IoT hub vygeneruje SAS URI. Zařízení můžete použít tento identifikátor URI pro SAS toosecurely nahrávání kontejneru objektů blob tooa souboru. Hello služby IoT Hub a hello sady SDK zařízení koordinovat hello proces, který generuje hello identifikátor URI pro SAS a je k dispozici tooa zařízení toouse tooupload soubor.

Postupujte podle pokynů hello v [nahrávání souborů konfigurovat pomocí portálu Azure hello](../articles/iot-hub/iot-hub-configure-file-upload.md) tooassociate Centrum IoT tooyour účet úložiště Azure. Ujistěte se, že kontejner objektů blob je spojen s služby IoT hub a že jsou povolené soubor oznámení.

![Povolit oznámení soubor portálu](media/iot-hub-associate-storage/enable-file-notifications.png)