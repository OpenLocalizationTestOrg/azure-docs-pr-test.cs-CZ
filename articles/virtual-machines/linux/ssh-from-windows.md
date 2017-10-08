---
title: "klíče aaaUse SSH se systémem Windows pro virtuální počítače s Linuxem | Microsoft Docs"
description: "Zjistěte, jak toogenerate a používání SSH klíče na počítači tooconnect tooa Linux virtuálního počítače s Windows v Azure."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: danlep
ms.openlocfilehash: 6c44217332538857cc2ca2e85de4b476aa71251c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-ssh-keys-with-windows-on-azure"></a>Jak klíče tooUse SSH se systémem Windows v Azure
> [!div class="op_single_selector"]
> * [Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> * [Linux nebo Mac.](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>
>

Když připojíte tooLinux virtuálních počítačů (VM) v Azure, měli byste použít [public key cryptography](https://wikipedia.org/wiki/Public-key_cryptography) tooprovide bezpečnější toolog způsob, jak v tooyour virtuálního počítače s Linuxem. Tento proces zahrnuje veřejné a privátní výměny klíčů pomocí hello zabezpečené shell (SSH) příkaz tooauthenticate místo zadání uživatelského jména a hesla. Hesla jsou útoky citlivé toobrute-force, zvláště na straně Internetu virtuálních počítačů, jako jsou třeba webové servery. Tento článek obsahuje přehled klíčů SSH a jak toogenerate hello odpovídající klíče na počítači se systémem Windows.

## <a name="overview-of-ssh-and-keys"></a>Přehled SSH a klíče
Tooyour virtuálního počítače s Linuxem můžete bezpečně přihlašovat pomocí veřejného a privátního klíče:

* Hello **veřejný klíč** je umístěn na virtuálním počítačům s Linuxem nebo jinou službu, že si přejete toouse u kryptografie využívající veřejného klíče.
* Hello **privátní klíč** tom, co jste při přihlášení, tooverify je přítomen tooyour virtuálního počítače s Linuxem vaši identitu. Chraňte tento privátní klíč. Nesdílejte ho.

Tyto veřejné a soukromé klíče lze použít na více virtuálních počítačů a služeb. Pro každý virtuální počítač nebo služba chcete tooaccess nepotřebujete pár klíčů. Podrobnější přehled, najdete v tématu [public key cryptography](https://wikipedia.org/wiki/Public-key_cryptography).

SSH je protokol šifrované připojení, která umožňuje zabezpečený přihlášení přes nezabezpečený připojení. Je hello výchozí připojení protokolu pro virtuální počítače Linux hostované v Azure. I když SSH, samotné poskytuje šifrované připojení, pomocí hesla s připojeními SSH stále zanechává hello virtuálních počítačů bude zranitelný toobrute-force útoky, nebo hádání hesel. Bezpečnější a upřednostňovanou metodu pro připojování tooa virtuálního počítače pomocí protokolu SSH je pomocí těchto veřejné a soukromé klíče, také známé jako klíče SSH.

Pokud nechcete, aby toouse klíče SSH, je stále přihlásit tooyour virtuální počítače s Linuxem pomocí hesla. Pokud virtuální počítač není zveřejněné toohello Internetu, může být dostatečná pomocí hesla. Můžete však stále potřebovat toomanage hesla pro každý virtuální počítač s Linuxem a Udržovat zásady hesel v pořádku a postupy, jako je minimální délka hesla a pravidelně aktualizuje. Hello použití klíčů SSH snižuje složitost hello správy jednotlivých přihlašovacích údajů napříč více virtuálními počítači.

## <a name="windows-packages-and-ssh-clients"></a>Balíčky pro systém Windows a klientů SSH
Připojit tooand spravovat virtuální počítače s Linuxem v Azure pomocí **klient SSH**. Počítače se systémem Windows obvykle nemají nainstalovaného klienta SSH. Přidat Bash pro Windows Hello Windows 10 Anniversary Update a hello nejnovější aktualizace služby Windows 10 Creators nabízí další aktualizace. Tato subsystému Windows pro Linux umožňuje toorun a přístup nástroje, jako je například klientem SSH nativně v rámci prostředí Bash. Můžete pak provedením jakéhokoliv z dokumentace Linux hello, například [jak páry klíč SSH toogenerate pro Linux](mac-create-ssh-keys.md). Bash pro Windows je stále ve vývoji a je považován za verzi beta. Další informace o Bash pro systém Windows najdete v tématu [Bash na Ubuntu v systému Windows](https://msdn.microsoft.com/commandline/wsl/about).

Pokud chcete toouse něco jiného než Bash pro systém Windows, jsou běžné Windows SSH klientů, které můžete nainstalovat součástí hello následující balíčky:

* [Git pro Windows](https://git-for-windows.github.io/)
* [puTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [MobaXterm](http://mobaxterm.mobatek.net/)
* [Emulaci](https://cygwin.com/)


## <a name="which-key-files-do-you-need-toocreate"></a>Které klíče soubory potřebujete toocreate?
Azure vyžaduje minimálně 2048bitové **ssh-rsa** formátu veřejné a soukromé klíče. Pokud spravujete pomocí modelu nasazení Classic hello prostředky Azure, musíte taky toogenerate PEM (`.pem` souboru).

Zde jsou hello scénáře nasazení a hello typy souborů, které můžete použít v každém:

1. **SSH-rsa** klíče jsou požadovány pro všechna nasazení pomocí hello [portál Azure](https://portal.azure.com)a nasazení Resource Manager pomocí hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md).
   * Tyto klíče jsou obvykle že potřebovat všechny většina lidí.
2. A `.pem` soubor je požadovaná toocreate virtuální počítače pomocí nasazení Classic hello. Tyto klíče jsou podporovány v nasazení Classic při použití hello [portál Azure](https://portal.azure.com) nebo [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md).
   * Potřebujete jenom toocreate tyto další klíčů a certifikátů Pokud spravujete prostředky vytvořené pomocí modelu nasazení Classic hello.

## <a name="install-git-for-windows"></a>Instalace Gitu pro Windows
Hello předchozí části uveden více balíčků, které zahrnují hello `openssl` nástroje pro systém Windows. Tento nástroj je potřebné toocreate veřejné a soukromé klíče. Následující příklady podrobnosti o tom, jak Hello tooinstall a používat **Git pro Windows**, i když můžete podle toho, která balíček dáváte přednost. **Git pro Windows** dává vám přístup toosome další open-source softwaru ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) nástrojů a pomůcek, které mohou být užitečné při práci s virtuální počítače s Linuxem.

1. Stáhněte a nainstalujte **Git pro Windows** z hello následující umístění: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).
2. Přijměte výchozí možnosti hello během hello postup instalace pouze v případě potřebujete toochange je.
3. Spustit **Git Bash** z hello **nabídce Start** > **Git** > **Git Bash**. Hello konzoly vypadá podobně jako toohello následující ukázka:

    ![Prostředí Git Bash pro Windows](./media/ssh-from-windows/git-bash-window.png)

## <a name="create-a-private-key"></a>Vytvoření privátního klíče
1. Ve vaší **Git Bash** okně použití `openssl.exe` toocreate privátní klíč. Hello následující příklad vytvoří klíč s názvem `myPrivateKey` a certifikát s názvem `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    výstup Hello vypadá podobně jako toohello následující ukázka:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key too'myPrivateKey.key'
    -----
    You are about toobe asked tooenter information that will be incorporated
    into your certificate request.
    What you are about tooenter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', hello field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

   Pokud se zobrazí chybová zpráva: bash, zkuste otevřít nové **Git Bash** okno se zvýšenými oprávněními. Spusťte hello `openssl` příkaz.

2. Odpověď hello vyzve k zadání názvu země, umístění, název organizace, atd.
3. Váš nový privátní klíč a certifikát se vytvoří v aktuální pracovní adresář. Jako bezpečnostní opatření měli byste nastavit hello oprávnění na váš privátní klíč, aby pouze budete mít přístup:

    ```bash
    chmod 0600 myPrivateKey.key
    ```

4. Hello [další části](#create-a-private-key-for-putty) podrobnosti o pomocí PuTTYgen tooboth zobrazení a použití hello veřejný klíč a vytvoření privátní klíč specifické pro používání PuTTY tooSSH tooLinux virtuálních počítačů. Hello následující příkaz vytvoří soubor veřejného klíče s názvem `myPublicKey.key` , můžete použít:

    ```bash
    openssl.exe rsa -pubout -in myPrivateKey.key -out myPublicKey.key
    ```

5. Pokud budete také potřebovat toomanage klasické prostředky, převést hello `myCert.pem` příliš`myCert.cer` (X509, kódování DER certifikát). Proveďte tento volitelný krok jenom v případě, že potřebujete toospecifically spravovat starší klasické prostředky.

    Převeďte hello certifikát pomocí hello následující příkaz:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Vytvoření privátní klíč pro PuTTY
PuTTY je běžné SSH klient pro systém Windows. Můžete se volné toouse libovolného klienta SSH, který chcete. toouse PuTTY, musíte toocreate další typ klíče - privátní klíč PuTTY (PPK). Pokud nechcete, aby toouse PuTTY, tuto část přeskočte.

Hello následující příklad vytvoří tento další privátní klíč speciálně pro PuTTY toouse:

1. Použití **Git Bash** tooconvert vaší privátní klíče do privátní klíč RSA, že můžete porozumět PuTTYgen. Hello následující příklad vytvoří klíč s názvem `myPrivateKey_rsa` z hello existující klíč s názvem `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Jako bezpečnostní opatření měli byste nastavit hello oprávnění na váš privátní klíč, aby pouze budete mít přístup:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```
2. Stažení a spuštění PuTTYgen z hello následující umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
3. V nabídce hello: **soubor** > **zatížení privátní klíč**
4. Najít váš privátní klíč (`myPrivateKey_rsa` v předchozím příkladu hello). Výchozí adresář Hello při spuštění **Git Bash** je `C:\Users\%username%`. Změnit hello soubor filtru tooshow **všechny soubory (\*.\*)** :

    ![Načtení hello existující soukromý klíč do PuTTYgen](./media/ssh-from-windows/load-private-key.png)
5. Klikněte na tlačítko **otevřete**. Příkazovém řádku uvedena, že byl úspěšně importován klíči hello:

    ![Byly úspěšně importovány klíče tooPuTTYgen](./media/ssh-from-windows/successfully-imported-key.png)
6. Klikněte na tlačítko **OK** tooclose hello řádku.
7. veřejný klíč Hello se zobrazí v horní části hello Dobrý den **PuTTYgen** okno. Zkopírujte a vložte tento veřejný klíč do hello portál Azure nebo do šablony Azure Resource Manager, když vytvoříte virtuální počítač s Linuxem. Můžete také kliknout na **uložit veřejný klíč** toosave počítač tooyour kopie:

    ![Uložte soubor PuTTY veřejného klíče](./media/ssh-from-windows/save-public-key.png)

    Hello následující příklad ukazuje, jak by zkopírujte a vložte tento veřejný klíč do hello portálu Azure při vytváření virtuálního počítače s Linuxem. veřejný klíč Hello je obvykle pak uloženy v `~/.ssh/authorized_keys` na nový virtuální počítač.

    ![Veřejný klíč použít při vytváření virtuálního počítače v hello portálu Azure](./media/ssh-from-windows/use-public-key-azure-portal.png)
8. Zpět v **PuTTYgen**, klikněte na tlačítko **uložit privátní klíč**:

    ![Uložte soubor privátního klíče PuTTY](./media/ssh-from-windows/save-ppk-file.png)

   > [!WARNING]
   > Zobrazí výzva s dotazem chcete toocontinue bez zadávání přístupové heslo klíče. Přístupové heslo je stejné jako heslo připojené tooyour privátní klíč. I když někdo byly tooobtain váš privátní klíč, stále by nebyly možné tooauthenticate pomocí právě hello klíče. Potřebují by také hello přístupové heslo. Bez přístupové heslo Pokud někdo získá váš privátní klíč, se můžete přihlásit tooany virtuálního počítače nebo služby tohoto použije klíč. Doporučujeme že vytvořit přístupové heslo. Ale pokud zapomenete heslo hello, neexistuje žádný způsob, jak toorecover ho.
   >
   >

    Pokud chcete tooenter přístupové heslo, klikněte na tlačítko **ne**, zadejte přístupové heslo v hlavním okně PuTTYgen hello a pak klikněte na tlačítko **uložit privátní klíč** znovu. Jinak, klikněte na tlačítko **Ano** toocontinue bez zadání hello volitelné přístupové heslo.
9. Zadejte název a umístění toosave souboru PPK.

## <a name="use-putty-toossh-tooa-linux-machine"></a>Použít Putty tooSSH tooa počítač Linux
PuTTY znovu, je běžné SSH klient pro systém Windows. Můžete se volné toouse libovolného klienta SSH, který chcete. Dobrý den, následující kroky podrobnosti o tom, jak toouse vaší privátní klíče tooauthenticate s svého virtuálního počítače Azure pomocí protokolu SSH. Hello kroky se podobají ostatní klíče SSH klienty z hlediska nutnosti tooload vaší privátní klíče tooauthenticate hello připojení SSH.

1. Stažení a spuštění putty z hello následující umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Vyplňte hello název hostitele nebo IP adresu virtuálního počítače z hello portálu Azure:

    ![Otevřít nové připojení PuTTY](./media/ssh-from-windows/putty-new-connection.png)
3. Před výběrem **otevřete**, klikněte na tlačítko **připojení** > **SSH** > **Auth** kartě. Procházet tooand vyberte privátní klíč:

    ![Vyberte PuTTY privátní klíč pro ověřování](./media/ssh-from-windows/putty-auth-dialog.png)
4. Klikněte na tlačítko **otevřete** tooconnect tooyour virtuálního počítače

## <a name="next-steps"></a>Další kroky
Můžete také vygenerovat hello veřejné a soukromé klíče [pomocí OS X a Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Další informace o Bash pro systém Windows a výhodách hello operačních systémů nástroje snadno dostupné na počítači s Windows najdete v tématu [Bash na Ubuntu v systému Windows](https://msdn.microsoft.com/commandline/wsl/about).

Pokud máte potíže při používání SSH tooconnect tooyour virtuální počítače s Linuxem, přečtěte si téma [řešení SSH připojení tooan virtuální počítač Azure s Linuxem](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
