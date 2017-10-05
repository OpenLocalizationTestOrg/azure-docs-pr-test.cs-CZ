---
title: "Azure Disk Encryption – nejčastější dotazy | Microsoft Docs"
description: "Tento článek obsahuje odpovědi na nejčastější dotazy pro Microsoft Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux."
services: security
documentationcenter: na
author: deventiwari
manager: avibm
editor: yuridio
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: devtiw
ms.openlocfilehash: 0d15bf42c156ea7a72c54d690f4016877913efe4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-disk-encryption-frequently-asked-questions-faq"></a>Azure Disk Encryption nejčastější dotazy (FAQ)

Tyto nejčastější dotazy odpovídá na dotazy týkající se Azure disk encryption pro systém Windows a virtuálních počítačů IaaS Linux, další informace o této službě, přečtěte si [Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="general-questions"></a>Obecné otázky
**Otázka:** Které oblasti je Azure disk encryption v GA?

**Odpověď:** Azure disk encryption pro systém Windows a virtuálních počítačů IaaS Linux je k dispozici v GA ve všech oblastech Azure veřejné.

**Otázka:** vyskytne uživatele, jsou k dispozici s Azure Disk Encryption?

**Odpověď:** Azure Disk Encryption GA podporuje šablon Azure Resource Manageru, Azure PowerShell, rozhraní příkazového řádku Azure. To vám přináší značnou flexibilitu v tom, že existují tři různé možnosti pro povolení šifrování disku pro virtuální počítače IaaS. Další informace o uživatelské rozhraní a pokyny krok za krokem je k dispozici v Azure Disk Encryption scénáře nasazení a prostředí.

**Otázka:** kolik Azure Disk Encryption náklady?

**Odpověď:** je bezplatná pro šifrování disky virtuálních počítačů s Azure Disk Encryption.

**Otázka:** jaké vrstvy virtuálního počítače je možné používat Azure Disk Encryption s?

**Odpověď:** Azure Disk Encryption je dostupná jen pro standardní virtuální počítače vrstvy včetně [A, D, DS, G, GS, F](https://azure.microsoft.com/pricing/details/virtual-machines/) a tak dále virtuální počítače IaaS řady včetně virtuálních počítačů s storage úrovně premium. Není k dispozici na základní virtuálních počítačích vrstvy.

**Otázka:** co Linuxových distribucích podporuje Azure Disk Encryption?

**Odpověď:** Azure Disk Encryption je podporováno v následujících Linuxových distribucích serveru a verze:

| Distribuce systému Linux | Verze | Typ svazku podporovaný pro šifrování|
| --- | --- |--- |
| Ubuntu | 16.04. DENNĚ LTS | Disk operačního systému a dat |
| Ubuntu | 14.04.5-DAILY-LTS | Disk operačního systému a dat |
| RHEL | 7.3 | Disk operačního systému a dat |
| RHEL | 7.2 | Disk operačního systému a dat |
| RHEL | 6.8 | Disk operačního systému a dat |
| RHEL | 6.7 | Datový disk |
| CentOS | 7.3 | Disk operačního systému a dat |
| CentOS | 7.2N | Disk operačního systému a dat |
| CentOS | 6.8 | Disk operačního systému a dat |
| CentOS | 7.1 | Datový disk |
| CentOS | 7.0 | Datový disk |
| CentOS | 6.7 | Datový disk |
| CentOS | 6.6 | Datový disk |
| CentOS | 6.5 | Datový disk |
| openSUSE | 13.2 | Datový disk |
| SLES | 12 SP1 | Datový disk |
| SLES | Priorita: 12-SP1 | Datový disk |
| SLES | HPC 12 | Datový disk |
| SLES | Priorita: 11-SP4 | Datový disk |
| SLES | 11 SP4 | Datový disk |

**Otázka:** Jak můžu začít pomocí Azure Disk Encryption?

**Odpověď:** zákazníci mohou zjistěte, jak začít v dokumentu White Paper Azure Disk Encryption umístěné čtení [sem](https://docs.microsoft.com/azure/security/azure-security-disk-encryption)

**Otázka:** můžete šifrovat spouštěcí a datové svazky s Azure Disk Encryption?

**Odpověď:** Ano, můžete šifrovat spouštěcí a datové svazky pro systém Windows a virtuálních počítačů IaaS Linux. U virtuálních počítačů Windows nelze zašifrovat data bez první encrpting svazku operačního systému. Pro virtuální počítače s Linuxem můžete nejdřív šifrovat datový svazek bez svazku encryptinng operačního systému. Jakmile jste zašifrovali svazku operačního systému pro clusterech, zakázáním šifrování na svazku operačního systému pro virtuální počítače IaaS Linux není podporována.

**Otázka:** nemá Azure Disk Encryption povolit "přineste si vlastní klíč" (BYOK) funkce?

**Odpověď:** Ano, můžete zadat vlastní klíče šifrovací klíče. Tyto klíče jsou chráněné v Azure Key Vault, což je úložiště klíčů pro Azure Disk Encryption. Další podrobnosti o scénářích klíče podpora šifrování pomocí klíče najdete v tématu Azure Disk Encryption scénáře nasazení a prostředí

**Otázka:** můžete použít Azure vytvořit klíče šifrovací klíč?

**Odpověď:** Azure Key vault Ano, můžete použít ke generování klíče šifrovací klíč pro použití Azure disk encryption. Tyto klíče jsou chráněné v Azure Key Vault, což je úložiště klíčů pro Azure Disk Encryption. Další podrobnosti o scénářích klíče podpora šifrování pomocí klíče najdete v tématu Azure Disk Encryption scénáře nasazení a prostředí

**Otázka:** je možné pomocí služby správy klíčů místní/HSM zabezpečit šifrovacích klíčů?

**Odpověď:** pomocí místní správy klíčů služby nebo modul hardwarového zabezpečení nelze zabezpečit šifrovacích klíčů s Azure disk encryption. Aby se předešlo šifrovací klíče můžete použít pouze službu Azure trezoru klíčů. Další podrobnosti o scénářích klíče podpora šifrování pomocí klíče najdete v tématu Azure Disk Encryption scénáře nasazení a prostředí

**Otázka:** jaké jsou požadavky na konfiguraci služby Azure disk encryption?

**Odpověď:** Azure disk encryption požadovaných skript Powershellu pro vytvoření AAD aplikace, vytvoření nového trezoru klíčů nebo instalační program existující trezor klíčů pro přístup k šifrování disku povolit šifrování a chránit tajných klíčů a klíč.  Další podrobnosti o scénářích klíče podpora šifrování pomocí klíče najdete v tématu Azure Disk Encryption požadavky a scénáře nasazení a prostředí

**Otázka:** kde může získat další informace o tom, jak pomocí prostředí PowerShell pro konfiguraci Azure Disk Encryption?

**Odpověď:** máme některé skvělé články v tom, jak můžete provádět základní úlohy Azure Disk Encryption, jakož i pokročilejší scénáře. Pro základní úlohy, podívejte se na prozkoumat [Azure Disk Encryption s prostředím Azure PowerShell – část 1](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Pro pokročilejší scénáře, najdete v části prozkoumat [Azure Disk Encryption s prostředím Azure PowerShell – část 2](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/)

**Otázka:** jakou verzi prostředí Azure PowerShell podporuje Azure Disk Encryption?

**Odpověď:** použít nejnovější verzi Azure PowerShell SDK verze konfigurovat Azure Disk Encryption. Stáhněte si nejnovější verzi [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Azure Disk Encryption sadou Azure SDK verze 1.1.0 není podporován.

> [!NOTE]
> Rozšíření Linux Azure šifrování disku preview je zastaralý. Podrobnosti najdete v dokumentaci k [sem](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/)

**Otázka:** můžete použít Azure disk encryption na mé vlastní bitovou kopii systému Linux?

**Odpověď:** Azure disk encryption nelze použít na vlastní bitovou kopii systému Linux. Podporujeme pouze obrázky Linux Galerie pro podporovaných distribucích popsali výše. Vlastní Image Linux aktuálně Nepodporujeme

**Otázka:** můžete použít aktualizace pro Red Hat virtuálního počítače s Linuxem pomocí Yum aktualizace?

**Odpověď:** Ano, můžete provést aktualizaci a nebo opravit virtuální počítač Red Hat Linnux následujících pokynů uvedených [sem](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/)

**Otázka:** kde lze přejít dotaz nebo poskytnout zpětnou vazbu

**Odpověď:** můžete zadat, požádejte dotazy nebo připomínky ve fóru Azure disk encryption [sem](https://social.msdn.microsoft.com/Forums/home?forum=AzureDiskEncryption)

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli Další informace o nejčastější dotazy týkající se Azure disk šifrování, další informace o této služby a jeho schopnosti číst:

- [Použít šifrování disku v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Šifrování virtuálního počítače Azure](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Šifrování na Rest Azure dat](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
