---
title: "Řešení potíží: 'Služby Active Directory, je položka chybí nebo není k dispozici | Microsoft Docs"
description: "Jaké toodo při položku nabídky služby Active Directory se nezobrazí v hello portálu pro správu Azure."
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
ms.openlocfilehash: d7355a4e39141f0b09272dc5615c309b23c8c70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a><span data-ttu-id="b0000-103">Řešení potíží: 'Služby Active Directory, je položka chybí nebo není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0000-103">Troubleshooting: 'Active Directory' item is missing or not available</span></span>
<span data-ttu-id="b0000-104">Řadu hello pokyny k používání funkce služby Azure Active Directory a služby začínat řetězcem "přejděte toohello Azure Management Portal a klikněte na tlačítko **služby Active Directory**."</span><span class="sxs-lookup"><span data-stu-id="b0000-104">Many of hello instructions for using Azure Active Directory features and services begin with "Go toohello Azure Management Portal and click **Active Directory**."</span></span> <span data-ttu-id="b0000-105">Ale co můžete udělat, pokud není uvedené hello služby Active Directory rozšíření nebo položku nabídky, nebo pokud je označen **není k dispozici**?</span><span class="sxs-lookup"><span data-stu-id="b0000-105">But what do you do if hello Active Directory extension or menu item does not appear or if it is marked **Not Available**?</span></span> <span data-ttu-id="b0000-106">Toto téma je navrženou toohelp.</span><span class="sxs-lookup"><span data-stu-id="b0000-106">This topic is designed toohelp.</span></span> <span data-ttu-id="b0000-107">Popisuje hello podmínky, za kterých **služby Active Directory** se nezobrazí nebo není k dispozici a vysvětluje, jak tooproceed.</span><span class="sxs-lookup"><span data-stu-id="b0000-107">It describes hello conditions under which **Active Directory** does not appear or is unavailable and explains how tooproceed.</span></span>

## <a name="active-directory-is-missing"></a><span data-ttu-id="b0000-108">Chybí služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="b0000-108">Active Directory is missing</span></span>
<span data-ttu-id="b0000-109">Obvykle **služby Active Directory** položka je zobrazena v nabídce levém navigačním hello.</span><span class="sxs-lookup"><span data-stu-id="b0000-109">Typically, an **Active Directory** item appears in hello left navigation menu.</span></span> <span data-ttu-id="b0000-110">Hello pokyny v Azure Active Directory postupech předpokládají, že tato položka je v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="b0000-110">hello instructions in Azure Active Directory procedures assume that this item is in your view.</span></span>

![Snímek obrazovky: služby Active Directory v Azure](./media/active-directory-troubleshooting/typical-view.png)

<span data-ttu-id="b0000-112">Hello služby Active Directory položka je zobrazena v levé navigační nabídce hello, pokud žádné z následujících podmínek hello hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="b0000-112">hello Active Directory item appears in hello left navigation menu when any of hello following conditions is true.</span></span> <span data-ttu-id="b0000-113">V opačném hello položky nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="b0000-113">Otherwise, hello item does not appear.</span></span>

* <span data-ttu-id="b0000-114">aktuální uživatel Hello přihlášeni pomocí účtu Microsoft (dříve označované jako účet Windows Live ID).</span><span class="sxs-lookup"><span data-stu-id="b0000-114">hello current user signed on with a Microsoft account (formerly known as a Windows Live ID).</span></span>
  
    <span data-ttu-id="b0000-115">NEBO</span><span class="sxs-lookup"><span data-stu-id="b0000-115">OR</span></span>
* <span data-ttu-id="b0000-116">Hello klientovi Azure má adresáře a hello aktuálního účtu je správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="b0000-116">hello Azure tenant has a directory and hello current account is a directory administrator.</span></span>
  
    <span data-ttu-id="b0000-117">NEBO</span><span class="sxs-lookup"><span data-stu-id="b0000-117">OR</span></span>
* <span data-ttu-id="b0000-118">Hello klientovi Azure má alespoň jeden obor názvů řízení přístupu Azure AD (ACS).</span><span class="sxs-lookup"><span data-stu-id="b0000-118">hello Azure tenant has at least one Azure AD Access Control (ACS) namespace.</span></span> <span data-ttu-id="b0000-119">Další informace najdete v tématu [Namespace řízení přístupu](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0000-119">For more information, see [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span></span>
  
    <span data-ttu-id="b0000-120">NEBO</span><span class="sxs-lookup"><span data-stu-id="b0000-120">OR</span></span>
* <span data-ttu-id="b0000-121">Hello klientovi Azure má aspoň jednoho poskytovatele ověřování Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="b0000-121">hello Azure tenant has at least one Azure Multi-Factor Authentication provider.</span></span> <span data-ttu-id="b0000-122">Další informace najdete v tématu [Správa poskytovatelé Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="b0000-122">For more information, see [Administering Azure Multi-Factor Authentication Providers](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span></span>

<span data-ttu-id="b0000-123">toocreate na obor názvů řízení přístupu nebo poskytovatele služby Multi-Factor Authentication, klikněte na tlačítko **+ nový** > **App Services** > **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b0000-123">toocreate an Access Control namespace or a Multi-Factor Authentication provider, click **+New** > **App Services** > **Active Directory**.</span></span>

<span data-ttu-id="b0000-124">adresář tooa tooget práva pro správu, mají správce přiřadit tooyour účtu s rolí.</span><span class="sxs-lookup"><span data-stu-id="b0000-124">tooget administrative rights tooa directory, have an administrator assign an administrator role tooyour account.</span></span> <span data-ttu-id="b0000-125">Podrobnosti najdete v tématu [přiřazení rolí správce](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="b0000-125">For details, see [Assigning administrator roles](active-directory-assign-admin-roles.md).</span></span>

## <a name="active-directory-is-not-available"></a><span data-ttu-id="b0000-126">Služba Active Directory není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0000-126">Active Directory is not available</span></span>
<span data-ttu-id="b0000-127">Když kliknete na tlačítko **+ nový** > **App Services**, **služby Active Directory** položka je zobrazena.</span><span class="sxs-lookup"><span data-stu-id="b0000-127">When you click **+New** > **App Services**, an **Active Directory** item appears.</span></span> <span data-ttu-id="b0000-128">Konkrétně hello položky služby Active Directory se zobrazí, když některé z funkcí služby Active Directory hello, jako je například adresář, řízení přístupu nebo zprostředkovatel vícefaktorového ověřování, jsou k dispozici toohello aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="b0000-128">Specifically, hello Active Directory item appears when any of hello Active Directory features, such as Directory, Access Control, or Multi-Factor Auth Provider, are available toohello current user.</span></span>

<span data-ttu-id="b0000-129">Ale při načítání stránky hello hello položka není k dispozici a je označen jako **není k dispozici**.</span><span class="sxs-lookup"><span data-stu-id="b0000-129">However, while hello page is loading, hello item is dimmed and is marked **Not Available**.</span></span> <span data-ttu-id="b0000-130">Toto je dočasný stav.</span><span class="sxs-lookup"><span data-stu-id="b0000-130">This is a temporary state.</span></span> <span data-ttu-id="b0000-131">Pokud jste Počkejte několik sekund, hello položky k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b0000-131">If you wait a few seconds, hello item becomes available.</span></span> <span data-ttu-id="b0000-132">Pokud se prodlužuje hello zpoždění, aktualizace webovou stránku hello často řeší problém hello.</span><span class="sxs-lookup"><span data-stu-id="b0000-132">If hello delay is prolonged, refreshing hello web page often resolves hello problem.</span></span>

![Snímek obrazovky: není k dispozici služba Active Directory](./media/active-directory-troubleshooting/not-available.png)

