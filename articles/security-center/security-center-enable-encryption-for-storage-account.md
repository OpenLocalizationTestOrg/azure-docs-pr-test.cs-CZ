---
title: "Povolit šifrování pro účet úložiště v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak implementovat Azure Security Center doporučení ** povolit šifrování pro rozhraní Azure úložiště účet **."
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
ms.openlocfilehash: b7b2e8a12cbab68da9c8fcc348e8e3c543607007
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a><span data-ttu-id="a4d15-103">Povolit šifrování pro účet úložiště Azure v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="a4d15-103">Enable encryption for Azure storage account in Azure Security Center</span></span>
<span data-ttu-id="a4d15-104">Azure Security Center může doporučujeme povolit šifrování služby úložiště Azure pro data v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="a4d15-104">Azure Security Center may recommend that you enable Azure Storage Service Encryption for data at rest.</span></span>

<span data-ttu-id="a4d15-105">Šifrování služby úložiště (SSE) funguje tak, že šifrování dat, když je zapsán do úložiště Azure a dešifrování dat před načtení.</span><span class="sxs-lookup"><span data-stu-id="a4d15-105">Storage Service Encryption (SSE) works by encrypting the data when it is written to Azure storage and decrypting the data before retrieval.</span></span>  <span data-ttu-id="a4d15-106">SSE aktuálně nejsou k dispozici pouze pro službu Azure Blob lze použít pro objekty BLOB bloku, objektů BLOB stránky a doplňovacích objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="a4d15-106">SSE is currently available only for the Azure Blob service and can be used for block blobs, page blobs, and append blobs.</span></span>  <span data-ttu-id="a4d15-107">Další informace najdete v tématu [šifrování služby úložiště pro data v klidovém stavu](../storage/common/storage-service-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="a4d15-107">To learn more, see [Storage Service Encryption for data at rest](../storage/common/storage-service-encryption.md).</span></span>


> [!Note]
> <span data-ttu-id="a4d15-108">Po povolení šifrování, se šifrují jenom nová data.</span><span class="sxs-lookup"><span data-stu-id="a4d15-108">After enabling encryption, only new data is encrypted.</span></span> <span data-ttu-id="a4d15-109">Všechny existující objekty BLOB ve vašem účtu úložiště zůstat nezašifrovaný.</span><span class="sxs-lookup"><span data-stu-id="a4d15-109">Any existing blobs in your storage account remain unencrypted.</span></span> <span data-ttu-id="a4d15-110">K šifrování existující objekty BLOB, najdete v článku [nejčastější dotazy týkající se úložiště služby šifrování](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).</span><span class="sxs-lookup"><span data-stu-id="a4d15-110">To encrypt existing blobs, see the [Storage Service Encryption FAQ](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).</span></span>
>
>

<span data-ttu-id="a4d15-111">Šifrování služby úložiště je podporována pouze na účty úložiště Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a4d15-111">Storage Service Encryption is only supported on Resource Manager storage accounts.</span></span> <span data-ttu-id="a4d15-112">Klasické účty úložiště nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="a4d15-112">Classic storage accounts are not currently supported.</span></span> <span data-ttu-id="a4d15-113">Abyste pochopili, classic a modelech nasazení Resource Manager, najdete v části [modelech nasazení Azure](../azure-classic-rm.md).</span><span class="sxs-lookup"><span data-stu-id="a4d15-113">To understand the classic and Resource Manager deployment models, see [Azure deployment models](../azure-classic-rm.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a4d15-114">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="a4d15-114">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="a4d15-115">Tento dokument není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="a4d15-115">This document is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="a4d15-116">Implementace doporučení</span><span class="sxs-lookup"><span data-stu-id="a4d15-116">Implement the recommendation</span></span>
1. <span data-ttu-id="a4d15-117">V **doporučení** vyberte **povolit šifrování pro účet úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="a4d15-117">In the **Recommendations** blade, select **Enable encryption for Azure Storage Account**.</span></span>
   <span data-ttu-id="a4d15-118">![Povolení šifrování pro účet úložiště][1]</span><span class="sxs-lookup"><span data-stu-id="a4d15-118">![Enable encryption for storage account][1]</span></span>
2. <span data-ttu-id="a4d15-119">**Povolit šifrování úložiště** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="a4d15-119">The **Enable storage encryption** blade opens.</span></span> <span data-ttu-id="a4d15-120">Toto okno obsahuje seznam účtů úložiště Azure, kde je zakázaný šifrování úložiště.</span><span class="sxs-lookup"><span data-stu-id="a4d15-120">This blade lists the Azure storage accounts where storage encryption is disabled.</span></span> <span data-ttu-id="a4d15-121">V tomto příkladu budeme vyberte **storageacct1**.</span><span class="sxs-lookup"><span data-stu-id="a4d15-121">In this example, let's select **storageacct1**.</span></span>
   <span data-ttu-id="a4d15-122">![Povolit šifrování úložiště][2]</span><span class="sxs-lookup"><span data-stu-id="a4d15-122">![Enable storage encryption][2]</span></span>
3. <span data-ttu-id="a4d15-123">**Šifrování** okně **storageacct1** otevře.</span><span class="sxs-lookup"><span data-stu-id="a4d15-123">The **Encryption** blade for **storageacct1** opens.</span></span> <span data-ttu-id="a4d15-124">Vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="a4d15-124">Select **Enabled**.</span></span>
   <span data-ttu-id="a4d15-125">![Okno šifrování][3]</span><span class="sxs-lookup"><span data-stu-id="a4d15-125">![Encryption blade][3]</span></span>
4. <span data-ttu-id="a4d15-126">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a4d15-126">Select **Save**.</span></span>

<span data-ttu-id="a4d15-127">Nyní jste povolili šifrování úložiště pro **storageacct1**.</span><span class="sxs-lookup"><span data-stu-id="a4d15-127">You have now enabled storage encryption for **storageacct1**.</span></span>


## <a name="see-also"></a><span data-ttu-id="a4d15-128">Viz také</span><span class="sxs-lookup"><span data-stu-id="a4d15-128">See also</span></span>
<span data-ttu-id="a4d15-129">Tento dokument vám ukázal, jak provést doporučení Security Center "Povolit šifrování pro účet služby Azure Storage."</span><span class="sxs-lookup"><span data-stu-id="a4d15-129">This document showed you how to implement the Security Center recommendation "Enable encryption for Azure Storage Account."</span></span> <span data-ttu-id="a4d15-130">Další informace o šifrování služby úložiště Azure, naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="a4d15-130">To learn more about Azure Storage Service Encryption, see the following:</span></span>

* [<span data-ttu-id="a4d15-131">Šifrování služby úložiště Azure pro Data v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="a4d15-131">Azure Storage Service Encryption for Data at Rest</span></span>](../storage/common/storage-service-encryption.md)

<span data-ttu-id="a4d15-132">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="a4d15-132">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="a4d15-133">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="a4d15-133">[Setting security policies in Azure Security Center](security-center-policies.md) - Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="a4d15-134">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="a4d15-134">[Security health monitoring in Azure Security Center](security-center-monitoring.md) - Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="a4d15-135">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="a4d15-135">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) - Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="a4d15-136">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="a4d15-136">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) - Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="a4d15-137">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití této služby.</span><span class="sxs-lookup"><span data-stu-id="a4d15-137">[Azure Security Center FAQ](security-center-faq.md) - Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="a4d15-138">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="a4d15-138">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) - Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
