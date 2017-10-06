---
title: "Nejčastější dotazy k šifrování disku aaaAzure | Microsoft Docs"
description: "Tento článek obsahuje odpovědi toofrequently dotazy pro Microsoft Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux."
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
ms.openlocfilehash: 17f084628ba4ef22e9d37dd3052ef10f8eb2cd7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-frequently-asked-questions-faq"></a>Azure Disk Encryption nejčastější dotazy (FAQ)

Tyto nejčastější dotazy odpovídá na dotazy týkající se Azure disk encryption pro systém Windows a virtuálních počítačů IaaS Linux, další informace o této službě, přečtěte si [Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="general-questions"></a>Obecné otázky
**Otázka:** Které oblasti je Azure disk encryption v GA?

**Odpověď:** Azure disk encryption pro systém Windows a virtuálních počítačů IaaS Linux je k dispozici v GA ve všech oblastech Azure veřejné.

**Otázka:** vyskytne uživatele, jsou k dispozici s Azure Disk Encryption?

**Odpověď:** Azure Disk Encryption GA podporuje šablon Azure Resource Manageru, Azure PowerShell, rozhraní příkazového řádku Azure. To vám přináší značnou flexibilitu v tom, že existují tři různé možnosti pro povolení šifrování disku pro virtuální počítače IaaS. Další podrobnosti o hello uživatelské rozhraní a pokyny krok za krokem je k dispozici ve scénářích nasazení Azure Disk Encryption hello a prostředí.

**Otázka:** kolik Azure Disk Encryption náklady?

**Odpověď:** je bezplatná pro šifrování disky virtuálních počítačů s Azure Disk Encryption.

**Otázka:** jaké vrstvy virtuálního počítače je možné používat Azure Disk Encryption s?

**Odpověď:** Azure Disk Encryption je dostupná jen pro standardní virtuální počítače vrstvy včetně [A, D, DS, G, GS, F](https://azure.microsoft.com/pricing/details/virtual-machines/) a tak dále virtuální počítače IaaS řady včetně virtuálních počítačů s storage úrovně premium. Není k dispozici na základní virtuálních počítačích vrstvy.

**Otázka:** co Linuxových distribucích podporuje Azure Disk Encryption?

**Odpověď:** Azure Disk Encryption je podporována v hello následující Linux serveru distribucích a verzích:

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

**Odpověď:** zákazníci mohou získat informace, jak tooget spustí hello čtení dokumentu White Paper Azure Disk Encryption nachází [sem](https://docs.microsoft.com/azure/security/azure-security-disk-encryption)

**Otázka:** můžete šifrovat spouštěcí a datové svazky s Azure Disk Encryption?

**Odpověď:** Ano, můžete šifrovat spouštěcí a datové svazky pro systém Windows a virtuálních počítačů IaaS Linux. U virtuálních počítačů Windows nelze zašifrovat hello dat bez první encrpting hello svazku operačního systému. Pro virtuální počítače s Linuxem můžete nejdřív šifrovat hello datový svazek bez encryptinng hello svazku operačního systému. Jakmile jste zašifrovali svazku hello operačního systému pro clusterech, zakázáním šifrování na svazku operačního systému pro virtuální počítače IaaS Linux není podporována.

**Otázka:** nemá Azure Disk Encryption povolit "přineste si vlastní klíč" (BYOK) funkce?

**Odpověď:** Ano, můžete zadat vlastní klíče šifrovací klíče. Tyto klíče jsou chráněné v Azure Key Vault, což je hello úložiště klíčů pro Azure Disk Encryption. Další informace o hello klíče šifrovací klíč podporu scénářů najdete v tématu scénáře nasazení Azure Disk Encryption hello a prostředí

**Otázka:** můžete použít Azure vytvořit klíče šifrovací klíč?

**Odpověď:** Ano, můžete použít Azure Key vault toogenerate klíče šifrovací klíč pro použití Azure disk encryption. Tyto klíče jsou chráněné v Azure Key Vault, což je hello úložiště klíčů pro Azure Disk Encryption. Další informace o hello klíče šifrovací klíč podporu scénářů najdete v tématu scénáře nasazení Azure Disk Encryption hello a prostředí

**Otázka:** můžete použít místní správy klíčů služby nebo HSM toosafeguard hello šifrovacích klíčů?

**Odpověď:** hello místní správy klíčů služby nebo HSM toosafeguard hello šifrovací klíče nelze použít s Azure disk encryption. Můžete použít pouze hello Azure trezor klíčů služby toosafeguard hello šifrovací klíče. Další informace o hello klíče šifrovací klíč podporu scénářů najdete v tématu scénáře nasazení Azure Disk Encryption hello a prostředí

**Otázka:** co jsou hello požadavky tooconfigure Azure disk encryption?

**Odpověď:** hello Azure disk encryption požadovaných prostředí PowerShell toocreate AAD aplikace skriptu, vytvoření nového trezoru klíčů nebo nastavit existující trezor klíčů pro disk šifrování přístup tooenable šifrování a chránit tajných klíčů a klíč.  Další informace o hello klíče šifrovací klíč podporu scénářů najdete v tématu požadavky pro Azure Disk Encryption hello a scénáře nasazení a prostředí

**Otázka:** kde může získat další informace o toouse prostředí PowerShell pro konfiguraci Azure Disk Encryption?

**Odpověď:** máme některé skvělé články v tom, jak můžete provádět základní úlohy Azure Disk Encryption, jakož i pokročilejší scénáře. Pro základní úlohy hello, podívejte se na prozkoumat [Azure Disk Encryption s prostředím Azure PowerShell – část 1](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Pro pokročilejší scénáře, najdete v části prozkoumat [Azure Disk Encryption s prostředím Azure PowerShell – část 2](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/)

**Otázka:** jakou verzi prostředí Azure PowerShell podporuje Azure Disk Encryption?

**Odpověď:** použití hello nejnovější verzi Azure PowerShell SDK verze tooconfigure Azure Disk Encryption. Stáhněte si nejnovější verzi hello [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Azure Disk Encryption sadou Azure SDK verze 1.1.0 není podporován.

> [!NOTE]
> Hello Linux Azure disk encryption preview rozšíření je zastaralý. Podrobnosti najdete toodocumentation [sem](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/)

**Otázka:** můžete použít Azure disk encryption na mé vlastní bitovou kopii systému Linux?

**Odpověď:** Azure disk encryption nelze použít na vlastní bitovou kopii systému Linux. Podporujeme jenom hello Galerie Linux bitové kopie pro hello podporované distribucích popsali výše. Vlastní Image Linux aktuálně Nepodporujeme

**Otázka:** můžete použít aktualizace tooa virtuálního počítače s Linuxem Red Hat používání Yum aktualizace?

**Odpověď:** Ano, můžete provést aktualizaci a nebo opravit virtuální počítač Red Hat Linnux následujících pokynů uvedených [sem](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/)

**Otázka:** kde mohu přejděte tooask otázku nebo poskytnutí zpětné vazby

**Odpověď:** můžete zadat, požádejte dotazy nebo připomínky na hello Azure disk encryption fórum [sem](https://social.msdn.microsoft.com/Forums/home?forum=AzureDiskEncryption)

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli více o hello nejčastějšími dotazy související tooAzure šifrování disku, další informace o této služby a jeho schopnosti číst:

- [Použít šifrování disku v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Šifrování virtuálního počítače Azure](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Šifrování na Rest Azure dat](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
