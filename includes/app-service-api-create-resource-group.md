<span data-ttu-id="64bed-101">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="64bed-101">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [resource group intro text](resource-group.md)]

<span data-ttu-id="64bed-102">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westeurope* umístění.</span><span class="sxs-lookup"><span data-stu-id="64bed-102">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="64bed-103">k dispozici umístění hello toosee, spusťte hello `az appservice list-locations` příkaz.</span><span class="sxs-lookup"><span data-stu-id="64bed-103">toosee hello available locations, run hello `az appservice list-locations` command.</span></span> <span data-ttu-id="64bed-104">Obvykle budete prostředky vytvářet v oblasti, kterou máte blízko.</span><span class="sxs-lookup"><span data-stu-id="64bed-104">You generally create resources in a region near you.</span></span>
