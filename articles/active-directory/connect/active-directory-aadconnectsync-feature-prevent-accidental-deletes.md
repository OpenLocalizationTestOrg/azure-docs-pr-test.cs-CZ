---
title: "Synchronizace Azure AD Connect: prevence náhodného odstranění | Microsoft Docs"
description: "Toto téma popisuje funkci prevence náhodného odstranění (prevence náhodného odstranění) ve službě Azure AD Connect."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b852cb4-2850-40a1-8280-8724081601f7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: a33fb729cff5007e40820af696cfec823a3ecfde
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a><span data-ttu-id="6fcde-103">Synchronizace Azure AD Connect: Prevence náhodného odstranění</span><span class="sxs-lookup"><span data-stu-id="6fcde-103">Azure AD Connect sync: Prevent accidental deletes</span></span>
<span data-ttu-id="6fcde-104">Toto téma popisuje funkci prevence náhodného odstranění (prevence náhodného odstranění) ve službě Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="6fcde-104">This topic describes the prevent accidental deletes (preventing accidental deletions) feature in Azure AD Connect.</span></span>

<span data-ttu-id="6fcde-105">Při instalaci Azure AD Connect, zabránit náhodnému odstranění je ve výchozím nastavení povolené a nakonfigurované tak, aby nepovoloval exportu s více než 500 odstranění.</span><span class="sxs-lookup"><span data-stu-id="6fcde-105">When installing Azure AD Connect, prevent accidental deletes is enabled by default and configured to not allow an export with more than 500 deletes.</span></span> <span data-ttu-id="6fcde-106">Tato funkce slouží k ochraně před náhodným konfigurace změny a změny do vašeho místního adresáře, které má vliv na mnoho uživatelů a dalších objektů.</span><span class="sxs-lookup"><span data-stu-id="6fcde-106">This feature is designed to protect you from accidental configuration changes and changes to your on-premises directory that would affect many users and other objects.</span></span>

## <a name="what-is-prevent-accidental-deletes"></a><span data-ttu-id="6fcde-107">Co je prevence náhodného odstranění</span><span class="sxs-lookup"><span data-stu-id="6fcde-107">What is prevent accidental deletes</span></span>
<span data-ttu-id="6fcde-108">Běžné scénáře až uvidíte mnoho odstranění patří:</span><span class="sxs-lookup"><span data-stu-id="6fcde-108">Common scenarios when you see many deletes include:</span></span>

* <span data-ttu-id="6fcde-109">Změny [filtrování](active-directory-aadconnectsync-configure-filtering.md) kde celou [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) nebo [domény](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) nezaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="6fcde-109">Changes to [filtering](active-directory-aadconnectsync-configure-filtering.md) where an entire [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) or [domain](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) is unselected.</span></span>
* <span data-ttu-id="6fcde-110">Všechny objekty v organizační jednotce se odstraní.</span><span class="sxs-lookup"><span data-stu-id="6fcde-110">All objects in an OU are deleted.</span></span>
* <span data-ttu-id="6fcde-111">Organizační jednotka se přejmenuje tak všechny objekty v ní se považují za mimo rozsah pro synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="6fcde-111">An OU is renamed so all objects in it are considered to be out of scope for synchronization.</span></span>

<span data-ttu-id="6fcde-112">Výchozí hodnota 500 objektů lze změnit pomocí prostředí PowerShell pomocí `Enable-ADSyncExportDeletionThreshold`.</span><span class="sxs-lookup"><span data-stu-id="6fcde-112">The default value of 500 objects can be changed with PowerShell using `Enable-ADSyncExportDeletionThreshold`.</span></span> <span data-ttu-id="6fcde-113">Měli byste nakonfigurovat tuto hodnotu, aby odpovídal zadané velikosti vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="6fcde-113">You should configure this value to fit the size of your organization.</span></span> <span data-ttu-id="6fcde-114">Vzhledem k tomu, že plánovače synchronizace spouští každých 30 minut, je hodnota číslo z odstranění vidět do 30 minut.</span><span class="sxs-lookup"><span data-stu-id="6fcde-114">Since the sync scheduler runs every 30 minutes, the value is the number of deletes seen within 30 minutes.</span></span>

<span data-ttu-id="6fcde-115">Pokud jsou moc odstranění připravený být exportovány do služby Azure AD, poté se zastaví exportu a obdržíte e-mail takto:</span><span class="sxs-lookup"><span data-stu-id="6fcde-115">If there are too many deletes staged to be exported to Azure AD, then the export stops and you receive an email like this:</span></span>

![Zabránit náhodného odstranění e-mailu](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> <span data-ttu-id="6fcde-117">*Hello (technický kontakt). V (čas) synchronizační služba Identity zjistil, že počet odstranění překročena prahová hodnota odstranění nakonfigurované pro (název organizace). Pro odstranění v této synchronizace identit spustit byly odeslány z (number) Celkový počet objektů. To splňuje nebo překračuje prahovou hodnotu nakonfigurované odstranění objektů (number). Potřebujeme, abyste k poskytování potvrzení, že tyto odstraněním měla by být zpracována před jsme bude pokračovat. Podrobnosti viz zabránilo náhodným odstraněním další informace o chybě uvedených v této e-mailové zprávy.*</span><span class="sxs-lookup"><span data-stu-id="6fcde-117">*Hello (technical contact). At (time) the Identity synchronization service detected that the number of deletions exceeded the configured deletion threshold for (organization name). A total of (number) objects were sent for deletion in this Identity synchronization run. This met or exceeded the configured deletion threshold value of (number) objects. We need you to provide confirmation that these deletions should be processed before we will proceed. Please see the preventing accidental deletions for more information about the error listed in this email message.*</span></span>
>
> 

<span data-ttu-id="6fcde-118">Stav můžete také zobrazit `stopped-deletion-threshold-exceeded` když se podíváte **Synchronization Service Manager** uživatelského rozhraní pro Export profil.</span><span class="sxs-lookup"><span data-stu-id="6fcde-118">You can also see the status `stopped-deletion-threshold-exceeded` when you look in the **Synchronization Service Manager** UI for the Export profile.</span></span>
<span data-ttu-id="6fcde-119">![Prevence náhodného odstranění uživatelského rozhraní Správce služby synchronizace](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)</span><span class="sxs-lookup"><span data-stu-id="6fcde-119">![Prevent Accidental deletes Sync Service Manager UI](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)</span></span>

<span data-ttu-id="6fcde-120">Pokud jste to neočekávané, prozkoumejte a provést opravné akce.</span><span class="sxs-lookup"><span data-stu-id="6fcde-120">If this was unexpected, then investigate and take corrective actions.</span></span> <span data-ttu-id="6fcde-121">Pokud chcete zobrazit, které objekty se mají odstranit, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="6fcde-121">To see which objects are about to be deleted, do the following:</span></span>

1. <span data-ttu-id="6fcde-122">Spustit **synchronizační služba** z nabídky Start.</span><span class="sxs-lookup"><span data-stu-id="6fcde-122">Start **Synchronization Service** from the Start Menu.</span></span>
2. <span data-ttu-id="6fcde-123">Přejděte na **konektory**.</span><span class="sxs-lookup"><span data-stu-id="6fcde-123">Go to **Connectors**.</span></span>
3. <span data-ttu-id="6fcde-124">Vyberte konektor s typem **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6fcde-124">Select the Connector with type **Azure Active Directory**.</span></span>
4. <span data-ttu-id="6fcde-125">V části **akce** napravo, vyberte **hledání prostoru konektoru**.</span><span class="sxs-lookup"><span data-stu-id="6fcde-125">Under **Actions** to the right, select **Search Connector Space**.</span></span>
5. <span data-ttu-id="6fcde-126">V místní nabídce v části **oboru**, vyberte **odpojen od** a vyberte čas v minulosti.</span><span class="sxs-lookup"><span data-stu-id="6fcde-126">In the pop-up under **Scope**, select **Disconnected Since** and pick a time in the past.</span></span> <span data-ttu-id="6fcde-127">Klikněte na tlačítko **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="6fcde-127">Click **Search**.</span></span> <span data-ttu-id="6fcde-128">Tato stránka obsahuje zobrazení všech objektů, které má být odstraněn.</span><span class="sxs-lookup"><span data-stu-id="6fcde-128">This page provides a view of all objects about to be deleted.</span></span> <span data-ttu-id="6fcde-129">Kliknutím na jednotlivé položky, můžete získat další informace o objektu.</span><span class="sxs-lookup"><span data-stu-id="6fcde-129">By clicking each item, you can get additional information about the object.</span></span> <span data-ttu-id="6fcde-130">Můžete také kliknout na **sloupec nastavení** přidat další atributy, které mají být zobrazeny v mřížce.</span><span class="sxs-lookup"><span data-stu-id="6fcde-130">You can also click **Column Setting** to add additional attributes to be visible in the grid.</span></span>

![Hledání prostoru konektoru](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

<span data-ttu-id="6fcde-132">V případě potřeby všechna odstranění se potom postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="6fcde-132">If all the deletes are desired, then do the following:</span></span>

1. <span data-ttu-id="6fcde-133">Chcete-li získat aktuální prahová hodnota odstranění, spusťte rutinu prostředí PowerShell `Get-ADSyncExportDeletionThreshold`.</span><span class="sxs-lookup"><span data-stu-id="6fcde-133">To retrieve the current deletion threshold, run the PowerShell cmdlet `Get-ADSyncExportDeletionThreshold`.</span></span> <span data-ttu-id="6fcde-134">Zadejte účet Azure AD globálního správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="6fcde-134">Provide an Azure AD Global Administrator account and password.</span></span> <span data-ttu-id="6fcde-135">Výchozí hodnota je 500.</span><span class="sxs-lookup"><span data-stu-id="6fcde-135">The default value is 500.</span></span>
2. <span data-ttu-id="6fcde-136">K dočasnému zakázání této ochrany a nechat tyto odstranění projít, spusťte rutinu prostředí PowerShell: `Disable-ADSyncExportDeletionThreshold`.</span><span class="sxs-lookup"><span data-stu-id="6fcde-136">To temporarily disable this protection and let those deletes go through, run the PowerShell cmdlet: `Disable-ADSyncExportDeletionThreshold`.</span></span> <span data-ttu-id="6fcde-137">Zadejte účet Azure AD globálního správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="6fcde-137">Provide an Azure AD Global Administrator account and password.</span></span>
   <span data-ttu-id="6fcde-138">![Přihlašovací údaje](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)</span><span class="sxs-lookup"><span data-stu-id="6fcde-138">![Credentials](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)</span></span>
3. <span data-ttu-id="6fcde-139">Pomocí Azure konektor služby Active Directory stále vybrána, vyberte akci, která **spustit** a vyberte **exportovat**.</span><span class="sxs-lookup"><span data-stu-id="6fcde-139">With the Azure Active Directory Connector still selected, select the action **Run** and select **Export**.</span></span>
4. <span data-ttu-id="6fcde-140">Chcete-li znovu povolit ochranu, spusťte rutinu prostředí PowerShell: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`.</span><span class="sxs-lookup"><span data-stu-id="6fcde-140">To re-enable the protection, run the PowerShell cmdlet: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`.</span></span> <span data-ttu-id="6fcde-141">Nahraďte hodnotu, kterou jste si všimli při načítání aktuální prahová hodnota odstranění 500.</span><span class="sxs-lookup"><span data-stu-id="6fcde-141">Replace 500 with the value you noticed when retrieving the current deletion threshold.</span></span> <span data-ttu-id="6fcde-142">Zadejte účet Azure AD globálního správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="6fcde-142">Provide an Azure AD Global Administrator account and password.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6fcde-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6fcde-143">Next steps</span></span>
<span data-ttu-id="6fcde-144">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="6fcde-144">**Overview topics**</span></span>

* [<span data-ttu-id="6fcde-145">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="6fcde-145">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="6fcde-146">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6fcde-146">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
