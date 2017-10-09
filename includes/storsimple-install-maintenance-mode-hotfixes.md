<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>tooinstall opravy hotfix režimu údržby pomocí prostředí Windows PowerShell pro StorSimple
> [!IMPORTANT]
> V režimu údržby potřebujete opravu hotfix hello tooapply nejdřív na jednom řadiči a na hello jiný řadič.
> 
> 

1. Hello zařízení uvést do režimu údržby. V tématu [krok 2: režim údržby zadejte](../articles/storsimple/storsimple-update-device.md#step2) pokyny tooenter režimu údržby.
2. tooapply hello hotfix, zadejte:
   
     `Start-HcsHotfix` 
3. Po zobrazení výzvy zadejte hello cesta toohello síťové sdílené složky, která obsahuje soubory oprav hotfix hello.
4. Zobrazí se výzva k potvrzení. Typ **Y** tooproceed s hello instalace opravy hotfix.
5. Poté, co jste použili hello opravu hotfix na jednom řadiči, přihlášení toohello jiný řadič. Nainstalujte opravu hotfix hello, stejně jako u předchozí řadiče hello.
6. Po použití hello opravy hotfix, ukončete režim údržby. V tématu [krok 4: režim údržby ukončení](../articles/storsimple/storsimple-update-device.md#step4) pokyny.

