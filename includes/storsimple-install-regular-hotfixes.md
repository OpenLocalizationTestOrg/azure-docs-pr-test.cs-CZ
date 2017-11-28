<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="f24df-101">Pro instalaci regulární oprav hotfix prostřednictvím Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="f24df-101">To install regular hotfixes via Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="f24df-102">Připojte ke konzole sériového portu zařízení.</span><span class="sxs-lookup"><span data-stu-id="f24df-102">Connect to the device serial console.</span></span> <span data-ttu-id="f24df-103">Další informace najdete v tématu [krok 1: připojení ke konzole sériového portu](../articles/storsimple/storsimple-update-device.md#step1).</span><span class="sxs-lookup"><span data-stu-id="f24df-103">For more information, see [Step 1: Connect to the serial console](../articles/storsimple/storsimple-update-device.md#step1).</span></span>
2. <span data-ttu-id="f24df-104">V nabídce konzoly sériového portu, vyberte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="f24df-104">In the serial console menu, select option 1, **Log in with full access**.</span></span> <span data-ttu-id="f24df-105">Zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="f24df-105">Type the password.</span></span> <span data-ttu-id="f24df-106">Výchozí heslo je **Heslo1**.</span><span class="sxs-lookup"><span data-stu-id="f24df-106">The default password is **Password1**.</span></span>
3. <span data-ttu-id="f24df-107">Na příkazovém řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="f24df-107">At the command prompt, type:</span></span>
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > <span data-ttu-id="f24df-108">Tento příkaz se vztahuje pouze na regulární opravy hotfix.</span><span class="sxs-lookup"><span data-stu-id="f24df-108">This command applies only to regular hotfixes.</span></span> <span data-ttu-id="f24df-109">Tento příkaz spustit na jenom jeden řadič, ale oba řadiče se pak zaktualizuje.</span><span class="sxs-lookup"><span data-stu-id="f24df-109">You run this command on only one controller, but both controllers will be updated.</span></span>
    > <span data-ttu-id="f24df-110">Může dojít k selhání řadiče během procesu aktualizace. převzetí služeb při selhání však nebude mít vliv dostupnosti systému nebo operace.</span><span class="sxs-lookup"><span data-stu-id="f24df-110">You may notice a controller failover during the update process; however, the failover will not affect system availability or operation.</span></span>

4. <span data-ttu-id="f24df-111">Po zobrazení výzvy zadejte cestu k sdílené síťové složce, která obsahuje soubory oprav hotfix.</span><span class="sxs-lookup"><span data-stu-id="f24df-111">When prompted, supply the path to the network shared folder that contains the hotfix files.</span></span>
5. <span data-ttu-id="f24df-112">Zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="f24df-112">You will be prompted for confirmation.</span></span> <span data-ttu-id="f24df-113">Typ **Y** pokračovat v instalaci oprav hotfix.</span><span class="sxs-lookup"><span data-stu-id="f24df-113">Type **Y** to proceed with the hotfix installation.</span></span>

