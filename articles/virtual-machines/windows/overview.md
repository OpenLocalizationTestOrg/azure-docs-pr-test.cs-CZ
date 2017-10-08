---
title: "Přehled virtuálních počítačů aaaWindows | Microsoft Docs"
description: "Zjistěte, jak vytvářet a spravovat virtuální počítače s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: fbae9c8e-2341-4ed0-bb20-fd4debb2f9ca
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 8015b1aa4bcdaac2e721f25d85a5bc995a22f0f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-windows-virtual-machines-in-azure"></a>Přehled virtuálních počítačů s Windows v Azure

Azure Virtual Machines (VM) je jedním z několika typů [škálovatelných výpočetních prostředků na vyžádání](../../app-service-web/choose-web-site-cloud-service-vm.md), které Azure nabízí. Obvykle zvolíte virtuální počítač, pokud potřebujete větší kontrolu nad výpočetním prostředí hello než nabízí technologie hello jiné možnosti. Tento článek obsahuje informace o tom, co byste měli zvážit před vytvořením virtuálního počítače, jak ho vytvořit a jak ho spravovat.

Virtuálnímu počítači Azure poskytuje hello flexibilitu virtualizace bez nutnosti toobuy a udržovat hello fyzický hardware, které ho spouští. Však stále potřebujete toomaintain hello virtuálního počítače provedením úloh, jako je například konfigurace, opravy a instalace hello software, který běží na něm.

Virtuální počítače Azure lze použít různými způsoby. Tady je několik příkladů:

* **Vývoj a testování** – virtuálních počítačích Azure nabízí rychlý a snadný způsob toocreate počítač s konkrétní konfigurací požadované toocode a testování aplikací.
* **Aplikace v cloudu hello** – protože vyžádání pro vaši aplikaci můžete kolísá, může mít ekonomický smysl toorun ho na virtuálním počítači v Azure. Za další virtuální počítače platíte, když je potřebujete, a když ne, tak je vypnete.
* **Rozšířené datacenter** – virtuální počítače ve virtuální sítě Azure lze snadno síti připojené tooyour organizace.

Hello počet virtuálních počítačů, které vaše aplikace používá můžete postupně škálovat a out toowhatever je požadovaná toomeet vašim potřebám.

## <a name="what-do-i-need-toothink-about-before-creating-a-vm"></a>Co potřebuji toothink o před vytvořením virtuálního počítače?
Při sestavování infrastruktury aplikace v Azure vždy existuje velké množství [aspektů návrhu](/architecture/reference-architectures/virtual-machines-linux?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tyto aspekty virtuálního počítače jsou důležité toothink o, než začnete:

* Hello názvy prostředků aplikace
* Hello umístění, kde jsou uložené prostředky hello
* velikost Hello hello virtuálních počítačů
* maximální počet virtuálních počítačů, které lze vytvořit Hello
* Spustí Hello operační systém, který hello virtuálních počítačů
* Konfigurace Hello hello virtuální počítač po jeho spuštění
* Hello související prostředky této hello musí virtuálních počítačů

### <a name="naming"></a>Pojmenování
Virtuální počítač má [název](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) přiřazené tooit a má název počítače, který je nakonfigurovaný jako součást hello operačního systému. Hello název virtuálního počítače může být až too15 znaků.

Pokud používáte disk operačního systému Azure toocreate hello, hello název počítače a název virtuálního počítače hello jsou hello stejné. Pokud jste [odesílání a použití vlastní image](upload-generalized-managed.md) , dříve nakonfigurované operační systém obsahoval a použít ho toocreate virtuálního počítače, názvy hello se může lišit. Doporučujeme, při nahrávání souboru bitové kopie, abyste vytvořili hello název počítače v hello operačního systému a název virtuálního počítače hello hello stejné.

### <a name="locations"></a>Umístění
Všechny prostředky, které jsou vytvořené v Azure jsou rozmístěny v několika [zeměpisné oblasti](https://azure.microsoft.com/regions/) kolem hello, world. Obvykle se nazývá hello oblast **umístění** při vytváření virtuálního počítače. Pro virtuální počítač určuje hello umístění, kde jsou uloženy hello virtuální pevné disky.

V této tabulce jsou uvedeny některé hello způsoby, které můžete získat seznam dostupných umístění.

| Metoda | Popis |
| --- | --- |
| portál Azure |Vyberte umístění ze seznamu hello při vytváření virtuálního počítače. |
| Azure PowerShell |Použití hello [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation) příkaz. |
| REST API |Použití hello [seznam umístění](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_ListLocations) operaci. |

### <a name="vm-size"></a>Velikost virtuálního počítače
Hello [velikost](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello virtuální počítač, který používáte, je určen hello zatížením, které chcete toorun. velikost Hello, který zvolíte, pak určuje faktorů, jako je například zpracování kapacita napájení, paměť a úložiště. Azure nabízí širokou škálu velikostí toosupport mnoho typů použití.

Azure poplatky za [hodinovou cenu](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) podle velikosti hello Virtuálního počítače a operačního systému. V případě neúplných hodin Azure účtuje pouze jako minut hello používá. Služba Storage je oceněna a účtována samostatně.

### <a name="vm-limits"></a>Omezení virtuálního počítače
Vaše předplatné má výchozí [kvótami](../../azure-subscription-service-limits.md) na místo, které by mohlo mít vliv nasazení hello mnoho virtuálních počítačů pro váš projekt. aktuální limit Hello na základě za předplatné je 20 virtuálních počítačů podle oblastí. Limity lze navýšit tak, že vyplníte lístek podpory s žádostí o navýšení.

### <a name="operating-system-disks-and-images"></a>Disky a image operačních systémů
Virtuální počítače používat [virtuální pevné disky (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toostore svoje operační systém (OS) a data. Virtuální pevné disky se taky používají pro hello bitové kopie, které můžete vybrat z tooinstall operační systém. 

Azure poskytuje mnoho [marketplace image](https://azure.microsoft.com/marketplace/virtual-machines/) toouse s různými verzemi a typů operačních systémů Windows Server. Image pořízené na webu Marketplace jsou identifikované vydavatelem image, nabídkou, skladovou jednotkou (SKU) a verzí (verze je obvykle uvedena jako poslední). 

Tato tabulka ukazuje některé způsoby, které můžete najít hello informace pro bitovou kopii.

| Metoda | Popis |
| --- | --- |
| portál Azure |Hello hodnoty jsou specifikované automaticky za vás, když vyberete toouse bitové kopie. |
| Azure PowerShell |[Get-AzureRMVMImagePublisher](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimagepublisher) -Location "umístění"<BR>[Get-AzureRMVMImageOffer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimageoffer) -Location "umístění" -Publisher "název_vydavatele"<BR>[Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) -Location "umístění" -Publisher "název_vydavatele" -Offer "název_nabídky" |
| Rozhraní REST API |[Vypsat vydavatele imagí](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers)<BR>[Vypsat nabídky imagí](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers)<BR>[Vypsat skladové jednotky (SKU) imagí](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus) |

Můžete zvolit příliš[odesílání a použití vlastní image](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account) a když to uděláte, se nepoužívají hello název vydavatele, nabídky a sku.

### <a name="extensions"></a>Rozšíření
[Rozšíření](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) virtuálního počítače poskytují vašemu virtuálnímu počítači další schopnosti prostřednictvím konfigurace po nasazení a automatizovaných úloh.

Pomocí rozšíření můžete provádět tyto běžné úlohy:

* **Spustit vlastní skripty** – hello [rozšíření vlastních skriptů](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vám pomůže nakonfigurovat úlohy na hello virtuálních počítačů tak, že spustíte skript při hello virtuálního počítače je zřízený.
* **Nasazení a správě konfigurace** – hello [rozšíření prostředí PowerShell požadovaného stavu konfigurace (DSC)](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) slouží k nastavení DSC na virtuální počítač toomanage konfigurace a prostředí.
* **Shromažďování dat diagnostiky** – hello [rozšíření diagnostiky Azure](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pomáhá nakonfigurovat hello virtuálních počítačů toocollect diagnostiky data, která lze použít toomonitor hello stavu aplikace.

### <a name="related-resources"></a>Související prostředky
Hello prostředky v této tabulce jsou používány hello virtuálních počítačů a potřebovat tooexist nebo vytvořit při vytváření hello virtuálních počítačů.

| Prostředek | Požaduje se | Popis |
| --- | --- | --- |
| [Skupina prostředků](../../azure-resource-manager/resource-group-overview.md) |Ano |Hello virtuálního počítače musí být součástí skupiny prostředků. |
| [Účet úložiště](../../storage/common/storage-create-storage-account.md) |Ano |Hello virtuálních počítačů musí toostore účet úložiště hello virtuální pevné disky. |
| [Virtuální síť](../../virtual-network/virtual-networks-overview.md) |Ano |Hello virtuálního počítače musí být členem skupiny virtuální sítě. |
| [Veřejná IP adresa](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) |Ne |Hello virtuální počítač může mít veřejnou IP adresu přiřazenou tooit tooremotely k němu přístup. |
| [Síťové rozhraní](../../virtual-network/virtual-network-network-interface.md) |Ano |Hello virtuálních počítačů musí hello síťové rozhraní toocommunicate v síti hello. |
| [Datové disky](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |Ne |Hello virtuálních počítačů může zahrnovat možnosti úložiště tooexpand datové disky. |

## <a name="how-do-i-create-my-first-vm"></a>Jak vytvořím svůj první virtuální počítač?
Virtuální počítač můžete vytvořit několika způsoby. Výběr Hello, které vytváříte, závisí na hello prostředí, který se v. 

Tato tabulka obsahuje informace o tooget jste spustili, vytvoření virtuálního počítače.

| Metoda | Článek |
| --- | --- |
| portál Azure |[Vytvoření virtuálního počítače se systémem Windows pomocí portálu hello](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Šablony |[Vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Azure PowerShell |[Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Klientské sady SDK |[Nasazení prostředků Azure pomocí jazyka C#](csharp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Rozhraní REST API |[Vytvořit nebo aktualizovat virtuální počítač](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-create-or-update) |

Věříte, že se to nikdy nestane, ale občas dojde k nějaké chybě. Pokud k této situaci dojde tooyou, podívejte se na informace hello v [problémy při nasazení řešení potíží s Resource Manager pomocí vytvoření virtuálního počítače s Windows v Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="how-do-i-manage-hello-vm-that-i-created"></a>Jak lze spravovat hello virtuální počítač, který vytvořil jsem?
Virtuální počítače můžete spravovat pomocí portálu (přes prohlížeč), nástrojů příkazového řádku s podporou pro skriptování, nebo přímo prostřednictvím rozhraní API. Některé typické úlohy správy, které můžete provést se získání informací o používání virtuálních počítačů, protokolování na tooa virtuálním Počítačem, správu dostupnosti a provádění zálohování.

### <a name="get-information-about-a-vm"></a>Získání informací o virtuálním počítači
Tato tabulka uvádí některé hello způsoby, které můžete získat informace o virtuální počítač.

| Metoda | Popis |
| --- | --- |
| portál Azure |V nabídce centra hello, klikněte na tlačítko **virtuální počítače** a potom vyberte ze seznamu hello hello virtuálních počítačů. V okně hello hello virtuálních počítačů máte přístup k informacím toooverview, hodnoty nastavení a monitorování metriky. |
| Azure PowerShell |Informace o používání virtuálních počítačů toomanage prostředí PowerShell najdete v tématu [vytvořit a spravovat virtuální počítače Windows hello modul Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| REST API |Použití hello [informace o získání virtuálních počítačů](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-get) operace tooget informace o virtuální počítač. |
| Klientské sady SDK |Informace o použití jazyka C# toomanage virtuálních počítačů najdete v tématu [spravovat virtuální počítače Azure pomocí Azure Resource Manager a C#](csharp-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |

### <a name="log-on-toohello-vm"></a>Přihlaste se toohello virtuálních počítačů
Použijete tlačítko Připojit hello v hello portál Azure příliš[spustit relaci Remote Desktop (RDP)](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Můžete někdy problémů při pokusu o toouse vzdáleného připojení. Pokud k této situaci dojde tooyou, podívejte se na informace nápovědy hello v [tooan připojení ke vzdálené ploše řešení potíží s Azure virtuální počítač se systémem Windows](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="manage-availability"></a>Správa dostupnosti
Je důležité, aby vám toounderstand jak příliš[zajištění vysoké dostupnosti](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pro vaši aplikaci. Tato konfigurace zahrnuje vytvoření více tooensure virtuální počítače se systémem alespoň jeden.

Pro vaše nasazení tooqualify pro naše 99.95 Smlouvy o úrovni služeb virtuálních počítačů, je třeba splnit toodeploy minimálně dva virtuální počítače spuštěné úlohy uvnitř [skupinu dostupnosti](tutorial-availability-sets.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tato konfigurace zajišťuje distribuci vašich virtuálních počítačů mezi více domén selhání a jejich nasazení na hostitele s různými časovými obdobími údržby. Hello úplné [smlouva Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) vysvětluje hello záruku dostupnosti Azure jako celek.

### <a name="back-up-hello-vm"></a>Zálohování hello virtuálních počítačů
A [trezor služeb zotavení](../../backup/backup-introduction-to-azure-backup.md) je použité tooprotect data a prostředky služby Azure Backup a Azure Site Recovery. Trezor služeb zotavení můžete použít příliš[nasadit a spravovat zálohy pro správce prostředků virtuálních počítačů nasazených pomocí prostředí PowerShell](../../backup/backup-azure-vms-automation.md). 

## <a name="next-steps"></a>Další kroky
* Pokud vaše je toowork s virtuální počítače s Linuxem, podívejte se na [Azure a Linux](../linux/overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Další informace o hello pokyny ohledně nastavení infrastruktury v hello [návod infrastruktury Azure příklad](infrastructure-example.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
