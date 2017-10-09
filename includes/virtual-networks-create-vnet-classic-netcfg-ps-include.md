## <a name="how-toocreate-a-virtual-network-using-a-network-config-file-from-powershell"></a>Jak toocreate virtuální sítě pomocí konfigurace sítě souboru z prostředí PowerShell
Azure používá všechny dostupné tooa předplatné virtuální sítě toodefine souboru xml. Můžete stažení tohoto souboru, toomodify upravit nebo odstranit existující virtuální sítě a vytvořit nové virtuální sítě. V tomto kurzu zjistíte, jak toodownload tento soubor označuje tooas sítě souboru konfigurace (nebo netcfg) a upravit ho toocreate nové virtuální sítě. toolearn Další informace o hello sítě konfigurační soubor, najdete v části hello [schéma konfigurace virtuální sítě Azure](https://msdn.microsoft.com/library/azure/jj157100.aspx).

toocreate virtuální síť s souboru netcfg pomocí prostředí PowerShell, dokončení hello následující kroky:

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, dokončení hello kroky v hello [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) článek, potom přihlásit tooAzure a vybrat své předplatné.
2. Z konzoly Azure PowerShell text hello, použijte hello **Get-AzureVnetConfig** rutiny toodownload hello sítě konfiguračního souboru tooa adresáře v počítači tak, že spustíte následující příkaz hello: 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   Očekávaný výstup:
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. Otevřete soubor hello jste uložili v kroku 2 pomocí jakékoli aplikace XML nebo textovém editoru a vyhledejte hello  **<VirtualNetworkSites>**  element. Pokud máte žádné sítě, které už vytvořený, každá síť se zobrazí jako vlastní  **<VirtualNetworkSite>**  element.
4. toocreate hello virtuální sítě v tomto scénáři, přidejte následující XML přímo pod hello hello  **<VirtualNetworkSites>**  element:

   ```xml
        <VirtualNetworkSite name="TestVNet" Location="East US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>
   ```
   
5. Uložte soubor konfigurace sítě hello.
6. Z konzoly Azure PowerShell text hello, použijte hello **Set-AzureVnetConfig** rutiny tooupload hello sítě konfigurační soubor spuštěním hello následující příkaz: 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   Vrátí výstup:
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   Pokud **OperationStatus** není *úspěšné* v hello vrátil výstup, zkontrolujte soubor xml hello chyb a dokončení kroku 6 znovu.

7. Z konzoly Azure PowerShell text hello, použijte hello **Get-AzureVnetSite** tooverify rutiny, která hello nové sítě se přidal spuštěním hello následující příkaz: 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   Hello vrátil (zkrácení) výstup zahrnuje hello následující text:
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```
