<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="98133-101">Pro instalaci oprav hotfix režimu údržby pomocí prostředí Windows PowerShell pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="98133-101">To install Maintenance mode hotfixes via Windows PowerShell for StorSimple</span></span>
> [!IMPORTANT]
> <span data-ttu-id="98133-102">V režimu údržby musíte nejprve použít opravu hotfix na jednom řadiči a poté na jiný řadič.</span><span class="sxs-lookup"><span data-stu-id="98133-102">In Maintenance mode, you need to apply the hotfix first on one controller and then on the other controller.</span></span>
> 
> 

1. <span data-ttu-id="98133-103">Umístíte zařízení do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="98133-103">Place the device into Maintenance mode.</span></span> <span data-ttu-id="98133-104">V tématu [krok 2: Zadejte údržby režimu](../articles/storsimple/storsimple-update-device.md#step2) pokyny o tom, jak přechod do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="98133-104">See [Step 2: Enter Maintenance mode](../articles/storsimple/storsimple-update-device.md#step2) for instructions on how to enter Maintenance mode.</span></span>
2. <span data-ttu-id="98133-105">Chcete-li použít opravu hotfix, zadejte:</span><span class="sxs-lookup"><span data-stu-id="98133-105">To apply the hotfix, type:</span></span>
   
     `Start-HcsHotfix` 
3. <span data-ttu-id="98133-106">Po zobrazení výzvy zadejte cestu k sdílené síťové složce, která obsahuje soubory oprav hotfix.</span><span class="sxs-lookup"><span data-stu-id="98133-106">When prompted, supply the path to the network shared folder that contains the hotfix files.</span></span>
4. <span data-ttu-id="98133-107">Zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="98133-107">You will be prompted for confirmation.</span></span> <span data-ttu-id="98133-108">Typ **Y** pokračovat v instalaci oprav hotfix.</span><span class="sxs-lookup"><span data-stu-id="98133-108">Type **Y** to proceed with the hotfix installation.</span></span>
5. <span data-ttu-id="98133-109">Po instalaci opravy hotfix na jednom řadiči, přihlaste se k jiné řadiče.</span><span class="sxs-lookup"><span data-stu-id="98133-109">After you have applied the hotfix on one controller, log on to the other controller.</span></span> <span data-ttu-id="98133-110">Opravy hotfix, stejně jako u předchozí kontroleru.</span><span class="sxs-lookup"><span data-stu-id="98133-110">Apply the hotfix as you did for the previous controller.</span></span>
6. <span data-ttu-id="98133-111">Po použití opravy hotfix, ukončete režim údržby.</span><span class="sxs-lookup"><span data-stu-id="98133-111">After the hotfixes are applied, exit Maintenance mode.</span></span> <span data-ttu-id="98133-112">V tématu [krok 4: režim údržby ukončení](../articles/storsimple/storsimple-update-device.md#step4) pokyny.</span><span class="sxs-lookup"><span data-stu-id="98133-112">See [Step 4: Exit Maintenance mode](../articles/storsimple/storsimple-update-device.md#step4) for instructions.</span></span>

