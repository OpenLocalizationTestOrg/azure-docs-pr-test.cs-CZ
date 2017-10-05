---
title: "Ochrana služby Azure SQL a data v Azure Security Center | Microsoft Docs"
description: "Tento dokument adresy doporučení v Azure Security Center, které vám pomůžou chránit vaše data a služba Azure SQL a zůstat souladu se zásadami zabezpečení."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 0c3a11e9a86767641533b16de1b96b4c59bfdf51
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a><span data-ttu-id="424e0-103">Ochrana služby Azure SQL a data v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="424e0-103">Protecting Azure SQL service and data in Azure Security Center</span></span>
<span data-ttu-id="424e0-104">Azure Security Center analyzuje stav zabezpečení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="424e0-104">Azure Security Center analyzes the security state of your Azure resources.</span></span> <span data-ttu-id="424e0-105">Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří se doporučení, která vás provede procesem konfigurace potřebných kontrol.</span><span class="sxs-lookup"><span data-stu-id="424e0-105">When Security Center identifies potential security vulnerabilities, it creates recommendations that guide you through the process of configuring the needed controls.</span></span>  <span data-ttu-id="424e0-106">Doporučení platí pro typy prostředků Azure: virtuální počítače (VM), sítě, SQL a datům a aplikacím.</span><span class="sxs-lookup"><span data-stu-id="424e0-106">Recommendations apply to Azure resource types: virtual machines (VMs), networking, SQL and data, and applications.</span></span>

<span data-ttu-id="424e0-107">Tento článek se zaměřuje na doporučení, která se týkají služby Azure SQL a data.</span><span class="sxs-lookup"><span data-stu-id="424e0-107">This article addresses recommendations that apply to Azure SQL service and data.</span></span> <span data-ttu-id="424e0-108">Doporučení center kolem povolení auditování pro servery Azure SQL a databáze, povolení šifrování u databází SQL a povolení šifrování vašeho účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="424e0-108">Recommendations center around enabling auditing for Azure SQL servers and databases, enabling encryption for SQL databases, and enabling encryption of your Azure storage account.</span></span>  <span data-ttu-id="424e0-109">Použijte v následující tabulce vám pomohou pochopit dostupné doporučení služby a data SQL a co každé z nich dělá Pokud použijete ho jako odkaz.</span><span class="sxs-lookup"><span data-stu-id="424e0-109">Use the table below as a reference to help you understand the available SQL service and data recommendations and what each one does if you apply it.</span></span>

## <a name="available-sql-service-and-data-recommendations"></a><span data-ttu-id="424e0-110">K dispozici doporučení služby a dat SQL</span><span class="sxs-lookup"><span data-stu-id="424e0-110">Available SQL service and data recommendations</span></span>
| <span data-ttu-id="424e0-111">Doporučení</span><span class="sxs-lookup"><span data-stu-id="424e0-111">Recommendation</span></span> | <span data-ttu-id="424e0-112">Popis</span><span class="sxs-lookup"><span data-stu-id="424e0-112">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="424e0-113">Povolení auditování a detekce hrozeb na SQL serverech</span><span class="sxs-lookup"><span data-stu-id="424e0-113">Enable auditing and threat detection on SQL servers</span></span>](security-center-enable-auditing-on-sql-servers.md) |<span data-ttu-id="424e0-114">Doporučuje zapnout auditování a zjišťování hrozeb pro servery Azure SQL (služba Azure SQL pouze; neobsahuje SQL běžících na virtuálních počítačích).</span><span class="sxs-lookup"><span data-stu-id="424e0-114">Recommends that you turn on auditing and threat detection for Azure SQL servers (Azure SQL service only; doesn't include SQL running on your virtual machines).</span></span> |
| [<span data-ttu-id="424e0-115">Povolení auditování a detekce hrozeb v databázích SQL</span><span class="sxs-lookup"><span data-stu-id="424e0-115">Enable auditing and threat detection on SQL databases</span></span>](security-center-enable-auditing-on-sql-databases.md) |<span data-ttu-id="424e0-116">Doporučuje zapnout auditování a zjišťování hrozeb pro databáze Azure SQL (služba Azure SQL pouze; neobsahuje SQL běžících na virtuálních počítačích).</span><span class="sxs-lookup"><span data-stu-id="424e0-116">Recommends that you turn on auditing and threat detection for Azure SQL databases (Azure SQL service only; doesn't include SQL running on your virtual machines).</span></span> |
| [<span data-ttu-id="424e0-117">Povolit transparentní šifrování dat v databázích SQL</span><span class="sxs-lookup"><span data-stu-id="424e0-117">Enable Transparent Data Encryption on SQL databases</span></span>](security-center-enable-transparent-data-encryption.md) |<span data-ttu-id="424e0-118">Doporučuje se, že povolíte šifrování pro databáze SQL (pouze služby Azure SQL).</span><span class="sxs-lookup"><span data-stu-id="424e0-118">Recommends that you enable encryption for SQL databases (Azure SQL service only).</span></span> |

## <a name="see-also"></a><span data-ttu-id="424e0-119">Viz také</span><span class="sxs-lookup"><span data-stu-id="424e0-119">See also</span></span>
<span data-ttu-id="424e0-120">Další informace o doporučení, které se vztahují na jiné typy prostředků Azure, naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="424e0-120">To learn more about recommendations that apply to other Azure resource types, see the following:</span></span>

* [<span data-ttu-id="424e0-121">Ochrana virtuálních počítačů v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="424e0-121">Protecting your virtual machines in Azure Security Center</span></span>](security-center-virtual-machine-recommendations.md)
* [<span data-ttu-id="424e0-122">Ochrana aplikací v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="424e0-122">Protecting your applications in Azure Security Center</span></span>](security-center-application-recommendations.md)
* [<span data-ttu-id="424e0-123">Ochrana sítě v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="424e0-123">Protecting your network in Azure Security Center</span></span>](security-center-network-recommendations.md)

<span data-ttu-id="424e0-124">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="424e0-124">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="424e0-125">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak konfigurovat zásady zabezpečení pro svá předplatná Azure a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="424e0-125">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="424e0-126">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.</span><span class="sxs-lookup"><span data-stu-id="424e0-126">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="424e0-127">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – Přečtěte si nejčastější dotazy k používání této služby.</span><span class="sxs-lookup"><span data-stu-id="424e0-127">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
