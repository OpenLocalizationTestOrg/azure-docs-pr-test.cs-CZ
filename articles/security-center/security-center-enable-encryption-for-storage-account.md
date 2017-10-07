---
title: "aaaEnable šifrování pro účet úložiště v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello doporučení služby Azure Security Center ** povolit šifrování pro rozhraní Azure úložiště účet **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: terrylan
ms.openlocfilehash: c5cbafbf3a8be86f213dcf1c0c0ddcc0222b3d95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a>Povolit šifrování pro účet úložiště Azure v Azure Security Center
Azure Security Center může doporučujeme povolit šifrování služby úložiště Azure pro data v klidovém stavu.

Šifrování služby úložiště (SSE) funguje tak, že šifrování dat hello, když dojde k zapsání tooAzure úložiště a dešifrování dat hello před načtení.  SSE aktuálně nejsou k dispozici pouze pro hello služby objektů Blob Azure lze použít pro objekty BLOB bloku, objektů BLOB stránky a doplňovacích objektů BLOB.  Další, najdete v části toolearn [šifrování služby úložiště pro data v klidovém stavu](../storage/common/storage-service-encryption.md).


> [!Note]
> Po povolení šifrování, se šifrují jenom nová data. Všechny existující objekty BLOB ve vašem účtu úložiště zůstat nezašifrovaný. tooencrypt existující objekty BLOB, najdete v části hello [nejčastější dotazy týkající se úložiště služby šifrování](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).
>
>

Šifrování služby úložiště je podporována pouze na účty úložiště Resource Manager. Klasické účty úložiště nejsou aktuálně podporovány. toounderstand hello classic a modelech nasazení Resource Manager, najdete v části [modelech nasazení Azure](../azure-classic-rm.md).

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Tento dokument není to podrobný průvodce.
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení** vyberte **povolit šifrování pro účet úložiště Azure**.
   ![Povolení šifrování pro účet úložiště][1]
2. Hello **povolit šifrování úložiště** otevře se okno. Toto okno seznam účtů úložiště Azure hello, kde je zakázaný šifrování úložiště. V tomto příkladu budeme vyberte **storageacct1**.
   ![Povolit šifrování úložiště][2]
3. Hello **šifrování** okně **storageacct1** otevře. Vyberte **povoleno**.
   ![Okno šifrování][3]
4. Vyberte **Uložit**.

Nyní jste povolili šifrování úložiště pro **storageacct1**.


## <a name="see-also"></a>Viz také
Tento dokument ukázal, jak tooimplement hello Security Center doporučení "Povolit šifrování pro účet služby Azure Storage." toolearn Další informace o šifrování služby úložiště Azure, najdete v části hello následující:

* [Šifrování služby úložiště Azure pro Data v klidovém stavu](../storage/common/storage-service-encryption.md)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
