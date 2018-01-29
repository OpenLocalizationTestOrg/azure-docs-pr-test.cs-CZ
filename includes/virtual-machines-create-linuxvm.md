
1. Přihlaste se k předplatnému Azure, pomocí kroků uvedených v tématu věnovaném [připojení k Azure z rozhraní příkazového řádku Azure CLI 1.0](/cli/azure/authenticate-azure-cli).

2. Pomocí následujícího postupu zkontrolujte, že používáte režim nasazení Classic:

    ```azurecli
    azure config mode asm
    ```

3. Pomocí následujícího příkazu najděte v seznamu dostupných imagí linuxovou image, kterou chcete nahrát:

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    V okně příkazového řádku Windows místo grep použijte **find**.
   
4. Pomocí příkazu `azure vm create` vytvořte virtuální počítač s linuxovou imagí z předcházejícího seznamu. Tento krok vytvoří cloudovou službu a účet úložiště. Pomocí možnosti `-c` je také možné připojit tento virtuální počítač ke stávající cloudové službě. Pomocí možnosti `-e` vytvořte koncový bod SSH pro přihlášení k virtuálnímu počítači s Linuxem. V následujícím příkladu se vytvoří virtuální počítač s názvem `myVM` pomocí image `Ubuntu-14_04_4-LTS` v umístění `West US` a přidá se uživatelské jméno `ops`:
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    Výstup se podobá následujícímu příkladu:

    ```azurecli
    info:    Executing command vm create
    + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB
    + Looking up cloud service
    info:    cloud service myVM not found.
    + Creating cloud service
    + Retrieving storage accounts
    + Creating VM
    info:    vm create command OK
    ```
   
   > [!NOTE]
   > Pro virtuální počítač s Linuxem musíte v příkazu `vm create` zadat možnost `-e`. Po vytvoření virtuálního počítače už není možné SSH povolit. Další informace o SSH najdete v tématu [Jak použít SSH s Linuxem v Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

5. Atributy virtuálního počítače můžete ověřit pomocí příkazu `azure vm show`. Následující příklad zobrazí informace pro virtuální počítač s názvem `myVM`:

    ```azurecli   
    azure vm show myVM
    ```

6. Ke spuštění virtuálního počítače použijte příkaz `azure vm start`:

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a>Další kroky
Podrobné informace o všech těchto příkazech rozhraní příkazového řádku Azure CLI 1.0 pro virtuální počítače najdete v tématu věnovaném [použití rozhraní příkazového řádku Azure CLI 1.0 s rozhraním API nasazení Classic](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

