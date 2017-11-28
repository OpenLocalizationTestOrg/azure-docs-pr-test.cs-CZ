---
title: "aaaProtect osobních údajů v Microsoft Azure | Microsoft Docs"
description: "Nejprve článek v řadě články toohelp používáte Azure tooprotect osobní data"
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
ms.openlocfilehash: cbffd3872552cbd0f12539535898c41ecf7789e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-in-microsoft-azure"></a><span data-ttu-id="a705b-103">Ochrana osobních údajů v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="a705b-103">Protect personal data in Microsoft Azure</span></span>

<span data-ttu-id="a705b-104">Tento článek představuje řadu články, které vám pomohou používat zabezpečení Azure technologie a služby tooprotect osobní data.</span><span class="sxs-lookup"><span data-stu-id="a705b-104">This article introduces a series of articles that help you use Azure security technologies and services tooprotect personal data.</span></span> <span data-ttu-id="a705b-105">Toto je klíčovým požadavkem pro mnoho společností a dodržování předpisů a zásad správného řízení iniciativy odvětví.</span><span class="sxs-lookup"><span data-stu-id="a705b-105">This is a key requirement for many corporate and industry compliance and governance initiatives.</span></span> <span data-ttu-id="a705b-106">Hello scénář, problém prohlášení a společnosti cíle jsou zahrnuty sem.</span><span class="sxs-lookup"><span data-stu-id="a705b-106">hello scenario, problem statement and company goals are included here.</span></span>

## <a name="scenario-and-problem-statement"></a><span data-ttu-id="a705b-107">Scénáře a problému</span><span class="sxs-lookup"><span data-stu-id="a705b-107">Scenario and problem statement</span></span>

<span data-ttu-id="a705b-108">Velké výletních společnosti, centrálou ve Spojených státech amerických hello je rozšířit jeho itineráře toooffer operace v hello Středozemního, Jaderského a baltský moři, jakož i hello Britské ostrovy.</span><span class="sxs-lookup"><span data-stu-id="a705b-108">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, Adriatic, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="a705b-109">toosupport těchto úsilí získala menší výletních Víceřádkový na základě v Itálii Německo, Dánsko a hello Spojené království</span><span class="sxs-lookup"><span data-stu-id="a705b-109">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and hello U.K.</span></span>

<span data-ttu-id="a705b-110">Hello společnost používá Microsoft Azure toostore firemní data v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="a705b-110">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="a705b-111">To může zahrnovat zaměstnanců nebo zákazníků informace, jako:</span><span class="sxs-lookup"><span data-stu-id="a705b-111">This may include employee and/or customer information such as:</span></span>

- <span data-ttu-id="a705b-112">adresy</span><span class="sxs-lookup"><span data-stu-id="a705b-112">addresses</span></span>
- <span data-ttu-id="a705b-113">telefonní čísla</span><span class="sxs-lookup"><span data-stu-id="a705b-113">phone numbers</span></span>
- <span data-ttu-id="a705b-114">daň identifikační čísla</span><span class="sxs-lookup"><span data-stu-id="a705b-114">tax identification numbers</span></span>
- <span data-ttu-id="a705b-115">lékařské informace</span><span class="sxs-lookup"><span data-stu-id="a705b-115">medical information</span></span>
- <span data-ttu-id="a705b-116">informace o kreditní kartě</span><span class="sxs-lookup"><span data-stu-id="a705b-116">credit card information</span></span>

<span data-ttu-id="a705b-117">Hello společnosti musí ochrany osobních údajů hello dat zaměstnanců a zákazníků při vytváření přístupné toothose oddělení dat, které je třeba ji.</span><span class="sxs-lookup"><span data-stu-id="a705b-117">hello company must protect hello privacy of employee and customer data while making data accessible toothose departments that need it.</span></span> <span data-ttu-id="a705b-118">(například oddělení mzdy a rezervace)</span><span class="sxs-lookup"><span data-stu-id="a705b-118">(such as payroll and reservations departments)</span></span>

## <a name="company-goals"></a><span data-ttu-id="a705b-119">Cíle společnosti</span><span class="sxs-lookup"><span data-stu-id="a705b-119">Company goals</span></span> 

- <span data-ttu-id="a705b-120">Zdroje dat, které obsahují osobní data se šifrují, pokud umístěný do cloudového úložiště.</span><span class="sxs-lookup"><span data-stu-id="a705b-120">Data sources that contain personal data are encrypted when residing in cloud storage.</span></span>

- <span data-ttu-id="a705b-121">Osobní údaje, které je přenést z jednoho umístění tooanother během přenosu šifrovány.</span><span class="sxs-lookup"><span data-stu-id="a705b-121">Personal data that is transferred from one location tooanother is encrypted while in-transit.</span></span> <span data-ttu-id="a705b-122">Toto je hodnota true, pokud je hello data přechodu přes hello virtuální sítě nebo přes hello Internet mezi hello podnikovém datovém centru a hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="a705b-122">This is true if hello data is traveling across hello virtual network or across hello Internet between hello corporate datacenter and hello Azure cloud.</span></span>

- <span data-ttu-id="a705b-123">Důvěrnost a integrita osobních údajů je chráněný před neoprávněným přístupem správu silné identit a technologie pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="a705b-123">Confidentiality and integrity of personal data is protected from unauthorized access by strong identity management and access control technologies.</span></span>

- <span data-ttu-id="a705b-124">Osobní data je chráněný před přes porušení zabezpečení dat prostřednictvím monitorování ohrožení zabezpečení a hrozeb.</span><span class="sxs-lookup"><span data-stu-id="a705b-124">Personal data is protected from exposure via data breach via monitoring for vulnerabilities and threats.</span></span>

- <span data-ttu-id="a705b-125">Hello stav zabezpečení služby Azure, které uložení nebo na přenos osobních dat bude posouzeno tooidentify příležitosti toobetter chránit osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="a705b-125">hello security state of Azure services that store or transmit personal data is assessed tooidentify opportunities toobetter protect personal data.</span></span>

## <a name="data-protection-guidance"></a><span data-ttu-id="a705b-126">Pokyny pro ochranu dat</span><span class="sxs-lookup"><span data-stu-id="a705b-126">Data protection guidance</span></span>

<span data-ttu-id="a705b-127">Následující články Hello obsahovat technické tooguidance postupy, které vám pomohou dosáhnout cíle ochrany osobních údajů hello uvedené výše:</span><span class="sxs-lookup"><span data-stu-id="a705b-127">hello following articles contain technical how-tooguidance that will help you attain hello personal data protection goals listed above:</span></span>

- [<span data-ttu-id="a705b-128">Azure šifrovací technologie</span><span class="sxs-lookup"><span data-stu-id="a705b-128">Azure encryption technologies</span></span>](protect-personal-data-at-rest.md)

- [<span data-ttu-id="a705b-129">Azure šifrovací technologie</span><span class="sxs-lookup"><span data-stu-id="a705b-129">Azure encryption technologies</span></span>](protect-personal-data-in-transit-encryption.md)

- [<span data-ttu-id="a705b-130">Azure technologie identit a přístupu</span><span class="sxs-lookup"><span data-stu-id="a705b-130">Azure identity and access technologies</span></span>](protect-personal-data-identity-access-controls.md)

- [<span data-ttu-id="a705b-131">Technologie zabezpečení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="a705b-131">Azure network security technologies</span></span>](protect-personal-data-network-security.md)

- [<span data-ttu-id="a705b-132">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="a705b-132">Azure Security Center</span></span>](protect-personal-data-azure-security-center.md)



## <a name="next-steps"></a><span data-ttu-id="a705b-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a705b-133">Next steps</span></span>

- [<span data-ttu-id="a705b-134">Server s informacemi o zabezpečení Azure</span><span class="sxs-lookup"><span data-stu-id="a705b-134">Azure Security Information Site</span></span>](https://aka.ms/AzureSecInfo)

- [<span data-ttu-id="a705b-135">Centrum zabezpečení Microsoft</span><span class="sxs-lookup"><span data-stu-id="a705b-135">Microsoft Trust Center</span></span>](https://www.microsoft.com/TrustCenter/default.aspx)

- [<span data-ttu-id="a705b-136">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="a705b-136">Azure Security Center</span></span>](https://azure.microsoft.com/services/security-center/)

- [<span data-ttu-id="a705b-137">Blog týmu zabezpečení Azure</span><span class="sxs-lookup"><span data-stu-id="a705b-137">Azure Security Team Blog</span></span>](https://www.azuresecurityorg)

- [<span data-ttu-id="a705b-138">Azure.com Blog - zabezpečení</span><span class="sxs-lookup"><span data-stu-id="a705b-138">Azure.com Blog - Security</span></span>](https://azure.microsoft.com/blog/topics/security/)
