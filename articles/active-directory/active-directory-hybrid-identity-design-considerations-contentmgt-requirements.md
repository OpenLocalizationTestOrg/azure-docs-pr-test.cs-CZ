---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity - stanovení požadavků na správu obsahu | Microsoft Docs"
description: "Poskytuje přehled o tom, jak toodetermine hello správy obsahu požadavky vaší firmy. Většinou když má uživatel svůj vlastní zařízení mu může mít také více přihlašovací údaje, které bude možné střídavých podle toohello aplikace, kterou používá. Je důležité toodifferentiate jaké obsah se vytvořil pomocí osobní přihlašovacích údajů oproti hello ty, které jsou vytvořené pomocí podnikové přihlašovací údaje. Řešení identity by měl být schopný toointeract s cloud services tooprovide toohello integrované prostředí koncového uživatele při zajištění jeho ochrany osobních údajů a zvýšit hello ochranu před úniky dat."
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
ms.openlocfilehash: 607d366633c37b65ec5cf8ae5c64d73ca1cc96b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a><span data-ttu-id="13d94-106">Stanovení požadavků na správu obsahu pro vaše řešení hybridní identity</span><span class="sxs-lookup"><span data-stu-id="13d94-106">Determine content management requirements for your hybrid identity solution</span></span>
<span data-ttu-id="13d94-107">Principy hello správy obsahu požadavků pro vaši firmu může směrovat ovlivnit vaše rozhodnutí, na které toouse hybridní identity řešení.</span><span class="sxs-lookup"><span data-stu-id="13d94-107">Understanding hello content management requirements for your business may direct affect your decision on which hybrid identity solution toouse.</span></span> <span data-ttu-id="13d94-108">S text hello, jak narůstá počet více zařízení a funkce hello uživatelé toobring svá vlastní zařízení ([BYOD](http://aka.ms/byodcg)), hello společnosti musí chránit svá vlastní data, ale je také musí zachovat ochranu osobních údajů uživatelů.</span><span class="sxs-lookup"><span data-stu-id="13d94-108">With hello proliferation of multiple devices and hello capability of users toobring their own devices ([BYOD](http://aka.ms/byodcg)), hello company must protect its own data but it also must keep user’s privacy intact.</span></span> <span data-ttu-id="13d94-109">Většinou když má uživatel svůj vlastní zařízení mu může mít také více přihlašovací údaje, které bude možné střídavých podle toohello aplikace, kterou používá.</span><span class="sxs-lookup"><span data-stu-id="13d94-109">Usually when a user has his own device he might have also multiple credentials that will be alternating according toohello application that he uses.</span></span> <span data-ttu-id="13d94-110">Je důležité toodifferentiate jaké obsah se vytvořil pomocí osobní přihlašovacích údajů oproti hello ty, které jsou vytvořené pomocí podnikové přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="13d94-110">It is important toodifferentiate what content was created using personal credentials versus hello ones created using corporate credentials.</span></span> <span data-ttu-id="13d94-111">Řešení identity by měl být schopný toointeract s cloud services tooprovide toohello integrované prostředí koncového uživatele při zajištění jeho ochrany osobních údajů a zvýšit hello ochranu před úniky dat.</span><span class="sxs-lookup"><span data-stu-id="13d94-111">Your identity solution should be able toointeract with cloud services tooprovide a seamless experience toohello end user while ensure his privacy and increase hello protection against data leakage.</span></span> 

<span data-ttu-id="13d94-112">Různé technické ovládací prvky správy obsahu tooprovide pořadí bude řešení identity využít, jak je znázorněno na následujícím obrázku hello:</span><span class="sxs-lookup"><span data-stu-id="13d94-112">Your identity solution will be leveraged by different technical controls in order tooprovide content management as shown in hello figure below:</span></span>

![](./media/hybrid-id-design-considerations/securitycontrols.png)

<span data-ttu-id="13d94-113">**Ovládací prvky zabezpečení, které bude možné využívat váš systém správy identit**</span><span class="sxs-lookup"><span data-stu-id="13d94-113">**Security controls that will be leveraging your identity management system**</span></span>

<span data-ttu-id="13d94-114">Obecně platí požadavky na správu obsahu bude využívat váš systém správy identit v hello následující oblasti:</span><span class="sxs-lookup"><span data-stu-id="13d94-114">In general, content management requirements will leverage your identity management system in hello following areas:</span></span>

* <span data-ttu-id="13d94-115">Ochrana osobních údajů: identifikace hello uživatele, který vlastní prostředek a použití hello příslušných ovládacích prvků toomaintain integrity.</span><span class="sxs-lookup"><span data-stu-id="13d94-115">Privacy: identifying hello user that owns a resource and applying hello appropriate controls toomaintain integrity.</span></span>
* <span data-ttu-id="13d94-116">Klasifikace dat: Identifikujte hello uživatele nebo skupinu a úroveň přístupu tooan objektu podle tooits klasifikace.</span><span class="sxs-lookup"><span data-stu-id="13d94-116">Data Classification: identify hello user or group and level of access tooan object according tooits classification.</span></span> 
* <span data-ttu-id="13d94-117">Ochrana úniku dat: ovládací prvky zabezpečení odpovědnost za ochranu tooavoid úniku dat, bude nutné toointeract s hello identity systému toovalidate hello identitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="13d94-117">Data Leakage Protection: security controls responsible for protecting data tooavoid leakage will need toointeract with hello identity system toovalidate hello user’s identity.</span></span> <span data-ttu-id="13d94-118">To je důležité pro auditování záznam účel.</span><span class="sxs-lookup"><span data-stu-id="13d94-118">This is also important for auditing trail purpose.</span></span>

> [!NOTE]
> <span data-ttu-id="13d94-119">Čtení [klasifikace dat pro cloud připravenosti](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) Další informace o osvědčených postupech a pokyny pro klasifikaci dat.</span><span class="sxs-lookup"><span data-stu-id="13d94-119">Read [data classification for cloud readiness](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) for more information about best practices and guidelines for data classification.</span></span>
> 
> 

<span data-ttu-id="13d94-120">Při plánování řešení hybridní identity Ujistěte se, že hello následující otázky jsou zodpovězeny podle požadavků organizace tooyour:</span><span class="sxs-lookup"><span data-stu-id="13d94-120">When planning your hybrid identity solution ensure that hello following questions are answered according tooyour organization’s requirements:</span></span>

* <span data-ttu-id="13d94-121">Má vaše společnost nějaké kontrolních mechanismů pro zabezpečení v místě tooenforce data o ochraně osobních údajů?</span><span class="sxs-lookup"><span data-stu-id="13d94-121">Does your company have security controls in place tooenforce data privacy?</span></span>
  * <span data-ttu-id="13d94-122">Pokud ano, kontrolní mechanismy zabezpečení bude možné toointegrate s hello hybridní řešení identit se probíhající tooadopt?</span><span class="sxs-lookup"><span data-stu-id="13d94-122">If yes, will those security controls be able toointegrate with hello hybrid identity solution that you are going tooadopt?</span></span>
* <span data-ttu-id="13d94-123">Používá vaše společnost klasifikace dat?</span><span class="sxs-lookup"><span data-stu-id="13d94-123">Does your company use data classification?</span></span>
  * <span data-ttu-id="13d94-124">Pokud ano, je možné toointegrate aktuální řešení hello hello hybridní řešení identit se probíhající tooadopt?</span><span class="sxs-lookup"><span data-stu-id="13d94-124">If yes, is hello current solution able toointegrate with hello hybrid identity solution that you are going tooadopt?</span></span>
* <span data-ttu-id="13d94-125">Vaše společnost aktuálně nemá řešení pro úniku dat?</span><span class="sxs-lookup"><span data-stu-id="13d94-125">Does your company currently have any solution for data leakage?</span></span> 
  * <span data-ttu-id="13d94-126">Pokud ano, je možné toointegrate aktuální řešení hello hello hybridní řešení identit se probíhající tooadopt?</span><span class="sxs-lookup"><span data-stu-id="13d94-126">If yes, is hello current solution able toointegrate with hello hybrid identity solution that you are going tooadopt?</span></span>
* <span data-ttu-id="13d94-127">Potřebuje vaše společnost tooresources tooaudit přístup?</span><span class="sxs-lookup"><span data-stu-id="13d94-127">Does your company need tooaudit access tooresources?</span></span>
  * <span data-ttu-id="13d94-128">Pokud ano, jaké typy prostředků?</span><span class="sxs-lookup"><span data-stu-id="13d94-128">If yes, what type of resources?</span></span>
  * <span data-ttu-id="13d94-129">Pokud ano, jakou úroveň informací je potřeba?</span><span class="sxs-lookup"><span data-stu-id="13d94-129">If yes, what level of information is necessary?</span></span>
  * <span data-ttu-id="13d94-130">Pokud ano, kde protokol auditování hello se musí nacházet?</span><span class="sxs-lookup"><span data-stu-id="13d94-130">If yes, where hello audit log must reside?</span></span> <span data-ttu-id="13d94-131">Místní nebo v cloudu hello?</span><span class="sxs-lookup"><span data-stu-id="13d94-131">On-premises or in hello cloud?</span></span>
* <span data-ttu-id="13d94-132">Potřebuje vaše společnost tooencrypt e-mailů, které obsahují citlivá data (čísla sociálního zabezpečení, čísla platebních karet a podobně)?</span><span class="sxs-lookup"><span data-stu-id="13d94-132">Does your company need tooencrypt any emails that contain sensitive data (SSNs, credit card numbers, etc)?</span></span>
* <span data-ttu-id="13d94-133">Potřebuje vaše společnost tooencrypt všechny dokumenty nebo obsah sdílet s externími obchodními partnery?</span><span class="sxs-lookup"><span data-stu-id="13d94-133">Does your company need tooencrypt all documents/contents shared with external business partners?</span></span>
* <span data-ttu-id="13d94-134">Potřebuje vaše společnost tooenforce podnikové zásady na určité typy e-mailů (dělat žádné Odpovědět všem, nepředávat)?</span><span class="sxs-lookup"><span data-stu-id="13d94-134">Does your company need tooenforce corporate policies on certain kinds of emails (do no reply all, do not forward)?</span></span>

> [!NOTE]
> <span data-ttu-id="13d94-135">Ujistěte se, že tootake poznámky u každé odpovědi a pochopit hello důvody, které hello odpovědí.</span><span class="sxs-lookup"><span data-stu-id="13d94-135">Make sure tootake notes of each answer and understand hello rationale behind hello answer.</span></span> <span data-ttu-id="13d94-136">[Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přenášeny po hello dostupných možností a výhod i nevýhod jednotlivých možností.</span><span class="sxs-lookup"><span data-stu-id="13d94-136">[Define Data Protection Strategy](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) will go over hello options available and advantages/disadvantages of each option.</span></span>  <span data-ttu-id="13d94-137">Po zodpovězení těchto otázek, které vyberete která možnost nejlépe vyhovuje vaší firmě potřebuje.</span><span class="sxs-lookup"><span data-stu-id="13d94-137">By having answered those questions you will select which option best suits your business needs.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="13d94-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13d94-138">Next steps</span></span>
[<span data-ttu-id="13d94-139">Určete požadavky řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="13d94-139">Determine access control requirements</span></span>](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a><span data-ttu-id="13d94-140">Viz také</span><span class="sxs-lookup"><span data-stu-id="13d94-140">See Also</span></span>
[<span data-ttu-id="13d94-141">Přehled aspektů návrhu</span><span class="sxs-lookup"><span data-stu-id="13d94-141">Design considerations overview</span></span>](active-directory-hybrid-identity-design-considerations-overview.md)

