---
title: "Synchronizace Azure AD Connect: provozní úlohy a důležité informace | Microsoft Docs"
description: "Toto téma popisuje provozní úlohy pro synchronizaci Azure AD Connect a jak tooprepare pro provoz této součásti."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e6b21262e0924785808888d66f08a04a56581edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a><span data-ttu-id="da2bf-103">Synchronizace Azure AD Connect: provozní úlohy a zvážení</span><span class="sxs-lookup"><span data-stu-id="da2bf-103">Azure AD Connect sync: Operational tasks and consideration</span></span>
<span data-ttu-id="da2bf-104">Hello cílem tohoto tématu je toodescribe provozní úlohy pro synchronizaci Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="da2bf-104">hello objective of this topic is toodescribe operational tasks for Azure AD Connect sync.</span></span>

## <a name="staging-mode"></a><span data-ttu-id="da2bf-105">Pracovní režim</span><span class="sxs-lookup"><span data-stu-id="da2bf-105">Staging mode</span></span>
<span data-ttu-id="da2bf-106">Pracovní režim lze použít pro několik scénářů, včetně:</span><span class="sxs-lookup"><span data-stu-id="da2bf-106">Staging mode can be used for several scenarios, including:</span></span>

* <span data-ttu-id="da2bf-107">Vysoká dostupnost.</span><span class="sxs-lookup"><span data-stu-id="da2bf-107">High availability.</span></span>
* <span data-ttu-id="da2bf-108">Testování a nasazení nové změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="da2bf-108">Test and deploy new configuration changes.</span></span>
* <span data-ttu-id="da2bf-109">Zavedení nového serveru a vyřadit z provozu původní hello.</span><span class="sxs-lookup"><span data-stu-id="da2bf-109">Introduce a new server and decommission hello old.</span></span>

<span data-ttu-id="da2bf-110">Se serverem v pracovním režimu můžete provádět změny toohello konfigurace a preview hello změny před prováděním active hello server.</span><span class="sxs-lookup"><span data-stu-id="da2bf-110">With a server in staging mode, you can make changes toohello configuration and preview hello changes before you make hello server active.</span></span> <span data-ttu-id="da2bf-111">Můžete taky toorun úplný import a úplnou synchronizaci tooverify všechny změny se očekává, než provedete tyto změny do vašeho provozního prostředí.</span><span class="sxs-lookup"><span data-stu-id="da2bf-111">It also allows you toorun full import and full synchronization tooverify that all changes are expected before you make these changes into your production environment.</span></span>

<span data-ttu-id="da2bf-112">Během instalace, můžete vybrat toobe server hello v **pracovním režimu**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-112">During installation, you can select hello server toobe in **staging mode**.</span></span> <span data-ttu-id="da2bf-113">Tato akce aktivuje hello serveru pro import a synchronizaci, ale nespustí žádné export.</span><span class="sxs-lookup"><span data-stu-id="da2bf-113">This action makes hello server active for import and synchronization, but it does not run any exports.</span></span> <span data-ttu-id="da2bf-114">Server v pracovní režimu není spuštěna synchronizace hesel nebo zpětný zápis hesla, i v případě, že jste vybrali tyto funkce během instalace.</span><span class="sxs-lookup"><span data-stu-id="da2bf-114">A server in staging mode is not running password sync or password writeback, even if you selected these features during installation.</span></span> <span data-ttu-id="da2bf-115">Pokud zakážete pracovní režim, hello server spustí export, umožňuje synchronizaci hesel a umožňuje zpětný zápis hesla.</span><span class="sxs-lookup"><span data-stu-id="da2bf-115">When you disable staging mode, hello server starts exporting, enables password sync, and enables password writeback.</span></span>

<span data-ttu-id="da2bf-116">Můžete vynutit exportu pomocí Správce služby synchronizace hello.</span><span class="sxs-lookup"><span data-stu-id="da2bf-116">You can still force an export by using hello synchronization service manager.</span></span>

<span data-ttu-id="da2bf-117">Server v pracovní režimu pokračuje tooreceive změny ze služby Active Directory a Azure AD.</span><span class="sxs-lookup"><span data-stu-id="da2bf-117">A server in staging mode continues tooreceive changes from Active Directory and Azure AD.</span></span> <span data-ttu-id="da2bf-118">Má vždy kopii hello poslední změny a může trvat velmi rychlé přes hello odpovědnosti jiného serveru.</span><span class="sxs-lookup"><span data-stu-id="da2bf-118">It always has a copy of hello latest changes and can very fast take over hello responsibilities of another server.</span></span> <span data-ttu-id="da2bf-119">Pokud provedete konfiguraci změny tooyour primární server, je vaše odpovědnosti toomake hello stejný server toohello změny v pracovním režimu.</span><span class="sxs-lookup"><span data-stu-id="da2bf-119">If you make configuration changes tooyour primary server, it is your responsibility toomake hello same changes toohello server in staging mode.</span></span>

<span data-ttu-id="da2bf-120">Pro těch, které můžete s znalostmi starší technologie synchronizace se liší hello pracovním režimu, protože hello server má svou vlastní databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="da2bf-120">For those of you with knowledge of older sync technologies, hello staging mode is different since hello server has its own SQL database.</span></span> <span data-ttu-id="da2bf-121">Tato architektura umožňuje hello pracovní režim serveru toobe umístěný v jiném datovém centru.</span><span class="sxs-lookup"><span data-stu-id="da2bf-121">This architecture allows hello staging mode server toobe located in a different datacenter.</span></span>

### <a name="verify-hello-configuration-of-a-server"></a><span data-ttu-id="da2bf-122">Ověření konfigurace hello serveru</span><span class="sxs-lookup"><span data-stu-id="da2bf-122">Verify hello configuration of a server</span></span>
<span data-ttu-id="da2bf-123">tooapply tuto metodu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="da2bf-123">tooapply this method, follow these steps:</span></span>

1. [<span data-ttu-id="da2bf-124">Příprava</span><span class="sxs-lookup"><span data-stu-id="da2bf-124">Prepare</span></span>](#prepare)
2. [<span data-ttu-id="da2bf-125">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="da2bf-125">Configuration</span></span>](#configuration)
3. [<span data-ttu-id="da2bf-126">Import a synchronizaci</span><span class="sxs-lookup"><span data-stu-id="da2bf-126">Import and Synchronize</span></span>](#import-and-synchronize)
4. [<span data-ttu-id="da2bf-127">Ověření</span><span class="sxs-lookup"><span data-stu-id="da2bf-127">Verify</span></span>](#verify)
5. [<span data-ttu-id="da2bf-128">Přepínač aktivní server</span><span class="sxs-lookup"><span data-stu-id="da2bf-128">Switch active server</span></span>](#switch-active-server)

#### <a name="prepare"></a><span data-ttu-id="da2bf-129">Příprava</span><span class="sxs-lookup"><span data-stu-id="da2bf-129">Prepare</span></span>
1. <span data-ttu-id="da2bf-130">Nainstalujte Azure AD Connect, vyberte **pracovním režimu**a zrušte výběr **spustit synchronizaci** na poslední stránce Průvodce instalací hello hello.</span><span class="sxs-lookup"><span data-stu-id="da2bf-130">Install Azure AD Connect, select **staging mode**, and unselect **start synchronization** on hello last page in hello installation wizard.</span></span> <span data-ttu-id="da2bf-131">Tento režim vám umožní toorun hello synchronizační modul ručně.</span><span class="sxs-lookup"><span data-stu-id="da2bf-131">This mode allows you toorun hello sync engine manually.</span></span>
   <span data-ttu-id="da2bf-132">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)</span><span class="sxs-lookup"><span data-stu-id="da2bf-132">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)</span></span>
2. <span data-ttu-id="da2bf-133">Vypnout nebo přihlášení a ze hello spusťte nabídky vyberte možnost přihlášení **synchronizační služba**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-133">Sign off/sign in and from hello start menu select **Synchronization Service**.</span></span>

#### <a name="configuration"></a><span data-ttu-id="da2bf-134">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="da2bf-134">Configuration</span></span>
<span data-ttu-id="da2bf-135">Pokud primární server toohello vlastní změny a chcete toocompare hello konfigurace provedli s hello pracovní server, použijte [nástroje Azure AD Connect konfigurace dokumentace](https://github.com/Microsoft/AADConnectConfigDocumenter).</span><span class="sxs-lookup"><span data-stu-id="da2bf-135">If you have made custom changes toohello primary server and want toocompare hello configuration with hello staging server, then use [Azure AD Connect configuration documenter](https://github.com/Microsoft/AADConnectConfigDocumenter).</span></span>

#### <a name="import-and-synchronize"></a><span data-ttu-id="da2bf-136">Import a synchronizaci</span><span class="sxs-lookup"><span data-stu-id="da2bf-136">Import and Synchronize</span></span>
1. <span data-ttu-id="da2bf-137">Vyberte **konektory**, a vyberte hello první konektor s typem hello **Active Directory Domain Services**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-137">Select **Connectors**, and select hello first Connector with hello type **Active Directory Domain Services**.</span></span> <span data-ttu-id="da2bf-138">Klikněte na tlačítko **spustit**, vyberte **úplný import**, a **OK**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-138">Click **Run**, select **Full import**, and **OK**.</span></span> <span data-ttu-id="da2bf-139">Proveďte tyto kroky pro všechny konektory tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="da2bf-139">Do these steps for all Connectors of this type.</span></span>
2. <span data-ttu-id="da2bf-140">Vyberte hello konektor s typem **Azure Active Directory (Microsoft)**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-140">Select hello Connector with type **Azure Active Directory (Microsoft)**.</span></span> <span data-ttu-id="da2bf-141">Klikněte na tlačítko **spustit**, vyberte **úplný import**, a **OK**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-141">Click **Run**, select **Full import**, and **OK**.</span></span>
3. <span data-ttu-id="da2bf-142">Ujistěte se, že je stále vybraná karta hello konektory.</span><span class="sxs-lookup"><span data-stu-id="da2bf-142">Make sure hello tab Connectors is still selected.</span></span> <span data-ttu-id="da2bf-143">Pro každý konektor s typem **Active Directory Domain Services**, klikněte na tlačítko **spustit**, vyberte **rozdílová synchronizace**, a **OK**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-143">For each Connector with type **Active Directory Domain Services**, click **Run**, select **Delta Synchronization**, and **OK**.</span></span>
4. <span data-ttu-id="da2bf-144">Vyberte hello konektor s typem **Azure Active Directory (Microsoft)**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-144">Select hello Connector with type **Azure Active Directory (Microsoft)**.</span></span> <span data-ttu-id="da2bf-145">Klikněte na tlačítko **spustit**, vyberte **rozdílová synchronizace**, a **OK**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-145">Click **Run**, select **Delta Synchronization**, and **OK**.</span></span>

<span data-ttu-id="da2bf-146">Máte nyní dvoufázové instalace export změní tooAzure AD a místní AD (Pokud používáte hybridní nasazení systému Exchange).</span><span class="sxs-lookup"><span data-stu-id="da2bf-146">You have now staged export changes tooAzure AD and on-premises AD (if you are using Exchange hybrid deployment).</span></span> <span data-ttu-id="da2bf-147">Další kroky Hello povolit tooinspect novinky o toochange před zahájením ve skutečnosti hello export toohello adresáře.</span><span class="sxs-lookup"><span data-stu-id="da2bf-147">hello next steps allow you tooinspect what is about toochange before you actually start hello export toohello directories.</span></span>

#### <a name="verify"></a><span data-ttu-id="da2bf-148">Ověřit</span><span class="sxs-lookup"><span data-stu-id="da2bf-148">Verify</span></span>
1. <span data-ttu-id="da2bf-149">Spusťte příkazový řádek a přejděte příliš`%ProgramFiles%\Microsoft Azure AD Sync\bin`</span><span class="sxs-lookup"><span data-stu-id="da2bf-149">Start a cmd prompt and go too`%ProgramFiles%\Microsoft Azure AD Sync\bin`</span></span>
2. <span data-ttu-id="da2bf-150">Spustit: `csexport "Name of Connector" %temp%\export.xml /f:x` hello název hello konektoru najdete v synchronizační služba.</span><span class="sxs-lookup"><span data-stu-id="da2bf-150">Run: `csexport "Name of Connector" %temp%\export.xml /f:x` hello name of hello Connector can be found in Synchronization Service.</span></span> <span data-ttu-id="da2bf-151">Obsahuje název podobné too"contoso.com – AAD" pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="da2bf-151">It has a name similar too"contoso.com – AAD" for Azure AD.</span></span>
3. <span data-ttu-id="da2bf-152">Zkopírujte skript prostředí PowerShell hello z oddílu hello [CSAnalyzer](#appendix-csanalyzer) tooa soubor s názvem `csanalyzer.ps1`.</span><span class="sxs-lookup"><span data-stu-id="da2bf-152">Copy hello PowerShell script from hello section [CSAnalyzer](#appendix-csanalyzer) tooa file named `csanalyzer.ps1`.</span></span>
4. <span data-ttu-id="da2bf-153">Otevřete okno prostředí PowerShell a procházet toohello složku, kde můžete vytvořit skript prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="da2bf-153">Open a PowerShell window and browse toohello folder where you created hello PowerShell script.</span></span>
5. <span data-ttu-id="da2bf-154">Spustit: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.</span><span class="sxs-lookup"><span data-stu-id="da2bf-154">Run: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.</span></span>
6. <span data-ttu-id="da2bf-155">Nyní máte soubor s názvem **processedusers1.csv** , může být prověřen v aplikaci Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="da2bf-155">You now have a file named **processedusers1.csv** that can be examined in Microsoft Excel.</span></span> <span data-ttu-id="da2bf-156">Všechny změny dvoufázové instalace toobe exportovat tooAzure AD se nacházejí v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="da2bf-156">All changes staged toobe exported tooAzure AD are found in this file.</span></span>
7. <span data-ttu-id="da2bf-157">Proveďte potřebné změny toohello data nebo konfigurace a spusťte tyto kroky opakujte (Import a synchronizaci a ověřte, zda) až hello změny, které jsou o toobe exportovat se očekává.</span><span class="sxs-lookup"><span data-stu-id="da2bf-157">Make necessary changes toohello data or configuration and run these steps again (Import and Synchronize and Verify) until hello changes that are about toobe exported are expected.</span></span>

#### <a name="switch-active-server"></a><span data-ttu-id="da2bf-158">Přepínač aktivní server</span><span class="sxs-lookup"><span data-stu-id="da2bf-158">Switch active server</span></span>
1. <span data-ttu-id="da2bf-159">Na aktuálně aktivní server hello vypnout server hello (DirSync nebo FIM nebo Azure AD Sync), takže ji není export tooAzure AD nebo ho nastavit v pracovním režimu (Azure AD Connect).</span><span class="sxs-lookup"><span data-stu-id="da2bf-159">On hello currently active server, either turn off hello server (DirSync/FIM/Azure AD Sync) so it is not exporting tooAzure AD or set it in staging mode (Azure AD Connect).</span></span>
2. <span data-ttu-id="da2bf-160">Spustit Průvodce instalací hello na serveru hello v **pracovním režimu** a zakázat **pracovním režimu**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-160">Run hello installation wizard on hello server in **staging mode** and disable **staging mode**.</span></span>
   <span data-ttu-id="da2bf-161">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)</span><span class="sxs-lookup"><span data-stu-id="da2bf-161">![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)</span></span>

## <a name="disaster-recovery"></a><span data-ttu-id="da2bf-162">Zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="da2bf-162">Disaster recovery</span></span>
<span data-ttu-id="da2bf-163">V případě, že dojde k havárii, kde ztratíte hello synchronizační server, je součástí návrhu implementace hello tooplan pro jaké toodo.</span><span class="sxs-lookup"><span data-stu-id="da2bf-163">Part of hello implementation design is tooplan for what toodo in case there is a disaster where you lose hello sync server.</span></span> <span data-ttu-id="da2bf-164">Existují různé modely toouse a které jeden toouse závisí na několika faktorech včetně:</span><span class="sxs-lookup"><span data-stu-id="da2bf-164">There are different models toouse and which one toouse depends on several factors including:</span></span>

* <span data-ttu-id="da2bf-165">Co je tolerance, nebude možné zkontrolujte změny tooobjects ve službě Azure AD během hello výpadku?</span><span class="sxs-lookup"><span data-stu-id="da2bf-165">What is your tolerance for not being able make changes tooobjects in Azure AD during hello downtime?</span></span>
* <span data-ttu-id="da2bf-166">Pokud používáte synchronizace hesel, hello uživatelé přijmout, ke kterým mají toouse hello staré heslo ve službě Azure AD v případě, že se změní ho místně?</span><span class="sxs-lookup"><span data-stu-id="da2bf-166">If you use password synchronization, do hello users accept that they have toouse hello old password in Azure AD in case they change it on-premises?</span></span>
* <span data-ttu-id="da2bf-167">Máte závislost na v reálném čase operace, jako je zpětný zápis hesla?</span><span class="sxs-lookup"><span data-stu-id="da2bf-167">Do you have a dependency on real-time operations, such as password writeback?</span></span>

<span data-ttu-id="da2bf-168">V závislosti na hello odpovědi toothese otázky a zásad vaší organizace může být implementováno jednu z následujících strategií hello:</span><span class="sxs-lookup"><span data-stu-id="da2bf-168">Depending on hello answers toothese questions and your organization’s policy, one of hello following strategies can be implemented:</span></span>

* <span data-ttu-id="da2bf-169">Znovu sestavte v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="da2bf-169">Rebuild when needed.</span></span>
* <span data-ttu-id="da2bf-170">K výměně za chodu pohotovostní server, označuje jako **pracovním režimu**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-170">Have a spare standby server, known as **staging mode**.</span></span>
* <span data-ttu-id="da2bf-171">Virtuální počítače použijte.</span><span class="sxs-lookup"><span data-stu-id="da2bf-171">Use virtual machines.</span></span>

<span data-ttu-id="da2bf-172">Pokud nepoužijete hello integrovanou databází SQL Express, tak také byste měli revidovat hello [vysoké dostupnosti SQL](#sql-high-availability) části.</span><span class="sxs-lookup"><span data-stu-id="da2bf-172">If you do not use hello built-in SQL Express database, then you should also review hello [SQL High Availability](#sql-high-availability) section.</span></span>

### <a name="rebuild-when-needed"></a><span data-ttu-id="da2bf-173">Opětovné sestavení v případě potřeby</span><span class="sxs-lookup"><span data-stu-id="da2bf-173">Rebuild when needed</span></span>
<span data-ttu-id="da2bf-174">Vhodným strategie je tooplan pro opětovném sestavení serveru v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="da2bf-174">A viable strategy is tooplan for a server rebuild when needed.</span></span> <span data-ttu-id="da2bf-175">Obvykle instalaci hello synchronizační modul a hello počáteční import a synchronizace může být dokončeny v rámci několik hodin.</span><span class="sxs-lookup"><span data-stu-id="da2bf-175">Usually, installing hello sync engine and do hello initial import and sync can be completed within a few hours.</span></span> <span data-ttu-id="da2bf-176">Pokud není k dispozici náhradní server, je možné tootemporarily použití synchronizační modul domény Řadič toohost hello.</span><span class="sxs-lookup"><span data-stu-id="da2bf-176">If there isn’t a spare server available, it is possible tootemporarily use a domain controller toohost hello sync engine.</span></span>

<span data-ttu-id="da2bf-177">Hello synchronizační modul server neukládá jakýkoli stav o objektech hello, takže hello databáze může být znovu sestavit z hello dat ve službě Active Directory a Azure AD.</span><span class="sxs-lookup"><span data-stu-id="da2bf-177">hello sync engine server does not store any state about hello objects so hello database can be rebuilt from hello data in Active Directory and Azure AD.</span></span> <span data-ttu-id="da2bf-178">Hello **sourceAnchor** atribut je použité toojoin hello objekty z místního a hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="da2bf-178">hello **sourceAnchor** attribute is used toojoin hello objects from on-premises and hello cloud.</span></span> <span data-ttu-id="da2bf-179">Pokud znovu vytvoříte místní server hello s existující objekty a hello cloudu a potom hello synchronizaci modul odpovídá tyto objekty společně na přeinstalování znova.</span><span class="sxs-lookup"><span data-stu-id="da2bf-179">If you rebuild hello server with existing objects on-premises and hello cloud, then hello sync engine matches those objects together again on reinstallation.</span></span> <span data-ttu-id="da2bf-180">Hello potřebujete toodocument a uložte si hello změny konfigurace provedené toohello serveru, například pravidla filtrování a synchronizace.</span><span class="sxs-lookup"><span data-stu-id="da2bf-180">hello things you need toodocument and save are hello configuration changes made toohello server, such as filtering and synchronization rules.</span></span> <span data-ttu-id="da2bf-181">Před zahájením synchronizace ho znovu použít tyto vlastní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="da2bf-181">These custom configurations must be reapplied before you start synchronizing.</span></span>

### <a name="have-a-spare-standby-server---staging-mode"></a><span data-ttu-id="da2bf-182">K výměně za chodu pohotovostní server - pracovní režim</span><span class="sxs-lookup"><span data-stu-id="da2bf-182">Have a spare standby server - staging mode</span></span>
<span data-ttu-id="da2bf-183">Pokud máte složitější prostředí, pak má jeden nebo více pohotovostní serverů doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="da2bf-183">If you have a more complex environment, then having one or more standby servers is recommended.</span></span> <span data-ttu-id="da2bf-184">Během instalace, můžete povolit serveru toobe v **pracovním režimu**.</span><span class="sxs-lookup"><span data-stu-id="da2bf-184">During installation, you can enable a server toobe in **staging mode**.</span></span>

<span data-ttu-id="da2bf-185">Další informace najdete v tématu [pracovním režimu](#staging-mode).</span><span class="sxs-lookup"><span data-stu-id="da2bf-185">For more information, see [staging mode](#staging-mode).</span></span>

### <a name="use-virtual-machines"></a><span data-ttu-id="da2bf-186">Použijte virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="da2bf-186">Use virtual machines</span></span>
<span data-ttu-id="da2bf-187">Je metoda běžných a podporovanou toorun hello synchronizační modul ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="da2bf-187">A common and supported method is toorun hello sync engine in a virtual machine.</span></span> <span data-ttu-id="da2bf-188">V případě, že má hostitel hello problém, může být hello bitové kopie s hello synchronizační modul server migrované tooanother server.</span><span class="sxs-lookup"><span data-stu-id="da2bf-188">In case hello host has an issue, hello image with hello sync engine server can be migrated tooanother server.</span></span>

### <a name="sql-high-availability"></a><span data-ttu-id="da2bf-189">Vysoká dostupnost SQL</span><span class="sxs-lookup"><span data-stu-id="da2bf-189">SQL High Availability</span></span>
<span data-ttu-id="da2bf-190">Pokud nepoužíváte hello SQL Server Express, která se dodává s Azure AD Connect, pak vysoká dostupnost pro SQL Server měli byste také zvážit.</span><span class="sxs-lookup"><span data-stu-id="da2bf-190">If you are not using hello SQL Server Express that comes with Azure AD Connect, then high availability for SQL Server should also be considered.</span></span> <span data-ttu-id="da2bf-191">řešení s vysokou dostupností Hello podporované zahrnují clusteringu SQL a AOA (skupiny dostupnosti Always On).</span><span class="sxs-lookup"><span data-stu-id="da2bf-191">hello high availability solutions supported include SQL clustering and AOA (Always On Availability Groups).</span></span> <span data-ttu-id="da2bf-192">Nepodporované řešení zahrnují zrcadlení.</span><span class="sxs-lookup"><span data-stu-id="da2bf-192">Unsupported solutions include mirroring.</span></span>

<span data-ttu-id="da2bf-193">Přidala se podpora pro SQL AOA tooAzure AD Connect ve verzi 1.1.524.0.</span><span class="sxs-lookup"><span data-stu-id="da2bf-193">Support for SQL AOA was added tooAzure AD Connect in version 1.1.524.0.</span></span> <span data-ttu-id="da2bf-194">Před instalací Azure AD Connect je nutné povolit SQL AOA.</span><span class="sxs-lookup"><span data-stu-id="da2bf-194">You must enable SQL AOA before installing Azure AD Connect.</span></span> <span data-ttu-id="da2bf-195">Při instalaci Azure AD Connect zjistí, zda zadaná instance SQL hello je povoleno pro SQL AOA, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="da2bf-195">During installation, Azure AD Connect detects whether hello SQL instance provided is enabled for SQL AOA or not.</span></span> <span data-ttu-id="da2bf-196">Pokud je povoleno SQL AOA, Azure AD Connect další hodnoty, pokud SQL AOA je nakonfigurované toouse replikace synchronní nebo asynchronní replikaci.</span><span class="sxs-lookup"><span data-stu-id="da2bf-196">If SQL AOA is enabled, Azure AD Connect further figures out if SQL AOA is configured toouse synchronous replication or asynchronous replication.</span></span> <span data-ttu-id="da2bf-197">Při nastavování hello naslouchací proces skupiny dostupnosti, se doporučuje, abyste nastavili too0 vlastnost RegisterAllProvidersIP hello.</span><span class="sxs-lookup"><span data-stu-id="da2bf-197">When setting up hello Availability Group Listener, it is recommended that you set hello RegisterAllProvidersIP property too0.</span></span> <span data-ttu-id="da2bf-198">Je to proto, že Azure AD Connect v současné době používá tooSQL tooconnect Nativní klient systému SQL a SQL Native Client hello použití MultiSubNetFailover vlastnosti nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="da2bf-198">This is because Azure AD Connect currently uses SQL Native Client tooconnect tooSQL and SQL Native Client does not support hello use of MultiSubNetFailover property.</span></span>

## <a name="appendix-csanalyzer"></a><span data-ttu-id="da2bf-199">Příloha CSAnalyzer</span><span class="sxs-lookup"><span data-stu-id="da2bf-199">Appendix CSAnalyzer</span></span>
<span data-ttu-id="da2bf-200">Části hello [ověřte](#verify) o toouse tento skript.</span><span class="sxs-lookup"><span data-stu-id="da2bf-200">See hello section [verify](#verify) on how toouse this script.</span></span>

```
Param(
    [Parameter(Mandatory=$true, HelpMessage="Must be a file generated using csexport 'Name of Connector' export.xml /f:x)")]
    [string]$xmltoimport="%temp%\exportedStage1a.xml",
    [Parameter(Mandatory=$false, HelpMessage="Maximum number of users per output file")][int]$batchsize=1000,
    [Parameter(Mandatory=$false, HelpMessage="Show console output")][bool]$showOutput=$false
)

#LINQ isn't loaded automatically, so force it
[Reflection.Assembly]::Load("System.Xml.Linq, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089") | Out-Null

[int]$count=1
[int]$outputfilecount=1
[array]$objOutputUsers=@()

#XML must be generated using "csexport "Name of Connector" export.xml /f:x"
write-host "Importing XML" -ForegroundColor Yellow

#XmlReader.Create won't properly resolve hello file location,
#so expand and then resolve it
$resolvedXMLtoimport=Resolve-Path -Path ([Environment]::ExpandEnvironmentVariables($xmltoimport))

#use an XmlReader toodeal with even large files
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
$result=$reader.ReadToDescendant('cs-object')
do 
{
    #create hello object placeholder
    #adding them up here means we can enforce consistency
    $objOutputUser=New-Object psobject
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name ID -Value ""
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name Type -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name DN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name operation -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name UPN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name displayName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name sourceAnchor -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name alias -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name primarySMTP -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name onPremisesSamAccountName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name mail -Value ""

    $user = [System.Xml.Linq.XElement]::ReadFrom($reader)
    if ($showOutput) {Write-Host Found an exported object... -ForegroundColor Green}

    #object id
    $outID=$user.Attribute('id').Value
    if ($showOutput) {Write-Host ID: $outID}
    $objOutputUser.ID=$outID

    #object type
    $outType=$user.Attribute('object-type').Value
    if ($showOutput) {Write-Host Type: $outType}
    $objOutputUser.Type=$outType

    #dn
    $outDN= $user.Element('unapplied-export').Element('delta').Attribute('dn').Value
    if ($showOutput) {Write-Host DN: $outDN}
    $objOutputUser.DN=$outDN

    #operation
    $outOperation= $user.Element('unapplied-export').Element('delta').Attribute('operation').Value
    if ($showOutput) {Write-Host Operation: $outOperation}
    $objOutputUser.operation=$outOperation

    #now that we have hello basics, go get hello details

    foreach ($attr in $user.Element('unapplied-export-hologram').Element('entry').Elements("attr"))
    {
        $attrvalue=$attr.Attribute('name').Value
        $internalvalue= $attr.Element('value').Value

        switch ($attrvalue)
        {
            "userPrincipalName"
            {
                if ($showOutput) {Write-Host UPN: $internalvalue}
                $objOutputUser.UPN=$internalvalue
            }
            "displayName"
            {
                if ($showOutput) {Write-Host displayName: $internalvalue}
                $objOutputUser.displayName=$internalvalue
            }
            "sourceAnchor"
            {
                if ($showOutput) {Write-Host sourceAnchor: $internalvalue}
                $objOutputUser.sourceAnchor=$internalvalue
            }
            "alias"
            {
                if ($showOutput) {Write-Host alias: $internalvalue}
                $objOutputUser.alias=$internalvalue
            }
            "proxyAddresses"
            {
                if ($showOutput) {Write-Host primarySMTP: ($internalvalue -replace "SMTP:","")}
                $objOutputUser.primarySMTP=$internalvalue -replace "SMTP:",""
            }
        }
    }

    $objOutputUsers += $objOutputUser

    Write-Progress -activity "Processing ${xmltoimport} in batches of ${batchsize}" -status "Batch ${outputfilecount}: " -percentComplete (($objOutputUsers.Count / $batchsize) * 100)

    #every so often, dump hello processed users in case we blow up somewhere
    if ($count % $batchsize -eq 0)
    {
        Write-Host Hit hello maximum users processed without completion... -ForegroundColor Yellow

        #export hello collection of users as as CSV
        Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
        $objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation

        #increment hello output file counter
        $outputfilecount+=1

        #reset hello collection and hello user counter
        $objOutputUsers = $null
        $count=0
    }

    $count+=1

    #need toobail out of hello loop if no more users tooprocess
    if ($reader.NodeType -eq [System.Xml.XmlNodeType]::EndElement)
    {
        break
    }

} while ($reader.Read)

#need toowrite out any users that didn't get picked up in a batch of 1000
#export hello collection of users as as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a><span data-ttu-id="da2bf-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="da2bf-201">Next steps</span></span>
<span data-ttu-id="da2bf-202">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="da2bf-202">**Overview topics**</span></span>  

* [<span data-ttu-id="da2bf-203">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="da2bf-203">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)  
* [<span data-ttu-id="da2bf-204">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="da2bf-204">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)  
