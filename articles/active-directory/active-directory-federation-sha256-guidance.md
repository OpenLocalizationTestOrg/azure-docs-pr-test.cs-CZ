---
title: "algoritmus hash podpisu aaaChange Office 365 vztahu důvěryhodnosti předávající strany | Microsoft Docs"
description: "Tato stránka obsahuje pokyny pro změnu algoritmus SHA pro vztah důvěryhodnosti federace s Office 365"
keywords: "SHA1, SHA256, O365, federace, aadconnect, služby AD FS, služby ad fs, změny sha, vztah důvěryhodnosti federace, vztah důvěryhodnosti předávající strany"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: samueld
editor: 
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
ms.openlocfilehash: 3333d1384aff8bdf6b3bcc894f8c633fd9ccc3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a><span data-ttu-id="11449-104">Změňte algoritmus hash podpisu pro Office 365 vztah důvěryhodnosti předávající strany</span><span class="sxs-lookup"><span data-stu-id="11449-104">Change signature hash algorithm for Office 365 relying party trust</span></span>
## <a name="overview"></a><span data-ttu-id="11449-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="11449-105">Overview</span></span>
<span data-ttu-id="11449-106">Active Directory Federation Services (AD FS) podepisuje jeho tooensure Azure Active Directory tooMicrosoft tokeny, který nemůže být úmyslně poškozena.</span><span class="sxs-lookup"><span data-stu-id="11449-106">Active Directory Federation Services (AD FS) signs its tokens tooMicrosoft Azure Active Directory tooensure that they cannot be tampered with.</span></span> <span data-ttu-id="11449-107">Tento podpis může být založen na SHA1 nebo SHA256.</span><span class="sxs-lookup"><span data-stu-id="11449-107">This signature can be based on SHA1 or SHA256.</span></span> <span data-ttu-id="11449-108">Azure Active Directory teď podporuje tokeny, které jsou podepsané s algoritmem SHA256 a doporučujeme nastavení hello token podpisový algoritmus tooSHA256 hello nejvyšší úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="11449-108">Azure Active Directory now supports tokens signed with an SHA256 algorithm, and we recommend setting hello token-signing algorithm tooSHA256 for hello highest level of security.</span></span> <span data-ttu-id="11449-109">Tento článek popisuje kroky hello potřeby tooset hello token podpisový algoritmus toohello více secure úroveň SHA256.</span><span class="sxs-lookup"><span data-stu-id="11449-109">This article describes hello steps needed tooset hello token-signing algorithm toohello more secure SHA256 level.</span></span>

>[!NOTE]
><span data-ttu-id="11449-110">Společnost Microsoft doporučuje použití SHA256 jako algoritmu hello k podepisování tokenů, jako je bezpečnější než SHA1 ale SHA1 zůstává stále podporované možnosti.</span><span class="sxs-lookup"><span data-stu-id="11449-110">Microsoft recommends usage of SHA256 as hello algorithm for signing tokens as it is more secure than SHA1 but SHA1 still remains a supported option.</span></span>

## <a name="change-hello-token-signing-algorithm"></a><span data-ttu-id="11449-111">Změna algoritmu podpisové hello</span><span class="sxs-lookup"><span data-stu-id="11449-111">Change hello token-signing algorithm</span></span>
<span data-ttu-id="11449-112">Po nastavení algoritmus podpisu hello s jedním z hello dva procesy služby AD FS podepisuje tokeny hello pro Office 365 vztah důvěryhodnosti předávající strany s SHA256.</span><span class="sxs-lookup"><span data-stu-id="11449-112">After you have set hello signature algorithm with one of hello two processes below, AD FS signs hello tokens for Office 365 relying party trust with SHA256.</span></span> <span data-ttu-id="11449-113">Nepotřebujete toomake změny další konfigurace a tato změna nemá žádný vliv na vaše možnost tooaccess Office 365 nebo jiné aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="11449-113">You don't need toomake any extra configuration changes, and this change has no impact on your ability tooaccess Office 365 or other Azure AD applications.</span></span>

### <a name="ad-fs-management-console"></a><span data-ttu-id="11449-114">Konzola pro správu AD FS</span><span class="sxs-lookup"><span data-stu-id="11449-114">AD FS management console</span></span>
1. <span data-ttu-id="11449-115">Otevřete konzolu pro správu hello AD FS na server hello primární AD FS.</span><span class="sxs-lookup"><span data-stu-id="11449-115">Open hello AD FS management console on hello primary AD FS server.</span></span>
2. <span data-ttu-id="11449-116">Rozbalte uzel hello AD FS a klikněte na tlačítko **vztahy důvěryhodnosti předávající strany**.</span><span class="sxs-lookup"><span data-stu-id="11449-116">Expand hello AD FS node and click **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="11449-117">Klikněte pravým tlačítkem na důvěryhodnosti předávající strany Office 365 nebo Azure a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="11449-117">Right-click your Office 365/Azure relying party trust and select **Properties**.</span></span>
4. <span data-ttu-id="11449-118">Vyberte hello **Upřesnit** kartě a vyberte hello zabezpečený algoritmus hash SHA256.</span><span class="sxs-lookup"><span data-stu-id="11449-118">Select hello **Advanced** tab and select hello secure hash algorithm SHA256.</span></span>
5. <span data-ttu-id="11449-119">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="11449-119">Click **OK**.</span></span>

![Podpisový algoritmus SHA256 – konzoly MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a><span data-ttu-id="11449-121">Rutiny služby AD FS prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="11449-121">AD FS PowerShell cmdlets</span></span>
1. <span data-ttu-id="11449-122">Na všechny servery služby AD FS otevřete prostředí PowerShell v části oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="11449-122">On any AD FS server, open PowerShell under administrator privileges.</span></span>
2. <span data-ttu-id="11449-123">Sada hello zabezpečený algoritmus hash pomocí hello **Set-AdfsRelyingPartyTrust** rutiny.</span><span class="sxs-lookup"><span data-stu-id="11449-123">Set hello secure hash algorithm by using hello **Set-AdfsRelyingPartyTrust** cmdlet.</span></span>
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a><span data-ttu-id="11449-124">Také pro čtení</span><span class="sxs-lookup"><span data-stu-id="11449-124">Also read</span></span>
* [<span data-ttu-id="11449-125">Opravit vztah důvěryhodnosti Office 365 s Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="11449-125">Repair Office 365 trust with Azure AD Connect</span></span>](connect/active-directory-aadconnect-federation-management.md#repairthetrust)

