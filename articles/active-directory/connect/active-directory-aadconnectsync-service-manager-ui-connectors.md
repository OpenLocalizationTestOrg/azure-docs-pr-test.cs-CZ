---
title: "Konektory v uživatelském rozhraní Azure AD Synchronization Service Manager | Microsoft Docs"
description: "Porozumět kartě konektory ve Správci služby synchronizace Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0fae4b1755ca95466eeffb5ce61c1c7855d7381
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="using-connectors-with-the-azure-ad-connect-sync-service-manager"></a><span data-ttu-id="96f67-103">Použití konektorů s Azure AD Connect Sync Správce služeb</span><span class="sxs-lookup"><span data-stu-id="96f67-103">Using connectors with the Azure AD Connect Sync Service Manager</span></span>

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

<span data-ttu-id="96f67-105">Na kartě konektory slouží ke správě všech systémů, synchronizační modul je připojena k.</span><span class="sxs-lookup"><span data-stu-id="96f67-105">The Connectors tab is used to manage all systems the sync engine is connected to.</span></span>

## <a name="connector-actions"></a><span data-ttu-id="96f67-106">Konektor akce</span><span class="sxs-lookup"><span data-stu-id="96f67-106">Connector actions</span></span>
| <span data-ttu-id="96f67-107">Akce</span><span class="sxs-lookup"><span data-stu-id="96f67-107">Action</span></span> | <span data-ttu-id="96f67-108">Komentář</span><span class="sxs-lookup"><span data-stu-id="96f67-108">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="96f67-109">Vytvořit</span><span class="sxs-lookup"><span data-stu-id="96f67-109">Create</span></span> |<span data-ttu-id="96f67-110">Nepoužívejte.</span><span class="sxs-lookup"><span data-stu-id="96f67-110">Do not use.</span></span> <span data-ttu-id="96f67-111">Pro připojení k další doménové struktury AD, použijte Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="96f67-111">For connecting to additional AD forests, use the installation wizard.</span></span> |
| <span data-ttu-id="96f67-112">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="96f67-112">Properties</span></span> |<span data-ttu-id="96f67-113">Použít pro filtrování organizační jednotky a domény.</span><span class="sxs-lookup"><span data-stu-id="96f67-113">Used for domain and OU filtering.</span></span> |
| [<span data-ttu-id="96f67-114">Odstranění</span><span class="sxs-lookup"><span data-stu-id="96f67-114">Delete</span></span>](#delete) |<span data-ttu-id="96f67-115">Používá se buď odstranit data v prostoru konektoru nebo odstranit připojení k doménové struktuře.</span><span class="sxs-lookup"><span data-stu-id="96f67-115">Used to either delete the data in the connector space or to delete connection to a forest.</span></span> |
| [<span data-ttu-id="96f67-116">Konfigurace profilů spuštění</span><span class="sxs-lookup"><span data-stu-id="96f67-116">Configure Run Profiles</span></span>](#configure-run-profiles) |<span data-ttu-id="96f67-117">S výjimkou domény filtrování, co zde konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="96f67-117">Except for domain filtering, nothing to configure here.</span></span> <span data-ttu-id="96f67-118">Tato akce můžete použít zobrazíte už nakonfigurované profilů spuštění.</span><span class="sxs-lookup"><span data-stu-id="96f67-118">You can use this action to see already configured run profiles.</span></span> |
| <span data-ttu-id="96f67-119">Spuštění</span><span class="sxs-lookup"><span data-stu-id="96f67-119">Run</span></span> |<span data-ttu-id="96f67-120">Používá ke spuštění jednorázové spuštění profilu.</span><span class="sxs-lookup"><span data-stu-id="96f67-120">Used to start a one-off run of a profile.</span></span> |
| <span data-ttu-id="96f67-121">Zastavit</span><span class="sxs-lookup"><span data-stu-id="96f67-121">Stop</span></span> |<span data-ttu-id="96f67-122">Zastaví konektor aktuálně spuštěných profil.</span><span class="sxs-lookup"><span data-stu-id="96f67-122">Stops a Connector currently running a profile.</span></span> |
| <span data-ttu-id="96f67-123">Export konektoru</span><span class="sxs-lookup"><span data-stu-id="96f67-123">Export Connector</span></span> |<span data-ttu-id="96f67-124">Nepoužívejte.</span><span class="sxs-lookup"><span data-stu-id="96f67-124">Do not use.</span></span> |
| <span data-ttu-id="96f67-125">Konektor pro import</span><span class="sxs-lookup"><span data-stu-id="96f67-125">Import Connector</span></span> |<span data-ttu-id="96f67-126">Nepoužívejte.</span><span class="sxs-lookup"><span data-stu-id="96f67-126">Do not use.</span></span> |
| <span data-ttu-id="96f67-127">Aktualizace konektoru</span><span class="sxs-lookup"><span data-stu-id="96f67-127">Update Connector</span></span> |<span data-ttu-id="96f67-128">Nepoužívejte.</span><span class="sxs-lookup"><span data-stu-id="96f67-128">Do not use.</span></span> |
| <span data-ttu-id="96f67-129">Aktualizovat schéma</span><span class="sxs-lookup"><span data-stu-id="96f67-129">Refresh Schema</span></span> |<span data-ttu-id="96f67-130">Aktualizuje v mezipaměti schématu.</span><span class="sxs-lookup"><span data-stu-id="96f67-130">Refreshes the cached schema.</span></span> <span data-ttu-id="96f67-131">Je upřednostňovanou možnost v Průvodci instalací místo toho chcete použít, od které také aktualizace synchronizovat pravidla.</span><span class="sxs-lookup"><span data-stu-id="96f67-131">It is preferred to use the option in the installation wizard instead, since that also updates sync rules.</span></span> |
| [<span data-ttu-id="96f67-132">Hledání prostoru konektoru</span><span class="sxs-lookup"><span data-stu-id="96f67-132">Search Connector Space</span></span>](#search-connector-space) |<span data-ttu-id="96f67-133">Použít k vyhledání objektů a [podle objektu a jeho data prostřednictvím systému](#follow-an-object-and-its-data-through-the-system).</span><span class="sxs-lookup"><span data-stu-id="96f67-133">Used to find objects and to [Follow an object and its data through the system](#follow-an-object-and-its-data-through-the-system).</span></span> |

### <a name="delete"></a><span data-ttu-id="96f67-134">Odstranění</span><span class="sxs-lookup"><span data-stu-id="96f67-134">Delete</span></span>
<span data-ttu-id="96f67-135">Akci odstranění se používá pro dvě různé věci.</span><span class="sxs-lookup"><span data-stu-id="96f67-135">The delete action is used for two different things.</span></span>  
![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

<span data-ttu-id="96f67-137">Možnost **odstranit pouze prostor konektoru** odeberete všechna data, ale ponechat konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="96f67-137">The option **Delete connector space only** removes all data, but keep the configuration.</span></span>

<span data-ttu-id="96f67-138">Možnost **odstranit konektor a konektor místo** odebere data a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="96f67-138">The option **Delete Connector and connector space** removes the data and the configuration.</span></span> <span data-ttu-id="96f67-139">Tato možnost se používá, když nechcete připojit k doménové struktuře už.</span><span class="sxs-lookup"><span data-stu-id="96f67-139">This option is used when you do not want to connect to a forest anymore.</span></span>

<span data-ttu-id="96f67-140">Obě možnosti synchronizovat všechny objekty a aktualizovat úložiště metaverse objekty.</span><span class="sxs-lookup"><span data-stu-id="96f67-140">Both options sync all objects and update the metaverse objects.</span></span> <span data-ttu-id="96f67-141">Tato akce je dlouhotrvající operace.</span><span class="sxs-lookup"><span data-stu-id="96f67-141">This action is a long running operation.</span></span>

### <a name="configure-run-profiles"></a><span data-ttu-id="96f67-142">Konfigurace profilů spuštění</span><span class="sxs-lookup"><span data-stu-id="96f67-142">Configure Run Profiles</span></span>
<span data-ttu-id="96f67-143">Tato možnost umožňuje najdete v části profilů spuštění, který je nakonfigurován pro konektor.</span><span class="sxs-lookup"><span data-stu-id="96f67-143">This option allows you to see the run profiles configured for a Connector.</span></span>

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a><span data-ttu-id="96f67-145">Hledání prostoru konektoru</span><span class="sxs-lookup"><span data-stu-id="96f67-145">Search Connector Space</span></span>
<span data-ttu-id="96f67-146">Akce místa konektoru vyhledávání je užitečné k nalezení objektů a dat potíže.</span><span class="sxs-lookup"><span data-stu-id="96f67-146">The search connector space action is useful to find objects and troubleshoot data issues.</span></span>

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

<span data-ttu-id="96f67-148">Začněte výběrem **oboru**.</span><span class="sxs-lookup"><span data-stu-id="96f67-148">Start by selecting a **scope**.</span></span> <span data-ttu-id="96f67-149">Můžete hledat na základě dat (relativního Rozlišujícího, rozlišující název, ukotvení podstromu), nebo stavu objektu (všechny ostatní možnosti).</span><span class="sxs-lookup"><span data-stu-id="96f67-149">You can search based on data (RDN, DN, Anchor, Sub-Tree), or state of the object (all other options).</span></span>  
<span data-ttu-id="96f67-150">![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)</span><span class="sxs-lookup"><span data-stu-id="96f67-150">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)</span></span>  
<span data-ttu-id="96f67-151">Pokud například provedete hledání podstromu, získáte všechny objekty v jedné organizační jednotce.</span><span class="sxs-lookup"><span data-stu-id="96f67-151">If you for example do a Sub-Tree search, you get all objects in one OU.</span></span>  
<span data-ttu-id="96f67-152">![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)</span><span class="sxs-lookup"><span data-stu-id="96f67-152">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)</span></span>  
<span data-ttu-id="96f67-153">Z této tabulky můžete vybrat objekt, vyberte **vlastnosti**, a [na něho kliknout,](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) z prostoru konektoru zdroje, přes úložiště metaverse a do prostoru konektoru cíl.</span><span class="sxs-lookup"><span data-stu-id="96f67-153">From this grid you can select an object, select **properties**, and [follow it](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) from the source connector space, through the metaverse, and to the target connector space.</span></span>

### <a name="changing-the-ad-ds-account-password"></a><span data-ttu-id="96f67-154">Změna hesla účtu služby AD DS</span><span class="sxs-lookup"><span data-stu-id="96f67-154">Changing the AD DS account password</span></span>
<span data-ttu-id="96f67-155">Pokud změníte heslo k účtu, služba synchronizace už nebude moct importu a exportu změny místní AD.</span><span class="sxs-lookup"><span data-stu-id="96f67-155">If you change the account password, the Synchronization Service will no longer be able to import/export changes to on-premises AD.</span></span>   <span data-ttu-id="96f67-156">Může se zobrazit následující:</span><span class="sxs-lookup"><span data-stu-id="96f67-156">You may see the following:</span></span>

- <span data-ttu-id="96f67-157">Krok importu a exportu pro službu AD connector selže s chybou "bez pověření start".</span><span class="sxs-lookup"><span data-stu-id="96f67-157">The import/export step for the AD connector fails with "no-start-credentials" error.</span></span>
- <span data-ttu-id="96f67-158">V prohlížeči událostí systému Windows v protokolu událostí aplikace obsahuje chybu s 6000 ID události a zprávou "agenta pro správu"contoso.com"Nepodařilo se spustit, protože přihlašovací údaje nebyly platné."</span><span class="sxs-lookup"><span data-stu-id="96f67-158">Under Windows Event Viewer, the application event log contains an error with Event ID 6000 and message “The management agent “contoso.com” failed to run because the credentials were invalid.”</span></span>

<span data-ttu-id="96f67-159">Chcete-li problém vyřešit, aktualizujte uživatelský účet služby AD DS pomocí následující:</span><span class="sxs-lookup"><span data-stu-id="96f67-159">To resolve the issue, update the AD DS user account using the following:</span></span>


1. <span data-ttu-id="96f67-160">Spusťte Synchronization Service Manager (Služba synchronizace → START).</span><span class="sxs-lookup"><span data-stu-id="96f67-160">Start the Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="96f67-161">![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="96f67-161">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>
2. <span data-ttu-id="96f67-162">Přejděte na **konektory** kartě.</span><span class="sxs-lookup"><span data-stu-id="96f67-162">Go to the **Connectors** tab.</span></span>
3. <span data-ttu-id="96f67-163">Vyberte konektor AD, která je nakonfigurována pro použití účtu služby AD DS.</span><span class="sxs-lookup"><span data-stu-id="96f67-163">Select the AD Connector which is configured to use the AD DS account.</span></span>
4. <span data-ttu-id="96f67-164">V části Akce, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="96f67-164">Under Actions, select **Properties**.</span></span>
5. <span data-ttu-id="96f67-165">V dialogovém okně automaticky otevírané okno Vyberte možnost připojit k doménové struktuře služby Active Directory:</span><span class="sxs-lookup"><span data-stu-id="96f67-165">In the pop-up dialog, select Connect to Active Directory Forest:</span></span>
6. <span data-ttu-id="96f67-166">Určuje název doménové struktury odpovídající místní AD.</span><span class="sxs-lookup"><span data-stu-id="96f67-166">The Forest name indicates the corresponding on-prem AD.</span></span>
7. <span data-ttu-id="96f67-167">Určuje, uživatelské jméno účtu služby AD DS používán k synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="96f67-167">The User name indicates the AD DS account used for synchronization.</span></span>
8. <span data-ttu-id="96f67-168">Zadejte nové heslo účtu služby AD DS do textového pole hesla ![Azure AD Connect Sync šifrování klíče nástroj](media/active-directory-aadconnectsync-encryption-key/key6.png)</span><span class="sxs-lookup"><span data-stu-id="96f67-168">Enter the new password of the AD DS account in the Password textbox ![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span></span>
9. <span data-ttu-id="96f67-169">Kliknutím na tlačítko OK uložte nové heslo a restartujte synchronizační službu odebrat staré heslo z mezipaměti paměti.</span><span class="sxs-lookup"><span data-stu-id="96f67-169">Click OK to save the new password and restart the Synchronization Service to remove the old password from memory cache.</span></span>



## <a name="next-steps"></a><span data-ttu-id="96f67-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96f67-170">Next steps</span></span>
<span data-ttu-id="96f67-171">Další informace o [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="96f67-171">Learn more about the [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="96f67-172">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="96f67-172">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
