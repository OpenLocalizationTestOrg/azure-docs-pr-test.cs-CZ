---
title: "aaaEndorsed distribucích systému Linux | Microsoft Docs"
description: "Další informace o Linux na distribuce schválené pro Azure, včetně pokynů pro Ubuntu, CentOS, Oracle a SUSE."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: f006972d4611434c62b72a1d8df60caf753e15f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="linux-on-distributions-endorsed-by-azure"></a>Linux na distribuce schválené pro Azure
Partneři poskytovat Linux obrázků v hello Azure Marketplace. Pracujeme s různými tooadd komunit Linux i více typů toohello schválené distribučního seznamu. V hello té doby pro distribuce, které nejsou k dispozici z hello Marketplace, můžete vždy zahrnout vlastní Linux podle následujících pokynů hello v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="supported-distributions-and-versions"></a>Podporované distribuce a verze
Hello následující tabulka uvádí hello Linuxových distribucích a verzích, které jsou podporovány v Azure. Odkazovat příliš[podporu pro Linux Image v nástroji Microsoft Azure](https://support.microsoft.com/en-us/kb/2941892) podrobnější informace.

Hello Linux integrační služby (LIS) ovladače pro Hyper-V a Azure jsou moduly jádra Microsoft přispívá přímo toohello nadřazeného Linux jádra.  Některé ovladače LIS jsou součástí hello distribuční jádra ve výchozím nastavení. Starší distribuce, které jsou založeny na Red Hat Enterprise (RHEL) / CentOS jsou k dispozici jako samostatný soubor ke stažení na [Linux Integration Services verze 4.1 pro Hyper-V](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409). V tématu [Linux jádra požadavky](create-upload-generic.md#linux-kernel-requirements) Další informace o ovladačích LIS hello.

Hello Azure Linux Agent je již předem nainstalován v Azure Marketplace hello bitové kopie a je obvykle dostupná z úložiště balíčků distribuce hello. Zdrojový kód najdete na [Githubu](https://github.com/azure/walinuxagent).

| Distribuce | Verze | Ovladače | Agent |
| --- | --- | --- | --- |
| CentOS |CentOS 6.3 + 7.0 + |CentOS 6.3: [LIS stáhnout](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: V jádra |Balíček: V [úložišti](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) v části "WALinuxAgent" <br/>Zdrojový kód: [Githubu](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0+ |Jádra systému |Zdrojový kód: [Githubu](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7.9 +, 8.2 + |Jádra systému |Balíček: V úložišti v části "příkaz waagent" <br/>Zdrojový kód: [Githubu](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+, 7.0+ |Jádra systému |Balíček: V úložišti v části "WALinuxAgent" <br/>Zdrojový kód: [Githubu](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 6.7 +, 7.1 + |Jádra systému |Balíček: V úložišti v části "WALinuxAgent" <br/>Zdrojový kód: [Githubu](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise |SLES/SLES pro SAP<br>11 SP4<br>12 SP1 +|Jádra systému |Balíček:<p> pro 11 v [cloudu: nástroje](https://build.opensuse.org/project/show/Cloud:Tools) úložišti<br>pro 12 součástí modulu "Veřejného cloudu" v části "python agenta azure"<br/>Zdrojový kód: [Githubu](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |openSUSE přestupného 42.1 + |Jádra systému |Balíček: V [cloudu: nástroje](https://build.opensuse.org/project/show/Cloud:Tools) úložišti v části "python agenta azure" <br/>Zdrojový kód: [Githubu](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04, 14.04, 16.10 a 16.04 |Jádra systému |Balíček: V úložišti v části "walinuxagent" <br/>Zdrojový kód: [Githubu](https://github.com/Azure/WALinuxAgent) |

## <a name="partners"></a>Partneři

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Z webu CoreOS hello:

*CoreOS je určená pro zabezpečení, konzistence a spolehlivosti. Místo instalace balíčků prostřednictvím yum nebo výstižný, CoreOS používá Linux kontejnery toomanage vašim službám na vyšší úrovni abstrakce. V rámci kontejneru, který se dá spustit na jeden nebo více počítačů CoreOS balíčku kódu jedné služby a všechny závislosti.*

### <a name="credativ"></a>Credativ
[http://www.credativ.co.uk/credativ-blog/debian-Images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ je nezávislé poradě a služby společnosti, který se specializuje na hello k vývoji a implementaci řešení pro odborníky v oblasti s použitím bezplatný software. Jako počáteční open-source odborníky obsahuje Credativ mezinárodní rozpoznávání s řadu IT oddělení, které používají jejich podporu. Ve spojení se společností Microsoft Credativ aktuálně připravuje odpovídající Debian bitové kopie pro Debian 8 (Klára) a Debian před 7 (Wheezy). Obě speciálně navrženého toorun v Azure a obrázků se dají snadno spravovat prostřednictvím platformy hello. Credativ se také podporují hello dlouhodobé údržby a aktualizace hello Debian bitových kopií pro Azure prostřednictvím centra podpory jeho otevřený zdroj.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Strategie pro Oracle je toooffer široké portfolio řešení pro veřejné a privátní cloudy. strategie Hello poskytuje zákazníkům možnost volby a flexibilitu v tom, jak nasadit software Oracle v cloudech Oracle a ostatních cloudů. Oracle pro spolupráci se společností Microsoft umožňuje zákazníkům toodeploy Oracle software v Microsoft veřejných a privátních cloudů s jistotou hello certifikační a podpory z databáze Oracle.  Oracle na závazků a investice do řešení Oracle veřejného a privátního cloudu se nezmění.

### <a name="red-hat"></a>Red Hat
[http://www.RedHat.com/en/partners/strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Dobrý den světě přední poskytovatel řešení s otevřeným zdrojem, Red Hat pomáhá více než 90 % žádnou 500 společností vyřešit obchodní problémy, Zarovnat IT a obchodní strategie a příprava pro budoucí hello technologie. Red Hat tomu poskytování zabezpečeného řešení prostřednictvím otevřete obchodní model a model cenově dostupné, předvídatelný předplatné.

### <a name="suse"></a>SUSE
[http://www.SuSE.com/SuSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server v Azure je Principy platforma, která poskytuje vyšší spolehlivost a zabezpečení pro cloud computing. SUSE pro univerzální platformu Linux zajišťuje bezproblémovou integraci s Azure cloud services toodeliver snadno spravovatelné cloudové prostředí. Více než 9,200 certifikovaných aplikací z více než 1800 nezávislí dodavatelé softwaru pro SUSE Linux Enterprise Server SUSE zajistí, že úlohy běžící v datovém centru hello podporované můžou s jistotou nasazené na Azure.

### <a name="canonical"></a>Canonical
[http://www.Ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Kanonický inženýrství a otevřete komunity zásad správného řízení jednotka Ubuntu na úspěch v klienta, serveru a cloud computing, což zahrnuje osobní cloudové služby pro spotřebitele. Kanonický na vizi jednotné a bezplatná platforma v Ubuntu, z phone toocloud poskytuje řadu jednotné rozhraní pro hello telefon, tablet, TV a plochy. Tato vize díky Ubuntu hello první volbou pro různé instituce z osoby provádějící toohello poskytovatelů veřejného cloudu electronics příjemce a Oblíbené položky mezi jednotlivé technologists.

S vývojáři a engineering Center kolem hello, world Canonical je jednoznačně umístěného toopartner osoby provádějící hardwaru, poskytovatelů obsahu a softwaru vývojáři toobring Ubuntu řešení toomarket pro počítače, servery a kapesních zařízeních.
