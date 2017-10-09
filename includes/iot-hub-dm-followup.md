## <a name="customize-and-extend-hello-device-management-actions"></a>Přizpůsobit a rozšířit hello akce správy zařízení

Řešení IoT můžete rozbalit hello definovaná sada schémat správy zařízení nebo povolit vlastní vzorky pomocí dvojče zařízení hello a primitiv metoda cloud zařízení. Další akce správy zařízení příklady obnovení továrního nastavení, aktualizaci firmwaru, aktualizace softwaru, řízení spotřeby, správu sítí a připojení a šifrování dat.

## <a name="device-maintenance-windows"></a>Časová období údržby zařízení

Obvykle konfigurace zařízení tooperform akcí v čase, který minimalizuje přerušení a výpadky. Časová období údržby zařízení jsou běžně používané vzor toodefine hello čas, kdy zařízení musí aktualizovat její konfiguraci. Back-end řešení můžete použít hello požadovaných vlastností toodefine twin hello zařízení a aktivace zásady na zařízení, která umožňuje časové období údržby. Pokud zařízení obdrží zásady okno údržby hello, můžete použít hello hlášené vlastnost hello zařízení twin tooreport hello stav hello zásad. Hello back-end aplikace pak může použít zařízení twin dotazy tooattest toocompliance zařízení a každou zásadu.

## <a name="next-steps"></a>Další kroky

V tomto kurzu se na zařízení používá přímá metoda tootrigger vzdálené restartování počítače. Můžete použít hello hlášené vlastnosti tooreport hello restartujte poslední čas z hello zařízení a dotazované hello zařízení twin toodiscover hello restartování poslední čas hello zařízení z cloudu hello.

toocontinue Začínáme se službou IoT Hub a vzory správy zařízení, jako je vzdálené přes hello letecké firmware aktualizace, najdete v části:

[Kurz: Jak toodo firmwarem aktualizovat][lnk-fwupdate]

toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

najdete v části Začínáme se službou IoT Hub, toocontinue [Začínáme se službou IoT Edge][lnk-iot-edge].

[lnk-fwupdate]: ../articles/iot-hub/iot-hub-node-node-firmware-update.md
[lnk-tutorial-jobs]: ../articles/iot-hub/iot-hub-node-node-schedule-jobs.md
[lnk-iot-edge]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md