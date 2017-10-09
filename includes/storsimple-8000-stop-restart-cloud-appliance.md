#### <a name="toostop-and-start-a-cloud-appliance"></a>toostop a spusťte cloudu zařízení

1. toostop o cloudu zařízení přejděte toohello virtuálních počítačů pro vaše zařízení cloudu.
    ![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart1.png)

2. Na panelu příkazů hello, klikněte na **Zastavit**.

    ![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart2.png)

3. Po zobrazení výzvy k potvrzení klikněte na **Ano**.

    ![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart3.png)

4. Když zastavíte virtuální počítač, dojde k jeho uvolnění. Probíhá zastavení hello cloudu zařízení, i když je jeho stav **Deallocating**. Po zastavení hello cloudu zařízení je jeho stav **zastaveném (nepřiřazeném)**.

    ![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart4.png)

5. Po zastavení virtuálního počítače klikněte na **spustit** (k dispozici tlačítko) toostart hello virtuálních počítačů. Po spuštění hello cloudu zařízení, je její stav **Začínáme**.

    ![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart5.png)

Pomocí následující rutiny toostop hello a spuštění zařízení cloudu.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-cloud-appliance"></a>toorestart zařízení cloudu

toorestart o cloudu zařízení přejděte toohello virtuálních počítačů pro vaše zařízení cloudu. Na panelu příkazů hello, klikněte na **restartujte**. Po zobrazení výzvy potvrďte hello restartování. Když hello cloudu zařízení je připraven pro toouse jste, je její stav **systémem**.

![Virtuální počítač s řešením StorSimple Cloud Appliance](./media/storsimple-8000-stop-restart-cloud-appliance/sca-stop-restart6.png)

Pomocí následující rutiny toorestart zařízení cloudu hello.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

