---
title: "Použití služby Vzdálená plocha pro virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Postup instalace a konfigurace vzdálené plochy (xrdp) pro připojení k virtuální počítač s Linuxem v Azure pomocí nástroje s grafickým rozhraním"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: iainfou
ms.openlocfilehash: d8d6130a270285c84c1dd057a3512cdeb39287f6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="install-and-configure-remote-desktop-to-connect-to-a-linux-vm-in-azure"></a><span data-ttu-id="6d535-103">Instalace a konfigurace vzdálené plochy pro připojení k virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="6d535-103">Install and configure Remote Desktop to connect to a Linux VM in Azure</span></span>
<span data-ttu-id="6d535-104">Linux virtuálních počítačů (VM) v Azure jsou obvykle spravovat z příkazového řádku pomocí připojení zabezpečené shell (SSH).</span><span class="sxs-lookup"><span data-stu-id="6d535-104">Linux virtual machines (VMs) in Azure are usually managed from the command line using a secure shell (SSH) connection.</span></span> <span data-ttu-id="6d535-105">Při vydání nových do systému Linux, nebo pro rychlé řešení potíží scénáře, může být snazší pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="6d535-105">When new to Linux, or for quick troubleshooting scenarios, the use of remote desktop may be easier.</span></span> <span data-ttu-id="6d535-106">Tento článek podrobně popisují postup instalace a konfigurace prostředí plochy ([xfce](https://www.xfce.org)) a vzdálené plochy ([xrdp](http://www.xrdp.org)) pro váš virtuální počítač s Linuxem pomocí modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6d535-106">This article details how to install and configure a desktop environment ([xfce](https://www.xfce.org)) and remote desktop ([xrdp](http://www.xrdp.org)) for your Linux VM using the Resource Manager deployment model.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="6d535-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6d535-107">Prerequisites</span></span>
<span data-ttu-id="6d535-108">Tento článek vyžaduje existující virtuální počítač s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="6d535-108">This article requires an existing Linux VM in Azure.</span></span> <span data-ttu-id="6d535-109">Pokud potřebujete k vytvoření virtuálního počítače, použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="6d535-109">If you need to create a VM, use one of the following methods:</span></span>

- <span data-ttu-id="6d535-110">[Rozhraní příkazového řádku Azure 2.0](quick-create-cli.md)</span><span class="sxs-lookup"><span data-stu-id="6d535-110">The [Azure CLI 2.0](quick-create-cli.md)</span></span>
- <span data-ttu-id="6d535-111">[Portálu Azure](quick-create-portal.md)</span><span class="sxs-lookup"><span data-stu-id="6d535-111">The [Azure portal](quick-create-portal.md)</span></span>


## <a name="install-a-desktop-environment-on-your-linux-vm"></a><span data-ttu-id="6d535-112">Instalace prostředí plochy na virtuálním počítačům s Linuxem</span><span class="sxs-lookup"><span data-stu-id="6d535-112">Install a desktop environment on your Linux VM</span></span>
<span data-ttu-id="6d535-113">Většina virtuální počítače s Linuxem v Azure, nemají prostředí plochy nainstalované ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6d535-113">Most Linux VMs in Azure do not have a desktop environment installed by default.</span></span> <span data-ttu-id="6d535-114">Virtuální počítače s Linuxem se běžně spravují pomocí připojení SSH a místo prostředí plochy.</span><span class="sxs-lookup"><span data-stu-id="6d535-114">Linux VMs are commonly managed using SSH connections rather than a desktop environment.</span></span> <span data-ttu-id="6d535-115">Existují různé prostředí plochy v systému Linux, které můžete.</span><span class="sxs-lookup"><span data-stu-id="6d535-115">There are various desktop environments in Linux that you can choose.</span></span> <span data-ttu-id="6d535-116">V závislosti na zvoleném prostředí plochy může využívat jeden až 2 GB místa na disku a trvat 5 až 10 minut, nainstalovat a nakonfigurovat všechny požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="6d535-116">Depending on your choice of desktop environment, it may consume one to 2 GB of disk space, and take 5 to 10 minutes to install and configure all the required packages.</span></span>

<span data-ttu-id="6d535-117">Následující příklad nainstaluje jednoduchá [xfce4](https://www.xfce.org/) prostředí plochy na virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="6d535-117">The following example installs the lightweight [xfce4](https://www.xfce.org/) desktop environment on an Ubuntu VM.</span></span> <span data-ttu-id="6d535-118">Příkazy pro jiné distribuce jsou mírně odlišné (použijte `yum` na Red Hat Enterprise Linux nainstalovat a nakonfigurovat příslušný `selinux` pravidla, nebo použijte `zypper` k instalaci na SUSE, třeba).</span><span class="sxs-lookup"><span data-stu-id="6d535-118">Commands for other distributions vary slightly (use `yum` to install on Red Hat Enterprise Linux and configure appropriate `selinux` rules, or use `zypper` to install on SUSE, for example).</span></span>

<span data-ttu-id="6d535-119">První, SSH k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="6d535-119">First, SSH to your VM.</span></span> <span data-ttu-id="6d535-120">V následujícím příkladu se připojuje k virtuálnímu počítači s názvem *myvm.westus.cloudapp.azure.com* k uživatelskému jménu *azureuser*:</span><span class="sxs-lookup"><span data-stu-id="6d535-120">The following example connects to the VM named *myvm.westus.cloudapp.azure.com* with the username of *azureuser*:</span></span>

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

<span data-ttu-id="6d535-121">Pokud používáte systém Windows a jsou nutné další informace o používání SSH, přečtěte si téma [klíče použití SSH se systémem Windows](ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="6d535-121">If you are using Windows and need more information on using SSH, see [How to use SSH keys with Windows](ssh-from-windows.md).</span></span>

<span data-ttu-id="6d535-122">Dále nainstalujte pomocí xfce `apt` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6d535-122">Next, install xfce using `apt` as follows:</span></span>

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a><span data-ttu-id="6d535-123">Instalace a konfigurace serveru vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="6d535-123">Install and configure a remote desktop server</span></span>
<span data-ttu-id="6d535-124">Teď, když máte prostředí plochy nainstalovat, nakonfigurujte služby vzdálené plochy k přijímat příchozí připojení.</span><span class="sxs-lookup"><span data-stu-id="6d535-124">Now that you have a desktop environment installed, configure a remote desktop service to listen for incoming connections.</span></span> <span data-ttu-id="6d535-125">[xrdp](http://xrdp.org) serveru protokolu RDP (Remote Desktop) s otevřeným zdrojem, která je k dispozici na většině Linuxových distribucích a pracuje s xfce.</span><span class="sxs-lookup"><span data-stu-id="6d535-125">[xrdp](http://xrdp.org) is an open source Remote Desktop Protocol (RDP) server that is available on most Linux distributions, and works well with xfce.</span></span> <span data-ttu-id="6d535-126">Nainstalujte xrdp vašeho virtuálního počítače s Ubuntu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6d535-126">Install xrdp on your Ubuntu VM as follows:</span></span>

```bash
sudo apt-get install xrdp
```

<span data-ttu-id="6d535-127">Řekněte xrdp jaké prostředí plochy použít při spuštění relace.</span><span class="sxs-lookup"><span data-stu-id="6d535-127">Tell xrdp what desktop environment to use when you start your session.</span></span> <span data-ttu-id="6d535-128">Nakonfigurujte xrdp používat xfce jako prostředí plochy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6d535-128">Configure xrdp to use xfce as your desktop environment as follows:</span></span>

```bash
echo xfce4-session >~/.xsession
```

<span data-ttu-id="6d535-129">Restartujte službu xrdp změny se projeví následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6d535-129">Restart the xrdp service for the changes to take effect as follows:</span></span>

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a><span data-ttu-id="6d535-130">Nastavení hesla místního uživatelského účtu</span><span class="sxs-lookup"><span data-stu-id="6d535-130">Set a local user account password</span></span>
<span data-ttu-id="6d535-131">Pokud jste vytvořili heslo pro uživatelský účet při vytváření virtuálního počítače, tento krok přeskočte.</span><span class="sxs-lookup"><span data-stu-id="6d535-131">If you created a password for your user account when you created your VM, skip this step.</span></span> <span data-ttu-id="6d535-132">Pokud používáte ověření pomocí klíče SSH pouze a nemají heslo místního účtu nastaven, zadejte heslo, než použijete xrdp k přihlášení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="6d535-132">If you only use SSH key authentication and do not have a local account password set, specify a password before you use xrdp to log in to your VM.</span></span> <span data-ttu-id="6d535-133">xrdp nemůže přijmout klíče SSH pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="6d535-133">xrdp cannot accept SSH keys for authentication.</span></span> <span data-ttu-id="6d535-134">Následující příklad určuje heslo pro uživatelský účet *azureuser*:</span><span class="sxs-lookup"><span data-stu-id="6d535-134">The following example specifies a password for the user account *azureuser*:</span></span>

```bash
sudo passwd azureuser
```

> [!NOTE]
> <span data-ttu-id="6d535-135">Zadání hesla nelze aktualizovat konfiguraci SSHD k povolení přihlášení heslo, pokud je aktuálně nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="6d535-135">Specifying a password does not update your SSHD configuration to permit password logins if it currently does not.</span></span> <span data-ttu-id="6d535-136">Z hlediska zabezpečení, můžete se chcete připojit k virtuálnímu počítači pomocí tunelového propojení SSH pomocí ověřování na základě klíčů a potom se připojte k xrdp.</span><span class="sxs-lookup"><span data-stu-id="6d535-136">From a security perspective, you may wish to connect to your VM with an SSH tunnel using key-based authentication and then connect to xrdp.</span></span> <span data-ttu-id="6d535-137">Pokud ano, přejděte na vytváření pravidla skupiny zabezpečení sítě umožňující vzdálené plochy přenos následující krok.</span><span class="sxs-lookup"><span data-stu-id="6d535-137">If so, skip the following step on creating a network security group rule to allow remote desktop traffic.</span></span>


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a><span data-ttu-id="6d535-138">Vytvoření pravidla skupiny zabezpečení sítě pro přenosy vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="6d535-138">Create a Network Security Group rule for Remote Desktop traffic</span></span>
<span data-ttu-id="6d535-139">Pokud chcete povolit přenosy vzdálené plochy k dosažení virtuálním počítačům s Linuxem, zabezpečení sítě skupiny pravidlo musí být vytvořen, který umožňuje TCP na portu 3389 k dosažení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6d535-139">To allow Remote Desktop traffic to reach your Linux VM, a network security group rule needs to be created that allows TCP on port 3389 to reach your VM.</span></span> <span data-ttu-id="6d535-140">Další informace o pravidel skupiny zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="6d535-140">For more information about network security group rules, see [What is a Network Security Group?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span> <span data-ttu-id="6d535-141">Můžete také [pomocí portálu Azure k vytvoření pravidla skupiny zabezpečení sítě](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6d535-141">You can also [use the Azure portal to create a network security group rule](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="6d535-142">Následující příklady vytvoření pravidla skupiny zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) s názvem *myNetworkSecurityGroupRule* k *povolit* provoz na *tcp* port *3389*.</span><span class="sxs-lookup"><span data-stu-id="6d535-142">The following examples create a network security group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) named *myNetworkSecurityGroupRule* to *allow* traffic on *tcp* port *3389*.</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1010 \
    --destination-port-range 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a><span data-ttu-id="6d535-143">Připojení virtuálním počítačům s Linuxem pomocí klienta vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="6d535-143">Connect your Linux VM with a Remote Desktop client</span></span>
<span data-ttu-id="6d535-144">Otevřete váš místní klient vzdálené plochy a připojení k IP adresu nebo název DNS vašeho virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="6d535-144">Open your local remote desktop client and connect to the IP address or DNS name of your Linux VM.</span></span> <span data-ttu-id="6d535-145">Zadejte uživatelské jméno a heslo pro uživatelský účet na vašem virtuálním počítači následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6d535-145">Enter the username and password for the user account on your VM as follows:</span></span>

![Připojení k xrdp pomocí vašeho klienta vzdálené plochy](./media/use-remote-desktop/remote-desktop-client.png)

<span data-ttu-id="6d535-147">Po ověření prostředí plochy xfce načíst a vypadat podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="6d535-147">After authenticating, the xfce desktop environment will load and look similar to the following example:</span></span>

![prostředí plochy xfce prostřednictvím xrdp](./media/use-remote-desktop/xfce-desktop-environment.png)


## <a name="troubleshoot"></a><span data-ttu-id="6d535-149">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="6d535-149">Troubleshoot</span></span>
<span data-ttu-id="6d535-150">Pokud se nemůžete připojit k virtuálním počítačům s Linuxem pomocí klienta vzdálené plochy, použijte `netstat` na virtuálním počítačům s Linuxem k ověření, že je váš virtuální počítač nenaslouchá pro připojení RDP:</span><span class="sxs-lookup"><span data-stu-id="6d535-150">If you cannot connect to your Linux VM using a Remote Desktop client, use `netstat` on your Linux VM to verify that your VM is listening for RDP connections  as follows:</span></span>

```bash
sudo netstat -plnt | grep rdp
```

<span data-ttu-id="6d535-151">Následující příklad ukazuje, virtuální počítač naslouchá na TCP port 3389 podle očekávání:</span><span class="sxs-lookup"><span data-stu-id="6d535-151">The following example shows the VM listening on TCP port 3389 as expected:</span></span>

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

<span data-ttu-id="6d535-152">Pokud není služba xrdp naslouchá, virtuálního počítače s Ubuntu spusťte znovu službu na následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6d535-152">If the xrdp service is not listening, on an Ubuntu VM restart the service as follows:</span></span>

```bash
sudo service xrdp restart
```

<span data-ttu-id="6d535-153">Zkontrolujte protokoly */var/log*Thug na vaše virtuálního počítače s Ubuntu pro indikaci, proč služba nereaguje.</span><span class="sxs-lookup"><span data-stu-id="6d535-153">Review logs in */var/log*Thug  on your Ubuntu VM for indications as to why the service may not be responding.</span></span> <span data-ttu-id="6d535-154">Také můžete monitorovat syslog při pokusu o připojení ke vzdálené ploše zobrazíte všechny chyby:</span><span class="sxs-lookup"><span data-stu-id="6d535-154">You can also monitor the syslog during a remote desktop connection attempt to view any errors:</span></span>

```bash
tail -f /var/log/syslog
```

<span data-ttu-id="6d535-155">Další Linuxových distribucích například Red Hat Enterprise Linux a SUSE může mít různé způsoby, jak restartujte služby a umístění souborů alternativní protokolu ke kontrole.</span><span class="sxs-lookup"><span data-stu-id="6d535-155">Other Linux distributions such as Red Hat Enterprise Linux and SUSE may have different ways to restart services and alternate log file locations to review.</span></span>

<span data-ttu-id="6d535-156">Pokud neobdrží všechny odpovědi v klientovi vzdálené plochy a nejsou vidět všechny události v systémovém protokolu, toto chování Určuje, že vzdálené plochy provoz nemůže připojit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6d535-156">If you do not receive any response in your remote desktop client and do not see any events in the system log, this behavior indicates that remote desktop traffic cannot reach the VM.</span></span> <span data-ttu-id="6d535-157">Zkontrolujte vaše pravidel skupiny zabezpečení sítě k zajištění, že máte pravidlo pro povolení TCP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="6d535-157">Review your network security group rules to ensure that you have a rule to permit TCP on port 3389.</span></span> <span data-ttu-id="6d535-158">Další informace najdete v tématu [problémů s připojením aplikace](../windows/troubleshoot-app-connection.md).</span><span class="sxs-lookup"><span data-stu-id="6d535-158">For more information, see [Troubleshoot application connectivity issues](../windows/troubleshoot-app-connection.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="6d535-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d535-159">Next steps</span></span>
<span data-ttu-id="6d535-160">Další informace o vytváření a používání klíčů SSH s virtuální počítače s Linuxem najdete v tématu [vytvořit SSH klíče pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="6d535-160">For more information about creating and using SSH keys with Linux VMs, see [Create SSH keys for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

<span data-ttu-id="6d535-161">Informace o používání SSH ze systému Windows najdete v tématu [klíče použití SSH se systémem Windows](ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="6d535-161">For information on using SSH from Windows, see [How to use SSH keys with Windows](ssh-from-windows.md).</span></span>

