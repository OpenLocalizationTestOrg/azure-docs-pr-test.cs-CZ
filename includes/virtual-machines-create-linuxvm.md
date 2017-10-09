
1. Přihlaste se tooyour předplatného Azure, pomocí kroků uvedených v tomto seznamu hello [připojit tooAzure z hello Azure CLI 1.0](../articles/xplat-cli-connect.md).

2. Zkontrolujte, zda že jsou v režimu nasazení Classic hello následujícím způsobem:

    ```azurecli
    azure config mode asm
    ```

3. Zjistěte hello Linux image, které chcete tooload z dostupných imagí hello následujícím způsobem:

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    V okně příkazového řádku Windows místo grep použijte **find**.
   
4. Použití `azure vm create` toocreate virtuální počítač s bitovou kopii systému Linux hello z předchozího seznamu hello. Tento krok vytvoří cloudovou službu a účet úložiště. Může také připojit tento počítač tooan existující Cloudová služba se `-c` možnost. Vytvoření toolog koncový bod SSH v toohello Linux virtuální počítač s hello `-e` možnost. Hello následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí hello `Ubuntu-14_04_4-LTS` bitové kopie v hello `West US` umístění a přidá uživatelské jméno `ops`:
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    Hello výstup je podobné toohello následující ukázka:

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
   > Pro virtuální počítač s Linuxem, je nutné zadat hello `-e` možnost `vm create`. Není možné tooenable SSH po vytvoření hello virtuálního počítače. Další informace o SSH, najdete v tématu [jak tooUse SSH s Linuxem v Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

5. Atributy hello hello virtuálních počítačů lze ověřit pomocí hello `azure vm show` příkaz. Hello následující příklad uvádí informace o hello virtuálního počítače s názvem `myVM`:

    ```azurecli   
    azure vm show myVM
    ```

6. Spustit virtuální počítač s hello `azure vm start` příkaz takto:

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a>Další kroky
Podrobnosti o všech těchto příkazů virtuálního počítače Azure CLI 1.0, přečtěte si hello [hello pomocí Azure CLI 1.0 pomocí rozhraní API nasazení Classic hello](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

