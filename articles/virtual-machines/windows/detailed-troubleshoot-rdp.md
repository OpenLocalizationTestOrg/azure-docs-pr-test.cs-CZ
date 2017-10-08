---
title: "Vzdálená plocha aaaDetailed odstraňování potíží v Azure | Microsoft Docs"
description: "Přečtěte si podrobné řešení potíží pro vzdálené plochy chyby, kde nemůžete tooa Windows virtuálních počítačů v Azure"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: "nelze připojit tooremote plochy, řešení potíží s vzdálené plochy, nemůže připojit ke vzdálené ploše, vzdálené plochy chyby, řešení potíží vzdálené plochy, potížích se vzdálenou plochou"
ms.assetid: 9da36f3d-30dd-44af-824b-8ce5ef07e5e0
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: support-article
ms.date: 07/25/2017
ms.author: genli
ms.openlocfilehash: fcb0d06aa66b748f3ebbbbe3431471d3cbe7c60d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-toowindows-vms-in-azure"></a>Podrobný postup řešení potíží pro připojení ke vzdálené ploše problémy tooWindows virtuálních počítačů v Azure
Tento článek obsahuje podrobný postup řešení potíží toodiagnose a opravte komplexní chyby Vzdálená plocha systému Windows Azure virtuálních počítačů.

> [!IMPORTANT]
> tooeliminate hello více běžných chyb vzdálené plochy, ujistěte se, že tooread [hello základní článku Poradce při potížích pro vzdálenou plochu](troubleshoot-rdp-connection.md) než budete pokračovat.

Může dojít ke vzdálené ploše chybovou zprávu, která není vypadat hello specifické chybové zprávy popsané v [hello základní Vzdálená plocha Průvodce odstraňováním potíží s](troubleshoot-rdp-connection.md). Postupujte podle těchto kroků toodetermine proč hello klienta vzdálené plochy (RDP) je služba protokolu RDP nelze tooconnect toohello na hello virtuálního počítače Azure.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello MSDN Azure a hello Stack Overflow fóra](https://azure.microsoft.com/support/forums/). Alternativně můžete také soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na tlačítko **získat podporu**. Informace o používání podporu Azure, najdete v tématu hello [Microsoft Azure Support – nejčastější dotazy](https://azure.microsoft.com/support/faq/).

## <a name="components-of-a-remote-desktop-connection"></a>Součásti připojení vzdálené plochy
připojení ke vzdálené ploše podílí Hello následující součásti:

![](./media/detailed-troubleshoot-rdp/tshootrdp_0.png)

Než budete pokračovat, může pomoci toomentally zkontrolujte, co se změnilo od hello poslední úspěšné připojení ke vzdálené ploše připojení toohello virtuálních počítačů. Například:

* Hello veřejnou IP adresu hello virtuálního počítače nebo hello cloudové služby, která obsahuje hello virtuálních počítačů (také nazývané hello virtuální IP adresy [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) se změnila. Hello RDP selhání může být způsobeno vaší mezipaměti klienta DNS má stále hello *starou IP adresu* zaregistrovat pro název DNS hello. Vyprázdnění mezipaměti klienta DNS a zkuste se znovu připojit hello virtuálních počítačů. Nebo se připojte přímo s novou VIP hello.
* Používáte aplikace třetích stran toomanage vaše připojení ke vzdálené ploše místo použití hello připojení vygenerované hello portálu Azure. Ověřte, že tato konfigurace aplikace hello zahrnuje hello správný port TCP pro hello provoz vzdálené plochy. Můžete zkontrolovat tento port pro klasické virtuální počítač v hello [portál Azure](https://portal.azure.com), kliknutím na nastavení hello Virtuálního počítače > Koncové body.

## <a name="preliminary-steps"></a>Předběžné kroky
Před pokračováním toohello podrobné řešení problémů

* Zkontrolujte stav hello hello virtuálního počítače v hello portál Azure pro všechny zřejmé problémy.
* Postupujte podle hello [oprava rychlé kroky pro běžné chyby RDP v základním řešení problémů hello průvodce](troubleshoot-rdp-connection.md#quick-troubleshooting-steps).

Zkuste se znovu připojit toohello virtuálního počítače prostřednictvím vzdálené plochy po provedení těchto kroků.

## <a name="detailed-troubleshooting-steps"></a>Podrobný poradce při potížích
klient vzdálené plochy Hello nemusí být možné tooreach hello služby Vzdálená plocha na hello virtuálního počítače Azure kvůli tooissues v hello následující zdroje:

* [Klient služby Vzdálená plocha počítače](#source-1-remote-desktop-client-computer)
* [Hraniční zařízení intranetu organizace](#source-2-organization-intranet-edge-device)
* [Koncový bod služby v cloudu a přístup k seznamu řízení (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Skupiny zabezpečení sítě](#source-4-network-security-groups)
* [Systém Windows Azure virtuálních počítačů](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>Zdroj 1: Klient služby Vzdálená plocha počítače
Ověřte, že váš počítač provádět Vzdálená plocha připojení tooanother místně, počítače se systémem Windows.

![](./media/detailed-troubleshoot-rdp/tshootrdp_1.png)

Pokud není, můžete zkontrolujte hello následující nastavení v počítači:

* Nastavení místní brány firewall, které blokuje provoz vzdálené plochy.
* Místně nainstalován klientský software proxy serveru, který brání připojení ke vzdálené ploše.
* Místně nainstalován software, který brání připojení ke vzdálené ploše monitorování sítě.
* Jiné typy zabezpečení softwaru, které monitorování provozu nebo povolí nebo zakáže určité typy přenosů, který brání připojení ke vzdálené ploše.

V těchto případech dočasně zakázat hello softwaru a zkuste to tooconnect tooan místním počítači přes vzdálenou plochu. Pokud můžete zjistit skutečný příčina hello tímto způsobem, pracujete s vaší sítě Správce toocorrect hello softwaru nastavení tooallow připojení ke vzdálené ploše.

## <a name="source-2-organization-intranet-edge-device"></a>Zdroj 2: Organizace intranetu hraniční zařízení
Ověřte, zda počítač přímo připojen toohello, které má Internet tooyour připojení ke vzdálené ploše virtuálního počítače Azure.

![](./media/detailed-troubleshoot-rdp/tshootrdp_2.png)

Pokud nemáte počítač, který je přímo připojený toohello Internetu, vytvoření a testování pomocí Azure nového virtuálního počítače v prostředku skupiny nebo cloudovou službu. Další informace najdete v tématu [vytvoření virtuálního počítače se systémem Windows v Azure](../virtual-machines-windows-hero-tutorial.md). Hello virtuálního počítače a skupiny prostředků hello nebo hello Cloudová služba, můžete odstranit po testu hello.

Pokud je možné vytvořit připojení vzdálené plochy s počítačem přímo připojené toohello Internetu, zkontrolujte vaše organizace intranetu hraniční zařízení pro:

* K interní firewall blokující toohello připojení HTTPS Internetu.
* Proxy server, který brání připojení ke vzdálené ploše.
* Software spuštěný na zařízení ve vaší hraniční síti, která brání připojení ke vzdálené ploše monitorování sítě nebo zjišťování neoprávněných vniknutí.

Spolupracovat s vaší správce toocorrect hello nastavení sítě vaší organizace intranetu hraniční zařízení tooallow založený na protokolu HTTPS vzdálené plochy připojení toohello Internetu.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Zdroj 3: Koncový bod služby Cloud a seznamu ACL
Pro virtuální počítače vytvořené pomocí modelu nasazení Classic hello, ověřte, zda, jiný virtuální počítač Azure, která je v hello stejné cloudové služby nebo virtuální sítě můžete provést tooyour připojení ke vzdálené ploše virtuálního počítače Azure.

![](./media/detailed-troubleshoot-rdp/tshootrdp_3.png)

> [!NOTE]
> Pro virtuální počítače vytvořené ve službě Správce prostředků, přeskočte příliš[zdroj 4: skupin zabezpečení sítě](#source-4-network-security-groups).

Pokud jste jiný virtuální počítač v hello stejné cloudové služby nebo virtuální sítě, vytvořte ho. Postupujte podle kroků hello v [vytvoření virtuálního počítače se systémem Windows v Azure](../virtual-machines-windows-hero-tutorial.md). Odstraňte hello testovací virtuální počítač po dokončení testu hello.

Pokud se můžete připojit přes vzdálenou plochu tooa virtuálního počítače v hello stejné cloudové služby nebo virtuální sítě, zkontrolujte tato nastavení:

* konfigurace koncového bodu Hello přenosům vzdálené plochy na hello cílovém virtuálním počítači: hello privátní port TCP hello koncového bodu musí odpovídat portu TCP hello, na které hello naslouchá služby Vzdálená plocha Virtuálního počítače (výchozí hodnota je 3389).
* Hello seznamu ACL pro koncový bod provoz vzdálené plochy hello na cíli hello virtuálních počítačů: seznamy ACL umožňují toospecify povolené nebo zakázané příchozí přenosy z Internetu na základě jeho zdrojové IP adresy hello. Nesprávně nakonfigurované seznamy ACL můžete zabránit příchozí vzdálené plochy provoz toohello koncový bod. Zkontrolujte vaší tooensure seznamy ACL, který je povolen příchozí provoz z veřejné IP adresy proxy nebo jiný server edge. Další informace najdete v tématu [co je seznamu pro řízení přístupu sítě (ACL)?](../../virtual-network/virtual-networks-acl.md)

toocheck Pokud koncový bod hello je hello zdroj hello problém, odeberte hello aktuální koncový bod a vytvořte novou, výběrem náhodný port v hello rozsahu 49152 až 65535 pro externí port číslo hello. Další informace najdete v tématu [jak tooset koncové body tooa virtuálního počítače](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="source-4-network-security-groups"></a>Zdroje 4: Skupiny zabezpečení sítě
Skupiny zabezpečení sítě povolit podrobnější řízení povolené příchozí a odchozí přenosy. Můžete vytvořit pravidla pokrývání uzlů podsítě a cloudových služeb v virtuální sítě Azure.

Použití [IP tok ověření](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) tooconfirm Pokud pravidla v skupinu zabezpečení sítě blokuje tooor provoz z virtuálního počítače. Můžete také zkontrolovat efektivní zabezpečení skupiny pravidel tooensure příchozí "Povolit" NSG pravidel existuje a prioritu pro RDP port (standardně 3389). Další informace najdete v tématu [tok provozu pomocí efektivní pravidla zabezpečení tootroubleshoot virtuálních počítačů](../../virtual-network/virtual-network-nsg-troubleshoot-portal.md#using-effective-security-rules-to-troubleshoot-vm-traffic-flow).

## <a name="source-5-windows-based-azure-vm"></a>Zdroj 5: Založené na systému Windows virtuálního počítače Azure
![](./media/detailed-troubleshoot-rdp/tshootrdp_5.png)

Postupujte podle pokynů hello v [v tomto článku](reset-rdp.md). Tento článek resetuje hello služby Vzdálená plocha na virtuálním počítači hello:

* Povolte pravidlo výchozí brány Windows Firewall "Vzdálené plochy" hello (TCP port 3389).
* Povolte připojení ke vzdálené ploše nastavením too0 hodnotu registru HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections hello.

Zkuste hello připojení z vašeho počítače znovu. Pokud jste stále nemůžete tooconnect přes vzdálenou plochu, zkontrolujte hello následující možné problémy:

* Hello služby Vzdálená plocha není spuštěna na cíli hello virtuálních počítačů.
* Hello služby Vzdálená plocha není naslouchá na portu TCP 3389.
* Brána Windows Firewall nebo jiné místní brány firewall má odchozí pravidlo, které brání provoz vzdálené plochy.
* Zjišťování neoprávněných vniknutí nebo monitorování softwaru systémem hello virtuální počítač Azure sítě brání připojení ke vzdálené ploše.

Pro virtuální počítače vytvořené pomocí modelu nasazení classic hello můžete použít toohello vzdálené relace prostředí Azure PowerShell virtuální počítač Azure. Nejprve je třeba tooinstall certifikát pro cloudové služby hostování hello virtuálního počítače. Přejděte příliš[tooAzure konfigurovat zabezpečení vzdáleného prostředí PowerShell přístup virtuálních počítačů](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) a stáhnout hello **InstallWinRMCertAzureVM.ps1** skriptu souboru tooyour místního počítače.

Pokud jste to ještě neudělali, potom nainstalujte Azure PowerShell. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

Dále otevřete příkazový řádek prostředí Azure PowerShell a změňte hello aktuální toohello umístění složky hello **InstallWinRMCertAzureVM.ps1** souboru skriptu. toorun skript Azure PowerShell, musíte nastavit zásady správné spouštění hello. Spustit hello **Get-ExecutionPolicy** příkaz toodetermine vaše aktuální úroveň zásad. Informace o nastavení odpovídající úroveň hello najdete v tématu [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).

V dalším kroku zadat název vašeho předplatného Azure, hello název cloudové služby a název vašeho virtuálního počítače (odebrání hello < a > znaků) a potom spuštěním těchto příkazů.

```powershell
$subscr="<Name of your Azure subscription>"
$serviceName="<Name of hello cloud service that contains hello target virtual machine>"
$vmName="<Name of hello target virtual machine>"
.\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName
```

Můžete získat název správné předplatné hello hello *Název_předplatného* vlastnost hello zobrazení hello **Get-AzureSubscription** příkaz. Název cloudové služby hello hello virtuálního počítače můžete získat z hello *ServiceName* sloupce v zobrazení hello hello **Get-AzureVM** příkaz.

Zkontrolujte, pokud máte hello nový certifikát. Otevřete modul snap-in Certifikáty pro aktuálního uživatele hello a zkontrolujte hello **Trusted Root Certification Authorities\Certificates** složky. Měli byste vidět certifikát s názvem DNS hello vaše cloudové služby v hello vydáno toocolumn (Příklad: cloudservice4testing.cloudapp.net).

V dalším kroku spuštění vzdálené relace prostředí Azure PowerShell pomocí těchto příkazů.

```powershell
$uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
$creds = Get-Credential
Enter-PSSession -ConnectionUri $uri -Credential $creds
```

Po zadání přihlašovacích údajů správce platný, měli byste vidět něco podobné toohello příkazového řádku Azure PowerShell následující:

```powershell
[cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>
```

první část Hello tuto výzvu je název vaší cloudové služby, který obsahuje hello cílovém virtuálním počítači, který může lišit od "cloudservice4testing.cloudapp.net". Teď můžete vydat služby tooinvestigate hello problémy uvedené příkazy prostředí Azure PowerShell pro tento cloud a opravte konfiguraci hello.

### <a name="toomanually-correct-hello-remote-desktop-services-listening-tcp-port"></a>toomanually správné hello TCP port naslouchání služby Vzdálená plocha
V hello vzdálené relace příkazového řádku Azure PowerShell spusťte tento příkaz.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

Hello číslo_portu vlastnost ukazuje hello aktuální číslo portu. V případě potřeby změňte hello vzdálené plochy port číslo back tooits výchozí hodnotu (3389) pomocí tohoto příkazu.

```powershell
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389
```

Ověřte, že hello port byl změněné too3389 pomocí tohoto příkazu.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

Pomocí tohoto příkazu Ukončit hello vzdálenou relaci prostředí Azure PowerShell.

```powershell
Exit-PSSession
```

Ověřte, že tento hello koncový bod vzdálené plochy pro hello virtuálního počítače Azure také používá port TCP. 3398 jako svůj vnitřní port. Restartujte hello virtuální počítač Azure a zkuste to znovu připojení ke vzdálené ploše hello.

## <a name="additional-resources"></a>Další zdroje
[Jak služba tooreset heslo nebo hello vzdálené plochy pro virtuální počítače s Windows](reset-rdp.md)

[Jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview)

[Řešení potíží s Secure Shell (SSH) připojení tooa systémem Linux virtuálního počítače Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Řešení potíží s přístup tooan aplikace běžící na virtuálním počítači Azure](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

