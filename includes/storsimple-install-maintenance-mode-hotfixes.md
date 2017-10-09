<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="a3287-101">tooinstall opravy hotfix režimu údržby pomocí prostředí Windows PowerShell pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="a3287-101">tooinstall Maintenance mode hotfixes via Windows PowerShell for StorSimple</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a3287-102">V režimu údržby potřebujete opravu hotfix hello tooapply nejdřív na jednom řadiči a na hello jiný řadič.</span><span class="sxs-lookup"><span data-stu-id="a3287-102">In Maintenance mode, you need tooapply hello hotfix first on one controller and then on hello other controller.</span></span>
> 
> 

1. <span data-ttu-id="a3287-103">Hello zařízení uvést do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="a3287-103">Place hello device into Maintenance mode.</span></span> <span data-ttu-id="a3287-104">V tématu [krok 2: režim údržby zadejte](../articles/storsimple/storsimple-update-device.md#step2) pokyny tooenter režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="a3287-104">See [Step 2: Enter Maintenance mode](../articles/storsimple/storsimple-update-device.md#step2) for instructions on how tooenter Maintenance mode.</span></span>
2. <span data-ttu-id="a3287-105">tooapply hello hotfix, zadejte:</span><span class="sxs-lookup"><span data-stu-id="a3287-105">tooapply hello hotfix, type:</span></span>
   
     `Start-HcsHotfix` 
3. <span data-ttu-id="a3287-106">Po zobrazení výzvy zadejte hello cesta toohello síťové sdílené složky, která obsahuje soubory oprav hotfix hello.</span><span class="sxs-lookup"><span data-stu-id="a3287-106">When prompted, supply hello path toohello network shared folder that contains hello hotfix files.</span></span>
4. <span data-ttu-id="a3287-107">Zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="a3287-107">You will be prompted for confirmation.</span></span> <span data-ttu-id="a3287-108">Typ **Y** tooproceed s hello instalace opravy hotfix.</span><span class="sxs-lookup"><span data-stu-id="a3287-108">Type **Y** tooproceed with hello hotfix installation.</span></span>
5. <span data-ttu-id="a3287-109">Poté, co jste použili hello opravu hotfix na jednom řadiči, přihlášení toohello jiný řadič.</span><span class="sxs-lookup"><span data-stu-id="a3287-109">After you have applied hello hotfix on one controller, log on toohello other controller.</span></span> <span data-ttu-id="a3287-110">Nainstalujte opravu hotfix hello, stejně jako u předchozí řadiče hello.</span><span class="sxs-lookup"><span data-stu-id="a3287-110">Apply hello hotfix as you did for hello previous controller.</span></span>
6. <span data-ttu-id="a3287-111">Po použití hello opravy hotfix, ukončete režim údržby.</span><span class="sxs-lookup"><span data-stu-id="a3287-111">After hello hotfixes are applied, exit Maintenance mode.</span></span> <span data-ttu-id="a3287-112">V tématu [krok 4: režim údržby ukončení](../articles/storsimple/storsimple-update-device.md#step4) pokyny.</span><span class="sxs-lookup"><span data-stu-id="a3287-112">See [Step 4: Exit Maintenance mode](../articles/storsimple/storsimple-update-device.md#step4) for instructions.</span></span>

