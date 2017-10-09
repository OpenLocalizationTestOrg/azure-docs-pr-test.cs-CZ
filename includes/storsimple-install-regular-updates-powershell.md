<!--author=SharS last changed: 11/18/16-->

#### <a name="tooinstall-regular-updates-via-windows-powershell-for-storsimple"></a>tooinstall pravidelné aktualizace prostřednictvím Windows Powershellu pro StorSimple
1. Otevření konzoly sériového portu zařízení hello a vyberte možnost 1, **přihlásit úplný přístup**. Zadejte heslo hello. výchozí heslo Hello je *Heslo1*. 
2. Hello příkazového řádku zadejte:
   
     `Get-HcsUpdateAvailability`
   
    Zobrazí se upozornění, pokud jsou k dispozici aktualizace, a zda jsou aktualizace hello rušivý nebo omezovaly.
3. Hello příkazového řádku zadejte:
   
     `Start-HcsUpdate`
   
    Spustí se proces aktualizace Hello.

> [!IMPORTANT]
> * Tento příkaz se vztahuje pouze tooregular aktualizace. Tento příkaz spustit na jenom jeden řadič, ale oba řadiče se pak zaktualizuje. 
> * Může dojít k selhání řadiče během procesu aktualizace hello; převzetí služeb při selhání hello však nebude mít vliv dostupnosti systému nebo operace.
> 
> 

