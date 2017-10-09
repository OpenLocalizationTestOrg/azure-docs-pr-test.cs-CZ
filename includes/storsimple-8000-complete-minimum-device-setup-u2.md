<!--author=alkohli last changed: 01/12/17-->

#### <a name="toocomplete-hello-minimum-storsimple-device-setup"></a>toocomplete hello minimální instalace zařízení StorSimple

   > [!NOTE]
   > Název zařízení hello nelze změnit po dokončení hello minimální instalace zařízení.
   
1. Z hello tabulkové seznam zařízení v hello **zařízení** okně vyberte a klikněte na zařízení. Hello zařízení **připraveni tooset až** stavu. Hello **konfigurace zařízení** otevře se okno.

     ![Síťová rozhraní pro minimální instalaci zařízení StorSimple](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig1.png)

2. V hello **konfigurace zařízení** okno:
   
   1. Zadejte **Přátelské oslovení** svého zařízení. Hello výchozí název zařízení odráží informace, jako je model hello zařízení a sériové číslo. Zařízení můžete přiřadit popisný název tvořený až toomanage too64 znaků.
   2. Sada hello **časové pásmo** podle hello zeměpisného umístění, ve které hello zařízení nasazuje. Toto časové pásmo bude zařízení používat pro všechny naplánované operace.
   3. V části hello **DATA 0 nastavení**:

       1. Vaše DATA 0, síťové rozhraní se zobrazuje jako Povolené s hello sítě nastavení (IP, podsítě, brány) pomocí Průvodce instalací hello. Síťové rozhraní DATA 0 je zároveň automaticky povolené pro cloud i standard iSCSI.

       2. Zadejte, zda text hello pevné IP adresy pro řadič 0 a řadič 1. **pevné IP adresy řadiče Hello potřebovat toobe volné IP adresy v podsíti hello přístupné pomocí hello zařízení IP adresy.** Pokud hello rozhraní DATA 0 byl nakonfigurovali pro protokol IPv4, hello pevné IP adresy potřeba toobe součástí hello formátu protokolu IPv4. Pokud jste pro konfiguraci protokolu IPv6 uvedli předponu, pevné IP adresy hello jsou naplněny automaticky v těchto polích.

            ![Síťová rozhraní pro minimální instalaci zařízení StorSimple](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig2.png)

            pevné IP adresy pro řadič hello Hello se používají pro obsluhu hello aktualizace toohello zařízení. Proto hello pevné IP adresy musí být směrovatelné a musí umožňovat tooconnect toohello Internetu. Můžete zkontrolovat, že řadiči pevné IP adresy jsou směrovatelné pomocí hello [Test-HcsmConnection] [ Test] rutiny. Následující příklad ukazuje, pevné IP adresy řadiče jsou směrované toohello Internet a přístup Hello hello servery Microsoft Update.

            ![Rutina Test-HcsmConnection zobrazující směrovatelné IP adresy](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig3.png)

1. Klikněte na **OK**. Konfigurace zařízení Hello spustí. Po dokončení konfigurace hello zařízení, budete upozorněni. Hello změny stavu zařízení příliš**Online** v hello **zařízení** okno.

    ![Síťová rozhraní pro minimální instalaci zařízení StorSimple](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig4.png)

<!--Link reference-->
[Test]: https://technet.microsoft.com/library/dn715782(v=wps.630).aspx
