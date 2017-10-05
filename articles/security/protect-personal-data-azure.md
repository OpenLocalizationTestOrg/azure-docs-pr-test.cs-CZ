---
title: "Ochrana osobních údajů v Microsoft Azure | Microsoft Docs"
description: "První článek v řadě článků vám pomohou používat Azure k ochraně osobních údajů"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: dfb046374397c8a19672ce6b67741903fff6e178
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="protect-personal-data-in-microsoft-azure"></a><span data-ttu-id="b9bda-103">Ochrana osobních údajů v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="b9bda-103">Protect personal data in Microsoft Azure</span></span>

<span data-ttu-id="b9bda-104">Tento článek představuje řadu články, které vám pomohou používat Azure bezpečnostních technologií a služeb na ochranu osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="b9bda-104">This article introduces a series of articles that help you use Azure security technologies and services to protect personal data.</span></span> <span data-ttu-id="b9bda-105">Toto je klíčovým požadavkem pro mnoho společností a dodržování předpisů a zásad správného řízení iniciativy odvětví.</span><span class="sxs-lookup"><span data-stu-id="b9bda-105">This is a key requirement for many corporate and industry compliance and governance initiatives.</span></span> <span data-ttu-id="b9bda-106">Scénář, problém prohlášení a společnosti cíle jsou zahrnuté v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="b9bda-106">The scenario, problem statement and company goals are included here.</span></span>

## <a name="scenario-and-problem-statement"></a><span data-ttu-id="b9bda-107">Scénáře a problému</span><span class="sxs-lookup"><span data-stu-id="b9bda-107">Scenario and problem statement</span></span>

<span data-ttu-id="b9bda-108">Velké výletních společnosti, centrálou ve Spojených státech amerických, je rozšířit jeho operací a nabídnout itineráře v Středomoří, Jaderského a baltský moři, jakož i Britské ostrovy.</span><span class="sxs-lookup"><span data-stu-id="b9bda-108">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, Adriatic, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="b9bda-109">Pro podporu těchto úsilí, získala menší výletních Víceřádkový na základě v Itálii, Německo, Dánsko a Spojeném království</span><span class="sxs-lookup"><span data-stu-id="b9bda-109">To support those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and the U.K.</span></span>

<span data-ttu-id="b9bda-110">Společnost používá Microsoft Azure k ukládání firemních dat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="b9bda-110">The company uses Microsoft Azure to store corporate data in the cloud.</span></span> <span data-ttu-id="b9bda-111">To může zahrnovat zaměstnanců nebo zákazníků informace, jako:</span><span class="sxs-lookup"><span data-stu-id="b9bda-111">This may include employee and/or customer information such as:</span></span>

- <span data-ttu-id="b9bda-112">adresy</span><span class="sxs-lookup"><span data-stu-id="b9bda-112">addresses</span></span>
- <span data-ttu-id="b9bda-113">telefonní čísla</span><span class="sxs-lookup"><span data-stu-id="b9bda-113">phone numbers</span></span>
- <span data-ttu-id="b9bda-114">daň identifikační čísla</span><span class="sxs-lookup"><span data-stu-id="b9bda-114">tax identification numbers</span></span>
- <span data-ttu-id="b9bda-115">lékařské informace</span><span class="sxs-lookup"><span data-stu-id="b9bda-115">medical information</span></span>
- <span data-ttu-id="b9bda-116">informace o kreditní kartě</span><span class="sxs-lookup"><span data-stu-id="b9bda-116">credit card information</span></span>

<span data-ttu-id="b9bda-117">Společnosti musí při zpřístupnění dat pro tyto oddělení, které je třeba jej chránit ochrana osobních údajů zaměstnanců a zákazníků.</span><span class="sxs-lookup"><span data-stu-id="b9bda-117">The company must protect the privacy of employee and customer data while making data accessible to those departments that need it.</span></span> <span data-ttu-id="b9bda-118">(například oddělení mzdy a rezervace)</span><span class="sxs-lookup"><span data-stu-id="b9bda-118">(such as payroll and reservations departments)</span></span>

## <a name="company-goals"></a><span data-ttu-id="b9bda-119">Cíle společnosti</span><span class="sxs-lookup"><span data-stu-id="b9bda-119">Company goals</span></span> 

- <span data-ttu-id="b9bda-120">Zdroje dat, které obsahují osobní data se šifrují, pokud umístěný do cloudového úložiště.</span><span class="sxs-lookup"><span data-stu-id="b9bda-120">Data sources that contain personal data are encrypted when residing in cloud storage.</span></span>

- <span data-ttu-id="b9bda-121">Během přenosu šifrovány osobní data, která se přenáší z jednoho umístění do druhého.</span><span class="sxs-lookup"><span data-stu-id="b9bda-121">Personal data that is transferred from one location to another is encrypted while in-transit.</span></span> <span data-ttu-id="b9bda-122">Toto je hodnota true, pokud se data mezi podnikovém datovém centru a cloudu Azure cestě přes virtuální síť nebo přes Internet.</span><span class="sxs-lookup"><span data-stu-id="b9bda-122">This is true if the data is traveling across the virtual network or across the Internet between the corporate datacenter and the Azure cloud.</span></span>

- <span data-ttu-id="b9bda-123">Důvěrnost a integrita osobních údajů je chráněný před neoprávněným přístupem správu silné identit a technologie pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="b9bda-123">Confidentiality and integrity of personal data is protected from unauthorized access by strong identity management and access control technologies.</span></span>

- <span data-ttu-id="b9bda-124">Osobní data je chráněný před přes porušení zabezpečení dat prostřednictvím monitorování ohrožení zabezpečení a hrozeb.</span><span class="sxs-lookup"><span data-stu-id="b9bda-124">Personal data is protected from exposure via data breach via monitoring for vulnerabilities and threats.</span></span>

- <span data-ttu-id="b9bda-125">Stav zabezpečení služby Azure, které uložení nebo na přenos osobních dat bude posouzeno identifikovat příležitosti k lepší ochraně osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="b9bda-125">The security state of Azure services that store or transmit personal data is assessed to identify opportunities to better protect personal data.</span></span>

## <a name="data-protection-guidance"></a><span data-ttu-id="b9bda-126">Pokyny pro ochranu dat</span><span class="sxs-lookup"><span data-stu-id="b9bda-126">Data protection guidance</span></span>

<span data-ttu-id="b9bda-127">V následujících článcích obsahovat technické postupy pokyny, které vám pomohou dosáhnout cíle ochrany osobních údajů uvedených výše:</span><span class="sxs-lookup"><span data-stu-id="b9bda-127">The following articles contain technical how-to guidance that will help you attain the personal data protection goals listed above:</span></span>

- [<span data-ttu-id="b9bda-128">Azure šifrovací technologie</span><span class="sxs-lookup"><span data-stu-id="b9bda-128">Azure encryption technologies</span></span>](protect-personal-data-at-rest.md)

- [<span data-ttu-id="b9bda-129">Azure šifrovací technologie</span><span class="sxs-lookup"><span data-stu-id="b9bda-129">Azure encryption technologies</span></span>](protect-personal-data-in-transit-encryption.md)

- [<span data-ttu-id="b9bda-130">Azure technologie identit a přístupu</span><span class="sxs-lookup"><span data-stu-id="b9bda-130">Azure identity and access technologies</span></span>](protect-personal-data-identity-access-controls.md)

- [<span data-ttu-id="b9bda-131">Technologie zabezpečení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="b9bda-131">Azure network security technologies</span></span>](protect-personal-data-network-security.md)

- [<span data-ttu-id="b9bda-132">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="b9bda-132">Azure Security Center</span></span>](protect-personal-data-azure-security-center.md)



## <a name="next-steps"></a><span data-ttu-id="b9bda-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9bda-133">Next steps</span></span>

- [<span data-ttu-id="b9bda-134">Server s informacemi o zabezpečení Azure</span><span class="sxs-lookup"><span data-stu-id="b9bda-134">Azure Security Information Site</span></span>](https://aka.ms/AzureSecInfo)

- [<span data-ttu-id="b9bda-135">Centrum zabezpečení Microsoft</span><span class="sxs-lookup"><span data-stu-id="b9bda-135">Microsoft Trust Center</span></span>](https://www.microsoft.com/TrustCenter/default.aspx)

- [<span data-ttu-id="b9bda-136">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="b9bda-136">Azure Security Center</span></span>](https://azure.microsoft.com/services/security-center/)

- [<span data-ttu-id="b9bda-137">Blog týmu zabezpečení Azure</span><span class="sxs-lookup"><span data-stu-id="b9bda-137">Azure Security Team Blog</span></span>](https://www.azuresecurityorg)

- [<span data-ttu-id="b9bda-138">Azure.com Blog - zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b9bda-138">Azure.com Blog - Security</span></span>](https://azure.microsoft.com/blog/topics/security/)
