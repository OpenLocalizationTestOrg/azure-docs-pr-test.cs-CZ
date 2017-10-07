---
title: "zabezpečení aaaAzure osvědčených postupů a vzorů | Microsoft Docs"
description: "Hello článek poskytuje základní informace o osvědčených postupech zabezpečení Azure a vzory a kurátorované seznam osvědčené postupy zabezpečení pro různé prostředky Azure."
services: azure-security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 1cbbf8dc-ea94-4a7e-8fa0-c2cb198956c5
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2017
ms.author: terrylan
ms.openlocfilehash: eaaa9457faa1d5906275eb1fd8988d4d4aad101a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-best-practices-and-patterns"></a><span data-ttu-id="c2878-103">Osvědčené postupy zabezpečení Azure a vzory</span><span class="sxs-lookup"><span data-stu-id="c2878-103">Azure security best practices and patterns</span></span>
<span data-ttu-id="c2878-104">V současné době máme hello následující osvědčené postupy a vzory články zabezpečení Azure.</span><span class="sxs-lookup"><span data-stu-id="c2878-104">We currently have hello following Azure security best practices and patterns articles.</span></span> <span data-ttu-id="c2878-105">Ujistěte se, že toovisit tento web pravidelně aktualizuje toosee tooour rostoucí seznam zabezpečení Azure osvědčených postupů a vzorů:</span><span class="sxs-lookup"><span data-stu-id="c2878-105">Make sure toovisit this site periodically toosee updates tooour growing list of Azure security best practices and patterns:</span></span>  

* [<span data-ttu-id="c2878-106">Osvědčené postupy zabezpečení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="c2878-106">Azure network security best practices</span></span>](azure-security-network-security-best-practices.md)
* [<span data-ttu-id="c2878-107">Zabezpečení dat Azure a šifrování osvědčené postupy</span><span class="sxs-lookup"><span data-stu-id="c2878-107">Azure data security and encryption best practices</span></span>](azure-security-data-encryption-best-practices.md)
* [<span data-ttu-id="c2878-108">Osvědčené postupy zabezpečení řízení správy identit a přístupu</span><span class="sxs-lookup"><span data-stu-id="c2878-108">Identity management and access control security best practices</span></span>](azure-security-identity-management-best-practices.md)
* [<span data-ttu-id="c2878-109">Osvědčené postupy pro zabezpečení Internetu věcí</span><span class="sxs-lookup"><span data-stu-id="c2878-109">Internet of Things security best practices</span></span>](azure-security-iot-best-practices.md)
* <span data-ttu-id="c2878-110">[Osvědčené postupy zabezpečení azure IaaS] (azure iaas.md zabezpečení)</span><span class="sxs-lookup"><span data-stu-id="c2878-110">[Azure IaaS Security Best Practices] (azure-security-iaas.md)</span></span>
* [<span data-ttu-id="c2878-111">Osvědčené postupy zabezpečení Azure hranic</span><span class="sxs-lookup"><span data-stu-id="c2878-111">Azure boundary security best practices</span></span>](../best-practices-network-security.md)
* [<span data-ttu-id="c2878-112">Implementace zabezpečené hybridní síťové architektury v Azure</span><span class="sxs-lookup"><span data-stu-id="c2878-112">Implementing a secure hybrid network architecture in Azure</span></span>](../guidance/guidance-iaas-ra-secure-vnet-hybrid.md)
* <span data-ttu-id="c2878-113">[Azure PaaS osvědčených postupů] (https://docs.microsoft.com/en-us/azure/security/security-paas-deployments)</span><span class="sxs-lookup"><span data-stu-id="c2878-113">[Azure PaaS Best Practices] (https://docs.microsoft.com/en-us/azure/security/security-paas-deployments)</span></span>

<span data-ttu-id="c2878-114">Azure poskytuje zabezpečené platformy, na které můžete sestavit vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="c2878-114">Azure provides a secure platform on which you can build your solutions.</span></span> <span data-ttu-id="c2878-115">Poskytujeme také službách a technologiích toomake řešení v Azure bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="c2878-115">We also provide services and technologies toomake your solutions on Azure more secure.</span></span> <span data-ttu-id="c2878-116">Z důvodu hello mnoho možností k dispozici tooyou, řada z vás mají znělá zájem o co Microsoft doporučuje, jako nejlepší postupy a vzory pro zlepšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c2878-116">Because of hello many options available tooyou, many of you have voiced an interest in what Microsoft recommends as best practices and patterns for improving security.</span></span>

<span data-ttu-id="c2878-117">Pochopení váš zájem jsme vytvořili kolekci dokumentů, které popisují věcí, které můžete provést danou hello správné kontextu zabezpečení hello tooimprove Azure nasazení.</span><span class="sxs-lookup"><span data-stu-id="c2878-117">We understand your interest and have created a collection of documents that describe things you can do, given hello right context, tooimprove hello security of Azure deployments.</span></span>

<span data-ttu-id="c2878-118">V těchto osvědčené postupy a vzory články probereme kolekce osvědčených postupů a vzorů užitečné pro konkrétní témata.</span><span class="sxs-lookup"><span data-stu-id="c2878-118">In these best practices and patterns articles, we discuss a collection of best practices and useful patterns for specific topics.</span></span> <span data-ttu-id="c2878-119">Tyto doporučené postupy a vzory jsou odvozené z našich zkušeností s těmito technologiemi a hello prostředí zákazníků, jako sami.</span><span class="sxs-lookup"><span data-stu-id="c2878-119">These best practices and patterns are derived from our experiences with these technologies and hello experiences of customers like yourself.</span></span>

<span data-ttu-id="c2878-120">Pro každý osvědčený postup se snažíme tooexplain:</span><span class="sxs-lookup"><span data-stu-id="c2878-120">For each best practice we strive tooexplain:</span></span>

* <span data-ttu-id="c2878-121">Jaké hello osvědčeným postupem je</span><span class="sxs-lookup"><span data-stu-id="c2878-121">What hello best practice is</span></span>
* <span data-ttu-id="c2878-122">Proč chcete tooenable, že osvědčeným postupem</span><span class="sxs-lookup"><span data-stu-id="c2878-122">Why you want tooenable that best practice</span></span>
* <span data-ttu-id="c2878-123">Pokud selže tooenable hello osvědčený postup, co mohou být výsledek hello</span><span class="sxs-lookup"><span data-stu-id="c2878-123">What might be hello result if you fail tooenable hello best practice</span></span>
* <span data-ttu-id="c2878-124">Osvědčeným postupem toohello možné alternativy</span><span class="sxs-lookup"><span data-stu-id="c2878-124">Possible alternatives toohello best practice</span></span>
* <span data-ttu-id="c2878-125">Jak můžete získat informace tooenable hello osvědčený postup</span><span class="sxs-lookup"><span data-stu-id="c2878-125">How you can learn tooenable hello best practice</span></span>

<span data-ttu-id="c2878-126">Těšíme tooincluding mnoho další články o zabezpečení Azure architektura a osvědčených postupech.</span><span class="sxs-lookup"><span data-stu-id="c2878-126">We look forward tooincluding many more articles on Azure security architecture and best practices.</span></span> <span data-ttu-id="c2878-127">Pokud jsou témata, které chcete nám tooinclude, dejte nám vědět, v oblasti hello diskuzi na hello dolní části této stránky.</span><span class="sxs-lookup"><span data-stu-id="c2878-127">If there are topics that you'd like us tooinclude, let us know in hello discussion area at hello bottom of this page.</span></span>
