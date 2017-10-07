---
title: "aaaCapture bitové kopie virtuálního počítače s Linuxem | Microsoft Docs"
description: "Zjistěte, jak toocapture bitové kopie založené na systému Linux Azure virtuálního počítače (VM) vytvořené pomocí modelu nasazení classic hello."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17d7ffee-a58e-4290-9de1-64c3cf1ddc05
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: 33c4059d5bb919a86bdc3492abca540750f365ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocapture-a-classic-linux-virtual-machine-as-an-image"></a>Jak toocapture klasický Linuxový virtuální počítač jako image
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tento článek ukazuje, jak toocapture classic Azure virtuálního počítače (VM) s Linuxem jako image toocreate jiné virtuální počítače. Tento image obsahuje hello disk operačního systému a datové disky připojené toohello virtuálních počítačů. Proto musíte tooconfigure neobsahuje síťové konfigurace, které při vytváření hello další virtuální počítač z bitové kopie hello.

Úložiště Azure hello bitové kopie v rámci **bitové kopie**, společně s všechny Image, které jste odeslali. Další informace o bitových kopiích naleznete v tématu [o Image virtuálních počítačů v Azure][About Virtual Machine Images in Azure].

## <a name="before-you-begin"></a>Než začnete
Tento postup předpokládá, že jste už vytvořili pomocí modelu nasazení Classic hello a nakonfigurované hello operačního systému, včetně připojení jakýchkoli datových disků virtuálního počítače Azure. Pokud potřebujete toocreate virtuálního počítače, přečtěte si [jak tooCreate virtuální počítač s Linuxem][How tooCreate a Linux Virtual Machine].

## <a name="capture-hello-virtual-machine"></a>Zachytit virtuální počítač hello
1. [Připojit virtuální počítač toohello](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pomocí SSH klienta podle vašeho výběru.
2. V okně hello SSH zadejte následující příkaz hello. Hello výstup z `waagent` může mírně lišit v závislosti na verzi hello tohoto obslužného nástroje:

    ```bash
    sudo waagent -deprovision+user
    ```

    Hello předchozí příkaz pokusí tooclean hello systému a nastavit jej jako vhodný pro reprovisioning. Tato operace provádí hello následující úlohy:

   * Odebere hostitele klíče SSH (Pokud Provisioning.RegenerateSshHostKeyPair 'y' v konfiguračním souboru hello)
   * Vymaže konfiguraci názvový server v /etc/resolv.conf
   * Odebere hello `root` heslo uživatele z/etc/stínové (Pokud Provisioning.DeleteRootPassword 'y' v konfiguračním souboru hello)
   * Odstraní zapůjčení DHCP klientů uložená v mezipaměti
   * Resetování hostitele toolocalhost.localdomain název
   * Odstraní poslední účet zřízení uživatele hello (získaný z /var/lib/waagent) **a související data**.

     > [!NOTE]
     > Zrušení zřízení odstraní soubory a data příliš "generalize" hello image. Spusťte pouze na virtuálním počítači tento příkaz, že máte v úmyslu toocapture jako novou šablonu bitové kopie. Nezaručuje se této bitové kopie hello vymaže všechny citlivých informací nebo je vhodný pro opětovnou distribuci toothird strany.

3. Typ **y** toocontinue. Můžete přidat hello `-force` parametr tooavoid tento krok potvrzení.
4. Typ **ukončení** tooclose hello SSH klienta.

   > [!NOTE]
   > Hello zbývající kroky předpokládají už máte [nainstalované rozhraní příkazového řádku Azure hello](../../../cli-install-nodejs.md) na klientském počítači. Všechny hello následující kroky lze provést v hello [portál Azure](http://portal.azure.com).

5. Na klientském počítači otevřete rozhraní příkazového řádku Azure a přihlášení tooyour předplatného Azure. Podrobnosti najdete v tématu [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure hello](../../../xplat-cli-connect.md).

   > [!NOTE]
   > V hello portálu Azure Přihlaste se toohello portálu.

6. Ujistěte se, že jste v režimu správy služby:

    ```azurecli
    azure config mode asm
    ```

7. Vypněte virtuální počítač, který je již zrušit hello. Hello následující příklad vypne hello virtuálního počítače s názvem `myVM`:

    ```azurecli
    azure vm shutdown myVM
    ```
   V případě potřeby můžete zobrazit seznam všech hello virtuální počítače vytvořené v rámci vašeho předplatného pomocí`azure vm list`

   > [!NOTE]
   > Pokud používáte hello portálu Azure, vyberte hello virtuálních počítačů a klikněte na tlačítko **Zastavit** vypnout hello virtuálních počítačů.

8. Pokud je zastavená hello virtuálních počítačů, zachycení bitové kopie hello. Následující příklad zachycení Hello hello virtuálního počítače s názvem `myVM` a vytváří zobecněný bitovou kopii s názvem `myNewVM`:

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    Hello `-t` podpříkaz odstranění hello původní virtuální počítač.

    > [!NOTE]
    > V hello portálu Azure, budete moct zachytit image výběrem **Image** nabídce hello rozbočovače. Budete potřebovat následující informace k bitové kopii hello hello toosupply: název, skupinu prostředků, umístění, typ operačního systému a cestu k úložišti objektů blob.

9. Hello nová bitová kopie je nyní k dispozici v seznamu hello bitových kopií, které se dají použít tooconfigure žádné nové virtuální počítače. Můžete ji zobrazit pomocí příkazu hello:

   ```azurecli
   azure vm image list
   ```

   Na hello [portál Azure](http://portal.azure.com), hello nová bitová kopie se zobrazí v hello **Image virtuálních počítačů (klasické)** , patří toohello **výpočetní** služby. Dostanete **Image virtuálních počítačů (klasické)** kliknutím _další služby_ dole hello hello Azure služby seznamu a pak vyhledávání v hello **výpočetní** služby.   

   ![Zachycení Image úspěšné](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Další kroky
Hello bitová kopie je připravená toobe používá toocreate virtuálních počítačů. Můžete použít příkaz příkazového řádku Azure CLI hello `azure vm create` a název bitové kopie hello napájení jste vytvořili. Další informace najdete v tématu [hello pomocí rozhraní příkazového řádku Azure s modelem nasazení Classic](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

Můžete taky použít hello [portál Azure](http://portal.azure.com) toocreate vlastní virtuální počítač pomocí hello **Image** metoda a výběrem hello bitovou kopii, kterou jste vytvořili. Další informace najdete v tématu [jak tooCreate vlastní virtuální počítač][How tooCreate a Custom Virtual Machine].

**Viz také:** [Azure Linux Agent uživatelská příručka](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How tooCreate a Custom Virtual Machine]:create-custom.md
[How tooAttach a Data Disk tooa Virtual Machine]:attach-disk.md
[How tooCreate a Linux Virtual Machine]:create-custom.md
