---
title: "Zakažte hesla SSH na Linux virtuálního počítače tak, že konfigurace SSHD | Microsoft Docs"
description: "Zakázáním heslo přihlášení SSH Zabezpečte virtuálním počítačům s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 46137640-a7d2-40e5-a1e9-9effef7eb190
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/26/2016
ms.author: v-livech
ms.openlocfilehash: dc45a1cdce29cef061acc5c7e5b15d9d89265cd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Zakázat konfigurací SSHD hesla SSH na Linux virtuálního počítače
Tento článek se zaměřuje na zamknutí přihlášení zabezpečení virtuálním počítačům s Linuxem.  Jakmile SSH port 22, je otevřené pro spuštění robotů world pokusu o přihlášení pomocí účelem uhodnutí hesla.  Co uděláme v tomto článku je zakázat přihlášení hesla přes SSH.  Možnost používat hesla odebráním úplně jsme chránit virtuálního počítače s Linuxem z tohoto typu útoku hrubou silou.  Přidání bonusové je že nakonfigurujeme SSHD Linux a Povolit jenom přihlášení prostřednictvím veřejné a privátní klíče SSH, zdaleka nejbezpečnější způsob, jak přihlášení do systému Linux.  Možné kombinace by vyžadovaly tak snadno uhodnout privátní klíč je enormním způsobem a proto nedoporučuje robotů ze i bothering pokusí hrubou silou klíče SSH.

## <a name="goals"></a>Cíle
* Nakonfigurujte SSHD tak, aby zakázala:
  * Heslo přihlášení
  * Přihlášení uživatele root
  * Ověřování výzvy a odezvy
* Nakonfigurujte SSHD umožňuje:
  * pouze klíče přihlášení SSH
* Restartujte SSHD, avšak zůstane přihlášen
* Otestovat novou konfiguraci SSHD

## <a name="introduction"></a>Úvod
[SSH definované](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD je SSH Server, který běží na virtuální počítač s Linuxem.  SSH je klienta, která se spouští z prostředí na pracovní stanici MacBook nebo Linux.  SSH je také protokol použitý k zabezpečení a zašifrování komunikace mezi pracovní stanice a virtuálního počítače s Linuxem.

V tomto článku je velmi důležité udržovat jeden přihlášení k virtuálním počítačům s Linuxem otevřete pro celý procházení prostřednictvím.  Z tohoto důvodu jsme se otevře dvě terminály a SSH pro virtuální počítač s Linuxem z obou z nich.  První terminálu použijeme k provedení změn SSHDs konfigurační soubor a restartujte službu SSHD.  Druhý terminálu použijeme k testování tyto změny po restartování služby.  Protože jsme zakazování SSH hesla a program předávající výhradně na klíče SSH, pokud nejsou správné klíče SSH a zavřete připojení k virtuálnímu počítači, virtuální počítač bude trvale uzamčena a nebude moci přihlásit k němu nutnosti ho odstranit a znovu vytvoří.

## <a name="prerequisites"></a>Požadavky
* [Vytvoření klíčů SSH na Linuxu a Macu pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Účet Azure
  * [registrace bezplatné zkušební verze](https://azure.microsoft.com/pricing/free-trial/)
  * [Azure Portal](http://portal.azure.com)
* Virtuální počítač s Linuxem spuštěné v azure
* SSH veřejné a privátní klíče RSA ve`~/.ssh/`
* Veřejný klíč SSH v `~/.ssh/authorized_keys` na virtuální počítač s Linuxem
* Sudo práva na virtuálním počítači
* Port 22, otevřete

## <a name="quick-commands"></a>Rychlé příkazy
*Po ostřílené Linux správci, kteří právě má verzi TLDR Začněte zde.  Pro všechny ostatní, který chce tuto část přeskočit podrobné vysvětlení a ukázce.*

```bash
sudo vim /etc/ssh/sshd_config
```

Upravte konfigurační soubor následujícím způsobem:

```sh
# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

Restartujte službu SSHD. Na základě Debian distribucích:

```bash
sudo service ssh restart
```

Na základě Red Hat distribucích:

```bash
sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Podrobné procházení prostřednictvím
Přihlášení k virtuálního počítače s Linuxem v terminálu 1 (T1).  Přihlášení k virtuálního počítače s Linuxem v terminálu 2 (T2).

V čase T2 přidáme upravovat soubor konfigurace SSHD.  

```bash
sudo vim /etc/ssh/sshd_config
```

Odsud upravíme nastavení hesla zakázání a povolení přihlášení klíče SSH.  Existuje mnoho nastavení v tento soubor, který by měl prozkoumat a změňte zabezpečit Linux & SSH jako podle potřeby.

#### <a name="disable-password-logins"></a>Zakázat přihlášení heslo

```sh
# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Povolit ověření veřejného klíče

```sh
# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Zakázat kořenové přihlášení

```sh
# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Zakázat ověřování výzvy a odezvy
```sh
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Restartujte SSHD
Z prostředí T1 ověřte, že jste stále přihlášení.  To je důležité, takže můžete není zamknout z virtuálního počítače Pokud klíče SSH nejsou správné, vzhledem k tomu, že hesla jsou nyní zakázány.  Pokud všechna nastavení jsou nesprávná na virtuálním počítačům s Linuxem T1 můžete opravit sshd_config budete přihlášeni stále a SSH ponechá připojení aktivní během SSHD službu restartujte.

Z T2 spustit:

##### <a name="on-the-debian-family"></a>V Debian řady
```bash
sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>V dané rodině RedHat
```bash
sudo service sshd restart
```

Hesla jsou nyní zakázány na vašem virtuálním počítači, ochrana před hrubou silou pokusů o přihlášení heslo.  Pouze SSH klíče povolená, že nebudete moci přihlásit rychlejší a mnohem bezpečnější.

