

Rozšíření virtuálních počítačů vám můžou pomoci:

* Upravit funkce zabezpečení a identity, jako je resetování hodnot účtu a použití antimalwaru
* Spustit, zastavit nebo konfigurovat monitorování a diagnostiku
* Resetovat nebo instalovat funkce připojení, jako je RDP a SSH
* Diagnostikovat, monitorovat a spravovat virtuální počítače

Existuje ještě mnoho dalších funkcí. Pravidelně se vydávají nové funkce rozšíření virtuálních počítačů. Tento článek popisuje hello Azure virtuálních počítačů agentů pro Windows a Linux, a jak podporují funkce rozšíření virtuálního počítače. Seznam rozšíření virtuálních počítačů podle kategorií funkcí najdete v tématu [Rozšíření a funkce virtuálních počítačů Azure](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="azure-vm-agents-for-windows-and-linux"></a>Agenti virtuálních počítačů Azure pro Windows a Linux
Hello agenta Azure virtuální počítače (VM Agent) je zabezpečené, šedé – proces, který nainstaluje a nakonfiguruje, odebere rozšíření virtuálního počítače na instancích virtuálních počítačů Azure. Hello agenta virtuálního počítače funguje jako služba hello zabezpečené místní řízení pro vaše virtuální počítač Azure. Hello rozšíření, které hello zatížení agenta poskytují konkrétní funkce tooincrease produktivitu pomocí hello instance.

Existují dva agenti virtuálních počítačů Azure – jeden pro virtuální počítače s Windows a jeden pro virtuální počítače s Linuxem.

Pokud chcete jeden nebo více rozšíření virtuálního počítače toouse instance virtuálního počítače, hello instance musí mít nainstalovaného agenta virtuálního počítače. Image virtuálního počítače vytvořit pomocí hello portál Azure a bitovou kopii z hello **Marketplace** automaticky nainstaluje agenta virtuálního počítače v procesu vytváření hello. Pokud instance virtuálního počítače chybí Agent virtuálního počítače, můžete nainstalovat hello agenta virtuálního počítače po vytvoření instance virtuálního počítače hello. Nebo můžete nainstalovat agenta hello ve vlastní image virtuálního počítače, který potom odešlete.

> [!IMPORTANT]
> Tito agenti virtuálního počítače jsou velmi nenáročné služby, které umožňují zabezpečenou správu instancí virtuálního počítače. Mohou existovat případy, ve kterých nechcete hello agenta virtuálního počítače. Pokud ano, být jisti toocreate virtuálních počítačů, které nemají hello Agent virtuálního počítače nainstalovaný pomocí hello příkazového řádku Azure CLI nebo Powershellu. I když hello agenta virtuálního počítače může být fyzicky odstraněna, hello chování rozšíření virtuálního počítače na instanci hello není definován. Proto se odebírání nainstalovaného agenta virtuálního počítače nepodporuje.
>

Hello agenta virtuálního počítače je povolena v následujících situacích hello:

* Při vytváření instance virtuálního počítače pomocí hello portál Azure a vybrat bitovou kopii z hello **Marketplace**,
* Při vytváření instance virtuálního počítače pomocí hello [New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx) nebo hello [New-AzureQuickVM](https://msdn.microsoft.com/library/azure/dn495183.aspx) rutiny. Virtuální počítač bez agenta virtuálního počítače můžete vytvořit přidáním hello **– DisableGuestAgent** parametr toohello [přidat AzureProvisioningConfig](https://msdn.microsoft.com/library/azure/dn495299.aspx) rutiny

* Pokud ručně stáhnout a nainstalovat hello agenta virtuálního počítače na existující instance virtuálních počítačů a nastavte hello **parametr ProvisionGuestAgent** hodnota příliš**true**. Tento postup můžete použít pro agenty Windows i Linux pomocí příkazu PowerShellu nebo volání REST. (Pokud není nastaveno hello **parametr ProvisionGuestAgent** hodnotu po ruční instalaci agenta virtuálního počítače, přidání hello hello agenta virtuálního počítače není zjištěna správně hello.) hello následující příklad ukazuje kód jak toodo tento pomocí prostředí PowerShell, kde hello `$svc` a `$name` již byla určena argumenty:

      $vm = Get-AzureVM –ServiceName $svc –Name $name
      $vm.VM.ProvisionGuestAgent = $TRUE
      Update-AzureVM –Name $name –VM $vm.VM –ServiceName $svc

* Když vytvoříte image virtuálního počítače, která obsahuje nainstalovaného agenta virtuálního počítače. Jakmile hello bitové kopie s hello agenta virtuálního počítače existuje, můžete nahrát tooAzure této bitové kopie. Virtuální počítač s Windows, stáhněte si hello [souboru MSI agenta virtuálního počítače Windows](http://go.microsoft.com/fwlink/?LinkID=394789) a nainstalujte hello agenta virtuálního počítače. Pro virtuální počítač s Linuxem, nainstalujte agenta virtuálního počítače z úložiště Githubu hello nacházející se v hello <https://github.com/Azure/WALinuxAgent>. Další informace o tom, jak tooinstall hello agenta virtuálního počítače v systému Linux, najdete v části hello [Azure Linux virtuálního počítače agenta uživatelská příručka](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> V PaaS, hello agenta virtuálního počítače se nazývá **WindowsAzureGuestAgent**a je vždy k dispozici na webových a pracovních rolí virtuálního počítače. (Další informace najdete v tématu [architektura Role Azure](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx).) hello agenta virtuálního počítače pro virtuální počítače Role můžete nyní přidat rozšíření toohello cloudové služby virtuálních počítačů v hello stejným způsobem, který pro trvalých virtuálních počítačů. Hello největších rozdíl mezi rozšíření virtuálního počítače na roli virtuálních počítačů a trvalé virtuálních počítačů je po přidání rozšíření virtuálního počítače hello. S rolí virtuálních počítačů se přidají rozšíření první toohello Cloudová služba, pak toohello nasazení v rámci dané cloudové služby.
>
> Použití [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) toolist rutiny všechny dostupné role rozšíření virtuálního počítače.
>
>

## <a name="find-add-update-and-remove-vm-extensions"></a>Vyhledání, přidání, aktualizace a odebrání rozšíření virtuálních počítačů
Podrobnosti o těchto úlohách najdete v tématu, které se zabývá [přidáním, vyhledáním, aktualizací a odebráním rozšíření virtuálního počítače Azure](../articles/virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
