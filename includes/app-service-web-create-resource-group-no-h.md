Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.

[!INCLUDE [resource group intro text](resource-group.md)]

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westeurope* umístění.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Obvykle vytvoříte prostředek skupiny a hello prostředky v oblasti okolo vás. toosee podporované umístění Azure Web Apps, spusťte hello `az appservice list-locations` příkaz. 
