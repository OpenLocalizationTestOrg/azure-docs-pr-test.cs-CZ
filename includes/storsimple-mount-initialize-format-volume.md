<!--author=SharS last changed: 9/17/15-->

#### <a name="toomount-initialize-and-format-a-volume"></a>toomount, inicializace a formátování svazků
1. Spusťte iniciátor iSCSI společnosti Microsoft hello.
2. V hello **vlastnosti iniciátoru iSCSI** okně na hello **zjišťování** , klikněte na **vyhledat portál**.
3. V hello **zjistit cílový portál** dialogové okno, zadejte hello IP adresu vašeho iSCSI povolený síťového rozhraní a pak klikněte na tlačítko **OK**. 
4. V hello **vlastnosti iniciátoru iSCSI** okně na hello **cíle** najděte hello **zjištěné cíle**. Stav zařízení Hello by měla zobrazovat jako **neaktivní**.
5. Vyberte cílové zařízení hello a pak klikněte na tlačítko **Connect**. Po připojení zařízení hello, stav hello by se měl změnit příliš**připojeno**. (Další informace o použití iniciátoru iSCSI společnosti Microsoft hello najdete v tématu [instalace a konfigurace Microsoft iSCSI Initiator][1]).
6. Na hostiteli s Windows, stiskněte klávesu s logem Windows hello + X a potom klikněte na **spustit**. 
7. V hello **spustit** dialogové okno, typ **Diskmgmt.msc**. Klikněte na tlačítko **OK**a hello **Správa disků** se zobrazí dialogové okno. Hello pravém podokně se zobrazí hello svazky na hostiteli.
8. V hello **Správa disků** okně hello připojené svazky zobrazí jak ukazuje následující obrázek hello. Klikněte pravým tlačítkem na hello zjištěné svazek (klikněte na název disku hello) a potom klikněte na **Online**.
   
     ![Inicializace a formátování svazku](./media/storsimple-mount-initialize-format-volume/HCS_InitializeFormatVolume-include.png) 
9. Klikněte pravým tlačítkem na hello svazek (klikněte na název disku hello) znovu a potom klikněte na **inicializovat**.
10. tooformat jednoduchý svazek, proveďte následující kroky hello:
    
    1. Vyberte svazek hello, pravým tlačítkem myši (klikněte na pravou část hello) a klikněte na tlačítko **nový jednoduchý svazek**.
    2. V průvodci Nový jednoduchý svazek hello zadejte hello svazek velikost a písmeno jednotky a nakonfigurujte hello svazek jako systém souborů NTFS.
    3. Zvolte velikost alokační jednotky 64 kB. Tato velikost alokační jednotky dobře funguje s algoritmy pro odstranění duplicit hello používá v hello řešení StorSimple.
    4. Proveďte rychlé formátování.

![Dostupné video](./media/storsimple-mount-initialize-format-volume/Video_icon.png)**Dostupné video**

toowatch video, které ukazuje, jak toomount, inicializovat a formát svazek StorSimple, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/mount-initialize-and-format-a-storsimple-volume/).

<!--Link references-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx
