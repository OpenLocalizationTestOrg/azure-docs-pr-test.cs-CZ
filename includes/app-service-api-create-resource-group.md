<span data-ttu-id="f0fc5-101">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="f0fc5-101">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [resource group intro text](resource-group.md)]

<span data-ttu-id="f0fc5-102">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *westeurope*.</span><span class="sxs-lookup"><span data-stu-id="f0fc5-102">The following example creates a resource group named *myResourceGroup* in the *westeurope* location.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="f0fc5-103">Pokud chcete zobrazit dostupná umístění, spusťte příkaz `az appservice list-locations`.</span><span class="sxs-lookup"><span data-stu-id="f0fc5-103">To see the available locations, run the `az appservice list-locations` command.</span></span> <span data-ttu-id="f0fc5-104">Obvykle budete prostředky vytvářet v oblasti, kterou máte blízko.</span><span class="sxs-lookup"><span data-stu-id="f0fc5-104">You generally create resources in a region near you.</span></span>
