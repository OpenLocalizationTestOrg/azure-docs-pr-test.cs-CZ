


Sadu dostupnosti pomáhá udržovat vaše virtuální počítače, které jsou k dispozici při výpadku, například při údržbě. Umístění podobně nakonfigurované dva nebo více virtuálních počítačů v nastavení dostupnosti vytvoří hello redundance potřeby toomaintain dostupnost hello aplikacím nebo službám, které běží ve virtuálním počítači. Podrobnosti o tomto postupu najdete v tématu [spravovat hello dostupnosti virtuálních počítačů][Manage hello availability of virtual machines].

Je nejlepší postup toouse skupiny dostupnosti a vyrovnávání zatížení toohelp koncových bodů zajistěte, aby byl aplikaci vždy k dispozici a spuštěná efektivně. Podrobnosti o koncové body s vyrovnáváním zatížení najdete v tématu [pro infrastrukturu Azure služby Vyrovnávání zatížení][Load balancing for Azure infrastructure services].

Klasické virtuální počítače můžete přidat do dostupnosti nastavit pomocí jedné ze dvou možností:

* [Možnost 1: Vytvoření virtuálního počítače a sadu dostupnosti v hello stejný čas][Option 1: Create a virtual machine and an availability set at hello same time]. Potom můžete přidáte nové virtuální počítače toohello nastavit při vytváření těchto virtuálních počítačů.
* [Možnost 2: Přidat stávající sadu dostupnosti virtuálního počítače tooan][Option 2: Add an existing virtual machine tooan availability set].

> [!NOTE]
> V hello klasického modelu, hello virtuálních počítačů, které chcete mít tooput ve stejné skupině dostupnosti musí patřit toohello stejné cloudové služby.
> 
> 

## <a id="createset"></a>Možnost 1: vytvoření virtuálního počítače a sadu dostupnosti v hello stejný čas
Můžete použít buď hello portál Azure nebo Azure PowerShell příkazy toodo to.

toouse hello portálu Azure:

1. Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **+ nový**a potom klikněte na **virtuální počítač**.
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. Vyberte Marketplace bitovou kopii virtuálního počítače hello chcete toouse. Můžete toocreate virtuálního počítače se systémem Linux nebo Windows.
4. Pro hello vybrané virtuální počítače, ověřte, že model nasazení hello je nastaven příliš**Classic** a pak klikněte na tlačítko **vytvořit**
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. Zadejte název virtuálního počítače, uživatelské jméno a heslo (pro počítače s Windows) nebo veřejný klíč SSH (pro počítače se systémem Linux). 
6. Zvolte velikost virtuálního počítače hello a pak klikněte na tlačítko **vyberte** toocontinue.
7. Zvolte **volitelné konfigurace > sadu dostupnosti**a vyberte skupinu dostupnosti hello chcete tooadd hello virtuálního počítače.
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. Zkontrolujte nastavení konfigurace. Když jste hotovi, klikněte na tlačítko **vytvořit**.
9. Zatímco Azure vytváří virtuální počítač, můžete sledovat průběh hello pod **virtuální počítače** v nabídce centra hello.

toouse prostředí Azure PowerShell příkazy toocreate virtuální počítač Azure a přidejte ho sady dostupnosti. nový nebo existující tooa, přečtěte si [toocreate použití Azure PowerShell a předem nakonfigurujte virtuálních počítačích se systémem Windows](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a id="addmachine"></a>Možnost 2: Přidat stávající sadu dostupnosti tooan virtuálního počítače
V hello portálu Azure můžete přidat existující klasické virtuální počítače tooan stávající sadu dostupnosti nebo vytvořte novou pro ně. (Nezapomeňte, že hello hello virtuální počítače ve stejné skupině dostupnosti musí patřit toohello stejné cloudové služby.) Hello kroky jsou hello téměř stejný. V prostředí Azure PowerShell můžete přidat hello virtuálního počítače tooan stávající sadu dostupnosti.

1. Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **virtuálních počítačů (klasické)**.
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. Hello seznam virtuálních počítačů vyberte název hello hello virtuálního počítače, které mají tooadd toohello sady.
4. Zvolte **sadu dostupnosti** z virtuálního počítače hello **nastavení**.
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. Vyberte hello sady dostupnosti. Chcete tooadd hello virtuálního počítače. virtuální počítač Hello musí patřit toohello stejná jako skupina dostupnosti hello Cloudová služba.
   
    ![Obrázek alternativní text](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. Klikněte na **Uložit**.

Příkazy prostředí Azure PowerShell toouse, otevřete relaci prostředí Azure PowerShell úrovni správce a spusťte následující příkaz hello. Pro hello zástupné symboly (například &lt;VmCloudServiceName&gt;), nahraďte vše v rámci hello uvozovky, včetně hello < a > znaků, se hello správnými názvy.

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> Hello virtuální počítač může mít toobe restartovat toofinish přidáním toohello sady dostupnosti.
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at hello same time]: #createset
[Option 2: Add an existing virtual machine tooan availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage hello availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

