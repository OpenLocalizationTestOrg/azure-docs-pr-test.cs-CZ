---
title: "dvojice klíčů aaaDetailed kroky toocreate SSH pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Přečtěte si informace toocreate další kroky SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem v Azure, společně s konkrétní certifikáty pro použití v odlišných situacích."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 6/28/2017
ms.author: danlep
ms.openlocfilehash: 9ac52ef4dc87e73b9c07ccc323adc955829e2014
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-walk-through-toocreate-an-ssh-key-pair-and-additional-certificates-for-a-linux-vm-in-azure"></a>Podrobné procházení prostřednictvím toocreate dvojici klíčů SSH a další certifikáty pro virtuální počítač s Linuxem v Azure
S dvojici klíčů SSH můžete vytvořit virtuální počítače v Azure, která výchozí toousing klíče SSH k ověření, což eliminuje potřebu hello toolog hesla v. Hesla můžete uhádnout a otevřete virtuální počítače až toorelentless hrubou silou pokusy o tooguess heslo. Virtuální počítače vytvořené pomocí rozhraní příkazového řádku Azure nebo správce prostředků šablony hello může obsahovat veřejný klíč SSH jako součást nasazení hello, odebrání krok konfigurace nasazení post zakázání heslo přihlášení SSH. Tento článek obsahuje podrobné pokyny a další příklady generování certifikátů, například pro použití s virtuálním počítačům s Linuxem. Pokud chcete, aby tooquickly vytvořit a použít dvojici klíčů SSH naleznete v tématu [jak spárujte toocreate veřejné a privátní klíč SSH pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md).

## <a name="understanding-ssh-keys"></a>Principy klíčů SSH

Použití veřejného a privátního klíče SSH je hello nejjednodušší způsob, jak toolog v tooyour servery se systémem Linux. [Šifrování veřejného klíče](https://en.wikipedia.org/wiki/Public-key_cryptography) poskytuje mnohem bezpečnější toolog způsob, jak v tooyour Linuxem nebo BSD v Azure než používání hesel, které můžou být hrubou silou mnohem snadněji.

Veřejný klíč je možné s kýmkoli sdílet. Pouze vy (nebo vaše místní infrastruktura zabezpečení) ale máte privátní klíč.  musí mít privátní klíč SSH Hello [velmi zabezpečeného hesla](https://www.xkcd.com/936/) (zdroj:[xkcd.com](https://xkcd.com)) toosafeguard ho.  Toto heslo je právě tooaccess hello SSH soubor privátního klíče a **není** hello heslo uživatelského účtu.  Když přidáte klíč SSH tooyour heslo, zašifruje hello privátního klíče pomocí standardu AES 128-bit, tak, aby hello privátní klíč nelze bez hesla toodecrypt hello ho.  Pokud útočník stole privátní klíč a že klíč neměl heslo, nebudou moct toouse to privátního klíče toolog v tooany servery, které mají hello odpovídající veřejný klíč.  Pokud je privátní klíč chráněný heslem, útočník ho nemůže použít, což znamená další úroveň zabezpečení pro vaši infrastrukturu v Azure.

Tento článek vytvoří protokol SSH verze 2 pár veřejného a privátního klíče souboru RSA (také odkazované tooas "ssh-rsa" klíče), které se doporučují pro nasazení s Azure Resource Manager. *SSH-rsa* klíče, je nutné hello [portál](https://portal.azure.com) pro classic a nasazení Resource Manager.

## <a name="ssh-keys-use-and-benefits"></a>Použití a výhody klíčů SSH

Azure vyžaduje minimálně 2048bitové protokol SSH verze 2 formátu veřejného a privátního klíče RSA; Hello soubor veřejného klíče má hello `.pub` formátu kontejneru. použití klíče toocreate hello `ssh-keygen`, který zeptá na několik otázek a pak zapíše privátní klíč a odpovídající veřejný klíč. Když je vytvořen virtuální počítač Azure, Azure kopie hello veřejného klíče toohello `~/.ssh/authorized_keys` složky na hello virtuálních počítačů. Klíče SSH v `~/.ssh/authorized_keys` jsou použité toochallenge hello toomatch hello odpovídající privátní klíč klienta na připojení přihlášení SSH.  Při vytváření virtuálního počítače Azure Linux pomocí klíče SSH pro ověřování Azure nakonfiguruje hello SSHD toonot serveru povolit přihlášení heslo, pouze klíče SSH.  Proto tak, že vytvoříte virtuální počítače Azure s Linuxem pomocí klíče SSH, můžete pomoct zabezpečit hello nasazení virtuálního počítače a uložit sami hello typické konfiguraci po nasazení krok zakázání hesla v hello **sshd_config** souboru.

## <a name="using-ssh-keygen"></a>Použití příkazu ssh-keygen

Tento příkaz vytvoří heslo zabezpečené (šifrovaný) pár klíčů SSH, které se používá RSA 2048 bitů a je označeno jako komentář tooeasily identifikaci.  

SSH klíče jsou ve výchozím nastavení uchovávány v hello `~/.ssh` adresáře.  Pokud jste `~/.ssh` adresář, hello `ssh-keygen` příkaz ji vytvoří, můžete s hello správná oprávnění.

```bash
ssh-keygen \
    -t rsa \
    -b 2048 \
    -C "azureuser@myserver" \
    -f ~/.ssh/id_rsa \
    -N mypassword
```

*Vysvětlení příkazu*

`ssh-keygen`= hello program použitý toocreate hello klíče

`-t rsa`= Typ klíče toocreate, což je formát RSA hello [wikipedia][parens na konci](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `) 
 `-b 2048` = bity klíče hello

`-C "azureuser@myserver"`= Komentář identifikaci připojením toohello konec tooeasily hello soubor veřejného klíče.  Za normálních okolností e-mail se používá jako komentář hello ale můžete použít, ať je nejvhodnější pro vaši infrastrukturu.

## <a name="classic-deploy-using-asm"></a>Klasické nasazení pomocí `asm`

Pokud používáte model nasazení classic hello (`asm` režimu v hello rozhraní příkazového řádku), můžete použít veřejný klíč SSH-RSA nebo RFC4716 naformátovaný klíče v kontejneru pem.  veřejný klíč SSH-RSA Hello je, co byl vytvořen dříve v této konfigurace pomocí článku `ssh-keygen`.

toocreate RFC4716 naformátovaný klíče z existující veřejný klíč SSH:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Příklad ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "azureuser@myserver"
Generating public/private rsa key pair.
Enter file in which toosave hello key (/home/azureuser/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/id_rsa.
Your public key has been saved in /home/azureuser/.ssh/id_rsa.pub.
hello key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 azureuser@myserver
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

`Enter file in which toosave hello key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

Hello název páru klíčů pro tento článek.  Pár klíčů s názvem **id_rsa** je výchozí hello a některé nástroje by se dalo očekávat hello **id_rsa** soubor privátního klíče jmenovat, takže je vhodné. Hello directory `~/.ssh/` hello výchozí umístění pro páry klíčů SSH a konfigurační soubor SSH hello.  Pokud není zadaný s úplnou cestou `ssh-keygen` vytvoří hello klíče v hello aktuální pracovní adresář, není hello výchozí `~/.ssh`.

Seznam hello `~/.ssh` adresáře.

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

Heslo klíče:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`označuje tooa heslo pro soubor privátního klíče hello jako "přístupové heslo."  Je *důrazně* doporučená tooadd privátní klíč tooyour heslo. Heslo chránící hello soubor klíče každý, kdo má hello souboru můžete použít bez ho toolog tooany serveru, který má odpovídající veřejný klíč hello. Přidání hesla (heslo) nabízí další ochranu v případě, že někdo dokáže toogain přístup tooyour soubor privátního klíče, získáte čas toochange hello klíče používají tooauthenticate můžete.

## <a name="using-ssh-agent-toostore-your-private-key-password"></a>Pomocí ssh-agent toostore hesla k soukromému klíči

tooavoid zadáním hesla soubor privátního klíče každém přihlašování přes SSH můžete použít `ssh-agent` toocache heslo soubor privátního klíče. Pokud používáte algoritmu Mac, hello klíčenky v OSX bezpečně uloží hesla k privátním klíčům hello při vyvolání `ssh-agent`.

Ověřte a pomocí ssh-agent a ssh přidejte tooinform hello SSH systému o soubory klíčů hello tak, aby heslo hello nebudete potřebovat toobe používá interaktivně.

```bash
eval "$(ssh-agent -s)"
```

Nyní přidejte privátní klíč hello příliš`ssh-agent` pomocí příkazu hello `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

heslo soukromého klíče Hello jsou teď uložená v `ssh-agent`.

## <a name="using-ssh-copy-id-toocopy-hello-key-tooan-existing-vm"></a>Pomocí `ssh-copy-id` toocopy hello klíče tooan existující virtuální počítač
Pokud jste již vytvořili virtuální počítač můžete nainstalovat hello nové SSH veřejného klíče tooyour virtuálního počítače s Linuxem pomocí:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>Vytvoření a nakonfigurování konfiguračního souboru SSH

Je doporučené osvědčené toocreate postup a nakonfigurovat `~/.ssh/config` souboru toospeed až přihlašování a pro optimalizaci vaše chování klienta SSH.

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

Dále si je toocreate virtuální počítače Azure s Linuxem pomocí hello nové veřejného klíče SSH.  Virtuální počítače Azure, které jsou vytvořeny pomocí veřejného klíče SSH jako hello přihlašovací údaje jsou lépe zabezpečené než virtuální počítače vytvořené pomocí hello výchozí přihlašovací metoda hesla.  Virtuální počítače Azure vytvořené pomocí klíčů SSH jsou ve výchozím nastavení nakonfigurované se zakázaným heslem, aby se vyloučily pokusy o rozluštění hesla (útokem hrubou silou).

* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí portálu Azure hello](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure hello](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
