<!--author=alkohli last changed: 9/17/15-->

#### <a name="toocomplete-hello-minimum-storsimple-device-setup"></a>toocomplete hello minimální instalace zařízení StorSimple
1. Vyberte zařízení hello a klikněte na tlačítko **rychlý Start**. Klikněte na tlačítko **dokončení instalace zařízení** Průvodce konfigurace zařízení toostart hello.
2. V Průvodci konfigurace zařízení hello **základní nastavení** dialogové okno pole, hello následující:
   
   1. Zadejte **Přátelské oslovení** svého zařízení. Hello výchozí název zařízení odráží informace, jako je model hello zařízení a sériové číslo. Zařízení můžete přiřadit popisný název tvořený až toomanage too64 znaků.
   2. Sada hello **časové pásmo** podle hello zeměpisného umístění, ve které hello zařízení nasazuje. Toto časové pásmo bude zařízení používat pro všechny naplánované operace.
   3. V části **Nastavení DNS** zadejte adresu pro **Sekundární server DNS**. Pokud používáte IPv6, hello pole se vyplní podle hello předpony protokolu IPv6 zadané v rozhraní Windows PowerShell hello. 
      Pokud není nakonfigurováno hello sekundární server DNS, nebude povolená toosave konfiguraci zařízení.
   4. V seznamu rozhraní s podporou iSCSI povolte aspoň jednu síť pro iSCSI. Aspoň jedno síťové rozhraní musí toobe povolenou podporu cloudu a jedno rozhraní musí toobe iSCSI povolený. Rozhraní DATA 0 má automaticky povolenou podporu cloudu.
      
      ![Základní nastavení minimální instalace zařízení StorSimple](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupBasicSettings1-include.png)
3. Klikněte na ikonu šipky hello. ![Ikona šipky StorSimple](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)
4. V hello **síťových rozhraní** dialogovém okně zadejte hello pevné IP adresy pro řadič 0 a řadič 1. **pevné IP adresy řadiče Hello potřebovat toobe volné IP adresy v podsíti hello přístupné pomocí hello zařízení IP adresy.** Pokud hello rozhraní DATA 0 byl nakonfigurovali pro protokol IPv4, hello pevné IP adresy potřeba toobe součástí hello formátu protokolu IPv4. Pokud jste pro konfiguraci protokolu IPv6 uvedli předponu, pevné IP adresy hello vyplní automaticky v těchto polích.

    ![Síťová rozhraní pro minimální instalaci zařízení StorSimple](./media/storsimple-complete-minimum-device-setup-u1/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

    Hello pevné IP adresy pro řadič hello se používají pro obsluhu hello aktualizace toohello zařízení, a proto hello pevné IP adresy musí být směrovatelné a musí umožňovat tooconnect toohello Internetu. Můžete zkontrolovat, že řadiči pevné IP adresy jsou směrovatelné pomocí hello [Test-HcsmConnection] [ Test] rutiny. Následující příklad ukazuje, pevné IP adresy řadiče jsou směrované toohello Internet a přístup Hello hello servery Microsoft Update. 

     ![Rutina Test-HcsmConnection zobrazující směrovatelné IP adresy](./media/storsimple-complete-minimum-device-setup-u1/Test-HcsmConnectionOutputRegisteredDevice.png)

1. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí StorSimple](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png).
   Vrátíte se zařízení toohello **rychlý Start** stránky.
   
   > [!NOTE]
   > Nastavení můžete upravit všechny hello jiných zařízení kdykoli díky přístupu k hello **konfigurace** stránky.
   > 
   > 

<!--Link reference-->
[Test]: https://technet.microsoft.com/library/dn715782(v=wps.630).aspx