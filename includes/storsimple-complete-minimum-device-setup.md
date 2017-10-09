<!--author=alkohli last changed: 9/17/15-->

#### <a name="toocomplete-hello-minimum-storsimple-device-setup"></a>toocomplete hello minimální instalace zařízení StorSimple
1. V hello **zařízení** stránky, vyberte hello zařízení, klikněte na šipku hello u stránku hello zařízení název toogo toohello konkrétní zařízení. 
   
    ![Stránka Zařízení s online zařízeními](./media/storsimple-complete-minimum-device-setup/HCS_DevicesPageM-include.png) 
2. Klikněte na ikonu rychlého startu ![ikona rychlý Start](./media/storsimple-complete-minimum-device-setup/HCS_QuickStartIcon-include.png) tooaccess hello stránka rychlého startu zařízení. Klikněte na tlačítko **dokončení instalace zařízení** toostart hello **konfigurace zařízení** průvodce.
   
    ![Stránka rychlého startu zařízení](./media/storsimple-complete-minimum-device-setup/Device_Quick_Start_page_1M.png)
3. Na hello **základní nastavení** stránky, hello následující:
   
   1. Zadejte **Přátelské oslovení** svého zařízení. Hello výchozí název zařízení odráží informace, jako je model hello zařízení a sériové číslo. Zařízení můžete přiřadit popisný název tvořený až toomanage too64 znaků.
   2. Sada hello **časové pásmo** podle hello zeměpisného umístění, ve které hello zařízení nasazuje. Toto časové pásmo bude zařízení používat pro všechny naplánované operace.
   3. V části **Nastavení DNS** zadejte adresu pro **Sekundární server DNS**. Pokud používáte IPv6, hello pole se vyplní podle hello předpony protokolu IPv6 zadané v rozhraní Windows PowerShell hello. 
      Pokud není nakonfigurováno hello sekundární server DNS, nebude povolená toosave konfiguraci zařízení.
   4. V seznamu rozhraní s podporou iSCSI povolte aspoň jednu síť pro iSCSI. Aspoň jedno síťové rozhraní musí toobe povolenou podporu cloudu a jedno rozhraní musí toobe iSCSI povolený. Rozhraní DATA 0 má automaticky povolenou podporu cloudu.
      
      ![Základní nastavení minimální instalace zařízení StorSimple](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupBasicSettings1-include.png)
4. Klikněte na ikonu šipky hello. ![Ikona šipky StorSimple](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)
5. Na hello **síťových rozhraní** zadejte hello pevné IP adresy pro řadič 0 a řadič 1. Pokud hello rozhraní DATA 0 byl nakonfigurovali pro protokol IPv4, hello pevné IP adresy potřeba toobe součástí hello formátu protokolu IPv4. Pokud jste pro konfiguraci protokolu IPv6 uvedli předponu, pevné IP adresy hello vyplní automaticky v těchto polích.

    > [!NOTE] 
    > - pevné IP adresy řadiče Hello potřebovat toobe volné IP adresy v podsíti hello přístupné pomocí hello zařízení IP adresy.
    > - Hello pevné IP adresy pro řadič hello se používají pro obsluhu hello aktualizace toohello zařízení, a proto hello pevné IP adresy musí být směrovatelné a musí umožňovat tooconnect toohello Internetu.

    ![Síťová rozhraní pro minimální instalaci zařízení StorSimple](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

1. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí StorSimple](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png).
   Vrátíte se zařízení toohello **rychlý Start** stránky.
   
   > [!NOTE]
   > Nastavení můžete upravit všechny hello jiných zařízení kdykoli díky přístupu k hello **konfigurace** stránky.
   > 
   > 

![Dostupné video](./media/storsimple-complete-minimum-device-setup/Video_icon.png)**Dostupné video**

toowatch video, které ukazuje, jak toocomplete hello minimální instalace zařízení, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/minimum-storsimple-device-setup/).

