---
title: "Oprávnění kořenového aaaUse na virtuální počítače s Linuxem | Microsoft Docs"
description: "Zjistěte, jak toouse kořenové oprávnění na virtuální počítač s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: a2c106a2-dceb-43a3-9dd1-50ed77685952
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 9411588c5fd0c86c4c73b3e44fbb56ab150013d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Použití kořenových oprávnění u virtuálních počítačů s Linuxem v Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Ve výchozím nastavení, hello `root` je uživatel zakázán na virtuální počítače s Linuxem v Azure. Uživatelé mohou spouštět příkazy se zvýšeným oprávněním pomocí hello `sudo` příkaz. Hello prostředí však mohou lišit v závislosti na tom, jak zřízenou hello systému.

1. **Klíč SSH a heslo nebo heslo pouze** -zřízenou hello virtuální počítač buď certifikátem (`.CER` soubor) nebo klíč SSH a také heslo, nebo jenom uživatelské jméno a heslo. V takovém případě `sudo` výzvou k zadání hesla hello uživatele, před provedením příkazu hello.
2. **Klíč SSH pouze** -zřízenou hello virtuální počítač s certifikátem (`.cer`, `.pem`, nebo `.pub` soubor) nebo klíč SSH, ale žádné heslo.  V takovém případě `sudo` **nikoli** výzvy pro heslo hello uživatele před provedením příkazu hello.

## <a name="ssh-key-and-password-or-password-only"></a>SSH klíč a heslo nebo pouze heslo
Přihlaste se k hello Linux virtuálního počítače pomocí ověřování SSH klíč nebo heslo a potom spusťte příkazy pomocí `sudo`, například:

    # sudo <command>
    [sudo] password for azureuser:

V takovém případě hello uživateli zobrazí výzva k zadání hesla. Po zadání hesla hello `sudo` spustí příkaz hello s `root` oprávnění.

Můžete také povolit passwordless sudo úpravou hello `/etc/sudoers.d/waagent` souboru, například:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Tato změna umožňuje passwordless sudo uživatelem hello "azureuser".

## <a name="ssh-key-only"></a>SSH pouze klíč
Přihlaste se k hello Linux virtuálního počítače pomocí ověření pomocí klíče SSH, a poté spusťte příkazy pomocí `sudo`, například:

    # sudo <command>

V takovém případě bude uživatel hello **není** vyzváni k zadání hesla. Po stiskněte `<enter>`, `sudo` spustí příkaz hello s `root` oprávnění.

