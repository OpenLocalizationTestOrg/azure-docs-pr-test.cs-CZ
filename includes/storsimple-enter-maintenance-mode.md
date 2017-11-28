<!--author=SharS last changed: 12/01/15-->

#### <a name="to-enter-maintenance-mode"></a><span data-ttu-id="9d947-101">Přejít do režimu údržby</span><span class="sxs-lookup"><span data-stu-id="9d947-101">To enter Maintenance mode</span></span>
1. <span data-ttu-id="9d947-102">V nabídce konzoly sériového portu, zvolte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="9d947-102">In the serial console menu, choose option 1, **Log in with full access**.</span></span>
2. <span data-ttu-id="9d947-103">Zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="9d947-103">Type the password.</span></span> <span data-ttu-id="9d947-104">Výchozí heslo je **Heslo1**.</span><span class="sxs-lookup"><span data-stu-id="9d947-104">The default password is **Password1**.</span></span>
3. <span data-ttu-id="9d947-105">Na příkazovém řádku zadejte</span><span class="sxs-lookup"><span data-stu-id="9d947-105">At the command prompt, type</span></span>
   
     `Enter-HcsMaintenanceMode`
4. <span data-ttu-id="9d947-106">Zobrazí se upozornění oznamující, že bude přerušit všechny vstupně-výstupní požadavky a severu připojení k portálu Azure classic režimu údržby a zobrazí se výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="9d947-106">You will see a warning message telling you that Maintenance mode will disrupt all I/O requests and sever the connection to the Azure classic portal, and you will be prompted for confirmation.</span></span> <span data-ttu-id="9d947-107">Typ **Y** vstoupit do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="9d947-107">Type **Y** to enter Maintenance mode.</span></span>
   
    <span data-ttu-id="9d947-108">Oba řadiče se restartuje.</span><span class="sxs-lookup"><span data-stu-id="9d947-108">Both controllers will restart.</span></span> <span data-ttu-id="9d947-109">Po dokončení restartování se zobrazí další zpráva označující, že zařízení je v režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="9d947-109">When the restart is complete, another message will appear indicating that the device is in Maintenance mode.</span></span>

