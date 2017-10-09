<!--author=SharS last changed: 02/04/2016-->

#### <a name="toocreate-a-volume"></a>toocreate svazku
1. Na zařízení hello **rychlý Start** klikněte na tlačítko **přidat svazek**. Tím se spustí hello Průvodci přidáním svazku.
2. V hello Průvodce přidáním svazku, v části **základní nastavení**, hello následující:
   
   1. Zadejte **Název** svazku.
   2. Zadejte hello **zřízená kapacita** svazku v GB nebo TB. kapacita svazku Hello musí být mezi 1 GB a 64 TB pro fyzické zařízení.
   3. V rozevíracím seznamu hello, vyberte hello **typ použití** svazku. 
   4. Pokud tento svazek používáte k archivaci dat, vyberte hello **použít tento svazek pro archivní data s méně často používaná** zaškrtávací políčko. Ve všech ostatních případech vyberte možnost **Tiered Volume** (Vrstvený svazek). (Vrstvené svazky se dřív označovaly jako primární svazky).
      
        ![Přidání svazku](./media/storsimple-create-volume/ScreenshotUpdate1VolumeFlow.png)
      
      1. Klikněte na ikonu šipky hello ![ikona šipky](./media/storsimple-create-volume/HCS_ArrowIcon-include.png) toogo toohello další stránku.
3. V hello **další nastavení** dialogu přidat nový záznam řízení přístupu (ACR):
   
   1. Zadejte **Název** záznamu ACR.
   2. V části **název iniciátoru iSCSI**, poskytovat hello iSCSI kvalifikovaný název (IQN) hostitele s Windows. Pokud hello IQN nemáte, přejděte příliš[Get hello názvu IQN hostitele Windows serveru](#get-the-iqn-of-a-windows-server-host).
   3. Doporučujeme povolit výchozí zálohování tak, že vyberete hello **povolit výchozí zálohování tohoto svazku** zaškrtávací políčko. Hello výchozí zálohování vytvoří zásadu, která se spustí ve 22:30 každý den (čas zařízení) a vytvoří cloudový snímek tohoto svazku.
      
      > [!NOTE]
      > Po povolení zálohování hello zde nelze vrátit. Toto nastavení bude potřebovat tooedit hello svazku toomodify.
      > 
      > 
      
        ![Přidání svazku](./media/storsimple-create-volume/AddVolume2-include.png)
4. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-create-volume/HCS_CheckIcon-include.png). Vytvoří se svazek s hello zadané nastavení.

![Dostupné video](./media/storsimple-create-volume/Video_icon.png)**Dostupné video**

toowatch video, které ukazuje, jak toocreate svazek StorSimple, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/create-a-storsimple-volume/).

