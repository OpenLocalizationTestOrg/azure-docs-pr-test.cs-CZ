---
title: "aaaUsing hostnames tooregister dynamického DNS"
description: "Tato stránka obsahuje údaje o tom, tooset až dynamického DNS tooregister názvy hostitelů v serverech DNS."
services: dns
documentationcenter: na
author: GarethBradshawMSFT
manager: timlt
editor: 
ms.assetid: c315961a-fa33-45cf-82b9-4551e70d32dd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2017
ms.author: garbrad
ms.openlocfilehash: 8d4b44265714e6976f26bfb3446e8101aa70996a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-dynamic-dns-tooregister-hostnames-in-your-own-dns-server"></a>Použití dynamického DNS tooregister názvy hostitelů v serveru DNS
[Azure poskytuje překlad](virtual-networks-name-resolution-for-vms-and-role-instances.md) pro virtuální počítače (VM) a instancí rolí. Ale pokud vaše překlad musí přejít nad rámec těch, které poskytuje Azure, můžete zadat vlastní servery DNS. Tato poskytuje hello power tootailor vaše řešení toosuit DNS specifické potřeby. Například může být nutné tooaccess místních prostředků pomocí řadiče domény služby Active Directory.

Pokud vaše vlastní servery DNS jsou hostované jako virtuální počítače Azure, které může předávat název hostitele dotazuje na hello stejné názvy hostitelů tooresolve tooAzure virtuální sítě. Pokud nechcete, aby toouse této trase, můžete zaregistrovat vaše názvy hostitelů virtuálních počítačů v serveru DNS pomocí dynamického DNS.  Azure nemá toodirectly možnost (například přihlašovací údaje) hello alternativní uspořádání často je třeba vytvořit záznamy v vaše servery DNS. Tady jsou některé běžné scénáře s alternativy.

## <a name="windows-clients"></a>Klienti systému Windows
Klienty připojené k jiné doméně systému Windows pokus zabezpečená aktualizace dynamického DNS (DDNS), když se budou spouštět nebo když se změní své IP adresy. název DNS Hello je název hostitele hello plus hello primární příponu DNS. Azure zůstane prázdné hello primární příponu DNS, ale to můžete nastavit v hello virtuálních počítačů, prostřednictvím hello [uživatelského rozhraní](https://technet.microsoft.com/library/cc794784.aspx) nebo [pomocí automatizace](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).

Klienty připojené k doméně systému Windows registrovat IP adresy s řadičem domény hello pomocí zabezpečené dynamické DNS. Proces připojení k doméně Hello nastaví hello primární příponu DNS na klientovi hello a vytvoří a udržuje hello vztah důvěryhodnosti.

## <a name="linux-clients"></a>Klienti Linux
Klienti Linux obecně nezaregistrujete sami hello serveru DNS při spuštění, se předpokládá, že hello DHCP server dělá. Servery DHCP Azure nemají možnost hello nebo přihlašovací údaje tooregister záznamy v serveru DNS.  Můžete použít nástroj nazvaný *nsupdate*, který je součástí balíčku hello vazby, aktualizuje toosend dynamického DNS. Protože je standardizovaný hello protokol dynamické DNS, můžete použít *nsupdate* i když nepoužíváte vazby na serveru DNS hello.

Můžete použít hello háky, které jsou poskytovány toocreate klienta DHCP hello a udržovat hello položka názvu hostitele v serveru DNS hello. Během cyklu hello DHCP, spustí klienta hello hello skripty v */etc/dhcp/dhclient-exit-hooks.d/*. To může být použit tooregister hello novou IP adresu pomocí *nsupdate*. Například:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on hello primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        
        

Můžete taky hello *nsupdate* tooperform příkaz DNS zabezpečené dynamické aktualizace. Například pokud používáte server DNS vytvořit vazbu, páru veřejného a privátního klíče RSA je [generované](http://linux.yyz.us/nsupdate/).  je Hello DNS server [nakonfigurované](http://linux.yyz.us/dns/ddns-server.html) s hello veřejné součástí klíče hello tak to můžete ověřit podpis hello u hello požadavku. Je nutné použít hello *-k* tooprovide možnost hello dvojice klíč příliš*nsupdate* aby hello požadavek toobe podepsané aktualizace dynamického DNS.

Pokud používáte server DNS systému Windows, můžete použít ověřování pomocí protokolu Kerberos s hello *-g* parametr v *nsupdate* (není k dispozici ve verzi Windows hello *nsupdate* ). toodo tuto, použijte *kinit* tooload hello přihlašovací údaje (například z [keytab souboru](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)). Potom *nsupdate -g* se vyzvedávat hello přihlašovací údaje z mezipaměti hello.

V případě potřeby můžete přidat tooyour přípon vyhledávání DNS virtuálních počítačů. přípona DNS Hello je uveden v hello */etc/resolv.conf* souboru. Většina distribucích systému Linux automaticky spravovat hello obsah tohoto souboru, takže obvykle nelze jej upravovat. Přípona hello však můžete přepsat pomocí klienta DHCP hello *nahrazují* příkaz. toodo to v */etc/dhcp/dhclient.conf*, přidejte:

        supersede domain-name <required-dns-suffix>;

