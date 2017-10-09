---
title: "přístup k samoobslužné služby aplikaci toouse aaaHow | Microsoft Docs"
description: "Povolit aplikaci Samoobslužné služby přístup tooallow uživatelé toofind svých vlastních aplikacích"
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
ms.openlocfilehash: 03a44c20d544a6232fa802bcffaf70e5030ad3ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-self-service-application-access"></a><span data-ttu-id="c1fff-103">Jak přistupovat k toouse samoobslužné služby aplikace</span><span class="sxs-lookup"><span data-stu-id="c1fff-103">How toouse self-service application access</span></span>

<span data-ttu-id="c1fff-104">Než uživatelům můžete zjistit samoobslužné aplikací z jejich přístupového panelu, musíte tooenable **přístup k aplikaci Samoobslužné služby** tooany aplikace, které chcete tooallow uživatelé tooself-zjišťovat a požádat o přístup k.</span><span class="sxs-lookup"><span data-stu-id="c1fff-104">Before your users can self-discover applications from their access panel, you need tooenable **Self-service application access** tooany applications that you wish tooallow users tooself-discover and request access to.</span></span>

<span data-ttu-id="c1fff-105">Tato funkce je skvělým způsobem, abyste toosave čas a peníze jako IT oddělení a důrazně doporučujeme jako součást nasazení moderních aplikací s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c1fff-105">This feature is a great way for you toosave time and money as an IT group, and is highly recommended as part of a modern applications deployment with Azure Active Directory.</span></span>

<span data-ttu-id="c1fff-106">Pomocí této funkce můžete:</span><span class="sxs-lookup"><span data-stu-id="c1fff-106">Using this feature, you can:</span></span>

-   <span data-ttu-id="c1fff-107">Umožní uživatelům samoobslužné zjišťování aplikací z hello [Panel přístupu aplikace](https://myapps.microsoft.com/) bez bothering hello IT skupiny.</span><span class="sxs-lookup"><span data-stu-id="c1fff-107">Let users self-discover applications from hello [Application Access Panel](https://myapps.microsoft.com/) without bothering hello IT group.</span></span>

-   <span data-ttu-id="c1fff-108">Přidejte tyto skupiny předem nakonfigurovaná tooa uživatelů tak, aby v tématu, který žádá o přístup, odebrání přístupu a spravovat role hello přiřazené toothem.</span><span class="sxs-lookup"><span data-stu-id="c1fff-108">Add those users tooa pre-configured group so you can see who has requested access, remove access, and manage hello roles assigned toothem.</span></span>

-   <span data-ttu-id="c1fff-109">Volitelně povolení schvalovatel obchodní tooapprove aplikace požadavků na přístup, není třeba hello IT oddělení.</span><span class="sxs-lookup"><span data-stu-id="c1fff-109">Optionally allow a business approver tooapprove application access requests so hello IT group doesn’t have to.</span></span>

-   <span data-ttu-id="c1fff-110">Volitelně můžete nakonfigurujte až too10 jednotlivce, kteří mohou schválit přístup toothis aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1fff-110">Optionally configure up too10 individuals who may approve access toothis application.</span></span>

-   <span data-ttu-id="c1fff-111">Volitelně můžete povolit schvalovatel obchodní tooset hello hesla uživatele, můžete použít toosign v aplikaci toohello vpravo od schvalovatele hello firmy [Panel přístupu aplikace](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="c1fff-111">Optionally allow a business approver tooset hello passwords those users can use toosign in toohello application, right from hello business approver’s [Application Access Panel](https://myapps.microsoft.com/).</span></span>

-   <span data-ttu-id="c1fff-112">Volitelně můžete automaticky přiřadíte přímo přiřazené uživatelům samoobslužné služby tooan aplikační role.</span><span class="sxs-lookup"><span data-stu-id="c1fff-112">Optionally automatically assign self-service assigned users tooan application role directly.</span></span>

## <a name="enable-self-service-application-access-tooallow-users-toofind-their-own-applications"></a><span data-ttu-id="c1fff-113">Povolit aplikaci Samoobslužné služby přístup tooallow uživatelé toofind svých vlastních aplikacích</span><span class="sxs-lookup"><span data-stu-id="c1fff-113">Enable self-service application access tooallow users toofind their own applications</span></span>

<span data-ttu-id="c1fff-114">Přístup k aplikaci Samoobslužné služby je skvělý způsob tooallow uživatelé tooself-zjišťovat aplikace, můžete také povolit hello firmy skupiny tooapprove přístup toothose aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1fff-114">Self-service application access is a great way tooallow users tooself-discover applications, optionally allow hello business group tooapprove access toothose applications.</span></span> <span data-ttu-id="c1fff-115">Můžete povolit, že pověření hello hello firmy skupiny toomanage přiřadili toothose uživatelů pro heslo jednotné přihlašování v aplikacích přímo z jejich panelů přístup.</span><span class="sxs-lookup"><span data-stu-id="c1fff-115">You can allow hello business group toomanage hello credentials assigned toothose users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="c1fff-116">tooenable aplikace Samoobslužné služby přístup tooan aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="c1fff-116">tooenable self-service application access tooan application, follow hello steps below:</span></span>

1.  <span data-ttu-id="c1fff-117">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="c1fff-117">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="c1fff-118">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="c1fff-118">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c1fff-119">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="c1fff-119">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c1fff-120">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="c1fff-120">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="c1fff-121">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="c1fff-121">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="c1fff-122">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="c1fff-122">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="c1fff-123">Vyberte aplikaci hello chcete tooenable samoobslužné služby přístup toofrom hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="c1fff-123">Select hello application you want tooenable Self-service access toofrom hello list.</span></span>

7.  <span data-ttu-id="c1fff-124">Po načtení hello aplikace, klikněte na **samoobslužné služby** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="c1fff-124">Once hello application loads, click **Self-service** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="c1fff-125">tooenable přístup k aplikaci Samoobslužné služby pro tuto aplikaci, aktivujte hello **povolit uživatelům toorequest přístup toothis aplikace?** přepnutí příliš**Ano.**</span><span class="sxs-lookup"><span data-stu-id="c1fff-125">tooenable Self-service application access for this application, turn hello **Allow users toorequest access toothis application?** toggle too**Yes.**</span></span>

9.  <span data-ttu-id="c1fff-126">V dalším kroku tooselect hello skupiny toowhich uživatelů, kteří požadují přístup toothis aplikace by měla být přidány, klikněte na tlačítko hello selektor další toohello popisek **toowhich skupina by měla přiřazená byli přidáni uživatelé?** a vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="c1fff-126">Next, tooselect hello group toowhich users who request access toothis application should be added, click hello selector next toohello label **toowhich group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="c1fff-127">**Volitelné:** nechcete-li toorequire obchodní schválení předtím, než mohou uživatelé přístup, nastavte hello **vyžadovat schválení před udělením přístupu toothis aplikace?** přepnutí příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="c1fff-127">**Optional:** If you wish toorequire a business approval before users are allowed access, set hello **Require approval before granting access toothis application?** toggle too**Yes**.</span></span>

11. <span data-ttu-id="c1fff-128">**Volitelné: pro aplikace pomocí hesla jednotné přihlašování na pouze** Pokud chcete tyto firmy schvalovatelů toospecify hello hesel, která se posílají toothis žádost o schválení uživatelé tooallow, nastavte hello **tooset schvalovatelů povolit hesla uživatele pro tuto aplikaci?**  přepnutí příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="c1fff-128">**Optional: For applications using password single-sign on only,** if you wish tooallow those business approvers toospecify hello passwords that are sent toothis application for approved users, set hello **Allow approvers tooset user’s passwords for this application?** toggle too**Yes**.</span></span>

12. <span data-ttu-id="c1fff-129">**Volitelné:** toospecify hello firmy schvalovatelů, kteří mají povoleno tooapprove přístup toothis aplikace, klikněte na tlačítko hello selektor další toohello popisek **kdo je povolená tooapprove přístup toothis aplikace?** tooselect nahoru too10 jednotlivé obchodní schvalovatelů.</span><span class="sxs-lookup"><span data-stu-id="c1fff-129">**Optional:** toospecify hello business approvers who are allowed tooapprove access toothis application, click hello selector next toohello label **Who is allowed tooapprove access toothis application?** tooselect up too10 individual business approvers.</span></span>

   * <span data-ttu-id="c1fff-130">Skupiny nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="c1fff-130">Groups are not supported.</span></span>

13. <span data-ttu-id="c1fff-131">**Volitelné:** **pro aplikace, které zveřejňují role**, pokud chcete, aby role tooa tooassign schválené uživatelé samoobslužné služby, klikněte na tlačítko Další toohello hello selektor **toowhich role lze přiřadit uživatelům v této aplikace?**  tooselect hello role toowhich by měla být přiřazená tyto uživatele.</span><span class="sxs-lookup"><span data-stu-id="c1fff-131">**Optional:** **For applications which expose roles**, if you wish tooassign self-service approved users tooa role, click hello selector next toohello **toowhich role should users be assigned in this application?** tooselect hello role toowhich these users should be assigned.</span></span>

14. <span data-ttu-id="c1fff-132">Klikněte na tlačítko hello **Uložit** tlačítko hello horní části okna toofinish hello.</span><span class="sxs-lookup"><span data-stu-id="c1fff-132">Click hello **Save** button at hello top of hello blade toofinish.</span></span>

<span data-ttu-id="c1fff-133">Po dokončení konfigurace samoobslužné služby aplikace, uživatelé mohou přejít tootheir [Panel přístupu aplikace](https://myapps.microsoft.com/) a klikněte na tlačítko hello **+ přidat** tlačítko toofind hello aplikace toowhich jste povolili Samoobslužné služby přístup.</span><span class="sxs-lookup"><span data-stu-id="c1fff-133">Once you complete Self-service application configuration, users can navigate tootheir [Application Access Panel](https://myapps.microsoft.com/) and click hello **+Add** button toofind hello apps toowhich you have enabled Self-service access.</span></span> <span data-ttu-id="c1fff-134">Obchodní schvalovatelů také zobrazit oznámení v jejich [Panel přístupu aplikace](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="c1fff-134">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="c1fff-135">Můžete povolit e-mail s upozorněním, když uživatel požaduje přístup tooan aplikace, která vyžaduje schválení.</span><span class="sxs-lookup"><span data-stu-id="c1fff-135">You can enable an email notifying them when a user has requested access tooan application that requires their approval.</span></span> 

<span data-ttu-id="c1fff-136">Tato schválení podporují jeden schválení pracovních pouze, což znamená, že pokud zadáte několik schvalovatelů, jeden schvalovatel může schválit přístup toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1fff-136">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approve access toohello application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1fff-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1fff-137">Next steps</span></span>
[<span data-ttu-id="c1fff-138">Nastavení služby Azure Active Directory pro samoobslužnou správu skupin</span><span class="sxs-lookup"><span data-stu-id="c1fff-138">Setting up Azure Active Directory for self-service group management</span></span>](active-directory-accessmanagement-self-service-group-management.md)
