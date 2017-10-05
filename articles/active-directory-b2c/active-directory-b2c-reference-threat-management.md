---
title: "Azure Active Directory B2C: Hrozby správy | Microsoft Docs"
description: "Další informace o zjišťování a ke zmírnění techniky pro útoky DOS a útoky heslo v Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: Ajith Alexander
editor: 
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2016
ms.author: 
ms.openlocfilehash: 9472cb01eb713e297053727b1a314293574bb657
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-threat-management"></a><span data-ttu-id="1826a-103">Azure Active Directory B2C: Hrozby správy</span><span class="sxs-lookup"><span data-stu-id="1826a-103">Azure Active Directory B2C: Threat management</span></span>

<span data-ttu-id="1826a-104">Správa hrozeb zahrnuje plánování pro ochranu před útoky na systém a sítě.</span><span class="sxs-lookup"><span data-stu-id="1826a-104">Threat management includes planning for protection from attacks against your system and networks.</span></span> <span data-ttu-id="1826a-105">Útoky DoS provést prostředky k dispozici příslušným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="1826a-105">Denial-of-service attacks might make resources unavailable to intended users.</span></span> <span data-ttu-id="1826a-106">Heslo útoky vést k neoprávněnému přístupu k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="1826a-106">Password attacks lead to unauthorized access to resources.</span></span> <span data-ttu-id="1826a-107">Azure Active Directory B2C (Azure AD B2C) obsahuje integrované funkce, které vám umožní chránit vaše data před tyto hrozby několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="1826a-107">Azure Active Directory B2C (Azure AD B2C) has built-in features that can help you protect your data against these threats in multiple ways.</span></span>

## <a name="denial-of-service-attacks"></a><span data-ttu-id="1826a-108">Útoky DoS</span><span class="sxs-lookup"><span data-stu-id="1826a-108">Denial-of-service attacks</span></span>

<span data-ttu-id="1826a-109">Azure AD B2C používá detekce a zmírnění techniky, jako třeba SYN soubory cookie a omezení rychlost a připojení k ochraně před útoky DoS příslušných prostředků.</span><span class="sxs-lookup"><span data-stu-id="1826a-109">Azure AD B2C uses detection and mitigation techniques like SYN cookies, and rate and connection limits to protect underlying resources against denial-of-service attacks.</span></span>

## <a name="password-attacks"></a><span data-ttu-id="1826a-110">Útoky heslo</span><span class="sxs-lookup"><span data-stu-id="1826a-110">Password attacks</span></span>

<span data-ttu-id="1826a-111">Zmírnění techniky Azure AD B2C má také nastavené pro útoky heslo.</span><span class="sxs-lookup"><span data-stu-id="1826a-111">Azure AD B2C also has mitigation techniques in place for password attacks.</span></span> <span data-ttu-id="1826a-112">Zmírnění dopadů zahrnuje útoky hrubou silou heslo a slovníkovým útokům heslo.</span><span class="sxs-lookup"><span data-stu-id="1826a-112">Mitigation includes brute-force password attacks and dictionary password attacks.</span></span> <span data-ttu-id="1826a-113">Hesla, které jsou nastavené uživatelé musí být dostatečně komplexní.</span><span class="sxs-lookup"><span data-stu-id="1826a-113">Passwords that are set by users are required to be reasonably complex.</span></span> <span data-ttu-id="1826a-114">Pomocí různých signály analyzuje Azure AD B2C integritu požadavků.</span><span class="sxs-lookup"><span data-stu-id="1826a-114">By using various signals, Azure AD B2C analyzes the integrity of requests.</span></span> <span data-ttu-id="1826a-115">Azure AD B2C je určena pro inteligentně před hackery a botnetů rozlišit příslušným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="1826a-115">Azure AD B2C is designed to intelligently differentiate intended users from hackers and botnets.</span></span> <span data-ttu-id="1826a-116">Azure AD B2C poskytuje sofistikované strategie pro lock účty podle zadaná v pravděpodobnost útoku hesla.</span><span class="sxs-lookup"><span data-stu-id="1826a-116">Azure AD B2C provides a sophisticated strategy to lock accounts based on the passwords entered, in the likelihood of an attack.</span></span>

<span data-ttu-id="1826a-117">Další informace najdete v článku [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span><span class="sxs-lookup"><span data-stu-id="1826a-117">For more information, visit the [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span></span>
