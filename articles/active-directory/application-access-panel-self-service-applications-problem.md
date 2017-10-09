---
title: "aaaProblem pomocí samoobslužné služby aplikace access | Microsoft Docs"
description: "Řešení potíží s přístupem související aplikace služby tooself problémy"
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
ms.openlocfilehash: 2487be1df191a4e7fd0bcc0ebbe4ea62fae0fd5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-using-self-service-application-access"></a><span data-ttu-id="51758-103">Problém pomocí samoobslužné služby aplikace access</span><span class="sxs-lookup"><span data-stu-id="51758-103">Problem using self-service application access</span></span>

<span data-ttu-id="51758-104">Přístup k aplikaci Samoobslužné služby je skvělý způsob tooallow uživatelé tooself-zjišťovat aplikace, můžete také povolit hello firmy skupiny tooapprove přístup toothose aplikace.</span><span class="sxs-lookup"><span data-stu-id="51758-104">Self-service application access is a great way tooallow users tooself-discover applications, optionally allow hello business group tooapprove access toothose applications.</span></span> <span data-ttu-id="51758-105">Můžete povolit, že pověření hello hello firmy skupiny toomanage přiřadili toothose uživatelů pro heslo jednotné přihlašování v aplikacích přímo z jejich panelů přístup.</span><span class="sxs-lookup"><span data-stu-id="51758-105">You can allow hello business group toomanage hello credentials assigned toothose users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="51758-106">Než uživatelům můžete zjistit samoobslužné aplikací z jejich přístupového panelu, musíte tooenable **přístup k aplikaci Samoobslužné služby** tooany aplikace, které chcete tooallow uživatelé tooself-zjišťovat a požádat o přístup k.</span><span class="sxs-lookup"><span data-stu-id="51758-106">Before your users can self-discover applications from their access panel, you need tooenable **Self-service application access** tooany applications that you wish tooallow users tooself-discover and request access to.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="51758-107">Obecné problémy toocheck nejprve</span><span class="sxs-lookup"><span data-stu-id="51758-107">General issues toocheck first</span></span>

-   <span data-ttu-id="51758-108">Zajistěte, aby byl správně nakonfigurován přístup k aplikaci Samoobslužné služby.</span><span class="sxs-lookup"><span data-stu-id="51758-108">Make sure self-service application access is configured correctly.</span></span> <span data-ttu-id="51758-109">Viz "Jak tooconfigure samoobslužné služby aplikace přistupovat ke".</span><span class="sxs-lookup"><span data-stu-id="51758-109">See “How tooconfigure self-service application access”.</span></span>

-   <span data-ttu-id="51758-110">Zajistěte, aby hello uživatele nebo skupinu byl povolen přístup k aplikaci Samoobslužné služby toorequest.</span><span class="sxs-lookup"><span data-stu-id="51758-110">Make sure hello user or group has been enabled toorequest self-service application access.</span></span>

-   <span data-ttu-id="51758-111">Zkontrolujte, zda text hello návštěvy hello správné nastavené pro přístup k aplikaci Samoobslužné služby.</span><span class="sxs-lookup"><span data-stu-id="51758-111">Make sure hello user is visiting hello correct place for self-service application access.</span></span> <span data-ttu-id="51758-112">Uživatelé mohou přejít tootheir [Panel přístupu aplikace](https://myapps.microsoft.com/) a klikněte na tlačítko hello **+ přidat** tlačítko toofind hello aplikace toowhich jste povolili samoobslužné služby přístup.</span><span class="sxs-lookup"><span data-stu-id="51758-112">users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled self-service access.</span></span>

-   <span data-ttu-id="51758-113">Pokud přístup k aplikaci Samoobslužné služby byl právě nakonfigurovali, opakujte toosign a odhlašování do přístupového panelu hello uživatele po několika minutách toosee Pokud se nezobrazují změny hello samoobslužné služby přístup.</span><span class="sxs-lookup"><span data-stu-id="51758-113">If self-service application access was just recently configured, try toosign in and out again into hello user’s Access Panel after a few minutes toosee if hello self-service access changes have appeared.</span></span>

## <a name="how-tooconfigure-self-service-application-access"></a><span data-ttu-id="51758-114">Jak přistupovat k tooconfigure samoobslužné služby aplikace</span><span class="sxs-lookup"><span data-stu-id="51758-114">How tooconfigure self-service application access</span></span>

<span data-ttu-id="51758-115">tooenable aplikace Samoobslužné služby přístup tooan aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="51758-115">tooenable self-service application access tooan application, follow hello steps below:</span></span>

1.  <span data-ttu-id="51758-116">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="51758-116">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="51758-117">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="51758-117">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="51758-118">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="51758-118">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="51758-119">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="51758-119">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="51758-120">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="51758-120">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="51758-121">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="51758-121">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="51758-122">Vyberte aplikaci hello chcete tooenable samoobslužné služby přístup toofrom hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="51758-122">Select hello application you want tooenable Self-service access toofrom hello list.</span></span>

7.  <span data-ttu-id="51758-123">Po načtení hello aplikace, klikněte na **samoobslužné služby** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="51758-123">Once hello application loads, click **Self-service** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="51758-124">tooenable přístup k aplikaci Samoobslužné služby pro tuto aplikaci, aktivujte hello **povolit uživatelům toorequest přístup toothis aplikace?** přepnutí příliš**Ano.**</span><span class="sxs-lookup"><span data-stu-id="51758-124">tooenable Self-service application access for this application, turn hello **Allow users toorequest access toothis application?** toggle too**Yes.**</span></span>

9.  <span data-ttu-id="51758-125">V dalším kroku tooselect hello skupiny toowhich uživatelů, kteří požadují přístup toothis aplikace by měla být přidány, klikněte na tlačítko hello selektor další toohello popisek **toowhich skupina by měla přiřazená byli přidáni uživatelé?** a vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="51758-125">Next, tooselect hello group toowhich users who request access toothis application should be added, click hello selector next toohello label **toowhich group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="51758-126">**Volitelné:** nechcete-li toorequire obchodní schválení předtím, než mohou uživatelé přístup, nastavte hello **vyžadovat schválení před udělením přístupu toothis aplikace?** přepnutí příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="51758-126">**Optional:** If you wish toorequire a business approval before users are allowed access, set hello **Require approval before granting access toothis application?** toggle too**Yes**.</span></span>

11. <span data-ttu-id="51758-127">**Volitelné: pro aplikace pomocí hesla jednotné přihlašování na pouze** Pokud chcete tyto firmy schvalovatelů toospecify hello hesel, která se posílají toothis žádost o schválení uživatelé tooallow, nastavte hello **tooset schvalovatelů povolit hesla uživatele pro tuto aplikaci?**  přepnutí příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="51758-127">**Optional: For applications using password single-sign on only,** if you wish tooallow those business approvers toospecify hello passwords that are sent toothis application for approved users, set hello **Allow approvers tooset user’s passwords for this application?** toggle too**Yes**.</span></span>

12. <span data-ttu-id="51758-128">**Volitelné:** toospecify hello firmy schvalovatelů, kteří mají povoleno tooapprove přístup toothis aplikace, klikněte na tlačítko hello selektor další toohello popisek **kdo je povolená tooapprove přístup toothis aplikace?** tooselect nahoru too10 jednotlivé obchodní schvalovatelů.</span><span class="sxs-lookup"><span data-stu-id="51758-128">**Optional:** toospecify hello business approvers who are allowed tooapprove access toothis application, click hello selector next toohello label **Who is allowed tooapprove access toothis application?** tooselect up too10 individual business approvers.</span></span>

 >[!NOTE]
 > <span data-ttu-id="51758-129">Skupiny nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="51758-129">Groups are not supported.</span></span>
 >
 >

13. <span data-ttu-id="51758-130">**Volitelné:** **pro aplikace, které zveřejňují role**, pokud chcete, aby role tooa tooassign schválené uživatelé samoobslužné služby, klikněte na tlačítko Další toohello hello selektor **toowhich role lze přiřadit uživatelům v této aplikace?**  tooselect hello role toowhich by měla být přiřazená tyto uživatele.</span><span class="sxs-lookup"><span data-stu-id="51758-130">**Optional:** **For applications which expose roles**, if you wish tooassign self-service approved users tooa role, click hello selector next toohello **toowhich role should users be assigned in this application?** tooselect hello role toowhich these users should be assigned.</span></span>

14. <span data-ttu-id="51758-131">Klikněte na tlačítko hello **Uložit** tlačítko hello horní části okna toofinish hello.</span><span class="sxs-lookup"><span data-stu-id="51758-131">Click hello **Save** button at hello top of hello blade toofinish.</span></span>

<span data-ttu-id="51758-132">Po dokončení konfigurace samoobslužné služby aplikace, uživatelé mohou přejít tootheir [Panel přístupu aplikace](https://myapps.microsoft.com/) a klikněte na tlačítko hello **+ přidat** tlačítko toofind hello aplikace toowhich jste povolili Samoobslužné služby přístup.</span><span class="sxs-lookup"><span data-stu-id="51758-132">Once you complete Self-service application configuration, users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled Self-service access.</span></span> <span data-ttu-id="51758-133">Obchodní schvalovatelů také zobrazit oznámení v jejich [Panel přístupu aplikace](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="51758-133">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="51758-134">Můžete povolit e-mail s upozorněním, když uživatel požaduje přístup tooan aplikace, která vyžaduje schválení.</span><span class="sxs-lookup"><span data-stu-id="51758-134">You can enable an email notifying them when a user has requested access tooan application that requires their approval.</span></span> 

<span data-ttu-id="51758-135">Tato schválení podporují jeden schválení pracovních pouze, což znamená, že pokud zadáte několik schvalovatelů, jeden schvalovatel může schválit přístup toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="51758-135">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approve access toohello application.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a><span data-ttu-id="51758-136">Pokud tyto kroky řešení potíží Nepokoušejte se vyřešit problém hello</span><span class="sxs-lookup"><span data-stu-id="51758-136">If these troubleshooting steps do not resolve hello issue</span></span> 

<span data-ttu-id="51758-137">Otevřete lístek podpory se hello, pokud je k dispozici následující informace:</span><span class="sxs-lookup"><span data-stu-id="51758-137">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="51758-138">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="51758-138">Correlation error ID</span></span>

-   <span data-ttu-id="51758-139">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="51758-139">UPN (user email address)</span></span>

-   <span data-ttu-id="51758-140">TenantID</span><span class="sxs-lookup"><span data-stu-id="51758-140">TenantID</span></span>

-   <span data-ttu-id="51758-141">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="51758-141">Browser type</span></span>

-   <span data-ttu-id="51758-142">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="51758-142">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="51758-143">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="51758-143">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="51758-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51758-144">Next steps</span></span>
[<span data-ttu-id="51758-145">Nastavení služby Azure Active Directory pro samoobslužnou správu skupin</span><span class="sxs-lookup"><span data-stu-id="51758-145">Setting up Azure Active Directory for self-service group management</span></span>](active-directory-accessmanagement-self-service-group-management.md)
