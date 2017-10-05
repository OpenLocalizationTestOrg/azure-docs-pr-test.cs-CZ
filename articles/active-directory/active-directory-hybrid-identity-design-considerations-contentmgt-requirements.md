---
title: "Azure Active Directory hybridní identity aspekty návrhu - stanovení požadavků na správu obsahu | Microsoft Docs"
description: "Poskytuje přehled o tom, jak určit požadavky na správu obsahu vaší firmy. Většinou když má uživatel svůj vlastní zařízení mu může mít také více přihlašovací údaje, které bude střídavých závislosti na aplikaci, kterou používá. Je důležité k rozlišení postupu, který obsah byl vytvořen pomocí osobní přihlašovacích údajů oproti těm, které jsou vytvořené pomocí podnikové přihlašovací údaje. Řešení identity měli být schopní pracovat s cloud services a poskytuje bezproblémové prostředí pro koncového uživatele při zajištění jeho ochrany osobních údajů a zvýšit ochranu před úniky dat."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: dd1ef776-db4d-4ab8-9761-2adaa5a4f004
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 840de1e1fcba74285788d51d8f544375f0affa77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="4474b-106">Stanovení požadavků na správu obsahu pro vaše řešení hybridní identity</span><span class="sxs-lookup"><span data-stu-id="4474b-106">Determine content management requirements for your hybrid identity solution</span></span>
<span data-ttu-id="4474b-107">Seznámení s požadavky na správu obsahu pro vaši organizaci může přímo ovlivnit vaše rozhodnutí, na které hybridní řešení identit používat.</span><span class="sxs-lookup"><span data-stu-id="4474b-107">Understanding the content management requirements for your business may direct affect your decision on which hybrid identity solution to use.</span></span> <span data-ttu-id="4474b-108">S tím, jak narůstá více zařízení a schopnost uživatelům přinést si vlastní zařízení ([BYOD](http://aka.ms/byodcg)), musí společnost chránit svá vlastní data, ale ho také musí zachovat ochranu osobních údajů uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4474b-108">With the proliferation of multiple devices and the capability of users to bring their own devices ([BYOD](http://aka.ms/byodcg)), the company must protect its own data but it also must keep user’s privacy intact.</span></span> <span data-ttu-id="4474b-109">Většinou když má uživatel svůj vlastní zařízení mu může mít také více přihlašovací údaje, které bude střídavých závislosti na aplikaci, kterou používá.</span><span class="sxs-lookup"><span data-stu-id="4474b-109">Usually when a user has his own device he might have also multiple credentials that will be alternating according to the application that he uses.</span></span> <span data-ttu-id="4474b-110">Je důležité k rozlišení postupu, který obsah byl vytvořen pomocí osobní přihlašovacích údajů oproti těm, které jsou vytvořené pomocí podnikové přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="4474b-110">It is important to differentiate what content was created using personal credentials versus the ones created using corporate credentials.</span></span> <span data-ttu-id="4474b-111">Řešení identity měli být schopní pracovat s cloud services a poskytuje bezproblémové prostředí pro koncového uživatele při zajištění jeho ochrany osobních údajů a zvýšit ochranu před úniky dat.</span><span class="sxs-lookup"><span data-stu-id="4474b-111">Your identity solution should be able to interact with cloud services to provide a seamless experience to the end user while ensure his privacy and increase the protection against data leakage.</span></span> 

<span data-ttu-id="4474b-112">Chcete-li poskytovat správy obsahu, jak je znázorněno na obrázku níže bude řešení identity využít různé technické ovládací prvky:</span><span class="sxs-lookup"><span data-stu-id="4474b-112">Your identity solution will be leveraged by different technical controls in order to provide content management as shown in the figure below:</span></span>

![](./media/hybrid-id-design-considerations/securitycontrols.png)

<span data-ttu-id="4474b-113">**Ovládací prvky zabezpečení, které bude možné využívat váš systém správy identit**</span><span class="sxs-lookup"><span data-stu-id="4474b-113">**Security controls that will be leveraging your identity management system**</span></span>

<span data-ttu-id="4474b-114">Obecně platí požadavky na správu obsahu bude využívat váš systém správy identit v těchto oblastech:</span><span class="sxs-lookup"><span data-stu-id="4474b-114">In general, content management requirements will leverage your identity management system in the following areas:</span></span>

* <span data-ttu-id="4474b-115">Ochrana osobních údajů: identifikace uživatele, který vlastní prostředek a použití příslušných ovládacích prvků k udržení integrity.</span><span class="sxs-lookup"><span data-stu-id="4474b-115">Privacy: identifying the user that owns a resource and applying the appropriate controls to maintain integrity.</span></span>
* <span data-ttu-id="4474b-116">Klasifikace dat: Identifikujte uživatele nebo skupinu a úroveň přístupu k objektu podle jejich klasifikace.</span><span class="sxs-lookup"><span data-stu-id="4474b-116">Data Classification: identify the user or group and level of access to an object according to its classification.</span></span> 
* <span data-ttu-id="4474b-117">Ochrana úniku dat: ovládací prvky zabezpečení odpovědnost za ochranu dat, aby se zabránilo úniku potřebovat k interakci se systém identit k ověření identity uživatele.</span><span class="sxs-lookup"><span data-stu-id="4474b-117">Data Leakage Protection: security controls responsible for protecting data to avoid leakage will need to interact with the identity system to validate the user’s identity.</span></span> <span data-ttu-id="4474b-118">To je důležité pro auditování záznam účel.</span><span class="sxs-lookup"><span data-stu-id="4474b-118">This is also important for auditing trail purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="4474b-119">Čtení [klasifikace dat pro cloud připravenosti](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) Další informace o osvědčených postupech a pokyny pro klasifikaci dat.</span><span class="sxs-lookup"><span data-stu-id="4474b-119">Read [data classification for cloud readiness](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) for more information about best practices and guidelines for data classification.</span></span>
> 
> 

<span data-ttu-id="4474b-120">Při plánování řešení hybridní identity se ujistěte, že následující otázky jsou zodpovězeny v souladu s požadavky vaší organizace:</span><span class="sxs-lookup"><span data-stu-id="4474b-120">When planning your hybrid identity solution ensure that the following questions are answered according to your organization’s requirements:</span></span>

* <span data-ttu-id="4474b-121">Má vaše společnost zavedené ovládací prvky zabezpečení k vynucení ochrany osobních údajů?</span><span class="sxs-lookup"><span data-stu-id="4474b-121">Does your company have security controls in place to enforce data privacy?</span></span>
  * <span data-ttu-id="4474b-122">Pokud ano, bude kontrolní mechanismy zabezpečení možné integrovat řešení hybridní identity, který chcete použít?</span><span class="sxs-lookup"><span data-stu-id="4474b-122">If yes, will those security controls be able to integrate with the hybrid identity solution that you are going to adopt?</span></span>
* <span data-ttu-id="4474b-123">Používá vaše společnost klasifikace dat?</span><span class="sxs-lookup"><span data-stu-id="4474b-123">Does your company use data classification?</span></span>
  * <span data-ttu-id="4474b-124">Pokud ano, je aktuální řešení schopné integrovat řešení hybridní identity, který chcete použít?</span><span class="sxs-lookup"><span data-stu-id="4474b-124">If yes, is the current solution able to integrate with the hybrid identity solution that you are going to adopt?</span></span>
* <span data-ttu-id="4474b-125">Vaše společnost aktuálně nemá řešení pro úniku dat?</span><span class="sxs-lookup"><span data-stu-id="4474b-125">Does your company currently have any solution for data leakage?</span></span> 
  * <span data-ttu-id="4474b-126">Pokud ano, je aktuální řešení schopné integrovat řešení hybridní identity, který chcete použít?</span><span class="sxs-lookup"><span data-stu-id="4474b-126">If yes, is the current solution able to integrate with the hybrid identity solution that you are going to adopt?</span></span>
* <span data-ttu-id="4474b-127">Potřebuje vaše společnost auditovat přístup k prostředkům?</span><span class="sxs-lookup"><span data-stu-id="4474b-127">Does your company need to audit access to resources?</span></span>
  * <span data-ttu-id="4474b-128">Pokud ano, jaké typy prostředků?</span><span class="sxs-lookup"><span data-stu-id="4474b-128">If yes, what type of resources?</span></span>
  * <span data-ttu-id="4474b-129">Pokud ano, jakou úroveň informací je potřeba?</span><span class="sxs-lookup"><span data-stu-id="4474b-129">If yes, what level of information is necessary?</span></span>
  * <span data-ttu-id="4474b-130">Pokud ano, kde protokol auditu se musí nacházet?</span><span class="sxs-lookup"><span data-stu-id="4474b-130">If yes, where the audit log must reside?</span></span> <span data-ttu-id="4474b-131">Místní nebo v cloudu?</span><span class="sxs-lookup"><span data-stu-id="4474b-131">On-premises or in the cloud?</span></span>
* <span data-ttu-id="4474b-132">Potřebuje vaše společnost k šifrování e-mailů, které obsahují citlivá data (čísla sociálního zabezpečení, čísla platebních karet a podobně)?</span><span class="sxs-lookup"><span data-stu-id="4474b-132">Does your company need to encrypt any emails that contain sensitive data (SSNs, credit card numbers, etc)?</span></span>
* <span data-ttu-id="4474b-133">Potřebuje vaše společnost k šifrování všech dokumentů nebo obsah sdílet s externími obchodními partnery?</span><span class="sxs-lookup"><span data-stu-id="4474b-133">Does your company need to encrypt all documents/contents shared with external business partners?</span></span>
* <span data-ttu-id="4474b-134">Vaše společnost potřebuje vynucovat podnikové zásady na určité typy e-mailů (dělat žádné Odpovědět všem, nepředávat)?</span><span class="sxs-lookup"><span data-stu-id="4474b-134">Does your company need to enforce corporate policies on certain kinds of emails (do no reply all, do not forward)?</span></span>

> [!NOTE]
> <span data-ttu-id="4474b-135">Zajistěte, aby poznamenejte každou odpověď a pochopit důvody odpověď.</span><span class="sxs-lookup"><span data-stu-id="4474b-135">Make sure to take notes of each answer and understand the rationale behind the answer.</span></span> <span data-ttu-id="4474b-136">[Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přenášeny po dostupných možností a výhod i nevýhod jednotlivých možností.</span><span class="sxs-lookup"><span data-stu-id="4474b-136">[Define Data Protection Strategy](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) will go over the options available and advantages/disadvantages of each option.</span></span>  <span data-ttu-id="4474b-137">Po zodpovězení těchto otázek, které vyberete která možnost nejlépe vyhovuje vaší firmě potřebuje.</span><span class="sxs-lookup"><span data-stu-id="4474b-137">By having answered those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="4474b-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4474b-138">Next steps</span></span>
[<span data-ttu-id="4474b-139">Určete požadavky řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="4474b-139">Determine access control requirements</span></span>](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a><span data-ttu-id="4474b-140">Viz také</span><span class="sxs-lookup"><span data-stu-id="4474b-140">See Also</span></span>
[<span data-ttu-id="4474b-141">Přehled aspektů návrhu</span><span class="sxs-lookup"><span data-stu-id="4474b-141">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

