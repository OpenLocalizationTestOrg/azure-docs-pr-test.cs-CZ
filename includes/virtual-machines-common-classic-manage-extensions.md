


## <a name="using-vm-extensions"></a>Pomocí rozšíření virtuálního počítače
Rozšíření virtuálního počítače Azure implementovat chování nebo funkce, které buď další programy fungovat na virtuálních počítačích Azure (například hello **WebDeployForVSDevTest** rozšíření umožňuje sadě Visual Studio tooWeb nasadit řešení na vašem virtuálním počítači Azure) nebo zadejte Hello schopnost toointeract s hello virtuálních počítačů toosupport některé jiné chování (například vy můžete používat rozšíření hello přístup virtuálních počítačů z prostředí PowerShell, hello rozhraní příkazového řádku Azure a tooreset klienti REST nebo upravte hodnoty vzdáleného přístupu na vašem virtuálním počítači Azure).

> [!IMPORTANT]
> Úplný seznam rozšíření hello funkce podporují, najdete v části [rozšíření virtuálního počítače Azure a funkce](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Protože každý rozšíření virtuálního počítače podporuje konkrétní funkci, přesně co můžete a nemůžete dělat s příponou závisí na hello rozšíření. Proto před změnou virtuálního počítače, ujistěte se, že jste četli hello dokumentaci hello chcete toouse rozšíření virtuálního počítače. Odebírání některé rozšíření virtuálního počítače není podporováno; ostatní uživatelé mají vlastnosti, které lze nastavit, které výrazně mění chování virtuálních počítačů.
> 
> 

Hello běžné úkoly, jsou:

1. Hledání dostupných rozšíření
2. Aktualizace načíst rozšíření
3. Přidání rozšíření
4. Odebrání rozšíření

## <a name="find-available-extensions"></a>Najít dostupných rozšíření
Můžete vyhledat rozšíření a použití rozšířené informace:

* PowerShell
* Rozhraní Azure napříč platformami příkazového řádku (Azure CLI)
* Rozhraní REST API pro správu služeb

### <a name="azure-powershell"></a>Azure PowerShell
Některá rozšíření mít rutiny prostředí PowerShell, které jsou specifické toothem, což může zjednodušit jejich konfigurace z prostředí PowerShell; ale hello následující rutiny fungovat pro všechny rozšíření virtuálního počítače.

Můžete použít následující rutiny tooobtain informace o dostupných rozšíření hello:

* Pro instance webové role nebo rolí pracovního procesu, můžete použít hello [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) rutiny.
* Pro instance virtuálních počítačů, můžete použít hello [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) rutiny.
  
   Například hello následující příklad ukazuje kód jak toolist informace pro hello **IaaSDiagnostics** rozšíření pomocí prostředí PowerShell.
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a>Rozhraní příkazového řádku Azure (Azure CLI)
Některá rozšíření mají příkazy rozhraní příkazového řádku Azure, které jsou konkrétní toothem (hello Docker rozšíření virtuálního počítače je jedním z příkladů), který může zjednodušit jejich konfigurace; ale hello následující příkazy práci pro všechna rozšíření virtuálního počítače.

Můžete použít hello **seznamu rozšíření virtuálních počítačů azure** příkaz tooobtain informace o dostupných rozšíření a použijte hello **–-json** možnost toodisplay všechny dostupné informace o jeden nebo více rozšíření. Pokud použijete název rozšíření, vrátí příkaz hello JSON popis všech dostupných rozšíření.

Například hello následující příklad kódu ukazuje, jak toolist hello informace pro hello **IaaSDiagnostics** rozšíření pomocí rozhraní příkazového řádku Azure hello **seznamu rozšíření virtuálních počítačů azure** příkazu a použití hello **–-json** možnost tooreturn úplné informace.

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a>Rozhraní REST API pro správu služeb
Můžete použít následující rozhraní REST API tooobtain informace o dostupných rozšíření hello:

* Pro instance webové role nebo rolí pracovního procesu, můžete použít hello [seznam dostupných rozšíření](https://msdn.microsoft.com/library/dn169559.aspx) operaci. verze hello toolist rozšíření k dispozici, můžete použít [verze rozšíření seznamu](https://msdn.microsoft.com/library/dn495437.aspx).
* Pro instance virtuálních počítačů, můžete použít hello [rozšíření prostředků seznamu](https://msdn.microsoft.com/library/dn495441.aspx) operaci. verze hello toolist rozšíření k dispozici, můžete použít [verze rozšíření prostředků seznamu](https://msdn.microsoft.com/library/dn495440.aspx).

## <a name="add-update-or-disable-extensions"></a>Přidání, aktualizace nebo zakázat rozšíření
Rozšíření můžete přidat, pokud je vytvořena instance nebo mohou být přidány tooa spuštěna instance. Rozšíření můžete aktualizovat, zakázat nebo odebrat. Tyto akce můžete provést pomocí rutin prostředí Azure PowerShell nebo pomocí operace REST API pro správu služby hello. Parametry jsou požadované tooinstall a nastavit některá rozšíření. Veřejné a privátní parametry jsou podporovány pro rozšíření.

### <a name="azure-powershell"></a>Azure PowerShell
Pomocí rutin prostředí Azure PowerShell je hello nejjednodušší způsob, jak tooadd a aktualizace rozšíření. Při použití rutiny rozšíření hello většinu hello konfigurace rozšíření hello se provádí za vás. V některých případech může být nutné tooprogrammatically přidat rozšíření. Pokud tuto funkci potřebujete toodo, je nutné zadat hello konfigurace rozšíření hello.

Můžete použít následující rutiny tooknow, zda rozšíření vyžaduje konfiguraci parametrů veřejné a privátní hello:

* Pro instance webové role nebo rolí pracovního procesu, můžete použít hello **Get-AzureServiceAvailableExtension** rutiny.
* Pro instance virtuálních počítačů, můžete použít hello **Get-AzureVMAvailableExtension** rutiny.

### <a name="service-management-rest-apis"></a>Rozhraní REST API pro správu služeb
Když načtete seznam dostupných rozšíření pomocí hello rozhraní REST API, zobrazí se informace o tom, jak rozšíření hello toobe nakonfigurované. vrácené informace Hello může zobrazit informace o parametrech reprezentována na schéma veřejné a privátní schématu. Veřejné parametr hodnoty jsou vráceny v dotazech o instancích hello. Hodnoty parametru privátní nebudou zobrazeny.

Můžete použít následující tooknow rozhraní REST API, zda rozšíření vyžaduje konfiguraci parametrů veřejné a privátní hello:

* Pro instance webové role nebo rolí pracovního procesu, hello **PublicConfigurationSchema** a **PrivateConfigurationSchema** elementy obsahují informace hello hello odezvy z hello [seznamu Dostupná rozšíření](https://msdn.microsoft.com/library/dn169559.aspx) operaci.
* Pro instance virtuálních počítačů, hello **PublicConfigurationSchema** a **PrivateConfigurationSchema** elementy obsahují informace hello hello odezvy z hello [seznamu Rozšíření prostředků](https://msdn.microsoft.com/library/dn495441.aspx) operaci.

> [!NOTE]
> Rozšíření můžete také použít konfigurace, které jsou definovány s JSON. Pokud tyto typy rozšíření používají, jenom hello **SampleConfig** element se používá.
> 
> 

