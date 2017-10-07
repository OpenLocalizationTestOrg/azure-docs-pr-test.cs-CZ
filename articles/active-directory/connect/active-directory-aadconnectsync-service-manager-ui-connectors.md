---
title: "aaaConnectors v hello uživatelského rozhraní Azure AD Synchronization Service Manager | Microsoft Docs"
description: "Porozumět hello konektory karta v hello Synchronization Service Manager pro Azure AD Connect."
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
ms.openlocfilehash: c0969630313178b1e299385b1289360c8f787cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-connectors-with-hello-azure-ad-connect-sync-service-manager"></a><span data-ttu-id="d49a1-103">Pomocí konektorů hello Správce synchronizace služby Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="d49a1-103">Using connectors with hello Azure AD Connect Sync Service Manager</span></span>

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

<span data-ttu-id="d49a1-105">Karta konektory Hello je použité toomanage všechny systémy hello synchronizační modul je připojena k.</span><span class="sxs-lookup"><span data-stu-id="d49a1-105">hello Connectors tab is used toomanage all systems hello sync engine is connected to.</span></span>

## <a name="connector-actions"></a><span data-ttu-id="d49a1-106">Konektor akce</span><span class="sxs-lookup"><span data-stu-id="d49a1-106">Connector actions</span></span>
| <span data-ttu-id="d49a1-107">Akce</span><span class="sxs-lookup"><span data-stu-id="d49a1-107">Action</span></span> | <span data-ttu-id="d49a1-108">Komentář</span><span class="sxs-lookup"><span data-stu-id="d49a1-108">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="d49a1-109">Vytvořit</span><span class="sxs-lookup"><span data-stu-id="d49a1-109">Create</span></span> |<span data-ttu-id="d49a1-110">Nepoužívejte.</span><span class="sxs-lookup"><span data-stu-id="d49a1-110">Do not use.</span></span> <span data-ttu-id="d49a1-111">Pro připojení tooadditional AD doménových struktur, použijte Průvodce instalací hello.</span><span class="sxs-lookup"><span data-stu-id="d49a1-111">For connecting tooadditional AD forests, use hello installation wizard.</span></span> |
| <span data-ttu-id="d49a1-112">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d49a1-112">Properties</span></span> |<span data-ttu-id="d49a1-113">Použít pro filtrování organizační jednotky a domény.</span><span class="sxs-lookup"><span data-stu-id="d49a1-113">Used for domain and OU filtering.</span></span> |
| [<span data-ttu-id="d49a1-114">Odstranění</span><span class="sxs-lookup"><span data-stu-id="d49a1-114">Delete</span></span>](#delete) |<span data-ttu-id="d49a1-115">Použít tooeither odstranit data hello hello konektoru místo nebo toodelete připojení tooa doménovou strukturu.</span><span class="sxs-lookup"><span data-stu-id="d49a1-115">Used tooeither delete hello data in hello connector space or toodelete connection tooa forest.</span></span> |
| [<span data-ttu-id="d49a1-116">Konfigurace profilů spuštění</span><span class="sxs-lookup"><span data-stu-id="d49a1-116">Configure Run Profiles</span></span>](#configure-run-profiles) |<span data-ttu-id="d49a1-117">S výjimkou domény filtrování, nic tooconfigure sem.</span><span class="sxs-lookup"><span data-stu-id="d49a1-117">Except for domain filtering, nothing tooconfigure here.</span></span> <span data-ttu-id="d49a1-118">Tuto akci lze použít profily spuštění toosee již nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="d49a1-118">You can use this action toosee already configured run profiles.</span></span> |
| <span data-ttu-id="d49a1-119">Spusťte</span><span class="sxs-lookup"><span data-stu-id="d49a1-119">Run</span></span> |<span data-ttu-id="d49a1-120">Použít toostart výjimečný spuštění profilu.</span><span class="sxs-lookup"><span data-stu-id="d49a1-120">Used toostart a one-off run of a profile.</span></span> |
| <span data-ttu-id="d49a1-121">Zastavit</span><span class="sxs-lookup"><span data-stu-id="d49a1-121">Stop</span></span> |<span data-ttu-id="d49a1-122">Zastaví konektor aktuálně spuštěných profil.</span><span class="sxs-lookup"><span data-stu-id="d49a1-122">Stops a Connector currently running a profile.</span></span> |
| <span data-ttu-id="d49a1-123">Export konektoru</span><span class="sxs-lookup"><span data-stu-id="d49a1-123">Export Connector</span></span> |<span data-ttu-id="d49a1-124">Nepoužívejte.</span><span class="sxs-lookup"><span data-stu-id="d49a1-124">Do not use.</span></span> |
| <span data-ttu-id="d49a1-125">Konektor pro import</span><span class="sxs-lookup"><span data-stu-id="d49a1-125">Import Connector</span></span> |<span data-ttu-id="d49a1-126">Nepoužívejte.</span><span class="sxs-lookup"><span data-stu-id="d49a1-126">Do not use.</span></span> |
| <span data-ttu-id="d49a1-127">Aktualizace konektoru</span><span class="sxs-lookup"><span data-stu-id="d49a1-127">Update Connector</span></span> |<span data-ttu-id="d49a1-128">Nepoužívejte.</span><span class="sxs-lookup"><span data-stu-id="d49a1-128">Do not use.</span></span> |
| <span data-ttu-id="d49a1-129">Aktualizovat schéma</span><span class="sxs-lookup"><span data-stu-id="d49a1-129">Refresh Schema</span></span> |<span data-ttu-id="d49a1-130">Aktualizuje hello uložené v mezipaměti schématu.</span><span class="sxs-lookup"><span data-stu-id="d49a1-130">Refreshes hello cached schema.</span></span> <span data-ttu-id="d49a1-131">Jedná se upřednostňované toouse hello možnost v Průvodci instalací hello místo, od které také aktualizace synchronizovat pravidla.</span><span class="sxs-lookup"><span data-stu-id="d49a1-131">It is preferred toouse hello option in hello installation wizard instead, since that also updates sync rules.</span></span> |
| [<span data-ttu-id="d49a1-132">Hledání prostoru konektoru</span><span class="sxs-lookup"><span data-stu-id="d49a1-132">Search Connector Space</span></span>](#search-connector-space) |<span data-ttu-id="d49a1-133">Použít toofind objekty a příliš[podle objektu a jeho data prostřednictvím systému hello](#follow-an-object-and-its-data-through-the-system).</span><span class="sxs-lookup"><span data-stu-id="d49a1-133">Used toofind objects and too[Follow an object and its data through hello system](#follow-an-object-and-its-data-through-the-system).</span></span> |

### <a name="delete"></a><span data-ttu-id="d49a1-134">Odstranění</span><span class="sxs-lookup"><span data-stu-id="d49a1-134">Delete</span></span>
<span data-ttu-id="d49a1-135">akci odstranění Hello se používá pro dvě různé věci.</span><span class="sxs-lookup"><span data-stu-id="d49a1-135">hello delete action is used for two different things.</span></span>  
![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

<span data-ttu-id="d49a1-137">Hello možnost **odstranit pouze prostor konektoru** odeberete všechna data, ale zachovat hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d49a1-137">hello option **Delete connector space only** removes all data, but keep hello configuration.</span></span>

<span data-ttu-id="d49a1-138">Hello možnost **odstranit konektor a konektor místo** odebere hello dat a konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="d49a1-138">hello option **Delete Connector and connector space** removes hello data and hello configuration.</span></span> <span data-ttu-id="d49a1-139">Tato možnost se používá, když nechcete, aby tooconnect tooa doménové struktury už.</span><span class="sxs-lookup"><span data-stu-id="d49a1-139">This option is used when you do not want tooconnect tooa forest anymore.</span></span>

<span data-ttu-id="d49a1-140">Obě možnosti synchronizovat všechny objekty a aktualizovat objekty metaverse hello.</span><span class="sxs-lookup"><span data-stu-id="d49a1-140">Both options sync all objects and update hello metaverse objects.</span></span> <span data-ttu-id="d49a1-141">Tato akce je dlouhotrvající operace.</span><span class="sxs-lookup"><span data-stu-id="d49a1-141">This action is a long running operation.</span></span>

### <a name="configure-run-profiles"></a><span data-ttu-id="d49a1-142">Konfigurace profilů spuštění</span><span class="sxs-lookup"><span data-stu-id="d49a1-142">Configure Run Profiles</span></span>
<span data-ttu-id="d49a1-143">Tato možnost umožňuje toosee hello profily nakonfigurované pro konektor spuštění.</span><span class="sxs-lookup"><span data-stu-id="d49a1-143">This option allows you toosee hello run profiles configured for a Connector.</span></span>

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a><span data-ttu-id="d49a1-145">Hledání prostoru konektoru</span><span class="sxs-lookup"><span data-stu-id="d49a1-145">Search Connector Space</span></span>
<span data-ttu-id="d49a1-146">Akce místa konektoru vyhledávání Hello je užitečné toofind objekty a data potíže.</span><span class="sxs-lookup"><span data-stu-id="d49a1-146">hello search connector space action is useful toofind objects and troubleshoot data issues.</span></span>

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

<span data-ttu-id="d49a1-148">Začněte výběrem **oboru**.</span><span class="sxs-lookup"><span data-stu-id="d49a1-148">Start by selecting a **scope**.</span></span> <span data-ttu-id="d49a1-149">Můžete hledat na základě dat (relativního Rozlišujícího, rozlišující název, ukotvení podstromu), nebo stavu objektu hello (všechny ostatní možnosti).</span><span class="sxs-lookup"><span data-stu-id="d49a1-149">You can search based on data (RDN, DN, Anchor, Sub-Tree), or state of hello object (all other options).</span></span>  
<span data-ttu-id="d49a1-150">![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)</span><span class="sxs-lookup"><span data-stu-id="d49a1-150">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)</span></span>  
<span data-ttu-id="d49a1-151">Pokud například provedete hledání podstromu, získáte všechny objekty v jedné organizační jednotce.</span><span class="sxs-lookup"><span data-stu-id="d49a1-151">If you for example do a Sub-Tree search, you get all objects in one OU.</span></span>  
<span data-ttu-id="d49a1-152">![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)</span><span class="sxs-lookup"><span data-stu-id="d49a1-152">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)</span></span>  
<span data-ttu-id="d49a1-153">Z této tabulky můžete vybrat objekt, vyberte **vlastnosti**, a [na něho kliknout,](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) z prostoru konektoru zdroj hello, prostřednictvím hello metaverse a prostoru konektoru toohello cíl.</span><span class="sxs-lookup"><span data-stu-id="d49a1-153">From this grid you can select an object, select **properties**, and [follow it](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) from hello source connector space, through hello metaverse, and toohello target connector space.</span></span>

### <a name="changing-hello-ad-ds-account-password"></a><span data-ttu-id="d49a1-154">Změna hesla účtu hello AD DS</span><span class="sxs-lookup"><span data-stu-id="d49a1-154">Changing hello AD DS account password</span></span>
<span data-ttu-id="d49a1-155">Pokud změníte heslo účtu hello, hello synchronizační služba již nebude možné tooimport a exportu změny tooon místní AD.</span><span class="sxs-lookup"><span data-stu-id="d49a1-155">If you change hello account password, hello Synchronization Service will no longer be able tooimport/export changes tooon-premises AD.</span></span>   <span data-ttu-id="d49a1-156">Mohou se zobrazit následující hello:</span><span class="sxs-lookup"><span data-stu-id="d49a1-156">You may see hello following:</span></span>

- <span data-ttu-id="d49a1-157">Hello importu a exportu krok pro hello AD konektoru selže s chybou "bez pověření start".</span><span class="sxs-lookup"><span data-stu-id="d49a1-157">hello import/export step for hello AD connector fails with "no-start-credentials" error.</span></span>
- <span data-ttu-id="d49a1-158">V prohlížeči událostí systému Windows hello protokolu událostí aplikací obsahuje chybu s 6000 ID události a zprávu "hello toorun správy agenta"contoso.com"se nezdařilo, protože pověření hello nebyly platné."</span><span class="sxs-lookup"><span data-stu-id="d49a1-158">Under Windows Event Viewer, hello application event log contains an error with Event ID 6000 and message “hello management agent “contoso.com” failed toorun because hello credentials were invalid.”</span></span>

<span data-ttu-id="d49a1-159">tooresolve hello vydání aktualizace hello služby AD DS uživatelskému účtu pomocí hello následující:</span><span class="sxs-lookup"><span data-stu-id="d49a1-159">tooresolve hello issue, update hello AD DS user account using hello following:</span></span>


1. <span data-ttu-id="d49a1-160">Spusťte hello Synchronization Service Manager (START → synchronizace Service).</span><span class="sxs-lookup"><span data-stu-id="d49a1-160">Start hello Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="d49a1-161">![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="d49a1-161">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>
2. <span data-ttu-id="d49a1-162">Přejděte toohello **konektory** kartě.</span><span class="sxs-lookup"><span data-stu-id="d49a1-162">Go toohello **Connectors** tab.</span></span>
3. <span data-ttu-id="d49a1-163">Vyberte hello AD konektor, který je nakonfigurovaný toouse hello účet služby AD DS.</span><span class="sxs-lookup"><span data-stu-id="d49a1-163">Select hello AD Connector which is configured toouse hello AD DS account.</span></span>
4. <span data-ttu-id="d49a1-164">V části Akce, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="d49a1-164">Under Actions, select **Properties**.</span></span>
5. <span data-ttu-id="d49a1-165">V místním dialogovém okně hello vyberte připojení tooActive Directory doménové struktury:</span><span class="sxs-lookup"><span data-stu-id="d49a1-165">In hello pop-up dialog, select Connect tooActive Directory Forest:</span></span>
6. <span data-ttu-id="d49a1-166">Název doménové struktury Hello označuje hello odpovídající místní AD.</span><span class="sxs-lookup"><span data-stu-id="d49a1-166">hello Forest name indicates hello corresponding on-prem AD.</span></span>
7. <span data-ttu-id="d49a1-167">Uživatelské jméno Hello označuje hello AD DS účet používaný pro synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="d49a1-167">hello User name indicates hello AD DS account used for synchronization.</span></span>
8. <span data-ttu-id="d49a1-168">Zadejte nové heslo hello hello AD DS účtu v textovém poli heslo hello ![Azure AD Connect Sync šifrování klíče nástroj](media/active-directory-aadconnectsync-encryption-key/key6.png)</span><span class="sxs-lookup"><span data-stu-id="d49a1-168">Enter hello new password of hello AD DS account in hello Password textbox ![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span></span>
9. <span data-ttu-id="d49a1-169">Klikněte na tlačítko OK toosave hello nové heslo a restartujte hello synchronizační služba tooremove hello staré heslo z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d49a1-169">Click OK toosave hello new password and restart hello Synchronization Service tooremove hello old password from memory cache.</span></span>



## <a name="next-steps"></a><span data-ttu-id="d49a1-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d49a1-170">Next steps</span></span>
<span data-ttu-id="d49a1-171">Další informace o hello [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d49a1-171">Learn more about hello [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="d49a1-172">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="d49a1-172">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
