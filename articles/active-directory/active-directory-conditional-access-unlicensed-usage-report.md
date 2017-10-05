---
title: "Zprávu nelicencovaná využití | Microsoft Docs"
description: "Nelicencovaného využití sestava pomáhá identifikovat uživatelé bez licence, které používají placené funkce Azure AD."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: c0b4f455f067e825362bdecc02ea62d7984f0d31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="unlicensed-usage-report"></a><span data-ttu-id="ff309-103">Zprávu nelicencovaná využití</span><span class="sxs-lookup"><span data-stu-id="ff309-103">Unlicensed usage report</span></span>
<span data-ttu-id="ff309-104">Nelicencovaného využití sestava pomáhá identifikovat uživatelé bez licence, které používají placené funkce Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff309-104">The unlicensed usage report helps you identify unlicensed users that are using paid Azure AD features.</span></span> <span data-ttu-id="ff309-105">To umožňuje, aby lépe použijete licencí, které jste zakoupili a k identifikaci víte, že pokud potřebujete další licence.</span><span class="sxs-lookup"><span data-stu-id="ff309-105">This allows you to make better use of licenses that you have purchased and to identify you know when you may need additional licenses.</span></span> 

<span data-ttu-id="ff309-106">Tato sestava zobrazuje využití aktivní placené funkce za posledních 30 dní.</span><span class="sxs-lookup"><span data-stu-id="ff309-106">The report shows active usage of the paid features in the last 30 days.</span></span> 

## <a name="report-structure"></a><span data-ttu-id="ff309-107">Struktura sestavy</span><span class="sxs-lookup"><span data-stu-id="ff309-107">Report structure</span></span>
| <span data-ttu-id="ff309-108">Název sloupce</span><span class="sxs-lookup"><span data-stu-id="ff309-108">Column name</span></span> | <span data-ttu-id="ff309-109">Popis</span><span class="sxs-lookup"><span data-stu-id="ff309-109">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ff309-110">Nelicencovaného uživatele</span><span class="sxs-lookup"><span data-stu-id="ff309-110">Unlicensed User</span></span> |<span data-ttu-id="ff309-111">Jméno uživatele</span><span class="sxs-lookup"><span data-stu-id="ff309-111">Name of the user</span></span> |
| <span data-ttu-id="ff309-112">Funkce</span><span class="sxs-lookup"><span data-stu-id="ff309-112">Feature</span></span> |<span data-ttu-id="ff309-113">Název funkce.</span><span class="sxs-lookup"><span data-stu-id="ff309-113">The feature name.</span></span> <span data-ttu-id="ff309-114">Příklad: podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="ff309-114">For example: conditional access</span></span> |
| <span data-ttu-id="ff309-115">Aplikace s přístupem</span><span class="sxs-lookup"><span data-stu-id="ff309-115">Application Accessed</span></span> |<span data-ttu-id="ff309-116">Název aplikace, která pracuje s funkcí.</span><span class="sxs-lookup"><span data-stu-id="ff309-116">The name of the application that is being accessed with the feature.</span></span> <span data-ttu-id="ff309-117">Příklad: Office 365 Sharepointu Online</span><span class="sxs-lookup"><span data-stu-id="ff309-117">For example: Office 365 SharePoint Online</span></span> |

> [!NOTE]
> <span data-ttu-id="ff309-118">Pokud uživatelský účet je odstraněný sloupec 'Nelicencovaného uživatele' vyplní s ID, jako je 1003000090D8B285</span><span class="sxs-lookup"><span data-stu-id="ff309-118">If a user account has been deleted the ‘Unlicensed User’ column will be populated with an ID, like 1003000090D8B285</span></span>
> 
> 

## <a name="conditional-access-feature"></a><span data-ttu-id="ff309-119">Funkce podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="ff309-119">Conditional access feature</span></span>
<span data-ttu-id="ff309-120">Uživatelé bez licence budou označeny při přístupu služby, který má zásady podmíněného přístupu použito v případě, že nemají licenci Azure AD Premium.</span><span class="sxs-lookup"><span data-stu-id="ff309-120">Unlicensed users will be flagged when they access a service that has conditional access policy applied if they do not have an Azure AD Premium license.</span></span> 

<span data-ttu-id="ff309-121">To se vztahuje na MFA / zásady umístění, jakož i zařízení zásady, které pomocí Intune.</span><span class="sxs-lookup"><span data-stu-id="ff309-121">This applies to MFA / Location policies as well as device polices that use Intune.</span></span>

## <a name="see-also"></a><span data-ttu-id="ff309-122">Viz také</span><span class="sxs-lookup"><span data-stu-id="ff309-122">See also</span></span>
* [<span data-ttu-id="ff309-123">Použití podmíněného přístupu s Office 365 a další služby Azure Active Directory připojení aplikace</span><span class="sxs-lookup"><span data-stu-id="ff309-123">Using Conditional Access with Office 365 and other Azure Active Directory connected apps</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="ff309-124">Začínáme s podmíněným přístupem ke službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff309-124">Getting started with conditional access to Azure AD</span></span>](active-directory-conditional-access-azuread-connected-apps.md) 

