---
title: "aaaVulnerabilities detekovaných službou Azure Active Directory Identity Protection | Microsoft Docs"
description: "Přehled hello chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92233a5b-cb34-4d28-88cc-d5d29c0f3256
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 5e1cb401f8b566a180eb46e3420a090bcfc66767
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a><span data-ttu-id="6fc60-104">Chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="6fc60-104">Vulnerabilities detected by Azure Active Directory Identity Protection</span></span>
<span data-ttu-id="6fc60-105">Ohrožení zabezpečení jsou slabá místa ve vašem prostředí, může útočník zneužít.</span><span class="sxs-lookup"><span data-stu-id="6fc60-105">Vulnerabilities are weaknesses in your environment that can be exploited by an attacker.</span></span> <span data-ttu-id="6fc60-106">Doporučujeme, abyste vyřešte tyto chyby zabezpečení tooimprove hello postavení zabezpečení vaší organizace a útočníkům zabránit v jejich využití.</span><span class="sxs-lookup"><span data-stu-id="6fc60-106">We recommend that you address these vulnerabilities tooimprove hello security posture of your organization, and prevent attackers from exploiting them.</span></span>


<span data-ttu-id="6fc60-107">![ohrožení zabezpečení](./media/active-directory-identityprotection-vulnerabilities/101.png "ohrožení zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="6fc60-107">![vulnerabilities](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")</span></span>



<span data-ttu-id="6fc60-108">Hello následující části poskytují přehled o ohrožení zabezpečení hello hlášené Identity Protection.</span><span class="sxs-lookup"><span data-stu-id="6fc60-108">hello following sections provide you with an overview of hello vulnerabilities reported by Identity Protection.</span></span>

## <a name="multi-factor-authentication-registration-not-configured"></a><span data-ttu-id="6fc60-109">Registrace služby Multi-Factor authentication není nakonfigurováno</span><span class="sxs-lookup"><span data-stu-id="6fc60-109">Multi-factor authentication registration not configured</span></span>
<span data-ttu-id="6fc60-110">Toto ohrožení zabezpečení umožňuje řídit hello nasazení služby Azure Multi-Factor Authentication ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="6fc60-110">This vulnerability helps you control hello deployment of Azure Multi-Factor Authentication in your organization.</span></span> 

<span data-ttu-id="6fc60-111">Ověřování Azure Multi-Factor authentication poskytuje druhou vrstvu zabezpečení toouser ověřování.</span><span class="sxs-lookup"><span data-stu-id="6fc60-111">Azure multi-factor authentication provides a second layer of security toouser authentication.</span></span> <span data-ttu-id="6fc60-112">Při splnění požadavků uživatelů pro jednoduchý proces přihlášení pomáhá chránit přístup toodata a aplikace.</span><span class="sxs-lookup"><span data-stu-id="6fc60-112">It helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="6fc60-113">Zajišťuje silné ověřování přes celou řadu možností snadno ověření – telefonní hovor, textová zpráva nebo mobilní aplikace oznámení nebo ověřovací kód a 3. stran tokeny OATH.</span><span class="sxs-lookup"><span data-stu-id="6fc60-113">It delivers strong authentication via a range of easy verification options—phone call, text message, or mobile app notification or verification code and 3rd party OATH tokens.</span></span>

<span data-ttu-id="6fc60-114">Doporučujeme vám, že vyžadujete ověřování Azure Multi-Factor Authentication pro přihlášení uživatele. Služba Multi-Factor authentication hrají roli klíče v zásadách podmíněného přístupu na základě riziko k dispozici prostřednictvím Identity Protection.</span><span class="sxs-lookup"><span data-stu-id="6fc60-114">We recommend that you require Azure Multi-Factor Authentication for user sign-ins. Multi-factor authentication plays a key role in risk-based conditional access policies available through Identity Protection.</span></span>

<span data-ttu-id="6fc60-115">Další podrobnosti najdete v tématu [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="6fc60-115">For more details, see [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>

## <a name="unmanaged-cloud-apps"></a><span data-ttu-id="6fc60-116">Nespravované cloudové aplikace</span><span class="sxs-lookup"><span data-stu-id="6fc60-116">Unmanaged cloud apps</span></span>
<span data-ttu-id="6fc60-117">Toto ohrožení zabezpečení pomáhá identifikovat nespravované cloudových aplikací ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="6fc60-117">This vulnerability helps you identify unmanaged cloud apps in your organization.</span></span>

<span data-ttu-id="6fc60-118">V rámci moderní firmy IT oddělení neberou často všechny hello cloudové aplikace, uživateli v organizaci používáte toodo práci.</span><span class="sxs-lookup"><span data-stu-id="6fc60-118">In modern enterprises, IT departments are often unaware of all hello cloud applications that users in their organization are using toodo their work.</span></span> <span data-ttu-id="6fc60-119">Je snadno toosee, proč by správci mají obavy o neoprávněný přístup k datům toocorporate, možné úniku a další bezpečnostní rizika.</span><span class="sxs-lookup"><span data-stu-id="6fc60-119">It is easy toosee why administrators would have concerns about unauthorized access toocorporate data, possible data leakage and other security risks.</span></span> 

<span data-ttu-id="6fc60-120">Doporučujeme vám, že vaše organizace nasazení Cloud App Discovery toodiscover nespravované cloudových aplikací a toomanage tyto aplikace pomocí Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6fc60-120">We recommend that your organization deploy Cloud App Discovery toodiscover unmanaged cloud applications, and toomanage these applications using Azure Active Directory.</span></span>

<span data-ttu-id="6fc60-121">Další podrobnosti najdete v tématu [hledání nespravované cloudových aplikací s Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6fc60-121">For more details, see [Finding unmanaged cloud applications with Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span></span>

## <a name="security-alerts-from-privileged-identity-management"></a><span data-ttu-id="6fc60-122">Výstrahy zabezpečení z Správa privilegovaných identit</span><span class="sxs-lookup"><span data-stu-id="6fc60-122">Security Alerts from Privileged Identity Management</span></span>
<span data-ttu-id="6fc60-123">Toto ohrožení zabezpečení vám pomůže zjistit a vyřešit výstrahy týkající se privilegované identity ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="6fc60-123">This vulnerability helps you discover and resolve alerts about privileged identities in your organization.</span></span>  

<span data-ttu-id="6fc60-124">Uživatelé toocarry tooenable out privilegovaných operací, organizace potřebují uživatelé toogrant dočasné nebo trvalé privilegovaný přístup k ve službě Azure AD, Azure nebo Office 365 prostředků nebo jiných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6fc60-124">tooenable users toocarry out privileged operations, organizations need toogrant users temporary or permanent privileged access in Azure AD, Azure or Office 365 resources, or other SaaS apps.</span></span> <span data-ttu-id="6fc60-125">Každý z těchto mohou uživatelé s oprávněním zvyšuje hello prostor pro útoky vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="6fc60-125">Each of these privileged users increases hello attack surface of your organization.</span></span> <span data-ttu-id="6fc60-126">Toto ohrožení zabezpečení vám pomůže identifikovat uživatele s nepotřebné privilegovaného přístupu a proveďte příslušnou akci tooreduce nebo odstranění hello nebezpečí, jež představují.</span><span class="sxs-lookup"><span data-stu-id="6fc60-126">This vulnerability helps you identify users with unnecessary privileged access, and take appropriate action tooreduce or eliminate hello risk they pose.</span></span> 

<span data-ttu-id="6fc60-127">Doporučujeme vám, že vaše organizace používá Azure AD Privileged Identity Management toomanage, řízení a monitorování privilegované identity a jejich tooresources přístupu ve službě Azure AD a také jiných služeb Microsoft online services jako je Office 365 nebo Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="6fc60-127">We recommend that your organization uses Azure AD Privileged Identity Management toomanage, control, and monitor privileged identities and their access tooresources in Azure AD as well as other Microsoft online services like Office 365 or Microsoft Intune.</span></span>

<span data-ttu-id="6fc60-128">Další podrobnosti najdete v tématu [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span><span class="sxs-lookup"><span data-stu-id="6fc60-128">For more details, see [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span></span> 

## <a name="see-also"></a><span data-ttu-id="6fc60-129">Viz také</span><span class="sxs-lookup"><span data-stu-id="6fc60-129">See also</span></span>
* [<span data-ttu-id="6fc60-130">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6fc60-130">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

