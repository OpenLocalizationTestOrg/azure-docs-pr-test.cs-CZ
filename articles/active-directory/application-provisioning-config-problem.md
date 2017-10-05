---
title: "Problém konfiguraci zřizování uživatelů k aplikaci Galerie Azure AD | Microsoft Docs"
description: "Postup řešení běžných problémů s potýkají při konfiguraci zřizování uživatelů na aplikaci již uveden v galerii aplikací Azure AD"
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
ms.openlocfilehash: 44e344095352f2bc6b27e389fc8be2cdf3e368d8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-user-provisioning-to-an-azure-ad-gallery-application"></a><span data-ttu-id="c9e7a-103">Problém konfiguraci zřizování uživatelů k aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9e7a-103">Problem configuring user provisioning to an Azure AD Gallery application</span></span>

<span data-ttu-id="c9e7a-104">Konfigurace [zřizování automatické uživatelů](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) pro aplikaci (Pokud je podporuje), vyžaduje, aby konkrétní pokyny platí Příprava aplikací pro automatické zřizování.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-104">Configuring [automatic user provisioning](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) for an app (where supported), requires that specific instructions be followed to prepare the application for automatic provisioning.</span></span> <span data-ttu-id="c9e7a-105">Potom můžete portál Azure ke konfiguraci zřizování služby synchronizace uživatelských účtů do aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-105">Then you can use the Azure portal to configure the provisioning service to synchronize user accounts to the application.</span></span>

<span data-ttu-id="c9e7a-106">Vždy byste měli začít hledání kurzu instalace specifické pro vytvoření zřizování pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-106">You should always start by finding the setup tutorial specific to setting up provisioning for your application.</span></span> <span data-ttu-id="c9e7a-107">Potom postupujte podle těchto kroků konfigurace aplikace a Azure AD k vytvoření zřizování připojení.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-107">Then follow those steps to configure both the app and Azure AD to create the provisioning connection.</span></span> <span data-ttu-id="c9e7a-108">Seznam kurzů aplikace najdete na [seznamu kurzy o tom, jak integrovat SaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span><span class="sxs-lookup"><span data-stu-id="c9e7a-108">A list of app tutorials can be found at [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).</span></span>

## <a name="how-to-see-if-provisioning-is-working"></a><span data-ttu-id="c9e7a-109">Jak zjistit, zda pracuje zřizování</span><span class="sxs-lookup"><span data-stu-id="c9e7a-109">How to see if provisioning is working</span></span> 

<span data-ttu-id="c9e7a-110">Po nakonfigurování služby lze většinu přehledy operaci služby rozlišovat ze dvou míst:</span><span class="sxs-lookup"><span data-stu-id="c9e7a-110">Once the service is configured, most insights into the operation of the service can be drawn from two places:</span></span>

-   <span data-ttu-id="c9e7a-111">**Protokoly auditu** – protokoly zřizování auditu záznam všech operací prováděných zřizování službou, včetně dotazování Azure AD pro přiřazené uživatele, kteří jsou v oboru pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-111">**Audit logs** – The provisioning audit logs record all the operations performed by the provisioning service, including querying Azure AD for assigned users that are in scope for provisioning.</span></span> <span data-ttu-id="c9e7a-112">Dotaz na cílové aplikace existenci uživatelům porovnávání uživatelských objektů mezi systémem.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-112">Query the target app for the existence of those users, comparing the user objects between the system.</span></span> <span data-ttu-id="c9e7a-113">Potom přidání, aktualizace nebo zakázat účet uživatele v cílovém systému podle porovnání.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-113">Then add, update, or disable the user account in the target system based on the comparison.</span></span> <span data-ttu-id="c9e7a-114">Zřizování protokolů auditu na portálu Azure v přístupné **Azure Active Directory &gt; podnikové aplikace &gt; \[název aplikace\] &gt; protokolech auditování** kartě.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-114">The provisioning audit logs can be accessed in the Azure portal, in the **Azure Active Directory &gt; Enterprise Apps &gt; \[Application Name\] &gt; Audit Logs** tab.</span></span> <span data-ttu-id="c9e7a-115">Filtrovat protokoly **zřizování účtu** kategorii zobrazíte jen zřizování události pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-115">Filter the logs on the **Account Provisioning** category to only see the provisioning events for that app.</span></span>

-   <span data-ttu-id="c9e7a-116">**Stav – zřízení** souhrn poslední zřizování spusťte pro danou aplikaci si můžete prohlédnout ve **Azure Active Directory &gt; podnikové aplikace &gt; \[název aplikace\] &gt; Zřizování** části, v dolní části obrazovky v části Nastavení služby.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-116">**Provisioning status –** A summary of the last provisioning run for a given app can be seen in the **Azure Active Directory &gt; Enterprise Apps &gt; \[Application Name\] &gt;Provisioning** section, at the bottom of the screen under the service settings.</span></span> <span data-ttu-id="c9e7a-117">Tento oddíl shrnuje, kolik uživatelů (nebo skupin) jsou nyní synchronizovány mezi těmito dvěma systémy, a pokud nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-117">This section summarizes how many users (and/or groups) are currently being synchronized between the two systems, and if there are any errors.</span></span> <span data-ttu-id="c9e7a-118">Podrobnosti o chybě se v protokolech auditu.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-118">Error details be in the audit logs.</span></span> <span data-ttu-id="c9e7a-119">Všimněte si, že stav zřizování nesmí být naplněny až do dokončení jeden úplné počáteční synchronizaci mezi službou Azure AD a aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-119">Note that the provisioning status not be populated until one full initial synchronization has been completed between Azure AD and the app.</span></span>

## <a name="general-problem-areas-with-provisioning-to-consider"></a><span data-ttu-id="c9e7a-120">Obecné problémových oblastí se zřizováním vzít v úvahu</span><span class="sxs-lookup"><span data-stu-id="c9e7a-120">General problem areas with provisioning to consider</span></span>

<span data-ttu-id="c9e7a-121">Níže je seznam obecné problémových oblastí, které můžete rozbalit Pokud máte představu o kde začít.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-121">Below is a list of the general problem areas that you can drill into if you have an idea of where to start.</span></span>

* [<span data-ttu-id="c9e7a-122">Zřizování služby nezobrazí spuštění</span><span class="sxs-lookup"><span data-stu-id="c9e7a-122">Provisioning service does not appear to start</span></span>](#provisioning-service-does-not-appear-to-start)
* [<span data-ttu-id="c9e7a-123">Nelze uložit konfiguraci z důvodu přihlašovací údaje aplikace nepracuje</span><span class="sxs-lookup"><span data-stu-id="c9e7a-123">Can’t save configuration due to app credentials not working</span></span>](#can’t-save-configuration-due-to-app-credentials-not-working)
* [<span data-ttu-id="c9e7a-124">Protokoly auditu, že uživatelé jsou "přeskočen" a není zajišťováno, i když jsou přiřazeny</span><span class="sxs-lookup"><span data-stu-id="c9e7a-124">Audit logs say users are “skipped” and not provisioned, even though they are assigned</span></span>](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a><span data-ttu-id="c9e7a-125">Zřizování služby nezobrazí spuštění</span><span class="sxs-lookup"><span data-stu-id="c9e7a-125">Provisioning service does not appear to start</span></span>

<span data-ttu-id="c9e7a-126">Pokud nastavíte **Stav zřizování** být **na** v **Azure Active Directory &gt; podnikové aplikace &gt; \[název aplikace\] &gt;zřizování** části portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-126">If you set the **Provisioning Status** to be **On** in the **Azure Active Directory &gt; Enterprise Apps &gt; \[Application Name\] &gt;Provisioning** section of the Azure portal.</span></span> <span data-ttu-id="c9e7a-127">Žádné další podrobnosti o stavu se ale zobrazují na této stránce po následné znovu načte.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-127">However no other status details are shown on that page after subsequent reloads.</span></span> <span data-ttu-id="c9e7a-128">Je pravděpodobné, že služba běží, avšak nedokončil ještě počáteční synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-128">It is likely that the service is running but has not completed an initial synchronization yet.</span></span> <span data-ttu-id="c9e7a-129">Zkontrolujte **protokoly auditu** popsáno výše, a určit, jakým operacím služby provádí, a pokud nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-129">Check the **Audit logs** described above to determine what operations the service is performing, and if there are any errors.</span></span>

>[!NOTE]
><span data-ttu-id="c9e7a-130">Počáteční synchronizace může trvat od 20 minut několik hodin v závislosti na velikosti adresáře služby Azure AD a počet uživatelů v oboru pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-130">An initial sync can take anywhere from 20 minutes to several hours, depending on the size of the Azure AD directory and the number of users in scope for provisioning.</span></span> <span data-ttu-id="c9e7a-131">Následné synchronizace po počáteční synchronizace být rychlejší jako službu zřizování ukládá vodoznaky, které představují stav obou systémů po počáteční synchronizaci, zvýšení výkonu následné synchronizace.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-131">Subsequent syncs after the initial sync be faster, as the provisioning service stores watermarks that represent the state of both systems after the initial sync, improving performance of subsequent syncs.</span></span>
>
>

## <a name="cant-save-configuration-due-to-app-credentials-not-working"></a><span data-ttu-id="c9e7a-132">Nelze uložit konfiguraci z důvodu přihlašovací údaje aplikace nepracuje</span><span class="sxs-lookup"><span data-stu-id="c9e7a-132">Can’t save configuration due to app credentials not working</span></span>

<span data-ttu-id="c9e7a-133">Aby zřizování pracovat Azure AD vyžaduje platné přihlašovací údaje, které umožňují připojení k rozhraní API poskytované aplikaci pro správu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-133">In order for provisioning to work, Azure AD requires valid credentials that allow it to connect to a user management API provided by that app.</span></span> <span data-ttu-id="c9e7a-134">Pokud tyto přihlašovací údaje nefungují, nebo neznáte wat je, přečtěte si kurz pro nastavení této aplikace a popsané.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-134">If these credentials do not work, or you don’t know wat they are, review the tutorial for setting up this app, described previously.</span></span>

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a><span data-ttu-id="c9e7a-135">Protokoly auditu, že uživatelé jsou přeskočeny a není zajišťováno i když jsou přiřazeny</span><span class="sxs-lookup"><span data-stu-id="c9e7a-135">Audit logs say users are skipped and not provisioned even though they are assigned</span></span>

<span data-ttu-id="c9e7a-136">Když uživatel se zobrazí jako "přeskočen" v protokolech auditu, je velmi důležité Číst rozšířené podrobnosti v protokolu zprávy a určete důvod.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-136">When a user shows up as “skipped” in the audit logs, it is very important to read the extended details in the log message to determine the reason.</span></span> <span data-ttu-id="c9e7a-137">Níže jsou uvedeny běžné příčiny a řešení:</span><span class="sxs-lookup"><span data-stu-id="c9e7a-137">Below are common reasons and resolutions:</span></span>

-   <span data-ttu-id="c9e7a-138">**Byl nakonfigurován oboru filtru** **, je filtrování uživatele podle hodnota atributu**.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-138">**A scoping filter has been configured** **that is filtering the user out based on an attribute value**.</span></span> <span data-ttu-id="c9e7a-139">Další informace o filtry oborů najdete v tématu <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-139">For more information on scoping filters, see <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.</span></span>

-   <span data-ttu-id="c9e7a-140">**Uživatel je "není oprávněn efektivně".**</span><span class="sxs-lookup"><span data-stu-id="c9e7a-140">**The user is “not effectively entitled”.**</span></span> <span data-ttu-id="c9e7a-141">Pokud se zobrazí tato konkrétní chybová zpráva, je to, protože došlo k potížím s uživatelský záznam přiřazení uložené ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-141">If you see this specific error message, it is because there is a problem with the user assignment record stored in Azure AD.</span></span> <span data-ttu-id="c9e7a-142">Opravte tento problém, zrušte přiřazení uživatele (nebo skupiny) z aplikace a znovu přiřadit.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-142">To fix this issue, un-assign the user (or group) from the app, and re-assign it again.</span></span> <span data-ttu-id="c9e7a-143">Další informace o přiřazení najdete v tématu <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-143">For more information on assignment, see <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.</span></span>

-   <span data-ttu-id="c9e7a-144">**Požadovaný atribut nebyl nalezen nebo není vyplněná pro uživatele.**</span><span class="sxs-lookup"><span data-stu-id="c9e7a-144">**A required attribute is missing or not populated for a user.**</span></span> <span data-ttu-id="c9e7a-145">Důležité vzít v úvahu při nastavování zřizování být zkontrolujte a nakonfigurujte mapování atributů a pracovních postupů, které definují, které uživatele (nebo skupiny) vlastnosti toku z Azure AD k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-145">An important thing to consider when setting up provisioning be to review and configure the attribute mappings and workflows that define which user (or group) properties flow from Azure AD to the application.</span></span> <span data-ttu-id="c9e7a-146">To zahrnuje nastavení "odpovídající vlastnost", které použít k jednoznačné identifikaci a odpovídají uživatele nebo skupiny mezi těmito dvěma systémy.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-146">This includes setting the “matching property” that be used to uniquely identify and match users/groups between the two systems.</span></span> <span data-ttu-id="c9e7a-147">Další informace o tomto procesu důležité najdete v tématu <https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings>.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-147">For more information on this important process, see <https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings>.</span></span>

   * <span data-ttu-id="c9e7a-148">**Mapování pro skupiny atributů:** zřizování název skupiny a údaje skupiny, kromě členy, pokud pro některé aplikace podporován.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-148">**Attribute mappings for groups:** Provisioning of the group name and group details, in addition to the members, if supported for some applications.</span></span> <span data-ttu-id="c9e7a-149">Můžete povolit nebo zakázat tuto funkci povolením nebo zakázáním **mapování** pro objekty skupiny ukazuje **zřizování** kartě.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-149">You can enable or disable this functionality by enabling or disabling the **Mapping** for group objects shown in the **Provisioning** tab.</span></span> <span data-ttu-id="c9e7a-150">Pokud je povoleno zřizování skupiny, nezapomeňte si projít mapování atributů k zajištění, že na odpovídající pole je používána pro "Odpovídající ID".</span><span class="sxs-lookup"><span data-stu-id="c9e7a-150">If provisioning groups is enabled, be sure to review the attribute mappings to ensure an appropriate field is being used for the “matching ID”.</span></span> <span data-ttu-id="c9e7a-151">Může to být alias zobrazovaný název nebo e-mailu), protože skupiny a její členy nelze zřídit Pokud odpovídající vlastnost je prázdná nebo není vyplněná skupiny ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9e7a-151">This can be the display name or email alias), as the group and its members not be provisioned if the matching property is empty or not populated for a group in Azure AD.</span></span>

#<a name="next-steps"></a><span data-ttu-id="c9e7a-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9e7a-152">Next steps</span></span>
[<span data-ttu-id="c9e7a-153">Automatizovat uživatele zajišťování a rušení zajištění pro aplikace SaaS ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c9e7a-153">Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory</span></span>](active-directory-saas-app-provisioning.md)