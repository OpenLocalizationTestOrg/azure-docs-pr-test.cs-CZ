---
title: "Pomocí oprávnění kořenového na virtuální počítače s Linuxem | Microsoft Docs"
description: "Další informace o použití oprávnění kořenového na virtuální počítač s Linuxem v Azure."
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
ms.openlocfilehash: dc39db1f5fecffb60499a5420bfe72850e2fffd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Použití kořenových oprávnění u virtuálních počítačů s Linuxem v Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Ve výchozím nastavení `root` je uživatel zakázán na virtuální počítače s Linuxem v Azure. Uživatelé mohou spouštět příkazy se zvýšeným oprávněním pomocí `sudo` příkaz. Možnosti však mohou lišit v závislosti na tom, jak zřízenou systému.

1. **Klíč SSH a heslo nebo heslo pouze** -zřízenou virtuální počítač buď certifikátem (`.CER` soubor) nebo klíč SSH a také heslo, nebo jenom uživatelské jméno a heslo. V takovém případě `sudo` výzvou k zadání hesla před provedením příkazu.
2. **Klíč SSH pouze** -zřízenou virtuální počítač s certifikátem (`.cer`, `.pem`, nebo `.pub` soubor) nebo klíč SSH, ale žádné heslo.  V takovém případě `sudo` **nikoli** výzvy pro heslo uživatele před provedením příkazu.

## <a name="ssh-key-and-password-or-password-only"></a>SSH klíč a heslo nebo pouze heslo
Přihlaste se k virtuálnímu počítači Linux pomocí ověřování SSH klíč nebo heslo a potom spusťte příkazy pomocí `sudo`, například:

    # sudo <command>
    [sudo] password for azureuser:

V tomto případě uživatel se vyzve k zadání hesla. Po zadání hesla `sudo` spustí příkaz s `root` oprávnění.

Můžete také povolit passwordless sudo úpravou `/etc/sudoers.d/waagent` souboru, například:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Tato změna umožňuje passwordless sudo uživatelem "azureuser".

## <a name="ssh-key-only"></a>SSH pouze klíč
Přihlaste se k virtuálnímu počítači Linux pomocí ověření pomocí klíče SSH, a poté spusťte příkazy pomocí `sudo`, například:

    # sudo <command>

V takovém případě bude uživatel **není** vyzváni k zadání hesla. Po stiskněte `<enter>`, `sudo` spustí příkaz s `root` oprávnění.

