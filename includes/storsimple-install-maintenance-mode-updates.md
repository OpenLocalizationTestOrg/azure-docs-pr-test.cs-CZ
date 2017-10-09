<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>aktualizace režimu údržby tooinstall prostřednictvím Windows Powershellu pro StorSimple
1. Pokud jste tak ještě neučinili, přístup k hello konzoly sériového portu zařízení a vyberte možnost 1, **přihlásit úplný přístup**. 
2. Zadejte heslo hello. výchozí heslo Hello je **Heslo1**.
3. Hello příkazového řádku zadejte:
   
     `Get-HcsUpdateAvailability` 
4. Zobrazí se upozornění, pokud jsou k dispozici aktualizace, a zda jsou aktualizace hello rušivý nebo omezovaly. tooapply rušivý aktualizace, musíte tooput hello zařízení do režimu údržby. V tématu [krok 2: Zadejte údržby režimu](../articles/storsimple/storsimple-update-device.md#step2) pokyny.
5. Když je zařízení v režimu údržby, hello příkazového řádku, zadejte:`Start-HcsUpdate`
6. Zobrazí se výzva k potvrzení. Jakmile potvrdíte hello aktualizace, budou nainstalovány na hello řadiči, který se právě používají. Po instalaci aktualizací hello hello řadiče se restartuje. 
7. Monitorování stavu hello aktualizací. Zadejte:
   
    `Get-HcsUpdateStatus`
   
    Pokud hello `RunInProgress` je `True`, hello aktualizaci stále probíhá. Pokud `RunInProgress` je `False`, znamená to, že hello aktualizace byla dokončena.  
8. Při instalaci aktualizace hello na aktuální řadič hello a má restartovat, připojte toohello jiný řadič a provedení kroků 1 až 6.
9. Po aktualizaci oba řadiče ukončení režimu údržby. V tématu [krok 4: režim údržby ukončení](../articles/storsimple/storsimple-update-device.md#step4) pokyny.

