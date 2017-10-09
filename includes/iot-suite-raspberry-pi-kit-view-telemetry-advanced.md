## <a name="view-hello-telemetry"></a>Zobrazení telemetrie hello

Hello malin platformy je nyní odesílání telemetrie toohello řešení vzdáleného monitorování. Můžete zobrazit telemetrii hello na řídicí panel řešení hello. Můžete také odeslat zprávy tooyour malin pí z řídicí panel řešení hello.

- Přejděte toohello řídicí panel řešení.
- Vyberte zařízení v hello **zařízení tooView** rozevíracího seznamu.
- Hello telemetrie z hello malin platformy zobrazí na řídicím panelu hello.

![Zobrazení telemetrie z hello malin platformy][img-telemetry-display]

## <a name="initiate-hello-firmware-update"></a>Spustit aktualizaci firmwaru hello

proces aktualizace firmwaru Hello stáhne a nainstaluje aktualizovanou verzi aplikace hello zařízení klienta na hello malin pí. Další informace o procesu aktualizace firmwaru hello, najdete v části Popis hello vzor aktualizace firmwaru hello v [přehled správy zařízení s centrem IoT][lnk-update-pattern].

Proces aktualizace firmwaru hello iniciovat volání metody na hello zařízení. Tato metoda je asynchronní a vrátí ihned zahájí proces aktualizace hello. používá zařízení Hello hlášené vlastnosti toonotify hello řešení o průběhu hello hello aktualizace.

Vyvolání metody na vaše malin pí z řídicí panel řešení hello. Když hello malin pí poprvé připojí toohello řešení vzdáleného monitorování, odešle informace o metodách hello, kterou podporuje. 

[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry-advanced/telemetry.png
[lnk-update-pattern]: ../articles/iot-hub/iot-hub-device-management-overview.md
