---
title: "Přidání aplikace Azure AD Galerie aaaProblem | Microsoft Docs"
description: "Pochopení hello běžné problémy uživatelé setkávají při přidávání aplikací Galerie Azure AD a co můžete dělat tooresolve je"
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
ms.openlocfilehash: 654f98116176d5590563c0471b92809f8763fbd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a><span data-ttu-id="fe534-103">Problém, přidání aplikace Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="fe534-103">Problem adding an Azure AD Gallery application</span></span>

<span data-ttu-id="fe534-104">Tento článek vám pomůže toounderstand hello běžné problémy uživatelé setkávají při přidávání aplikací Galerie Azure AD a co můžete dělat tooresolve je.</span><span class="sxs-lookup"><span data-stu-id="fe534-104">This article help you toounderstand hello common problems people face when adding Azure AD Gallery applications and what you can do tooresolve them.</span></span>

## <a name="i-clicked-hello-add-button-and-my-application-took-a-long-time-tooappear"></a><span data-ttu-id="fe534-105">Klepnutí na tlačítko "Přidat", tlačítka a Moje aplikace trvalo dlouhou dobu tooappear hello</span><span class="sxs-lookup"><span data-stu-id="fe534-105">I clicked hello “add” button and my application took a long time tooappear</span></span>

<span data-ttu-id="fe534-106">Za určitých okolností může trvat 1 – 2 minutách (a někdy delší) pro tooappear aplikace po jeho přidání tooyour adresáře.</span><span class="sxs-lookup"><span data-stu-id="fe534-106">Under some circumstances, it can take 1-2 minutes (and sometimes longer) for an application tooappear after adding it tooyour directory.</span></span> <span data-ttu-id="fe534-107">Přestože to není hello normální očekávaný výkon, můžete zjistit, přidání aplikace hello je v průběhu kliknutím na hello **oznámení** ikonu (hello zvonku) v hello pravém horním rohu stránky hello [portálu Azure](https://portal.azure.com/)a hledá **probíhá** nebo **dokončeno** oznámení s názvem bez přípony **vytvoření aplikace.**</span><span class="sxs-lookup"><span data-stu-id="fe534-107">While this is not hello normal expected performance, you can see hello application addition is in progress by clicking on hello **Notifications** icon (hello bell) in hello upper right of hello [Azure Portal](https://portal.azure.com/) and looking for an **In Progress** or **Completed** notification labeled **Create application.**</span></span>

<span data-ttu-id="fe534-108">Pokud vaše aplikace se nikdy přidá, nebo dojde k chybě při kliknutí na hello **přidat** tlačítko se zobrazí **oznámení** v **chyba** stavu.</span><span class="sxs-lookup"><span data-stu-id="fe534-108">If your application is never added, or you encounter an error when clicking hello **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="fe534-109">Pokud chcete další informace o toolearn chyba hello další tooor sdílet s engingeer podpory, zobrazí se další informace o chybě hello podle následujících kroků hello v hello [jak toosee hello podrobnosti o portálu oznámení](#how-to-see-the-details-of-a-portal-notification) části.</span><span class="sxs-lookup"><span data-stu-id="fe534-109">If you want more details about hello error toolearn more tooor share with a support engingeer, you can see more information about hello error by following hello steps in hello [How toosee hello details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

## <a name="i-clicked-hello-add-button-and-my-application-didnt-appear"></a><span data-ttu-id="fe534-110">Po klepnutí tlačítko "Přidat" hello a nezobrazilo Moje aplikace</span><span class="sxs-lookup"><span data-stu-id="fe534-110">I clicked hello “add” button and my application didn’t appear</span></span>

<span data-ttu-id="fe534-111">V některých případech z důvodu problémů s tootransient, problémy se sítí nebo chyby, přidání selhání aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe534-111">Sometimes, due tootransient issues, networking problems, or a bug, adding an application fail.</span></span> <span data-ttu-id="fe534-112">Můžete zadat, to se stane, když kliknete na tlačítko hello **oznámení** ikonu (hello zvonku) v hello pravé horní části hello portálu Azure a červený (!) najdete v části Další tooyour ikonu **vytvořit aplikaci** oznámení.</span><span class="sxs-lookup"><span data-stu-id="fe534-112">You can tell this happens when you click hello **Notifications** icon (hello bell) in hello upper right of hello Azure Portal and you see a red (!) icon next tooyour **Create application** notification.</span></span> <span data-ttu-id="fe534-113">To znamená, že došlo k chybě při vytváření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fe534-113">This indicates there was an error when creating hello application.</span></span>

<span data-ttu-id="fe534-114">Pokud dojde k chybě při kliknutí na hello **přidat** tlačítko se zobrazí **oznámení** v **chyba** stavu.</span><span class="sxs-lookup"><span data-stu-id="fe534-114">If you encounter an error when clicking hello **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="fe534-115">Pokud chcete další informace o toolearn chyba hello další tooor sdílet s engingeer podpory, zobrazí se další informace o chybě hello podle následujících kroků hello v hello [jak toosee hello podrobnosti o portálu oznámení](#how-to-see-the-details-of-a-portal-notification) části.</span><span class="sxs-lookup"><span data-stu-id="fe534-115">If you want more details about hello error toolearn more tooor share with a support engingeer, you can see more information about hello error by following hello steps in hello [How toosee hello details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

 ## <a name="i-dont-know-how-tooset-up-my-application-once-ive-added-it"></a><span data-ttu-id="fe534-116">Neznámého jak tooset až po Moje aplikace byly přidány ho</span><span class="sxs-lookup"><span data-stu-id="fe534-116">I don’t know how tooset up my application once I’ve added it</span></span>

<span data-ttu-id="fe534-117">Pokud potřebujete pomoc, získávání informací o aplikace, hello [seznamu kurzy o tooIntegrate SaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) článek je vhodné místo toostart.</span><span class="sxs-lookup"><span data-stu-id="fe534-117">If you need help learning about applications, hello [List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) article is a good place toostart.</span></span>

<span data-ttu-id="fe534-118">V přidání toothis hello [knihovna dokumentů aplikace Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) vám pomůžou toolearn informace o jednotné přihlašování s Azure AD a jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="fe534-118">In addition toothis, hello [Azure AD Applications Document Library](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) help you toolearn more about single sign-on with Azure AD and how it works.</span></span>

## <a name="how-toosee-hello-details-of-a-portal-notification"></a><span data-ttu-id="fe534-119">Jak toosee hello podrobnosti o portálu oznámení</span><span class="sxs-lookup"><span data-stu-id="fe534-119">How toosee hello details of a portal notification</span></span>

<span data-ttu-id="fe534-120">Podle následujících kroků hello, můžete zjistit podrobnosti o hello všechna portálu oznámení:</span><span class="sxs-lookup"><span data-stu-id="fe534-120">You can see hello details of any portal notification by following hello steps below:</span></span>

1.  <span data-ttu-id="fe534-121">Klikněte na tlačítko hello **oznámení** ikonu (hello zvonku) v pravé horní hello části hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fe534-121">click hello **Notifications** icon (hello bell) in hello upper right of hello Azure Portal</span></span>

2.  <span data-ttu-id="fe534-122">Vyberte všechna oznámení v **chyba** stavu (ty s další toothem červený (!)).</span><span class="sxs-lookup"><span data-stu-id="fe534-122">Select any notification in an **Error** state (those with a red (!) next toothem).</span></span>

    >[!NOTE]
    ><span data-ttu-id="fe534-123">Oznámení v nelze kliknout **úspěšné** nebo **probíhá** stavu.</span><span class="sxs-lookup"><span data-stu-id="fe534-123">You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
    >
    >

3.  <span data-ttu-id="fe534-124">Tento otevřený hello **podrobnosti oznámení** okno.</span><span class="sxs-lookup"><span data-stu-id="fe534-124">This open hello **Notification Details** blade.</span></span>

4.  <span data-ttu-id="fe534-125">Tyto informace použijte sami toounderstand další podrobnosti o problému hello.</span><span class="sxs-lookup"><span data-stu-id="fe534-125">Use this information yourself toounderstand more details about hello problem.</span></span>

5.  <span data-ttu-id="fe534-126">Pokud stále potřebujete pomoc, můžete také sdílet tyto informace se na podporu pracovníka nebo hello skupiny tooget Nápověda k produktu s váš problém.</span><span class="sxs-lookup"><span data-stu-id="fe534-126">If you still need help, you can also share this information with a support engineer or hello product group tooget help with your problem.</span></span>

6.  <span data-ttu-id="fe534-127">Klikněte na tlačítko hello **kopie** **ikonu** toohello napravo od hello **Chyba kopírování** textbox toocopy všechny hello tooshare podrobnosti oznámení s skupiny pracovník podpory nebo produkt</span><span class="sxs-lookup"><span data-stu-id="fe534-127">Click hello **copy** **icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer</span></span>

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a><span data-ttu-id="fe534-128">Jak pomoci tooget odesláním pracovníka podpory tooa podrobnosti oznámení</span><span class="sxs-lookup"><span data-stu-id="fe534-128">How tooget help by sending notification details tooa support engineer</span></span>

<span data-ttu-id="fe534-129">To je velmi důležité, že můžete sdílet **všechny níže uvedené podrobnosti o hello** s pracovníka podpory, pokud potřebujete pomoc, tak, aby se vám může pomoct rychle.</span><span class="sxs-lookup"><span data-stu-id="fe534-129">It is very important that you share **all hello details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="fe534-130">Můžete to provést pomocí snadno **uděláte snímek,** nebo kliknutím hello **ikona chyby kopie**, najít toohello napravo od hello **Chyba kopírování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="fe534-130">You can do this easily by **taking a screenshot,** or by clicking hello **Copy error icon**, found toohello right of hello **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="fe534-131">Vysvětlení podrobnosti oznámení</span><span class="sxs-lookup"><span data-stu-id="fe534-131">Notification Details Explained</span></span>

<span data-ttu-id="fe534-132">Hello níže vysvětluje více co každý hello oznámení položek znamená a poskytuje příklady každého z nich.</span><span class="sxs-lookup"><span data-stu-id="fe534-132">hello below explains more what each of hello notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="fe534-133">Základní oznámení položky</span><span class="sxs-lookup"><span data-stu-id="fe534-133">Essential Notification Items</span></span>

-   <span data-ttu-id="fe534-134">**Název** – hello popisný název hello oznámení</span><span class="sxs-lookup"><span data-stu-id="fe534-134">**Title** – hello descriptive title of hello notification</span></span>

  * <span data-ttu-id="fe534-135">Příklad – **nastavení proxy aplikace**</span><span class="sxs-lookup"><span data-stu-id="fe534-135">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="fe534-136">**Popis** – hello popis co došlo k chybě v důsledku operace hello</span><span class="sxs-lookup"><span data-stu-id="fe534-136">**Description** – hello description of what occurred as a result of hello operation</span></span>

    -   <span data-ttu-id="fe534-137">Příklad – **zadaná interní adresa url je již používán jinou aplikací**</span><span class="sxs-lookup"><span data-stu-id="fe534-137">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="fe534-138">**Id oznámení** – hello jedinečné id oznámení hello</span><span class="sxs-lookup"><span data-stu-id="fe534-138">**Notification Id** – hello unique id of hello notification</span></span>

    -   <span data-ttu-id="fe534-139">Příklad – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="fe534-139">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="fe534-140">**Id žádosti klienta** – id konkrétního požadavku hello provedené prohlížeč</span><span class="sxs-lookup"><span data-stu-id="fe534-140">**Client Request Id** – hello specific request id made by your browser</span></span>

    -   <span data-ttu-id="fe534-141">Příklad – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="fe534-141">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="fe534-142">**Čas UTC razítko** – hello časové razítko, během které hello oznámení došlo k chybě, v UTC</span><span class="sxs-lookup"><span data-stu-id="fe534-142">**Time Stamp UTC** – hello timestamp during which hello notification occurred, in UTC</span></span>

    -   <span data-ttu-id="fe534-143">Příklad – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="fe534-143">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="fe534-144">**Interní Id transakce** – hello interní ID můžeme použít toolook hello Chyba v našem systému</span><span class="sxs-lookup"><span data-stu-id="fe534-144">**Internal Transaction Id** – hello internal ID we can use toolook hello error up in our systems</span></span>

    -   <span data-ttu-id="fe534-145">Příklad – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="fe534-145">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="fe534-146">**Hlavní název uživatele** – hello uživatele, který provedl operaci hello</span><span class="sxs-lookup"><span data-stu-id="fe534-146">**UPN** – hello user who performed hello operation</span></span>

    -   <span data-ttu-id="fe534-147">Příklad –**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="fe534-147">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="fe534-148">**Id klienta** – hello jedinečné ID klienta hello, který hello uživatele, který provedl operaci hello byl členem</span><span class="sxs-lookup"><span data-stu-id="fe534-148">**Tenant Id** – hello unique ID of hello tenant that hello user who performed hello operation was a member of</span></span>

    -   <span data-ttu-id="fe534-149">Příklad – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="fe534-149">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="fe534-150">**Objekt uživatele Id** – hello jedinečné ID hello uživatele, který provedl operaci hello</span><span class="sxs-lookup"><span data-stu-id="fe534-150">**User object Id** – hello unique ID of hello user who performed hello operation</span></span>

    -   <span data-ttu-id="fe534-151">Příklad – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="fe534-151">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="fe534-152">Podrobné oznámení položky</span><span class="sxs-lookup"><span data-stu-id="fe534-152">Detailed Notification Items</span></span>

-   <span data-ttu-id="fe534-153">**Zobrazovaný název** – **(nesmí být prázdné)** podrobnější zobrazovaný název pro chybu hello</span><span class="sxs-lookup"><span data-stu-id="fe534-153">**Display Name** – **(can be empty)** a more detailed display name for hello error</span></span>

    -   <span data-ttu-id="fe534-154">Příklad – **nastavení proxy aplikace**</span><span class="sxs-lookup"><span data-stu-id="fe534-154">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="fe534-155">**Stav** – hello konkrétní stav hello oznámení</span><span class="sxs-lookup"><span data-stu-id="fe534-155">**Status** – hello specific status of hello notification</span></span>

    -   <span data-ttu-id="fe534-156">Příklad – **se nezdařilo**</span><span class="sxs-lookup"><span data-stu-id="fe534-156">Example – **Failed**</span></span>

-   <span data-ttu-id="fe534-157">**Id objektu** – **(nesmí být prázdné)** hello ID objektu, podle které hello byla provedena operace</span><span class="sxs-lookup"><span data-stu-id="fe534-157">**Object Id** – **(can be empty)** hello object ID against which hello operation was performed</span></span>

    -   <span data-ttu-id="fe534-158">Příklad – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="fe534-158">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="fe534-159">**Podrobnosti o** – hello podrobný popis co došlo k chybě v důsledku operace hello</span><span class="sxs-lookup"><span data-stu-id="fe534-159">**Details** – hello detailed description of what occurred as a result of hello operation</span></span>

    -   <span data-ttu-id="fe534-160">Příklad – **interní adresa url, http://bing.com/' je neplatná, protože je již používán**</span><span class="sxs-lookup"><span data-stu-id="fe534-160">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="fe534-161">**Chyba při kopírování** – klikněte na tlačítko hello **ikona kopírování** toohello napravo od hello **Chyba kopírování** textbox toocopy všechny hello tooshare podrobnosti oznámení s skupiny pracovník podpory nebo produkt</span><span class="sxs-lookup"><span data-stu-id="fe534-161">**Copy error** – Click hello **copy icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer</span></span>

    -   <span data-ttu-id="fe534-162">Příklad```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="fe534-162">Example ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe534-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe534-163">Next steps</span></span>
[<span data-ttu-id="fe534-164">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fe534-164">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
