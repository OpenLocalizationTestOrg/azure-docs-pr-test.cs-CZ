---
title: "Řešení potíží: 'Služby Active Directory, je položka chybí nebo není k dispozici | Microsoft Docs"
description: "Co dělat, pokud položka nabídky služby Active Directory se nezobrazí na portálu správy Azure."
services: active-directory
documentationcenter: na
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 3383020d-6397-43ea-b7aa-c6a9d6a1e3df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.openlocfilehash: be3a797c4a405fd2f6636e67f4c961dd6d143486
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a><span data-ttu-id="6219f-103">Řešení potíží: 'Služby Active Directory, je položka chybí nebo není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6219f-103">Troubleshooting: 'Active Directory' item is missing or not available</span></span>
<span data-ttu-id="6219f-104">Řadu pokynů pro použití funkce Azure Active Directory a služby začínat řetězcem "přejděte na portálu pro správu Azure a klikněte na tlačítko **služby Active Directory**."</span><span class="sxs-lookup"><span data-stu-id="6219f-104">Many of the instructions for using Azure Active Directory features and services begin with "Go to the Azure Management Portal and click **Active Directory**."</span></span> <span data-ttu-id="6219f-105">Ale co můžete udělat, pokud položka rozšíření nebo nabídky služby Active Directory se nezobrazí nebo pokud je označen **není k dispozici**?</span><span class="sxs-lookup"><span data-stu-id="6219f-105">But what do you do if the Active Directory extension or menu item does not appear or if it is marked **Not Available**?</span></span> <span data-ttu-id="6219f-106">Toto téma je navržená tak, abyste.</span><span class="sxs-lookup"><span data-stu-id="6219f-106">This topic is designed to help.</span></span> <span data-ttu-id="6219f-107">Popisuje podmínky, pod kterým **služby Active Directory** se nezobrazí nebo není k dispozici a vysvětluje, jak pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6219f-107">It describes the conditions under which **Active Directory** does not appear or is unavailable and explains how to proceed.</span></span>

## <a name="active-directory-is-missing"></a><span data-ttu-id="6219f-108">Chybí služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="6219f-108">Active Directory is missing</span></span>
<span data-ttu-id="6219f-109">Obvykle **služby Active Directory** položka je zobrazena v levé navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="6219f-109">Typically, an **Active Directory** item appears in the left navigation menu.</span></span> <span data-ttu-id="6219f-110">Pokyny v Azure Active Directory postupech předpokládají, že tato položka je v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6219f-110">The instructions in Azure Active Directory procedures assume that this item is in your view.</span></span>

![Snímek obrazovky: služby Active Directory v Azure](./media/active-directory-troubleshooting/typical-view.png)

<span data-ttu-id="6219f-112">Pokud platí některá z následujících podmínek, zobrazí se v levém navigační nabídce položky služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6219f-112">The Active Directory item appears in the left navigation menu when any of the following conditions is true.</span></span> <span data-ttu-id="6219f-113">Položka, jinak hodnota nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="6219f-113">Otherwise, the item does not appear.</span></span>

* <span data-ttu-id="6219f-114">Aktuální uživatel ověřen pomocí účtu Microsoft (dříve označované jako účet Windows Live ID).</span><span class="sxs-lookup"><span data-stu-id="6219f-114">The current user signed on with a Microsoft account (formerly known as a Windows Live ID).</span></span>
  
    <span data-ttu-id="6219f-115">NEBO</span><span class="sxs-lookup"><span data-stu-id="6219f-115">OR</span></span>
* <span data-ttu-id="6219f-116">Ke klientovi Azure má adresáře a aktuálního účtu je správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="6219f-116">The Azure tenant has a directory and the current account is a directory administrator.</span></span>
  
    <span data-ttu-id="6219f-117">NEBO</span><span class="sxs-lookup"><span data-stu-id="6219f-117">OR</span></span>
* <span data-ttu-id="6219f-118">Ke klientovi Azure má alespoň jeden obor názvů řízení přístupu Azure AD (ACS).</span><span class="sxs-lookup"><span data-stu-id="6219f-118">The Azure tenant has at least one Azure AD Access Control (ACS) namespace.</span></span> <span data-ttu-id="6219f-119">Další informace najdete v tématu [Namespace řízení přístupu](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span><span class="sxs-lookup"><span data-stu-id="6219f-119">For more information, see [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span></span>
  
    <span data-ttu-id="6219f-120">NEBO</span><span class="sxs-lookup"><span data-stu-id="6219f-120">OR</span></span>
* <span data-ttu-id="6219f-121">Ke klientovi Azure má aspoň jednoho poskytovatele ověřování Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="6219f-121">The Azure tenant has at least one Azure Multi-Factor Authentication provider.</span></span> <span data-ttu-id="6219f-122">Další informace najdete v tématu [Správa poskytovatelé Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="6219f-122">For more information, see [Administering Azure Multi-Factor Authentication Providers](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span></span>

<span data-ttu-id="6219f-123">Chcete-li vytvořit na obor názvů řízení přístupu nebo poskytovatele služby Multi-Factor Authentication, klikněte na tlačítko **+ nový** > **App Services** > **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6219f-123">To create an Access Control namespace or a Multi-Factor Authentication provider, click **+New** > **App Services** > **Active Directory**.</span></span>

<span data-ttu-id="6219f-124">Potřebujete práva správce k adresáři, požádejte správce, přiřadit role správce účtu.</span><span class="sxs-lookup"><span data-stu-id="6219f-124">To get administrative rights to a directory, have an administrator assign an administrator role to your account.</span></span> <span data-ttu-id="6219f-125">Podrobnosti najdete v tématu [přiřazení rolí správce](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="6219f-125">For details, see [Assigning administrator roles](active-directory-assign-admin-roles.md).</span></span>

## <a name="active-directory-is-not-available"></a><span data-ttu-id="6219f-126">Služba Active Directory není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6219f-126">Active Directory is not available</span></span>
<span data-ttu-id="6219f-127">Když kliknete na tlačítko **+ nový** > **App Services**, **služby Active Directory** položka je zobrazena.</span><span class="sxs-lookup"><span data-stu-id="6219f-127">When you click **+New** > **App Services**, an **Active Directory** item appears.</span></span> <span data-ttu-id="6219f-128">Konkrétně položky služby Active Directory se zobrazí, když žádnou z funkcí služby Active Directory, jako je například adresář, řízení přístupu nebo zprostředkovatel vícefaktorového ověřování, jsou k dispozici pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="6219f-128">Specifically, the Active Directory item appears when any of the Active Directory features, such as Directory, Access Control, or Multi-Factor Auth Provider, are available to the current user.</span></span>

<span data-ttu-id="6219f-129">Ale při načítání stránky, položka je neaktivní a je označen **není k dispozici**.</span><span class="sxs-lookup"><span data-stu-id="6219f-129">However, while the page is loading, the item is dimmed and is marked **Not Available**.</span></span> <span data-ttu-id="6219f-130">Toto je dočasný stav.</span><span class="sxs-lookup"><span data-stu-id="6219f-130">This is a temporary state.</span></span> <span data-ttu-id="6219f-131">Pokud jste Počkejte několik sekund, položka bude k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6219f-131">If you wait a few seconds, the item becomes available.</span></span> <span data-ttu-id="6219f-132">Pokud se prodlužuje zpoždění, obnovení webové stránky často řeší problém.</span><span class="sxs-lookup"><span data-stu-id="6219f-132">If the delay is prolonged, refreshing the web page often resolves the problem.</span></span>

![Snímek obrazovky: není k dispozici služba Active Directory](./media/active-directory-troubleshooting/not-available.png)

