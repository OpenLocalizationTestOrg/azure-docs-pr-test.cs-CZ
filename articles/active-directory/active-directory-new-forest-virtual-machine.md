---
title: "aaaInstall doménovou strukturu služby Active Directory na virtuální síť Azure | Microsoft Docs"
description: "Kurz, který vysvětluje, jak doménové struktury toocreate nové služby Active Directory na virtuálním počítači (VM) na virtuální síti Azure."
services: active-directory, virtual-network
keywords: "služby Active directory virtuálního počítače, instalace služby active directory doménové struktury, videa služby azure active directory "
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
tags: 
ms.assetid: eb7170d0-266a-4caa-adce-1855589d65d1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/06/2017
ms.author: joflore
ms.openlocfilehash: 08121130777cc3c206d7b5b38974982884dca1c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Instalace nové doménové struktury služby Active Directory na virtuální síť Azure
Toto téma ukazuje, jak toocreate nové Windows Server Active Directory prostředí na virtuální síť Azure na virtuálním počítači (VM) na [virtuální síť Azure](../virtual-network/virtual-networks-overview.md). V takovém případě hello virtuální síť Azure, není připojený tooan do místní sítě.

Může být také zájem o tyto související témata:

* Video, které představuje tyto kroky, najdete v části [jak tooinstall nové služby Active Directory doménové struktury na virtuální síť Azure](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* Volitelně můžete [konfigurace VPN typu site-to-site](../vpn-gateway/vpn-gateway-site-to-site-create.md) a pak nainstalovat novou doménovou strukturu nebo rozšířit místní doménové struktuře tooan virtuální síť Azure. Tyto pokyny najdete v části [instalaci řadiče domény repliky Active Directory ve virtuální síti Azure](active-directory-install-replica-active-directory-domain-controller.md).
* Koncepční informace o instalaci služby Active Directory Domain Services (AD DS) na virtuální síť Azure, najdete v části [pokyny pro nasazení systému Windows Server Active Directory ve virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Diagram scénáře
V tomto scénáři externí uživatelé potřebovat tooaccess aplikace, které běží na serverech připojených k doméně. Hello virtuálních počítačů, které používat hello aplikačních serverů a hello virtuálních počítačů, které spustit řadiče domény jsou nainstalovány nainstalovaný ve své vlastní cloudové služby v rámci virtuální sítě Azure. Jsou taky součástí sadu dostupnosti pro lepší odolnost proti chybám.

![Doménové struktury Active Directory na virtuální počítače ve virtuální síti Azure ][1] 7

## <a name="how-does-this-differ-from-on-premises"></a>Jak to se liší od místně?
Není k dispozici mnoho rozdíl mezi instalací řadiče domény v Azure a místní. Hello hlavní rozdíly jsou uvedeny v následující tabulce hello.

| tooconfigure... | Lokálně | Virtuální síť Azure |
| --- | --- | --- |
| **IP adresa pro řadič domény hello** |Přiřadit statickou IP adresu ve vlastnostech síťového adaptéru hello |Spustit tooassign rutiny Set-AzureStaticVNetIP hello statickou IP adresu |
| **Překladač služby DNS klienta** |Nastavení adresy serveru preferované a alternativní server DNS na hello vlastnosti síťového adaptéru členů domény |Nastavit adresu serveru DNS na vlastnosti hello hello virtuální sítě |
| **Úložiště databáze služby Active Directory** |Volitelně můžete změnit výchozí umístění úložiště hello z C:\ |Je třeba toochange výchozí umístění úložiště z C:\ |

## <a name="create-an-azure-virtual-network"></a>Vytvoření virtuální sítě Azure
1. Přihlaste se toohello portál Azure classic.
2. Vytvoření virtuální sítě. Klikněte na tlačítko **sítě** > **vytvořit virtuální síť**. Použijte hodnoty hello hello následující průvodce hello toocomplete tabulky.

   | Na této stránce průvodce... | Zadejte tyto hodnoty |
   | --- | --- |
   |  **Podrobnosti virtuální sítě** |<p>Název: Zadejte název pro vaši virtuální síť</p><p>Oblast: Zvolte nejbližší oblast hello</p> |
   |  **DNS a sítě VPN** |<p>Ponechat prázdné DNS server</p><p>Nemáte vyberte buď možnost VPN</p> |
   |  **Adresní prostory virtuální sítě** |<p>Název podsítě: Zadejte název pro podsíť</p><p>Počáteční IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p> |

## <a name="create-vms-toorun-hello-domain-controller-and-dns-server-roles"></a>Vytvoření řadiče domény hello toorun virtuální počítače a role serveru DNS
Opakujte hello následující kroky toocreate virtuální počítače toohost hello řadič domény role podle potřeby. Měli byste nasadit aspoň dva virtuální řadiče domény tooprovide odolnost proti chybám a redundance. Pokud hello virtuální síť Azure zahrnuje aspoň dva řadiče domény, které jsou nakonfigurované podobně (to znamená, že jsou oba GC spuštění serveru DNS, a ani jeden z nich obsahuje všechny FSMO role a tak dále) pak umístit hello virtuálních počítačů, které běží ty řadiče domény v sadu dostupnosti pro lepší odolnost proti chybám.

toocreate hello virtuálních počítačů pomocí prostředí Windows PowerShell místo hello uživatelského rozhraní, najdete v části [toocreate použití Azure PowerShell a předem nakonfigurujte virtuálních počítačích se systémem Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. Na portálu classic hello, klikněte na tlačítko **nový** > **výpočetní** > **virtuálního počítače** > **z Galerie**. Použijte následující hodnoty toocomplete hello Průvodce hello. Přijměte hello výchozí hodnota pro nastavení, není-li navrhované nebo vyžaduje jinou hodnotu.

   | Na této stránce průvodce... | Zadejte tyto hodnoty |
   | --- | --- |
   |  **Vyberte bitovou kopii** |Windows Server 2012 R2 Datacenter |
   |  **Konfigurace virtuálního počítače** |<p>Název virtuálního počítače: Zadejte název bez přípony (například AzureDC1).</p><p>Nové uživatelské jméno: Zadejte název hello uživatele. Tento uživatel bude členem místní skupiny Administrators hello na hello virtuálních počítačů. Je nutné tento název toosign v toohello virtuálních počítačů pro hello poprvé. Hello předdefinovaný účet s názvem správce nebude fungovat.</p><p>Nové heslo nebo potvrďte: Zadejte heslo</p> |
   |  **Konfigurace virtuálního počítače** |<p>Cloudové služby: Zvolte <b>vytvořte novou cloudovou službu</b> pro hello hello první virtuální počítač a vyberte, že stejný cloudový název služby, když vytvoříte další virtuální počítače, které budou hostiteli role řadiče domény.</p><p>Název DNS cloudové služby: Zadejte globálně jedinečného názvu</p><p>Oblasti nebo skupiny vztahů nebo virtuální sítě: Zadejte název virtuální sítě hello (například WestUSVNet).</p><p>Účet úložiště: Zvolte <b>použít účet úložiště automaticky generované</b> pro hello hello první virtuální počítač a potom vyberte, že název stejný účet úložiště, když vytvoříte další virtuální počítače, které budou hostiteli role řadiče domény.</p><p>Skupinu dostupnosti: Zvolit <b>vytvořit skupinu dostupnosti</b>.</p><p>Název sady dostupnosti:, zadejte název pro sadu při vytváření dostupnosti hello hello první virtuální počítač a potom vyberte stejný název, když vytvoříte další virtuální počítače.</p> |
   |  **Konfigurace virtuálního počítače** |<p>Vyberte <b>hello instalace agenta virtuálního počítače</b> a další rozšíření, budete potřebovat.</p> |
2. Připojte tooeach disku virtuálního počítače, který se spustí hello role serveru řadiče domény. Další disk Hello je potřebné toostore hello AD databáze, protokoly a adresáře SYSVOL. Zadejte velikost disku hello (například 10 GB) a nechte hello **předvoleb mezipaměti hostitele** nastavit příliš**žádné**. Hello pokyny najdete v tématu [jak tooAttach tooa datový Disk virtuálního počítače Windows](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Po prvním přihlášení toohello virtuálních počítačů, otevřete **správce serveru** > **Souborová služba a služba úložiště** toocreate svazku na tento disk pomocí systému souborů NTFS.
4. Vyhradit statickou IP adresu pro virtuální počítače, které budou spouštět role řadiče domény hello. tooreserve statickou IP adresu, stáhněte si hello instalačního programu webové platformy a [nainstalovat Azure PowerShell](/powershell/azure/overview) a spusťte rutinu Set-AzureStaticVNetIP hello. Například:

    `Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM`

Další informace o nastavení statické IP adresy najdete v tématu [nakonfigurovat statické interní IP adresy pro virtuální počítač](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-windows-server-active-directory"></a>Nainstalujte Windows Server Active Directory
Pomocí stejné rutiny hello příliš[instalaci služby AD DS](https://technet.microsoft.com/library/jj574166.aspx) použít místní (tedy můžete hello uživatelského rozhraní, soubor odpovědí nebo prostředí Windows PowerShell). Je nutné tooprovide správce přihlašovacích údajů tooinstall nové doménové struktury. umístění hello toospecify hello databáze služby Active Directory, protokolům a adresáři SYSVOL, změnit výchozí umístění úložiště hello z hello disk operačního systému disku toohello další data, že jste připojili toohello virtuálních počítačů.

Po dokončení instalace řadiče domény hello toohello virtuální počítač znovu připojit a přihlásit toohello řadič domény. Pamatujte přihlašovací údaje domény toospecify.

## <a name="reset-hello-dns-server-for-hello-azure-virtual-network"></a>Resetování hello serveru DNS pro hello virtuální síť Azure
1. Obnovte nastavení služby pro předávání DNS hello hello nového řadiče domény a DNS serveru.
   1. Ve Správci serveru klikněte na tlačítko **nástroje** > **DNS**.
   2. V **Správce DNS**, klikněte pravým tlačítkem na název hello hello DNS serveru a klikněte na **vlastnosti**.
   3. Na hello **předávání** , klikněte hello hello předávání IP adresu a klikněte na tlačítko **upravit**.  Vyberte hello IP adresu a klikněte na tlačítko **odstranit**.
   4. Klikněte na tlačítko **OK** tooclose hello editoru a **Ok** znovu tooclose hello vlastnosti serveru DNS.
2. Aktualizujte nastavení serveru DNS hello hello virtuální sítě.
   1. Klikněte na tlačítko **virtuální sítě** > dvakrát klikněte na virtuální síť hello jste vytvořili > **konfigurace** > **servery DNS**, zadejte název hello a hello DIP jednoho Hello virtuálních počítačů, kterém běží role serveru DNS řadiče domény nebo hello a klikněte na tlačítko **Uložit**.
   2. Vyberte hello virtuálních počítačů a klikněte na **restartujte** tootrigger hello virtuálních počítačů tooconfigure nastavení překladu DNS s IP adresou hello hello nového serveru DNS.

## <a name="create-vms-for-domain-members"></a>Vytváření virtuálních počítačů pro členy domény
1. Opakujte hello následující kroky toocreate virtuální počítače toorun jako aplikačních serverů. Přijměte hello výchozí hodnota pro nastavení, není-li navrhované nebo vyžaduje jinou hodnotu.

   | Na této stránce průvodce... | Zadejte tyto hodnoty |
   | --- | --- |
   |  **Vyberte bitovou kopii** |Windows Server 2012 R2 Datacenter |
   |  **Konfigurace virtuálního počítače** |<p>Název virtuálního počítače: Zadejte název bez přípony (například AppServer1).</p><p>Nové uživatelské jméno: Zadejte název hello uživatele. Tento uživatel bude členem místní skupiny Administrators hello na hello virtuálních počítačů. Je nutné tento název toosign v toohello virtuálních počítačů pro hello poprvé. Hello předdefinovaný účet s názvem správce nebude fungovat.</p><p>Nové heslo nebo potvrďte: Zadejte heslo</p> |
   |  **Konfigurace virtuálního počítače** |<p>Cloudové služby: Zvolte **vytvořte novou cloudovou službu** pro hello první virtuální počítač a vyberte stejný cloudový název služby, když vytvoříte další virtuální počítače, který bude hostitelem aplikace hello.</p><p>Název DNS cloudové služby: Zadejte globálně jedinečného názvu</p><p>Oblasti nebo skupiny vztahů nebo virtuální sítě: Zadejte název virtuální sítě hello (například WestUSVNet).</p><p>Účet úložiště: Zvolte **použít účet úložiště automaticky generované** pro hello první virtuální počítač a potom vyberte, že název stejný účet úložiště, když vytvoříte další virtuální počítače, který bude hostitelem aplikace hello.</p><p>Skupinu dostupnosti: Zvolit **vytvořit skupinu dostupnosti**.</p><p>Název sady dostupnosti:, zadejte název pro sadu při vytváření dostupnosti hello hello první virtuální počítač a potom vyberte stejný název, když vytvoříte další virtuální počítače.</p> |
   |  **Konfigurace virtuálního počítače** |<p>Vyberte <b>hello instalace agenta virtuálního počítače</b> a další rozšíření, budete potřebovat.</p> |
2. Po zřízení každý virtuální počítač, přihlaste se a připojte ho toohello doméně. V **správce serveru**, klikněte na tlačítko **místní Server** > **pracovní skupiny** > **změnu...** a pak vyberte **domény** a název typu hello vaší místní domény. Zadejte přihlašovací údaje uživatele domény a pak restartujte počítač toocomplete hello hello – připojení k doméně.

toocreate hello virtuálních počítačů pomocí prostředí Windows PowerShell místo hello uživatelského rozhraní, najdete v části [toocreate použití Azure PowerShell a předem nakonfigurujte virtuálních počítačích se systémem Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Další informace o používání prostředí Windows PowerShell najdete v tématu [Začínáme s Azure rutiny](/powershell/azure/overview) a [Reference k rutině Azure](/powershell/azure/get-started-azureps).

## <a name="see-also"></a>Viz také
* [Jak tooinstall nové služby Active Directory doménové struktury na virtuální síť Azure](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* [Pokyny pro nasazení systému Windows Server Active Directory na virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Konfigurace VPN typu Site-to-Site](../vpn-gateway/vpn-gateway-site-to-site-create.md)
* [Instalaci řadiče domény repliky Active Directory v virtuální sítě Azure](active-directory-install-replica-active-directory-domain-controller.md)
* [Microsoft Azure IaaS IT specialista: Základy (01) virtuálního počítače](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IaaS IT specialista: (05) vytváření virtuální sítě a připojení mezi různými místy](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md)
* [Jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Referenční informace k rutinám Azure](/powershell/azure/get-started-azureps)
* [Nastavení statické IP adresy virtuálních počítačů Azure](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
* [Jak tooAzure tooassign statickou IP adresu virtuálního počítače](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
* [Instalace nové doménové struktury Active Directory](https://technet.microsoft.com/library/jj574166.aspx)
* [Úvod tooActive virtualizace Directory Domain Services (AD DS) (úroveň 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
