---
title: "hesla aaaDisable SSH na Linux virtuálního počítače tak, že nakonfigurujete SSHD | Microsoft Docs"
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
ms.openlocfilehash: fb67b2f5b8b3bf2ba214858940b04f2ea9013fb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Zakázat konfigurací SSHD hesla SSH na Linux virtuálního počítače
Tento článek se zaměřuje na toolock dolů hello přihlášení zabezpečení virtuálním počítačům s Linuxem.  Při otevření hello portu SSH 22 toohello world robotů spustit při toologin podle účelem uhodnutí hesla.  Co uděláme v tomto článku je zakázat přihlášení hesla přes SSH.  Úplně odebráním hello možnost vynutit toouse hesla chráníme hello virtuálního počítače s Linuxem z tohoto typu hrubou útoku.  Hello přidat bonusové je nakonfigurujeme Linux SSHD tooonly povolit přihlášení prostřednictvím veřejné a privátní klíče SSH, zdaleka hello nejbezpečnější způsob toologin tooLinux.  Hello možných kombinací je by vyžadovaly tooguess hello privátní klíč je enormním způsobem a proto nedoporučuje robotů ze i bothering tootry toobrute force SSH klíče.

## <a name="goals"></a>Cíle
* Konfigurace SSHD toodisallow:
  * Heslo přihlášení
  * Přihlášení uživatele root
  * Ověřování výzvy a odezvy
* Konfigurace SSHD tooallow:
  * pouze klíče přihlášení SSH
* Restartujte SSHD, avšak zůstane přihlášen
* Konfigurace nového SSHD testovací hello

## <a name="introduction"></a>Úvod
[SSH definované](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD je hello SSH serveru, který běží na hello virtuálního počítače s Linuxem.  SSH je klienta, která se spouští z prostředí na pracovní stanici MacBook nebo Linux.  SSH je také protokol hello používá toosecure a zašifrování komunikace hello mezi hello virtuálního počítače s Linuxem a pracovní stanice.

Tento článek je velmi důležité tookeep jeden virtuální počítač s Linuxem otevřené pro celý hello provede tooyour přihlášení.  Z tohoto důvodu jsme se otevře dvěma terminály a toohello SSH virtuálního počítače s Linuxem z obou z nich.  Jsme použije hello první terminálu toomake hello změny tooSSHDs konfigurační soubor a restartujte službu SSHD hello.  Použijeme hello druhý terminálu tootest těch, které změní po restartování služby hello.  Protože jsme zakazování SSH hesla a program předávající výhradně na klíče SSH, pokud nejsou správné klíče SSH a zavřete toohello hello připojení virtuálních počítačů, hello virtuálních počítačů bude trvale uzamčena a nebude možné toologin tooit ho budete vyžadovat toobe odstraněno a znovu vytvořeno.

## <a name="prerequisites"></a>Požadavky
* [Vytvoření klíčů SSH na Linuxu a Macu pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Účet Azure
  * [registrace bezplatné zkušební verze](https://azure.microsoft.com/pricing/free-trial/)
  * [Azure Portal](http://portal.azure.com)
* Virtuální počítač s Linuxem spuštěné v azure
* SSH veřejné a privátní klíče RSA ve`~/.ssh/`
* Veřejný klíč SSH v `~/.ssh/authorized_keys` na hello virtuálního počítače s Linuxem
* Oprávnění Sudo v hello virtuálních počítačů
* Port 22, otevřete

## <a name="quick-commands"></a>Rychlé příkazy
*Po ostřílené Linux správci, kteří chtějí právě hello TLDR verze Začněte zde.  Pro všechny ostatní, který chce hello tuto část přeskočit podrobné vysvětlení a ukázce.*

```bash
sudo vim /etc/ssh/sshd_config
```

Upravte soubor konfigurace hello následujícím způsobem:

```sh
# Change PasswordAuthentication toothis:
PasswordAuthentication no

# Change PubkeyAuthentication toothis:
PubkeyAuthentication yes

# Change PermitRootLogin toothis:
PermitRootLogin no

# Change ChallengeResponseAuthentication toothis:
ChallengeResponseAuthentication no
```

Restartujte službu SSHD hello. Na základě Debian distribucích:

```bash
sudo service ssh restart
```

Na základě Red Hat distribucích:

```bash
sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Podrobné procházení prostřednictvím
Přihlášení toohello virtuálního počítače s Linuxem v terminálu 1 (T1).  Přihlášení toohello virtuálního počítače s Linuxem v terminálu 2 (T2).

V čase T2 přidáme tooedit hello SSHD konfigurační soubor.  

```bash
sudo vim /etc/ssh/sshd_config
```

Zde jsme upravit právě hello nastavení toodisable hesla a povolit přihlášení klíče SSH.  Existuje mnoho nastavení v tomto souboru, měli byste prozkoumat a toomake Linux & SSH jako zabezpečené, jako je třeba změnit.

#### <a name="disable-password-logins"></a>Zakázat přihlášení heslo

```sh
# Change PasswordAuthentication toothis:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Povolit ověření veřejného klíče

```sh
# Change PubkeyAuthentication toothis:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Zakázat kořenové přihlášení

```sh
# Change PermitRootLogin toothis:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Zakázat ověřování výzvy a odezvy
```sh
# Change ChallengeResponseAuthentication toothis:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Restartujte SSHD
Z prostředí hello T1 ověřte, že jste stále přihlášení.  To je důležité, takže můžete není zamknout z virtuálního počítače Pokud klíče SSH nejsou správné, vzhledem k tomu, že hesla jsou nyní zakázány.  Pokud jsou všechna nastavení nesprávné na virtuálním počítačům s Linuxem můžete použít T1 toofix sshd_config, které budete přihlášeni stále a SSH bude udržovat připojení hello zachování připojení při hello SSHD service restartujte.

Z T2 spustit:

##### <a name="on-hello-debian-family"></a>Na hello Debian rodiny
```bash
sudo service ssh restart
```

##### <a name="on-hello-redhat-family"></a>Na hello rodiny RedHat
```bash
sudo service sshd restart
```

Hesla jsou nyní zakázány na vašem virtuálním počítači, ochrana před hrubou silou pokusů o přihlášení heslo.  Pouze SSH klíče povolené bude možné toologin rychlejší a mnohem bezpečnější.

