---
title: "Chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection | Microsoft Docs"
description: "Přehled chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection."
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
ms.openlocfilehash: 364873ff54099a6123e40b12e819d1745751f285
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a><span data-ttu-id="1edf5-104">Chyb zabezpečení detekovaných službou Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="1edf5-104">Vulnerabilities detected by Azure Active Directory Identity Protection</span></span>
<span data-ttu-id="1edf5-105">Ohrožení zabezpečení jsou slabá místa ve vašem prostředí, může útočník zneužít.</span><span class="sxs-lookup"><span data-stu-id="1edf5-105">Vulnerabilities are weaknesses in your environment that can be exploited by an attacker.</span></span> <span data-ttu-id="1edf5-106">Doporučujeme, abyste tyto nedostatky ke zlepšení postavení zabezpečení vaší organizace a útočníkům zabránit v jejich využití.</span><span class="sxs-lookup"><span data-stu-id="1edf5-106">We recommend that you address these vulnerabilities to improve the security posture of your organization, and prevent attackers from exploiting them.</span></span>


<span data-ttu-id="1edf5-107">![ohrožení zabezpečení](./media/active-directory-identityprotection-vulnerabilities/101.png "ohrožení zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="1edf5-107">![vulnerabilities](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")</span></span>



<span data-ttu-id="1edf5-108">Následující části poskytují přehled o ohrožení zabezpečení hlášené Identity Protection.</span><span class="sxs-lookup"><span data-stu-id="1edf5-108">The following sections provide you with an overview of the vulnerabilities reported by Identity Protection.</span></span>

## <a name="multi-factor-authentication-registration-not-configured"></a><span data-ttu-id="1edf5-109">Registrace služby Multi-Factor authentication není nakonfigurováno</span><span class="sxs-lookup"><span data-stu-id="1edf5-109">Multi-factor authentication registration not configured</span></span>
<span data-ttu-id="1edf5-110">Toto ohrožení zabezpečení umožňuje řízení nasazení služby Azure Multi-Factor Authentication ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="1edf5-110">This vulnerability helps you control the deployment of Azure Multi-Factor Authentication in your organization.</span></span> 

<span data-ttu-id="1edf5-111">Ověřování Azure Multi-Factor authentication poskytuje druhou vrstvu zabezpečení pro ověřování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1edf5-111">Azure multi-factor authentication provides a second layer of security to user authentication.</span></span> <span data-ttu-id="1edf5-112">Ho pomáhá zabezpečit přístup k datům a aplikacím při splnění požadavků uživatelů pro jednoduchý proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1edf5-112">It helps safeguard access to data and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="1edf5-113">Zajišťuje silné ověřování přes celou řadu možností snadno ověření – telefonní hovor, textová zpráva nebo mobilní aplikace oznámení nebo ověřovací kód a 3. stran tokeny OATH.</span><span class="sxs-lookup"><span data-stu-id="1edf5-113">It delivers strong authentication via a range of easy verification options—phone call, text message, or mobile app notification or verification code and 3rd party OATH tokens.</span></span>

<span data-ttu-id="1edf5-114">Doporučujeme vám, že vyžadujete ověřování Azure Multi-Factor Authentication pro přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="1edf5-114">We recommend that you require Azure Multi-Factor Authentication for user sign-ins.</span></span> <span data-ttu-id="1edf5-115">Služba Multi-Factor authentication hrají roli klíče v zásadách podmíněného přístupu na základě riziko k dispozici prostřednictvím Identity Protection.</span><span class="sxs-lookup"><span data-stu-id="1edf5-115">Multi-factor authentication plays a key role in risk-based conditional access policies available through Identity Protection.</span></span>

<span data-ttu-id="1edf5-116">Další podrobnosti najdete v tématu [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="1edf5-116">For more details, see [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>

## <a name="unmanaged-cloud-apps"></a><span data-ttu-id="1edf5-117">Nespravované cloudové aplikace</span><span class="sxs-lookup"><span data-stu-id="1edf5-117">Unmanaged cloud apps</span></span>
<span data-ttu-id="1edf5-118">Toto ohrožení zabezpečení pomáhá identifikovat nespravované cloudových aplikací ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="1edf5-118">This vulnerability helps you identify unmanaged cloud apps in your organization.</span></span>

<span data-ttu-id="1edf5-119">V rámci moderní firmy IT oddělení neberou často všechny cloudové aplikace, které uživateli v organizaci používáte ke své práci.</span><span class="sxs-lookup"><span data-stu-id="1edf5-119">In modern enterprises, IT departments are often unaware of all the cloud applications that users in their organization are using to do their work.</span></span> <span data-ttu-id="1edf5-120">Je snadno zjistit, proč by správci mají obavy o neoprávněný přístup k podnikovým datům, možné úniku a další bezpečnostní rizika.</span><span class="sxs-lookup"><span data-stu-id="1edf5-120">It is easy to see why administrators would have concerns about unauthorized access to corporate data, possible data leakage and other security risks.</span></span> 

<span data-ttu-id="1edf5-121">Doporučujeme vám, že vaše organizace nasadit Cloud App Discovery ke zjišťování nespravované cloudových aplikací a ke správě těchto aplikací pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1edf5-121">We recommend that your organization deploy Cloud App Discovery to discover unmanaged cloud applications, and to manage these applications using Azure Active Directory.</span></span>

<span data-ttu-id="1edf5-122">Další podrobnosti najdete v tématu [hledání nespravované cloudových aplikací s Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1edf5-122">For more details, see [Finding unmanaged cloud applications with Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span></span>

## <a name="security-alerts-from-privileged-identity-management"></a><span data-ttu-id="1edf5-123">Výstrahy zabezpečení z Správa privilegovaných identit</span><span class="sxs-lookup"><span data-stu-id="1edf5-123">Security Alerts from Privileged Identity Management</span></span>
<span data-ttu-id="1edf5-124">Toto ohrožení zabezpečení vám pomůže zjistit a vyřešit výstrahy týkající se privilegované identity ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="1edf5-124">This vulnerability helps you discover and resolve alerts about privileged identities in your organization.</span></span>  

<span data-ttu-id="1edf5-125">Pokud chcete povolit uživatelům provádět privilegované operace, organizace potřebují udělit uživatelům dočasné nebo trvalé privilegovaného přístupu ve službě Azure AD, Azure nebo Office 365 prostředků nebo jiných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="1edf5-125">To enable users to carry out privileged operations, organizations need to grant users temporary or permanent privileged access in Azure AD, Azure or Office 365 resources, or other SaaS apps.</span></span> <span data-ttu-id="1edf5-126">Každý z těchto mohou uživatelé s oprávněním zvětšuje prostor pro útoky vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="1edf5-126">Each of these privileged users increases the attack surface of your organization.</span></span> <span data-ttu-id="1edf5-127">Toto ohrožení zabezpečení pomáhá identifikovat uživatele s nepotřebné privilegovaného přístupu a proveďte příslušné akce k omezit nebo odstranit, které budou představovat riziko.</span><span class="sxs-lookup"><span data-stu-id="1edf5-127">This vulnerability helps you identify users with unnecessary privileged access, and take appropriate action to reduce or eliminate the risk they pose.</span></span> 

<span data-ttu-id="1edf5-128">Doporučujeme, abyste vaše organizace používá Azure AD Privileged Identity Management pro správu, řízení a monitorování privilegované identity a jejich přístup k prostředkům ve službě Azure AD, jakož i jiných služeb Microsoft online services jako je Office 365 nebo Microsoft Intune.</span><span class="sxs-lookup"><span data-stu-id="1edf5-128">We recommend that your organization uses Azure AD Privileged Identity Management to manage, control, and monitor privileged identities and their access to resources in Azure AD as well as other Microsoft online services like Office 365 or Microsoft Intune.</span></span>

<span data-ttu-id="1edf5-129">Další podrobnosti najdete v tématu [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span><span class="sxs-lookup"><span data-stu-id="1edf5-129">For more details, see [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span></span> 

## <a name="see-also"></a><span data-ttu-id="1edf5-130">Viz také</span><span class="sxs-lookup"><span data-stu-id="1edf5-130">See also</span></span>
* [<span data-ttu-id="1edf5-131">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1edf5-131">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

