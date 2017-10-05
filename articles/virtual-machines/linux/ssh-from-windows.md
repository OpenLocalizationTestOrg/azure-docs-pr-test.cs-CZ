---
title: "Používat klíče SSH se systémem Windows pro virtuální počítače s Linuxem | Microsoft Docs"
description: "Zjistěte, jak vygenerovat a používat klíče SSH na počítači se systémem Windows pro připojení k virtuální počítač s Linuxem v Azure."
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
ms.openlocfilehash: 66837a3a153cda041f5351c52c8ccb1f8ccfea50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a><span data-ttu-id="08c60-103">Postup použití SSH klíče s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="08c60-103">How to Use SSH keys with Windows on Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="08c60-104">Windows</span><span class="sxs-lookup"><span data-stu-id="08c60-104">Windows</span></span>](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> * [<span data-ttu-id="08c60-105">Linux nebo Mac.</span><span class="sxs-lookup"><span data-stu-id="08c60-105">Linux/Mac</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>
>

<span data-ttu-id="08c60-106">Když připojíte k Linux virtuálních počítačů (VM) v Azure, měli byste použít [public key cryptography](https://wikipedia.org/wiki/Public-key_cryptography) zajistit bezpečnější způsob, jak Přihlaste se k virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="08c60-106">When you connect to Linux virtual machines (VMs) in Azure, you should use [public-key cryptography](https://wikipedia.org/wiki/Public-key_cryptography) to provide a more secure way to log in to your Linux VM.</span></span> <span data-ttu-id="08c60-107">Tento proces zahrnuje veřejné a privátní výměny klíčů pomocí příkazu zabezpečené shell (SSH) k ověření sami spíše než uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="08c60-107">This process involves a public and private key exchange using the secure shell (SSH) command to authenticate yourself rather than a username and password.</span></span> <span data-ttu-id="08c60-108">Hesla se stát terčem útoků, zvláště na straně Internetu virtuálních počítačů, jako jsou třeba webové servery hrubou silou.</span><span class="sxs-lookup"><span data-stu-id="08c60-108">Passwords are vulnerable to brute-force attacks, especially on Internet-facing VMs such as web servers.</span></span> <span data-ttu-id="08c60-109">Tento článek obsahuje přehled klíčů SSH a jak vygenerovat odpovídající klíče na počítači se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="08c60-109">This article provides an overview of SSH keys and how to generate the appropriate keys on a Windows computer.</span></span>

## <a name="overview-of-ssh-and-keys"></a><span data-ttu-id="08c60-110">Přehled SSH a klíče</span><span class="sxs-lookup"><span data-stu-id="08c60-110">Overview of SSH and keys</span></span>
<span data-ttu-id="08c60-111">Vám umožní bezpečně přihlásit k virtuálním počítačům s Linuxem pomocí veřejného a privátního klíče:</span><span class="sxs-lookup"><span data-stu-id="08c60-111">You can securely log in to your Linux VM by using public and private keys:</span></span>

* <span data-ttu-id="08c60-112">**Veřejný klíč** je umístěn na virtuálním počítačům s Linuxem nebo jiné služby, který chcete použít u kryptografie využívající veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="08c60-112">The **public key** is placed on your Linux VM, or any other service that you wish to use with public-key cryptography.</span></span>
* <span data-ttu-id="08c60-113">**Privátní klíč** je co je k dispozici k virtuálním počítačům s Linuxem při přihlášení, ověřit vaši identitu.</span><span class="sxs-lookup"><span data-stu-id="08c60-113">The **private key** is what you present to your Linux VM when you log in, to verify your identity.</span></span> <span data-ttu-id="08c60-114">Chraňte tento privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="08c60-114">Protect this private key.</span></span> <span data-ttu-id="08c60-115">Nesdílejte ho.</span><span class="sxs-lookup"><span data-stu-id="08c60-115">Do not share it.</span></span>

<span data-ttu-id="08c60-116">Tyto veřejné a soukromé klíče lze použít na více virtuálních počítačů a služeb.</span><span class="sxs-lookup"><span data-stu-id="08c60-116">These public and private keys can be used on multiple VMs and services.</span></span> <span data-ttu-id="08c60-117">Pro každý virtuální počítač nebo službu, kterou chcete pro přístup k nepotřebujete pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="08c60-117">You do not need a pair of keys for each VM or service you wish to access.</span></span> <span data-ttu-id="08c60-118">Podrobnější přehled, najdete v tématu [public key cryptography](https://wikipedia.org/wiki/Public-key_cryptography).</span><span class="sxs-lookup"><span data-stu-id="08c60-118">For a more detailed overview, see [public-key cryptography](https://wikipedia.org/wiki/Public-key_cryptography).</span></span>

<span data-ttu-id="08c60-119">SSH je protokol šifrované připojení, která umožňuje zabezpečený přihlášení přes nezabezpečený připojení.</span><span class="sxs-lookup"><span data-stu-id="08c60-119">SSH is an encrypted connection protocol that allows secure logins over unsecured connections.</span></span> <span data-ttu-id="08c60-120">Je výchozím protokolem připojení pro virtuální počítače Linux hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="08c60-120">It is the default connection protocol for Linux VMs hosted in Azure.</span></span> <span data-ttu-id="08c60-121">I když SSH, samotné poskytuje šifrované připojení, pomocí hesla s připojeními SSH stále zranitelný virtuálního počítače vůči hrubou silou útoky, nebo hádání hesel.</span><span class="sxs-lookup"><span data-stu-id="08c60-121">Although SSH itself provides an encrypted connection, using passwords with SSH connections still leaves the VM vulnerable to brute-force attacks or guessing of passwords.</span></span> <span data-ttu-id="08c60-122">Bezpečnější a upřednostňované metoda připojení k virtuálnímu počítači pomocí protokolu SSH je pomocí těchto veřejné a soukromé klíče, také známé jako klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="08c60-122">A more secure and preferred method of connecting to a VM using SSH is by using these public and private keys, also known as SSH keys.</span></span>

<span data-ttu-id="08c60-123">Pokud nechcete používat klíče SSH, můžete přesto k přihlašujete vaše virtuální počítače s Linuxem pomocí hesla.</span><span class="sxs-lookup"><span data-stu-id="08c60-123">If you do not wish to use SSH keys, you can still log in to your Linux VMs using a password.</span></span> <span data-ttu-id="08c60-124">Pokud není virtuální počítač přístup k Internetu, může být dostatečná pomocí hesla.</span><span class="sxs-lookup"><span data-stu-id="08c60-124">If your VM is not exposed to the Internet, using passwords may be sufficient.</span></span> <span data-ttu-id="08c60-125">Však stále potřebujete spravovat hesla pro každý virtuální počítač s Linuxem a Udržovat zásady hesel v pořádku a postupy, jako je minimální délka hesla a pravidelně aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="08c60-125">However, you still need to manage your passwords for each Linux VM and maintain healthy password policies and practices, such as minimum password length and regularly updating them.</span></span> <span data-ttu-id="08c60-126">Použití klíče SSH snižuje složitost správy jednotlivých přihlašovacích údajů napříč více virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="08c60-126">The use of SSH keys reduces the complexity of managing individual credentials across multiple VMs.</span></span>

## <a name="windows-packages-and-ssh-clients"></a><span data-ttu-id="08c60-127">Balíčky pro systém Windows a klientů SSH</span><span class="sxs-lookup"><span data-stu-id="08c60-127">Windows packages and SSH clients</span></span>
<span data-ttu-id="08c60-128">Připojení k a spravovat virtuální počítače s Linuxem v Azure pomocí **klient SSH**.</span><span class="sxs-lookup"><span data-stu-id="08c60-128">You connect to and manage Linux VMs in Azure using an **SSH client**.</span></span> <span data-ttu-id="08c60-129">Počítače se systémem Windows obvykle nemají nainstalovaného klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="08c60-129">Windows computers do not typically have an SSH client installed.</span></span> <span data-ttu-id="08c60-130">Aktualizace Windows 10 Anniversary přidána Bash pro systém Windows a nejnovější aktualizace Windows 10 Creators poskytuje další aktualizace.</span><span class="sxs-lookup"><span data-stu-id="08c60-130">The Windows 10 Anniversary Update added Bash for Windows, and the latest Windows 10 Creators Update provides additional updates.</span></span> <span data-ttu-id="08c60-131">Tato subsystému Windows pro Linux umožňuje spouštět a přístup nástroje, jako je například klientem SSH nativně v rámci prostředí Bash.</span><span class="sxs-lookup"><span data-stu-id="08c60-131">This Windows Subsystem for Linux allows you to run and access utilities such as an SSH client natively within a Bash shell.</span></span> <span data-ttu-id="08c60-132">Můžete pak provedením jakéhokoliv z dokumentace Linux, například [jak vygenerovat páry klíčů SSH pro Linux](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="08c60-132">You can then follow any of the Linux docs, such as [How to generate SSH key pairs for Linux](mac-create-ssh-keys.md).</span></span> <span data-ttu-id="08c60-133">Bash pro Windows je stále ve vývoji a je považován za verzi beta.</span><span class="sxs-lookup"><span data-stu-id="08c60-133">Bash for Windows is still under development, and is considered a beta release.</span></span> <span data-ttu-id="08c60-134">Další informace o Bash pro systém Windows najdete v tématu [Bash na Ubuntu v systému Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="08c60-134">For more information about Bash for Windows, see [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span>

<span data-ttu-id="08c60-135">Pokud chcete použít něco jiného než Bash pro systém Windows, jsou běžné Windows SSH klientů, které můžete nainstalovat součástí následujících balíčků:</span><span class="sxs-lookup"><span data-stu-id="08c60-135">If you wish to use something other than Bash for Windows, common Windows SSH clients you can install are included in the following packages:</span></span>

* [<span data-ttu-id="08c60-136">Git pro Windows</span><span class="sxs-lookup"><span data-stu-id="08c60-136">Git For Windows</span></span>](https://git-for-windows.github.io/)
* [<span data-ttu-id="08c60-137">puTTY</span><span class="sxs-lookup"><span data-stu-id="08c60-137">puTTY</span></span>](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [<span data-ttu-id="08c60-138">MobaXterm</span><span class="sxs-lookup"><span data-stu-id="08c60-138">MobaXterm</span></span>](http://mobaxterm.mobatek.net/)
* [<span data-ttu-id="08c60-139">Emulaci</span><span class="sxs-lookup"><span data-stu-id="08c60-139">Cygwin</span></span>](https://cygwin.com/)


## <a name="which-key-files-do-you-need-to-create"></a><span data-ttu-id="08c60-140">Klíče souborů, které je třeba vytvořit?</span><span class="sxs-lookup"><span data-stu-id="08c60-140">Which key files do you need to create?</span></span>
<span data-ttu-id="08c60-141">Azure vyžaduje minimálně 2048bitové **ssh-rsa** formátu veřejné a soukromé klíče.</span><span class="sxs-lookup"><span data-stu-id="08c60-141">Azure requires at least 2048-bit, **ssh-rsa** formatted public and private keys.</span></span> <span data-ttu-id="08c60-142">Pokud spravujete prostředků Azure pomocí modelu nasazení Classic, musíte taky generovat PEM (`.pem` souboru).</span><span class="sxs-lookup"><span data-stu-id="08c60-142">If you are managing Azure resources using the Classic deployment model, you also need to generate a PEM (`.pem` file).</span></span>

<span data-ttu-id="08c60-143">Zde jsou scénáře nasazení a typy souborů, které můžete použít v každém:</span><span class="sxs-lookup"><span data-stu-id="08c60-143">Here are the deployment scenarios, and the types of files you use in each:</span></span>

1. <span data-ttu-id="08c60-144">**SSH-rsa** klíče jsou požadovány pro nasazení pomocí [portál Azure](https://portal.azure.com)a nasazení Resource Manager pomocí [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="08c60-144">**ssh-rsa** keys are required for any deployment using the [Azure portal](https://portal.azure.com), and Resource Manager deployments using the [Azure CLI](../../cli-install-nodejs.md).</span></span>
   * <span data-ttu-id="08c60-145">Tyto klíče jsou obvykle že potřebovat všechny většina lidí.</span><span class="sxs-lookup"><span data-stu-id="08c60-145">These keys are usually all most people need.</span></span>
2. <span data-ttu-id="08c60-146">A `.pem` soubor je vyžadován pro vytvoření virtuálních počítačů pomocí nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="08c60-146">A `.pem` file is required to create VMs using the Classic deployment.</span></span> <span data-ttu-id="08c60-147">Tyto klíče jsou podporovány v nasazení Classic při použití [portál Azure](https://portal.azure.com) nebo [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="08c60-147">These keys are supported in Classic deployments when using the [Azure portal](https://portal.azure.com) or [Azure CLI](../../cli-install-nodejs.md).</span></span>
   * <span data-ttu-id="08c60-148">Potřebujete vytvořit tyto další klíčů a certifikátů, pokud spravujete prostředky vytvořené pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="08c60-148">You only need to create these additional keys and certificates if you are managing resources created using the Classic deployment model.</span></span>

## <a name="install-git-for-windows"></a><span data-ttu-id="08c60-149">Instalace Gitu pro Windows</span><span class="sxs-lookup"><span data-stu-id="08c60-149">Install Git for Windows</span></span>
<span data-ttu-id="08c60-150">V předchozí části uveden více balíčků, které zahrnují `openssl` nástroje pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="08c60-150">The preceding section listed several packages that include the `openssl` tool for Windows.</span></span> <span data-ttu-id="08c60-151">Tento nástroj je potřeba k vytvoření veřejné a soukromé klíče.</span><span class="sxs-lookup"><span data-stu-id="08c60-151">This tool is needed to create public and private keys.</span></span> <span data-ttu-id="08c60-152">Následující příklady jsou upřesněny postupy instalace a použití **Git pro Windows**, i když můžete podle toho, která balíček dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="08c60-152">The following examples detail how to install and use **Git for Windows**, though you can choose whichever package you prefer.</span></span> <span data-ttu-id="08c60-153">**Git pro Windows** dává vám přístup k některé další open-source softwaru ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) nástrojů a pomůcek, které mohou být užitečné při práci s virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="08c60-153">**Git for Windows** gives you access to some additional open-source software ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) tools and utilities that may be useful as you work with Linux VMs.</span></span>

1. <span data-ttu-id="08c60-154">Stáhněte a nainstalujte **Git pro Windows** z následujícího umístění: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).</span><span class="sxs-lookup"><span data-stu-id="08c60-154">Download and install **Git for Windows** from the following location: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).</span></span>
2. <span data-ttu-id="08c60-155">Pokud potřebujete konkrétně je změnit, přijměte výchozí nastavení během procesu instalace.</span><span class="sxs-lookup"><span data-stu-id="08c60-155">Accept the default options during the install process unless you specifically need to change them.</span></span>
3. <span data-ttu-id="08c60-156">Spustit **Git Bash** z **nabídky Start** > **Git** > **Git Bash**.</span><span class="sxs-lookup"><span data-stu-id="08c60-156">Run **Git Bash** from the **Start Menu** > **Git** > **Git Bash**.</span></span> <span data-ttu-id="08c60-157">Konzole vypadá podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="08c60-157">The console looks similar to the following example:</span></span>

    ![Prostředí Git Bash pro Windows](./media/ssh-from-windows/git-bash-window.png)

## <a name="create-a-private-key"></a><span data-ttu-id="08c60-159">Vytvoření privátního klíče</span><span class="sxs-lookup"><span data-stu-id="08c60-159">Create a private key</span></span>
1. <span data-ttu-id="08c60-160">Ve vaší **Git Bash** okně použití `openssl.exe` vytvořit privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="08c60-160">In your **Git Bash** window, use `openssl.exe` to create a private key.</span></span> <span data-ttu-id="08c60-161">Následující příklad vytvoří klíč s názvem `myPrivateKey` a certifikát s názvem `myCert.pem`:</span><span class="sxs-lookup"><span data-stu-id="08c60-161">The following example creates a key named `myPrivateKey` and certificate named `myCert.pem`:</span></span>

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    <span data-ttu-id="08c60-162">Výstup bude vypadat podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="08c60-162">The output looks similar to the following example:</span></span>

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

   <span data-ttu-id="08c60-163">Pokud se zobrazí chybová zpráva: bash, zkuste otevřít nové **Git Bash** okno se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="08c60-163">If bash reports an error, try opening a new **Git Bash** window with elevated privileges.</span></span> <span data-ttu-id="08c60-164">Potom spusťte znovu `openssl` příkaz.</span><span class="sxs-lookup"><span data-stu-id="08c60-164">Then, rerun the `openssl` command.</span></span>

2. <span data-ttu-id="08c60-165">Odpovězte pokynů pro název země, umístění, název organizace, atd.</span><span class="sxs-lookup"><span data-stu-id="08c60-165">Answer the prompts for country name, location, organization name, etc.</span></span>
3. <span data-ttu-id="08c60-166">Váš nový privátní klíč a certifikát se vytvoří v aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="08c60-166">Your new private key and certificate are created in your current working directory.</span></span> <span data-ttu-id="08c60-167">Jako bezpečnostní opatření měli byste nastavit oprávnění na váš privátní klíč tak, aby pouze můžete k němu přístup:</span><span class="sxs-lookup"><span data-stu-id="08c60-167">As a security measure, you should set the permissions on your private key so that only you can access it:</span></span>

    ```bash
    chmod 0600 myPrivateKey.key
    ```

4. <span data-ttu-id="08c60-168">[Další části](#create-a-private-key-for-putty) podrobnosti, jak zobrazit a použití veřejný klíč a vytvoření privátního klíče specifický pro použití klienta PuTTY k SSH pro virtuální počítače s Linuxem pomocí PuTTYgen.</span><span class="sxs-lookup"><span data-stu-id="08c60-168">The [next section](#create-a-private-key-for-putty) details using PuTTYgen to both view and use the public key, and create a private key specific for using PuTTY to SSH to Linux VMs.</span></span> <span data-ttu-id="08c60-169">Následující příkaz vytvoří soubor veřejného klíče s názvem `myPublicKey.key` , můžete použít:</span><span class="sxs-lookup"><span data-stu-id="08c60-169">The following command generates a public key file named `myPublicKey.key` that you can use right away:</span></span>

    ```bash
    openssl.exe rsa -pubout -in myPrivateKey.key -out myPublicKey.key
    ```

5. <span data-ttu-id="08c60-170">Pokud potřebujete spravovat klasické prostředky, převést `myCert.pem` k `myCert.cer` (X509, kódování DER certifikát).</span><span class="sxs-lookup"><span data-stu-id="08c60-170">If you also need to manage Classic resources, convert the `myCert.pem` to `myCert.cer` (DER encoded X509 certificate).</span></span> <span data-ttu-id="08c60-171">Tento volitelný krok proveďte jenom v případě, že budete muset konkrétně spravovat starší klasické prostředky.</span><span class="sxs-lookup"><span data-stu-id="08c60-171">Perform this optional step only if you need to specifically manage older Classic resources.</span></span>

    <span data-ttu-id="08c60-172">Převeďte certifikát pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="08c60-172">Convert the certificate using the following command:</span></span>

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a><span data-ttu-id="08c60-173">Vytvoření privátní klíč pro PuTTY</span><span class="sxs-lookup"><span data-stu-id="08c60-173">Create a private key for PuTTY</span></span>
<span data-ttu-id="08c60-174">PuTTY je běžné SSH klient pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="08c60-174">PuTTY is a common SSH client for Windows.</span></span> <span data-ttu-id="08c60-175">Jste libovolného klienta SSH, který chcete používat.</span><span class="sxs-lookup"><span data-stu-id="08c60-175">You are free to use any SSH client that you wish.</span></span> <span data-ttu-id="08c60-176">Prostřednictvím PuTTY, musíte vytvořit další typ klíče - privátní klíč PuTTY (PPK).</span><span class="sxs-lookup"><span data-stu-id="08c60-176">To use PuTTY, you need to create an additional type of key - a PuTTY Private Key (PPK).</span></span> <span data-ttu-id="08c60-177">Pokud nechcete, aby prostřednictvím PuTTY, tuto část přeskočte.</span><span class="sxs-lookup"><span data-stu-id="08c60-177">If you do not wish to use PuTTY, skip this section.</span></span>

<span data-ttu-id="08c60-178">Následující příklad vytvoří tento další privátní klíč speciálně pro PuTTY používat:</span><span class="sxs-lookup"><span data-stu-id="08c60-178">The following example creates this additional private key specifically for PuTTY to use:</span></span>

1. <span data-ttu-id="08c60-179">Použití **Git Bash** převést svůj privátní klíč do privátní klíč RSA, který můžete porozumět PuTTYgen.</span><span class="sxs-lookup"><span data-stu-id="08c60-179">Use **Git Bash** to convert your private key into an RSA private key that PuTTYgen can understand.</span></span> <span data-ttu-id="08c60-180">Následující příklad vytvoří klíč s názvem `myPrivateKey_rsa` z existující klíč s názvem `myPrivateKey`:</span><span class="sxs-lookup"><span data-stu-id="08c60-180">The following example creates a key named `myPrivateKey_rsa` from the existing key named `myPrivateKey`:</span></span>

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    <span data-ttu-id="08c60-181">Jako bezpečnostní opatření měli byste nastavit oprávnění na váš privátní klíč tak, aby pouze můžete k němu přístup:</span><span class="sxs-lookup"><span data-stu-id="08c60-181">As a security measure, you should set the permissions on your private key so that only you can access it:</span></span>

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```
2. <span data-ttu-id="08c60-182">Stažení a spuštění PuTTYgen z následujícího umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="08c60-182">Download and run PuTTYgen from the following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
3. <span data-ttu-id="08c60-183">Klikněte na nabídku: **soubor** > **zatížení privátní klíč**</span><span class="sxs-lookup"><span data-stu-id="08c60-183">Click the menu: **File** > **Load Private Key**</span></span>
4. <span data-ttu-id="08c60-184">Najít váš privátní klíč (`myPrivateKey_rsa` v předchozím příkladu).</span><span class="sxs-lookup"><span data-stu-id="08c60-184">Locate your private key (`myPrivateKey_rsa` in the previous example).</span></span> <span data-ttu-id="08c60-185">Výchozí adresář při spuštění **Git Bash** je `C:\Users\%username%`.</span><span class="sxs-lookup"><span data-stu-id="08c60-185">The default directory when you start **Git Bash** is `C:\Users\%username%`.</span></span> <span data-ttu-id="08c60-186">Změnit filtr souborů zobrazíte **všechny soubory (\*.\*)** :</span><span class="sxs-lookup"><span data-stu-id="08c60-186">Change the file filter to show **All Files (\*.\*)**:</span></span>

    ![Načtení existující soukromý klíč do PuTTYgen](./media/ssh-from-windows/load-private-key.png)
5. <span data-ttu-id="08c60-188">Klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="08c60-188">Click **Open**.</span></span> <span data-ttu-id="08c60-189">Příkazovém řádku uvedena, že klíč musí být úspěšně naimportována:</span><span class="sxs-lookup"><span data-stu-id="08c60-189">A prompt indicates that the key has been successfully imported:</span></span>

    ![Byly úspěšně importovány. klíč PuTTYgen](./media/ssh-from-windows/successfully-imported-key.png)
6. <span data-ttu-id="08c60-191">Klikněte na tlačítko **OK** k zavřete okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="08c60-191">Click **OK** to close the prompt.</span></span>
7. <span data-ttu-id="08c60-192">Veřejný klíč se zobrazí v horní části **PuTTYgen** okno.</span><span class="sxs-lookup"><span data-stu-id="08c60-192">The public key is displayed at the top of the **PuTTYgen** window.</span></span> <span data-ttu-id="08c60-193">Zkopírujte a vložte tento veřejný klíč do portálu Azure nebo do šablony Azure Resource Manager, když vytvoříte virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="08c60-193">You copy and paste this public key into the Azure portal or Azure Resource Manager template when you create a Linux VM.</span></span> <span data-ttu-id="08c60-194">Můžete také kliknout na **uložit veřejný klíč** pro uložení kopie do počítače:</span><span class="sxs-lookup"><span data-stu-id="08c60-194">You can also click **Save public key** to save a copy to your computer:</span></span>

    ![Uložte soubor PuTTY veřejného klíče](./media/ssh-from-windows/save-public-key.png)

    <span data-ttu-id="08c60-196">Následující příklad ukazuje, jak by zkopírujte a vložte tento veřejný klíč do portálu Azure při vytváření virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="08c60-196">The following example shows how you would copy and paste this public key into the Azure portal when you create a Linux VM.</span></span> <span data-ttu-id="08c60-197">Veřejný klíč je obvykle pak uloženy v `~/.ssh/authorized_keys` na nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="08c60-197">The public key is typically then stored in `~/.ssh/authorized_keys` on your new VM.</span></span>

    ![Veřejný klíč použít při vytváření virtuálního počítače na portálu Azure](./media/ssh-from-windows/use-public-key-azure-portal.png)
8. <span data-ttu-id="08c60-199">Zpět v **PuTTYgen**, klikněte na tlačítko **uložit privátní klíč**:</span><span class="sxs-lookup"><span data-stu-id="08c60-199">Back in **PuTTYgen**, Click **Save private Key**:</span></span>

    ![Uložte soubor privátního klíče PuTTY](./media/ssh-from-windows/save-ppk-file.png)

   > [!WARNING]
   > <span data-ttu-id="08c60-201">Zobrazí výzva s dotazem Pokud chcete pokračovat bez zadávání přístupové heslo klíče.</span><span class="sxs-lookup"><span data-stu-id="08c60-201">A prompt asks if you wish to continue without entering a passphrase for your key.</span></span> <span data-ttu-id="08c60-202">Přístupové heslo je stejné jako heslo připojené k privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="08c60-202">A passphrase is like a password attached to your private key.</span></span> <span data-ttu-id="08c60-203">I když někdo chtěli získat privátní klíč, se stále nepůjdou ověřit pomocí právě klíče.</span><span class="sxs-lookup"><span data-stu-id="08c60-203">Even if someone were to obtain your private key, they still would not be able to authenticate using just the key.</span></span> <span data-ttu-id="08c60-204">Potřebují by také heslo.</span><span class="sxs-lookup"><span data-stu-id="08c60-204">They would also need the passphrase.</span></span> <span data-ttu-id="08c60-205">Bez přístupové heslo Pokud někdo získá váš privátní klíč, můžete přihlášení k žádné virtuální počítač nebo služba, která používá tento klíč.</span><span class="sxs-lookup"><span data-stu-id="08c60-205">Without a passphrase, if someone obtains your private key, they can log in to any VM or service that uses that key.</span></span> <span data-ttu-id="08c60-206">Doporučujeme že vytvořit přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="08c60-206">We recommend you create a passphrase.</span></span> <span data-ttu-id="08c60-207">Pokud však heslo zapomenete, neexistuje žádný způsob, jak jej obnovit.</span><span class="sxs-lookup"><span data-stu-id="08c60-207">However, if you forget the passphrase, there is no way to recover it.</span></span>
   >
   >

    <span data-ttu-id="08c60-208">Pokud chcete zadat přístupové heslo, klikněte na tlačítko **ne**, zadejte přístupové heslo v hlavním okně PuTTYgen a pak klikněte na tlačítko **uložit privátní klíč** znovu.</span><span class="sxs-lookup"><span data-stu-id="08c60-208">If you wish to enter a passphrase, click **No**, enter a passphrase in the main PuTTYgen window, and then click **Save private key** again.</span></span> <span data-ttu-id="08c60-209">Jinak, klikněte na tlačítko **Ano** pokračujte bez zadání volitelné přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="08c60-209">Otherwise, click **Yes** to continue without providing the optional passphrase.</span></span>
9. <span data-ttu-id="08c60-210">Zadejte název a umístění pro uložení souboru PPK.</span><span class="sxs-lookup"><span data-stu-id="08c60-210">Enter a name and location to save your PPK file.</span></span>

## <a name="use-putty-to-ssh-to-a-linux-machine"></a><span data-ttu-id="08c60-211">Použití klienta Putty k SSH na počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="08c60-211">Use Putty to SSH to a Linux Machine</span></span>
<span data-ttu-id="08c60-212">PuTTY znovu, je běžné SSH klient pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="08c60-212">Again, PuTTY is a common SSH client for Windows.</span></span> <span data-ttu-id="08c60-213">Jste libovolného klienta SSH, který chcete používat.</span><span class="sxs-lookup"><span data-stu-id="08c60-213">You are free to use any SSH client that you wish.</span></span> <span data-ttu-id="08c60-214">Následující kroky podrobnosti, jak používat privátní klíč k ověření pomocí svého virtuálního počítače Azure pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="08c60-214">The following steps detail how to use your private key to authenticate with your Azure VM using SSH.</span></span> <span data-ttu-id="08c60-215">Kroky se podobají v jiných SSH klíče klientů z hlediska museli načíst váš privátní klíč k ověření připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="08c60-215">The steps are similar in other SSH key clients in terms of needing to load your private key to authenticate the SSH connection.</span></span>

1. <span data-ttu-id="08c60-216">Stažení a spuštění putty z následujícího umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="08c60-216">Download and run putty from the following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
2. <span data-ttu-id="08c60-217">Zadejte název hostitele nebo IP adresa vašeho virtuálního počítače z portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="08c60-217">Fill in the host name or IP address of your VM from the Azure portal:</span></span>

    ![Otevřít nové připojení PuTTY](./media/ssh-from-windows/putty-new-connection.png)
3. <span data-ttu-id="08c60-219">Před výběrem **otevřete**, klikněte na tlačítko **připojení** > **SSH** > **Auth** kartě.</span><span class="sxs-lookup"><span data-stu-id="08c60-219">Before selecting **Open**, click **Connection** > **SSH** > **Auth** tab.</span></span> <span data-ttu-id="08c60-220">Vyhledejte a vyberte privátní klíč:</span><span class="sxs-lookup"><span data-stu-id="08c60-220">Browse to and select your private key:</span></span>

    ![Vyberte PuTTY privátní klíč pro ověřování](./media/ssh-from-windows/putty-auth-dialog.png)
4. <span data-ttu-id="08c60-222">Klikněte na tlačítko **otevřete** pro připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="08c60-222">Click **Open** to connect to your virtual machine</span></span>

## <a name="next-steps"></a><span data-ttu-id="08c60-223">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08c60-223">Next steps</span></span>
<span data-ttu-id="08c60-224">Můžete také vygenerovat veřejné a soukromé klíče [pomocí OS X a Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="08c60-224">You can also generate the public and private keys [using OS X and Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="08c60-225">Další informace o Bash pro systém Windows a o výhodách operačních systémů nástroje snadno dostupné na počítači s Windows najdete v tématu [Bash na Ubuntu v systému Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="08c60-225">For more information about Bash for Windows and the benefits of having OSS tools readily available on your Windows computer, see [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span>

<span data-ttu-id="08c60-226">Pokud máte potíže při používání SSH připojit k virtuální počítače Linux, najdete v článku [řešení SSH připojení k virtuální počítač Azure Linux](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="08c60-226">If you have trouble using SSH to connect to your Linux VMs, see [Troubleshoot SSH connections to an Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
