<!--author=SharS last changed: 9/17/15-->

#### <a name="tooconnect-through-hello-serial-console"></a>tooconnect prostřednictvím sériové konzoly hello
1. Připojte zařízení toohello sériový kabel (přímo nebo přes sériového adaptéru USB).
2. Otevřete hello **ovládací panely**a pak otevřete hello **Správce zařízení**.
3. Port hello COM určete, jak ukazuje následující obrázek hello.
   
     ![Připojení prostřednictvím sériové konzoly](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. Spusťte PuTTY. 
5. V pravém podokně hello změnit hello **typ připojení** příliš**sériové**.
6. V pravém podokně hello zadejte odpovídající port COM. hello. Ujistěte se, že hello sériové konfigurační parametry nastavené takto:
   
   * Rychlost: 115 200
   * Datové bity: 8
   * Stop-bity: 1
   * Parita: žádná
   * Řízení toku: žádné
     
     Tato nastavení jsou uvedené v hello následující obrázek.
     
     ![Nastavení PuTTY](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > Pokud hello výchozí nastavení řízení toku nefunguje, zkuste nastavit řízení toku hello tooXON/XOFF.
     > 
     > 
7. Klikněte na tlačítko **otevřete** toostart a sériovou relaci.

