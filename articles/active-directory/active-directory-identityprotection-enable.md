---
title: "Povolení ochrany identit Azure Active Directory | Microsoft Docs"
description: "Informace o povolení Azure Active Directory Identity Protection."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f7a7ffaf-76bf-4cc7-96a1-86c944275c82
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: e0683e837086ca08f4b4b3f49695bdd2b4063ea4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enabling-azure-active-directory-identity-protection"></a><span data-ttu-id="27140-104">Povolení ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="27140-104">Enabling Azure Active Directory Identity Protection</span></span>
<span data-ttu-id="27140-105">Azure Active Directory Identity Protection je nová funkce, která poskytuje ucelený přehled o podezřelých aktivit přihlášení a potenciální ohrožení zabezpečení a s oznámení, nápravy doporučení a zásad založených na riziko pomáhá chránit vaší firmy.</span><span class="sxs-lookup"><span data-stu-id="27140-105">Azure Active Directory Identity Protection is a new capability that provides a consolidated view into suspicious sign-in activities and potential vulnerabilities and with notifications, remediation recommendations and risk-based policies helps you protect your business.</span></span> 

<span data-ttu-id="27140-106">Služba zjistí podezřelé aktivity pro koncové uživatele a identity oprávněními (správce) podle signály jako útoky hrubou silou, úniku přihlašovacích údajů, přihlášení z neznámých míst, nakažených zařízení pro ochranu proti tyto aktivity v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="27140-106">The service detects suspicious activities for end user and privileged (admin) identities based on signals like brute force attacks, leaked credentials, sign ins from unfamiliar locations, infected devices, to protect against these activities in real-time.</span></span> <span data-ttu-id="27140-107">Je důležité podle těchto podezřelých aktivit, závažnost riziko uživatele je počítaný a na základě riziko zásady lze konfigurovat a automaticky chránit identity vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="27140-107">More importantly, based on these suspicious activities, a user risk severity is computed and risk-based policies can be configured and automatically protect the identities of your organization.</span></span> <span data-ttu-id="27140-108">Další podrobnosti najdete v tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="27140-108">For more details, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>

<span data-ttu-id="27140-109">Tato témata ukazuje, jak povolit Azure Active Directory Identity Protection.</span><span class="sxs-lookup"><span data-stu-id="27140-109">This topics shows how to enable Azure Active Directory Identity Protection.</span></span>

## <a name="steps-to-enable-azure-active-directory-identity-protection"></a><span data-ttu-id="27140-110">Postup povolení Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="27140-110">Steps to enable Azure Active Directory Identity Protection</span></span>
1. <span data-ttu-id="27140-111">[Přihlášení](https://ms.portal.azure.com/) na portálu Azure jako globální správce.</span><span class="sxs-lookup"><span data-stu-id="27140-111">[Sign-on](https://ms.portal.azure.com/) to your Azure portal as global administrator.</span></span> 
2. <span data-ttu-id="27140-112">Na portálu Azure klikněte na tlačítko **Marketplace**.</span><span class="sxs-lookup"><span data-stu-id="27140-112">In the Azure portal, click **Marketplace**.</span></span>
   
    <span data-ttu-id="27140-113">![Vytvoření](./media/active-directory-identityprotection-enable/01.png "vytvořit")</span><span class="sxs-lookup"><span data-stu-id="27140-113">![Create](./media/active-directory-identityprotection-enable/01.png "Create")</span></span>
3. <span data-ttu-id="27140-114">V seznamu aplikací klepněte na **zabezpečení a identita**.</span><span class="sxs-lookup"><span data-stu-id="27140-114">In the applications list, click **Security + Identity**.</span></span>
   
    <span data-ttu-id="27140-115">![Vytvoření](./media/active-directory-identityprotection-enable/02.png "vytvořit")</span><span class="sxs-lookup"><span data-stu-id="27140-115">![Create](./media/active-directory-identityprotection-enable/02.png "Create")</span></span>
4. <span data-ttu-id="27140-116">Klikněte na tlačítko **Azure AD Identity Protection**.</span><span class="sxs-lookup"><span data-stu-id="27140-116">Click **Azure AD Identity Protection**.</span></span>
   
    <span data-ttu-id="27140-117">![Vytvoření](./media/active-directory-identityprotection-enable/03.png "vytvořit")</span><span class="sxs-lookup"><span data-stu-id="27140-117">![Create](./media/active-directory-identityprotection-enable/03.png "Create")</span></span>
5. <span data-ttu-id="27140-118">Na **Azure AD Identity Protection** okně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="27140-118">On the **Azure AD Identity Protection** blade, click **Create**.</span></span>
   
    <span data-ttu-id="27140-119">![Vytvoření](./media/active-directory-identityprotection-enable/04.png "vytvořit")</span><span class="sxs-lookup"><span data-stu-id="27140-119">![Create](./media/active-directory-identityprotection-enable/04.png "Create")</span></span>

## <a name="next-steps"></a><span data-ttu-id="27140-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27140-120">Next Steps</span></span>
* [<span data-ttu-id="27140-121">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="27140-121">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

