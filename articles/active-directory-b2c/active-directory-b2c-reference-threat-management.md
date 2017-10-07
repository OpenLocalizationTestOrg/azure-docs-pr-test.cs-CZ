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
ms.openlocfilehash: 60bc0cc392b332cc4e9741ddb97dfa58e68ed420
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-threat-management"></a><span data-ttu-id="3580a-103">Azure Active Directory B2C: Hrozby správy</span><span class="sxs-lookup"><span data-stu-id="3580a-103">Azure Active Directory B2C: Threat management</span></span>

<span data-ttu-id="3580a-104">Správa hrozeb zahrnuje plánování pro ochranu před útoky na systém a sítě.</span><span class="sxs-lookup"><span data-stu-id="3580a-104">Threat management includes planning for protection from attacks against your system and networks.</span></span> <span data-ttu-id="3580a-105">Útoky DOS může aby prostředky k dispozici toointended uživatele.</span><span class="sxs-lookup"><span data-stu-id="3580a-105">Denial-of-service attacks might make resources unavailable toointended users.</span></span> <span data-ttu-id="3580a-106">Heslo útoky realizace toounauthorized přístup tooresources.</span><span class="sxs-lookup"><span data-stu-id="3580a-106">Password attacks lead toounauthorized access tooresources.</span></span> <span data-ttu-id="3580a-107">Azure Active Directory B2C (Azure AD B2C) obsahuje integrované funkce, které vám umožní chránit vaše data před tyto hrozby několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="3580a-107">Azure Active Directory B2C (Azure AD B2C) has built-in features that can help you protect your data against these threats in multiple ways.</span></span>

## <a name="denial-of-service-attacks"></a><span data-ttu-id="3580a-108">Útoky DoS</span><span class="sxs-lookup"><span data-stu-id="3580a-108">Denial-of-service attacks</span></span>

<span data-ttu-id="3580a-109">Azure AD B2C používá detekce a zmírnění techniky, jako jsou soubory cookie SYN a rychlost a připojení tooprotect omezení, které jsou základní prostředky před útoky DOS.</span><span class="sxs-lookup"><span data-stu-id="3580a-109">Azure AD B2C uses detection and mitigation techniques like SYN cookies, and rate and connection limits tooprotect underlying resources against denial-of-service attacks.</span></span>

## <a name="password-attacks"></a><span data-ttu-id="3580a-110">Útoky heslo</span><span class="sxs-lookup"><span data-stu-id="3580a-110">Password attacks</span></span>

<span data-ttu-id="3580a-111">Zmírnění techniky Azure AD B2C má také nastavené pro útoky heslo.</span><span class="sxs-lookup"><span data-stu-id="3580a-111">Azure AD B2C also has mitigation techniques in place for password attacks.</span></span> <span data-ttu-id="3580a-112">Zmírnění dopadů zahrnuje útoky hrubou silou heslo a slovníkovým útokům heslo.</span><span class="sxs-lookup"><span data-stu-id="3580a-112">Mitigation includes brute-force password attacks and dictionary password attacks.</span></span> <span data-ttu-id="3580a-113">Hesla, které jsou nastavené uživatelé jsou požadované toobe přiměřeně komplexní.</span><span class="sxs-lookup"><span data-stu-id="3580a-113">Passwords that are set by users are required toobe reasonably complex.</span></span> <span data-ttu-id="3580a-114">Pomocí různých signály analyzuje Azure AD B2C hello integritu požadavků.</span><span class="sxs-lookup"><span data-stu-id="3580a-114">By using various signals, Azure AD B2C analyzes hello integrity of requests.</span></span> <span data-ttu-id="3580a-115">Azure AD B2C je navržený tak, toointelligently rozlišit před hackery a botnetů příslušným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="3580a-115">Azure AD B2C is designed toointelligently differentiate intended users from hackers and botnets.</span></span> <span data-ttu-id="3580a-116">Azure AD B2C poskytuje sofistikované strategie toolock účty podle hello hesel zadali v hello pravděpodobnost útoku.</span><span class="sxs-lookup"><span data-stu-id="3580a-116">Azure AD B2C provides a sophisticated strategy toolock accounts based on hello passwords entered, in hello likelihood of an attack.</span></span>

<span data-ttu-id="3580a-117">Další informace najdete v článku hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span><span class="sxs-lookup"><span data-stu-id="3580a-117">For more information, visit hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span></span>
