---
title: "Azure AD vrstvené heslo zabezpečení | Microsoft Docs"
description: "Vysvětluje, jak Azure AD vynucuje silná hesla a chrání hesla uživatelů z zločinci,"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: joflore
ms.openlocfilehash: 32464307ccb082b25538eaa522c1cdedef1ca555
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="a-multi-tiered-approach-to-azure-ad-password-security"></a><span data-ttu-id="e6b5f-103">Víceúrovňová způsob zabezpečení hesel Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6b5f-103">A multi-tiered approach to Azure AD password security</span></span>

<span data-ttu-id="e6b5f-104">Tento článek popisuje některé z osvědčených postupů můžete provést jako uživatel, nebo jako správce k ochraně služby Azure Active Directory (Azure AD) nebo Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-104">This article discusses some best practices you can follow as a user or as an administrator to protect your Azure Active Directory (Azure AD) or Microsoft Account.</span></span>

 > [!NOTE]
 > <span data-ttu-id="e6b5f-105">Správci služby Azure AD můžete resetovat hesla uživatelů pomocí pokynů v článku [resetování hesla pro uživatele v Azure Active Directory](active-directory-users-reset-password-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e6b5f-105">Azure AD administrators can reset user passwords using the guidance in the article [Reset the password for a user in Azure Active Directory](active-directory-users-reset-password-azure-portal.md).</span></span>
 >
 > <span data-ttu-id="e6b5f-106">Uživatelům můžete resetovat vlastní hesla pomocí pokynů v článku [nápovědy zapomenuté heslo Azure AD](active-directory-passwords-update-your-own-password.md).</span><span class="sxs-lookup"><span data-stu-id="e6b5f-106">Users can reset their own password using the guidance in the article [Help I forgot my Azure AD password](active-directory-passwords-update-your-own-password.md).</span></span>
 >

## <a name="password-requirements"></a><span data-ttu-id="e6b5f-107">Požadavky na heslo</span><span class="sxs-lookup"><span data-stu-id="e6b5f-107">Password requirements</span></span>

<span data-ttu-id="e6b5f-108">Azure AD zahrnuje následující běžné přístupy k zabezpečení hesel:</span><span class="sxs-lookup"><span data-stu-id="e6b5f-108">Azure AD incorporates the following common approaches to securing passwords:</span></span>

* <span data-ttu-id="e6b5f-109">Požadavky na délku hesla</span><span class="sxs-lookup"><span data-stu-id="e6b5f-109">Password length requirements</span></span>
* <span data-ttu-id="e6b5f-110">Požadavky na složitost hesla</span><span class="sxs-lookup"><span data-stu-id="e6b5f-110">Password complexity requirements</span></span>
* <span data-ttu-id="e6b5f-111">Pravidelné a opakované vypršení platnosti hesla</span><span class="sxs-lookup"><span data-stu-id="e6b5f-111">Regular and periodic password expiration</span></span>

<span data-ttu-id="e6b5f-112">Informace o vytvoření nového ve službě Azure Active Directory, naleznete v tématu [Azure AD samoobslužného resetování hesla pro IT profesionály](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="e6b5f-112">For information about password reset in Azure Active Directory, see the topic [Azure AD self-service password reset for the IT professional](active-directory-passwords.md).</span></span>

## <a name="azure-ad-password-protections"></a><span data-ttu-id="e6b5f-113">Ochrana hesla služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6b5f-113">Azure AD password protections</span></span>

<span data-ttu-id="e6b5f-114">Azure AD a systémem účtů Microsoft používat odvětví ověřené přístupy k zajištění zabezpečení ochranu uživatele a správce hesel, včetně:</span><span class="sxs-lookup"><span data-stu-id="e6b5f-114">Azure AD and the Microsoft Account System use industry proven approaches to ensure secure protection of user and administrator passwords including:</span></span>

* <span data-ttu-id="e6b5f-115">Dynamicky zakázaná hesla</span><span class="sxs-lookup"><span data-stu-id="e6b5f-115">Dynamically banned passwords</span></span>
* <span data-ttu-id="e6b5f-116">Inteligentní uzamčení hesla</span><span class="sxs-lookup"><span data-stu-id="e6b5f-116">Smart Password Lockout</span></span>

<span data-ttu-id="e6b5f-117">Informace o správě hesel podle aktuální research, najdete v dokumentu White Paper [heslo pokyny](http://aka.ms/passwordguidance).</span><span class="sxs-lookup"><span data-stu-id="e6b5f-117">For information about password management based on current research, see the whitepaper [Password Guidance](http://aka.ms/passwordguidance).</span></span>

### <a name="dynamically-banned-passwords"></a><span data-ttu-id="e6b5f-118">Dynamicky zakázaná hesla</span><span class="sxs-lookup"><span data-stu-id="e6b5f-118">Dynamically banned passwords</span></span>

<span data-ttu-id="e6b5f-119">Azure AD a Accounts Microsoft ochrana heslem zabezpečit pomocí dynamicky zakazování běžně používané hesla.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-119">Azure AD and Microsoft Accounts safeguard password protection by dynamically banning commonly used passwords.</span></span> <span data-ttu-id="e6b5f-120">Tým Azure AD Identity Protection pravidelně analyzuje seznamy zakázaných hesel, aby bránil uživatelům ve zvolení běžně používaných hesel.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-120">The Azure ID Identity Protection team routinely analyzes banned password lists, preventing users from selecting commonly used passwords.</span></span> <span data-ttu-id="e6b5f-121">Tato služba je k dispozici pro zákazníky Azure AD a Microsoft Account Service.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-121">This service is available to Azure AD and the Microsoft Account Service customers.</span></span>

<span data-ttu-id="e6b5f-122">Při vytváření hesel, je důležité pro správce k podpoře uživatelům zvolit frází heslo, které zahrnují jedinečnou kombinaci písmena, čísla, znaky nebo slova.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-122">When creating passwords, it is important for administrators to encourage users to choose password phrases that include a unique combination of letters, numbers, characters, or words.</span></span> <span data-ttu-id="e6b5f-123">Tento přístup umožňuje, aby téměř možné být ohrožené ale usnadňuje uživatelům mějte na paměti, aby uživatelská hesla.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-123">This approach helps to make user passwords nearly impossible to be compromised but easier for users to remember.</span></span>

#### <a name="password-breaches"></a><span data-ttu-id="e6b5f-124">Narušení heslo</span><span class="sxs-lookup"><span data-stu-id="e6b5f-124">Password breaches</span></span>

<span data-ttu-id="e6b5f-125">Společnost Microsoft pracuje vždy zůstala krok před zločinci.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-125">Microsoft is always working to stay one step ahead of cyber-criminals.</span></span>

<span data-ttu-id="e6b5f-126">Tým Azure AD Identity Protection průběžně analyzuje hesla, která se běžně používají.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-126">The Azure AD Identity Protection team continually analyzes passwords that are commonly used.</span></span> <span data-ttu-id="e6b5f-127">Kybernetičtí zločinci také používají podobné strategie k úpravě svých útoků, jako je například vytváření [duhové tabulky](https://en.wikipedia.org/wiki/Rainbow_table) pro prolamování hodnot hash hesel.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-127">Cyber-criminals also use similar strategies to inform their attacks, such as building a [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) for cracking password hashes.</span></span>

<span data-ttu-id="e6b5f-128">Microsoft průběžně analyzuje [úniky dat](https://www.privacyrights.org/data-breaches) a udržuje dynamicky aktualizovaný seznam zakázaných hesel, který zajišťuje, že citlivá hesla jsou zakázána dřív, než se stanou skutečnou hrozbou zákazníkům služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-128">Microsoft continually analyzes [data breaches](https://www.privacyrights.org/data-breaches) to maintain a dynamically updated banned password list, which ensures that vulnerable passwords are banned before they become a real threat to Azure AD customers.</span></span> <span data-ttu-id="e6b5f-129">Další informace o našem aktuálním úsilí týkajícím se zabezpečení najdete ve zprávě [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6b5f-129">For more information about our current security efforts, see the [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx).</span></span>

### <a name="smart-password-lockout"></a><span data-ttu-id="e6b5f-130">Inteligentní uzamčení hesla</span><span class="sxs-lookup"><span data-stu-id="e6b5f-130">Smart Password Lockout</span></span>

<span data-ttu-id="e6b5f-131">Když Azure AD zjistí, že potenciální kybernetický zločinec se snaží o prolomení uživatelského hesla, uživatelský účet se uzamkne pomocí inteligentního uzamčení hesla.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-131">When Azure AD detects a potential cyber-criminal trying to hack into a user password, we lock the user account with Smart Password Lockout.</span></span> <span data-ttu-id="e6b5f-132">Azure AD vyhodnocuje riziko spojené s konkrétními přihlašovacími relacemi.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-132">Azure AD is designed to determine the risk associated with specific login sessions.</span></span> <span data-ttu-id="e6b5f-133">Potom pomocí nejnovější data zabezpečení použijeme uzamčení sémantiku zastavit internetových hrozeb.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-133">Then using the most up-to-date security data, we apply lockout semantics to stop cyber threats.</span></span>

<span data-ttu-id="e6b5f-134">Pokud se uživatel uzamkne mimo Azure AD, jejich obrazovky vypadá podobně jako následující:</span><span class="sxs-lookup"><span data-stu-id="e6b5f-134">If a user is locked out of Azure AD, their screen looks similar to the one that follows:</span></span>

  ![Uzamčení a vyloučení z Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)

<span data-ttu-id="e6b5f-136">Pro ostatní účty Microsoft jejich obrazovky vypadá podobně jako následující:</span><span class="sxs-lookup"><span data-stu-id="e6b5f-136">For other Microsoft accounts, their screen looks similar to the one that follows:</span></span>

  ![Uzamčení a vyloučení z účtu Microsoft](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

<span data-ttu-id="e6b5f-138">Informace o vytvoření nového ve službě Azure Active Directory, naleznete v tématu [Azure AD samoobslužného resetování hesla pro IT profesionály](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="e6b5f-138">For information about password reset in Azure Active Directory, see the topic [Azure AD self-service password reset for the IT professional](active-directory-passwords.md).</span></span>

  >[!NOTE]
  ><span data-ttu-id="e6b5f-139">Pokud jste správce Azure AD, můžete chtít použít [Windows Hello](https://www.microsoft.com/windows/windows-hello), aby vaši uživatelé přestali vytvářet tradiční hesla úplně.</span><span class="sxs-lookup"><span data-stu-id="e6b5f-139">If you are an Azure AD administrator, you may want to use [Windows Hello](https://www.microsoft.com/windows/windows-hello) to avoid having your users create traditional passwords altogether.</span></span>
  >

## <a name="next-steps"></a><span data-ttu-id="e6b5f-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e6b5f-140">Next steps</span></span>

* [<span data-ttu-id="e6b5f-141">Postup aktualizace vlastního hesla</span><span class="sxs-lookup"><span data-stu-id="e6b5f-141">How to update your own password</span></span>](active-directory-passwords-update-your-own-password.md)
* [<span data-ttu-id="e6b5f-142">Základy správy identit Azure</span><span class="sxs-lookup"><span data-stu-id="e6b5f-142">The fundamentals of Azure identity management</span></span>](fundamentals-identity.md)
* [<span data-ttu-id="e6b5f-143">Aktivita resetování zprávu o heslo</span><span class="sxs-lookup"><span data-stu-id="e6b5f-143">Report on password reset activity</span></span>](active-directory-passwords-reporting.md)


