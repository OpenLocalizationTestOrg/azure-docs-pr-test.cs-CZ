
1. <span data-ttu-id="56943-101">Přihlaste se tooyour předplatného Azure, pomocí kroků uvedených v tomto seznamu hello [připojit tooAzure z hello Azure CLI 1.0](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="56943-101">Sign in tooyour Azure subscription using hello steps listed in [Connect tooAzure from hello Azure CLI 1.0](../articles/xplat-cli-connect.md).</span></span>

2. <span data-ttu-id="56943-102">Zkontrolujte, zda že jsou v režimu nasazení Classic hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="56943-102">Make sure you are in hello Classic deployment mode as follows:</span></span>

    ```azurecli
    azure config mode asm
    ```

3. <span data-ttu-id="56943-103">Zjistěte hello Linux image, které chcete tooload z dostupných imagí hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="56943-103">Find out hello Linux image that you want tooload from hello available images as follows:</span></span>

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    <span data-ttu-id="56943-104">V okně příkazového řádku Windows místo grep použijte **find**.</span><span class="sxs-lookup"><span data-stu-id="56943-104">In a Windows command-prompt window, use **find** instead of grep.</span></span>
   
4. <span data-ttu-id="56943-105">Použití `azure vm create` toocreate virtuální počítač s bitovou kopii systému Linux hello z předchozího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="56943-105">Use `azure vm create` toocreate a VM with hello Linux image from hello previous list.</span></span> <span data-ttu-id="56943-106">Tento krok vytvoří cloudovou službu a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="56943-106">This step creates a cloud service and storage account.</span></span> <span data-ttu-id="56943-107">Může také připojit tento počítač tooan existující Cloudová služba se `-c` možnost.</span><span class="sxs-lookup"><span data-stu-id="56943-107">You could also connect this VM tooan existing cloud service with a `-c` option.</span></span> <span data-ttu-id="56943-108">Vytvoření toolog koncový bod SSH v toohello Linux virtuální počítač s hello `-e` možnost.</span><span class="sxs-lookup"><span data-stu-id="56943-108">Create an SSH endpoint toolog in toohello Linux virtual machine with hello `-e` option.</span></span> <span data-ttu-id="56943-109">Hello následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí hello `Ubuntu-14_04_4-LTS` bitové kopie v hello `West US` umístění a přidá uživatelské jméno `ops`:</span><span class="sxs-lookup"><span data-stu-id="56943-109">hello following example creates a VM named `myVM` using hello `Ubuntu-14_04_4-LTS` image in hello `West US` location, and adds a user name `ops`:</span></span>
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    <span data-ttu-id="56943-110">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="56943-110">hello output is similar toohello following example:</span></span>

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
   > <span data-ttu-id="56943-111">Pro virtuální počítač s Linuxem, je nutné zadat hello `-e` možnost `vm create`.</span><span class="sxs-lookup"><span data-stu-id="56943-111">For a Linux virtual machine, you must provide hello `-e` option in `vm create`.</span></span> <span data-ttu-id="56943-112">Není možné tooenable SSH po vytvoření hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="56943-112">It is not possible tooenable SSH after hello virtual machine has been created.</span></span> <span data-ttu-id="56943-113">Další informace o SSH, najdete v tématu [jak tooUse SSH s Linuxem v Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="56943-113">For more details on SSH, read [How tooUse SSH with Linux on Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

5. <span data-ttu-id="56943-114">Atributy hello hello virtuálních počítačů lze ověřit pomocí hello `azure vm show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="56943-114">You can verify hello attributes of hello VM by using hello `azure vm show` command.</span></span> <span data-ttu-id="56943-115">Hello následující příklad uvádí informace o hello virtuálního počítače s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="56943-115">hello following example lists information for hello VM named `myVM`:</span></span>

    ```azurecli   
    azure vm show myVM
    ```

6. <span data-ttu-id="56943-116">Spustit virtuální počítač s hello `azure vm start` příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="56943-116">Start your VM with hello `azure vm start` command as follows:</span></span>

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a><span data-ttu-id="56943-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56943-117">Next steps</span></span>
<span data-ttu-id="56943-118">Podrobnosti o všech těchto příkazů virtuálního počítače Azure CLI 1.0, přečtěte si hello [hello pomocí Azure CLI 1.0 pomocí rozhraní API nasazení Classic hello](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="56943-118">For details on all these Azure CLI 1.0 virtual machine commands, read hello [Using hello Azure CLI 1.0 with hello Classic deployment API](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

