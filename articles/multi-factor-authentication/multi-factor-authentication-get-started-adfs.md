---
title: "Krok aaaTwo ověřování a služby AD FS - Azure MFA | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, který popisuje, jak tooget pracovat s Azure MFA a AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 44fbba68-6cf9-46c1-a9df-736580b68ae3
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2017
ms.author: kgremban
ms.openlocfilehash: 7c1c925039d3cb753ba60e286168e5869faeae4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a><span data-ttu-id="c1228-103">Začínáme s ověřováním Azure Multi-Factor Authentication a službami Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="c1228-103">Getting started with Azure Multi-Factor Authentication and Active Directory Federation Services</span></span>
<span data-ttu-id="c1228-104"><center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center></span><span class="sxs-lookup"><span data-stu-id="c1228-104"><center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center></span></span>

<span data-ttu-id="c1228-105">Pokud má vaše organizace federované místní služby Active Directory s Azure Active Directory využívající službu AD FS, jsou k dispozici následující 2 možnosti používání ověřování Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="c1228-105">If your organization has federated your on-premises Active Directory with Azure Active Directory using AD FS, there are two options for using Azure Multi-Factor Authentication.</span></span>

* <span data-ttu-id="c1228-106">Zabezpečení cloudových prostředků pomocí ověřování Azure Multi-Factor Authentication nebo služeb Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="c1228-106">Secure cloud resources using Azure Multi-Factor Authentication or Active Directory Federation Services</span></span>
* <span data-ttu-id="c1228-107">Zabezpečení cloudu a místních prostředků pomocí ověřování Azure Multi-Factor Authentication Server</span><span class="sxs-lookup"><span data-stu-id="c1228-107">Secure cloud and on-premises resources using Azure Multi-Factor Authentication Server</span></span>

<span data-ttu-id="c1228-108">Hello následující tabulka shrnuje zkušeností ověřování hello mezi zabezpečením prostředků pomocí ověřování Azure Multi-Factor Authentication a AD FS</span><span class="sxs-lookup"><span data-stu-id="c1228-108">hello following table summarizes hello verification experience between securing resources with Azure Multi-Factor Authentication and AD FS</span></span>

| <span data-ttu-id="c1228-109">Zkušenosti s ověřováním – aplikace založené na prohlížeči</span><span class="sxs-lookup"><span data-stu-id="c1228-109">Verification Experience - Browser-based Apps</span></span> | <span data-ttu-id="c1228-110">Zkušenosti s ověřováním – aplikace nezaložené na prohlížeči</span><span class="sxs-lookup"><span data-stu-id="c1228-110">Verification Experience - Non-Browser-based Apps</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="c1228-111">Zabezpečení prostředků Azure AD pomocí Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="c1228-111">Securing Azure AD resources using Azure Multi-Factor Authentication</span></span> |<li><span data-ttu-id="c1228-112">prvním krokem ověření Hello se provádí místně pomocí služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="c1228-112">hello first verification step is performed on-premises using AD FS.</span></span></li> <li><span data-ttu-id="c1228-113">druhý krok Hello je metoda založené na telefonu prováděná pomocí cloudového ověřování.</span><span class="sxs-lookup"><span data-stu-id="c1228-113">hello second step is a phone-based method carried out using cloud authentication.</span></span></li> |
| <span data-ttu-id="c1228-114">Zabezpečení prostředků Azure AD pomocí Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="c1228-114">Securing Azure AD resources using Active Directory Federation Services</span></span> |<li><span data-ttu-id="c1228-115">prvním krokem ověření Hello se provádí místně pomocí služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="c1228-115">hello first verification step is performed on-premises using AD FS.</span></span></li><li><span data-ttu-id="c1228-116">druhým krokem Hello je prováděn v místě dodržením deklarace identity hello.</span><span class="sxs-lookup"><span data-stu-id="c1228-116">hello second step is performed on-premises by honoring hello claim.</span></span></li> |

<span data-ttu-id="c1228-117">Upozornění s hesly aplikací pro federované uživatele:</span><span class="sxs-lookup"><span data-stu-id="c1228-117">Caveats with app passwords for federated users:</span></span>

* <span data-ttu-id="c1228-118">Hesla aplikace se ověřují pomocí cloudového ověřování, takže obcházejí federaci.</span><span class="sxs-lookup"><span data-stu-id="c1228-118">App passwords are verified using cloud authentication, so they bypass federation.</span></span> <span data-ttu-id="c1228-119">Federace se aktivně používá jenom při nastavování hesla aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1228-119">Federation is only actively used when setting up an app password.</span></span>
* <span data-ttu-id="c1228-120">Místní nastavení služby Access Control klienta nejsou dodržená hesly aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1228-120">On-premises Client Access Control settings are not honored by app passwords.</span></span>
* <span data-ttu-id="c1228-121">U hesel aplikací ztratíte schopnost ověřování přihlašování v místě.</span><span class="sxs-lookup"><span data-stu-id="c1228-121">You lose on-premises authentication-logging capability for app passwords.</span></span>
* <span data-ttu-id="c1228-122">Zakázání/odstranění účtu může trvat hodiny toothree pro synchronizaci adresáře, přičemž dojde ke zpoždění zakázání/odstranění hesla aplikace v cloudové identitě hello.</span><span class="sxs-lookup"><span data-stu-id="c1228-122">Account disable/deletion may take up toothree hours for directory sync, delaying disable/deletion of app passwords in hello cloud identity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1228-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1228-123">Next steps</span></span>
<span data-ttu-id="c1228-124">Informace o nastavení ověřování Azure Multi-Factor Authentication nebo hello Azure Multi-Factor Authentication Server se službou AD FS najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="c1228-124">For information on setting up either Azure Multi-Factor Authentication or hello Azure Multi-Factor Authentication Server with AD FS, see hello following articles:</span></span>

* [<span data-ttu-id="c1228-125">Zabezpečené cloudové prostředky používající ověřování Azure Multi-Factor Authentication a AD FS</span><span class="sxs-lookup"><span data-stu-id="c1228-125">Secure cloud resources using Azure Multi-Factor Authentication and AD FS</span></span>](multi-factor-authentication-get-started-adfs-cloud.md)
* [<span data-ttu-id="c1228-126">Zabezpečený cloud a místní prostředky používající ověřování Azure Multi-Factor Authentication Server s Windows Server 2012 R2 AD FS</span><span class="sxs-lookup"><span data-stu-id="c1228-126">Secure cloud and on-premises resources using Azure Multi-Factor Authentication Server with Windows Server 2012 R2 AD FS</span></span>](multi-factor-authentication-get-started-adfs-w2k12.md)
* [<span data-ttu-id="c1228-127">Zabezpečené cloudové a lokální prostředky používající Microsoft Azure Multi-Factor Authentication Serveru s AD FS 2.0</span><span class="sxs-lookup"><span data-stu-id="c1228-127">Secure cloud and on-premises resources using Azure Multi-Factor Authentication Server with AD FS 2.0</span></span>](multi-factor-authentication-get-started-adfs-adfs2.md)
