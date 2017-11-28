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
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a><span data-ttu-id="3e44f-103">Povolit šifrování pro účet úložiště Azure v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="3e44f-103">Enable encryption for Azure storage account in Azure Security Center</span></span>
<span data-ttu-id="3e44f-104">Azure Security Center může doporučujeme povolit šifrování služby úložiště Azure pro data v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="3e44f-104">Azure Security Center may recommend that you enable Azure Storage Service Encryption for data at rest.</span></span>

<span data-ttu-id="3e44f-105">Šifrování služby úložiště (SSE) funguje tak, že šifrování dat hello, když dojde k zapsání tooAzure úložiště a dešifrování dat hello před načtení.</span><span class="sxs-lookup"><span data-stu-id="3e44f-105">Storage Service Encryption (SSE) works by encrypting hello data when it is written tooAzure storage and decrypting hello data before retrieval.</span></span>  <span data-ttu-id="3e44f-106">SSE aktuálně nejsou k dispozici pouze pro hello služby objektů Blob Azure lze použít pro objekty BLOB bloku, objektů BLOB stránky a doplňovacích objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="3e44f-106">SSE is currently available only for hello Azure Blob service and can be used for block blobs, page blobs, and append blobs.</span></span>  <span data-ttu-id="3e44f-107">Další, najdete v části toolearn [šifrování služby úložiště pro data v klidovém stavu](../storage/common/storage-service-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="3e44f-107">toolearn more, see [Storage Service Encryption for data at rest](../storage/common/storage-service-encryption.md).</span></span>


> [!Note]
> <span data-ttu-id="3e44f-108">Po povolení šifrování, se šifrují jenom nová data.</span><span class="sxs-lookup"><span data-stu-id="3e44f-108">After enabling encryption, only new data is encrypted.</span></span> <span data-ttu-id="3e44f-109">Všechny existující objekty BLOB ve vašem účtu úložiště zůstat nezašifrovaný.</span><span class="sxs-lookup"><span data-stu-id="3e44f-109">Any existing blobs in your storage account remain unencrypted.</span></span> <span data-ttu-id="3e44f-110">tooencrypt existující objekty BLOB, najdete v části hello [nejčastější dotazy týkající se úložiště služby šifrování](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).</span><span class="sxs-lookup"><span data-stu-id="3e44f-110">tooencrypt existing blobs, see hello [Storage Service Encryption FAQ](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).</span></span>
>
>

<span data-ttu-id="3e44f-111">Šifrování služby úložiště je podporována pouze na účty úložiště Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3e44f-111">Storage Service Encryption is only supported on Resource Manager storage accounts.</span></span> <span data-ttu-id="3e44f-112">Klasické účty úložiště nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="3e44f-112">Classic storage accounts are not currently supported.</span></span> <span data-ttu-id="3e44f-113">toounderstand hello classic a modelech nasazení Resource Manager, najdete v části [modelech nasazení Azure](../azure-classic-rm.md).</span><span class="sxs-lookup"><span data-stu-id="3e44f-113">toounderstand hello classic and Resource Manager deployment models, see [Azure deployment models](../azure-classic-rm.md).</span></span>

> [!NOTE]
> <span data-ttu-id="3e44f-114">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="3e44f-114">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="3e44f-115">Tento dokument není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="3e44f-115">This document is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="3e44f-116">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="3e44f-116">Implement hello recommendation</span></span>
1. <span data-ttu-id="3e44f-117">V hello **doporučení** vyberte **povolit šifrování pro účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="3e44f-117">In hello **Recommendations** blade, select **Enable encryption for Azure Storage Account**.</span></span>
   <span data-ttu-id="3e44f-118">![Povolení šifrování pro účet úložiště][1]</span><span class="sxs-lookup"><span data-stu-id="3e44f-118">![Enable encryption for storage account][1]</span></span>
2. <span data-ttu-id="3e44f-119">Hello **povolit šifrování úložiště** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="3e44f-119">hello **Enable storage encryption** blade opens.</span></span> <span data-ttu-id="3e44f-120">Toto okno seznam účtů úložiště Azure hello, kde je zakázaný šifrování úložiště.</span><span class="sxs-lookup"><span data-stu-id="3e44f-120">This blade lists hello Azure storage accounts where storage encryption is disabled.</span></span> <span data-ttu-id="3e44f-121">V tomto příkladu budeme vyberte **storageacct1**.</span><span class="sxs-lookup"><span data-stu-id="3e44f-121">In this example, let's select **storageacct1**.</span></span>
   <span data-ttu-id="3e44f-122">![Povolit šifrování úložiště][2]</span><span class="sxs-lookup"><span data-stu-id="3e44f-122">![Enable storage encryption][2]</span></span>
3. <span data-ttu-id="3e44f-123">Hello **šifrování** okně **storageacct1** otevře.</span><span class="sxs-lookup"><span data-stu-id="3e44f-123">hello **Encryption** blade for **storageacct1** opens.</span></span> <span data-ttu-id="3e44f-124">Vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="3e44f-124">Select **Enabled**.</span></span>
   <span data-ttu-id="3e44f-125">![Okno šifrování][3]</span><span class="sxs-lookup"><span data-stu-id="3e44f-125">![Encryption blade][3]</span></span>
4. <span data-ttu-id="3e44f-126">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3e44f-126">Select **Save**.</span></span>

<span data-ttu-id="3e44f-127">Nyní jste povolili šifrování úložiště pro **storageacct1**.</span><span class="sxs-lookup"><span data-stu-id="3e44f-127">You have now enabled storage encryption for **storageacct1**.</span></span>


## <a name="see-also"></a><span data-ttu-id="3e44f-128">Viz také</span><span class="sxs-lookup"><span data-stu-id="3e44f-128">See also</span></span>
<span data-ttu-id="3e44f-129">Tento dokument ukázal, jak tooimplement hello Security Center doporučení "Povolit šifrování pro účet služby Azure Storage."</span><span class="sxs-lookup"><span data-stu-id="3e44f-129">This document showed you how tooimplement hello Security Center recommendation "Enable encryption for Azure Storage Account."</span></span> <span data-ttu-id="3e44f-130">toolearn Další informace o šifrování služby úložiště Azure, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="3e44f-130">toolearn more about Azure Storage Service Encryption, see hello following:</span></span>

* [<span data-ttu-id="3e44f-131">Šifrování služby úložiště Azure pro Data v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="3e44f-131">Azure Storage Service Encryption for Data at Rest</span></span>](../storage/common/storage-service-encryption.md)

<span data-ttu-id="3e44f-132">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="3e44f-132">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="3e44f-133">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="3e44f-133">[Setting security policies in Azure Security Center](security-center-policies.md) - Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="3e44f-134">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="3e44f-134">[Security health monitoring in Azure Security Center](security-center-monitoring.md) - Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="3e44f-135">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="3e44f-135">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) - Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="3e44f-136">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="3e44f-136">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) - Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="3e44f-137">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="3e44f-137">[Azure Security Center FAQ](security-center-faq.md) - Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="3e44f-138">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="3e44f-138">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) - Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
