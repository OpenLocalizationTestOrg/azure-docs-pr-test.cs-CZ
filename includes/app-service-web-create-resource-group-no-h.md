<span data-ttu-id="482af-101">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="482af-101">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [resource group intro text](resource-group.md)]

<span data-ttu-id="482af-102">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westeurope* umístění.</span><span class="sxs-lookup"><span data-stu-id="482af-102">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="482af-103">Obvykle vytvoříte prostředek skupiny a hello prostředky v oblasti okolo vás.</span><span class="sxs-lookup"><span data-stu-id="482af-103">You generally create your resource group and hello resources in a region near you.</span></span> <span data-ttu-id="482af-104">toosee podporované umístění Azure Web Apps, spusťte hello `az appservice list-locations` příkaz.</span><span class="sxs-lookup"><span data-stu-id="482af-104">toosee all supported locations for Azure Web Apps, run hello `az appservice list-locations` command.</span></span> 
