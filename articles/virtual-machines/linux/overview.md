---
title: "aaaOverview virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Popisuje Azure Compute, úložiště a síťové služby s virtuální počítače s Linuxem."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: rickstercdn
manager: timlt
editor: 
ms.assetid: 7965a80f-ea24-4cc2-bc43-60b574101902
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 958e219d53026d787a7a41f2fd13c29c739a9238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-and-linux"></a>Azure a Linux
Microsoft Azure je rostoucí kolekce integrovaných veřejné cloudové služby, včetně analýzy, virtuálních počítačů, databází, mobilní a sítě, úložiště a webové&mdash;ideální pro hostování vašeho řešení.  Microsoft Azure poskytuje škálovatelný výpočetní platforma, která vám umožní tooonly platím co se používá, když chcete ho – bez nutnosti tooinvest v místní hardware.  Azure je připraven, pokud se tooscale řešení nahoru a out toowhatever škálování vyžadují tooservice hello potřebám vašim klientům.

Pokud jste se seznámili s hello různým funkcím služby Amazon na AWS, můžete zkontrolovat hello Azure vs AWS [definice mapování dokumentu](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

## <a name="regions"></a>Oblasti
Prostředky Microsoft Azure jsou rozmístěny v několika zeměpisných oblastí kolem hello, world.  "region" představuje více datových centrech v jedné zeměpisné oblasti.  Máme 34 oblastech je všeobecně dostupná kolem hello, world s další 4 oblastí, oznámí. Protože jsme se pokračováním tooexpand naše globální pokrytí - uchováváme, že aktualizovaný seznam stávajících a nově oznámeno oblasti.

* [Oblasti Azure](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Dostupnost
Jsme oznámila odvětví úvodní jedna instance virtuálního počítače smlouvy o úrovni služeb 99,9 %, pokud že nasazujete hello virtuálních počítačů pomocí úložiště premium pro všechny disky.  Aby vaše nasazení tooqualify pro naše standardní 99,95 % Smlouvy o úrovni služeb virtuálních počítačů, budete potřebovat toodeploy minimálně dva virtuální počítače spuštěné úlohy v rámci skupiny dostupnosti. Tím bude zajištěno, virtuální počítače jsou rozmístěny v několika domén selhání v našich datacentrech a také nasadit na hostitele se systémem windows odlišné údržby. Hello úplné [smlouva Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) vysvětluje hello záruku dostupnosti Azure jako celek.

## <a name="managed-disks"></a>Managed Disks

Spravované disky obslužných rutin Azure Storage vytváření účtů a správu hello pozadí pro vás a zajistí, že nemáte tooworry o limitech škálovatelnosti hello hello účtu úložiště. Jednoduše zadejte velikost disku hello a úroveň výkonu hello (Standard nebo Premium) a Azure vytváří a spravuje hello disku za vás. To i v případě přidat disky nebo škálovat nahoru a dolů hello virtuálních počítačů, nemáte tooworry o úložišti hello používá. Pokud vytváříte nové virtuální počítače, [použít hello Azure CLI 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo hello portálu toocreate Azure virtuální počítače s spravované operačního systému a datové disky. Pokud máte virtuální počítače s nespravované disky, můžete [převést vaše virtuální počítače toobe zálohovaný s spravované disky](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Můžete také spravovat vlastních bitových kopií v jeden účet úložiště na oblast Azure a jejich použití toocreate stovky virtuálních počítačů v hello stejného předplatného. Další informace o spravovaných disky, najdete v tématu hello [přehled disky spravované](../windows/managed-disks-overview.md).

## <a name="azure-virtual-machines--instances"></a>Virtuální počítače Azure & instance
Microsoft Azure podporuje spuštění počet oblíbených Linuxových distribucích spravovaný společností číslo partnerů.  Zjistíte distribuce například Red Hat Enterprise, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD a další obsah v hello Azure Marketplace. Jsme aktivně pracovat s různými tooadd komunit Linux i více typů toohello [schválené pro Azure distribucích systému Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) seznamu.

Pokud vaše upřednostňované distro Linux výběru není momentálně nachází v galerii hello, vám může "přineste vlastní Linux" virtuální počítač podle [vytvoření a nahrání virtuálního pevného disku Linux v Azure](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Virtuální počítače v Azure umožní toodeploy širokou škálu řešení výpočetní agilní způsobem. Můžete nasadit prakticky jakoukoli úlohu a jakýkoli jazyk na téměř všechny operační systém – Windows, Linux, nebo vytvořili vlastní z jednoho z našich rostoucí seznamu partnerů. Není stále zobrazen, co hledáte?  Nemusíte si dělat starosti – může také přinést si vlastní Image z místní.

## <a name="vm-sizes"></a>Velikosti virtuálních počítačů
Při nasazení virtuálního počítače v Azure, kterou budete tooselect velikost virtuálního počítače do jedné z našich řady velikostí, která je vhodná tooyour zatížení. velikost Hello ovlivní také hello zpracování napájení, paměti a kapacitu úložiště virtuálního počítače hello. Fakturuje se podle hello množství času hello používá a využívání jeho přidělené prostředky virtuálních počítačů. Úplný seznam [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tady jsou některé základní pokyny pro výběr velikost virtuálního počítače z jednoho z našich řady (A, D, DS, G a GS).
* Virtuální počítače, A-series jsou naše hodnotu za cenu vstupní úrovně virtuálních počítačů pro lehké úlohy a scénáře vývoje/testování. Jsou k dispozici ve všech oblastech a můžou se připojit a používat všechny standardní prostředky k dispozici toovirtual počítače.
* A-series velikosti (A8 - A11) jsou speciální výpočetní náročné konfigurace vhodná pro vysoce výkonné aplikace, výpočetní cluster.
* Virtuální počítače, D-series jsou navrženou toorun aplikace, které potřebují vyšší výpočetní výkon a výkon dočasné disku. Virtuální počítače D-series zadejte rychlejších procesorů vyšší poměr paměti jádra a na jednotku SSD (SSD) pro dočasným diskovým hello.
* Dv2-series, je hello nejnovější verzi naše D-series, funkce výkonnější procesor. Hello Dv2-series procesoru je asi 35 % rychlejší než hello D-series procesoru. Je založena na hello nejnovější generace 2.4 v3® GHz Intel Xeon E5-2673 procesoru (Haskell) a s hello Intel Turbo nárůst technologie 2.0, můžete přejít do too3.2 GHz. má Hello Dv2-series hello stejné konfigurace paměti a disku jako hello D-series.
* Virtuální počítače G-series nabízejí hello nejvíce paměti a spustit na hostitelích, které mají procesory Intel Xeon E5 V3 rodiny.

Poznámka: DS-series a GS-series virtuální počítače mají přístup tooPremium úložiště – naše SSD zálohovaný vysoce výkonné, nízkou latencí úložiště pro zatížení s intenzivním vstupně-výstupní operace. V některých oblastech je dostupná služba Storage úrovně Premium. Podrobnosti najdete tady:

* [Úložiště Premium: Vysoce výkonné úložiště pro úlohy virtuálního počítače Azure](../../storage/common/storage-premium-storage.md)

## <a name="automation"></a>Automation
tooachieve správné jazykovou verzi DevOps, všechny infrastruktury musí být kód.  Pokud všechny hello infrastruktury život v kódu můžete snadno se znovu vytvoří (Phoenix servery).  Azure funguje u všech hlavních automatizace hello tooling jako Ansible, Chef, Puppet a SaltStack.  Azure má také vlastní nástrojů pro automatizaci:

* [Šablony Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure zavádí podporu pro [cloudu init](http://cloud-init.io/) napříč většina distribucích systému Linux, které ji podporují.  Ubuntu Canonical na virtuální počítače jsou aktuálně nasazená s inicializací cloudu ve výchozím nastavení povolené.  Red klobouky RHEL, CentOS a Fedora podporují init cloudu, ale hello Azure Image udržované RedHat nemají cloudu inicializací nainstalována.  toouse cloudu inicializací na řadu RedHat operačního systému, je nutné vytvořit vlastní image s inicializací cloudu nainstalována.

* [Pomocí init cloudu na virtuálních počítačích Azure Linux](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quotas"></a>Kvóty
Každé předplatné Azure má výchozí kvótami v místě, ke kterému by mohlo mít vliv nasazení hello velký počet virtuálních počítačů pro váš projekt. aktuální limit Hello na základě za předplatné je 20 virtuálních počítačů podle oblastí.  Maximální kvóty mohou být vyvolány rychle a snadno o vyplnění lístek podpory požaduje limit zvýší.  Další informace o omezení kvót:

* [Omezení služby předplatného Azure](../../azure-subscription-service-limits.md)

## <a name="partners"></a>Partneři
Microsoft úzce spolupracuje s našimi partnery tooensure hello bitové kopie, které jsou aktualizovány a optimalizované pro Azure modulu runtime.  Další informace o našich partnerů zkontrolujte stránky své marketplace.

* Linux ve službě Azure - [distribuce schválené](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* SUSE - [Azure Marketplace - SUSE Linux Enterprise Server](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=%27SUSE%27)
* Red Hat - [Azure Marketplace - Red Hat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)
* Kanonický - [Azure Marketplace - LTS Ubuntu Server 16.04](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)
* Debian - [Azure Marketplace - 8 Debian "Klára"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)
* FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
* CoreOS - [Azure Marketplace - CoreOS (stabilní)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)
* RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)
* Bitnami - [Bitnami knihovny pro Azure.](https://azure.bitnami.com/)
* Mesosphere - [Azure Marketplace - Mesosphere DC/OS v Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)
* Docker - [Azure Marketplace - Azure Container Service pomocí Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)
* Volaných - [Azure Marketplace - CloudBees volaných platformy](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)

## <a name="getting-started-with-linux-on-azure"></a>Začínáme s Linuxem v Azure
toobegin pomocí Azure budete potřebovat účet Azure, hello nainstalované rozhraní příkazového řádku Azure a pár veřejného a privátního klíče SSH.

### <a name="sign-up-for-an-account"></a>Registrace účtu
Hello prvním krokem při použití hello cloudu Azure je toosign pro účet Azure.  Přejděte toohello [registraci účtu Azure](https://azure.microsoft.com/pricing/free-trial/) stránka tooget spuštěna.

### <a name="install-hello-cli"></a>Nainstalujte hello rozhraní příkazového řádku
S váš nový účet Azure můžete začít používat okamžitě pomocí portálu Azure, což je webové správce panely hello.  toomanage hello cloudu Azure prostřednictvím hello příkazového řádku, nainstalujete hello `azure-cli`.  Nainstalujte hello [Azure CLI 2.0](/cli/azure/install-azure-cli) na pracovní stanici Mac nebo Linux.

### <a name="create-an-ssh-key-pair"></a>Vytvoření páru klíčů SSH
Nyní máte účet Azure, hello Azure webový portál a hello rozhraní příkazového řádku Azure.  dalším krokem Hello je toocreate pár klíčů SSH, která je použité tooSSH do Linuxu bez použití hesla.  [Vytvoření klíčů SSH na Linuxu a Macu](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooenable heslo bez přihlášení a lepší zabezpečení.

### <a name="create-a-vm-using-hello-cli"></a>Vytvoření virtuálního počítače pomocí hello rozhraní příkazového řádku
Vytvoření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku hello je rychlý způsob toodeploy virtuální počítač bez terminálu výstupu hello pracujete v.  Všechny objekty, které můžete zadat na webový portál hello je k dispozici prostřednictvím příkazového řádku příznak nebo přepínač.  

* [Vytvoření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku hello](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="create-a-vm-in-hello-portal"></a>Vytvoření virtuálního počítače na portálu hello
Vytvoření virtuálního počítače s Linuxem v Azure webový portál hello je způsob tooeasily bodu a klikněte na tlačítko prostřednictvím hello různé možnosti tooget tooa nasazení.  Nepoužívejte příznaky příkazového řádku nebo přepínače jsou možné tooview dobrý webové rozložení různé možnosti a nastavení.  Všechno, co k dispozici prostřednictvím rozhraní příkazového řádku hello je k dispozici hello portálu.

* [Vytvoření virtuálního počítače s Linuxem pomocí hello portálu](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="login-using-ssh-without-a-password"></a>Přihlášení pomocí protokolu SSH bez zadání hesla
Hello virtuální počítač je nyní spuštěna v Azure a jsou připravené toolog v.  Pomocí hesla toolog v pomocí protokolu SSH je nezabezpečené a časově náročné.  Pomocí klíče SSH je nejbezpečnější způsob a také hello nejrychlejší způsob, jak toologin hello.  Můžete vytvořit virtuální počítač s Linuxem pomocí portálu hello nebo hello rozhraní příkazového řádku, máte dvě možnosti ověřování.  Pokud si zvolíte heslo SSH, nakonfiguruje Azure hello virtuálních počítačů tooallow přihlášení pomocí hesla.  Pokud jste zvolili toouse veřejný klíč SSH, Azure automaticky nakonfiguruje hello virtuálních počítačů tooonly povolit přihlášení pomocí protokolu SSH klíče a zakáže přihlášení heslo. Povolení přihlášení klíče SSH, použijte virtuálním počítačům s Linuxem pomocí pouze toosecure hello možnost veřejného klíče SSH při vytváření virtuálních počítačů v portálu hello nebo rozhraní příkazového řádku hello.

## <a name="related-azure-components"></a>Související Azure součásti
## <a name="storage"></a>Úložiště
* [Úvod tooMicrosoft Azure Storage](../../storage/common/storage-introduction.md)
* [Přidat tooa disku virtuálního počítače s Linuxem pomocí rozhraní azure příkazového řádku hello](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Jak tooattach datový disk tooa virtuálního počítače s Linuxem v hello portálu Azure](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Sítě
* [Přehled virtuálních sítí](../../virtual-network/virtual-networks-overview.md)
* [IP adresy v Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Otevření portů tooa virtuálního počítače s Linuxem v Azure](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření plně kvalifikovaný název domény v hello portálu Azure](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="containers"></a>Kontejnery
* [Virtuální počítače a kontejnerů v Azure](containers.md)
* [Azure Container Service Úvod](../../container-service/container-service-intro.md)
* [Nasazení clusteru Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md)

## <a name="next-steps"></a>Další kroky
Teď máte přehled systému Linux v Azure.  dalším krokem Hello je toodive v a vytvořit několik virtuálních počítačů!

* [Prozkoumejte rostoucí seznam ukázkové skripty pro běžné úkoly prostřednictvím AzureCLI](cli-samples.md)
