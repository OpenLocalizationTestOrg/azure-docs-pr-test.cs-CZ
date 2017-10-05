---
title: "Algoritmus hash podpisu změnu vztahu důvěryhodnosti předávající strany Office 365 | Microsoft Docs"
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
ms.openlocfilehash: c581b1468630a9f28204592c936360b72f42f0d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a><span data-ttu-id="b0e2d-104">Změňte algoritmus hash podpisu pro Office 365 vztah důvěryhodnosti předávající strany</span><span class="sxs-lookup"><span data-stu-id="b0e2d-104">Change signature hash algorithm for Office 365 relying party trust</span></span>
## <a name="overview"></a><span data-ttu-id="b0e2d-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="b0e2d-105">Overview</span></span>
<span data-ttu-id="b0e2d-106">Active Directory Federation Services (AD FS) podepisuje jeho tokenů pro Microsoft Azure Active Directory pro zajištění, aby nemohlo být manipulováno s.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-106">Active Directory Federation Services (AD FS) signs its tokens to Microsoft Azure Active Directory to ensure that they cannot be tampered with.</span></span> <span data-ttu-id="b0e2d-107">Tento podpis může být založen na SHA1 nebo SHA256.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-107">This signature can be based on SHA1 or SHA256.</span></span> <span data-ttu-id="b0e2d-108">Azure Active Directory teď podporuje tokeny, které jsou podepsané s algoritmem SHA256 a doporučujeme nastavení token podpisový algoritmus SHA256 pro nejvyšší úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-108">Azure Active Directory now supports tokens signed with an SHA256 algorithm, and we recommend setting the token-signing algorithm to SHA256 for the highest level of security.</span></span> <span data-ttu-id="b0e2d-109">Tento článek popisuje kroky potřebné k nastavte token podpisový algoritmus SHA256 bezpečnější úroveň.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-109">This article describes the steps needed to set the token-signing algorithm to the more secure SHA256 level.</span></span>

>[!NOTE]
><span data-ttu-id="b0e2d-110">Společnost Microsoft doporučuje použití SHA256 jako algoritmu k podepisování tokenů, jako je bezpečnější než SHA1 ale SHA1 zůstává stále podporované možnosti.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-110">Microsoft recommends usage of SHA256 as the algorithm for signing tokens as it is more secure than SHA1 but SHA1 still remains a supported option.</span></span>

## <a name="change-the-token-signing-algorithm"></a><span data-ttu-id="b0e2d-111">Změna algoritmu podpisové</span><span class="sxs-lookup"><span data-stu-id="b0e2d-111">Change the token-signing algorithm</span></span>
<span data-ttu-id="b0e2d-112">Po nastavení algoritmus podpisu s jedním z dva procesy níže, služby AD FS podepisuje tokeny pro Office 365 vztah důvěryhodnosti předávající strany s SHA256.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-112">After you have set the signature algorithm with one of the two processes below, AD FS signs the tokens for Office 365 relying party trust with SHA256.</span></span> <span data-ttu-id="b0e2d-113">Nepotřebujete žádné další konfigurace změny a tato změna nemá žádný vliv na schopnost přístupu k Office 365 nebo jiné aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-113">You don't need to make any extra configuration changes, and this change has no impact on your ability to access Office 365 or other Azure AD applications.</span></span>

### <a name="ad-fs-management-console"></a><span data-ttu-id="b0e2d-114">Konzola pro správu AD FS</span><span class="sxs-lookup"><span data-stu-id="b0e2d-114">AD FS management console</span></span>
1. <span data-ttu-id="b0e2d-115">Otevřete konzolu pro správu služby AD FS na primárním serveru služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-115">Open the AD FS management console on the primary AD FS server.</span></span>
2. <span data-ttu-id="b0e2d-116">Rozbalte uzel služby AD FS a klikněte na tlačítko **vztahy důvěryhodnosti předávající strany**.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-116">Expand the AD FS node and click **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="b0e2d-117">Klikněte pravým tlačítkem na důvěryhodnosti předávající strany Office 365 nebo Azure a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-117">Right-click your Office 365/Azure relying party trust and select **Properties**.</span></span>
4. <span data-ttu-id="b0e2d-118">Vyberte **Upřesnit** a vyberte algoritmus zabezpečené hash SHA256.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-118">Select the **Advanced** tab and select the secure hash algorithm SHA256.</span></span>
5. <span data-ttu-id="b0e2d-119">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-119">Click **OK**.</span></span>

![Podpisový algoritmus SHA256 – konzoly MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a><span data-ttu-id="b0e2d-121">Rutiny služby AD FS prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b0e2d-121">AD FS PowerShell cmdlets</span></span>
1. <span data-ttu-id="b0e2d-122">Na všechny servery služby AD FS otevřete prostředí PowerShell v části oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-122">On any AD FS server, open PowerShell under administrator privileges.</span></span>
2. <span data-ttu-id="b0e2d-123">Nastavení algoritmu hash zabezpečené pomocí **Set-AdfsRelyingPartyTrust** rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0e2d-123">Set the secure hash algorithm by using the **Set-AdfsRelyingPartyTrust** cmdlet.</span></span>
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a><span data-ttu-id="b0e2d-124">Také pro čtení</span><span class="sxs-lookup"><span data-stu-id="b0e2d-124">Also read</span></span>
* [<span data-ttu-id="b0e2d-125">Opravit vztah důvěryhodnosti Office 365 s Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="b0e2d-125">Repair Office 365 trust with Azure AD Connect</span></span>](connect/active-directory-aadconnect-federation-management.md#repairthetrust)

