---
title: "Problém pomocí samoobslužné služby aplikace access | Microsoft Docs"
description: "Řešení potíží s problémy související s přístup k aplikaci Samoobslužné služby"
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
ms.reviewer: japere
ms.openlocfilehash: 217726709a1fdb02275de5a76a1352ea9c350600
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problem-using-self-service-application-access"></a><span data-ttu-id="046bd-103">Problém pomocí samoobslužné služby aplikace access</span><span class="sxs-lookup"><span data-stu-id="046bd-103">Problem using self-service application access</span></span>

<span data-ttu-id="046bd-104">Přístup k aplikaci Samoobslužné služby je skvělým způsobem, jak povolit uživatelům samoobslužné zjišťování aplikací, můžete povolit obchodní skupiny můžete schválit přístup pro tyto aplikace.</span><span class="sxs-lookup"><span data-stu-id="046bd-104">Self-service application access is a great way to allow users to self-discover applications, optionally allow the business group to approve access to those applications.</span></span> <span data-ttu-id="046bd-105">Můžete povolit obchodní skupině pro správu přiřazené pro tyto uživatele pro heslo jednotné přihlašování v aplikacích vpravo z jejich přístup panelů přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="046bd-105">You can allow the business group to manage the credentials assigned to those users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="046bd-106">Než uživatelům můžete zjistit samoobslužné aplikací z jejich přístupového panelu, musíte povolit **přístup k aplikaci Samoobslužné služby** pro všechny aplikace, které chcete povolit uživatelům samoobslužné zjišťování a požádat o přístup k.</span><span class="sxs-lookup"><span data-stu-id="046bd-106">Before your users can self-discover applications from their access panel, you need to enable **Self-service application access** to any applications that you wish to allow users to self-discover and request access to.</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="046bd-107">Běžné problémy a proveďte nejprve kontrolu</span><span class="sxs-lookup"><span data-stu-id="046bd-107">General issues to check first</span></span>

-   <span data-ttu-id="046bd-108">Zajistěte, aby byl správně nakonfigurován přístup k aplikaci Samoobslužné služby.</span><span class="sxs-lookup"><span data-stu-id="046bd-108">Make sure self-service application access is configured correctly.</span></span> <span data-ttu-id="046bd-109">V kapitole "Jak konfigurovat přístup k aplikaci Samoobslužné služby".</span><span class="sxs-lookup"><span data-stu-id="046bd-109">See “How to configure self-service application access”.</span></span>

-   <span data-ttu-id="046bd-110">Zkontrolujte, zda že uživatel nebo skupina povolen požádat o přístup k aplikaci Samoobslužné služby.</span><span class="sxs-lookup"><span data-stu-id="046bd-110">Make sure the user or group has been enabled to request self-service application access.</span></span>

-   <span data-ttu-id="046bd-111">Zkontrolujte, zda že uživatel je návštěvu správné místa pro přístup k aplikaci Samoobslužné služby.</span><span class="sxs-lookup"><span data-stu-id="046bd-111">Make sure the user is visiting the correct place for self-service application access.</span></span> <span data-ttu-id="046bd-112">Uživatelé mohou přejít na jejich [Panel přístupu aplikace](https://myapps.microsoft.com/) a klikněte na tlačítko **+ přidat** tlačítko k vyhledání aplikace, na které jste povolili samoobslužné služby přístup.</span><span class="sxs-lookup"><span data-stu-id="046bd-112">users can navigate to their [Application Access Panel](https://myapps.microsoft.com/) and click the **+Add** button to find the apps to which you have enabled self-service access.</span></span>

-   <span data-ttu-id="046bd-113">Pokud přístup k aplikaci Samoobslužné služby byl právě nakonfigurovali, zkuste pro přihlášení a odhlášení znovu do uživatele přístupového panelu po několika minutách chcete zobrazit, pokud se nezobrazují změny samoobslužné služby přístup.</span><span class="sxs-lookup"><span data-stu-id="046bd-113">If self-service application access was just recently configured, try to sign in and out again into the user’s Access Panel after a few minutes to see if the self-service access changes have appeared.</span></span>

## <a name="how-to-configure-self-service-application-access"></a><span data-ttu-id="046bd-114">Postup konfigurace přístupu k aplikaci Samoobslužné služby</span><span class="sxs-lookup"><span data-stu-id="046bd-114">How to configure self-service application access</span></span>

<span data-ttu-id="046bd-115">Pokud chcete povolit samoobslužné služby aplikaci přístup k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="046bd-115">To enable self-service application access to an application, follow the steps below:</span></span>

1.  <span data-ttu-id="046bd-116">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="046bd-116">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="046bd-117">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="046bd-117">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="046bd-118">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="046bd-118">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="046bd-119">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="046bd-119">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="046bd-120">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="046bd-120">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="046bd-121">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="046bd-121">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="046bd-122">Vyberte aplikaci, které chcete povolit samoobslužné přístup ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="046bd-122">Select the application you want to enable Self-service access to from the list.</span></span>

7.  <span data-ttu-id="046bd-123">Po načtení aplikace, klikněte na **samoobslužné služby** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="046bd-123">Once the application loads, click **Self-service** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="046bd-124">Pokud chcete povolit přístup k aplikaci Samoobslužné služby pro tuto aplikaci, zapněte **povolit uživatelům žádat o přístup k této aplikaci?** přepnutím **Ano.**</span><span class="sxs-lookup"><span data-stu-id="046bd-124">To enable Self-service application access for this application, turn the **Allow users to request access to this application?** toggle to **Yes.**</span></span>

9.  <span data-ttu-id="046bd-125">V dalším kroku vyberte skupiny, které uživatelům, kteří požadují by se měl přístup k této aplikaci přidat, klikněte na tlačítko modulu pro výběr vedle popisek **skupinu, pro kterou má přiřazené byli přidáni uživatelé?** a vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="046bd-125">Next, to select the group to which users who request access to this application should be added, click the selector next to the label **To which group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="046bd-126">**Volitelné:** nastaví, pokud chcete vyžadovat schválení obchodní před uživatelé mají povolen přístup **vyžadovat schválení před udělením přístupu k této aplikaci?** přepnutím **Ano**.</span><span class="sxs-lookup"><span data-stu-id="046bd-126">**Optional:** If you wish to require a business approval before users are allowed access, set the **Require approval before granting access to this application?** toggle to **Yes**.</span></span>

11. <span data-ttu-id="046bd-127">**Volitelné: pro aplikace pomocí hesla jednotné přihlašování na pouze** Pokud chcete povolit tyto firmy schvalovatelů k zadání hesla, které se odesílají na tuto žádost o schválení uživatelé, nastavte **povolit schvalovatelů k nastavení hesla uživatele pro tuto aplikaci?** přepnutím **Ano**.</span><span class="sxs-lookup"><span data-stu-id="046bd-127">**Optional: For applications using password single-sign on only,** if you wish to allow those business approvers to specify the passwords that are sent to this application for approved users, set the **Allow approvers to set user’s passwords for this application?** toggle to **Yes**.</span></span>

12. <span data-ttu-id="046bd-128">**Volitelné:** pro zadání schvalovatelů firmy, kteří mají povoleno schválit přístup k této aplikaci, klikněte na výběr vedle popisek **kdo může schválit přístup k této aplikaci?** můžete vybrat až 10 jednotlivé obchodní schvalovatele.</span><span class="sxs-lookup"><span data-stu-id="046bd-128">**Optional:** To specify the business approvers who are allowed to approve access to this application, click the selector next to the label **Who is allowed to approve access to this application?** to select up to 10 individual business approvers.</span></span>

 >[!NOTE]
 > <span data-ttu-id="046bd-129">Skupiny nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="046bd-129">Groups are not supported.</span></span>
 >
 >

13. <span data-ttu-id="046bd-130">**Volitelné:** **pro aplikace, které zveřejňují role**, pokud chcete přiřadit roli schválené uživatelé samoobslužné služby, klikněte na modulu pro výběr vedle **do role, které by měl být přiřazena uživatelům v této aplikaci?** vyberte roli, ke kterému by se měla přiřadit těmto uživatelům.</span><span class="sxs-lookup"><span data-stu-id="046bd-130">**Optional:** **For applications which expose roles**, if you wish to assign self-service approved users to a role, click the selector next to the **To which role should users be assigned in this application?** to select the role to which these users should be assigned.</span></span>

14. <span data-ttu-id="046bd-131">Klikněte **Uložit** tlačítka v horní části okna dokončit.</span><span class="sxs-lookup"><span data-stu-id="046bd-131">Click the **Save** button at the top of the blade to finish.</span></span>

<span data-ttu-id="046bd-132">Po dokončení konfigurace samoobslužné služby aplikace, uživatelé mohou přejít na jejich [Panel přístupu aplikace](https://myapps.microsoft.com/) a klikněte na tlačítko **+ přidat** tlačítko k vyhledání aplikace, na které jste povolili samoobslužné služby přístup.</span><span class="sxs-lookup"><span data-stu-id="046bd-132">Once you complete Self-service application configuration, users can navigate to their [Application Access Panel](https://myapps.microsoft.com/) and click the **+Add** button to find the apps to which you have enabled Self-service access.</span></span> <span data-ttu-id="046bd-133">Obchodní schvalovatelů také zobrazit oznámení v jejich [Panel přístupu aplikace](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="046bd-133">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="046bd-134">Můžete povolit e-mail s upozorněním, když uživatel požaduje přístup k aplikaci, která vyžaduje schválení.</span><span class="sxs-lookup"><span data-stu-id="046bd-134">You can enable an email notifying them when a user has requested access to an application that requires their approval.</span></span> 

<span data-ttu-id="046bd-135">Tato schválení podporují jeden schválení pracovní postupy, což znamená, že pokud zadáte několik schvalovatelů, jeden schvalovatel může schválit přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="046bd-135">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approve access to the application.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a><span data-ttu-id="046bd-136">Pokud tyto kroky řešení potíží problém nevyřeší</span><span class="sxs-lookup"><span data-stu-id="046bd-136">If these troubleshooting steps do not resolve the issue</span></span> 

<span data-ttu-id="046bd-137">Otevřete lístek podpory s následujícími informacemi, pokud je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="046bd-137">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="046bd-138">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="046bd-138">Correlation error ID</span></span>

-   <span data-ttu-id="046bd-139">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="046bd-139">UPN (user email address)</span></span>

-   <span data-ttu-id="046bd-140">TenantID</span><span class="sxs-lookup"><span data-stu-id="046bd-140">TenantID</span></span>

-   <span data-ttu-id="046bd-141">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="046bd-141">Browser type</span></span>

-   <span data-ttu-id="046bd-142">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="046bd-142">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="046bd-143">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="046bd-143">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="046bd-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="046bd-144">Next steps</span></span>
[<span data-ttu-id="046bd-145">Nastavení služby Azure Active Directory pro samoobslužnou správu skupin</span><span class="sxs-lookup"><span data-stu-id="046bd-145">Setting up Azure Active Directory for self-service group management</span></span>](active-directory-accessmanagement-self-service-group-management.md)
