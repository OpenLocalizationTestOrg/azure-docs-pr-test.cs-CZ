---
title: "Výběr uživatelská jména pro Linux | Microsoft Docs"
description: "Zjistěte, jak vybrat uživatelská jména pro virtuální počítač s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 33b50c97-92f1-46c9-a623-e37f67459c5c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 1874d72e5f88816036667932371ff28704d186c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="selecting-user-names-for-linux-on-azure"></a>Výběr uživatelských jmen pro Linux v Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Při zřizování virtuální počítač s Linuxem v Azure musíte zadat název jiné kořenové uživatele, který můžete později použít k přihlášení do virtuálního počítače. Můžete se rozhodnout název nového uživatele, nebo pokud zřizování prostřednictvím portálu Azure classic můžete přijmout výchozí název "azureuser".

Ve většině případů tohoto uživatele, nebude existovat bitová kopie a vytvoří se během procesu zřizování. Pokud uživatel na základní image virtuálního počítače existuje, pak Azure Linux agent jednoduše nakonfiguruje heslo nebo klíč SSH pro tohoto uživatele na základě informací, které jste zadali při vytváření virtuálního počítače.

**Ale**, Linux definuje sadu uživatelská jména, která by se neměla používat. Proces zřizování bude **nezdaří** Pokud se pokusíte zřízení virtuálního počítače s Linuxem pomocí stávajícího uživatele systému, která je definována jako uživatel s UID 0-99. Typickým příkladem je `root` uživatele, který má UID 0.

* Viz také: [základní Linux Standard - rozsahy ID uživatele](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Následuje seznam běžných uživatelů předdefinované systému CentOS a Ubuntu, že byste neměli používat při zřizování virtuální počítač s Linuxem v Azure. Tento seznam je jenom jako příklad naleznete v dokumentaci k distribuční zajistit, že zadané uživatelské jméno, které zvolíte nejsou v konfliktu s existujícím uživatelem systému.

## <a name="centos"></a>CentOS
* abrt
* ADM
* zvuk
* Koš
* CD-ROM
* cgred
* Démon procesu
* dbus
* síti službou
* vyhrazené IP adresy
* disk
* disketovou
* FTP
* FTP
* hry
* Gopher
* haldaemon
* zastavení
* kmem
* Zámek
* lineárního programování úloh
* E-mailu
* Man
* mem
* nfsnobody
* nikdo
* NTP
* Operátor
* oprofile
* postdrop
* operátory přípony
* qpidd
* kořenové
* RPC
* rpcuser
* saslauth
* Vypnutí
* slocate
* sshd
* stapdev
* stapusr
* Synchronizace
* Sys
* pásky
* Test
* tcpdump
* TTY
* uživatelé
* utempter
* procesu utmp
* UUCP
* vcsa
* video
* Wheel

## <a name="ubuntu"></a>Ubuntu
* ADM
* Správce
* zvuk
* zálohování
* Koš
* CD-ROM
* crontab
* Démon procesu
* síti službou
* vyhrazené IP adresy
* disk
* Fax
* disketovou
* pojistka
* hry
* gnats
* IRC
* kmem
* na šířku
* libuuid
* seznam
* lineárního programování úloh
* E-mailu
* Man
* MessageBus
* mlocate
* netdev
* Novinky
* nikdo
* "nogroup"
* Operátor
* plugdev
* Proxy server
* kořenové
* SASL
* stínové
* src
* SSH
* sshd
* zaměstnanci
* sudo
* Synchronizace
* Sys
* syslog
* pásky
* TTY
* uživatelé
* procesu utmp
* UUCP
* video
* hlas
* whoopsie
* WWW-data

