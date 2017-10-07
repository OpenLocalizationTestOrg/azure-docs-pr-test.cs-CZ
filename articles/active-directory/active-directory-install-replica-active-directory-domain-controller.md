---
title: "aaaInstall repliky řadiče domény služby Active Directory v Azure | Microsoft Docs"
description: "Kurz, který vysvětluje, jak tooinstall řadiče domény ze služby Active Directory v místní doménové struktury na virtuální počítač Azure."
services: virtual-network
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: virtual-network
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 74876fce2ca2a29f02c2828f9b3a21227248233a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Instalace repliky řadiče domény služby Active Directory v virtuální sítě Azure
Toto téma ukazuje, jak tooinstall další řadiče domény (také označované jako repliku řadiče domény) pro místní doménu služby Active Directory na virtuálních počítačích Azure (VM) ve virtuální sítě Azure.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.

Může být také zájem o tyto související témata:

* Volitelně můžete nainstalovat novou doménovou strukturu služby Active Directory na virtuální síť Azure. Tyto pokyny najdete v části [nainstalovat novou doménovou strukturu služby Active Directory na virtuální síť Azure](active-directory-new-forest-virtual-machine.md).
* Koncepční informace o instalaci služby Active Directory Domain Services (AD DS) na virtuální síť Azure, najdete v části [pokyny pro nasazení systému Windows Server Active Directory ve virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Diagram scénáře
V tomto scénáři externí uživatelé potřebovat tooaccess aplikace, které běží na serverech připojených k doméně. Hello virtuálních počítačů, které běží hello aplikační servery a hello repliky řadiče domény jsou nainstalované v virtuální sítě Azure. Hello virtuální sítě může být připojené toohello místní sítě tak, že [site-to-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) připojení, jak je znázorněno v následující hello diagram nebo můžete použít [ExpressRoute](../expressroute/expressroute-locations-providers.md) pro rychlejší připojení.

Hello aplikační servery a řadiče domény hello nasazených v rámci samostatné cloudové služby toodistribute výpočetní zpracování a v [skupiny dostupnosti](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pro lepší odolnost proti chybám.
Hello řadiče domény replikovat mezi sebou a se místní řadiče domény pomocí replikace služby Active Directory. Žádné nástrojů pro synchronizaci, je potřeba.

![Diagram pf repliky řadiče domény služby Active Directory síť Azure][1]

## <a name="create-an-active-directory-site-for-hello-azure-virtual-network"></a>Vytvoření lokality služby Active Directory pro hello virtuální síť Azure
Je vhodné toocreate lokality ve službě Active Directory, který představuje hello sítě oblast odpovídající toohello virtuální sítě. Který pomáhá optimalizovat ověřování, replikace a další operace umístění řadiče domény. Hello následující kroky popisují, jak toocreate lokality a pro další informace viz [přidání nového serveru](https://technet.microsoft.com/library/cc781496.aspx).

1. Otevřete lokality a služby Active Directory: **správce serveru** > **nástroje** > **lokality a služby Active Directory**.
2. Vytvoření oblasti toorepresent hello lokality, které jste vytvořili virtuální sítě Azure: klikněte na tlačítko **lokality** > **akce** > **nové lokality** > typ Název Hello hello nové lokality, jako je například Azure USA – západ > vyberte propojení lokalit > **OK**.
3. Vytvořte podsíť a přidružit novou lokalitu hello: klikněte dvakrát na **lokality** > klikněte pravým tlačítkem na **podsítě** > **novou podsíť** > zadejte rozsah adres IP hello Hello virtuální sítě (například 10.1.0.0/16 v diagramu scénář hello) > vyberte hello nový web Azure > **OK**.

## <a name="create-an-azure-virtual-network"></a>Vytvoření virtuální sítě Azure
1. V hello [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **nový** > **síťové služby** > **virtuální sítě**  >  **Vytvořit vlastní** a použití hello následující hodnoty toocomplete hello průvodce.

   | Na této stránce průvodce... | Zadejte tyto hodnoty |
   | --- | --- |
   |  **Podrobnosti virtuální sítě** |<p>Název: Zadejte název pro virtuální síť hello, jako je například WestUSVNet.</p><p>Oblast: Zvolte nejbližší oblast hello.</p> |
   |  **DNS a síť VPN připojení** |<p>Servery DNS: Zadejte název hello a jeden nebo více serverů DNS místní IP adresu.</p><p>Připojení: Vyberte **konfigurace VPN typu site-to-site**.</p><p>Místní sítě: Zadejte novou místní síť.</p><p>Pokud používáte místo VPN ExpressRoute, přečtěte si téma [nakonfigurujte připojení ExpressRoute prostřednictvím poskytovatele Exchange](../expressroute/expressroute-locations-providers.md).</p> |
   |  **Připojení Site-to-site** |<p>Název: Zadejte název pro hello do místní sítě.</p><p>IP adresa zařízení VPN: Zadejte hello veřejnou IP adresu hello zařízení, které se budou připojovat toohello virtuální sítě. Hello zařízení VPN nesmí být umístěné za adres (NAT)</p><p>Adresa: Zadejte hello rozsahy adres ve vaší místní síti (například 192.168.0.0/16 v diagramu scénář hello).</p> |
   |  **Adresní prostory virtuální sítě** |<p>Adresní prostor: Zadejte hello rozsah IP adres pro virtuální počítače, které chcete toorun v hello virtuální sítě Azure (například 10.1.0.0/16 v diagramu scénář hello). Tento rozsah adres se nesmí překrývat s rozsahy adres hello hello místní sítě.</p><p>Podsítě: Zadejte název a adresu pro podsíť pro aplikační servery hello (například front-endu, 10.1.1.0/24) a pro hello řadiče domény (například back-end, 10.1.2.0/24).</p><p>Klikněte na tlačítko **přidat podsíť brány**.</p> |
2. Dále nakonfigurujete toocreate brány virtuální sítě hello zabezpečené připojení site-to-site VPN. V tématu [konfigurovat bránu virtuální sítě](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) pokyny hello.
3. Vytvořte hello site-to-site VPN připojení mezi hello nové virtuální sítě a místní zařízení VPN. V tématu [konfigurovat bránu virtuální sítě](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) pokyny hello.

## <a name="create-azure-vms-for-hello-dc-roles"></a>Vytvořit virtuální počítače Azure pro hello role řadiče domény
Opakujte hello následující kroky toocreate virtuální počítače toohost hello řadič domény role podle potřeby. Měli byste nasadit aspoň dva virtuální řadiče domény tooprovide odolnost proti chybám a redundance. Pokud hello virtuální síť Azure zahrnuje aspoň dva řadiče domény, které jsou nakonfigurované podobně (to znamená, že jsou oba GC spuštění serveru DNS, a ani jeden z nich obsahuje všechny FSMO role a tak dále) pak umístit hello virtuálních počítačů, které běží ty řadiče domény v sadu dostupnosti pro lepší odolnost proti chybám.
toocreate hello virtuálních počítačů pomocí prostředí Windows PowerShell místo hello uživatelského rozhraní, najdete v části [toocreate použití Azure PowerShell a předem nakonfigurujte virtuálních počítačích se systémem Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. V hello [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **nový** > **výpočetní** > **virtuálního počítače**  >  **Z Galerie**. Použijte následující hodnoty toocomplete hello Průvodce hello. Přijměte hello výchozí hodnota pro nastavení, není-li navrhované nebo vyžaduje jinou hodnotu.

   | Na této stránce průvodce... | Zadejte tyto hodnoty |
   | --- | --- |
   |  **Vyberte bitovou kopii** |Windows Server 2012 R2 Datacenter |
   |  **Konfigurace virtuálního počítače** |<p>Název virtuálního počítače: Zadejte název bez přípony (například AzureDC1).</p><p>Nové uživatelské jméno: Zadejte název hello uživatele. Tento uživatel bude členem místní skupiny Administrators hello na hello virtuálních počítačů. Je nutné tento název toosign v toohello virtuálních počítačů pro hello poprvé. Hello předdefinovaný účet s názvem správce nebude fungovat.</p><p>Nové heslo nebo potvrďte: Zadejte heslo</p> |
   |  **Konfigurace virtuálního počítače** |<p>Cloudové služby: Zvolte <b>vytvořte novou cloudovou službu</b> pro hello hello první virtuální počítač a vyberte, že stejný cloudový název služby, když vytvoříte další virtuální počítače, které budou hostiteli role řadiče domény.</p><p>Název DNS cloudové služby: Zadejte globálně jedinečného názvu</p><p>Oblasti nebo skupiny vztahů nebo virtuální sítě: Zadejte název virtuální sítě hello (například WestUSVNet).</p><p>Účet úložiště: Zvolte <b>použít účet úložiště automaticky generované</b> pro hello hello první virtuální počítač a potom vyberte, že název stejný účet úložiště, když vytvoříte další virtuální počítače, které budou hostiteli role řadiče domény.</p><p>Skupinu dostupnosti: Zvolit <b>vytvořit skupinu dostupnosti</b>.</p><p>Název sady dostupnosti:, zadejte název pro sadu při vytváření dostupnosti hello hello první virtuální počítač a potom vyberte stejný název, když vytvoříte další virtuální počítače.</p> |
   |  **Konfigurace virtuálního počítače** |<p>Vyberte <b>hello instalace agenta virtuálního počítače</b> a další rozšíření, budete potřebovat.</p> |
2. Připojte tooeach disku virtuálního počítače, který se spustí hello role serveru řadiče domény. Další disk Hello je potřebné toostore hello AD databáze, protokoly a adresáře SYSVOL. Zadejte velikost disku hello (například 10 GB) a nechte hello **předvoleb mezipaměti hostitele** nastavit příliš**žádné**. Hello pokyny najdete v tématu [jak tooAttach tooa datový Disk virtuálního počítače Windows](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Po prvním přihlášení toohello virtuálních počítačů, otevřete **správce serveru** > **Souborová služba a služba úložiště** toocreate svazku na tento disk pomocí systému souborů NTFS.
4. Vyhradit statickou IP adresu pro virtuální počítače, které budou spouštět role řadiče domény hello. tooreserve statickou IP adresu, stáhněte si hello instalačního programu webové platformy a [nainstalovat Azure PowerShell](/powershell/azure/overview) a spusťte rutinu Set-AzureStaticVNetIP hello. Například:

    ' Get-AzureVM - ServiceName AzureDC1-název AzureDC1 | Set-AzureStaticVNetIP - IPAddress 10.0.0.4 | Update-AzureVM

Další informace o nastavení statické IP adresy najdete v tématu [nakonfigurovat statické interní IP adresy pro virtuální počítač](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Nainstalovat službu AD DS na virtuálních počítačích Azure
Přihlaste se tooa virtuálního počítače a ověřte, že máte připojení mezi hello site-to-site VPN nebo ExpressRoute připojení tooresources ve vaší místní síti. Nainstalujte na hello virtuálních počítačích Azure AD DS. Můžete použít stejný postup, že používáte tooinstall dalšího řadiče domény v místní síti (uživatelského rozhraní, prostředí Windows PowerShell nebo pomocí souboru odpovědí). Při instalaci služby AD DS, zkontrolujte, zda že zadejte hello nového svazku k umístění hello hello databáze AD, protokoly a adresáře SYSVOL. Pokud potřebujete aktualizačnímu programu na instalaci služby AD DS, najdete v části [instalace služby Active Directory Domain Services (úroveň 100)](https://technet.microsoft.com/library/hh472162.aspx) nebo [instalace řadiče domény repliky Windows serveru 2012 v existující doméně (úroveň 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-hello-virtual-network"></a>Znovu nakonfigurujte server DNS pro virtuální síť hello
1. V hello [portál Azure](https://portal.azure.com), v hello **vyhledávání prostředků** zadejte *virtuální sítě*, pak klikněte na tlačítko **virtuální sítě (klasické)** v výsledky hledání Hello. Klikněte na název hello hello virtuální sítě a potom [změnit konfiguraci IP adres serveru hello DNS pro vaši virtuální síť](../virtual-network/virtual-network-manage-network.md#dns-servers) toouse hello statické IP adresy přiřazené toohello řadičů domény repliky místo hello IP adresy DNS na místě servery.
2. Klikněte na tlačítko tooensure hello repliky všechny virtuální počítače řadiče domény ve virtuální síti hello jsou nakonfigurované servery DNS toouse ve virtuální síti hello, **virtuální počítače**, klikněte ve sloupci Stav hello pro každý virtuální počítač a pak klikněte na tlačítko **restartování** . Počkejte, dokud hello virtuální počítač zobrazuje **systémem** stavu, než se pokusíte toosign do ní.

## <a name="create-vms-for-application-servers"></a>Vytvořit virtuální počítače pro aplikační servery
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

## <a name="additional-resources"></a>Další zdroje
* [Pokyny pro nasazení systému Windows Server Active Directory na virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Jak tooupload existující místní tooAzure řadiče domény technologie Hyper-V pomocí prostředí Azure PowerShell](http://support.microsoft.com/kb/2904015)
* [Instalace nové doménové struktury služby Active Directory na virtuální síť Azure](active-directory-new-forest-virtual-machine.md)
* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Microsoft Azure IaaS IT specialista: Základy (01) virtuálního počítače](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IaaS IT specialista: (05) vytváření virtuální sítě a připojení mezi různými místy](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Azure PowerShell](/powershell/azure/overview)
* [Rutiny pro správu Azure](/powershell/module/azurerm.compute/#virtual_machines)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
