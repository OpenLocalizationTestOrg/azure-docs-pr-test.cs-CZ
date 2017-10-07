---
title: "osvědčené postupy zabezpečení aaaAzure virtuálního počítače | Microsoft Docs"
description: "Tento článek obsahuje celou řadu zabezpečení osvědčené postupy toobe používat ve virtuálních počítačích, které jsou umístěné v Azure."
services: security
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 5e757abe-16f6-41d5-b1be-e647100036d8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: yurid
ms.openlocfilehash: b03bcc75fde6d49897f9a7f6f15aec87456edd0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-azure-vm-security"></a>Doporučené postupy pro zabezpečení virtuálního počítače Azure

Ve většině infrastruktura jako služba (IaaS) scénáře [Azure virtuální počítače (VM)](https://docs.microsoft.com/en-us/azure/virtual-machines/) jsou hello hlavní zatížení pro organizace, které používají cloud computing. Tuto skutečnost je obzvláště zřejmé ve [hybridní scénáře](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) kde tooslowly organizace chcete migrovat úlohy toohello cloudu. V takových scénářů, postupujte podle hello [obecné bezpečnostní aspekty IaaS](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx)a použijte osvědčené postupy tooall zabezpečení virtuální počítače.

Tento článek popisuje různé osvědčené postupy zabezpečení virtuálních počítačů, všechny odvozené od našich zákazníků a vlastní přímé prostředí s virtuálními počítači.

Hello osvědčené postupy jsou založené na konsenzu názorů a budou pracovat s aktuální možnostech platformy Azure a sady funkcí. Protože časem změnit názory a technologie, plánujeme tooupdate Tento článek pravidelně tooreflect tyto změny.

Pro každý osvědčený postup hello článek vysvětluje:

* Jaké hello osvědčeným postupem je.
* Proto je vhodné tooenable ho.
* Jak můžete získat informace tooenable ho.
* Co může dojít, pokud selže tooenable ho.
* Možné alternativy toohello osvědčený postup.

článek Hello prozkoumá hello následující osvědčené postupy zabezpečení virtuálních počítačů:

* Virtuální počítač ověřování a řízení přístupu
* Přístup k dostupnosti a sítě virtuálních počítačů
* Ochrana dat v klidovém stavu uložených ve virtuálních počítačích vynucením šifrování
* Spravovat vaše aktualizace virtuálního počítače
* Spravovat lepšímu zabezpečení virtuálního počítače
* Monitorování výkonu virtuálních počítačů

## <a name="vm-authentication-and-access-control"></a>Virtuální počítač ověřování a řízení přístupu

Hello prvním krokem při ochraně virtuálního počítače je možné tooset si nové virtuální počítače jsou tooensure, který jen na autorizované uživatele. Můžete použít [zásady Azure Resource Manager](../azure-resource-manager/resource-manager-policy.md) tooestablish konvence pro prostředky ve vaší organizaci a vytvářet vlastní zásady a použít tyto zásady tooresources, jako například [skupiny prostředků](../azure-resource-manager/resource-group-overview.md).

Virtuální počítače, které patří přirozeně tooa skupinu prostředků dědí jejími zásadami. I když tento postup doporučujeme toomanaging virtuální počítače, můžete také zásady řízení přístupu tooindividual virtuálních počítačů pomocí [řízení přístupu na základě role (RBAC)](../active-directory/role-based-access-control-configure.md).

Když povolíte Resource Manager zásady a přístup virtuálních počítačů toocontrol RBAC, můžete k vylepšování celkové zabezpečení virtuálních počítačů. Doporučujeme vám konsolidace virtuálních počítačů s hello stejný životní cyklus do hello stejnou skupinu prostředků. Pomocí skupin prostředků, můžete nasadit, monitorování a souhrnné náklady pro vaše prostředky fakturace. tooaccess tooenable uživatele a nastavení virtuálních počítačů, použijte [alespoň oprávnění přístupu](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models). A při přiřazení oprávnění toousers naplánovat toouse hello následující předdefinované role Azure:

- [Virtuální počítač Přispěvatel](../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor): můžete spravovat virtuální počítače, ale není hello virtuální sítě nebo úložiště toowhich účtu jsou připojené.
- [Classic Přispěvatel virtuálních počítačů](../active-directory/role-based-access-built-in-roles.md#classic-virtual-machine-contributor): můžete spravovat virtuální počítače vytvořené pomocí modelu nasazení classic hello, ale není hello virtuální sítě nebo úložiště účet toowhich hello virtuální počítače jsou připojené.
- [Správce zabezpečení](../active-directory/role-based-access-built-in-roles.md#security-manager): můžete spravovat součásti zabezpečení, zásady zabezpečení a virtuálních počítačů.
- [Uživatel DevTest Labs](../active-directory/role-based-access-built-in-roles.md#devtest-labs-user): můžete zobrazit vše, co a připojení, spuštění, restartování a vypnout virtuální počítače.

Nesdílejte účty a hesla mezi správci a nemáte opakovaně používat hesla mezi více uživatelských účtů nebo služeb, zejména hesla pro sociálních médií nebo dalšími aktivitami bez oprávnění správce. V ideálním případě byste měli používat [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) tooset šablony až virtuální počítače bezpečně. Pomocí tohoto přístupu může posílit vaše volby nasazení a vynucení nastavení zabezpečení v rámci nasazení hello.

Organizace, které nebudou vynucovat řízení přístupu k datům díky funkcí, jako je RBAC může být jejich uživatelům udělen další oprávnění, než je nezbytné. Nevhodný uživatelská data toocertain přístupu může ohrozit přímo tato data.

## <a name="vm-availability-and-network-access"></a>Přístup k dostupnosti a sítě virtuálních počítačů

Pokud virtuální počítač používá kritické aplikace, které potřebují toohave vysokou dostupnost, důrazně doporučujeme použít víc virtuálních počítačů. Pro lepší dostupnost, vytvořte alespoň dva virtuální počítače v hello [skupinu dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md).

[Azure Vyrovnávání zatížení](../load-balancer/load-balancer-overview.md) také vyžaduje, aby Vyrovnávání zatížení sítě virtuálních počítačů patří toohello stejné skupině dostupnosti. Pokud tyto virtuální počítače musí mít přístup hello Internet, musíte nakonfigurovat [nástroj pro vyrovnávání zatížení internetového](../load-balancer/load-balancer-internet-overview.md).

Pokud virtuální počítače jsou zveřejněné toohello Internet, je důležité, které [řízení toku provozu sítě s skupiny zabezpečení sítě (Nsg)](../virtual-network/virtual-networks-nsg.md). Vzhledem k tomu, že skupiny Nsg můžou být použité toosubnets, můžete minimalizovat hello počet skupin Nsg svoje prostředky seskupíte podle podsítí a následným použitím skupin Nsg toohello podsítě. Hello je toocreate úroveň izolace sítě, což lze provést tak, že správně nakonfigurujete hello [zabezpečení sítě](../best-practices-network-security.md) možnosti v Azure.

Můžete taky hello za běhu (JIT) virtuálních počítačů přístupu k funkci z toocontrol Azure Security Center, který má tooa vzdáleného přístupu konkrétním virtuálním počítači a jak dlouho.

Organizace, které nejsou vynutit omezení přístup k síti tooInternet přístupem virtuální počítače jsou zveřejněné toosecurity rizika, například k útoku hrubou silou protokol RDP (Remote Desktop).

## <a name="protect-data-at-rest-in-your-vms-by-enforcing-encryption"></a>Chránit data uložená v virtuální počítače vynucením šifrování

[Šifrování dat v klidovém stavu](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) je povinný krok směrem k suverenity data o ochraně osobních údajů a dodržování předpisů a data. [Azure Disk Encryption](../security/azure-security-disk-encryption.md) umožňuje tooencrypt správci IT Windows a disky virtuálních počítačů IaaS Linux. Šifrování disku kombinuje funkce Bitlockeru Windows hello standardní a dm-crypt hello Linux funkce tooprovide šifrování na svazku pro hello operačního systému a datové disky hello.

Můžete použít šifrování disku toohelp zabezpečit vaše data toomeet vaše organizace požadavky na zabezpečení a dodržování předpisů. Organizaci byste měli zvážit použití šifrování toohelp zmírnit rizika související toounauthorized data access. Doporučujeme také šifrování jednotky, než napíšete toothem citlivá data.

Být jisti tooencrypt tooprotect svazky data vašeho virtuálního počítače je na rest v účtu úložiště Azure. Zabezpečit hello šifrovacích klíčů a tajný klíč pomocí [Azure Key Vault](https://azure.microsoft.com/en-us/documentation/articles/key-vault-whatis/).

Organizace, které není vynuceno šifrování dat jsou více zveřejněné toodata integrity problémy. Například může neoprávněným nebo neautorizovaných serverů uživatelů odcizit data v ohrožené účty nebo získání neoprávněného přístupu toodata programového v ClearFormat. Kromě pořízení na těchto rizik, toocomply s oborové směrnice brát společnosti musí prokázat, že jejich jsou výkonu opatrností a používá správný zabezpečení ovládací prvky tooenhance jejich zabezpečení dat.

toolearn Další informace o šifrování disku, najdete v části [Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux](azure-security-disk-encryption.md).


## <a name="manage-your-vm-updates"></a>Spravovat vaše aktualizace virtuálního počítače

Virtuální počítače Azure, jako jsou všechny místní virtuální počítače, protože jsou určený toobe spravovaná uživatelem, Azure nemá push toothem aktualizace systému Windows. Jste však podporovat tooleave hello automatické nastavení Windows Update povolena. Další možností je toodeploy [Windows Server Update Services (WSUS)](https://technet.microsoft.com/windowsserver/bb332157.aspx) nebo jiné vhodné správy aktualizací produktu buď na jiný počítač nebo místní. Služba WSUS a Windows Update zachovat virtuální počítače aktuální. Doporučujeme také, že používáte prohledávání tooverify produktu, které jsou všechny virtuální počítače IaaS toodate.

Uložené Image poskytovaný Azure jsou pravidelně aktualizované tooinclude hello zaokrouhlí nejnovější aktualizací systému Windows. Však není zaručeno, že bude aktuální v době nasazení bitové kopie hello. Mírné prodleva (maximálně několik týdnů) následující veřejné verze může být možné. Pro kontrolu a instalaci všech aktualizací systému Windows musí být prvním krokem hello každé nasazení. Tato míra je obzvláště důležité tooapply při nasazování bitových kopií, které pocházejí z vy nebo vaše vlastní knihovny. Ve výchozím nastavení se automaticky aktualizují bitové kopie, které jsou k dispozici jako součást hello Azure Marketplace.

Organizace, které nejsou vynutit zásady aktualizace softwaru jsou více zveřejněné toothreats, které zneužívají ohrožení zabezpečení známé, dříve pevný. Kromě riskujete tyto hrozby, toocomply s předpisy odvětví společnosti musí prokázat, že jejich jsou výkonu opatrností a pomocí ovládacích prvků toohelp správné zabezpečení zajistit zabezpečení hello jejich úlohy umístěný v cloudu hello.

Je důležité tooemphasize, které aktualizace softwaru osvědčené postupy pro tradičních datových center a Azure IaaS mít mnoho podobností. Proto doporučujeme vyhodnotit vaše aktuální softwarové aktualizace zásady tooinclude virtuálních počítačů.

## <a name="manage-your-vm-security-posture"></a>Spravovat lepšímu zabezpečení virtuálního počítače

Internetový hrozeb se vyvíjejí a zabezpečení virtuální počítače vyžaduje s formátováním monitorovací schopností, které můžete rychle zjišťovat hrozby, zabránit neoprávněnému přístupu tooyour prostředky, výstrahy aktivovat a snížil počet falešných poplachů. Hello postavení zabezpečení pro taková zatížení zahrnuje všechny aspekty zabezpečení hello virtuální počítač, z aktualizace správu toosecure síťový přístup.

toomonitor hello postavení zabezpečení vaší [Windows](../security-center/security-center-virtual-machine.md) a [virtuální počítače s Linuxem](../security-center/security-center-linux-virtual-machine.md), použijte [Azure Security Center](../security-center/security-center-intro.md). V Azure Security Center chrání virtuální počítače díky hello následující možnosti:

* Použít nastavení zabezpečení operačního systému s doporučenou konfiguraci pravidla
* Identifikovat a stáhnout systému zabezpečení a důležité aktualizace, které může být chybějící
* Nasazení doporučení ochrany proti malwaru Endpoint
* Ověření šifrování disku
* Hodnocení a opravu chyb zabezpečení
* Detekce hrozeb

Security Center můžete sledovat aktivně hrozby a potenciální hrozby jsou viditelné v rámci **výstrahy zabezpečení**. Korelační hrozeb jsou agregován v rámci jednoho zobrazení názvem **incidentu zabezpečení**.

toounderstand jak Security Center pomáhá identifikovat potenciální hrozby v virtuální počítače umístěné v Azure, podívejte se na následující videa hello:

<iframe src="https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Security-Center-in-Incident-Response/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

Organizace, které nevynucují postavení silné zabezpečení pro jejich virtuální počítače zůstanou nebere v úvahu potenciální pokusů z ovládacích prvků zabezpečení toocircumvent navázat neoprávnění uživatelé.

## <a name="monitor-vm-performance"></a>Monitorování výkonu virtuálních počítačů

Zneužití prostředku může být problém, když počítač procesy spotřebovávají více prostředků, než by měly. Problémy s výkonem se virtuální počítač může způsobit přerušení tooservice, která porušuje zásadu zabezpečení hello dostupnosti. Z tohoto důvodu není nutné toomonitor virtuálních počítačů přístup pouze reaktivně při problému dochází, ale také proaktivně proti základní výkon naměřenou při běžném provozu.

Analýzou [soubory protokolů Azure diagnostiky](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/), můžete sledovat vaše prostředky virtuálních počítačů a identifikovat potenciální problémy, které může dojít k ohrožení výkon a dostupnost. Hello rozšíření diagnostiky Azure nabízí funkce monitorování a Diagnostika na virtuálních počítačích se systémem Windows. Tyto možnosti můžete povolit, přiložením rozšíření hello jako součást hello [šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md).

Můžete také použít [Azure monitorování](../monitoring-and-diagnostics/monitoring-overview-metrics.md) toogain přehled o stavu vaší prostředků.

Organizace, které nejsou sledovat výkon virtuálních počítačů jsou nelze toodetermine, jestli jsou některé změny v vzory výkonu normální nebo neobvyklé. Pokud hello virtuálního počítače je spotřebovávat více prostředků, než je obvyklé, takové anomálií by to znamenat potenciální útoky z externí prostředek nebo ohroženými procesu spuštěného v hello virtuálních počítačů.
