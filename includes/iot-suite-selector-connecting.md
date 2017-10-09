> [!div class="op_single_selector"]
> * [C ve Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C v Linuxu](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> 
> 

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

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/