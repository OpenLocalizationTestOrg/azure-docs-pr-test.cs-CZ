---
title: "aaaTesting SAP NetWeaver na virtuálních počítačích Microsoft Azure SUSE Linux | Microsoft Docs"
description: "Testování SAP NetWeaveru na virtuálních počítačích Microsoft Azure se SUSE Linuxem"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 645e358b-3ca1-4d3d-bf70-b0f287498d7a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: hermannd
ms.openlocfilehash: 0e3faab05417a1a15541e2b79aa7eddacda44611
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Provoz SAP NetWeaveru na virtuálních počítačích Microsoft Azure se SUSE Linuxem
Tento článek popisuje různé věci tooconsider, když používáte SAP NetWeaver na Microsoft Azure SUSE Linux virtuální počítače (VM). Od 19 2016 může SAP NetWeaver oficiálně podporované v SUSE virtuální počítače s Linuxem v Azure. Všechny podrobnosti týkající se verze Linux, verze SAP jádra a tak dále najdete v 1928533 Poznámka SAP "SAP aplikace v Azure: podporované produkty a typy virtuálních počítačů Azure".
Další dokumentaci o SAP na virtuální počítače s Linuxem naleznete zde: [pomocí SAP na Linuxové virtuální počítače (VM)](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Hello následující informace by měly pomoci vyhnout se některé Potenciální nástrahy.

## <a name="suse-images-on-azure-for-running-sap"></a>SUSE Image na platformě Azure pro spuštění SAP
Pro SAP NetWeaver spuštěné v Azure, použijte pouze SUSE Linux Enterprise Server SLES 12 (SPx) – viz poznámka SAP 1928533 také. Speciální SUSE image je v hello Azure Marketplace ("SLES 11 SP3 pro SAP CAL"), ale tento není určený pro obecné použití. Nepoužívejte tuto bitovou kopii, protože je vyhrazený pro hello [knihovny zařízení cloudu SAP](https://cal.sap.com/) řešení.  

Měli byste použít Správce prostředků Azure pro všechny nové testy a instalací v Azure. toolook pro SUSE SLES bitové kopie a verze pomocí prostředí Azure PowerShell nebo hello rozhraní příkazového řádku Azure (CLI), hello použijte následující příkazy. Pak můžete použít výstup hello, například toodefine hello bitovou kopii operačního systému v šabloně JSON pro nasazení nového virtuálního počítače SUSE Linux.
Tyto příkazy prostředí PowerShell jsou platné pro prostředí Azure PowerShell verze 1.0.1 a novější.

I když je stále možné toouse hello standardní SLES bitové kopie pro SAP instalace doporučujeme použití toomake hello nové SLES pro SAP bitové kopie, které jsou k dispozici nyní na hello Azure obrázek Galerie. Další informace o těchto bitových kopií naleznete na odpovídající hello [stránky Azure Marketplace]( https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SUSE.SLES-SAP ) nebo hello [webové stránky SUSE – nejčastější dotazy o SLES pro SAP]( https://www.suse.com/products/sles-for-sap/frequently-asked-questions/ ).


* Vyhledejte existující vydavatelů, včetně SUSE:
  
   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```
* Vyhledejte existující nabídky z SUSE:
  
   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```
* Vyhledejte SUSE SLES nabídky:
  
   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP"
   CLI : azure vm image list-skus westeurope SUSE SLES
   CLI : azure vm image list-skus westeurope SUSE SLES-SAP
   ```
* Podívejte se na konkrétní verzi SLES SKU:
  
   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12-SP2"
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP" -skus "12-SP2"
   CLI : azure vm image list westeurope SUSE SLES 12-SP2
   CLI : azure vm image list westeurope SUSE SLES-SAP 12-SP2
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Instalace WALinuxAgent ve virtuálním počítači SUSE
agent Hello názvem WALinuxAgent je součástí hello SLES obrázků v hello Azure Marketplace. Informace o instalaci ručně (například při nahrávání operačním systémem SLES virtuální pevný disk (VHD) z místní) najdete v tématu:

* [OpenSUSE](http://software.opensuse.org/package/WALinuxAgent)
* [Azure](../../linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SUSE](https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP "rozšířené monitorování"
SAP "rozšířené, monitorování" je povinné požadovaných toorun SAP v Azure. Zkontrolujte, jestli Všimněte si podrobnosti v SAP, 2191498 "SAP na Linux s Azure: rozšířené monitorování".

## <a name="attaching-azure-data-disks-tooan-azure-linux-vm"></a>Připojování dat Azure tooan disky virtuálního počítače Azure s Linuxem
Tooan dat Azure disky virtuálního počítače s Linuxem Azure by nikdy připojit pomocí ID hello zařízení. Místo toho použijte identifikátor UUID hello (UUID). Pokud používáte nástroje s grafickým rozhraním toomount Azure datových disků, například buďte opatrní. Zkontrolujte položky hello v /etc/fstab.

Hello problém s ID zařízení hello je, že mohou změnit, a pak může přečnívat hello virtuálního počítače Azure v procesu spouštění hello. toomitigate hello problém, můžete přidat parametr nofail hello v /etc/fstab. Ale pozor s nofail vzhledem k tomu, že aplikace může použít hello přípojného bodu jako před a které může zapisovat do systému souborů kořenové hello v případě, že během spouštění hello nebyl připojené externí Azure datový disk.

Jedinou výjimkou toomounting Hello prostřednictvím UUID je připojení disku operačního systému při odstraňování potíží, jak je popsáno v následující části hello.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Řešení potíží s SUSE virtuální počítač, který už není dostupný
Můžou nastat situace, kdy SUSE virtuálního počítače na platformě Azure přestane reagovat v procesu spouštění hello (například s chybu týkající se připojení disků). Tento problém můžete ověřit pomocí funkci hello spouštění diagnostiky pro virtuální počítače Azure v2 v hello portálu Azure. Další informace najdete v tématu [spouštění diagnostiky](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Jedním ze způsobů toosolve hello problém je, že disk s operačním systémem hello tooattach z hello poškozený tooanother počítač SUSE virtuálního počítače na platformě Azure. Proveďte potřebné změny, jako jsou úpravy /etc/fstab nebo odebíráním pravidel udev sítě, jak je popsáno v další části hello.

Neexistuje jeden tooconsider důležité. Nasazení několik virtuálních počítačů SUSE z hello stejnou bitovou kopii Azure Marketplace (například SLES 11 SP4) způsobí, že hello operačního systému připojit disk tooalways podle hello stejným UUID. Proto pomocí tooattach hello UUID disku operačního systému z různých virtuálního počítače, který byl nasazen pomocí hello stejnou bitovou kopii Azure Marketplace bude mít za následek dva identické identifikátory UUID. To způsobuje problémy a může znamenat, že hello virtuálních počítačů, které jsou určené výhradně pro řešení potíží se ve skutečnosti spustí ze hello připojen a poškozeného disku operačního systému místo hello původní.

Existují dva způsoby tooavoid toto:

* Použijte jinou bitovou kopii Azure Marketplace pro hello řešení potíží s virtuálního počítače (například SLES 11 SPx místo SLES 12).
* Nemáte připojení hello poškozený disk operačního systému z jiného virtuálního počítače pomocí UUID – použití něco jiného.

## <a name="uploading-a-suse-vm-from-on-premises-tooazure"></a>Odesílání SUSE virtuálního počítače z místní tooAzure
Popis hello kroky tooupload SUSE virtuálního počítače z místní tooAzure najdete v tématu [Příprava virtuálního počítače, SLES nebo openSUSE pro Azure](../../linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Pokud chcete, aby tooupload virtuální počítač bez hello deprovision krok na konci hello (například tookeep existující instalaci SAP, stejně jako název hostitele hello), zkontrolujte hello následující položky:

* Ujistěte se, že tento disk hello OS připojený pomocí UUID a není hello ID zařízení. Změna tooUUID právě v /etc/fstab není dost pro disk s operačním systémem hello. Nezapomeňte také tooadapt hello spouštěcí zavaděč prostřednictvím YaST nebo úpravou /boot/grub/menu.lst.
* Pokud používáte formát VHDX hello pro disk s operačním systémem SUSE hello a převeďte ho tooVHD pro nahrávání tooAzure, je velmi pravděpodobné, že zařízení sítě hello se změní ze eth0 tooeth1. tooavoid problémy při v Azure se spouští později změnit back tooeth0, jak je popsáno v [opravě eth0 v klonovat SLES 11 VMware](https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Kromě toho je toowhat popsanou v článku hello, doporučujeme odebrat toto:

   /lib/udev/rules.d/75-persistent-NET-Generator.Rules

Hello Azure Linux Agent (příkaz waagent) toohelp vyhnout potenciální problémy, můžete také nainstalovat, dokud nejsou k dispozici několik síťových adaptérů.

## <a name="deploying-a-suse-vm-on-azure"></a>Nasazení virtuálního počítače SUSE v Azure
Nové virtuální počítače SUSE musí vytvořit pomocí šablony soubory JSON v novém modelu Azure Resource Manager hello. Po vytvoření souboru šablony JSON hello můžete nasadit hello virtuálních počítačů pomocí hello následující příkaz rozhraní příkazového řádku jako alternativní tooPowerShell:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Další podrobnosti o souborech JSON šablony najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../../../resource-group-authoring-templates.md) a [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).

Další informace o rozhraní příkazového řádku a Azure Resource Manager najdete v tématu [hello použití Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](../../../xplat-cli-azure-resource-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP klíč licence a hardwaru
Hello oficiální SAP Azure certifikační nového mechanismu nemohla přináší toocalculate hello SAP hardwaru klíč, který se používá pro hello SAP licenci. Hello SAP jádra měl toobe přizpůsobit toomake použití tohoto objektu. Dřívější verze SAP jádra pro Linux neobsahuje tato změna kódu. Proto v určitých situacích (třeba virtuální počítač Azure Změna velikosti), hello SAP hardwaru klíč změnit a hello SAP licence byla již nebude platný. To je vyřešen v hello nejnovější SAP Linux jádra. Podrobnosti zkontrolujte Poznámka SAP 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>Balíček sapconf SUSE / přizpůsobená adm
SUSE poskytuje balíček s názvem "sapconf", který spravuje sadu nastavení specifických pro SAP. Pro další podrobnosti o jaké tohoto balíčku a jak tooinstall a jeho použití naleznete v tématu [pomocí sapconf tooprepare systémy SUSE Linux Enterprise Server toorun SAP](https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) a [co je sapconf nebo jak tooprepare operačního systému SUSE Linux Enterprise Server pro systémy SAP? ](http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

V hello zatím je nový nástroj, který nahrazuje sapconf - přizpůsobená adm. Jeden můžete najít další podrobnosti o tomto nástroji následující hello dva odkazy níže.

SLES dokumentaci o přizpůsobená adm profil sap-hana lze nalézt [sem](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

Ladění systémy pro SAP úloh s přizpůsobená adm - naleznete [sem](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) v kapitole 6.2

## <a name="nfs-share-in-distributed-sap-installations"></a>V distribuované instalaci SAP sdílených složek NFS
Pokud máte distribuované instalaci – například, kam chcete tooinstall hello databáze a hello SAP aplikační servery v samostatné virtuální počítače – můžete sdílet hello /sapmnt directory prostřednictvím systému souborů NFS. Pokud máte problémy s kroky instalace hello po vytvoření sdílených složek NFS pro /sapmnt hello, zkontrolujte toosee, pokud je pro sdílenou složku hello nastavené "no_root_squash".

## <a name="logical-volumes"></a>Logické svazky
V posledních hello Pokud logické značný zapotřebí na více disků dat Azure (například pro databázi SAP hello), se doporučuje toouse mdadm jako lvm nebyla ověřena plně ještě v Azure. jak zjistit, tooset až Linux RAID na platformě Azure pomocí mdadm, toolearn [konfigurace softwaru diskového pole RAID v systému Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). V hello zatím od začátku 2016 může také lvm je plně podporovaný v Azure a lze použít jako alternativní toomdadm. Další informace týkající se lvm v Azure najdete v části [konfigurace LVM na virtuální počítač s Linuxem v Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-suse-repository"></a>Úložiště Azure SUSE
Pokud máte potíže s přístup toohello standardní Azure SUSE úložiště, můžete použít jednoduchý příkaz tooreset ho. K tomu může dojít, pokud vytvoříte privátní bitové kopie operačního systému do oblasti Azure a pak kopírování hello image tooa jiné oblasti místo toodeploy nové virtuální počítače, které jsou založeny na tuto privátní bitovou kopii operačního systému. Stačí spusťte následující příkaz uvnitř hello virtuálních počítačů hello:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome plochy
Pokud chcete toouse hello Gnome plochy tooinstall celý systém ukázku SAP uvnitř jeden virtuální počítač – včetně SAP grafickým uživatelským rozhraním, prohlížeče a konzoly pro správu SAP – použijte tuto nápovědu tooinstall ho Image Azure SLES hello:

   Pro SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Pro SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-hello-cloud"></a>SAP podpora pro Oracle v systému Linux v cloudu hello
Je omezení podpory z databáze Oracle v systému Linux ve virtualizovaných prostředích. I když to není tématu specifické pro Azure, je důležité toounderstand. SAP nepodporuje Oracle na SUSE nebo Red Hat ve veřejném cloudu, jako je například Azure. toodiscuss tohoto tématu, obraťte se přímo Oracle.

