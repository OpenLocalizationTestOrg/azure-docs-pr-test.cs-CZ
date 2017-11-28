---
title: "aaaAzure AD vrstvené heslo zabezpečení | Microsoft Docs"
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
ms.openlocfilehash: 10d8b600d9f4c02355b2cd8c5dccf8505aaf210d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="a-multi-tiered-approach-tooazure-ad-password-security"></a><span data-ttu-id="b5014-103">Zabezpečení přístupu víceúrovňových tooAzure AD heslo</span><span class="sxs-lookup"><span data-stu-id="b5014-103">A multi-tiered approach tooAzure AD password security</span></span>

<span data-ttu-id="b5014-104">Tento článek popisuje některé z osvědčených postupů můžete postupovat podle uživatele nebo tooprotect správce Azure Active Directory (Azure AD) nebo Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b5014-104">This article discusses some best practices you can follow as a user or as an administrator tooprotect your Azure Active Directory (Azure AD) or Microsoft Account.</span></span>

 > [!NOTE]
 > <span data-ttu-id="b5014-105">Správci služby Azure AD můžete resetovat hesla uživatelů podle pokynů hello v článku hello [resetovat hello heslo pro uživatele v Azure Active Directory](active-directory-users-reset-password-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b5014-105">Azure AD administrators can reset user passwords using hello guidance in hello article [Reset hello password for a user in Azure Active Directory](active-directory-users-reset-password-azure-portal.md).</span></span>
 >
 > <span data-ttu-id="b5014-106">Uživatelům můžete resetovat vlastní heslo pomocí hello pokyny v článku hello [nápovědy zapomenuté heslo Azure AD](active-directory-passwords-update-your-own-password.md).</span><span class="sxs-lookup"><span data-stu-id="b5014-106">Users can reset their own password using hello guidance in hello article [Help I forgot my Azure AD password](active-directory-passwords-update-your-own-password.md).</span></span>
 >

## <a name="password-requirements"></a><span data-ttu-id="b5014-107">Požadavky na heslo</span><span class="sxs-lookup"><span data-stu-id="b5014-107">Password requirements</span></span>

<span data-ttu-id="b5014-108">Azure AD zahrnuje hello následující běžné přístupy toosecuring hesla:</span><span class="sxs-lookup"><span data-stu-id="b5014-108">Azure AD incorporates hello following common approaches toosecuring passwords:</span></span>

* <span data-ttu-id="b5014-109">Požadavky na délku hesla</span><span class="sxs-lookup"><span data-stu-id="b5014-109">Password length requirements</span></span>
* <span data-ttu-id="b5014-110">Požadavky na složitost hesla</span><span class="sxs-lookup"><span data-stu-id="b5014-110">Password complexity requirements</span></span>
* <span data-ttu-id="b5014-111">Pravidelné a opakované vypršení platnosti hesla</span><span class="sxs-lookup"><span data-stu-id="b5014-111">Regular and periodic password expiration</span></span>

<span data-ttu-id="b5014-112">Informace o vytvoření nového ve službě Azure Active Directory najdete v tématu hello [Azure AD samoobslužného resetování hesla pro odborníky v oblasti IT hello](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="b5014-112">For information about password reset in Azure Active Directory, see hello topic [Azure AD self-service password reset for hello IT professional](active-directory-passwords.md).</span></span>

## <a name="azure-ad-password-protections"></a><span data-ttu-id="b5014-113">Ochrana hesla služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5014-113">Azure AD password protections</span></span>

<span data-ttu-id="b5014-114">Azure AD a hello systémem účtů Microsoft použít odvětví ověřené blíží tooensure zabezpečení ochranu uživatele a správce hesel, včetně:</span><span class="sxs-lookup"><span data-stu-id="b5014-114">Azure AD and hello Microsoft Account System use industry proven approaches tooensure secure protection of user and administrator passwords including:</span></span>

* <span data-ttu-id="b5014-115">Dynamicky zakázaná hesla</span><span class="sxs-lookup"><span data-stu-id="b5014-115">Dynamically banned passwords</span></span>
* <span data-ttu-id="b5014-116">Inteligentní uzamčení hesla</span><span class="sxs-lookup"><span data-stu-id="b5014-116">Smart Password Lockout</span></span>

<span data-ttu-id="b5014-117">Informace o správě hesel podle aktuální research, najdete v dokumentu White Paper hello [heslo pokyny](http://aka.ms/passwordguidance).</span><span class="sxs-lookup"><span data-stu-id="b5014-117">For information about password management based on current research, see hello whitepaper [Password Guidance](http://aka.ms/passwordguidance).</span></span>

### <a name="dynamically-banned-passwords"></a><span data-ttu-id="b5014-118">Dynamicky zakázaná hesla</span><span class="sxs-lookup"><span data-stu-id="b5014-118">Dynamically banned passwords</span></span>

<span data-ttu-id="b5014-119">Azure AD a Accounts Microsoft ochrana heslem zabezpečit pomocí dynamicky zakazování běžně používané hesla.</span><span class="sxs-lookup"><span data-stu-id="b5014-119">Azure AD and Microsoft Accounts safeguard password protection by dynamically banning commonly used passwords.</span></span> <span data-ttu-id="b5014-120">tým ochrany identit Azure ID Hello pravidelně analyzuje seznamů zakázaných heslo, brání uživatelům v výběr běžně používané hesla.</span><span class="sxs-lookup"><span data-stu-id="b5014-120">hello Azure ID Identity Protection team routinely analyzes banned password lists, preventing users from selecting commonly used passwords.</span></span> <span data-ttu-id="b5014-121">Tato služba je k dispozici tooAzure AD a Zákazníci využívající službu účet Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="b5014-121">This service is available tooAzure AD and hello Microsoft Account Service customers.</span></span>

<span data-ttu-id="b5014-122">Při vytváření hesel, je důležité správci tooencourage uživatelé toochoose heslo vět, které zahrnují jedinečnou kombinaci písmena, čísla, znaky nebo slova.</span><span class="sxs-lookup"><span data-stu-id="b5014-122">When creating passwords, it is important for administrators tooencourage users toochoose password phrases that include a unique combination of letters, numbers, characters, or words.</span></span> <span data-ttu-id="b5014-123">Tento přístup pomáhá téměř nemožné toobe ohrožení zabezpečení, ale usnadňuje uživatelům tooremember toomake uživatelská hesla.</span><span class="sxs-lookup"><span data-stu-id="b5014-123">This approach helps toomake user passwords nearly impossible toobe compromised but easier for users tooremember.</span></span>

#### <a name="password-breaches"></a><span data-ttu-id="b5014-124">Narušení heslo</span><span class="sxs-lookup"><span data-stu-id="b5014-124">Password breaches</span></span>

<span data-ttu-id="b5014-125">Společnost Microsoft pracuje vždy toostay jeden krok před zločinci.</span><span class="sxs-lookup"><span data-stu-id="b5014-125">Microsoft is always working toostay one step ahead of cyber-criminals.</span></span>

<span data-ttu-id="b5014-126">tým Azure AD Identity Protection Hello průběžně analyzuje hesla, které se běžně používají.</span><span class="sxs-lookup"><span data-stu-id="b5014-126">hello Azure AD Identity Protection team continually analyzes passwords that are commonly used.</span></span> <span data-ttu-id="b5014-127">Zločinci také použít podobné tooinform strategie jejich útoky, jako je například vytváření [duhové tabulky](https://en.wikipedia.org/wiki/Rainbow_table) pro krakování hodnot hash hesel.</span><span class="sxs-lookup"><span data-stu-id="b5014-127">Cyber-criminals also use similar strategies tooinform their attacks, such as building a [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) for cracking password hashes.</span></span>

<span data-ttu-id="b5014-128">Společnost Microsoft průběžně analyzuje [úniky dat](https://www.privacyrights.org/data-breaches) toomaintain seznam dynamicky aktualizované zakázaného heslo, které zajišťuje, že citlivé hesla jsou zakázané dřív, než narostou reálné hrozby tooAzure AD zákazníků.</span><span class="sxs-lookup"><span data-stu-id="b5014-128">Microsoft continually analyzes [data breaches](https://www.privacyrights.org/data-breaches) toomaintain a dynamically updated banned password list, which ensures that vulnerable passwords are banned before they become a real threat tooAzure AD customers.</span></span> <span data-ttu-id="b5014-129">Další informace o našich aktuální úsilí zabezpečení najdete v tématu hello [sestavy Intelligence zabezpečení Microsoft](https://www.microsoft.com/security/sir/default.aspx).</span><span class="sxs-lookup"><span data-stu-id="b5014-129">For more information about our current security efforts, see hello [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx).</span></span>

### <a name="smart-password-lockout"></a><span data-ttu-id="b5014-130">Inteligentní uzamčení hesla</span><span class="sxs-lookup"><span data-stu-id="b5014-130">Smart Password Lockout</span></span>

<span data-ttu-id="b5014-131">Pokud Azure AD zjistí potenciální snažíme toohack internetový trestního do heslo uživatele, jsme uzamčení hello uživatelský účet s inteligentní uzamčení heslo.</span><span class="sxs-lookup"><span data-stu-id="b5014-131">When Azure AD detects a potential cyber-criminal trying toohack into a user password, we lock hello user account with Smart Password Lockout.</span></span> <span data-ttu-id="b5014-132">Azure AD je navržený tak, toodetermine hello riziko spojené s konkrétní přihlašovací relace.</span><span class="sxs-lookup"><span data-stu-id="b5014-132">Azure AD is designed toodetermine hello risk associated with specific login sessions.</span></span> <span data-ttu-id="b5014-133">Potom pomocí hello nejaktuálnější data zabezpečení, použijeme uzamčení sémantiku toostop internetových hrozeb.</span><span class="sxs-lookup"><span data-stu-id="b5014-133">Then using hello most up-to-date security data, we apply lockout semantics toostop cyber threats.</span></span>

<span data-ttu-id="b5014-134">Pokud se uživatel uzamkne mimo Azure AD, jejich obrazovky vypadá podobně jako toohello, ten, který následuje:</span><span class="sxs-lookup"><span data-stu-id="b5014-134">If a user is locked out of Azure AD, their screen looks similar toohello one that follows:</span></span>

  ![Uzamčení a vyloučení z Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)

<span data-ttu-id="b5014-136">Pro ostatní účty Microsoft jejich obrazovky vypadá podobně jako toohello, ten, který následuje:</span><span class="sxs-lookup"><span data-stu-id="b5014-136">For other Microsoft accounts, their screen looks similar toohello one that follows:</span></span>

  ![Uzamčení a vyloučení z účtu Microsoft](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

<span data-ttu-id="b5014-138">Informace o vytvoření nového ve službě Azure Active Directory najdete v tématu hello [Azure AD samoobslužného resetování hesla pro odborníky v oblasti IT hello](active-directory-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="b5014-138">For information about password reset in Azure Active Directory, see hello topic [Azure AD self-service password reset for hello IT professional](active-directory-passwords.md).</span></span>

  >[!NOTE]
  ><span data-ttu-id="b5014-139">Pokud jste správce Azure AD, může být vhodné toouse [Windows Hello](https://www.microsoft.com/windows/windows-hello) tooavoid s uživatelům vytvářet zcela tradiční hesla.</span><span class="sxs-lookup"><span data-stu-id="b5014-139">If you are an Azure AD administrator, you may want toouse [Windows Hello](https://www.microsoft.com/windows/windows-hello) tooavoid having your users create traditional passwords altogether.</span></span>
  >

## <a name="next-steps"></a><span data-ttu-id="b5014-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5014-140">Next steps</span></span>

* [<span data-ttu-id="b5014-141">Jak tooupdate své vlastní heslo</span><span class="sxs-lookup"><span data-stu-id="b5014-141">How tooupdate your own password</span></span>](active-directory-passwords-update-your-own-password.md)
* [<span data-ttu-id="b5014-142">Hello základy Azure identity management.</span><span class="sxs-lookup"><span data-stu-id="b5014-142">hello fundamentals of Azure identity management</span></span>](fundamentals-identity.md)
* [<span data-ttu-id="b5014-143">Aktivita resetování zprávu o heslo</span><span class="sxs-lookup"><span data-stu-id="b5014-143">Report on password reset activity</span></span>](active-directory-passwords-reporting.md)


