---
title: "šifrování disku aaaApply v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** použít šifrování disku **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 6cc7824a-8d6b-4a5f-ab40-e3bbaebc4a91
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: cd803f1120018c5c86da91186eec1e59d425ede7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apply-disk-encryption-in-azure-security-center"></a>Použít šifrování disku v Azure Security Center
Azure Security Center doporučuje použít šifrování disku, pokud máte Windows nebo virtuálního počítače s Linuxem disků, které nejsou šifrovány pomocí Azure Disk Encryption. Disk Encryption umožňuje šifrování disků systému Windows a virtuálních počítačů IaaS Linux.  Šifrování se doporučuje pro hello operačního systému a datové svazky na vašem virtuálním počítači.

Šifrování disku používá hello oborový standard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkce systému Windows a hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funkce systému Linux. Tyto funkce poskytují operačního systému a dat šifrování toohelp chránit a zabezpečení dat a splňují vaše organizace zabezpečení a dodržování předpisů závazky. Šifrování disku je integrovaná s [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp můžete řídit a spravovat hello disku šifrovacích klíčů a tajných klíčů ve vašem předplatném Key Vault, a zajistit, že všechna data v hello disky virtuálních počítačů jsou zašifrovaná přinejmenším ve vaší [ Úložiště Azure](https://azure.microsoft.com/documentation/services/storage/).

> [!NOTE]
> Azure Disk Encryption je podporována v hello následující operační systémy Windows server – Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2. Šifrování disku je podporován v následujících operačních systémů Linux server - Ubuntu, CentOS, SUSE a SUSE Linux Enterprise Server (SLES) hello.
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení** vyberte **použít šifrování disku**.
2. V hello **použít šifrování disku** okno, zobrazí se seznam virtuálních počítačů, u kterých se doporučuje šifrování disku.
3. Postupujte podle hello pokyny tooapply šifrování toothese virtuálních počítačů.

![][1]

tooencrypt virtuální počítače Azure, které byly identifikovány pomocí služby Security Center potřebují šifrování, doporučujeme hello následující kroky:

* Instalace a konfigurace Azure Powershellu. To vám umožní toorun hello prostředí PowerShell příkazy požadované tooset až hello požadavky požadované tooencrypt virtuální počítače Azure.
* Získat a spustit skript prostředí Azure PowerShell Azure Disk Encryption požadavky hello.
* Šifrování virtuálních počítačů.

[Šifrování virtuálního počítače Azure](security-center-disk-encryption.md) vás provede tyto kroky.  Toto téma předpokládá, že používáte Windows 10 jako hello klientský počítač, ve kterém můžete nakonfigurovat šifrování disku.

Existuje mnoho přístupů, které lze použít pro virtuální počítače Azure. Pokud jste už dobře versed v prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure, dáte možná přednost alternativním přístupům toouse. toolearn o tyto postupy, najdete v části [Azure disk encryption](../security/azure-security-disk-encryption.md).

## <a name="see-also"></a>Viz také
Tento dokument ukázal, jak tooimplement hello Security Center doporučení "použít šifrování disku." toolearn Další informace o šifrování disku, najdete v části hello následující:

* [Šifrování a klíč správy s Azure Key Vault](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video, 36 minut 39 sekund) – zjistěte, jak toouse disku Správa šifrování pro virtuální počítače IaaS a toohelp Azure Key Vault chránit a chrání vaše data.
* [Šifrování virtuálního počítače Azure](security-center-disk-encryption.md) (dokument) – zjistěte, jak tooencrypt virtuální počítače Azure.
* [Azure disk encryption](../security/azure-security-disk-encryption.md) (dokument) – zjistěte, jak tooenable disku šifrování pro systém Windows a virtuální počítače s Linuxem.

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
