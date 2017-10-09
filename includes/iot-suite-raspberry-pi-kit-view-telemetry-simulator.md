## <a name="view-hello-telemetry"></a>Zobrazení telemetrie hello

Hello malin platformy je nyní odesílání telemetrie toohello řešení vzdáleného monitorování. Můžete zobrazit telemetrii hello na řídicí panel řešení hello. Můžete také odeslat zprávy tooyour malin pí z řídicí panel řešení hello.

- Přejděte toohello řídicí panel řešení.
- Vyberte zařízení v hello **zařízení tooView** rozevíracího seznamu.
- Hello telemetrie z hello malin platformy zobrazí na řídicím panelu hello.

![Zobrazení telemetrie z hello malin platformy][img-telemetry-display]

## <a name="act-on-hello-device"></a>Fungovat na zařízení hello

Z řídicího panelu řešení hello můžete volat metody na vaše malin pí. Když hello malin pí připojí toohello řešení vzdáleného monitorování, odešle informace o metodách hello, kterou podporuje.

- Na řídicím panelu řešení hello, klikněte na tlačítko **zařízení** toovisit hello **zařízení** stránky. Vyberte platformy vaší malin v hello **seznam zařízení**. Zvolte **metody**:

    ![Seznam zařízení na řídicím panelu][img-list-devices]

- Na hello **vyvolání metody** vyberte **LightBlink** v hello **metoda** rozevíracího seznamu.

- Zvolte **InvokeMethod**. Simulátor Hello vytiskne zprávy v konzole hello na hello malin pí. aplikace Hello na hello malin pí odešle řídicí panel řešení back toohello potvrzení:

    ![Zobrazit historii – metoda][img-method-history]

- Můžete přepnout hello DIODU zapnout a vypnout pomocí hello **ChangeLightStatus** metoda s **LightStatusValue** nastavit příliš**1** pro na nebo **0** pro vypnout.

> [!WARNING]
> Pokud necháte hello vzdálené monitorování řešení, které jsou spuštěné v účtu Azure, se vám účtuje hello, když ji spustí. Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config]. Po dokončení používat, odstraňte hello předkonfigurované řešení z účtu Azure.


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md