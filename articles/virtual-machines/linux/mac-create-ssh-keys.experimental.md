---
title: "dvojice klíčů aaaCreate SSH pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Vytvořte bezpečně dvojici veřejného a privátního klíče SSH pro virtuální počítače Azure s Linuxem."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: rasquill
ms.openlocfilehash: c4c7cec77c9b48295f2a28c8179b30a4dc38a555
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a>Vytvoření páru veřejného a privátního klíče SSH pro virtuální počítače s Linuxem

Tento článek ukazuje, jak toogenerate protocol verze 2 RSA veřejné a privátní klíč SSH souboru toouse dvojice virtuálních počítačů Linux.  S dvojici klíčů SSH můžete vytvořit virtuální počítače v Azure, která výchozí toousing klíče SSH k ověření, což eliminuje potřebu hello toolog hesla v.  Hesla můžete uhádnout a otevřete virtuální počítače až toorelentless hrubou silou pokusy o tooguess heslo. Virtuální počítače vytvořené pomocí šablon Azure nebo hello `azure-cli` může obsahovat veřejný klíč SSH jako součást nasazení hello, odebrání krok konfigurace nasazení post zakázání heslo přihlášení SSH.

## <a name="quick-commands"></a>Rychlé příkazy

Spuštěním následujících příkazů z prostředí Bash, nahraďte hello příklady vlastní volby hello.

Souboru veřejného klíče SSH se ve výchozím nastavení vytvoří v `~/.ssh/id_rsa.pub`. Po zobrazení výzvy pomocí hello následující příkaz, byste měli vytvořit toosecure "tzn. přístupové heslo soukromého klíče. (heslo hello je, že heslo použít tooencrypt privátní klíč.)

```bash
ssh-keygen -t rsa -b 2048 
```

Přidejte hello nově vytvořený klíč příliš`ssh-agent`:

```bash
ssh-add ~/.ssh/id_rsa
```

> [!IMPORTANT] 
> Hello výše pracovní příkazy v operačních systémech Linux téměř všechny distribuce, ale nemusí pracovat v kontejnerech, protože hello prostředí může být výrazně omezené. 

## <a name="detailed-walkthrough"></a>Podrobný postup

Použití veřejného a privátního klíče SSH je hello nejjednodušší způsob, jak toolog v tooyour servery se systémem Linux. [Šifrování veřejného klíče](https://en.wikipedia.org/wiki/Public-key_cryptography) poskytuje mnohem bezpečnější toolog způsob, jak v tooyour Linuxem nebo BSD v Azure než používání hesel, které můžou být hrubou silou mnohem snadněji.

> [!IMPORTANT]
> Veřejný klíč je možné s kýmkoli sdílet. Pouze vy (nebo vaše místní infrastruktura zabezpečení) ale máte privátní klíč.  musí mít privátní klíč SSH Hello [velmi zabezpečeného hesla](https://www.xkcd.com/936/) (zdroj:[xkcd.com](https://xkcd.com)) toosafeguard ho.  Toto heslo je právě tooaccess hello privátní klíč SSH a **není** hello heslo uživatelského účtu.  Když přidáte klíč SSH tooyour heslo, zašifruje hello privátního klíče pomocí standardu AES 128-bit, tak, aby hello privátní klíč nelze bez hesla toodecrypt hello ho.  Pokud útočník stole privátní klíč a že klíč neměl heslo, nebudou moct toouse to privátního klíče toolog v tooany servery, které mají hello odpovídající veřejný klíč.  Pokud je privátní klíč chráněný heslem, útočník ho nemůže použít, což znamená další úroveň zabezpečení pro vaši infrastrukturu v Azure.

Tento článek vytvoří protokol SSH verze 2 RSA soubory veřejného a privátního klíče, které se doporučují pro nasazení na hello Resource Manager.  *SSH-rsa* klíče, je nutné hello [portál](https://portal.azure.com) pro classic a nasazení Resource Manager.

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a>Zakázání hesel SSH použitím klíčů SSH

Azure vyžaduje minimálně 2048bitové veřejné a privátní klíče ve formátu ssh-rsa. použití klíče toocreate hello `ssh-keygen`, který zeptá na několik otázek a pak zapíše privátní klíč a odpovídající veřejný klíč. Když je vytvořen virtuální počítač Azure, veřejný klíč hello zkopírován příliš`~/.ssh/authorized_keys`.  Klíče SSH v `~/.ssh/authorized_keys` jsou použité toochallenge hello toomatch hello odpovídající privátní klíč klienta na připojení přihlášení SSH.  Při vytváření virtuálního počítače Azure Linux pomocí klíče SSH pro ověřování Azure nakonfiguruje hello SSHD toonot serveru povolit přihlášení heslo, pouze klíče SSH.  Proto tak, že vytvoříte virtuální počítače Azure s Linuxem pomocí klíče SSH, můžete pomoct zabezpečené hello nasazení virtuálního počítače a uložit sami hello typické konfiguraci po nasazení krok zakázání hesla v konfiguračním souboru sshd_config hello.

## <a name="using-ssh-keygen"></a>Použití příkazu ssh-keygen

Tento příkaz vytvoří heslo zabezpečené (šifrovaný) pár klíčů SSH, které se používá RSA 2048 bitů a je označeno jako komentář tooeasily identifikaci.  

SSH klíče jsou ve výchozím nastavení uchovávány v hello `~/.ssh` adresáře.  Pokud jste `~/.ssh` adresář, hello `ssh-keygen` příkaz ji vytvoří, můžete s hello správná oprávnění.

```bash
ssh-keygen \
-t rsa \
-b 2048 
```

*Vysvětlení příkazu*

`ssh-keygen`= hello program použitý toocreate hello klíče

`-t rsa`= Typ klíče toocreate, což je formát RSA hello [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= bity klíče hello


## <a name="classic-portal-and-x509-certs"></a>Klasický portál a certifikáty X.509

Pokud používáte hello Azure [portálu classic](https://manage.windowsazure.com/), vyžaduje certifikátů X.509 pro hello klíče SSH.  Jiné typy veřejných klíčů SSH nejsou povolené, *musí* jít o certifikáty X.509.

toocreate certifikát X.509 z existující soukromý klíč SSH-RSA:

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a>Klasické nasazení pomocí `asm`

Pokud používáte hello klasického nasazení modelu (Správa služby Azure CLI `asm`), můžete použít veřejný klíč SSH-RSA nebo RFC4716 naformátovaný klíče v **.pem** kontejneru.  veřejný klíč SSH-RSA Hello je, co byl vytvořen dříve v této konfigurace pomocí článku `ssh-keygen`.

toocreate RFC4716 naformátovaný klíče z existující veřejný klíč SSH:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Příklad ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which toosave hello key (/home/ahmet/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ahmet/.ssh/id_rsa.
Your public key has been saved in /home/ahmet/.ssh/id_rsa.pub.
hello key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
hello keys randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Uložené soubory klíčů:

`Enter file in which toosave hello key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

Hello název páru klíčů pro tento článek.  Pár klíčů s názvem **id_rsa** je výchozí hello a některé nástroje by se dalo očekávat hello **id_rsa** soubor privátního klíče jmenovat, takže je vhodné. Hello directory `~/.ssh/` hello výchozí umístění pro páry klíčů SSH a konfigurační soubor SSH hello.  Pokud není zadaný s úplnou cestou `ssh-keygen` bude vytvoření hello klíčů v hello aktuální pracovní adresář, není hello výchozí `~/.ssh`.

Seznam hello `~/.ssh` adresáře.

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

Heslo klíče:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`označuje tooa heslo použité tooencrypt hello privátní klíč jako "přístupové heslo."  Je *důrazně* doporučená tooadd páry klíčů tooyour přístupové heslo. Bez přístupové heslo chránící hello privátní klíč každý, kdo má soubor klíče hello ho může použít toolog tooany server, který má odpovídající veřejný klíč hello. Přidání přístupového hesla o nabízí další ochranu v případě, že někdo dokáže toogain přístup tooyour soubor privátního klíče, budete čas toochange hello klíče používají tooauthenticate můžete.

## <a name="using-ssh-agent-toostore-your-private-key-password"></a>Pomocí ssh-agent toostore hesla k soukromému klíči

přístupové heslo souboru tooavoid psát svůj privátní klíč v každém přihlašování přes SSH můžete použít `ssh-agent` toocache heslo soubor privátního klíče. Pokud používáte algoritmu Mac, hello klíčenky v OSX bezpečně uloží hesla k privátním klíčům hello při vyvolání `ssh-agent`.

Ověřte a použít `ssh-agent` a `ssh-add` tooinform hello SSH systému o soubory klíčů hello tak, aby heslo hello nebudete potřebovat toobe používá interaktivně.

```bash
eval "$(ssh-agent -s)"
```

Nyní přidejte privátní klíč hello příliš`ssh-agent` pomocí příkazu hello `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

heslo soukromého klíče Hello jsou teď uložená v `ssh-agent`.

## <a name="using-ssh-copy-id-tooinstall-hello-new-key"></a>Pomocí `ssh-copy-id` tooinstall hello nový klíč
Pokud jste již vytvořili virtuální počítač můžete nainstalovat hello nové SSH veřejného klíče tooyour virtuálního počítače s Linuxem s hello následující příkaz, nahraďte hello virtuálních počítačů uživatelské jméno a adresa serveru hello vlastními hodnotami:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>Vytvoření a nakonfigurování konfiguračního souboru SSH

Je nejlepší postup toocreate a nakonfigurovat `~/.ssh/config` souboru toospeed až přihlašování a pro optimalizaci vaše chování klienta SSH.

Hello následující příklad ukazuje standardní konfigurace.

### <a name="create-hello-file"></a>Vytvoření souboru hello

```bash
touch ~/.ssh/config
```

### <a name="edit-hello-file-tooadd-hello-new-ssh-configuration"></a>Úpravy hello souboru tooadd hello novou konfiguraci SSH:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Příklad souboru `~/.ssh/config`:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Tato poskytuje konfigurace SSH můžete částech pro každý server tooenable každý toohave svůj vlastní vyhrazený pár klíčů. Hello výchozí nastavení (`Host *`) jsou pro všechny hostitele, kteří neodpovídají žádné hello konkrétních hostitelů vyšší nahoru v konfiguračním souboru hello.

### <a name="config-file-explained"></a>Vysvětlení konfiguračního souboru

`Host`= Název hello hello hostitele volaného na hello terminálu.  `ssh fedora22`informuje `SSH` toouse hello hodnoty v bloku nastavení hello s názvem bez přípony `Host fedora22` Poznámka: hostitele může být libovolný popisek, který je smysl pro vaše použití a nepředstavuje skutečný název hostitele hello žádného serveru.

`Hostname 102.160.203.241`= hello IP adresu nebo název DNS pro server hello přistupuje.

`User ahmet`= toouse účet hello vzdáleného uživatele při přihlášení toohello serveru.

`PubKeyAuthentication yes`= říká protokolu SSH, které mají být toouse toolog klíče SSH v.

`IdentityFile /home/ahmet/.ssh/id_id_rsa`= hello SSH privátní klíč a odpovídající toouse veřejného klíče pro ověřování.

## <a name="ssh-into-linux-without-a-password"></a>Přihlášení do Linuxu bez hesla pomocí protokolu SSH

Nyní když máte dvojici klíčů SSH a nakonfigurovaný konfigurační soubor SSH, jsou možné toolog v tooyour virtuálního počítače s Linuxem rychle a bezpečně. Hello poprvé přihlašujete tooa serveru pomocí SSH klíče hello příkazové řádky můžete pro hello přístupové heslo pro tento soubor klíče.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Vysvětlení příkazu

Když `ssh fedora22` se spustí SSH nejdříve vyhledá a načte všechna nastavení z hello `Host fedora22` blok a pak načte všechna hello zbývající nastavení z posledního bloku hello `Host *`.

## <a name="next-steps"></a>Další kroky

Dále si je toocreate virtuální počítače Azure s Linuxem pomocí hello nové veřejného klíče SSH.  Virtuální počítače Azure, které jsou vytvořeny pomocí veřejného klíče SSH jako hello přihlašovací údaje jsou lépe zabezpečené než virtuální počítače vytvořené pomocí hello výchozí přihlašovací metoda hesla.  Virtuální počítače Azure vytvořené pomocí klíčů SSH jsou ve výchozím nastavení nakonfigurované se zakázaným heslem, aby se vyloučily pokusy o rozluštění hesla (útokem hrubou silou). Pokud potřebujete další pomoc při vytváření dvojici klíčů SSH nebo vyžadovat další certifikáty, například jako pro použití s hello klasický portál, najdete v části [podrobné kroky toocreate páry klíčů SSH a certifikáty](create-ssh-keys-detailed.md).

* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí portálu Azure hello](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure hello](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
