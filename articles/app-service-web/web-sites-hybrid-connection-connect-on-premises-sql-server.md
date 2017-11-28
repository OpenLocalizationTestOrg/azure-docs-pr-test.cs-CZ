---
title: "Připojení k místnímu SQL serveru z webové aplikace v Azure App Service pomocí hybridních připojení"
description: "Vytvoření webové aplikace v Microsoft Azure a připojte ho k databázi systému SQL Server na místě"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 2b4e0539-1a0b-4aa1-8a69-b4b053c3b2e5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2016
ms.author: cephalin
ms.openlocfilehash: 12456ef3e2aecfa7a03cca97de2ff6ffd9602357
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a><span data-ttu-id="b794a-103">Připojení k místnímu SQL serveru z webové aplikace v Azure App Service pomocí hybridních připojení</span><span class="sxs-lookup"><span data-stu-id="b794a-103">Connect to on-premises SQL Server from a web app in Azure App Service using Hybrid Connections</span></span>
<span data-ttu-id="b794a-104">Hybridní připojení se můžete připojit [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webových aplikací pro místní prostředky, které používají statický port TCP.</span><span class="sxs-lookup"><span data-stu-id="b794a-104">Hybrid Connections can connect [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps to on-premises resources that use a static TCP port.</span></span> <span data-ttu-id="b794a-105">Podporované prostředky zahrnují Microsoft SQL Server, MySQL, webové rozhraní API HTTP, služby App Service a většina vlastních webových služeb.</span><span class="sxs-lookup"><span data-stu-id="b794a-105">Supported resources include Microsoft SQL Server, MySQL, HTTP Web APIs, App Service, and most custom Web Services.</span></span>

<span data-ttu-id="b794a-106">V tomto kurzu se dozvíte, jak vytvořit webové aplikace služby App Service v [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715), webovou aplikaci připojit k místní databázi SQL serveru pomocí nové funkce hybridní připojení, vytvořit jednoduchou aplikaci ASP.NET který používat hybridní připojení a nasadit aplikace do webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="b794a-106">In this tutorial, you will learn how to create an App Service web app in the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), connect the web app to your local on-premises SQL Server database using the new Hybrid Connection feature, create a simple ASP.NET application that will use the hybrid connection, and deploy the application to the App Service web app.</span></span> <span data-ttu-id="b794a-107">Hotové webové aplikace v Azure uloží v databázi členství, která je místní přihlašovací údaje uživatele.</span><span class="sxs-lookup"><span data-stu-id="b794a-107">The completed web app on Azure stores user credentials in a membership database that is on-premises.</span></span> <span data-ttu-id="b794a-108">Kurz předpokládá žádné předchozí zkušenosti s používáním Azure nebo v technologii ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b794a-108">The tutorial assumes no prior experience using Azure or ASP.NET.</span></span>

> [!NOTE]
> <span data-ttu-id="b794a-109">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b794a-109">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="b794a-110">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="b794a-110">No credit cards required; no commitments.</span></span>
> 
> <span data-ttu-id="b794a-111">Je k dispozici pouze v části webových aplikací funkci hybridní připojení [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b794a-111">The Web Apps portion of the Hybrid Connections feature is available only in the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="b794a-112">Vytvoření připojení ve službě BizTalk Services naleznete v tématu [hybridní připojení](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span><span class="sxs-lookup"><span data-stu-id="b794a-112">To create a connection in BizTalk Services, see [Hybrid Connections](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span></span>  
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="b794a-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b794a-113">Prerequisites</span></span>
<span data-ttu-id="b794a-114">K dokončení tohoto kurzu, budete potřebovat následující produkty.</span><span class="sxs-lookup"><span data-stu-id="b794a-114">To complete this tutorial, you'll need the following products.</span></span> <span data-ttu-id="b794a-115">Všechny jsou k dispozici v bezplatné verze, abyste mohli začít, vývoj pro Azure zcela zdarma.</span><span class="sxs-lookup"><span data-stu-id="b794a-115">All are available in free versions, so you can start developing for Azure entirely for free.</span></span>

* <span data-ttu-id="b794a-116">**Předplatné Azure** – bezplatné předplatné, najdete v části [bezplatná zkušební verze Azure](/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b794a-116">**Azure subscription** - For a free subscription, see [Azure Free Trial](/pricing/free-trial/).</span></span>
* <span data-ttu-id="b794a-117">**Visual Studio 2013** – Pokud chcete stáhnout bezplatnou zkušební verzi Visual Studio 2013, najdete v části [Visual Studio stáhne](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span><span class="sxs-lookup"><span data-stu-id="b794a-117">**Visual Studio 2013** - To download a free trial version of Visual Studio 2013, see [Visual Studio Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span></span> <span data-ttu-id="b794a-118">Než budete pokračovat, nainstalujte tento.</span><span class="sxs-lookup"><span data-stu-id="b794a-118">Install this before continuing.</span></span>
* <span data-ttu-id="b794a-119">**Rozhraní Microsoft .NET Framework 3.5 Service Pack 1** – Pokud je váš operační systém Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 nebo Windows Server 2008 R2, můžete ho povolit v Ovládacích panelech > programy a funkce > zapnout Funkce systému Windows nebo vypnout.</span><span class="sxs-lookup"><span data-stu-id="b794a-119">**Microsoft .NET Framework 3.5 Service Pack 1** - If your operating system is Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, or Windows Server 2008 R2, you can enable this in Control Panel > Programs and Features > Turn Windows features on or off.</span></span> <span data-ttu-id="b794a-120">Jinak, můžete ji stáhnout z [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span><span class="sxs-lookup"><span data-stu-id="b794a-120">Otherwise, you can download it from the [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span></span>
* <span data-ttu-id="b794a-121">**SQL Server 2014 Express with Tools** -Microsoft SQL Server Express si zdarma stáhněte na [databáze platforma Microsoft webové stránky](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="b794a-121">**SQL Server 2014 Express with Tools** - download Microsoft SQL Server Express for free at the [Microsoft Web Platform Database page](http://www.microsoft.com/web/platform/database.aspx).</span></span> <span data-ttu-id="b794a-122">Vyberte **Express** (ne LocalDB) verze.</span><span class="sxs-lookup"><span data-stu-id="b794a-122">Choose the **Express** (not LocalDB) version.</span></span> <span data-ttu-id="b794a-123">**Express with Tools** verze zahrnuje SQL Server Management Studio, které budete používat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b794a-123">The **Express with Tools** version includes SQL Server Management Studio, which you will use in this tutorial.</span></span>
* <span data-ttu-id="b794a-124">**SQL Server Management Studio Express** – to je součástí systému SQL Server 2014 Express pomocí nástroje pro stažení výše uvedených, ale pokud potřebujete nainstalovat samostatně, můžete stáhnout a nainstalujte ji z [SQL Server Express stahování stránka](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="b794a-124">**SQL Server Management Studio Express** - This is included with the SQL Server 2014 Express with Tools download mentioned above, but if you need to install it separately, you can download and install it from the [SQL Server Express download page](http://www.microsoft.com/web/platform/database.aspx).</span></span>

<span data-ttu-id="b794a-125">Kurz předpokládá, že máte předplatné Azure, jestli máte nainstalovanou sadu Visual Studio 2013, a že jste nainstalovali nebo povoleno rozhraní .NET Framework 3.5.</span><span class="sxs-lookup"><span data-stu-id="b794a-125">The tutorial assumes that you have an Azure subscription, that you have installed Visual Studio 2013, and that you have installed or enabled .NET Framework 3.5.</span></span> <span data-ttu-id="b794a-126">Tento kurz ukazuje, jak nainstalovat systém SQL Server 2014 Express v konfiguraci, který pracuje s funkci Azure hybridní připojení (výchozí instanci s statický port TCP).</span><span class="sxs-lookup"><span data-stu-id="b794a-126">The tutorial shows you how to install SQL Server 2014 Express in a configuration that works well with the Azure Hybrid Connections feature (a default instance with a static TCP port).</span></span> <span data-ttu-id="b794a-127">Před zahájením tohoto kurzu, stáhněte z umístění uvedených výše, pokud nemáte nainstalovaný server SQL Server SQL Server 2014 Express with Tools.</span><span class="sxs-lookup"><span data-stu-id="b794a-127">Before starting the tutorial, download SQL Server 2014 Express with Tools from the location mentioned above if you do not have SQL Server installed.</span></span>

### <a name="notes"></a><span data-ttu-id="b794a-128">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b794a-128">Notes</span></span>
<span data-ttu-id="b794a-129">Používat hybridní připojení místní databázi systému SQL Server nebo SQL Server Express, musí být povolená na statický port TCP/IP.</span><span class="sxs-lookup"><span data-stu-id="b794a-129">To use an on-premises SQL Server or SQL Server Express database with a hybrid connection, TCP/IP needs to be enabled on a static port.</span></span> <span data-ttu-id="b794a-130">Výchozí instance na serveru SQL Server používají statický port 1433, zatímco pojmenovaných instancí nepodporují.</span><span class="sxs-lookup"><span data-stu-id="b794a-130">Default instances on SQL Server use static port 1433, whereas named instances do not.</span></span>

<span data-ttu-id="b794a-131">Počítač, na kterém jste nainstalovali agenta na místní správce hybridního připojení:</span><span class="sxs-lookup"><span data-stu-id="b794a-131">The computer on which you install the on-premises Hybrid Connection Manager agent:</span></span>

* <span data-ttu-id="b794a-132">Musí mít odchozí připojení k Azure přes:</span><span class="sxs-lookup"><span data-stu-id="b794a-132">Must have outbound connectivity to Azure over:</span></span>

| <span data-ttu-id="b794a-133">Port</span><span class="sxs-lookup"><span data-stu-id="b794a-133">Port</span></span> | <span data-ttu-id="b794a-134">Proč</span><span class="sxs-lookup"><span data-stu-id="b794a-134">Why</span></span> |
| --- | --- |
| <span data-ttu-id="b794a-135">80</span><span class="sxs-lookup"><span data-stu-id="b794a-135">80</span></span> |<span data-ttu-id="b794a-136">**Požadované** pro port HTTP pro ověření certifikátu a volitelně pro datové připojení.</span><span class="sxs-lookup"><span data-stu-id="b794a-136">**Required** for HTTP port for certificate validation and optionally for data connectivity.</span></span> |
| <span data-ttu-id="b794a-137">443</span><span class="sxs-lookup"><span data-stu-id="b794a-137">443</span></span> |<span data-ttu-id="b794a-138">**Volitelné** pro datové připojení.</span><span class="sxs-lookup"><span data-stu-id="b794a-138">**Optional** for data connectivity.</span></span> <span data-ttu-id="b794a-139">Pokud na hodnotu 443 odchozí připojení není k dispozici, použije se TCP port 80.</span><span class="sxs-lookup"><span data-stu-id="b794a-139">If outbound connectivity to 443 is unavailable, TCP port 80 is used.</span></span> |
| <span data-ttu-id="b794a-140">5671 a 9352</span><span class="sxs-lookup"><span data-stu-id="b794a-140">5671 and 9352</span></span> |<span data-ttu-id="b794a-141">**Doporučená** ale volitelné pro datové připojení.</span><span class="sxs-lookup"><span data-stu-id="b794a-141">**Recommended** but Optional for data connectivity.</span></span> <span data-ttu-id="b794a-142">Všimněte si, že tento režim obvykle dává vyšší propustnost.</span><span class="sxs-lookup"><span data-stu-id="b794a-142">Note this mode usually yields higher throughput.</span></span> <span data-ttu-id="b794a-143">Pokud k těmto portům odchozí připojení není k dispozici, použije se port 443 protokolu TCP.</span><span class="sxs-lookup"><span data-stu-id="b794a-143">If outbound connectivity to these ports is unavailable, TCP port 443 is used.</span></span> |

* <span data-ttu-id="b794a-144">Musí být schopen kontaktovat *hostname*:*ČísloPortu* vaše místní prostředku.</span><span class="sxs-lookup"><span data-stu-id="b794a-144">Must be able to reach the *hostname*:*portnumber* of your on-premises resource.</span></span>

<span data-ttu-id="b794a-145">Postup v tomto článku předpokládá, že používáte prohlížeč z počítače, který bude hostitelem agenta místní hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="b794a-145">The steps in this article assume that you are using the browser from the computer that will host the on-premises hybrid connection agent.</span></span>

<span data-ttu-id="b794a-146">Pokud už máte nainstalovaný v konfiguraci a v prostředí, které splňuje podmínky popsané výše server SQL Server, můžete přeskočit a začínat [vytvořit databázi systému SQL Server na místě](#CreateSQLDB).</span><span class="sxs-lookup"><span data-stu-id="b794a-146">If you already have SQL Server installed in a configuration and in an environment that meets the conditions described above, you can skip ahead and start with [Create a SQL Server database on-premises](#CreateSQLDB).</span></span>

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a><span data-ttu-id="b794a-147">A.</span><span class="sxs-lookup"><span data-stu-id="b794a-147">A.</span></span> <span data-ttu-id="b794a-148">Nainstalujte systém SQL Server Express, povolte protokol TCP/IP a vytvořit databázi systému SQL Server na místě</span><span class="sxs-lookup"><span data-stu-id="b794a-148">Install SQL Server Express, enable TCP/IP, and create a SQL Server database on-premises</span></span>
<span data-ttu-id="b794a-149">V této části se dozvíte, jak nainstalovat systém SQL Server Express, povolte protokol TCP/IP a vytvořit databázi tak, aby webová aplikace bude fungovat s portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b794a-149">This section shows you how to install SQL Server Express, enable TCP/IP, and create a database so that your web application will work with the Azure Portal.</span></span>

### <a name="install-sql-server-express"></a><span data-ttu-id="b794a-150">Nainstalujte SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="b794a-150">Install SQL Server Express</span></span>
1. <span data-ttu-id="b794a-151">Chcete-li nainstalovat systém SQL Server Express, spusťte **SQLEXPRWT_x64_ENU.exe** nebo **SQLEXPR_x86_ENU.exe** soubor, který jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="b794a-151">To install SQL Server Express, run the **SQLEXPRWT_x64_ENU.exe** or **SQLEXPR_x86_ENU.exe** file that you downloaded.</span></span> <span data-ttu-id="b794a-152">Zobrazí se Průvodce Centrum instalace SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="b794a-152">The SQL Server Installation Center wizard appears.</span></span>
   
    ![Instalace serveru SQL][SQLServerInstall]
2. <span data-ttu-id="b794a-154">Zvolte **samostatná instalace nový Server SQL nebo přidání funkcí do existující instalace**.</span><span class="sxs-lookup"><span data-stu-id="b794a-154">Choose **New SQL Server stand-alone installation or add features to an existing installation**.</span></span> <span data-ttu-id="b794a-155">Postupujte podle pokynů, až na přijímání výchozí možnosti a nastavení, **konfigurace Instance** stránky.</span><span class="sxs-lookup"><span data-stu-id="b794a-155">Follow the instructions, accepting the default choices and settings, until you get to the **Instance Configuration** page.</span></span>
3. <span data-ttu-id="b794a-156">Na **konfigurace Instance** vyberte **výchozí instance**.</span><span class="sxs-lookup"><span data-stu-id="b794a-156">On the **Instance Configuration** page, choose **Default instance**.</span></span>
   
    ![Vyberte výchozí instanci][ChooseDefaultInstance]
   
    <span data-ttu-id="b794a-158">Ve výchozím nastavení výchozí instance systému SQL Server čeká na požadavky od klientů systému SQL Server na statický port 1433, což je co vyžaduje funkci hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="b794a-158">By default, the default instance of SQL Server listens for requests from SQL Server clients on static port 1433, which is what the Hybrid Connections feature requires.</span></span> <span data-ttu-id="b794a-159">Pojmenované instance používají dynamické porty a UDP, které nejsou podporovány hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="b794a-159">Named instances use dynamic ports and UDP, which are not supported by Hybrid Connections.</span></span>
4. <span data-ttu-id="b794a-160">Přijměte výchozí hodnoty na **konfigurace serveru** stránky.</span><span class="sxs-lookup"><span data-stu-id="b794a-160">Accept the defaults on the **Server Configuration** page.</span></span>
5. <span data-ttu-id="b794a-161">Na **konfigurace databázového stroje** v části **režim ověřování**, zvolte **smíšený režim (ověřování systému SQL Server a ověřování systému Windows)**a zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="b794a-161">On the **Database Engine Configuration** page, under **Authentication Mode**, choose **Mixed Mode (SQL Server authentication and Windows authentication)**, and provide a password.</span></span>
   
    ![Zvolte ve smíšeném režimu][ChooseMixedMode]
   
    <span data-ttu-id="b794a-163">V tomto kurzu budete používat ověřování serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b794a-163">In this tutorial, you will be using SQL Server authentication.</span></span> <span data-ttu-id="b794a-164">Ujistěte se, že jste si pamatujete heslo, které poskytujete, protože jej budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="b794a-164">Be sure to remember the password that you provide, because you will need it later.</span></span>
6. <span data-ttu-id="b794a-165">Krok procházení zbývající části v průvodci pro dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="b794a-165">Step through the rest of the wizard to complete the installation.</span></span>

### <a name="enable-tcpip"></a><span data-ttu-id="b794a-166">Povolte protokol TCP/IP</span><span class="sxs-lookup"><span data-stu-id="b794a-166">Enable TCP/IP</span></span>
<span data-ttu-id="b794a-167">Pokud chcete povolit protokol TCP/IP, budete používat SQL Server Configuration Manager, který byl nainstalován při instalaci systému SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="b794a-167">To enable TCP/IP, you will use SQL Server Configuration Manager, which was installed when you installed SQL Server Express.</span></span> <span data-ttu-id="b794a-168">Postupujte podle kroků v [povolit síťový protokol TCP/IP pro SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b794a-168">Follow the steps in [Enable TCP/IP Network Protocol for SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) before continuing.</span></span>

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a><span data-ttu-id="b794a-169">Vytvořit databázi systému SQL Server na místě</span><span class="sxs-lookup"><span data-stu-id="b794a-169">Create a SQL Server database on-premises</span></span>
<span data-ttu-id="b794a-170">Webové aplikace Visual Studio vyžaduje členství v databázi, která je přístupná v Azure.</span><span class="sxs-lookup"><span data-stu-id="b794a-170">Your Visual Studio web application requires a membership database that can be accessed by Azure.</span></span> <span data-ttu-id="b794a-171">To vyžaduje databázi systému SQL Server nebo SQL Server Express (ne LocalDB databázi, šablona MVC používá ve výchozím nastavení), tak další vytvoříte databázi členství.</span><span class="sxs-lookup"><span data-stu-id="b794a-171">This requires a SQL Server or SQL Server Express database (not the LocalDB database that the MVC template uses by default), so you'll create the membership database next.</span></span>

1. <span data-ttu-id="b794a-172">V systému SQL Server Management Studio připojte k systému SQL Server, který jste právě nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="b794a-172">In SQL Server Management Studio, connect to the SQL Server you just installed.</span></span> <span data-ttu-id="b794a-173">(Pokud **připojit k serveru** dialogové okno nezobrazí automaticky, přejděte na **Průzkumník objektů** v levém podokně klikněte na **připojit**a pak klikněte na tlačítko  **Databáze modul**.) ![Připojení k serveru][SSMSConnectToServer]</span><span class="sxs-lookup"><span data-stu-id="b794a-173">(If the **Connect to Server** dialog does not appear automatically, navigate to **Object Explorer** in the left pane, click **Connect**, and then click **Database Engine**.) ![Connect to Server][SSMSConnectToServer]</span></span>
   
    <span data-ttu-id="b794a-174">Pro **typ serveru**, zvolte **databázový stroj**.</span><span class="sxs-lookup"><span data-stu-id="b794a-174">For **Server type**, choose **Database Engine**.</span></span> <span data-ttu-id="b794a-175">Pro **název serveru**, můžete použít **localhost** nebo název počítače, který používáte.</span><span class="sxs-lookup"><span data-stu-id="b794a-175">For **Server name**, you can use **localhost** or the name of the computer that you are using.</span></span> <span data-ttu-id="b794a-176">Zvolte **ověřování serveru SQL Server**a potom se přihlaste pomocí uživatelského jména správce systému a heslo, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="b794a-176">Choose **SQL Server authentication**, and then log in with the sa user name and the password that you created earlier.</span></span>
2. <span data-ttu-id="b794a-177">Chcete-li vytvořit novou databázi pomocí SQL Server Management Studio, klikněte pravým tlačítkem **databáze** v Průzkumníku objektů a potom klikněte na **novou databázi**.</span><span class="sxs-lookup"><span data-stu-id="b794a-177">To create a new database by using SQL Server Management Studio, right-click **Databases** in Object Explorer, and then click **New Database**.</span></span>
   
    ![Vytvoření nové databáze][SSMScreateNewDB]
3. <span data-ttu-id="b794a-179">V **novou databázi** dialogové okno, zadejte MembershipDB pro název databáze a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b794a-179">In the **New Database** dialog, enter MembershipDB for the database name, and then click **OK**.</span></span>
   
    ![Zadejte název databáze][SSMSprovideDBname]
   
    <span data-ttu-id="b794a-181">Všimněte si, že jste nebyly provedeny žádné změny do databáze v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="b794a-181">Note that you do not make any changes to the database at this point.</span></span> <span data-ttu-id="b794a-182">Informace o členství v budou automaticky později přidány webovou aplikací, když jej spustíte.</span><span class="sxs-lookup"><span data-stu-id="b794a-182">The membership information will be added automatically later by your web application when you run it.</span></span>
4. <span data-ttu-id="b794a-183">V Průzkumníku objektů, pokud rozbalíte **databáze**, uvidíte, že byla vytvořena databáze členství.</span><span class="sxs-lookup"><span data-stu-id="b794a-183">In Object Explorer, if you expand **Databases**, you will see that the membership database has been created.</span></span>
   
    ![MembershipDB vytvořen][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="b794a-185">B.</span><span class="sxs-lookup"><span data-stu-id="b794a-185">B.</span></span> <span data-ttu-id="b794a-186">Vytvořit webovou aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b794a-186">Create a web app in the Azure Portal</span></span>
> [!NOTE]
> <span data-ttu-id="b794a-187">Pokud jste již vytvořili webovou aplikaci na portálu Azure, který chcete použít pro tento kurz, můžete přeskočit na [vytvořit hybridní připojení a služby BizTalk](#CreateHC) a pokračovat dál.</span><span class="sxs-lookup"><span data-stu-id="b794a-187">If you have already created a web app in the Azure Portal that you want to use for this tutorial, you can skip ahead to [Create a Hybrid Connection and a BizTalk Service](#CreateHC) and continue from there.</span></span>
> 
> 

1. <span data-ttu-id="b794a-188">V [portálu Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b794a-188">In the [Azure Portal](https://portal.azure.com), click **New** > **Web + Mobile** > **Web app**.</span></span>
   
    ![tlačítko Nový][New]
2. <span data-ttu-id="b794a-190">Konfigurace webové aplikace a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b794a-190">Configure your web app, and then click **Create**.</span></span>
   
    ![Název webu][WebsiteCreationBlade]
3. <span data-ttu-id="b794a-192">Po chvíli se webové aplikace se vytvoří a otevře se okno jeho webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b794a-192">After a few moments, the web app is created and its web app blade appears.</span></span> <span data-ttu-id="b794a-193">V okně se svisle posouvatelným řídicí panel, který vám umožní spravovat webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b794a-193">The blade is a vertically scrollable dashboard that lets you manage your web app.</span></span>
   
    ![Spuštění webu][WebSiteRunningBlade]
   
    <span data-ttu-id="b794a-195">Pokud chcete ověřit, webová aplikace je za provozu, můžete kliknout na **Procházet** ikonu zobrazíte výchozí stránky.</span><span class="sxs-lookup"><span data-stu-id="b794a-195">To verify the web app is live, you can click the **Browse** icon to display the default page.</span></span>

<span data-ttu-id="b794a-196">V dalším kroku vytvoříte hybridní připojení a služba BizTalk webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b794a-196">Next, you will create a hybrid connection and a BizTalk service for the web app.</span></span>

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a><span data-ttu-id="b794a-197">C.</span><span class="sxs-lookup"><span data-stu-id="b794a-197">C.</span></span> <span data-ttu-id="b794a-198">Vytvoření hybridní připojení a služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="b794a-198">Create a Hybrid Connection and a BizTalk Service</span></span>
1. <span data-ttu-id="b794a-199">Zpět na portálu, přejděte do nastavení a klikněte na tlačítko **sítě** > **nakonfigurovat koncové body hybridního připojení**.</span><span class="sxs-lookup"><span data-stu-id="b794a-199">Back in the Portal, go to settings and click **Networking** > **Configure your hybrid connection endpoints**.</span></span>
   
    ![Hybridní připojení][CreateHCHCIcon]
2. <span data-ttu-id="b794a-201">V okně hybridní připojení, klikněte na **přidat** > **nové hybridní připojení**.</span><span class="sxs-lookup"><span data-stu-id="b794a-201">On the Hybrid connections blade, click **Add** > **New hybrid connection**.</span></span>
3. <span data-ttu-id="b794a-202">Na **vytvořit hybridní připojení** okno:</span><span class="sxs-lookup"><span data-stu-id="b794a-202">On the **Create hybrid connection** blade:</span></span>
   
   * <span data-ttu-id="b794a-203">Pro **název**, zadejte název pro připojení.</span><span class="sxs-lookup"><span data-stu-id="b794a-203">For **Name**, provide a name for the connection.</span></span>
   * <span data-ttu-id="b794a-204">Pro **Hostname**, zadejte název počítače hostitelského počítače systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b794a-204">For **Hostname**, enter the computer name of your SQL Server host computer.</span></span>
   * <span data-ttu-id="b794a-205">Pro **Port**, zadejte 1433 (výchozí port pro SQL Server).</span><span class="sxs-lookup"><span data-stu-id="b794a-205">For **Port**, enter 1433 (the default port for SQL Server).</span></span>
   * <span data-ttu-id="b794a-206">Klikněte na tlačítko **služby BizTalk** > **novou službu BizTalk** a zadejte název pro službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="b794a-206">Click **BizTalk Service** > **New BizTalk Service** and enter a name for the BizTalk service.</span></span>
     
     ![Vytvořit hybridní připojení.][TwinCreateHCBlades]
4. <span data-ttu-id="b794a-208">Klikněte na tlačítko **OK** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="b794a-208">Click **OK** twice.</span></span>
   
    <span data-ttu-id="b794a-209">Po dokončení procesu **oznámení** oblasti blikat zelená **úspěch** a **hybridní připojení** okno se zobrazí nový hybridní připojení se stavem jako **Nepřipojeno**.</span><span class="sxs-lookup"><span data-stu-id="b794a-209">When the process completes, the **Notifications** area will flash a green **SUCCESS** and the **Hybrid connection** blade will show the new hybrid connection with the status as **Not connected**.</span></span>
   
    ![Vytvoření jednoho hybridní připojení][CreateHCOneConnectionCreated]

<span data-ttu-id="b794a-211">V tomto okamžiku jste dokončili důležitou součástí infrastruktury cloudu hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="b794a-211">At this point, you have completed an important part of the cloud hybrid connection infrastructure.</span></span> <span data-ttu-id="b794a-212">V dalším kroku vytvoříte odpovídající část místně.</span><span class="sxs-lookup"><span data-stu-id="b794a-212">Next, you will create a corresponding on-premises piece.</span></span>

<a name="InstallHCM"></a>

## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a><span data-ttu-id="b794a-213">D.</span><span class="sxs-lookup"><span data-stu-id="b794a-213">D.</span></span> <span data-ttu-id="b794a-214">Instalovat místní správce hybridního připojení k připojení</span><span class="sxs-lookup"><span data-stu-id="b794a-214">Install the on-premises Hybrid Connection Manager to complete the connection</span></span>
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

<span data-ttu-id="b794a-215">Teď, když je kompletní infrastrukturu hybridní připojení, vytvoříte webovou aplikaci, která jej používá.</span><span class="sxs-lookup"><span data-stu-id="b794a-215">Now that the hybrid connection infrastructure is complete, you will create a web application that uses it.</span></span>

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a><span data-ttu-id="b794a-216">E.</span><span class="sxs-lookup"><span data-stu-id="b794a-216">E.</span></span> <span data-ttu-id="b794a-217">Vytvořit základní projekt ASP.NET, upravit připojovací řetězec databáze a spusťte projekt lokálně.</span><span class="sxs-lookup"><span data-stu-id="b794a-217">Create a basic ASP.NET web project, edit the database connection string, and run the project locally</span></span>
### <a name="create-a-basic-aspnet-project"></a><span data-ttu-id="b794a-218">Vytvoření základního projektu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b794a-218">Create a basic ASP.NET project</span></span>
1. <span data-ttu-id="b794a-219">V sadě Visual Studio na **souboru** nabídky, vytvořte nový projekt:</span><span class="sxs-lookup"><span data-stu-id="b794a-219">In Visual Studio, on the **File** menu, create a new Project:</span></span>
   
    ![Nový projekt Visual Studio][HCVSNewProject]
2. <span data-ttu-id="b794a-221">V **šablony** části **nový projekt** dialogovém okně, vyberte **webové** a zvolte **webové aplikace ASP.NET**a pak klikněte na tlačítko  **OK**.</span><span class="sxs-lookup"><span data-stu-id="b794a-221">In the **Templates** section of the **New Project** dialog, select **Web** and choose **ASP.NET Web Application**, and then click **OK**.</span></span>
   
    ![Vyberte webové aplikace ASP.NET][HCVSChooseASPNET]
3. <span data-ttu-id="b794a-223">V **nový projekt ASP.NET** dialogovém okně, vyberte **MVC**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b794a-223">In the **New ASP.NET Project** dialog, choose **MVC**, and then click **OK**.</span></span>
   
    ![Zvolte MVC][HCVSChooseMVC]
4. <span data-ttu-id="b794a-225">Po vytvoření projektu, zobrazí se stránka readme aplikace.</span><span class="sxs-lookup"><span data-stu-id="b794a-225">When the project has been created, the application readme page appears.</span></span> <span data-ttu-id="b794a-226">Ještě nespouštějte webového projektu.</span><span class="sxs-lookup"><span data-stu-id="b794a-226">Do not run the web project yet.</span></span>
   
    ![Stránce Readme][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a><span data-ttu-id="b794a-228">Upravit připojovací řetězec databáze pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="b794a-228">Edit the database connection string for the application</span></span>
<span data-ttu-id="b794a-229">V tomto kroku upravíte připojovací řetězec, který určuje aplikace, kde najít místní databáze SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="b794a-229">In this step, you edit the connection string that tells your application where to find your local SQL Server Express database.</span></span> <span data-ttu-id="b794a-230">Připojovací řetězec je v souboru Web.config aplikace, který obsahuje informace o konfiguraci pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b794a-230">The connection string is in the application's Web.config file, which contains configuration information for the application.</span></span>

> [!NOTE]
> <span data-ttu-id="b794a-231">Aby se zajistilo, že vaše aplikace používá databázi, kterou jste vytvořili v systému SQL Server Express a nikoli k tomu v sadě Visual Studio výchozí LocalDB, je důležité, provedení tohoto kroku před spuštěním projektu.</span><span class="sxs-lookup"><span data-stu-id="b794a-231">To ensure that your application uses the database that you created in SQL Server Express, and not the one in Visual Studio's default LocalDB, it is important that you complete this step before running your project.</span></span>
> 
> 

1. <span data-ttu-id="b794a-232">V Průzkumníku řešení poklikejte na soubor Web.config.</span><span class="sxs-lookup"><span data-stu-id="b794a-232">In Solution Explorer, double-click the Web.config file.</span></span>
   
    ![Soubor web.config][HCVSChooseWebConfig]
2. <span data-ttu-id="b794a-234">Upravit **connectionStrings** oddílu tak, aby odkazoval na databázi systému SQL Server na místním počítači, řiďte se syntaxí v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b794a-234">Edit the **connectionStrings** section to point to the SQL Server database on your local machine, following the syntax in the following example:</span></span>
   
    ![Připojovací řetězec][HCVSConnectionString]
   
    <span data-ttu-id="b794a-236">Při sestavování připojovací řetězec, mějte na paměti následující:</span><span class="sxs-lookup"><span data-stu-id="b794a-236">When composing the connection string, keep in mind the following:</span></span>
   
   * <span data-ttu-id="b794a-237">Pokud se připojujete k pojmenované instanci namísto výchozí instance (například YourServer\SQLEXPRESS), musíte nakonfigurovat SQL Server na použití statických portů.</span><span class="sxs-lookup"><span data-stu-id="b794a-237">If you are connecting to a named instance instead of a default instance (for example, YourServer\SQLEXPRESS), you must configure your SQL Server to use static ports.</span></span> <span data-ttu-id="b794a-238">Informace o konfiguraci statických portů najdete v tématu [postup konfigurace systému SQL Server tak, aby naslouchala na určitém portu](http://support.microsoft.com/kb/823938).</span><span class="sxs-lookup"><span data-stu-id="b794a-238">For information on configuring static ports, see [How to configure SQL Server to listen on a specific port](http://support.microsoft.com/kb/823938).</span></span> <span data-ttu-id="b794a-239">Ve výchozím nastavení používají pojmenované instance UDP a dynamické porty, které nejsou podporovány hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="b794a-239">By default, named instances use UDP and dynamic ports, which are not supported by Hybrid Connections.</span></span>
   * <span data-ttu-id="b794a-240">Je doporučeno, můžete určit port (standardně, jak je znázorněno v příkladu 1433) na připojovací řetězec tak, aby vám může být opravdu, místní systém SQL Server má TCP povoleno a používá správný port.</span><span class="sxs-lookup"><span data-stu-id="b794a-240">It is recommended that you specify the port (1433 by default, as shown in the example) on the connection string so that you can be sure that your local SQL Server has TCP enabled and is using the correct port.</span></span>
   * <span data-ttu-id="b794a-241">Nezapomeňte použít ověřování systému SQL Server se pokud chcete připojit, zadání ID uživatele a heslo v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="b794a-241">Remember to use SQL Server Authentication to connect, specifying the user ID and password in your connection string.</span></span>
3. <span data-ttu-id="b794a-242">Klikněte na tlačítko **Uložit** v sadě Visual Studio k uložení souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="b794a-242">Click **Save** in Visual Studio to save the Web.config file.</span></span>

### <a name="run-the-project-locally-and-register-a-new-user"></a><span data-ttu-id="b794a-243">Spusťte projekt lokálně a registraci nového uživatele</span><span class="sxs-lookup"><span data-stu-id="b794a-243">Run the project locally and register a new user</span></span>
1. <span data-ttu-id="b794a-244">Teď spusťte místně nový webový projekt kliknutím na tlačítko Procházet pod ladění.</span><span class="sxs-lookup"><span data-stu-id="b794a-244">Now, run your new web project locally by clicking the browse button under Debug.</span></span> <span data-ttu-id="b794a-245">Tento příklad používá aplikace Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="b794a-245">This example uses Internet Explorer.</span></span>
   
    ![Spusťte projekt][HCVSRunProject]
2. <span data-ttu-id="b794a-247">V pravém horním rohu stránky výchozí webové stránky, zvolte **zaregistrovat** zaregistrovat nový účet:</span><span class="sxs-lookup"><span data-stu-id="b794a-247">On the upper right of the default web page, choose **Register** to register a new account:</span></span>
   
    ![Zaregistrovat nový účet][HCVSRegisterLocally]
3. <span data-ttu-id="b794a-249">Zadejte uživatelské jméno a heslo:</span><span class="sxs-lookup"><span data-stu-id="b794a-249">Enter a user name and password:</span></span>
   
    ![Zadejte uživatelské jméno a heslo][HCVSCreateNewAccount]
   
    <span data-ttu-id="b794a-251">Toto automaticky vytvoří databázi na vaše místní systém SQL Server, který obsahuje informace o členství pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b794a-251">This automatically creates a database on your local SQL Server that holds the membership information for your application.</span></span> <span data-ttu-id="b794a-252">Jeden z tabulky (**dbo. AspNetUsers**) blokování webové aplikace přihlašovací údaje uživatele jako ty, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="b794a-252">One of the tables (**dbo.AspNetUsers**) holds web app user credentials like the ones that you just entered.</span></span> <span data-ttu-id="b794a-253">Zobrazí se tato tabulka později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b794a-253">You will see this table later in the tutorial.</span></span>
4. <span data-ttu-id="b794a-254">Zavřete okno prohlížeče výchozí webové stránky.</span><span class="sxs-lookup"><span data-stu-id="b794a-254">Close the browser window of the default web page.</span></span> <span data-ttu-id="b794a-255">To ukončí aplikaci v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b794a-255">This stops the application in Visual Studio.</span></span>

<span data-ttu-id="b794a-256">Nyní jste připraveni na další krok, který je k publikování aplikace do Azure a otestovat ji.</span><span class="sxs-lookup"><span data-stu-id="b794a-256">You are now ready for the next step, which is to publish the application to Azure and test it.</span></span>

<a name="PubNTest"></a>

## <a name="f-publish-the-web-application-to-azure-and-test-it"></a><span data-ttu-id="b794a-257">F.</span><span class="sxs-lookup"><span data-stu-id="b794a-257">F.</span></span> <span data-ttu-id="b794a-258">Publikování webové aplikace do Azure a otestovat ji</span><span class="sxs-lookup"><span data-stu-id="b794a-258">Publish the web application to Azure and test it</span></span>
<span data-ttu-id="b794a-259">Nyní budete publikování aplikace do webové aplikace služby App Service a otestujte ji chcete zobrazit, jak hybridní připojení, které jste dříve nakonfigurovali je používána pro webovou aplikaci připojit k databázi na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="b794a-259">Now, you'll publish your application to your App Service web app and then test it to see how the hybrid connection you configured earlier is being used to connect your web app to the database on your local machine.</span></span>

### <a name="publish-the-web-application"></a><span data-ttu-id="b794a-260">Publikování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="b794a-260">Publish the web application</span></span>
1. <span data-ttu-id="b794a-261">Můžete si stáhnout váš profil publikování pro webové aplikace App Service na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b794a-261">You can download your publishing profile for the App Service web app in the Azure Portal.</span></span> <span data-ttu-id="b794a-262">V okně vaší webové aplikace, klikněte na tlačítko **profilu publikování Get**a pak soubor uložte do počítače.</span><span class="sxs-lookup"><span data-stu-id="b794a-262">On the blade for your web app, click **Get publish profile**, and then save the file to your computer.</span></span>
   
    ![Stažení profilu publikování][PortalDownloadPublishProfile]
   
    <span data-ttu-id="b794a-264">Dále budete importovat tento soubor do webové aplikace Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b794a-264">Next, you will import this file into your Visual Studio web application.</span></span>
2. <span data-ttu-id="b794a-265">V sadě Visual Studio, klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="b794a-265">In Visual Studio, right-click the project name in Solution Explorer and select **Publish**.</span></span>
   
    ![Vyberte publikování][HCVSRightClickProjectSelectPublish]
3. <span data-ttu-id="b794a-267">V **Publikovat Web** dialogové okno na **profil** , zvolte **Import**.</span><span class="sxs-lookup"><span data-stu-id="b794a-267">In the **Publish Web** dialog, on the **Profile** tab, choose **Import**.</span></span>
   
    ![Import][HCVSPublishWebDialogImport]
4. <span data-ttu-id="b794a-269">Přejděte do vaší stažený profil publikování, vyberte ho a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b794a-269">Browse to your downloaded publishing profile, select it, and then click **OK**.</span></span>
   
    ![Vyhledejte profil][HCVSBrowseToImportPubProfile]
5. <span data-ttu-id="b794a-271">Informace o publikování je naimportována a zobrazí v **připojení** kartě dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b794a-271">Your publishing information is imported and displays on the **Connection** tab of the dialog.</span></span>
   
    ![Klikněte na tlačítko Publikovat][HCVSClickPublish]
   
    <span data-ttu-id="b794a-273">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="b794a-273">Click **Publish**.</span></span>
   
    <span data-ttu-id="b794a-274">Po dokončení publikování, bude váš prohlížeč spustit a zobrazit aplikace teď známé ASP.NET – s tím rozdílem, že nyní je za provozu v cloudu Azure!</span><span class="sxs-lookup"><span data-stu-id="b794a-274">When publishing completes, your browser will launch and show your now familiar ASP.NET application -- except that now it is live in the Azure cloud!</span></span>

<span data-ttu-id="b794a-275">V dalším kroku použijete za provozu webové aplikace zobrazíte jeho hybridní připojení v akci.</span><span class="sxs-lookup"><span data-stu-id="b794a-275">Next, you will use your live web application to see its Hybrid Connection in action.</span></span>

### <a name="test-the-completed-web-application-on-azure"></a><span data-ttu-id="b794a-276">Testování hotové webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="b794a-276">Test the completed web application on Azure</span></span>
1. <span data-ttu-id="b794a-277">V horní části napravo od webové stránky v Azure, zvolte **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="b794a-277">On the top right of your web page on Azure, choose **Log in**.</span></span>
   
    ![Test přihlášení][HCTestLogIn]
2. <span data-ttu-id="b794a-279">Webové aplikace služby App Service je teď připojený k databázi členství webové aplikace na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="b794a-279">Your App Service web app is now connected to your web application's membership database on your local machine.</span></span> <span data-ttu-id="b794a-280">Chcete-li to ověřit, přihlaste se pomocí stejných přihlašovacích údajů, které jste zadali dříve v místní databázi.</span><span class="sxs-lookup"><span data-stu-id="b794a-280">To verify this, log in with the same credentials that you entered in the local database earlier.</span></span>
   
    ![Hello pozdravu][HCTestHelloContoso]
3. <span data-ttu-id="b794a-282">Chcete-li otestovat další nové hybridní připojení, odhlaste se z Azure webové aplikace a zaregistrujte se jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="b794a-282">To further test your new hybrid connection, log off of your Azure web application and register as another user.</span></span> <span data-ttu-id="b794a-283">Zadejte nové uživatelské jméno a heslo a potom klikněte na **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="b794a-283">Provide a new user name and password, and then click **Register**.</span></span>
   
    ![Test zaregistrovat jiný uživatel][HCTestRegisterRelecloud]
4. <span data-ttu-id="b794a-285">Pokud chcete ověřit, že byly uloženy přihlašovací údaje nového uživatele v místní databázi prostřednictvím hybridní připojení, otevřete SQL Management Studio v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="b794a-285">To verify that the new user's credentials have been stored in your local database through your hybrid connection, open SQL Management Studio on your local computer.</span></span> <span data-ttu-id="b794a-286">V Průzkumníku objektů rozbalte **MembershipDB** databáze a potom rozbalte **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="b794a-286">In Object Explorer, expand the **MembershipDB** database, and then expand **Tables**.</span></span> <span data-ttu-id="b794a-287">Klikněte pravým tlačítkem myši **dbo. AspNetUsers** členství tabulky a zvolte **řádky vyberte Top 1000** zobrazíte výsledky.</span><span class="sxs-lookup"><span data-stu-id="b794a-287">Right-click the **dbo.AspNetUsers** membership table and choose **Select Top 1000 Rows** to view the results.</span></span>
   
    ![Zobrazení výsledků][HCTestSSMSTree]
5. <span data-ttu-id="b794a-289">Vaše tabulka členství v místní nyní zobrazuje oba účty – ten, který jste vytvořili místně a ten, který jste vytvořili v cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="b794a-289">Your local membership table now shows both accounts - the one that you created locally, and the one that you created in the Azure cloud.</span></span> <span data-ttu-id="b794a-290">Ten, který jste vytvořili v cloudu byly uloženy do místní databáze pomocí funkce Azure hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="b794a-290">The one that you created in the cloud has been saved to your on-premises database through Azure's Hybrid Connection feature.</span></span>
   
    ![Registrovaní uživatelé v místní databázi][HCTestShowMemberDb]

<span data-ttu-id="b794a-292">Nyní jste vytvořili a nasazené webové aplikace ASP.NET, která používá hybridní připojení mezi webovou aplikaci v cloudu Azure a místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b794a-292">You have now created and deployed an ASP.NET web application that uses a hybrid connection between a web app in the Azure cloud and an on-premises SQL Server database.</span></span> <span data-ttu-id="b794a-293">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="b794a-293">Congratulations!</span></span>

## <a name="see-also"></a><span data-ttu-id="b794a-294">Viz také</span><span class="sxs-lookup"><span data-stu-id="b794a-294">See Also</span></span>
[<span data-ttu-id="b794a-295">Přehled hybridních připojení</span><span class="sxs-lookup"><span data-stu-id="b794a-295">Hybrid Connections overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[<span data-ttu-id="b794a-296">Josh Točitost zavádí hybridní připojení (video na kanálu 9)</span><span class="sxs-lookup"><span data-stu-id="b794a-296">Josh Twist introduces hybrid connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[<span data-ttu-id="b794a-297">Přehled hybridních připojení</span><span class="sxs-lookup"><span data-stu-id="b794a-297">Hybrid Connections overview</span></span>](/services/biztalk-services/)

[<span data-ttu-id="b794a-298">BizTalk Services: Karty řídicí panel, sledování, škálování, konfigurovat a hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="b794a-298">BizTalk Services: Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[<span data-ttu-id="b794a-299">Vytváření reálného hybridního cloudu s bezproblémové přenositelnost aplikace (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="b794a-299">Building a Real-World Hybrid Cloud with Seamless Application Portability (Channel 9 video)</span></span>](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[<span data-ttu-id="b794a-300">Přístup k místním prostředkům v Azure App Service pomocí hybridních připojení</span><span class="sxs-lookup"><span data-stu-id="b794a-300">Access on-premises resources using hybrid connections in Azure App Service</span></span>](web-sites-hybrid-connection-get-started.md)

[<span data-ttu-id="b794a-301">Přehled technologie ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="b794a-301">ASP.NET Identity Overview</span></span>](http://www.asp.net/identity)

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
