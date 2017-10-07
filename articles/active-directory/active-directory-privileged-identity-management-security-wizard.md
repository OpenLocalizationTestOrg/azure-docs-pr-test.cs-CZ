---
title: "Průvodce zabezpečení aaaThe Azure AD Privileged Identity Management"
description: "Hello prvním použití hello Azure Active Directory Privileged Identity Management rozšíření, zobrazí se Průvodce zabezpečení. Tento článek popisuje postup použití Průvodce hello hello."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: a53a3719-8cc7-4fc7-8164-aafca192871b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 0b3019134d3c7cfac33b3acfcf430b4d4f67b119
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-security-wizard-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="677c9-104">Pomocí Průvodce hello zabezpečení v Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="677c9-104">Using hello security wizard in Azure AD Privileged Identity Management</span></span> 
<span data-ttu-id="677c9-105">Pokud jste pro vaši organizaci hello první osoba toorun Azure Privileged Identity Management (PIM), zobrazí se průvodce.</span><span class="sxs-lookup"><span data-stu-id="677c9-105">If you're hello first person toorun Azure Privileged Identity Management (PIM) for your organization, you will be presented with a wizard.</span></span> <span data-ttu-id="677c9-106">Hello Průvodce vám pomůže pochopit hello bezpečnostními riziky privilegované identity a jak toouse PIM tooreduce těchto rizik.</span><span class="sxs-lookup"><span data-stu-id="677c9-106">hello wizard helps you understand hello security risks of privileged identities and how toouse PIM tooreduce those risks.</span></span> <span data-ttu-id="677c9-107">Nepotřebujete toomake všechna přiřazení role tooexisting změny v Průvodci hello, pokud dáváte přednost toodo později.</span><span class="sxs-lookup"><span data-stu-id="677c9-107">You don't need toomake any changes tooexisting role assignments in hello wizard, if you prefer toodo it later.</span></span>

## <a name="what-tooexpect"></a><span data-ttu-id="677c9-108">Jaké tooexpect</span><span class="sxs-lookup"><span data-stu-id="677c9-108">What tooexpect</span></span>
<span data-ttu-id="677c9-109">Předtím, než vaše organizace zahájí pomocí PIM, všechna přiřazení rolí jsou trvalé: uživatelé hello se vždycky nacházejí v těchto rolí i v případě, že v současné době nepotřebují svá oprávnění.</span><span class="sxs-lookup"><span data-stu-id="677c9-109">Before your organization starts using PIM, all role assignments are permanent: hello users are always in these roles even if they do not presently need their privileges.</span></span>  <span data-ttu-id="677c9-110">prvním krokem Hello hello průvodci se dozvíte, seznam rolí s vysokými oprávněními a počet uživatelů, kteří jsou aktuálně v těchto rolí.</span><span class="sxs-lookup"><span data-stu-id="677c9-110">hello first step of hello wizard shows you a list of high-privileged roles and how many users are currently in those roles.</span></span> <span data-ttu-id="677c9-111">Můžete zobrazit další podrobnosti v konkrétní roli toolearn tooa Další informace o uživatelé Pokud nejste obeznámeni nejméně jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="677c9-111">You can drill in tooa particular role toolearn more about users if one or more of them are unfamiliar.</span></span>

<span data-ttu-id="677c9-112">druhý krok průvodce hello Hello vám dává přiřazení rolí správce toochange příležitosti.</span><span class="sxs-lookup"><span data-stu-id="677c9-112">hello second step of hello wizard gives you an opportunity toochange administrator's role assignments.</span></span>  

> [!WARNING]
> <span data-ttu-id="677c9-113">Je důležité, že máte minimálně jeden globální správce a více než jeden správce privilegovaných rolí s účtem organizace (ne účet Microsoft).</span><span class="sxs-lookup"><span data-stu-id="677c9-113">It is important that you have at least one global administrator, and more than one privileged role administrator with an organizational account (not a Microsoft account).</span></span> <span data-ttu-id="677c9-114">Pokud existuje jenom jeden správce privilegovaných rolí, hello organizace nebude mít toomanage PIM Pokud tento účet je odstraněný.</span><span class="sxs-lookup"><span data-stu-id="677c9-114">If there is only one privileged role administrator, hello organization will not be able toomanage PIM if that account is deleted.</span></span>
> <span data-ttu-id="677c9-115">Navíc zachovat přiřazení rolí trvalé, pokud má uživatel účet Microsoft (účet, že na toosign ve tooMicrosoft služby, jako je Skype a Outlook.com).</span><span class="sxs-lookup"><span data-stu-id="677c9-115">Also, keep role assignments permanent if a user has a Microsoft account (An account they use toosign in tooMicrosoft services like Skype and Outlook.com).</span></span> <span data-ttu-id="677c9-116">Pokud máte v plánu toorequire MFA pro aktivaci pro tuto roli, tento uživatel bude uzamčena na.</span><span class="sxs-lookup"><span data-stu-id="677c9-116">If you plan toorequire MFA for activation for that role, that user will be locked out.</span></span>
> 
> 

<span data-ttu-id="677c9-117">Po provedení změn hello Průvodce zobrazí.</span><span class="sxs-lookup"><span data-stu-id="677c9-117">After you have made changes, hello wizard will no longer show up.</span></span> <span data-ttu-id="677c9-118">Hello příštím vy nebo jiný správce privilegovaných rolí používat PIM, zobrazí se hello PIM řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="677c9-118">hello next time you or another privileged role administrator use PIM, you will see hello PIM dashboard.</span></span>  

* <span data-ttu-id="677c9-119">Pokud by jako tooadd nebo odebrat uživatele z role nebo změnit přiřazení z trvalé tooeligible, přečtěte si informace v [jak tooadd nebo odebrat roli uživatele](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span><span class="sxs-lookup"><span data-stu-id="677c9-119">If you would like tooadd or remove users from roles or change assignments from permanent tooeligible, read more at [how tooadd or remove a user's role](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span></span>
* <span data-ttu-id="677c9-120">Pokud chcete toogive další uživatelé přistupovat k toomanage PIM, další informace v [přístupu toomanage v PIM toogive](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="677c9-120">If you would like toogive more users access toomanage PIM, read more at [how toogive access toomanage in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="677c9-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="677c9-121">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

