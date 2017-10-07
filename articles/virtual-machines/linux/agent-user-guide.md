---
title: "aaaAzure přehled agenta virtuálního počítače systému Linux | Microsoft Docs"
description: "Zjistěte, jak tooinstall a konfigurace agenta pro Linux (příkaz waagent) toomanage virtuální počítač interakci s Kontroleru prostředků infrastruktury Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: szark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4e08c84d9205f4db7aae6fd1568ec1f15fba395c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-and-using-hello-azure-linux-agent"></a>Informace o používání hello Azure Linux Agent
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Úvod
Hello Microsoft Azure Linux Agent (příkaz waagent) spravuje Linux & FreeBSD zřizování a virtuálních počítačů interakci s hello Kontroleru prostředků infrastruktury Azure. Poskytuje následující funkce pro nasazení systému Linux a FreeBSD IaaS hello:

> [!NOTE]
> Viz hello Azure Linux agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md) další podrobnosti.
> 
> 

* **Zřizování bitové kopie**
  
  * Vytvoření uživatelského účtu
  * Konfigurace typů ověřování SSH
  * Nasazení veřejné klíče SSH a páry klíčů
  * Název hostitele hello nastavení
  * Hello hostitele název toohello platforma pro publikování DNS
  * Generování sestav platforma toohello otisk prstu klíče SSH hostitele
  * Správa prostředků disku
  * Formátování a připojení hello prostředků disku
  * Konfigurace velikosti odkládacího souboru
* **Sítě**
  
  * Spravuje kompatibility tooimprove trasy se servery DHCP, platformy
  * Zajišťuje hello stabilitu hello název síťového rozhraní
* **Jádra**
  
  * Nakonfiguruje virtuální technologie NUMA (zakázat jádra < 2.6.37)
  * Využívá šifrování technologie Hyper-V pro /dev/random
  * Nakonfiguruje SCSI vypršení časových limitů pro zařízení hello kořenové, (který může být vzdálený)
* **Diagnostika**
  
  * Konzole přesměrování toohello sériového portu
* **Nasazení SCVMM**
  
  * Zjišťuje a bootstraps hello VMM agenta pro Linux v prostředí System Center Virtual Machine Manager 2012 R2
* **Rozšíření virtuálního počítače**
  
  * Vložit součásti autorem Microsoftu a partnerů do automation tooenable software a konfigurace virtuálního počítače s Linuxem (IaaS)
  * Odkaz na implementaci rozšíření virtuálního počítače na [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)

## <a name="communication"></a>Komunikace
prostřednictvím dvou kanálů dojde k Hello toku informací z agenta toohello hello platforem:

* Při spouštění počítače připojit DVD pro nasazení IaaS. Tento disk DVD zahrnuje kompatibilní se standardem OVF konfigurační soubor, který obsahuje všechny informace o zřizování než skutečná keypairs SSH hello.
* Koncový bod TCP vystavení rozhraní REST API používá tooobtain nasazení a konfiguraci topologie.

## <a name="requirements"></a>Požadavky
Hello následující systémy, které jsou známé toowork s hello Azure Linux Agent:

> [!NOTE]
> Tento seznam se můžou lišit od hello oficiální seznam podporovaných systémů na hello platforma Microsoft Azure podle postupu popsaného tady: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)
> 
> 

* CoreOS
* CentOS 6.3 +
* Red Hat Enterprise Linux 6.7 +
* Debian 7.0 +
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP3 +
* Oracle Linux 6.4 +

Jiné podporované systémy:

* FreeBSD 10 + (Azure Linux Agent v2.0.10 +)

Hello agenta systému Linux, závisí na některé balíčky systému v pořadí toofunction správně:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Nástroje systému souborů: čtvrceny sfdisk, fdisk, mkfs,
* Nástroje pro hesla: chpasswd, sudo
* Nástroje pro zpracování textu: menšit grep
* Síťové nástroje: trasy protokolu ip
* Podpora jádra pro připojení UDF systémy.

## <a name="installation"></a>Instalace
Instalace pomocí ot. / min nebo bázi DEB balíček z úložiště balíčků vaší distribuce je hello upřednostňovaný způsob instalace a upgrade hello Azure Linux Agent. Všechny hello [schválené distribuční zprostředkovatelé](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) balíček Azure Linux agent hello integrovat do svých bitové kopie a úložiště.

Naleznete v dokumentaci toohello v hello [Azure Linux Agent úložišti na Githubu](https://github.com/Azure/WALinuxAgent) pro Upřesnit možnosti instalace, například při instalaci z umístění zdroje nebo toocustom nebo předpony.

## <a name="command-line-options"></a>Možnosti příkazového řádku
### <a name="flags"></a>Příznaky
* verbose: zvýšit podrobnost zadaný příkaz
* Vynutit: přeskočit interaktivní potvrzení pro některé příkazy

### <a name="commands"></a>Příkazy
* Nápověda: uvádí hello podporované příkazy a značky.
* deprovision: Pokus tooclean hello systému a nastavit jej jako vhodný pro zřizování znovu. Tuto operaci odstraněny hello následující:
  
  * Všechny klíče SSH hostitele (Pokud Provisioning.RegenerateSshHostKeyPair 'y' v konfiguračním souboru hello)
  * Konfigurace názvový server v /etc/resolv.conf
  * Kořenové heslo z/etc/shadow (Pokud Provisioning.DeleteRootPassword 'y' v konfiguračním souboru hello)
  * V mezipaměti klienta zapůjčení DHCP
  * Resetování hostitele toolocalhost.localdomain název

> [!WARNING]
> Zrušení zřízení nezaručí, že této bitové kopie hello je nezaškrtnuté všech citlivých informací a je určená pro opětovnou distribuci.
> 
> 

* deprovision + uživatele: provede vše pod - deprovision (výše) a taky odstraní poslední účet zřízení uživatele hello (získaný z /var/lib/waagent) a související data. Tento parametr je při jeho rušení obrázek, který byl dříve zřizování v Azure, může být zachycen a znovu použít.
* verze: Zobrazí hello verzi příkaz waagent
* serialconsole: nakonfiguruje GRUB toomark ttyS0 (hello první sériového portu) jako konzola spouštěcí hello. To zajišťuje, že jsou protokoly spuštění jádra odeslané toothe sériového portu a k dispozici pro ladění.
* Démon: příkaz waagent spustit jako démon toomanage interakci s platformou hello. Tento argument je zadaný toowaagent ve skriptu init příkaz waagent hello.
* spustit: Spusťte příkaz waagent jako proces na pozadí

## <a name="configuration"></a>Konfigurace
Konfigurační soubor (nebo etc/waagent.conf) ovládacích prvků hello akce příkaz waagent. Ukázkový soubor konfigurace je zobrazena níže:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

Hello různé možnosti konfigurace jsou podrobně popsány v níže. Možnosti konfigurace jsou tři typy; Logická hodnota, řetězec nebo celé číslo. Možnosti konfigurace Boolean Hello lze zadat jako "y" nebo "n". Hello speciální – klíčové slovo "Žádný" může být použita pro některé řetězec typ konfigurace položky podle popisu níže.

**Provisioning.Enabled:**  
Typ: logická hodnota  
Výchozí: y

To umožňuje hello uživatele tooenable nebo zakázat hello zřizování funkce v agentovi hello. Platné hodnoty jsou "y" nebo "n". Pokud zřizování je zakázaná, hostitele a uživatelské klíče SSH hello obrázku jsou zachovány a veškeré zadané v hello Azure zřizování rozhraní API konfigurace je ignorována.

> [!NOTE]
> Hello `Provisioning.Enabled` výchozí hodnoty parametrů příliš "n" Image Ubuntu cloudu, který použít cloudové init pro zřizování.
> 
> 

**Provisioning.DeleteRootPassword:**  
Typ: logická hodnota  
Výchozí: n

Pokud sada hello kořenové heslo v souboru hello/etc/stínové vymazáním během hello procesu zřizování.

**Provisioning.RegenerateSshHostKeyPair:**  
Typ: logická hodnota  
Výchozí: y

Pokud sadu, všechny hostitele páry klíčů SSH (ecdsa, dsa a rsa), se odstraní při zřizování z etc/ssh/hello. A je generována jeden nový pár klíčů.

typ šifrování Hello pro nový pár klíčů hello je možné konfigurovat pomocí hello Provisioning.SshHostKeyPairType položku. Upozorňujeme, že některé distribuce bude znovu vytvořit páry klíčů SSH pro všechny chybějící typy šifrování při restartování démon procesu SSH hello (třeba po restartování).

**Provisioning.SshHostKeyPairType:**  
Typ: Řetězec  
Výchozí: rsa

To je možné nastavit tooan šifrovací algoritmus typ, který je podporován proces démon programu SSH hello hello virtuálního počítače. Hello obvykle podporované hodnoty jsou "rsa", "dsa" a "ecdsa". Všimněte si, že "putty.exe" v systému Windows nepodporuje "ecdsa". Ano Pokud máte v úmyslu toouse putty.exe na Windows tooconnect tooa Linux nasazení, použijte "rsa" nebo "dsa".

**Provisioning.MonitorHostName:**  
Typ: logická hodnota  
Výchozí: y

Pokud nastavíte, příkaz waagent bude monitorovat hello Linux virtuálního počítače pro název hostitele změny (jak vrácené příkazem "název hostitele" hello) a automaticky aktualizovat hello konfigurace sítí v hello image tooreflect hello změn. V názvu hello toopush pořadí změnit toohello servery DNS, sítě bude restartována v hello virtuálního počítače. To způsobí Stručný postup ke ztrátě připojení k Internetu.

**Provisioning.DecodeCustomData**  
Typ: logická hodnota  
Výchozí: n

Pokud nastavíte, příkaz waagent bude dekódovat CustomData z formátu Base64.

**Provisioning.ExecuteCustomData**  
Typ: logická hodnota  
Výchozí: n

Pokud nastavíte, příkaz waagent provede CustomData po zřízení.

**Provisioning.PasswordCryptId**  
Typ: řetězec  
Výchozí: 6

Algoritmus používaný crypt při generování hodnoty hash hesla.  
 1 - ALGORITMUS MD5  
 2a - Blowfish  
 5 - SHA-256  
 6 - SHA-512  

**Provisioning.PasswordCryptSaltLength**  
Typ: řetězec  
Výchozí: 10

Délka náhodných salt používá při generování hodnoty hash hesla.

**ResourceDisk.Format:**  
Typ: logická hodnota  
Výchozí: y

Pokud nastavíte, hello prostředků disku poskytované hello platformy bude formátu a připojené pomocí příkaz waagent, pokud je typ systému souborů hello požadoval uživatel hello v "ResourceDisk.Filesystem" než "ntfs". Jeden oddíl typu Linux (83) bude k dispozici na disku hello. Všimněte si, že tento oddíl nebude naformátovaný, pokud ho můžete úspěšně připojit.

**ResourceDisk.Filesystem:**  
Typ: Řetězec  
Výchozí: ext4

Určuje typ systému souborů hello hello prostředků disku. Podporované hodnoty se liší podle distribuce systému Linux. Pokud je řetězec hello X, potom mkfs. X by měla být k dispozici na bitovou kopii systému Linux hello. Bitové kopie SLES 11 by měl obvykle používají 'ext3'. Obrázky FreeBSD tady by měl použít 'ufs2'.

**ResourceDisk.MountPoint:**  
Typ: Řetězec  
Výchozí hodnota: / mnt nebo prostředků 

Toto nastavení určuje hello cestu, kde je připojena hello prostředků disku. Všimněte si, je tento disk hello prostředků *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů.

**ResourceDisk.MountOptions**  
Typ: Řetězec  
Výchozí: žádná

Určuje disku připojení možnosti toobe předán toohello připojení -o příkaz. Toto je čárkami oddělený seznam hodnot, např. 'nodev, nosuid'. V tématu mount(8) podrobnosti.

**ResourceDisk.EnableSwap:**  
Typ: logická hodnota  
Výchozí: n

Pokud nastavit odkládacího souboru (/ swapfile) je vytvořen na prostředku disku hello a přidat systému toohello velikosti odkládacího souboru.

**ResourceDisk.SwapSizeMB:**  
Typ: celé číslo  
Výchozí: 0

velikost Hello hello odkládacího souboru v megabajtech.

**Logs.Verbose:**  
Typ: logická hodnota  
Výchozí: n

Pokud je boosted sady podrobností protokolu. Příkaz Waagent protokoly too/var/log/waagent.log a využívá protokoly toorotate hello systému logrotate funkce.

**OPERAČNÍ SYSTÉM. EnableRDMA**  
Typ: logická hodnota  
Výchozí: n

Pokud nastavíte, hello agent bude pokus tooinstall a pak můžete načíst ovladač jádra RDMA, která odpovídá hello verzi firmwaru hello na hello základní hardware.

**OPERAČNÍ SYSTÉM. RootDeviceScsiTimeout:**  
Typ: celé číslo  
Výchozí: 300

Tím se nakonfiguruje hello SCSI vypršení časového limitu v sekundách na disku a datové jednotky hello operačního systému. Pokud není nastavena, hello systému, které budou použity výchozí hodnoty.

**OPERAČNÍ SYSTÉM. OpensslPath:**  
Typ: Řetězec  
Výchozí: žádná

To může být použité toospecify alternativní cestu pro binární toouse hello openssl pro kryptografické operace.

**HttpProxy.Host HttpProxy.Port**  
Typ: Řetězec  
Výchozí: žádná

Pokud nastavíte, hello agent použije tento proxy server tooaccess hello Internetu. 

## <a name="ubuntu-cloud-images"></a>Ubuntu cloudu obrázků
Všimněte si, že Ubuntu cloudu Image využívat [cloudu init](https://launchpad.net/ubuntu/+source/cloud-init) tooperform mnoho úlohy konfigurace, které by jinak spravovány nástrojem hello Azure Linux Agent.  Je třeba počítat hello následující rozdíly:

* **Provisioning.Enabled** příliš "n" Image Ubuntu cloudu, využívající cloudu init tooperform zřizování úlohy výchozí hodnoty.
* Hello následující konfigurační parametry nemají vliv na cloudu bitové kopie Ubuntu, použít cloudové init toomanage hello prostředků disku a velikost odkládacího souboru místo:
  
  * **ResourceDisk.Format**
  * **ResourceDisk.Filesystem**
  * **ResourceDisk.MountPoint**
  * **ResourceDisk.EnableSwap**
  * **ResourceDisk.SwapSizeMB**
* Zobrazit hello následující prostředky tooconfigure hello prostředků disku přípojného bodu a záměna prostoru Ubuntu cloudu Image při zřizování:
  
  * [Ubuntu Wiki: Konfigurace oddílů Swap](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [Vložení vlastní Data do virtuálního počítače Azure](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

