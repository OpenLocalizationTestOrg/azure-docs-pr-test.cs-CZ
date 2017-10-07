---
title: "aaaMove soubory tooand z virtuálních počítačů Linux Azure s spojovací bod služby | Microsoft Docs"
description: "Bezpečně přesunout tooand soubory z virtuálního počítače s Linuxem v Azure pomocí spojovací bod služby a dvojici klíčů SSH."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 9e4dce667aa581df74aec3d45a6ba5e52f777d1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-files-tooand-from-a-linux-vm-using-scp"></a>Přesun tooand soubory z virtuálního počítače s Linuxem pomocí spojovací bod služby

Tento článek ukazuje, jak toomove soubory z pracovní stanici si tooan virtuální počítač Azure s Linuxem nebo Linux virtuálního počítače Azure dolů tooyour pracovní stanici, pomocí zabezpečené kopírování (SCP). Přesouvání souborů mezi pracovní stanice a virtuální počítač s Linuxem, rychle a bezpečně, je důležité pro správu infrastruktury Azure. 

V tomto článku budete potřebovat Linux virtuálních počítačů nasadit v Azure pomocí [SSH soubory veřejného a privátního klíče](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Budete také potřebovat klientem spojovací bod služby v místním počítači. Je postavená na SSH a součástí prostředí Bash výchozí hello většina Linux a počítače Mac a některé součásti pro Windows.

## <a name="quick-commands"></a>Rychlé příkazy

Zkopírujte soubor do toohello virtuálního počítače s Linuxem

```bash
scp file azureuser@azurehost:directory/targetfile
```

Zkopírujte soubor dolů z hello virtuálního počítače s Linuxem

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a>Podrobný postup

Jako příklady, můžeme přesunout soubor konfigurace Azure až tooa virtuálního počítače s Linuxem a stahují adresář souboru protokolu, jak pomocí klíče spojovací bod služby a SSH.   

## <a name="ssh-key-pair-authentication"></a>Ověřování pár klíčů SSH

Spojovací bod služby používá SSH pro hello přenosové vrstvy. SSH popisovače hello ověřování na cílovém hostiteli hello a přesune soubor hello v tunelu šifrované ve výchozím nastavení provádí pomocí protokolu SSH. Pro ověřování SSH lze použít uživatelských jmen a hesel. Veřejné a soukromé klíče ověřování SSH jsou však doporučujeme jako osvědčený postup zabezpečení. Po ověření hello připojení SSH spojovací bod služby poté zahájí kopírování souboru hello. Pomocí správně nakonfigurovaných `~/.ssh/config` a veřejného a privátního klíče SSH, hello spojovací bod služby připojení lze navázat jen pomocí názvu serveru (nebo IP adresa). Pokud máte pouze jeden klíč SSH, spojovací bod služby hledá pro něj v hello `~/.ssh/` adresář a používá je ve výchozím nastavení toolog v toohello virtuálních počítačů.

Další informace o konfiguraci vašeho `~/.ssh/config` a veřejné a soukromé klíče SSH, najdete v části [vytvořit SSH klíče](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="scp-a-file-tooa-linux-vm"></a>Spojovací bod služby tooa soubor virtuálního počítače s Linuxem

Hello prvním příkladu jsme zkopírujte soubor konfigurace Azure až tooa Linux virtuálního počítače, který je použité toodeploy automation. Protože tento soubor obsahuje přihlašovací údaje Azure API, které obsahují tajné klíče, je důležité zabezpečení. Hello šifrované tunelového propojení SSH poskytované chrání obsah hello hello souboru.

Následující příkaz kopie hello místní Hello *.azure/config* souboru tooan virtuální počítač Azure s plně kvalifikovaný název domény *myserver.eastus.cloudapp.azure.com*. uživatelské jméno správce hello na hello virtuální počítač Azure je *azureuser* . Hello soubor je cílové toohello */home/azureuser/* adresáře. Nahraďte vlastními hodnotami v tomto příkazu.

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a>Spojovací bod služby adresáře z virtuálního počítače s Linuxem

V tomto příkladu jsme zkopírujte adresář souborů protokolu z hello virtuálního počítače s Linuxem dolů tooyour pracovní stanice. Soubor protokolu může nebo nemusí obsahovat citlivá nebo tajný data. Použití spojovací bod služby zajišťuje ale, že jsou šifrovaná hello obsah souborů protokolu hello. Pomocí souborů hello tootransfer spojovací bod služby je nejjednodušší způsob, jak tooget hello hello adresář protokolu a soubory dolů pracovní stanice tooyour při zároveň zabezpečené.

Hello následující příkaz zkopíruje soubory v hello */home/azureuser/logs/* v hello virtuálního počítače Azure toohello místní TMP adresáře:

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

Hello `-r` rozhraní příkazového řádku příznak dá pokyn spojovací bod služby toorecursively kopie hello souborů a adresářů z bodu hello hello adresáře uvedené v příkazu hello.  Všimněte si, že hello syntaxe příkazového řádku je také podobné tooa `cp` zkopírujte příkaz.

## <a name="next-steps"></a>Další kroky

* [Spravovat uživatele, SSH a zkontrolujte nebo hello oprava disky na virtuální počítače Azure s Linuxem pomocí rozšíření VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
