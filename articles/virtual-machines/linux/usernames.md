---
title: "aaaSelecting uživatelská jména pro Linux | Microsoft Docs"
description: "Zjistěte, jak se uživatel tooselect názvů pro virtuální počítač s Linuxem v Azure."
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
ms.openlocfilehash: c65e2ac46f40bb8c9d74cccbaf248a070c0fa6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="selecting-user-names-for-linux-on-azure"></a>Výběr uživatelských jmen pro Linux v Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Při zřizování virtuální počítač s Linuxem v Azure musíte zadat název hello nekořenovými uživatele, které můžete později použít toolog do hello virtuálních počítačů. Můžete se rozhodnout hello název hello nového uživatele, nebo pokud zřizování prostřednictvím hello portál Azure classic můžete přijmout výchozí hello název "azureuser".

Ve většině případů tento uživatel nebude existovat na základní image hello a vytvoří se během procesu zřizování hello. Pokud uživatel hello existuje na hello základní image virtuálního počítače, pak hello Azure Linux agent jednoduše nakonfiguruje hello heslo nebo klíč SSH pro tohoto uživatele na základě informací o hello, které jste zadali při vytváření hello virtuálních počítačů.

**Ale**, Linux definuje sadu uživatelská jména, která by se neměla používat. Zřizování se proces Hello **nezdaří** Pokud se pokusíte tooprovision virtuálního počítače s Linuxem pomocí stávajícího uživatele systému, která je definována jako uživatel s UID 0-99. Typickým příkladem je hello `root` uživatele, který má UID 0.

* Viz také: [základní Linux Standard - rozsahy ID uživatele](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Hello následuje seznam běžných uživatelů předdefinované systému CentOS a Ubuntu, že byste neměli používat při zřizování virtuální počítač s Linuxem v Azure. Tento seznam je jenom jako příklad naleznete v dokumentaci toohello pro vaše tooensure distribuční tímto uživatelským jménem, které zvolíte není v konfliktu s existujícím uživatelem systému hello.

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

