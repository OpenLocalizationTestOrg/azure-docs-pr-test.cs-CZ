---
title: "aaaCreate služby Azure BizTalk Services v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak tooprovision nebo vytvoření služby Azure BizTalk Services v hello portál Azure; MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 6781cadada8ac9c84e1fe045d2b0f995811f75b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-biztalk-services-using-hello-azure-portal"></a><span data-ttu-id="c32b8-103">Vytvoření služby BizTalk Services pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c32b8-103">Create BizTalk Services using hello Azure portal</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]


> [!TIP]
> <span data-ttu-id="c32b8-104">toosign v toohello portálu Azure, potřebujete účet Azure a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="c32b8-104">toosign in toohello Azure portal, you need an Azure account and Azure subscription.</span></span> <span data-ttu-id="c32b8-105">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="c32b8-105">If you don't have an account, you can create a free trial account within a few minutes.</span></span> <span data-ttu-id="c32b8-106">Podívejte se na stránku [bezplatné zkušební verze Azure](http://go.microsoft.com/fwlink/p/?LinkID=239738).</span><span class="sxs-lookup"><span data-stu-id="c32b8-106">See [Azure Free Trial](http://go.microsoft.com/fwlink/p/?LinkID=239738).</span></span>


## <span data-ttu-id="c32b8-107"><a name="CreateService"></a>Vytvoření služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="c32b8-107"><a name="CreateService"></a>Create a BizTalk Service</span></span>
<span data-ttu-id="c32b8-108">V závislosti na hello edice zvolíte může být k dispozici veškeré nastavení služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-108">Depending on hello Edition you choose, not all BizTalk Service settings may be available.</span></span>

1. <span data-ttu-id="c32b8-109">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="c32b8-109">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="c32b8-110">V hello spodním navigačním podokně vyberte **nový**:</span><span class="sxs-lookup"><span data-stu-id="c32b8-110">In hello bottom navigation pane, select **NEW**:</span></span>  
   <span data-ttu-id="c32b8-111">![Vyberte tlačítko Nový hello][NEWButton]</span><span class="sxs-lookup"><span data-stu-id="c32b8-111">![Select hello New button][NEWButton]</span></span>
3. <span data-ttu-id="c32b8-112">Vyberte možnosti **APP SERVICES** > **BIZTALK SERVICE** (Služba BizTalk) > **CUSTOM CREATE** (Vytvořit vlastní):</span><span class="sxs-lookup"><span data-stu-id="c32b8-112">Select **APP SERVICES** > **BIZTALK SERVICE** > **CUSTOM CREATE**:</span></span>  
   <span data-ttu-id="c32b8-113">![Výběr služby BizTalk a možnosti Custom Create (Vytvořit vlastní)][NewBizTalkService]</span><span class="sxs-lookup"><span data-stu-id="c32b8-113">![Select BizTalk Service and select Custom Create][NewBizTalkService]</span></span>
4. <span data-ttu-id="c32b8-114">Zadejte nastavení služby BizTalk hello:</span><span class="sxs-lookup"><span data-stu-id="c32b8-114">Enter hello BizTalk Service settings:</span></span>
   
    <table border="1">
    <tr>
    <td><span data-ttu-id="c32b8-115"><strong>BizTalk service name (Název služby BizTalk)</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-115"><strong>BizTalk service name</strong></span></span></td>
    <td><span data-ttu-id="c32b8-116">Můžete zadat libovolný název, ale buďte konkrétní.</span><span class="sxs-lookup"><span data-stu-id="c32b8-116">You can enter any name but be specific.</span></span> <span data-ttu-id="c32b8-117">Možné příklady:</span><span class="sxs-lookup"><span data-stu-id="c32b8-117">Some examples include:</span></span><br/><br/><span data-ttu-id="c32b8-118">
    <em>moje_firma</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="c32b8-118">
    <em>mycompany</em>.biztalk.windows.net</span></span><br/><span data-ttu-id="c32b8-119">
    <em>moje_firma_moje_aplikace</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="c32b8-119">
    <em>mycompanymyapplication</em>.biztalk.windows.net</span></span><br/><span data-ttu-id="c32b8-120">
    <em>moje_aplikace</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="c32b8-120">
    <em>myapplication</em>.biztalk.windows.net</span></span><br/><br/><span data-ttu-id="c32b8-121">". biztalk.windows.net" je automaticky přidané toohello název zadáte.</span><span class="sxs-lookup"><span data-stu-id="c32b8-121">".biztalk.windows.net" is automatically added toohello name you enter.</span></span> <span data-ttu-id="c32b8-122">Tím se vytvoří adresu URL, která je služeb vaší BizTalk, jako je třeba použít tooaccess <strong>https://<em>Moje_aplikace</em>. biztalk.windows.net</strong>.</span><span class="sxs-lookup"><span data-stu-id="c32b8-122">This creates a URL that is used tooaccess your BizTalk Service, like <strong>https://<em>myapplication</em>.biztalk.windows.net</strong>.</span></span>
    </td>
    </tr>
    <tr>
    <td><span data-ttu-id="c32b8-123"><strong>Edice</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-123"><strong>Edition</strong></span></span></td>
    <td><span data-ttu-id="c32b8-124">Pokud jste ve fázi testování/vývoje hello, vyberte možnost <strong>vývojáře</strong>.</span><span class="sxs-lookup"><span data-stu-id="c32b8-124">If you are in hello testing/development phase, choose <strong>Developer</strong>.</span></span> <span data-ttu-id="c32b8-125">Pokud jste v produkční fázi hello, použijte hello <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk Services: Tabulka edic</a> toodetermine Pokud <strong>Premium</strong>, <strong>standardní</strong>, nebo <strong>Basic</strong>je pro vaši obchodní situaci hello nejvhodnější.</span><span class="sxs-lookup"><span data-stu-id="c32b8-125">If you are in hello production phase, use hello <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk Services: Editions Chart</a> toodetermine if <strong>Premium</strong>, <strong>Standard</strong>, or <strong>Basic</strong> is hello correct choice for your business scenario.</span></span>
    </td>
    </tr>
    <tr>
    <td><span data-ttu-id="c32b8-126"><strong>Oblast</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-126"><strong>Region</strong></span></span></td>
    <td><span data-ttu-id="c32b8-127">Vyberte zeměpisnou oblast toohost hello svoji službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-127">Select hello geographic region toohost your BizTalk Service.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c32b8-128"><strong>Domain URL (Adresa URL domény)</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-128"><strong>Domain URL</strong></span></span></td>
    <td><span data-ttu-id="c32b8-129"><strong>Volitelné</strong>.</span><span class="sxs-lookup"><span data-stu-id="c32b8-129"><strong>Optional</strong>.</span></span> <span data-ttu-id="c32b8-130">Ve výchozím nastavení, je adresa URL domény hello <em>Název_vaší_služby_biztalk</em>. biztalk.windows.net.</span><span class="sxs-lookup"><span data-stu-id="c32b8-130">By default, hello domain URL is <em>YourBizTalkServiceName</em>.biztalk.windows.net.</span></span> <span data-ttu-id="c32b8-131">Můžete ale zadat i vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="c32b8-131">A custom domain can also be entered.</span></span> <span data-ttu-id="c32b8-132">Pokud máte třeba doménu <em>contoso</em>, můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="c32b8-132">For example, if your domain is <em>contoso</em>, you can enter:</span></span> <br/><br/><span data-ttu-id="c32b8-133">
    <em>moje_firma</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c32b8-133">
    <em>MyCompany</em>.contoso.com</span></span><br/><span data-ttu-id="c32b8-134">
    <em>moje_firma_moje_aplikace</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c32b8-134">
    <em>MyCompanyMyApplication</em>.contoso.com</span></span><br/><span data-ttu-id="c32b8-135">
    <em>moje_aplikace</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c32b8-135">
    <em>MyApplication</em>.contoso.com</span></span><br/><span data-ttu-id="c32b8-136">
    <em>nazev_vasi_sluzby_BizTalk</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="c32b8-136">
    <em>YourBizTalkServiceName</em>.contoso.com</span></span><br/>
    </td>
    </tr>
    </table>
<span data-ttu-id="c32b8-137">Vyberte šipku další hello.</span><span class="sxs-lookup"><span data-stu-id="c32b8-137">Select hello NEXT arrow.</span></span>
<span data-ttu-id="c32b8-138">5.</span><span class="sxs-lookup"><span data-stu-id="c32b8-138">5.</span></span> <span data-ttu-id="c32b8-139">Zadejte nastavení databáze a hello úložiště:</span><span class="sxs-lookup"><span data-stu-id="c32b8-139">Enter hello Storage and Database Settings:</span></span>  <table border="1">
    <tr>
    <td><span data-ttu-id="c32b8-140"><strong>Monitoring/Archiving storage account (Účet úložiště pro monitorování/archivaci)</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-140"><strong>Monitoring/Archiving storage account</strong></span></span></td>
    <td><span data-ttu-id="c32b8-141">Vyberte stávající účet úložiště nebo vytvořte nový.</span><span class="sxs-lookup"><span data-stu-id="c32b8-141">Select an existing storage account or create a new storage account.</span></span> <br/><br/><span data-ttu-id="c32b8-142">Pokud vytvoříte nový účet úložiště, zadejte hello <strong>název účtu úložiště</strong>.</span><span class="sxs-lookup"><span data-stu-id="c32b8-142">If you create a new Storage account, enter hello <strong>Storage Account Name</strong>.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c32b8-143"><strong>Tracking database (Databáze sledování)</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-143"><strong>Tracking database</strong></span></span></td>
    <td><span data-ttu-id="c32b8-144">Pokud použijete existující službu Azure SQL Database, nesmí ji používat žádná jiná služba BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-144">If you use an existing Azure SQL Database, it cannot be used by another BizTalk Service.</span></span> <span data-ttu-id="c32b8-145">Musíte hello přihlašovací jméno a heslo zadané při vytvoření Server databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="c32b8-145">You need hello login name and password entered when that Azure SQL Database Server was created.</span></span><br/><br/><span data-ttu-id="c32b8-146"><strong>TIP</strong> databázi vytvořit hello sledování a účet úložiště pro monitorování/archivaci v hello stejné oblasti jako hello služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-146"><strong>TIP</strong> Create hello Tracking database and Monitoring/Archiving storage account in hello same region as hello BizTalk Service.</span></span></td>
    </tr>
    </table>
<span data-ttu-id="c32b8-147">Vyberte šipku další hello.</span><span class="sxs-lookup"><span data-stu-id="c32b8-147">Select hello NEXT arrow.</span></span>
<span data-ttu-id="c32b8-148">6.</span><span class="sxs-lookup"><span data-stu-id="c32b8-148">6.</span></span> <span data-ttu-id="c32b8-149">Zadejte nastavení databáze hello:</span><span class="sxs-lookup"><span data-stu-id="c32b8-149">Enter hello Database settings:</span></span>  <table border="1">
    <tr>
    <td><span data-ttu-id="c32b8-150"><strong>Název</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-150"><strong>Name</strong></span></span></td>
    <td><span data-ttu-id="c32b8-151">K dispozici v případě <strong>vytvoření nové instance databáze SQL</strong> je vybraný v předchozí úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="c32b8-151">Available when <strong>Create a new SQL Database instance</strong> is selected in hello previous screen.</span></span>
    <br/><br/>
<span data-ttu-id="c32b8-152">Zadejte toobe název databáze SQL používat svoji službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-152">Enter a SQL Database name toobe used by your BizTalk Service.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c32b8-153"><strong>Server</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-153"><strong>Server</strong></span></span></td>
    <td><span data-ttu-id="c32b8-154">K dispozici v případě <strong>vytvoření nové instance databáze SQL</strong> je vybraný v předchozí úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="c32b8-154">Available when <strong>Create a new SQL Database instance</strong> is selected in hello previous screen.</span></span>
    <br/><br/>
<span data-ttu-id="c32b8-155">Vyberte existující server služby SQL Database nebo vytvořte nový.</span><span class="sxs-lookup"><span data-stu-id="c32b8-155">Select an existing SQL Database Server or create a new SQL Database server.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c32b8-156"><strong>Server login name (Jméno pro přihlášení na server)</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-156"><strong>Server login name</strong></span></span></td>
    <td><span data-ttu-id="c32b8-157">Zadejte hello přihlašovací uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="c32b8-157">Enter hello login user name.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c32b8-158"><strong>Server login password (Heslo pro přihlášení na server)</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-158"><strong>Server login password</strong></span></span></td>
    <td><span data-ttu-id="c32b8-159">Zadejte heslo pro přihlášení hello.</span><span class="sxs-lookup"><span data-stu-id="c32b8-159">Enter hello login password.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="c32b8-160"><strong>Oblast</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-160"><strong>Region</strong></span></span></td>
    <td><span data-ttu-id="c32b8-161">Tato možnost je dostupná, jenom pokud je vybraná možnost <strong>Create a new SQL Database instance</strong> (Vytvořit novou instanci služby SQL Database).</span><span class="sxs-lookup"><span data-stu-id="c32b8-161">Available when <strong>Create a new SQL Database instance</strong> is selected.</span></span> <span data-ttu-id="c32b8-162">Vyberte zeměpisnou oblast toohost hello vaší databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="c32b8-162">Select hello geographic region toohost your SQL Database.</span></span></td>
    </tr>
    </table>

<span data-ttu-id="c32b8-163">Vyberte hello zaškrtnutí toocomplete hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="c32b8-163">Select hello check mark toocomplete hello wizard.</span></span> <span data-ttu-id="c32b8-164">Zobrazí se ikona průběhu Hello:</span><span class="sxs-lookup"><span data-stu-id="c32b8-164">hello progress icon appears:</span></span>  
![Po dokončení se zobrazí ikona průběhu][ProgressComplete]

<span data-ttu-id="c32b8-166">Po dokončení hello služby BizTalk Azure je vytvořená a připravená pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="c32b8-166">When complete, hello Azure BizTalk Service is created and ready for your applications.</span></span> <span data-ttu-id="c32b8-167">Hello výchozí nastavení je dostatečné.</span><span class="sxs-lookup"><span data-stu-id="c32b8-167">hello default settings are sufficient.</span></span> <span data-ttu-id="c32b8-168">Pokud chcete toochange hello výchozí nastavení, vyberte **BIZTALK SERVICES** v levém navigačním podokně text hello a potom vyberte svoji službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-168">If you want toochange hello default settings, select **BIZTALK SERVICES** in hello left navigation pane, and then select your BizTalk Service.</span></span> <span data-ttu-id="c32b8-169">Další nastavení, které se zobrazují v hello [karty řídicí panel, sledování a škálování](biztalk-dashboard-monitor-scale-tabs.md) v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="c32b8-169">Additional settings are displayed in hello [Dashboard, Monitor, and Scale tabs](biztalk-dashboard-monitor-scale-tabs.md) at hello top.</span></span>

<span data-ttu-id="c32b8-170">V závislosti na stavu hello hello služby BizTalk nejsou některé operace, které nelze dokončit.</span><span class="sxs-lookup"><span data-stu-id="c32b8-170">Depending on hello state of hello BizTalk Service, there are some operations that cannot be completed.</span></span> <span data-ttu-id="c32b8-171">Seznam těchto operací najdete příliš[BizTalk Services: Tabulka stavů](biztalk-service-state-chart.md).</span><span class="sxs-lookup"><span data-stu-id="c32b8-171">For a list of these operations, go too[BizTalk Services State Chart](biztalk-service-state-chart.md).</span></span>

## <a name="post-provisioning-steps"></a><span data-ttu-id="c32b8-172">Kroky pro zřízení</span><span class="sxs-lookup"><span data-stu-id="c32b8-172">Post-provisioning steps</span></span>
* [<span data-ttu-id="c32b8-173">Nainstalujte certifikát hello v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="c32b8-173">Install hello certificate on a local computer</span></span>](#InstallCert)
* [<span data-ttu-id="c32b8-174">Přidání certifikátu pro produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="c32b8-174">Add a production-ready certificate</span></span>](#AddCert)
* [<span data-ttu-id="c32b8-175">Získání oboru názvů řízení přístupu hello</span><span class="sxs-lookup"><span data-stu-id="c32b8-175">Get hello Access Control namespace</span></span>](#ACS)

#### <span data-ttu-id="c32b8-176"><a name="InstallCert"></a>Nainstalujte certifikát hello v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="c32b8-176"><a name="InstallCert"></a>Install hello certificate on a local computer</span></span>
<span data-ttu-id="c32b8-177">V rámci zřizování služby BizTalk se vytvoří certifikát podepsaný svým držitelem a přidruží se k vašemu předplatnému služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-177">As part of BizTalk Service provisioning, a self-signed certificate is created and associated with your BizTalk Service subscription.</span></span> <span data-ttu-id="c32b8-178">Musíte tento certifikát stáhnout a nainstalovat na počítačích, ze kterých buď nasazujete aplikace služby BizTalk, nebo posílat zprávy tooa koncového bodu služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-178">You must download this certificate and install it on computers from where you either deploy BizTalk Service applications or send messages tooa BizTalk Service endpoint.</span></span>

1. <span data-ttu-id="c32b8-179">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="c32b8-179">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="c32b8-180">Vyberte **BIZTALK SERVICES** v levém navigačním podokně text hello a potom vyberte své předplatné služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-180">Select **BIZTALK SERVICES** in hello left navigation pane, and then select your BizTalk Service subscription.</span></span>
3. <span data-ttu-id="c32b8-181">Vyberte hello **řídicí panel** kartě.</span><span class="sxs-lookup"><span data-stu-id="c32b8-181">Select hello **Dashboard** tab.</span></span>
4. <span data-ttu-id="c32b8-182">Vyberte **Download SSL Certificate** (Stáhnout certifikát SSL):</span><span class="sxs-lookup"><span data-stu-id="c32b8-182">Select **Download SSL Certificate**:</span></span>  
   <span data-ttu-id="c32b8-183">![Úprava certifikátu SSL][QuickGlance]</span><span class="sxs-lookup"><span data-stu-id="c32b8-183">![Modify SSL Certificate][QuickGlance]</span></span>
5. <span data-ttu-id="c32b8-184">Dvakrát klikněte na certifikát hello a spusťte přes hello Průvodce tooinstall hello certifikát.</span><span class="sxs-lookup"><span data-stu-id="c32b8-184">Double-click hello certificate and run through hello wizard tooinstall hello certificate.</span></span> <span data-ttu-id="c32b8-185">Zajistěte, aby certifikát hello hello instalujete **důvěryhodné kořenové certifikační úřady** uložit.</span><span class="sxs-lookup"><span data-stu-id="c32b8-185">Make sure you install hello certificate under hello **Trusted Root Certificate Authorities** store.</span></span>

#### <span data-ttu-id="c32b8-186"><a name="AddCert"></a>Přidání certifikátu pro produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="c32b8-186"><a name="AddCert"></a>Add a production-ready certificate</span></span>
<span data-ttu-id="c32b8-187">Hello podepsaný svým držitelem, který se automaticky vytvoří při vytváření služby BizTalk Services je určena pro použití ve vývojovém prostředí pouze.</span><span class="sxs-lookup"><span data-stu-id="c32b8-187">hello self-signed certificate that is automatically created when creating BizTalk Services is intended for use in development environments only.</span></span> <span data-ttu-id="c32b8-188">V produkčních scénářích ho nahraďte certifikátem pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="c32b8-188">For production scenarios, replace it with a production-ready certificate.</span></span>

1. <span data-ttu-id="c32b8-189">Na hello **řídicí panel** vyberte **certifikát SSL aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="c32b8-189">On hello **Dashboard** tab, select **Update SSL Certificate**.</span></span>
2. <span data-ttu-id="c32b8-190">Procházet tooyour soukromý certifikát SSL (*CertificateName*.pfx), obsahuje název vaší služby BizTalk, zadejte heslo hello a pak klikněte na tlačítko zaškrtnutí hello.</span><span class="sxs-lookup"><span data-stu-id="c32b8-190">Browse tooyour private SSL certificate (*CertificateName*.pfx) that includes your BizTalk Service name, enter hello password, and then click hello check mark.</span></span>

#### <span data-ttu-id="c32b8-191"><a name="ACS"></a>Získání oboru názvů řízení přístupu hello</span><span class="sxs-lookup"><span data-stu-id="c32b8-191"><a name="ACS"></a>Get hello Access Control namespace</span></span>
1. <span data-ttu-id="c32b8-192">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="c32b8-192">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="c32b8-193">Vyberte **BIZTALK SERVICES** v levém navigačním podokně text hello a potom vyberte svoji službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-193">Select **BIZTALK SERVICES** in hello left navigation pane, and then select your BizTalk Service.</span></span>
3. <span data-ttu-id="c32b8-194">Hello hlavním panelu vyberte **informace o připojení**:</span><span class="sxs-lookup"><span data-stu-id="c32b8-194">In hello task bar, select **Connection Information**:</span></span>  
   <span data-ttu-id="c32b8-195">![Výběr možnosti Informace o připojení][ACSConnectInfo]</span><span class="sxs-lookup"><span data-stu-id="c32b8-195">![Select Connection Information][ACSConnectInfo]</span></span>
4. <span data-ttu-id="c32b8-196">Zkopírujte hodnoty řízení přístupu hello.</span><span class="sxs-lookup"><span data-stu-id="c32b8-196">Copy hello Access Control values.</span></span>

<span data-ttu-id="c32b8-197">Tento obor názvů řízení přístupu zadáváte při nasazení projektu služby BizTalk v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c32b8-197">When you deploy a BizTalk Service project from Visual Studio, you enter this Access Control namespace.</span></span> <span data-ttu-id="c32b8-198">obor názvů řízení přístupu Hello se automaticky vytvoří pro vaši službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-198">hello Access Control namespace is automatically created for your BizTalk Service.</span></span>

<span data-ttu-id="c32b8-199">Hello hodnoty řízení přístupu můžete použít s libovolnou aplikací.</span><span class="sxs-lookup"><span data-stu-id="c32b8-199">hello Access Control values can be used with any application.</span></span> <span data-ttu-id="c32b8-200">Při vytváření služby Azure BizTalk Services tento obor názvů řízení přístupu řídí ověřování hello s nasazením služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-200">When Azure BizTalk Services is created, this Access Control namespace controls hello authentication with your BizTalk Service deployment.</span></span> <span data-ttu-id="c32b8-201">Zvolte, pokud chcete toochange hello předplatné nebo spravovat obor názvů hello **služby ACTIVE DIRECTORY** v hello levém navigačním podokně a potom zvolte svůj obor názvů.</span><span class="sxs-lookup"><span data-stu-id="c32b8-201">If you want toochange hello subscription or manage hello namespace, select **ACTIVE DIRECTORY** in hello left navigation pane and then select your namespace.</span></span> <span data-ttu-id="c32b8-202">Možnosti jsou uvedené na hlavním panelu Hello.</span><span class="sxs-lookup"><span data-stu-id="c32b8-202">hello task bar lists your options.</span></span>

<span data-ttu-id="c32b8-203">Kliknutím na tlačítko **spravovat** otevře hello portál pro správu řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="c32b8-203">Clicking **Manage** opens hello Access Control Management Portal.</span></span> <span data-ttu-id="c32b8-204">V hello portál pro správu řízení přístupu, hello používá služba BizTalk **identity služby**:</span><span class="sxs-lookup"><span data-stu-id="c32b8-204">In hello Access Control Management Portal, hello BizTalk Service uses **Service identities**:</span></span>  
<span data-ttu-id="c32b8-205">![Identita služby ACS v hello portál pro správu řízení přístupu][ACSServiceIdentities]</span><span class="sxs-lookup"><span data-stu-id="c32b8-205">![ACS Service Identities in hello Access Control Management Portal][ACSServiceIdentities]</span></span>

<span data-ttu-id="c32b8-206">Hello identita služby Řízení přístupu je sada přihlašovacích údajů, které umožňuje aplikacím nebo klientům tooauthenticate přímo pomocí řízení přístupu a získání tokenu.</span><span class="sxs-lookup"><span data-stu-id="c32b8-206">hello Access Control service identity is a set of credentials that allow applications or clients tooauthenticate directly with Access Control and receive a token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c32b8-207">Hello služba BizTalk používá **vlastníka** hello výchozí identitu služby a hello **heslo** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c32b8-207">hello BizTalk Service uses **Owner** for hello default service identity and hello **Password** value.</span></span> <span data-ttu-id="c32b8-208">Pokud místo hello hodnoty heslo použijete hodnotu symetrický klíč hello, hello následující může dojít k chybě.</span><span class="sxs-lookup"><span data-stu-id="c32b8-208">If you use hello Symmetric Key value instead of hello Password value, hello following error may occur.</span></span><br/><br/><span data-ttu-id="c32b8-209">*Účet služby pro správu řízení přístupu toohello se nejde připojit s hello zadané přihlašovací údaje*</span><span class="sxs-lookup"><span data-stu-id="c32b8-209">*Could not connect toohello Access Control Management Service account with hello specified credentials*</span></span>
> 
> 

<span data-ttu-id="c32b8-210">V článku [Správa oboru názvů služby ACS](https://msdn.microsoft.com/library/azure/hh674478.aspx) najdete některé pokyny a doporučení.</span><span class="sxs-lookup"><span data-stu-id="c32b8-210">[Managing Your ACS Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) lists some guidelines and recommendations.</span></span>

## <a name="requirements-explained"></a><span data-ttu-id="c32b8-211">Vysvětlení požadavků</span><span class="sxs-lookup"><span data-stu-id="c32b8-211">Requirements explained</span></span>
<span data-ttu-id="c32b8-212">Tyto požadavky se nevztahují toohello edice Free.</span><span class="sxs-lookup"><span data-stu-id="c32b8-212">These requirements do not apply toohello Free Edition.</span></span>

<table border="1">
<tr bgcolor="FAF9F9">
        <td><span data-ttu-id="c32b8-213"><strong>Co potřebujete?</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-213"><strong>What you need</strong></span></span></td>
        <td><span data-ttu-id="c32b8-214"><strong>Proč to potřebujete?</strong></span><span class="sxs-lookup"><span data-stu-id="c32b8-214"><strong>Why you need it</strong></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="c32b8-215">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="c32b8-215">Azure subscription</span></span></td>
<td><span data-ttu-id="c32b8-216">Hello předplatné Určuje, kdo může přihlásit toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c32b8-216">hello subscription determines who can sign in toohello Azure portal.</span></span> <span data-ttu-id="c32b8-217">Hello držitel účtu vytvoří předplatné hello na <a HREF="https://account.windowsazure.com/Subscriptions"> předplatných Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="c32b8-217">hello Account holder creates hello subscription at <a HREF="https://account.windowsazure.com/Subscriptions"> Azure Subscriptions</a>.</span></span>
<br/><br/>
<span data-ttu-id="c32b8-218">Hello účet Azure může mít několik odběrů a můžete spravovat každý, kdo je povoleno.</span><span class="sxs-lookup"><span data-stu-id="c32b8-218">hello Azure account can have multiple subscriptions and can be managed by anyone who is permitted.</span></span> <span data-ttu-id="c32b8-219">Třeba váš držitel účtu Azure vytvoří předplatné s názvem <em>Predplatne_služby_biztalk</em> a poskytuje hello správcům služby BizTalk v rámci vaší společnosti (například ContosoBTSAdmins@live.com) přístup k toothis předplatné.</span><span class="sxs-lookup"><span data-stu-id="c32b8-219">For example, your Azure account holder creates a subscription named <em>BizTalkServiceSubscription</em> and gives hello BizTalk Administrators within your company (for example, ContosoBTSAdmins@live.com) access toothis subscription.</span></span> <span data-ttu-id="c32b8-220">V tomto scénáři správcům služby BizTalk hello přihlásit toohello portál Azure a máte úplný správce práva tooall hello hostované služby v hello předplatného, včetně služby Azure BizTalk Services.</span><span class="sxs-lookup"><span data-stu-id="c32b8-220">In this scenario, hello BizTalk Administrators sign in toohello Azure portal and have full Administrator rights tooall hello hosted services in hello subscription, including Azure BizTalk Services.</span></span> <span data-ttu-id="c32b8-221">Hello správcům služby BizTalk nejsou držiteli účtu Azure hello a proto nemají přístup tooany fakturační informace.</span><span class="sxs-lookup"><span data-stu-id="c32b8-221">hello BizTalk Administrators are not hello Azure account holders and therefore don't have access tooany billing information.</span></span>
<br/><br/><span data-ttu-id="c32b8-222">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Správa předplatných a účtů úložiště v hello portál Azure</a> poskytuje další informace.</span><span class="sxs-lookup"><span data-stu-id="c32b8-222">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577"> Manage Subscriptions and Storage Accounts in hello Azure portal</a> provides more information.</span></span>
</td>
</tr>
<tr>
<td><span data-ttu-id="c32b8-223">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c32b8-223">Azure SQL Database</span></span></td>
<td><span data-ttu-id="c32b8-224">Ukládá hello tabulek, zobrazení a uložených procedur používaných hello služby BizTalk, včetně dat sledování hello.</span><span class="sxs-lookup"><span data-stu-id="c32b8-224">Stores hello tables, views, and stored procedures used by hello BizTalk Service, including hello Tracking data.</span></span>
<br/><br/>
<span data-ttu-id="c32b8-225">Když vytváříte službu BizTalk, můžete použít existující server SQL Azure nebo službu Azure SQL Database nebo můžete automaticky vytvořit nový server nebo databázi.</span><span class="sxs-lookup"><span data-stu-id="c32b8-225">When you create a BizTalk Service, you can use an existing Azure SQL Server, Azure SQL Database, or automatically create a new Server or Database.</span></span>
<br/><br/>
<span data-ttu-id="c32b8-226">Hello škálování databáze SQL se automaticky nakonfiguruje.</span><span class="sxs-lookup"><span data-stu-id="c32b8-226">hello SQL Database scale is automatically configured.</span></span> <span data-ttu-id="c32b8-227">Obvykle hello stačí výchozí škálování pro službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-227">Typically, hello default scale is sufficient for a BizTalk Service.</span></span> <span data-ttu-id="c32b8-228">Změna měřítka hello má vliv na ceny.</span><span class="sxs-lookup"><span data-stu-id="c32b8-228">Changing hello scale impacts pricing.</span></span> <span data-ttu-id="c32b8-229">Další informace najdete v tématu <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">o účtech a cenách služby Azure SQL Database</a>
.</span><span class="sxs-lookup"><span data-stu-id="c32b8-229">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930"> Accounts and Billing in Azure SQL Database</a>
</span></span><br/><br/><span data-ttu-id="c32b8-230">
<strong>Poznámky</strong>
</span><span class="sxs-lookup"><span data-stu-id="c32b8-230">
<strong>Notes</strong>
</span></span><br/>
<ul>
<li> <span data-ttu-id="c32b8-231">Při vytváření nového serveru SQL Azure a služby SQL Database automaticky dojde k povolení služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="c32b8-231">When you create a new Azure SQL Server and Database, Azure Services is automatically enabled.</span></span> <span data-ttu-id="c32b8-232">Hello služba BizTalk vyžaduje Azure Services povolit.</span><span class="sxs-lookup"><span data-stu-id="c32b8-232">hello BizTalk Service requires Azure Services be enabled.</span></span></li>
<li><span data-ttu-id="c32b8-233">Pokud vytvoříte novou databázi SQL Azure na existující Server SQL Azure, hello pravidla brány firewall hello serveru se nezmění.</span><span class="sxs-lookup"><span data-stu-id="c32b8-233">If you create a new Azure SQL Database on an existing Azure SQL Server, hello firewall rules of hello Server are not changed.</span></span> <span data-ttu-id="c32b8-234">V důsledku toho je možné, že jiné služby Azure nejsou povoleny databáze serveru toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="c32b8-234">As a result, it's possible other Azure Services are not allowed access toohello Server's databases.</span></span></li>
</ul>
</td>
</tr>
<tr>
<td><span data-ttu-id="c32b8-235">Obor názvů řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="c32b8-235">Azure Access Control namespace</span></span></td>
<td><span data-ttu-id="c32b8-236">Ověřuje se u služby Azure BizTalk Services.</span><span class="sxs-lookup"><span data-stu-id="c32b8-236">Authenticates with Azure BizTalk Services.</span></span> <span data-ttu-id="c32b8-237">Tento obor názvů řízení přístupu zadáváte při nasazení projektu služby BizTalk v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c32b8-237">When you deploy a BizTalk Service project from Visual Studio, you enter this Access Control namespace.</span></span> <span data-ttu-id="c32b8-238">Při vytváření služby BizTalk se automaticky vytvoří obor názvů řízení přístupu hello.</span><span class="sxs-lookup"><span data-stu-id="c32b8-238">When you create a BizTalk Service, hello Access Control namespace is automatically created.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="c32b8-239">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c32b8-239">Azure Storage account</span></span></td>
<td><span data-ttu-id="c32b8-240">Poskytuje přístup k tootables, objekty BLOB a fronty používané vaší služby BizTalk toosave hello následující:</span><span class="sxs-lookup"><span data-stu-id="c32b8-240">Gives access tootables, blobs, and queues used by your BizTalk Service toosave hello following:</span></span>

<ul>
<li><span data-ttu-id="c32b8-241">Protokolové soubory hello tohoto monitorování služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-241">Log files that monitor hello BizTalk Service.</span></span> <span data-ttu-id="c32b8-242">Hello výstup monitorování se také zobrazí v hello **monitorování** ve hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c32b8-242">hello monitoring output is also displayed in hello **Monitoring** tab in hello Azure portal.</span></span></li>
<li><span data-ttu-id="c32b8-243">Při vytváření X12 nebo smlouvy AS2 mezi partnery, můžete povolit vlastnosti zprávy, které funkce toostore hello archivaci.</span><span class="sxs-lookup"><span data-stu-id="c32b8-243">When creating an X12 or AS2 agreement between partners, you can enable hello Archiving feature toostore message properties.</span></span> <span data-ttu-id="c32b8-244">Tato data se uloží do hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c32b8-244">This data is saved in hello Storage account.</span></span></li>
</ul>
<br/>
<span data-ttu-id="c32b8-245">Při vytváření služby BizTalk můžete použít stávající účet služby Storage nebo automaticky vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="c32b8-245">When you create a BizTalk Service, you can use an existing Storage account or automatically create a new Storage account.</span></span>
<br/><br/>
<span data-ttu-id="c32b8-246">Výchozí nastavení služby Storage Hello je dostatečné pro službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-246">hello default Storage settings are sufficient for a BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="c32b8-247">Při vytváření účtu služby Storage se automaticky vytvoří primární a sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="c32b8-247">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="c32b8-248">Tyto klíče řídí přístup tooyour účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c32b8-248">These Keys control access tooyour Storage account.</span></span> <span data-ttu-id="c32b8-249">Hello služba BizTalk automaticky používá hello primární klíč.</span><span class="sxs-lookup"><span data-stu-id="c32b8-249">hello BizTalk Service automatically uses hello Primary Key.</span></span>
<br/><br/>
<span data-ttu-id="c32b8-250">Další informace najdete v článku o <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">službě Storage</a>.</span><span class="sxs-lookup"><span data-stu-id="c32b8-250">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671"> Storage</a> for more information.</span></span>
</td>
</tr>

<tr>
<td><span data-ttu-id="c32b8-251">Privátní certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="c32b8-251">SSL private certificate</span></span></td>
<td>
<span data-ttu-id="c32b8-252">Při vytváření služby Azure BizTalk se vytvoří také adresa URL HTTPS, která obsahuje název vaší služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-252">When an Azure BizTalk Service is created, an HTTPS URL that includes your BizTalk Service name is also created.</span></span> <span data-ttu-id="c32b8-253">Tato adresa URL je automaticky nakonfigurované toouse jenom pro vývojové certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="c32b8-253">This URL is automatically configured toouse a self-signed development-only certificate.</span></span> <span data-ttu-id="c32b8-254">V produkčním prostředí budete potřebovat privátní certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="c32b8-254">For production, you need a private SSL certificate.</span></span>
<br/><br/><span data-ttu-id="c32b8-255">
<strong>Důležité informace o certifikátu SSL</strong>

</span><span class="sxs-lookup"><span data-stu-id="c32b8-255">
<strong>Important SSL Certificate Information</strong>

</span></span><ul>
<li><span data-ttu-id="c32b8-256">Datum vypršení platnosti certifikátu Hello musí být menší než 5 let.</span><span class="sxs-lookup"><span data-stu-id="c32b8-256">hello certificate expiration date must be less than 5 years.</span></span></li>
<li><span data-ttu-id="c32b8-257">Všechny privátní certifikáty vyžadují heslo.</span><span class="sxs-lookup"><span data-stu-id="c32b8-257">All private certificates require a password.</span></span> <span data-ttu-id="c32b8-258">Heslo si zapamatujte a jako osvědčený postup také doporučujeme ho sdělit vašim správcům.</span><span class="sxs-lookup"><span data-stu-id="c32b8-258">Know this password and as a best practice, share this password with your administrators.</span></span></li>
<li><span data-ttu-id="c32b8-259">Certifikáty podepsané svým držitelem se používají v testovacím/vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="c32b8-259">Self-signed certificates are used in a test/development environment.</span></span> <span data-ttu-id="c32b8-260">Pokud používáte certifikáty podepsané svým držitelem, importujte hello certifikát tooyour osobním úložišti certifikátů a hello úložiště certifikátů důvěryhodných kořenových certifikačních autorit.</span><span class="sxs-lookup"><span data-stu-id="c32b8-260">When using self-signed certificates, import hello certificate tooyour Personal certificate store and hello Trusted Root Certification Authorities certificate store.</span></span></li>
</ul>
<br/><span data-ttu-id="c32b8-261">Při odesílání hello produkční certifikát požadavek tooyour certifikační autority, zadejte následující vlastnosti certifikátu hello:</span><span class="sxs-lookup"><span data-stu-id="c32b8-261">When sending hello production certificate request tooyour certification authority, give hello following certificate properties:</span></span>
<br/>

<ul>
<li><span data-ttu-id="c32b8-262"><strong>Rozšířené použití klíče</strong>: Služba Azure BizTalk Services vyžaduje minimálně ověření serveru.</span><span class="sxs-lookup"><span data-stu-id="c32b8-262"><strong>Enhanced Key Usage</strong>: At a minimum, Azure BizTalk Services requires Server Authentication.</span></span></li>
<li><span data-ttu-id="c32b8-263"><strong>Běžný název</strong>: Zadejte hello plně kvalifikovaný název domény (FQDN) adresy URL služby Azure BizTalk.</span><span class="sxs-lookup"><span data-stu-id="c32b8-263"><strong>Common Name</strong>: Enter hello fully qualified domain name (FQDN) of your Azure BizTalk Service URL.</span></span> <span data-ttu-id="c32b8-264">Projděte si část <a HREF="#CreateService">Vytvoření služby BizTalk</a> v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="c32b8-264">See <a HREF="#CreateService">Create a BizTalk Service</a> in this article.</span></span></li>
</ul>
<br/>
<span data-ttu-id="c32b8-265">Po vytvoření služby BizTalk hello lze přidat nový nebo jiný certifikát.</span><span class="sxs-lookup"><span data-stu-id="c32b8-265">A new or different certificate can be added after hello BizTalk Service is created.</span></span>
</td>
</tr>
</table>
<!---Loc Comment: Please, check link [Create a BizTalk Service] since it is not redirecting tooany location.--->



## <a name="hybrid-connections"></a><span data-ttu-id="c32b8-266">Hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="c32b8-266">Hybrid Connections</span></span>
<span data-ttu-id="c32b8-267">Při vytváření služby BizTalk Azure hello **hybridní připojení** karta je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="c32b8-267">When you create an Azure BizTalk Service, hello **Hybrid Connections** tab is available:</span></span>

![Karta Hybridní připojení][HybridConnectionTab]

<span data-ttu-id="c32b8-269">Hybridní připojení jsou použité tooconnect Azure web nebo mobilní služby Azure tooany místní prostředek, který používá statický port TCP, jako je například SQL Server, MySQL, webové rozhraní API HTTP, Mobile Services a většina vlastních webových služeb.</span><span class="sxs-lookup"><span data-stu-id="c32b8-269">Hybrid Connections are used tooconnect an Azure website or Azure mobile service tooany on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, Mobile Services, and most custom Web Services.</span></span>  <span data-ttu-id="c32b8-270">Hybridní připojení a hello služba BizTalk Adapter Service se liší.</span><span class="sxs-lookup"><span data-stu-id="c32b8-270">Hybrid Connections and hello BizTalk Adapter Service are different.</span></span> <span data-ttu-id="c32b8-271">Hello služba BizTalk Adapter Service je použité tooconnect služby Azure BizTalk Services tooan místní obchodnímu systému (LOB) systém.</span><span class="sxs-lookup"><span data-stu-id="c32b8-271">hello BizTalk Adapter Service is used tooconnect Azure BizTalk Services tooan on-premises Line of Business (LOB) system.</span></span>

 <span data-ttu-id="c32b8-272">V tématu [hybridní připojení](integration-hybrid-connection-overview.md) toolearn další, včetně vytváření a správě hybridních připojení.</span><span class="sxs-lookup"><span data-stu-id="c32b8-272">See [Hybrid Connections](integration-hybrid-connection-overview.md) toolearn more, including creating and managing Hybrid Connections.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c32b8-273">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c32b8-273">Next steps</span></span>
<span data-ttu-id="c32b8-274">Je teď vytvořená služby BizTalk, seznamte se s hello různých [BizTalk Services: karty řídicí panel, sledování a škálování](biztalk-dashboard-monitor-scale-tabs.md).</span><span class="sxs-lookup"><span data-stu-id="c32b8-274">Now that a BizTalk Service is created, familiarize yourself with hello different [BizTalk Services: Dashboard, Monitor and Scale tabs](biztalk-dashboard-monitor-scale-tabs.md).</span></span> <span data-ttu-id="c32b8-275">Služba BizTalk je připravená pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="c32b8-275">Your BizTalk Service is ready for your applications.</span></span> <span data-ttu-id="c32b8-276">vytváření aplikací, přejděte příliš toostart[služby Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span><span class="sxs-lookup"><span data-stu-id="c32b8-276">toostart creating applications, go too[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span></span>

## <a name="see-also"></a><span data-ttu-id="c32b8-277">Viz také</span><span class="sxs-lookup"><span data-stu-id="c32b8-277">See also</span></span>
* [<span data-ttu-id="c32b8-278">BizTalk Services: Tabulka edic</span><span class="sxs-lookup"><span data-stu-id="c32b8-278">BizTalk Services: Editions Chart</span></span>](biztalk-editions-feature-chart.md)<br/>
* [<span data-ttu-id="c32b8-279">BizTalk Services: Tabulka stavů</span><span class="sxs-lookup"><span data-stu-id="c32b8-279">BizTalk Services: State Chart</span></span>](biztalk-service-state-chart.md)<br/>
* [<span data-ttu-id="c32b8-280">BizTalk Services: Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="c32b8-280">BizTalk Services: Backup and Restore</span></span>](biztalk-backup-restore.md)<br/>
* [<span data-ttu-id="c32b8-281">BizTalk Services: Omezování</span><span class="sxs-lookup"><span data-stu-id="c32b8-281">BizTalk Services: Throttling</span></span>](biztalk-throttling-thresholds.md)<br/>
* [<span data-ttu-id="c32b8-282">BizTalk Services: Název a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="c32b8-282">BizTalk Services: Issuer Name and Issuer Key</span></span>](biztalk-issuer-name-issuer-key.md)<br/>
* [<span data-ttu-id="c32b8-283">Jak začít používat hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="c32b8-283">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="c32b8-284">Hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="c32b8-284">Hybrid Connections</span></span>](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
