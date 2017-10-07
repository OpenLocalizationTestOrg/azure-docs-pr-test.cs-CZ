---
title: "Vzdálená plocha tooa aaaUse virtuálního počítače s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall a konfigurace vzdálené plochy (xrdp) tooconnect tooa virtuálního počítače s Linuxem v Azure pomocí nástroje s grafickým rozhraním"
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
ms.openlocfilehash: 64d30be101ceeb49fc05bb10293ad63db358efe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-remote-desktop-tooconnect-tooa-linux-vm-in-azure"></a><span data-ttu-id="2fcdb-103">Instalace a konfigurace vzdálené plochy tooconnect tooa virtuálního počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="2fcdb-103">Install and configure Remote Desktop tooconnect tooa Linux VM in Azure</span></span>
<span data-ttu-id="2fcdb-104">Linux virtuálních počítačů (VM) v Azure jsou obvykle spravovat z příkazového řádku hello pomocí připojení zabezpečené shell (SSH).</span><span class="sxs-lookup"><span data-stu-id="2fcdb-104">Linux virtual machines (VMs) in Azure are usually managed from hello command line using a secure shell (SSH) connection.</span></span> <span data-ttu-id="2fcdb-105">Pokud nové tooLinux, nebo pro rychlé řešení potíží scénáře, může být snazší hello pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-105">When new tooLinux, or for quick troubleshooting scenarios, hello use of remote desktop may be easier.</span></span> <span data-ttu-id="2fcdb-106">Tento článek podrobnosti o tom, jak tooinstall a konfigurace prostředí plochy ([xfce](https://www.xfce.org)) a vzdálené plochy ([xrdp](http://www.xrdp.org)) pro váš virtuální počítač s Linuxem pomocí modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-106">This article details how tooinstall and configure a desktop environment ([xfce](https://www.xfce.org)) and remote desktop ([xrdp](http://www.xrdp.org)) for your Linux VM using hello Resource Manager deployment model.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="2fcdb-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2fcdb-107">Prerequisites</span></span>
<span data-ttu-id="2fcdb-108">Tento článek vyžaduje existující virtuální počítač s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-108">This article requires an existing Linux VM in Azure.</span></span> <span data-ttu-id="2fcdb-109">Pokud potřebujete toocreate virtuální počítač, použijte jednu z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-109">If you need toocreate a VM, use one of hello following methods:</span></span>

- <span data-ttu-id="2fcdb-110">Hello [2.0 rozhraní příkazového řádku Azure](quick-create-cli.md)</span><span class="sxs-lookup"><span data-stu-id="2fcdb-110">hello [Azure CLI 2.0](quick-create-cli.md)</span></span>
- <span data-ttu-id="2fcdb-111">Hello [portálu Azure](quick-create-portal.md)</span><span class="sxs-lookup"><span data-stu-id="2fcdb-111">hello [Azure portal](quick-create-portal.md)</span></span>


## <a name="install-a-desktop-environment-on-your-linux-vm"></a><span data-ttu-id="2fcdb-112">Instalace prostředí plochy na virtuálním počítačům s Linuxem</span><span class="sxs-lookup"><span data-stu-id="2fcdb-112">Install a desktop environment on your Linux VM</span></span>
<span data-ttu-id="2fcdb-113">Většina virtuální počítače s Linuxem v Azure, nemají prostředí plochy nainstalované ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-113">Most Linux VMs in Azure do not have a desktop environment installed by default.</span></span> <span data-ttu-id="2fcdb-114">Virtuální počítače s Linuxem se běžně spravují pomocí připojení SSH a místo prostředí plochy.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-114">Linux VMs are commonly managed using SSH connections rather than a desktop environment.</span></span> <span data-ttu-id="2fcdb-115">Existují různé prostředí plochy v systému Linux, které můžete.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-115">There are various desktop environments in Linux that you can choose.</span></span> <span data-ttu-id="2fcdb-116">V závislosti na zvoleném prostředí plochy může využívat jeden too2 GB místa na disku a trvat 5 minut tooinstall too10 a nakonfigurovat všechny hello požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-116">Depending on your choice of desktop environment, it may consume one too2 GB of disk space, and take 5 too10 minutes tooinstall and configure all hello required packages.</span></span>

<span data-ttu-id="2fcdb-117">Hello následující příklad nainstaluje hello lightweight [xfce4](https://www.xfce.org/) prostředí plochy na virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-117">hello following example installs hello lightweight [xfce4](https://www.xfce.org/) desktop environment on an Ubuntu VM.</span></span> <span data-ttu-id="2fcdb-118">Příkazy pro jiné distribuce jsou mírně odlišné (použijte `yum` tooinstall na Red Hat Enterprise Linux a nakonfigurujte příslušné `selinux` pravidla, nebo použijte `zypper` tooinstall na SUSE, např.).</span><span class="sxs-lookup"><span data-stu-id="2fcdb-118">Commands for other distributions vary slightly (use `yum` tooinstall on Red Hat Enterprise Linux and configure appropriate `selinux` rules, or use `zypper` tooinstall on SUSE, for example).</span></span>

<span data-ttu-id="2fcdb-119">První, tooyour SSH virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-119">First, SSH tooyour VM.</span></span> <span data-ttu-id="2fcdb-120">Hello následující příklad se připojí toohello virtuálního počítače s názvem *myvm.westus.cloudapp.azure.com* uživatelskému jménu hello *azureuser*:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-120">hello following example connects toohello VM named *myvm.westus.cloudapp.azure.com* with hello username of *azureuser*:</span></span>

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

<span data-ttu-id="2fcdb-121">Pokud používáte systém Windows a jsou nutné další informace o používání SSH, přečtěte si téma [jak klíče toouse SSH se systémem Windows](ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="2fcdb-121">If you are using Windows and need more information on using SSH, see [How toouse SSH keys with Windows](ssh-from-windows.md).</span></span>

<span data-ttu-id="2fcdb-122">Dále nainstalujte pomocí xfce `apt` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-122">Next, install xfce using `apt` as follows:</span></span>

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a><span data-ttu-id="2fcdb-123">Instalace a konfigurace serveru vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="2fcdb-123">Install and configure a remote desktop server</span></span>
<span data-ttu-id="2fcdb-124">Teď, když máte prostředí plochy nainstalovat, nakonfigurujte toolisten služby vzdálené plochy pro příchozí spojení.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-124">Now that you have a desktop environment installed, configure a remote desktop service toolisten for incoming connections.</span></span> <span data-ttu-id="2fcdb-125">[xrdp](http://xrdp.org) serveru protokolu RDP (Remote Desktop) s otevřeným zdrojem, která je k dispozici na většině Linuxových distribucích a pracuje s xfce.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-125">[xrdp](http://xrdp.org) is an open source Remote Desktop Protocol (RDP) server that is available on most Linux distributions, and works well with xfce.</span></span> <span data-ttu-id="2fcdb-126">Nainstalujte xrdp vašeho virtuálního počítače s Ubuntu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-126">Install xrdp on your Ubuntu VM as follows:</span></span>

```bash
sudo apt-get install xrdp
```

<span data-ttu-id="2fcdb-127">Řekněte xrdp jaké prostředí plochy toouse při spuštění relace.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-127">Tell xrdp what desktop environment toouse when you start your session.</span></span> <span data-ttu-id="2fcdb-128">V prostředí plochy nakonfigurujte xrdp toouse xfce následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-128">Configure xrdp toouse xfce as your desktop environment as follows:</span></span>

```bash
echo xfce4-session >~/.xsession
```

<span data-ttu-id="2fcdb-129">Restartujte službu xrdp hello hello změny tootake efektu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-129">Restart hello xrdp service for hello changes tootake effect as follows:</span></span>

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a><span data-ttu-id="2fcdb-130">Nastavení hesla místního uživatelského účtu</span><span class="sxs-lookup"><span data-stu-id="2fcdb-130">Set a local user account password</span></span>
<span data-ttu-id="2fcdb-131">Pokud jste vytvořili heslo pro uživatelský účet při vytváření virtuálního počítače, tento krok přeskočte.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-131">If you created a password for your user account when you created your VM, skip this step.</span></span> <span data-ttu-id="2fcdb-132">Pokud používáte ověření pomocí klíče SSH pouze a nemají heslo místního účtu nastaven, zadejte heslo, než použijete xrdp toolog v tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-132">If you only use SSH key authentication and do not have a local account password set, specify a password before you use xrdp toolog in tooyour VM.</span></span> <span data-ttu-id="2fcdb-133">xrdp nemůže přijmout klíče SSH pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-133">xrdp cannot accept SSH keys for authentication.</span></span> <span data-ttu-id="2fcdb-134">Hello následující příklad určuje heslo pro uživatelský účet hello *azureuser*:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-134">hello following example specifies a password for hello user account *azureuser*:</span></span>

```bash
sudo passwd azureuser
```

> [!NOTE]
> <span data-ttu-id="2fcdb-135">Zadání hesla neaktualizuje vaše přihlášení SSHD konfigurace toopermit heslo, pokud je aktuálně nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-135">Specifying a password does not update your SSHD configuration toopermit password logins if it currently does not.</span></span> <span data-ttu-id="2fcdb-136">Z hlediska zabezpečení, můžete chcete tooconnect tooyour virtuálních počítačů s tunelového propojení SSH pomocí ověřování na základě klíčů a potom se připojte tooxrdp.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-136">From a security perspective, you may wish tooconnect tooyour VM with an SSH tunnel using key-based authentication and then connect tooxrdp.</span></span> <span data-ttu-id="2fcdb-137">Pokud ano, přeskočte hello následující krok k vytvoření zabezpečení skupiny pravidlo tooallow vzdálené plochy síti.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-137">If so, skip hello following step on creating a network security group rule tooallow remote desktop traffic.</span></span>


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a><span data-ttu-id="2fcdb-138">Vytvoření pravidla skupiny zabezpečení sítě pro přenosy vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="2fcdb-138">Create a Network Security Group rule for Remote Desktop traffic</span></span>
<span data-ttu-id="2fcdb-139">tooallow vzdálené plochy provoz tooreach virtuálním počítačům s Linuxem, pravidlo pro skupinu zabezpečení sítě musí vytvořit toobe umožňující TCP na portu 3389 tooreach virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-139">tooallow Remote Desktop traffic tooreach your Linux VM, a network security group rule needs toobe created that allows TCP on port 3389 tooreach your VM.</span></span> <span data-ttu-id="2fcdb-140">Další informace o pravidel skupiny zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2fcdb-140">For more information about network security group rules, see [What is a Network Security Group?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span> <span data-ttu-id="2fcdb-141">Můžete také [použití hello Azure portálu toocreate pravidlo pro skupinu zabezpečení sítě](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2fcdb-141">You can also [use hello Azure portal toocreate a network security group rule](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="2fcdb-142">Hello následující příklady vytvořit pravidlo skupiny zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) s názvem *myNetworkSecurityGroupRule* příliš*povolit* provoz na *tcp* port *3389*.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-142">hello following examples create a network security group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) named *myNetworkSecurityGroupRule* too*allow* traffic on *tcp* port *3389*.</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1010 \
    --destination-port-range 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a><span data-ttu-id="2fcdb-143">Připojení virtuálním počítačům s Linuxem pomocí klienta vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="2fcdb-143">Connect your Linux VM with a Remote Desktop client</span></span>
<span data-ttu-id="2fcdb-144">Otevřete váš místní klient služby Vzdálená plocha a připojte toohello IP adresu nebo název DNS vašeho virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-144">Open your local remote desktop client and connect toohello IP address or DNS name of your Linux VM.</span></span> <span data-ttu-id="2fcdb-145">Zadejte hello uživatelské jméno a heslo pro uživatelský účet hello na vašem virtuálním počítači následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-145">Enter hello username and password for hello user account on your VM as follows:</span></span>

![Připojit tooxrdp pomocí vašeho klienta vzdálené plochy](./media/use-remote-desktop/remote-desktop-client.png)

<span data-ttu-id="2fcdb-147">Po ověření prostředí plochy xfce hello načíst a vypadat podobně jako toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-147">After authenticating, hello xfce desktop environment will load and look similar toohello following example:</span></span>

![prostředí plochy xfce prostřednictvím xrdp](./media/use-remote-desktop/xfce-desktop-environment.png)


## <a name="troubleshoot"></a><span data-ttu-id="2fcdb-149">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="2fcdb-149">Troubleshoot</span></span>
<span data-ttu-id="2fcdb-150">Pokud se nemůžete připojit tooyour virtuálního počítače s Linuxem pomocí klienta vzdálené plochy, použijte `netstat` na váš virtuální počítač s Linuxem tooverify, která virtuálního počítače naslouchá pro připojení RDP následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-150">If you cannot connect tooyour Linux VM using a Remote Desktop client, use `netstat` on your Linux VM tooverify that your VM is listening for RDP connections  as follows:</span></span>

```bash
sudo netstat -plnt | grep rdp
```

<span data-ttu-id="2fcdb-151">Následující příklad ukazuje Hello hello virtuálních počítačů naslouchá na TCP port 3389 podle očekávání:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-151">hello following example shows hello VM listening on TCP port 3389 as expected:</span></span>

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

<span data-ttu-id="2fcdb-152">Pokud není služba xrdp hello naslouchá, na virtuálního počítače s Ubuntu službu hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-152">If hello xrdp service is not listening, on an Ubuntu VM restart hello service as follows:</span></span>

```bash
sudo service xrdp restart
```

<span data-ttu-id="2fcdb-153">Zkontrolujte protokoly */var/log*Thug na vaše virtuálního počítače s Ubuntu pro indikaci jako toowhy hello služba nereaguje.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-153">Review logs in */var/log*Thug  on your Ubuntu VM for indications as toowhy hello service may not be responding.</span></span> <span data-ttu-id="2fcdb-154">Můžete také sledovat hello syslog během tooview pokus o připojení ke vzdálené ploše všechny chyby:</span><span class="sxs-lookup"><span data-stu-id="2fcdb-154">You can also monitor hello syslog during a remote desktop connection attempt tooview any errors:</span></span>

```bash
tail -f /var/log/syslog
```

<span data-ttu-id="2fcdb-155">Další Linuxových distribucích například Red Hat Enterprise Linux a SUSE může mít různé způsoby toorestart služeb a tooreview umístění souboru alternativní protokolu.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-155">Other Linux distributions such as Red Hat Enterprise Linux and SUSE may have different ways toorestart services and alternate log file locations tooreview.</span></span>

<span data-ttu-id="2fcdb-156">Pokud neobdrží všechny odpovědi v klientovi vzdálené plochy a nejsou vidět všechny události v protokolu systému hello, toto chování Určuje, že vzdálené plochy provoz nelze kontaktovat hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-156">If you do not receive any response in your remote desktop client and do not see any events in hello system log, this behavior indicates that remote desktop traffic cannot reach hello VM.</span></span> <span data-ttu-id="2fcdb-157">Zkontrolujte vaše síť zabezpečení skupiny pravidel tooensure mít pravidlo toopermit TCP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="2fcdb-157">Review your network security group rules tooensure that you have a rule toopermit TCP on port 3389.</span></span> <span data-ttu-id="2fcdb-158">Další informace najdete v tématu [problémů s připojením aplikace](../windows/troubleshoot-app-connection.md).</span><span class="sxs-lookup"><span data-stu-id="2fcdb-158">For more information, see [Troubleshoot application connectivity issues](../windows/troubleshoot-app-connection.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="2fcdb-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2fcdb-159">Next steps</span></span>
<span data-ttu-id="2fcdb-160">Další informace o vytváření a používání klíčů SSH s virtuální počítače s Linuxem najdete v tématu [vytvořit SSH klíče pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="2fcdb-160">For more information about creating and using SSH keys with Linux VMs, see [Create SSH keys for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

<span data-ttu-id="2fcdb-161">Informace o používání SSH ze systému Windows najdete v tématu [jak klíče toouse SSH se systémem Windows](ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="2fcdb-161">For information on using SSH from Windows, see [How toouse SSH keys with Windows](ssh-from-windows.md).</span></span>

