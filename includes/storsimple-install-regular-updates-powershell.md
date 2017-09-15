<!--author=SharS last changed: 11/18/16-->

#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="3b7bc-101">Chcete-li nainstalovat pravidelné aktualizace pomocí prostředí Windows PowerShell pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="3b7bc-101">To install regular updates via Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="3b7bc-102">Otevření konzoly sériového portu zařízení a vyberte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="3b7bc-102">Open the device serial console and select option 1, **Log in with full access**.</span></span> <span data-ttu-id="3b7bc-103">Zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="3b7bc-103">Type the password.</span></span> <span data-ttu-id="3b7bc-104">Výchozí heslo je *Heslo1*.</span><span class="sxs-lookup"><span data-stu-id="3b7bc-104">The default password is *Password1*.</span></span> 
2. <span data-ttu-id="3b7bc-105">Na příkazovém řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="3b7bc-105">At the command prompt, type:</span></span>
   
     `Get-HcsUpdateAvailability`
   
    <span data-ttu-id="3b7bc-106">Zobrazí se upozornění, pokud jsou k dispozici aktualizace, a zda jsou aktualizace rušivý nebo omezovaly.</span><span class="sxs-lookup"><span data-stu-id="3b7bc-106">You will be notified if updates are available and whether the updates are disruptive or non-disruptive.</span></span>
3. <span data-ttu-id="3b7bc-107">Na příkazovém řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="3b7bc-107">At the command prompt, type:</span></span>
   
     `Start-HcsUpdate`
   
    <span data-ttu-id="3b7bc-108">Spustí se proces aktualizace.</span><span class="sxs-lookup"><span data-stu-id="3b7bc-108">The update process will start.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="3b7bc-109">Tento příkaz se vztahuje pouze na pravidelné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="3b7bc-109">This command applies only to regular updates.</span></span> <span data-ttu-id="3b7bc-110">Tento příkaz spustit na jenom jeden řadič, ale oba řadiče se pak zaktualizuje.</span><span class="sxs-lookup"><span data-stu-id="3b7bc-110">You run this command on only one controller, but both controllers will be updated.</span></span> 
> * <span data-ttu-id="3b7bc-111">Může dojít k selhání řadiče během procesu aktualizace. převzetí služeb při selhání však nebude mít vliv dostupnosti systému nebo operace.</span><span class="sxs-lookup"><span data-stu-id="3b7bc-111">You may notice a controller failover during the update process; however, the failover will not affect system availability or operation.</span></span>
> 
> 

