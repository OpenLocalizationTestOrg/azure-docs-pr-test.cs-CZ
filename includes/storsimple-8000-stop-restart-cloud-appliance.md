#### <a name="toostop-and-start-a-cloud-appliance"></a><span data-ttu-id="18100-101">toostop a spusťte cloudu zařízení</span><span class="sxs-lookup"><span data-stu-id="18100-101">toostop and start a cloud appliance</span></span>

1. <span data-ttu-id="18100-102">toostop o cloudu zařízení přejděte toohello virtuálních počítačů pro vaše zařízení cloudu.</span><span class="sxs-lookup"><span data-stu-id="18100-102">toostop a cloud appliance, go toohello VM for your cloud appliance.</span></span>
    <span data-ttu-id="18100-103">![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)</span><span class="sxs-lookup"><span data-stu-id="18100-103">![StorSimple Cloud Appliance Virtual Machine](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)</span></span>

2. <span data-ttu-id="18100-104">Na panelu příkazů hello, klikněte na **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="18100-104">From hello command bar, click **Stop**.</span></span>

    ![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. <span data-ttu-id="18100-106">Po zobrazení výzvy k potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="18100-106">When prompted for confirmation, click **Yes**.</span></span>

    ![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. <span data-ttu-id="18100-108">Když zastavíte virtuální počítač, dojde k jeho uvolnění.</span><span class="sxs-lookup"><span data-stu-id="18100-108">When you stop a VM, it gets deallocated.</span></span> <span data-ttu-id="18100-109">Probíhá zastavení hello cloudu zařízení, i když je jeho stav **Deallocating**.</span><span class="sxs-lookup"><span data-stu-id="18100-109">While hello cloud appliance is stopping, its status is **Deallocating**.</span></span> <span data-ttu-id="18100-110">Po zastavení hello cloudu zařízení je jeho stav **zastaveném (nepřiřazeném)**.</span><span class="sxs-lookup"><span data-stu-id="18100-110">After hello cloud appliance is stopped, its status is **Stopped (deallocated)**.</span></span>

    ![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. <span data-ttu-id="18100-112">Po zastavení virtuálního počítače klikněte na **spustit** (k dispozici tlačítko) toostart hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="18100-112">Once a VM is stopped, click **Start** (button becomes available) toostart hello VM.</span></span> <span data-ttu-id="18100-113">Po spuštění hello cloudu zařízení, je její stav **Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="18100-113">After hello cloud appliance has started up, its status is **Started**.</span></span>

    ![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

<span data-ttu-id="18100-115">Pomocí následující rutiny toostop hello a spuštění zařízení cloudu.</span><span class="sxs-lookup"><span data-stu-id="18100-115">Use hello following cmdlets toostop and start a cloud appliance.</span></span>

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-cloud-appliance"></a><span data-ttu-id="18100-116">toorestart zařízení cloudu</span><span class="sxs-lookup"><span data-stu-id="18100-116">toorestart a cloud appliance</span></span>

<span data-ttu-id="18100-117">toorestart o cloudu zařízení přejděte toohello virtuálních počítačů pro vaše zařízení cloudu.</span><span class="sxs-lookup"><span data-stu-id="18100-117">toorestart a cloud appliance, go toohello VM for your cloud appliance.</span></span> <span data-ttu-id="18100-118">Na panelu příkazů hello, klikněte na **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="18100-118">From hello command bar, click **Restart**.</span></span> <span data-ttu-id="18100-119">Po zobrazení výzvy potvrďte hello restartování.</span><span class="sxs-lookup"><span data-stu-id="18100-119">When prompted, confirm hello restart.</span></span> <span data-ttu-id="18100-120">Když hello cloudu zařízení je připraven pro toouse jste, je její stav **systémem**.</span><span class="sxs-lookup"><span data-stu-id="18100-120">When hello cloud appliance is ready for you toouse, its status is **Running**.</span></span>

![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

<span data-ttu-id="18100-122">Pomocí následující rutiny toorestart zařízení cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="18100-122">Use hello following cmdlet toorestart a cloud appliance.</span></span>

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

