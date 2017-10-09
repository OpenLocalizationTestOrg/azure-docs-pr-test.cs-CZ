## <a name="configure-hello-nodejs-simulated-device"></a>Konfigurace simulované zařízení Node.js hello
1. Na panelu vzdáleného monitorování hello, klikněte na tlačítko **+ přidat zařízení** a pak přidejte *vlastní zařízení*. Poznamenejte hello IoT Hub, název hostitele, id zařízení a klíč zařízení. Musíte je později v tomto kurzu při přípravě hello remote_monitoring.js zařízení klientské aplikace.
2. Ujistěte se, že Node.js verze 0.12.x nebo novější je nainstalován na vývojovém počítači. Spustit `node --version` na příkazovém řádku nebo v prostředí toocheck hello verzi. Informace o používání balíček správce tooinstall Node.js v systému Linux najdete v tématu [instalací Node.js prostřednictvím Správce balíčků][node-linux].
3. Pokud jste nainstalovali Node.js, klonování hello nejnovější verzi hello [azure-iot-sdk uzlu] [ lnk-github-repo] úložiště tooyour vývojovém počítači. Vždy používat hello **hlavní** větve pro hello nejnovější verzi hello knihovny a ukázky.
4. Z vaší místní kopii hello [azure-iot-sdk uzlu] [ lnk-github-repo] úložiště, kopie hello následující dva soubory z hello uzlu, zařízení nebo ukázky tooan prázdná složka na vývojovém počítači:
   
   * Packages.JSON
   * remote_monitoring.js
5. Otevřete soubor remote_monitoring.js hello a vyhledejte hello za definicí proměnné:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Nahraďte **[připojovací řetězec služby IoT Hub zařízení]** připojovacím řetězcem zařízení. Použijte hello hodnoty pro název hostitele služby IoT Hub, id zařízení a klíč zařízení, který jste si poznamenali v kroku 1. Připojovací řetězec zařízení má hello následující formát:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Pokud je váš název hostitele služby IoT Hub **contoso** a je id zařízení **mydevice**, připojovací řetězec vypadá jako hello následující fragment kódu:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Uložte soubor hello. Spusťte následující příkazy v prostředí nebo příkazového řádku ve složce hello, který obsahuje soubory potřebné balíčky tooinstall hello tyto hello a pak spusťte hello ukázkovou aplikaci:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Sledovat dynamické telemetrie v akci
řídicí panel Hello zobrazuje hello teploty a vlhkosti telemetrie z hello existující Simulovaná zařízení:

![Hello výchozí řídicí panely][image1]

Pokud vyberete Node.js hello simulované se zařízení, které jste spustili v předchozí části hello, uvidíte teploty a vlhkosti, externí teplotní telemetrie:

![Přidat externí teplotu toohello řídicí panel][image2]

řešení vzdáleného monitorování Hello automaticky zjišťuje hello další externí teplotní telemetrie typu a přidá na řídicí panel hello toohello grafu.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png