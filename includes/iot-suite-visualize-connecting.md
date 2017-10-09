## <a name="view-device-telemetry-in-hello-dashboard"></a>Zobrazení telemetrie zařízení na řídicím panelu hello
řídicí panel Hello v hello vzdálené monitorování umožňuje řešení můžete tooview hello telemetrie zařízení odesílat tooIoT rozbočovače.

1. V prohlížeči, návratový toohello vzdálené monitorování řídicí panel řešení, klikněte na tlačítko **zařízení** v hello levém panelu toonavigate toohello **seznam zařízení**.
2. V hello **seznam zařízení**, měli byste vidět, že hello stav zařízení je **systémem**. Pokud ne, klikněte na tlačítko **povolit zařízení** v hello **podrobnosti o zařízení** panelu.
   
    ![Zobrazení stavu zařízení][18]
3. Klikněte na tlačítko **řídicí panel** tooreturn toohello řídicí panel, vyberte příslušné zařízení v hello **zařízení tooView** tooview rozevíracího seznamu jeho telemetrie. Hello telemetrie z ukázkové aplikace hello je 50 jednotky pro interní teploty, 55 jednotky pro externí teplotu a 50 jednotky pro vlhkosti.
   
    ![Zobrazení telemetrie zařízení][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a>Volání metody na zařízení
Hello řídicího panelu řešení vzdáleného monitorování hello umožňuje tooinvoke metody zařízení prostřednictvím služby IoT Hub. Například v hello řešení vzdáleného sledování můžete vyvolat metodu toosimulate, restartování zařízení.

1. V hello vzdálené monitorování řídicí panel řešení, klikněte na **zařízení** v hello levém panelu toonavigate toohello **seznam zařízení**.
2. Klikněte na tlačítko **ID zařízení** pro zařízení v hello **seznam zařízení**.
3. V hello **podrobnosti o zařízení** panelu, klikněte na tlačítko **metody**.
   
    ![Metody zařízení][13]
4. V hello **metoda** rozevíracího seznamu, vyberte **InitiateFirmwareUpdate**a potom v **FWPACKAGEURI** zadejte fiktivní adresu URL. Klikněte na tlačítko **vyvolání metody** toocall hello metodu hello zařízení.
   
    ![Vyvolání metody zařízení][14]
   

5. Zobrazí se zpráva v konzole hello spuštění kódu vašeho zařízení, když zařízení hello zpracovává metoda hello. výsledky Hello hello metody přidají toohello historie hello řešení portálu:

    ![Zobrazení historie metod][img-method-history]

## <a name="next-steps"></a>Další kroky
článek Hello [přizpůsobení předkonfigurovaných řešení] [ lnk-customize] popisuje několik způsobů, jak můžete rozšířit této ukázce. Mezi možná rozšíření patří skutečné senzory a implementace dalších příkazů.

Další informace o hello [oprávnění na webu azureiotsuite.com hello][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
