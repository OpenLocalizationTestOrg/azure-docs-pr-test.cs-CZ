---
title: "aaaProtecting Azure SQL služby a data v Azure Security Center | Microsoft Docs"
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
ms.openlocfilehash: 75d782d3c2418f9645139e4cd6ecb7765c488f91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a><span data-ttu-id="4c384-103">Ochrana služby Azure SQL a data v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4c384-103">Protecting Azure SQL service and data in Azure Security Center</span></span>
<span data-ttu-id="4c384-104">Azure Security Center analyzuje stav zabezpečení hello vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="4c384-104">Azure Security Center analyzes hello security state of your Azure resources.</span></span> <span data-ttu-id="4c384-105">Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří se doporučení, která vás provede procesem hello konfigurace hello potřebné ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="4c384-105">When Security Center identifies potential security vulnerabilities, it creates recommendations that guide you through hello process of configuring hello needed controls.</span></span>  <span data-ttu-id="4c384-106">Doporučení se týkají tooAzure typy prostředků: virtuální počítače (VM), sítě, SQL a datům a aplikacím.</span><span class="sxs-lookup"><span data-stu-id="4c384-106">Recommendations apply tooAzure resource types: virtual machines (VMs), networking, SQL and data, and applications.</span></span>

<span data-ttu-id="4c384-107">Tento článek se zaměřuje na doporučení, které se vztahují tooAzure SQL služby a data.</span><span class="sxs-lookup"><span data-stu-id="4c384-107">This article addresses recommendations that apply tooAzure SQL service and data.</span></span> <span data-ttu-id="4c384-108">Doporučení center kolem povolení auditování pro servery Azure SQL a databáze, povolení šifrování u databází SQL a povolení šifrování vašeho účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="4c384-108">Recommendations center around enabling auditing for Azure SQL servers and databases, enabling encryption for SQL databases, and enabling encryption of your Azure storage account.</span></span>  <span data-ttu-id="4c384-109">Použití hello tabulce jako referenční toohelp porozumíte hello k dispozici SQL služby a data doporučení a co každé z nich dělá, pokud ji použijete.</span><span class="sxs-lookup"><span data-stu-id="4c384-109">Use hello table below as a reference toohelp you understand hello available SQL service and data recommendations and what each one does if you apply it.</span></span>

## <a name="available-sql-service-and-data-recommendations"></a><span data-ttu-id="4c384-110">K dispozici doporučení služby a dat SQL</span><span class="sxs-lookup"><span data-stu-id="4c384-110">Available SQL service and data recommendations</span></span>
| <span data-ttu-id="4c384-111">Doporučení</span><span class="sxs-lookup"><span data-stu-id="4c384-111">Recommendation</span></span> | <span data-ttu-id="4c384-112">Popis</span><span class="sxs-lookup"><span data-stu-id="4c384-112">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="4c384-113">Povolení auditování a detekce hrozeb na SQL serverech</span><span class="sxs-lookup"><span data-stu-id="4c384-113">Enable auditing and threat detection on SQL servers</span></span>](security-center-enable-auditing-on-sql-servers.md) |<span data-ttu-id="4c384-114">Doporučuje zapnout auditování a zjišťování hrozeb pro servery Azure SQL (služba Azure SQL pouze; neobsahuje SQL běžících na virtuálních počítačích).</span><span class="sxs-lookup"><span data-stu-id="4c384-114">Recommends that you turn on auditing and threat detection for Azure SQL servers (Azure SQL service only; doesn't include SQL running on your virtual machines).</span></span> |
| [<span data-ttu-id="4c384-115">Povolení auditování a detekce hrozeb v databázích SQL</span><span class="sxs-lookup"><span data-stu-id="4c384-115">Enable auditing and threat detection on SQL databases</span></span>](security-center-enable-auditing-on-sql-databases.md) |<span data-ttu-id="4c384-116">Doporučuje zapnout auditování a zjišťování hrozeb pro databáze Azure SQL (služba Azure SQL pouze; neobsahuje SQL běžících na virtuálních počítačích).</span><span class="sxs-lookup"><span data-stu-id="4c384-116">Recommends that you turn on auditing and threat detection for Azure SQL databases (Azure SQL service only; doesn't include SQL running on your virtual machines).</span></span> |
| [<span data-ttu-id="4c384-117">Povolit transparentní šifrování dat v databázích SQL</span><span class="sxs-lookup"><span data-stu-id="4c384-117">Enable Transparent Data Encryption on SQL databases</span></span>](security-center-enable-transparent-data-encryption.md) |<span data-ttu-id="4c384-118">Doporučuje se, že povolíte šifrování pro databáze SQL (pouze služby Azure SQL).</span><span class="sxs-lookup"><span data-stu-id="4c384-118">Recommends that you enable encryption for SQL databases (Azure SQL service only).</span></span> |

## <a name="see-also"></a><span data-ttu-id="4c384-119">Viz také</span><span class="sxs-lookup"><span data-stu-id="4c384-119">See also</span></span>
<span data-ttu-id="4c384-120">toolearn Další informace o doporučení, které se vztahují tooother typy prostředků Azure, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="4c384-120">toolearn more about recommendations that apply tooother Azure resource types, see hello following:</span></span>

* [<span data-ttu-id="4c384-121">Ochrana virtuálních počítačů v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4c384-121">Protecting your virtual machines in Azure Security Center</span></span>](security-center-virtual-machine-recommendations.md)
* [<span data-ttu-id="4c384-122">Ochrana aplikací v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4c384-122">Protecting your applications in Azure Security Center</span></span>](security-center-application-recommendations.md)
* [<span data-ttu-id="4c384-123">Ochrana sítě v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4c384-123">Protecting your network in Azure Security Center</span></span>](security-center-network-recommendations.md)

<span data-ttu-id="4c384-124">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="4c384-124">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="4c384-125">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="4c384-125">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="4c384-126">[Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4c384-126">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="4c384-127">[Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="4c384-127">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
