---
title: "Problém, přidání aplikace Galerie Azure AD | Microsoft Docs"
description: "Pochopení tučné osoby běžné problémy při přidávání aplikací Galerie Azure AD a jak je vyřešit"
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
ms.openlocfilehash: b3ae472d52208d3c76424d29192c1eb982639572
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a><span data-ttu-id="48f6e-103">Problém, přidání aplikace Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="48f6e-103">Problem adding an Azure AD Gallery application</span></span>

<span data-ttu-id="48f6e-104">Tento článek vám pomůže lépe porozumět tučné osoby běžné problémy při přidávání aplikací Galerie Azure AD a jak je vyřešit.</span><span class="sxs-lookup"><span data-stu-id="48f6e-104">This article help you to understand the common problems people face when adding Azure AD Gallery applications and what you can do to resolve them.</span></span>

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a><span data-ttu-id="48f6e-105">Po klepnutí na tlačítko "Přidat" a Moje aplikace trvalo dlouhou dobu, než se objeví</span><span class="sxs-lookup"><span data-stu-id="48f6e-105">I clicked the “add” button and my application took a long time to appear</span></span>

<span data-ttu-id="48f6e-106">Za určitých okolností může trvat 1 – 2 minutách (a někdy delší) pro aplikace se objeví po přidání do vašeho adresáře.</span><span class="sxs-lookup"><span data-stu-id="48f6e-106">Under some circumstances, it can take 1-2 minutes (and sometimes longer) for an application to appear after adding it to your directory.</span></span> <span data-ttu-id="48f6e-107">Přestože to není normální očekávaný výkon, můžete zjistit, přidání aplikace je v průběhu kliknutím na **oznámení** ikonu (zvonku) v pravém horním rohu stránky [portálu Azure](https://portal.azure.com/) a vyhledávání pro **probíhá** nebo **dokončeno** oznámení s názvem bez přípony **vytvoření aplikace.**</span><span class="sxs-lookup"><span data-stu-id="48f6e-107">While this is not the normal expected performance, you can see the application addition is in progress by clicking on the **Notifications** icon (the bell) in the upper right of the [Azure Portal](https://portal.azure.com/) and looking for an **In Progress** or **Completed** notification labeled **Create application.**</span></span>

<span data-ttu-id="48f6e-108">Pokud vaše aplikace se nikdy přidá, nebo dojde k chybě při kliknutí na **přidat** tlačítko se zobrazí **oznámení** v **chyba** stavu.</span><span class="sxs-lookup"><span data-stu-id="48f6e-108">If your application is never added, or you encounter an error when clicking the **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="48f6e-109">Pokud chcete další informace o této chybě Další informace o nebo ke sdílení s engingeer podpory, zobrazí se další informace o této chybě podle kroků v [postup najdete v části Podrobnosti o portálu oznámení](#how-to-see-the-details-of-a-portal-notification) části.</span><span class="sxs-lookup"><span data-stu-id="48f6e-109">If you want more details about the error to learn more to or share with a support engingeer, you can see more information about the error by following the steps in the [How to see the details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a><span data-ttu-id="48f6e-110">Po klepnutí na tlačítko "Přidat" a nezobrazilo Moje aplikace</span><span class="sxs-lookup"><span data-stu-id="48f6e-110">I clicked the “add” button and my application didn’t appear</span></span>

<span data-ttu-id="48f6e-111">V některých případech kvůli přechodným potížím, problémy se sítí nebo chyby, přidání selhání aplikace.</span><span class="sxs-lookup"><span data-stu-id="48f6e-111">Sometimes, due to transient issues, networking problems, or a bug, adding an application fail.</span></span> <span data-ttu-id="48f6e-112">Můžete zjistit, to se stane, když kliknete **oznámení** ikonu (zvonku) v pravém horním rohu stránky na portálu Azure a v tématu ikonou červený (!) vedle vaše **vytvořit aplikaci** oznámení.</span><span class="sxs-lookup"><span data-stu-id="48f6e-112">You can tell this happens when you click the **Notifications** icon (the bell) in the upper right of the Azure Portal and you see a red (!) icon next to your **Create application** notification.</span></span> <span data-ttu-id="48f6e-113">To znamená, že došlo k chybě při vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="48f6e-113">This indicates there was an error when creating the application.</span></span>

<span data-ttu-id="48f6e-114">Pokud dojde k chybě při kliknutí na **přidat** tlačítko se zobrazí **oznámení** v **chyba** stavu.</span><span class="sxs-lookup"><span data-stu-id="48f6e-114">If you encounter an error when clicking the **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="48f6e-115">Pokud chcete další informace o této chybě Další informace o nebo ke sdílení s engingeer podpory, zobrazí se další informace o této chybě podle kroků v [postup najdete v části Podrobnosti o portálu oznámení](#how-to-see-the-details-of-a-portal-notification) části.</span><span class="sxs-lookup"><span data-stu-id="48f6e-115">If you want more details about the error to learn more to or share with a support engingeer, you can see more information about the error by following the steps in the [How to see the details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

 ## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a><span data-ttu-id="48f6e-116">I nemusíte vědět, jak nastavit Moje aplikace, když jste jej přidali</span><span class="sxs-lookup"><span data-stu-id="48f6e-116">I don’t know how to set up my application once I’ve added it</span></span>

<span data-ttu-id="48f6e-117">Pokud potřebujete pomoc, získávání informací o aplikace, [seznamu kurzy o tom, jak integrovat SaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) článku je vhodná pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="48f6e-117">If you need help learning about applications, the [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) article is a good place to start.</span></span>

<span data-ttu-id="48f6e-118">Kromě toho [knihovna dokumentů aplikace Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) pomoci při další informace o jednotné přihlašování s Azure AD a jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="48f6e-118">In addition to this, the [Azure AD Applications Document Library](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) help you to learn more about single sign-on with Azure AD and how it works.</span></span>

## <a name="how-to-see-the-details-of-a-portal-notification"></a><span data-ttu-id="48f6e-119">Postup najdete v části Podrobnosti o portálu oznámení</span><span class="sxs-lookup"><span data-stu-id="48f6e-119">How to see the details of a portal notification</span></span>

<span data-ttu-id="48f6e-120">Pomocí následujícího postupu můžete zobrazit podrobnosti o portálu oznámení:</span><span class="sxs-lookup"><span data-stu-id="48f6e-120">You can see the details of any portal notification by following the steps below:</span></span>

1.  <span data-ttu-id="48f6e-121">klikněte **oznámení** ikonu (zvonku) v pravém horním rohu stránky na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="48f6e-121">click the **Notifications** icon (the bell) in the upper right of the Azure Portal</span></span>

2.  <span data-ttu-id="48f6e-122">Vyberte všechna oznámení v **chyba** stavu (ty se zobrazí červený (!) vedle je).</span><span class="sxs-lookup"><span data-stu-id="48f6e-122">Select any notification in an **Error** state (those with a red (!) next to them).</span></span>

    >[!NOTE]
    ><span data-ttu-id="48f6e-123">Oznámení v nelze kliknout **úspěšné** nebo **probíhá** stavu.</span><span class="sxs-lookup"><span data-stu-id="48f6e-123">You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
    >
    >

3.  <span data-ttu-id="48f6e-124">Tento otevřený **podrobnosti oznámení** okno.</span><span class="sxs-lookup"><span data-stu-id="48f6e-124">This open the **Notification Details** blade.</span></span>

4.  <span data-ttu-id="48f6e-125">Tyto informace použít sami, abyste porozuměli další podrobnosti o problému.</span><span class="sxs-lookup"><span data-stu-id="48f6e-125">Use this information yourself to understand more details about the problem.</span></span>

5.  <span data-ttu-id="48f6e-126">Pokud stále potřebujete pomoc, můžete také sdílet tyto informace se pracovníka podpory nebo product group získat pomoc s váš problém.</span><span class="sxs-lookup"><span data-stu-id="48f6e-126">If you still need help, you can also share this information with a support engineer or the product group to get help with your problem.</span></span>

6.  <span data-ttu-id="48f6e-127">Klikněte na tlačítko **kopie** **ikonu** napravo od **Chyba kopírování** textbox zkopírovat všechny podrobnosti oznámení sdílet s skupiny pracovník podpory nebo produkt</span><span class="sxs-lookup"><span data-stu-id="48f6e-127">Click the **copy** **icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer</span></span>

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a><span data-ttu-id="48f6e-128">Získání nápovědy poslat podrobnosti oznámení na pracovníka podpory</span><span class="sxs-lookup"><span data-stu-id="48f6e-128">How to get help by sending notification details to a support engineer</span></span>

<span data-ttu-id="48f6e-129">To je velmi důležité, že můžete sdílet **všechny níže uvedené podrobnosti** s pracovníka podpory, pokud potřebujete pomoc, tak, aby se vám může pomoct rychle.</span><span class="sxs-lookup"><span data-stu-id="48f6e-129">It is very important that you share **all the details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="48f6e-130">Můžete to provést pomocí snadno **uděláte snímek,** nebo kliknutím **ikona chyby kopie**, který se nachází vpravo od **Chyba kopírování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="48f6e-130">You can do this easily by **taking a screenshot,** or by clicking the **Copy error icon**, found to the right of the **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="48f6e-131">Vysvětlení podrobnosti oznámení</span><span class="sxs-lookup"><span data-stu-id="48f6e-131">Notification Details Explained</span></span>

<span data-ttu-id="48f6e-132">Níže se dozvíte více co každý oznámení položky znamená a jsou uvedeny příklady každý z nich.</span><span class="sxs-lookup"><span data-stu-id="48f6e-132">The below explains more what each of the notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="48f6e-133">Základní oznámení položky</span><span class="sxs-lookup"><span data-stu-id="48f6e-133">Essential Notification Items</span></span>

-   <span data-ttu-id="48f6e-134">**Název** – popisný název oznámení.</span><span class="sxs-lookup"><span data-stu-id="48f6e-134">**Title** – the descriptive title of the notification</span></span>

  * <span data-ttu-id="48f6e-135">Příklad – **nastavení proxy aplikace**</span><span class="sxs-lookup"><span data-stu-id="48f6e-135">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="48f6e-136">**Popis** – popis co došlo k chybě v důsledku operaci</span><span class="sxs-lookup"><span data-stu-id="48f6e-136">**Description** – the description of what occurred as a result of the operation</span></span>

    -   <span data-ttu-id="48f6e-137">Příklad – **zadaná interní adresa url je již používán jinou aplikací**</span><span class="sxs-lookup"><span data-stu-id="48f6e-137">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="48f6e-138">**Id oznámení** – jedinečné id oznámení.</span><span class="sxs-lookup"><span data-stu-id="48f6e-138">**Notification Id** – the unique id of the notification</span></span>

    -   <span data-ttu-id="48f6e-139">Příklad – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="48f6e-139">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="48f6e-140">**Id žádosti klienta** – id konkrétní žádosti provedené prohlížeč</span><span class="sxs-lookup"><span data-stu-id="48f6e-140">**Client Request Id** – the specific request id made by your browser</span></span>

    -   <span data-ttu-id="48f6e-141">Příklad – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="48f6e-141">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="48f6e-142">**Čas UTC razítko** – časové razítko, během které oznámení došlo k chybě, v UTC</span><span class="sxs-lookup"><span data-stu-id="48f6e-142">**Time Stamp UTC** – the timestamp during which the notification occurred, in UTC</span></span>

    -   <span data-ttu-id="48f6e-143">Příklad – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="48f6e-143">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="48f6e-144">**Interní Id transakce** – interní ID můžeme použít k vyhledání Chyba v našem systému</span><span class="sxs-lookup"><span data-stu-id="48f6e-144">**Internal Transaction Id** – the internal ID we can use to look the error up in our systems</span></span>

    -   <span data-ttu-id="48f6e-145">Příklad – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="48f6e-145">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="48f6e-146">**Hlavní název uživatele** – uživatel, který provedl operaci</span><span class="sxs-lookup"><span data-stu-id="48f6e-146">**UPN** – the user who performed the operation</span></span>

    -   <span data-ttu-id="48f6e-147">Příklad –**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="48f6e-147">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="48f6e-148">**Id klienta** – jedinečné ID klienta, který uživatel, který provedl operaci byl uživatel členem</span><span class="sxs-lookup"><span data-stu-id="48f6e-148">**Tenant Id** – the unique ID of the tenant that the user who performed the operation was a member of</span></span>

    -   <span data-ttu-id="48f6e-149">Příklad – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="48f6e-149">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="48f6e-150">**Objekt uživatele Id** – jedinečné ID uživatele, který provádí operaci</span><span class="sxs-lookup"><span data-stu-id="48f6e-150">**User object Id** – the unique ID of the user who performed the operation</span></span>

    -   <span data-ttu-id="48f6e-151">Příklad – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="48f6e-151">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="48f6e-152">Podrobné oznámení položky</span><span class="sxs-lookup"><span data-stu-id="48f6e-152">Detailed Notification Items</span></span>

-   <span data-ttu-id="48f6e-153">**Zobrazovaný název** – **(nesmí být prázdné)** podrobnější zobrazovaný název chyby</span><span class="sxs-lookup"><span data-stu-id="48f6e-153">**Display Name** – **(can be empty)** a more detailed display name for the error</span></span>

    -   <span data-ttu-id="48f6e-154">Příklad – **nastavení proxy aplikace**</span><span class="sxs-lookup"><span data-stu-id="48f6e-154">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="48f6e-155">**Stav** – konkrétní stav oznámení.</span><span class="sxs-lookup"><span data-stu-id="48f6e-155">**Status** – the specific status of the notification</span></span>

    -   <span data-ttu-id="48f6e-156">Příklad – **se nezdařilo**</span><span class="sxs-lookup"><span data-stu-id="48f6e-156">Example – **Failed**</span></span>

-   <span data-ttu-id="48f6e-157">**Id objektu** – **(nesmí být prázdné)** ID objektu, pro kterou byla provedena operace</span><span class="sxs-lookup"><span data-stu-id="48f6e-157">**Object Id** – **(can be empty)** the object ID against which the operation was performed</span></span>

    -   <span data-ttu-id="48f6e-158">Příklad – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="48f6e-158">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="48f6e-159">**Podrobnosti o** – podrobný popis co došlo k chybě v důsledku operaci</span><span class="sxs-lookup"><span data-stu-id="48f6e-159">**Details** – the detailed description of what occurred as a result of the operation</span></span>

    -   <span data-ttu-id="48f6e-160">Příklad – **interní adresa url, http://bing.com/' je neplatná, protože je již používán**</span><span class="sxs-lookup"><span data-stu-id="48f6e-160">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="48f6e-161">**Chyba při kopírování** – klikněte na tlačítko **ikona kopírování** napravo od **Chyba kopírování** textbox zkopírovat všechny podrobnosti oznámení sdílet s skupiny pracovník podpory nebo produkt</span><span class="sxs-lookup"><span data-stu-id="48f6e-161">**Copy error** – Click the **copy icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer</span></span>

    -   <span data-ttu-id="48f6e-162">Příklad```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="48f6e-162">Example ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="48f6e-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48f6e-163">Next steps</span></span>
[<span data-ttu-id="48f6e-164">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="48f6e-164">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
