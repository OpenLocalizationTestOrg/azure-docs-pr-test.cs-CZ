#### <a name="toostop-and-start-a-virtual-device"></a><span data-ttu-id="06951-101">toostop a spuštění virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="06951-101">toostop and start a virtual device</span></span>
<span data-ttu-id="06951-102">toostop virtuálního zařízení, klikněte na jeho název a potom klikněte na **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="06951-102">toostop a virtual device, click its name, and then click **Shutdown**.</span></span> <span data-ttu-id="06951-103">Hello virtuální zařízení vypíná, i když je jeho stav **zastavení**.</span><span class="sxs-lookup"><span data-stu-id="06951-103">While hello virtual device is shutting down, its status is **Stopping**.</span></span> <span data-ttu-id="06951-104">Po zastavení virtuálního zařízení hello je jeho stav **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="06951-104">After hello virtual device is stopped, its status is **Stopped**.</span></span>

<span data-ttu-id="06951-105">Pomocí následující rutiny toostop hello a spuštění virtuálního zařízení.</span><span class="sxs-lookup"><span data-stu-id="06951-105">Use hello following cmdlets toostop and start a virtual device.</span></span>

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-virtual-device"></a><span data-ttu-id="06951-106">toorestart virtuálního zařízení</span><span class="sxs-lookup"><span data-stu-id="06951-106">toorestart a virtual device</span></span>
<span data-ttu-id="06951-107">Když běží virtuální zařízení a chcete, aby toorestart, klikněte na jeho název a pak klikněte na tlačítko **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="06951-107">When a virtual device is running and you want toorestart it, click its name, and then click **Restart**.</span></span> <span data-ttu-id="06951-108">Během restartování virtuálního zařízení hello je jeho stav **restartování**.</span><span class="sxs-lookup"><span data-stu-id="06951-108">While hello virtual device is restarting, its status is **Restarting**.</span></span> <span data-ttu-id="06951-109">Když je připraven pro toouse jste hello virtuální zařízení, je její stav **systémem**.</span><span class="sxs-lookup"><span data-stu-id="06951-109">When hello virtual device is ready for you toouse, its status is **Running**.</span></span>

<span data-ttu-id="06951-110">Pomocí následující rutiny toorestart virtuálního zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="06951-110">Use hello following cmdlet toorestart a virtual device.</span></span>

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

