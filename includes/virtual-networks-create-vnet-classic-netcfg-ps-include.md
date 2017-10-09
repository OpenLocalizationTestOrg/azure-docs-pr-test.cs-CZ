## <a name="how-toocreate-a-virtual-network-using-a-network-config-file-from-powershell"></a><span data-ttu-id="ab9e5-101">Jak toocreate virtuální sítě pomocí konfigurace sítě souboru z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ab9e5-101">How toocreate a virtual network using a network config file from PowerShell</span></span>
<span data-ttu-id="ab9e5-102">Azure používá všechny dostupné tooa předplatné virtuální sítě toodefine souboru xml.</span><span class="sxs-lookup"><span data-stu-id="ab9e5-102">Azure uses an xml file toodefine all virtual networks available tooa subscription.</span></span> <span data-ttu-id="ab9e5-103">Můžete stažení tohoto souboru, toomodify upravit nebo odstranit existující virtuální sítě a vytvořit nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ab9e5-103">You can download this file, edit it toomodify or delete existing virtual networks, and create new virtual networks.</span></span> <span data-ttu-id="ab9e5-104">V tomto kurzu zjistíte, jak toodownload tento soubor označuje tooas sítě souboru konfigurace (nebo netcfg) a upravit ho toocreate nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ab9e5-104">In this tutorial, you learn how toodownload this file, referred tooas network configuration (or netcfg) file, and edit it toocreate a new virtual network.</span></span> <span data-ttu-id="ab9e5-105">toolearn Další informace o hello sítě konfigurační soubor, najdete v části hello [schéma konfigurace virtuální sítě Azure](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="ab9e5-105">toolearn more about hello network configuration file, see hello [Azure virtual network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>

<span data-ttu-id="ab9e5-106">toocreate virtuální síť s souboru netcfg pomocí prostředí PowerShell, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ab9e5-106">toocreate a virtual network with a netcfg file using PowerShell, complete hello following steps:</span></span>

1. <span data-ttu-id="ab9e5-107">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, dokončení hello kroky v hello [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) článek, potom přihlásit tooAzure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="ab9e5-107">If you have never used Azure PowerShell, complete hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azureps-cmdlets-docs) article, then sign in tooAzure and select your subscription.</span></span>
2. <span data-ttu-id="ab9e5-108">Z konzoly Azure PowerShell text hello, použijte hello **Get-AzureVnetConfig** rutiny toodownload hello sítě konfiguračního souboru tooa adresáře v počítači tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ab9e5-108">From hello Azure PowerShell console, use hello **Get-AzureVnetConfig** cmdlet toodownload hello network configuration file tooa directory on your computer by running hello following command:</span></span> 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   <span data-ttu-id="ab9e5-109">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="ab9e5-109">Expected output:</span></span>
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. <span data-ttu-id="ab9e5-110">Otevřete soubor hello jste uložili v kroku 2 pomocí jakékoli aplikace XML nebo textovém editoru a vyhledejte hello  **<VirtualNetworkSites>**  element.</span><span class="sxs-lookup"><span data-stu-id="ab9e5-110">Open hello file you saved in step 2 using any XML or text editor application, and look for hello **<VirtualNetworkSites>** element.</span></span> <span data-ttu-id="ab9e5-111">Pokud máte žádné sítě, které už vytvořený, každá síť se zobrazí jako vlastní  **<VirtualNetworkSite>**  element.</span><span class="sxs-lookup"><span data-stu-id="ab9e5-111">If you have any networks already created, each network is displayed as its own **<VirtualNetworkSite>** element.</span></span>
4. <span data-ttu-id="ab9e5-112">toocreate hello virtuální sítě v tomto scénáři, přidejte následující XML přímo pod hello hello  **<VirtualNetworkSites>**  element:</span><span class="sxs-lookup"><span data-stu-id="ab9e5-112">toocreate hello virtual network described in this scenario, add hello following XML just under hello **<VirtualNetworkSites>** element:</span></span>

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
   
5. <span data-ttu-id="ab9e5-113">Uložte soubor konfigurace sítě hello.</span><span class="sxs-lookup"><span data-stu-id="ab9e5-113">Save hello network configuration file.</span></span>
6. <span data-ttu-id="ab9e5-114">Z konzoly Azure PowerShell text hello, použijte hello **Set-AzureVnetConfig** rutiny tooupload hello sítě konfigurační soubor spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ab9e5-114">From hello Azure PowerShell console, use hello **Set-AzureVnetConfig** cmdlet tooupload hello network configuration file by running hello following command:</span></span> 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   <span data-ttu-id="ab9e5-115">Vrátí výstup:</span><span class="sxs-lookup"><span data-stu-id="ab9e5-115">Returned output:</span></span>
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   <span data-ttu-id="ab9e5-116">Pokud **OperationStatus** není *úspěšné* v hello vrátil výstup, zkontrolujte soubor xml hello chyb a dokončení kroku 6 znovu.</span><span class="sxs-lookup"><span data-stu-id="ab9e5-116">If **OperationStatus** is not *Succeeded* in hello returned output, check hello xml file for errors and complete step 6 again.</span></span>

7. <span data-ttu-id="ab9e5-117">Z konzoly Azure PowerShell text hello, použijte hello **Get-AzureVnetSite** tooverify rutiny, která hello nové sítě se přidal spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ab9e5-117">From hello Azure PowerShell console, use hello **Get-AzureVnetSite** cmdlet tooverify that hello new network was added by running hello following command:</span></span> 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   <span data-ttu-id="ab9e5-118">Hello vrátil (zkrácení) výstup zahrnuje hello následující text:</span><span class="sxs-lookup"><span data-stu-id="ab9e5-118">hello returned (abbreviated) output includes hello following text:</span></span>
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```
