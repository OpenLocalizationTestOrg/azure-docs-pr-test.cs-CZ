---
title: "klíče aaaUse SSH se systémem Windows pro virtuální počítače s Linuxem | Microsoft Docs"
description: "Zjistěte, jak toogenerate a používání SSH klíče na počítači tooconnect tooa Linux virtuálního počítače s Windows v Azure."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: danlep
ms.openlocfilehash: 6c44217332538857cc2ca2e85de4b476aa71251c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-ssh-keys-with-windows-on-azure"></a><span data-ttu-id="6eb49-103">Jak klíče tooUse SSH se systémem Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="6eb49-103">How tooUse SSH keys with Windows on Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6eb49-104">Windows</span><span class="sxs-lookup"><span data-stu-id="6eb49-104">Windows</span></span>](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> * [<span data-ttu-id="6eb49-105">Linux nebo Mac.</span><span class="sxs-lookup"><span data-stu-id="6eb49-105">Linux/Mac</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>
>

<span data-ttu-id="6eb49-106">Když připojíte tooLinux virtuálních počítačů (VM) v Azure, měli byste použít [public key cryptography](https://wikipedia.org/wiki/Public-key_cryptography) tooprovide bezpečnější toolog způsob, jak v tooyour virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6eb49-106">When you connect tooLinux virtual machines (VMs) in Azure, you should use [public-key cryptography](https://wikipedia.org/wiki/Public-key_cryptography) tooprovide a more secure way toolog in tooyour Linux VM.</span></span> <span data-ttu-id="6eb49-107">Tento proces zahrnuje veřejné a privátní výměny klíčů pomocí hello zabezpečené shell (SSH) příkaz tooauthenticate místo zadání uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="6eb49-107">This process involves a public and private key exchange using hello secure shell (SSH) command tooauthenticate yourself rather than a username and password.</span></span> <span data-ttu-id="6eb49-108">Hesla jsou útoky citlivé toobrute-force, zvláště na straně Internetu virtuálních počítačů, jako jsou třeba webové servery.</span><span class="sxs-lookup"><span data-stu-id="6eb49-108">Passwords are vulnerable toobrute-force attacks, especially on Internet-facing VMs such as web servers.</span></span> <span data-ttu-id="6eb49-109">Tento článek obsahuje přehled klíčů SSH a jak toogenerate hello odpovídající klíče na počítači se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="6eb49-109">This article provides an overview of SSH keys and how toogenerate hello appropriate keys on a Windows computer.</span></span>

## <a name="overview-of-ssh-and-keys"></a><span data-ttu-id="6eb49-110">Přehled SSH a klíče</span><span class="sxs-lookup"><span data-stu-id="6eb49-110">Overview of SSH and keys</span></span>
<span data-ttu-id="6eb49-111">Tooyour virtuálního počítače s Linuxem můžete bezpečně přihlašovat pomocí veřejného a privátního klíče:</span><span class="sxs-lookup"><span data-stu-id="6eb49-111">You can securely log in tooyour Linux VM by using public and private keys:</span></span>

* <span data-ttu-id="6eb49-112">Hello **veřejný klíč** je umístěn na virtuálním počítačům s Linuxem nebo jinou službu, že si přejete toouse u kryptografie využívající veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="6eb49-112">hello **public key** is placed on your Linux VM, or any other service that you wish toouse with public-key cryptography.</span></span>
* <span data-ttu-id="6eb49-113">Hello **privátní klíč** tom, co jste při přihlášení, tooverify je přítomen tooyour virtuálního počítače s Linuxem vaši identitu.</span><span class="sxs-lookup"><span data-stu-id="6eb49-113">hello **private key** is what you present tooyour Linux VM when you log in, tooverify your identity.</span></span> <span data-ttu-id="6eb49-114">Chraňte tento privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="6eb49-114">Protect this private key.</span></span> <span data-ttu-id="6eb49-115">Nesdílejte ho.</span><span class="sxs-lookup"><span data-stu-id="6eb49-115">Do not share it.</span></span>

<span data-ttu-id="6eb49-116">Tyto veřejné a soukromé klíče lze použít na více virtuálních počítačů a služeb.</span><span class="sxs-lookup"><span data-stu-id="6eb49-116">These public and private keys can be used on multiple VMs and services.</span></span> <span data-ttu-id="6eb49-117">Pro každý virtuální počítač nebo služba chcete tooaccess nepotřebujete pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="6eb49-117">You do not need a pair of keys for each VM or service you wish tooaccess.</span></span> <span data-ttu-id="6eb49-118">Podrobnější přehled, najdete v tématu [public key cryptography](https://wikipedia.org/wiki/Public-key_cryptography).</span><span class="sxs-lookup"><span data-stu-id="6eb49-118">For a more detailed overview, see [public-key cryptography](https://wikipedia.org/wiki/Public-key_cryptography).</span></span>

<span data-ttu-id="6eb49-119">SSH je protokol šifrované připojení, která umožňuje zabezpečený přihlášení přes nezabezpečený připojení.</span><span class="sxs-lookup"><span data-stu-id="6eb49-119">SSH is an encrypted connection protocol that allows secure logins over unsecured connections.</span></span> <span data-ttu-id="6eb49-120">Je hello výchozí připojení protokolu pro virtuální počítače Linux hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="6eb49-120">It is hello default connection protocol for Linux VMs hosted in Azure.</span></span> <span data-ttu-id="6eb49-121">I když SSH, samotné poskytuje šifrované připojení, pomocí hesla s připojeními SSH stále zanechává hello virtuálních počítačů bude zranitelný toobrute-force útoky, nebo hádání hesel.</span><span class="sxs-lookup"><span data-stu-id="6eb49-121">Although SSH itself provides an encrypted connection, using passwords with SSH connections still leaves hello VM vulnerable toobrute-force attacks or guessing of passwords.</span></span> <span data-ttu-id="6eb49-122">Bezpečnější a upřednostňovanou metodu pro připojování tooa virtuálního počítače pomocí protokolu SSH je pomocí těchto veřejné a soukromé klíče, také známé jako klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="6eb49-122">A more secure and preferred method of connecting tooa VM using SSH is by using these public and private keys, also known as SSH keys.</span></span>

<span data-ttu-id="6eb49-123">Pokud nechcete, aby toouse klíče SSH, je stále přihlásit tooyour virtuální počítače s Linuxem pomocí hesla.</span><span class="sxs-lookup"><span data-stu-id="6eb49-123">If you do not wish toouse SSH keys, you can still log in tooyour Linux VMs using a password.</span></span> <span data-ttu-id="6eb49-124">Pokud virtuální počítač není zveřejněné toohello Internetu, může být dostatečná pomocí hesla.</span><span class="sxs-lookup"><span data-stu-id="6eb49-124">If your VM is not exposed toohello Internet, using passwords may be sufficient.</span></span> <span data-ttu-id="6eb49-125">Můžete však stále potřebovat toomanage hesla pro každý virtuální počítač s Linuxem a Udržovat zásady hesel v pořádku a postupy, jako je minimální délka hesla a pravidelně aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="6eb49-125">However, you still need toomanage your passwords for each Linux VM and maintain healthy password policies and practices, such as minimum password length and regularly updating them.</span></span> <span data-ttu-id="6eb49-126">Hello použití klíčů SSH snižuje složitost hello správy jednotlivých přihlašovacích údajů napříč více virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="6eb49-126">hello use of SSH keys reduces hello complexity of managing individual credentials across multiple VMs.</span></span>

## <a name="windows-packages-and-ssh-clients"></a><span data-ttu-id="6eb49-127">Balíčky pro systém Windows a klientů SSH</span><span class="sxs-lookup"><span data-stu-id="6eb49-127">Windows packages and SSH clients</span></span>
<span data-ttu-id="6eb49-128">Připojit tooand spravovat virtuální počítače s Linuxem v Azure pomocí **klient SSH**.</span><span class="sxs-lookup"><span data-stu-id="6eb49-128">You connect tooand manage Linux VMs in Azure using an **SSH client**.</span></span> <span data-ttu-id="6eb49-129">Počítače se systémem Windows obvykle nemají nainstalovaného klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="6eb49-129">Windows computers do not typically have an SSH client installed.</span></span> <span data-ttu-id="6eb49-130">Přidat Bash pro Windows Hello Windows 10 Anniversary Update a hello nejnovější aktualizace služby Windows 10 Creators nabízí další aktualizace.</span><span class="sxs-lookup"><span data-stu-id="6eb49-130">hello Windows 10 Anniversary Update added Bash for Windows, and hello latest Windows 10 Creators Update provides additional updates.</span></span> <span data-ttu-id="6eb49-131">Tato subsystému Windows pro Linux umožňuje toorun a přístup nástroje, jako je například klientem SSH nativně v rámci prostředí Bash.</span><span class="sxs-lookup"><span data-stu-id="6eb49-131">This Windows Subsystem for Linux allows you toorun and access utilities such as an SSH client natively within a Bash shell.</span></span> <span data-ttu-id="6eb49-132">Můžete pak provedením jakéhokoliv z dokumentace Linux hello, například [jak páry klíč SSH toogenerate pro Linux](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="6eb49-132">You can then follow any of hello Linux docs, such as [How toogenerate SSH key pairs for Linux](mac-create-ssh-keys.md).</span></span> <span data-ttu-id="6eb49-133">Bash pro Windows je stále ve vývoji a je považován za verzi beta.</span><span class="sxs-lookup"><span data-stu-id="6eb49-133">Bash for Windows is still under development, and is considered a beta release.</span></span> <span data-ttu-id="6eb49-134">Další informace o Bash pro systém Windows najdete v tématu [Bash na Ubuntu v systému Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="6eb49-134">For more information about Bash for Windows, see [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span>

<span data-ttu-id="6eb49-135">Pokud chcete toouse něco jiného než Bash pro systém Windows, jsou běžné Windows SSH klientů, které můžete nainstalovat součástí hello následující balíčky:</span><span class="sxs-lookup"><span data-stu-id="6eb49-135">If you wish toouse something other than Bash for Windows, common Windows SSH clients you can install are included in hello following packages:</span></span>

* [<span data-ttu-id="6eb49-136">Git pro Windows</span><span class="sxs-lookup"><span data-stu-id="6eb49-136">Git For Windows</span></span>](https://git-for-windows.github.io/)
* [<span data-ttu-id="6eb49-137">puTTY</span><span class="sxs-lookup"><span data-stu-id="6eb49-137">puTTY</span></span>](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [<span data-ttu-id="6eb49-138">MobaXterm</span><span class="sxs-lookup"><span data-stu-id="6eb49-138">MobaXterm</span></span>](http://mobaxterm.mobatek.net/)
* [<span data-ttu-id="6eb49-139">Emulaci</span><span class="sxs-lookup"><span data-stu-id="6eb49-139">Cygwin</span></span>](https://cygwin.com/)


## <a name="which-key-files-do-you-need-toocreate"></a><span data-ttu-id="6eb49-140">Které klíče soubory potřebujete toocreate?</span><span class="sxs-lookup"><span data-stu-id="6eb49-140">Which key files do you need toocreate?</span></span>
<span data-ttu-id="6eb49-141">Azure vyžaduje minimálně 2048bitové **ssh-rsa** formátu veřejné a soukromé klíče.</span><span class="sxs-lookup"><span data-stu-id="6eb49-141">Azure requires at least 2048-bit, **ssh-rsa** formatted public and private keys.</span></span> <span data-ttu-id="6eb49-142">Pokud spravujete pomocí modelu nasazení Classic hello prostředky Azure, musíte taky toogenerate PEM (`.pem` souboru).</span><span class="sxs-lookup"><span data-stu-id="6eb49-142">If you are managing Azure resources using hello Classic deployment model, you also need toogenerate a PEM (`.pem` file).</span></span>

<span data-ttu-id="6eb49-143">Zde jsou hello scénáře nasazení a hello typy souborů, které můžete použít v každém:</span><span class="sxs-lookup"><span data-stu-id="6eb49-143">Here are hello deployment scenarios, and hello types of files you use in each:</span></span>

1. <span data-ttu-id="6eb49-144">**SSH-rsa** klíče jsou požadovány pro všechna nasazení pomocí hello [portál Azure](https://portal.azure.com)a nasazení Resource Manager pomocí hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="6eb49-144">**ssh-rsa** keys are required for any deployment using hello [Azure portal](https://portal.azure.com), and Resource Manager deployments using hello [Azure CLI](../../cli-install-nodejs.md).</span></span>
   * <span data-ttu-id="6eb49-145">Tyto klíče jsou obvykle že potřebovat všechny většina lidí.</span><span class="sxs-lookup"><span data-stu-id="6eb49-145">These keys are usually all most people need.</span></span>
2. <span data-ttu-id="6eb49-146">A `.pem` soubor je požadovaná toocreate virtuální počítače pomocí nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="6eb49-146">A `.pem` file is required toocreate VMs using hello Classic deployment.</span></span> <span data-ttu-id="6eb49-147">Tyto klíče jsou podporovány v nasazení Classic při použití hello [portál Azure](https://portal.azure.com) nebo [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="6eb49-147">These keys are supported in Classic deployments when using hello [Azure portal](https://portal.azure.com) or [Azure CLI](../../cli-install-nodejs.md).</span></span>
   * <span data-ttu-id="6eb49-148">Potřebujete jenom toocreate tyto další klíčů a certifikátů Pokud spravujete prostředky vytvořené pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="6eb49-148">You only need toocreate these additional keys and certificates if you are managing resources created using hello Classic deployment model.</span></span>

## <a name="install-git-for-windows"></a><span data-ttu-id="6eb49-149">Instalace Gitu pro Windows</span><span class="sxs-lookup"><span data-stu-id="6eb49-149">Install Git for Windows</span></span>
<span data-ttu-id="6eb49-150">Hello předchozí části uveden více balíčků, které zahrnují hello `openssl` nástroje pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="6eb49-150">hello preceding section listed several packages that include hello `openssl` tool for Windows.</span></span> <span data-ttu-id="6eb49-151">Tento nástroj je potřebné toocreate veřejné a soukromé klíče.</span><span class="sxs-lookup"><span data-stu-id="6eb49-151">This tool is needed toocreate public and private keys.</span></span> <span data-ttu-id="6eb49-152">Následující příklady podrobnosti o tom, jak Hello tooinstall a používat **Git pro Windows**, i když můžete podle toho, která balíček dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="6eb49-152">hello following examples detail how tooinstall and use **Git for Windows**, though you can choose whichever package you prefer.</span></span> <span data-ttu-id="6eb49-153">**Git pro Windows** dává vám přístup toosome další open-source softwaru ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) nástrojů a pomůcek, které mohou být užitečné při práci s virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6eb49-153">**Git for Windows** gives you access toosome additional open-source software ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) tools and utilities that may be useful as you work with Linux VMs.</span></span>

1. <span data-ttu-id="6eb49-154">Stáhněte a nainstalujte **Git pro Windows** z hello následující umístění: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).</span><span class="sxs-lookup"><span data-stu-id="6eb49-154">Download and install **Git for Windows** from hello following location: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).</span></span>
2. <span data-ttu-id="6eb49-155">Přijměte výchozí možnosti hello během hello postup instalace pouze v případě potřebujete toochange je.</span><span class="sxs-lookup"><span data-stu-id="6eb49-155">Accept hello default options during hello install process unless you specifically need toochange them.</span></span>
3. <span data-ttu-id="6eb49-156">Spustit **Git Bash** z hello **nabídce Start** > **Git** > **Git Bash**.</span><span class="sxs-lookup"><span data-stu-id="6eb49-156">Run **Git Bash** from hello **Start Menu** > **Git** > **Git Bash**.</span></span> <span data-ttu-id="6eb49-157">Hello konzoly vypadá podobně jako toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="6eb49-157">hello console looks similar toohello following example:</span></span>

    ![Prostředí Git Bash pro Windows](./media/ssh-from-windows/git-bash-window.png)

## <a name="create-a-private-key"></a><span data-ttu-id="6eb49-159">Vytvoření privátního klíče</span><span class="sxs-lookup"><span data-stu-id="6eb49-159">Create a private key</span></span>
1. <span data-ttu-id="6eb49-160">Ve vaší **Git Bash** okně použití `openssl.exe` toocreate privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="6eb49-160">In your **Git Bash** window, use `openssl.exe` toocreate a private key.</span></span> <span data-ttu-id="6eb49-161">Hello následující příklad vytvoří klíč s názvem `myPrivateKey` a certifikát s názvem `myCert.pem`:</span><span class="sxs-lookup"><span data-stu-id="6eb49-161">hello following example creates a key named `myPrivateKey` and certificate named `myCert.pem`:</span></span>

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    <span data-ttu-id="6eb49-162">výstup Hello vypadá podobně jako toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="6eb49-162">hello output looks similar toohello following example:</span></span>

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key too'myPrivateKey.key'
    -----
    You are about toobe asked tooenter information that will be incorporated
    into your certificate request.
    What you are about tooenter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', hello field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

   <span data-ttu-id="6eb49-163">Pokud se zobrazí chybová zpráva: bash, zkuste otevřít nové **Git Bash** okno se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="6eb49-163">If bash reports an error, try opening a new **Git Bash** window with elevated privileges.</span></span> <span data-ttu-id="6eb49-164">Spusťte hello `openssl` příkaz.</span><span class="sxs-lookup"><span data-stu-id="6eb49-164">Then, rerun hello `openssl` command.</span></span>

2. <span data-ttu-id="6eb49-165">Odpověď hello vyzve k zadání názvu země, umístění, název organizace, atd.</span><span class="sxs-lookup"><span data-stu-id="6eb49-165">Answer hello prompts for country name, location, organization name, etc.</span></span>
3. <span data-ttu-id="6eb49-166">Váš nový privátní klíč a certifikát se vytvoří v aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="6eb49-166">Your new private key and certificate are created in your current working directory.</span></span> <span data-ttu-id="6eb49-167">Jako bezpečnostní opatření měli byste nastavit hello oprávnění na váš privátní klíč, aby pouze budete mít přístup:</span><span class="sxs-lookup"><span data-stu-id="6eb49-167">As a security measure, you should set hello permissions on your private key so that only you can access it:</span></span>

    ```bash
    chmod 0600 myPrivateKey.key
    ```

4. <span data-ttu-id="6eb49-168">Hello [další části](#create-a-private-key-for-putty) podrobnosti o pomocí PuTTYgen tooboth zobrazení a použití hello veřejný klíč a vytvoření privátní klíč specifické pro používání PuTTY tooSSH tooLinux virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6eb49-168">hello [next section](#create-a-private-key-for-putty) details using PuTTYgen tooboth view and use hello public key, and create a private key specific for using PuTTY tooSSH tooLinux VMs.</span></span> <span data-ttu-id="6eb49-169">Hello následující příkaz vytvoří soubor veřejného klíče s názvem `myPublicKey.key` , můžete použít:</span><span class="sxs-lookup"><span data-stu-id="6eb49-169">hello following command generates a public key file named `myPublicKey.key` that you can use right away:</span></span>

    ```bash
    openssl.exe rsa -pubout -in myPrivateKey.key -out myPublicKey.key
    ```

5. <span data-ttu-id="6eb49-170">Pokud budete také potřebovat toomanage klasické prostředky, převést hello `myCert.pem` příliš`myCert.cer` (X509, kódování DER certifikát).</span><span class="sxs-lookup"><span data-stu-id="6eb49-170">If you also need toomanage Classic resources, convert hello `myCert.pem` too`myCert.cer` (DER encoded X509 certificate).</span></span> <span data-ttu-id="6eb49-171">Proveďte tento volitelný krok jenom v případě, že potřebujete toospecifically spravovat starší klasické prostředky.</span><span class="sxs-lookup"><span data-stu-id="6eb49-171">Perform this optional step only if you need toospecifically manage older Classic resources.</span></span>

    <span data-ttu-id="6eb49-172">Převeďte hello certifikát pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6eb49-172">Convert hello certificate using hello following command:</span></span>

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a><span data-ttu-id="6eb49-173">Vytvoření privátní klíč pro PuTTY</span><span class="sxs-lookup"><span data-stu-id="6eb49-173">Create a private key for PuTTY</span></span>
<span data-ttu-id="6eb49-174">PuTTY je běžné SSH klient pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="6eb49-174">PuTTY is a common SSH client for Windows.</span></span> <span data-ttu-id="6eb49-175">Můžete se volné toouse libovolného klienta SSH, který chcete.</span><span class="sxs-lookup"><span data-stu-id="6eb49-175">You are free toouse any SSH client that you wish.</span></span> <span data-ttu-id="6eb49-176">toouse PuTTY, musíte toocreate další typ klíče - privátní klíč PuTTY (PPK).</span><span class="sxs-lookup"><span data-stu-id="6eb49-176">toouse PuTTY, you need toocreate an additional type of key - a PuTTY Private Key (PPK).</span></span> <span data-ttu-id="6eb49-177">Pokud nechcete, aby toouse PuTTY, tuto část přeskočte.</span><span class="sxs-lookup"><span data-stu-id="6eb49-177">If you do not wish toouse PuTTY, skip this section.</span></span>

<span data-ttu-id="6eb49-178">Hello následující příklad vytvoří tento další privátní klíč speciálně pro PuTTY toouse:</span><span class="sxs-lookup"><span data-stu-id="6eb49-178">hello following example creates this additional private key specifically for PuTTY toouse:</span></span>

1. <span data-ttu-id="6eb49-179">Použití **Git Bash** tooconvert vaší privátní klíče do privátní klíč RSA, že můžete porozumět PuTTYgen.</span><span class="sxs-lookup"><span data-stu-id="6eb49-179">Use **Git Bash** tooconvert your private key into an RSA private key that PuTTYgen can understand.</span></span> <span data-ttu-id="6eb49-180">Hello následující příklad vytvoří klíč s názvem `myPrivateKey_rsa` z hello existující klíč s názvem `myPrivateKey`:</span><span class="sxs-lookup"><span data-stu-id="6eb49-180">hello following example creates a key named `myPrivateKey_rsa` from hello existing key named `myPrivateKey`:</span></span>

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    <span data-ttu-id="6eb49-181">Jako bezpečnostní opatření měli byste nastavit hello oprávnění na váš privátní klíč, aby pouze budete mít přístup:</span><span class="sxs-lookup"><span data-stu-id="6eb49-181">As a security measure, you should set hello permissions on your private key so that only you can access it:</span></span>

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```
2. <span data-ttu-id="6eb49-182">Stažení a spuštění PuTTYgen z hello následující umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="6eb49-182">Download and run PuTTYgen from hello following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
3. <span data-ttu-id="6eb49-183">V nabídce hello: **soubor** > **zatížení privátní klíč**</span><span class="sxs-lookup"><span data-stu-id="6eb49-183">Click hello menu: **File** > **Load Private Key**</span></span>
4. <span data-ttu-id="6eb49-184">Najít váš privátní klíč (`myPrivateKey_rsa` v předchozím příkladu hello).</span><span class="sxs-lookup"><span data-stu-id="6eb49-184">Locate your private key (`myPrivateKey_rsa` in hello previous example).</span></span> <span data-ttu-id="6eb49-185">Výchozí adresář Hello při spuštění **Git Bash** je `C:\Users\%username%`.</span><span class="sxs-lookup"><span data-stu-id="6eb49-185">hello default directory when you start **Git Bash** is `C:\Users\%username%`.</span></span> <span data-ttu-id="6eb49-186">Změnit hello soubor filtru tooshow **všechny soubory (\*.\*)** :</span><span class="sxs-lookup"><span data-stu-id="6eb49-186">Change hello file filter tooshow **All Files (\*.\*)**:</span></span>

    ![Načtení hello existující soukromý klíč do PuTTYgen](./media/ssh-from-windows/load-private-key.png)
5. <span data-ttu-id="6eb49-188">Klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="6eb49-188">Click **Open**.</span></span> <span data-ttu-id="6eb49-189">Příkazovém řádku uvedena, že byl úspěšně importován klíči hello:</span><span class="sxs-lookup"><span data-stu-id="6eb49-189">A prompt indicates that hello key has been successfully imported:</span></span>

    ![Byly úspěšně importovány klíče tooPuTTYgen](./media/ssh-from-windows/successfully-imported-key.png)
6. <span data-ttu-id="6eb49-191">Klikněte na tlačítko **OK** tooclose hello řádku.</span><span class="sxs-lookup"><span data-stu-id="6eb49-191">Click **OK** tooclose hello prompt.</span></span>
7. <span data-ttu-id="6eb49-192">veřejný klíč Hello se zobrazí v horní části hello Dobrý den **PuTTYgen** okno.</span><span class="sxs-lookup"><span data-stu-id="6eb49-192">hello public key is displayed at hello top of hello **PuTTYgen** window.</span></span> <span data-ttu-id="6eb49-193">Zkopírujte a vložte tento veřejný klíč do hello portál Azure nebo do šablony Azure Resource Manager, když vytvoříte virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6eb49-193">You copy and paste this public key into hello Azure portal or Azure Resource Manager template when you create a Linux VM.</span></span> <span data-ttu-id="6eb49-194">Můžete také kliknout na **uložit veřejný klíč** toosave počítač tooyour kopie:</span><span class="sxs-lookup"><span data-stu-id="6eb49-194">You can also click **Save public key** toosave a copy tooyour computer:</span></span>

    ![Uložte soubor PuTTY veřejného klíče](./media/ssh-from-windows/save-public-key.png)

    <span data-ttu-id="6eb49-196">Hello následující příklad ukazuje, jak by zkopírujte a vložte tento veřejný klíč do hello portálu Azure při vytváření virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6eb49-196">hello following example shows how you would copy and paste this public key into hello Azure portal when you create a Linux VM.</span></span> <span data-ttu-id="6eb49-197">veřejný klíč Hello je obvykle pak uloženy v `~/.ssh/authorized_keys` na nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6eb49-197">hello public key is typically then stored in `~/.ssh/authorized_keys` on your new VM.</span></span>

    ![Veřejný klíč použít při vytváření virtuálního počítače v hello portálu Azure](./media/ssh-from-windows/use-public-key-azure-portal.png)
8. <span data-ttu-id="6eb49-199">Zpět v **PuTTYgen**, klikněte na tlačítko **uložit privátní klíč**:</span><span class="sxs-lookup"><span data-stu-id="6eb49-199">Back in **PuTTYgen**, Click **Save private Key**:</span></span>

    ![Uložte soubor privátního klíče PuTTY](./media/ssh-from-windows/save-ppk-file.png)

   > [!WARNING]
   > <span data-ttu-id="6eb49-201">Zobrazí výzva s dotazem chcete toocontinue bez zadávání přístupové heslo klíče.</span><span class="sxs-lookup"><span data-stu-id="6eb49-201">A prompt asks if you wish toocontinue without entering a passphrase for your key.</span></span> <span data-ttu-id="6eb49-202">Přístupové heslo je stejné jako heslo připojené tooyour privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="6eb49-202">A passphrase is like a password attached tooyour private key.</span></span> <span data-ttu-id="6eb49-203">I když někdo byly tooobtain váš privátní klíč, stále by nebyly možné tooauthenticate pomocí právě hello klíče.</span><span class="sxs-lookup"><span data-stu-id="6eb49-203">Even if someone were tooobtain your private key, they still would not be able tooauthenticate using just hello key.</span></span> <span data-ttu-id="6eb49-204">Potřebují by také hello přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="6eb49-204">They would also need hello passphrase.</span></span> <span data-ttu-id="6eb49-205">Bez přístupové heslo Pokud někdo získá váš privátní klíč, se můžete přihlásit tooany virtuálního počítače nebo služby tohoto použije klíč.</span><span class="sxs-lookup"><span data-stu-id="6eb49-205">Without a passphrase, if someone obtains your private key, they can log in tooany VM or service that uses that key.</span></span> <span data-ttu-id="6eb49-206">Doporučujeme že vytvořit přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="6eb49-206">We recommend you create a passphrase.</span></span> <span data-ttu-id="6eb49-207">Ale pokud zapomenete heslo hello, neexistuje žádný způsob, jak toorecover ho.</span><span class="sxs-lookup"><span data-stu-id="6eb49-207">However, if you forget hello passphrase, there is no way toorecover it.</span></span>
   >
   >

    <span data-ttu-id="6eb49-208">Pokud chcete tooenter přístupové heslo, klikněte na tlačítko **ne**, zadejte přístupové heslo v hlavním okně PuTTYgen hello a pak klikněte na tlačítko **uložit privátní klíč** znovu.</span><span class="sxs-lookup"><span data-stu-id="6eb49-208">If you wish tooenter a passphrase, click **No**, enter a passphrase in hello main PuTTYgen window, and then click **Save private key** again.</span></span> <span data-ttu-id="6eb49-209">Jinak, klikněte na tlačítko **Ano** toocontinue bez zadání hello volitelné přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="6eb49-209">Otherwise, click **Yes** toocontinue without providing hello optional passphrase.</span></span>
9. <span data-ttu-id="6eb49-210">Zadejte název a umístění toosave souboru PPK.</span><span class="sxs-lookup"><span data-stu-id="6eb49-210">Enter a name and location toosave your PPK file.</span></span>

## <a name="use-putty-toossh-tooa-linux-machine"></a><span data-ttu-id="6eb49-211">Použít Putty tooSSH tooa počítač Linux</span><span class="sxs-lookup"><span data-stu-id="6eb49-211">Use Putty tooSSH tooa Linux Machine</span></span>
<span data-ttu-id="6eb49-212">PuTTY znovu, je běžné SSH klient pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="6eb49-212">Again, PuTTY is a common SSH client for Windows.</span></span> <span data-ttu-id="6eb49-213">Můžete se volné toouse libovolného klienta SSH, který chcete.</span><span class="sxs-lookup"><span data-stu-id="6eb49-213">You are free toouse any SSH client that you wish.</span></span> <span data-ttu-id="6eb49-214">Dobrý den, následující kroky podrobnosti o tom, jak toouse vaší privátní klíče tooauthenticate s svého virtuálního počítače Azure pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="6eb49-214">hello following steps detail how toouse your private key tooauthenticate with your Azure VM using SSH.</span></span> <span data-ttu-id="6eb49-215">Hello kroky se podobají ostatní klíče SSH klienty z hlediska nutnosti tooload vaší privátní klíče tooauthenticate hello připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="6eb49-215">hello steps are similar in other SSH key clients in terms of needing tooload your private key tooauthenticate hello SSH connection.</span></span>

1. <span data-ttu-id="6eb49-216">Stažení a spuštění putty z hello následující umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="6eb49-216">Download and run putty from hello following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
2. <span data-ttu-id="6eb49-217">Vyplňte hello název hostitele nebo IP adresu virtuálního počítače z hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="6eb49-217">Fill in hello host name or IP address of your VM from hello Azure portal:</span></span>

    ![Otevřít nové připojení PuTTY](./media/ssh-from-windows/putty-new-connection.png)
3. <span data-ttu-id="6eb49-219">Před výběrem **otevřete**, klikněte na tlačítko **připojení** > **SSH** > **Auth** kartě. Procházet tooand vyberte privátní klíč:</span><span class="sxs-lookup"><span data-stu-id="6eb49-219">Before selecting **Open**, click **Connection** > **SSH** > **Auth** tab. Browse tooand select your private key:</span></span>

    ![Vyberte PuTTY privátní klíč pro ověřování](./media/ssh-from-windows/putty-auth-dialog.png)
4. <span data-ttu-id="6eb49-221">Klikněte na tlačítko **otevřete** tooconnect tooyour virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6eb49-221">Click **Open** tooconnect tooyour virtual machine</span></span>

## <a name="next-steps"></a><span data-ttu-id="6eb49-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6eb49-222">Next steps</span></span>
<span data-ttu-id="6eb49-223">Můžete také vygenerovat hello veřejné a soukromé klíče [pomocí OS X a Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6eb49-223">You can also generate hello public and private keys [using OS X and Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="6eb49-224">Další informace o Bash pro systém Windows a výhodách hello operačních systémů nástroje snadno dostupné na počítači s Windows najdete v tématu [Bash na Ubuntu v systému Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="6eb49-224">For more information about Bash for Windows and hello benefits of having OSS tools readily available on your Windows computer, see [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span>

<span data-ttu-id="6eb49-225">Pokud máte potíže při používání SSH tooconnect tooyour virtuální počítače s Linuxem, přečtěte si téma [řešení SSH připojení tooan virtuální počítač Azure s Linuxem](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6eb49-225">If you have trouble using SSH tooconnect tooyour Linux VMs, see [Troubleshoot SSH connections tooan Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
