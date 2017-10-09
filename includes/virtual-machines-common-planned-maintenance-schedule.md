

## <a name="multi-and-single-instance-vms"></a>Více a jedna Instance virtuálních počítačů
Mnoho zákazníků systémem Azure počet je důležité, aby můžete naplánovat při jejich virtuální počítače podstoupit plánované údržby kvůli výpadku toohello – asi 15 minut – k tomu dojde během údržby. Řízení toohelp sad dostupnosti můžete použít, když virtuální počítače zřízené obdrží plánované údržby.

Existují dvě možné konfigurace pro virtuální počítače spuštěné v Azure. Virtuální počítače jsou buď nakonfigurovány jako s více instancemi nebo s jednou instancí. Pokud jsou virtuální počítače v nastavení dostupnosti, pak jsou nakonfigurované jako s více instancemi. Všimněte si, i jeden virtuálních počítačů je možné nasadit v nastavení dostupnosti, takže jsou považovány za s více instancemi. Pokud virtuální počítače nejsou v nastavení dostupnosti, pak jsou nakonfigurované jako jedné instance.  Podrobnosti o dostupnosti sadách najdete v tématu [hello Správa dostupnosti virtuálních počítačů z Windows](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [hello Správa dostupnosti virtuálních počítačů systému Linux](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Plánované údržby aktualizuje toosingle instance a virtuální počítače s více instancemi dojít samostatně. Změna konfigurace vaší single-instance toobe virtuální počítače (pokud jsou s více instancemi) nebo s více instancemi toobe (pokud jde o jednoduchou) můžete řídit, pokud jejich virtuální počítače přijímat hello plánované údržby. V tématu [plánované údržby pro virtuální počítače s Linuxem Azure](../articles/virtual-machines/linux/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [plánované údržby pro virtuální počítače s Azure Windows](../articles/virtual-machines/windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) podrobnosti o plánované údržby pro virtuální počítače Azure.

## <a name="for-multi-instance-configuration"></a>Pro konfiguraci s více instancemi
Můžete vybrat hello čas plánované údržby ovlivní vaše virtuální počítače, které jsou nasazeny v konfiguraci skupiny dostupnosti odebráním těchto virtuálních počítačů ze skupiny dostupnosti.

1. E-mail je odeslán tooyou sedm dní kalendáře před hello plánované údržby tooyour virtuální počítače v konfiguraci s více instancemi. Hello ID předplatného a názvy virtuálních počítačů s více instancemi hello vliv jsou zahrnuty v textu hello hello e-mailů.
2. Během těchto sedm dní, můžete zvolit hello čas vaše instance jsou aktualizovány odebráním virtuální počítače s více instancemi v této oblasti z jejich sady dostupnosti. Tato změna konfigurace způsobí restartování, jako hello virtuálního počítače je přechod z jednoho fyzického hostitele, určený pro údržbu, tooanother fyzického hostitele, který není cílová pro údržbu.
3. Hello virtuálních počítačů můžete odebrat z její skupinou dostupnosti ve hello portálu Azure.

   1. Hello portálu vyberte tooremove hello virtuálních počítačů z hello sady dostupnosti.  

   2. V části **nastavení**, klikněte na tlačítko **sadu dostupnosti**.

      ![Výběr skupiny dostupnosti](./media/virtual-machines-planned-maintenance-schedule/availabilitysetselection.png)

   3. V rámci dostupnosti hello nastavte rozevírací nabídce, vyberte "Není součástí skupiny dostupnosti."

      ![Odebrat ze sady](./media/virtual-machines-planned-maintenance-schedule/availabilitysetwarning.png)

   4. V horní části hello, klikněte na tlačítko **Uložit**. Klikněte na tlačítko **Ano** hello tooacknowledge, který tato akce restartuje počítač.

   >[!TIP]
   >Hello toomulti-instance virtuálního počítače můžete později změnit konfiguraci výběrem jedné z hello uvedené skupiny dostupnosti.

4. Odebrat ze skupiny dostupnosti virtuálních počítačů jsou hostitelé přesunutý tooSingle Instance a neaktualizují během hello plánované údržby tooAvailability nastavení konfigurace.
5. Po dokončení aktualizace tooAvailability hello nastavit virtuální počítače (podle tooschedule uvedených v hello původního e-mailu), měli byste přidat hello virtuální počítače zpět do jejich skupiny dostupnosti. Stane součástí nastavení dostupnosti změní konfiguraci virtuálních počítačů hello jako s více instancemi a má za následek restart. Obvykle po dokončení všech aktualizací s více instancemi napříč celou prostředí Azure hello, odpovídá jedné instance údržby.

Odebrání virtuálního počítače ze skupiny dostupnosti můžete také dosáhnout pomocí Azure PowerShell:

```
Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Remove-AzureAvailabilitySet | Update-AzureVM
```

## <a name="for-single-instance-configuration"></a>Pro konfiguraci s jednou instancí
Můžete vybrat hello čas plánované údržby ovlivňuje můžete virtuální počítače v konfiguraci s jednou instancí přidáním těchto virtuálních počítačů do skupiny dostupnosti.

Podrobný postup

1. E-mail je odeslán tooyou sedm dní kalendáře před hello plánované údržby tooVMs v konfiguraci s jednou instancí. Hello ID předplatného a názvy hello vliv na jedné Instance virtuálních počítačů jsou zahrnuty v textu hello hello e-mailů.
2. Během těchto sedm dní, můžete zvolit hello čas instanci nastavit restartování přidáním vaší dostupnosti tooan jedné instance virtuálních počítačů v této stejné oblasti. Tato změna konfigurace způsobí restartování, jako hello virtuálního počítače je přechod z jednoho fyzického hostitele, určený pro údržbu, tooanother fyzického hostitele, který není cílová pro údržbu.
3. Postupujte podle pokynů sem tooadd existující virtuální počítače do skupiny dostupnosti pomocí hello portál Azure a prostředí Azure PowerShell. (Viz hello prostředí Azure PowerShell vzorku, který zahrnuje následující kroky.)
4. Jakmile tyto virtuální počítače jsou překonfigurovat jako s více instancemi, jsou vyloučeny z hello plánované údržby tooSingle instance virtuálních počítačů.
5. Po dokončení aktualizace hello jedné instance virtuálního počítače (podle tooschedule v hello původního e-mailu), můžete vrátit hello virtuální počítače toosingle-instance odebráním hello virtuální počítače z jejich skupiny dostupnosti.

Přidávání virtuálních počítačů tooan také sadu dostupnosti můžete dosáhnout pomocí Azure PowerShell:

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

<!--Anchors-->



<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
