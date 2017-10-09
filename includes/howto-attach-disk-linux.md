
Další informace o discích najdete v tématu věnovaném [diskům a virtuálním pevným diskům pro virtuální počítače](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

<a id="attachempty"></a>

## <a name="attach-an-empty-disk"></a>Připojení prázdného disku
1. Otevřete Azure CLI 1.0 a [připojit tooyour předplatné](../articles/xplat-cli-connect.md). Zkontrolujte, že jste v režimu Azure Service Management (`azure config mode asm`).
2. Zadejte `azure vm disk attach-new` toocreate a připojit nový disk, jak ukazuje následující příklad hello. Nahraďte *Můjvp* s názvem hello systému Linux virtuálního počítače a zadejte velikost hello hello disku v GB, což je *100GB* v tomto příkladu:

    ```azurecli
    azure vm disk attach-new myVM 100
    ```

3. Jakmile hello datový disk se vytvoří a připojené, je uvedena ve výstup hello `azure vm disk list <virtual-machine-name>` jak ukazuje následující příklad hello:
   
    ```azurecli
    azure vm disk list TestVM
    ```

    Hello výstup je podobné toohello následující ukázka:

    ```bash
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        myVM-2645b8030676c8f8.vhd  Linux
     data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

<a id="attachexisting"></a>

## <a name="attach-an-existing-disk"></a>Připojení stávajícího disku
Připojení stávajícího disku vyžaduje, aby v účtu úložiště byl dostupný soubor .vhd.

1. Otevřete Azure CLI 1.0 a [připojit tooyour předplatné](../articles/xplat-cli-connect.md). Zkontrolujte, že jste v režimu Azure Service Management (`azure config mode asm`).
2. Zkontrolujte, pokud hello virtuálního pevného disku, které chcete tooattach, je již nahrán tooyour předplatné Azure:
   
    ```azurecli
    azure vm disk list
    ```

    Hello výstup je podobné toohello následující ukázka:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
     data:    Name                                          OS
     data:    --------------------------------------------  -----
     data:    myTestVhd                                     Linux
     data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
     data:    TestVM-ubuntuVMasm-0-201508060040530369
     info:    vm disk list command OK
    ```

3. Pokud nenajdete hello disku, při němž toouse, místní předplatné tooyour virtuální pevný disk může odeslat pomocí `azure vm disk create` nebo `azure vm disk upload`. Příklad `disk create` by jako hello následující ukázka:
   
    ```azurecli
    azure vm disk create myVhd .\TempDisk\test.VHD -l "East US" -o Linux
    ```

    Hello výstup je podobné toohello následující ukázka:

    ```azurecli
    info:    Executing command vm disk create
    + Retrieving storage accounts
    info:    VHD size : 10 GB
    info:    Uploading 10485760.5 KB
    Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
    info:    Finishing computing MD5 hash, 16% is complete.
    info:    https://mystorageaccount.blob.core.windows.net/disks/myVHD.vhd was
    uploaded successfully
    info:    vm disk create command OK
    ```
   
   Můžete také používat `azure vm disk upload` tooupload účet virtuálního pevného disku tooa konkrétní úložiště. Přečtěte si další informace o hello příkazy toomanage vaše datových disků virtuálního počítače Azure [přes zde](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

4. Teď můžete připojit hello potřeby virtuálního pevného disku tooyour virtuálního počítače:
   
    ```azurecli
    azure vm disk attach myVM myVhd
    ```
   
   Ujistěte se, že tooreplace *Můjvp* s názvem hello virtuálního počítače, a *myVHD* s vaší požadované virtuální pevný disk.

5. Můžete ověřit hello disk je připojený toohello virtuální počítač s `azure vm disk list <virtual-machine-name>`:
   
    ```azurecli
    azure vm disk list myVM
    ```

    Hello výstup je podobné toohello následující ukázka:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    1    10        test.VHD
     data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

> [!NOTE]
> Po přidání datový disk, budete potřebovat toolog toohello virtuálního počítače a inicializaci hello disku, aby hello virtuálního počítače můžete použít hello disku pro úložiště (viz hello následující kroky pro další informace o tom, jak toodo inicializovat hello disk).
> 
> 

