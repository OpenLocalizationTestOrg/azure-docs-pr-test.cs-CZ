---
title: "aaaProvide zabezpečení kontaktní údaje v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooprovide zabezpečení kontaktní údaje v Azure Security Center."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 6c378c9c84c6e4fce70b2541e4cc121018700269
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provide-security-contact-details-in-azure-security-center"></a><span data-ttu-id="1e594-103">Zadejte kontaktní údaje zabezpečení v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="1e594-103">Provide security contact details in Azure Security Center</span></span>
<span data-ttu-id="1e594-104">Azure Security Center doporučí, pokud jste to ještě neudělali zadejte kontaktní údaje zabezpečení pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="1e594-104">Azure Security Center will recommend that you provide security contact details for your Azure subscription if you haven’t already.</span></span> <span data-ttu-id="1e594-105">Tyto informace použije Microsoft toocontact vám, zda text hello Microsoft Security Response Center (MSRC) zjistí, že zákaznických údajů měl strana nezákonné nebo neoprávněný přístup k.</span><span class="sxs-lookup"><span data-stu-id="1e594-105">This information will be used by Microsoft toocontact you if hello Microsoft Security Response Center (MSRC) discovers that your customer data has been accessed by an unlawful or unauthorized party.</span></span> <span data-ttu-id="1e594-106">Střediska MSRC provádí, vyberte možnost zabezpečení monitorování hello síť Azure a infrastruktury a přijímá stížností intelligence a zneužití ohrožení od jiných výrobců.</span><span class="sxs-lookup"><span data-stu-id="1e594-106">MSRC performs select security monitoring of hello Azure network and infrastructure and receives threat intelligence and abuse complaints from third parties.</span></span>

<span data-ttu-id="1e594-107">E-mailové oznámení se odesílají na hello první denní výskyt výstrahy a jenom pro výstrahy s vysokou závažností.</span><span class="sxs-lookup"><span data-stu-id="1e594-107">An email notification is sent on hello first daily occurrence of an alert and only for high severity alerts.</span></span> <span data-ttu-id="1e594-108">Předvolby e-mailu lze konfigurovat pouze pro zásady předplatného.</span><span class="sxs-lookup"><span data-stu-id="1e594-108">Email preferences can only be configured for subscription policies.</span></span> <span data-ttu-id="1e594-109">Skupiny prostředků v rámci předplatného zdědí tato nastavení.</span><span class="sxs-lookup"><span data-stu-id="1e594-109">Resource groups within a subscription will inherit these settings.</span></span>

> [!NOTE]
> <span data-ttu-id="1e594-110">Toto téma představuje hello služby pomocí příklad nasazení.</span><span class="sxs-lookup"><span data-stu-id="1e594-110">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="1e594-111">Není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="1e594-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="1e594-112">Implementace doporučení hello</span><span class="sxs-lookup"><span data-stu-id="1e594-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="1e594-113">V hello **doporučení** vyberte **zadejte kontaktní údaje zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="1e594-113">In hello **Recommendations** blade, select **Provide security contact details**.</span></span>
   <span data-ttu-id="1e594-114">![Zadejte kontakt zabezpečení][1]</span><span class="sxs-lookup"><span data-stu-id="1e594-114">![Provide security contact][1]</span></span>
2. <span data-ttu-id="1e594-115">Otevře se okno hello **zadejte kontaktní údaje zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="1e594-115">This opens hello blade **Provide security contact details**.</span></span> <span data-ttu-id="1e594-116">Vyberte hello předplatného Azure tooprovide kontaktní informace na.</span><span class="sxs-lookup"><span data-stu-id="1e594-116">Select hello Azure subscription tooprovide contact information on.</span></span>
   <span data-ttu-id="1e594-117">![Poskytnutí podrobností kontaktů zabezpečení][2]</span><span class="sxs-lookup"><span data-stu-id="1e594-117">![Provide security contact details][2]</span></span>
3. <span data-ttu-id="1e594-118">Druhý **zadejte kontaktní údaje zabezpečení** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="1e594-118">A second **Provide security contact details** blade opens.</span></span>

   * <span data-ttu-id="1e594-119">Zadejte hello zabezpečení kontaktní e- mailových adres oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="1e594-119">Enter hello security contact email address or addresses separated by commas.</span></span> <span data-ttu-id="1e594-120">Není k dispozici několik toohello limit e-mailové adresy, které můžete zadat.</span><span class="sxs-lookup"><span data-stu-id="1e594-120">There is not a limit toohello number of email addresses that you can enter.</span></span>
   * <span data-ttu-id="1e594-121">Zadejte jeden zabezpečení kontaktní telefonní číslo v mezinárodním.</span><span class="sxs-lookup"><span data-stu-id="1e594-121">Enter one security contact international phone number.</span></span>
   * <span data-ttu-id="1e594-122">e-mailů tooreceive o vysokou závažností výstrahy, zapněte možnost hello **zaslat mi e-maily o výstrahách**.</span><span class="sxs-lookup"><span data-stu-id="1e594-122">tooreceive emails about high severity alerts, turn on hello option **Send me emails about alerts**.</span></span>
   * <span data-ttu-id="1e594-123">V budoucích hello budete mít hello možnost toosend e-mailové oznámení toosubscription vlastníky.</span><span class="sxs-lookup"><span data-stu-id="1e594-123">In hello future, you will have hello option toosend email notifications toosubscription owners.</span></span> <span data-ttu-id="1e594-124">Tato možnost není aktuálně k dispozici.</span><span class="sxs-lookup"><span data-stu-id="1e594-124">This option is currently grayed out.</span></span>
   * <span data-ttu-id="1e594-125">Vyberte **OK** tooapply hello zabezpečení obraťte se na informace tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="1e594-125">Select **OK** tooapply hello security contact information tooyour subscription.</span></span>

## <a name="see-also"></a><span data-ttu-id="1e594-126">Viz také</span><span class="sxs-lookup"><span data-stu-id="1e594-126">See also</span></span>
<span data-ttu-id="1e594-127">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="1e594-127">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="1e594-128">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="1e594-128">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="1e594-129">[Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="1e594-129">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="1e594-130">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="1e594-130">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="1e594-131">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1e594-131">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="1e594-132">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="1e594-132">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="1e594-133">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="1e594-133">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="1e594-134">[Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.</span><span class="sxs-lookup"><span data-stu-id="1e594-134">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
