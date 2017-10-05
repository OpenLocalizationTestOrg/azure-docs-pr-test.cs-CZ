---
title: "Synchronizace Azure AD Connect: technické koncepty | Microsoft Docs"
description: "Vysvětluje technické koncepty synchronizace Azure AD Connect."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 731cfeb3-beaf-4d02-aef4-b02a8f99fd11
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi;andkjell
ms.openlocfilehash: 6cf8debc6443bb60fc5f601ea4aa392eb2f13a8f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-technical-concepts"></a><span data-ttu-id="f7cab-103">Synchronizace služby Azure AD Connect: Technické koncepty</span><span class="sxs-lookup"><span data-stu-id="f7cab-103">Azure AD Connect sync: Technical Concepts</span></span>
<span data-ttu-id="f7cab-104">Tento článek je uveden seznam tématu [Principy architektura](active-directory-aadconnectsync-technical-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="f7cab-104">This article is a summary of the topic [Understanding architecture](active-directory-aadconnectsync-technical-concepts.md).</span></span>

<span data-ttu-id="f7cab-105">Synchronizace Azure AD Connect je založen na plnou metaadresáře synchronizace platformy.</span><span class="sxs-lookup"><span data-stu-id="f7cab-105">Azure AD Connect sync builds upon a solid metadirectory synchronization platform.</span></span>
<span data-ttu-id="f7cab-106">V následujících částech zavést koncepty metaadresáře synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f7cab-106">The following sections introduce the concepts for metadirectory synchronization.</span></span>
<span data-ttu-id="f7cab-107">Sestavování na serveru MIIS, ILM a FIM, synchronizaci služby Active Directory Azure poskytuje další platformy pro připojení ke zdrojům dat, synchronizaci dat mezi zdroje dat, jakož i zajišťování a rušení zajištění identity.</span><span class="sxs-lookup"><span data-stu-id="f7cab-107">Building upon MIIS, ILM, and FIM, the Azure Active Directory Sync Services provides the next platform for connecting to data sources, synchronizing data between data sources, as well as the provisioning and deprovisioning of identities.</span></span>

![Technické koncepce](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

<span data-ttu-id="f7cab-109">Následující části obsahují další podrobnosti o následující aspekty synchronizační služba FIM:</span><span class="sxs-lookup"><span data-stu-id="f7cab-109">The following sections provide more details about the following aspects of the FIM Synchronization Service:</span></span>

* <span data-ttu-id="f7cab-110">konektor</span><span class="sxs-lookup"><span data-stu-id="f7cab-110">Connector</span></span>
* <span data-ttu-id="f7cab-111">Toku atributů</span><span class="sxs-lookup"><span data-stu-id="f7cab-111">Attribute flow</span></span>
* <span data-ttu-id="f7cab-112">Prostoru konektoru</span><span class="sxs-lookup"><span data-stu-id="f7cab-112">Connector space</span></span>
* <span data-ttu-id="f7cab-113">Úložiště Metaverse</span><span class="sxs-lookup"><span data-stu-id="f7cab-113">Metaverse</span></span>
* <span data-ttu-id="f7cab-114">Zřizování</span><span class="sxs-lookup"><span data-stu-id="f7cab-114">Provisioning</span></span>

## <a name="connector"></a><span data-ttu-id="f7cab-115">konektor</span><span class="sxs-lookup"><span data-stu-id="f7cab-115">Connector</span></span>
<span data-ttu-id="f7cab-116">Moduly kódu, které se používají ke komunikaci s připojený adresář se nazývají konektory (dříve označované jako agenti pro správu (MAs)).</span><span class="sxs-lookup"><span data-stu-id="f7cab-116">The code modules that are used to communicate with a connected directory are called connectors (formerly known as  management agents (MAs)).</span></span>

<span data-ttu-id="f7cab-117">Byly nainstalovány na počítači se systémem synchronizace Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="f7cab-117">These are installed on the computer running Azure AD Connect sync.</span></span>
<span data-ttu-id="f7cab-118">Konektory umožňují bez agentů komunikaci pomocí protokolů vzdáleného systému namísto spoléhání na nasazení specializované agentů.</span><span class="sxs-lookup"><span data-stu-id="f7cab-118">The connectors provide the agentless ability to converse by using remote system protocols instead of relying on the deployment of specialized agents.</span></span> <span data-ttu-id="f7cab-119">To znamená ke snížení rizika a dobu nasazení, zejména v případě, že zabývající se kritické aplikace a systémy.</span><span class="sxs-lookup"><span data-stu-id="f7cab-119">This means decreased risk and deployment times, especially when dealing with critical applications and systems.</span></span>

<span data-ttu-id="f7cab-120">Na obrázku výše konektor je totožná s prostoru konektoru, ale zahrnuje veškerou komunikaci s externím systému.</span><span class="sxs-lookup"><span data-stu-id="f7cab-120">In the picture above, the connector is synonymous with the connector space but encompasses all communication with the external system.</span></span>

<span data-ttu-id="f7cab-121">Konektor je zodpovědná za všechny import a export funkcí do systému a uvolní vývojáři z museli pochopit, jak se připojit do každého systému nativně při přizpůsobení transformace dat pomocí deklarativní zřizování.</span><span class="sxs-lookup"><span data-stu-id="f7cab-121">The connector is responsible for all import and export functionality to the system and frees developers from needing to understand how to connect to each system natively when using declarative provisioning to customize data transformations.</span></span>

<span data-ttu-id="f7cab-122">Import a export pouze v případě naplánované, aby vám umožnil další izolaci od změny, ke kterým dochází v rámci systému, protože změny nejsou automaticky rozšířena do připojeného zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="f7cab-122">Imports and exports only occur when scheduled, allowing for further insulation from changes occurring within the system, since changes do not automatically propagate to the connected data source.</span></span> <span data-ttu-id="f7cab-123">Kromě toho mohou vývojáři vytvořit svoje vlastní konektory pro připojení k prakticky libovolný zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="f7cab-123">In addition, developers may also create their own connectors for connecting to virtually any data source.</span></span>

## <a name="attribute-flow"></a><span data-ttu-id="f7cab-124">Toku atributů</span><span class="sxs-lookup"><span data-stu-id="f7cab-124">Attribute flow</span></span>
<span data-ttu-id="f7cab-125">Úložiště metaverse je konsolidované zobrazení všechny připojené k identit od sousedního prostor konektoru.</span><span class="sxs-lookup"><span data-stu-id="f7cab-125">The metaverse is the consolidated view of all joined identities from neighboring connector spaces.</span></span> <span data-ttu-id="f7cab-126">Ve výše uvedeném obrázku je znázorněn toku atributů linkami se šipkami pro příchozí a odchozí tok.</span><span class="sxs-lookup"><span data-stu-id="f7cab-126">In the figure above, attribute flow is depicted by lines with arrowheads for both inbound and outbound flow.</span></span> <span data-ttu-id="f7cab-127">Tok atributů je proces kopírování nebo transformace dat v jednom systému a všech atributů toků (příchozí nebo odchozí).</span><span class="sxs-lookup"><span data-stu-id="f7cab-127">Attribute flow is the process of copying or transforming data from one system to another and all attribute flows (inbound or outbound).</span></span>

<span data-ttu-id="f7cab-128">Tok atributů probíhá mezi prostoru konektoru a úložiště metaverse obousměrně když jsou naplánované operace synchronizace (úplná nebo rozdílová).</span><span class="sxs-lookup"><span data-stu-id="f7cab-128">Attribute flow occurs between the connector space and the metaverse bi-directionally when synchronization (full or delta) operations are scheduled to run.</span></span>

<span data-ttu-id="f7cab-129">Tok atributů dochází pouze při spuštění těchto synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f7cab-129">Attribute flow only occurs when these synchronizations are run.</span></span> <span data-ttu-id="f7cab-130">Toky atributů jsou definovány v synchronizační pravidla.</span><span class="sxs-lookup"><span data-stu-id="f7cab-130">Attribute flows are defined in Synchronization Rules.</span></span> <span data-ttu-id="f7cab-131">To může být příchozí (ISR na obrázku výše) nebo odchozí (OSR na obrázku výše).</span><span class="sxs-lookup"><span data-stu-id="f7cab-131">These can be inbound (ISR in the picture above) or outbound (OSR in the picture above).</span></span>

## <a name="connected-system"></a><span data-ttu-id="f7cab-132">Připojený systém</span><span class="sxs-lookup"><span data-stu-id="f7cab-132">Connected system</span></span>
<span data-ttu-id="f7cab-133">Připojený systém (neboli připojený adresář) je odkazující na vzdáleném systému Azure AD Connect sync připojil k a čtení a zápis dat identity do a z.</span><span class="sxs-lookup"><span data-stu-id="f7cab-133">Connected system (aka connected directory) is referring to the remote system Azure AD Connect sync has connected to and reading and writing identity data to and from.</span></span>

## <a name="connector-space"></a><span data-ttu-id="f7cab-134">Prostoru konektoru</span><span class="sxs-lookup"><span data-stu-id="f7cab-134">Connector space</span></span>
<span data-ttu-id="f7cab-135">Každý připojeného zdroje dat je reprezentován jako filtrované podmnožina objektů a atributů v prostoru konektoru.</span><span class="sxs-lookup"><span data-stu-id="f7cab-135">Each connected data source is represented as a filtered subset of the objects and attributes in the connector space.</span></span>
<span data-ttu-id="f7cab-136">To umožňuje pracovat místně bez nutnosti kontaktování vzdáleného systému při synchronizaci objekty služby sync a omezuje interakci importuje a exportuje jenom.</span><span class="sxs-lookup"><span data-stu-id="f7cab-136">This allows the sync service to operate locally without the need to contact the remote system when synchronizing the objects and restricts interaction to imports and exports only.</span></span>

<span data-ttu-id="f7cab-137">Pokud zdroj dat a konektor schopnost poskytovat seznam sad změn (Rozdílový import), pak provozní efektivitu zvyšuje výrazně jako pouze změny od posledního cyklu dotazování se vyměňují.</span><span class="sxs-lookup"><span data-stu-id="f7cab-137">When the data source and the connector have the ability to provide a list of changes (a delta import), then the operational efficiency increases dramatically as only changes since the last polling cycle are exchanged.</span></span> <span data-ttu-id="f7cab-138">Prostoru konektoru insulates připojeného zdroje dat z šíření automaticky tím, že vyžaduje, aby plán konektor importuje a exportuje změny.</span><span class="sxs-lookup"><span data-stu-id="f7cab-138">The connector space insulates the connected data source from changes propagating automatically by requiring that the connector schedule imports and exports.</span></span> <span data-ttu-id="f7cab-139">Tato přidané pojištění uděluje jistotu při testování, zobrazení náhledu nebo potvrzení, že příští aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f7cab-139">This added insurance grants you peace of mind while testing, previewing, or confirming the next update.</span></span>

## <a name="metaverse"></a><span data-ttu-id="f7cab-140">Úložiště Metaverse</span><span class="sxs-lookup"><span data-stu-id="f7cab-140">Metaverse</span></span>
<span data-ttu-id="f7cab-141">Úložiště metaverse je konsolidované zobrazení všechny připojené k identit od sousedního prostor konektoru.</span><span class="sxs-lookup"><span data-stu-id="f7cab-141">The metaverse is the consolidated view of all joined identities from neighboring connector spaces.</span></span>

<span data-ttu-id="f7cab-142">Jako identity jsou spojeny dohromady a je mu přiřazená autority pro různé atributy prostřednictvím mapování toku importu, objektu centrální úložiště metaverse začne shromažďují informace z více systémů.</span><span class="sxs-lookup"><span data-stu-id="f7cab-142">As identities are linked together and authority is assigned for various attributes through import flow mappings, the central metaverse object begins to aggregate information from multiple systems.</span></span> <span data-ttu-id="f7cab-143">Mapování tohoto datového toku atributů objektu provádění informace, které odchozí systémy.</span><span class="sxs-lookup"><span data-stu-id="f7cab-143">From this object attribute flow, mappings carry information to outbound systems.</span></span>

<span data-ttu-id="f7cab-144">Objekty vytvářejí, když je autoritativní systému projekty do úložiště metaverse.</span><span class="sxs-lookup"><span data-stu-id="f7cab-144">Objects are created when an authoritative system projects them into the metaverse.</span></span> <span data-ttu-id="f7cab-145">Jakmile se odeberou všechna připojení, je odstraněn objekt úložiště metaverse.</span><span class="sxs-lookup"><span data-stu-id="f7cab-145">As soon as all connections are removed, the metaverse object is deleted.</span></span>

<span data-ttu-id="f7cab-146">Objekty v úložišti metaverse nelze upravovat přímo.</span><span class="sxs-lookup"><span data-stu-id="f7cab-146">Objects in the metaverse cannot be edited directly.</span></span> <span data-ttu-id="f7cab-147">Všechna data v objektu musí být podílí prostřednictvím toku atributů.</span><span class="sxs-lookup"><span data-stu-id="f7cab-147">All data in the object must be contributed through attribute flow.</span></span> <span data-ttu-id="f7cab-148">Úložiště metaverse udržuje trvalé konektory s každou prostoru konektoru.</span><span class="sxs-lookup"><span data-stu-id="f7cab-148">The metaverse maintains persistent connectors with each connector space.</span></span> <span data-ttu-id="f7cab-149">Tyto konektory nevyžadují přehodnocení pro každou synchronizaci spustit.</span><span class="sxs-lookup"><span data-stu-id="f7cab-149">These connectors do not require reevaluation for each synchronization run.</span></span> <span data-ttu-id="f7cab-150">To znamená, že synchronizace Azure AD Connect nemá najít odpovídající vzdálenému objektu pokaždé, když.</span><span class="sxs-lookup"><span data-stu-id="f7cab-150">This means that Azure AD Connect sync does not have to locate the matching remote object each time.</span></span> <span data-ttu-id="f7cab-151">Tím je zabráněno potřebu nákladná agentů, aby se zabránilo změny atributů, které obvykle bude zodpovídat za korelace objekty.</span><span class="sxs-lookup"><span data-stu-id="f7cab-151">This avoids the need for costly agents to prevent changes to attributes that would normally be responsible for correlating the objects.</span></span>

<span data-ttu-id="f7cab-152">Při zjišťování nových zdrojů dat, které mohou mít dříve existující objekty, které je třeba spravovat, synchronizace Azure AD Connect procesu označovaného jako pravidlo připojení používá k vyhodnocení potenciální kandidáty ke které má vytvořit odkaz.</span><span class="sxs-lookup"><span data-stu-id="f7cab-152">When discovering new data sources that may have preexisting objects that need to be managed, Azure AD Connect sync uses a process called a join rule to evaluate potential candidates with which to establish a link.</span></span>
<span data-ttu-id="f7cab-153">Po vytvoření odkazu výskytu toto testování a toku normální atributů může proběhnout mezi vzdálené připojeného zdroje dat a úložiště metaverse.</span><span class="sxs-lookup"><span data-stu-id="f7cab-153">Once the link is established, this evaluation does not reoccur and normal attribute flow can occur between the remote connected data source and the metaverse.</span></span>

## <a name="provisioning"></a><span data-ttu-id="f7cab-154">Zřizování</span><span class="sxs-lookup"><span data-stu-id="f7cab-154">Provisioning</span></span>
<span data-ttu-id="f7cab-155">Když autoritativní zdroj projekty nový objekt do úložiště metaverse nového objektu prostoru konektoru lze vytvořit v představující podřízené připojeného zdroje dat jiný konektor.</span><span class="sxs-lookup"><span data-stu-id="f7cab-155">When an authoritative source projects a new object into the metaverse a new connector space object can be created in another Connector representing a downstream connected data source.</span></span>

<span data-ttu-id="f7cab-156">To vytváří ze své podstaty odkaz a obousměrně pokračovat toku atributů.</span><span class="sxs-lookup"><span data-stu-id="f7cab-156">This inherently establishes a link, and attribute flow can proceed bi-directionally.</span></span>

<span data-ttu-id="f7cab-157">Vždy, když pravidlo zjistí, že je potřeba vytvořit nový objekt prostoru konektoru, nazývá se zřizování.</span><span class="sxs-lookup"><span data-stu-id="f7cab-157">Whenever a rule determines that a new connector space object needs to be created, it is called provisioning.</span></span> <span data-ttu-id="f7cab-158">Ale protože tato operace je provedeno pouze v rámci prostoru konektoru, je neprovádí do připojeného zdroje dat do provedení exportu.</span><span class="sxs-lookup"><span data-stu-id="f7cab-158">However, because this operation only takes place within the connector space, it does not carry over into the connected data source until an export is performed.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7cab-159">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f7cab-159">Additional Resources</span></span>
* [<span data-ttu-id="f7cab-160">Azure AD Connect Sync: Možnosti přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="f7cab-160">Azure AD Connect Sync: Customizing Synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="f7cab-161">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f7cab-161">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png