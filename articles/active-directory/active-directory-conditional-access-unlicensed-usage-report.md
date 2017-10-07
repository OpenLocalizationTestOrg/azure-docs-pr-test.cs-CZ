---
title: "aaaUnlicensed sestav využití | Microsoft Docs"
description: "Hello nelicencovaného využití sestava pomáhá identifikovat uživatelé bez licence, které používají placené funkce Azure AD."
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
ms.openlocfilehash: c44d1756b4641d7ca88434017eedb6c5e2567cb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="unlicensed-usage-report"></a><span data-ttu-id="3eaed-103">Zprávu nelicencovaná využití</span><span class="sxs-lookup"><span data-stu-id="3eaed-103">Unlicensed usage report</span></span>
<span data-ttu-id="3eaed-104">Hello nelicencovaného využití sestava pomáhá identifikovat uživatelé bez licence, které používají placené funkce Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3eaed-104">hello unlicensed usage report helps you identify unlicensed users that are using paid Azure AD features.</span></span> <span data-ttu-id="3eaed-105">To vám umožní lepší využití toomake licencí, které jste zakoupili a tooidentify víte, že pokud budete potřebovat další licence.</span><span class="sxs-lookup"><span data-stu-id="3eaed-105">This allows you toomake better use of licenses that you have purchased and tooidentify you know when you may need additional licenses.</span></span> 

<span data-ttu-id="3eaed-106">Sestava Hello zobrazuje aktivní využití hello placené funkce v hello posledních 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="3eaed-106">hello report shows active usage of hello paid features in hello last 30 days.</span></span> 

## <a name="report-structure"></a><span data-ttu-id="3eaed-107">Struktura sestavy</span><span class="sxs-lookup"><span data-stu-id="3eaed-107">Report structure</span></span>
| <span data-ttu-id="3eaed-108">Název sloupce</span><span class="sxs-lookup"><span data-stu-id="3eaed-108">Column name</span></span> | <span data-ttu-id="3eaed-109">Popis</span><span class="sxs-lookup"><span data-stu-id="3eaed-109">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="3eaed-110">Nelicencovaného uživatele</span><span class="sxs-lookup"><span data-stu-id="3eaed-110">Unlicensed User</span></span> |<span data-ttu-id="3eaed-111">Jméno uživatele, hello</span><span class="sxs-lookup"><span data-stu-id="3eaed-111">Name of hello user</span></span> |
| <span data-ttu-id="3eaed-112">Funkce</span><span class="sxs-lookup"><span data-stu-id="3eaed-112">Feature</span></span> |<span data-ttu-id="3eaed-113">název funkce Hello.</span><span class="sxs-lookup"><span data-stu-id="3eaed-113">hello feature name.</span></span> <span data-ttu-id="3eaed-114">Příklad: podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="3eaed-114">For example: conditional access</span></span> |
| <span data-ttu-id="3eaed-115">Aplikace s přístupem</span><span class="sxs-lookup"><span data-stu-id="3eaed-115">Application Accessed</span></span> |<span data-ttu-id="3eaed-116">Název Hello hello aplikace, která pracuje s funkcí hello.</span><span class="sxs-lookup"><span data-stu-id="3eaed-116">hello name of hello application that is being accessed with hello feature.</span></span> <span data-ttu-id="3eaed-117">Příklad: Office 365 Sharepointu Online</span><span class="sxs-lookup"><span data-stu-id="3eaed-117">For example: Office 365 SharePoint Online</span></span> |

> [!NOTE]
> <span data-ttu-id="3eaed-118">Pokud byl odstraněn uživatelským účtem hello "Nelicencovaného uživatelem" vyplní sloupec s ID, jako je 1003000090D8B285</span><span class="sxs-lookup"><span data-stu-id="3eaed-118">If a user account has been deleted hello ‘Unlicensed User’ column will be populated with an ID, like 1003000090D8B285</span></span>
> 
> 

## <a name="conditional-access-feature"></a><span data-ttu-id="3eaed-119">Funkce podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="3eaed-119">Conditional access feature</span></span>
<span data-ttu-id="3eaed-120">Uživatelé bez licence budou označeny při přístupu služby, který má zásady podmíněného přístupu použito v případě, že nemají licenci Azure AD Premium.</span><span class="sxs-lookup"><span data-stu-id="3eaed-120">Unlicensed users will be flagged when they access a service that has conditional access policy applied if they do not have an Azure AD Premium license.</span></span> 

<span data-ttu-id="3eaed-121">To platí tooMFA nebo zásady umístění, jakož i zařízení zásady, které pomocí Intune.</span><span class="sxs-lookup"><span data-stu-id="3eaed-121">This applies tooMFA / Location policies as well as device polices that use Intune.</span></span>

## <a name="see-also"></a><span data-ttu-id="3eaed-122">Viz také</span><span class="sxs-lookup"><span data-stu-id="3eaed-122">See also</span></span>
* [<span data-ttu-id="3eaed-123">Použití podmíněného přístupu s Office 365 a další služby Azure Active Directory připojení aplikace</span><span class="sxs-lookup"><span data-stu-id="3eaed-123">Using Conditional Access with Office 365 and other Azure Active Directory connected apps</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="3eaed-124">Začínáme s podmíněným přístupem tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="3eaed-124">Getting started with conditional access tooAzure AD</span></span>](active-directory-conditional-access-azuread-connected-apps.md) 

