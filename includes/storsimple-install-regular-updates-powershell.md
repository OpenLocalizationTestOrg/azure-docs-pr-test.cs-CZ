<!--author=SharS last changed: 11/18/16-->

#### <a name="tooinstall-regular-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="87816-101">tooinstall pravidelné aktualizace prostřednictvím Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="87816-101">tooinstall regular updates via Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="87816-102">Otevření konzoly sériového portu zařízení hello a vyberte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="87816-102">Open hello device serial console and select option 1, **Log in with full access**.</span></span> <span data-ttu-id="87816-103">Zadejte heslo hello.</span><span class="sxs-lookup"><span data-stu-id="87816-103">Type hello password.</span></span> <span data-ttu-id="87816-104">výchozí heslo Hello je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="87816-104">hello default password is *Password1*.</span></span> 
2. <span data-ttu-id="87816-105">Hello příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="87816-105">At hello command prompt, type:</span></span>
   
     `Get-HcsUpdateAvailability`
   
    <span data-ttu-id="87816-106">Zobrazí se upozornění, pokud jsou k dispozici aktualizace, a zda jsou aktualizace hello rušivý nebo omezovaly.</span><span class="sxs-lookup"><span data-stu-id="87816-106">You will be notified if updates are available and whether hello updates are disruptive or non-disruptive.</span></span>
3. <span data-ttu-id="87816-107">Hello příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="87816-107">At hello command prompt, type:</span></span>
   
     `Start-HcsUpdate`
   
    <span data-ttu-id="87816-108">Spustí se proces aktualizace Hello.</span><span class="sxs-lookup"><span data-stu-id="87816-108">hello update process will start.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="87816-109">Tento příkaz se vztahuje pouze tooregular aktualizace.</span><span class="sxs-lookup"><span data-stu-id="87816-109">This command applies only tooregular updates.</span></span> <span data-ttu-id="87816-110">Tento příkaz spustit na jenom jeden řadič, ale oba řadiče se pak zaktualizuje.</span><span class="sxs-lookup"><span data-stu-id="87816-110">You run this command on only one controller, but both controllers will be updated.</span></span> 
> * <span data-ttu-id="87816-111">Může dojít k selhání řadiče během procesu aktualizace hello; převzetí služeb při selhání hello však nebude mít vliv dostupnosti systému nebo operace.</span><span class="sxs-lookup"><span data-stu-id="87816-111">You may notice a controller failover during hello update process; however, hello failover will not affect system availability or operation.</span></span>
> 
> 

