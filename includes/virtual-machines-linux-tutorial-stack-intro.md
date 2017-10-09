## <a name="create-a-resource-group"></a><span data-ttu-id="19728-101">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="19728-101">Create a resource group</span></span>

<span data-ttu-id="19728-102">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="19728-102">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="19728-103">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="19728-103">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="19728-104">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="19728-104">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a><span data-ttu-id="19728-105">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="19728-105">Create a virtual machine</span></span>

<span data-ttu-id="19728-106">Vytvoření virtuálního počítače s hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="19728-106">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="19728-107">Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* a vytvoří klíče SSH, pokud už neexistují ve výchozím umístění klíče.</span><span class="sxs-lookup"><span data-stu-id="19728-107">hello following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="19728-108">toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.</span><span class="sxs-lookup"><span data-stu-id="19728-108">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="19728-109">Po vytvoření hello virtuálních počítačů, hello rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace.</span><span class="sxs-lookup"><span data-stu-id="19728-109">When hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="19728-110">Poznamenejte si hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="19728-110">Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="19728-111">Tato adresa je použité tooaccess hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="19728-111">This address is used tooaccess hello VM.</span></span>

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```



## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="19728-112">Otevření portu 80 pro webový provoz</span><span class="sxs-lookup"><span data-stu-id="19728-112">Open port 80 for web traffic</span></span> 

<span data-ttu-id="19728-113">Ve výchozím nastavení jsou povoleny pouze připojení SSH do virtuálních počítačů Linux nasazené v Azure.</span><span class="sxs-lookup"><span data-stu-id="19728-113">By default, only SSH connections are allowed into Linux VMs deployed in Azure.</span></span> <span data-ttu-id="19728-114">Protože tento virtuální počítač bude toobe webový server, je třeba tooopen port 80 z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="19728-114">Because this VM is going toobe a web server, you need tooopen port 80 from hello internet.</span></span> <span data-ttu-id="19728-115">Použití hello [az virtuálních počítačů open-port](/cli/azure/vm#open-port) příkaz tooopen hello požadovaného portu.</span><span class="sxs-lookup"><span data-stu-id="19728-115">Use hello [az vm open-port](/cli/azure/vm#open-port) command tooopen hello desired port.</span></span>  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a><span data-ttu-id="19728-116">Připojení SSH k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="19728-116">SSH into your VM</span></span>


<span data-ttu-id="19728-117">Pokud neznáte hello veřejnou IP adresu vašeho virtuálního počítače, spusťte hello [seznam veřejné ip sítě az](/cli/azure/network/public-ip#list) příkaz:</span><span class="sxs-lookup"><span data-stu-id="19728-117">If you don't already know hello public IP address of your VM, run hello [az network public-ip list](/cli/azure/network/public-ip#list) command:</span></span>


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

<span data-ttu-id="19728-118">Použití hello následující příkaz toocreate na relace SSH s hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="19728-118">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="19728-119">Nahraďte hello správné veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="19728-119">Substitute hello correct public IP address of your virtual machine.</span></span> <span data-ttu-id="19728-120">V tomto příkladu hello IP adresa je *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="19728-120">In this example, hello IP address is *40.68.254.142*.</span></span>

```bash
ssh azureuser@40.68.254.142
```

