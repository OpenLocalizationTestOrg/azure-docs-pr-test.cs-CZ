## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvoření virtuálního počítače s hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz. 

Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* a vytvoří klíče SSH, pokud už neexistují ve výchozím umístění klíče. toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Po vytvoření hello virtuálních počítačů, hello rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace. Poznamenejte si hello `publicIpAddress`. Tato adresa je použité tooaccess hello virtuálních počítačů.

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



## <a name="open-port-80-for-web-traffic"></a>Otevření portu 80 pro webový provoz 

Ve výchozím nastavení jsou povoleny pouze připojení SSH do virtuálních počítačů Linux nasazené v Azure. Protože tento virtuální počítač bude toobe webový server, je třeba tooopen port 80 z hello Internetu. Použití hello [az virtuálních počítačů open-port](/cli/azure/vm#open-port) příkaz tooopen hello požadovaného portu.  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a>Připojení SSH k virtuálnímu počítači


Pokud neznáte hello veřejnou IP adresu vašeho virtuálního počítače, spusťte hello [seznam veřejné ip sítě az](/cli/azure/network/public-ip#list) příkaz:


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Použití hello následující příkaz toocreate na relace SSH s hello virtuálního počítače. Nahraďte hello správné veřejnou IP adresu virtuálního počítače. V tomto příkladu hello IP adresa je *40.68.254.142*.

```bash
ssh azureuser@40.68.254.142
```

