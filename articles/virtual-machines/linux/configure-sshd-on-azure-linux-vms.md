---
title: "Konfigurace SSHD na virtuálních počítačích Azure Linux | Microsoft Docs"
description: "Nakonfigurujte SSHD pro osvědčené postupy zabezpečení a uzamčení SSH na Linux virtuálních počítačích Azure."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/21/2016
ms.author: v-livech
ms.openlocfilehash: 0195c385b00ab92b2b92ce8ff00983a0d91bf3a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-sshd-on-azure-linux-vms"></a>Konfigurace SSHD na virtuálních počítačích Azure Linux

Tento článek ukazuje, jak mají uzamčení SSH serveru v systému Linux, osvědčené postupy zabezpečení a také urychlit proces přihlášení SSH pomocí protokolu SSH klíče místo hesla.  Pro další uzamčení SSHD přidáme zakázat kořenového uživatele nebudou moci přihlásit, omezit uživatele, kteří mohou k přihlášení pomocí seznam schválených skupin, zakázat protokol SSH verze 1, nastavit minimální klíče bit a nakonfigurujte automaticky odhlášení uživatelů nečinnosti.  Požadavky na tomto článku jsou: účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a [SSH soubory veřejného a privátního klíče](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-commands"></a>Rychlé příkazy

Konfigurace `/etc/ssh/sshd_config` s následujícím nastavením:

### <a name="disable-password-logins"></a>Zakázat přihlášení heslo

```bash
PasswordAuthentication no
```

### <a name="disable-login-by-the-root-user"></a>Zakázat přihlášení uživatele root

```bash
PermitRootLogin no
```

### <a name="allowed-groups-list"></a>Skupiny seznamu povolených aplikací

```bash
AllowGroups wheel
```

### <a name="allowed-users-list"></a>Seznam povolených uživatelů

```bash
AllowUsers ahmet ralph
```

### <a name="disable-ssh-protocol-version-1"></a>Zakázat protokol SSH verze 1

```bash
Protocol 2
```

### <a name="minimum-key-bits"></a>Minimální klíče služby bits

```bash
ServerKeyBits 2048
```

### <a name="disconnect-idle-users"></a>Odpojit neaktivní uživatele

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="detailed-walkthrough"></a>Podrobný postup

SSHD je SSH Server, který běží na virtuální počítač s Linuxem.  SSH je klient spuštěný v systému Windows z prostředí na vaše MacBook Linux pracovní stanice, nebo Bash.  SSH je také protokol použitý k zabezpečení a zašifrování komunikace mezi pracovní stanice a virtuálního počítače s Linuxem vytvoření SSH také virtuální privátní sítě (virtuální privátní sítě).

V tomto článku je velmi důležité udržovat jeden přihlášení k virtuálním počítačům s Linuxem otevřené pro celý návodu.  Po vytvoření připojení SSH zůstane jako otevřít relaci tak dlouho, dokud není okno zavřeno.  Jeden Terminálové přihlášení, umožní změny prováděné SSHD service bez uzamknutí Pokud narušující změně.  Pokud jste zamknout z virtuálního počítače Linux s konfigurací porušený SSHD, Azure nabízí možnost obnovit poškozený SSHD konfigurace s [rozšíření pro přístup virtuálních počítačů Azure](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Z tohoto důvodu jsme otevřít dva terminály a SSH pro virtuální počítač s Linuxem z obou z nich.  Pro provedení změn SSHDs konfigurační soubor a restartujte službu SSHD používáme první terminálu.  Druhý terminálu používáme k testování tyto změny po restartování služby.  Protože jsme zakazování SSH hesla a program předávající výhradně na klíče SSH, pokud nejsou správné klíče SSH a zavřete připojení k virtuálnímu počítači, virtuální počítač bude trvale uzamčena a nebude moci přihlásit k němu nutnosti ho odstranit a znovu vytvoří.

## <a name="disable-password-logins"></a>Zakázat přihlášení heslo

Nejrychlejší způsob, jak zabezpečit virtuálního počítače s Linuxem je zakázat přihlášení heslo.  Pokud jsou povolené heslo přihlášení, robotů procházení webu bude okamžitě počáteční pokus o hrubou silou uhodnout heslo pro váš virtuální počítač s Linuxem pomocí protokolu SSH.  Zakázání heslo přihlášení úplně, umožňuje serveru SSH ignorovat všechny pokusů o přihlášení heslo.

```bash
PasswordAuthentication no
```

## <a name="disable-login-by-the-root-user"></a>Zakázat přihlášení uživatele root

Následující osvědčené postupy Linux, `root` uživatel by měl být nikdy přihlášení k přes SSH nebo pomocí `sudo su`.  Všechny příkazy, které vyžadují oprávnění na úrovni kořenového by měl být vždy spustit `sudo` příkaz, který protokoluje všechny akce pro budoucí auditování.  Zakázání `root` uživatele přihlásit se pomocí protokolu SSH je zabezpečení nejlepší postupy krok, které zajišťuje, aby jenom Autorizovaní uživatelé smějí SSH.

```bash
PermitRootLogin no
```

## <a name="allowed-groups-list"></a>Skupiny seznamu povolených aplikací

SSH nabízí metody omezení uživatelů a skupin, které jsou povolené nebo zakázané přihlásit se přes protokol SSH.  Tato funkce používá seznamy schválí nebo zamítne konkrétní uživatele a skupiny z přihlášení.  Nastavení skupiny kolečka `AllowGroups` seznamu omezuje schválené přihlášení přes protokol SSH pouze uživatelské účty, které jsou ve skupině kolečka.

```bash
AllowGroups wheel
```

## <a name="allowed-users-list"></a>Seznam povolených uživatelů

Omezení přihlášení SSH jenom uživatelům je konkrétnější způsob, jak provést stejný úloha, která `AllowGroups` je.  

```bash
AllowUsers ahmet ralph
```

## <a name="disable-ssh-protocol-version-1"></a>Zakázat protokol SSH verze 1

Protokol SSH verze 1 je nezabezpečené a by mělo být zakázáno.  Protokol SSH verze 2 je aktuální verze, která nabízí bezpečný způsob SSH na váš server.  Zakázání SSH verze 1 odmítne všechny SSH klienty, kteří se pokoušejí navázat spojení se serverem SSH pomocí SSH verze 1.  Připojení k serveru SSH jsou povoleny pouze připojení SSH verze 2.

```bash
Protocol 2
```

## <a name="minimum-key-bits"></a>Minimální klíče služby bits

Následující osvědčené postupy zabezpečení přihlášení SSH heslo jsou zakázána a který se má použít k ověření serveru SSH jsou povoleny pouze klíče SSH.  Tyto klíče SSH lze vytvořit pomocí jinou délku klíče, měřen v bitech.  Osvědčené postupy stavy, zda jsou klíče 2048 bitů minimální přijatelnou síly klíče.  Klíče menší než 2 048 bitů, teoreticky může být poškozený.  Nastavení `ServerKeyBits` k `2048` umožňuje všechna připojení pomocí klíče 2 048 bitů nebo větší a Odepřít připojení menší než 2 048 bitů.

```bash
ServerKeyBits 2048
```

## <a name="disconnect-idle-users"></a>Odpojit neaktivní uživatele

SSH má možnost Odpojit uživatele, kteří mají otevřená připojení, které zůstaly déle než nastavit dobu v sekundách nečinnosti.  Udržování otevřených relací pouze na uživatele, kteří jsou aktivní omezuje zranitelnost virtuálního počítače systému Linux.

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="restart-sshd"></a>Restartujte SSHD

Chcete-li povolit nastavení v `/etc/ssh/sshd_config` restartujte SSHD procesu, který restartuje SSH server.  Bez ztráty otevřít relaci SSH zůstanou otevřené okno terminálu, který používáte pro restartování serveru SSH.  K otestování nové SSH nastavení serveru použijte druhý terminálu okně nebo záložce.  Použití samostatných terminál k testování připojení SSH můžete přejít zpět a provést další změny `/etc/ssh/sshd_config` v první terminálu, bez uzamknutí SSHD změnou ukončování řádků.  

### <a name="on-redhat-centos-and-fedora"></a>Red Hat, Centos a Fedora

```bash
service sshd restart
```

### <a name="on-debian--ubuntu"></a>Na Debian a Ubuntu

```bash
service ssh restart
```

## <a name="reset-sshd-using-azure-reset-access"></a>Resetování SSHD pomocí Azure resetování access

Pokud jste se z narušující změně uzamčen SSHD konfigurace můžete použít rozšíření přístup k virtuálnímu počítači Azure resetování konfigurace SSHD zpět do původní konfigurace.

Nahraďte všechny názvy příklad vlastními.

```azurecli
azure vm reset-access \
--resource-group myResourceGroup \
--name myVM \
--reset-ssh
```

## <a name="install-fail2ban"></a>Nainstalujte Fail2ban

Důrazně doporučujeme při instalaci a nastavení aplikace s otevřeným zdrojem Fail2ban, která blokuje opakovaných pokusech o přihlášení k virtuálním počítačům s Linuxem přes protokol SSH pomocí hrubou silou.  Fail2ban protokoly opakovaných neúspěšných pokusech o přihlášení přes SSH a pak vytvoří pravidla brány firewall pro blokování IP adresu, která jsou na pokusy pocházející z.

* [Fail2ban domovské stránky](http://www.fail2ban.org/wiki/index.php/Main_Page)

## <a name="next-steps"></a>Další kroky

Teď, když jste nakonfigurovali a uzamčené serverem SSH na Linux virtuálního počítače neexistují další bezpečnostní osvědčené postupy, které můžete provést.  

* [Spravovat uživatele, SSH a zkontrolujte nebo opravte disky na virtuálních počítačích Azure Linux pomocí rozšíření VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [Šifrování disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [Přístup a zabezpečení v šablonách Azure Resource Manager](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
