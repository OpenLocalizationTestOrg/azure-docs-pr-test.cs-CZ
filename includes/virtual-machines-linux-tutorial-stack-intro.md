## <a name="create-a-resource-group"></a><span data-ttu-id="63966-101">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="63966-101">Create a resource group</span></span>

<span data-ttu-id="63966-102">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="63966-102">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="63966-103">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="63966-103">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="63966-104">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.</span><span class="sxs-lookup"><span data-stu-id="63966-104">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a><span data-ttu-id="63966-105">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="63966-105">Create a virtual machine</span></span>

<span data-ttu-id="63966-106">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="63966-106">Create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="63966-107">Následující příklad vytvoří virtuální počítač *myVM*, a pokud ve výchozím umístění klíčů ještě neexistují klíče SSH, vytvoří je.</span><span class="sxs-lookup"><span data-stu-id="63966-107">The following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="63966-108">Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="63966-108">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="63966-109">Po vytvoření virtuálního počítače se v Azure CLI zobrazí podobné informace jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="63966-109">When the VM has been created, the Azure CLI shows information similar to the following example.</span></span> <span data-ttu-id="63966-110">Poznamenejte si `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="63966-110">Take note of the `publicIpAddress`.</span></span> <span data-ttu-id="63966-111">Tato adresa se používá pro přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="63966-111">This address is used to access the VM.</span></span>

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



## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="63966-112">Otevření portu 80 pro webový provoz</span><span class="sxs-lookup"><span data-stu-id="63966-112">Open port 80 for web traffic</span></span> 

<span data-ttu-id="63966-113">Ve výchozím nastavení jsou povoleny pouze připojení SSH do virtuálních počítačů Linux nasazené v Azure.</span><span class="sxs-lookup"><span data-stu-id="63966-113">By default, only SSH connections are allowed into Linux VMs deployed in Azure.</span></span> <span data-ttu-id="63966-114">Protože tento virtuální počítač bude webový server, budete muset otevřít port 80 z Internetu.</span><span class="sxs-lookup"><span data-stu-id="63966-114">Because this VM is going to be a web server, you need to open port 80 from the internet.</span></span> <span data-ttu-id="63966-115">Požadovaný port otevřete pomocí příkazu [az vm open-port](/cli/azure/vm#open-port).</span><span class="sxs-lookup"><span data-stu-id="63966-115">Use the [az vm open-port](/cli/azure/vm#open-port) command to open the desired port.</span></span>  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a><span data-ttu-id="63966-116">Připojení SSH k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="63966-116">SSH into your VM</span></span>


<span data-ttu-id="63966-117">Pokud si nejste jisti již veřejnou IP adresu vašeho virtuálního počítače, spusťte [seznam veřejné ip sítě az](/cli/azure/network/public-ip#list) příkaz:</span><span class="sxs-lookup"><span data-stu-id="63966-117">If you don't already know the public IP address of your VM, run the [az network public-ip list](/cli/azure/network/public-ip#list) command:</span></span>


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

<span data-ttu-id="63966-118">Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci SSH.</span><span class="sxs-lookup"><span data-stu-id="63966-118">Use the following command to create an SSH session with the virtual machine.</span></span> <span data-ttu-id="63966-119">Nahraďte správné veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="63966-119">Substitute the correct public IP address of your virtual machine.</span></span> <span data-ttu-id="63966-120">V tomto příkladu IP adresa je *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="63966-120">In this example, the IP address is *40.68.254.142*.</span></span>

```bash
ssh azureuser@40.68.254.142
```

