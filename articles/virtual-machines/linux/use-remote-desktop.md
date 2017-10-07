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
# <a name="install-and-configure-remote-desktop-tooconnect-tooa-linux-vm-in-azure"></a>Instalace a konfigurace vzdálené plochy tooconnect tooa virtuálního počítače s Linuxem v Azure
Linux virtuálních počítačů (VM) v Azure jsou obvykle spravovat z příkazového řádku hello pomocí připojení zabezpečené shell (SSH). Pokud nové tooLinux, nebo pro rychlé řešení potíží scénáře, může být snazší hello pomocí vzdálené plochy. Tento článek podrobnosti o tom, jak tooinstall a konfigurace prostředí plochy ([xfce](https://www.xfce.org)) a vzdálené plochy ([xrdp](http://www.xrdp.org)) pro váš virtuální počítač s Linuxem pomocí modelu nasazení Resource Manager hello.


## <a name="prerequisites"></a>Požadavky
Tento článek vyžaduje existující virtuální počítač s Linuxem v Azure. Pokud potřebujete toocreate virtuální počítač, použijte jednu z následujících metod hello:

- Hello [2.0 rozhraní příkazového řádku Azure](quick-create-cli.md)
- Hello [portálu Azure](quick-create-portal.md)


## <a name="install-a-desktop-environment-on-your-linux-vm"></a>Instalace prostředí plochy na virtuálním počítačům s Linuxem
Většina virtuální počítače s Linuxem v Azure, nemají prostředí plochy nainstalované ve výchozím nastavení. Virtuální počítače s Linuxem se běžně spravují pomocí připojení SSH a místo prostředí plochy. Existují různé prostředí plochy v systému Linux, které můžete. V závislosti na zvoleném prostředí plochy může využívat jeden too2 GB místa na disku a trvat 5 minut tooinstall too10 a nakonfigurovat všechny hello požadované balíčky.

Hello následující příklad nainstaluje hello lightweight [xfce4](https://www.xfce.org/) prostředí plochy na virtuálního počítače s Ubuntu. Příkazy pro jiné distribuce jsou mírně odlišné (použijte `yum` tooinstall na Red Hat Enterprise Linux a nakonfigurujte příslušné `selinux` pravidla, nebo použijte `zypper` tooinstall na SUSE, např.).

První, tooyour SSH virtuálních počítačů. Hello následující příklad se připojí toohello virtuálního počítače s názvem *myvm.westus.cloudapp.azure.com* uživatelskému jménu hello *azureuser*:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Pokud používáte systém Windows a jsou nutné další informace o používání SSH, přečtěte si téma [jak klíče toouse SSH se systémem Windows](ssh-from-windows.md).

Dále nainstalujte pomocí xfce `apt` následujícím způsobem:

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a>Instalace a konfigurace serveru vzdálené plochy
Teď, když máte prostředí plochy nainstalovat, nakonfigurujte toolisten služby vzdálené plochy pro příchozí spojení. [xrdp](http://xrdp.org) serveru protokolu RDP (Remote Desktop) s otevřeným zdrojem, která je k dispozici na většině Linuxových distribucích a pracuje s xfce. Nainstalujte xrdp vašeho virtuálního počítače s Ubuntu následujícím způsobem:

```bash
sudo apt-get install xrdp
```

Řekněte xrdp jaké prostředí plochy toouse při spuštění relace. V prostředí plochy nakonfigurujte xrdp toouse xfce následujícím způsobem:

```bash
echo xfce4-session >~/.xsession
```

Restartujte službu xrdp hello hello změny tootake efektu následujícím způsobem:

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a>Nastavení hesla místního uživatelského účtu
Pokud jste vytvořili heslo pro uživatelský účet při vytváření virtuálního počítače, tento krok přeskočte. Pokud používáte ověření pomocí klíče SSH pouze a nemají heslo místního účtu nastaven, zadejte heslo, než použijete xrdp toolog v tooyour virtuálních počítačů. xrdp nemůže přijmout klíče SSH pro ověřování. Hello následující příklad určuje heslo pro uživatelský účet hello *azureuser*:

```bash
sudo passwd azureuser
```

> [!NOTE]
> Zadání hesla neaktualizuje vaše přihlášení SSHD konfigurace toopermit heslo, pokud je aktuálně nepoužívá. Z hlediska zabezpečení, můžete chcete tooconnect tooyour virtuálních počítačů s tunelového propojení SSH pomocí ověřování na základě klíčů a potom se připojte tooxrdp. Pokud ano, přeskočte hello následující krok k vytvoření zabezpečení skupiny pravidlo tooallow vzdálené plochy síti.


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a>Vytvoření pravidla skupiny zabezpečení sítě pro přenosy vzdálené plochy
tooallow vzdálené plochy provoz tooreach virtuálním počítačům s Linuxem, pravidlo pro skupinu zabezpečení sítě musí vytvořit toobe umožňující TCP na portu 3389 tooreach virtuálního počítače. Další informace o pravidel skupiny zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Můžete také [použití hello Azure portálu toocreate pravidlo pro skupinu zabezpečení sítě](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Hello následující příklady vytvořit pravidlo skupiny zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) s názvem *myNetworkSecurityGroupRule* příliš*povolit* provoz na *tcp* port *3389*.

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1010 \
    --destination-port-range 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a>Připojení virtuálním počítačům s Linuxem pomocí klienta vzdálené plochy
Otevřete váš místní klient služby Vzdálená plocha a připojte toohello IP adresu nebo název DNS vašeho virtuálního počítače s Linuxem. Zadejte hello uživatelské jméno a heslo pro uživatelský účet hello na vašem virtuálním počítači následujícím způsobem:

![Připojit tooxrdp pomocí vašeho klienta vzdálené plochy](./media/use-remote-desktop/remote-desktop-client.png)

Po ověření prostředí plochy xfce hello načíst a vypadat podobně jako toohello následující ukázka:

![prostředí plochy xfce prostřednictvím xrdp](./media/use-remote-desktop/xfce-desktop-environment.png)


## <a name="troubleshoot"></a>Řešení potíží
Pokud se nemůžete připojit tooyour virtuálního počítače s Linuxem pomocí klienta vzdálené plochy, použijte `netstat` na váš virtuální počítač s Linuxem tooverify, která virtuálního počítače naslouchá pro připojení RDP následujícím způsobem:

```bash
sudo netstat -plnt | grep rdp
```

Následující příklad ukazuje Hello hello virtuálních počítačů naslouchá na TCP port 3389 podle očekávání:

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

Pokud není služba xrdp hello naslouchá, na virtuálního počítače s Ubuntu službu hello následujícím způsobem:

```bash
sudo service xrdp restart
```

Zkontrolujte protokoly */var/log*Thug na vaše virtuálního počítače s Ubuntu pro indikaci jako toowhy hello služba nereaguje. Můžete také sledovat hello syslog během tooview pokus o připojení ke vzdálené ploše všechny chyby:

```bash
tail -f /var/log/syslog
```

Další Linuxových distribucích například Red Hat Enterprise Linux a SUSE může mít různé způsoby toorestart služeb a tooreview umístění souboru alternativní protokolu.

Pokud neobdrží všechny odpovědi v klientovi vzdálené plochy a nejsou vidět všechny události v protokolu systému hello, toto chování Určuje, že vzdálené plochy provoz nelze kontaktovat hello virtuálních počítačů. Zkontrolujte vaše síť zabezpečení skupiny pravidel tooensure mít pravidlo toopermit TCP na portu 3389. Další informace najdete v tématu [problémů s připojením aplikace](../windows/troubleshoot-app-connection.md).


## <a name="next-steps"></a>Další kroky
Další informace o vytváření a používání klíčů SSH s virtuální počítače s Linuxem najdete v tématu [vytvořit SSH klíče pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md).

Informace o používání SSH ze systému Windows najdete v tématu [jak klíče toouse SSH se systémem Windows](ssh-from-windows.md).

