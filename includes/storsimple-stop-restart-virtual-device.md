#### <a name="toostop-and-start-a-virtual-device"></a>toostop a spuštění virtuálního zařízení
toostop virtuálního zařízení, klikněte na jeho název a potom klikněte na **vypnutí**. Hello virtuální zařízení vypíná, i když je jeho stav **zastavení**. Po zastavení virtuálního zařízení hello je jeho stav **Zastaveno**.

Pomocí následující rutiny toostop hello a spuštění virtuálního zařízení.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="toorestart-a-virtual-device"></a>toorestart virtuálního zařízení
Když běží virtuální zařízení a chcete, aby toorestart, klikněte na jeho název a pak klikněte na tlačítko **restartujte**. Během restartování virtuálního zařízení hello je jeho stav **restartování**. Když je připraven pro toouse jste hello virtuální zařízení, je její stav **systémem**.

Pomocí následující rutiny toorestart virtuálního zařízení hello.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

