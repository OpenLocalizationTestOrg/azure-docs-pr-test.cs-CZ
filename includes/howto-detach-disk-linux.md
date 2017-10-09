Pokud již nepotřebujete datový disk, který je připojený tooa virtuální počítač (VM), můžete ho snadno odpojit. Při odpojení disku z hello virtuálního počítače není hello disk odebrat z úložiště. Pokud chcete toouse hello existující data na disku hello znovu, můžete ji můžete opět připojit toohello stejného virtuálního počítače nebo jiný.  

> [!NOTE]
> Virtuální počítač v Azure používá různé typy disků – disk operačního systému, místní dočasný disk a volitelné datové disky. Podrobnosti najdete v tématu [Disky a virtuální pevné disky (VHD) pro virtuální počítače](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Pokud odstraníte hello virtuálních počítačů, nelze odpojit disk operačního systému.

## <a name="find-hello-disk"></a>Najít hello disk
Než můžete odpojit disk z virtuálního počítače je třeba toofind out hello číslo logické jednotky, které je identifikátor pro toobe disku hello odpojit. toodo, postupujte takto:

1. Otevřete rozhraní příkazového řádku Azure a [připojit tooyour předplatné](../articles/xplat-cli-connect.md). Zkontrolujte, že jste v režimu Azure Service Management (`azure config mode asm`).
2. Zjistěte, které disky jsou připojené tooyour virtuálních počítačů. Hello následující příklad vypíše disky pro virtuální počítač s názvem hello `myVM`:

    ```azurecli
    azure vm disk list myVM
    ```

    Hello výstup je podobné toohello následující ukázka:

    ```azurecli
    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
      data:    Lun  Size(GB)  Blob-Name                         OS
      data:    ---  --------  --------------------------------  -----
      data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
      data:    0    30        myDataDisk.vhd
      info:    vm disk list command OK
    ```

3. Poznámka: hello logické jednotky nebo hello **číslo logické jednotky** hello disku, které chcete toodetach.

## <a name="remove-operating-system-references-toohello-disk"></a>Odeberte disk toohello odkazy operačního systému
Před odpojením hello disku z hello Linux hosta, měli byste si ověřit, že všechny oddíly v hello disku nejsou používány. Ujistěte se, že hello operační systém nebude pokoušet tooremount je po restartování systému. Tyto kroky vrátit zpět konfigurace hello pravděpodobně jste vytvořili při [připojení](../articles/virtual-machines/linux/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) hello disku.

1. Použití hello `lsscsi` identifikátoru příkazu toodiscover hello disku. `lsscsi` můžete nainstalovat pomocí příkazu `yum install lsscsi` (v distribucích založených na Red Hat) nebo `apt-get install lsscsi` (v distribucích založených na Debian). Můžete najít identifikátor disku hello, hledaný pomocí hello číslo logické jednotky. Poslední číslo hello řazené kolekce členů v jednotlivých řádcích Hello je hello logické jednotky. V následující ukázka z hello `lsscsi`, logickou jednotku LUN 0 mapuje příliš  */dev/sdc*

    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```

2. Použití `fdisk -l <disk>` oddíly hello toodiscover přidružené toobe disku hello odpojit. Hello následující příklad ukazuje výstup hello `/dev/sdc`:

    ```bash
    Disk /dev/sdc: 1098.4 GB, 1098437885952 bytes, 2145386496 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x5a1d2a1a
    
        Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048  2145386495  1072692224   83  Linux
    ```

3. Odpojte každý oddíl uvedené pro hello disk. Odpojí technologie Hello následující příklad `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

4. Použití hello `blkid` příkaz toodiscovery hello identifikátory UUID pro všechny oddíly. Hello výstup je podobné toohello následující ukázka:

    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

5. Odebrat položky v hello **/etc/fstab** soubor přidružený hello zařízení cesty nebo identifikátory UUID pro všechny oddíly pro toobe disku hello odpojit.  Záznamy pro tento příklad můžou být:

    ```sh  
   UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
   ```

    nebo
   
   ```sh   
   /dev/sdc1   /datadrive   ext4   defaults   1   2
   ```

## <a name="detach-hello-disk"></a>Odpojte hello disk
Po nalezení číslo logické jednotky hello hello disku a odkazy na odebrané hello operačního systému jste připravené toodetach ho:

1. Odpojit hello vybraný disk z virtuálního počítače hello spuštěním příkazu hello `azure vm disk detach
   <virtual-machine-name> <LUN>`. Hello následující příklad odpojí LUN `0` z hello virtuálního počítače s názvem `myVM`:
   
    ```azurecli
    azure vm disk detach myVM 0
    ```

2. Můžete zkontrolovat, pokud tu spuštěním odpojit hello disk `azure vm disk list` znovu. Následující příklad kontroly Hello hello virtuálního počítače s názvem `myVM`:
   
    ```azurecli
    azure vm disk list myVM
    ```

    Hello výstup je podobné toohello následující příklad, který ukazuje, že je již připojen hello datový disk:

    ```azurecli
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
     info:    vm disk list command OK
    ```

Hello odpojit disk zůstane v úložišti, ale je už připojené tooa virtuální počítač.

