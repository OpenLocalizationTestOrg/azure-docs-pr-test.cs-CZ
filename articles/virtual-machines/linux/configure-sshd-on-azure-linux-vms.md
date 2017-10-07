---
title: "aaaConfigure SSHD ve virtuálních počítačích Azure Linux | Microsoft Docs"
description: "Konfigurace SSHD osvědčené postupy zabezpečení a toolockdown SSH tooAzure virtuální počítače s Linuxem."
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
ms.openlocfilehash: c2361be7199a24b129c06acfc899dd32f6e1d6fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-sshd-on-azure-linux-vms"></a>Konfigurace SSHD na virtuálních počítačích Azure Linux

Tento článek ukazuje, jak toolockdown hello SSH Server Linux, tooprovide osvědčené postupy zabezpečení a také toospeed až hello procesu přihlášení SSH pomocí klíče SSH místo hesla.  uzamčení toofurther SSHD přidáme toodisable hello kořenového uživatele nebudou moct toologin omezit hello uživatele, které jsou povoleny toologin prostřednictvím seznam schválených skupiny zakázat protokol SSH verze 1, nastavit minimální klíče bit a nakonfigurovat automatické odhlášení uživatelů nečinnosti.  Hello požadavky v tomto článku jsou: účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a [SSH soubory veřejného a privátního klíče](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-commands"></a>Rychlé příkazy

Konfigurace `/etc/ssh/sshd_config` s hello následující nastavení:

### <a name="disable-password-logins"></a>Zakázat přihlášení heslo

```bash
PasswordAuthentication no
```

### <a name="disable-login-by-hello-root-user"></a>Zakázat přihlášení uživatele root hello

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

SSHD je hello SSH serveru, který běží na hello virtuálního počítače s Linuxem.  SSH je klient spuštěný v systému Windows z prostředí na vaše MacBook Linux pracovní stanice, nebo Bash.  SSH je také protokol hello používá toosecure a zašifrování komunikace hello mezi hello vytvoření SSH také virtuální privátní sítě (virtuální privátní sítě) virtuálního počítače s Linuxem a pracovní stanice.

Tento článek je velmi důležité tookeep tooyour jeden přihlášení pro celý návodu hello otevřít virtuálního počítače s Linuxem.  Po vytvoření připojení SSH zůstane jako otevřít relaci tak dlouho, dokud není okno hello zavřít.  Jeden Terminálové přihlášení, umožní pro toohello toobe provedené změny SSHD service bez uzamknutí Pokud narušující změně.  Pokud získat uzamčen virtuálním počítačům s Linuxem s konfigurací porušený SSHD, Azure nabízí možnost tooreset hello porušený SSHD konfigurace s hello [rozšíření pro přístup virtuálních počítačů Azure](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Z tohoto důvodu jsme otevřít dvěma terminály a toohello SSH virtuálního počítače s Linuxem z obou z nich.  Jsme použijte hello první terminálu toomake hello změny tooSSHDs konfigurační soubor a restartujte službu SSHD hello.  Používáme hello druhý terminálu tootest těch, které změní po restartování služby hello.  Protože jsme zakazování SSH hesla a program předávající výhradně na klíče SSH, pokud nejsou správné klíče SSH a zavřete toohello hello připojení virtuálních počítačů, hello virtuálních počítačů bude trvale uzamčena a nebude možné toologin tooit ho budete vyžadovat toobe odstraněno a znovu vytvořeno.

## <a name="disable-password-logins"></a>Zakázat přihlášení heslo

Hello toosecure nejrychlejší způsob, jak jste virtuální počítač s Linuxem je toodisable heslo přihlášení.  Pokud jsou povolené přihlášení heslo, robotů procházení webové hello okamžitě začít pokus toobrute vynutit odhad hello heslo pro váš virtuální počítač s Linuxem pomocí protokolu SSH.  Zakázáním heslo přihlášení úplně, povolí hello SSH serveru tooignore všech pokusů o přihlášení heslo.

```bash
PasswordAuthentication no
```

## <a name="disable-login-by-hello-root-user"></a>Zakázat přihlášení uživatele root hello

Následující osvědčené postupy Linux hello `root` uživatel by měl být nikdy přihlášení k přes SSH nebo pomocí `sudo su`.  Všechny příkazy, které vyžadují oprávnění na úrovni kořenového by měl být vždy spustit prostřednictvím hello `sudo` příkaz, který protokoluje všechny akce pro budoucí auditování.  Zakázání hello `root` zabezpečení osvědčené postupy krok zajistí jenom Autorizovaní uživatelé smějí tooSSH je uživatel přihlásit se pomocí protokolu SSH.

```bash
PermitRootLogin no
```

## <a name="allowed-groups-list"></a>Skupiny seznamu povolených aplikací

SSH nabízí metody omezení uživatelů a skupin, které jsou povolené nebo zakázané přihlásit se přes protokol SSH.  Tato funkce používá tooapprove seznamy nebo odepřít konkrétní uživatele a skupiny ze přihlášení.  Nastavení hello wheel skupiny toohello `AllowGroups` seznamu omezuje schválené přihlášení přes SSH toojust uživatelské účty, které jsou ve skupině wheel hello.

```bash
AllowGroups wheel
```

## <a name="allowed-users-list"></a>Seznam povolených uživatelů

Omezit uživatelům toojust přihlášení SSH je více určitým způsobem tooaccomplish hello stejná úloha, která `AllowGroups` je.  

```bash
AllowUsers ahmet ralph
```

## <a name="disable-ssh-protocol-version-1"></a>Zakázat protokol SSH verze 1

Protokol SSH verze 1 je nezabezpečené a by mělo být zakázáno.  Protokol SSH verze 2 je hello aktuální verzi, která nabízí bezpečný tooSSH tooyour serveru.  Zakázání SSH verze 1 odmítne všechny SSH klienty, kteří se pokoušejí tooestablish připojení k serveru SSH hello pomocí SSH verze 1.  Pouze verze 2 připojení SSH. jsou povolené toonegotiate připojení k serveru SSH hello.

```bash
Protocol 2
```

## <a name="minimum-key-bits"></a>Minimální klíče služby bits

Následující osvědčené postupy zabezpečení jsou zakázané heslo SSH přihlášení a toobe použít s serverem SSH hello tooauthenticate jsou povoleny pouze klíče SSH.  Tyto klíče SSH lze vytvořit pomocí jinou délku klíče, měřen v bitech.  Osvědčené postupy stavy, zda jsou klíče 2048 bitů hello minimální přijatelnou síly klíče.  Klíče menší než 2 048 bitů, teoreticky může být poškozený.  Nastavení hello `ServerKeyBits` příliš`2048` umožňuje všechna připojení pomocí klíče 2 048 bitů nebo větší a Odepřít připojení menší než 2 048 bitů.

```bash
ServerKeyBits 2048
```

## <a name="disconnect-idle-users"></a>Odpojit neaktivní uživatele

SSH má hello možnost toodisconnect uživatele, kteří mají otevřená připojení, které zůstaly déle než nastavit dobu v sekundách nečinnosti.  Udržování otevřených relací tooonly uživatelům, kteří jsou aktivní omezení hello úniku hello virtuálního počítače s Linuxem.

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="restart-sshd"></a>Restartujte SSHD

nastavení hello tooenable v `/etc/ssh/sshd_config` restartujte hello SSHD procesu, který restartuje hello SSH server.  okno terminálu Hello používáte toorestart hello SSH server zůstanou otevřené bez ztráty hello otevřít relaci SSH.  Nastavení nové SSH serveru tootest hello použít druhý okno terminálu nebo kartě.  Pomocí samostatných terminálu tootest hello připojení SSH umožňuje toogo zpět a zkontrolujte další změny toohello `/etc/ssh/sshd_config` v první terminálu hello bez uzamknutí SSHD změnou ukončování řádků.  

### <a name="on-redhat-centos-and-fedora"></a>Red Hat, Centos a Fedora

```bash
service sshd restart
```

### <a name="on-debian--ubuntu"></a>Na Debian a Ubuntu

```bash
service ssh restart
```

## <a name="reset-sshd-using-azure-reset-access"></a>Resetování SSHD pomocí Azure resetování access

Pokud jsou uzamčen z konfigurace SSHD toohello narušující změny můžete hello virtuální počítač Azure – rozšíření pro přístup tooreset hello SSHD back toohello původní konfigurace.

Nahraďte všechny názvy příklad vlastními.

```azurecli
azure vm reset-access \
--resource-group myResourceGroup \
--name myVM \
--reset-ssh
```

## <a name="install-fail2ban"></a>Nainstalujte Fail2ban

Důrazně doporučujeme tooinstall a instalační program hello s otevřeným zdrojem aplikace Fail2ban, které bloky opakované pokusy o toologin tooyour virtuálního počítače s Linuxem přes protokol SSH pomocí hrubou silou.  Protokoly Fail2ban opakované pokusy o toologin převzal SSH a poté vytvoří pravidla brány firewall tooblock hello IP adresu, která jsou pokusy o hello pocházející z.

* [Fail2ban domovské stránky](http://www.fail2ban.org/wiki/index.php/Main_Page)

## <a name="next-steps"></a>Další kroky

Teď, když jste nakonfigurovali a uzamčené hello SSH serveru na virtuálním počítačům s Linuxem existují další bezpečnostní osvědčené postupy, které můžete provést.  

* [Spravovat uživatele, SSH a zkontrolujte nebo hello oprava disky na virtuální počítače Azure s Linuxem pomocí rozšíření VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [Šifrování disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure hello](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [Přístup a zabezpečení v šablonách Azure Resource Manager](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
