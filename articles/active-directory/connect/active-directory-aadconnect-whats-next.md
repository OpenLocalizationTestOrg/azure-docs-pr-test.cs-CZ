---
title: "Azure AD Connect: Další kroky a jak toomanage Azure AD Connect | Microsoft Docs"
description: "Zjistěte, jak tooextend hello výchozí konfigurace a provozní úlohy pro Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 4404aaff24d54d76b83baca3b331a6a250ba4c03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="next-steps-and-how-toomanage-azure-ad-connect"></a><span data-ttu-id="f9a2d-103">Další kroky a jak toomanage Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="f9a2d-103">Next steps and how toomanage Azure AD Connect</span></span>
<span data-ttu-id="f9a2d-104">Použijte hello provozní postupy v této článku toocustomize Azure Active Directory (Azure AD) připojit toomeet potřebám a požadavkům vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-104">Use hello operational procedures in this article toocustomize Azure Active Directory (Azure AD) Connect toomeet your organization's needs and requirements.</span></span>  

## <a name="add-additional-sync-admins"></a><span data-ttu-id="f9a2d-105">Přidat další synchronizaci admins</span><span class="sxs-lookup"><span data-stu-id="f9a2d-105">Add additional sync admins</span></span>
<span data-ttu-id="f9a2d-106">Ve výchozím nastavení jsou pouze hello uživatele, který hello instalace a místní správce může toomanage hello nainstalovat synchronizační modul.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-106">By default, only hello user who did hello installation and local admins are able toomanage hello installed sync engine.</span></span> <span data-ttu-id="f9a2d-107">Pro možnost tooaccess toobe dalším osobám a spravovat hello synchronizační modul, vyhledat hello skupinu s názvem ADSyncAdmins na místním serveru hello a přidat je toothis skupiny.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-107">For additional people toobe able tooaccess and manage hello sync engine, locate hello group named ADSyncAdmins on hello local server and add them toothis group.</span></span>

## <a name="assign-licenses-tooazure-ad-premium-and-enterprise-mobility-suite-users"></a><span data-ttu-id="f9a2d-108">Přiřazení tooAzure licencí AD Premium a Enterprise Mobility Suite uživatelů</span><span class="sxs-lookup"><span data-stu-id="f9a2d-108">Assign licenses tooAzure AD Premium and Enterprise Mobility Suite users</span></span>
<span data-ttu-id="f9a2d-109">Teď, když uživatelé byly synchronizovány toohello cloudu, je nutné tooassign je licenci, můžete začít s cloudové aplikace, jako je například Office 365.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-109">Now that your users have been synchronized toohello cloud, you need tooassign them a license so they can get going with cloud apps such as Office 365.</span></span>

### <a name="tooassign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a><span data-ttu-id="f9a2d-110">tooassign Azure AD Premium nebo Enterprise Mobility Suite licencí</span><span class="sxs-lookup"><span data-stu-id="f9a2d-110">tooassign an Azure AD Premium or Enterprise Mobility Suite License</span></span>

1. <span data-ttu-id="f9a2d-111">Přihlaste se toohello portálu Azure jako správce.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-111">Sign in toohello Azure portal as an admin.</span></span>
2. <span data-ttu-id="f9a2d-112">Na levé straně hello vyberte **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-112">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="f9a2d-113">Na hello **služby Active Directory** klikněte dvakrát hello adresář, který má hello uživatele můžete určit tooset nahoru.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-113">On hello **Active Directory** page, double-click hello directory that has hello users you want tooset up.</span></span>
4. <span data-ttu-id="f9a2d-114">Hello horní části stránky adresáře hello, vyberte **licence**.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-114">At hello top of hello directory page, select **Licenses**.</span></span>
5. <span data-ttu-id="f9a2d-115">Na hello **licence** vyberte **Active Directory Premium** nebo **Enterprise Mobility Suite**a potom klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-115">On hello **Licenses** page, select **Active Directory Premium** or **Enterprise Mobility Suite**, and then click **Assign**.</span></span>
6. <span data-ttu-id="f9a2d-116">V dialogovém okně hello vyberte možnost uživatelé hello má tooassign licence a potom klikněte na hello zaškrtnutí ikonu toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-116">In hello dialog box, select hello users you want tooassign licenses to, and then click hello check mark icon toosave hello changes.</span></span>

## <a name="verify-hello-scheduled-synchronization-task"></a><span data-ttu-id="f9a2d-117">Ověření úlohy plánované synchronizace hello</span><span class="sxs-lookup"><span data-stu-id="f9a2d-117">Verify hello scheduled synchronization task</span></span>
<span data-ttu-id="f9a2d-118">Použijte hello Azure portálu toocheck hello stav synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-118">Use hello Azure portal toocheck hello status of a synchronization.</span></span>

### <a name="tooverify-hello-scheduled-synchronization-task"></a><span data-ttu-id="f9a2d-119">Úloha tooverify hello naplánované synchronizace</span><span class="sxs-lookup"><span data-stu-id="f9a2d-119">tooverify hello scheduled synchronization task</span></span>
1. <span data-ttu-id="f9a2d-120">Přihlaste se toohello portálu Azure jako správce.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-120">Sign in toohello Azure portal as an admin.</span></span>
2. <span data-ttu-id="f9a2d-121">Na levé straně hello vyberte **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-121">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="f9a2d-122">Na hello **služby Active Directory** klikněte dvakrát hello adresář, který má hello uživatele můžete určit tooset nahoru.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-122">On hello **Active Directory** page, double-click hello directory that has hello users you want tooset up.</span></span>
4. <span data-ttu-id="f9a2d-123">Hello horní části stránky adresáře hello, vyberte **integrace adresáře**.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-123">At hello top of hello directory page, select **Directory Integration**.</span></span>
5. <span data-ttu-id="f9a2d-124">V části **integraci s místní službou active directory**, Poznámka hello čas poslední synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-124">Under **integration with local active directory**, note hello last sync time.</span></span>

<span data-ttu-id="f9a2d-125"><center>![Čas synchronizace adresáře](./media/active-directory-aadconnect-whats-next/verify.png)</center></span><span class="sxs-lookup"><span data-stu-id="f9a2d-125"><center>![Directory sync time](./media/active-directory-aadconnect-whats-next/verify.png)</center></span></span>

## <a name="start-a-scheduled-synchronization-task"></a><span data-ttu-id="f9a2d-126">Spustit úlohu plánované synchronizace</span><span class="sxs-lookup"><span data-stu-id="f9a2d-126">Start a scheduled synchronization task</span></span>
<span data-ttu-id="f9a2d-127">Pokud potřebujete toorun úloha synchronizace, musíte znovu spuštěním prostřednictvím Průvodce Azure AD Connect hello.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-127">If you need toorun a synchronization task, you can do this by running through hello Azure AD Connect wizard again.</span></span>  <span data-ttu-id="f9a2d-128">Je nutné tooprovide přihlašovací údaje Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-128">You need tooprovide your Azure AD credentials.</span></span>  <span data-ttu-id="f9a2d-129">V Průvodci hello vyberte hello **přizpůsobit možnosti synchronizace** úloh a klikněte na tlačítko **Další** toomove prostřednictvím Průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-129">In hello wizard, select hello **Customize synchronization options** task, and click **Next** toomove through hello wizard.</span></span> <span data-ttu-id="f9a2d-130">Na konci hello, ujistěte se, že hello **zahájit proces synchronizace hello ihned po dokončení počáteční konfigurace hello** políčka.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-130">At hello end, ensure that hello **Start hello synchronization process as soon as hello initial configuration completes** box is selected.</span></span>

<span data-ttu-id="f9a2d-131"><center>![Spustit synchronizaci.](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span><span class="sxs-lookup"><span data-stu-id="f9a2d-131"><center>![Start synchronization](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span></span>

<span data-ttu-id="f9a2d-132">Další informace o hello Azure AD Connect sync Scheduler najdete v tématu [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span><span class="sxs-lookup"><span data-stu-id="f9a2d-132">For more information on hello Azure AD Connect sync Scheduler, see [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span></span>

## <a name="additional-tasks-available-in-azure-ad-connect"></a><span data-ttu-id="f9a2d-133">Další úlohy, které jsou k dispozici ve službě Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="f9a2d-133">Additional tasks available in Azure AD Connect</span></span>
<span data-ttu-id="f9a2d-134">Po počáteční instalace systému služby Azure AD Connect můžete vždy spustit Průvodce hello znovu z zástupce hello Azure AD Connect úvodní stránky nebo plochy.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-134">After your initial installation of Azure AD Connect, you can always start hello wizard again from hello Azure AD Connect start page or desktop shortcut.</span></span>  <span data-ttu-id="f9a2d-135">Si všimnete, že znovu projít průvodce hello poskytuje některé nové možnosti ve formě hello další úkoly.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-135">You will notice that going through hello wizard again provides some new options in hello form of additional tasks.</span></span>  

<span data-ttu-id="f9a2d-136">Hello následující tabulka obsahuje souhrn tyto úlohy a stručný popis jednotlivých úloh.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-136">hello following table provides a summary of these tasks and a brief description of each task.</span></span>

![Seznam dalších úloh](./media/active-directory-aadconnect-whats-next/addtasks.png)

| <span data-ttu-id="f9a2d-138">Další úlohy</span><span class="sxs-lookup"><span data-stu-id="f9a2d-138">Additional task</span></span> | <span data-ttu-id="f9a2d-139">Popis</span><span class="sxs-lookup"><span data-stu-id="f9a2d-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f9a2d-140">**Zobrazení hello si vybrali tento scénář**</span><span class="sxs-lookup"><span data-stu-id="f9a2d-140">**View hello selected scenario**</span></span> |<span data-ttu-id="f9a2d-141">Zobrazte vaše aktuální řešení Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-141">View your current Azure AD Connect solution.</span></span>  <span data-ttu-id="f9a2d-142">To zahrnuje obecná nastavení, synchronizovat adresáře a nastavení synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-142">This includes general settings, synchronized directories, and sync settings.</span></span> |
| <span data-ttu-id="f9a2d-143">**Přizpůsobit možnosti synchronizace**</span><span class="sxs-lookup"><span data-stu-id="f9a2d-143">**Customize synchronization options**</span></span> |<span data-ttu-id="f9a2d-144">Změnit hello aktuální konfiguraci jako přidává další konfigurace toohello doménových struktur služby Active Directory nebo povolení možnosti synchronizace, jako je například uživatele, skupiny, zařízení nebo zpětný zápis hesla.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-144">Change hello current configuration like adding additional Active Directory forests toohello configuration, or enabling sync options such as user, group, device, or password write-back.</span></span> |
| <span data-ttu-id="f9a2d-145">**Zapnout pracovní režim**</span><span class="sxs-lookup"><span data-stu-id="f9a2d-145">**Enable Staging Mode**</span></span> |<span data-ttu-id="f9a2d-146">Fáze informace, které není synchronizován okamžitě a není exportovat tooAzure AD nebo místní služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-146">Stage information that is not immediately synchronized and is not exported tooAzure AD or on-premises Active Directory.</span></span>  <span data-ttu-id="f9a2d-147">Pomocí této funkce můžete zobrazit náhled hello synchronizace předtím, než k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="f9a2d-147">With this feature, you can preview hello synchronizations before they occur.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f9a2d-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f9a2d-148">Next steps</span></span>
<span data-ttu-id="f9a2d-149">Další informace o [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="f9a2d-149">Learn more about [integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
