---
title: "připojení SSH aaaTroubleshoot problémy tooan virtuálního počítače Azure | Microsoft Docs"
description: "Jak tootroubleshoot problémy například \"Připojení SSH se nezdařilo\" nebo \"odmítl připojení SSH' pro virtuální počítač Azure s Linuxem."
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
ms.openlocfilehash: dfb4e75e571c8306edf5f300c4e0f07a5fe7750a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-ssh-connections-tooan-azure-linux-vm-that-fails-errors-out-or-is-refused"></a><span data-ttu-id="0c5a6-104">Řešení potíží s tooan připojení SSH Linux virtuálního počítače Azure, který selže, chyby, nebo bylo odmítnuto</span><span class="sxs-lookup"><span data-stu-id="0c5a6-104">Troubleshoot SSH connections tooan Azure Linux VM that fails, errors out, or is refused</span></span>
<span data-ttu-id="0c5a6-105">Existují různé příčiny, že dojde k chybám Secure Shell (SSH), selhání připojení SSH, nebo SSH bylo odmítnuto, když zkusíte tooconnect tooa Linuxový virtuální počítač (VM).</span><span class="sxs-lookup"><span data-stu-id="0c5a6-105">There are various reasons that you encounter Secure Shell (SSH) errors, SSH connection failures, or SSH is refused when you try tooconnect tooa Linux virtual machine (VM).</span></span> <span data-ttu-id="0c5a6-106">Tento článek vám pomůže najít a správné hello problémy.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-106">This article helps you find and correct hello problems.</span></span> <span data-ttu-id="0c5a6-107">Můžete použít hello portál Azure, rozhraní příkazového řádku Azure nebo rozšíření pro přístup virtuálních počítačů pro Linux tootroubleshoot a vyřešit potíže s připojením.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-107">You can use hello Azure portal, Azure CLI, or VM Access Extension for Linux tootroubleshoot and resolve connection problems.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="0c5a6-108">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](http://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="0c5a6-108">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and Stack Overflow forums](http://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="0c5a6-109">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-109">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="0c5a6-110">Přejděte toohello [podporu Azure lokality](http://azure.microsoft.com/support/options/) a vyberte **získat podporu**.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-110">Go toohello [Azure support site](http://azure.microsoft.com/support/options/) and select **Get support**.</span></span> <span data-ttu-id="0c5a6-111">Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](http://azure.microsoft.com/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="0c5a6-111">For information about using Azure Support, read hello [Microsoft Azure support FAQ](http://azure.microsoft.com/support/faq/).</span></span>

## <a name="quick-troubleshooting-steps"></a><span data-ttu-id="0c5a6-112">Rychlé řešení potíží</span><span class="sxs-lookup"><span data-stu-id="0c5a6-112">Quick troubleshooting steps</span></span>
<span data-ttu-id="0c5a6-113">Po dokončení každého kroku řešení potíží pokuste o připojení toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-113">After each troubleshooting step, try reconnecting toohello VM.</span></span>

1. <span data-ttu-id="0c5a6-114">Obnovte konfiguraci SSH hello.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-114">Reset hello SSH configuration.</span></span>
2. <span data-ttu-id="0c5a6-115">Resetovat hello pověření pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-115">Reset hello credentials for hello user.</span></span>
3. <span data-ttu-id="0c5a6-116">Ověřte hello [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) pravidla povolit provoz protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-116">Verify hello [Network Security Group](../../virtual-network/virtual-networks-nsg.md) rules permit SSH traffic.</span></span>
   * <span data-ttu-id="0c5a6-117">Zkontrolujte, zda pravidlo skupinu zabezpečení sítě existuje provoz toopermit SSH (ve výchozím nastavení je to TCP port 22).</span><span class="sxs-lookup"><span data-stu-id="0c5a6-117">Ensure that a Network Security Group rule exists toopermit SSH traffic (by default, TCP port 22).</span></span>
   * <span data-ttu-id="0c5a6-118">Nemůžete použít přesměrování portu / mapování bez použití pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-118">You cannot use port redirection / mapping without using an Azure load balancer.</span></span>
4. <span data-ttu-id="0c5a6-119">Zkontrolujte hello [stavu prostředků virtuálních počítačů](../../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0c5a6-119">Check hello [VM resource health](../../resource-health/resource-health-overview.md).</span></span> 
   * <span data-ttu-id="0c5a6-120">Ujistěte se, že hello virtuálních počítačů sestavy, že je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-120">Ensure that hello VM reports as being healthy.</span></span>
   * <span data-ttu-id="0c5a6-121">Pokud máte povolené Diagnostika spouštění, ověřte, zda hello virtuálního počítače není reporting spouštěcí chyby v protokolech hello.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-121">If you have boot diagnostics enabled, verify hello VM is not reporting boot errors in hello logs.</span></span>
5. <span data-ttu-id="0c5a6-122">Restartujte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-122">Restart hello VM.</span></span>
6. <span data-ttu-id="0c5a6-123">Znovu nasaďte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-123">Redeploy hello VM.</span></span>

<span data-ttu-id="0c5a6-124">Pokračujte ve čtení pro podrobnější pro řešení potíží a vysvětlení.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-124">Continue reading for more detailed troubleshooting steps and explanations.</span></span>

## <a name="available-methods-tootroubleshoot-ssh-connection-issues"></a><span data-ttu-id="0c5a6-125">Dostupné metody potíží s připojeními SSH tootroubleshoot</span><span class="sxs-lookup"><span data-stu-id="0c5a6-125">Available methods tootroubleshoot SSH connection issues</span></span>
<span data-ttu-id="0c5a6-126">Můžete resetovat konfiguraci SSH pomocí jedné z následujících metod hello nebo přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-126">You can reset credentials or SSH configuration using one of hello following methods:</span></span>

* <span data-ttu-id="0c5a6-127">[Portál Azure](#use-the-azure-portal) – výborné Pokud potřebujete tooquickly resetovat konfiguraci SSH hello nebo klíč SSH a nebudete mít hello nainstalovány nástroje pro Azure.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-127">[Azure portal](#use-the-azure-portal) - great if you need tooquickly reset hello SSH configuration or SSH key and you don't have hello Azure tools installed.</span></span>
* <span data-ttu-id="0c5a6-128">[Azure CLI 2.0](#use-the-azure-cli-20) – Pokud jste už na příkazovém řádku hello, rychle resetování konfigurace SSH hello nebo přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-128">[Azure CLI 2.0](#use-the-azure-cli-20) - if you are already on hello command line, quickly reset hello SSH configuration or credentials.</span></span> <span data-ttu-id="0c5a6-129">Můžete taky hello [1.0 rozhraní příkazového řádku Azure](#use-the-azure-cli-10)</span><span class="sxs-lookup"><span data-stu-id="0c5a6-129">You can also use hello [Azure CLI 1.0](#use-the-azure-cli-10)</span></span>
* <span data-ttu-id="0c5a6-130">[Azure rozšíření VMAccessForLinux](#use-the-vmaccess-extension) – vytvoření a opakovaně používat json definice soubory tooreset hello SSH konfigurace nebo přihlašovací údaje uživatele.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-130">[Azure VMAccessForLinux extension](#use-the-vmaccess-extension) - create and reuse json definition files tooreset hello SSH configuration or user credentials.</span></span>

<span data-ttu-id="0c5a6-131">Po dokončení každého kroku řešení potíží zkuste se znovu připojit tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-131">After each troubleshooting step, try connecting tooyour VM again.</span></span> <span data-ttu-id="0c5a6-132">Pokud se pořád nemůžete připojit, zkuste hello další krok.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-132">If you still cannot connect, try hello next step.</span></span>

## <a name="use-hello-azure-portal"></a><span data-ttu-id="0c5a6-133">Hello použití portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0c5a6-133">Use hello Azure portal</span></span>
<span data-ttu-id="0c5a6-134">Hello portál Azure poskytuje hello tooreset rychlým způsobem SSH konfigurace nebo přihlašovací údaje uživatele bez instalace žádné nástroje na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-134">hello Azure portal provides a quick way tooreset hello SSH configuration or user credentials without installing any tools on your local computer.</span></span>

<span data-ttu-id="0c5a6-135">Vyberte virtuální počítač v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-135">Select your VM in hello Azure portal.</span></span> <span data-ttu-id="0c5a6-136">Projděte dolů toohello **podporu + Poradce při potížích s** a vyberte **resetovat heslo** jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-136">Scroll down toohello **Support + Troubleshooting** section and select **Reset password** as in hello following example:</span></span>

![Resetování konfigurace SSH nebo přihlašovací údaje v hello portálu Azure](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-hello-ssh-configuration"></a><span data-ttu-id="0c5a6-138">Obnovte konfiguraci SSH hello</span><span class="sxs-lookup"><span data-stu-id="0c5a6-138">Reset hello SSH configuration</span></span>
<span data-ttu-id="0c5a6-139">Jako první krok, vyberte `Reset configuration only` z hello **režimu** rozevírací nabídky jako v předchozím snímku obrazovky hello klikněte hello **resetovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-139">As a first step, select `Reset configuration only` from hello **Mode** drop-down menu as in hello preceding screenshot, then click hello **Reset** button.</span></span> <span data-ttu-id="0c5a6-140">Po dokončení této akce opakujte tooaccess virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-140">Once this action has completed, try tooaccess your VM again.</span></span>

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="0c5a6-141">Resetovat SSH přihlašovací údaje pro uživatele</span><span class="sxs-lookup"><span data-stu-id="0c5a6-141">Reset SSH credentials for a user</span></span>
<span data-ttu-id="0c5a6-142">tooreset hello přihlašovací údaje stávajícího uživatele, vyberte buď `Reset SSH public key` nebo `Reset password` z hello **režimu** rozevírací nabídky jako hello předchozím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-142">tooreset hello credentials of an existing user, select either `Reset SSH public key` or `Reset password` from hello **Mode** drop-down menu as in hello preceding screenshot.</span></span> <span data-ttu-id="0c5a6-143">Zadejte uživatelské jméno hello a klíč SSH nebo nové heslo a pak klikněte na hello **resetovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-143">Specify hello username and an SSH key or new password, then click hello **Reset** button.</span></span>

<span data-ttu-id="0c5a6-144">Můžete také vytvořit uživatele s oprávněními sudo v hello virtuálního počítače z této nabídky.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-144">You can also create a user with sudo privileges on hello VM from this menu.</span></span> <span data-ttu-id="0c5a6-145">Zadejte nové uživatelské jméno a přiřazené heslo nebo klíč SSH a pak klikněte na tlačítko hello **resetovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-145">Enter a new username and associated password or SSH key, and then click hello **Reset** button.</span></span>

## <a name="use-hello-azure-cli-20"></a><span data-ttu-id="0c5a6-146">Hello použití Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0c5a6-146">Use hello Azure CLI 2.0</span></span>
<span data-ttu-id="0c5a6-147">Pokud jste to ještě neudělali, nainstalujte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="0c5a6-147">If you haven't already, install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="0c5a6-148">Pokud jste vytvořili a nahrát vlastní image disku Linux, ujistěte se, zda text hello [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verze 2.0.5 nebo novější je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-148">If you created and uploaded a custom Linux disk image, make sure hello [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 or later is installed.</span></span> <span data-ttu-id="0c5a6-149">Pro virtuální počítače vytvořené pomocí Galerie obrázků toto rozšíření přístup k již instalovaných a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-149">For VMs created using Gallery images, this access extension is already installed and configured for you.</span></span>

### <a name="reset-ssh-configuration"></a><span data-ttu-id="0c5a6-150">Obnovte konfiguraci SSH</span><span class="sxs-lookup"><span data-stu-id="0c5a6-150">Reset SSH configuration</span></span>
<span data-ttu-id="0c5a6-151">Můžete Nejdřív zkuste resetuje hello SSH toodefault hodnoty konfigurace a restartování serveru hello SSH na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-151">You can initially try resetting hello SSH configuration toodefault values and rebooting hello SSH server on hello VM.</span></span> <span data-ttu-id="0c5a6-152">Všimněte si, že to nezmění hello název uživatelského účtu, hesla nebo klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-152">Note that this does not change hello user account name, password, or SSH keys.</span></span>
<span data-ttu-id="0c5a6-153">Hello následující příklad používá [az virtuálního počítače uživatele resetování-ssh](/cli/azure/vm/user#reset-ssh) tooreset hello konfiguraci SSH na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-153">hello following example uses [az vm user reset-ssh](/cli/azure/vm/user#reset-ssh) tooreset hello SSH configuration on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-154">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-154">Use your own values as follows:</span></span>

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="0c5a6-155">Resetovat SSH přihlašovací údaje pro uživatele</span><span class="sxs-lookup"><span data-stu-id="0c5a6-155">Reset SSH credentials for a user</span></span>
<span data-ttu-id="0c5a6-156">Hello následující příklad používá [aktualizace uživatele virtuálního počítače az](/cli/azure/vm/user#update) tooreset hello přihlašovací údaje pro `myUsername` toohello hodnota zadaná v `myPassword`, na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-156">hello following example uses [az vm user update](/cli/azure/vm/user#update) tooreset hello credentials for `myUsername` toohello value specified in `myPassword`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-157">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-157">Use your own values as follows:</span></span>

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

<span data-ttu-id="0c5a6-158">Pokud používáte ověření pomocí klíče SSH, můžete resetovat hello klíč SSH pro daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-158">If using SSH key authentication, you can reset hello SSH key for a given user.</span></span> <span data-ttu-id="0c5a6-159">Hello následující příklad používá **az virtuální počítač přístup k set-linux-user** klíč SSH hello tooupdate uložené v `~/.ssh/id_rsa.pub` pro hello uživatele s názvem `myUsername`, na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-159">hello following example uses **az vm access set-linux-user** tooupdate hello SSH key stored in `~/.ssh/id_rsa.pub` for hello user named `myUsername`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-160">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-160">Use your own values as follows:</span></span>

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-hello-vmaccess-extension"></a><span data-ttu-id="0c5a6-161">Použití rozšíření VMAccess hello</span><span class="sxs-lookup"><span data-stu-id="0c5a6-161">Use hello VMAccess extension</span></span>
<span data-ttu-id="0c5a6-162">v souboru json, který definuje akce toocarry si přečte Hello rozšíření pro přístup virtuálních počítačů pro Linux. Mezi ně patří resetování SSHD, resetování klíče SSH nebo přidání uživatele.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-162">hello VM Access Extension for Linux reads in a json file that defines actions toocarry out. These actions include resetting SSHD, resetting an SSH key, or adding a user.</span></span> <span data-ttu-id="0c5a6-163">Dál používat hello rozhraní příkazového řádku Azure toocall hello rozšíření VMAccess, ale můžete opakovaně použít soubory json hello napříč více virtuálními počítači v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-163">You still use hello Azure CLI toocall hello VMAccess extension, but you can reuse hello json files across multiple VMs if desired.</span></span> <span data-ttu-id="0c5a6-164">Tento přístup umožňuje toocreate úložiště json souborů, které může být volána pro daný scénáře.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-164">This approach allows you toocreate a repository of json files that can then be called for given scenarios.</span></span>

### <a name="reset-sshd"></a><span data-ttu-id="0c5a6-165">Resetování SSHD</span><span class="sxs-lookup"><span data-stu-id="0c5a6-165">Reset SSHD</span></span>
<span data-ttu-id="0c5a6-166">Vytvořte soubor s názvem `settings.json` s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-166">Create a file named `settings.json` with hello following content:</span></span>

```json
{  
    "reset_ssh":"True"
}
```

<span data-ttu-id="0c5a6-167">Pomocí hello rozhraní příkazového řádku Azure, potom zavolejte hello `VMAccessForLinux` tooreset rozšíření připojení SSHD zadáním souboru json.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-167">Using hello Azure CLI, you then call hello `VMAccessForLinux` extension tooreset your SSHD connection by specifying your json file.</span></span> <span data-ttu-id="0c5a6-168">Hello následující příklad používá [nastavení rozšíření virtuálního az](/cli/azure/vm/extension#set) tooreset SSHD na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-168">hello following example uses [az vm extension set](/cli/azure/vm/extension#set) tooreset SSHD on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-169">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-169">Use your own values as follows:</span></span>

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="0c5a6-170">Resetovat SSH přihlašovací údaje pro uživatele</span><span class="sxs-lookup"><span data-stu-id="0c5a6-170">Reset SSH credentials for a user</span></span>
<span data-ttu-id="0c5a6-171">Pokud se SSHD zobrazí toofunction správně, můžete resetovat hello pověření pro uživatele třetím osobám.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-171">If SSHD appears toofunction correctly, you can reset hello credentials for a giver user.</span></span> <span data-ttu-id="0c5a6-172">tooreset hello heslo pro uživatele, vytvořte soubor s názvem `settings.json`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-172">tooreset hello password for a user, create a file named `settings.json`.</span></span> <span data-ttu-id="0c5a6-173">Hello následující příklad resetuje hello přihlašovací údaje pro `myUsername` toohello hodnota zadaná v `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-173">hello following example resets hello credentials for `myUsername` toohello value specified in `myPassword`.</span></span> <span data-ttu-id="0c5a6-174">Zadejte hello následující řádky do vaší `settings.json` souboru pomocí vlastní hodnoty:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-174">Enter hello following lines into your `settings.json` file, using your own values:</span></span>

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

<span data-ttu-id="0c5a6-175">Nebo tooreset hello klíč SSH pro uživatele, nejprve vytvořte soubor s názvem `settings.json`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-175">Or tooreset hello SSH key for a user, first create a file named `settings.json`.</span></span> <span data-ttu-id="0c5a6-176">Hello následující příklad resetuje hello přihlašovací údaje pro `myUsername` toohello hodnota zadaná v `myPassword`, na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-176">hello following example resets hello credentials for `myUsername` toohello value specified in `myPassword`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-177">Zadejte hello následující řádky do vaší `settings.json` souboru pomocí vlastní hodnoty:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-177">Enter hello following lines into your `settings.json` file, using your own values:</span></span>

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

<span data-ttu-id="0c5a6-178">Po vytvoření souboru json, použít hello rozhraní příkazového řádku Azure toocall hello `VMAccessForLinux` rozšíření tooreset přihlašovací údaje uživatele SSH zadáním souboru json.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-178">After creating your json file, use hello Azure CLI toocall hello `VMAccessForLinux` extension tooreset your SSH user credentials by specifying your json file.</span></span> <span data-ttu-id="0c5a6-179">Hello následující příklad resetuje přihlašovací údaje u hello virtuálního počítače s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-179">hello following example resets credentials on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-180">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-180">Use your own values as follows:</span></span>

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-hello-azure-cli-10"></a><span data-ttu-id="0c5a6-181">Hello použití Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="0c5a6-181">Use hello Azure CLI 1.0</span></span>
<span data-ttu-id="0c5a6-182">Pokud jste to ještě neudělali, [nainstalovat hello Azure CLI 1.0 a připojte tooyour předplatné](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="0c5a6-182">If you haven't already, [install hello Azure CLI 1.0 and connect tooyour Azure subscription](../../cli-install-nodejs.md).</span></span> <span data-ttu-id="0c5a6-183">Ujistěte se, že režimu Resource Manager používáte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-183">Make sure that you are using Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="0c5a6-184">Pokud jste vytvořili a nahrát vlastní image disku Linux, ujistěte se, zda text hello [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verze 2.0.5 nebo novější je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-184">If you created and uploaded a custom Linux disk image, make sure hello [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) version 2.0.5 or later is installed.</span></span> <span data-ttu-id="0c5a6-185">Pro virtuální počítače vytvořené pomocí Galerie obrázků toto rozšíření přístup k již instalovaných a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-185">For VMs created using Gallery images, this access extension is already installed and configured for you.</span></span>

### <a name="reset-ssh-configuration"></a><span data-ttu-id="0c5a6-186">Obnovte konfiguraci SSH</span><span class="sxs-lookup"><span data-stu-id="0c5a6-186">Reset SSH configuration</span></span>
<span data-ttu-id="0c5a6-187">může být nesprávné konfigurace SSHD Hello, sám sebe nebo hello služby došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-187">hello SSHD configuration itself may be misconfigured or hello service encountered an error.</span></span> <span data-ttu-id="0c5a6-188">Můžete resetovat SSHD toomake se, že je platný hello konfiguraci SSH, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-188">You can reset SSHD toomake sure hello SSH configuration itself is valid.</span></span> <span data-ttu-id="0c5a6-189">Resetování SSHD by měl být hello první krok odstraňování problémů, které můžete provést.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-189">Resetting SSHD should be hello first troubleshooting step you take.</span></span>

<span data-ttu-id="0c5a6-190">Hello následující příklad resetuje SSHD na virtuálním počítači s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-190">hello following example resets SSHD on a VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-191">Používejte vlastní názvy virtuálních počítačů a prostředků skupiny takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-191">Use your own VM and resource group names as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a><span data-ttu-id="0c5a6-192">Resetovat SSH přihlašovací údaje pro uživatele</span><span class="sxs-lookup"><span data-stu-id="0c5a6-192">Reset SSH credentials for a user</span></span>
<span data-ttu-id="0c5a6-193">Pokud se SSHD zobrazí toofunction správně, můžete resetovat hello heslo pro uživatele třetím osobám.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-193">If SSHD appears toofunction correctly, you can reset hello password for a giver user.</span></span> <span data-ttu-id="0c5a6-194">Hello následující příklad resetuje hello přihlašovací údaje pro `myUsername` toohello hodnota zadaná v `myPassword`, na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-194">hello following example resets hello credentials for `myUsername` toohello value specified in `myPassword`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-195">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-195">Use your own values as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

<span data-ttu-id="0c5a6-196">Pokud používáte ověření pomocí klíče SSH, můžete resetovat hello klíč SSH pro daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-196">If using SSH key authentication, you can reset hello SSH key for a given user.</span></span> <span data-ttu-id="0c5a6-197">Následující příklad aktualizace Hello hello klíč SSH, které jsou uložené v `~/.ssh/id_rsa.pub` pro hello uživatele s názvem `myUsername`, na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-197">hello following example updates hello SSH key stored in `~/.ssh/id_rsa.pub` for hello user named `myUsername`, on hello VM named `myVM` in `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-198">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-198">Use your own values as follows:</span></span>

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a><span data-ttu-id="0c5a6-199">Restartování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0c5a6-199">Restart a VM</span></span>
<span data-ttu-id="0c5a6-200">Pokud máte resetovat hello SSH konfigurace a uživatelské přihlašovací údaje, nebo došlo k chybě při tom, můžete zkusit restartovat tooaddress hello virtuálního počítače základní výpočetní problémy.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-200">If you have reset hello SSH configuration and user credentials, or encountered an error in doing so, you can try restarting hello VM tooaddress underlying compute issues.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="0c5a6-201">portál Azure</span><span class="sxs-lookup"><span data-stu-id="0c5a6-201">Azure portal</span></span>
<span data-ttu-id="0c5a6-202">virtuální počítač pomocí toorestart hello portálu Azure vyberte hello váš počítač a klikněte na tlačítko **restartujte** tlačítko jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-202">toorestart a VM using hello Azure portal, select your VM and click hello **Restart** button as in hello following example:</span></span>

![Restartování virtuálního počítače v hello portálu Azure](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli-10"></a><span data-ttu-id="0c5a6-204">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="0c5a6-204">Azure CLI 1.0</span></span>
<span data-ttu-id="0c5a6-205">Následující příklad restartování Hello hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-205">hello following example restarts hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-206">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-206">Use your own values as follows:</span></span>

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a><span data-ttu-id="0c5a6-207">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0c5a6-207">Azure CLI 2.0</span></span>
<span data-ttu-id="0c5a6-208">Hello následující příklad používá [restartování virtuálního počítače az](/cli/azure/vm#restart) toorestart hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-208">hello following example uses [az vm restart](/cli/azure/vm#restart) toorestart hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-209">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-209">Use your own values as follows:</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a><span data-ttu-id="0c5a6-210">Znovunasazení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0c5a6-210">Redeploy a VM</span></span>
<span data-ttu-id="0c5a6-211">Můžete znovu nasadit uzlu tooanother virtuálních počítačů v rámci Azure, která může vyřešit problémy s základní sítě.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-211">You can redeploy a VM tooanother node within Azure, which may correct any underlying networking issues.</span></span> <span data-ttu-id="0c5a6-212">Informace o opětovného nasazení virtuálního počítače najdete v tématu [znovu nasadit virtuální počítač toonew Azure uzlu](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0c5a6-212">For information about redeploying a VM, see [Redeploy virtual machine toonew Azure node](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="0c5a6-213">Po dokončení této operace dočasné disku data budou ztracena a dynamické IP adresy, které jsou přidruženy hello virtuálního počítače budou aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-213">After this operation finishes, ephemeral disk data will be lost and dynamic IP addresses that are associated with hello virtual machine will be updated.</span></span>
> 
> 

### <a name="azure-portal"></a><span data-ttu-id="0c5a6-214">portál Azure</span><span class="sxs-lookup"><span data-stu-id="0c5a6-214">Azure portal</span></span>
<span data-ttu-id="0c5a6-215">virtuální počítač pomocí tooredeploy hello portálu Azure vyberte virtuální počítač a přejděte dolů toohello **podporu + Poradce při potížích s** části.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-215">tooredeploy a VM using hello Azure portal, select your VM and scroll down toohello **Support + Troubleshooting** section.</span></span> <span data-ttu-id="0c5a6-216">Klikněte na tlačítko hello **znovu nasaďte** tlačítko jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-216">Click hello **Redeploy** button as in hello following example:</span></span>

![Opětovné nasazení virtuálního počítače v hello portálu Azure](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli-10"></a><span data-ttu-id="0c5a6-218">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="0c5a6-218">Azure CLI 1.0</span></span>
<span data-ttu-id="0c5a6-219">Následující příklad opětovně nasadí Hello hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-219">hello following example redeploys hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-220">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-220">Use your own values as follows:</span></span>

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a><span data-ttu-id="0c5a6-221">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0c5a6-221">Azure CLI 2.0</span></span>
<span data-ttu-id="0c5a6-222">Následující příklad použití Hello [az virtuálního počítače znovu ho zaveďte](/cli/azure/vm#redeploy) tooredeploy hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-222">hello following example use [az vm redeploy](/cli/azure/vm#redeploy) tooredeploy hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span> <span data-ttu-id="0c5a6-223">Použijte vlastní hodnoty takto:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-223">Use your own values as follows:</span></span>

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-hello-classic-deployment-model"></a><span data-ttu-id="0c5a6-224">Virtuální počítače vytvořené pomocí modelu nasazení Classic hello</span><span class="sxs-lookup"><span data-stu-id="0c5a6-224">VMs created by using hello Classic deployment model</span></span>
<span data-ttu-id="0c5a6-225">Opakujte tyto kroky tooresolve hello nejběžnější SSH selhání připojení pro virtuální počítače, které byly vytvořeny pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-225">Try these steps tooresolve hello most common SSH connection failures for VMs that were created by using hello classic deployment model.</span></span> <span data-ttu-id="0c5a6-226">Po dokončení každého kroku pokuste o připojení toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-226">After each step, try reconnecting toohello VM.</span></span>

* <span data-ttu-id="0c5a6-227">Obnovte vzdálený přístup z hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0c5a6-227">Reset remote access from hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="0c5a6-228">Na hello portálu Azure, vyberte virtuální počítač a klikněte na tlačítko hello **resetovat vzdálený...**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-228">On hello Azure portal, select your VM and click hello **Reset Remote...** button.</span></span>
* <span data-ttu-id="0c5a6-229">Restartujte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-229">Restart hello VM.</span></span> <span data-ttu-id="0c5a6-230">Na hello [portál Azure](https://portal.azure.com), vyberte virtuální počítač a klikněte na tlačítko hello **restartujte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-230">On hello [Azure portal](https://portal.azure.com), select your VM and click hello **Restart** button.</span></span>
    
* <span data-ttu-id="0c5a6-231">Znovu nasaďte hello virtuálních počítačů tooa nového Azure uzlu.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-231">Redeploy hello VM tooa new Azure node.</span></span> <span data-ttu-id="0c5a6-232">Informace o tom najdete v části tooredeploy virtuální počítač, [znovu nasadit virtuální počítač toonew Azure uzlu](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0c5a6-232">For information about how tooredeploy a VM, see [Redeploy virtual machine toonew Azure node](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
  
    <span data-ttu-id="0c5a6-233">Po dokončení této operace dočasné disku data budou ztracena a dynamické IP adresy, které jsou přidruženy hello virtuálního počítače budou aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-233">After this operation finishes, ephemeral disk data will be lost and dynamic IP addresses that are associated with hello virtual machine will be updated.</span></span>
* <span data-ttu-id="0c5a6-234">Postupujte podle pokynů hello v [jak tooreset heslo nebo SSH pro virtuální počítače se systémem Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) na:</span><span class="sxs-lookup"><span data-stu-id="0c5a6-234">Follow hello instructions in [How tooreset a password or SSH for Linux-based virtual machines](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) to:</span></span>
  
  * <span data-ttu-id="0c5a6-235">Resetovat hello heslo nebo klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-235">Reset hello password or SSH key.</span></span>
  * <span data-ttu-id="0c5a6-236">Vytvoření *sudo* uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-236">Create a *sudo* user account.</span></span>
  * <span data-ttu-id="0c5a6-237">Obnovte konfiguraci SSH hello.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-237">Reset hello SSH configuration.</span></span>
* <span data-ttu-id="0c5a6-238">Zkontrolujte stav prostředku hello Virtuálního počítače pro všechny platformy problémy.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-238">Check hello VM's resource health for any platform issues.</span></span><br>
     <span data-ttu-id="0c5a6-239">Vyberte virtuální počítač a přejděte dolů **nastavení** > **Kontrola stavu**.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-239">Select your VM and scroll down **Settings** > **Check Health**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0c5a6-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0c5a6-240">Additional resources</span></span>
* <span data-ttu-id="0c5a6-241">Pokud jste stále se nedaří tooSSH tooyour virtuálních počítačů po následující hello po provedení kroků, najdete v části [podrobnější pokyny k odstraňování potíží](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooreview další kroky tooresolve problém.</span><span class="sxs-lookup"><span data-stu-id="0c5a6-241">If you are still unable tooSSH tooyour VM after following hello after steps, see [more detailed troubleshooting steps](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooreview additional steps tooresolve your issue.</span></span>
* <span data-ttu-id="0c5a6-242">Další informace o odstraňování potíží s přístup k aplikaci najdete v tématu [Poradce při potížích přístup tooan aplikace běžící na virtuálním počítači Azure](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="0c5a6-242">For more information about troubleshooting application access, see [Troubleshoot access tooan application running on an Azure virtual machine](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="0c5a6-243">Další informace o odstraňování potíží s virtuálních počítačů, které byly vytvořeny pomocí modelu nasazení classic hello najdete v tématu [jak tooreset heslo nebo SSH pro virtuální počítače se systémem Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0c5a6-243">For more information about troubleshooting virtual machines that were created by using hello classic deployment model, see [How tooreset a password or SSH for Linux-based virtual machines](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

