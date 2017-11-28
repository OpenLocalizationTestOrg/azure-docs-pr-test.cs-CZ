---
title: "Řešení potíží s připojeními SSH pro virtuální počítač Azure | Microsoft Docs"
description: "Postup řešení potíží, třeba \"Připojení SSH se nezdařilo\" nebo \"odmítl připojení SSH' pro virtuální počítač Azure s Linuxem."
keywords: "SSH připojení odmítnuto, ssh chyby, azure ssh, připojení SSH se nezdařilo"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: dcb82e19-29b2-47bb-99f2-900d4cfb5bbb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: 3a282c8b2c2ba2749de6a2d3688bd57d75703b22
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a><span data-ttu-id="273d3-104">Řešení potíží s připojení SSH pro virtuální počítač Azure Linux který selže, chyby, nebo bylo odmítnuto</span><span class="sxs-lookup"><span data-stu-id="273d3-104">Troubleshoot SSH connections to an Azure Linux VM that fails, errors out, or is refused</span></span>
<span data-ttu-id="273d3-105">Existují různé příčiny, že dojde k chybám Secure Shell (SSH), selhání připojení SSH, nebo SSH bylo odmítnuto, při pokusu o připojení k virtuálnímu počítači (VM) Linux.</span><span class="sxs-lookup"><span data-stu-id="273d3-105">There are various reasons that you encounter Secure Shell (SSH) errors, SSH connection failures, or SSH is refused when you try to connect to a Linux virtual machine (VM).</span></span> <span data-ttu-id="273d3-106">Tento článek pomůže najít a opravit problémy.</span><span class="sxs-lookup"><span data-stu-id="273d3-106">This article helps you find and correct the problems.</span></span> <span data-ttu-id="273d3-107">Portál Azure, rozhraní příkazového řádku Azure nebo rozšíření pro přístup virtuálních počítačů pro Linux můžete použít k řešení problémů s připojením.</span><span class="sxs-lookup"><span data-stu-id="273d3-107">You can use the Azure portal, Azure CLI, or VM Access Extension for Linux to troubleshoot and resolve connection problems.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="273d3-108">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](http://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="273d3-108">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and Stack Overflow forums](http://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="273d3-109">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="273d3-109">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="273d3-110">Přejděte na [podporu Azure lokality](http://azure.microsoft.com/support/options/) a vyberte **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="273d3-110">Go to the [Azure support site](http://azure.microsoft.com/support/options/) and select **Get support**.</span></span> <span data-ttu-id="273d3-111">Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](http://azure.microsoft.com/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="273d3-111">For information about using Azure Support, read the [Microsoft Azure support FAQ](http://azure.microsoft.com/support/faq/).</span></span>

## <a name="quick-troubleshooting-steps"></a><span data-ttu-id="273d3-112">Rychlé řešení potíží</span><span class="sxs-lookup"><span data-stu-id="273d3-112">Quick troubleshooting steps</span></span>
<span data-ttu-id="273d3-113">Po dokončení každého kroku řešení potíží pokuste o připojení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="273d3-113">After each troubleshooting step, try reconnecting to the VM.</span></span>

1. <span data-ttu-id="273d3-114">Obnovte konfiguraci SSH.</span><span class="sxs-lookup"><span data-stu-id="273d3-114">Reset the SSH configuration.</span></span>
2. <span data-ttu-id="273d3-115">Resetovat přihlašovací údaje pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="273d3-115">Reset the credentials for the user.</span></span>
3. <span data-ttu-id="273d3-116">Ověřte [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) pravidla povolit provoz protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="273d3-116">Verify the [Network Security Group](../../virtual-network/virtual-networks-nsg.md) rules permit SSH traffic.</span></span>
   * <span data-ttu-id="273d3-117">Zkontrolujte, zda pravidlo skupinu zabezpečení sítě pro povolení komunikace SSH (ve výchozím nastavení je to TCP port 22).</span><span class="sxs-lookup"><span data-stu-id="273d3-117">Ensure that a Network Security Group rule exists to permit SSH traffic (by default, TCP port 22).</span></span>
   * <span data-ttu-id="273d3-118">Nemůžete použít přesměrování portu / mapování bez použití pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="273d3-118">You cannot use port redirection / mapping without using an Azure load balancer.</span></span>
4. <span data-ttu-id="273d3-119">Zkontrolujte [stavu prostředků virtuálních počítačů](../../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="273d3-119">Check the [VM resource health](../../resource-health/resource-health-overview.md).</span></span> 
   * <span data-ttu-id="273d3-120">Ujistěte se, že virtuální počítač hlásí, že je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="273d3-120">Ensure that the VM reports as being healthy.</span></span>
   * <span data-ttu-id="273d3-121">Pokud máte povolené Diagnostika spouštění, ověřte, zda že virtuální počítač není reporting spouštěcí chyby v protokolech.</span><span class="sxs-lookup"><span data-stu-id="273d3-121">If you have boot diagnostics enabled, verify the VM is not reporting boot errors in the logs.</span></span>
5. <span data-ttu-id="273d3-122">Restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="273d3-122">Restart the VM.</span></span>
6. <span data-ttu-id="273d3-123">Znovu nasaďte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="273d3-123">Redeploy the VM.</span></span>

<span data-ttu-id="273d3-124">Pokračujte ve čtení pro podrobnější pro řešení potíží a vysvětlení.</span><span class="sxs-lookup"><span data-stu-id="273d3-124">Continue reading for more detailed troubleshooting steps and explanations.</span></span>

## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a><span data-ttu-id="273d3-125">Dostupné metody k řešení potíží s připojeními SSH</span><span class="sxs-lookup"><span data-stu-id="273d3-125">Available methods to troubleshoot SSH connection issues</span></span>
<span data-ttu-id="273d3-126">Můžete obnovit přihlašovací údaje nebo konfiguraci SSH pomocí jedné z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="273d3-126">You can reset credentials or SSH configuration using one of the following methods:</span></span>

* <span data-ttu-id="273d3-127">[Portál Azure](#use-the-azure-portal) – skvělé, pokud budete potřebovat rychle resetovat konfiguraci SSH nebo klíč SSH a nemáte Azure nástroje nainstalované.</span><span class="sxs-lookup"><span data-stu-id="273d3-127">[Azure portal](#use-the-azure-portal) - great if you need to quickly reset the SSH configuration or SSH key and you don't have the Azure tools installed.</span></span>
* <span data-ttu-id="273d3-128">[Azure CLI 2.0](#use-the-azure-cli-20) – Pokud jste už na příkazovém řádku, rychle obnovit konfiguraci SSH nebo přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="273d3-128">[Azure CLI 2.0](#use-the-azure-cli-20) - if you are already on the command line, quickly reset the SSH configuration or credentials.</span></span> <span data-ttu-id="273d3-129">Můžete také [1.0 rozhraní příkazového řádku Azure](#use-the-azure-cli-10)</span><span class="sxs-lookup"><span data-stu-id="273d3-129">You can also use the [Azure CLI 1.0](#use-the-azure-cli-10)</span></span>
* <span data-ttu-id="273d3-130">[Azure rozšíření VMAccessForLinux](#use-the-vmaccess-extension) – vytvoření a opakovaně soubory json definice se resetovat přihlašovací údaje pro konfiguraci nebo uživatele SSH.</span><span class="sxs-lookup"><span data-stu-id="273d3-130">[Azure VMAccessForLinux extension](#use-the-vmaccess-extension) - create and reuse json definition files to reset the SSH configuration or user credentials.</span></span>

<span data-ttu-id="273d3-131">Po dokončení každého kroku řešení potíží zkuste znovu připojit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="273d3-131">After each troubleshooting step, try connecting to your VM again.</span></span> <span data-ttu-id="273d3-132">Pokud se pořád nemůžete připojit, zkuste na další krok.</span><span class="sxs-lookup"><span data-stu-id="273d3-132">If you still cannot connect, try the next step.</span></span>

## <a name="use-the-azure-portal"></a><span data-ttu-id="273d3-133">Použití webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="273d3-133">Use the Azure portal</span></span>
<span data-ttu-id="273d3-134">Portál Azure poskytuje rychlý způsob, jak resetovat přihlašovací údaje pro konfiguraci nebo uživatele SSH bez instalace žádné nástroje na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="273d3-134">The Azure portal provides a quick way to reset the SSH configuration or user credentials without installing any tools on your local computer.</span></span>

<span data-ttu-id="273d3-135">Vyberte virtuální počítač na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="273d3-135">Select your VM in the Azure portal.</span></span> <span data-ttu-id="273d3-136">Přejděte dolů k položce **podporu + Poradce při potížích s** a vyberte **resetovat heslo** jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="273d3-136">Scroll down to the **Support + Troubleshooting** section and select **Reset password** as in the following example:</span></span>

![Resetování konfigurace SSH nebo přihlašovací údaje na portálu Azure](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a><span data-ttu-id="273d3-138">Resetování konfigurace SSH</span><span class="sxs-lookup"><span data-stu-id="273d3-138">Reset the SSH configuration</span></span>
<span data-ttu-id="273d3-139">Jako první krok, vyberte `Reset configuration only` z **režimu** rozevírací nabídky jako v předchozím snímku obrazovky klikněte **resetovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="273d3-139">As a first step, select `Reset configuration only` from the **Mode** drop-down menu as in the preceding screenshot, then click the **Reset** button.</span></span> <span data-ttu-id="273d3-140">Po dokončení této akce pokusu o přístup k virtuálnímu počítači znovu.</span><span class="sxs-lookup"><span data-stu-id="273d3-140">Once this action has completed, try to access your VM again.</span></span>

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="273d3-141">Resetovat SSH přihlašovací údaje pro uživatele</span><span class="sxs-lookup"><span data-stu-id="273d3-141">Reset SSH credentials for a user</span></span>
<span data-ttu-id="273d3-142">Se resetovat přihlašovací údaje stávajícího uživatele, vyberte buď `Reset SSH public key` nebo `Reset password` z **režimu** rozevírací nabídky jako v předchozím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="273d3-142">To reset the credentials of an existing user, select either `Reset SSH public key` or `Reset password` from the **Mode** drop-down menu as in the preceding screenshot.</span></span> <span data-ttu-id="273d3-143">Zadejte uživatelské jméno a klíč SSH nebo nové heslo a pak klikněte na **resetovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="273d3-143">Specify the username and an SSH key or new password, then click the **Reset** button.</span></span>

<span data-ttu-id="273d3-144">Můžete také vytvořit uživatele s oprávněními sudo do virtuálního počítače z této nabídky.</span><span class="sxs-lookup"><span data-stu-id="273d3-144">You can also create a user with sudo privileges on the VM from this menu.</span></span> <span data-ttu-id="273d3-145">Zadejte nové uživatelské jméno a přiřazené heslo nebo klíč SSH a pak klikněte **resetovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="273d3-145">Enter a new username and associated password or SSH key, and then click the **Reset** button.</span></span>

## <a name="use-the-azure-cli-20"></a><span data-ttu-id="273d3-146">Použití Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="273d3-146">Use the Azure CLI 2.0</span></span>
<span data-ttu-id="273d3-147">Pokud jste to ještě neudělali, nainstalujte nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se k Azure účet pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="273d3-147">If you haven't already, install the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="273d3-148">Pokud jste vytvořili a nahrát vlastní image disku Linux, zkontrolujte [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verze 2.0.5 nebo novější je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="273d3-148">If you created and uploaded a custom Linux disk image, make sure the [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 or later is installed.</span></span> <span data-ttu-id="273d3-149">Pro virtuální počítače vytvořené pomocí Galerie obrázků toto rozšíření přístup k již instalovaných a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="273d3-149">For VMs created using Gallery images, this access extension is already installed and configured for you.</span></span>

### <a name="reset-ssh-configuration"></a><span data-ttu-id="273d3-150">Obnovte konfiguraci SSH</span><span class="sxs-lookup"><span data-stu-id="273d3-150">Reset SSH configuration</span></span>
<span data-ttu-id="273d3-151">Můžete Nejdřív zkuste resetuje se konfigurace SSH výchozí hodnoty a restartování serveru SSH ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="273d3-151">You can initially try resetting the SSH configuration to default values and rebooting the SSH server on the VM.</span></span> <span data-ttu-id="273d3-152">Všimněte si, že to nezmění název uživatelského účtu, hesla nebo klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="273d3-152">Note that this does not change the user account name, password, or SSH keys.</span></span>
<span data-ttu-id="273d3-153">Následující příklad používá [az virtuálního počítače uživatele resetování-ssh](/cli/azure/vm/user#reset-ssh) resetování konfigurace SSH pro virtuální počítač s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-153">The following example uses [az vm user reset-ssh](/cli/azure/vm/user#reset-ssh) to reset the SSH configuration on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="273d3-154">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-154">Use your own values as follows:</span></span>

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="273d3-155">Resetovat SSH přihlašovací údaje pro uživatele</span><span class="sxs-lookup"><span data-stu-id="273d3-155">Reset SSH credentials for a user</span></span>
<span data-ttu-id="273d3-156">Následující příklad používá [aktualizace uživatele virtuálního počítače az](/cli/azure/vm/user#update) se resetovat přihlašovací údaje pro `myUsername` na hodnotu zadanou v `myPassword`, na virtuální počítač s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-156">The following example uses [az vm user update](/cli/azure/vm/user#update) to reset the credentials for `myUsername` to the value specified in `myPassword`, on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="273d3-157">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-157">Use your own values as follows:</span></span>

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

<span data-ttu-id="273d3-158">Pokud používáte ověření pomocí klíče SSH, můžete resetovat klíč SSH pro daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="273d3-158">If using SSH key authentication, you can reset the SSH key for a given user.</span></span> <span data-ttu-id="273d3-159">Následující příklad používá **az virtuální počítač přístup k set-linux-user** aktualizovat klíč SSH, které jsou uložené v `~/.ssh/id_rsa.pub` pro uživatele s názvem `myUsername`, na virtuální počítač s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-159">The following example uses **az vm access set-linux-user** to update the SSH key stored in `~/.ssh/id_rsa.pub` for the user named `myUsername`, on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="273d3-160">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-160">Use your own values as follows:</span></span>

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-the-vmaccess-extension"></a><span data-ttu-id="273d3-161">Používá rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="273d3-161">Use the VMAccess extension</span></span>
<span data-ttu-id="273d3-162">Rozšíření pro přístup virtuálních počítačů pro Linux přečte v souboru json, který definuje akce k provedení. Mezi ně patří resetování SSHD, resetování klíče SSH nebo přidání uživatele.</span><span class="sxs-lookup"><span data-stu-id="273d3-162">The VM Access Extension for Linux reads in a json file that defines actions to carry out. These actions include resetting SSHD, resetting an SSH key, or adding a user.</span></span> <span data-ttu-id="273d3-163">Dál používat rozhraní příkazového řádku Azure k volání rozšíření VMAccess, ale můžete opakovaně použít soubory json napříč více virtuálními počítači v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="273d3-163">You still use the Azure CLI to call the VMAccess extension, but you can reuse the json files across multiple VMs if desired.</span></span> <span data-ttu-id="273d3-164">Tento přístup umožňuje vytvořit úložiště json souborů, které může být volána pro daný scénáře.</span><span class="sxs-lookup"><span data-stu-id="273d3-164">This approach allows you to create a repository of json files that can then be called for given scenarios.</span></span>

### <a name="reset-sshd"></a><span data-ttu-id="273d3-165">Resetování SSHD</span><span class="sxs-lookup"><span data-stu-id="273d3-165">Reset SSHD</span></span>
<span data-ttu-id="273d3-166">Vytvořte soubor s názvem `settings.json` s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="273d3-166">Create a file named `settings.json` with the following content:</span></span>

```json
{  
    "reset_ssh":"True"
}
```

<span data-ttu-id="273d3-167">Použití Azure CLI, potom zavolejte `VMAccessForLinux` rozšíření pro resetování připojení SSHD zadáním souboru json.</span><span class="sxs-lookup"><span data-stu-id="273d3-167">Using the Azure CLI, you then call the `VMAccessForLinux` extension to reset your SSHD connection by specifying your json file.</span></span> <span data-ttu-id="273d3-168">Následující příklad používá [nastavení rozšíření virtuálního az](/cli/azure/vm/extension#set) resetovat SSHD ve virtuálním počítači s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-168">The following example uses [az vm extension set](/cli/azure/vm/extension#set) to reset SSHD on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="273d3-169">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-169">Use your own values as follows:</span></span>

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="273d3-170">Resetovat SSH přihlašovací údaje pro uživatele</span><span class="sxs-lookup"><span data-stu-id="273d3-170">Reset SSH credentials for a user</span></span>
<span data-ttu-id="273d3-171">Pokud se zobrazí SSHD fungoval správně, můžete resetovat přihlašovací údaje pro uživatele třetím osobám.</span><span class="sxs-lookup"><span data-stu-id="273d3-171">If SSHD appears to function correctly, you can reset the credentials for a giver user.</span></span> <span data-ttu-id="273d3-172">Resetování hesla pro uživatele, vytvořte soubor s názvem `settings.json`.</span><span class="sxs-lookup"><span data-stu-id="273d3-172">To reset the password for a user, create a file named `settings.json`.</span></span> <span data-ttu-id="273d3-173">Následující příklad resetuje přihlašovací údaje pro `myUsername` na hodnotu zadanou v `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="273d3-173">The following example resets the credentials for `myUsername` to the value specified in `myPassword`.</span></span> <span data-ttu-id="273d3-174">Zadejte následující řádky do vaší `settings.json` souboru pomocí vlastní hodnoty:</span><span class="sxs-lookup"><span data-stu-id="273d3-174">Enter the following lines into your `settings.json` file, using your own values:</span></span>

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

<span data-ttu-id="273d3-175">Nebo si Pokud chcete resetovat klíč SSH pro uživatele, nejprve vytvořte soubor s názvem `settings.json`.</span><span class="sxs-lookup"><span data-stu-id="273d3-175">Or to reset the SSH key for a user, first create a file named `settings.json`.</span></span> <span data-ttu-id="273d3-176">Následující příklad resetuje přihlašovací údaje pro `myUsername` na hodnotu zadanou v `myPassword`, na virtuální počítač s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-176">The following example resets the credentials for `myUsername` to the value specified in `myPassword`, on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="273d3-177">Zadejte následující řádky do vaší `settings.json` souboru pomocí vlastní hodnoty:</span><span class="sxs-lookup"><span data-stu-id="273d3-177">Enter the following lines into your `settings.json` file, using your own values:</span></span>

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

<span data-ttu-id="273d3-178">Po vytvoření souboru json, pomocí rozhraní příkazového řádku Azure k volání `VMAccessForLinux` rozšíření k resetování přihlašovacích údajů uživatele SSH zadáním souboru json.</span><span class="sxs-lookup"><span data-stu-id="273d3-178">After creating your json file, use the Azure CLI to call the `VMAccessForLinux` extension to reset your SSH user credentials by specifying your json file.</span></span> <span data-ttu-id="273d3-179">Následující příklad resetuje přihlašovací údaje u virtuálního počítače s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-179">The following example resets credentials on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="273d3-180">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-180">Use your own values as follows:</span></span>

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-the-azure-cli-10"></a><span data-ttu-id="273d3-181">Použití Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="273d3-181">Use the Azure CLI 1.0</span></span>
<span data-ttu-id="273d3-182">Pokud jste to ještě neudělali, [nainstalovat Azure CLI 1.0 a připojit se k předplatnému Azure](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="273d3-182">If you haven't already, [install the Azure CLI 1.0 and connect to your Azure subscription](../../cli-install-nodejs.md).</span></span> <span data-ttu-id="273d3-183">Ujistěte se, že režimu Resource Manager používáte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="273d3-183">Make sure that you are using Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="273d3-184">Pokud jste vytvořili a nahrát vlastní image disku Linux, zkontrolujte [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verze 2.0.5 nebo novější je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="273d3-184">If you created and uploaded a custom Linux disk image, make sure the [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 or later is installed.</span></span> <span data-ttu-id="273d3-185">Pro virtuální počítače vytvořené pomocí Galerie obrázků toto rozšíření přístup k již instalovaných a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="273d3-185">For VMs created using Gallery images, this access extension is already installed and configured for you.</span></span>

### <a name="reset-ssh-configuration"></a><span data-ttu-id="273d3-186">Obnovte konfiguraci SSH</span><span class="sxs-lookup"><span data-stu-id="273d3-186">Reset SSH configuration</span></span>
<span data-ttu-id="273d3-187">Může být nesprávné konfigurace SSHD sám sebe nebo ve službě došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="273d3-187">The SSHD configuration itself may be misconfigured or the service encountered an error.</span></span> <span data-ttu-id="273d3-188">Můžete resetovat SSHD a ujistěte se, že samotný konfiguraci SSH je platný.</span><span class="sxs-lookup"><span data-stu-id="273d3-188">You can reset SSHD to make sure the SSH configuration itself is valid.</span></span> <span data-ttu-id="273d3-189">Resetování SSHD musí být prvním krokem řešení potíží, které můžete provést.</span><span class="sxs-lookup"><span data-stu-id="273d3-189">Resetting SSHD should be the first troubleshooting step you take.</span></span>

<span data-ttu-id="273d3-190">Následující příklad resetuje SSHD na virtuálním počítači s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-190">The following example resets SSHD on a VM named `myVM` in the resource group named `myResourceGroup`.</span></span> <span data-ttu-id="273d3-191">Používejte vlastní názvy virtuálních počítačů a prostředků skupiny takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-191">Use your own VM and resource group names as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="273d3-192">Resetovat SSH přihlašovací údaje pro uživatele</span><span class="sxs-lookup"><span data-stu-id="273d3-192">Reset SSH credentials for a user</span></span>
<span data-ttu-id="273d3-193">Pokud se zobrazí SSHD fungoval správně, můžete resetovat heslo pro uživatele třetím osobám.</span><span class="sxs-lookup"><span data-stu-id="273d3-193">If SSHD appears to function correctly, you can reset the password for a giver user.</span></span> <span data-ttu-id="273d3-194">Následující příklad resetuje přihlašovací údaje pro `myUsername` na hodnotu zadanou v `myPassword`, na virtuální počítač s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-194">The following example resets the credentials for `myUsername` to the value specified in `myPassword`, on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="273d3-195">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-195">Use your own values as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

<span data-ttu-id="273d3-196">Pokud používáte ověření pomocí klíče SSH, můžete resetovat klíč SSH pro daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="273d3-196">If using SSH key authentication, you can reset the SSH key for a given user.</span></span> <span data-ttu-id="273d3-197">Následující příklad aktualizuje klíč SSH, které jsou uložené v `~/.ssh/id_rsa.pub` pro uživatele s názvem `myUsername`, na virtuální počítač s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-197">The following example updates the SSH key stored in `~/.ssh/id_rsa.pub` for the user named `myUsername`, on the VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="273d3-198">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-198">Use your own values as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a><span data-ttu-id="273d3-199">Restartování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="273d3-199">Restart a VM</span></span>
<span data-ttu-id="273d3-200">Pokud máte resetování konfigurace a uživatelská pověření SSH, nebo došlo k chybě při tom, můžete zkusit restartování virtuálního počítače na adresu základní výpočetní problémy.</span><span class="sxs-lookup"><span data-stu-id="273d3-200">If you have reset the SSH configuration and user credentials, or encountered an error in doing so, you can try restarting the VM to address underlying compute issues.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="273d3-201">portál Azure</span><span class="sxs-lookup"><span data-stu-id="273d3-201">Azure portal</span></span>
<span data-ttu-id="273d3-202">Restartování virtuálního počítače pomocí portálu Azure, vyberte virtuální počítač a klikněte na **restartujte** tlačítko jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="273d3-202">To restart a VM using the Azure portal, select your VM and click the **Restart** button as in the following example:</span></span>

![Restartujte virtuální počítač na portálu Azure](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli-10"></a><span data-ttu-id="273d3-204">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="273d3-204">Azure CLI 1.0</span></span>
<span data-ttu-id="273d3-205">Následující příklad restartuje virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-205">The following example restarts the VM named `myVM` in the resource group named `myResourceGroup`.</span></span> <span data-ttu-id="273d3-206">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-206">Use your own values as follows:</span></span>

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a><span data-ttu-id="273d3-207">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="273d3-207">Azure CLI 2.0</span></span>
<span data-ttu-id="273d3-208">Následující příklad používá [restartování virtuálního počítače az](/cli/azure/vm#restart) restartovat virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-208">The following example uses [az vm restart](/cli/azure/vm#restart) to restart the VM named `myVM` in the resource group named `myResourceGroup`.</span></span> <span data-ttu-id="273d3-209">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-209">Use your own values as follows:</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a><span data-ttu-id="273d3-210">Znovunasazení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="273d3-210">Redeploy a VM</span></span>
<span data-ttu-id="273d3-211">Můžete znovu nasadit virtuální počítač do jiného uzlu v rámci Azure, která může vyřešit problémy s základní sítě.</span><span class="sxs-lookup"><span data-stu-id="273d3-211">You can redeploy a VM to another node within Azure, which may correct any underlying networking issues.</span></span> <span data-ttu-id="273d3-212">Informace o opětovného nasazení virtuálního počítače najdete v tématu [znovu nasadit virtuální počítač do nového uzlu Azure](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="273d3-212">For information about redeploying a VM, see [Redeploy virtual machine to new Azure node](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="273d3-213">Po dokončení této operace dočasné disku data budou ztracena a zaktualizuje dynamické IP adresy, které jsou spojeny s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="273d3-213">After this operation finishes, ephemeral disk data will be lost and dynamic IP addresses that are associated with the virtual machine will be updated.</span></span>
> 
> 

### <a name="azure-portal"></a><span data-ttu-id="273d3-214">portál Azure</span><span class="sxs-lookup"><span data-stu-id="273d3-214">Azure portal</span></span>
<span data-ttu-id="273d3-215">K opětovnému nasazení virtuálního počítače pomocí portálu Azure, vyberte virtuální počítač a přejděte dolů k položce **podporu + Poradce při potížích s** části.</span><span class="sxs-lookup"><span data-stu-id="273d3-215">To redeploy a VM using the Azure portal, select your VM and scroll down to the **Support + Troubleshooting** section.</span></span> <span data-ttu-id="273d3-216">Klikněte **znovu nasaďte** tlačítko jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="273d3-216">Click the **Redeploy** button as in the following example:</span></span>

![Opětovné nasazení virtuálního počítače na portálu Azure](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli-10"></a><span data-ttu-id="273d3-218">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="273d3-218">Azure CLI 1.0</span></span>
<span data-ttu-id="273d3-219">Následující příklad opětovně nasadí virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-219">The following example redeploys the VM named `myVM` in the resource group named `myResourceGroup`.</span></span> <span data-ttu-id="273d3-220">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-220">Use your own values as follows:</span></span>

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a><span data-ttu-id="273d3-221">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="273d3-221">Azure CLI 2.0</span></span>
<span data-ttu-id="273d3-222">Následující příklad použití [az virtuálního počítače znovu ho zaveďte](/cli/azure/vm#redeploy) znovu nasadit virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="273d3-222">The following example use [az vm redeploy](/cli/azure/vm#redeploy) to redeploy the VM named `myVM` in the resource group named `myResourceGroup`.</span></span> <span data-ttu-id="273d3-223">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="273d3-223">Use your own values as follows:</span></span>

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a><span data-ttu-id="273d3-224">Virtuální počítače vytvořené pomocí modelu nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="273d3-224">VMs created by using the Classic deployment model</span></span>
<span data-ttu-id="273d3-225">Opakujte tyto kroky k vyřešení většiny běžných chyb připojení SSH pro virtuální počítače, které byly vytvořeny pomocí modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="273d3-225">Try these steps to resolve the most common SSH connection failures for VMs that were created by using the classic deployment model.</span></span> <span data-ttu-id="273d3-226">Po dokončení každého kroku pokuste o připojení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="273d3-226">After each step, try reconnecting to the VM.</span></span>

* <span data-ttu-id="273d3-227">Obnovte vzdálený přístup z [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="273d3-227">Reset remote access from the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="273d3-228">Na portálu Azure vyberte virtuální počítač a klikněte na **resetovat vzdálený...**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="273d3-228">On the Azure portal, select your VM and click the **Reset Remote...** button.</span></span>
* <span data-ttu-id="273d3-229">Restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="273d3-229">Restart the VM.</span></span> <span data-ttu-id="273d3-230">Na [portál Azure](https://portal.azure.com), vyberte virtuální počítač a klikněte **restartujte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="273d3-230">On the [Azure portal](https://portal.azure.com), select your VM and click the **Restart** button.</span></span>
    
* <span data-ttu-id="273d3-231">Opětovné nasazení virtuálního počítače do nového uzlu Azure.</span><span class="sxs-lookup"><span data-stu-id="273d3-231">Redeploy the VM to a new Azure node.</span></span> <span data-ttu-id="273d3-232">Informace o tom, jak znovu nasadit virtuální počítač najdete v tématu [znovu nasadit virtuální počítač do nového uzlu Azure](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="273d3-232">For information about how to redeploy a VM, see [Redeploy virtual machine to new Azure node](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
  
    <span data-ttu-id="273d3-233">Po dokončení této operace dočasné disku data budou ztracena a zaktualizuje dynamické IP adresy, které jsou spojeny s virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="273d3-233">After this operation finishes, ephemeral disk data will be lost and dynamic IP addresses that are associated with the virtual machine will be updated.</span></span>
* <span data-ttu-id="273d3-234">Postupujte podle pokynů v [jak resetovat heslo nebo SSH pro virtuální počítače se systémem Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) na:</span><span class="sxs-lookup"><span data-stu-id="273d3-234">Follow the instructions in [How to reset a password or SSH for Linux-based virtual machines](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) to:</span></span>
  
  * <span data-ttu-id="273d3-235">Resetovat heslo nebo klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="273d3-235">Reset the password or SSH key.</span></span>
  * <span data-ttu-id="273d3-236">Vytvoření *sudo* uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="273d3-236">Create a *sudo* user account.</span></span>
  * <span data-ttu-id="273d3-237">Obnovte konfiguraci SSH.</span><span class="sxs-lookup"><span data-stu-id="273d3-237">Reset the SSH configuration.</span></span>
* <span data-ttu-id="273d3-238">Zkontrolujte stav Virtuálního počítače prostředků pro nějaký problém s platformou.</span><span class="sxs-lookup"><span data-stu-id="273d3-238">Check the VM's resource health for any platform issues.</span></span><br>
     <span data-ttu-id="273d3-239">Vyberte virtuální počítač a přejděte dolů **nastavení** > **Kontrola stavu**.</span><span class="sxs-lookup"><span data-stu-id="273d3-239">Select your VM and scroll down **Settings** > **Check Health**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="273d3-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="273d3-240">Additional resources</span></span>
* <span data-ttu-id="273d3-241">Pokud jste stále se nedaří SSH pro virtuální počítač po provedení kroků po, najdete v části [podrobnější pokyny k odstraňování potíží](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) zobrazíte další kroky k vyřešení problému.</span><span class="sxs-lookup"><span data-stu-id="273d3-241">If you are still unable to SSH to your VM after following the after steps, see [more detailed troubleshooting steps](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to review additional steps to resolve your issue.</span></span>
* <span data-ttu-id="273d3-242">Další informace o odstraňování potíží s přístup k aplikaci najdete v tématu [řešení potíží s přístupem k aplikaci spuštěné na virtuálním počítači Azure](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="273d3-242">For more information about troubleshooting application access, see [Troubleshoot access to an application running on an Azure virtual machine](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="273d3-243">Další informace o odstraňování potíží s virtuálních počítačů, které byly vytvořeny pomocí modelu nasazení classic najdete v tématu [jak resetovat heslo nebo SSH pro virtuální počítače se systémem Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="273d3-243">For more information about troubleshooting virtual machines that were created by using the classic deployment model, see [How to reset a password or SSH for Linux-based virtual machines](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

