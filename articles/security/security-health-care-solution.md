---
title: "Praktické příručka k návrhu bezpečné zdravotní péče řešení v Azure | Microsoft Docs"
description: " Tento článek pomáhá pochopit, jak zlepšit zabezpečení vašich zdravotní péče řešení pomocí služby Azure a funkce, které nakonfigurujete. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 7e5b082d-dc9c-4d4f-b3f1-75edcdafbd8f
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/07/2017
ms.author: terrylan
ms.openlocfilehash: 34ded89eb7fe005be2341f96e5b883ec73d9e0a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="a-practical-guide-to-designing-secure-health-care-solutions-in-azure"></a><span data-ttu-id="f52b0-103">Praktické příručka k návrhu bezpečné zdravotní péče řešení v Azure</span><span class="sxs-lookup"><span data-stu-id="f52b0-103">A practical guide to designing secure health care solutions in Azure</span></span>
<span data-ttu-id="f52b0-104">Startupy stavu odvětví, systémových integrátorech (si), nezávislí dodavatelé softwaru (ISV) a zdravotnická zvažování přesunu do Azure hledáte pokyny, které pomáhá je začlenit ovládacích prvků zabezpečení ke splnění svých povinností dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="f52b0-104">Health industry startups, system integrators (SIs), independent software vendors (ISVs), and healthcare organizations considering a move to Azure are looking for guidance that helps them incorporate security controls to meet their compliance obligations.</span></span>

<span data-ttu-id="f52b0-105">[Praktické Průvodce pro navrhování zabezpečení zdravotnictví řešení v Microsoft Azure](https://aka.ms/azureindustrysecurity) vám pomůže pochopit, jak zlepšit zabezpečení pro vaše řešení pomocí služby Azure a funkce, které můžete nakonfigurovat podle svých požadavků.</span><span class="sxs-lookup"><span data-stu-id="f52b0-105">[A Practical Guide to Designing Secure Health Care Solutions in Microsoft Azure](https://aka.ms/azureindustrysecurity) helps you understand how you can improve security for your solutions by using the Azure services and features that you can configure based on your requirements.</span></span>
<span data-ttu-id="f52b0-106">Obsah je rozdělené do tří hlavních částech:</span><span class="sxs-lookup"><span data-stu-id="f52b0-106">The content is divided into three major sections:</span></span>

1. <span data-ttu-id="f52b0-107">Aspekty pokyny pro používání cloudové technologie, včetně řízení rizik sdílené odpovědnosti, vytvoření informačního zabezpečení správy systému, odvětví principy a místními předpisy a zřízení standardní pracovní postupy.</span><span class="sxs-lookup"><span data-stu-id="f52b0-107">Considerations guidance for using cloud technology, including risk management, shared responsibility, establishing an information security management system, understanding industry and local regulations, and establishing standard operating procedures.</span></span>
2. <span data-ttu-id="f52b0-108">Zásady zabezpečení klíčů, které jsou obě zarovnán standardní správu zabezpečení Standardní informace, jako je například ISO 27001 a standardní vývojové procesy, jako je například společnosti Microsoft životního cyklu SDL (Security Development).</span><span class="sxs-lookup"><span data-stu-id="f52b0-108">Key security principles that are both aligned to a standard information security management standard, such as ISO 27001, and standard development processes, such as Microsoft’s Security Development Lifecycle (SDL).</span></span>
3. <span data-ttu-id="f52b0-109">Použití klíče zásady, které je případy použití podle demonstraci zarovnání z hlediska architekt řešení, kde je zarovnán požadavky pro řešení správy informace zabezpečení Standardní.</span><span class="sxs-lookup"><span data-stu-id="f52b0-109">Applying the key principles to use cases by demonstrating alignment from a solution architect perspective, where requirements for the solutions are aligned to the information security management standard.</span></span>

<span data-ttu-id="f52b0-110">Věříme, že zjistíte, [A praktické Průvodce pro navrhování řešení zabezpečení zdravotnictví](https://aka.ms/azureindustrysecurity) užitečné a pokud máte jakékoli dotazy nebo návrhy, dejte nám vědět, protože komentář níže.</span><span class="sxs-lookup"><span data-stu-id="f52b0-110">We hope you find [A Practical Guide to Designing Secure Health Care Solutions](https://aka.ms/azureindustrysecurity) helpful and if you have any questions or suggestions, let us know by leaving a comment below.</span></span>
