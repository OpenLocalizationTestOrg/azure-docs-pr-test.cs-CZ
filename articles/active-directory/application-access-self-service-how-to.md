---
title: "Postup konfigurace samoobslužné služby aplikace přiřazení | Microsoft Docs"
description: "Povolit přístup k aplikaci Samoobslužné služby umožnit uživatelům najít vlastní aplikace"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 7991dc19d41c5eb8e149c3ee08069e1a162929cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-self-service-application-assignment"></a><span data-ttu-id="2abf0-103">Postup konfigurace samoobslužné služby aplikace přiřazení</span><span class="sxs-lookup"><span data-stu-id="2abf0-103">How to configure self-service application assignment</span></span>

<span data-ttu-id="2abf0-104">Než uživatelům můžete zjistit samoobslužné aplikací z jejich přístupového panelu, musíte povolit **přístup k aplikaci Samoobslužné služby** pro všechny aplikace, které chcete povolit uživatelům samoobslužné zjišťování a požádat o přístup k.</span><span class="sxs-lookup"><span data-stu-id="2abf0-104">Before your users can self-discover applications from their access panel, you need to enable **Self-service application access** to any applications that you wish to allow users to self-discover and request access to.</span></span>

<span data-ttu-id="2abf0-105">Tato funkce je skvělý způsob, jak ušetřit čas i finanční prostředky jako skupinu IT a důrazně doporučujeme jako součást nasazení moderních aplikací s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2abf0-105">This feature is a great way for you to save time and money as an IT group, and is highly recommended as part of a modern applications deployment with Azure Active Directory.</span></span>

<span data-ttu-id="2abf0-106">Pomocí této funkce můžete:</span><span class="sxs-lookup"><span data-stu-id="2abf0-106">Using this feature, you can:</span></span>

-   <span data-ttu-id="2abf0-107">Umožní uživatelům samoobslužné zjišťování aplikací z [Panel přístupu aplikace](https://myapps.microsoft.com/) bez bothering IT oddělení.</span><span class="sxs-lookup"><span data-stu-id="2abf0-107">Let users self-discover applications from the [Application Access Panel](https://myapps.microsoft.com/) without bothering the IT group.</span></span>

-   <span data-ttu-id="2abf0-108">Přidání těchto uživatelů do skupinu předem nakonfigurovaná tak, aby v tématu, který žádá o přístup, odebrat přístup a spravovat role přiřazené k nim.</span><span class="sxs-lookup"><span data-stu-id="2abf0-108">Add those users to a pre-configured group so you can see who has requested access, remove access, and manage the roles assigned to them.</span></span>

-   <span data-ttu-id="2abf0-109">Volitelně můžete povolit obchodní schvalovatel ke schválení žádosti o přístup aplikace, není třeba IT oddělení.</span><span class="sxs-lookup"><span data-stu-id="2abf0-109">Optionally allow a business approver to approve application access requests so the IT group doesn’t have to.</span></span>

-   <span data-ttu-id="2abf0-110">Volitelně můžete nakonfigurujte až 10 jednotlivce, kteří mohou schválit přístup k této aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2abf0-110">Optionally configure up to 10 individuals who may approve access to this application.</span></span>

-   <span data-ttu-id="2abf0-111">Volitelně můžete povolit obchodní schvalovatel k nastavení hesla uživatele, můžete použít k přihlášení do aplikace, vpravo od schvalovatele obchodní [Panel přístupu aplikace](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="2abf0-111">Optionally allow a business approver to set the passwords those users can use to sign in to the application, right from the business approver’s [Application Access Panel](https://myapps.microsoft.com/).</span></span>

-   <span data-ttu-id="2abf0-112">Volitelně můžete automaticky přiřadíte přímo přiřazené aplikační role uživatele samoobslužné služby.</span><span class="sxs-lookup"><span data-stu-id="2abf0-112">Optionally automatically assign self-service assigned users to an application role directly.</span></span>

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a><span data-ttu-id="2abf0-113">Povolit přístup k aplikaci Samoobslužné služby umožnit uživatelům najít vlastní aplikace</span><span class="sxs-lookup"><span data-stu-id="2abf0-113">Enable self-service application access to allow users to find their own applications</span></span>

<span data-ttu-id="2abf0-114">Přístup k aplikaci Samoobslužné služby je skvělým způsobem, jak povolit uživatelům samoobslužné zjišťování aplikací, můžete povolit obchodní skupiny můžete schválit přístup pro tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="2abf0-114">Self-service application access is a great way to allow users to self-discover applications, optionally allow the business group to approve access to those applications.</span></span> <span data-ttu-id="2abf0-115">Můžete povolit obchodní skupině pro správu přiřazené pro tyto uživatele pro heslo jednotné přihlašování v aplikacích vpravo z jejich přístup panelů přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="2abf0-115">You can allow the business group to manage the credentials assigned to those users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="2abf0-116">Pokud chcete povolit samoobslužné služby aplikaci přístup k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="2abf0-116">To enable self-service application access to an application, follow the steps below:</span></span>

1.  <span data-ttu-id="2abf0-117">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="2abf0-117">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="2abf0-118">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="2abf0-118">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="2abf0-119">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="2abf0-119">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="2abf0-120">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2abf0-120">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="2abf0-121">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="2abf0-121">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="2abf0-122">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="2abf0-122">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="2abf0-123">Vyberte aplikaci, které chcete povolit samoobslužné přístup ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="2abf0-123">Select the application you want to enable Self-service access to from the list.</span></span>

7.  <span data-ttu-id="2abf0-124">Po načtení aplikace, klikněte na **samoobslužné služby** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="2abf0-124">Once the application loads, click **Self-service** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="2abf0-125">Pokud chcete povolit přístup k aplikaci Samoobslužné služby pro tuto aplikaci, zapněte **povolit uživatelům žádat o přístup k této aplikaci?** přepnutím **Ano.**</span><span class="sxs-lookup"><span data-stu-id="2abf0-125">To enable Self-service application access for this application, turn the **Allow users to request access to this application?** toggle to **Yes.**</span></span>

9.  <span data-ttu-id="2abf0-126">V dalším kroku vyberte skupiny, které uživatelům, kteří požadují by se měl přístup k této aplikaci přidat, klikněte na tlačítko modulu pro výběr vedle popisek **skupinu, pro kterou má přiřazené byli přidáni uživatelé?** a vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="2abf0-126">Next, to select the group to which users who request access to this application should be added, click the selector next to the label **To which group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="2abf0-127">**Volitelné:** nastaví, pokud chcete vyžadovat schválení obchodní před uživatelé mají povolen přístup **vyžadovat schválení před udělením přístupu k této aplikaci?** přepnutím **Ano**.</span><span class="sxs-lookup"><span data-stu-id="2abf0-127">**Optional:** If you wish to require a business approval before users are allowed access, set the **Require approval before granting access to this application?** toggle to **Yes**.</span></span>

11. <span data-ttu-id="2abf0-128">**Volitelné: pro aplikace pomocí hesla jednotné přihlašování na pouze** Pokud chcete povolit tyto firmy schvalovatelů k zadání hesla, které se odesílají na tuto žádost o schválení uživatelé, nastavte **povolit schvalovatelů k nastavení hesla uživatele pro tuto aplikaci?** přepnutím **Ano**.</span><span class="sxs-lookup"><span data-stu-id="2abf0-128">**Optional: For applications using password single-sign on only,** if you wish to allow those business approvers to specify the passwords that are sent to this application for approved users, set the **Allow approvers to set user’s passwords for this application?** toggle to **Yes**.</span></span>

12. <span data-ttu-id="2abf0-129">**Volitelné:** pro zadání schvalovatelů firmy, kteří mají povoleno schválit přístup k této aplikaci, klikněte na výběr vedle popisek **kdo může schválit přístup k této aplikaci?** můžete vybrat až 10 jednotlivé obchodní schvalovatele.</span><span class="sxs-lookup"><span data-stu-id="2abf0-129">**Optional:** To specify the business approvers who are allowed to approve access to this application, click the selector next to the label **Who is allowed to approve access to this application?** to select up to 10 individual business approvers.</span></span>

   >[!NOTE]
   ><span data-ttu-id="2abf0-130">Skupiny nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="2abf0-130">Groups are not supported.</span></span>
   >
   >

13. <span data-ttu-id="2abf0-131">**Volitelné:** **pro aplikace, které zveřejňují role**, pokud chcete přiřadit roli schválené uživatelé samoobslužné služby, klikněte na modulu pro výběr vedle **do role, které by měl být přiřazena uživatelům v této aplikaci?** vyberte roli, ke kterému by se měla přiřadit těmto uživatelům.</span><span class="sxs-lookup"><span data-stu-id="2abf0-131">**Optional:** **For applications which expose roles**, if you wish to assign self-service approved users to a role, click the selector next to the **To which role should users be assigned in this application?** to select the role to which these users should be assigned.</span></span>

14. <span data-ttu-id="2abf0-132">Klikněte **Uložit** tlačítka v horní části okna dokončit.</span><span class="sxs-lookup"><span data-stu-id="2abf0-132">Click the **Save** button at the top of the blade to finish.</span></span>

<span data-ttu-id="2abf0-133">Po dokončení konfigurace samoobslužné služby aplikace, uživatelé mohou přejít na jejich [Panel přístupu aplikace](https://myapps.microsoft.com/) a klikněte na tlačítko **+ přidat** tlačítko k vyhledání aplikace, na které jste povolili samoobslužné služby přístup.</span><span class="sxs-lookup"><span data-stu-id="2abf0-133">Once you complete Self-service application configuration, users can navigate to their [Application Access Panel](https://myapps.microsoft.com/) and click the **+Add** button to find the apps to which you have enabled Self-service access.</span></span> <span data-ttu-id="2abf0-134">Obchodní schvalovatelů také zobrazit oznámení v jejich [Panel přístupu aplikace](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="2abf0-134">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="2abf0-135">Můžete povolit e-mail s upozorněním, když uživatel požaduje přístup k aplikaci, která vyžaduje schválení.</span><span class="sxs-lookup"><span data-stu-id="2abf0-135">You can enable an email notifying them when a user has requested access to an application that requires their approval.</span></span> 

<span data-ttu-id="2abf0-136">Tato schválení podporovat pouze jeden schválení pracovních, což znamená, že pokud zadáte několik schvalovatelů, může jeden schvalovatel schvalovatel přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2abf0-136">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access to the application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2abf0-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2abf0-137">Next steps</span></span>
[<span data-ttu-id="2abf0-138">Nastavení služby Azure Active Directory pro samoobslužnou správu skupin</span><span class="sxs-lookup"><span data-stu-id="2abf0-138">Setting up Azure Active Directory for self-service group management</span></span>](active-directory-accessmanagement-self-service-group-management.md)
