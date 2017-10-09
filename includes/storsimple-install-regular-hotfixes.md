<!--author=SharS last changed: 9/17/15-->

#### <a name="tooinstall-regular-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="7985b-101">regulární opravy hotfix tooinstall prostřednictvím Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="7985b-101">tooinstall regular hotfixes via Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="7985b-102">Připojte toohello konzoly sériového portu zařízení.</span><span class="sxs-lookup"><span data-stu-id="7985b-102">Connect toohello device serial console.</span></span> <span data-ttu-id="7985b-103">Další informace najdete v tématu [krok 1: připojení konzoly sériového portu toohello](../articles/storsimple/storsimple-update-device.md#step1).</span><span class="sxs-lookup"><span data-stu-id="7985b-103">For more information, see [Step 1: Connect toohello serial console](../articles/storsimple/storsimple-update-device.md#step1).</span></span>
2. <span data-ttu-id="7985b-104">V nabídce konzoly sériového portu hello, vyberte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="7985b-104">In hello serial console menu, select option 1, **Log in with full access**.</span></span> <span data-ttu-id="7985b-105">Zadejte heslo hello.</span><span class="sxs-lookup"><span data-stu-id="7985b-105">Type hello password.</span></span> <span data-ttu-id="7985b-106">výchozí heslo Hello je **Heslo1**.</span><span class="sxs-lookup"><span data-stu-id="7985b-106">hello default password is **Password1**.</span></span>
3. <span data-ttu-id="7985b-107">Hello příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="7985b-107">At hello command prompt, type:</span></span>
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > <span data-ttu-id="7985b-108">Tento příkaz se vztahuje pouze tooregular opravy hotfix.</span><span class="sxs-lookup"><span data-stu-id="7985b-108">This command applies only tooregular hotfixes.</span></span> <span data-ttu-id="7985b-109">Tento příkaz spustit na jenom jeden řadič, ale oba řadiče se pak zaktualizuje.</span><span class="sxs-lookup"><span data-stu-id="7985b-109">You run this command on only one controller, but both controllers will be updated.</span></span>
    > <span data-ttu-id="7985b-110">Může dojít k selhání řadiče během procesu aktualizace hello; převzetí služeb při selhání hello však nebude mít vliv dostupnosti systému nebo operace.</span><span class="sxs-lookup"><span data-stu-id="7985b-110">You may notice a controller failover during hello update process; however, hello failover will not affect system availability or operation.</span></span>

4. <span data-ttu-id="7985b-111">Po zobrazení výzvy zadejte hello cesta toohello síťové sdílené složky, která obsahuje soubory oprav hotfix hello.</span><span class="sxs-lookup"><span data-stu-id="7985b-111">When prompted, supply hello path toohello network shared folder that contains hello hotfix files.</span></span>
5. <span data-ttu-id="7985b-112">Zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="7985b-112">You will be prompted for confirmation.</span></span> <span data-ttu-id="7985b-113">Typ **Y** tooproceed s hello instalace opravy hotfix.</span><span class="sxs-lookup"><span data-stu-id="7985b-113">Type **Y** tooproceed with hello hotfix installation.</span></span>

